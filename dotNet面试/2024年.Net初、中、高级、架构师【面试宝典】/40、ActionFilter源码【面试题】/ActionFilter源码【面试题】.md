# ActionFilter源码【面试题】

1. 什么是Action Filter？
   答：Action Filter是ASP.NET MVC框架中的一种特性，用于在控制器方法执行前后或视图渲染前后执行一些逻辑，例如日志记录、身份验证、异常处理等。
2. Action Filter的作用是什么？
   答：Action Filter的作用是在控制器方法执行前后或视图渲染前后执行一些通用的逻辑，以实现代码的复用和统一处理。
3. Action Filter的执行顺序是怎样的？
   答：Action Filter的执行顺序是先执行全局过滤器，然后执行控制器级别的过滤器，最后执行动作方法级别的过滤器。
4. Action Filter的分类有哪些？
   答：Action Filter可以分为全局过滤器、控制器过滤器和动作方法过滤器三种类型。
5. 如何定义一个Action Filter？
   答：可以通过继承ActionFilterAttribute类来定义一个Action Filter，并重写其中的方法来实现自定义的逻辑。
6. Action Filter的生命周期是怎样的？
   答：Action Filter的生命周期包括实例化、依赖注入、执行OnActionExecuting方法、执行Action方法、执行OnActionExecuted方法、执行视图渲染前后的逻辑。
7. 如何在控制器中应用Action Filter？
   答：可以通过在控制器类或控制器方法上使用[ActionFilter]特性来应用Action Filter。
8. 如何在全局中注册Action Filter？
   答：可以通过在Global.asax.cs文件中的Application_Start方法中调用GlobalFilters类的Add方法来注册全局Action Filter。
9. Action Filter的参数传递方式有哪些？
   答：Action Filter的参数传递方式包括通过属性、构造函数、依赖注入等方式传递参数。
10. 如何在Action Filter中获取请求上下文信息？
    答：可以通过ActionExecutingContext参数的HttpContext属性来获取请求上下文信息。
11. 如何在Action Filter中修改请求或响应？
    答：可以通过ActionExecutingContext和ActionExecutedContext参数的HttpContext属性来修改请求或响应。
12. 如何在Action Filter中处理异常？
    答：可以通过重写OnException方法来处理Action方法中的异常，并进行相应的处理逻辑。
13. 如何在Action Filter中实现身份验证？
    答：可以通过重写OnActionExecuting方法，在该方法中进行身份验证的逻辑判断，并根据结果进行相应的处理。
14. 如何在Action Filter中实现日志记录？
    答：可以通过重写OnActionExecuting和OnActionExecuted方法，在这些方法中进行日志记录的逻辑。
15. 如何在Action Filter中实现缓存？
    答：可以通过重写OnActionExecuting方法，在该方法中判断是否存在缓存数据，并根据结果进行相应的处理。
16. 如何在Action Filter中实现权限控制？
    答：可以通过重写OnActionExecuting方法，在该方法中判断用户的权限，并根据结果进行相应的处理。
17. 如何在Action Filter中实现性能监控？
    答：可以通过重写OnActionExecuting和OnActionExecuted方法，在这些方法中进行性能监控的逻辑。
18. 如何在Action Filter中实现输入验证？
    答：可以通过重写OnActionExecuting方法，在该方法中对输入数据进行验证，并根据结果进行相应的处理。
19. 如何在Action Filter中实现输出缓存？
    答：可以通过重写OnActionExecuting方法，在该方法中判断是否存在缓存数据，并根据结果进行相应的处理。
20. 如何在Action Filter中实现跨域访问控制？
    答：可以通过重写OnActionExecuting方法，在该方法中设置响应头的Access-Control-Allow-Origin属性来实现跨域访问控制。