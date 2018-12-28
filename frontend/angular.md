### Angular in Action

**Communicating between Angular components**

Angular applications are based around components that communicate with one another. You pass data into a child component using inputs and can emit events up to a parent component that shares data.



**Angular: a platform, not a framework**

- Dedicated CLI for application development, testing, and deployment
- Ofﬂine rendering capabilities on many back-end server platforms
- Desktop-, mobile-, and browser-based application execution environments
6 chapter 1 Angular: a modern web platform
- Comprehensive UI component libraries, such as Material Design



**Server rendering for faster loading**—Mobile devices are the primary way to access the internet these days, and mobile connections are frequently slow and unreliable. A server-side rendering option allows you to resolve data bindings and render components on the server so the initial payload sent to the user is pre-initialized. It can also optimize and send the necessary bytes for a quick initial load time and lazy load the other assets as needed.



**Performance in the browser**—One of the major pain points of JavaScript is that it’s single threaded, which means that JavaScript can only handle one instruction at a time. In modern browsers, a newer technology known as web workers allows Angular to push some of the execution of the compiler into another process. This means that a lot more processing can occur, and it allows things like animations and user interactions to be smoother.



**SEO**—There’s a major concern about how heavy JavaScript applications are crawled by search engines. Universal rendering means we can detect crawlers and render the site for them so that content is ready without having to worry if the crawler executes JavaScript (some do, some don’t). This will certainly enhance SEO efforts for Angular applications.



**Multiple platforms**—Many developers want to use other platforms for their back ends, such as .NET or PHP. Angular can be compiled in the platform of choice, assuming there’s a supported renderer. Angular will provide support for NodeJS, but the community is actively building and maintaining rendering support for other platforms such as Java and Go.



**Components’ key characteristics**

- Encapsulation—Keeping component logic in a single place
- Isolation—Keeping component internals hidden from external actors
- Reusability—Allowing component reuse with minimal effort
- Evented—Emitting events during the lifecycle of the component
- Customizable—Making it possible to style and extend the component
- Declarative—Using a component with simple declarative markup 



**Shadow DOM**

Shadow DOM is really your best friend when it comes to trying to isolate styling behaviors inside of a component. The Shadow DOM is an isolated Document Object Model (DOM) tree that’s detached from the typical CSS inheritance, allowing you to create a barrier between markup inside and outside of the Shadow DOM. For example, if you have a button inside of a Shadow DOM and a button outside, any CSS for the button written outside the Shadow DOM won’t affect the button inside it.  



**Promises** are another construct to help deal with asynchronous calls, which are useful for making API requests, for example. Promises have a major limitation in that they’re only useful for one call cycle. For example, if you wanted to have a promise return a value on an event like a user click, that promise would resolve on the frst click. But you might be interested in handling every user click action. Normally, you’d use an event listener for this, and that allows you to handle events over time. This is an important distinction: Observables are like event handlers in that they continue to process data over time and allow you to continuously handle that stream of data. 







