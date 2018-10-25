

```
node -v
node <js>
```

## Debugging
```
node inspect app.js
or
npm I -g node-inspect
node-inspect app.js
```

- node debugger
- node inspector
- vscode



## nvm

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

## Error
```
Interal watch failed ENOSPC:
echo fs.inotify.max_user_watches=582222 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
```



## Modularization

### CommonJS

用在服务端

```js
// a.js
module.exports = {
    a: 1
}
// or
exports.a = 1

// b.js
var module = require('./a.js'); //require同步加载
module.a // -> log 1
```



### AMD( Asynchronous Module Definition )

用在浏览器

```js
require([module], callback);
```

Javascript库(require.js, curl.js)实现了AMD规范

提前执行（异步加载：依赖先执行）+延迟执行

解决两个问题

- 多个js文件可能有依赖关系，被依赖的文件需要早于依赖它的文件加载到浏览器
- js加载的时候浏览器会停止页面渲染，加载文件越多，页面失去响应时间越长

```js
main.js 
//别名配置
requirejs.config({ paths: {
    jquery: 'jquery.min' //省略.js 
} }); 

//引入模块，用变量$表示jquery模块 
requirejs(['jquery'], function ($) {
    $('body').css('background-color','red'); 
});
```

```js
// AMD
define(['./a', './b'], function(a, b) {
    a.do()
    b.do()
})
define(function(require, exports, module) {
    var a = require('./a')  
    a.doSomething()   
    var b = require('./b')
    b.doSomething()
})
```



### CMD( Common Module Definition )

用在浏览器

延迟执行（运行按需加载，顺序执行）

与requireJS相似，但是模块定义方式和模块加载时机不同



### es6

```js
// file a.js
export function a() {}
export function b() {}
// file b.js
export default function() {}

import {a, b} from './a.js'
import XXX from './b.js'
```

### x

```js
//导出class
class Deer {}
module.exports = Deer;
var Deer = require("./Deer");

//导出带名函数
module.exports.foo = () => {};
var xx = require("./xx");
xx.foo();

//导出匿名函数
module.exports = function() {};
var xx = require("./xx");
xx();

// 导出数组
module.exports = ["a", "b", "c"];

// 导出多个函数
module.exports = {
  foo: () => {},
  bar: () => {}
};
```



## 应用场景

- 前端：react\vue\angular、应用实践、架构
- 后端：核心特性、Web应用、微服务、封装API、组装RPC服务、测试、部署

- 跨平台：前端、移动端、PC端（electron）
- 工具：预编译、webpack、工程化



## 优点

简单易学

性能好

部署容易

处理高并发场景下的大量服务器请求

生态强大（大量NPM模块）

开源的

跨平台

高效（I/O处理）



## NODEJS是什么

Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, npm, is the largest ecosystem of open source libraries in the world.

- 基于 Google V8 引擎的 JavaScript 运行时环境
- Chrome V8 引擎以 C/C++ 为主
- 事件驱动（event-driven），非阻塞 I/O 模型（non-blocking I/O model），由C/C++ 编写的事件循环处理库Libuv，处理 I/O 操作
- npm包管理器



## 基本原理

<img src="https://static.cnodejs.org/Fh2MIT1r4YStGl9ZEEzt7N4lEbqX">



<img src="https://static.cnodejs.org/FkTMjCoX4xyL0rJtmm7oBc6V0i8W" />



## 语法

bind/call/apply

正则

数据结构

数组用法

面向对象写法



### TypeScript

- 规模化编程，像Java，静态类型，面向对象
- 开源
- TypeScript + VSCode 





## 相关书籍

《JavaScript设计模式》：未看

《JavaScript权威指南》：没看完

《Node.js in action》：看完

《深入浅出Node.js》:未看

《了不起的Node.js》：未看

## npm

### version

```
设version 0.1.0
npm version major //v1.0.0 主版本号
npm version premajor //v1.0.0-0 预备主版本
npm version minor //v0.1.0 次版本号
npm version preminor //v0.1.0-0 预备次版本
npm version patch //v0.1.1 修订号
npm version prepatch //v0.1.1-0 预备修订版
npm version prerelease //v0.1.2-0 预发布版本
```

### 常用命令

```
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



### 常用模块

- nodemon：运行实时更新



## 异步流程

- Error-first Callback

```js
function(err, res) {
    // process the error and result
}
```

- EventEmitter

```js
var EventEmitter = require('events')
var util = require('util')

var MyEmitter = function () {

}

util.inherits(MyEmitter, EventEmitter)

const myEmitter = new MyEmitter();

myEmitter.on('event', (a, b) => {
    console.log(a, b, this);
});

myEmitter.emit('event', 'a', 'b');
```

- Promise

```js
new Promise(function(resolve, reject) {
  // do a thing, possibly async, then…
  if (/* everything turned out fine */) {
    resolve("Stuff worked!");
  }
  else {
    reject(Error("It broke"));
  }
}).then(function(text){
    console.log(text)// Stuff worked!
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



## Concept

It's important to note that setTimeout(..) doesn't put your callback on the event loop queue. What it does is set up a timer; when the timer expires, the environment places your callback into the event loop, such that some future tick will pick it up and execute it.

 

It's very common to conflate the terms "async" and "parallel," but they are actually quite different. Remember, async is about the gap between now and later. But parallel is about things being able to occur simultaneously.

 

JavaScript's single-threading

 

Concurrency is when two or more "processes" are executing simultaneously over the same period, regardless of whether their individual constituent operations happen in parallel (at the same instant on separate processors or cores) or not. You can think of concurrency then as "process"-level (or task-level) parallelism, as opposed to operation-level parallelism (separate-processor threads).

 

Whenever there are events to run, the event loop runs until the queue is empty. Each iteration of the event loop is a "tick." User interaction, IO, and timers enqueue events on the event queue.



Promise belongs to microtask and setTimeout belongs to macrotask

 

Microtasks include process.nextTick, promise, Object.observe and MutationObserver

 

Macrotasks include script, setTimeout, setInterval, setImmediate, I/O and UI rendering

 

So the correct sequence of an event loop looks like this:

1.Execute synchronous codes, which belongs to macrotask

2.Once call stack is empty, query if any microtasks need to be executed

3.Execute all the microtasks

4.If necessary, render the UI

5.Then start the next round of the Event loop, and execute the asynchronous operations in the macrotask

 

===MVVM===

In the JQuery period, if you need to refresh the UI, you need to get the corresponding DOM and then update the UI, so the data and business logic are strongly-coupled with the page.

 

In MVVM, the UI is driven by data. Once the data is changed, the corresponding UI will be refreshed. If the UI changes, the corresponding data will also be changed. In this way, we can only care about the data flow in business processing without dealing with the page directly. ViewModel only cares about the processing of data and business and does not care how View handles data. In this case, we can separate the View from the Model. If either party changes, it does not necessarily need to change the other party, and some reusable logic can be placed in a ViewModel, allowing multiple Views to reuse this ViewModel.

 

In MVVM, the core is the two-way binding of data, such as dirty checking by Angular and data hijacking in Vue.

 



































