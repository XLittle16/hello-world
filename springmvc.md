1、url寻找

1、解析参数

org.springframework.web.servlet.mvc.method.RequestMappingInfo#getMatchingCondition

2、构建匹配的处理类

org.springframework.web.method.HandlerMethod#HandlerMethod(org.springframework.web.method.HandlerMethod, java.lang.Object)

3、添加处理链

org.springframework.web.servlet.handler.AbstractHandlerMapping#getHandlerExecutionChain

4、获取请求参数

org.springframework.web.method.support.InvocableHandlerMethod#getMethodArgumentValues



4.1 mybatis匹配路径（mappedStatements对应关系）

org.apache.ibatis.session.Configuration#getMappedStatement(java.lang.String, boolean)

4.2 解析mybatis节点信息

org.apache.ibatis.scripting.xmltags.MixedSqlNode#apply

```
<select id="getUserByBean" parameterType="userBean" resultType="userBean">
    select
    <include refid="sysUserResult"></include>
    from d_user where 1=1
    <include refid="sysUserParams"></include>
</select>
```

4.3 解析if语句的值

org.apache.ibatis.ognl.Ognl#getValue(java.lang.Object, java.util.Map, java.lang.Object, java.lang.Class)

and语句解析org.apache.ibatis.ognl.ASTAnd#getValueBody

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

# Springmvc初始化

## 1、WebApplicationContext

![](E:\A学习笔记\记录图片\WebApplicationContext.png)



## 2、ConfigurableWebApplicationContext

![](E:\A学习笔记\记录图片\ConfigurableWebApplicationContext.png)



配置文件读取，

参数为：contextConfigLocation

```
String configLocationParam = sc.getInitParameter(CONFIG_LOCATION_PARAM);
if (configLocationParam != null) {
   wac.setConfigLocation(configLocationParam);
}
```

```
org.springframework.web.context.ContextLoader#configureAndRefreshWebApplicationContext
org.springframework.context.ConfigurableApplicationContext#refresh
```

```
wac.refresh()
1、org.springframework.context.support.AbstractApplicationContext#prepareRefresh
> 设置容器状态，起送时间，closed=false，active=true
> 初始化容器环境的配置
> 校验环境配置
> 初始化一个集合earlyApplicationEvents--》ApplicationEvent

2、获取一个最新的工厂org.springframework.context.support.AbstractApplicationContext#obtainFreshBeanFactory
第一步刷新工厂
org.springframework.context.support.AbstractRefreshableApplicationContext#refreshBeanFactory
* 判断beanFacotry工厂是否存在this.beanFactory != null
* 不为空，则将bean工厂销毁‘
* 创建默认工厂DefaultListableBeanFactory-》org.springframework.context.support.AbstractRefreshableApplicationContext#createBeanFactory
* 
```

![](E:\A学习笔记\记录图片\DefaultListableBeanFactory.png)



```
* 设置容器id
org.springframework.context.support.AbstractApplicationContext#getId

org.springframework.beans.factory.support.DefaultListableBeanFactory#setSerializationId
* 自定义beanFactory，设置属性，allowBeanDefinitionOverriding，允许重新注册相同名字的不同definitioan，默认true；自动替换前者--》org.springframework.beans.factory.support.DefaultListableBeanFactory#allowBeanDefinitionOverriding
allowCircularReferences是否自动尝试解析beans之前的循环引用，默认为true；对于单例作用域的bean，可以通过“setAllowCircularReferences(false)”来禁用循环引用，这样如果存在循环引用就会抛出异常来通知用户--》org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#allowCircularReferences


org.springframework.context.support.AbstractRefreshableApplicationContext#customizeBeanFactory

* 加载beanDefinition org.springframework.web.context.support.XmlWebApplicationContext#loadBeanDefinitions(org.springframework.beans.factory.support.DefaultListableBeanFactory)
 > 初始化一个beanDefinitiaonReader阅读器，PathMatchingResourcePatternResolver
路径匹配资源模式解析器，StandardEnvironment，XmlValidationModeDetectorXml验证模式检测器，ResourceLoader资源加载器
 > 初始化beanDefinitonReader，默认实现为空
 > 加载beanDeifinition，使用给定的Xml bean定义阅读器加载bean定义
 1）加载配置位置，默认为/WEB-INF/applicationContext.xml或者有namespace,则默认的位置为/WEB-INF/${namespace}.xml
 配置的位置为configLocations，web.xml配置参数为contextConfigLocation
  <!--把applicationContext.xml加入到配置文件中-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>
            /WEB-INF/applicationContext.xml,/WEB-INF/spring-config-redis.xml,classpath*:spring-config-redis.xml
        </param-value>
    </context-param>
   2）阅读器将文件路径加载成beanDefinitions
   org.springframework.beans.factory.support.AbstractBeanDefinitionReader#loadBeanDefinitions(java.lang.String)
   org.springframework.beans.factory.support.AbstractBeanDefinitionReader#loadBeanDefinitions(java.lang.String, java.util.Set<org.springframework.core.io.Resource>)
   加载beanDefinition
   org.springframework.beans.factory.xml.XmlBeanDefinitionReader#doLoadBeanDefinitions
   解析成Document
   注册beanDefinition
   beanDefinitionMap.size()为beanDefinition的大小
   
   org.springframework.beans.factory.xml.DefaultBeanDefinitionDocumentReader#doRegisterBeanDefinitions
   解析器委托
   org.springframework.beans.factory.xml.BeanDefinitionParserDelegate
  属性 EANS_NAMESPACE_URI = "http://www.springframework.org/schema/beans";
   org.springframework.beans.factory.xml.DefaultBeanDefinitionDocumentReader#parseBeanDefinitions
   解析节点
   org.springframework.beans.factory.xml.BeanDefinitionParserDelegate#parseCustomElement(org.w3c.dom.Element, org.springframework.beans.factory.config.BeanDefinition)
   解析
   org.springframework.context.annotation.ComponentScanBeanDefinitionParser#parse
   1）解析扫描包名  readerContext-》XmlBeanDefinitionReader-》StandardServletEnvironment ->PropertySourcesPropertyResolver
   
   org.springframework.core.env.AbstractPropertyResolver#resolvePlaceholders
   创建PropertyPlaceholderHelper，设置属性，simplePrefix=’{‘ valueSeparator=’：‘
   解析org.springframework.core.env.AbstractPropertyResolver#doResolvePlaceholders
   解析值org.springframework.util.PropertyPlaceholderHelper#parseStringValue
   2）配置扫描，扫描bean 定义然后注册他们
   创建ClassPathBeanDefinitionScanner 
   BeanDefinitionDefaults 默认定义bean的属性
   BeanNameGenerator =AnnotationBeanNameGenerator 
   ScopeMetadataResolver=AnnotationScopeMetadataResolver，代理模式为NO->org.springframework.context.annotation.ScopedProxyMode
   注册默认的过滤器
     注入了两个，一个AnnotationTypeFilter-》Component.class
               一个AnnotationTypeFilter-》ClassUtils.forName("javax.annotation.ManagedBean", cl)
   设置默认的BeanDefinitionDefaults
   设置AutowireCandidatePatterns
       			org.springframework.context.annotation.ComponentScanBeanDefinitionParser#parseBeanNameGenerator
   org.springframework.context.annotation.ComponentScanBeanDefinitionParser#parseScope
      org.springframework.context.annotation.ComponentScanBeanDefinitionParser#parseTypeFilters
    3)开始扫描
 org.springframework.context.annotation.ClassPathBeanDefinitionScanner#doScan
     
     扫描包下的所有文件org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider#findCandidateComponents
     
     AnnotationMetadataReadingVisitor
     3.1）查找候选的bean
      ① 将文件缓存到metadataReaderCache里面
     
     将满足的文件类org.springframework.context.annotation.ScannedGenericBeanDefinition，设置为beanDefinition
      ②  将满足的bean放在Set<BeanDefinition> candidates，配置了@Configuration，@Service等的类   
     3.2）开始注册bean 
       ① 设置BeanDefinition的scope，例如singlton
       ② 设置beanName 例如AsyncConfiguration
       ③ postProcessBeanDefinition 设置beanDefinitionDefaults默认属性
       ④ 处理设置通用属性，一般为空org.springframework.context.annotation.AnnotationConfigUtils#processCommonDefinitionAnnotations(org.springframework.beans.factory.annotation.AnnotatedBeanDefinition)
       ⑤ 校验候选bean，判断是否需要注册，或者与已有的冲突，查看工程this.beanDefinitionMap.containsKey(beanName)是否包含bean
       ⑥ 校验通过
       a、则新建BeanDefinitionHolder
       b、设置代理模式，如果有代理设置引用
        c、Set<BeanDefinitionHolder> beanDefinitions增加该对象
        d、注册bean           org.springframework.beans.factory.support.BeanDefinitionReaderUtils#registerBeanDefinition
        启动阶段，直接赋值
        this.beanDefinitionMap.put(beanName, beanDefinition);
		this.beanDefinitionNames.add(beanName);
		this.manualSingletonNames.remove(beanName);
有引用的话注册引用
    返回所有的BeanDefinitionHolder
    
    4）注册组件
org.springframework.context.annotation.ComponentScanBeanDefinitionParser#registerComponents
     4.1）生成CompositeComponentDefinition
         将步骤3返回的beandefinitionhold放在组件bean中
     4.2）默认基础组件的bean注入到beanDefinitionMap中，且设置组件到组口beam中，一共6个
     
     4.3）通知组件注册成功readerContext.fireComponentRegistered(compositeDef)
		
	最后设置工厂
	返回共厂
	

3、准备bean工厂为上下文的使用
	
    
		
       
```





### 类解释

```
beanNameGenerator 生成bean的名字
```



```
DependsOn注解
```





```
ScopedProxyMode代理模式
```





```
BeanDefinitionParserDelegate bean定义解析器
作用：解析xml，定义beanDefinition

1、解析元素成为一个beanDefiniton：org.springframework.beans.factory.xml.BeanDefinitionParserDelegate#parseBeanDefinitionElement(org.w3c.dom.Element, java.lang.String, org.springframework.beans.factory.config.BeanDefinition)
然后返回一个BeanDefinitionHolder
例如：
 <bean class="example.util.redislock.SpringContextHolder" id="a" class="b,;d,;e"/>

逻辑：
1）获取id和name属性
2）设置引用别名aliases=设置引用为name以",;"分割的数数组
3）设置beanName =id ,如果id为空，即beanName为空，且name属性不为空，设置beanName为aliases的第一个值，例如例子中的b，且引用去掉第一个
4）校验beanName 唯一性，
    设置名字是否找到，String foundName = null;
    如果beanName不为空，且usedNames使用的名字包含了，则设置找到的名字为foundName = beanName;
    如果没找到，将引用的名字aliases取和usedNames使用过的名字里面只要有一个找到了，就则设置找到了
    foundName有值，找到了有重复的则报错，不能有一样的
    
  设置使用  this.usedNames.add(beanName);
		   this.usedNames.addAll(aliases);
5）调用重载方法AbstractBeanDefinition parseBeanDefinitionElement(
			Element ele, String beanName, BeanDefinition containingBean) 
     有beanName的 
      
      5.1）设置parseState
      5.2）设置calss、parent属性
      5.3）初始化一个AbstractBeanDefinition
      5.4）解析属性
      5.5）设置描述信息description
      5.6）解析元元素，子节点包含meta属相
      5.6）解析当前元素的子元素，解析查找覆盖属性，LookupOverrideSubElements，子元素包含lookup-method属性
      5.7）解析当前元素的子元素，解替换方法属性，parseReplacedMethodSubElements，子元素包含replaced-method属性
      5.8）解析当前元素的子节点中包含的构造函数参数constructor-arg，parseConstructorArgElements
      5.9）解析当前元素的子节点中包含property属性参数property，parsePropertyElements
      5.10）解析前元素的子节点中包含qualifier属性参数qualifier，parseQualifierElements
      5.11)设置资源
      5.12）设置源
6）如果beanName为空
   6.1）生成一个beanName
   
7）如果beanName不为空，则新建一个new BeanDefinitionHolder(beanDefinition, beanName, aliasesArray)返回即可


2、点缀beanDefiniton
org.springframework.beans.factory.xml.BeanDefinitionParserDelegate#decorateBeanDefinitionIfRequired(org.w3c.dom.Element, org.springframework.beans.factory.config.BeanDefinitionHolder, org.springframework.beans.factory.config.BeanDefinition)
添加bean对象的属性信息

3、解析beanDefinition属性
org.springframework.beans.factory.xml.BeanDefinitionParserDelegate#parseBeanDefinitionAttributes
根据元素节点，beanName，初始化抽象的AbstractBeanDefinition
解析所有的属相，返回AbstractBeanDefinition
1）不能使用废弃的老的1.x版本singleton属性，应该使用scope属性
2）设置scope属性，abstract，lazy-init，factory-method，factory-bean等各种属性 

4、解析元节点
org.springframework.beans.factory.xml.BeanDefinitionParserDelegate#parseMetaElements
根据节点，设置元节点的信息
1）获取当前节点的所有子节点
2）循环子节点的属相，如果包含meta属性，则设置BeanDefinition设置meta属相

5、解析指定元素的子元素中包含constructor-arg参数
org.springframework.beans.factory.xml.BeanDefinitionParserDelegate#parseConstructorArgElements
无返回值，设置beanDefiniton的构造参数属相
1）获取当前节点的所有子节点
2）循环子节点的属相，如果包含constructor-arg属性，调用处理解析constructor-arg元素
parseConstructorArgElement
2.1）获取属性index，type，name
2.2.1）如果index不为空
  1、index<0，报错
  2、设置解析状态parseState加入ConstructorArgumentEntry实体
  3、解析属性值org.springframework.beans.factory.xml.BeanDefinitionParserDelegate#parsePropertyValue
  4、解析回来的属性值，设置type、name、source
  5、设置构造参数类指标增加一个addIndexedArgumentValue
  6、弹出this.parseState.pop();

2.2.2) 如果index为空
  1、设置解析状态parseState加入ConstructorArgumentEntry实体
  2、解析属性parsePropertyValue
  3、解析回来的属性值，设置type、name、source
  4、设置构造参数类指标增加一个addGenericArgumentValue
  5、弹出this.parseState.pop();


6、解析属性值constructor-arg,针对已经有了index属相，那么ref和value不能共存
 org.springframework.beans.factory.xml.BeanDefinitionParserDelegate#parsePropertyValue
解析元素的值，返回对象，或者设置值
<bean class="org.springframework.data.redis.connection.RedisNode">
                    <constructor-arg index="0" value="${sentinel.name3}"/>
                    <constructor-arg index="1" value="${sentinel.port3}"/>
                </bean>
  
逻辑：
1）判断当前子节点，不能包含除description和meta，超过一个子元素
2）获取当前节点的，ref属相和value属性
3）同时包含两个报错
4）如果只有ref属性
 4.1）ref值不能为空
 4.2）初始化一个RuntimeBeanReference(refName)返回ref参数
5）如果只包含value属性 
  设置一个TypedStringValue valueHolder存放值，返回valueHolder
6）否则设置subElement，解析parsePropertySubElement
子元素解析，有ref、bean
例子一：
<bean id="exampleBean" class="examples.ExampleBean">
<!-- constructor injection using the nested ref element -->
    <constructor-arg>
        <ref bean="anotherExampleBean"/>
        </constructor-arg>
        <!-- constructor injection using the neater ref attribute -->
    <constructor-arg ref="yetAnotherBean"/>
    <constructor-arg type="int" value="1"/>
</bean>

例子二：
<bean id="serverFactory" class="org.eclipse.jetty...WebSocketServerFactory">
<constructor-arg>
    <bean class="org.eclipse.jetty...WebSocketPolicy">
        <constructor-arg value="SERVER"/>
        <property name="inputBufferSize" value="8092"/>
        <property name="idleTimeout" value="600000"/>
    </bean>
</constructor-arg>
</bean>

   7）都没有则报错

7、解析子元素的属相值
parsePropertySubElement

1）不是默认命名空间的，自定义的元素解析
2）解析bean属性
3）解析ref属性
4）解析idref
5）value
6）null
7）array
8）list
9）set
10）map
11）props


8、解析属性节点元素
org.springframework.beans.factory.xml.BeanDefinitionParserDelegate#parsePropertyElement
给当前节点设置属性值
逻辑：
1）获取name的属相值
2）this.parseState.push(new PropertyEntry(propertyName));
3）判断点钱节点是否已经包含了name属性，包含则报错，不能重复
4）解析属性值给定属相名name的值，调用parsePropertyValue
5）解析元属性parseMetaElements
6）设置属相值
7）this.parseState.pop();

9、解析给定节点的qualifier节点
1）获取当前节点的所有子节点
2）循环处理带有qualifier属性的子节点，解析qualifier节点parseQualifierElement

10、解析qualifier节点
org.springframework.beans.factory.xml.BeanDefinitionParserDelegate#parseQualifierElement
例如：
<bean class="example.SimpleMovieCatalog">
    <qualifier type="MovieQualifier">
        <attribute key="format" value="VHS"/>
        <attribute key="genre" value="Action"/>
    </qualifier>
    <!-- inject any dependencies required by this bean -->
</bean>
1）获取type属性值，没有则报错
2）设置this.parseState.push(new QualifierEntry(typeName));
3）新建一个AutowireCandidateQualifier  qualifier
4）设置qualifier的属性值，value对应的值
5）循环子节点，解析attribute标签的节点
   5.1）获取attribute标签的属相值，key和value
   5.2）如果没有，则报错
   5.3）都有值则新建一个BeanMetadataAttribute
   5.4) 设置qualifier属性值
6）给beanDefiniton设置qualifier值
7）this.parseState.pop();

11、
decorateIfRequired
org.springframework.beans.factory.xml.BeanDefinitionParserDelegate#decorateIfRequired

里面中的“spring.handlers”文件中的对象解析

```



```
SourceExtractor

```

```
BeanDefinitionReaderUtils
1、org.springframework.beans.factory.support.BeanDefinitionReaderUtils#createBeanDefinition
根据bString parentName, String className, ClassLoader classLoader
返回一个AbstractBeanDefinition
逻辑：
   1）初始化一个GenericBeanDefinition，设置空的构造函数值ConstructorArgumentValues和一个空的属性值列表MutablePropertyValues
   2）设置parentName
   3）设置classLoader不为空，设置setBeanClass，调用ClassUtils.forName(className, classLoader)
      为空只是设置名字
      

```



```
DefaultBeanDefinitionDocumentReader

org.springframework.beans.factory.xml.DefaultBeanDefinitionDocumentReader#processBeanDefinition
处理过程：1、调用BeanDefinitionParserDelegate，解析成一个bean
        2、点缀beanDefinition的内容
        3、注册bean
        4、注册组件，并同时对象

```



```
DefaultListableBeanFactory
1、注册对象
org.springframework.beans.factory.support.DefaultListableBeanFactory#registerBeanDefinition
核心
        this.beanDefinitionMap.put(beanName, beanDefinition);
		this.beanDefinitionNames.add(beanName);
		this.manualSingletonNames.remove(beanName);


```





# 关于Spring 的Classpath: 和 Classpath*: 源码解析

https://blog.csdn.net/zzrdark_/article/details/60576661

解析资源的方式:

org.springframework.core.io.support.PathMatchingResourcePatternResolver#getResources



```
Launcher
```

资源加载器，appClassLoader



### DTD与XSD区别

### #### DTD(Document Type Definition)

>   DTD(Document Type Definition)文档类型定义，是一种XML约束模式语言，是XML文件的验证机制，可以通过比较XML文档和DTD文件来看文档是否符合规范，元素和标签使用是否正确。一个DTD文档包含：元素的定义规则，元素间关系的定义规则，元素可使用的属性，可使用的实体或符号规则。
>
>

### #### XSD(XML Schemas Definition)

>  XSD(XML Schemas Definition), XML Schema 描述了XML文档的结构，可以用一个指定的XML Schema来验证某个XML文档，以检查该XML文档是否符合其要求。XML Schema本身就是一个XML文档，可以通过XML解析器解析它。



分析了一个aop标签如何解析成一个BeanDefinition http://www.mamicode.com/info-detail-1793847.html
