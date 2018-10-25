> 参考资料：
>
> https://github.com/stephentian/33-js-concepts
>
> https://developer.mozilla.org/en-US/docs/Glossary/Call_stack
>
> https://juejin.im/post/59e85eebf265da430d571f89



### call stack

A **call stack** is a mechanism for an interpreter (like  the JavaScript interpreter in a web browser) to keep track of its place  in a script that calls multiple functions — what function is currently being run, what functions are called from within that function and should be called next, etc.

- When a script calls a function, the interpreter adds it to the call stack and then starts carrying out the function.
- Any functions that are called by that function are added to the call stack further up, and run where their calls are reached.
- When the current function is finished, the interpreter takes it off  the stack and resumes execution where it left off in the last code  listing.
- If the stack takes up more space than it had assigned to it, it results in a "stack overflow" error.



```js
function greeting() {
   // [1] Some codes here
   sayHi();
   // [2] Some codes here
}
function sayHi() {
   return "Hi!";
}

// Invoke the `greeting` function
greeting();

// [3] Some codes here
```



The code above would be executed like this:

1. Ignore all functions, until it reaches the `greeting()` function.

2. Invoke the `greeting()` function.

3. Add the `greeting` function to the call stack list.

   > Call stack list:
   >
   > greeting

4. Execute all lines of code inside the `greeting` function.

5. Get to the `sayHi()` function.

6. Add the `sayHi()` function to the call stack list.

   > Call stack list:
   >
   > greeting
   >
   > sayHi

7. Execute all lines of code inside the `sayHi()` function, until reaches its end.

8. Return execution to the line that invoked `sayHi()` and continue executing the rest of the `greeting()` function.

9. Delete the `sayHi()` function from our call stack list.

   > Call stack list:
   >
   > greeting

10. When everything inside the `greeting()` function has been executed, return to its invoking line to continue executing the rest of the JS code.

11. Delete the `greeting()` function from the call stack list.

    > Call stack list:
    >
    > EMPTY

We started with an empty Call Stack, and whenever we invoke a function,  it is automatically added to the Call Stack, after executing all of its  codes, it is automatically removed from the Call Stack. At the end, we  ended up with an empty stack too.



### event loop

<img width="80%" src="https://user-gold-cdn.xitu.io/2017/11/21/15fdd88994142347?imageView2/0/w/1280/h/960/ignore-error/1" />



- 同步的进入主线程，异步的进入Event Table并注册函数
- 当指定的事情完成时，Event Table会将这个函数移入Event Queue
- 主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行
- 上述过程不断重复，也就是Event Loop(事件循环)

JS引擎存在monitoring process进程，不断检查主线程执行栈是否为空，一旦为空，就会去Event Queue检查是否有等待被调用的函数



 ```js
let data = [];
$.ajax({
    url:www.javascript.com,
    data:data,
    success:() => {
        console.log('发送成功!');
    }
})
console.log('代码执行结束');
 ```

- ajax进入Event Table，注册回调函数`success`
- 执行`console.log('代码执行结束')`
- ajax事件完成，回调函数`success`进入Event Queue
- 主线程从Event Queue读取回调函数`success`并执行

 

### setTimeout

 ```
setTimeout(() => {
    task();
},3000);

sleep(10000000);
 ```

1. `task()`进入Event Table并注册,计时开始
2. 执行`sleep`函数，很慢，非常慢，计时仍在继续
3. 3秒到了，计时事件`timeout`完成，`task()`进入Event Queue，但是`sleep`也太慢了吧，还没执行完，只好等着
4. `sleep`终于执行完了，`task()`终于从Event Queue进入了主线程执行



结果`task()` 执行时间的远大于3秒

 

###  setInterval

`setInterval(fn,ms)`每过`ms`秒，会有`fn`进入Event Queue。一旦**setInterval的回调函数fn执行时间超过了延迟时间ms，那么就完全看不出来有时间间隔了**。

  

### task

macro-task(宏任务)：包括整体代码script，setTimeout，setInterval

micro-task(微任务)：Promise，process.nextTick



<img width="85%" src="https://user-gold-cdn.xitu.io/2017/11/21/15fdcea13361a1ec?imageView2/0/w/1280/h/960/ignore-error/1" />

事件循环的顺序，决定JS代码的执行顺序。进入整体代码(宏任务)后，开始第一次循环。接着执行所有的微任务。然后再次从宏任务开始，找到其中一个任务队列执行完毕，再执行所有的微任务。

例子一：

```js
setTimeout(function() {
    console.log('setTimeout');
})

new Promise(function(resolve) {
    console.log('promise');
    resolve();
}).then(function() {
    console.log('then');
})

console.log('console');

//promise
//console
//then
//setTimeout
```

第一轮事件循环

1. 整体script作为第一个宏任务进入主线程，进入主线程
2. `setTimeout`的回调函数注册后分发到宏任务Event Queue
3. `new Promise`立即执行，`then`函数分发到微任务Event Queue
4. `console.log()`，立即执行
5. 整体代码script作为第一个宏任务执行结束
6. 检查微任务，执行微任务Event Queue的`then`

第二轮事件循环

1. 从宏任务Event Queue开始，执行`setTimeout`的回调函数



例子二：

```js
console.log('1');

setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
process.nextTick(function() {
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')
})

setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})

// 1 7 6 8 2 4 3 5 9 11 10 12
```

第一轮事件循环

1. 整体script作为第一个宏任务进入主线程，执行`console.log('1')`

2. `setTimeout `回调函数被分发到宏任务Event Queue中，记为`setTimeout1`

   微任务：

   宏任务：  setTimeout1

3. `process.nextTick() `回调函数被分发到微任务Event Queue中，记为`process1`

   微任务：process1

   宏任务：  setTimeout1
   
4. `new Promise`直接执行，`then`被分发到微任务Event Queue中，记为`then1`

   微任务：process1，then1

   宏任务：  setTimeout1
   
5. `setTimeout `回调函数被分发到宏任务Event Queue中，记为`setTimeout2`

   微任务：process1，then1

   宏任务：  setTimeout1，setTimeout2
   
6. 宏任务（整体代码）结束，检查微任务

7. 执行微任务`process1`

8. 执行微任务`then1`

第二轮事件循环

1. 执行宏任务`setTimeout1`，原理同上

 第三轮事件循环

1. 执行宏任务`setTimeout2`，原理同上



补充：用nodejs执行会有差异，`setTimeout1`和`setTimeout2`似乎是同时执行

 

<img src="https://user-gold-cdn.xitu.io/2017/11/21/15fdd96beade6575?imageView2/0/w/1280/h/960/ignore-error/1" />

 





























