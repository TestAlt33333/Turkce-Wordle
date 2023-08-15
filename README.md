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

#### Creation of the WebServer

```csharp
WebServer server = new WebServer(string URLPrefix);
```

> * **string URLPrefix:** The host name with scheme, subdomain *(optional)* and port *(optional)* *[Examples: "http://localhost:8080/", "https://test.testdomain.com/"]*
