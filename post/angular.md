## Angular in Action



## 1, Angular: a modern web platform



### 1.4 Angular: a platform, not a framework

Angular comes with a leaner core library and makes additional features available as separate packages that can be used as needed. It also has many tools that push it beyond a simple framework, including the following:

- Dedicated CLI for application development, testing, and deployment
- Ofﬂine rendering capabilities on many back-end server platforms
- Desktop-, mobile-, and browser-based application execution environments
- Comprehensive UI component libraries, such as Material Design



**Angular CLI**

The CLI has a number of features that aid in the development of Angular apps. Here are the primary features:

- Generates new project scaffolding—Instead of having to create a new project from an existing project or creating all the files yourself, the CLI will generate a full project with a basic app already started for you.
- Generates new application pieces—Need a new component? Easy; it can generate the files for you. It can generate components, services, routes, and pipes, and it also will automatically ensure they are fully wired up in the build process.
- Manages the entire build toolchain—Because files need to be processed before being served to the client (such as TypeScript compilation), the CLI will process your source files and build them into an optimized version for development or production.
- Serves a localhost development server—The CLI handles the build ﬂow and then starts a server listening on localhost so you can see the results, with a live reload feature. 
- Incorporates code linting and formatting code—Helps enforce quality code by using the CLI to lint your code for style and semantic errors, and it can also help format your code automatically to the style rules.
- Supports running unit and e2e tests—Tests are vital, so the CLI sets up Karma for running your unit tests and works with Protractor to execute your e2e tests. It will automatically pick up and execute new tests as they’re generated. 



**Server rendering and the compiler**

A few primary use cases: 

- Server rendering for faster loading—Mobile devices are the primary way to access the internet these days, and mobile connections are frequently slow and unreliable. A server-side rendering option allows you to resolve data bindings and render components on the server so the initial payload sent to the user is pre-initialized. It can also optimize and send the necessary bytes for a quick initial load time and lazy load the other assets as needed.
- Performance in the browser—One of the major pain points of JavaScript is that it’s single threaded, which means that JavaScript can only handle one instruction at a time. In modern browsers, a newer technology known as web workers allows Angular to push some of the execution of the compiler into another process. This means that a lot more processing can occur, and it allows things like animations and user interactions to be smoother. 
- SEO—There’s a major concern about how heavy JavaScript applications are crawled by search engines. Universal rendering means we can detect crawlers and render the site for them so that content is ready without having to worry if the crawler executes JavaScript (some do, some don’t). This will certainly enhance SEO efforts for Angular applications.
- Multiple platforms—Many developers want to use other platforms for their back ends, such as .NET or PHP. Angular can be compiled in the platform of choice, assuming there’s a supported renderer. Angular will provide support for NodeJS, but the community is actively building and maintaining rendering support for other platforms such as Java and Go. 




### 1.5 Component architecture

A component is a way to create custom HTML elements in your application. 



**Components' key characteristics**

Components have some concepts that drive their design and architecture:

- Encapsulation—Keeping component logic in a single place
- Isolation—Keeping component internals hidden from external actors
- Reusability—Allowing component reuse with minimal effort
- Evented—Emitting events during the lifecycle of the component
- Customizable—Making it possible to style and extend the component
- Declarative—Using a component with simple declarative markup 



Several standards are required in order to implement the full vision of web components: 

- Custom elements (encapsulation, declarative, reusability, evented)
- Shadow DOM (isolation, encapsulation, customizable)
- Templates (encapsulation, isolation)
- JavaScript modules (encapsulation, isolation, reusability) 



**Custom Elements**

HTML is the language of the web because it describes the content of a page in a fairly concise set of elements. 



The offcial specification for custom elements is intended to allow developers to create new HTML elements that essentially blend naturally and natively into the DOM. 



Every Angular component is a custom element and fulflls the four tenets (and more) that we expect to get from a custom element. 



**Shadow DOM**

The Shadow DOM is an isolated Document Object Model (DOM) tree that’s detached from the typical CSS inheritance, allowing you to create a barrier between markup inside and outside of the Shadow DOM. For example, if you have a button inside of a Shadow DOM and a button outside, any CSS for the button written outside the Shadow DOM won’t affect the button inside it. 



The CSS selector is the only way to limit which elements receive that particular styling. 



https://github.com/Jamie956/javascript/blob/master/shadow-dom.html



Shadow DOM enables the best form of encapsulation available in the browser for styles and templates. 



**Templates**

Templates are often used with the Shadow DOM because it allows you to define the template and then inject it into the shadow root. Without templates, the Shadow DOM APIs would require us to inject content node by node. They’re also used by Angular as part of the lifecycle of components and the compilation process, allowing Angular to keep isolated, inert copies of the template as data changes and needs to be recompiled. 



**JavaScript modules**

Previously, developers had to build a bundle of all the assets for the web application ahead of time and deliver the whole package to the user. 



### 1.6 Modern JavaScript and Angular

- Classes
- Decorators
- Modules
- Template literals 



**Observables**

observables are a newer pattern for JavaScript applications to manage asynchronous activities.  

Promises have a major limitation in that they’re only useful for one call cycle. 

To use observables, you subscribe to the stream of data and pass a function that will run every time there’s a new piece of data.  



### 1.7 TypeScript and Angular

The basic value proposition of TypeScript is it can force restrictions on what types of values variables hold. 



TypeScript can help catch many simple syntax errors before they affect your application. 



Here are the primary reasons to use TypeScript:

- Adds clarity to your code—Variables that have types are easier to understand, because other developers (or yourself in six months) don’t have to think very hard about what the variable should be. 
- Enables a smarter editor—When you use TypeScript with a supported editor, you’ll get automatic IntelliSense support for your code. As you write, the editor can suggest known variables or functions and tell you what type of value it expects.
- Catches errors before you run code—TypeScript will catch syntax errors before you run the code in the browser, helping to reduce the feedback loop when you write invalid code.
- Entirely optional—You can use types when you want, and optionally leave them out where it doesn’t matter. 



**Summary**

- Angular is a platform, with many key elements such as tooling, UI libraries, and testing built in or easily incorporated into your application projects.
- Applications are essentially combinations of components. These components build upon the core principles of encapsulation, isolation, and reusability, which should have events, be customizable, and be declarative.
- ES6 and TypeScript provide a lot of the underpinnings for Angular’s architecture and syntax, making it a powerful framework without having to build a lot of custom language capabilities. 



## 2, Building your first Angular app



- Bootstrapping the app—To start the app, we’ll use the bootstrap feature to kick things off once they’re loaded. This happens once during the app lifecycle, and we’ll bootstrap the App component.
- Creating components—Angular is all about components, and we’ll create several components for different purposes. We’ll learn about how they’re built and how they nest to create complex applications.
- Creating services and using HttpClient—For code reuse, we’ll encapsulate some logic that helps manage the list of stocks into a service and also uses the HttpClient service from Angular to load stock quote data.
- Using pipes and directives in templates—Using pipes, we can transform data from one format into another during display, such as formatting a timestamp into a local date format. Directives are useful tools to modify the behavior of DOM elements inside a template, such as the ability to repeat pieces or conditionally show elements.
- Setting up routing—Most applications need the ability to allow users to navigate around the application, and by using the router we can see how to route between different components. 



### 2.4 How Angular renders the base application

**App component**

The @Component annotation declares that this class is a component by accepting an object. It has a selector property that declares the HTML selector of the component.
That means the component is used in the template by adding an HTML tag `<app-root> </app-root>`. 

The templateUrl property declares a link to a template containing an HTML template. Likewise, the styleUrls property contains an array of links to any CSS files that should be loaded for this component.

{{title}}, This is called interpolation and is frequently used to display data in a template. 



**App module**

The App module is the packaging that helps tell Angular what’s available to render. 

The NgModule decorator takes an object with a few different properties. 

The declarations property is to provide a list of any components and directives to make available to the entire application. 

The imports property is an array of other modules upon which this module depends 

The providers property, which is empty by default. Any services that are created are to be listed here 

The bootstrap property defines which components to bootstrap at runtime. 



**Bootstrapping the app**

The application must be bootstrapped at runtime to start the process of rendering. 



The role of main.ts is to bootstrap the Angular application.



The platformBrowserDynamic object is used to tell Angular which module is being loading 

It can take a moment for all the assets to load and initialize before the component renders. this is known as Just in Time compilation (JiT) 



### 2.5 Building services

Services are objects that abstract some common logic that you plan to reuse in multiple places.  

Another way to think of services is as sharable objects that any part of your app can import as needed. 

The intention of a service is to enable reuse of code. A service might be a set of common methods that need to be shared. You could have various “helper methods” that you don’t want to write over and over, such as utilities to parse data formats or authentication logic that needs to be run in multiple places. 



### 2.6 Creating your first component

Directives allow you to modify the behavior and display of DOM elements in a template.  

Directives make it possible to add some conditional logic or otherwise modify the way the template behaves or is rendered. 

The NgClass directive is able to add or remove CSS classes to and from the element. 

The safe navigation operator ?. will silently fail and not display anything if the property is missing. 

Pipes, which are added directly into the expression to format the output.  

Pipes only modify the data before it is displayed, and do not change the value in the controller. 

Input annotation. This indicates that this property is to be provided to the component by a parent component passing it to the summary.  

The host selector is a way to specify that you want the styles to apply to the element that hosts the element 



### 2.7 Components that use components and services

The constructor method runs as soon as the component is created.  

Don’t load data from the service in the constructor 

Components expose a number of lifecycle hooks that allow you to execute commands at various stages of rendering, giving you greater control over when things occur. 



### 2.8 Components with forms and events

This doesn’t require the OnInit lifecycle hook, because it’s a synchronous request to get data that exists in memory. 

(submit)="add()" is an event binding. 

[(ngModel)]="stock" attribute is a two-way binding that will sync the value of the input and the value of the property in the controller anytime it changes from either location. 



### 2.9 Application routing

Routing, configures the different pages that the application can render 

The router works by declaring an outlet in the template, which is the place in the template that the final rendered component will be displayed.  



**Summary**

- Angular apps are components that contain a tree of components. The root app is bootstrapped on page load to initialize the application.
- A component is an ES6 class with an @Component annotation that adds metadata to the class for Angular to properly render it. 
- Directives are attributes that modify the template in some way, such as NgIf, which conditionally shows or hides the DOM element based on the value of an expression. 

- Routing in Angular is based around paths mapping to a component. Routes will render a single component, and that component will also be able to render any additional components it needs. 



## 3, App essentials



### 3.1 Entities in Angular

These different entities have specific roles and capabilities, and you’ll be using them in various combinations to create your application. Here is a quick overview of the types: 

- Modules—Objects that help you to organize dependencies into discrete units
- Components—New elements that will compose the majority of your application’s structure and logic
- Directives—Objects that modify elements to give them new capabilities or change behaviors
- Pipes—Functions that format data before it’s rendered
- Services—Reusable objects that fll niche roles such as data access or helper utilities 



**Modules**

Modules are buckets for storing related entities for easy reuse and distribution. 



JavaScript modules are language constructs and are a way to separate code into different files that can be loaded as needed. 



Angular modules are logical constructs used for organizing similar groups of entities 



The @NgModule decorator contains the metadata for the App module, and the empty class acts as the vessel for storing the data. 



The declarations array contains a list of all components and directives that the application’s main module wants to make available to the entire application. 



The providers array contains a list of all of the services that you want to make available to the whole application. 



The imports array contains a list of the other modules that this module depends upon. 



To start rendering, Angular also needs to know what component(s) to render on the screen, and it looks at the bootstrap array for this list. 



**Components**

A component is an encapsulated element that maintains its own internal logic for how it desires to render some output 



Here are the key principles of a component:

- Encapsulation—Keep component logic isolated
- Isolation—Keep component internals hidden
- Reusability—Allow component reuse with minimal effort
- Event-based—Emit events during the lifecycle of the component
- Customizable—Possible to style and extend the component
- Declarative—Component used with simple declarative markup 



The Dashboard component holds the data for all the stocks and binds that information into the individual Summary components. Each Summary component uses the provided stock data to display itself. Any changes in the dashboard data will cause the child Summary components to be updated. 



**Directives**

Angular favors putting logic and capabilities straight into the HTML markup of the application 

NgIf gives an element the ability to conditionally render or be removed 

There are three categories of directives: attribute directives, structural directives, and components. 

Attribute directives (NgClass) modify the appearance or behavior of an element. 

Structural directives (NgIf , NgFor) modify the DOM tree based on some conditions.  



The primary default directives provided by Angular consist of the following:

- NgClass—Conditionally apply a class to an element
- NgStyle—Conditionally apply a set of styles to an element
- NgIf—Conditionally insert or remove an element from the DOM
- NgFor—Iterate over a collection of items
- NgSwitch—Conditionally display an item from a set of options 



**Pipes**

Using pipes, we can transform the data in the view during rendering without changing the underlying data value. 



**Services**

Reuse functional pieces of JavaScript logic across your application. 



### 3.2 How Angular begins to render an app



<img src="../img/How Angular renders the base app into the browser.png" />



Immediately upon loading the page, the bootstrapper is called to begin Angular execution. 



It loads your App module and reads through any additional dependencies that need to be loaded and bootstrapped. 



Renders the App component, which is the root element of your application. 



Any child components are also rendered as part of the component tree.  

It will also resolve bindings and set up event listeners for anything that declares it. 



### 3.3 Types of compilers

Angular provides two types of compilers, called the Just-in-Time (JiT) compiler and the Ahead-of-Time (AoT) compiler 



With JiT compilation, it means that the compiling of the application happens in the browser only after the assets have all been loaded. 



AoT, is a way to render the content before sending it to the browser.  



### 3.4 Dependency injection

Dependency injection (DI) is a pattern for obtaining objects that uses a registry to maintain a list of available objects and a service that allows you to request the object you need. Rather than having to pass around objects, you can ask for what you need when you need it. 



Injector. This is the service that Angular provides for requesting and registering dependencies 

Providers are responsible for creating the instanceof the object requested. 



### 3.5 Change detection

Change detection is the mechanism for keeping data and the rendered views in sync with one another. Changes always come down from the model into the view, and Angular employs a unidirectional propagation of changes from parents down to children. This helps ensure that if a parent changes, any children are also checked, due to potential linked data. 



JavaScript has no guaranteed way to notify about any change to an object, so Angular runs this process instead. 



When a value change is detected in a component, it will update the component and potentially any child components as well.  



Angular has two ways for changes to be triggered. The Default mode will traverse the entire tree looking for changes with each change detection process. The OnPush mode tells Angular that the component only cares about changes to any values that are input into the component from its parent, and gives Angular the ability to skip checking the component during change detection if it already knows the parent hasn’t changed. 



Change detection is triggered by either events, receiving HTTP responses, or timers intervals. 



Change detection is the mechanism that allows components to be updated when data changes in a parent component, and ensure views and data are in sync. 



### 3.6 Template expressions and bingdings

Angular allows the placement of logic and customization directly into the template 



Template concepts:

- Interpolation—Displaying content in the page
- Attribute and property bindings—Linking data from the component controller into attributes or properties of other elements
- Event bindings—Adding event listeners to elements
- Directives—Modifying the behavior or adding additional structure to elements
- Pipes—Formatting data before it’s displayed on the page 



Template expressions are used in three places: for interpolation, property bindings, and event bindings. 



Bindings are the conduit for data or methods to be used from a controller in the template; they allow data in the controller to ﬂow into the template, or events to call from the template back into the controller. 



**Property bindings**

property bindings, which allow you to bind values to properties of an element to modify their behavior or appearance. 

As with interpolation, the bindings are evaluated in the component context, so the binding will reference properties of the controller. 

interpolation is a shortcut for a property binding to the textContent property of an element.  



Using the [] syntax binds to an element’s property, not the attribute. This is an important distinction, because properties are the DOM element’s property. That makes it possible to use any valid HTML element property (such as the img src property). Instead of binding the data to the attribute, you’re binding data directly to the element property, which is quite efficient 



**Summary** 

- An Angular application is a tree of components, and there is always a root application component.
- The various entity types (modules, components, directives, pipes, services) each have a specific role and purpose.
- Angular has two types of compilers, Ahead-of-Time (AoT) and Just-in-Time (JiT), to give you different ways to render the application.
- Dependency injection is fundamental for Angular to track all the objects in the application and make them available when they’re requested.
- Change detection keeps the components in sync with the model data as asynchronous changes occur from user input or other events.
- Templates contain several types of bindings: interpolation for displaying data, property bindings for modifying the element’s properties, attribute bindings for modifying non-property values of an element, and event bindings for handling events. 



4, Component basics

5, Advanced components

6, Services

7, Routing

8, Building custom directives and pipes

9, Forms

10, Testing your application

11, Angular in production












