call stack

> Reference
>
> https://developer.mozilla.org/en-US/docs/Glossary/Call_stack

A **call stack** is a mechanism for an interpreter (like  the JavaScript interpreter in a web browser) to keep track of its place  in a script that calls multiple functions — what function is currently being run, what functions are called from within that function and should be called next, etc.

- When a script calls a function, the interpreter adds it to the call stack and then starts carrying out the function.
- Any functions that are called by that function are added to the call stack further up, and run where their calls are reached.
- When the current function is finished, the interpreter takes it off  the stack and resumes execution where it left off in the last code  listing.
- If the stack takes up more space than it had assigned to it, it results in a "stack overflow" error.



```js
function greeting() {
   sayHi();
}
function sayHi() {
   return "Hi!";
}
greeting();
```

The code above would be executed like this:

1. Ignore all functions, until it reaches the `greeting()` function.

2. Invoke the `greeting()` function.

3. Add the `greeting` function to the call stack list.

   Call stack list:

   greeting

4. Execute all lines of code inside the `greeting` function.

5. Get to the `sayHi()` function.

6. Add the `sayHi()` function to the call stack list.

   Call stack list:

   greeting

   sayHi

7. Execute all lines of code inside the `sayHi()` function, until reaches its end.

8. Return execution to the line that invoked `sayHi()` and continue executing the rest of the `greeting()` function.

9. Delete the `sayHi()` function from our call stack list.

   Call stack list:

   greeting

10. When everything inside the `greeting()` function has been executed, return to its invoking line to continue executing the rest of the JS code.

11. Delete the `greeting()` function from the call stack list.

    Call stack list: EMPTY


We started with an empty Call Stack, and whenever we invoke a function,  it is automatically added to the Call Stack, after executing all of its  codes, it is automatically removed from the Call Stack. At the end, we  ended up with an empty stack too.



### event loop

<img width="80%" src="https://user-gold-cdn.xitu.io/2017/11/21/15fdd88994142347?imageView2/0/w/1280/h/960/ignore-error/1" />



主线程内的任务为空时，会去检查Event Queue的函数



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

1. ajax进入Event Table，注册回调函数`success`
2. 执行`console.log('代码执行结束')`
3. ajax事件完成，回调函数`success`进入Event Queue
4. 主线程从Event Queue读取回调函数，执行`success`

 

### setTimeout

It's important to note that setTimeout(..) doesn't put your callback on the event loop queue. What it does is set up a timer; when the timer expires, the environment places your callback into the event loop, such that some future tick will pick it up and execute it.



 ```js
setTimeout(() => {
    task();
},3000);
sleep(10000000);
 ```

1. `task()`进入Event Table，计时开始
2. 执行`sleep`
3. 3秒到了，计时事件`timeout`完成，`task()`进入Event Queue，等待主线程
4. `sleep`执行完
5. 扫描Event Queue，`task()`进入主线程执行

 

###  setInterval

`setInterval(fn,ms)`每过`ms`秒，`fn`进入Event Queue

fn执行时间大于ms时，看起来fn连续执行，没有间隔



### task

> Reference
>
> https://juejin.im/post/59e85eebf265da430d571f89

Microtasks(微任务) include process.nextTick, promise, Object.observe and MutationObserver 

Macrotasks(宏任务) include script, setTimeout, setInterval, setImmediate, I/O and UI rendering



<img width="85%" src="https://user-gold-cdn.xitu.io/2017/11/21/15fdcea13361a1ec?imageView2/0/w/1280/h/960/ignore-error/1" />



So the correct sequence of an event loop looks like this:

1.Execute synchronous codes, which belongs to macrotask

2.Once call stack is empty, query if any microtasks need to be executed

3.Execute all the microtasks

4.If necessary, render the UI

5.Then start the next round of the Event loop, and execute the asynchronous operations in the macrotask

 

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

第一轮事件循环

1. 宏任务script进入主线程
2. `setTimeout`的回调函数注册后分发到宏任务Event Queue
3. `new Promise`立即执行，`then`函数分发到微任务Event Queue
4. 立即执行`console.log()`
5. 宏任务script执行结束
6. 检查微任务Event Queue，执行`then`

第二轮事件循环

1. 检查宏任务Event Queue，执行`setTimeout`的回调函数



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

### primitive

> Reference
>
> https://developer.mozilla.org/en-US/docs/Glossary/Primitive
>
> https://justjavac.com/javascript/2012/12/22/javascript-values-not-everything-is-an-object.html

A **primitive** (primitive value, primitive data type) is data that is not an [object](https://developer.mozilla.org/en-US/docs/Glossary/object) and has no [methods](https://developer.mozilla.org/en-US/docs/Glossary/method). In [JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript), there are 6 primitive data types: [string](https://developer.mozilla.org/en-US/docs/Glossary/string), [number](https://developer.mozilla.org/en-US/docs/Glossary/number), [boolean](https://developer.mozilla.org/en-US/docs/Glossary/boolean), [null](https://developer.mozilla.org/en-US/docs/Glossary/null), [undefined](https://developer.mozilla.org/en-US/docs/Glossary/undefined), [symbol](https://developer.mozilla.org/en-US/docs/Glossary/symbol) (new in [ECMAScript](https://developer.mozilla.org/en-US/docs/Glossary/ECMAScript) 2015).



All primitives are **immutable,** i.e., they cannot be  altered. It is important not to confuse a primitive itself with a  variable assigned a primitive value. The variable may be reassigned a  new value, but the existing value can not be changed in the ways that  objects, arrays, and functions can be altered.



占用空间固定，保存在栈中（当一个方法执行时，每个方法都会建立自己的内存栈，在这个方法内定义的变量将会逐个放入这块栈内存里，随着方法的执行结束，这个方法的内存栈也将自然销毁了。因此，所有在方法中定义的变量都是放在栈内存中的；栈中存储的是基础变量以及一些对象的引用变量，基础变量的值是存储在栈中，而引用变量存储在栈中的是指向堆中的数组或者对象的地址



A primitive type has a fixed size in memory. For example, a number occupies eight bytes of memory, and a boolean value can be represented with only one bit. The number type is the largest of the primitive types. If each JavaScript variable reserves eight bytes of memory, the variable can directly hold any primitive value.



```js
//1.原始值不可变
var str = "abc";
str.foo = 123;
str.foo //undefined

//2.原始值没有内部标识，按值比较
"abc" === "abc"
true
```



隐式转换：

```js
Boolean(undefined)//false
Boolean(0)//false
'1'+2 //12
Boolean('false')//true
Boolean('undefined')//true
3 + true; // 4
```



转换成false：

"" 空字串
0, -0, NaN
null
undefined
false





### object

占用空间不固定，保存在堆中（当我们在程序中创建一个对象时，这个对象将被保存到运行时数据区中，以便反复利用（因为对象的创建成本通常较大），这个运行时数据区就是堆内存。堆内存中的对象不会随方法的结束而销毁，即使方法结束后，这个对象还可能被另一个引用变量所引用（方法的参数传递时很常见），则这个对象依然不会被销毁，只有当一个对象没有任何引用变量引用它时，系统的垃圾回收机制才会在核实的时候回收它



分为：

原始值的包装器：`Boolean`, `Number`, `String`

`[]` 就是 `new Array()`

`{}` 就是 `new Object()`

`function() {}` 就是 `new Function()`

`/\s*/` 就是  `new RegExp("\\s*")`

`new Date("2011-12-24")`



特点：

```js
//1.可变
var obj = {};
obj.foo = 123;
obj.foo//123

//2.每个对象都有自己唯一的标识符，通过字面量或构造函数创建的对象和任何其他对象都不相等
{} === {}
false

//对象是通过引用来比较的，只有两个对象有相同的标识，才认为这个对象是相等的
var obj = {};
obj === obj

//3.变量保存了对象的引用，因此，如果两个变量应用了相同的对象——我们改变其中一个变量时，两一个也会随之改变
var var1 = {};
var var2 = var1;
var1.foo = 123;
var2.foo //123
```



```js
Object.prototype.toString.call(1) // "[object Number]"
Object.prototype.toString.call('hi') // "[object String]"
Object.prototype.toString.call({a:'hi'}) // "[object Object]"
Object.prototype.toString.call([1,'a']) // "[object Array]"
Object.prototype.toString.call(true) // "[object Boolean]"
Object.prototype.toString.call(() => {}) // "[object Function]"
Object.prototype.toString.call(null) // "[object Null]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(Symbol(1)) // "[object Symbol]"
```



#### wrap

```js
typeof "abc" //'string'
typeof new String("abc") //'object'

"abc" instanceof String //false
new String("abc") instanceof String //true

"abc" === new String("abc") //false

var a = new String("abc");
var b = new String("abc");
a == b //false
a == a //true
```



### Equality comparisons

- double equals (`==`) will perform a type conversion when comparing two things, and will handle `NaN`, `-0`, and `+0` specially to conform to IEEE 754 (so `NaN != NaN`, and `-0 == +0`);

- triple equals (`===`) will do the same comparison (including the special handling for `NaN`, `-0`, and `+0`) but without type conversion, by simply always returning `false` if the types differ;

- `Object.is` does no type conversion and no special handling for `NaN`, `-0`, and `+0` (giving it the same behavior as `===` except on those special numeric values).


```js
//两个变量指向同一个对象
var a1 = ['Hi!'];
var a2 = a1;
console.log(a1 === a2); // true

//两个变量指向不同对象
var b1 = ["Hi!"];
var b2 = ["Hi!"];
console.log(b1 === b2); // false

//比较对象的值
var arr1str = JSON.stringify(arr1);
var arr2str = JSON.stringify(arr2);
console.log(arr1str === arr2str); // true
```



### reference types

> Reference
>
> https://www.cnblogs.com/leiting/p/8081413.html
>
> https://blog.fundebug.com/2017/08/09/explain_value_reference_in_js/

```js
//1.值传递
function multiply(x, y) {
    return x * y;
}
multiply(2, 3);

//2.引用传递
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





### this

> Reference
>
> https://www.jianshu.com/p/6b4333e78bf5
>
> https://juejin.im/post/5b9f176b6fb9a05d3827d03f

this是函数运行时所在的环境对象

this 指向最后调用它的对象

箭头函数的 this 始终指向函数定义时的 this，而非执行时

匿名函数的 this 指向 window

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



1. 隐式绑定

   ```js
   const user = {
       name: 'Tyler',
       greet() {
           console.log(this.name);
       }
   };
   user.greet();
   ```

2. 显式绑定

   ```js
   function greet () {
       alert(`Hello, my name is ${this.name}`)
   };
   
   const user = {
       name: 'Tyler',
       age: 27,
   };
   
   greet.call(user);
   ```

3. new 绑定

   ```js
   function User (name, age) {
     this.name = name
     this.age = age
   };
   
   const me = new User('Tyler', 27);//this指向新对象
   ```

4. window 绑定

   ```js
   window.age = 27
   
   function sayAge () {
       console.log(`My age is ${this.age}`)
   }
   
   sayAge ();
   ```



改变this指向

- 使用 ES6 的箭头函数
- 在函数内部使用 `_this = this`
- 使用 `apply`、`call`、`bind`
- new 实例化一个对象



#### apply

改变函数的指向，参数是一个数组，参数为空时，指向全局对象

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



#### call

跟apply相似，传的参数是一个个的



#### bind

跟call相似，但返回一个函数

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

> Reference
>
> https://javascript.ruanyifeng.com/dom/node.html

DOM（Document Object Model）：JavaScript 操作网页的接口，它的作用是将网页转为一个 JavaScript 对象，从而可以用脚本进行各种操作

浏览器会根据 DOM 模型，将结构化文档（比如 HTML 和 XML）解析成一系列的节点，再由这些节点组成一个树状结构（DOM Tree）。所有的节点和最终的树状结构，都有规范的对外接口



Node：DOM 的最小组成单位

DOM 树：由各种不同类型的节点组成



**7种节点**

Document

Element

Attribute

Text

DocumentFragment

DocumentType

Comment



**节点的三种关系（除根节点）**

parentNode：直接的上级节点

childNodes：直接的下级节点

sibling：拥有同一个父节点的节点



**Node 接口的属性**

```js
Node.nodeType
Node.nodeName
Node.nodeValue
Node.textContent //后代节点的文本内容
Node.baseURI
Node.ownerDocument //返回当前节点所在的顶层文档对象
Node.nextSibling //当前节点的下一个节点
Node.previousSibling //当前节点的上一个节点
Node.parentNode
Node.parentElement //当前节点的父元素节点
Node.firstChild
Node.lastChild
Node.childNodes //当前节点的所有子节点
Node.isConnected //当前节点是否在文档之中
```



**Node接口方法**

```js
Node.appendChild()
Node.hasChildNodes()
Node.cloneNode()
Node.insertBefore()
Node.removeChild()
Node.replaceChild()
Node.contains()
Node.compareDocumentPosition()
Node.isEqualNode()
Node.isSameNode()
Node.normalize()
Node.getRootNode()
```



**NodeList 接口**

```js
NodeList.prototype.length
NodeList.prototype.forEach()
NodeList.prototype.item() //成员的位置
NodeList.prototype.keys()
NodeList.prototype.values()
NodeList.prototype.entries()
```



**ParentNode 接口**

```js
ParentNode.children
ParentNode.firstElementChild
ParentNode.lastElementChild
ParentNode.childElementCount
ParentNode.append()
ParentNode.prepend()
```



**ChildNode 接口**

```js
ChildNode.remove()
ChildNode.before()
ChildNode.after()
ChildNode.replaceWith()
```



###  new

> Reference
>
> http://javascript.ruanyifeng.com/oop/basic.html

构造函数名首字母大写

函数内的`this`是对象实例

`new`执行构造函数，返回一个实例对象

```js
var Vehicle = function () {
    'use strict';//防止this指向全局对象
    this.price = 1000;
};
var v = new Vehicle();
v.price // 1000
```

 

 new执行步骤

1. 创建一个空对象，作为将要返回的对象实例
2. 将这个空对象的原型，指向构造函数的`prototype`属性
3. 将这个空对象赋值给函数内部的`this`关键字
4. 开始执行构造函数内部的代码



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



`new`命令调用时，`new.target`指向当前函数

```js
function f() {
    console.log(new.target === f);
}

f() // false
new f() // true
```



Object.create()

```js
var person1 = {
    name: 'Tom',
    age: 38,
    greeting: function() {
        console.log('Hi! I\'m ' + this.name + '.');
    }
};

var person2 = Object.create(person1);

person2.name
person2.greeting()
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




