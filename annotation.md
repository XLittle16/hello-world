1、类型为@interface

2、
```java
@Target(FIELD)
@Retention(RUNTIME)
@Documented
public @interface FruitColor {

	Color color() default Color.GREEN;
}
```

3、RetentionPolicy.RUNTIME 多数使用，这样才能反射拿到，做相应的动作
默认为CLASS 编译时使用，装入jvm就没有了 @UsesJava8
SOURCE只在源码有，可当一个提示 @Override
