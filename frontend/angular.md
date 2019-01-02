### Angular in Action

**Communicating between Angular components**

Angular applications are based around components that communicate with one another. You pass data into a child component using inputs and can emit events up to a parent component that shares data.



**Angular: a platform, not a framework**

- Dedicated CLI for application development, testing, and deployment
- Ofﬂine rendering capabilities on many back-end server platforms
- Desktop-, mobile-, and browser-based application execution environments
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



**Why TypeScript**
- Adds clarity to your code—Variables that have types are easier to understand, because other developers (or yourself in six months) don’t have to think very hard about what the variable should be.
- Enables a smarter editor—When you use TypeScript with a supported editor, you’ll get automatic IntelliSense support for your code. As you write, the editor can suggest known variables or functions and tell you what type of value it expects.
- Catches errors before you run code—TypeScript will catch syntax errors before you run the code in the browser, helping to reduce the feedback loop when you write invalid code.
- Entirely optional—You can use types when you want, and optionally leave them out where it doesn’t matter. 



**Features and capabilities with Angular** 

- Angular is a platform, with many key elements such as tooling, UI libraries, and
testing built in or easily incorporated into your application projects.
- Applications are essentially combinations of components. These components
build upon the core principles of encapsulation, isolation, and reusability, which
should have events, be customizable, and be declarative.
- ES6 and TypeScript provide a lot of the underpinnings for Angular’s architecture
and syntax, making it a powerful framework without having to build a lot of custom language capabilities. 



**Bootstrapping the app**—To start the app, we’ll use the bootstrap feature to kick things off once they’re loaded. This happens once during the app lifecycle, and we’ll bootstrap the App component.

**Creating components**—Angular is all about components, and we’ll create several components for different purposes. We’ll learn about how they’re built and how they nest to create complex applications.

**Creating services and using HttpClient**—For code reuse, we’ll encapsulate some logic that helps manage the list of stocks into a service and also uses the HttpClient service from Angular to load stock quote data.

**Using pipes and directives in templates**—Using pipes, we can transform data from one format into another during display, such as formatting a timestamp into a local date format. Directives are useful tools to modify the behavior of DOM elements inside a template, such as the ability to repeat pieces or conditionally show elements.

**Setting up routing**—Most applications need the ability to allow users to navigate around the application, and by using the router we can see how to route between different components. 



Angular requires at least one **component** and one **module**. A component is the basic building block of Angular applications and acts much like any other HTML element. A module is a way for Angular to organize different parts of the application into a single unit that Angular can understand. You might think of components as LEGO® bricks, which can be many different shapes, sizes, and colors, and modules would be the packaging the LEGOs come in. Components are for functionality and structure, whereas modules are for packaging and distribution. 



- **Angular apps** are components that contain a tree of components. The root app is bootstrapped on page load to initialize the application.
- A **component** is an ES6 class with an @Component annotation that adds metadata to the class for Angular to properly render it. 
- **Services** are also ES6 modules and should be designed for portability. Any ES6 class could be used, even if it isn’t specifcally meant for Angular.
- **Directives** are attributes that modify the template in some way, such as NgIf, which conditionally shows or hides the DOM element based on the value of an expression.
- Angular has built-in **form** support that includes the ability to automatically validate, group, and bind data with any form control, as well as use events.
- **Routing** in Angular is based around paths mapping to a component. Routes will render a single component, and that component will also be able to render any additional components it needs. 



**The primary default directives** 

- NgClass—Conditionally apply a class to an element
- NgStyle—Conditionally apply a set of styles to an element
- NgIf—Conditionally insert or remove an element from the DOM
- NgFor—Iterate over a collection of items
- NgSwitch—Conditionally display an item from a set of options



**Change detection** is the mechanism that allows components to be updated when data changes in a parent component, and ensure views and data are in sync.



**Templates concepts** 

- Interpolation—Displaying content in the page
- Attribute and property bindings—Linking data from the component controller into attributes or properties of other elements
- Event bindings—Adding event listeners to elements
- Directives—Modifying the behavior or adding additional structure to elements
- Pipes—Formatting data before it’s displayed on the page 

















