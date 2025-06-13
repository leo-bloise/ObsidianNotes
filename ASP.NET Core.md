ASP.NET Core is a C# framework to develop web applications. ASP.NET Core uses the host model to configure and launch the web application.
## Host

Host objects are objects that encapsulates and manages app's resources and lifetime like:
- Logging
- Configuration
- Dependency Injection.
- `IHostedServices` implementations.

When talking about Web applications, one of the `IHostedServices` implementations is the web server. Besides, it uses the `Builder` pattern to allow customization of the host object itself. 

Generally, the host is built, configured and run by the class `Program`, which contains the startup code.  The most generic way of creating a web application with `Host` is through the code below:

```c#
IHost app = Host.CreateDefaultBuilder(args)  
    .ConfigureWebHostDefaults(webBuilder =>  
    {  
	    webBuilder.UseStartup<Startup>();        
    }).Build();  
app.Run();
```

`CreateDefaultBuilder` provides common configuration and other services for the app itself. Besides, `ConfigureWebHostDefaults` provides `Kestrel` as the default web server and other configurations.

If you want to create web applications without a class called `Startup`, you can use the `WebApplication` to create an `WebApplicationBuilder` and it'll build to you a `WebApplication` instance.

> ASP.NET Core is a modular framework, so the building process of an ASP.NET Core host involves adding services to the host and configuring them. We have a bunch of methods that adds a set of services to perform a specific task and we need to configuring them after building the app. Each kind of APP has its own set of capabilities needed and you must go through the documentation to learn them.
## Controller-Based APIs

Controller-Based APIs are HTTP APIs that uses the MVC pattern to handle the requests. Every controller must inherit from `ControllerBase`. It provides a set of methods and properties to handle HTTP requests. Besides, you must configure the controllers and map them like this:

```c#
WebApplicationBuilder builder = WebApplication.CreateBuilder(args);
builder.Services.AddControllers();
var app = builder.Build();
app.MapControllers();
app.Run();
```

`AddControllers` add services related to controllers, such as Model Binding and validation, which involves formatting and stuff, but it does not add support for `views`, for example. In this case, we'll be using attribute routing. 

Attribute routing allows us to specify the route to something through Attributes. They can be specified using the `Http VERB` or only the URL route:

```c#
using Microsoft.AspNetCore.Mvc;  
  
namespace TaskManager.Controllers;  
  
[ApiController]  
public class UsersController : ControllerBase  
{  
    [Route("/users")]  
    public string Get()  
    {        
	    return "Hello World";  
    }
}
```

In this case, you can reach the `Get` method (called `Action`) through any http VERB and using, for example: `GET https://example.com/users`

If you want to specify the HTTP VERB, you can filter also by the HTTP Verb like this:

```c#
using Microsoft.AspNetCore.Mvc;  
  
namespace TaskManager.Controllers;  
  
[ApiController]  
[Route("[controller]")]
public class UsersController : ControllerBase  
{  
    [HttpGet()]  
    public string Get()  
    {        
	    return "Hello World";  
    }
}
```

In this case, we have the following happening:
- The route is defined with `Route` attribute on top of the controller class and it uses token replacement for defining the route. In this case, it would be `users`. You have other options for this, check [this link](https://learn.microsoft.com/en-us/aspnet/core/mvc/controllers/routing?view=aspnetcore-9.0#token-replacement-in-route-templates-controller-action-area).
- The route is combined with `HttpGet`. It does not declare any kind of route, so it'd be `\` and, as it's an `HttpGet`, it would only accept `GET requests`.
Besides, it uses the `[ApiController]` attribute, which defines:
- Attribute Routing as a requirement, it does not allow conventional routing.
- Generates an automatic 400 for invalid models after Model Binding.
	- The default body for 400 is `ValidationProblemDetails`.
	- We can override the object, follow this [link](https://learn.microsoft.com/en-us/aspnet/core/web-api/?view=aspnetcore-9.0#automatic-http-400-responses)
	- 