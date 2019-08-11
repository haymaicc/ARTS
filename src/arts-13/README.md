# Algorithm
> https://leetcode.com/problems/number-of-1-bits/description/

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        while(n != 0 ){
            n= n & (n-1);
            count++;
        }
        return count;
    }
}
```

这个题目的大意是： 给定一个无符号的整数， 返回其用二进制表式的时候的1的个数。

这里用一个trick， 可以轻松求出。 就是n & (n - 1) 可以消除 n 最后的一个1的原理。

为什么能消除最后一个1， 其实也比较简单，大家自己想一下

这样我们可以不断进行n = n & (n - 1)直到n === 0 ， 说明没有一个1了。 这个时候我们消除了多少1变成一个1都没有了， 就说明n有多少个1了

# Review
> https://aaronparecki.com/2018/07/07/7/oauth-for-the-open-web
> http://seanthefish.com/2018/07/13/oauth-guide-23/index.html

## OAuth教程--IndieAuth

todo


# Tips
> https://blog.csdn.net/u011974987/article/details/51027795

## Java 四种线程池的用法分析

Java通过Executors提供四种线程池，分别为：

* newCachedThreadPool创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。
* newFixedThreadPool 创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。
* newScheduledThreadPool 创建一个定长线程池，支持定时及周期性任务执行。
* newSingleThreadExecutor 创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。

### newCachedThreadPool

创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。示例代码如下：

```java
ExecutorService cachedThreadPool = Executors.newCachedThreadPool();
    for (int i = 0; i < 10; i++) {
        final int index = i;
    try {
        Thread.sleep(index * 1000);
    } 
        catch (InterruptedException e) {
            e.printStackTrace();
    }

cachedThreadPool.execute(new Runnable() {

@Override
public void run() {
    System.out.println(index);
}
});
}
```

线程池为无限大，当执行第二个任务时第一个任务已经完成，会复用执行第一个任务的线程，而不用每次新建线程。

### newFixedThreadPool

创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。示例代码如下：

```java
ExecutorService fixedThreadPool = Executors.newFixedThreadPool(3);
    for (int i = 0; i < 10; i++) {
    final int index = i;

    fixedThreadPool.execute(new Runnable() {

@Override
public void run() {
try {
    System.out.println(index);
    Thread.sleep(2000);
} catch (InterruptedException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
    }
}
});
}
```

因为线程池大小为3，每个任务输出index后sleep 2秒，所以每两秒打印3个数字。

定长线程池的大小最好根据系统资源进行设置。如Runtime.getRuntime().availableProcessors()。可参考PreloadDataCache。

### newScheduledThreadPool

创建一个定长线程池，支持定时及周期性任务执行。延迟执行示例代码如下：

```java
ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(5);
 scheduledThreadPool.schedule(new Runnable() {

@Override
public void run() {
    System.out.println("delay 3 seconds");
}
}, 3, TimeUnit.SECONDS);
```
表示延迟3秒执行。

定期执行示例代码如下：

```java
scheduledThreadPool.scheduleAtFixedRate(new Runnable() {

@Override
public void run() {
    System.out.println("delay 1 seconds, and excute every 3 seconds");
}
}, 1, 3, TimeUnit.SECONDS);
```

表示延迟1秒后每3秒执行一次。

ScheduledExecutorService比Timer更安全，功能更强大

### newSingleThreadExecutor
创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。示例代码如下：

```java
ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();
for (int i = 0; i < 10; i++) {
final int index = i;
singleThreadExecutor.execute(new Runnable() {

@Override
public void run() {
    try {
        System.out.println(index);
    Thread.sleep(2000);
} catch (InterruptedException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
        }
}
    });
}
```
结果依次输出，相当于顺序执行各个任务。

现行大多数GUI程序都是单线程的。Android中单线程可用于数据库操作，文件操作，应用批量安装，应用批量删除等不适合并发但可能IO阻塞性及影响UI线程响应的操作。

# Share
> https://mp.weixin.qq.com/s/gWNqNS46keB3w7eNR9DWMQ

判断要不要做一件事，可以看能不能把它写进年终总结里。
