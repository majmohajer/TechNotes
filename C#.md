[HOME](home.md)  
### Sql performance
Check index usage statistics

## Nullable Annotations (nullable type ? and null-forgiving operator !)
A nullable reference type is noted using the same syntax as nullable value types: a ? is appended to the type of the variable.    
When nullable reference types are enabled, any variable where the ? isn't appended to the type name is a non-nullable reference type. That includes all reference type variables in existing code once you enable this feature. However, any implicitly typed local variables (declared using var) are nullable reference types.

<br />
Sometimes you must override a warning when you know a variable isn't null, but the compiler determines its null-state is maybe-null. You use the null-forgiving operator ! following a variable name to force the null-state to be not-null. For example, if you know the name variable isn't null but the compiler issues a warning, you can write the following code to override the compiler's analysis:    

> name!.Length;

The runtime behavior of a program making use of nullable annotations is the same if all the nullable annotations, (? and !), are removed. Their only purpose is expressing design intent and providing information for null state analysis.

### Delegates
There are three types:  Action, Funcs and Predicate.   
Action delegate defines a method that can be called on arguments but does not return a result

Func– A Func delegate defines a method that can be called on arguments and returns a result.   
Func <int, string, bool> myDel is same as delegate bool myDel(int a, string b);   

Predicate– Defines a method that can be called on arguments and always returns the bool.
Predicate<string> myDel is same as delegate bool myDel(string s);

### Desing Patterns
**Mediator pattern** uses an intermediary class to communicate between classes of similar types. An example is different airplanes using a Control Tower to talk to each other.

### Any issue in regard to calling ToList() in a Linq query too early?
.ToList() makes a query to materialize. Lazy loading or deferred execution are concerns to have in mind when deciding when to make that call.

I was told by interviewer that sometimes the where clause does not work before the ToList call and it causes performance issue.

## Web API

### What is attribute routing?
ASP.NET MVC5 and WEB API 2 supports a new type of routing, called attribute routing. In this routing, attributes are used to define routes. Attribute routing provides you more control over the URIs by defining routes directly on actions and controllers in your ASP.NET MVC application and WEB API

### AllowAnonymous with Authorize
 [AllowAnonymous] bypasses authorization statements. If you combine [AllowAnonymous] and an [Authorize] attribute, the [Authorize] attributes are ignored. For example if you apply [AllowAnonymous] at the controller level:

Any authorization requirements from [Authorize] attributes on the same controller or action methods on the controller are ignored.
Authentication middleware is not short-circuited but doesn't need to succeed.

### How you limit access to a controller based on specific user criteria?
You can create a Policy:
Services.AddAuthorization(options =>
    {
        options.AddPolicy("MustHaveTheseAttributes", policy =>
        {

        }
        );
    });

then specify the policy name with Authorize attribute for the controller
[Authorize(Policy = "MustHaveTheseAttributes") ]

### Action return types
ASP.NET Core provides the following options for web API controller action return types:

In Asp.Net MVC 5, ActionResult is an abstract class that drives from Object and represents the result of an action method.
Derived Types:
System.Web.Mvc.ContentResult
System.Web.Mvc.EmptyResult
System.Web.Mvc.FileResult
System.Web.Mvc.HttpStatusCodeResult
System.Web.Mvc.JavaScriptResult
System.Web.Mvc.JsonResult
System.Web.Mvc.RedirectResult
System.Web.Mvc.RedirectToRouteResult
System.Web.Mvc.ViewResultBase

In Asp.Net Core, ActionResult is an abstract class that is the default implementation of IActionResult and its ExecuteResult(ActionContext) method is called by MVC.
Derived Types:
Microsoft.AspNetCore.Mvc.ChallengeResult
Microsoft.AspNetCore.Mvc.ContentResult
Microsoft.AspNetCore.Mvc.EmptyResult
Microsoft.AspNetCore.Mvc.FileResult
Microsoft.AspNetCore.Mvc.ForbidResult
Microsoft.AspNetCore.Mvc.JsonResult
Microsoft.AspNetCore.Mvc.LocalRedirectResult
Microsoft.AspNetCore.Mvc.ObjectResult
Microsoft.AspNetCore.Mvc.PartialViewResult
Microsoft.AspNetCore.Mvc.RazorPages.PageResult
Microsoft.AspNetCore.Mvc.RedirectResult
Microsoft.AspNetCore.Mvc.RedirectToActionResult
Microsoft.AspNetCore.Mvc.RedirectToPageResult
Microsoft.AspNetCore.Mvc.RedirectToRouteResult
Microsoft.AspNetCore.Mvc.SignInResult
Microsoft.AspNetCore.Mvc.SignOutResult
Microsoft.AspNetCore.Mvc.StatusCodeResult
Microsoft.AspNetCore.Mvc.ViewComponentResult
Microsoft.AspNetCore.Mvc.ViewResult



Specific type
IActionResult (Defines ExecuteResultAsync method that executes the result operation of the action method asynchronously. This method is called by MVC to process the result of an action method.)

ActionResult<T>
HttpResults
ViewResult 

### WebAPI Interview
-base class for a web api controller? ControllerBase Class

-What are some type that response can be sent back from a controller?
ASP.NET Core includes the ActionResult<T> return type for web API controller actions. It enables returning a type deriving from ActionResult or return a specific type. ActionResult<T> offers the following benefits over the IActionResult type

The IActionResult return type is appropriate when multiple ActionResult return types are possible in an action like BadRequestResult (400), NotFoundResult (404), and OkObjectResult (200).

Example:
[HttpGet("{id}")]
[ProducesResponseType(StatusCodes.Status200OK, Type = typeof(Product))]
[ProducesResponseType(StatusCodes.Status404NotFound)]
public IActionResult GetById_IActionResult(int id)
{
    var product = _productContext.Products.Find(id);
    return product == null ? NotFound() : Ok(product);
}

The NotFound convenience method is invoked as shorthand for return new NotFoundResult();.
Ok convenience method is invoked as shorthand for return new OkObjectResult(product);

https://learn.microsoft.com/en-us/aspnet/core/web-api/action-return-types?view=aspnetcore-7.0

### How many ways to register a service to be used for dependency injection by asp.net?
Create an interface of abstract class to be injected.

In Program.cs (Startup.cs maybe) ConfigureServices method add one of the following lines:

builder.Services.AddSingleton(Type serviceType) (single instance for application life cycle)
builder.Services.AddTransient (separate instance for each use)
builder.Services.AddScoped<IMyDependency, MyDependency>();  (separate instance for life time of each request)



### Where you handle exceptions in a web api/web service?
You use catch blocks when using HTTPClient and use Catch HttpResponseException but exception like 400 Bad Request do come back in body and not exception is thrown. 
You need to add EnsurceSuccess() to get exception for bad requests thrown.
<pre>
    try
    {
        var response = _httpClient.GetAsync("api/businesses").Result;
        if (!response.IsSuccessStatusCode) return null;

        return response.Content.ReadAsStringAsync().Result;
    }
    catch (Exception e)
    {
        _logger.LogError(e, "Something went wrong while fetching data from external service");
        return null;
    }
</pre>

You can also create an exception filter class that drives from ExceptionFilterAttribute and override OnException(ExceptionContext) or OnExceptionAsync(ExceptionContext) 

You can use the exception filter once the controller action method throws an unhandled exception that is not an HttpResponseException
---------------------------------------------
In Web API 2 you can create a global handler and register it instead of default existing exception handler:
<pre>
public class GlobalExceptionHandler: ExceptionHandler
    {
        public async override TaskHandleAsync(ExceptionHandlerContext context, CancellationTokencancellationToken)
        {
            // Access Exception using context.Exception;
            const string errorMessage = "An unexpected error occured";
            var response = context.Request.CreateResponse(HttpStatusCode.InternalServerError,
                new
                {
                    Message = errorMessage
                });
            response.Headers.Add("X-Error", errorMessage);
            context.Result = new ResponseMessageResult(response);
        }
    }
</pre>

### User-Secrets vs appsettings.json
Run this cmd in project folder:    dotnet user-secrets init

WebApplication.CreateBuilder initializes a new instance of the WebApplicationBuilder class with preconfigured defaults. The initialized WebApplicationBuilder (builder) provides default configuration and calls AddUserSecrets when the EnvironmentName is Development.


dotnet user-secrets set "Auth:JwtSecret" "ThisIsALocalJwtSecretKeyThatIsAtLeast256Bits"

dotnet user-secrets set "Auth:MicroserviceAuthKey" "TempAuthKeyForLocalDevelopment"

dotnet user-secrets set "ApplicationInsights:InstrumentationKey" "0d7e4812-5128-4c21-bf10-5d7fa0222733"


dotnet user-secrets set "Database:PassCode" "asdfsadfsadfsa"

dotnet user-secrets set "ServiceBus:PriceAuditTopic" "topic-priceaudit"
dotnet user-secrets set "ServiceBus:PriceAudit_LogPriceAudit" "sub-priceaudit-logpriceaudit"


Which one overrides the other?

Do you need to first add appsettings.json to configuration builder like this:
var builder = new ConfigurationBuilder()
        .SetBasePath(env.ContentRootPath)
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)



### Can abstract Class have a constructor?
Yes it can and this will be called before the constructor of derived class when instantiating a derived class.
C# always calls the parameterless constructor of the parent class unless you use the base() to call the specific constructor of the parent class.

base()
The base keyword is used to access members of the base class from within a derived class. Use it if you want to:
-Call a method on the base class that has been overridden by another method.
-Specify which base-class constructor should be called when creating instances of the derived class.


### Quick Questions
-Difference between String literals forms: raw, quoted, and verbatim.
Raw string literals can contain arbitrary text without requiring escape sequences and are enclosed in a minimum of three double quotation marks (""")
Quoted string literals are enclosed in double quotation marks (")
Verbatim starts with @ and are enclosed in double quotation marks
The advantage of verbatim strings is that escape sequences aren't processed as in @"c:\Docs\Source\a.txt" 

-Order of initialization in a class?

- How to initialize a property?
public DateTime DateCreated { get; private set; } = DateTime.Now;
 
-String Interpolation
var title = String.Format("{0} ({1})", post.Title, post.Comments.Count); 
var title = $"{post.Title} ({post.Comments.Count})";

-null-coallescing operator 
var count = post?.Tags?.Count ?? 0;

-How use null-conditional operator
var title = post?.Title;  or    var first = posts?[0];

-Difference between overloaded and overridden method?
You cannot override a non-virtual method.

-How use DI in asp.net core? Create a class and then in your code define a private variable of that type and in constructior add a parameter for that class and in constructor set the private variable to passed parameter. In main fucntion of program, then add builder.Services your Class so it can be injected by asp.net

-What does the readonly keyword does?
It marks a variable that cannot be modified but it can be set in constructor.

-How do you call a method on the base class that has been overridden by another method? use base keyword


-The result of x++ is the value of x before the operation and The result of ++x is the value of x after the operation

-What does this constraint mean?    where T : new()  
The type argument must have a public parameterless constructor. When used together with other constraints, the new() constraint must be specified last. 

How do you write an Indexer for EmployeeList class?
public Employee this [int index]  { ... }

 - Where clause vs Join condition (left and inner joins)
- File system vs database pros and cons

-Dispose & Using
-Some properties of a db connection and best practices

Test Knowledge of Passing Reference type parameters.  Write a method to swap to strings. It will  not work without ref keyword. See below:

static void SwapStrings(ref string s1, ref string s2)

     // The string parameter is passed by reference.

     // Any changes on parameters will affect the original variables.

     {

         string temp = s1;

         s1 = s2;

         s2 = temp;

         System.Console.WriteLine("Inside the method: {0} {1}", s1, s2);

     }

Explain Deffered  execution in Linq queries

Explain what is the difference between “visibility:hidden” and “display:none”?

They are both style properties

visibility:hidden: This property hides the element, but it still takes up space in the layout
display:none: It eliminates the element completely from the document. It does not take up any space, even though the HTML for it is still in the source code.