---
title: Servizi gRPC con ASP.NET Core
author: juntaoluo
description: Informazioni sui concetti di base per la scrittura di servizi gRPC con ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 02e443dfecf2f7464a8ecabfc0cac67854d63232
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412483"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="15574-103">Servizi gRPC con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15574-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="15574-104">Questo documento illustra come iniziare a usare i servizi di gRPC con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="15574-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15574-105">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="15574-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15574-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15574-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="15574-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="15574-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="15574-108">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="15574-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="15574-109">Introduzione all'uso del servizio gRPC in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15574-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="15574-110">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="15574-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15574-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15574-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="15574-112">Per istruzioni dettagliate su come creare un progetto gRPC, vedere [Introduzione ai servizi gRPC](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="15574-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="15574-113">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="15574-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="15574-114">Eseguire `dotnet new grpc -o GrpcGreeter` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="15574-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="15574-115">Aggiungere servizi gRPC a un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15574-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="15574-116">gRPC richiede il pacchetto [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="15574-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="15574-117">Configurare gRPC</span><span class="sxs-lookup"><span data-stu-id="15574-117">Configure gRPC</span></span>

<span data-ttu-id="15574-118">gRPC è abilitato con il `AddGrpc` metodo:</span><span class="sxs-lookup"><span data-stu-id="15574-118">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="15574-119">Ogni servizio gRPC viene aggiunto alla pipeline di routing tramite il `MapGrpcService` metodo:</span><span class="sxs-lookup"><span data-stu-id="15574-119">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="15574-120">ASP.NET Core middleware e funzionalità condividono la pipeline di routing, di conseguenza è possibile configurare un'app per gestire gestori di richieste aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="15574-120">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="15574-121">I gestori di richieste aggiuntivi, ad esempio i controller MVC, funzionano in parallelo con i servizi gRPC configurati.</span><span class="sxs-lookup"><span data-stu-id="15574-121">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="15574-122">Integrazione con API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15574-122">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="15574-123">i servizi gRPC hanno accesso completo alle funzionalità di ASP.NET Core come l' [inserimento](xref:fundamentals/dependency-injection) delle dipendenze e la [registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="15574-123">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="15574-124">Ad esempio, l'implementazione del servizio può risolvere un servizio logger dal contenitore DI inserimento delle dipendenze tramite il costruttore:</span><span class="sxs-lookup"><span data-stu-id="15574-124">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="15574-125">Per impostazione predefinita, l'implementazione del servizio gRPC è in grado di risolvere altri servizi DI con qualsiasi durata (singleton, ambito o temporaneo).</span><span class="sxs-lookup"><span data-stu-id="15574-125">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="15574-126">Risolvere HttpContext nei metodi gRPC</span><span class="sxs-lookup"><span data-stu-id="15574-126">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="15574-127">L'API gRPC consente di accedere ad alcuni dati del messaggio HTTP/2, ad esempio il metodo, l'host, l'intestazione e i trailer.</span><span class="sxs-lookup"><span data-stu-id="15574-127">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="15574-128">L'accesso avviene tramite `ServerCallContext` l'argomento passato a ogni metodo gRPC:</span><span class="sxs-lookup"><span data-stu-id="15574-128">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="15574-129">`ServerCallContext`non fornisce l'accesso completo a `HttpContext` in tutte le API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="15574-129">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="15574-130">Il `GetHttpContext` metodo`HttpContext` di estensione fornisce accesso completo a che rappresenta il messaggio http/2 sottostante nelle API ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="15574-130">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="15574-131">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="15574-131">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
