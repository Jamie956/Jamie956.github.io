## Life Cycle



![con life](..\img\con life.png)



## Thread Pool

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



## Lock

**乐观锁**：每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据。乐观锁适用于多读的应用类型，提高吞吐量

**CAS**：CAS是乐观锁技术，当多个线程尝试使用CAS同时更新同一个变量时，只有其中一个线程能更新变量的值，而其它线程都失败，失败的线程并不会被挂起，而是被告知这次竞争中失败，并可以再次尝试。



CAS 操作中包含三个操作数 —— 需要读写的内存位置（V）、进行比较的预期原值（A）和拟写入的新值(B)。如果内存位置V的值与预期原值A相匹配，那么处理器会自动将该位置值更新为新值B。否则处理器不做任何操作。



**悲观锁**：总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。Java的synchronized关键字的实现也是悲观锁。



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

    ![Thread states](..\img\Thread states.png)



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



### Synchronization

- In most practical multithreaded applications, two or more threads need to share access to the same data. What happens if two threads have access to the same object and each calls a method that modifes the state of the object? As you might imagine, the threads can step on each other’s toes. Depending on the order in which the data were accessed, corrupted objects can result. Such a situation is often called a race condition. 



**The Race Condition Explained**

![Simultaneous access by two threads](..\img\Simultaneous access by two threads.png)



**Lock Objects**

- This construct guarantees that only one thread at a time can enter the critical section. As soon as one thread locks the lock object, no other thread can get past the lock statement. When other threads call lock, they are deactivated until the frst thread unlocks the lock object. 

  ```java
  myLock.lock(); // a ReentrantLock object
  try{
      critical section
  }
  finally{
      myLock.unlock(); // make sure the lock is unlocked even if an exception is thrown
  }
  ```

- It is critically important that the unlock operation is enclosed in a finally clause. If the code in the critical section throws an exception, the lock must be unlocked. Otherwise, the other threads will be blocked forever. 

- Suppose one thread calls transfer and gets preempted before it is done. Suppose a second thread also calls transfer. The second thread cannot acquire the lock and is blocked in the call to the lock method. It is deactivated and must wait for the frst thread to fnish executing the transfer method. When the frst thread unlocks the lock, then the second thread can proceed 

- Note that each Bank object has its own ReentrantLock object. If two threads try to access the same Bank object, then the lock serves to serialize the access. However, if two threads access different Bank objects, each thread acquires a different lock and neither thread is blocked. This is as it should be, because the threads cannot interfere with one another when they manipulate different Bank instances. 

- The lock is called reentrant because a thread can repeatedly acquire a lock that it already owns. The lock has a hold count that keeps track of the nested calls to the lock method. The thread has to call unlock for every call to lock in order to relinquish the lock. Because of this feature, code protected by a lock can call another method that uses the same locks. 

- In general, you will want to protect blocks of code that update or inspect a shared object, so you can be assured that these operations run to completion before another thread can use the same object. 

- Be careful to ensure that the code in a critical section is not bypassed by throwing an exception. If an exception is thrown before the end of the section, the finally clause will relinquish the lock, but the object may be in a damaged state. 



**Condition Objects**

- Often, a thread enters a critical section only to discover that it can’t proceed until a condition is fulflled. Use a condition object to manage threads that have acquired a lock but cannot do useful work.  

- It is entirely possible that the current thread will be deactivated between the successful outcome of the test and the call to transfer. 

  ```java
  if (bank.getBalance(from) >= amount)
      // thread might be deactivated at this point
      bank.transfer(from, to, amount);
  ```

- You must make sure that no other thread can modify the balance between the test and the transfer action. You do so by protecting both the test and the transfer action with a lock: 

  ```java
  public void transfer(int from, int to, int amount){
      bankLock.lock();
      try{
          while (accounts[from] < amount){
              // wait
              . . .
          }
          // transfer funds
          . . .
      }
      finally{
          bankLock.unlock();
      }
  }
  ```

- When a thread calls await, it has no way of reactivating itself. It puts its faith in the other threads. If none of them bother to reactivate the waiting thread, it will never run again. This can lead to unpleasant deadlock situations. If all other threads are blocked and the last active thread calls await without unblocking one of the others, it also blocks. No thread is left to unblock the others, and the program hangs. 

- When should you call signalAll? The rule of thumb is to call signalAll whenever the state of an object changes in a way that might be advantageous to waiting threads. For example, whenever an account balance changes, the waiting threads should be given another chance to inspect the balance. In our example, we call signalAll when we have fnished the funds transfer. 

  ```java
  public void transfer(int from, int to, int amount){
      bankLock.lock();
      try{
          while (accounts[from] < amount)
              sufficientFunds.await();
          // transfer funds
          . . .
              sufficientFunds.signalAll();
      }
      finally{
          bankLock.unlock();
      }
  }
  ```

- Note that the call to signalAll does not immediately activate a waiting thread. It only unblocks the waiting threads so that they can compete for entry into the object after the current thread has relinquished the lock. 



**The synchronized Keyword**

- let us summarize the key points about locks and conditions:

  - A lock protects sections of code, allowing only one thread to execute the code at a time.
  - A lock manages threads that are trying to enter a protected code segment.
  - A lock can have one or more associated condition objects.
  - Each condition object manages threads that have entered a protected code section but that cannot proceed. 

- If a method is declared with the synchronized keyword, the object’s lock protects the entire method.  

  ```java
  public synchronized void method(){
      method body
  }
  //is the equivalent of
  public void method(){
      this.intrinsicLock.lock();
      try{
          method body
      }
      finally { this.intrinsicLock.unlock(); }
  }
  ```

- As you can see, using the synchronized keyword yields code that is much more concise. Of course, to understand this code, you have to know that each object has an intrinsic lock, and that the lock has an intrinsic condition. The lock manages the threads that try to enter a synchronized method. The condition manages the threads that have called wait.

  ```java
  class Bank{
      private double[] accounts;
      public synchronized void transfer(int from, int to, int amount) throws InterruptedException{
          while (accounts[from] < amount)
              wait(); // wait on intrinsic object lock's single condition
          accounts[from] -= amount;
          accounts[to] += amount;
          notifyAll(); // notify all threads waiting on the condition
      }
      public synchronized double getTotalBalance() { . . . }
  }
  ```

- It is also legal to declare static methods as synchronized. If such a method is called, it acquires the intrinsic lock of the associated class object. For example, if the Bank class has a static synchronized method, then the lock of the Bank.class object is locked when it is called. As a result, no other thread can call this or any other synchronized static method of the same class. 



**Synchronized Blocks**

- Here, the lock object is created only to use the lock that every Java object possesses. 

  ```java
  public void transfer(int from, int to, int amount)
  {
  synchronized (lock) // an ad-hoc lock
  {
  accounts[from] -= amount;
  accounts[to] += amount;
  }
  System.out.println(. . .);
  }
  ```

- Sometimes, programmers use the lock of an object to implement additional atomic operations—a practice known as client-side locking. 



**The Monitor Concept**

- In the terminology of Java, a monitor has these properties:
  - A monitor is a class with only private felds.
  - Each object of that class has an associated lock.
  - All methods are locked by that lock. In other words, if a client calls obj.method(), then the lock for obj is automatically acquired at the beginning of the method call and relinquished when the method returns. Since all felds are private, this arrangement ensures that no thread can access the felds while another thread manipulates them.
  - The lock can have any number of associated conditions. 
- Every object in Java has an intrinsic lock and an intrinsic condition. If a method is declared with the synchronized keyword, it acts like a monitor method. The condition variable is accessed by calling wait/notifyAll/notify. 



**Volatile Fields**

- Computers with multiple processors can temporarily hold memory values in registers or local memory caches. As a consequence, threads running in different processors may see different values for the same memory location!

- Compilers can reorder instructions for maximum throughput. Compilers won’t choose an ordering that changes the meaning of the code, but they make the assumption that memory values are only changed when there are explicit instructions in the code. However, a memory value can be changed by another thread! 

- The volatile keyword offers a lock-free mechanism for synchronizing access to an instance feld. If you declare a feld as volatile, then the compiler and the virtual machine take into account that the feld may be concurrently updated by another thread. 

- The compiler will insert the appropriate code to ensure that a change to the done variable in one thread is visible from any other thread that reads the variable. 

  ```java
  private volatile boolean done;
  public boolean isDone() { return done; }
  public void setDone() { done = true; }
  ```



**Final Variables**

- As you saw in the preceding section, you cannot safely read a feld from multiple threads unless you use locks or the volatile modifer. 

- There is one other situation in which it is safe to access a shared feld—when it is declared final. Consider 

  ```java
  final Map<String, Double> accounts = new HashMap<>();
  ```

- Other threads get to see the accounts variable after the constructor has fnished. 

- Without using final, there would be no guarantee that other threads would see the updated value of accounts—they might all see null, not the constructed HashMap. 

- Of course, the operations on the map are not thread safe. If multiple threads mutate and read the map, you still need synchronization. 



**Atomics**

- There are a number of classes in the java.util.concurrent.atomic package that use effcient machine-level instructions to guarantee atomicity of other operations without using locks. For example, the AtomicInteger class has methods incrementAndGet and decrementAndGet that atomically increment or decrement an integer. For example, you can safely generate a sequence of numbers like this: 

- The incrementAndGet method atomically increments the AtomicLong and returns the postincrement value. That is, the operations of getting the value, adding 1, setting it, and producing the new value cannot be interrupted. It is guaranteed that the correct value is computed and returned, even if multiple threads access the same instance concurrently 

  ```java
  public static AtomicLong nextNumber = new AtomicLong();
  // In some thread...
  long id = nextNumber.incrementAndGet();
  ```



**Deadlocks**

- It is possible that all threads get blocked because each is waiting for more money. Such a situation is called a deadlock. 



**Lock Testing and Timeouts**

- A thread blocks indefnitely when it calls the lock method to acquire a lock that is owned by another thread. You can be more cautious about acquiring a lock. The tryLock method tries to acquire a lock and returns true if it was successful. Otherwise, it immediately returns false, and the thread can go off and do something else. 

  ```java
  if (myLock.tryLock()){
      // now the thread owns the lock
      try { . . . }
      finally { myLock.unlock(); }
  }
  else
      // do something else
  ```

- The lock method cannot be interrupted. If a thread is interrupted while it is waiting to acquire a lock, the interrupted thread continues to be blocked until the lock is available. If a deadlock occurs, then the lock method can never terminate. 

- However, if you call tryLock with a timeout, an InterruptedException is thrown if the thread is interrupted while it is waiting. This is clearly a useful feature because it allows a program to break up deadlocks. 



**Why the stop and suspend Methods Are Deprecated**

- Let us turn to the stop method frst. This method terminates all pending methods, including the run method. When a thread is stopped, it immediately gives up the locks on all objects that it has locked. This can leave objects in an inconsistent state. For example, suppose a TransferRunnable is stopped in the middle of moving money from one account to another, after the withdrawal and before the deposit. Now the bank object is damaged. Since the lock has been relinquished, the damage is observable from the other threads that have not been stopped. 
- Next, let us see what is wrong with the suspend method. Unlike stop, suspend won’t damage objects. However, if you suspend a thread that owns a lock, then the lock Next, let us see what is wrong with the suspend method. Unlike stop, suspend won’t damage objects. However, if you suspend a thread that owns a lock, then the lock 



### Blocking Queues

- A blocking queue causes a thread to block when you try to add an element when the queue is currently full or to remove an element when the queue is empty. Blocking queues are a useful tool for coordinating the work of multiple threads. Worker threads can periodically deposit intermediate results into a blocking queue. Other worker threads remove the intermediate results and modify them further. The queue automatically balances the workload. If the frst set of threads runs slower than the second, the second set blocks while waiting for the results. If the frst set of threads runs faster, the queue flls up until the second set catches up.  
- The blocking queue methods fall into three categories that differ by the action they perform when the queue is full or empty. If you use the queue as a thread management tool, use the put and take methods. The add, remove, and element operations throw an exception when you try to add to a full queue or get the head of an empty queue. Of course, in a multithreaded program, the queue might become full or empty at any time, so you will instead want to use the offer, poll, and peek methods. These methods simply return with a failure indicator instead of throwing an exception if they cannot carry out their tasks. 
