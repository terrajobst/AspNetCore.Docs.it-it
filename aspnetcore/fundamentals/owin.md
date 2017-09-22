---
title: Aprire l'interfaccia Web per .NET (OWIN)
author: ardalis
description: Introduzione a aprire interfaccia Web per .NET (OWIN).
keywords: ASP.NET Core, interfaccia Web aperta per .NET, OWIN
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 70c4e6bc-a773-4039-96ec-6fe557c9369d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9edacb494c38d7812f9e3826ab9277cd1dffd675
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="2f038-104">Introduzione per aprire interfaccia Web per .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="2f038-104">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="2f038-105">Da [Steve Smith](https://ardalis.com/) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2f038-105">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2f038-106">ASP.NET Core supporta l'interfaccia Web aperta per .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="2f038-106">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="2f038-107">OWIN consente alle App web di essere scollegata dal server web.</span><span class="sxs-lookup"><span data-stu-id="2f038-107">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="2f038-108">Definisce un modo standard dei middleware per essere utilizzata in una pipeline per gestire le richieste e risposte associate.</span><span class="sxs-lookup"><span data-stu-id="2f038-108">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="2f038-109">Middleware e delle applicazioni ASP.NET Core possono interagire con middleware, server e applicazioni basate su OWIN.</span><span class="sxs-lookup"><span data-stu-id="2f038-109">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="2f038-110">OWIN fornisce un livello di separazione che consente di due modelli con modelli a oggetti diversi da utilizzare insieme.</span><span class="sxs-lookup"><span data-stu-id="2f038-110">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="2f038-111">Il `Microsoft.AspNetCore.Owin` pacchetto fornisce due implementazioni dell'adattatore:</span><span class="sxs-lookup"><span data-stu-id="2f038-111">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="2f038-112">ASP.NET Core per OWIN</span><span class="sxs-lookup"><span data-stu-id="2f038-112">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="2f038-113">OWIN per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2f038-113">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="2f038-114">In questo modo ASP.NET di base deve essere ospitato su un OWIN compatibile/host server o per altri componenti compatibile OWIN da eseguire su ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2f038-114">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="2f038-115">Nota: L'uso di questi adapter comporta con una riduzione delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="2f038-115">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="2f038-116">Applicazioni utilizzando solo i componenti di base di ASP.NET non devono utilizzare il pacchetto Owin o schede.</span><span class="sxs-lookup"><span data-stu-id="2f038-116">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

[<span data-ttu-id="2f038-117">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="2f038-117">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="2f038-118">Middleware OWIN in esecuzione nella pipeline ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2f038-118">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="2f038-119">Supporto OWIN del ASP.NET di base viene distribuito come parte di `Microsoft.AspNetCore.Owin` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="2f038-119">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="2f038-120">Per importare supporto OWIN nel progetto di installazione del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="2f038-120">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="2f038-121">Middleware OWIN conforme al [specifica OWIN](http://owin.org/spec/spec/owin-1.0.0.html), che richiede un `Func<IDictionary<string, object>, Task>` impostare interfaccia e chiavi specifiche (ad esempio `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="2f038-121">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="2f038-122">Middleware OWIN semplice seguente visualizza "Hello World":</span><span class="sxs-lookup"><span data-stu-id="2f038-122">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="2f038-123">La firma di esempio restituisce un `Task` e accetta un `IDictionary<string, object>` come richiesto da OWIN.</span><span class="sxs-lookup"><span data-stu-id="2f038-123">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="2f038-124">Il codice seguente viene illustrato come aggiungere il `OwinHello` middleware (illustrato in precedenza) per la pipeline ASP.NET con il `UseOwin` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="2f038-124">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

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

<span data-ttu-id="2f038-125">È possibile configurare altre azioni da intraprendere sul posto all'interno della pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="2f038-125">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="2f038-126">Le intestazioni di risposta devono essere modificate solo prima la prima scrittura nel flusso di risposta.</span><span class="sxs-lookup"><span data-stu-id="2f038-126">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="2f038-127">Più chiamate al metodo `UseOwin` è sconsigliato per motivi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="2f038-127">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="2f038-128">Componenti OWIN funzionerà meglio se raggruppati insieme.</span><span class="sxs-lookup"><span data-stu-id="2f038-128">OWIN components will operate best if grouped together.</span></span>

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

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="2f038-129">Tramite l'Hosting ASP.NET in un server basato su OWIN</span><span class="sxs-lookup"><span data-stu-id="2f038-129">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="2f038-130">Server basati su OWIN può ospitare applicazioni ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2f038-130">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="2f038-131">Un server di questo tipo è [Nowin](https://github.com/Bobris/Nowin), un server web di OWIN .NET.</span><span class="sxs-lookup"><span data-stu-id="2f038-131">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="2f038-132">Nell'esempio di questo articolo, ho incluso un progetto che fa riferimento a Nowin e viene utilizzato per creare un `IServer` in grado di self-hosting ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2f038-132">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="2f038-133">`IServer`è un'interfaccia che richiede un `Features` proprietà e un `Start` metodo.</span><span class="sxs-lookup"><span data-stu-id="2f038-133">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="2f038-134">`Start`è responsabile della configurazione e avvio del server, che in questo caso viene eseguito tramite una serie di chiamate API fluent impostare indirizzi analizzati il IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="2f038-134">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="2f038-135">Si noti che la configurazione di Microsoft Office fluent di `_builder` variabile specifica che le richieste verranno gestite dal `appFunc` definito in precedenza nel metodo.</span><span class="sxs-lookup"><span data-stu-id="2f038-135">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="2f038-136">Questo `Func` viene chiamato su ogni richiesta per elaborare le richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="2f038-136">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="2f038-137">Verrà inoltre aggiunto un `IWebHostBuilder` estensione rendono più semplice aggiungere e configurare il server Nowin.</span><span class="sxs-lookup"><span data-stu-id="2f038-137">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

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

<span data-ttu-id="2f038-138">A questo punto, tutto ciò che è necessario per eseguire un'applicazione ASP.NET utilizzando questo server personalizzato per chiamare l'estensione in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2f038-138">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

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

<span data-ttu-id="2f038-139">Ulteriori informazioni su ASP.NET [server](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="2f038-139">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="2f038-140">Eseguire ASP.NET Core in un server basato su OWIN e utilizzare il supporto di WebSocket</span><span class="sxs-lookup"><span data-stu-id="2f038-140">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="2f038-141">Un altro esempio di funzionalità dei server basati su OWIN può essere utilizzato da ASP.NET Core è l'accesso a funzionalità quali WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2f038-141">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="2f038-142">Il server web di OWIN .NET utilizzato nell'esempio precedente è il supporto per i socket Web incorporato, che può essere utilizzato da un'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2f038-142">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="2f038-143">L'esempio seguente mostra un'app web semplici che supporta i WebSocket e di restituisce tutti gli elementi inviati al server tramite WebSockets.</span><span class="sxs-lookup"><span data-stu-id="2f038-143">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

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

<span data-ttu-id="2f038-144">Questo [esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) viene configurata utilizzando lo stesso `NowinServer` a quello precedente, l'unica differenza è in modalità con cui l'applicazione è configurata nel relativo `Configure` (metodo).</span><span class="sxs-lookup"><span data-stu-id="2f038-144">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="2f038-145">Un test utilizzando [un client websocket semplice](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) illustra l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="2f038-145">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Client di Test Web Socket](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="2f038-147">Ambiente OWIN</span><span class="sxs-lookup"><span data-stu-id="2f038-147">OWIN environment</span></span>

<span data-ttu-id="2f038-148">È possibile creare un ambiente OWIN tramite il `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="2f038-148">You can construct a OWIN environment using the `HttpContext`.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="2f038-149">Chiavi OWIN</span><span class="sxs-lookup"><span data-stu-id="2f038-149">OWIN keys</span></span>

<span data-ttu-id="2f038-150">Dipende da OWIN un `IDictionary<string,object>` oggetto per comunicare informazioni attraverso uno scambio di richiesta/risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2f038-150">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="2f038-151">ASP.NET Core implementa le chiavi elencate di seguito.</span><span class="sxs-lookup"><span data-stu-id="2f038-151">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="2f038-152">Vedere il [specifica primario, estensioni](http://owin.org/#spec), e [chiavi comuni e le linee guida chiave OWIN](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="2f038-152">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="2f038-153">Dati della richiesta (versione 1.0.0 OWIN)</span><span class="sxs-lookup"><span data-stu-id="2f038-153">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="2f038-154">Chiave</span><span class="sxs-lookup"><span data-stu-id="2f038-154">Key</span></span>               | <span data-ttu-id="2f038-155">Valore (tipo)</span><span class="sxs-lookup"><span data-stu-id="2f038-155">Value (type)</span></span> | <span data-ttu-id="2f038-156">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2f038-156">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="2f038-157">owin. RequestScheme</span><span class="sxs-lookup"><span data-stu-id="2f038-157">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="2f038-158">owin. RequestMethod</span><span class="sxs-lookup"><span data-stu-id="2f038-158">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="2f038-159">owin. RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="2f038-159">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="2f038-160">owin. RequestPath</span><span class="sxs-lookup"><span data-stu-id="2f038-160">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="2f038-161">owin. RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="2f038-161">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="2f038-162">owin. RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="2f038-162">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="2f038-163">owin. RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="2f038-163">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="2f038-164">owin. RequestBody</span><span class="sxs-lookup"><span data-stu-id="2f038-164">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="2f038-165">Dati della richiesta (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="2f038-165">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="2f038-166">Chiave</span><span class="sxs-lookup"><span data-stu-id="2f038-166">Key</span></span>               | <span data-ttu-id="2f038-167">Valore (tipo)</span><span class="sxs-lookup"><span data-stu-id="2f038-167">Value (type)</span></span> | <span data-ttu-id="2f038-168">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2f038-168">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="2f038-169">owin. ID richiesta</span><span class="sxs-lookup"><span data-stu-id="2f038-169">owin.RequestId</span></span> | `String` | <span data-ttu-id="2f038-170">Facoltativo</span><span class="sxs-lookup"><span data-stu-id="2f038-170">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="2f038-171">Dati di risposta (versione 1.0.0 OWIN)</span><span class="sxs-lookup"><span data-stu-id="2f038-171">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="2f038-172">Chiave</span><span class="sxs-lookup"><span data-stu-id="2f038-172">Key</span></span>               | <span data-ttu-id="2f038-173">Valore (tipo)</span><span class="sxs-lookup"><span data-stu-id="2f038-173">Value (type)</span></span> | <span data-ttu-id="2f038-174">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2f038-174">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="2f038-175">owin. ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="2f038-175">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="2f038-176">Facoltativo</span><span class="sxs-lookup"><span data-stu-id="2f038-176">Optional</span></span> |
| <span data-ttu-id="2f038-177">owin. ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="2f038-177">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="2f038-178">Facoltativo</span><span class="sxs-lookup"><span data-stu-id="2f038-178">Optional</span></span> |
| <span data-ttu-id="2f038-179">owin. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="2f038-179">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="2f038-180">owin. ResponseBody</span><span class="sxs-lookup"><span data-stu-id="2f038-180">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="2f038-181">Altri dati (versione 1.0.0 OWIN)</span><span class="sxs-lookup"><span data-stu-id="2f038-181">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="2f038-182">Chiave</span><span class="sxs-lookup"><span data-stu-id="2f038-182">Key</span></span>               | <span data-ttu-id="2f038-183">Valore (tipo)</span><span class="sxs-lookup"><span data-stu-id="2f038-183">Value (type)</span></span> | <span data-ttu-id="2f038-184">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2f038-184">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="2f038-185">owin. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="2f038-185">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="2f038-186">owin. Versione</span><span class="sxs-lookup"><span data-stu-id="2f038-186">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="2f038-187">Chiavi comuni</span><span class="sxs-lookup"><span data-stu-id="2f038-187">Common Keys</span></span>

| <span data-ttu-id="2f038-188">Chiave</span><span class="sxs-lookup"><span data-stu-id="2f038-188">Key</span></span>               | <span data-ttu-id="2f038-189">Valore (tipo)</span><span class="sxs-lookup"><span data-stu-id="2f038-189">Value (type)</span></span> | <span data-ttu-id="2f038-190">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2f038-190">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="2f038-191">SSL. ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="2f038-191">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="2f038-192">SSL. LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="2f038-192">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="2f038-193">Server. IndirizzoIPRemoto</span><span class="sxs-lookup"><span data-stu-id="2f038-193">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="2f038-194">Server. PortaRemota</span><span class="sxs-lookup"><span data-stu-id="2f038-194">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="2f038-195">Server. LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="2f038-195">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="2f038-196">Server. Porta locale</span><span class="sxs-lookup"><span data-stu-id="2f038-196">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="2f038-197">Server. IsLocal</span><span class="sxs-lookup"><span data-stu-id="2f038-197">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="2f038-198">Server. Pseudoevento</span><span class="sxs-lookup"><span data-stu-id="2f038-198">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="2f038-199">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="2f038-199">SendFiles v0.3.0</span></span>

| <span data-ttu-id="2f038-200">Chiave</span><span class="sxs-lookup"><span data-stu-id="2f038-200">Key</span></span>               | <span data-ttu-id="2f038-201">Valore (tipo)</span><span class="sxs-lookup"><span data-stu-id="2f038-201">Value (type)</span></span> | <span data-ttu-id="2f038-202">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2f038-202">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="2f038-203">SendFile. SendAsync</span><span class="sxs-lookup"><span data-stu-id="2f038-203">sendfile.SendAsync</span></span> | <span data-ttu-id="2f038-204">Vedere [firma del delegato](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="2f038-204">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="2f038-205">Per ogni richiesta</span><span class="sxs-lookup"><span data-stu-id="2f038-205">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="2f038-206">V0.3.0 opaco</span><span class="sxs-lookup"><span data-stu-id="2f038-206">Opaque v0.3.0</span></span>

| <span data-ttu-id="2f038-207">Chiave</span><span class="sxs-lookup"><span data-stu-id="2f038-207">Key</span></span>               | <span data-ttu-id="2f038-208">Valore (tipo)</span><span class="sxs-lookup"><span data-stu-id="2f038-208">Value (type)</span></span> | <span data-ttu-id="2f038-209">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2f038-209">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="2f038-210">opaco. Versione</span><span class="sxs-lookup"><span data-stu-id="2f038-210">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="2f038-211">opaco. Aggiornamento</span><span class="sxs-lookup"><span data-stu-id="2f038-211">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="2f038-212">Vedere [firma del delegato](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="2f038-212">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="2f038-213">opaco. Flusso</span><span class="sxs-lookup"><span data-stu-id="2f038-213">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="2f038-214">opaco. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="2f038-214">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="2f038-215">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="2f038-215">WebSocket v0.3.0</span></span>

| <span data-ttu-id="2f038-216">Chiave</span><span class="sxs-lookup"><span data-stu-id="2f038-216">Key</span></span>               | <span data-ttu-id="2f038-217">Valore (tipo)</span><span class="sxs-lookup"><span data-stu-id="2f038-217">Value (type)</span></span> | <span data-ttu-id="2f038-218">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2f038-218">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="2f038-219">WebSocket. Versione</span><span class="sxs-lookup"><span data-stu-id="2f038-219">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="2f038-220">WebSocket. Accettare</span><span class="sxs-lookup"><span data-stu-id="2f038-220">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="2f038-221">Vedere [firma del delegato](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="2f038-221">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="2f038-222">WebSocket. AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="2f038-222">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="2f038-223">Non-spec</span><span class="sxs-lookup"><span data-stu-id="2f038-223">Non-spec</span></span> |
| <span data-ttu-id="2f038-224">WebSocket. Protocollo secondario</span><span class="sxs-lookup"><span data-stu-id="2f038-224">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="2f038-225">Vedere [RFC6455 sezione 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) passaggio 5.5</span><span class="sxs-lookup"><span data-stu-id="2f038-225">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="2f038-226">WebSocket. SendAsync</span><span class="sxs-lookup"><span data-stu-id="2f038-226">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="2f038-227">Vedere [firma del delegato](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="2f038-227">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="2f038-228">WebSocket. ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="2f038-228">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="2f038-229">Vedere [firma del delegato](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="2f038-229">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="2f038-230">WebSocket. CloseAsync</span><span class="sxs-lookup"><span data-stu-id="2f038-230">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="2f038-231">Vedere [firma del delegato](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="2f038-231">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="2f038-232">WebSocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="2f038-232">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="2f038-233">WebSocket. ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="2f038-233">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="2f038-234">Facoltativo</span><span class="sxs-lookup"><span data-stu-id="2f038-234">Optional</span></span> |
| <span data-ttu-id="2f038-235">WebSocket. ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="2f038-235">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="2f038-236">Facoltativo</span><span class="sxs-lookup"><span data-stu-id="2f038-236">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="2f038-237">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2f038-237">Additional Resources</span></span>

* [<span data-ttu-id="2f038-238">Middleware</span><span class="sxs-lookup"><span data-stu-id="2f038-238">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="2f038-239">Server</span><span class="sxs-lookup"><span data-stu-id="2f038-239">Servers</span></span>](servers/index.md)
