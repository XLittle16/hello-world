Maven学习官网

http://maven.apache.org/guides/introduction/introduction-to-optional-and-excludes-dependencies.html



# maven引入顺序

1、如果一个版本为1.2先导入，1.3的或者更高的会被遗弃



# maven可选依赖（Optional Dependencies）和依赖排除（Dependency Exclusions）

https://blog.csdn.net/ado1986/article/details/39547839

 一、可选依赖

当一个项目A依赖另一个项目B时，项目A可能很少一部分功能用到了项目B，此时就可以在A中配置对B的可选依赖。举例来说，一个类似hibernate的项目，它支持对mysql、oracle等各种数据库的支持，但是在引用这个项目时，我们可能只用到其对mysql的支持，此时就可以在这个项目中配置可选依赖。

 二、依赖排除

​    当一个项目A依赖项目B，而项目B同时依赖项目C，如果项目A中因为各种原因不想引用项目C，在配置项目B的依赖时，可以排除对C的依赖。

​    示例（假设配置的是A的pom.xml，依赖关系为：A --> B; B --> C）：
