### Call stack

> https://developer.mozilla.org/en-US/docs/Glossary/Call_stack

A call stack is a mechanism for an interpreter (like  the JavaScript interpreter in a web browser) to keep track of its place  in a script that calls multiple functions — what function is currently being run, what functions are called from within that function and should be called next, etc.

- When a script calls a function, the interpreter adds it to the call stack and then starts carrying out the function.
- Any functions that are called by that function are added to the call stack further up, and run where their calls are reached.
- When the current function is finished, the interpreter takes it off  the stack and resumes execution where it left off in the last code  listing.
- If the stack takes up more space than it had assigned to it, it results in a "stack overflow" error.



### Set Timeout

It's important to note that `setTimeout()` doesn't put your callback on the event loop queue. What it does is set up a timer; when the timer expires, the environment places your callback into the event loop, such that some future tick will pick it up and execute it.



###  Set Interval

The `setInterval()` method, offered on the Window and Worker interfaces, repeatedly calls a function or executes a code snippet, with a fixed time delay between each call. It returns an interval ID which uniquely identifies the interval



### Even Loop

> https://juejin.im/post/59e85eebf265da430d571f89

- Microtasks(微任务) include process.nextTick, promise, Object.observe and MutationObserver 

- Macrotasks(宏任务) include script, setTimeout, setInterval, setImmediate, I/O and UI rendering



<img width="85%" src="https://user-gold-cdn.xitu.io/2017/11/21/15fdcea13361a1ec?imageView2/0/w/1280/h/960/ignore-error/1" />

 

### Primitive

> https://developer.mozilla.org/en-US/docs/Glossary/Primitive
>

A **primitive** (primitive value, primitive data type) is data that is not an object  and has no methods In JavaScript, there are 6 primitive data types: string, number, boolean, null, undefined, symbol (new in ECMAScript 2015).



All primitives are **immutable,** i.e., they cannot be  altered. It is important not to confuse a primitive itself with a  variable assigned a primitive value. The variable may be reassigned a  new value, but the existing value can not be changed in the ways that  objects, arrays, and functions can be altered.



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



### Prototype

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



When it comes to inheritance, JavaScript only has one construct:  objects. Each object has a private property which holds a link to  another object called its **prototype**. That prototype object has a prototype of its own, and so on until an object is reached with `null` as its prototype. By definition, `null` has no prototype, and acts as the final link in this **prototype chain**.



<img src="https://www.ibm.com/developerworks/cn/web/1306_jiangjj_jsinstanceof/figure1.jpg" />



<img src="https://img-blog.csdn.net/20170503152146141?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3BpY3lCb2lsZWRGaXNo/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" />



**Prototype chain example**

```js
let f = function () {
   this.a = 1;
   this.b = 2;
}
let o = new f(); // {a: 1, b: 2}

f.prototype.b = 3;
f.prototype.c = 4;

// {a: 1, b: 2} -> {b: 3, c: 4} -> Object.prototype -> null
console.log(o.a); // 1
console.log(o.b); // 2
console.log(o.c); // 4
console.log(o.d); // undefined
```



```js
var a = {a: 1};
// a -> Object.prototype -> null

var b = Object.create(a);
// b -> a -> Object.prototype -> null
console.log(b.a); // 1 (inherited)

var c = Object.create(b);
// c -> b -> a -> Object.prototype -> null

var d = Object.create(null);
// d -> null
console.log(d.hasOwnProperty);
// undefined, because d doesn't inherit from Object.prototype  
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



### Variable

- Declarations
  - var: Declares a variable, optionally initializing it to a value.
  - let: Declares a block-scoped, local variable, optionally initializing it to a value.
  - const: Declares a block-scoped, read-only named constant.

- Variable scope

  ```js
  if (true) {
    let y = 5;
  }
  console.log(y);  // ReferenceError: y is not defined
  ```


- Variable hoisting

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

- Function hoisting

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



### Data types

- Six data types that are primitives:
  - Boolean. true and false.
  - null. A special keyword denoting a null value. Because JavaScript is case-sensitive, null is not the same as Null, NULL, or any other variant.
  - undefined. A top-level property whose value is not defined.
  - Number. An integer or floating point number. For example: 42 or 3.14159.
  - String. A sequence of characters that represent a text value. For example:  "Howdy"
  - Symbol (new in ECMAScript 2015). A data type whose instances are unique and immutable.

- conversion

```js
'37' - 7 // 30
'37' + 7 // "377"
```



### Statement

- Block statement

    ```js
    var x = 1;
    {
      var x = 2;
    }
    console.log(x); // outputs 2
    ```

- Conditional statements

    ```js
    if (condition) {
      statement_1;
    } else {
      statement_2;
    }
    ```

- Exception handling statements

    ```js
    throw 'Error2';   // String type
    throw 42;         // Number type
    throw true;       // Boolean type
    throw {toString: function() { return "I'm an object!"; } };
    ```



### Exception handling

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
} catch (e) {
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



### Promise

- A Promise is in one of these states:
  - pending: initial state, not fulfilled or rejected
  - fulfilled: successful operation
  - rejected: failed operation
  - settled: the Promise is either fulfilled or rejected, but not pending

<img src="https://mdn.mozillademos.org/files/8633/promises.png">



### Loops

```js
function t1() {
  //do...while statement
  var i = 0;
  do {
    i += 1;
    console.log(i);
  } while (i < 5);
  //1 2 3 4 5
}

function t2() {
  //while statement
  var i = 0;
  while (i < 5) {
    i++;
    console.log(i);
  }
  //1 2 3 4 5
}

function t3() {
  //break statement
  var i = 0;
  while (i < 5) {
    i++;
    if (i == 3) {
      break;
    }
    console.log(i);
  }
  //1 2
}

function t4() {
  //continue statement
  var i = 0;
  while (i < 5) {
    i++;
    if (i == 3) {
      continue;
    }
    console.log(i);
  }
  //1 2 4 5
}

function t5() {
  //for...in statement
  for (var i in [3, 5, 7]) {
    console.log(i);
  }
  //0 1 2
}

function t6() {
  //for...of statement
  for (var i of [3, 5, 7]) {
    console.log(i);
  }
  //3 5 7
}

t6();
```



### Closures

- Closures are one of the most powerful features of JavaScript. JavaScript allows for the nesting of functions and grants the inner function full  access to all the variables and functions defined inside the outer  function (and all other variables and functions that the outer function  has access to). However, the outer function does not have access to the  variables and functions defined inside the inner function. This provides a sort of encapsulation for the variables of the inner function. Also,  since the inner function has access to the scope of the outer function,  the variables and functions defined in the outer function will live  longer than the duration of the outer function execution, if the inner  function manages to survive beyond the life of the outer function. A  closure is created when the inner function is somehow made available to  any scope outside the outer function.



### Regular expressions

- Return result and info

```js
var myRe = /d(b+)d/g;
var myArray = myRe.exec('cdbbdbsbz');
//[ 'dbbd', 'bb', index: 1, input: 'cdbbdbsbz' ]
```

- Replace match item

```js
var re = /(\w+)\s(\w+)/;
var str = 'John Smith';
var newstr = str.replace(re, '$2, $1');
console.log(newstr);

// "Smith, John"
```

- Search match item

```js
var re = /\w+\s/g;
var str = 'fee fi fo fum';
var myArray = str.match(re);
console.log(myArray);

// ["fee ", "fi ", "fo "]
```


