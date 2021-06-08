# 多线程

## 要点

相比使用锁机制，并行算法和线程安全的数据结构更好

多线程在没有同步的情况下操作共享数据，结果不可预知

Runnable表示一个可以异步执行但不能返回结果的异步操作

Callable表示一个可以异步执行但不能返回结果的异步操作

ExecutorService将任务实例纳入执行计划

## 实现

1. 实现Runnable接口

```java
Runnable runnable=()->{};
```

## synchronized

### 应用方式

1. 同步实例方法:对于普通同步方法，锁是当前实例对象
2. 同步静态方法:对于静态同步方法，锁是当前类的 Class 对象
3. 同步代码块:对于同步方法块，锁是 synchronized 括号里配置的对象

### 原理

synchronized 代码块是由一对 monitorenter 和 monitorexit 指令实现的

synchronized 同步块对同一线程来说是可重入的，不会出现锁死问题。

synchronized 同步块是互斥的，即已进入的线程执行完成前，会阻塞其他试图进入的线程。

同步代码块:
synchronized 在修饰同步代码块时，是由 monitorenter 和 monitorexit 指令来实现同步的。
进入 monitorenter 指令后，线程将持有 Monitor 对象，退出 monitorenter 指令后，线程将释放该 Monitor 对象。

同步方法:
synchronized 修饰同步方法时，会设置一个 ACC_SYNCHRONIZED 标志。当方法调用时，调用指令将会检查该方法是否被设置 ACC_SYNCHRONIZED 访问标志。
如果设置了该标志，执行线程将先持有 Monitor 对象，然后再执行方法。在该方法运行期间，其它线程将无法获取到该 Mointor 对象,方法执行完成后，再释放该 Monitor 对象。
   