# IOC源码【面试题】

1. 什么是IOC（控制反转）？
   答：IOC（Inversion of Control）是一种设计模式，它将对象的创建和依赖关系的管理交给容器来完成，而不是由对象自身来完成。
2. IOC容器的作用是什么？
   答：IOC容器负责创建和管理对象的生命周期，并解决对象之间的依赖关系，使得对象之间的耦合度降低。
3. Spring框架中的IOC容器是什么？
   答：Spring框架中的IOC容器是BeanFactory或ApplicationContext，它们负责管理和控制对象的创建和依赖关系。
4. IOC容器是如何实现对象的创建和依赖注入的？
   答：IOC容器通过反射机制实现对象的创建，并通过依赖注入将对象所需的依赖关系注入到对象中。
5. 什么是依赖注入（DI）？
   答：依赖注入是指将对象所需的依赖关系从外部注入到对象中，而不是由对象自身来创建和管理依赖关系。
6. 依赖注入的方式有哪些？
   答：依赖注入的方式包括构造函数注入、Setter方法注入和接口注入。
7. 构造函数注入和Setter方法注入有什么区别？
   答：构造函数注入是通过构造函数来注入依赖关系，而Setter方法注入是通过Setter方法来注入依赖关系。
8. 什么是自动装配（Autowiring）？
   答：自动装配是指IOC容器根据对象的类型或名称自动将依赖关系注入到对象中，而不需要手动配置。
9. Spring框架中的自动装配有哪些模式？
   答：Spring框架中的自动装配模式包括按类型自动装配、按名称自动装配、构造函数自动装配和混合自动装配。
10. 什么是Bean的作用域（Scope）？
    答：Bean的作用域指定了Bean实例的生命周期和可见范围，包括单例（Singleton）、原型（Prototype）等。
11. Spring框架中的Bean的作用域有哪些？
    答：Spring框架中的Bean的作用域包括单例（Singleton）、原型（Prototype）、会话（Session）、请求（Request）等。
12. 如何在Spring框架中配置Bean的作用域？
    答：可以通过在Bean的配置文件中使用元素的scope属性来配置Bean的作用域。
13. 什么是Bean的生命周期？
    答：Bean的生命周期包括Bean的实例化、初始化和销毁三个阶段。
14. Spring框架中如何管理Bean的生命周期？
    答：Spring框架通过Bean的后置处理器（BeanPostProcessor）来管理Bean的生命周期，可以在Bean的初始化前后执行一些操作。
15. 什么是Bean的后置处理器（BeanPostProcessor）？
    答：Bean的后置处理器是一个接口，它定义了在Bean的初始化前后执行的方法，可以对Bean进行一些自定义的处理。
16. Spring框架中的Bean的后置处理器有哪些？
    答：Spring框架中的Bean的后置处理器包括Bean的初始化前后处理器和Bean的销毁前后处理器。
17. 如何在Spring框架中自定义Bean的后置处理器？
    答：可以实现BeanPostProcessor接口，并在配置文件中将自定义的后置处理器注册到IOC容器中。
18. 什么是Bean的初始化方法和销毁方法？
    答：Bean的初始化方法是在Bean实例化后执行的方法，用于进行一些初始化操作；Bean的销毁方法是在Bean销毁前执行的方法，用于进行一些清理操作。
19. 如何在Spring框架中配置Bean的初始化方法和销毁方法？
    答：可以通过在Bean的配置文件中使用元素的init-method和destroy-method属性来配置Bean的初始化方法和销毁方法。
20. Spring框架中如何处理循环依赖？
    答：Spring框架通过提前暴露半成品Bean和使用代理对象来解决循环依赖的问题。