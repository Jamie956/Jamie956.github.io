## Angular in Action

### 1.4 Angular: a platform, not a framework

Angular comes with a leaner core library and makes additional features available as
separate packages that can be used as needed. It also has many tools that push it beyond
a simple framework, including the following:

- Dedicated CLI for application development, testing, and deployment
- Ofﬂine rendering capabilities on many back-end server platforms
- Desktop-, mobile-, and browser-based application execution environments
- Comprehensive UI component libraries, such as Material Design



**Angular CLI**

The CLI has a number of features that aid in the development of Angular apps. Here
are the primary features:

- Generates new project scaffolding—Instead of having to create a new project from an
existing project or creating all the fles yourself, the CLI will generate a full project with a basic app already started for you.
- Generates new application pieces—Need a new component? Easy; it can generate
the fles for you. It can generate components, services, routes, and pipes, and it
also will automatically ensure they are fully wired up in the build process.
- Manages the entire build toolchain—Because fles need to be processed before
being served to the client (such as TypeScript compilation), the CLI will process
your source fles and build them into an optimized version for development or
production.
- Serves a localhost development server—The CLI handles the build ﬂow and then starts
a server listening on localhost so you can see the results, with a live reload feature. 
- Incorporates code linting and formatting code—Helps enforce quality code by using
the CLI to lint your code for style and semantic errors, and it can also help format
your code automatically to the style rules.
- Supports running unit and e2e tests—Tests are vital, so the CLI sets up Karma for
running your unit tests and works with Protractor to execute your e2e tests. It will
automatically pick up and execute new tests as they’re generated. 



**Server rendering and the compiler**

a few primary use cases: 

- Server rendering for faster loading—Mobile devices are the primary way to access the
  internet these days, and mobile connections are frequently slow and unreliable.
  A server-side rendering option allows you to resolve data bindings and render
  components on the server so the initial payload sent to the user is pre-initialized.
  It can also optimize and send the necessary bytes for a quick initial load time and
  lazy load the other assets as needed.
- Performance in the browser—One of the major pain points of JavaScript is that it’s
  single threaded, which means that JavaScript can only handle one instruction at
  a time. In modern browsers, a newer technology known as web workers allows
  Angular to push some of the execution of the compiler into another process.
  This means that a lot more processing can occur, and it allows things like animations and user interactions to be smoother. 
- SEO—There’s a major concern about how heavy JavaScript applications are
  crawled by search engines. Universal rendering means we can detect crawlers
  and render the site for them so that content is ready without having to worry
  if the crawler executes JavaScript (some do, some don’t). This will certainly
  enhance SEO efforts for Angular applications.
- Multiple platforms—Many developers want to use other platforms for their back
  ends, such as .NET or PHP. Angular can be compiled in the platform of choice,
  assuming there’s a supported renderer. Angular will provide support for NodeJS,
  but the community is actively building and maintaining rendering support for
  other platforms such as Java and Go. 



### 1.5 Component architecture

a component is a way to create custom HTML elements in your
application. 

**Components' key characteristics**

Components have some concepts that drive their design and architecture.  

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

HTML is the language of the web because it describes the content of a page in a fairly
concise set of elements. 



The offcial specifcation for custom elements is intended to allow developers to create new HTML elements that essentially blend naturally and natively into the DOM. 



Every Angular component is a custom element and fulflls the four
tenets (and more) that we expect to get from a custom element. 



**Shadow DOM**

The Shadow DOM is an isolated Document Object Model (DOM) tree that’s detached from the typical CSS inheritance, allowing you to create a barrier between markup inside and outside of the Shadow DOM. For example, if you have a button inside of a Shadow DOM
and a button outside, any CSS for the button written outside the Shadow DOM won’t
affect the button inside it. 



the CSS selector is the only way to limit which elements receive that particular styling. 



https://github.com/Jamie956/javascript/blob/master/shadow-dom.html



Shadow DOM enables the best form of encapsulation available in the browser for styles
and templates. 



**Templates**

Templates are often used with the Shadow DOM because it allows you to defne the
template and then inject it into the shadow root. Without templates, the Shadow DOM
APIs would require us to inject content node by node. They’re also used by Angular as
part of the lifecycle of components and the compilation process, allowing Angular to
keep isolated, inert copies of the template as data changes and needs to be recompiled. 



**JavaScript modules**

Previously, developers had to build a bundle of all the assets for the web application
ahead of time and deliver the whole package to the user. 



### 1.6 Modern JavaScript and Angular

- Classes
- Decorators
- Modules
- Template literals 



**Observables**

observables are a newer pattern for JavaScript applications to
manage asynchronous activities.  

observables are a newer pattern for JavaScript applications to
manage asynchronous activities.  



To use observables, you subscribe to the stream of data and pass a function that will
run every time there’s a new piece of data.  



### 1.7 TypeScript and Angular

The basic value proposition of TypeScript is it can force restrictions on what types of
values variables hold. 



TypeScript can help catch many simple syntax errors before they affect your application. 



Here are the primary reasons to use TypeScript 

- Adds clarity to your code—Variables that have types are easier to understand,
because other developers (or yourself in six months) don’t have to think very
hard about what the variable should be. 
- Enables a smarter editor—When you use TypeScript with a supported editor, you’ll
get automatic IntelliSense support for your code. As you write, the editor can
suggest known variables or functions and tell you what type of value it expects.
- Catches errors before you run code—TypeScript will catch syntax errors before you
run the code in the browser, helping to reduce the feedback loop when you write
invalid code.
- Entirely optional—You can use types when you want, and optionally leave them out
where it doesn’t matter. 



**Summary**

- Angular is a platform, with many key elements such as tooling, UI libraries, and
  testing built in or easily incorporated into your application projects.
- Applications are essentially combinations of components. These components
  build upon the core principles of encapsulation, isolation, and reusability, which
  should have events, be customizable, and be declarative.
- ES6 and TypeScript provide a lot of the underpinnings for Angular’s architecture
  and syntax, making it a powerful framework without having to build a lot of custom language capabilities. 



2, Building your first Angular app

3, App essentials

4, Component basics

5, Advanced components

6, Services

7, Routing

8, Building custom directives and pipes

9, Forms

10, Testing your application

11, Angular in production













