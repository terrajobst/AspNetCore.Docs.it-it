---
title: Migrazione dei servizi gRPC da C-core a ASP.NET Core
author: juntaoluo
description: Informazioni su come spostare un'app gRPC basata su C-core esistente per l'esecuzione in ASP.NET Core stack.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/25/2019
uid: grpc/migration
ms.openlocfilehash: 8f0d9dd980fa3281f30dc29d329d10ccd352ae72
ms.sourcegitcommit: 994da92edb0abf856b1655c18880028b15a28897
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2019
ms.locfileid: "71278709"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="fb858-103">Migrazione dei servizi gRPC da C-core a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb858-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="fb858-104">A cura di [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="fb858-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="fb858-105">A causa dell'implementazione dello stack sottostante, non tutte le funzionalità funzionano allo stesso modo tra le app [gRPC basate su C-core](https://grpc.io/blog/grpc-stacks) e le app basate su ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb858-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="fb858-106">Questo documento evidenzia le principali differenze per la migrazione tra i due stack.</span><span class="sxs-lookup"><span data-stu-id="fb858-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="fb858-107">durata dell'implementazione del servizio gRPC</span><span class="sxs-lookup"><span data-stu-id="fb858-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="fb858-108">Nello stack ASP.NET Core, per impostazione predefinita, i servizi gRPC vengono creati con una [durata con ambito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="fb858-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="fb858-109">Al contrario, gRPC C-core per impostazione predefinita viene associato a un servizio con una [durata singleton](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="fb858-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="fb858-110">Una durata con ambito consente all'implementazione del servizio di risolvere altri servizi con durata con ambito.</span><span class="sxs-lookup"><span data-stu-id="fb858-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="fb858-111">Ad esempio, una durata con ambito può essere risolta `DbContext` anche dal contenitore di inserimento delle dipendenze tramite l'inserimento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="fb858-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="fb858-112">Uso della durata con ambito:</span><span class="sxs-lookup"><span data-stu-id="fb858-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="fb858-113">Per ogni richiesta viene costruita una nuova istanza dell'implementazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="fb858-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="fb858-114">Non è possibile condividere lo stato tra le richieste tramite i membri di istanza nel tipo di implementazione.</span><span class="sxs-lookup"><span data-stu-id="fb858-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="fb858-115">Si prevede di archiviare gli stati condivisi in un servizio singleton nel contenitore DI inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="fb858-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="fb858-116">Gli stati condivisi archiviati vengono risolti nel costruttore dell'implementazione del servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="fb858-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="fb858-117">Per ulteriori informazioni sulla durata del servizio, vedere <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="fb858-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="fb858-118">Aggiungere un servizio singleton</span><span class="sxs-lookup"><span data-stu-id="fb858-118">Add a singleton service</span></span>

<span data-ttu-id="fb858-119">Per semplificare la transizione da un'implementazione di gRPC C-core a ASP.NET Core, è possibile modificare la durata del servizio dell'implementazione del servizio dall'ambito a Singleton.</span><span class="sxs-lookup"><span data-stu-id="fb858-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="fb858-120">Ciò comporta l'aggiunta di un'istanza dell'implementazione del servizio al contenitore DI DI:</span><span class="sxs-lookup"><span data-stu-id="fb858-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="fb858-121">Tuttavia, l'implementazione di un servizio con una durata singleton non è più in grado di risolvere i servizi con ambito tramite l'inserimento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="fb858-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="fb858-122">Configurare le opzioni dei servizi gRPC</span><span class="sxs-lookup"><span data-stu-id="fb858-122">Configure gRPC services options</span></span>

<span data-ttu-id="fb858-123">Nelle app basate su C-core, le impostazioni come `grpc.max_receive_message_length` e `grpc.max_send_message_length` vengono configurate con `ChannelOption` quando si [costruisce l'istanza del server](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="fb858-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="fb858-124">In ASP.NET Core, gRPC fornisce la configurazione tramite `GrpcServiceOptions` il tipo.</span><span class="sxs-lookup"><span data-stu-id="fb858-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="fb858-125">Ad esempio, la dimensione massima del messaggio in ingresso di un servizio gRPC può essere configurata tramite `AddGrpc`.</span><span class="sxs-lookup"><span data-stu-id="fb858-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`.</span></span> <span data-ttu-id="fb858-126">Nell'esempio seguente viene modificato il `ReceiveMaxMessageSize` valore predefinito di 4 MB in 16 MB:</span><span class="sxs-lookup"><span data-stu-id="fb858-126">The following example changes the default `ReceiveMaxMessageSize` of 4 MB to 16 MB:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

<span data-ttu-id="fb858-127">Per ulteriori informazioni sulla configurazione, vedere <xref:grpc/configuration>.</span><span class="sxs-lookup"><span data-stu-id="fb858-127">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="fb858-128">Registrazione</span><span class="sxs-lookup"><span data-stu-id="fb858-128">Logging</span></span>

<span data-ttu-id="fb858-129">Le app basate su C-Core `GrpcEnvironment` si basano su per [configurare il logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) a scopo di debug.</span><span class="sxs-lookup"><span data-stu-id="fb858-129">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="fb858-130">Lo stack di ASP.NET Core fornisce questa funzionalità tramite l' [API di registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="fb858-130">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="fb858-131">Ad esempio, è possibile aggiungere un logger al servizio gRPC tramite l'inserimento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="fb858-131">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="fb858-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="fb858-132">HTTPS</span></span>

<span data-ttu-id="fb858-133">Le app basate su C-core configurano HTTPS tramite la [Proprietà Server. Ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="fb858-133">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="fb858-134">Un concetto simile viene usato per configurare i server in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fb858-134">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="fb858-135">Ad esempio, gheppio usa la [configurazione dell'endpoint](xref:fundamentals/servers/kestrel#endpoint-configuration) per questa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="fb858-135">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="fb858-136">Intercettori e middleware</span><span class="sxs-lookup"><span data-stu-id="fb858-136">Interceptors and Middleware</span></span>

<span data-ttu-id="fb858-137">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offre funzionalità simili rispetto agli intercettori nelle app gRPC basate su C core.</span><span class="sxs-lookup"><span data-stu-id="fb858-137">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="fb858-138">Il middleware e gli intercettori sono concettualmente identici a quelli usati per costruire una pipeline che gestisce una richiesta gRPC.</span><span class="sxs-lookup"><span data-stu-id="fb858-138">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="fb858-139">Entrambi consentono di eseguire il lavoro prima o dopo il componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="fb858-139">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="fb858-140">Tuttavia, ASP.NET Core middleware opera sui messaggi HTTP/2 sottostanti, mentre gli intercettori operano sul livello gRPC dell'astrazione usando [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="fb858-140">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fb858-141">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fb858-141">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
