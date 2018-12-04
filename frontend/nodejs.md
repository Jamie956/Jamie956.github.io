## Debug

VSCode

1. Sidebar debug -> Add config -> Edit launch.json

   ```json
   {
     // Use IntelliSense to learn about possible attributes.
     // Hover to view descriptions of existing attributes.
     // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
     "version": "0.2.0",
     "configurations": [
       {
         "type": "node",
         "request": "launch",
         "name": "Launch Program",
         "program": "${workspaceFolder}/micro-packages/index.js" //执行文件
       }
     ]
   }
   ```

2. F5 Start Debugging



## 安装

```
sudo apt-get update
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.25.2/install.sh | bash
export NVM_DIR="/home/jamie/.nvm"[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" 
nvm
nvm install <version>
nvm ls
nvm use <version>
nvm alias default <version>
```



## Book

**To Do**

《JavaScript设计模式》

《了不起的Node.js》

**In Progress**

《JavaScript权威指南》

**Done**

《Node.js in action》

《深入浅出Node.js》




## 简介

Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, npm, is the largest ecosystem of open source libraries in the world.



<img width="60%" src="https://static.cnodejs.org/FkTMjCoX4xyL0rJtmm7oBc6V0i8W" />



### 特点

**异步I/O**

```
fs.readFile('/path', function(){})
```

It's very common to conflate the terms "async" and "parallel," but they are actually quite different. Remember, async is about the gap between now and later. But parallel is about things being able to occur simultaneously.



**事件与回调函数**

Whenever there are events to run, the event loop runs until the queue is empty. Each iteration of the event loop is a "tick." User interaction, IO, and timers enqueue events on the event queue.



**单线程**

优点

- 没有死锁
- 没有线程上下文切换带来的开销

缺点

- 单线程无法利用多核CPU
- 错误会使应用退出
- 大量计算占用CPU，导致无法继续调用异步I/O



### 应用场景

I/O密集型：Node利用事件循环的处理能力，而不是启动每一个线程为每一个请求服务，资源占用极少

CUP密集型：单线程下，如果有长时间运行计算，将会导致CPU时间片不能释放，使后续I/O无法发起，但是适当调整和分解大型运算任务为多个小任务，使得运算能够适时释放，不阻塞I/O调用的发起



## 模块机制

### CommonJS模块规范

- 模块引用

  `var math = require('math');`

- 模块定义

  `export.add = function(){};`

- 模块标识



同步，用在服务端



### Node的模块实现

Node引入模块步骤：

1. 路径分析
2. 文件定位
3. 编译执行

模块类型：

- 核心模块：由Node提供的模块，直接加载进内存中，加载速度最快
- 文件模块：用户编写的模块，运行时动态加载

无论核心模块还是文件模块，require()对相同模块的二次加载采用缓存优先的方式



### 包与NPM

```
设version 0.1.0
npm version major //v1.0.0 主版本号
npm version premajor //v1.0.0-0 预备主版本
npm version minor //v0.1.0 次版本号
npm version preminor //v0.1.0-0 预备次版本
npm version patch //v0.1.1 修订号
npm version prepatch //v0.1.1-0 预备修订版
npm version prerelease //v0.1.2-0 预发布版本

npm -v //查看node版本
npm config list //查看node配置
npm root //查看node_modules目录
npm root -g //查看全局node_modules目录

npm i <module> //安装最新模块
npm i //安装package.json里的模块
npm i -g <module> //全局安装模块
npm i -S <module> //安装模块到dependencies
npm i -D <lib> //安装模块到dev-dependencies
npm remove <module> //移除模块
npm update <module> //更新模块
npm view <module> //查看模块信息
npm view <module> versions //查看模块历史版本

npm init //生成package.json
npm init -y //生成package.json并设置默认值

npm run <script> //运行package.json里scripts定义好的命令
```



### 前后端共用模块

**AMD规范**

`defined(id?, dependencies?, factory);`

id和dependencies是可选的，factory的内存就是实际代码的内容



**CMD规范**

`define(['dep1','dep2'],function(dep1,dep2){})`



## 异步I/O

|        | 执行任务的方式 | 性能                     | 问题         |
| ------ | -------------- | ------------------------ | ------------ |
| 单线程 | 串行依次执行   | 慢的任务会阻塞后面的任务 |              |
| 多线程 | 并行           | 创建线程和上下文切换     | 锁、状态同步 |



异步I/O：单线程，远离多线程死锁、状态同步问题

阻塞I/O：调用之后等到系统内核层面完成所有操作后，调用才结束，比如读取磁盘上的一段文件，系统内核进行磁盘寻道、读取数据、复制数据到内存后，调用才结束



阻塞I/O造成CUP等待I/O，CPU的处理能力没用充分利用，为了提高性能，内核提供非阻塞I/O，它调用之后立刻返回



轮询：重复调用判断操作是否完成的技术



### Node的异步I/O

事件循环：进程启动时，Node创建一个类似while(true)的循环，每执行一次循环体（tick），查看是否有事件待处理，如果有就取出事件及其相关回调函数

观察者

请求对象



## 异步编程

### 异步变成解决方案

**事件发布/订阅模式**


- EventEmitter

```js
var EventEmitter = require('events')
var util = require('util')

//定义一个函数对象
var MyEmitter = function () {}

//继承EventEmitter
util.inherits(MyEmitter, EventEmitter)

//实例化函数对象
const myEmitter = new MyEmitter();

//创建event事件
myEmitter.on('event', (a, b) => {
    console.log(a, b, this);
});

//订阅event
myEmitter.emit('event', 'a', 'b');
```



**Promise/Deferred模式**

- Promise

```js
new Promise(function(resolve, reject) {
    if (ok) {
        resolve("Stuff worked!");
    }
    else {
        reject(Error("It broke"));
    }
}).then(function(text){
    console.log(text)
    return Promise.reject(new Error('my error'))
}).catch(function(err){
    console.log(err)
})
```

- Async/Await

```js
async (ctx, next) => {
    try {
        let students = await Student.getAllAsync();
        await ctx.render('students/index', {
            students : students
        })
    } catch (err) {
        return ctx.api_error(err);
    }
};
```



## 内存控制

### V8的垃圾回收机制与内存限制

Node中通过JavaScript使用的内存限制为1.4G（64位系统）

```shell
$ node
> process.memoryUsage();
{ rss: 22925312,
  heapTotal: 9682944, # V8申请到的堆内存
  heapUsed: 5281200, # 当前使用的量
  external: 16904 }

```



代码中声明变量并赋值时，所使用对象的内存就分配在堆中，如果已申请的堆空闲内存不够分配新的对象，将继续申请堆内存，直到堆的大小超过V8的限制为止



调整V8的内存限制

```
node --max-old-space-size=1700 test.js // 单位为MB，设置老年代内存空间的最大值
//或者
node --max-new-space-size=1024 test.js // 单位为kB，设置新生代内存空间的大小
```



**V8的垃圾回收机制**

- V8垃圾回收策略：分代式垃圾回收机制

- 现代垃圾回收算法：按对象的存活时间将内存的垃圾回收进行不同的分代，分别对不同分代的内存实现更高效的算法

- V8的内存分代：在V8中，主要将内存分为新生代（存活时间较短的对象）和老年代（存活时间较长或常驻内存的对象）

 -V8堆的整体大小 = 新生代所用内存空间（两个semispace空间大小） + 老年代的内存空间



- Scavenge算法：在分代的基础上，新生代的对象主要通过Scavenge算法进行垃圾回收，在Scavenge的具体现实中，主要采用了Cheney算法，该算法是一种采用复制的方式实现的垃圾回收算法，它将堆内存一分为二，每一部分空间称为semispace，在两个semispace空间中，只有一个处于使用中（From空间），另一个处于闲置状态（to空间）。分配对象时，先是在From空间进行分配，当开始垃圾回收时，检查From空间中的存活对象，将这些存活对象复制到To空间，而非存活对象占用的空间将会被释放。复制完成后，From空间和To空间的角色发生对换（翻转）。Scavenge的缺点是只能使用堆内存的一半，它是典型的牺牲空间换取时间的算法，无法大规模应用到所有的垃圾回收，但非常适合应用在生命周期短的新生代中

- 晋升：当一个对象经过多次复制依然存活时，被认为是生命周期较长的对象，随后会被移到老年代中

- 晋升条件：
  - 对象是否经历过Scavenge回收：对象从From空间中复制到To空间时，会检查它的内存地址来判断这个对象是否经历过一次Scavenge回收，如果经历过，把该对象从From空间复制到老年代空间，如果没有，就复制到To空间
  - To空间的内存占用比超过限制：对象从From空间中复制到To空间时，如果To空间使用超过25%，对象直接晋升到老年代空间

- Mark-Sweep & Mark-Compact：V8老年代主要采用Mark-Sweep & Mark-Compact 相结合的算法。
  - Mark-Sweep：分为标记和清除两个阶段，标记阶段遍历堆中所有对象，并标记活的对象，清除阶段就是清除没有被标记的对象。缺点是进行标记清除回收后，内存空间会出现不连续状态，当分配大对象时，碎片空间无法完成分配，会提前触发垃圾回收
  - Mark-Compact：标记整理，将活的对象往一端移动，移动完成后，直接清理边界外的内存



| 回收算法     | Mark-Sweep   | Mark-Compact | Scavenge           |
| ------------ | ------------ | ------------ | ------------------ |
| 速度         | 中等         | 最慢         | 最快               |
| 空间开销     | 少（有碎片） | 少（无碎片） | 双倍空间（无碎片） |
| 是否移动对象 | 否           | 是           | 是                 |

V8主要使用Mark-Sweep，在空间不足以对新生代晋升过来的对象进行分配时才使用Mark-Compact



**查看垃圾回收日志**

```
node --trace_gc -e "var a = []; for(var i=0; i<1000000; i++) a.push(new Array(100));" > gc.log
```

通过分析垃圾回收日志，可以了解垃圾回收的运行状况，找出垃圾回收的哪些阶段比较耗时，触发的原因是什么。



在Node启动时使用--prof参数，得到V8执行时的性能分析数据，其中包括垃圾回收时占用的时间

```js
// test01.js
for(var i=0; i<000000; i++) {
    var a = {};
}

//运行并生成v8.log
$ node --prof test01.js

//统计日志信息
linux-tick-processor v8.log
```



### 高效使用内存

**作用域**

- 标识符查找
- 作用域链
- 变量的主动释放



**闭包**

外部作用域访问内部作用域中的变量



### 内存指标

**查看内存使用情况**



- 查看进程的内存占用

```shell
$ node
> process.memoryUsage();
{ rss: 22925312,
  heapTotal: 9682944,
  heapUsed: 5281200,
  external: 16904 }
```

rss( resident set size )：即进程的常驻内存部分，单位字节

heapTotal：堆中总共申请的内存量，单位字节

heapUsed：堆中使用中的内存量，单位字节



- 查看系统的内存占用

totalmem(), freemem()查看内存使用情况

```shell
#查看内存使用情况
$ node
> os.totalmem() #总内存，单位字节
> os.freemem() #闲置内存，单位字节
```



**堆外内存**

堆外内存：不通过V8分配的内存

```js
var useMem = function(){
    var size = 200*1024*1024;
    var buffer = new Buffer(size);
    for (var i = 0; i < size; i++) {
        buffer[i] = 0;    
    }
    return buffer;
}

//Process: heapTotal 10.73MB heapUsed 3.65MB rss 3027.09MB
//rss的值远远超过V8的限制，因为Buffer对象不经过V8的内存分配机制
```



Node的内存构成主要由通过V8进行分配的部分和Node自行分配的部分，受V8的垃圾回收限制的主要是V8的堆内存



### 内存泄漏

实质：应当回收的对象出现意外而没有被回收，变成常驻在老年代中的对象

内存泄漏的情况：

- 缓存
- 队列消费不及时
- 作用未释放



**慎将内存当做缓存**

一个对象一旦被当做缓存来使用，就常驻在老年代，缓存中存储的键越多，长期存活的对象也越多，导致垃圾回收进行扫描和整理时，做了无用功



**关注队列状态**

消费速度低于生产速度时，会形成堆积



### 内存泄漏排查

工具

- v8-profiler
- node-heapdump
- node-mtrace
- dtrace
- node-memwatch



## Buffer

### Buffer结构

Buffer对象类似于数组，它的元素为16进制的两位数，即0-255的数值

Buffer所占用的内存不是通过V8分配，而是在Node的C++层面实现内存的申请，属于堆外内存



slab的状态：

- full 完全分配
- partial 部分分配
- empty 没有被分配



Node以8KB为界限区分Buffer是大对象还是小对象



真正的内存实在Node的C++层面提供，JavaScript层面只是使用它，当进行小而频繁的Buffer操作时，采用slab机制进行预先申请和事后分配，使JavaScript到操作系统之间不必有过多的内存申请方面的系统调用，对于大块的Buffer，则直接使用C++层面提供的内存



### Buffer的转换

str -> Buffer

`new Buffer(str, [encoding]);`

`buf.write(str, [offset], [length], [encoding])`



Buffer -> str

`buf.toString([encoding], [start], [end])`



### Buffer的拼接

```js
var chunks = [];
var size = 0;
res.on('data', function(chunk){
   chunks.push(chunk);
   size += chunk.length;
});
res.on('end',function(){
   var buf = Buffer.concat(chunks, size);
    var str = iconv.decode(buf, 'utf8');
    console.log(str);
});
```



### Buffer与性能

Buffer在文件I/O和网络I/O中运用广泛，在网络中传输，都需要转换为Buffer，以二进制数据传输，提高字符串到Buffer的转换效率，可以很大程度地提高网络吞吐率



## 网络编程

### 构建TCP服务

TCP是面向连接的协议，传输之前需要3次握手形成回话

只有会话形成后，server和client之间才能互相发送数据。在创建会话过程中，sever和client分别提供一个套接字，这两个套接字共同形成一个连接，server和client通过套接字实现两者之间连接的操作。

```js
//tcp-server.js
var net = require('net');

var server = net.createServer(function(socket){
    socket.on('data', function(data){
        console.log(data.toString());
        socket.write('from server to client');
    });

    socket.on('end', function(){
        console.log('server died');
    });
});

server.listen(8124, function(){
    console.log('server listen on port 8124');
});

//tcp-client.js
var net = require('net');

var client = net.connect({port:8124}, function(){
    console.log('client connected');
    client.write('from client to server');
});

client.on('data', function(data){
    console.log(data.toString());
    client.end();
});

client.on('end', function(){
    console.log('client disconnected');
});

```



Nagle算法：TCP针对网络中的小数据包的优化策略，要求缓冲区的数据达到一定数量或一定时间后才发出，所以小数据包会被Nagle算法合并，以此来优化网络



### 构建UDP服务

与TCP不同，UDP不是面向连接

UDP中，一个套接字可以与多个UDP通信，它虽然提供面向事务的简单不可靠信息传输服务，在网络差的情况下存在丢包严重问题，但由于它无须连接，资源消耗低，处理快速且灵活，应用于音频、视频、DNS



```js
//udp-server.js
var dgram = require("dgram");

var server = dgram.createSocket("udp4");

server.on("message", function(msg, rinfo) {
  console.log(
    "server got: " + msg + " from " + rinfo.address + ": " + rinfo.port
  );
});

server.on("listening", function() {
  var address = server.address();
  console.log("server listening " + address.address + ": " + address.port);
});

server.bind(41234);

//udp-client.js
var dgram = require('dgram');

var message = new Buffer('hi');

var client = dgram.createSocket('udp4');

client.send(message, 0, message.length, 41234, 'localhost', function(err, bytes){
  client.close();
});
```



### 构建HTTP服务

HyperText Transfer Protocol

HTTP构建在TCP之上，属于应用层协议，HTTP的两端是服务器和浏览器，即B/S模式

浏览器其实就是一个HTTP代理，用户的行为将会通过它转化为HTTP请求报文发送给服务端，服务端在处理请求后，发送响应报文给代理，代理在解析报文后，将用户需要的内容呈现在界面上。以浏览器打开一张图片为例：首先，浏览器构造HTTP报文发向图片服务端；然后，服务端判断报文中的要请求的地址，将磁盘中的图片文件以报文的形式发送给浏览器；浏览器接收完图片后，调用渲染引擎将其显示给用户。



Node的http模块包含对HTTP处理的封装，在Node中，HTTP服务继承自TCP服务器（net模块），它能够与多个客户端保持连接，由于其采用实事件驱动的形式，并不为每一个连接创建额外的线程或进程，保持很低的内存占用，所以能实现高并发。HTTP服务与TCP服务模型的区别在于，开启keepalive后，一个TCP会话可以用于多次请求和响应。TCP服务以connection为单位进行服务，HTTP服务以request为单位进行服务。http模块即是将connection到request的过程进行封装



```js
//http-server.js
var http = require("http");
http
    .createServer((req, res) => {
    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("Hello World");
})
    .listen(3000, console.log("Listening on port 3000."));


//http-client.js
var http = require("http");

var options = {
    hostname: "127.0.0.1",
    port: 3000,
    path: "/",
    method: "GET"
};

var req = http.request(options, function(res) {
    console.log(res.statusCode);
    console.log(JSON.stringify(res.headers));
    res.setEncoding("utf8");
    res.on("data", function(chunk) {
        console.log(chunk);
    });
});

req.end();
```



### 构建WebSocket服务

WebScoket实现了客户端与服务端之间的长连接，而Node事件驱动的方式十分擅长与大量的客户端保持高并发连接

与HTTP相比，WebSocket的好处：

- 客户端与服务端只建立一个TCP连接，可以使用更少的连接
- WebSocket服务端可以推送数据到客户端，更加灵活和高效
- 轻量级协议头，减少数据传送量



**WebSocket握手**

客户端建立连接时，通过HTTP发起请求报文，与普通HTTP协议不同的是协议头包含`Upgrede: websocket` 和 `Connection: Upgrade`



一旦WebSocket握手成功，服务端与客户端将会呈现对等的效果，都能接收和发送消息



**WebSocket数据传输**

在握手顺利完成后，当前连接将不再进行HTTP交互，而是开始WebSocket的数据协议，实现客户端与服务端的数据交换



## ch8 构建Web应用

### 基础功能

**请求方法**

**路径解析**

**查询字符串**

**Cookie**

**Session**

**缓存**

**Basic认证**



### 数据上传

**表单数据**

**其他格式**

**附件上传**

**数据上传与安全**



### 路由解析

**文件路径型**

**MVC**

**RESTful**



### 中间件

**异常处理**

**中间件与性能**



### 页面渲染

**内容响应**

**试图渲染**

**模板**

**Bigpipe**



## ch9 进程

## ch10 测试

## ch11 产品化


