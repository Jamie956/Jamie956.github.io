### Angular

An Angular application is a tree of components, and there is always a root application component.



### Platform

- Angular comes with a leaner core library and makes additional features available as separate packages that can be used as needed. It also has many tools that push it beyond a simple framework, including the following:
  - Dedicated CLI for application development, testing, and deployment
  - Ofﬂine rendering capabilities on many back-end server platforms
  - Desktop-, mobile-, and browser-based application execution environments
  - Comprehensive UI component libraries, such as Material Design
- Angular is a platform, with many key elements such as tooling, UI libraries, and testing built in or easily incorporated into your application projects.



### Angular CLI

- Generates new project scaffolding
- Generates new application pieces—It can generate components, services, routes, and pipes, and it also will automatically ensure they are fully wired up in the build process.
- Manages the entire build toolchain—The CLI will process your source files and build them into an optimized version for development or production.
- Serves a localhost development server
- Incorporates code linting and formatting code
- Supports running unit and e2e tests



### Server rendering

- Server rendering for faster loading—resolve data bindings and render components on the server so the initial payload sent to the user is pre-initialized. It can also optimize and send the necessary bytes for a quick initial load time and lazy load the other assets as needed.
- Performance in the browser—One of the major pain points of JavaScript is that it’s single threaded, which means that JavaScript can only handle one instruction at a time. In modern browsers, a newer technology known as web workers allows Angular to push some of the execution of the compiler into another process. This means that a lot more processing can occur, and it allows things like animations and user interactions to be smoother. 
- SEO—There’s a major concern about how heavy JavaScript applications are crawled by search engines. Universal rendering means we can detect crawlers and render the site for them so that content is ready without having to worry if the crawler executes JavaScript (some do, some don’t). This will certainly enhance SEO efforts for Angular applications.



### Component

- A component is a way to create custom HTML elements in your application.
- Applications are essentially combinations of components. These components build upon the core principles of encapsulation, isolation, and reusability, which should have events, be customizable, and be declarative.

- A component is an encapsulated element that maintains its own internal logic for how it desires to render some output

- Here are the key principles of a component:
  - Encapsulation—Keep component logic isolated
  - Isolation—Keep component internals hidden
  - Reusability—Allow component reuse with minimal effort
  - Event-based—Emit events during the lifecycle of the component
  - Customizable—Possible to style and extend the component
  - Declarative—Component used with simple declarative markup
- Here’s a list of the primary things that compose a component:
  - Component Metadata Decorator —All components must be annotated with the @Component() decorator to properly register the component with Angular. The metadata contains numerous properties to help modify the way the component behaves or is rendered.
  - Controller—The controller is the class that is decorated with @Component(), and it contains all the properties and methods for the component. Most of the logic exists in the controller.
  - Template—A component isn’t a component without a template. The markup for a component defines the layout and content of the UI that a user can see, and the rendered version of the template will look at the values from the controller to bind any 
- Here are the four roles of components 
  - App component—This is the root app component, and you only get one of these per application.
  - Display component—This is a stateless component that reﬂects the values passed into it, making it highly reusable.
  - Data component—This is a component that helps get data into the application by loading it from external sources.
  - Route component—When using the router, each route will render a component, and this makes the component intrinsically linked to the route. 



### Shadow DOM

- The Shadow DOM is an isolated Document Object Model (DOM) tree that’s detached from the typical CSS inheritance, allowing you to create a barrier between markup inside and outside of the Shadow DOM. For example, if you have a button inside of a Shadow DOM and a button outside, any CSS for the button written outside the Shadow DOM won’t affect the button inside it. 

  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
      <meta http-equiv="X-UA-Compatible" content="ie=edge" />
      <title>Document</title>
      <style>
        .lightdom {
          background-color: #333;
          color: #fff;
        }
      </style>
    </head>
    <body>
      <div class="lightdom">
        I'm normal card.
      </div>
    </body>
  
    <script>
      const div = document.createElement("div");
      const shadowRoot = div.attachShadow({ mode: 'open' });
      shadowRoot.innerHTML = `
      <div class="lightdom">
        I'm in the Shadow DOM.
      </div>`;
      document.body.appendChild(div);
    </script>
  </html>
  ```

- The CSS selector is the only way to limit which elements receive that particular styling. 

- Shadow DOM enables the best form of encapsulation available in the browser for styles and templates. 



### Templates

- Templates are often used with the Shadow DOM because it allows you to define the template and then inject it into the shadow root. Without templates, the Shadow DOM APIs would require us to inject content node by node. They’re also used by Angular as part of the lifecycle of components and the compilation process, allowing Angular to keep isolated, inert copies of the template as data changes and needs to be recompiled. 

- Angular allows the placement of logic and customization directly into the template 
- Template concepts:
  - Interpolation—Displaying content in the page
  - Attribute and property bindings—Linking data from the component controller into attributes or properties of other elements
  - Event bindings—Adding event listeners to elements
  - Directives—Modifying the behavior or adding additional structure to elements
  - Pipes—Formatting data before it’s displayed on the page 
- Template expressions are used in three places: for interpolation, property bindings, and event bindings. 
- Bindings are the conduit for data or methods to be used from a controller in the template; they allow data in the controller to ﬂow into the template, or events to call from the template back into the controller. 



### Lifecycle

- The App module is the packaging that helps tell Angular what’s available to render. 
- The NgModule decorator takes an object with a few different properties. 
- The declarations property is to provide a list of any components and directives to make available to the entire application. 
- The imports property is an array of other modules upon which this module depends 
- The providers property, which is empty by default. Any services that are created are to be listed here 
- The bootstrap property defines which components to bootstrap at runtime.

- The constructor method runs as soon as the component is created.

- Don’t load data from the service in the constructor

- Components expose a number of lifecycle hooks that allow you to execute commands at various stages of rendering, giving you greater control over when things occur.

- Angular apps are components that contain a tree of components. The root app is bootstrapped on page load to initialize the application.

- <img src="../img/How Angular renders the base app into the browser.png" />

- Immediately upon loading the page, the bootstrapper is called to begin Angular execution.

- It loads your App module and reads through any additional dependencies that need to be loaded and bootstrapped.

- Renders the App component, which is the root element of your application.

- Any child components are also rendered as part of the component tree.

- It will also resolve bindings and set up event listeners for anything that declares it.

- Component lifecycle while the application is running

  ![component behavior](..\img\Component lifecycle.png)



### Service

- Creating services and using HttpClient—For code reuse, we’ll encapsulate some logic that helps manage the list of stocks into a service and also uses the HttpClient service from Angular to load stock quote data.
- Services are objects that abstract some common logic that you plan to reuse in multiple places.  
- Another way to think of services is as sharable objects that any part of your app can import as needed. 
- The intention of a service is to enable reuse of code. A service might be a set of common methods that need to be shared. You could have various “helper methods” that you don’t want to write over and over, such as utilities to parse data formats or authentication logic that needs to be run in multiple places.
- Reuse functional pieces of JavaScript logic across your application.



### Routing

- Most applications need the ability to allow users to navigate around the application, and by using the router we can see how to route between different components.

- The router works by declaring an outlet in the template, which is the place in the template that the final rendered component will be displayed.
- Routing in Angular is based around paths mapping to a component. Routes will render a single component, and that component will also be able to render any additional components it needs.



### Template

- Directives allow you to modify the behavior and display of DOM elements in a template.  
- Directives make it possible to add some conditional logic or otherwise modify the way the template behaves or is rendered. 
- The NgClass directive is able to add or remove CSS classes to and from the element. 
- The safe navigation operator ?. will silently fail and not display anything if the property is missing. 
- Pipes, which are added directly into the expression to format the output.  
- Pipes only modify the data before it is displayed, and do not change the value in the controller. 
- Input annotation. This indicates that this property is to be provided to the component by a parent component passing it to the summary.  
- The host selector is a way to specify that you want the styles to apply to the element that hosts the element
- Directives are attributes that modify the template in some way, such as NgIf, which conditionally shows or hides the DOM element based on the value of an expression.
- Templates contain several types of bindings:
  - interpolation for displaying data
  - property bindings for modifying the element’s properties
  - attribute bindings for modifying non-property values of an element
  - event bindings for handling events



### Entities

- Modules—Objects that help you to organize dependencies into discrete units
- Components—New elements that will compose the majority of your application’s structure and logic
- Directives—Objects that modify elements to give them new capabilities or change behaviors
- Pipes—Functions that format data before it’s rendered
- Services—Reusable objects that fill niche roles such as data access or helper utilities 



### Modules

- The declarations array contains a list of all components and directives that the application’s main module wants to make available to the entire application. 

- The providers array contains a list of all of the services that you want to make available to the whole application. 

- The imports array contains a list of the other modules that this module depends upon. 

- To start rendering, Angular also needs to know what component(s) to render on the screen, and it looks at the bootstrap array for this list. 



### Directives

- Angular favors putting logic and capabilities straight into the HTML markup of the application 

- NgIf gives an element the ability to conditionally render or be removed 

- Attribute directives (NgClass) modify the appearance or behavior of an element. 

- Structural directives (NgIf , NgFor) modify the DOM tree based on some conditions.  

- The primary default directives provided by Angular consist of the following:
  - NgClass—Conditionally apply a class to an element
  - NgStyle—Conditionally apply a set of styles to an element
  - NgIf—Conditionally insert or remove an element from the DOM
  - NgFor—Iterate over a collection of items
  - NgSwitch—Conditionally display an item from a set of options 



### Pipes

- Using pipes, we can transform the data in the view during rendering without changing the underlying data value. 



### Compilers

- Just-in-Time (JiT): The compiling of the application happens in the browser only after the assets have all been loaded. 

- Ahead-of-Time (AoT): Render the content before sending it to the browser.  



### DI

- Dependency injection (DI) is a pattern for obtaining objects that uses a registry to maintain a list of available objects and a service that allows you to request the object you need. Rather than having to pass around objects, you can ask for what you need when you need it. 
- Injector: This is the service that Angular provides for requesting and registering dependencies 
- Providers are responsible for creating the instanceof the object requested.
- Dependency injection is fundamental for Angular to track all the objects in the application and make them available when they’re requested.



### Change detection

- Change detection is the mechanism for keeping data and the rendered views in sync with one another. Changes always come down from the model into the view, and Angular employs a unidirectional propagation of changes from parents down to children. This helps ensure that if a parent changes, any children are also checked, due to potential linked data. 

- When a value change is detected in a component, it will update the component and potentially any child components as well.  

- Angular has two ways for changes to be triggered.

  - The Default mode will traverse the entire tree looking for changes with each change detection process.
  - The OnPush mode tells Angular that the component only cares about changes to any values that are input into the component from its parent, and gives Angular the ability to skip checking the component during change detection if it already knows the parent hasn’t changed.

- Change detection is triggered by either events, receiving HTTP responses, or timers intervals.

- Change detection keeps the components in sync with the model data as asynchronous changes occur from user input or other events.

- Changes are always triggered by some asynchronous activity, such as when a user interacts with the page. When these changes occur, there is a chance (though no guarantee) that the application state or data has changed. Here are some examples: 

  - A user clicks a button to trigger a form submission (user activity).
  - An interval fires every x seconds to refresh data (intervals or timers).
  - Callbacks, observables, or promises are resolved (XHR requests, event streams). 

- Change detection starts at the top and goes down the tree by default, or with OnPush only goes down the tree with changed inputs. 

  ![Change detection](..\img\Change detection.png)



### Lifecycle hooks

- Lifecycle hooks aren’t like event listeners—they’re special methods with specifc names that are called during the component’s lifecycle if they’re defined. 
- Methods
  - OnChanges: Fires any time the input bindings have changed
  - OnInit: This runs once after the component has fully initialized
  - OnDestroy: Before a component is completely removed, the OnDestroy hook allows you to run some logic. 
  - DoCheck: Any time that change detection runs to determine whether the application needs to be updated 
  - AfterContentInit: When any content children have been fully initialized, this hook will allow you to do any initial work 
  - AfterContentChecked: Every time that Angular checks the content children 
  - AfterViewInit: This hook lets you run logic after all View Children have been initially rendered. 
  - AfterViewChecked: When Angular checks the component view and any View Children have been checked



## 5 Advanced components







### Communicating



**Output events and template variables**

```ts
import { Component, Output, EventEmitter } from '@angular/core';
@Component({
    selector: 'app-navbar',
    templateUrl: './navbar.component.html',
    styleUrls: ['./navbar.component.css']
})
export class NavbarComponent {
    @Output() onRefresh: EventEmitter<null> = new EventEmitter<null>();
    refresh() {
        this.onRefresh.emit();
    }
}
```



![output event](..\img\output event.png)



**View Child to reference components**

- ViewChild is a decorator for a controller property, like Inject or Output, which tells Angular to fll in that property with a reference to a specifc child component controller. It’s limited to injecting only children, so if you try to inject a component that isn’t a direct descendent, it will provide you with an undefined value. 

```ts
import { Component, ViewChild } from '@angular/core';
import { DashboardComponent } from './dashboard/dashboard.component';
@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css']
})
export class AppComponent {
    @ViewChild(DashboardComponent) dashboard: DashboardComponent;
    refresh() {
        this.dashboard.generateData();
    }
}
```

- If you ever need to do anything more than access properties or methods of a child controller, you probably need to use the ViewChild approach. This does cause a coupling between the two components, which should be avoided when possible. 



### Styling



**Adding styles to a component**

- Here we have a simple component that has styling applied from fve different approaches: 
  - Global CSS rules
  - Inline CSS style element
  - Inline style declaration
  - Component styles property
  - Component styleUrls property linked to a CSS fle 

**Encapsulation modes**

- One key capability is the ability to ensure that CSS styling for a component doesn’t bleed over into the rest of the app, which is called styling encapsulation.  

- Angular comes with three encapsulation modes for a view. 
  - None—No encapsulation is used during rendering of the view, and the component DOM is subject to the normal rules of CSS. Templates aren’t modifed when injected into the app except for the removal of any CSS style elements from the template to the document head.
  - Emulated—Emulated encapsulation is used to simulate styling encapsulation by adding unique CSS selectors to your CSS rules at runtime. CSS can cascade into the component easily from the global CSS rules.
  - Native—Uses the native Shadow DOM for styling and markup encapsulation and provides the best encapsulation. All styles are injected inside the shadow root and are therefore localized to the component. None of the templates or styles declared for the component are visible outside the component. 



**Dynamically rendering components**

- There are a few fairly common situations that you’ve seen where a dynamic component is often a good solution. For example 
  - Modals that display over the page with dynamic content
  - Alerts that conditionally display
  - Carousel or tabs that might dynamically expand the amount of content
  - Collapsible content that needs to be removed afterward 



## 6 Services

- Shared code across your application is almost always best placed inside of a service. In Angular, most of the time a service also is something that you can inject into your
  controllers using dependency injection, though there is no exact defnition of what makes an object a service. 
- Service roles
  - Injectable services are the typical Angular services that provide a feature for the
    application and work with Angular’s DI system to be injected into components. An example would be a service that handles how to load data from an API.
  - Non-injectable services are JavaScript objects that aren’t tied into Angular’s DI system and are just imported into the fle. This can be useful to make a service available outside of Angular’s DI, such as in the application’s main fle.
  - Helper services are services that make it easier to use a component or feature. An example would be a service to help manage the currently active alert on the page.
  - Data services are for sharing data across the application. An example is an object holding data for the logged-in user. 



**Dependency injection and injector trees**

- Why do we want to use DI instead of regular module loading in JavaScript? There are a few key reasons you’ll usually want to make your services injectable: 
  - It allows you to focus on consuming services and not worry about how to create them.
  - It resolves external dependencies for you, simplifying consumption.
  - It’s easier to test your Angular entities because you can always inject a mock service instead of the real thing.
  - It can allow you to control the injection of a service without having to worry about where another piece of the application might also inject the service.
  - It gives you a clean instance of the service for each injector tree. 
- DI is powerful and is vital for our applications to be easy to manage, but keeping up with the nuances can be a little challenging if you try to combine these capabilities all of the time. Here are a few tips to keep in mind when it comes to DI and services: 
  - Inject at the lowest level—Instead of adding everything to the App module providers array, try to add it to the lowest component providers array. This will minimize the “surface area” of your service to only be available to the components that might use it.
  - Name your services wisely—Give your services semantic and meaningful names. PostsService might be clear enough, or perhaps BlogPostsService, depending on the context. I fnd it’s easier to type a few more characters than it is to guess what a service named BPService might be, especially when multiple people are working on your application.
  - Keep services focused—Rather than creating one mega service that you inject everywhere with lots of abilities, make a sensible number of services that do specifc tasks. The longer your service is, the harder it is to maintain and test, and it will likely become tangled in your application.
  - Keep services meaningful—On the ﬂip side of keeping services focused, you’ll need to balance the utility of adding another service. Do you need a service for something that’s used only once? It may add more complexity for little beneft, so strike the right balance between number of services and their roles.
  - Use consistent patterns—Follow the same design for your services for consistency. For example, if you have several services that handle making REST API calls, you’d likely want to give them the same kinds of methods, like get to load an object, or save to store a record. 



**Using the HttpClient service**

- t’s considered best practice to never use HttpClient in the component controller directly (yet many articles and even the documentation may demonstrate this case), because this helps create a separation of concerns. That’s why we’ll create a new service that will use the HttpClient service and abstract some of the logic from the controller. 
- Often you’ll want to intercept HTTP requests or responses before they’re handled elsewhere in the application. 



**Helper services**

- As you build your application, you may see some code start to reappear in various places. That’s what all services help to reduce, but sometimes you’ll fnd that some
  code feels out of place and needs to be extracted into a service. When you do that, you’re creating a service that exposes helper functions to simplify your components.



**Summary**

- Services are best for taking on specifc tasks, such as data access and managing confguration.
- Services can be injected anywhere in the application as long as they’ve been registered with a provider.
- Angular provides an HttpClient service that helps manage making XHR requests, and you can wrap your own services around it to simplify data access.
- Services can hold values that get propagated, but you need to be aware of the injection tree to avoid getting different copies of the service.
- A class could be used like a service, without having to use dependency injection, but it should be limited to only situations that make sense.
- Angular exposes many entities in the API that you can use in your application, such as a Location service for managing URLs. 



## 7 Routing



**Route guards to limit access**

- Angular allows you to control the conditions that allow a route to render, which
  usually is done to prevent the application from going into a bad state. For example,
  you shouldn’t allow an unauthenticated user to view portions of the application that
  require them to be logged in or they will likely get a lot of errors. 

- A guard is like a lifecycle hook for route changes that allows an application to verify
  certain conditions or load data before a change occurs 

- Here are the fve types of guards and their basic roles: 

  - CanActivate—Used to determine whether the route can be activated (such as
    user validation)
  - CanActivateChild—Same as CanActivate, but specifcally for child routes
  - CanDeactivate—Used to determine whether the current route can be deactivated (such as preventing leaving an unsaved form without confrmation)
  - CanLoad—Used to determine whether the user can navigate to a lazy loaded
    module prior to loading it
  - Resolve—Used to access route data and pass data to the component’s list of
    providers 

- AuthGuard service for limiting access to routes 

  ```ts
  import { Injectable } from '@angular/core';
  import { CanActivate, Router, ActivatedRouteSnapshot, RouterStateSnapshot }
  from '@angular/router';
  import { UserService } from './user.service';
  @Injectable()
  export class AuthGuardService implements CanActivate {
      constructor(
      private userService: UserService,
       private router: Router) {}
      canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {
          if (!this.userService.isGuest()) {
              return true;
          } else {
              this.router.navigate(['/login'], {
                  queryParams: {
                      return: state.url
                  }
              });
              return false;
          }
      }
  }
  ```



**Lazy loading**

Feature modules can be lazy loaded into the application, giving you the ability to
reduce the fle size of the code that’s initially downloaded to users. It only loads
the module when the user navigates to a route that’s part of the feature module. 



## 8 Directives and Pipes

- The main reasons apply to both directives and pipes: 
  - Reuse and reduce—Instead of each component having to implement similar logic, it can be abstracted out and easily reused. This also reduces code footprint and helps standardize logic.
  - Maintainability and focused components—Components sometimes become a dumping ground for code and logic that are tangential to the component itself. Removing that from the component makes it easier to maintain your components.
  - Testability—Moving everything into smaller blocks means you can create smaller test cases and limit permutations. 



### Directives

- There are two types of directives: structural and attribute.  

- NgIf is a structural directive and is able to control whether the DOM element is rendered or not. On the other hand, the ngClass directive is an attribute directive and doesn’t have the * when it’s used, such as [ngClass]="{active: true}". 

- NgFor structural directive creates multiple instances of the Summary component,
  whereas the NgClass attribute modifes the background color of those same instances. 

  ![structural or attribute directive](D:\project\justnote\img\structural or attribute directive.png)

- Therefore, the primary difference between structural and attribute directives is that a
structural directive is designed to modify the DOM tree of an element, whereas an attribute directive is designed to only modify the properties or DOM of a single element. 



**Creating a structural directive**

- When the structural directive is rendered by Angular, it creates a placeholder space,
  called an embedded view, where the directive can decide what to insert inside of this new view container. 
- Data is passed through a pipe before being rendered in a template binding



### Pipes

- Pipes are essentially a way to format data

  ![pipe](D:\project\justnote\img\pipe.png)

- There are fundamentally two types of pipes: pure and impure. 

  - Pure pipes maintain no state information
  - Impure pipes maintain state

- How pure and impure pipes are handled by Angular

  ![pure and impure pipes](D:\project\justnote\img\pure and impure pipes.png)



**Pure pipe**

- Pure pipes are so named because they implement a pure function—a function that
  doesn’t maintain any internal state and returns the same output given the same input. 
- Pure functions are important for performance, because Angular doesn’t need to run
  them with each change detection lifecycle unless the input value has changed. This can
  save a reasonable amount of overhead for performance reasons. 



**Summary**

- Directives come in three ﬂavors: attribute, structural, and components.
- Attribute directives are the most common to create and are great for modifying
  an existing element.
- Structural directives are less common and are meant to be used to modify the
  existence or structure of DOM elements.
- Pure pipes are the most useful and allow you to transform a value before output
  using a pure function.
- Impure pipes allow you to maintain state inside of a pipe, but they’re run with
  every change detection check and are to be avoided if possible. 



## 9 Forms

Angular provides two approaches to building forms: reactive forms and template
forms. 



### Template-driven forms



**Custom validation with directives**

- Phone validator 

  ```js
  import { AbstractControl, ValidatorFn } from '@angular/forms';
  const expression = /((\(\d{3}\) ?)|(\d{3}-))?\d{3}-\d{4}/;
  export function PhoneValidator(): ValidatorFn {
      return (control: AbstractControl): { [key: string]: any } => {
          const valid = expression.test(control.value) && control.value.length <
                14;
          return valid ? null : { phone: true };
      };
  }
  ```



**Summary**

- Template-driven forms define the form using NgModel on form controls.
- You can apply normal HTML validation attributes, and NgModel will automatically
  try to validate based on those rules.
- Custom validation is possible through a custom validator function and directive,
  which gets registered with the built-in list of validators.
- The NgForm directive, though it can be transparent, exposes features to help
  manage submit events and overall form validation inspection.
- Reactive forms are different in that you define the form model in the controller
  and link form controls using FormControlName.
- You can observe the changes of a form control with reactive forms and run logic
  every time a new value is emitted.
- Reactive forms declare validation in the controller form defnition, and creating
  custom validations is easier because they don’t require a directive.
- Ultimately, both form patterns are available to you. I tend to use reactive forms,
  especially as the form gets more complex.
- Creating a new form control requires implementing the ControlValueAccessor
  methods and registering it with the controls provider. 



10 Testing your application

11 Angular in production