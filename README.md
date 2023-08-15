# SimpleWebServer

Simple Web Server for C#

[![NuGet](https://img.shields.io/nuget/v/SimpleWebServer.svg?label=NuGet)](https://nuget.org/packages/SimpleWebServer)

**Contact me from discord for additional help & suggestions**

**Discord: borasy**

## Simple Demo

Program.cs:

```csharp
using System;

using SimpleWebServer;
using SimpleWebServer.Extensions;

namespace ConsoleApp1
{
    internal class Program
    {
        static async Task Main(string[] args)
        {
            WebServer server = new WebServer("http://localhost:8080/");

            server.AddController<Controller1>();

            server.Start();

            Console.WriteLine("Started Listening");

            await Task.Delay(-1);
        }
    }
}
```

Controller1.cs:

```csharp
using System;
using System.Net;

using SimpleWebServer.Attributes;
using SimpleWebServer.Extensions;

namespace ConsoleApp1
{
    internal class Controller1
    {

        [WebPath("/")]
        public async void Main(HttpListenerContext ctx) => ctx.Redirect("/index");

        [WebPath("/index")]
        public async void Index(HttpListenerContext ctx)
        {
            string? name = ctx.Request.QueryString["name"];

            string response = $"Hello, {name ?? "World"}";

            bool success = ctx.CreateHTMLResponse(response);

            Console.WriteLine(success ? "Successfully created response" : "An error occured while creating response");
        }
    }
}
```

## Detailed Documentation

#### WebServer

<br>

*Creating The Server:*

```csharp
WebServer server = new WebServer(string RootURL);
```

* ***string RootURL:*** The host name with scheme, subdomain *(optional)* and port *(optional)* *[Examples: "http://localhost:8080/", "https://test.testdomain.com/"]*

<br>

*Starting The Server:*

```csharp
server.Start();
```

<br>

*Stopping The Server:*

```csharp
server.Stop();
```

<br>

*Adding Controller Class To The Server:*

```csharp
server.AddController<T>(PreExecuteControllerMethod PreExecute = null);
```

* ***T:*** The Controller Class
* ***PreExecuteControllerMethod PreExecute (Optional):*** PreExecute method for this controller (This method will be executed before the controller methods to handle bulk authentication/authorization. If it returns true, the specified controller method will be executed; otherwise, the specified controller method won't be executed)
  <br>
* ***RETURNS: Added endpoint count (int)***
  <br>

##### WebServer Events

* `WebServer.On404NotFound`: This event that will be invoked when a user sends a request to an undefined path. Use this event to create custom 405 response.
  <br>
* `WebServer.On405MethodNotAllolwed`: The event that will be invoked when a user sends a request with an invalid http method. Use this event to create custom 405 response.
  <br>

##### WebServer Delegates

* `WebServer.ControllerMethod`:
  The method that will be executed when a user sends a request to the specified path.
  ***INPUT PARAMETER:*** HttpListenerContext
  <br>
* `WebServer.PreExecuteControllerMethod`: This method will be executed before the controller methods to handle bulk authentication/authorization. If it returns true, the specified controller method will be executed; otherwise, the specified controller method won't be executed.
  ***INPUT PARAMETER:*** HttpListenerContext
  ***RETURNS:*** BOOL
