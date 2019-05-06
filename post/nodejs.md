
## What

- Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, npm, is the largest ecosystem of open source libraries in the world.
- Event-driven: In an event-driven application, there is generally a main loop that listens for events, and then triggers a callback function when one of those events is detected.

![Node-architeture](..\img\Node-architeture.png)



## NPM Commands

```shell
npm -v //version
npm config list //node config
npm root //node_modules root path
npm root -g

npm i <module> //安装最新模块
npm i //安装package.json里的模块
npm i -g <module> //全局安装模块
npm i -S <module> //安装模块到dependencies
npm i -D <lib> //安装模块到dev-dependencies
npm remove <module> //移除模块
npm update <module> //更新模块
npm view <module> //查看模块信息
npm view <module> versions //历史版本

npm init //生成package.json
npm init -y //生成package.json并设置默认值

npm run <script> //运行package.json里scripts定义好的命令

npm install -g npm-check-updates //更新全部依赖包
ncu -u //检查全部依赖包
```



## Thread

|        | 执行方式     |                                                              |
| ------ | ------------ | ------------------------------------------------------------ |
| 单线程 | 串行依次执行 | 1.无法利用多核CPU 2.错误会使应用退出 3.大量计算占用CPU，导致无法继续调用异步I/O |
| 多线程 | 并行         | 1.创建线程 2.线程上下文切换开销 3.死锁 4.锁、状态同步        |



## IO

- 阻塞I/O：调用之后等到系统内核层面完成所有操作后，调用才结束，比如读取磁盘上的一段文件，系统内核进行磁盘寻道、读取数据、复制数据到内存后，调用才结束

- 轮询：重复调用判断操作是否完成



## EventEmitter

- Node asynchronous event-driven architecture.

```js
var EventEmitter = require("events");

class MyEmitter extends EventEmitter {}
const myEmitter = new MyEmitter();

myEmitter.on("event", (a, b) => {
  console.log(a, b);
});

setTimeout(() => {
  myEmitter.emit("event", "1", "2");
}, 3000);
```



## Promise

```js
new Promise((resolve, reject) => {
    resolve(42);
    // reject(new Error("Something Wrong"));
}).then(res => {
    console.log(res);
}).catch(e => {
    console.log(e);
});
```



## Async/Await

```js
function resolveAfter2Seconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('resolved');
    }, 2000);
  });
}

async function asyncCall() {
  console.log('calling');
  var result = await resolveAfter2Seconds();
  console.log(result);
}

asyncCall();
```



## Memory

- Node中通过JavaScript使用的内存限制为1.4G（64位系统）

```shell
$ node
> process.memoryUsage();
{ rss: 22925312,
  heapTotal: 9682944, # V8申请到的堆内存
  heapUsed: 5281200, # 当前使用的量
  external: 16904 }
```

- 代码中声明变量并赋值时，所使用对象的内存就分配在堆中，如果已申请的堆空闲内存不够分配新的对象，将继续申请堆内存，直到堆的大小超过V8的限制为止

- 设置V8的内存限制

```
node --max-old-space-size=1700 test.js // 单位为MB，设置老年代内存空间的最大值
//或者
node --max-new-space-size=1024 test.js // 单位为kB，设置新生代内存空间的大小
```



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



## 多进程架构

Master-Worker模式（主从模式）

主进程：不负责具体业务处理，而是负责调度或管理工作进程，它是趋向于稳定的

工作进程：负责具体业务处理

```js
//worker.js
var http = require("http");

var port = Math.round((1 + Math.random()) * 1000);
http
    .createServer((req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("Hello World");
})
    .listen(port, console.log("Listening on port " + port));

//master.js
var fork = require("child_process").fork;

var cpus = require("os").cpus();

for (let i = 0; i < cpus.length; i++) {
    fork("./worker.js");
}
```

通过fork()复制的进程都是一个独立的进程，这个进程是新的V8实例



**创建子进程**

```js
var cp = require('child_process');
cp.spawn('node', ['worker.js']);
cp.exec('node worker.js', function(err, stdout, stderr){});
cp.execFile('worker.js', function(err, stdout, stderr){});
cp.fork('./worker.js');
```



| 类型       | 回调/异常 | 进程类型 | 执行类型       | 可设置超时 |
| ---------- | --------- | -------- | -------------- | ---------- |
| spawn()    | N         | 任意     | 命令           | N          |
| exec()     | Y         | 任意     | 命令           | Y          |
| execFile() | Y         | 任意     | 可执行文件     | Y          |
| fork()     | N         | Node     | JavaScript文件 | N          |



**进程间通信**

```js
//parent.js
var cp = require('child_process');
var n = cp.fork('./sub.js');

n.on('message', function(m){
    console.log('PARENT got message: ', m);
});

n.send({hello: 'world'});

//sub.js
process.on('message', function(m){
    console.log('CHILD got message: ', m);
});

process.send({foo: 'bar'});
```



**句柄传递**

句柄：一种可以用来标识资源的引用，它的内部包含了指向对象的文件描述符，比如句柄可以用来标识一个服务器端socket对象、一个客户端socket对象、一个UDP套接字、一个管道

发送句柄：主进程接收到socket请求后，将这个socket直接发送给工作进程，而不是重新与工作进程之间建立新的socket连接来转发数据

由于独立启动的进程互相之间并不知道文件描述，所以监听相同端口时就会失败。但对于send()发送的句柄还原出来的服务而言，它们的文件描述符是相同的，所以监听相同端口不会引起异常

多个应用监听相同端口时，文件描述符同一时间只能被某个进程所用。网络请求向服务器端发送时，只有一个进程能够抢到连接，这些进程服务是抢占式的。

```js
//parent.js
var cp = require('child_process');
var child1 = cp.fork('child1.js');
var child2 = cp.fork('child1.js');

var server = require('net').createServer();
server.listen(1337, function(){
  child1.send('server', server);
  child2.send('server', server);

  server.close();
});

//child.js
var http = require("http");
var server = http.createServer(function(req, res) {
  res.writeHead(200, { "Content-Type": "text/plain" });
  res.end("handled by child, pid is " + process.pid + "\n");
});

process.on("message", function(m, tcp) {
  if (m === "server") {
    tcp.on("connection", function(socket) {
      server.emit("connection", socket);
    });
  }
});
```



## Cluster模块

```js
//cluster.js
var cluster = require('cluster');
cluster .setupMaster({
    exec:"worker.js"
});
var cpus = require('os').cpus();
for(var i = 0; i < cpus.length; i++){
    cluster.fork();
}
```

**Cluster工作原理**

cluster模块是child_process和net模块的组合应用

cluster启动时，它会在内部启动TCP服务器，在cluster.fork()子进程时，将这个TCP服务器端socket的文件描述符发送给工作进程
