---
title: "Offene Webschnittstelle für .NET (OWIN)"
author: ardalis
description: "Einführung in die Weboberfläche für .NET (OWIN) zu öffnen."
keywords: "ASP.NET Core, offene Webschnittstelle für .NET, OWIN"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 70c4e6bc-a773-4039-96ec-6fe557c9369d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 28bd52edbdb3159642ce523dd63fde9d6001678c
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="e5d8a-104">Einführung in die Weboberfläche für .NET (OWIN) zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-104">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="e5d8a-105">Durch [Steve Smith](http://ardalis.com) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-105">By [Steve Smith](http://ardalis.com) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e5d8a-106">ASP.NET Core unterstützt die offene Webschnittstelle für .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="e5d8a-106">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="e5d8a-107">OWIN kann webapps auf Webservern entkoppelt werden.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-107">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="e5d8a-108">Definiert ein gängiges Verfahren für die Middleware, die in einer Pipeline zur Verarbeitung von Anforderungen und zugeordneten Antworten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-108">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="e5d8a-109">ASP.NET Core-Anwendungen und Middleware können mit OWIN-basierten Anwendungen, Server und Middleware zusammenarbeiten.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-109">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="e5d8a-110">OWIN stellt eine entkoppeln, die zwei Frameworks mit unterschiedlichen Objektmodelle zusammen verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-110">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="e5d8a-111">Die `Microsoft.AspNetCore.Owin` Paket stellt zwei adapterimplementierungen bereit:</span><span class="sxs-lookup"><span data-stu-id="e5d8a-111">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="e5d8a-112">ASP.NET Core zu OWIN</span><span class="sxs-lookup"><span data-stu-id="e5d8a-112">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="e5d8a-113">OWIN zu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e5d8a-113">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="e5d8a-114">Dadurch können ASP.NET Core gehostet werden, zusätzlich zu einem OWIN kompatiblen Server /-Host oder für andere OWIN kompatiblen Komponenten zusätzlich zu ASP.NET Core ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-114">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="e5d8a-115">Hinweis: Mit diesen Adaptern mit Leistungseinbußen kommt.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-115">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="e5d8a-116">Anwendungen mit nur ASP.NET Core-Komponenten sollten nicht die Owin-Paket oder ein Adapter verwenden.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-116">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

[<span data-ttu-id="e5d8a-117">Anzeigen oder Herunterladen von Beispielcode</span><span class="sxs-lookup"><span data-stu-id="e5d8a-117">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="e5d8a-118">OWIN-Middleware in der ASP.NET-Pipeline ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-118">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="e5d8a-119">ASP.NET Core des owin-Unterstützung bereitgestellt wird, als Teil der `Microsoft.AspNetCore.Owin` Paket.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-119">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="e5d8a-120">Sie können zur Installation dieses Pakets OWIN-Unterstützung in Ihr Projekt importieren.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-120">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="e5d8a-121">OWIN-Middleware entspricht der [OWIN-Spezifikation](http://owin.org/spec/spec/owin-1.0.0.html), wofür ein `Func<IDictionary<string, object>, Task>` Schnittstelle und bestimmte Schlüssel festgelegt werden (z. B. `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="e5d8a-121">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="e5d8a-122">Die folgenden einfachen owin-Middleware wird "Hello World" angezeigt:</span><span class="sxs-lookup"><span data-stu-id="e5d8a-122">The following simple OWIN middleware displays "Hello World":</span></span>

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}


```

<span data-ttu-id="e5d8a-123">Die Signatur Beispiel gibt eine `Task` und akzeptiert eine `IDictionary<string, object>` OWIN wie erforderlich.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-123">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="e5d8a-124">Der folgende Code veranschaulicht das Hinzufügen der `OwinHello` Middleware (siehe oben), an der ASP.NET-Pipeline mit der `UseOwin` -Erweiterungsmethode.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-124">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/OwinSample/Startup.cs"} -->

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}


```

<span data-ttu-id="e5d8a-125">Sie können andere Aktionen, erfolgen in der OWIN-Pipeline konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-125">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="e5d8a-126">Antwort-Header sollte nur geändert werden, vor dem ersten Schreiben in den Antwortstream.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-126">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="e5d8a-127">Mehrere Aufrufe `UseOwin` wird abgeraten, zur Verbesserung der Leistung.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-127">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="e5d8a-128">Owin-Komponenten funktionieren am besten, wenn einer Gruppe zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-128">OWIN components will operate best if grouped together.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name=hosting-on-owin></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="e5d8a-129">Verwenden von ASP.NET-Hosting auf einen OWIN-basierten server</span><span class="sxs-lookup"><span data-stu-id="e5d8a-129">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="e5d8a-130">OWIN-basierte Server können ASP.NET-Anwendungen hosten.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-130">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="e5d8a-131">Ein solcher Server ist [Nowin](https://github.com/Bobris/Nowin), einen Webserver .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-131">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="e5d8a-132">Im Beispiel für diesen Artikel haben ich enthalten ein Projekt, das Nowin verweist und verwendet er zum Erstellen einer `IServer` ASP.NET Core selbst hosten.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-132">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

<span data-ttu-id="e5d8a-133">[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]</span><span class="sxs-lookup"><span data-stu-id="e5d8a-133">[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]</span></span>

<span data-ttu-id="e5d8a-134">`IServer`ist eine Schnittstelle, die erfordert eine `Features` Eigenschaft und eine `Start` Methode.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-134">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="e5d8a-135">`Start`ist verantwortlich für das Konfigurieren und starten den Server, der in diesem Fall durch eine Reihe von fluent-API-Aufrufe abgeschlossen ist, die Adressen, die analysiert die IServerAddressesFeature festlegen.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-135">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="e5d8a-136">Beachten Sie, dass die fluent-Konfiguration von der `_builder` Variable gibt an, dass Anforderungen vom behandelt werden die `appFunc` weiter oben in der Methode definiert.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-136">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="e5d8a-137">Dies `Func` für jede Anforderung zur Verarbeitung von eingehenden Anforderungen aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-137">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="e5d8a-138">Wir fügen auch eine `IWebHostBuilder` Erweiterung hinzufügen und Konfigurieren des Servers Nowin zu erleichtern.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-138">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [11], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinSample/NowinWebHostBuilderExtensions.cs"} -->

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

<span data-ttu-id="e5d8a-139">Dabei vorhanden ist, ist erforderlich, führen Sie eine ASP.NET-Anwendung mit diesen benutzerdefinierten Server rufen Sie die Erweiterung in *"Program.cs"*:</span><span class="sxs-lookup"><span data-stu-id="e5d8a-139">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [15], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinSample/Program.cs"} -->

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

<span data-ttu-id="e5d8a-140">Erfahren Sie mehr über ASP.NET [Server](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e5d8a-140">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="e5d8a-141">Führen Sie ASP.NET Core auf einen OWIN-basierten Server und verwenden Sie die WebSockets-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="e5d8a-141">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="e5d8a-142">Ein weiteres Beispiel für Funktionen wie OWIN-basierten Servern genutzt werden, indem Sie ASP.NET Core ist der Zugriff auf Funktionen wie WebSockets.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-142">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="e5d8a-143">Der im vorherigen Beispiel verwendete .NET OWIN-Webserver hat die Unterstützung für WebSockets integriert, die von einer ASP.NET Core-Anwendung genutzt werden können.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-143">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="e5d8a-144">Das folgende Beispiel zeigt eine einfache Web-app, die WebSockets unterstützt und wiederholt wieder alles, was über WebSockets an den Server gesendet.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-144">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [7, 9, 10], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": true, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinWebSockets/Startup.cs"} -->

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

<span data-ttu-id="e5d8a-145">Dies [Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) konfiguriert ist, unter Verwendung des gleichen `NowinServer` wie das vorherige - ist der einzige Unterschied in der Konfiguration der Anwendung in seiner `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-145">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="e5d8a-146">Einen Test mit [einen einfachen Websocket-Client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) Anwendung veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="e5d8a-146">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Web-Socket-Testclient](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="e5d8a-148">OWIN-Umgebung</span><span class="sxs-lookup"><span data-stu-id="e5d8a-148">OWIN environment</span></span>

<span data-ttu-id="e5d8a-149">Erstellen Sie eine OWIN-Umgebung verwenden die `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-149">You can construct a OWIN environment using the `HttpContext`.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="e5d8a-150">OWIN-Schlüssel</span><span class="sxs-lookup"><span data-stu-id="e5d8a-150">OWIN keys</span></span>

<span data-ttu-id="e5d8a-151">OWIN richtet sich nach einem `IDictionary<string,object>` Objekt zur Übermittlung von Informationen in der gesamten einen HTTP-Anforderung/Antwort-Austausch.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-151">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="e5d8a-152">ASP.NET Core implementiert die unten aufgeführten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="e5d8a-152">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="e5d8a-153">Finden Sie unter der [primären-Spezifikation, Erweiterungen](http://owin.org/#spec), und [owin-Schlüssel Richtlinien und allgemeine Schlüssel](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="e5d8a-153">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="e5d8a-154">Anfordern von Daten (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-154">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="e5d8a-155">Key</span><span class="sxs-lookup"><span data-stu-id="e5d8a-155">Key</span></span>               | <span data-ttu-id="e5d8a-156">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-156">Value (type)</span></span> | <span data-ttu-id="e5d8a-157">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e5d8a-157">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e5d8a-158">Owin. RequestScheme</span><span class="sxs-lookup"><span data-stu-id="e5d8a-158">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="e5d8a-159">Owin. RequestMethod</span><span class="sxs-lookup"><span data-stu-id="e5d8a-159">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="e5d8a-160">Owin. RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="e5d8a-160">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="e5d8a-161">Owin. RequestPath</span><span class="sxs-lookup"><span data-stu-id="e5d8a-161">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="e5d8a-162">Owin. RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="e5d8a-162">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="e5d8a-163">Owin. RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="e5d8a-163">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="e5d8a-164">Owin. RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="e5d8a-164">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="e5d8a-165">Owin. RequestBody</span><span class="sxs-lookup"><span data-stu-id="e5d8a-165">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="e5d8a-166">Anfordern von Daten (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-166">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="e5d8a-167">Key</span><span class="sxs-lookup"><span data-stu-id="e5d8a-167">Key</span></span>               | <span data-ttu-id="e5d8a-168">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-168">Value (type)</span></span> | <span data-ttu-id="e5d8a-169">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e5d8a-169">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e5d8a-170">Owin. RequestId</span><span class="sxs-lookup"><span data-stu-id="e5d8a-170">owin.RequestId</span></span> | `String` | <span data-ttu-id="e5d8a-171">Optional</span><span class="sxs-lookup"><span data-stu-id="e5d8a-171">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="e5d8a-172">Antwortdaten (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-172">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="e5d8a-173">Key</span><span class="sxs-lookup"><span data-stu-id="e5d8a-173">Key</span></span>               | <span data-ttu-id="e5d8a-174">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-174">Value (type)</span></span> | <span data-ttu-id="e5d8a-175">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e5d8a-175">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e5d8a-176">Owin. ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="e5d8a-176">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="e5d8a-177">Optional</span><span class="sxs-lookup"><span data-stu-id="e5d8a-177">Optional</span></span> |
| <span data-ttu-id="e5d8a-178">Owin. ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="e5d8a-178">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="e5d8a-179">Optional</span><span class="sxs-lookup"><span data-stu-id="e5d8a-179">Optional</span></span> |
| <span data-ttu-id="e5d8a-180">Owin. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="e5d8a-180">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="e5d8a-181">Owin. ResponseBody</span><span class="sxs-lookup"><span data-stu-id="e5d8a-181">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="e5d8a-182">Andere Daten (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-182">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="e5d8a-183">Key</span><span class="sxs-lookup"><span data-stu-id="e5d8a-183">Key</span></span>               | <span data-ttu-id="e5d8a-184">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-184">Value (type)</span></span> | <span data-ttu-id="e5d8a-185">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e5d8a-185">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e5d8a-186">Owin. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="e5d8a-186">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="e5d8a-187">Owin. Version</span><span class="sxs-lookup"><span data-stu-id="e5d8a-187">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="e5d8a-188">Allgemeine Schlüssel</span><span class="sxs-lookup"><span data-stu-id="e5d8a-188">Common Keys</span></span>

| <span data-ttu-id="e5d8a-189">Key</span><span class="sxs-lookup"><span data-stu-id="e5d8a-189">Key</span></span>               | <span data-ttu-id="e5d8a-190">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-190">Value (type)</span></span> | <span data-ttu-id="e5d8a-191">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e5d8a-191">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e5d8a-192">SSL. ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="e5d8a-192">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="e5d8a-193">SSL. LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="e5d8a-193">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="e5d8a-194">Server. RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="e5d8a-194">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="e5d8a-195">Server. Remoteanschluss</span><span class="sxs-lookup"><span data-stu-id="e5d8a-195">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="e5d8a-196">Server. LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="e5d8a-196">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="e5d8a-197">Server. LocalPort</span><span class="sxs-lookup"><span data-stu-id="e5d8a-197">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="e5d8a-198">Server. IsLocal</span><span class="sxs-lookup"><span data-stu-id="e5d8a-198">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="e5d8a-199">Server. OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="e5d8a-199">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="e5d8a-200">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="e5d8a-200">SendFiles v0.3.0</span></span>

| <span data-ttu-id="e5d8a-201">Key</span><span class="sxs-lookup"><span data-stu-id="e5d8a-201">Key</span></span>               | <span data-ttu-id="e5d8a-202">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-202">Value (type)</span></span> | <span data-ttu-id="e5d8a-203">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e5d8a-203">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e5d8a-204">SendFile bereitstellen. "SendAsync"</span><span class="sxs-lookup"><span data-stu-id="e5d8a-204">sendfile.SendAsync</span></span> | <span data-ttu-id="e5d8a-205">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-205">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="e5d8a-206">Pro Anforderung</span><span class="sxs-lookup"><span data-stu-id="e5d8a-206">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="e5d8a-207">Nicht transparente v0.3.0</span><span class="sxs-lookup"><span data-stu-id="e5d8a-207">Opaque v0.3.0</span></span>

| <span data-ttu-id="e5d8a-208">Key</span><span class="sxs-lookup"><span data-stu-id="e5d8a-208">Key</span></span>               | <span data-ttu-id="e5d8a-209">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-209">Value (type)</span></span> | <span data-ttu-id="e5d8a-210">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e5d8a-210">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e5d8a-211">nicht transparent. Version</span><span class="sxs-lookup"><span data-stu-id="e5d8a-211">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="e5d8a-212">nicht transparent. Upgrade</span><span class="sxs-lookup"><span data-stu-id="e5d8a-212">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="e5d8a-213">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-213">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="e5d8a-214">nicht transparent. Stream</span><span class="sxs-lookup"><span data-stu-id="e5d8a-214">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="e5d8a-215">nicht transparent. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="e5d8a-215">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="e5d8a-216">WebSocket-v0.3.0</span><span class="sxs-lookup"><span data-stu-id="e5d8a-216">WebSocket v0.3.0</span></span>

| <span data-ttu-id="e5d8a-217">Key</span><span class="sxs-lookup"><span data-stu-id="e5d8a-217">Key</span></span>               | <span data-ttu-id="e5d8a-218">Wert (Datentyp)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-218">Value (type)</span></span> | <span data-ttu-id="e5d8a-219">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="e5d8a-219">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="e5d8a-220">WebSocket. Version</span><span class="sxs-lookup"><span data-stu-id="e5d8a-220">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="e5d8a-221">WebSocket. Akzeptieren</span><span class="sxs-lookup"><span data-stu-id="e5d8a-221">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="e5d8a-222">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-222">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="e5d8a-223">WebSocket. AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="e5d8a-223">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="e5d8a-224">Nicht-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="e5d8a-224">Non-spec</span></span> |
| <span data-ttu-id="e5d8a-225">WebSocket. Unterprotokoll</span><span class="sxs-lookup"><span data-stu-id="e5d8a-225">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="e5d8a-226">Finden Sie unter [RFC6455 Abschnitt 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Schritt 5.5</span><span class="sxs-lookup"><span data-stu-id="e5d8a-226">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="e5d8a-227">WebSocket. "SendAsync"</span><span class="sxs-lookup"><span data-stu-id="e5d8a-227">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="e5d8a-228">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="e5d8a-229">WebSocket. ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="e5d8a-229">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="e5d8a-230">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="e5d8a-231">WebSocket. CloseAsync</span><span class="sxs-lookup"><span data-stu-id="e5d8a-231">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="e5d8a-232">Finden Sie unter [Signatur des Delegaten](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="e5d8a-232">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="e5d8a-233">WebSocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="e5d8a-233">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="e5d8a-234">WebSocket. ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="e5d8a-234">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="e5d8a-235">Optional</span><span class="sxs-lookup"><span data-stu-id="e5d8a-235">Optional</span></span> |
| <span data-ttu-id="e5d8a-236">WebSocket. ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="e5d8a-236">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="e5d8a-237">Optional</span><span class="sxs-lookup"><span data-stu-id="e5d8a-237">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="e5d8a-238">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="e5d8a-238">Additional Resources</span></span>

* [<span data-ttu-id="e5d8a-239">Middleware</span><span class="sxs-lookup"><span data-stu-id="e5d8a-239">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="e5d8a-240">Server</span><span class="sxs-lookup"><span data-stu-id="e5d8a-240">Servers</span></span>](servers/index.md)