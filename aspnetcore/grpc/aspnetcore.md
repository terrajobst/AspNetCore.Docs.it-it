---
title: Servizi gRPC con ASP.NET Core
author: juntaoluo
description: Informazioni sui concetti di base per la scrittura di servizi gRPC con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 26f0d7610151460967b97665ed61deab1ef56d68
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862934"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="0b0e7-103">Servizi gRPC con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b0e7-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="0b0e7-104">Questo documento illustra come iniziare a usare i servizi di gRPC con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b0e7-105">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="0b0e7-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b0e7-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b0e7-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0b0e7-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0b0e7-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0b0e7-108">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="0b0e7-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="0b0e7-109">Introduzione all'uso del servizio gRPC in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b0e7-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="0b0e7-110">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0b0e7-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b0e7-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b0e7-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0b0e7-112">Per istruzioni dettagliate su come creare un progetto gRPC, vedere [Introduzione ai servizi gRPC](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="0b0e7-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0b0e7-113">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="0b0e7-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="0b0e7-114">Eseguire `dotnet new grpc -o GrpcGreeter` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="0b0e7-115">Aggiungere servizi gRPC a un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b0e7-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="0b0e7-116">gRPC richiede il pacchetto [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="0b0e7-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="0b0e7-117">Configurare gRPC</span><span class="sxs-lookup"><span data-stu-id="0b0e7-117">Configure gRPC</span></span>

<span data-ttu-id="0b0e7-118">gRPC è abilitato con il `AddGrpc` metodo:</span><span class="sxs-lookup"><span data-stu-id="0b0e7-118">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="0b0e7-119">Ogni servizio gRPC viene aggiunto alla pipeline di routing tramite il `MapGrpcService` metodo:</span><span class="sxs-lookup"><span data-stu-id="0b0e7-119">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="0b0e7-120">ASP.NET Core middleware e funzionalità condividono la pipeline di routing, di conseguenza è possibile configurare un'app per gestire gestori di richieste aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-120">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="0b0e7-121">I gestori di richieste aggiuntivi, ad esempio i controller MVC, funzionano in parallelo con i servizi gRPC configurati.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-121">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="0b0e7-122">Integrazione con API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b0e7-122">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="0b0e7-123">i servizi gRPC hanno accesso completo alle funzionalità di ASP.NET Core come l' [inserimento](xref:fundamentals/dependency-injection) delle dipendenze e la [registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="0b0e7-123">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="0b0e7-124">Ad esempio, l'implementazione del servizio può risolvere un servizio logger dal contenitore DI inserimento delle dipendenze tramite il costruttore:</span><span class="sxs-lookup"><span data-stu-id="0b0e7-124">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="0b0e7-125">Per impostazione predefinita, l'implementazione del servizio gRPC è in grado di risolvere altri servizi DI con qualsiasi durata (singleton, ambito o temporaneo).</span><span class="sxs-lookup"><span data-stu-id="0b0e7-125">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="0b0e7-126">Risolvere HttpContext nei metodi gRPC</span><span class="sxs-lookup"><span data-stu-id="0b0e7-126">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="0b0e7-127">L'API gRPC consente di accedere ad alcuni dati del messaggio HTTP/2, ad esempio il metodo, l'host, l'intestazione e i trailer.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-127">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="0b0e7-128">L'accesso avviene tramite `ServerCallContext` l'argomento passato a ogni metodo gRPC:</span><span class="sxs-lookup"><span data-stu-id="0b0e7-128">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="0b0e7-129">`ServerCallContext`non fornisce l'accesso completo a `HttpContext` in tutte le API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-129">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="0b0e7-130">Il `GetHttpContext` metodo`HttpContext` di estensione fornisce accesso completo a che rappresenta il messaggio http/2 sottostante nelle API ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="0b0e7-130">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="grpc-and-aspnet-core-on-macos"></a><span data-ttu-id="0b0e7-131">gRPC e ASP.NET Core in macOS</span><span class="sxs-lookup"><span data-stu-id="0b0e7-131">gRPC and ASP.NET Core on macOS</span></span>

<span data-ttu-id="0b0e7-132">Gheppio non supporta HTTP/2 con [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) in MacOS.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-132">Kestrel doesn't support HTTP/2 with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) on macOS.</span></span> <span data-ttu-id="0b0e7-133">Per impostazione predefinita, il ASP.NET Core modello e gli esempi di gRPC usano TLS.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-133">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="0b0e7-134">Quando si tenta di avviare il server gRPC, viene visualizzato il messaggio di errore seguente:</span><span class="sxs-lookup"><span data-stu-id="0b0e7-134">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="0b0e7-135">Impossibile eseguire il binding https://localhost:5001 a nell'interfaccia loopback IPv4: ' HTTP/2 su TLS non è supportato in macOS perché manca il supporto per ALPN .'.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-135">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="0b0e7-136">Per risolvere questo problema, configurare gheppio e il client gRPC per l'uso di HTTP/2 **senza** TLS.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-136">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 **without** TLS.</span></span> <span data-ttu-id="0b0e7-137">Questa operazione deve essere eseguita solo durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-137">You should only do this during development.</span></span> <span data-ttu-id="0b0e7-138">Se non si usa TLS, i messaggi gRPC vengono inviati senza crittografia.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-138">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="0b0e7-139">Gheppio deve configurare un endpoint HTTP/2 senza TLS in `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="0b0e7-139">Kestrel must configure a HTTP/2 endpoint without TLS in `Program.cs`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="0b0e7-140">Il client gRPC deve impostare l' `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` opzione su `true` e usare `http` nell'indirizzo del server:</span><span class="sxs-lookup"><span data-stu-id="0b0e7-140">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

> [!WARNING]
> <span data-ttu-id="0b0e7-141">HTTP/2 senza TLS deve essere usato solo durante lo sviluppo di app.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-141">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="0b0e7-142">Le applicazioni di produzione devono sempre usare la sicurezza del trasporto.</span><span class="sxs-lookup"><span data-stu-id="0b0e7-142">Production applications should always use transport security.</span></span> <span data-ttu-id="0b0e7-143">Per ulteriori informazioni, vedere [considerazioni sulla sicurezza in gRPC per ASP.NET Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="0b0e7-143">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0b0e7-144">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0b0e7-144">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
