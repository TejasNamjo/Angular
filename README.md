The Angular CLI generates files in order for your Angular application to be served successfully. 
main.bundle.js, which is the transpiled code that is specific to your application 
 vendor.bundle.js which includes all the third-party libraries and frameworks you depend on (including Angular) 
styles.bundle.js is a compilation of all the CSS styles that are needed for your application 
polyfills.bundle.js includes all the polyfills needed for supporting some capabilities in older browsers (like advanced ECMAScript features not yet available in all browsers) 
inline.bundle.js is a tiny file with webpack utilities and loaders that is needed for bootstrapping the application. 
 
The ng serve command is responsible for translating our TypeScript code into JavaScript for the browser to load. 
 
Basics of an Angular Application 
Root HTML—index.html 
Root component for our Angular application 
The only thing of note in the preceding code is the <app-root> element in the HTML, which is the marker for loading our application code. 
It is responsible for deciding which files are to be loaded. 
 
The Entry Point—main.ts 
The second important part of our bootstrapping piece is the main.ts file. 
It identifies which Angular module is to be loaded when the application starts. It can also change application-level configuration (like turning off framework-level asserts and verifications using the enableProdMode() flag). 
Its main aim is to point the Angular framework at the core module of your application and let it trigger the rest of your application source code from that point. 
 
Main Module—app.module.ts 
This is where your application-specific source code starts from. The application module file can be thought of as the core configuration of your application, from loading all the relevant and necessary dependencies, declaring which components will be used within your application, to marking which is the main entry point component of your application. 
Root Component—AppComponent 
 
UNDERSTANDING DATA BINDING 
Angular has a mechanism called data binding that allows you to keep a component’s properties in sync with the view. 
Angular supports two types of data binding: unidirectional (default) and two-way. With unidirectional data binding, the data is synchronized in one direction: either from the class member variable (property) to the view or from the view event to the class variable or a method that handles the event. 
Binding properties and events 
The easiest way to display a component property is to bind the property name through interpolation. With interpolation, you put the property name in the view template, enclosed in double curly braces: {{myHero}}. 
Interpolation allows you to incorporate calculated strings into the text between HTML element tags and within attribute assignments. 
The text between the braces is a template expression that Angular first evaluates and then converts to a string. 
Angular automatically pulls the value and inserts those values into the browser. Angular updates the display when these properties change. 
[hidden]="isValid" 
Use square brackets to bind the value from a class variable to a property of an HTML element or an Angular component. 
You can't use JavaScript expressions that have or promote side effects, including: 
Assignments (=, +=, -=, ...) 
Operators such as new, typeof, instanceof, etc. 
Chaining expressions with ; or , 
The increment and decrement operators ++ and -- 
Some of the ES2015+ operators 
 
unidirectional binding from the view to a class member, such as a method 
Template statements 
A template statement responds to an event raised by a binding target such as an element, component, or directive. 
To assign an event-handler function to an event, put the event name in parentheses in the component’s template. 
(click)="onClickEvent($event)  
(input)="onInputEvent() 
When the event specified in parentheses is triggered, the expression in double quotes is reevaluated. 
If you’re interested in analyzing the properties of the event object, add the $event argument to the method handler. In particular, the target property of the event object represents the DOM node where the event occurred. 
The context for terms in an expression is a blend of the template variables(#XYZ), the directive's context object (if it has one), and the component's members. If you reference a name that belongs to more than one of these namespaces, the template variable name takes precedence, followed by a name in the directive's context, and, lastly, the component's member names. 
When using template expressions follow these guidelines: 
No visible side effects 
Quick execution 
Simplicity 
 
Two-way Binding 
To denote two-way binding, surround a template element’s ngModel with both square brackets and parentheses [square brackets represent property binding, and parentheses represent event binding]. 
[(ngModel)]="shippingAddress" 
The app module imports FormsModule, required because you use the ngModel directive, which is part of the Forms API. 
ngModel directive hides onerous details behind its own ngModel input and ngModelChange output properties. 
The ngModel data property sets the element's value property and the ngModelChange event property listens for changes to the element's value. 
The [(ngModel)] syntax can only set a data-bound property. If you need to do something more or something different, you can write the expanded form. 
<input [ngModel]="currentHero.name" (ngModelChange)="setUppercaseName($event)"> 
HTML attribute vs. DOM property 
Attributes are defined by HTML. Properties are defined by the DOM (Document Object Model). 
Attributes initialize DOM properties and then they are done. Property values can change; attribute values can't. 
when the browser renders <input type="text" value="Bob">, it creates a corresponding DOM node with a value property initialized to "Bob". 
When the user enters "Sally" into the input box, the DOM element value property becomes "Sally". But the HTML value attribute remains unchanged if you ask the input element about that attribute: input.getAttribute('value') returns "Bob". 
The HTML attribute value specifies the initial value; the DOM value property is the current value. 
The HTML attribute and the DOM property are not the same thing, even when they have the same name. 
Template binding works with properties and events, not attributes. 
In the world of Angular, the only role of attributes is to initialize element and directive state. When you write a data binding, you're dealing exclusively with properties and events of the target object. HTML attributes effectively disappear. 
Interpolation and property binding can set only properties, not attributes. 
You need attribute bindings to create and bind to such attributes. 
Attribute binding syntax resembles property binding. Instead of an element property between brackets, start with the prefix attr, followed by a dot (.) and the name of the attribute. 
<td [attr.colspan]="1 + 1">One-Two</td> 
Class binding 
You can add and remove CSS class names from an element's class attribute with a class binding. 
Instead of an element property between brackets, start with the prefix class, optionally followed by a dot (.) and the name of a CSS class: [class.class-name]. 
Style binding 
You can set inline styles with a style binding. 
[style.background-color]="canSave ? 'cyan': 'grey'" 
 
INPUT AND OUTPUT PROPERTIES 
If an Angular component needs to receive values from the outside world, you can bind the producers of these values to the corresponding inputs of the component. 
If a component needs to communicate values to the outside world, it can emit events through its output properties. 
Input properties 
The input properties of a component annotated with the @Input() decorator are used to get data from the parent component. 
Whenever you make any change through child component, shared object will be changed in the parent component as well(due to object reference ) and Angular will detect changes and trigger the UI update.  
But if primitive data variable is shared, it won’t be changed in parent component. 
Intercept input property changes with a setter 
Use an input property setter to intercept and act upon a value from the parent. 
@Input() 
set name(name: string) { 
this._name = (name && name.trim()) || '<no name set>'; 
} 
  
get name(): string { return this._name; } 
} 
Detect and act upon changes to input property values with the ngOnChanges() method of the OnChanges lifecycle hook interface. 
Output properties and custom events 
Angular components can dispatch custom events using the EventEmitter object. These events are to be consumed by the component’s parent. EventEmitter is a subclass of Subject that can serve as both observable and observer, but typically you use EventEmitter just for emitting custom events that are handled in the template of the parent component. 
The directive creates an EventEmitter and exposes it as a property. The directive calls EventEmitter.emit(payload) to fire an event, passing in a message payload, which can be anything. Parent directives listen for the event by binding to this property and accessing the payload through the $event object. 
The parent component cannot data bind to the child's methods nor to its property. 
You can place a local variable, #xyz, on the tag <child-selector> representing the child component. That gives you a reference to the child component and the ability to access any of its properties or methods from within the parent template. 
 
Event bubbling 
Angular doesn’t offer an API to support event bubbling. 
If event bubbling is important to your app, don’t use EventEmitter; use native DOM events instead. 
Typing the event object reveals a significant objection to passing the entire DOM event into the method: the component has too much awareness of the template details. It can't extract information without knowing more than it should about the HTML implementation. That breaks the separation of concerns between the template (what the user sees) and the component (how the application processes user data). 
This problem is addressed by template reference variables. 
 
Get user input from a template reference variable 
There's another way to get the user data: use Angular template reference variables. These variables provide direct access to an element from within the template. To declare a template reference variable, precede an identifier with a hash (or pound) character (#). 
 [A template reference variable is often a reference to a DOM element within a template.] 
The scope of a reference variable is the entire template. 
<input #box (keyup)="0"> <p>{{box.value}}</p>  
<button (clikc)=”addBoxValue(box)” >Add Value</button> 
The template reference variable named box, declared on the <input> element, refers to the <input> element itself. The code uses the box variable to get the input element's value and display it with interpolation between <p> tags. 
The template is completely self contained. It doesn't bind to the component, and the component does nothing. 
Key event filtering (with key.enter) 
The (keyup) event handler hears every keystroke. Sometimes only the Enter key matters, because it signals that the user has finished typing. One way to reduce the noise would be to examine every $event.keyCode and take action only when the key is Enter. 
There's an easier way: bind to Angular's keyup.enter pseudo-event. Then Angular calls the event handler only when the user presses Enter. 
<input #box (keyup.enter)="onEnter(box.value)"> 
On blur 
In the previous example, the current state of the input box is lost if the user mouses away and clicks elsewhere on the page without first pressing Enter. The component's value property is updated only when the user presses Enter. 
To fix this issue, listen to both the Enter key and the blur event. 
(blur)="update(box.value)" 
Observations 
Use template variables to refer to elements 
Pass values, not elements 
Keep template statements simple 
 
The @ViewChild() decorator is handy when you need a reference to a child component. 
<input type="text" name="content" #serverContentInput> 
@ViewChild('serverContentInput') serverContentInput: ElementRef; 
The local variable approach is simple and easy. But it is limited because the parent-child wiring must be done entirely within the parent template. The parent component itself has no access to the child. 
You can't use the local variable technique if an instance of the parent component class must read or write child component values or must call child component methods. 
When the parent component class requires that kind of access, inject the child component into the parent as a ViewChild. 
By default VIEWCHILD will give child Component access, but if you want access to child HTML Element use  
<child #serverContentInput> </child> 
@ViewChild('child’, {read: ElementRef}) serverChildInput: ElementRef; 
 
 
The @ViewChildren() decorator would give you references to several children of the same type. 
 
The safe navigation operator ( ?. ) and null property paths 
The Angular safe navigation operator (?.) is a fluent and convenient way to guard against null and undefined values in property paths. 
The current hero's name is {{currentHero?.name}} 
The expression bails out when it hits the first null value. The display is blank, but the app keeps rolling without errors. 
The non-null assertion operator ( ! ) 
As of Typescript 2.0, you can enforce strict null checking with the --strictNullChecks flag. TypeScript then ensures that no variable is unintentionally null or undefined. 
In this mode, typed variables disallow null and undefined by default. The type checker throws an error if you leave a variable unassigned or try to assign null or undefined to a variable whose type disallows null and undefined. 
The type checker also throws an error if it can't determine whether a variable will be null or undefined at runtime. You can tell the type checker that it can't happen by applying the post-fix non-null assertion operator (!). 
The hero's name is {{hero!.name}} 
When the Angular compiler turns your template into TypeScript code, it prevents TypeScript from reporting that hero.name might be null or undefined. 
Built-in template functions 
The $any() type cast function 
Sometimes a binding expression triggers a type error during AOT compilation and it is not possible or difficult to fully specify the type. To silence the error, you can use the $any() cast function to cast the expression to the any type 
The item's undeclared best by date is: {{$any(item).bestByDate}} 
 
PROJECTING TEMPLATES AT RUNTIME WITH NGCONTENT 
In some cases, a parent component needs to render arbitrary markup within a child at runtime, and you can do that in Angular using projection. You can project a fragment of the parent component’s template onto its child’s template by using the ngContent directive. This is a two-step process: 
1.  In the child component’s template, include the tags <ng-content></ng-content> (the insertion point). 
2.  In the parent component, include the HTML fragment that you want to project into the child’s insertion point between tags representing the child component. 
 
Projecting onto multiple areas 
A component can have more than one <ng-content> tag in its template. 
consider an example where a child component’s template is split into three areas: header, content, and footer. To implement this, the child component needs to include two separate pairs of <ng-content></ng-content>s populated by the parent (header and footer). 
To ensure that the header and footer content will be rendered in the proper <ng-content> areas, you’ll use the select attribute, which can be any valid CSS selector (a CSS class, tag name, and so on). 
<ng-content select=".header"></ng-content> 
<div>This content is defined in child</div> 
<ng-content select=".footer"></ng-content> 
The content that arrives from the parent will be matched by the selector and rendered in the corresponding area. 
<ng-content> is preferable to binding to innerHTML for these reasons: 
innerHTML is a browser-specific API, whereas <ng-content> is platform independent. 
With <ng-content>, you can define multiple slots where the HTML fragments will be inserted. 
<ng-content> allows you to bind the parent component’s properties into projected HTML. 
Special selectors 
Component styles have a few special selectors from the world of shadow DOM style scoping 
:host 
Use the :host pseudo-class selector to target styles in the element that hosts(parent component) the component (as opposed to targeting(child component) elements inside the component's template). 
The :host selector is the only way to target the host element. You can't reach the host element from inside the component with other selectors because it's not part of the component's own template. The host element is in a parent component's template. 
:host-context 
Sometimes it's useful to apply styles based on some condition outside of a component's view. For example, a CSS theme class could be applied to the document <body> element, and you want to change how your component looks based on that. 
The ::ng-deep pseudo-class selector 
If we want our component styles to cascade to all child elements of a component, but not to any other element on the page. 
This combination of selectors is useful for example for applying styles to elements that were passed to the template using ng-content. 
 
View encapsulation modes 
Shadow DOM introduces scopes for CSS styles and encapsulation of DOM nodes in the browser. Shadow DOM allows you to hide the internals of a selected component from the global DOM tree. 
The encapsulation property of the @Component() decorator can have one of three values: 
ViewEncapsulation.Native— This can be used with browsers that support Shadow DOM. 
ViewEncapsulation.Emulated— By default, Angular emulates Shadow DOM support. 
ViewEncapsulation.None— If the styles have the same selectors, the last one wins. 
 
Change detection strategies 
Angular offers two CD strategies: Default and OnPush. If all components use the Default strategy, the Zone checks the entire component tree, regardless of where the change happened. 
If a particular component declares the OnPush strategy, the Zone checks this component and its children only if the bindings to the component’s input properties have changed, or if the component uses AsyncPipe, and the corresponding observable started emitting values. 
If a component that has the OnPush strategy changes a value of one of its properties bound to its template, the change detection cycle won’t be initiated. To declare the OnPush strategy, add the following line to the @Component() decorator: 
changeDetection: ChangeDetectionStrategy.OnPush 
The following frequently used browser mechanisms are patched to support change detection: 
all browser events (click, mouseover, keyup, etc.)  
setTimeout() and setInterval() 
Ajax requests 
Each Angular component has an associated change detector, which is created at application startup time. 
How change detection works? 
for each expression used in the template, it's comparing the current value of the property used in the expression with the previous value of that property. 
By default, Angular does not do deep object comparison to detect changes, it only takes into account properties used by the template. 
When using OnPush detectors, then the framework will check an OnPush component when any of its input properties changes, when it fires an event, or when an Observable fires an event 
If we build our application using immutable objects and immutable lists only, its possible to use OnPush everywhere transparently, without the risk of stumbling into change detection bugs. 
Avoiding change detection loops: Production vs Development mode 
One of the important properties of Angular change detection is that unlike Angular 1 it enforces a uni-directional data flow: when the data on our controller classes gets updated, change detection runs and updates the view. 
But that updating of the view does not itself trigger further changes which on their turn trigger further updates to the view, creating what in Angular 1 was called a digest cycle 
turning on/off change detection, and triggering it manually 
constructor(private ref: ChangeDetectorRef) { 
    ref.detach(); 
    setInterval(() => { 
      this.ref.detectChanges(); 
    }, 5000); 
  } 
Base class for Angular Views, provides change detection functionality. A change-detection tree collects all views that are to be checked for changes. Use the methods to add and remove views from the tree, initiate change-detection, and explicitly mark views as dirty, meaning that they have changed and need to be rerendered. 
COMPONENT LIFECYCLE 
 
The callbacks shown on the light-gray background will be invoked only once, and those on the darker background can be invoked multiple times during the component life span. The user sees the component after the initialization phase is complete. Then the change detection mechanism ensures that the component’s properties stay in sync with its UI. If the component is removed from the DOM tree as a result of the router’s navigation or a structural directive (such as *ngIf), Angular initiates the destroy phase. 
ngOnInit()— Invoked after the first invocation of ngOnChanges(), if any. Although you might initialize some component variables in the constructor, the properties of the component aren’t ready yet. By the time ngOnInit() is invoked, the component properties will have been initialized, which is why this method is mainly used for the initial data fetch. 
Use ngOnInit() for two main reasons: 
To perform complex initializations shortly after construction. 
To set up the component after Angular sets the input properties. 
ngDoCheck()— Called on each pass of the change detector. If you want to implement a custom change detection algorithm or add some debug code, write it in ngDoCheck(). Use the DoCheck hook to detect and act upon changes that Angular doesn't catch on its own. 
While the ngDoCheck() hook can detect when the Objects property has changed, it has a frightful cost. This hook is called with enormous frequency—after every change detection cycle no matter where the change occurred. 
ngAfterContentInit()— Invoked when the child component’s state is initialized and the projection completes. This method is called only if you used <ng-content> in your component’s template. the AfterContentInit() and AfterContentChecked() hooks that Angular calls after Angular projects external content into the component. 
ngAfterViewInit()— Invoked after a component’s view has been fully initialized. AfterViewInit() and AfterViewChecked() hooks that Angular calls after it creates a component's child views.  
Each lifecycle callback is declared in the interface with a name that matches the name of the callback without the prefix ng. But it is not required to implement the interface, you can only write the method and angular calls it. 
The interfaces are optional for JavaScript and Typescript developers from a purely technical perspective. The JavaScript language doesn't have interfaces. Angular can't see TypeScript interfaces at runtime because they disappear from the transpiled JavaScript. 
When Angular invokes ngOnChanges(), it provides a SimpleChanges object containing the old and new values of the modified input property and the flag indicating whether this is the first binding change. Angular calls its ngOnChanges() method whenever it detects changes to input properties of the component (or directive). 
If the value of the property is the reference to the object. Angular doesn't care that the object’s own property changed. The object reference didn't change so, from Angular's perspective, there is no change to report! 
Angular Elements 
Angular elements are Angular components packaged as custom elements, a web standard for defining new HTML elements in a framework-agnostic way. 
A custom element extends HTML by allowing you to define a tag whose content is created and controlled by JavaScript code. The browser maintains a CustomElementRegistry of defined custom elements (also called Web Components), which maps an instantiable JavaScript class to an HTML tag. 
The @angular/elements package exports a createCustomElement() API that provides a bridge from Angular's component interface and change detection functionality to the built-in DOM API. 
Transforming a component to a custom element makes all of the required Angular infrastructure available to the browser. 
How it works 
Use the createCustomElement() function to convert a component into a class that can be registered with the browser as a custom element. After you register your configured class with the browser's custom-element registry, you can use the new element just like a built-in HTML element in content that you add directly into the DOM: 
<my-popup message="Use Angular!"></my-popup> 
 
DIRECTIVES 
 
A directive is a class annotated with the @Directive() decorator. 
A directive can’t have its own UI, but can be attached to a component or a regular HTML element to change their visual representation. There are three kinds of directives in Angular: 
Components—directives with a template. 
Structural directives—change the DOM layout by adding and removing DOM elements. 
Attribute directives—change the appearance or behavior of an element, component, or another directive. 
An attribute directive minimally requires building a controller class annotated with @Directive, which specifies the selector that identifies the attribute. 
 ElementRef in the directive's constructor to inject a reference to the host DOM element. ElementRef grants direct access to the host DOM element through its nativeElement property. 
HostBinding 
Decorator that marks a DOM property as a host-binding property. 
Angular automatically checks host property bindings during change detection, and if a binding changes it updates the host element of the directive. 
Changes color for the elements where the directive is applied 
@HostBinding(‘style.color’) color: string = ‘green’;    
@HostBinding(‘style.color’) 
 get color() { return “green”; }  
Respond to user-initiated events 
The directive could be more dynamic. It could detect when the user mouses into or out of the element and respond by setting or clearing the highlight color. 
The @HostListener decorator lets you subscribe to events of the DOM element that hosts an attribute directive. 
Of course you could reach into the DOM with standard JavaScript and attach event listeners manually. There are at least three problems with that approach: 
You have to write the listeners correctly. 
The code must detach the listener when the directive is destroyed to avoid memory leaks. 
Talking to DOM API directly isn't a best practice. 
 @HostListener('mouseenter') onMouseEnter() { 
this.highlight('yellow'); 
} 
Pass values into the directive with an @Input data bindinglink 
@Input() highlight: string; 
<p appHighlight highlight='green' >This is attritue directive</p> 
 you can use directive name as property binding 
@Input(‘appHighlight’) highlight: string; 
<p [appHighlight] ='green' >This is attritue directive</p> 
 
Check about ‘ExportAs’ (used to export directive) 
Structural Directives 
Structural directives are responsible for HTML layout. They shape or reshape the DOM's structure, typically by adding, removing, or manipulating elements. 
Why remove rather than hide? 
A directive could hide the unwanted paragraph instead by setting its display style to none. 
Such a component's behavior continues even when hidden. The component stays attached to its DOM element. It keeps listening to events. Angular keeps checking for changes that could affect data bindings. Whatever the component was doing, it keeps doing. 
Although invisible, the component—and all of its descendant components—tie up resources. The performance and memory burden can be substantial, responsiveness can degrade, and the user sees nothing. 
The asterisk (*) prefix 
The asterisk is "syntactic sugar" for something a bit more complicated. Internally, Angular translates the *ngIf attribute into a <ng-template> element, wrapped around the host element. 
<ng-template [ngIf]="hero"> 
                                  <div class="name">{{hero.name}}</div> 
</ng-template> 
The *ngIf directive moved to the <ng-template> element where it became a property binding,[ngIf]. 
The rest of the <div>, including its class attribute, moved inside the <ng-template> element. 
The <ng-template> 
The <ng-template> is an Angular element for rendering HTML. It is never displayed directly. In fact, before rendering the view, Angular replaces the <ng-template> and its contents with a comment. 
If there is no structural directive and you merely wrap some elements in a <ng-template>, those elements disappear. 
Angular is already using ng-template under the hood in many of the structural directives: ngIf, ngFor and ngSwitch. 
<div class="lessons-list" *ngIf="lessons else loading"> … 
EQUIVALENT TO 
<ng-template [ngIf]="lessons" [ngIfElse]="loading"> 
<div class="lessons-list"> ...  
desugaring: 
the element onto which the structural directive ngIf was applied has been moved into an ng-template  
The expression of *ngIf has been split up and applied to two separate directives, using the [ngIf] and [ngIfElse] template input variable syntax 
 
Multiple Structural Directives 
<div class="lesson" *ngIf="lessons"  
       *ngFor="let lesson of lessons"> 
This would not work! Instead, we would get the following error message 
Uncaught Error: Template parse errors: Can't have multiple template bindings on one element. Use only one attribute 
This means that its not possible to apply two structural directives to the same element. 
ng-container structural directive allows to apply a structural directive to a section of the page without having to create an extra element 
One structural directive per host element 
You may apply only one structural directive to an element. 
Use <ng-container> when there's no single element to host the directive. The Angular <ng-container> is a grouping element that doesn't interfere with styles or layout because Angular doesn't put it in the DOM. 
The <ng-container> is a syntax element recognized by the Angular parser. It's not a directive, component, class, or interface. It's more like the curly braces in a JavaScript if-block. 
There is another major use case for the ng-container directive: it can also provide a placeholder for injecting a template dynamically into the page. 
Dynamic Template Creation with the ngTemplateOutlet directive 
We can take the template itself and instantiate it anywhere on the page, using the ngTemplateOutlet directive: 
<ng-template #loading> 
<div>Loading...</div>  
</ng-template> 
<ng-container *ngTemplateOutlet="loading"></ng-container> 
Template Context 
Inside the ng-template tag body, we have access to the same context variables that are visible in the outer template. 
But each template can also define its own set of input variables. 
<ng-template #estimateTemplate let-lessonsCounter="estimate"> 
The context object is passed to ngTemplateOutlet via the context property, that can receive any expression that evaluates to an object 
<ng-container   
   *ngTemplateOutlet="estimateTemplate;context:ctx"> 
</ng-container> 
ctx ==> object which properties used {estimate: value,….} 
The template can be injected just like any other DOM element or component, by providing the template reference name to the ViewChild decorator. 
Also we can also pass a template as an input parameter. 
<child [loadingtemplate]=”estimateTemplate”></child> 
Creating a directive is similar to creating a component. 
Import the Directive decorator (instead of the Component decorator). 
Import the Input, TemplateRef, and ViewContainerRef symbols; you'll need them for any structural directive. 
Apply the decorator to the directive class. 
Set the CSS attribute selector that identifies the directive when applied to an element in a template. 
You'll acquire the <ng-template> contents with a TemplateRef and access the view container through a ViewContainerRef. 
The directive consumer expects to bind a true/false condition. That means the directive needs an property, decorated with @Input 
Pipes 
A pipe takes in data as input and transforms it to a desired output. 
Parameterizing a pipe 
A pipe can accept any number of optional parameters to fine-tune its output. To add parameters to a pipe, follow the pipe name with a colon ( : ) and then the parameter value (such as currency:'EUR'). If the pipe accepts multiple parameters, separate the values with colons (such as slice:1:5) 
Custom pipes 
You can write your own custom pipes. 
pipe key points: 
A pipe is a class decorated with pipe metadata. 
The pipe class implements the PipeTransform interface's transform method that accepts an input value followed by optional parameters and returns the transformed value. 
There will be one additional argument to the transform method for each parameter passed to the pipe.  
To tell Angular that this is a pipe, you apply the @Pipe decorator, which you import from the core Angular library. 
The @Pipe decorator allows you to define the pipe name that you'll use within template expressions. It must be a valid JavaScript identifier. 
The PipeTransform interface 
The transform method is essential to a pipe. The PipeTransform interface defines that method and guides both tooling and the compiler. Technically, it's optional; Angular looks for and executes the transform method regardless. 
Pure and impure pipes 
Pure pipes 
Angular executes a pure pipe only when it detects a pure change to the input value. A pure change is either a change to a primitive input value (String, Number, Boolean, Symbol) or a changed object reference (Date, Array, Function, Object). 
Angular ignores changes within (composite) objects. 
Impure pipes 
Angular executes an impure pipe during every component change detection cycle. An impure pipe is called often, as often as every keystroke or mouse-move. 
 
The impure AsyncPipe 
The Angular AsyncPipe is an interesting example of an impure pipe. The AsyncPipe accepts a Promise or Observable as input and subscribes to the input automatically, eventually returning the emitted values. 
The AsyncPipe is also stateful. The pipe maintains a subscription to the input Observable and keeps delivering values from that Observable as they arrive. 
 
Dependency Injection in Angular 
Dependencies are services or objects that a class needs to perform its function. DI is a coding pattern in which a class asks for dependencies from external sources rather than creating them itself. 
 DI allows you to decouple application components and services by sparing them from knowing how to create their dependencies. 
Angular documentation uses the concept of a token, which is an arbitrary key representing an object to be injected. You map tokens to values for DI by specifying providers. A provider is an instruction to Angular about how to create an instance of an object for future injection into a target component, service, or directive. 
DI increases the testability of your components in isolation. You can easily inject mock objects if you want to unit test your code. 
DI is about creating and managing dependencies, and the framework component that does this is dubbed the injector. 
In order to get a service from a dependency injector, you have to give it a token. Angular usually handles this transaction by specifying a constructor parameter and its type. The parameter type serves as the injector lookup token. Angular passes this token to the injector and assigns the result to the parameter. 
INJECTORS AND PROVIDERS 
Each component can have an Injector instance capable of injecting objects or primitive values into a component. Any Angular application has a root injector available to all of its modules. To let the injector know what to inject, you specify the provider. An injector will inject the object or value specified in the provider into the constructor of a component. Providers allow you to map a custom type (or a token) to a concrete implementation of this type (or value). 
In Angular, you can inject a service into a class only via its constructor’s arguments. 
You can configure injectors with providers at different levels of your app, by setting a metadata value in one of three places: 
In the @Injectable() decorator for the service itself. 
In the @NgModule() decorator for an NgModule. 
In the @Component() decorator for a component. 
The @Injectable() decorator has the providedIn metadata option, where you can specify the provider of the decorated service class with the root injector, or with the injector for a specific NgModule. 
The @NgModule() and @Component() decorators have the providers metadata option, where you can configure providers for NgModule-level or component-level injectors.  
The providers line instructs the injector as follows: “When you need to construct an object that has an argument of type ProductService, create an instance of the registered class for injection into this object.” 
The providers property can be specified in the @Component() annotation or @NgModule. 
providers: [ProductService] <=Equivalent to =>  
providers:({ provide: ProductService, useClass: ProductService }) 
Provide => is a token. This token is used to identify the dependency to                          inject. 
declare a provider using the following properties: 
useClass— To map a token to a class 
useFactory— To map a token to a factory function that instantiates objects based on certain criteria 
useValue— To map a string or a special InjectionToken to an arbitrary value (non-class-based injection) 
lets you associate a fixed value with a DI token. 
The value of a value provider must be defined before you specify it. 
{ provide: Hero, useValue: someHero } --> provides an existing instance of the Hero class to use for the Hero token, rather than requiring the injector to create a new instance with new or use its own cached instance. Here, the token is the class itself.  
{provide: 'IS_PROD_ENVIRONMENT', useValue: true} 
The Angular dependency injection system is hierarchical. 
[NOTE: When you specify providers in the @Injectable() decorator of the service itself (typically at the app root level), optimization tools such as those used by the CLI's production builds can perform tree shaking, which removes services that aren't used by your app. Tree shaking results in smaller bundle sizes.] 
Ng-level provides service(same instance) for whole module. 
Component-level providers configure each component instance's own injector. Angular can only inject the corresponding services in that component instance or one of its descendant component instances. Angular can't inject the same service instance anywhere else. 
A component-provided service may have a limited lifetime. Each new instance of the component gets its own instance of the service. When the component instance is destroyed, so is that service instance. 
Individual components within an NgModule have their own injectors. You can limit the scope of a provider to a component and its children by configuring the provider at the component level using the @Component metadata. 
Explicit injection using injector 
Explicit injections using Angular's Injector service. This is the same injector Angular uses to support DI. 
constructor( private injector:Injector) { 
  this.product=injector.get(ProductService); } 
Avoid this pattern as it exposes the DI container to your implementation and adds a bit of noise. 
Using InjectionToken 
There are times when the dependency we define is either a primitive, interface, callable type, array or parameterized type, object, or function. In such a scenario, the class token cannot be used as there is no class. Angular solves this problem using InjectionToken 
1) To register a dependency using InjectionToken, we first need to create the InjectionToken class instance: 
       export const APP_CONFIG = new InjectionToken('Application Configuration'); 
2) Then, use the token to register the dependency: 
{ provide: APP_CONFIG, useValue: {name:'Test App', gridSetting: {...} ...}); 
3) And finally, inject the dependency anywhere using the @Inject decorator: 
constructor(@Inject(APP_CONFIG) config) { } 
Interestingly, when @Inject() is not present, the injector uses the type/class name of the parameter (class token) to locate the dependency. 
Angular also supports string tokens 
{ provide: 'appconfig', useValue: ….} 
Element injectors 
An injector does not actually belong to a component, but rather to the component instance's anchor element in the DOM. A different component instance on a different DOM element uses a different injector. 
Directives can also have dependencies, and you can configure providers in their @Directive() metadata. When you configure a provider for a component or directive using the providers property, that provider belongs to the injector for the anchor DOM element. 
@Injectable-level configuration 
The @Injectable() decorator identifies every service class. The providedIn metadata option for a service class configures a specific injector (typically root) to use the decorated class as a provider of the service. When an injectable class provides its own service to the root injector, the service is available anywhere the class is imported. 
The injector is responsible for creating service instances and injecting them into classes. Angular creates injectors for you as it executes the app, starting with the root injector that it creates during the bootstrap process. 
A provider tells an injector how to create the service. You must configure an injector with a provider before that injector can create a service (or provide any other kind of dependency). 
Instead of specifying the root injector, you can set providedIn to a specific NgModule. This is generally no different from configuring the injector of the NgModule itself, except that the service is tree-shakable if the NgModule doesn't use it. 
Injector bubbling 
When a component requests a dependency, Angular tries to satisfy that dependency with a provider registered in that component's own injector. If the component's injector lacks the provider, it passes the request up to its parent component's injector. If that injector can't satisfy the request, it passes the request along to the next parent injector up the tree. The requests keep bubbling up until Angular finds an injector that can handle the request or runs out of ancestor injectors. If it runs out of ancestors, Angular throws an error. 
From Angular 6+ you can provide application-wide services in a different way. 
Instead of adding a service class to the providers[]  array in AppModule , you can set the following config in @Injectable() : 
@Injectable({providedIn: 'root'}) 
export class MyService { ... } 
The "new syntax" does offer one advantage though: Services can be loaded lazily by Angular (behind the scenes) and redundant code can be removed automatically. 
Dependency Providers 
A dependency provider configures an injector with a DI token, which that injector uses to provide the concrete, runtime version of a dependency value. The injector relies on the provider configuration to create instances of the dependencies that it injects into components, directives, pipes, and other services. 
The Provider object literal 
The class-provider syntax is a shorthand expression that expands into a provider configuration, defined by the Provider interface. 
           providers: [Logger]   ==> providers: [{ provide: Logger, useClass: Logger }] 
The useClass provider key lets you create and return a new instance of the specified class.  
Different classes can provide the same service. 
[{ provide: Logger, useClass: BetterLogger }] 
Aliased class providers 
[ NewLogger,  
// Alias OldLogger w/ reference to NewLogger 
 { provide: OldLogger, useExisting: NewLogger}] 
The useExisting provider key lets you map one token to another. In effect, the first token is an alias for the service associated with the second token, creating two ways to access the same service object. 
You can use this technique to narrow an API through an aliasing interface.[check] 
// Class used as a "narrowing" interface that exposes a minimal logger 
// Other members of the actual implementation are invisible 
export abstract class MinimalLogger { 
  logs: string[]; 
  logInfo: (msg: string) => void; 
} 
providers: [{ provide: MinimalLogger, useExisting: LoggerService }] 
so only the logs and logInfo members are visible in a TypeScript-aware editor. 
Value providers 
Sometimes it's easier to provide a ready-made object rather than ask the injector to create it from a class. To inject an object you have already created, configure the injector with the useValue option. 
you can also inject the configuration object into any constructor that needs it, with the help of an @Inject() parameter decorator. 
constructor(@Inject(APP_CONFIG) config: AppConfig) {} 
Factory providers 
Sometimes you need to create a dependent value dynamically, based on information you won't have until run time. 
export let heroServiceProvider = 
  { provide: HeroService, 
    useFactory: heroServiceFactory(2),--> also can pass values to function 
    deps: [Logger, UserService] 
  }; 
The useFactory field tells Angular that the provider is a factory function whose implementation is heroServiceFactory. 
The deps property is an array of provider tokens. The Logger and UserService classes serve as tokens for their own class providers. The injector resolves these tokens and injects the corresponding services into the matching factory function parameters. 
providers: [ heroServiceProvider ] 
Another way to use useFactory 
@Injectable({ providedIn: 'root', useFactory: () => new Service('dependency'), }) 
When a component or service declares a dependency, the class constructor takes that dependency as a parameter. You can tell Angular that the dependency is optional by annotating the constructor parameter with @Optional(). 
constructor(@Optional() private logger: Logger) {} 
When using @Optional(), your code must be prepared for a null value. If you don't register a logger provider anywhere, the injector sets the value of logger to null. 
constructor(@Host() @Optional() private logger: Logger) {} 
The @Host() function decorating the constructor property ensures that you get a reference to the cache service from the parent. Angular throws an error if the parent lacks that service, even if a component higher in the component tree includes it. 
@self 
constructor property ensures that you get new instance of the service and not the reference from the parent. 
@SkipSelf 
constructor property ensures that you get the reference from the parent and not the new instance of the service. 
 
Inject 
A parameter decorator on a dependency parameter of a class constructor that specifies a custom provider of the dependency. 
When @Inject() is not present, the injector uses the type annotation of the parameter as the provider. 
constructor(@Inject('MyEngine') public engine: Engine) {}  
 
Platform injector [check this term] 
When you use providedIn:'root', you are configuring the root injector for the app, which is the injector for AppModule. The actual root of the entire injector hierarchy is a platform injector that is the parent of app-root injectors. This allows multiple apps to share a platform configuration. 
Angular dependency injection is easiest when the provider token is a class that is also the type of the returned dependency object, or service. 
However, a token doesn't have to be a class and even when it is a class, it doesn't have to be the same type as the returned object. 
Break circularities with a forward class reference (forwardRef) 
The order of class declaration matters in TypeScript. You can't refer directly to a class until it's been defined. 
This isn't usually a problem, especially if you adhere to the recommended one class per file rule. But sometimes circular references are unavoidable. You're in a bind when class 'A' refers to class 'B' and 'B' refers to 'A'. One of them has to be defined first. 
The Angular forwardRef() function creates an indirect reference that Angular can resolve later. 
providers: [{ provide: Parent, useExisting: forwardRef(() => AlexComponent) }] 
Tree-shakable providers 
Tree shaking refers to a compiler option that removes code from the final bundle if that code not referenced in an application. When providers are tree-shakable, the Angular compiler removes the associated services from the final output when it determines that they are not used in your application. This significantly reduces the size of your bundles. 
Angular always adds a component instance to its own injector. 
ROUTER 
In a single-page application (SPA), the web page won’t be reloaded, but its parts may change. You’ll want to add navigation to this application so it’ll change the content area of the page (known as the router outlet) based on the user’s actions. The Angular router allows you to configure and implement such navigation without performing a full page reload. 
Every application has one router object, and you need to configure the routes of your app. 
Angular offers two location strategies for implementing client-side navigation: 
HashLocationStrategy— A hash sign (#) is added to the URL, and the URL segment after the hash uniquely identifies the view to be used as a web page fragment. This strategy works with all browsers, including the old ones. 
PathLocationStrategy— This History API–based strategy works only in browsers that support HTML5. This is the default location strategy in Angular. 
@NgModule({...  providers:[{provide: LocationStrategy, useClass: HashLocationStrategy}]  }) 
 
If you want to serve an Angular app on a non-root path, you have to do the following: 
Add the <base> tag to the header of index.html, such as <base href="/mypath">, or use the --base-href option while running ng build. 
Assign a value for the APP_BASE_HREF constant in the root module and use it as the providersvalue. 
 
THE BUILDING BLOCKS OF CLIENT-SIDE NAVIGATION 
 
<base href> 
Most routing applications should add a <base> element to the index.html as the first child in the <head> tag to tell the router how to compose navigation URLs. 
<base href="/"> 
  
Router imports 
The Angular Router is an optional service that presents a particular component view for a given URL. 
It is in its own library package, @angular/router. 
import { RouterModule, Routes } from '@angular/router'; 
  
Configuration 
A routed Angular application has one singleton instance of the Router service. When the browser's URL changes, that router looks for a corresponding Route from which it can determine the component to display. 
  
A router has no routes until you configure it. 
  
const appRoutes: Routes = [ 
  { path: 'crisis-center', component: CrisisListComponent }, 
  { path: 'hero/:id',      component: HeroDetailComponent }] 
  
imports: [ 
    RouterModule.forRoot( 
      appRoutes, 
      { enableTracing: true } // <-- debugging purposes only 
    )] 
     
The order of the routes in the configuration matters and this is by design. The router uses a first-match wins strategy when matching routes, so more specific routes should be placed above less specific routes. 
  
Router outlet 
The RouterOutlet is a directive from the router library that is used like a component. It acts as a placeholder that marks the spot in the template where the router should display the components for that outlet. 
    <router-outlet></router-outlet> 
     
Router links 
    <a routerLink="/crisis-center" routerLinkActive="active">Crisis Center</a> 
The RouterLink directives on the anchor tags give the router control over those elements. 
  
The RouterLinkActive directive toggles css classes for active RouterLink bindings based on the current RouterState. 
  
Router state 
After the end of each successful navigation lifecycle, the router builds a tree of ActivatedRoute objects that make up the current state of the router. You can access the current RouterState from anywhere in the application using the Router service and the routerState property. 
Each ActivatedRoute in the RouterState provides methods to traverse up and down the route tree to get information from parent, child and sibling routes. 
Router events 
During each navigation, the Router emits navigation events through the Router.events property. These events range from when the navigation starts and ends to many points in between. 
 
PASSING DATA TO ROUTES 
The instance of the ActivatedRoute will include the passed parameters, as well as the route’s URL segment and other properties. 
constructor(route: ActivatedRoute) {  
   this.productID = route.snapshot.paramMap.get('id');    }  
The snapshot means “one-time deal,” and this property is used in scenarios when the parameters passed to the route never change. 
 
There are scenarios when the parameter value keeps changing after navigating to a route. In that case, instead of using the snapshot property, you need to subscribe to the ActivatedRoute.paramMapproperty, which will emit a new value. 
route.paramMap.subscribe( 
  params => this.productID = params.get('id')); 
The steps that Angular performed under the hood to render the main page of the application: 
1.  Check the content of each routerLink. 
2.  Concatenate the values specified in the array. If an array item is an expression, evaluate this expression (like productId). Finally, append the value of APP_BASE_HREF in the beginning of the resulting string. 
3.  The RouterLink directive adds the href attribute if this directive is attached to an <a>element; otherwise, it just listens to the click events. 
 
Passing query parameters to a route 
You can use query parameters (the URL segment after the question mark), as in the following URL: http://localhost:4200/products?category=sports. Query parameters aren’t scoped to a particular route, and if you want to pass them while navigating with the routerLink, you can do as follows: 
<a   [routerLink]="['/products']"  
[queryParams]="{category:'sports'}"> 
      Sports products </a> 
Using programatic navigation 
this.router.navigate(['/products'], 
                         {queryParams: {category: 'sports'}}); 
 
CHILD ROUTES 
An Angular application is a tree of components that have parent-child relations. A child component can have its own routes, but all routes are configured outside of any component. 
{path: 'product/:id', component: ProductDetailComponent,    children: [ 
       {path: '',           component: ProductDescriptionComponent},  
{path: 'seller/:id', component: SellerInfoComponent} ]} 
 
GUARDING ROUTES 
Angular offers several guard interfaces that give you a way to mediate navigation to and from a route. 
Guard interfaces: 
CanActivate allows or disallows navigation to a route. 
CanActivateChild mediates navigation to a child route. 
CanDeactivate allows or disallows navigating away from the current route[This guard is quite handy in cases when you want to warn the user that there are some unsaved changes in the view.]. 
Resolve ensures that the required data is retrieved before navigating to a route. 
CanLoad allows or disallows lazy-loading modules. 
 
CanDeactivate 
The CanDeactivate interface uses a parameterized type. 
Export class abc Implements CanDeactivate<ProductComponent> { 
canDeactivate(component: ProductComponent) {} 
} 
 
Resolve 
If you want to make sure that by the time the user navigates to a route some data structures are populated, create a Resolve guard that allows getting the data before the route is activated. A resolver is a class that implements the Resolve interface. The code in its resolve() method loads the required data, and only after the data arrives does the router navigate to the route. 
 
OBSERVABLES 
Angular offers another approach where you can consider any event as an observable stream of data happening over time. 
In Angular applications, you can get direct access to any DOM element using a special class, ElementRef. 
To turn a DOM event into an observable stream: 
1.  Get a reference to the DOM object. 
2.  Create an observable using Observable.fromEvent(), providing the reference to the DOM object and the event you want to subscribe to. 
3.  Subscribe to this observable and handle the events. 
 
It’s great that you can turn any DOM event into an observable, but directly accessing the DOM by using ElementRef is discouraged, because it may present some security vulnerabilities. 
 
One of the advantages of observables over promises is that observables can be cancelled. 
 
 
 
FORMS APIS 
The template-driven API, forms are fully programmed in the component’s template using directives, and the model object is created implicitly by Angular. The template defines the structure of the form, the format of its fields, and the validation rules. Because you’re limited to HTML syntax while defining the form, the template-driven approach suits only simple forms. 
The reactive API, you explicitly create the model object in TypeScript code and then link the HTML template elements to that model’s properties using special directives. You construct the form model object explicitly using the FormControl, FormGroup, and FormArray classes. 
TEMPLATE-DRIVEN FORMS 
Forms directives 
NgForm is the directive that represents the entire form. It’s automatically attached to every <form>element. NgForm implicitly creates an instance of the FormGroup class that represents the model and stores the form’s data. NgForm automatically discovers all child HTML elements marked with the NgModel directive and adds their values to the form model object. 
<form #f="ngForm" (ngSubmit)="onSubmit(f.value)" ></form> 
NgModel directive can be used for two-way data binding. But in the Forms API, NgModel plays a different role: it marks the HTML element that should become a part of the form model.  
NgModel represents a single field on the form. If an HTML element includes ngModel, Angular implicitly creates an instance of the FormControl class that represents the model and stores the fields’ data. 
<input type="text"         name="username" ngModel> 
NgModelGroup represents a part of the form and allows you to group form fields together. Like NgForm, it implicitly creates an instance of the FormGroup class. NgModelGroup creates a nested object inside the object stored in NgForm.value. 
<form #f="ngForm">   
<div ngModelGroup="passwords">  
<input type="text" name="password" ngModel>     <input type="text" name="pconfirm" ngModel>  
</div> 
</form> 

Directive 
Description 
NgForm 
Implicitly created directive that represents the entire form 
ngForm 
Used in templates to bind the template element (for example, <form>) to NgForm, typically assigned to a local template variable 
NgModel 
Implicitly created directive that marks the HTML element to be included in the form model 
ngForm 
Used in templates in form elements (for example, <input>) to be included in the form model 
name 
Used in templates in form elements to specify its name in the form model 
NgModelGroup 
Implicitly created directive that represents a part of the form, for example, password and confirm password fields 
ngModelGroup 
Used in templates to name a part of the form for future reference 
ngSubmit 
Intercepts the HTML form’s submit event 
 
REACTIVE FORMS 
 
You need to perform the following steps: 
1.  Import ReactiveFormsModule in the NgModule() where your component is declared. 
2.  In your TypeScript code, create an instance of the model object FormGroup to store the form’s values. 
3.  Create an HTML form template, adding reactive directives. 
4.  Use the instance of the FormGroup to access the form’s values. 
Adding ReactiveFormsModule to the @NgModule() decorator is a trivial operation. 
Form model 
The form model is a data structure that holds the form’s data. It can be constructed from FormControl, FormGroup, and FormArray classes. 
FormControl is an atomic form unit. Most often, it corresponds to a single <input> element, but it can also represent a more complex UI component like a calendar or a slider. A FormControlinstance stores the current value of the HTML element it corresponds to, the element’s validity status, and whether it’s been modified. 
FormGroup is a collection of FormControl objects and represents either the entire form or its part. FormGroup aggregates the values and validity of each FormControl in the group. If one of the controls in a group is invalid, the entire group becomes invalid. 
When you need to programmatically add (or remove) controls to a form, use FormArray. It’s similar to FormGroup but has a length variable. Whereas FormGroup represents an entire form or a fixed subset of a form’s fields, FormArray usually represents a collection of form controls that can grow or shrink. 
The reactive directives formGroup and formControl bind a DOM element to the model object using property-binding syntax with square brackets. The directives that link a DOM element to a TypeScript model’s properties by name are formGroupName, formControlName, and formArrayName. They can only be used inside the HTML element marked with the formGroupdirective. 
The formGroup directive binds an instance of the FormGroup class that represents the entire form model to a top-level form’s DOM element, usually a <form>. 
The formControl directive is used with individual form controls or single-control forms, when you don’t want to create a form model with FormGroup but still want to use Forms API features like validation and the reactive behavior provided by the FormControl.valueChanges property. 
 

Directive 
 
Description 
Directives for reactive forms 
 
 
  
FormGroup 
A class that represents the entire form or a subform; create its instance in the TypeScript code of the component. Its API is described at https://angular.io/api/forms/FormGroup. 
  
formGroup 
Used in templates to bind the template element (for example, <form>) to the explicitly created FormGroup; typically it’s assigned to a variable declared in the component. 
  
formGroupName 
Used in templates to bind a group of the template elements (for example, <div>) to the explicitly created FormGroup. 
  
FormControl 
Represents the value, validators, and validity status of an individual form control; create its instance in the TypeScript code of the component. Its API is described at https://angular.io/api/forms/FormControl. 
  
formControl 
Used in a template to bind an individual HTML element to the instance of FormControl. 
  
formControlName 
Used in templates in form elements to link an individual FormControl instance to an HTML element. 
  
FormArray 
Allows you to create a group of form controls dynamically and use the array indexes as control names. You create an instance of FormArray in TypeScript code. Its API is described at https://angular.io/api/forms/FormArray. 
  
formArrayName 
Used in a template as a reference to the instance of FormArray. 
Directives for template-driven forms 
 
 
  
NgForm 
Implicitly created directive that represents the entire form; it creates an instance of FormGroup. Its API is described at https://angular.io/api/forms/NgForm. 
  
ngForm 
Used in templates to bind the template element (for example, <form>) to NgForm; typically, it’s assigned to a local template variable. 
  
NgModel 
Implicitly created directive that marks the HTML element to be included in the form model. Its API is described at https://angular.io/api/forms/NgModel. 
  
ngModel 
Used in templates in form elements (for example, <input>) to be included in the form model. 
  
name 
Used in templates in form elements to specify its name in the form model. 
  
NgModelGroup 
Implicitly created directive that represents a part of the form, such as password and confirm password fields. Its API is described at https://angular.io/api/forms/NgModelGroup. 
  
ngModelGroup 
Used in templates to name a part of the form for future reference. 
Both template-driven and reactive forms 
 
 
  
ngSubmit 
Intercepts the HTML form’s submit event. 
Note that the name of any directive used in the component template starts with a lowercase letter. 
The Angular Forms API offers several functions for updating a form model including reset(), setValue(), and patchValue().  
The reset() function reinitializes the form model and resets the flags on the model, like touched, dirty, and others.  
The setValue() function is used for updating all values in a form model. The patchValue() function is used when you need to update the selected properties of a form model. 
USING FORMBUILDER 
The injectable service FormBuilder simplifies the creation of form models. It doesn’t provide any unique features compared to the direct use of the FormControl, FormGroup, and FormArray classes, but its API is terser and saves you from the repetitive instantiation of objects. 
Constructor(private fb: FormBuilder) { 
this.formgroup = this.fb.group({name: 'abc', email: 'test@abc'}); 
} 
USING BUILT-IN VALIDATORS 
The Angular Forms API includes the Validators class, with static functions such as required(), minLength(), maxLength(), pattern(), email(), and others. These built-in validators can be used in templates by specifying the directives required, minLength, maxLength, pattern, and email, respectively. 
Validators are functions that conform to the interface in the following listing 
interface ValidatorFn { 
  (c: AbstractControl): ValidationErrors | null; 
} 
If a validator function returns null, that means no errors. Otherwise, it’ll return a ValidationErrors object of type {[key: string]: any}, where the property names (error names) are strings, and values (error descriptions) can be of any type. 
 
The ValidationErrors object 
The error returned by a validator is represented by a JavaScript object that has a property whose name briefly describes an error. The property value can be of any type and may provide additional error details. 

Validator 
Description 
min 
A value can’t be less than the specified number; it can be used only with reactive forms. 
max 
A value can’t be greater than the specified number; it can be used only with reactive forms. 
required 
The form control must have a non-empty value. 
requiredTrue 
The form control must have the value true. 
email 
The form control value must be a valid email. 
minLength 
The form control must have a value of a minimum length. 
maxLength 
The form control can’t have more than the specified number of characters. 
pattern 
The form control’s value must match the specified regular expression. 
When you attach a validator, you can specify when validation should start. The updateOn property can take one of these values: 
change— This is the default mode, with validators checking a value as soon as it changes. You saw this behavior in the previous section when validating a phone number. 
blur— Checks validity of a value when the control loses focus. 
submit— Checks validity when the user submits the form. 
 
CUSTOM VALIDATORS IN REACTIVE FORMS 
You need to declare a function that accepts an instance of one of the control types—FormControl, FormGroup, or FormArray—and returns the ValidationErrors object or null. 
You can validate a group of form controls by attaching validator functions to a FormGroup instead of an individual FormControl. 
 
Using the reactive Forms API, you can change the validators attached to a form or one of its controls during runtime. 
You can do that using the setValidators() and updateValueAndValidity() functions. 
 
ASYNCHRONOUS VALIDATORS 
Asynchronous validators can be used to check form values by making requests to a remote server. Like synchronous validators, async validators are functions. The main difference is that async validators should return either an Observable or a Promise object. 
CUSTOM VALIDATORS IN TEMPLATE-DRIVEN FORMS 
With template-driven forms, you can use only directives to specify validators, so wrapping validator functions into directives is required. 
 
@Directive({  
selector: '[ssn]', 
providers: [{           provide: NG_VALIDATORS,            useValue: ssnValidator,           multi: true  }]}) 
class SsnValidatorDirective {} 
You register the validator function using the  predefined NG_VALIDATORS Angular token. This token is, in turn, injected by the NgModel directive, and NgModel gets the list of all validators attached to the HTML element. Then, NgModel passes validators to the FormControl instance it implicitly creates. The same mechanism is responsible for running validators; directives are just a different way to configure them. The multi property lets you associate multiple values with the same token. When the token is injected into the NgModel directive, NgModel gets a list of values instead of a single value. This enables you to pass multiple validators. 
 
 
HTTP module 
Modern browsers support two different APIs for making HTTP requests: the XMLHttpRequest interface and the fetch() API. 
The HttpClient in @angular/common/http offers a simplified client HTTP API for Angular applications that rests on the XMLHttpRequest interface exposed by browsers. Additional benefits of HttpClient include testability features, typed request and response objects, request and response interception, Observable apis, and streamlined error handling. 
How to use HttpClient 
1) import HttpClientModule into the AppModule 
2) inject the HttpClient into an application class 
HTTP GET 
Constructs an observable that, when subscribed, causes the configured GET request to execute on the server. 
By default the response is in JSON format.  
To change response, pass required format to get method. {responseType: ‘text’} 
To get response header and body pass {observe: ‘response’} to the method. 
HTTP Request Parameters 
The HTTP GET can also receive parameters, that correspond to the parameters in the HTTP url. 
build the HTTPParams object by chaining successive set() methods. This is because HTTPParams is immutable, and its API methods do not cause object mutation. 
params = new HttpParams() 
    .set('orderBy', '"$key"') 
    .set('limitToFirst', "1"); 
 
Equivalent fromString HTTP Parameters Syntax 
params = new HttpParams({ 
  fromString: 'orderBy="$key"&limitToFirst=1'  }); 
 
Equivalent request() API 
The GET calls that we saw above can all be rewritten in a more generic API, that also supports the other PUT, POST, DELETE methods. 
this.http 
    .request( 
        "GET", url, params); 
 
HTTP Headers 
If we want to add custom HTTP Headers to our HTTP request. 
headers = new HttpHeaders() 
            .set("X-CustomHeader", "custom header value"); 
 
PUT ==> methods typically used for data modification. PUT call         will replace the whole content of the course path with         a new object, even though we usually only want to               modify a couple of properties. 
PATCH ==> instead of providing a completely new version of a             resource, what we want to do is to just update a               single property. 
DELETE ==> logical delete of some data 
POST ==> Operation that we are trying to do does not fit the            description of any of the methods above (GET, PUT,              PATCH, DELETE), then we can use the HTTP wildcard              modification operation: POST. 
This operation is typically used to add new data to        the database 
 
Getting error details 
Two types of errors can occur. The server backend might reject the request, returning an HTTP response with a status code such as 404 or 500. These are error responses. 
Or something could go wrong on the client-side such as a network error that prevents the request from completing successfully or an exception thrown in an RxJS operator. These errors produce JavaScript ErrorEvent objects. 
The HttpClient captures both kinds of errors in its HttpErrorResponse and you can inspect that response to figure out what really happened. 
 
RxJs 
Reactive programming focuses on propagating changes without our having to explicitly specify how the propagation happens. This allows us to state what our code should do, without having to code every step to do it. 
RxJS is a JavaScript implementation of the Reactive Extensions, or Rx. 
An Observable represents a stream of data. 
[If an action has impact outside of the scope where it happens, we call this a side effect. Changing a variable external to our function, printing to the console, or updating a value in a database are examples of side effects.] 
An Observable provides us with a stream of values that we can manipulate as a whole instead of handling a single isolated event each time. 
The Observable sequence, or simply Observable is central to the Rx pattern. An Observable emits its values in order—like an iterator—but instead of its consumers requesting the next value, the Observable “pushes” values to consumers as they become available. 
RxJS is push-based, so the source of events (the Observable) will push new values to the consumer (the Subscriber), without the consumer requesting the next value. 
An Observable doesn’t start streaming items until it has at least one Observer subscribed to it. 
Like iterators, an Observable can signal when the sequence is completed. 
Using Observables, we can declare how to react to the sequence of elements, instead of reacting to individual items. 
Creating Observables 
const observable = Observable.create(observer => { 
       observer.next( "Simon" ); 
       observer.next( "Jen" ); 
       observer.complete(); // We are done  
     }); 
Whenever an event happens in an Observable, it calls the related method in all of its Subscribers. Subscribers have to implement the Observer interface. 
The Observer interface contains three methods: next, complete, and error. 
The create method in the Rx.Subscriber class takes functions for the next, complete, and error cases and returns a Subscriber instance. 
const subscriber = Subscriber.create( 
       value => console.log( `Next: ${value} ` ), 
       error => console.log( `Error: ${error} ` ), 
       () => console.log( "Completed" ) 
     ); 
In RxJS, methods that transform or query sequences are called operators. Operators are found in the static Rx.Observable object and in Observable instances. 
Rx.Observable 
From 
from converts various other objects and data types into Observables. 
OF 
Each argument becomes a next notification. 
FromEvent 
Creates an Observable from DOM events, or Node.js EventEmitter events or others. 
fromEvent accepts as a first argument event target, which is an object with methods for registering event handler functions. As a second argument it takes string that indicates type of event we want to listen for. 
Range 
Creates an Observable that emits a sequence of numbers within a specified range. 
Interval 
Timer 
Creates an Observable that starts emitting after an dueTime and emits ever increasing numbers after each period of time thereafter. 
 
OPERATORS 
MAP 
map is probably the most used operator. It takes an Observable and a function and applies that function to each of the values in the source Observable. It returns a new Observable with the transformed values. 
It could be that the function we pass to map does some asynchronous computation to transform the value. In that case, map would not work as expected. For these cases, it would be better to use flatMap . 
FILTER 
REDUCE 
reduce (also known as fold) takes an Observable and returns a new one that always contains a single item, which is the result of applying a function over each element. 
FLATMAP 
What can you do if you have an Observable whose emitted items are more Observables? 
The flatMap operator takes an Observable A whose elements are also Observables, and returns an Observable with the flattened values of A’s child Observables. 
 
EXPLICIT CANCELLATION 
Observables themselves don’t have a method to get canceled. Instead, whenever we subscribe to an Observable we get a Subscription. We can then call the methodunsubscribe on the Subscription, and it will stop receiving notifications from the Observable. 
Handling Errors 
The default behavior is that whenever an error happens, the Observable stops emitting items, and complete is not called. 
CATCHING ERRORS 
Observable instances have the catchError operator, which allows us to react to an error in the Observable and continue with another Observable. 
catchError takes either an Observable or a function that receives the error as a parameter and returns another Observable or throws error. 
catchError is useful for reacting to errors in a sequence, and it behaves much like the traditional try/catch block. 
 
RxJS’s Subject Class 
A Subject is a type that implements both Observer and Observable types. As an Observer, it can subscribe to Observables, and as an Observable it can produce values and have Observers subscribe to it. 
An RxJS Subject is a special type of Observable that allows values to be multicasted to many Observers. While plain Observables are unicast (each subscribed Observer owns an independent execution of the Observable), Subjects are multicast. 
A Subject is like an Observable, but can multicast to many Observers. Subjects are like EventEmitters: they maintain a registry of many listeners. 
subject = new Subject(); 
subject.subscribe({ 
next: (v) => console.log(`observerA: ${v}`) 
}); 
subject.next(1); 
subject.next(2); 
subject.complete(); 
ReplaySubject 
A variant of Subject that "replays" or emits old values to new subscribers. It buffers a set number of values and will emit those values immediately to any new subscribers in addition to emitting new values to existing subscribers. 
BehaviorSubject 
A variant of Subject that requires an initial value and emits its current value whenever it is subscribed to. 
Similary to ReplaySubject(1) 
AsyncSubject 
A variant of Subject that only emits a value when it completes. It will emit its latest value to all its observers on completion. 
 
