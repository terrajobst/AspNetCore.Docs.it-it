---
title: Attività in background con servizi ospitati in ASP.NET Core
author: guardrex
description: Informazioni su come implementare attività in background con servizi ospitati in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/hosted-services
ms.openlocfilehash: ace7fc8622864099b7c0e36e4a914de340d4d4e9
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="15678-103">Attività in background con servizi ospitati in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15678-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="15678-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="15678-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="15678-105">In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="15678-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="15678-106">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="15678-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="15678-107">In questo argomento vengono forniti tre esempi di servizio ospitato:</span><span class="sxs-lookup"><span data-stu-id="15678-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="15678-108">Attività in background eseguita su un timer.</span><span class="sxs-lookup"><span data-stu-id="15678-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="15678-109">Servizio ospitato che attiva un servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="15678-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="15678-110">Il servizio con ambito può utilizzare l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="15678-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="15678-111">Attività in background in coda che vengono eseguite in sequenza.</span><span class="sxs-lookup"><span data-stu-id="15678-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="15678-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="15678-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="15678-113">Interfaccia IHostedService</span><span class="sxs-lookup"><span data-stu-id="15678-113">IHostedService interface</span></span>

<span data-ttu-id="15678-114">I servizi ospitati implementano l'interfaccia [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="15678-114">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="15678-115">L'interfaccia definisce due metodi riservati agli oggetti gestiti dall'host:</span><span class="sxs-lookup"><span data-stu-id="15678-115">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="15678-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync): metodo chiamato dopo l'avvio del server e l'attivazione di [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted).</span><span class="sxs-lookup"><span data-stu-id="15678-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="15678-117">`StartAsync` contiene la logica per avviare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="15678-117">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="15678-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync): metodo attivato quando l'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="15678-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="15678-119">`StopAsync` contiene la logica per porre fine all'attività in background ed eliminare le eventuali risorse non gestite.</span><span class="sxs-lookup"><span data-stu-id="15678-119">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="15678-120">Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="15678-120">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="15678-121">Il servizio ospitato è un singleton che viene attivato una volta all'avvio dell'app e all'arresto normale al momento della chiusura dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="15678-121">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="15678-122">Quando [IDisposable](/dotnet/api/system.idisposable) viene implementato, le risorse possono essere eliminate quando viene eliminato il contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="15678-122">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="15678-123">Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="15678-123">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="15678-124">Attività in background programmate</span><span class="sxs-lookup"><span data-stu-id="15678-124">Timed background tasks</span></span>

<span data-ttu-id="15678-125">Un'attività programmata in background utilizza la classe [System.Threading.Timer](/dotnet/api/system.threading.timer).</span><span class="sxs-lookup"><span data-stu-id="15678-125">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="15678-126">Il timer attiva il metodo `DoWork` dell'attività.</span><span class="sxs-lookup"><span data-stu-id="15678-126">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="15678-127">Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="15678-127">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="15678-128">Il servizio è registrato in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="15678-128">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="15678-129">Utilizzo di un servizio con ambito in un'attività in background</span><span class="sxs-lookup"><span data-stu-id="15678-129">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="15678-130">Per utilizzare i servizi con ambito all'interno di un `IHostedService`, creare un ambito.</span><span class="sxs-lookup"><span data-stu-id="15678-130">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="15678-131">Non viene creato automaticamente alcun ambito per un servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="15678-131">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="15678-132">Il servizio dell'attività in background con ambito contiene la logica dell'attività in background.</span><span class="sxs-lookup"><span data-stu-id="15678-132">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="15678-133">Nell'esempio seguente [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) viene inserito nel servizio:</span><span class="sxs-lookup"><span data-stu-id="15678-133">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="15678-134">Il servizio ospitato crea un ambito per risolvere il servizio dell'attività in background con ambito e chiamare il relativo metodo `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="15678-134">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="15678-135">I servizi sono registrati in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="15678-135">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="15678-136">Attività in background in coda</span><span class="sxs-lookup"><span data-stu-id="15678-136">Queued background tasks</span></span>

<span data-ttu-id="15678-137">Una coda di attività in background si basa su .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([provvisoriamente pianificato per essere incorporato in ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="15678-137">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="15678-138">In `QueueHostedService` le attività in background (`workItem`) nella coda vengono rimosse dalla coda ed eseguite:</span><span class="sxs-lookup"><span data-stu-id="15678-138">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="15678-139">I servizi sono registrati in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="15678-139">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

<span data-ttu-id="15678-140">Nella classe modello della pagina di indice `IBackgroundTaskQueue` viene inserito nel costruttore e assegnato a `Queue`:</span><span class="sxs-lookup"><span data-stu-id="15678-140">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="15678-141">Quando si seleziona il pulsante **Aggiungi attività** nella pagina di indice, viene eseguito il metodo `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="15678-141">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="15678-142">`QueueBackgroundWorkItem` viene chiamato per accodare l'elemento di lavoro:</span><span class="sxs-lookup"><span data-stu-id="15678-142">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="15678-143">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="15678-143">Additional resources</span></span>

* [<span data-ttu-id="15678-144">Implementare attività in background in microservizi con IHostedService e la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="15678-144">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="15678-145">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="15678-145">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
