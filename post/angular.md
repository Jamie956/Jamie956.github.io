### Server rendering

- Server rendering for faster loading—resolve data bindings and render components on the server so the initial payload sent to the user is pre-initialized. It can also optimize and send the necessary bytes for a quick initial load time and lazy load the other assets as needed.
- Performance in the browser—One of the major pain points of JavaScript is that it’s single threaded, which means that JavaScript can only handle one instruction at a time. In modern browsers, a newer technology known as web workers allows Angular to push some of the execution of the compiler into another process. This means that a lot more processing can occur, and it allows things like animations and user interactions to be smoother. 
- SEO—There’s a major concern about how heavy JavaScript applications are crawled by search engines. Universal rendering means we can detect crawlers and render the site for them so that content is ready without having to worry if the crawler executes JavaScript (some do, some don’t). This will certainly enhance SEO efforts for Angular applications.



### Component

- Components—New elements that will compose the majority of your application’s structure and logic

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




### Lifecycle

```js
//===app.module.ts===
//The App module is the packaging that helps tell Angular what’s available to render.
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent } from './main/app.component';

//The NgModule decorator takes an object with a few different properties. 
@NgModule({
    //The declarations property is to provide a list of any components and directives to make available to the entire application.
    declarations: [
        AppComponent
    ],
    //The imports property is an array of other modules upon which this module depends
    imports: [
        BrowserModule
    ],
    //The providers property, which is empty by default. Any services that are created are to be listed here 
    providers: [],
    //The bootstrap property defines which components to bootstrap at runtime.
    bootstrap: [AppComponent]
})
export class AppModule { }
```



```js
//===heros.component.ts===
@Component({
    selector: "",
    template: ``
})
export class MyComponent {
    //The constructor method runs as soon as the component is created.
    constructor() {
        //Don’t load data from the service in the constructor
    }
    //Components expose a number of lifecycle hooks that allow you to execute commands at various stages of rendering, giving you greater control over when things occur.
    ngOnInit() { }
}
```



```js
//===app.component.ts===
//Angular apps are components that contain a tree of components. The root app is bootstrapped on page load to initialize the application.
import { Component } from '@angular/core';

@Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css']
})
export class AppComponent {
}
```



<img src="../img/How Angular renders the base app into the browser.png" />

- Immediately upon loading the page, the bootstrapper is called to begin Angular execution.

- It loads your App module and reads through any additional dependencies that need to be loaded and bootstrapped.

- Renders the App component, which is the root element of your application.

- Any child components are also rendered as part of the component tree.

- It will also resolve bindings and set up event listeners for anything that declares it.

- Component lifecycle while the application is running

  ![component behavior](..\img\Component lifecycle.png)



### Services

- Services—Reusable objects that fill niche roles such as data access or helper utilities

- Services are objects that abstract some common logic that you plan to reuse in multiple places.  
- Another way to think of services is as sharable objects that any part of your app can import as needed. 
- The intention of a service is to enable reuse of code. A service might be a set of common methods that need to be shared. You could have various “helper methods” that you don’t want to write over and over, such as utilities to parse data formats or authentication logic that needs to be run in multiple places.
- Shared code across your application is almost always best placed inside of a service. In Angular, most of the time a service also is something that you can inject into your
  controllers using dependency injection, though there is no exact defnition of what makes an object a service. 
- Injectable services are the typical Angular services that provide a feature for the
  application and work with Angular’s DI system to be injected into components. An example would be a service that handles how to load data from an API.
- Non-injectable services are JavaScript objects that aren’t tied into Angular’s DI system and are just imported into the file. This can be useful to make a service available outside of Angular’s DI, such as in the application’s main file.
- Helper services are services that make it easier to use a component or feature. An example would be a service to help manage the currently active alert on the page.
- Data services are for sharing data across the application. An example is an object holding data for the logged-in user. 
- As you build your application, you may see some code start to reappear in various places. That’s what all services help to reduce, but sometimes you’ll find that some
  code feels out of place and needs to be extracted into a service. When you do that, you’re creating a service that exposes helper functions to simplify your components.

- Services can be injected anywhere in the application as long as they’ve been registered with a provider.
- Angular provides an HttpClient service that helps manage making XHR requests, and you can wrap your own services around it to simplify data access.
- Services can hold values that get propagated, but you need to be aware of the injection tree to avoid getting different copies of the service.



### Routing

- Most applications need the ability to allow users to navigate around the application, and by using the router we can see how to route between different components.

- The router works by declaring an outlet in the template, which is the place in the template that the final rendered component will be displayed.
- Routing in Angular is based around paths mapping to a component. Routes will render a single component, and that component will also be able to render any additional components it needs.

- Angular allows you to control the conditions that allow a route to render, which
  usually is done to prevent the application from going into a bad state. For example,
  you shouldn’t allow an unauthenticated user to view portions of the application that
  require them to be logged in or they will likely get a lot of errors. 

- A guard is like a lifecycle hook for route changes that allows an application to verify
  certain conditions or load data before a change occurs 

- Here are the five types of guards and their basic roles:

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
  import { CanActivate, Router, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';
  import { UserService } from './user.service';
  
  @Injectable()
  export class AuthGuardService implements CanActivate {
      constructor(private userService: UserService, private router: Router) {}
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

- Lazy loading: Feature modules can be lazy loaded into the application, giving you the ability to reduce the file size of the code that’s initially downloaded to users. It only loads
  the module when the user navigates to a route that’s part of the feature module. 



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



### Modules

- Modules—Objects that help you to organize dependencies into discrete units

- The declarations array contains a list of all components and directives that the application’s main module wants to make available to the entire application. 

- The providers array contains a list of all of the services that you want to make available to the whole application. 

- The imports array contains a list of the other modules that this module depends upon. 

- To start rendering, Angular also needs to know what component(s) to render on the screen, and it looks at the bootstrap array for this list. 



### Directives

- Directives allow you to modify the behavior and display of DOM elements in a template.  
- Directives make it possible to add some conditional logic or otherwise modify the way the template behaves or is rendered. 
- The NgClass directive is able to add or remove CSS classes to and from the element. 
- The safe navigation operator ?. will silently fail and not display anything if the property is missing. 

- Angular favors putting logic and capabilities straight into the HTML markup of the application 

- NgIf gives an element the ability to conditionally render or be removed 

- The primary default directives provided by Angular consist of the following:
  - NgClass—Conditionally apply a class to an element
  - NgStyle—Conditionally apply a set of styles to an element
  - NgIf—Conditionally insert or remove an element from the DOM
  - NgFor—Iterate over a collection of items
  - NgSwitch—Conditionally display an item from a set of options 

![structural or attribute directive](D:/project/justnote/img/structural%20or%20attribute%20directive.png)



### Pipes

- Using pipes, we can transform the data in the view during rendering without changing the underlying data value. 
- Pipes added directly into the expression to format the output.  
- Pipes only modify the data before it is displayed, and do not change the value in the controller. 

![pipe](D:/project/justnote/img/pipe.png)

- Two types of pipes

  - Pure pipes maintain no state information and allow you to transform a value before output using a pure function.
  - Impure pipes allow you to maintain state inside of a pipe, but they’re run with
    every change detection check and are to be avoided if possible.

  ![pure and impure pipes](D:/project/justnote/img/pure%20and%20impure%20pipes.png)



### Compilers

- Just-in-Time (JiT): The compiling of the application happens in the browser only after the assets have all been loaded. 

- Ahead-of-Time (AoT): Render the content before sending it to the browser.  



### Dependency injection

- DI is a pattern for obtaining objects that uses a registry to maintain a list of available objects and a service that allows you to request the object you need. Rather than having to pass around objects, you can ask for what you need when you need it. 
- Injector: This is the service that Angular provides for requesting and registering dependencies 
- Providers are responsible for creating the instanceof the object requested.
- Dependency injection is fundamental for Angular to track all the objects in the application and make them available when they’re requested.
- Why do we want to use DI instead of regular module loading in JavaScript? There are a few key reasons you’ll usually want to make your services injectable: 
  - It allows you to focus on consuming services and not worry about how to create them.
  - It resolves external dependencies for you, simplifying consumption.
  - It’s easier to test your Angular entities because you can always inject a mock service instead of the real thing.
  - It can allow you to control the injection of a service without having to worry about where another piece of the application might also inject the service.
  - It gives you a clean instance of the service for each injector tree. 



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



### Communicating

![output event](..\img\output event.png)



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



**View Child to reference components**

- ViewChild is a decorator for a controller property, like Inject or Output, which tells Angular to fill in that property with a reference to a specifc child component controller. 

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



### Styling

- Five different approaches: 
  - Global CSS rules
  - Inline CSS style element
  - Inline style declaration
  - Component styles property
  - Component styleUrls property linked to a CSS file 

- One key capability is the ability to ensure that CSS styling for a component doesn’t bleed over into the rest of the app, which is called styling encapsulation.  

- Three encapsulation modes for a view. 
  - None—No encapsulation is used during rendering of the view, and the component DOM is subject to the normal rules of CSS. Templates aren’t modifed when injected into the app except for the removal of any CSS style elements from the template to the document head.
  - Emulated—Emulated encapsulation is used to simulate styling encapsulation by adding unique CSS selectors to your CSS rules at runtime. CSS can cascade into the component easily from the global CSS rules.
  - Native—Uses the native Shadow DOM for styling and markup encapsulation and provides the best encapsulation. All styles are injected inside the shadow root and are therefore localized to the component. None of the templates or styles declared for the component are visible outside the component. 



### Forms

- Template-driven forms define the form using NgModel on form controls.
- You can apply normal HTML validation attributes, and NgModel will automatically
  try to validate based on those rules.
- Reactive forms are different in that you define the form model in the controller
  and link form controls using FormControlName.
- You can observe the changes of a form control with reactive forms and run logic
  every time a new value is emitted.
- Ultimately, both form patterns are available to you. I tend to use reactive forms,
  especially as the form gets more complex.
