---
title: Java Beans vs Enterprise Java Beans
cover:
date: 2020-10-17
description:
tags: ['post', 'java']
draft: false
---

#### Java Bean

Java Bean encapsulate many objects into one (bean), so that they can be passed around as a single bean object instead of as multiple individual objects.

> Java Beans are classes that are serialazable, have a zero-arg constructor, and allow access to properties using getter and setter methods.

---

#### Enterprise Bean

> As mentioned on [Oracle Docs](https://docs.oracle.com/javaee/6/tutorial/doc/gipmb.html), an Enterprise Bean is a server-side component that encapsulates the business logic of an application.

The business logic is the code that fulfills the purpose of the application. In an inventory control application, for example, the enterprise beans might implement the business logic in methods called checkInventoryLevel and orderProduct. By invoking these methods, clients can access the inventory services provided by the application.

#### Benefits of Enterprise Beans

For several reasons, enterprise beans simplify the development of large, distributed applications.

First, because the EJB container provides system-level services to enterprise beans, the bean developer can concentrate on solving business problems. The EJB container, rather than the bean developer, is responsible for system-level services such as transaction management and security authorization.

Second, because the beans rather than the clients contain the application’s business logic, the client developer can focus on the presentation of the client. The client developer does not have to code the routines that implement business rules or access databases. As a result, the clients are thinner, a benefit that is particularly important for clients that run on small devices.

Third, because enterprise beans are portable components, the application assembler can build new applications from existing beans. These applications can run on any compliant Java EE server provided that they use the standard APIs.

#### When to Use Enterprise Beans

You should consider using enterprise beans if your application has any of the following requirements:

1. The application must be scalable. To accommodate a growing number of users, you may need to distribute an application’s components across multiple machines. Not only can the enterprise beans of an application run on different machines, but also their location will remain transparent to the clients.

2. Transactions must ensure data integrity. Enterprise beans support transactions, the mechanisms that manage the concurrent access of shared objects.

3. The application will have a variety of clients. With only a few lines of code, remote clients can easily locate enterprise beans. These clients can be thin, various, and numerous.

Types of Enterprise Beans

1. **Session** : Performs a task for a client; optionally may implement a web service
1. **Message-Driven** - Acts as a listener for a particular messaging type, such as the Java Message Service API

> Note that Entity beans have been replaced by Java Persistence API entities.
