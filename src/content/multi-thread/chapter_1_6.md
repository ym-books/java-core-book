# 线程的优先级

Java中的线程可以设置自己的优先级。优先级高的线程在资源竞争的时候会更有优势，更容易抢占到资源从而优先执行。当然，这只是一个概率问题，不是说优先级高的就一定总是优先执行，也可能出现资源竞争失败的情况，优先级低的也可能一直抢占不到资源，从而造成线程饥饿，一直不会执行。所以，在严格的场景下，还是要在应用层自己设计线程的调度，不能靠优先级丢给系统管理。

Java中，线程的优先级分为1-10，共10个等级。如果设置成小于1或者大于10将会抛出非法参数异常。 

JDK中预置三个常量来定义优先级,数字越大，优先级越高：
```java
/**
 * The minimum priority that a thread can have.
 */
public static final int MIN_PRIORITY = 1;

/**
 * The default priority that is assigned to a thread.
 */
public static final int NORM_PRIORITY = 5;

/**
 * The maximum priority that a thread can have.
 */
public static final int MAX_PRIORITY = 10;
```

