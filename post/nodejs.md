
## NodeJS

- Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, npm, is the largest ecosystem of open source libraries in the world.
- Event-driven: In an event-driven application, there is generally a main loop that listens for events, and then triggers a callback function when one of those events is detected.

![Node-architeture](..\img\Node-architeture.png)





## Thread

|        | 执行方式     |                                                              |
| ------ | ------------ | ------------------------------------------------------------ |
| 单线程 | 串行依次执行 | 1.无法利用多核CPU 2.错误会使应用退出 3.大量计算占用CPU，导致无法继续调用异步I/O |
| 多线程 | 并行         | 1.创建线程 2.线程上下文切换开销 3.死锁 4.锁、状态同步        |



## IO

- 阻塞I/O：调用之后等到系统内核层面完成所有操作后，调用才结束，比如读取磁盘上的一段文件，系统内核进行磁盘寻道、读取数据、复制数据到内存后，调用才结束

- 轮询：重复调用判断操作是否完成



## GC

- 堆内存
  - 新生代：空间为两个semispace空间大小，存活时间较短的对象
    - Scavenge算法（Cheney算法）：一种采用复制的方式实现的垃圾回收算法，将堆内存分为两个semispace空间，当开始垃圾回收时，检查From空间中的存活对象，将这些存活对象复制到To空间，而非存活对象占用的空间将会被释放。复制完成后，From空间和To空间的角色发生对换（翻转）。缺点是只能使用堆内存的一半，适合用在生命周期短的新生代中
  - 老年代：存活时间较长或常驻内存的对象
    - Mark-Sweep & Mark-Compact
      - Mark-Sweep：分为标记和清除两个阶段，标记阶段遍历堆中所有对象，并标记活的对象，清除阶段清除没有被标记的对象。缺点是清除，内存空间会出现不连续状态，当分配大对象时，碎片空间无法完成分配，会提前触发垃圾回收
      - Mark-Compact：标记整理，将活的对象往一端移动，移动完成后，直接清理边界外的内存

- 晋升
  - 对象是否经历过Scavenge回收：对象从From空间中复制到To空间时，会检查它的内存地址来判断这个对象是否经历过一次Scavenge回收，如果经历过，把该对象从From空间复制到老年代空间，如果没有，就复制到To空间
  - To空间的内存占用比超过限制：对象从From空间中复制到To空间时，如果To空间使用超过25%，对象直接晋升到老年代空间

| 回收算法     | Mark-Sweep   | Mark-Compact | Scavenge           |
| ------------ | ------------ | ------------ | ------------------ |
| 速度         | 中等         | 最慢         | 最快               |
| 空间开销     | 少（有碎片） | 少（无碎片） | 双倍空间（无碎片） |
| 是否移动对象 | 否           | 是           | 是                 |



## OOM

- 应当回收的对象出现意外而没有被回收，变成常驻在老年代中的对象

- 原因
  - 缓存：一个对象一旦被当做缓存来使用，就常驻在老年代，缓存中存储的键越多，长期存活的对象也越多，导致垃圾回收进行扫描和整理时，做了无用功
  - 队列消费不及时：消费速度低于生产速度时，会形成堆积
  - 作用域未释放





# 1



Node as "a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices." 

Node specifically uses V8, the virtual machine that powers Google Chrome, for
server-side programming. V8 gives a huge boost in performance because it cuts out the middleman, prefering straight compilation into native machine code over
executing bytecode or using an interpreter. 





## Asynchronous and Evented

event-driven (i.e. use an event loop )



non-blocking when handling I/O (i.e. use asynchronous I/O )

```js
// I/O does not block execution
$.post('/resource.json', function (data) {
    console.log(data);
});
// script execution continues
```

```js
// I/O blocks execution until finished
var data = $.post('/resource.json');
console.log(data);
//the response for resource.json would be stored in the variable data when it is ready and that the console.log function will not execute until then.
```



![non-blocking IO in browser](..\img\non-blocking IO in browser.png)

The I/O is happening asynchronously and not "blocking" the script execution
allowing the event loop to respond to whatever other interactions or requests that
are being performed on the page. This enables the browser to be responsive to the
client and handle a lot of interactivity on the page. 







Streaming Data

Serving HTTP

WebSocket

static file server

Socket.io

modules

data storage

Testing

middleware

templating





















