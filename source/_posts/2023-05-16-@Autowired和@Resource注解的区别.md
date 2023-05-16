---
title: Autowired和Resource注解的区别
date: 2023-05-16 22:35:12
tags: 
- 八股文
- Java
categories: 八股文
---

@Autowired和@Resource是两个常用的依赖注入注解，它们用于告诉Spring容器在创建对象时如何注入其他对象。
<!-- more -->
它们之间的主要区别如下：

1. 来源不同：@Autowired注解是Spring框架提供的；而@Resource注解是Java EE标准中定义的，可以使用在非Spring应用程序中。
2. 注入方式不同：@Autowired注解默认按照类型（class）自动装配，如果需要更精确的控制，可以结合@Qualifier注解使用；而@Resource注解默认按照名称（name）自动装配，也可以通过@Resource(name = "xxx")指定具体的名称。
3. 适用范围不同：@Autowired注解可以用于构造方法、成员变量、Setter方法和普通方法上；而@Resource注解只能用于成员变量和Setter方法上。
4. 匹配规则不同：@Autowired采用了类型匹配的机制进行注入，因此，如果容器中存在多个类型相同的对象，则会抛出异常；而@Resource采用了名称匹配的机制进行注入，如果名称匹配失败，则会尝试进行类型匹配。
5. 兼容性不同：由于@Autowired是Spring框架提供的注解，因此不兼容非Spring环境；而@Resource是Java EE标准中提供的注解，因此具有更好的兼容性。

总之，@Autowired和@Resource都是用于依赖注入的注解，它们各有优缺点。如果需要在Spring应用程序中进行依赖注入，建议使用@Autowired注解；如果需要在Java EE应用程序中进行依赖注入，或者需要更精确地控制依赖注入的方式，建议使用@Resource注解。
