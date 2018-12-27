## Debug

**VSCode**

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



**Node 内置Debbuger**

运行：

```shell
node --inspect app.js
```

打开浏览器：

chrome://inspect



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

### 异步编程解决方案

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
- 作用域未释放



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



Node的http模块包含对HTTP处理的封装，在Node中，HTTP服务继承自TCP服务器（net模块），它能够与多个客户端保持连接，由于其采用事件驱动的形式，并不为每一个连接创建额外的线程或进程，保持很低的内存占用，所以能实现高并发。HTTP服务与TCP服务模型的区别在于，开启keepalive后，一个TCP会话可以用于多次请求和响应。TCP服务以connection为单位进行服务，HTTP服务以request为单位进行服务。http模块即是将connection到request的过程进行封装



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



## 构建Web应用

### 基础功能

**请求方法**

取出headers -> 设置`req.method`

**路径解析**

场景：

- 静态文件服务器，根据路径查找磁盘中的文件，响应给客户端
- 根据路径来选择控制器



**查询字符串**

查询字符串位于路径之后`?foo=bar&baz=val` 



**Cookie**

Cookie处理步骤：

1. 服务器向客户端发送Cookie
2. 浏览器将Cookie保存
3. 之后每次浏览器都会将Cookie发到服务端



path：表示这个Cookie影响到的路径，当前访问的路径不满足该匹配时，浏览器则不发送这个Cookie

Expires & Max-Age：告知浏览器Cookie何时过期，如果不设置，关闭浏览器就会流失这个Cookie

HttpOnly：告知浏览器不允许通过脚本`document.cookie` 更改这个Cookie值

Secure：它的值为true时，表示创建的Cookie只能在HTTPS连接中被浏览器传递到服务器进行会话验证，如果是HTTP连接则不会传递



**Session**

Session的数据只保留在服务器端，客户端无法修改，数据的安全性得到一定的保障，数据也无须在协议中每次都被传递



为了解决性能问题和Session数据无法跨进程共享的问题，常用的方案是将Session集中化，将原本可能分散在多个进程里的数据，统一转移到集中的数据存储中。常用工具有Redis、Memcached，通过这些高效缓存，Node进程无须在内部维护数据对象，解决垃圾回收问题和内存限制



速度问题：

- Node与缓存服务保持长连接，而非频繁的短连接，握手导致的延迟只影响初始化
- 高速缓存直接在内存中进行数据存储和访问
- 缓存服务通常与Node进程运行在相同的机器上或者相同的机房里，网络速度受到的影响较小



**缓存**

**Basic认证**

Basic认证是当客户端与服务端进行请求时，允许通过用户名和密码实现的一种身份认证方式

如果一个页面需要Basic认证，它会检查请求报文头部中的Authorization字段的内容，该字段的值由认证方式和加密值构成 `Authorization: Basic dXNlcjpwsNK`

```js
var encode = function(username, password){
    return new Buffer(username + ':' + password).toString('base64);
};
```



### 数据上传

**表单数据**

`Content-Type: application/x-www-form-urlencoded`

```js
var handle = function(req, res){
    if(req.headers['content-type'] === 'application/x-www-form-urlencoded'){
        req.body = querystring.parse(req.rawBody);
    };
    todo(req, res);
};
```



**其他格式**

除了表单数据，常见的提交还有JSON和XML

`application/json`

`application/xml`



**附件上传**

`multipart/form-data`



**数据上传与安全**

内存限制

- 限制上传内容的大小，一旦超过限制，停止接收数据，并响应400
- 通过流式解析，将数据流导向到磁盘中，Node只保留文件路径等小数据



### 路由解析

**文件路径型**

静态文件

动态文件



**MVC**

Controller：路由解析，根据URL寻找对应的控制器和行为

Model：行为调用相关的模型，进行数据操作

View：数据操作结束后，调用视图和相关数据进行页面渲染，输出到客户端



## 进程

Node在V8引擎之上构建，JavaScript运行在单个进程的单个线程上，好处就是，程序状态单一，没有多线程情况下的锁、线程同步问题，操作系统在调度时较少上下文切换，提高CPU的使用率

但是如何利用多核CPU服务器

由于Node执行在单线程上，一旦线程上抛出的异常没有被捕获，会引起整个进程的崩溃



服务模型变迁：同步 -> 复制进程 -> 多线程 -> 事件驱动



### 多进程架构

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

通过fork()复制的进程都是一个独立的进程，这个进程是新的V8实例，



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



### 集群

**进程事件**

error

exit

close

disconnection



**自动重启**

自杀信号

限量重启



**负载均衡**

保证多个处理单元工作量公平的策略



**状态共享**

通知进程：发送通知和查询状态是否更改的进程



### Cluster模块

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

**Cluster事件**

fork

online

listening

disconnect

exit

setup



## 测试

### 单元测试

**单元测试的意义**

测试代码遵循原则

- 单一职责
- 接口抽象
- 层次分离



**单元测试介绍**

- 断言

  ```js
  var assert = require('assert');
  assert.equal(Math.max(1, 100), 100);
  ```

  should.js

- 测试框架

  mocha.js

  TDD（测试驱动开发）：关注所有功能是否被正确实现，每一个功能都具备对应的测试用例，表述方式偏向于功能说明书的风格

  ```js
  suite('Array', function(){
      setup(function(){});
      
      suite('#indexOf()', function(){
          test('should return -1 when not present', function(){
              assert.equal(-1, [1,2,3].indexOf(4));
          });
      });
  });
  ```



  	BDD（行为驱动开发）：关注整体行为是否符合预期，适合自顶向下的设计方式，表述方式更接近于自然语言的习惯

```
describe('Array', function(){
    before(function(){});

    describe('#indexOf()', function(){
        it('should return -1 when not present', function(){
            [1,2,3].indexOf(4).should.equal(-1);
        });
    });
});
```



- 测试用例：最少需要通过正向测试和反向测试来保证测试对功能的覆盖

  异步测试

  ```js
  //macha
  if('fs.readFile should be ok', function(done){
      fs.readFile('file_path', 'utf8', function(err, data){
          should.not.exist(err);
          done();
      });
  });
  ```



  	超时设置

- 测试覆盖率：用来定位没有测试到的代码行

  jscover

  blanket

- mock：模拟异常

- 私有方法测试

  rewrite

web 测试， supertest



**工程化与自动化**

makefile

持续集成，travis-ci



### 性能测试

**负载测试**

**基准测试**

统计在多少时间内执行了多少次某个方法

库：benchmark



**压力测试**

测试网络接口性能，指标包含有吞吐率、响应时间、并发数

库：ab


