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



**Lifecycle hook**

**OnChanges**: Fires any time the input bindings have changed. It will give you an object (SimpleChange) that includes the current and previous values so you can inspect what’s changed. This is most useful to read changes in binding values.

**OnInit**: This runs once after the component has fully initialized (though not necessarily when all child components are ready), which is after the frst OnChanges hook. This is the best place to do any initialization code, such as loading data from APIs.

**OnDestroy**: Before a component is completely removed, the OnDestroy hook allows you to run some logic. This is most useful if you need to stop listening for incoming data or clear a timer.

**DoCheck**: Any time that change detection runs to determine whether the application needs to be updated, the DoCheck lifecycle hook lets you implement your own type of change detection.

**AfterContentInit**: When any content children have been fully initialized, this hook will allow you to do any initial work necessary to fnish setting up the content children components, such as if you need to verify whether content passed in was valid or not.

**AfterContentChecked**: Every time that Angular checks the content children, this can run so you can implement additional change detection logic.

**AfterViewInit**: This hook lets you run logic after all View Children have been initially rendered. This lets you know when the whole component tree has fully initialized and can be manipulated. 

**AfterViewChecked**: When Angular checks the component view and any View Children have been checked, you can implement additional logic to determine whether changes occurred. 



**Four roles of components**

- App component—This is the root app component, and you only get one of these per application.
- Display component—This is a stateless component that reﬂects the values passed into it, making it highly reusable.
- Data component—This is a component that helps get data into the application by loading it from external sources.
- Route component—When using the router, each route will render a component, and this makes the component intrinsically linked to the route.



**View Child to reference components**

In order to access a child component’s controller inside a parent controller, we can leverage ViewChild to inject that controller into our component. This gives us a direct reference to the child controller so we can implement a call to the Dashboard component from the App component controller.

ViewChild is a decorator for a controller property, like Inject or Output, which tells Angular to fll in that property with a reference to a specifc child component controller. It’s limited to injecting only children, so if you try to inject a component that isn’t a direct descendent, it will provide you with an undefned value. 



**These are ways to add styles that are specifc to a single component**

- Inline CSS—Component templates can have inline CSS or style attributes to set the styles of the elements. These are the default ways to add style rules to HTML elements regardless of whether you’re using Angular.
- Component-linked CSS—Using the component’s styleUrls property with links to external CSS fles. Angular will load the CSS fle and inject the rules inside a style element for your app.
- Component inline CSS—Using the component’s styles property with an array of CSS rules, Angular will inject the rules inside a style element for your app. 



**CSS Priority**

1 Inline style attribute rules
2 Inline style block rules in the template
3 Component styles rules or styleUrls rules (if both, then last declared has
priority)
4 Global CSS rules 



Enter the **Shadow DOM**, the offcial native browser standard for encapsulating styles.
Shadow DOM provides us with a good set of features to ensure that our styles don’t conﬂict and bleed in or out of a component, except that it might not be available on older
browsers.



**routerLink**

In order to facilitate navigating around, links must know which URL to go to, and typically href is the attribute for an anchor tag that gives the browser that information. When you use href with links, the browser will request a new URL from the server, which isn’t what we want. With Angular, routerLink is the attribute directive that denotes the expected route to navigate to and allows the Angular router to handle the actual navigation. In short, if you use href to link to a page, it will trigger a page load from the server, even if it’s a valid Angular route, and that’s much slower than using the router. This is a primary tenet of client-side routing. 



**structural and attribute directives**

Therefore, the primary difference between structural and attribute directives is that a structural directive is designed to modify the DOM tree of an element, whereas an attribute directive is designed to only modify the properties or DOM of a single element. 



```
//attribute directive 
[cardType]="stock.change"
//structural directives
*delay="i * 100"
```



**directives and pipes**

- Directives come in three ﬂavors: attribute, structural, and components.
- Attribute directives are the most common to create and are great for modifying an existing element.
- Structural directives are less common and are meant to be used to modify the existence or structure of DOM elements.
- Pure pipes are the most useful and allow you to transform a value before output using a pure function.
- Impure pipes allow you to maintain state inside of a pipe, but they’re run with every change detection check and are to be avoided if possible. 



**Template-driven forms**

The primary goal of a form is to be able to synchronize the data in the view with data in the controller so it can be submitted to be handled. Secondary goals are to perform tasks like validation, notify about errors, and handle other events like cancel. 

















