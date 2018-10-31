## NodeJS

Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, npm, is the largest ecosystem of open source libraries in the world.

- 基于 Google V8 引擎的 JavaScript 运行时环境
- Chrome V8 引擎以 C/C++ 为主
- 事件驱动（event-driven），非阻塞 I/O 模型（non-blocking I/O model），由C/C++ 编写的事件循环处理库Libuv，处理 I/O 操作
- npm包管理器



<img src="https://static.cnodejs.org/Fh2MIT1r4YStGl9ZEEzt7N4lEbqX">



<img src="https://static.cnodejs.org/FkTMjCoX4xyL0rJtmm7oBc6V0i8W" />



### 应用

- 前端：react\vue\angular、应用实践、架构
- 后端：核心特性、Web应用、微服务、封装API、组装RPC服务、测试、部署
- 跨平台：前端、移动端、PC端（electron）
- 工具：预编译、webpack、工程化



### 优点

简单易学

性能好

部署容易

处理高并发场景下的大量服务器请求

生态强大（大量NPM模块）

开源的

跨平台

高效（I/O处理）



## Debug

```
node inspect app.js
or
npm I -g node-inspect
node-inspect app.js
```

- node debugger

- node inspector

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

> 参考
>
> https://segmentfault.com/a/1190000015991869#articleHeader8

### CommonJS

同步，用在服务端



1. 对于基本数据类型，属于复制。即会被模块缓存；同时，在另一个模块可以对该模块输出的变量重新赋值。
2. 对于复杂数据类型，属于浅拷贝。由于两个模块引用的对象指向同一个内存空间，因此对该模块的值做修改时会影响另一个模块。
3. 当使用require命令加载某个模块时，就会运行整个模块的代码。
4. 当使用require命令加载同一个模块时，不会再执行该模块，而是取到缓存之中的值。也就是说，CommonJS模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回第一次运行的结果，除非手动清除系统缓存。
5. 循环加载时，属于加载时执行。即脚本代码在require的时候，就会全部执行。一旦出现某个模块被"循环加载"，就只输出已经执行的部分，还未执行的部分不会输出。



```js
//导出对象
module.exports = {
    a: 1
}
var module = require('./a.js'); 
module.a // 1

//导出class
class Deer {}
module.exports = Deer;
var Deer = require("./Deer");

//导出带名函数
module.exports.foo = () => {};
var x = require("./x");
x.foo();

//导出匿名函数
module.exports = function() {};
var x = require("./x");
x();

// 导出数组
module.exports = ["a", "b", "c"];

// 导出多个函数
module.exports = {
  foo: () => {},
  bar: () => {}
};
```



### AMD

( Asynchronous Module Definition )

异步加载，用在浏览器

所有依赖模块的语句，都定义在一个回调函数中，等到加载完成之后，回调函数才执行。



- RequireJS执行流程：

1. require函数检查依赖的模块，根据配置文件，获取js文件的实际路径
2. 根据js文件实际路径，在dom中插入script节点，并绑定onload事件来获取该模块加载完成的通知。
3. 依赖script全部加载完成后，调用回调函数



```js
define(['./a', './b'], function(a, b) {
    a.do()
    b.do()
})
```



### CMD

( Common Module Definition )

异步加载，用在浏览器

延迟执行（运行按需加载，顺序执行）

与requireJS相似，但是模块定义方式和模块加载时机不同

```js
define(function(require, exports, module) {
    var a = require('./a')  
    a.doSomething()   
    var b = require('./b')
    b.doSomething()
})
```



### ES6

1. ES6模块中的值属于【动态只读引用】。
2. 对于只读来说，即不允许修改引入变量的值，import的变量是只读的，不论是基本数据类型还是复杂数据类型。当模块遇到import命令时，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。
3. 对于动态来说，原始值发生变化，import加载的值也会发生变化。不论是基本数据类型还是复杂数据类型。
4. 循环加载时，ES6模块是动态引用。只要两个模块之间存在某个引用，代码就能够执行。

```js
// file a.js
export function a() {}
export function b() {}
// file b.js
export default function() {}

import {a, b} from './a.js'
import x from './b.js'
```



## 相关书籍

还没看：

《JavaScript设计模式》

《了不起的Node.js》

正在看：

《JavaScript权威指南》

《深入浅出Node.js》

看完了：

《Node.js in action》



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

### 

## 流程控制

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



## Concept

- async vs. parallel

It's very common to conflate the terms "async" and "parallel," but they are actually quite different. Remember, async is about the gap between now and later. But parallel is about things being able to occur simultaneously.

 

- Concurrency

Concurrency is when two or more "processes" are executing simultaneously over the same period, regardless of whether their individual constituent operations happen in parallel (at the same instant on separate processors or cores) or not. You can think of concurrency then as "process"-level (or task-level) parallelism, as opposed to operation-level parallelism (separate-processor threads).



- tick

Whenever there are events to run, the event loop runs until the queue is empty. Each iteration of the event loop is a "tick." User interaction, IO, and timers enqueue events on the event queue.




