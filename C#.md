### What is attribute routing?
ASP.NET MVC5 and WEB API 2 supports a new type of routing, called attribute routing. In this routing, attributes are used to define routes. Attribute routing provides you more control over the URIs by defining routes directly on actions and controllers in your ASP.NET MVC application and WEB API

### Any issue in regard to calling ToList() in a Linq query too early?
.ToList() makes a query to materialize. Lazy loading or deferred execution are concerns to have in mind when deciding when to make that call.

I was told by interviewer that sometimes the where clause does not work before the ToList call and it causes performance issue.

### 


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

### Delegates
There are three types:  Action, Funcs and Predicate.   
Action delegate defines a method that can be called on arguments but does not return a result

Func– A Func delegate defines a method that can be called on arguments and returns a result.   
Func <int, string, bool> myDel is same as delegate bool myDel(int a, string b);   

Predicate– Defines a method that can be called on arguments and always returns the bool.
Predicate<string> myDel is same as delegate bool myDel(string s);

### Desing Patterns
**Mediator pattern** uses an intermediary class to communicate between classes of similar types. An example is different airplanes using a Control Tower to talk to each other.

### Where you handle exceptions in a web api/web service?
You can create an exception filter class that drives from ExceptionFilterAttribute and override OnException(ExceptionContext) or OnExceptionAsync(ExceptionContext) 

You can use the exception filter once the controller action method throws an unhandled exception that is not an HttpResponseExeption
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