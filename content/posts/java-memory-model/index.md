---
title: Everything about JVM, Java Memory Model and Garbage Collection
cover: ./java-7.jpg
date: 2020-07-12
description: 
tags: ['post', 'java']
draft: false
---


#### Java Virtual Machine Technology

The JDK provides one or more implementations of the Java™ virtual machine (VM):  
- On platforms typically used for client applications, the JDK comes with a VM implementation called the Java HotSpot™ Client VM (client VM). The client VM is tuned for reducing start-up time and memory footprint. It can be invoked by using the -client command-line option when launching an application.
- On all platforms, the JDK comes with an implementation of the Java virtual machine called the Java HotSpot Server VM (server VM). The server VM is designed for maximum program execution speed. It can be invoked by using the -server command-line option when launching an application.
Some features of Java HotSpot technology, common to both VM implementations, are the following.

  - Adaptive compiler - Applications are launched using a standard interpreter, but the code is then analyzed as it runs to detect performance bottlenecks, or "hot spots". The Java HotSpot VMs compile those performance-critical portions of the code for a boost in performance, while avoiding unnecessary compilation of seldom-used code (most of the program). The Java HotSpot VMs also use the adaptive compiler to decide, on the fly, how best to optimize compiled code with techniques such as in-lining. The runtime analysis performed by the compiler allows it to eliminate guesswork in determining which optimizations will yield the largest performance benefit.
  - Rapid memory allocation and garbage collection - Java HotSpot technology provides for rapid memory allocation for objects, and it offers a choice of fast, efficient, state-of-the-art garbage collectors.
  - Thread synchronization - The Java programming language allows for use of multiple, concurrent paths of program execution (called "threads"). Java HotSpot technology provides a thread-handling capability that is designed to scale readily for use in large, shared-memory multiprocessor servers.
Tools

#### Java Memory Model

**JVM** requires some memory for its functioning. This memory is consumed from the available memory on host OS. This can be managed programmatically by providing it in startup parameters to jvm. Inside JVM, there exist separate memory spaces **(Heap, Non-Heap, and Cache)** in order to store runtime data and compiled code.

![java-memory](./jvm-mem.png)

- In the image shown above for Java Memory Model:
  - Heap Space: Eden + Survivor + Tenured
  - Non-Heap Space: Stack + MetaSpace + Reserved (Not shown here)
  - Cache


#### Heap Memory

This is the place where objects live. Prior to Java 8, we have Permanent Generation also as part of Heap. But Java 8 onwards, this has been replaced with a non-heap memory named as MetaSpace. After Java 8, Heap Space is divided into below two parts:

- #### Young Generation
The Young Generation is the place where all the new objects are created. When this young generation is filled, garbage collection is performed. This garbage collection is called Minor GC. Young Generation is divided into three parts – **Eden and two Survivor(S1, S2) Memory Spaces**.

  - Most of the newly created objects are located in the Eden memory space. 
  - When Eden space is filled with objects, Minor GC is performed and all the survivor objects are moved to one of the survivor spaces.
  - Minor GC also checks the survivor objects and move them to the other survivor space. So at a time, one of the survivor space is always empty.

- #### Old Generation
Objects that are survived after many cycles of GC, are moved to the Old Generation memory space, also known as **Tenured Space**. Usually, it’s done by setting a threshold for the age of the young generation objects before they become eligible to be promoted to Old generation.

Followings are some JVM memory configurations when running resource-intensive Java programs.
>
- -Xms`value` - minimum or initial Heap size
- -Xmx`value` - maximum Heap size
- -Xmn`value` - size of Young Generation, rest of the space goes for Old Generation.
- -XX:NewSize=`size` - new generation heap size
- -XX:MaxNewSizeSetting - maximum New generation heap size
- -XX:MaxPermGenSetting - maximum size of Permanent generation
- -XX:SurvivorRatioSetting - new heap size ratios (e.g. if Young Gen size is 10m and memory switch is –XX:SurvivorRatio=2, then 5m will be reserved for Eden space and 2.5m each for both Survivor spaces, default value = 8)
- -XX:NewRatio - providing ratio of Old/New Gen sizes (default value = 2)


#### Non Heap Memory
The memory that doesn't belong to Heap Area, is informally referred as Non-Heap Memory. This area includes **Stack** and **MetaSpace**.


- #### Stack
This memory is used for execution of a thread and it contains method specific values and references to other objects in Heap.

- #### MetaSpace

  From Java 8 onwards, Permanent Generation has been replaced with MetaSpace, which is a non-heap memory. MetaSpace holds the reflective data of JVM itself, like Class Loader related data, Class metadata, methods and more.

  MetaSpace is not a contiguous memory and can auto increase its size up to the limit that underlying OS allows, whereas Perm Gen always has a fixed maximum size. As long as the classloader is alive, the metadata remains alive in the Metaspace and can’t be freed.

- #### Reserved Space
  Jvm has its some reserved memory space for variety of purposes.

#### Code Cache

  Code Cache is cache memory, that is used for the storage of compiled native codes generated by JIT compiler, JVM internal structures, loaded profiler agent code and data, etc.

#### Garbage Collection

Unlike languages like C, C++, where developers have to take care of the memory management of their programs on their own, Java has feature of automatic memory management, called as **Garbage Collection**. 

Its a thread running in background that looks into all the objects in memory and find out that are not referenced by any part of the program to mark them for GC. All these marked Objects are deleted, and the space is reclaimed for allocation to other objects.

Garbage Collection is Generational, the gc that runs in **Young Generation** is called minor Garbage Collection and the one that runs in **Old Generation**, is called the full Garbage Collection.

Full Garbage collection is more expensive in terms of resources, as deleting live objects from Old Generation requires more efforts and is slower, compared to deleting unreferenced objects from Young Generation. Therefore full GC's are **“Stop the World”** events because all application threads are stopped until the operation completes.

#### Garbage Collection Types

There are five types of garbage collection that we can use in our applications. We just need to use the JVM switch to enable the garbage collection strategy for the application. Let’s look at each of them one by one.

- **Serial GC (-XX:+UseSerialGC):** Serial GC uses the simple mark-sweep-compact approach for young and old generations garbage collection i.e Minor and Major GC.Serial GC is useful in client machines such as our simple stand-alone applications and machines with smaller CPU. It is good for small applications with low memory footprint.

- **Parallel GC (-XX:+UseParallelGC):** Parallel GC is same as Serial GC except that is spawns N threads for young generation garbage collection where N is the number of CPU cores in the system. We can control the number of threads using -XX:ParallelGCThreads=n JVM option. Parallel Garbage Collector is also called throughput collector because it uses multiple CPUs to speed up the GC performance. Parallel GC uses a single thread for Old Generation garbage collection.

- **Parallel Old GC (-XX:+UseParallelOldGC):** This is same as Parallel GC except that it uses multiple threads for both Young Generation and Old Generation garbage collection.

- **Concurrent Mark Sweep (CMS) Collector (-XX:+UseConcMarkSweepGC):** CMS Collector is also referred as concurrent low pause collector. It does the garbage collection for the Old generation. CMS collector tries to minimize the pauses due to garbage collection by doing most of the garbage collection work concurrently with the application threads.CMS collector on the young generation uses the same algorithm as that of the parallel collector. This garbage collector is suitable for responsive applications where we can’t afford longer pause times. We can limit the number of threads in CMS collector using -XX:ParallelCMSThreads=n JVM option.

- **G1 Garbage Collector (-XX:+UseG1GC):** The Garbage First or G1 garbage collector is available from Java 7 and its long term goal is to replace the CMS collector. The G1 collector is a parallel, concurrent, and incrementally compacting low-pause garbage collector. Garbage First Collector doesn’t work like other collectors and there is no concept of Young and Old generation space. It divides the heap space into multiple equal-sized heap regions. When a garbage collection is invoked, it first collects the region with lesser live data, hence “Garbage First”. You can find more details about it at Garbage-First Collector Oracle Documentation.

#### Brief overview of Hotspot JVM Architecture:

![java-hotspot](./java-hotspot.png)


>
##### Sources: 
>
Orcale JDK Documentations
>
Few More Important Resources that can be checked out:
   - [Table for Oracle JavaSE VM Options (SE 7 and Earlier)](https://www.oracle.com/java/technologies/javase/vmoptions-jsp.html)
   - [All Java Hotspot VM command line flags for Java 8](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html)
   - [All Java Hotspot VM command line flags for Java 7](https://docs.oracle.com/javase/7/docs/technotes/tools/solaris/java.html)
   - [Oracle JVM Troubleshooting Memory](https://www.oracle.com/webfolder/technetwork/tutorials/mooc/JVM_Troubleshooting/week1/lesson1.pdf)
   - [Sun Java System Application Server Enterprise Edition 8.2 Performance Tuning Guide](https://docs.oracle.com/cd/E19900-01/819-4742/index.html)
   - [Guidelines for Java SE 8.2 Heap Sizing](https://docs.oracle.com/cd/E19900-01/819-4742/abeij/index.html)
   - [Java SE 8.2 Heap Tuning Parameters](https://docs.oracle.com/cd/E19900-01/819-4742/abeik/index.html)
   - [All Java Platform, Standard Edition Documentation](https://docs.oracle.com/en/java/javase/index.html)
   - [JDK 14 Documentation](https://docs.oracle.com/en/java/javase/14/)

