[HOME](home.md)  

## Basics
### Install & Updates
To install a specific version globally, use
> npm uninstall -g @angular/cli  
> npm install -g @angular/cli@7.1.0  

Run **ng update @angular/core@17 @angular/cli@17** to update your application to Angular v17.  
You can't run ng update to update Angular applications more than one major version at a time.


## Tips

### @schematics
You can set defaults by modifying schema.json file under node_modules\@schematics\angular\component
For example to set changeDetection to be OnPush instead of Default or skip-tests to be true by default or do this:
> ng config schematics.@schematics/angular.component.changeDetection OnPush

### ng new
https://docs.angular.lat/cli/new  
Use --minimal for no testing framework, --style=css to set CSS type, 
> ng new simpleapp --skip-tests --style=css --ssr=no --routing=no

To create an empty workspace with a different name so you can create applications later (ng g application), use:   
> ng new --no-create-application

## routing
<pre>
const routes: Routes = [
  { path: 'first-component', component: FirstComponent },
  { path: 'second-component', component: SecondComponent },
];
</pre>

### !
The exclamation point is called the **non-null assertion** operator and it tells the TypeScript compiler that the value of this property won't be null or undefined.


<pre>
export class HousingLocationComponent {
  @Input() housingLocation!: HousingLocation;
}
</pre>

### @Input
To provide an input to your component from parent use input directive. Create an ingerface for object to be passed and define it with a non-null assertion operator ! as it will be passed in.

You can now mark component and directive inputs as required:  
<pre>
export class ColorPicker {
  @Input({ required: true }) defaultColor: string;
}
</pre>
If a template includes a component without specifying all of its required inputs, Angular reports an error at build time.  

### NgIf
Use it to guard against Null values. By default, NgIf prevents display of an element bound to a null value. To use NgIf to guard a div element, add *ngIf="yourProperty" to the div.



## Service
### @Injectable()   
In Angular, a class with the @Injectable() decorator that encapsulates non-UI logic and code that can be reused across an application. Angular distinguishes components from services to increase modularity and reusability.   

The @Injectable() metadata allows the service class to be used with the dependency injection mechanism. The injectable class is instantiated by a provider. Injectors maintain lists of providers and use them to provide service instances when they are required by components or other services.   
The @Injectable() decorator specifies that Angular can use that class in the DI system. The metadata, providedIn: 'root', means that the HeroService is provided throughout the application.   

<pre>
import { Injectable } from '@angular/core';
@Injectable({
  providedIn: 'root',
})
export class HeroService {}
</pre>

To inject a service as a dependency into a component, you can use the component's constructor() and supply a constructor argument with the dependency type.  


Do provide a service with the application root injector in the @Injectable decorator of the service.
<pre>
import {Injectable} from '@angular/core';
@Injectable({
  providedIn: 'root',
})
export class Service {}
</pre>

When you provide the service to a root injector, that instance of the service is shared and available in every class that needs the service. This is ideal when a service is sharing methods or state.   

When two different components need different instances of a service, it would be better to provide the service at the component level that needs the new and separate instance.  

Do use the @Injectable() class decorator instead of the @Inject parameter decorator when using types as tokens for the dependencies of a service.


<pre>
/* avoid */
export class HeroArena {
  constructor(
    @Inject(HeroService) private heroService: HeroService,
    @Inject(HttpClient) private http: HttpClient,
  ) {}
}

Do this instead:

@Injectable()
export class HeroArena {
  constructor(
    private heroService: HeroService,
    private http: HttpClient,
  ) {}
...
}
</pre>

### Issues with Angular   
Change detection is highly optimized and performant, but it can still cause slowdowns if the application runs it too frequently.

Change Detection  
If Angular handles an event within a component without OnPush strategy, the framework executes change detection on the entire component tree. Angular will skip descendant component subtrees with roots using OnPush, which have not received new inputs.
You can change it from default to OnPush this way:
<pre>
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush,
})
</pre>

Remedies (https://angular.io/guide/change-detection)  
1.To fix, you use Angular Developer Tool to profile change detections. You can find redundant change detections and remedy by moving them out of zone   

2.You can break a component into two sibling component if changes in one is causing change detection in other one that is slow (search by name on top of a list of people that have compution for each person). By breaking up to two component, changes in one will not cause change detection in the other one.  

### Directives
Components  
Structural directives  
Attribute DIRECTIVES	
- NgClass 	  Adds and removes a set of CSS classes.
- NgStyle 	  Adds and removes a set of HTML styles.
- NgModel   	Adds two-way data binding to an HTML form element.



### ng-container
The <ng-container> allows us to use structural directives without any extra element, making sure that the only DOM changes being applied are those dictated by the directives themselves.  

Multiple structural directives cannot be used on the same element; if you need to take advantage of more than one structural directive, it is advised to use an <ng-container> per structural directive.  

The most common scenario is with *ngIf and *ngFor.  


### Pipes  

Do not add filtering and sorting logic to pipes   
Style 04-13   
Avoid adding filtering or sorting logic into custom pipes.  

Do pre-compute the filtering and sorting logic in components or services before binding the model in templates.   

Why?
Filtering and especially sorting are expensive operations. As Angular can call pipe methods many times per second, sorting and filtering operations can degrade the user experience severely for even moderately-sized lists.   

### ngModel
This [(ngModel)] syntax can only set a data-bound property.   

To customize your configuration, write the expanded form, which separates the property and event binding. Use property binding to set the property and event binding to respond to changes.    

### Life Cycle Hooks  
Respond to events in the lifecycle of a component or directive by implementing one or more of the lifecycle hook interfaces in the Angular core library. The hooks give you the opportunity to act on a component or directive instance at the appropriate moment, as Angular creates, updates, or destroys that instance.  

Each interface defines the prototype for a single hook method, whose name is the interface name prefixed with ng. For example, the OnInit interface has a hook method named ngOnInit().   

ngOnInit()  
An ngOnInit() is a good place for a component to fetch its initial data.  

ngOnDestroy()   
This is the place to free resources that won't be garbage-collected automatically. You risk memory leaks if you neglect to do so.   

- Unsubscribe from Observables and DOM events  
- Stop interval timers
- Unregister all callbacks that the directive registered with global or application services
- The ngOnDestroy() method is also the time to notify another part of the application that the component is going away.

### Conventions
The vast majority of components should use a custom element name as their selector. All custom element names should include a hyphen. 

@Component({
  selector: '[dropzone]:not(textarea)',
  ...
})
export class DropZone { }


### Debugging

Is there any difference between View Page Source when you right click in Chrome and source shown in Developer Tools?

Developer tools shows source coud after js has modified it. View Page Source shows original html from server.

 ## Interview Questions
 ### What is the difference between ContentChild and ViewChild?   
 ### What is it used to store the return stuff from observable in a service?   
 ### What are the interceptors?   
 ### What is router-outlet?   


 ### What is the difference between code in constructor and in ngOnInit()?   
  Mostly we use ngOnInit for all the initialization/declaration and avoid stuff to work in the constructor. The constructor should only be used to initialize class members but shouldn't do actual "work".  

So you should use constructor() to setup Dependency Injection and not much else. ngOnInit() is better place to "start" - it's where/when components' bindings are resolved.   
Important to note that @Input values are not accessible in the constructor.  

Components are easier to test and debug when their constructors are simple, and all real work (especially calling a remote server) is handled in a separate method.


## Resources
- Read Angular Style Guide https://angular.dev/style-guide   
- Check version compatibility https://angular.dev/reference/versions  
- Check update issues:  https://angular.dev/update-guide  

Install Angualr Snippets (Version 18) by John Papa at https://marketplace.visualstudio.com/items?itemName=johnpapa.Angular2   


Udemy Angular-The Complete Guide  
Maximilian   

Advanced Angular Forms    decodedfrontend.io  
Get 15%-OFF for Your next Course 💥  
Save and use THANKS_FOR_PURCHASE code for next purchase  


Angular Reactive Forms by Jim Cooper  - Pluralsight  
Angular CLI by John Papa @plural  
Routing https://www.youtube.com/watch?v=tUCa3JcFILI  

Advance Form nested  
https://youtu.be/o74WSoJxGPI?si=l-lO0dj4ElDfjxrs  

https://blog.angular-university.io/angular-advanced-course/   

To update angular version go to https://update.angular.io/  




How do you dynamically load components?
Example of pipes
Example of Guards   
Hierachy of injectable calling

Factory pattern C#