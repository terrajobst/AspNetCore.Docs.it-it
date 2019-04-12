---
title: Migrazione dei servizi gRPC da C-core a ASP.NET Core
author: juntaoluo
description: Informazioni su come spostare un'app basata su gRPC C-core esistente per l'esecuzione all'inizio dello stack di ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: ffe5ccbd99c6920e093eddc00fc60a9f66aab527
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2019
ms.locfileid: "59515512"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="13dfa-103">Migrazione dei servizi gRPC da C-core a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13dfa-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="13dfa-104">A cura di [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="13dfa-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="13dfa-105">A causa dell'implementazione di stack sottostante, non tutte le funzionalità funzionano nello stesso modo tra [basate su C-core gRPC](https://grpc.io/blog/grpc-stacks) App e App basate su ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="13dfa-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="13dfa-106">Questo documento vengono evidenziate le differenze principali per la migrazione tra i due stack.</span><span class="sxs-lookup"><span data-stu-id="13dfa-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="13dfa-107">durata di implementazione del servizio gRPC</span><span class="sxs-lookup"><span data-stu-id="13dfa-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="13dfa-108">Nello stack di ASP.NET Core, servizi gRPC, per impostazione predefinita, vengono creati con una [durata con ambito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="13dfa-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="13dfa-109">Al contrario, gRPC C-core per impostazione predefinita associata a un servizio con un [durata del singleton](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="13dfa-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="13dfa-110">Una durata con ambita consente l'implementazione del servizio risolvere altri servizi con durata con ambita.</span><span class="sxs-lookup"><span data-stu-id="13dfa-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="13dfa-111">Ad esempio, una durata con ambita può anche risolvere `DBContext` dal contenitore di inserimento delle dipendenze tramite inserimento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="13dfa-111">For example, a scoped lifetime can also resolve `DBContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="13dfa-112">Utilizzo di durata con ambito:</span><span class="sxs-lookup"><span data-stu-id="13dfa-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="13dfa-113">Per ogni richiesta viene creata una nuova istanza dell'implementazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="13dfa-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="13dfa-114">Non è possibile condividere lo stato tra le richieste tramite i membri di istanza sul tipo di implementazione.</span><span class="sxs-lookup"><span data-stu-id="13dfa-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="13dfa-115">Nella previsione consiste nell'archiviare degli stati condivisi in un servizio singleton nel contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="13dfa-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="13dfa-116">Gli stati condivisi archiviati vengono risolti nel costruttore dell'implementazione del servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="13dfa-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span> 

<span data-ttu-id="13dfa-117">Per altre informazioni su durate del servizio, vedere <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="13dfa-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="13dfa-118">Aggiungere un servizio singleton</span><span class="sxs-lookup"><span data-stu-id="13dfa-118">Add a singleton service</span></span>

<span data-ttu-id="13dfa-119">Per facilitare la transizione da un'implementazione di C-core gRPC ad ASP.NET Core, è possibile modificare la durata del servizio di implementazione del servizio dal ha come ambito singleton.</span><span class="sxs-lookup"><span data-stu-id="13dfa-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="13dfa-120">Ciò implica l'aggiunta di un'istanza dell'implementazione del servizio per il contenitore di inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="13dfa-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="13dfa-121">Tuttavia, un'implementazione del servizio con una durata singleton non è più in grado di risolvere i servizi con ambiti tramite inserimento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="13dfa-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="13dfa-122">Configurare le opzioni di servizi gRPC</span><span class="sxs-lookup"><span data-stu-id="13dfa-122">Configure gRPC services options</span></span>

<span data-ttu-id="13dfa-123">Nelle App basate su C-core, impostazioni, ad esempio `grpc.max_receive_message_length` e `grpc.max_send_message_length` sono configurati con `ChannelOption` quando [creazione istanza del Server](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="13dfa-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="13dfa-124">In ASP.NET Core, `GrpcServiceOptions` fornisce un modo per configurare queste impostazioni.</span><span class="sxs-lookup"><span data-stu-id="13dfa-124">In ASP.NET Core, `GrpcServiceOptions` provides a way to configure these settings.</span></span> <span data-ttu-id="13dfa-125">Le impostazioni possono essere applicate a livello globale a tutti i servizi gRPC o a un tipo di implementazione del singolo servizio.</span><span class="sxs-lookup"><span data-stu-id="13dfa-125">The settings can be applied globally to all gRPC services or to an individual service implementation type.</span></span> <span data-ttu-id="13dfa-126">Le opzioni specificate per i tipi di implementazione di singoli servizi di eseguire l'override le impostazioni globali quando è configurato.</span><span class="sxs-lookup"><span data-stu-id="13dfa-126">Options specified for individual service implementation types override global settings when configured.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddGrpc(globalOptions =>
        {
            // Global settings
            globalOptions.SendMaxMessageSize = 4096
            globalOptions.ReceiveMaxMessageSize = 4096
        })
        .AddServiceOptions<GreeterService>(greeterOptions =>
        {
            // GreeterService settings. These will override global settings
            globalOptions.SendMaxMessageSize = 2048
            globalOptions.ReceiveMaxMessageSize = 2048
        })
}
```

## <a name="logging"></a><span data-ttu-id="13dfa-127">Registrazione</span><span class="sxs-lookup"><span data-stu-id="13dfa-127">Logging</span></span>

<span data-ttu-id="13dfa-128">Le app basate su C-core si basano sul `GrpcEnvironment` al [configurare il logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) a scopo di debug.</span><span class="sxs-lookup"><span data-stu-id="13dfa-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="13dfa-129">Lo stack di ASP.NET Core fornisce questa funzionalità tramite il [API di registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="13dfa-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="13dfa-130">Ad esempio, un logger può essere aggiunto al servizio gRPC tramite inserimento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="13dfa-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="13dfa-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="13dfa-131">HTTPS</span></span>

<span data-ttu-id="13dfa-132">Le app basate su C-core configurare HTTPS tramite il [Server.Ports proprietà](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="13dfa-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="13dfa-133">Un concetto simile viene utilizzato per configurare i server in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="13dfa-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="13dfa-134">Ad esempio, Usa Kestrel [configurazione dell'endpoint](xref:fundamentals/servers/kestrel#endpoint-configuration) per questa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="13dfa-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="13dfa-135">Middleware e intercettori</span><span class="sxs-lookup"><span data-stu-id="13dfa-135">Interceptors and Middleware</span></span>

<span data-ttu-id="13dfa-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offre funzionalità simili rispetto agli intercettori nelle App basate su C-core gRPC.</span><span class="sxs-lookup"><span data-stu-id="13dfa-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="13dfa-137">Middleware e intercettori concettualmente sono uguali perché entrambi vengono usati per costruire una pipeline che gestisce una richiesta gRPC.</span><span class="sxs-lookup"><span data-stu-id="13dfa-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="13dfa-138">Entrambi consentono operazioni da eseguire prima o dopo il componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="13dfa-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="13dfa-139">Tuttavia, middleware di ASP.NET Core agisce sui messaggi HTTP/2 sottostanti, mentre gli intercettori operano sul livello di astrazione usando gRPC il [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="13dfa-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="13dfa-140">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="13dfa-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
