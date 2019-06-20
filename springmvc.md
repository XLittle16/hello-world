1、url寻找



2、注解扫描



3、类型转换

现象：后台返回一个对象，前端显示不了报错

报错信息如下：

**Type** Exception Report

**Message** Request processing failed; nested exception is java.lang.IllegalArgumentException: No converter found for return value of type: class com.xl.sheyla.mvcdemo.bean.user.UserBean

**Description** The server encountered an unexpected condition that prevented it from fulfilling the request.

**Exception**

```
org.springframework.web.util.NestedServletException: Request processing failed; nested exception is java.lang.IllegalArgumentException: No converter found for return value of type: class com.xl.sheyla.mvcdemo.bean.user.UserBean
	org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:982)
	org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:861)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:635)
	org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:846)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:742)
	org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:52)
```

**Root Cause**

```
java.lang.IllegalArgumentException: No converter found for return value of type: class com.xl.sheyla.mvcdemo.bean.user.UserBean
	org.springframework.web.servlet.mvc.method.annotation.AbstractMessageConverterMethodProcessor.writeWithMessageConverters(AbstractMessageConverterMethodProcessor.java:187)
	org.springframework.web.servlet.mvc.method.annotation.RequestResponseBodyMethodProcessor.handleReturnValue(RequestResponseBodyMethodProcessor.java:174)
	org.springframework.web.method.support.HandlerMethodReturnValueHandlerComposite.handleReturnValue(HandlerMethodReturnValueHandlerComposite.java:81)
	org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:113)
	org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:827)
	org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:738)
	org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:85)
	org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:967)
	org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:901)
	org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:970)
	org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:861)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:635)
	org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:846)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:742)
	org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:52)
```

**Note** The full stack trace of the root cause is available in the server logs.

------

### Apache Tomcat/8.5.34

类：RequestResponseBodyMethodProcessor



说明文档参考：https://www.jianshu.com/p/2f633cb817f5

![RequestResponseBodyMethodProcessor](E:\code\类图\AbstractMessageConverterMethodProcessor.png)



对于消息转换器的调用，都是在RequestResponseBodyMethodProcessor类中完成的。它实现了HandlerMethodArgumentResolver和HandlerMethodReturnValueHandler两个接口，分别实现了处理参数和处理返回值的方法。



HandlerMethodArgumentResolver和HandlerMethodReturnValueHandler使用了策略模式，提供方法，1、是否支持；2、解析处理

实现在RequestResponseBodyMethodProcessor中



而要动用这些消息转换器，需要在特定的位置加上@RequestBody和@ResponseBody。

![实现是否支持解析](E:\code\类图\1.png)



org.springframework.web.servlet.DispatcherServlet#doDispatch



org.springframework.web.servlet.HandlerExecutionChain  

找到handler

org.springframework.web.servlet.DispatcherServlet#getHandlerAdapter

找到handlerAdapter

```
modelAttributeCache
```

org.springframework.web.method.support.HandlerMethodReturnValueHandlerComposite#selectHandler



org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter#getModelAndView



org.springframework.web.servlet.DispatcherServlet#resolveViewName

解析视图

org.springframework.web.servlet.view.AbstractView#render

渲染视图



org.springframework.web.servlet.HandlerExecutionChain#triggerAfterCompletion

执行拦截器
