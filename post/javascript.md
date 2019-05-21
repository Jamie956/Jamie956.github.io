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



### Closures

- Closures are one of the most powerful features of JavaScript. JavaScript allows for the nesting of functions and grants the inner function full  access to all the variables and functions defined inside the outer  function (and all other variables and functions that the outer function  has access to). However, the outer function does not have access to the  variables and functions defined inside the inner function. This provides a sort of encapsulation for the variables of the inner function. Also,  since the inner function has access to the scope of the outer function,  the variables and functions defined in the outer function will live  longer than the duration of the outer function execution, if the inner  function manages to survive beyond the life of the outer function. A  closure is created when the inner function is somehow made available to  any scope outside the outer function.

