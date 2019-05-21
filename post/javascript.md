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



### MVVM

In the JQuery period, if you need to refresh the UI, you need to get the corresponding DOM and then update the UI, so the data and business logic are strongly-coupled with the page.

 

In MVVM, the UI is driven by data. Once the data is changed, the corresponding UI will be refreshed. If the UI changes, the corresponding data will also be changed. In this way, we can only care about the data flow in business processing without dealing with the page directly. ViewModel only cares about the processing of data and business and does not care how View handles data. In this case, we can separate the View from the Model. If either party changes, it does not necessarily need to change the other party, and some reusable logic can be placed in a ViewModel, allowing multiple Views to reuse this ViewModel.

 

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



### Prototype

> https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain
>



When it comes to inheritance, JavaScript only has one construct:  objects. Each object has a private property which holds a link to  another object called its **prototype**. That prototype object has a prototype of its own, and so on until an object is reached with `null` as its prototype. By definition, `null` has no prototype, and acts as the final link in this **prototype chain**.



<img src="https://www.ibm.com/developerworks/cn/web/1306_jiangjj_jsinstanceof/figure1.jpg" />



<img src="https://img-blog.csdn.net/20170503152146141?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvU3BpY3lCb2lsZWRGaXNo/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" />



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


