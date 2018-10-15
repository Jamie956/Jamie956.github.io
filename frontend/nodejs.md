

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