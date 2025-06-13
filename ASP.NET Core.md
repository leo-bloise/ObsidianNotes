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

## Controller-Based APIs

Controller-Based APIs are HTTP APIs that uses the MVC pattern to handle the requests. Every controller must inherit from `ControllerBase`. It provides a set of methods and properties to handle HTTP requests.

