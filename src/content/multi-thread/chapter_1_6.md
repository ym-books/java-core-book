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

## 线程优先级的继承性

在Java中，线程的优先级具有继承性，在A线程中，创建启动B线程，那么B线程的优先级和A线程的优先级一样。看下面的演示示例：

```java
package com.ymu.javase.thread.priority;

//线程优先级的继承性
public class PriorityInheritanceDemo {

    public static void main(String[] args) {
        System.out.println("main线程优先级：" + Thread.currentThread().getPriority());
        Thread.currentThread().setPriority(8); //主线程优先级设置
        Thread1 thread1 = new Thread1();
//        thread1.setPriority(6);
        thread1.start();
    }
}

class Thread1 extends Thread {

    @Override
    public void run() {
        System.out.println("我是线程Thread1，我的优先级是：" + this.getPriority());
        //创建启动线程Thread2
        Thread2 thread2 = new Thread2();
        thread2.start();
    }
}
class Thread2 extends Thread {

    @Override
    public void run() {
        System.out.println("我是线程Thread，我的优先级是：" + this.getPriority());
    }
}

```
运行上面main方法输出：
```text
main线程优先级：5
我是线程Thread1，我的优先级是：8
我是线程Thread，我的优先级是：8
```
默认情况下，main线程优先级设置为5。当我们重新设置main线程为8的时候，再启动线程Thread1，那么Thread1的线程将和main线程的优先级一样，为8，而线程Thread1中启动了Thread2，所以同样的优先级也为8。

下面打开注释代码：`thread1.setPriority(6);`,把线程Thread1的优先级设置为6，那么它启动的线程应该也为6，而不是8。运行结果如下：
```text
main线程优先级：5
我是线程Thread1，我的优先级是：6
我是线程Thread，我的优先级是：6
```

## 优先级具有规则性


