# Java Concurrent

## Executor

线程池顶层接口:Executor

```java
public interface Executor {
    void execute(Runnable var1);
}
```

## ThreadPool

```java
public ThreadPoolExecutor(int corePoolSize,int maximumPoolSize,long keepAliveTime,TimeUnit unit,
        BlockingQueue<Runnable> workQueue,ThreadFactory threadFactory,RejectedExecutionHandler handler){}

//corePoolSize 核心线程最大值
//maximumPoolSize 线程总数最大值
//keepAliveTime 非核心线程闲置超时时长 设置allowCoreThreadTimeOut(true)，则会也作用于核心线程。
//unit keepAliveTime的单位
//BlockingQueue 阻塞队列
//threadFactory 线程工厂
//handler
```

