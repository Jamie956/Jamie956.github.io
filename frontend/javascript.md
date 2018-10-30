> 参考资料：
>
> https://github.com/stephentian/33-js-concepts
>
> https://developer.mozilla.org/en-US/docs/Glossary/Call_stack
>
> https://juejin.im/post/59e85eebf265da430d571f89
>
> https://developer.mozilla.org/en-US/docs/Glossary/Primitive
>
> https://wangdoc.com/javascript/types/number.html
>
> https://www.cnblogs.com/leiting/p/8081413.html
>
> https://blog.fundebug.com/2017/08/09/explain_value_reference_in_js/
>
> https://www.jianshu.com/p/6b4333e78bf5
>
> https://juejin.im/post/5b9f176b6fb9a05d3827d03f



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

### primitive

A **primitive** (primitive value, primitive data type) is data that is not an [object](https://developer.mozilla.org/en-US/docs/Glossary/object) and has no [methods](https://developer.mozilla.org/en-US/docs/Glossary/method). In [JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript), there are 6 primitive data types: [string](https://developer.mozilla.org/en-US/docs/Glossary/string), [number](https://developer.mozilla.org/en-US/docs/Glossary/number), [boolean](https://developer.mozilla.org/en-US/docs/Glossary/boolean), [null](https://developer.mozilla.org/en-US/docs/Glossary/null), [undefined](https://developer.mozilla.org/en-US/docs/Glossary/undefined), [symbol](https://developer.mozilla.org/en-US/docs/Glossary/symbol) (new in [ECMAScript](https://developer.mozilla.org/en-US/docs/Glossary/ECMAScript) 2015).

All primitives are **immutable,** i.e., they cannot be  altered. It is important not to confuse a primitive itself with a  variable assigned a primitive value. The variable may be reassigned a  new value, but the existing value can not be changed in the ways that  objects, arrays, and functions can be altered.



JavaScript 内部，所有数字都是以64位浮点数形式储存，即使整数也是如此

`1 === 1.0 // true`



浮点数不是精确的值

```js
0.1 + 0.2 === 0.3	//false
0.3 / 0.1	// 2.9999999999999996
(0.3 - 0.2) === (0.2 - 0.1)	// false
```



#### 数值精度

根据国际标准 IEEE 754，JavaScript 浮点数的64个二进制位

- 第1位：符号位，`0`表示正数，`1`表示负数
- 第2位到第12位（共11位）：指数部分
- 第13位到第64位（共52位）：小数部分（即有效数字）

符号位决定了一个数的正负，指数部分决定了数值的大小( 0 ~ 2047 )，小数部分决定了数值的精度( -2^53 ~ 2^53 )

```js
Math.pow(2, 53)
// 9007199254740992
 
Math.pow(2, 53) + 1
// 9007199254740992

Math.pow(2, 53) + 2
// 9007199254740994

// 多出的三个有效数字，将无法保存
9007199254740992111
// 9007199254740992000
```



#### 进制

- 十进制：没有前导0的数值
- 八进制：有前缀`0o`或`0O`的数值，或者有前导0、且只用到0-7的八个阿拉伯数字的数值
- 十六进制：有前缀`0x`或`0X`的数值
- 二进制：有前缀`0b`或`0B`的数值。



#### parseInt

```js
parseInt('123') // 123
parseInt('   81') // 81

parseInt(1.23) // 1
// 等同于
parseInt('1.23') // 1
```





### type

#### 值类型（primitive types）

- 占用空间固定，保存在栈中（当一个方法执行时，每个方法都会建立自己的内存栈，在这个方法内定义的变量将会逐个放入这块栈内存里，随着方法的执行结束，这个方法的内存栈也将自然销毁了。因此，所有在方法中定义的变量都是放在栈内存中的；栈中存储的是基础变量以及一些对象的引用变量，基础变量的值是存储在栈中，而引用变量存储在栈中的是指向堆中的数组或者对象的地址

- 保存与复制的是值本身

- 使用typeof检测数据的类型

- 基本类型数据是值类型（Numbers, boolean values, null undefined）

- A primitive type has a fixed size in memory. For example, a number
  occupies eight bytes of memory, and a boolean value can be
  represented with only one bit. The number type is the largest of the
  primitive types. If each JavaScript variable reserves eight bytes of
  memory, the variable can directly hold any primitive value.

```js
//值传递
var x = 10;
var a = x;
```



#### 引用类型（reference types）

- 占用空间不固定，保存在堆中（当我们在程序中创建一个对象时，这个对象将被保存到运行时数据区中，以便反复利用（因为对象的创建成本通常较大），这个运行时数据区就是堆内存。堆内存中的对象不会随方法的结束而销毁，即使方法结束后，这个对象还可能被另一个引用变量所引用（方法的参数传递时很常见），则这个对象依然不会被销毁，只有当一个对象没有任何引用变量引用它时，系统的垃圾回收机制才会在核实的时候回收它

- 保存与复制的是指向对象的一个指针

- 使用instanceof检测数据类型

- 使用new()方法构造出的对象是引用型（Object、Array、Function）

  ```js
  //引用传递
  var reference = [1];
  var refCopy = reference; //refCopy保存的是reference的地址
  ```




##### Object

```
var o = new Object(); //创建对象
```

属性

- constructor

  对创建对象的函数的引用（指针）。对于 Object 对象，该指针指向原始的 Object() 函数。

- Prototype

  对该对象的对象原型的引用。对于所有的对象，它默认返回 Object 对象的一个实例。



方法

- hasOwnProperty(property)

  判断对象是否有某个特定的属性。必须用字符串指定该属性。（例如，o.hasOwnProperty("name")）

- IsPrototypeOf(object)

  判断该对象是否为另一个对象的原型。

- PropertyIsEnumerable

  判断给定的属性是否可以用 for...in 语句进行枚举。

- ToString()

  返回对象的原始字符串表示。对于 Object 对象，ECMA-262 没有定义这个值，所以不同的 ECMAScript 实现具有不同的值。

- ValueOf()

  返回最适合该对象的原始值。对于许多对象，该方法返回的值都与 ToString() 的返回值相同。



`==`和`===`只会判断引用的地址是否相同

```js
//两个变量指向相同的对象
var arrRef = ['Hi!'];
var arrRef2 = arrRef;
console.log(arrRef === arrRef2); // true

//两个变量指向不同的对象
var arr1 = ["Hi!"];
var arr2 = ["Hi!"];
console.log(arr1 === arr2); // false

//比较对象的值
var arr1str = JSON.stringify(arr1);
var arr2str = JSON.stringify(arr2);
console.log(arr1str === arr2str); // true
```



##### 函数参数

基本类型数据传入函数时，函数会将这些数据拷贝赋值给函数的参数变量

```js
var hundred = 100;
var two = 2;
function multiply(x, y) {
    return x * y;
}
var twoHundred = multiply(hundred, two);
//hundred的值拷贝给变量x，two的值拷贝给变量y
```



##### 纯函数

对于一个函数，给定一个输入，返回一个唯一的输出。除此之外，不会对外部环境产生任何附带影响。我们机会称该函数为纯函数。所有函数内部定义的变量在函数返回之后都被垃圾回收掉。

如果函数的输入是对象(Array, Function, Object)，那么传入的是一个引用。对该变量的操作将会影响到原本的对象。

```js
//函数中的改动会影响对象
function changeAgeImpure(person) {
    person.age = 25;
    return person;
}
var alex = {
    name: 'Alex',
    age: 30
};
var changedAlex = changeAgeImpure(alex);
console.log(alex); // { name: 'Alex', age: 25 }
console.log(changedAlex); // { name: 'Alex', age: 25 }


//创建一个新对象
function changeAgePure(person) {
    //创建
    var newPersonObj = JSON.parse(JSON.stringify(person));
    newPersonObj.age = 25;
    return newPersonObj;
}
var alex = {
    name: 'Alex',
    age: 30
};
var alexChanged = changeAgePure(alex);
console.log(alex); // { name: 'Alex', age: 30 }
console.log(alexChanged); // { name: 'Alex', age: 25 }
```



```js
function changeAgeAndReference(person) {
    person.age = 25;
    person = {
        name: 'John',
        age: 50
    };
    
    return person;
}
var personObj1 = {
    name: 'Alex',
    age: 30
};
var personObj2 = changeAgeAndReference(personObj1);
console.log(personObj1); //{name: "Alex", age: 25}
console.log(personObj2); //{name: "John", age: 50}



```



### 类型转换

number

```js
var iNum = 10;
alert(iNum.toString(2));	//转2进制 "1010"
alert(iNum.toString(8));	//转8进制 "12"
alert(iNum.toString(16));	//转16进制 "A"
```



```
parseInt(String)
parseFloat(String)

var iNum1 = parseInt("AF", 16);	//解析16进制 175
```



字符串隐式转换

```js
Boolean(undefined)//false
Boolean(0)//false
'1'+2 //12
Boolean('false')//true
Boolean('undefined')//true
3 + true; // 4
```



JavaScript 中會被轉為 false 的值

```

"" 空字串
0, -0, NaN
null
undefined
false

```



### 比较

准确比较

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



比较对象

`instanceof` 检测左侧的 `__proto__` 原型链上，是否存在右侧的 `prototype` 原型



1. 所有的构造器的 `constructor` 都指向 `Function`
2. `Function` 的 `prototype` 指向一个特殊匿名函数，而这个特殊匿名函数的 `__proto__` 指向 `Object.prototype`



```js
//判断是否是指定对象
let person = function () {
}
let nicole = new person()
nicole instanceof person // true
```



```js
//判断是否是指定对象的子类
let person = function () {
}
let programmer = function () {
}
programmer.prototype = new person()
let nicole = new programmer()
nicole instanceof person // true
nicole instanceof programmer // true
```



==和===的异同点：

1. 比较双方都是对象时，只有指向同一个对象才会相等(包含==/===)。
2. ===要求比较双方类型相同并且值相等。
3. ==在比较双方类型不同的时候通常会进行隐式类型转换。



双等号将执行类型转换; 三等号将进行相同的比较，而不进行类型转换 (如果类型不同, 只是总会返回 false )



### this

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

`apply()`是函数的一个方法，作用是改变函数的调用对象

`apply()`的参数为空时，默认调用全局对象

```js
var x = 0;
var obj = {
    x: 1,
    y: function foo() {
        console.log(this.x);
    }
};

//apply使this默认指向全局对象
obj.y.apply(); // 0
//apply使this指向obj对象
obj.y.apply(obj); //1
```



#### call

call是每个函数都有的一个方法，在调用函数时为函数指定上下文



#### bind

`.bind` 和 `.call` 完全相同，除了不会立刻调用函数，而是返回一个能以后调用的新函数

```js
function greet() {
    console.log(`Hello, my name is ${this.name}`);
}

const user = {
    name: "Tyler",
    age: 27
};

const newFn = greet.bind(user);
newFn();
```



### scope

定义：作用域是变量与函数的可访问范围

作用：作用域控制变量与函数的可见性和生命周期

ECMAScript6之前只有全局作用域和函数作用域

Scope Chain（作用域链）：Javascript中一切皆对象，这些对象有一个`[[Scope]]`属性，该属性包含了函数被创建的作用域中对象的集合，它决定了哪些数据能被函数访问



- Lexical Scope（词法作用域，静态作用域），函数的作用域在函数定义的时确定
- Dynamic Scope（动态作用域），函数的作用域在函数调用的时候才确定



### closure

> 参考
>
> https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures

闭包：有权访问另一个函数作用域中的变量的函数

通常，函数的作用域及其所有变量都会在函数执行结束后被销毁。但是，在创建了一个闭包以后，这个函数的作用域就会一直保存到闭包不存在为止。

应用：设计私有的方法和变量。

缺点：减慢处理速度，内存消耗





