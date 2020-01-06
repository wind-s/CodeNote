#### Spring

想成为一名合格的 Java 后端工程师，最新Spring5的新特性、Spring5源码的构建、Springbean的生命周期、Spring循环依赖的源码设计、Spring扩展或者二次开发的原理、主流开源框架如何配合Spring特点等问题都必须要牢牢掌握。

##### 1.Bean的作用域：

    1.singleton：单例模式，Spring IoC容器中只会存在一个共享的Bean实例，无论有多少个Bean引用它，始终指向同一对象。
    2.prototype：原型模式，每次通过Spring容器获取prototype定义的bean时，容器都将创建一个新的Bean实例，每个Bean实例都有自己的属性和状态。
    3.request：在一次Http请求中，容器会返回该Bean的同一实例。而对不同的Http请求则会产生新的Bean，而且该bean仅在当前Http Request内有效。
    4.session：在一次Http Session中，容器会返回该Bean的同一实例。而对不同的Session请求则会创建新的实例，该bean实例仅在当前Session内有效。
    5.global Session：在一个全局的Http Session中，容器会返回该Bean的同一个实例，仅在使用portlet context时有效。

##### 2.IOC（DI）    

    1.控制反转
        由 Spring IOC 容器来负责对象的生命周期和对象之间的关系。IoC 容器控制对象的创建，依赖对象的获取被反转了
        没有 IoC 的时候我们都是在自己对象中主动去创建被依赖的对象，这是正转。但是有了 IoC 后，所依赖的对象直接由 IoC 容器创建后注入到被注入的对象中，依赖的对象由原来的主动获取变成被动接受，所以是反转.

##### 3.什么是Spring框架，Spring框架有哪些主要模块？4.使用Spring框架能带来哪些好处？

##### 5.mybatis源码当中利用了Spirng 的那些扩展？mybatis扩展Spring之后有哪些问题是无法解决的？
6.在Java中依赖注入有哪些方式？
7.eureka源码当中如何扩展的Spring？
8.Spring提供几种配置方式来设置元数据？Spring提供哪些配置形式？

##### 9.请解释Spring Bean的生命周期



##### 10.Spring容器当中包含了哪些常用组件，作用是什么，场景是什么？

##### 11.MyBatis 与 Hibernate 的区别是什么？MyBatis 如何实现模糊查询?
12.Nginx 反向代理实现高并发的具体步骤是什么？Nginx 搭建 Tomcat 集群的核心配置应该怎么写？

##### 13.AOP的底层实现原理

https://juejin.im/post/5b90e648f265da0aea695672

https://blog.csdn.net/fly910905/article/details/78641475

jdk动态代理，需要目标对象实现接口。

CgLib代理，不需要目标对象实现接口，spring会自动在JDK动态代理和CGLIB之间转换。

Spring AOP基于注解配置的情况下，需要依赖于AspectJ包的标准注解，但是不需要额外的编译以及AspectJ的织入器，而基于XML配置不需要，所以Spring AOP只是复用了AspectJ的注解，并没有其他依赖AspectJ的地方。

##### 14.静态代理和动态代理的区别？动态代理是怎么实现的？

***静态代理***：代理类在编译阶段生成，程序运行前就已经存在，那么这种代理方式被成为静态代理，这种情况下的代理类通常都是我们在Java代码中定义的。
***动态代理***：代理类在程序运行时创建，也就是说，这种情况下，代理类并不是在Java代码中定义的，而是在运行时根据我们在Java代码中的“指示”动态生成的。

目前，静态代理主要有AspectJ静态代理、JDK静态代理技术、而动态代理有JDK动态代理、Cglib动态代理技术，而Spring Aop是整合使用了JDK动态代理和Cglib动态代理两种技术，下面我们结合实例一步一步介绍所有的概念。

