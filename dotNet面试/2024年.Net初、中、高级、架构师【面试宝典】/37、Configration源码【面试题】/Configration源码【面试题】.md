# Configration源码【面试题】

1. 什么是@Configuration注解？
   答：@Configuration是Spring框架中的注解，用于标识一个类是配置类，其中定义了Bean的创建和依赖关系。
2. [@Configuration](https://github.com/Configuration)注解的作用是什么？
   答：@Configuration注解用于告诉Spring框架该类是一个配置类，其中定义了Bean的创建和依赖关系。
3. [@Configuration](https://github.com/Configuration)注解与@Component注解有什么区别？
   答：@Configuration注解是Spring框架中的注解，用于标识一个类是配置类；@Component注解是Spring框架中的注解，用于标识一个类是组件类。
4. [@Configuration](https://github.com/Configuration)注解与@Bean注解有什么关系？
   答：@Configuration注解中的方法使用@Bean注解来定义Bean的创建和依赖关系。
5. [@Bean](https://github.com/Bean)注解的作用是什么？
   答：@Bean注解用于告诉Spring框架该方法是一个Bean的创建方法，并将其注册到IOC容器中。
6. [@Bean](https://github.com/Bean)注解与@Component注解有什么区别？
   答：@Bean注解是Spring框架中的注解，用于标识一个方法是Bean的创建方法；@Component注解是Spring框架中的注解，用于标识一个类是组件类。
7. [@Bean](https://github.com/Bean)注解与@Autowired注解有什么关系？
   答：@Bean注解用于创建Bean对象，@Autowired注解用于自动注入Bean对象。
8. [@Bean](https://github.com/Bean)注解的方法可以有参数吗？
   答：是的，@Bean注解的方法可以有参数，参数可以通过@Autowired注解进行自动注入。
9. [@Bean](https://github.com/Bean)注解的方法可以返回多个Bean对象吗？
   答：是的，@Bean注解的方法可以返回多个Bean对象，可以通过返回一个Map或List来实现。
10. [@Bean](https://github.com/Bean)注解的方法可以指定Bean的作用域吗？
    答：是的，@Bean注解的方法可以通过在方法上使用@Scope注解来指定Bean的作用域。
11. [@Bean](https://github.com/Bean)注解的方法可以指定Bean的初始化方法和销毁方法吗？
    答：是的，@Bean注解的方法可以通过在方法上使用@PostConstruct和@PreDestroy注解来指定Bean的初始化方法和销毁方法。
12. [@Bean](https://github.com/Bean)注解的方法可以指定Bean的名称吗？
    答：是的，@Bean注解的方法可以通过在方法上使用@Qualifier注解来指定Bean的名称。
13. [@Bean](https://github.com/Bean)注解的方法可以指定Bean的依赖关系吗？
    答：是的，@Bean注解的方法可以通过在方法的参数上使用@Autowired注解来指定Bean的依赖关系。
14. [@Bean](https://github.com/Bean)注解的方法可以使用条件注解吗？
    答：是的，@Bean注解的方法可以通过在方法上使用@Conditional注解来指定条件，满足条件时才创建Bean对象。
15. [@Bean](https://github.com/Bean)注解的方法可以使用FactoryBean吗？
    答：是的，@Bean注解的方法可以返回一个FactoryBean对象，用于创建复杂的Bean对象。
16. [@Bean](https://github.com/Bean)注解的方法可以使用Lazy注解吗？
    答：是的，@Bean注解的方法可以通过在方法上使用@Lazy注解来指定Bean的延迟加载。
17. [@Bean](https://github.com/Bean)注解的方法可以使用Primary注解吗？
    答：是的，@Bean注解的方法可以通过在方法上使用@Primary注解来指定Bean的首选项。
18. [@Bean](https://github.com/Bean)注解的方法可以使用Profile注解吗？
    答：是的，@Bean注解的方法可以通过在方法上使用@Profile注解来指定Bean的配置环境。
19. [@Bean](https://github.com/Bean)注解的方法可以使用DependsOn注解吗？
    答：是的，@Bean注解的方法可以通过在方法上使用@DependsOn注解来指定Bean的依赖关系。
20. [@Bean](https://github.com/Bean)注解的方法可以使用Import注解吗？
    答：是的，@Bean注解的方法可以通过在方法上使用@Import注解来导入其他配置类中的Bean。