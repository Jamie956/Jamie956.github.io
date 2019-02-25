## **线程**

**定义**

在进程中独立运行的子任务

一个进程在其执行的过程中可以产生多个线程，同一个进程中的线程共享其进程中的内存和资源（共享的内存是堆内存和方法区内存，栈内存不共享），相比进程，线程之间的切换效率较高



**生命周期**

1. **新建(new)**：创建Thread实例时，此线程进入新建状态（未被启动）

   ```Thread t1 = new Thread();```

2. **就绪(runnable)**：线程已经被启动，正在等待被分配给CPU时间片，此时线程正在就绪队列中排队等候得到CPU资源

   ```t1.start();```

3. **运行(running)**：线程获得CPU资源正在执行任务，一直运行到结束，除非自动放弃CPU资源或者有优先级更高的线程进入

4. **阻塞(block)**：由于某种原因导致正在运行的线程让出CPU并暂停自己的执行

      1.  sleep：在指定时间后可进入就绪状态
      2.  wait：线程放入等待队列(waitting queue)中

5. **死亡(dead)**：线程执行完毕或被其它线程杀死时，进入死亡状态，这时不能再进入就绪状态

   1.  自然终止：正常运行run()方法后终止
   2.  异常终止：调用**stop()**方法让一个线程终止运行



<img src="https://camo.githubusercontent.com/5b764ff5af6204f82c7ae6237b20c41f9505aef8/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f392f313635316631396437633465393361333f773d38373626683d34393226663d706e6726733d313238303932" alt="https://camo.githubusercontent.com/5b764ff5af6204f82c7ae6237b20c41f9505aef8/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f382f392f313635316631396437633465393361333f773d38373626683d34393226663d706e6726733d313238303932" class="transparent">



**wait/notify**

- wait 让当前线程进入等待状态，并且释放锁
- notify 唤醒任意一个正在等待锁的线程，并且让它得到锁
- notifyAll 唤醒所有等待对象锁的线程，如果有多个线程都被唤醒，那么锁将会被他们争夺，同一时间只会有一个线程得到锁



**常用方法**

- `hread.currentThread()` 静态方法，获取当前线程对象；
- `isAlive()` 判断线程是否处于活动状态，即线程已启动且尚未终止；
- `Thread.sleep(long)` 在指定的毫秒数内让当前线程休眠，需要`catch InterruptedException`；
- `Thread.interrupted()` 判断该线程是否中断，执行后将清除中断标志；
- `isInterrupted()` 测试线程是否已经中断；
- `suspend()   resume()  stop()` 暂停、开始和结束线程，不应该使用。暂停方法不会释放资源；
- `yield()` 该线程放弃当前CPU资源，放弃后马上进行CPU竞争；
- `setPriority()`优先级具有继承性，即子线程有父线程的优先级；
  - 高优先级的线程总是大部分先执行完，但不代表高优先级将全部先完成；
  - 优先级较高的不一定每一次都先执行完，具有随机性；
  - 具体的与OS相关；
- `setDaemon()` 设置守护线程，当进程不存在非守护线程时则守护线程将销魂而后进程销毁；



**非线程安全**

存在多个线程对同一个对象中的实例变量进行并发访问控制由此导致的数据脏读；



## 线程池

- 减少创建和销毁线程的次数，重复使用线程

- 根据系统的承受能力，调整线程池中工作线程的数量，防止消耗过多内存导致服务器崩溃



**ThreadPoolExecutor**

- corePoolSize：默认情况下，在创建了线程池后，线程池中的线程数为0，当有任务来之后，就会创建一个线程去执行任务，当线程池中的线程数目达到corePoolSize后，就会把到达的任务放到缓存队列当中
- maximumPoolSize：线程池最大线程数，表示在线程池中最多能创建多少个线程
- keepAliveTime：表示线程没有任务执行时最多保持多久时间会终止。默认情况下，只有当线程池中的线程数大于corePoolSize时，keepAliveTime才会起作用，直到线程池中的线程数不大于corePoolSize
- unit：参数keepAliveTime的时间单位
- workQueue：一个阻塞队列，用来存储等待执行的任务
- threadFactory：线程工厂，主要用来创建线程
- handler：超出线程范围和队列容量的任务的处理程序，任务拒绝策略
- poolSize：当前的线程数
- mainLock：状态锁，对线程池状态（比如线程池大小，runState等）的改变都要使用这个锁
- workers：用来存放工作集
- allowCoreThreadTimeOut：是否允许为核心线程设置存活时间
- largestPoolSize： 曾经出现过的最大线程数
- completedTaskCount：执行完毕的任务个数

**实现原理**

- 线程池状态
- 任务的执行
  - 线程池线程数 < corePoolSize，来一个任务，就创建一个线程去执行这个任务
  - 线程池线程数 >= corePoolSize，来一个任务，尝试将它添加到任务缓存队列，如果添加成功，就等待空闲线程将它取出，否则（一般是任务缓存队列满了），就会尝试创建新线程执行这个任务
  - 线程池线程数 = maximumPoolSize，采取任务拒绝策略
  - 线程池线程数 > corePoolSize，如果线程空闲时间超过keepAliveTime，就会被终止，直到 线程池线程数 < corePoolSize

- 线程池中的线程初始化

- 任务缓存队列及排队策略

- 任务拒绝策略

  当线程池的任务缓存队列已满并且线程池中的线程数目达到maximumPoolSize，如果还有任务到来就会采取任务拒绝策略

```
ThreadPoolExecutor.AbortPolicy:丢弃任务并抛出RejectedExecutionException异常。
ThreadPoolExecutor.DiscardPolicy：也是丢弃任务，但是不抛出异常。
ThreadPoolExecutor.DiscardOldestPolicy：丢弃队列最前面的任务，然后重新尝试执行任务（重复此过程）
ThreadPoolExecutor.CallerRunsPolicy：由调用线程处理该任务 
```

- 线程池的关闭
  - shutdown()：不会立即终止线程池，等缓存队列中的任务执行完才终止，且不接受新任务
  - shutdownNow()：立即终止线程池，尝试打断正在执行的任务，清空任务缓存队列，返回尚未执行的任务
- 线程池容量的动态调整

**配置线程池大小**

- 如果是CPU密集型任务，就需要尽量压榨CPU，参考值可以设为 *N*CPU+1
- 如果是IO密集型任务，参考值可以设置为2**N*CPU



**创建**

```java
ThreadPoolExecutor.ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue)

threadPoolExecutor.execute(thread);
threadPoolExecutor.shutdown();

```



## 锁

**乐观锁**

> 每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据。乐观锁适用于多读的应用类型，提高吞吐量

**CAS**

> CAS是乐观锁技术，当多个线程尝试使用CAS同时更新同一个变量时，只有其中一个线程能更新变量的值，而其它线程都失败，失败的线程并不会被挂起，而是被告知这次竞争中失败，并可以再次尝试。



> CAS 操作中包含三个操作数 —— 需要读写的内存位置（V）、进行比较的预期原值（A）和拟写入的新值(B)。如果内存位置V的值与预期原值A相匹配，那么处理器会自动将该位置值更新为新值B。否则处理器不做任何操作。

**悲观锁**

> 总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。Java的synchronized关键字的实现也是悲观锁。

**存在问题**

1. 在多线程竞争下，加锁、释放锁会导致比较多的上下文切换和调度延时，引起性能问题
2. 一个线程持有锁会导致其它所有需要此锁的线程挂起
3. 如果一个优先级高的线程等待一个优先级低的线程释放锁会导致优先级倒置，引起性能风险

