# Spring

## AOP
动态代理（AOP）：代理类在运行时才生成
静态代理：编译期就已经生成

```
方面（Aspect）：横切关注点
连接点（Joinpoint）：方法调用或者异常抛出时做增强
通知（Advice）
切入点（Pointcut）：一个通知将被引发的连接点集合
引入（Introduction）
目标对象（Target Object）
AOP代理（AOP Proxy）
织入（Weaving）：将增强后的AOP代理组装到系统

```

### 生成代理对象
#### JDKProxy

#### Cgli
基于字节码技术的，使用的是ASM。asm是一个java字节码操纵框架，它能被用来动态生成类或者增强既有类的功能。ASM可以直接产生二进制class文件，也可以在类被加载入JVM之前动态改变类行为。


## IOC

