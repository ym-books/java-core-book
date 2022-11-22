# 守护线程

在Java中有两种线程，一种是用户线程，另外一种就是守护线程。

用户线程，就是系统工作线程，用来完成某种业务的操作。  

守护线程是一种特殊的线程。它的特性有”陪伴“的含义，是系统的守护者，驻守后台。比如垃圾回收线程、JIT线程就可以理解为守护线程。    

当进程中没有非守护线程了，也就是只有守护线程了，那就意味着这个应用程序实际上无事可做了，守护线程要守护的对象也不复存在了，那么整个程序就会自然的结束，Java虚拟机终止运行。

下面演示例子：

新建下面线程类如下：

```java
public class MyThread extends Thread {
    private int i;

    @Override
    public void run() {
        try {
            while (true) {
                i++;
                System.out.println("i=" + i);
                Thread.sleep(1000);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class DaemonDemo {

    public static void main(String[] args) throws InterruptedException {
        MyThread myThread = new MyThread();
        myThread.setName("daemon"); //设置成守护线程
        myThread.setDaemon(true); //设置为守护线程
        myThread.start();
        Thread.sleep(5000);
        System.out.println("离开线程了，守护线程也要停止了，就不再打印了");
    }
}
```
执行上面main方法，运行结果：
```text
i=1
i=2
i=3
i=4
i=5
离开线程了，守护线程也要停止了，就不再打印了

Process finished with exit code 0
```
从上面运行结果，我们可以看到，当main线程休眠挂起，还没执行完的时候。守护线程一直在执行打印。直到main线程执行结束后，守护线程也终止打印。然后应用中没有其他在执行的用户线程了，所以虚拟机退出，程序运行结束。


下面，我们添加一个用户线程，并让用户线程一直执行，看看守护线程会咋样。 

```java
class BThread extends Thread {

    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(1000);
                System.out.println("我是用户线程，永不停止");
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

修改main方法:
```java
public class DaemonDemo {

    public static void main(String[] args) throws InterruptedException {
        MyThread myThread = new MyThread();
        myThread.setName("daemon");
        myThread.setDaemon(true); //设置为守护线程
        myThread.start();

        BThread bThread = new BThread();
        bThread.setName("bthread");
        bThread.start();
    }
}

```

在mian中启动两个线程，一个设置为守护线程，另外一个是用户线程。用户线程不停执行。  

从运行结果，我们可以看到，守护线程也在不断的打印，从不停歇。那是因为进程中还有用户线程在执行。
