# 并发

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

2.继承Thread类

```java
public class MyThread extends Thread {
    @Override
    void run() {
    }
}
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
synchronized 在修饰同步代码块时，是由 monitorenter 和 monitorexit 指令来实现同步的。 进入 monitorenter 指令后，线程将持有 Monitor 对象，退出 monitorenter
指令后，线程将释放该 Monitor 对象。

同步方法:
synchronized 修饰同步方法时，会设置一个 ACC_SYNCHRONIZED 标志。当方法调用时，调用指令将会检查该方法是否被设置 ACC_SYNCHRONIZED 访问标志。 如果设置了该标志，执行线程将先持有 Monitor
对象，然后再执行方法。在该方法运行期间，其它线程将无法获取到该 Mointor 对象,方法执行完成后，再释放该 Monitor 对象。

### 锁状态

| 锁状态  |29 bit 或 61 bit    |1 bit 是否是偏向锁  |2 bit 锁标志位|
|:-----  | :----            | :----            | :----      |
| 无锁     |	                | 0                |01            |
| 偏向锁     |线程ID             | 1               |01          |
| 轻量级锁 |指向栈中锁记录的指针    |                  |00          |
| 重量级锁 |指向互斥量（重量级锁）的指针|		       |10          |
| GC标记     |	                |    	           |11          |

偏向锁原理:

线程进入这个同步块时，会去检查锁的Mark Word里面是不是放的自己的线程ID。 如果是，表明该线程已经获得了锁，以后该线程在进入和退出同步块时不需要花费CAS操作来加锁和解锁；
如果不是，就代表有另一个线程来竞争这个偏向锁。这个时候会尝试使用CAS来替换Mark Word里面的线程ID为新线程的ID，这个时候要分两种情况： 成功，表示之前的线程不存在了，Mark
Word里面的线程ID为新线程的ID，锁不会升级，仍然为偏向锁； 失败，表示之前的线程仍然存在，那么暂停之前的线程，设置偏向锁标识为0，并设置锁标志位为00，升级为轻量级锁，会按照轻量级锁的方式进行竞争锁。
当其他线程尝试竞争偏向锁时，持有偏向锁的线程才会释放锁。

轻量锁原理:

将锁的Mark Word复制到自己的Displaced Mark Word里面，然后线程尝试用CAS将锁的Mark Word替换为指向锁记录的指针。 如果成功，当前线程获得锁，如果失败，表示Mark
Word已经被替换成了其他线程的锁记录，说明在与其它线程竞争锁，当前线程就尝试使用自旋来获取锁。 自旋到一定程度（和JVM、操作系统相关），依然没有获取到锁，称为自旋失败，那么这个线程会阻塞,同时这个锁就会升级成重量级锁。
在释放锁时，当前线程会使用CAS操作将Displaced Mark Word的内容复制回锁的Mark Word里面。
如果没有发生竞争，那么这个复制的操作会成功。如果有其他线程因为自旋多次导致轻量级锁升级成了重量级锁，那么CAS操作会失败，此时会释放锁并唤醒被阻塞的线程。

重量锁原理:

当一个线程尝试获得锁时，如果该锁已经被占用，则会将该线程封装成一个ObjectWaiter对象插入到Contention List的队列的队首，然后调用park函数挂起当前线程。 当线程释放锁时，会从Contention
List或EntryList中挑选一个线程Heir presumptive(假定继承人)唤醒，假定继承人被唤醒后会尝试获得锁。

## 多线程优势

1. 进程间的通信比较复杂，而线程间的通信比较简单，通常情况下，我们需要使用共享资源，这些资源在线程间的通信比较容易。
2. 进程是重量级的，而线程是轻量级的，多线程方式的系统开销更小。

## volatile

作用:保证变量的内存可见性 禁止volatile变量与普通变量重排序

内存可见性:当一个线程对volatile修饰的变量进行写操作时，JMM会立即把该线程对应的本地内存中的共享变量的值刷新到主内存；
当一个线程对volatile修饰的变量进行读操作时，JMM会把立即该线程对应的本地内存置为无效，从主内存中读取共享变量的值。

禁止重排序:通过**内存屏障**实现

内存屏障分两种：读屏障（Load Barrier）和写屏障（Store Barrier）

JMM内存屏障插入策略:

1. 在每个volatile写操作前插入一个StoreStore屏障,禁止前面普通写指令与volatile写重排序
2. 在每个volatile写操作后插入一个StoreLoad屏障,禁止volatile写指令与后面volatile读/写指令重排序
3. 在每个volatile读操作后插入一个LoadLoad屏障,禁止volatile读指令与后面普通读指令重排序
4. 在每个volatile读操作后再插入一个LoadStore屏障,禁止volatile写指令与后面普通读指令重排序

特例:

```java
int a=0; // 声明普通变量
volatile boolean flag=false; // 声明volatile变量

// 以下两个变量的读操作是可以重排序的(普通读->volatile读)
int i=a; // 普通变量读
boolean j=flag; // volatile变量读
```

对比锁:在内存可见性上，volatile有着与锁相同的内存语义，可以作为一个“轻量级”的锁来使用。 volatile仅保证对单个volatile变量的读/写具有原子性，而锁可以保证整个临界区代码的执行具有原子性。
在功能上，锁比volatile更强大；在性能上，volatile更有优势。

## 线程池

### 优点

1. 复用已创建的线程，降低线程创建与摧毁的消耗。
2. 控制并发的数量。并发数量过多，可能会导致资源消耗过多，从而造成服务器崩溃（主要原因）。
3. 统一管理线程。

### ThreadPoolExecutor状态

1. 线程池创建后处于RUNNING状态。
2. 调用shutdown()方法后处于SHUTDOWN状态，线程池不能接受新的任务，清除一些空闲worker,会等待阻塞队列的任务完成。
3. 调用shutdownNow()方法后处于STOP状态，线程池不能接受新的任务，中断所有线程，阻塞队列中没有被执行的任务全部丢弃。 此时，poolsize=0,阻塞队列的size也为0。
4. 当所有的任务已终止，ctl记录的”任务数量”为0，线程池会变为TIDYING状态。接着会执行terminated()函数。
5. 线程池处在TIDYING状态时，执行完terminated()方法之后，就会由 TIDYING -> TERMINATED， 线程池被设置为TERMINATED状态。

### 处理流程
1. 线程总数量 < corePoolSize，无论线程是否空闲，都会新建一个核心线程执行任务（让核心线程数量快速达到corePoolSize，在核心线程数量 < corePoolSize时）。注意，这一步需要获得全局锁。
2. 线程总数量 >= corePoolSize时，新来的线程任务会进入任务队列中等待，然后空闲的核心线程会依次去缓存队列中取任务来执行（体现了线程复用）。
3. 当缓存队列满了，说明这个时候任务已经多到爆棚，需要一些“临时工”来执行这些任务了。于是会创建非核心线程去执行这个任务。注意，这一步需要获得全局锁。
4. 缓存队列满了， 且总线程数达到了maximumPoolSize，则会采取上面提到的拒绝策略进行处理。

### 常见线程池

1. newCachedThreadPool:corePoolSize=0 maximumPoolSize=Integer.MAX_VALUE 适用于短时间多任务
2. newFixedThreadPool:corePoolSize=n maximumPoolSize=n 
3. newSingleThreadExecutor:  corePoolSize == maximumPoolSize=1 不会创建非核心线程,所有任务按照先来先执行的顺序执行
4. newScheduledThreadPool: 定长线程池，支持定时及周期性任务执行