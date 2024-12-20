
### Resources
- Check version compatibility https://angular.dev/reference/versions  
- Check update issues:  https://angular.dev/update-guide  


Udemy Angular-The Complete Guide  
Maximilian   

Advanced Angular Forms    decodedfrontend.io  
Get 15%-OFF for Your next Course 💥  
Save and use THANKS_FOR_PURCHASE code for next purchase  


https://angular.io/guide/styleguide   

Angular Reactive Forms by Jim Cooper  - Pluralsight  
Angular CLI by John Papa @plural  
Routing https://www.youtube.com/watch?v=tUCa3JcFILI  

Advance Form nested  
https://youtu.be/o74WSoJxGPI?si=l-lO0dj4ElDfjxrs  

https://blog.angular-university.io/angular-advanced-course/   

To update angular version go to https://update.angular.io/  

### Tips
- The exclamation point is called the **non-null assertion** operator and it tells the TypeScript compiler that the value of this property won't be null or undefined.
<pre>
export class HousingLocationComponent {
  @Input() housingLocation!: HousingLocation;
}
</pre>

### Required inputs
You can now mark component and directive inputs as required:  
<pre>
export class ColorPicker {
  @Input({ required: true }) defaultColor: string;
}
</pre>
If a template includes a component without specifying all of its required inputs, Angular reports an error at build time.  

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
Common attribute DIRECTIVES	
- NgClass 	  Adds and removes a set of CSS classes.
- NgStyle 	  Adds and removes a set of HTML styles.
- NgModel   	Adds two-way data binding to an HTML form element.

### service   
In Angular, a class with the @Injectable() decorator that encapsulates non-UI logic and code that can be reused across an application. Angular distinguishes components from services to increase modularity and reusability.   

The @Injectable() metadata allows the service class to be used with the dependency injection mechanism. The injectable class is instantiated by a provider. Injectors maintain lists of providers and use them to provide service instances when they are required by components or other services.   

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