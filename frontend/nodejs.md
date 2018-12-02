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



## flow

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







## ch1 简介

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



**跨平台**

OS - libuv - Node



### 应用场景

I/O密集型：Node利用事件循环的处理能力，而不是启动每一个线程为每一个请求服务，资源占用极少

CUP密集型：单线程下，如果有长时间运行计算，将会导致CPU时间片不能释放，使后续I/O无法发起，但是适当调整和分解大型运算任务为多个小任务，使得运算能够适时释放，不阻塞I/O调用的发起



## ch2 模块机制

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



### 核心模块

### C/C++扩展模块

### 模块调用栈

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

玉伯提出

`define(['dep1','dep2'],function(dep1,dep2){})`



## ch3 异步I/O

|        | 执行任务的方式 | 性能                     | 问题         |
| ------ | -------------- | ------------------------ | ------------ |
| 单线程 | 串行依次执行   | 慢的任务会阻塞后面的任务 |              |
| 多线程 | 并行           | 创建线程和上下文切换     | 锁、状态同步 |



异步I/O：单线程，远离多线程死锁、状态同步问题

阻塞I/O：调用之后等到系统内核层面完成所有操作后，调用才结束，比如读取磁盘上的一段文件，系统内核进行磁盘寻道、读取数据、复制数据到内存后，调用才结束



阻塞I/O造成CUP等待I/O，CPU的处理能力没用充分利用，为了提高性能，内核提供非阻塞I/O，它调用之后立刻返回



轮询：重复调用判断操作是否完成的技术



### Node的异步I/O

事件循环

观察者

请求对象



### 非I/O的异步API

### 时间驱动与高性能服务器



## ch4 异步编程

## ch5 内存控制

## ch6 Buffer

## ch7 网络编程

## ch8 构建Web应用

## ch9 进程

## ch10 测试

## ch11 产品化

## 





































































