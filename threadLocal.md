* 一个线程有多个线程本地的变量，会出现hash冲突的问题
* 因为TreadLocal自己定义了一个静态内部类ThreadLocalMap，自己实现了hashcode
* Thread1--ThreadLocal1+ThreadLocal2
* Thread2--ThreadLocal1+ThreadLocal2
* 建议一般只设计一个ThreadLocal变量，可以是使用类似Shiro框架的的使用方法，存储过个值
* private static final ThreadLocal<Map<Object, Object>> resources = new InheritableThreadLocalMap<Map<Object, Object>>();
* 存储了多个值


