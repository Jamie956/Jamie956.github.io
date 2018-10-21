

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



## JS模块规范(CommonJS，AMD，CMD)

### CommonJS

用在服务端

定义的模块分为:{模块引用(require)} {模块定义(exports)} {模块标识(module)}

```js
module.exports //输出
```



```js
var fs = require('fs'); //
```

require是同步加载的



### AMD

用在浏览器

Asynchronous Module Definition

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



### CMD

Common Module Definition

用在浏览器

延迟执行（运行按需加载，根据顺序执行）

与requireJS相似，但是模块定义方式和模块加载（可以说运行、解析）时机不同







## 应用

- 大前端开发
- Web应用
- 封装API
- 组装RPC服务
- PC客户端
- 微服务



## 优点

简单易学

性能好

部署容易

处理高并发场景下的大量服务器请求

生态强大（大量NPM模块）

开源的

跨平台

高效（I/O处理）



## WHAT

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

《JavaScript设计模式》

《JavaScript权威指南》

