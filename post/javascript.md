### Call stack

> https://developer.mozilla.org/en-US/docs/Glossary/Call_stack

A call stack is a mechanism for an interpreter (like  the JavaScript interpreter in a web browser) to keep track of its place  in a script that calls multiple functions — what function is currently being run, what functions are called from within that function and should be called next, etc.

- When a script calls a function, the interpreter adds it to the call stack and then starts carrying out the function.
- Any functions that are called by that function are added to the call stack further up, and run where their calls are reached.
- When the current function is finished, the interpreter takes it off  the stack and resumes execution where it left off in the last code  listing.
- If the stack takes up more space than it had assigned to it, it results in a "stack overflow" error.



### Event loop

<img width="80%" src="https://user-gold-cdn.xitu.io/2017/11/21/15fdd88994142347?imageView2/0/w/1280/h/960/ignore-error/1" />



 ```js
let data = [];
$.ajax({
    url:www.javascript.com,
    data:data,
    success:() => {
        console.log('Send success!');
    }
})
console.log('End');
 ```

1. 注册回调函数`success`
2. 执行`console.log('End')`
3. ajax事件完成，回调函数`success`进入Event Queue
4. 主线程从Event Queue获取并执行`success`

 

### Set Timeout

It's important to note that setTimeout(..) doesn't put your callback on the event loop queue. What it does is set up a timer; when the timer expires, the environment places your callback into the event loop, such that some future tick will pick it up and execute it.



 ```js
setTimeout(fn,3000);
sleep(10000000);
 ```

1. `fn`进入Event Table，计时开始
2. `sleep`占据主线程
3. 3秒时，`fn`进入Event Queue，等待主线程
4. `sleep`等待1000秒后
5. Event Queue`fn`进入主线程执行

 

###  Set Interval

`setInterval(fn,ms)`每`ms`秒，`fn`进入Event Queue



### Task

> https://juejin.im/post/59e85eebf265da430d571f89

- Microtasks(微任务) include process.nextTick, promise, Object.observe and MutationObserver 

- Macrotasks(宏任务) include script, setTimeout, setInterval, setImmediate, I/O and UI rendering



<img width="85%" src="https://user-gold-cdn.xitu.io/2017/11/21/15fdcea13361a1ec?imageView2/0/w/1280/h/960/ignore-error/1" />



- So the correct sequence of an event loop looks like this:

1. Execute synchronous codes, which belongs to macrotask

2. Once call stack is empty, query if any microtasks need to be executed

3. Execute all the microtasks

4. If necessary, render the UI

5. Then start the next round of the Event loop, and execute the asynchronous operations in the macrotask

 

example a：

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

1st Event Loop

1. 主线程执行 Macrotasks `script`
2. `setTimeout` function 进入 Macrotasks Event Queue
3. `new Promise`立即执行，`then` function 进入 Microtasks Event Queue
4. 立即执行`console.log()`
5. Macrotasks `script`结束
6. 检查 Microtasks Event Queue
7. 执行`then` function

2nd Event Loop

1. 检查 Macrotasks Event Queue，执行`setTimeout` function



example b：

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

1. 宏任务script进入主线程

2. 执行`console.log('1')`

3. `setTimeout `回调函数被分发到宏任务Event Queue中，记为`setTimeout1`

   微任务：

   宏任务：  setTimeout1

4. `process.nextTick() `回调函数分发到微任务Event Queue中，记为`process1`

   微任务：process1

   宏任务：  setTimeout1

5. `new Promise`直接执行，`then`分发到微任务Event Queue中，记为`then1`

   微任务：process1，then1

   宏任务：  setTimeout1

6. `setTimeout `回调函数分发到宏任务Event Queue中，记为`setTimeout2`

   微任务：process1，then1

   宏任务：  setTimeout1，setTimeout2

7. 宏任务script结束

8. 检查微任务，执行`process1`

9. 检查微任务，执行`then1`

第二轮事件循环

1. 执行宏任务`setTimeout1`，原理同上

 第三轮事件循环

1. 执行宏任务`setTimeout2`，原理同上



疑问：用nodejs执行会有差异，`setTimeout1`和`setTimeout2`同时执行

 

<img src="https://user-gold-cdn.xitu.io/2017/11/21/15fdd96beade6575?imageView2/0/w/1280/h/960/ignore-error/1" />

### Primitive

> https://developer.mozilla.org/en-US/docs/Glossary/Primitive
>
> https://justjavac.com/javascript/2012/12/22/javascript-values-not-everything-is-an-object.html

A **primitive** (primitive value, primitive data type) is data that is not an object  and has no methods In JavaScript, there are 6 primitive data types: string, number, boolean, null, undefined, symbol (new in ECMAScript 2015).



All primitives are **immutable,** i.e., they cannot be  altered. It is important not to confuse a primitive itself with a  variable assigned a primitive value. The variable may be reassigned a  new value, but the existing value can not be changed in the ways that  objects, arrays, and functions can be altered.



### Method

方法执行时，建立内存栈，方法内的变量存入这个内存栈

方法结束时，方法内存栈销毁



### Object

- 占用空间不固定

- Object保存在堆

- Object不会随方法的结束而销毁

- 可变

  ```js
  var obj = {};
  obj.foo = 123;
  obj.foo//123
  ```

- 每个对象都有自己唯一的标识符，通过字面量或构造函数创建的对象和任何其他对象都不相等

  ```js
  {} === {}
  false
  ```

- 对象是通过引用来比较的，只有两个对象有相同的标识，才认为这个对象是相等的

  ```js
  var obj = {};
  obj === obj
  ```

- 变量保存对象的引用

  ```js
  var var1 = {};
  var var2 = var1;
  var1.foo = 123;
  var2.foo //123
  ```



### Reference types

> https://www.cnblogs.com/leiting/p/8081413.html
>
> https://blog.fundebug.com/2017/08/09/explain_value_reference_in_js/

- 值传递

  ```js
  function multiply(x, y) {
      return x * y;
  }
  multiply(2, 3);
  ```

- 引用传递

  ```js
  function change(person) {
      person.age = 25;
      return person;
  }
  var alex = {
      name: 'Alex',
      age: 30
  };
  var changedAlex = change(alex);
  console.log(alex); // { name: 'Alex', age: 25 }
  console.log(changedAlex); // { name: 'Alex', age: 25 }
  ```



### This

> https://www.jianshu.com/p/6b4333e78bf5
>
> https://juejin.im/post/5b9f176b6fb9a05d3827d03f

```js
//===example1===
var x = 1;
function test() {
    //this -> window
    console.log(this.x);
}
test();  // 1

//===example2===
var obj = {
    x: 1,
    y: function foo() {
        //this -> obj
        console.log(this.x);
    }
};
obj.y(); // 1

//====example3===
var x = 2;
function test() {
    this.x = 1;
}
//this -> obj
var obj = new test();
obj.x // 1
```



### Apply & Call & Bind

- apply 改变函数的指向，参数是一个数组，参数为空时，指向全局对象

```js
var x = 0;
var obj = {
    x: 1,
    y: function foo() {
        console.log(this.x);
    }
};

//this -> window
obj.y.apply(); // 0
//this -> obj
obj.y.apply(obj); //1
```



- call 与apply的区别：参数是多个
- bind 与call相似，返回一个函数

```js
function greet() {
    console.log(this.name);
}

const user = {
    name: "Tyler"
};

const newFn = greet.bind(user);
newFn(); //Tyler
```



### MVVM

In the JQuery period, if you need to refresh the UI, you need to get the corresponding DOM and then update the UI, so the data and business logic are strongly-coupled with the page.

 

In MVVM, the UI is driven by data. Once the data is changed, the corresponding UI will be refreshed. If the UI changes, the corresponding data will also be changed. In this way, we can only care about the data flow in business processing without dealing with the page directly. ViewModel only cares about the processing of data and business and does not care how View handles data. In this case, we can separate the View from the Model. If either party changes, it does not necessarily need to change the other party, and some reusable logic can be placed in a ViewModel, allowing multiple Views to reuse this ViewModel.

 

In MVVM, the core is the two-way binding of data, such as dirty checking by Angular and data hijacking in Vue.



### DOM

> https://javascript.ruanyifeng.com/dom/node.html

- DOM（Document Object Model）：网页转成的一个JS对象

- 结构化文档（比如 HTML 和 XML） -> DOM Tree

- Node：DOM 的最小组成单位

- DOM tree：由各种不同类型的节点组成

- type of node
  - Document
  - Element
  - Attribute
  - Text
  - DocumentFragment
  - DocumentType
  - Comment

- node relationship
  - parentNode：直接的上级节点
  - childNodes：直接的下级节点
  - sibling：拥有同一个父节点的节点



###  New

> http://javascript.ruanyifeng.com/oop/basic.html

- 规则

  ```js
  var Vehicle = function () {
      'use strict';//防止this指向全局对象
      //this 指向对象实例
      this.price = 1000;
  };
  //首字母大写
  var v = new Vehicle();
  v.price // 1000
  ```

- new 原理

  ```js
  function _new(/* 构造函数 */ constructor, /* 构造函数参数 */ params) {
      // 将 arguments 对象转为数组
      var args = [].slice.call(arguments);
      // 取出构造函数
      var constructor = args.shift();
      // 创建一个空对象，继承构造函数的 prototype 属性
      var context = Object.create(constructor.prototype);
      // 执行构造函数
      var result = constructor.apply(context, args);
      // 如果返回结果是对象，就直接返回，否则返回 context 对象
      return (typeof result === 'object' && result != null) ? result : context;
  }
  
  // 实例
  var actor = _new(Person, '张三', 28);
  ```



### prototype

> Reference
>
> https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain
>
> https://github.com/bigdots/blog/issues/1
>
> https://blog.csdn.net/SpicyBoiledFish/article/details/71123162
>
> https://blog.csdn.net/cecilia620/article/details/71158048
>
> https://www.jianshu.com/p/15ac7393bc1f
>
> https://github.com/stone0090/javascript-lessons/tree/master/2.5-Prototype

JavaScript is a bit confusing for developers experienced in  class-based languages (like Java or C++), as it is dynamic and does not  provide a `class` implementation per se (the `class` keyword is introduced in ES2015, but is syntactical sugar, JavaScript remains prototype-based).

When it comes to inheritance, JavaScript only has one construct:  objects. Each object has a private property which holds a link to  another object called its **prototype**. That prototype object has a prototype of its own, and so on until an object is reached with `null` as its prototype. By definition, `null` has no prototype, and acts as the final link in this **prototype chain**.



<img src="https://www.ibm.com/developerworks/cn/web/1306_jiangjj_jsinstanceof/figure1.jpg" />



```js
let f = function () {
   this.a = 1;
   this.b = 2;
}
let o = new f(); // {a: 1, b: 2}

f.prototype.b = 3;
f.prototype.c = 4;

// {a: 1, b: 2} ---> {b: 3, c: 4} ---> Object.prototype ---> null
console.log(o.a); // 1
console.log(o.b); // 2
console.log(o.c); // 4
console.log(o.d); // undefined
```



```js
var a = {a: 1};
// a ---> Object.prototype ---> null

var b = Object.create(a);
// b ---> a ---> Object.prototype ---> null
console.log(b.a); // 1 (inherited)

var c = Object.create(b);
// c ---> b ---> a ---> Object.prototype ---> null

var d = Object.create(null);
// d ---> null
console.log(d.hasOwnProperty);
// undefined, because d doesn't inherit from Object.prototype
```



构造函数、原型对象、实例化对象三者的关系 

<img src="https://img-blog.csdn.net/20170503151554392?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3BpY3lCb2lsZWRGaXNo/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" /> 



<img src="https://img-blog.csdn.net/20170503152146141?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3BpY3lCb2lsZWRGaXNo/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" /> 



configurable

```js 
var person = { name: 'TOM' } 
delete person.name; // true 

Object.defineProperty(person, 'name', { 
    configurable: false, 
    value: 'Jake' 
}) 
delete person.name // false 
console.log(person.name) // Jake 
person.name = "alex";  
console.log(person.name) // Jake 
```




 writable

```js 
var person = { name: 'TOM' }
person.name = 'Jake';
console.log(person.name);

Object.defineProperty(person, 'name', {
    writable: false 
})
person.name = 'alex';
console.log(person.name); // Jake
```

 

get/set

```js 
var person = {}
Object.defineProperties(person, {
    name: { value: 'Jake', configurable: true }, 
    age: {
        get: function() { return this.value || 22 },
        set: function(value) { this.value = value }
    }
})
 
person.name // Jake
person.age // 22
```

  

```js 
var person = {}
Object.defineProperty(person, 'name', {
    value: 'alex',
    writable: false,
    configurable: false
})
var descripter = Object.getOwnPropertyDescriptor(person, 'name');
console.log(descripter);

/*descripter = {
    configurable: false,
    enumerable: false,
    value: 'alex',
    writable: false
}*/
```



重写原型

```js
function Person(){}

Person.prototype = {
    constructor : Person,
    name : "Stone",
    age : 28,
    job: "Software Engineer",
    sayName : function () {
        console.log(this.name);
    }
};
```



```js
function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    //私有在构造函数内定义
    this.friends = ["ZhangSan", "LiSi"];
}

Person.prototype = {
    constructor : Person,
    //共享函数用原型定义
    sayName : function(){
        console.log(this.name);
    }
}

var person1 = new Person("Stone", 28, "Software Engineer");
var person2 = new Person("Sophie", 29, "English Teacher");

person1.friends.push("WangWu");
console.log(person1.friends);    // "ZhangSan,LiSi,WangWu"
console.log(person2.friends);    // "ZhangSan,LiSi"
console.log(person1.friends === person2.friends);    // false
console.log(person1.sayName === person2.sayName);    // true
```



### Grammar and types

**Declarations**

var: Declares a variable, optionally initializing it to a value.

let: Declares a block-scoped, local variable, optionally initializing it to a value.

const: Declares a block-scoped, read-only named constant.



**Variable scope**

```js
if (true) {
  let y = 5;
}
console.log(y);  // ReferenceError: y is not defined
```



**Variable hoisting**

```js
/**
 * Example 1
 */
console.log(x === undefined); // true
var x = 3;

/**
 * Example 2
 */
// will return a value of undefined
var myvar = 'my value';
 
(function() {
  console.log(myvar); // undefined
  var myvar = 'local value';
})();
```



**Function hoisting**

```js
/* Function declaration */

foo(); // "bar"

function foo() {
  console.log('bar');
}


/* Function expression */

baz(); // TypeError: baz is not a function

var baz = function() {
  console.log('bar2');
};
```



**Data types**

The latest ECMAScript standard defines seven data types:

- Six data types that are primitives:
  - Boolean. true and false.
  - null. A special keyword denoting a null value. Because JavaScript is case-sensitive, null is not the same as Null, NULL, or any other variant.
  - undefined. A top-level property whose value is not defined.
  - Number. An integer or floating point number. For example: 42 or 3.14159.
  - String. A sequence of characters that represent a text value. For example:  "Howdy"
  - Symbol (new in ECMAScript 2015). A data type whose instances are unique and immutable.
- and Object



**Data type conversion**

```js
'37' - 7 // 30
'37' + 7 // "377"
```



### Control flow and error handling

**Block statement**

```js
var x = 1;
{
  var x = 2;
}
console.log(x); // outputs 2
```



**Conditional statements**

```js
if (condition) {
  statement_1;
} else {
  statement_2;
}
```



**Exception handling statements**

```js
throw 'Error2';   // String type
throw 42;         // Number type
throw true;       // Boolean type
throw {toString: function() { return "I'm an object!"; } };
```



```js
// Create an object type UserException
function UserException(message) {
  this.message = message;
  this.name = 'UserException';
}

// Make the exception convert to a pretty string when used as a string 
// (e.g. by the error console)
UserException.prototype.toString = function() {
  return this.name + ': "' + this.message + '"';
}

// Create an instance of the object type and throw it
throw new UserException('Value too high');
```



```js
function getMonthName(mo) {
  mo = mo - 1; // Adjust month number for array index (1 = Jan, 12 = Dec)
  var months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul',
                'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
  if (months[mo]) {
    return months[mo];
  } else {
    throw 'InvalidMonthNo'; //throw keyword is used here
  }
}

try { // statements to try
  monthName = getMonthName(myMonth); // function could throw exception
}
catch (e) {
  monthName = 'unknown';
  logMyErrors(e); // pass exception object to error handler -> your own function
}
```



```js
openMyFile();
try {
  writeMyFile(theData); //This may throw an error
} catch(e) {  
  handleError(e); // If we got an error we handle it
} finally {
  closeMyFile(); // always close the resource
}
```



```js
function doSomethingErrorProne() {
  if (ourCodeMakesAMistake()) {
    throw (new Error('The message'));
  } else {
    doSomethingToGetAJavascriptError();
  }
}
....
try {
  doSomethingErrorProne();
} catch (e) {
  console.log(e.name); // logs 'Error'
  console.log(e.message); // logs 'The message' or a JavaScript error message)
}
```



**Promise**

A `Promise` is in one of these states:

- *pending*: initial state, not fulfilled or rejected.
- *fulfilled*: successful operation
- *rejected*: failed operation.
- *settled*: the Promise is either fulfilled or rejected, but not pending.

<img src="https://mdn.mozillademos.org/files/8633/promises.png">



### Loops and iteration

**for statement**
**do...while statement**

```js
var i = 0;
do {
  i += 1;
  console.log(i);
} while (i < 5);
```



**while statement**

```js
var n = 0;
var x = 0;
while (n < 3) {
  n++;
  x += n;
}
```



**labeled statement**

```js
markLoop:
while (theMark == true) {
   doSomething();
}
```



**break statement**

```js
for (var i = 0; i < a.length; i++) {
  if (a[i] == theValue) {
    break;
  }
}
```



**continue statement**

```js
var i = 0;
var n = 0;
while (i < 5) {
  i++;
  if (i == 3) {
    continue;
  }
  n += i;
  console.log(n);
}
//1,3,7,12
```



**for...in statement**

```js
var arr = [3, 5, 7];
arr.foo = 'hello';

for (var i in arr) {
   console.log(i); // logs "0", "1", "2", "foo"
}
```



**for...of statement**

```js
var arr = [3, 5, 7];
arr.foo = 'hello';

for (var i of arr) {
   console.log(i); // logs 3, 5, 7
}
```



### Functions



**recursion**

A function can refer to and call itself.

```js
function factorial(n) {
    console.trace();//查看调用栈
    if (n === 0) {
        return 1
    }
    return n * factorial(n - 1)
}
```



<img width="65%" src="https://user-gold-cdn.xitu.io/2017/6/20/d28ba98f3835845671655db33dfe14bb?imageView2/0/w/1280/h/960/ignore-error/1" />



尾递归：是一种递归的写法，避免不断将函数压栈最终导致堆栈溢出。通过设置一个累加参数，并且每一次都将当前的值累加上去，然后递归调用

```js
function factorial(n, total = 1) {
    console.trace();//查看调用栈
    if (n === 0) {
        return total
    }
    return factorial(n - 1, n * total)
}
factorial(3);
```



函数之间没有依赖关系，调用后可进行垃圾回收

factorial(3, 1)
factorial(2, 3)
factorial(1, 6)
factorial(0, 6)



补充：

Nodejs需要使用`strict mode`和`--harmony_tailcalls`开启尾递归(proper tail call)

```shell
node --harmony_tailcalls factorial.js
```



**Closures**

Closures are one of the most powerful features of JavaScript. JavaScript
allows for the nesting of functions and grants the inner function full 
access to all the variables and functions defined inside the outer 
function (and all other variables and functions that the outer function 
has access to). However, the outer function does not have access to the 
variables and functions defined inside the inner function. This provides
a sort of encapsulation for the variables of the inner function. Also, 
since the inner function has access to the scope of the outer function, 
the variables and functions defined in the outer function will live 
longer than the duration of the outer function execution, if the inner 
function manages to survive beyond the life of the outer function. A 
closure is created when the inner function is somehow made available to 
any scope outside the outer function.



闭包：有权访问另一个函数作用域中的变量的函数

通常，函数的作用域及其所有变量都会在函数执行结束后被销毁。但是，在创建了一个闭包以后，这个函数的作用域就会一直保存到闭包不存在为止

应用：设计私有的方法和变量

缺点：减慢处理速度，内存消耗



**Arguments object**

```js
function myConcat(separator) {
   var result = ''; // initialize list
   var i;
   // iterate through arguments
   for (i = 1; i < arguments.length; i++) {
      result += arguments[i] + separator;
   }
   return result;
}

// returns "red, orange, blue, "
myConcat(', ', 'red', 'orange', 'blue');
```



### Expressions and operators

**Bitwise operators**

The operands of all bitwise operators are converted to signed 32-bit  integers in two's complement format. Two's complement format means that a number's negative counterpart (e.g. 5 vs. -5) is all the number's bits  inverted (bitwise NOT of the number, a.k.a. ones' complement of the  number) plus one. For example, the following encodes the integer 314:



```
00000000000000000000000100111010
```

The following encodes `~314`, i.e. the ones' complement of `314`:

```
11111111111111111111111011000101
```

Finally, the following encodes `-314,` i.e. the two's complement of `314`:

```
11111111111111111111111011000110
```



-7>>1 = -4

```
00000000 00000000 00000000 00000111 //7
11111111 11111111 11111111 11111001 //-7
11111111 11111111 11111111 11111100	//-7>>1
00000000 00000000 00000000 00000100 //4
10000000 00000000 00000000 00000100 //-4
```



 -1>>>4 = ox0FFFFFFF

```
00000000 00000000 00000000 00000001 //1
11111111 11111111 11111111 11111111 //-1
00001111 11111111 11111111 11111111 //-1>>>4
```



**Unary operators**

delete

```js
x = 42;
var y = 43;
myobj = new Number();
myobj.h = 4;    // create property h
delete x;       // returns true (can delete if declared implicitly)
delete y;       // returns false (cannot delete if declared with var)
delete Math.PI; // returns false (cannot delete predefined properties)
delete myobj.h; // returns true (can delete user-defined properties)
delete myobj;   // returns true (can delete if declared implicitly)
```



typeof

```js
var myFun = new Function('5 + 2');
var shape = 'round';
var size = 1;
var foo = ['Apple', 'Mango', 'Orange'];
var today = new Date();

typeof myFun;       // returns "function"
typeof shape;       // returns "string"
typeof size;        // returns "number"
typeof foo;         // returns "object"
typeof today;       // returns "object"
typeof doesntExist; // returns "undefined"
```



**Relational operators**

```js
// Arrays
var trees = ['redwood', 'bay', 'cedar', 'oak', 'maple'];
0 in trees;        // returns true
3 in trees;        // returns true
6 in trees;        // returns false
'bay' in trees;    // returns false (you must specify the index number,
                   // not the value at that index)
'length' in trees; // returns true (length is an Array property)

// built-in objects
'PI' in Math;          // returns true
var myString = new String('coral');
'length' in myString;  // returns true

// Custom objects
var mycar = { make: 'Honda', model: 'Accord', year: 1998 };
'make' in mycar;  // returns true
'model' in mycar; // returns true
```



### Numbers and dates

**Numbers**

- Decimal numbers

  ```js
  1234567890
  42
  
  // Caution when using leading zeros:
  
  0888 // 888 parsed as decimal
  0777 // parsed as octal in non-strict mode (511 in decimal)
  /*
  Note that decimal literals can start with a zero (0) followed by another decimal digit, but if every digit after the leading 0 is smaller than 8, the number gets parsed as an octal number.
  */
  ```

- Binary numbers

  ```js
  var FLT_SIGNBIT  = 0b10000000000000000000000000000000; // 2147483648
  var FLT_EXPONENT = 0b01111111100000000000000000000000; // 2139095040
  var FLT_MANTISSA = 0B00000000011111111111111111111111; // 8388607
  ```


- Octal numbers

  ```js
  var n = 0755; // 493
  var m = 0644; // 420
  
  var a = 0o10; // ES2015: 8
  ```


- Hexadecimal numbers

  ```js
  0xFFFFFFFFFFFFFFFFF // 295147905179352830000
  0x123456789ABCDEF   // 81985529216486900
  0XA                 // 10
  ```


- Exponentiation

  ```js
  1E3   // 1000
  2e6   // 2000000
  0.1e2 // 10
  ```



JavaScript 内部，所有数字都是以64位浮点数形式储存

根据国际标准 IEEE 754，JavaScript 浮点数的64个二进制位

- 第1位：符号位，`0`表示正数，`1`表示负数
- 第2位到第12位（共11位）：指数部分，表示数值的大小( 0 ~ 2047 )
- 第13位到第64位（共52位）：小数部分（即有效数字），表示数值的精度( -2^53 ~ 2^53 )



```js
//浮点数不是精确的值
0.1 + 0.2 === 0.3	//false
0.3 / 0.1	// 2.9999999999999996
(0.3 - 0.2) === (0.2 - 0.1)	// false
```



### Regular expressions

```js
var myRe = /d(b+)d/g;
var myArray = myRe.exec('cdbbdbsbz');
```



```js
var re = /(\w+)\s(\w+)/;
var str = 'John Smith';
var newstr = str.replace(re, '$2, $1');
console.log(newstr);

// "Smith, John"
```



```js
var re = /\w+\s/g;
var str = 'fee fi fo fum';
var myArray = str.match(re);
console.log(myArray);

// ["fee ", "fi ", "fo "]
```



### Working with objects

In JavaScript, an object is a standalone entity, with properties and 
type. Compare it with a cup, for example. A cup is an object, with 
properties. A cup has a color, a design, weight, a material it is made 
of, etc. The same way, JavaScript objects can have properties, which 
define their characteristics.




