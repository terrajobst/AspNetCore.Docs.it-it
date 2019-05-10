---
title: Migrazione dei servizi gRPC da C-core a ASP.NET Core
author: juntaoluo
description: Informazioni su come spostare un'app basata su gRPC C-core esistente per l'esecuzione all'inizio dello stack di ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 47d74edd821124f0c8390d704ca7931b7eb6c4cd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895238"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="7c6b0-103">Migrazione dei servizi gRPC da C-core a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c6b0-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="7c6b0-104">A cura di [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="7c6b0-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="7c6b0-105">A causa dell'implementazione di stack sottostante, non tutte le funzionalità funzionano nello stesso modo tra [basate su C-core gRPC](https://grpc.io/blog/grpc-stacks) App e App basate su ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="7c6b0-106">Questo documento vengono evidenziate le differenze principali per la migrazione tra i due stack.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="7c6b0-107">durata di implementazione del servizio gRPC</span><span class="sxs-lookup"><span data-stu-id="7c6b0-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="7c6b0-108">Nello stack di ASP.NET Core, servizi gRPC, per impostazione predefinita, vengono creati con una [durata con ambito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="7c6b0-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="7c6b0-109">Al contrario, gRPC C-core per impostazione predefinita associata a un servizio con un [durata del singleton](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="7c6b0-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="7c6b0-110">Una durata con ambita consente l'implementazione del servizio risolvere altri servizi con durata con ambita.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="7c6b0-111">Ad esempio, una durata con ambita può anche risolvere `DBContext` dal contenitore di inserimento delle dipendenze tramite inserimento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-111">For example, a scoped lifetime can also resolve `DBContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="7c6b0-112">Utilizzo di durata con ambito:</span><span class="sxs-lookup"><span data-stu-id="7c6b0-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="7c6b0-113">Per ogni richiesta viene creata una nuova istanza dell'implementazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="7c6b0-114">Non è possibile condividere lo stato tra le richieste tramite i membri di istanza sul tipo di implementazione.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="7c6b0-115">Nella previsione consiste nell'archiviare degli stati condivisi in un servizio singleton nel contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="7c6b0-116">Gli stati condivisi archiviati vengono risolti nel costruttore dell'implementazione del servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="7c6b0-117">Per altre informazioni su durate del servizio, vedere <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="7c6b0-118">Aggiungere un servizio singleton</span><span class="sxs-lookup"><span data-stu-id="7c6b0-118">Add a singleton service</span></span>

<span data-ttu-id="7c6b0-119">Per facilitare la transizione da un'implementazione di C-core gRPC ad ASP.NET Core, è possibile modificare la durata del servizio di implementazione del servizio dal ha come ambito singleton.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="7c6b0-120">Ciò implica l'aggiunta di un'istanza dell'implementazione del servizio per il contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="7c6b0-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="7c6b0-121">Tuttavia, un'implementazione del servizio con una durata singleton non è più in grado di risolvere i servizi con ambiti tramite inserimento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="7c6b0-122">Configurare le opzioni di servizi gRPC</span><span class="sxs-lookup"><span data-stu-id="7c6b0-122">Configure gRPC services options</span></span>

<span data-ttu-id="7c6b0-123">Nelle App basate su C-core, impostazioni, ad esempio `grpc.max_receive_message_length` e `grpc.max_send_message_length` sono configurati con `ChannelOption` quando [creazione istanza del Server](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="7c6b0-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="7c6b0-124">In ASP.NET Core, gRPC fornisce la configurazione tramite il `GrpcServiceOptions` tipo.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="7c6b0-125">Ad esempio, gRPC di un servizio la dimensione massima dei messaggi in ingresso può essere configurata tramite `AddGrpc`:</span><span class="sxs-lookup"><span data-stu-id="7c6b0-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

<span data-ttu-id="7c6b0-126">Per altre informazioni sulla configurazione, vedere <xref:grpc/configuration>.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-126">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="7c6b0-127">Registrazione</span><span class="sxs-lookup"><span data-stu-id="7c6b0-127">Logging</span></span>

<span data-ttu-id="7c6b0-128">Le app basate su C-core si basano sul `GrpcEnvironment` al [configurare il logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) a scopo di debug.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="7c6b0-129">Lo stack di ASP.NET Core fornisce questa funzionalità tramite il [API di registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="7c6b0-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="7c6b0-130">Ad esempio, un logger può essere aggiunto al servizio gRPC tramite inserimento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="7c6b0-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="7c6b0-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="7c6b0-131">HTTPS</span></span>

<span data-ttu-id="7c6b0-132">Le app basate su C-core configurare HTTPS tramite il [Server.Ports proprietà](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="7c6b0-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="7c6b0-133">Un concetto simile viene utilizzato per configurare i server in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="7c6b0-134">Ad esempio, Usa Kestrel [configurazione dell'endpoint](xref:fundamentals/servers/kestrel#endpoint-configuration) per questa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="7c6b0-135">Middleware e intercettori</span><span class="sxs-lookup"><span data-stu-id="7c6b0-135">Interceptors and Middleware</span></span>

<span data-ttu-id="7c6b0-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offre funzionalità simili rispetto agli intercettori nelle App basate su C-core gRPC.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="7c6b0-137">Middleware e intercettori concettualmente sono uguali perché entrambi vengono usati per costruire una pipeline che gestisce una richiesta gRPC.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="7c6b0-138">Entrambi consentono operazioni da eseguire prima o dopo il componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="7c6b0-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="7c6b0-139">Tuttavia, middleware di ASP.NET Core agisce sui messaggi HTTP/2 sottostanti, mentre gli intercettori operano sul livello di astrazione usando gRPC il [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="7c6b0-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c6b0-140">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7c6b0-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
