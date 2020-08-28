---
title: Getting hang of Java Garbage Collection
cover: 
date: 2020-07-19
description: Everything about Garbage Collection, gc switches and its logs monitoring tools
tags: ['post','java']
draft: false
---

Unlike languages like C, C++, where developers have to take care of memory management of their programs on their own, Java has feature of **automatic memory management**, called as **Garbage Collection**. 

It's a thread running in background that looks into memory and finds out those objects that are not referenced anymore by any part of the program. These unreferenced Objects are deleted, and the space is reclaimed for allocation to other objects.

>
**GC is based on a couple of hypothesis:**
1. Most objects soon become unreachable.
2. References from 'old' objects to 'young' objects only exist in small numbers.


GC is carried out by daemon thread known as Garbage Collector. There is no way to force guaranteed GC to happen explicitly. Although there are API's available like System.gc() to trigger the GC, but the final decison resides with JVM.

If new allocations can not happen due to no free memory available on heap, it will throw **java.lang.OutOfMemoryError heap space**.

Garbage Collection is Generational, the gc that runs in Young Generation is called **minor Garbage Collection** and the one that runs on entire heap, is called **full Garbage Collection**. 

>
A typical full GC involves following three steps:
1. **Mark**: Starts from root node of your application(main), walks the object graph, marks objects that are reachable as live
2. **Sweep or Delete:** Delete unreachable objects
3. **Compacting**: Compact the memory by moving around objects and making allocation contiguous than fragmented.

  - Since the newly created objects are always placed in Eden Space, once this space becomes full, any new allocation fails. Hence minor GC kicks in, moving all marked live objects to Survivor Spaces and all remaining dead objects are deleted, emtying the Eden Space.
  - Next minor GC also checks in for live objects in first survivor space and moves them to other survivor space, and empties the first survivor space by freeing up unreferenced objects, and vice-versa. So at a time, one of the survivor space is always empty. Thus, having two survivor spaces save one extra step of Compacting in minor GC by moving fragmented objects alternatively in contiguous locations in survivor spaces.
  - Live objects are moved back and forth in alternate Survivor Spaces until the Objects survive a threshold number of GCs, after which all surviving objects are promoted to next generation, called as Old Generation.
    
    -XX:MaxTenuringThreshold : switch to set GC threshold for promoting to Old Generation 

Once the Old Generation is almost full, full GC kicks in, which runs on entire Heap from Young Generation to Old Generation and performs **mark**, **sweep** and **compacting**. And this GC is more expensive in terms of resources, as performing full GC on entire Heap requires more efforts and is slower, compared to performing minor GC on Young Generation which is quick, because it runs only on a section of Heap Space (eden and survivor space). 

Although, operation in minor GC is quick and not that intrusive, it doesn't really seem like halting the application but irrespective of that, all GC's are **“Stop the World”** events, because all application threads are stopped until the GC operation completes. 

#### Application Performance with respect to GC

- **Responsiveness or latency**

  It is time taken by an Application to respond with a requested piece of data. E.g. how quickly a desktop UI responds to an event, or a website returns a page.

  For Applications that focus on responsiveness, large pause times are not acceptable. The focus is on responding in short period of time.

- **Throughput**
  
  It is maximum amount of work done by Application in a specific period of time. E.g. the number of transactions completed in a given time.

  High pause times are acceptable for applications that focus on throughput. Since high throughput applications focus on benchmarks over longer periods of time, quick response time is not a consideration.

#### Garbage Collection Types

There are basically five types of garbage collectors that we can use in our applications. We just need to use the JVM switch to enable garbage collection strategy for the application. Let’s look at each of them one by one.

- **Serial GC (-XX:+UseSerialGC):** Serial GC is a very basic GC that comes with Java, uses the simple mark-sweep-compact approach using single thread. It is good for small applications with low memory footprint.

- **Parallel GC (-XX:+UseParallelGC):** Parallel GC is same as Serial GC except that it spawns N threads for young generation garbage collection where N is the number of CPU cores in the system. We can control number of threads using -XX:ParallelGCThreads=n JVM option. Parallel Garbage Collector is also called throughput collector because it uses multiple CPUs to speed up GC performance. Parallel GC uses a single thread for Old Generation garbage collection.

- **Parallel Old GC (-XX:+UseParallelOldGC):** This is same as Parallel GC except that it uses multiple threads for both Young Generation and Old Generation garbage collection.

- **Concurrent Mark Sweep (CMS) Collector (-XX:+UseConcMarkSweepGC):** CMS Collector is also referred as concurrent low pause collector. It does GC for the Old generation and tries to minimize pauses due to GC by doing most of the garbage collection work concurrently with application threads without waiting for Old Generation to get full. It does not pauses the application for most of the GC operations. It only pasuses for Mark/Remark step. CMS collector on the young generation uses same algorithm as that of the parallel collector. This garbage collector is suitable for responsive applications where we can’t afford longer pause times. We can limit the number of threads in CMS collector using -XX:ParallelCMSThreads=n JVM option. It is the most favored GC for most of the Web and Financial Applications.

- **G1 Garbage Collector (-XX:+UseG1GC):** The Garbage First or G1 Garbage Collector is available from Java 7 and its long term goal is to replace the CMS collector. The G1 collector is a parallel, concurrent, and incrementally compacting low-pause garbage collector. Garbage First Collector doesn’t work like other collectors and there is no concept of Young and Old generation space. It divides heap space into multiple equal-sized heap regions. When a garbage collection is invoked, it first collects the region with lesser live data, hence “Garbage First”. You can find more details about it at [Garbage-First Collector Oracle Documentation](https://docs.oracle.com/javase/9/gctuning/garbage-first-garbage-collector.htm#JSGCT-GUID-082C967F-2DAC-4B59-8A81-0CEC6EEB9016).

#### Enabling GC Logging

If GC is behaving suspiciously, then below logging switches can be enabled for analyzing performance with respect to GC:
  -  -verbose:gc
  -  -XX:+PrintGCDetails
  -  -Xloggc:gc.log

#### GC Tools

There are some tools that can be used for GC aid:
- Garbage Collection Log Analyzer from IBM
- VisualVM : free tool that comes with jdk instalation, command: jvisualvm (a helpful plugin visualgc)
- Java Heap Analysis Tool (jhat)
- TerraCotta Big Memory

>
##### References: 
>
- Orcale JDK Documentations
>
Few More Important Resources that can be checked out:
   - [Oracle JVM Troubleshooting Memory](https://www.oracle.com/webfolder/technetwork/tutorials/mooc/JVM_Troubleshooting/week1/lesson1.pdf)
   - [Sun Java System Application Server Enterprise Edition 8.2 Performance Tuning Guide](https://docs.oracle.com/cd/E19900-01/819-4742/index.html)
   - [Guidelines for Java SE 8.2 Heap Sizing](https://docs.oracle.com/cd/E19900-01/819-4742/abeij/index.html)
   - [Java SE 8.2 Heap Tuning Parameters](https://docs.oracle.com/cd/E19900-01/819-4742/abeik/index.html)
   - [All Java Platform, Standard Edition Documentation](https://docs.oracle.com/en/java/javase/index.html)
   - [Java Platform, Standard Edition HotSpot Virtual Machine Garbage Collection Tuning Guide](https://docs.oracle.com/javase/9/gctuning/introduction-garbage-collection-tuning.htm#JSGCT-GUID-326EB4CF-8C8C-4267-8355-21AB04F0D304)