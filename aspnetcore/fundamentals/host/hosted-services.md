---
title: Attività in background con servizi ospitati in ASP.NET Core
author: guardrex
description: Informazioni su come implementare attività in background con servizi ospitati in ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 13ac7e266b657bc186188b2b6f40204cfd936fca
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341821"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="c75fe-103">Attività in background con servizi ospitati in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c75fe-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="c75fe-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c75fe-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c75fe-105">In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="c75fe-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="c75fe-106">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="c75fe-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="c75fe-107">In questo argomento vengono forniti tre esempi di servizio ospitato:</span><span class="sxs-lookup"><span data-stu-id="c75fe-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="c75fe-108">Attività in background eseguita su un timer.</span><span class="sxs-lookup"><span data-stu-id="c75fe-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="c75fe-109">Servizio ospitato che attiva un servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="c75fe-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="c75fe-110">Il servizio con ambito può utilizzare l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="c75fe-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="c75fe-111">Attività in background in coda che vengono eseguite in sequenza.</span><span class="sxs-lookup"><span data-stu-id="c75fe-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="c75fe-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c75fe-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c75fe-113">L'app di esempio è disponibile in due versioni:</span><span class="sxs-lookup"><span data-stu-id="c75fe-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="c75fe-114">Host Web &ndash; L'host Web è utile per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="c75fe-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="c75fe-115">Il codice di esempio in questo argomento è tratto dalla versione host Web dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="c75fe-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="c75fe-116">Per altre informazioni, vedere l'argomento [Host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="c75fe-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="c75fe-117">Host generico &ndash; L'host generico è una novità di ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="c75fe-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="c75fe-118">Per altre informazioni, vedere l'argomento [Host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="c75fe-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker-end

## <a name="ihostedservice-interface"></a><span data-ttu-id="c75fe-119">Interfaccia IHostedService</span><span class="sxs-lookup"><span data-stu-id="c75fe-119">IHostedService interface</span></span>

<span data-ttu-id="c75fe-120">I servizi ospitati implementano l'interfaccia [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="c75fe-120">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="c75fe-121">L'interfaccia definisce due metodi riservati agli oggetti gestiti dall'host:</span><span class="sxs-lookup"><span data-stu-id="c75fe-121">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="c75fe-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync): metodo chiamato dopo l'avvio del server e l'attivazione di [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted).</span><span class="sxs-lookup"><span data-stu-id="c75fe-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="c75fe-123">`StartAsync` contiene la logica per avviare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="c75fe-123">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="c75fe-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync): metodo attivato quando l'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="c75fe-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="c75fe-125">`StopAsync` contiene la logica per porre fine all'attività in background ed eliminare le eventuali risorse non gestite.</span><span class="sxs-lookup"><span data-stu-id="c75fe-125">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="c75fe-126">Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="c75fe-126">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="c75fe-127">Il servizio ospitato viene attivato una volta all'avvio dell'app e all'arresto normale al momento della chiusura dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c75fe-127">The hosted service is activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="c75fe-128">Quando [IDisposable](/dotnet/api/system.idisposable) viene implementato, le risorse possono essere eliminate quando viene eliminato il contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="c75fe-128">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="c75fe-129">Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="c75fe-129">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="c75fe-130">Attività in background programmate</span><span class="sxs-lookup"><span data-stu-id="c75fe-130">Timed background tasks</span></span>

<span data-ttu-id="c75fe-131">Un'attività programmata in background utilizza la classe [System.Threading.Timer](/dotnet/api/system.threading.timer).</span><span class="sxs-lookup"><span data-stu-id="c75fe-131">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="c75fe-132">Il timer attiva il metodo `DoWork` dell'attività.</span><span class="sxs-lookup"><span data-stu-id="c75fe-132">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="c75fe-133">Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="c75fe-133">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c75fe-134">Il servizio registrato in `Startup.ConfigureServices` con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c75fe-134">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="c75fe-135">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c75fe-135">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c75fe-136">Il servizio è registrato in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c75fe-136">The service is registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="c75fe-137">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c75fe-137">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="c75fe-138">Utilizzo di un servizio con ambito in un'attività in background</span><span class="sxs-lookup"><span data-stu-id="c75fe-138">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="c75fe-139">Per utilizzare i servizi con ambito all'interno di un `IHostedService`, creare un ambito.</span><span class="sxs-lookup"><span data-stu-id="c75fe-139">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="c75fe-140">Non viene creato automaticamente alcun ambito per un servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="c75fe-140">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="c75fe-141">Il servizio dell'attività in background con ambito contiene la logica dell'attività in background.</span><span class="sxs-lookup"><span data-stu-id="c75fe-141">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="c75fe-142">Nell'esempio seguente [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) viene inserito nel servizio:</span><span class="sxs-lookup"><span data-stu-id="c75fe-142">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="c75fe-143">Il servizio ospitato crea un ambito per risolvere il servizio dell'attività in background con ambito e chiamare il relativo metodo `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="c75fe-143">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c75fe-144">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c75fe-144">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c75fe-145">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c75fe-145">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="c75fe-146">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="c75fe-146">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c75fe-147">I servizi sono registrati in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c75fe-147">The services are registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="c75fe-148">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="c75fe-148">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span></span>

::: moniker-end

## <a name="queued-background-tasks"></a><span data-ttu-id="c75fe-149">Attività in background in coda</span><span class="sxs-lookup"><span data-stu-id="c75fe-149">Queued background tasks</span></span>

<span data-ttu-id="c75fe-150">Una coda di attività in background si basa su .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([provvisoriamente pianificato per essere incorporato in ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="c75fe-150">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="c75fe-151">In `QueueHostedService` le attività in background (`workItem`) nella coda vengono rimosse dalla coda ed eseguite:</span><span class="sxs-lookup"><span data-stu-id="c75fe-151">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c75fe-152">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c75fe-152">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c75fe-153">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c75fe-153">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="c75fe-154">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="c75fe-154">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c75fe-155">I servizi sono registrati in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c75fe-155">The services are registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="c75fe-156">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="c75fe-156">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span></span>

::: moniker-end

<span data-ttu-id="c75fe-157">Nella classe modello della pagina di indice `IBackgroundTaskQueue` viene inserito nel costruttore e assegnato a `Queue`:</span><span class="sxs-lookup"><span data-stu-id="c75fe-157">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="c75fe-158">Quando si seleziona il pulsante **Aggiungi attività** nella pagina di indice, viene eseguito il metodo `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="c75fe-158">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="c75fe-159">`QueueBackgroundWorkItem` viene chiamato per accodare l'elemento di lavoro:</span><span class="sxs-lookup"><span data-stu-id="c75fe-159">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="c75fe-160">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c75fe-160">Additional resources</span></span>

* [<span data-ttu-id="c75fe-161">Implementare attività in background in microservizi con IHostedService e la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="c75fe-161">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="c75fe-162">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="c75fe-162">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
