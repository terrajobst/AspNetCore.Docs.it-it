---
title: Servizi gRPC con ASP.NET Core
author: juntaoluo
description: Informazioni sui concetti di base per la scrittura di servizi gRPC con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 28e6b8589bbe0b6a3723b64736c723c883302571
ms.sourcegitcommit: e6bd2bbe5683e9a7dbbc2f2eab644986e6dc8a87
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/03/2019
ms.locfileid: "70238167"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="3cfe5-103">Servizi gRPC con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3cfe5-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="3cfe5-104">Questo documento illustra come iniziare a usare i servizi di gRPC con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3cfe5-105">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3cfe5-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3cfe5-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3cfe5-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3cfe5-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3cfe5-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3cfe5-108">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3cfe5-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="3cfe5-109">Introduzione all'uso del servizio gRPC in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3cfe5-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="3cfe5-110">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="3cfe5-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3cfe5-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3cfe5-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3cfe5-112">Per istruzioni dettagliate su come creare un progetto gRPC, vedere [Introduzione ai servizi gRPC](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="3cfe5-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="3cfe5-113">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3cfe5-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="3cfe5-114">Eseguire `dotnet new grpc -o GrpcGreeter` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="3cfe5-115">Aggiungere servizi gRPC a un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3cfe5-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="3cfe5-116">gRPC richiede il pacchetto [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="3cfe5-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="3cfe5-117">Configurare gRPC</span><span class="sxs-lookup"><span data-stu-id="3cfe5-117">Configure gRPC</span></span>

<span data-ttu-id="3cfe5-118">In *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3cfe5-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="3cfe5-119">gRPC è abilitato con il `AddGrpc` metodo.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="3cfe5-120">Ogni servizio gRPC viene aggiunto alla pipeline di routing tramite il `MapGrpcService` metodo.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

<span data-ttu-id="3cfe5-121">ASP.NET Core middleware e funzionalità condividono la pipeline di routing, di conseguenza è possibile configurare un'app per gestire gestori di richieste aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="3cfe5-122">I gestori di richieste aggiuntivi, ad esempio i controller MVC, funzionano in parallelo con i servizi gRPC configurati.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="3cfe5-123">Configurare gheppio</span><span class="sxs-lookup"><span data-stu-id="3cfe5-123">Configure Kestrel</span></span>

<span data-ttu-id="3cfe5-124">Endpoint gRPC di Gheppio:</span><span class="sxs-lookup"><span data-stu-id="3cfe5-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="3cfe5-125">Richiedi HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-125">Require HTTP/2.</span></span>
* <span data-ttu-id="3cfe5-126">Deve essere protetto con HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-126">Should be secured with HTTPS.</span></span>

#### <a name="http2"></a><span data-ttu-id="3cfe5-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="3cfe5-127">HTTP/2</span></span>

<span data-ttu-id="3cfe5-128">gRPC richiede HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-128">gRPC requires HTTP/2.</span></span> <span data-ttu-id="3cfe5-129">gRPC per ASP.NET Core convalida [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) è `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-129">gRPC for ASP.NET Core validates [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) is `HTTP/2`.</span></span>

<span data-ttu-id="3cfe5-130">Gheppio [supporta http/2](xref:fundamentals/servers/kestrel#http2-support) nei sistemi operativi più recenti.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-130">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="3cfe5-131">Per impostazione predefinita, gli endpoint gheppio sono configurati per supportare connessioni HTTP/1.1 e HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-131">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

#### <a name="https"></a><span data-ttu-id="3cfe5-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3cfe5-132">HTTPS</span></span>

<span data-ttu-id="3cfe5-133">Gli endpoint gheppio usati per gRPC devono essere protetti con HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-133">Kestrel endpoints used for gRPC should be secured with HTTPS.</span></span> <span data-ttu-id="3cfe5-134">Durante lo sviluppo, viene creato automaticamente un endpoint HTTPS `https://localhost:5001` quando è presente il certificato di sviluppo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-134">In development, an HTTPS endpoint is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="3cfe5-135">Non è necessaria alcuna configurazione.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-135">No configuration is required.</span></span>

<span data-ttu-id="3cfe5-136">Nell'ambiente di produzione, HTTPS deve essere configurato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-136">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="3cfe5-137">Nell'esempio *appSettings. JSON* seguente viene fornito un endpoint HTTP/2 protetto con https:</span><span class="sxs-lookup"><span data-stu-id="3cfe5-137">In the following *appsettings.json* example, an HTTP/2 endpoint secured with HTTPS is provided:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http2"
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="3cfe5-138">In alternativa, è possibile configurare gli endpoint gheppio in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="3cfe5-138">Alternatively, Kestrel endpoints can be configured in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // This endpoint will use HTTP/2 and HTTPS on port 5001.
                options.Listen(IPAddress.Any, 5001, listenOptions =>
                {
                    listenOptions.Protocols = HttpProtocols.Http2;
                    listenOptions.UseHttps("<path to .pfx file>", 
                        "<certificate password>");
                });
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="3cfe5-139">Quando un endpoint HTTP/2 viene configurato senza HTTPS, l'endpoint [ListenOptions. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) deve essere impostato su `HttpProtocols.Http2`.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-139">When an HTTP/2 endpoint is configured without HTTPS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="3cfe5-140">`HttpProtocols.Http1AndHttp2`non può essere usato perché HTTPS è necessario per negoziare HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-140">`HttpProtocols.Http1AndHttp2` can't be used because HTTPS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="3cfe5-141">Senza HTTPS, tutte le connessioni all'endpoint vengono predefinite a HTTP/1.1 e le chiamate a gRPC hanno esito negativo.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-141">Without HTTPS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="3cfe5-142">Per ulteriori informazioni sull'abilitazione di HTTP/2 e HTTPS con gheppio, vedere la pagina relativa alla [configurazione dell'endpoint gheppio](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="3cfe5-142">For more information on enabling HTTP/2 and HTTPS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!NOTE]
> <span data-ttu-id="3cfe5-143">macOS non supporta ASP.NET Core gRPC con [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="3cfe5-143">macOS doesn't support ASP.NET Core gRPC with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="3cfe5-144">Per eseguire correttamente i servizi gRPC in macOS, è necessaria una configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-144">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="3cfe5-145">Per altre informazioni, vedere [Non è possibile avviare un'app ASP.NET Core gRPC in macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="3cfe5-145">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="3cfe5-146">Integrazione con API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3cfe5-146">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="3cfe5-147">i servizi gRPC hanno accesso completo alle funzionalità di ASP.NET Core come l' [inserimento delle dipendenze](xref:fundamentals/dependency-injection) e la [registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="3cfe5-147">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="3cfe5-148">Ad esempio, l'implementazione del servizio può risolvere un servizio logger dal contenitore DI inserimento delle dipendenze tramite il costruttore:</span><span class="sxs-lookup"><span data-stu-id="3cfe5-148">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="3cfe5-149">Per impostazione predefinita, l'implementazione del servizio gRPC è in grado di risolvere altri servizi DI con qualsiasi durata (singleton, ambito o temporaneo).</span><span class="sxs-lookup"><span data-stu-id="3cfe5-149">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="3cfe5-150">Risolvere HttpContext nei metodi gRPC</span><span class="sxs-lookup"><span data-stu-id="3cfe5-150">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="3cfe5-151">L'API gRPC consente di accedere ad alcuni dati del messaggio HTTP/2, ad esempio il metodo, l'host, l'intestazione e i trailer.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-151">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="3cfe5-152">L'accesso avviene tramite `ServerCallContext` l'argomento passato a ogni metodo gRPC:</span><span class="sxs-lookup"><span data-stu-id="3cfe5-152">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="3cfe5-153">`ServerCallContext`non fornisce l'accesso completo a `HttpContext` in tutte le API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3cfe5-153">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="3cfe5-154">Il `GetHttpContext` metodo`HttpContext` di estensione fornisce accesso completo a che rappresenta il messaggio http/2 sottostante nelle API ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="3cfe5-154">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="3cfe5-155">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3cfe5-155">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
