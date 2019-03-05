## **线程**



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



![con life](D:\project\justnote\img\con life.png)

**wait/notify**

- wait 让当前线程进入等待状态，并且释放锁
- notify 唤醒任意一个正在等待锁的线程，并且让它得到锁
- notifyAll 唤醒所有等待对象锁的线程，如果有多个线程都被唤醒，那么锁将会被他们争夺，同一时间只会有一个线程得到锁





**非线程安全**

存在多个线程对同一个对象中的实例变量进行并发访问控制由此导致的数据脏读



## 线程池

- 减少创建和销毁线程的次数，重复使用线程

- 根据系统的承受能力，调整线程池中工作线程的数量，防止消耗过多内存导致服务器崩溃



**ThreadPoolExecutor**

- corePoolSize：默认情况下，在创建了线程池后，线程池中的线程数为0，当有任务来之后，就会创建一个线程去执行任务，当线程池中的线程数目达到corePoolSize后，就会把到达的任务放到缓存队列当中
- maximumPoolSize：表示在线程池中最多能创建多少个线程
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





- 线程池线程数 < corePoolSize，来一个任务，就创建一个线程去执行这个任务
- 线程池线程数 >= corePoolSize，来一个任务，尝试将它添加到任务缓存队列，如果添加成功，就等待空闲线程将它取出，否则（一般是任务缓存队列满了），就会尝试创建新线程执行这个任务
- 线程池线程数 = maximumPoolSize，采取任务拒绝策略
- 线程池线程数 > corePoolSize，如果线程空闲时间超过keepAliveTime，就会被终止，直到线程池线程数 < corePoolSize





- 线程池的关闭
  - shutdown()：不会立即终止线程池，等缓存队列中的任务执行完才终止，且不接受新任务
  - shutdownNow()：立即终止线程池，尝试打断正在执行的任务，清空任务缓存队列，返回尚未执行的任务


**配置线程池大小**

- CPU密集型任务，参考值为 *N*CPU+1
- IO密集型任务，参考值为2**N*CPU



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



## Concurrency

- You are probably familiar with multitasking—your operating system’s ability tohave more than one program working at what seems like the same time. For example, you can print while editing or downloading your email. Nowadays, you are likely to have a computer with more than one CPU, but the number of concurrently executing processes is not limited by the number of CPUs. The operating system assigns CPU time slices to each process, giving the impression of parallel activity. 
- Multithreaded programs extend the idea of multitasking by taking it one level lower: Individual programs will appear to do multiple tasks at the same time. Each task is usually called a thread, which is short for thread of control. Programs that can run more than one thread at once are said to be multithreaded. 
- The essential difference is that while each process has a complete set of its own variables, threads share the same data.  
- shared variables make communication between threads more effcient and easier to program than interprocess communication. Moreover, on some operating systems, threads are more “lightweight” than processes—it takes less overhead to create and destroy individual threads than it does to launch new processes. 
- Multithreading is extremely useful in practice. For example, a browser should be able to simultaneously download multiple images. A web server needs to be able to serve concurrent requests. Graphical user interface (GUI) programs have a separate thread for gathering user interface events from the host operating environment. 



### Interrupting Threads

- A thread terminates when its run method returns—by executing a return statement, after executing the last statement in the method body, or if an exception occurs that is not caught in the method. In the initial release of Java, there also was a stop method that another thread could call to terminate a thread. However, that method is now deprecated.  
- When the interrupt method is called on a thread, the interrupted status of the thread is set. This is a boolean ﬂag that is present in every thread. Each thread should occasionally check whether it has been interrupted. 
- However, if a thread is blocked, it cannot check the interrupted status. This is where the InterruptedException comes in. When the interrupt method is called on a thread that blocks on a call such as sleep or wait, the blocking call is terminated by an InterruptedException. 
- There is no language requirement that a thread which is interrupted should terminate. Interrupting a thread simply grabs its attention. The interrupted thread can decide how to react to the interruption. Some threads are so important that they should handle the exception and continue. But quite commonly, a thread will simply want to interpret an interruption as a request for termination.  



### Thread States

- Threads can be in one of six states:
  - New
  - Runnable
  - Blocked
  - Waiting
  - Timed waiting
  - Terminated



**New Threads**

- When you create a thread with the new operator—for example, new Thread(r)—the thread is not yet running. This means that it is in the new state. When a thread is in the new state, the program has not started executing code inside of it. A certain amount of bookkeeping needs to be done before a thread can run. 



**Runnable Threads**

- Once you invoke the start method, the thread is in the runnable state. A runnable thread may or may not actually be running. It is up to the operating system to give the thread time to run. 
- Once a thread is running, it doesn’t necessarily keep running. In fact, it is desirable that running threads occasionally pause so that other threads have a chance to run. The details of thread scheduling depend on the services that the operating system provides. Preemptive scheduling systems give each runnable thread a slice of time to perform its task. When that slice of time is exhausted, the operating system preempts the thread and gives another thread an opportunity to work . When selecting the next thread, the operating system takes into account the thread priorities 
- All modern desktop and server operating systems use preemptive scheduling. However, small devices such as cell phones may use cooperative scheduling. In such a device, a thread loses control only when it calls the yield method, or when it is blocked or waiting. 
- Always keep in mind that a runnable thread may or may not be running at any given time. (This is why the state is called “runnable” and not “running.”) 



**Blocked and Waiting Threads**

- When a thread is blocked or waiting, it is temporarily inactive. It doesn’t execute any code and consumes minimal resources. It is up to the thread scheduler to reactivate it. The details depend on how the inactive state was reached. 

  - When the thread tries to acquire an intrinsic object lock (but not a Lock in the java.util.concurrent library) that is currently held by another thread, it becomes blocked. The thread becomes unblocked when all other threads have relinquished the lock and the thread scheduler has allowed this thread to hold it. 

  - When the thread waits for another thread to notify the scheduler of a condition, it enters the waiting state. This happens by calling the Object.wait or Thread.join method, or by waiting for a Lock or Condition in the java.util.concurrent library. In practice, the difference between the blocked and waiting state is not signifcant. 

  - Several methods have a timeout parameter. Calling them causes the thread to enter the timed waiting state. This state persists either until the timeout expires or the appropriate notifcation has been received. Methods with timeout include Thread.sleep and the timed versions of Object.wait, Thread.join, Lock.tryLock, and Condition.await. 

  - When a thread is blocked or waiting (or, of course, when it terminates), another thread will be scheduled to run. When a thread is reactivated (for example, because its timeout has expired or it has succeeded in acquiring a lock), the scheduler checks to see if it has a higher priority than the currently running threads. If so, it preempts one of the current threads and picks a new thread to run. 

    ![Thread states](D:\project\justnote\img\Thread states.png)



**Terminated Threads**

- A thread is terminated for one of two reasons:
  - It dies a natural death because the run method exits normally.
  - It dies abruptly because an uncaught exception terminates the run method.
- In particular, you can kill a thread by invoking its stop method. That method throws a ThreadDeath error object that kills the thread. However, the stop method is deprecated, and you should never call it in your own code. 



### Thread Properties

**Thread Priorities**

- In the Java programming language, every thread has a priority. By default, a thread inherits the priority of the thread that constructed it. You can increase or decrease the priority of any thread with the setPriority method. You can set the priority to any value between MIN_PRIORITY (defned as 1 in the Thread class) and MAX_PRIORITY (defned as 10). NORM_PRIORITY is defned as 5. 
- If you have several threads with a high priority that don’t become inactive, the lower-priority threads may never execute. Whenever the scheduler decides to run a new thread, it will choose among the highest-priority threads frst, even though that may starve the lower-priority threads completely. 



**Handlers for Uncaught Exceptions**

- The run method of a thread cannot throw any checked exceptions, but it can be terminated by an unchecked exception. In that case, the thread dies.

- However, there is no catch clause to which the exception can be propagated. Instead, just before the thread dies, the exception is passed to a handler for uncaught exceptions. 



Synchronization

Blocking Queues

Thread-Safe Collections

Callables and Futures

Executors

Synchronizers

Threads and Swing



