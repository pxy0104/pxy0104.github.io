---
title: ThreadLocal
date: 2023-06-04 17:34:16
tags:
- 八股文
- Java
- 解决方案
categories: Java
---

java.lang.ThreadLocal 是什么，有哪些用处，它是怎么实现的
<!-- more -->

# ThreadLocal

![01.png](1685871120015-fa2daf4b-4d6a-4072-9b49-af59a5f0d9cc.png)
ThreadLocal 是 Java 包中提供的一个类，它提供线程本地变量。如果创建一个 ThreadLocal 变量，那么访问这个变量的每个线程都会有这个变量的一个副本，在实际多线程操作时，操作的是自己本地内存中的变量，从而避免了线程安全问题。

## 实现原理

ThreadLocal 的实现原理是：每个线程都有一个 ThreadLocalMap（ThreadLocal 内部类），Map 中元素的键为 ThreadLocal，而值对应线程的变量副本。

因为每个线程都有自己的 ThreadLocalMap，其中存储的是当前线程的变量副本。所以，即使两个线程同时修改同一个 ThreadLocal 变量，它们实际上是在修改自己 ThreadLocalMap 中的 value，互不影响。所以ThreadLocal有自己特定的应用场景。

## 应用场景

ThreadLocal 有两个典型的使用场景：

1. 每个线程需要**一个独享的对象**（通常是工具类，典型需要使用的类有 SimpleDateFormat 和 Random）。
2. 每个线程内需要**保存全局变量**（例如在拦截器中获取用户信息），可以让不同方法直接使用，避免参数传递的麻烦。

此外，ThreadLocal 还可以用来解决数据库连接、Session 管理等问题。

## 缺点以及解决方案

ThreadLocal 有一个缺点是它不可继承性。这意味着子线程无法访问在父线程中设置的本地线程变量。为了解决这个问题，引入了一个新的类 InheritableThreadLocal。使用该类后，子线程可以访问在创建子线程时父线程当时的本地线程变量。

此外，在使用线程池的情况下，由于线程池是复用线程，不会重复创建，而上述的inheritableThreadLocals 是在创建子线程时才会将父线程的值复制到子线程，但是在线程池中不会重复创建，所以多次使用后，仍然记录的是第一次提交任务时的外部线程的值，造成了数据的错误。为了解决这个问题，阿里提出了 TransmittableThreadLocal 类。

同时，使用ThreadLocal也会降低代码的可重用性，在Java的多线程编程中，为保证多个线程对共享变量的安全访问，通常会使用synchronized来保证同一时刻只有一个线程对共享变量进行操作。这种情况下可以将类变量放到ThreadLocal类型的对象中，使变量在每个线程中都有独立拷贝，不会出现一个线程读取变量时而被另一个线程修改的现象。这种独立的拷贝与代码重用是违背的，所以说会降低代码的可重用性。但它不是类似全局变量，相反是类似局部变量。
