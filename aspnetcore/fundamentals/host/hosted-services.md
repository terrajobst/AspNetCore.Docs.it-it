---
title: Attività in background con servizi ospitati in ASP.NET Core
author: guardrex
description: Informazioni su come implementare attività in background con servizi ospitati in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 92905d86cb963d01f1806f08d07b270a7f6d8563
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207407"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="37aa3-103">Attività in background con servizi ospitati in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37aa3-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="37aa3-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="37aa3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="37aa3-105">In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="37aa3-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="37aa3-106">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="37aa3-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="37aa3-107">In questo argomento vengono forniti tre esempi di servizio ospitato:</span><span class="sxs-lookup"><span data-stu-id="37aa3-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="37aa3-108">Attività in background eseguita su un timer.</span><span class="sxs-lookup"><span data-stu-id="37aa3-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="37aa3-109">Servizio ospitato che attiva un servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="37aa3-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="37aa3-110">Il servizio con ambito può utilizzare l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="37aa3-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="37aa3-111">Attività in background in coda che vengono eseguite in sequenza.</span><span class="sxs-lookup"><span data-stu-id="37aa3-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="37aa3-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37aa3-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="37aa3-113">L'app di esempio è disponibile in due versioni:</span><span class="sxs-lookup"><span data-stu-id="37aa3-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="37aa3-114">Host Web &ndash; L'host Web è utile per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="37aa3-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="37aa3-115">Il codice di esempio in questo argomento è tratto dalla versione host Web dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="37aa3-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="37aa3-116">Per altre informazioni, vedere l'argomento [Host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="37aa3-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="37aa3-117">Host generico &ndash; L'host generico è una novità di ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="37aa3-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="37aa3-118">Per altre informazioni, vedere l'argomento [Host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="37aa3-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="37aa3-119">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="37aa3-119">Package</span></span>

<span data-ttu-id="37aa3-120">Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="37aa3-120">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="37aa3-121">Interfaccia IHostedService</span><span class="sxs-lookup"><span data-stu-id="37aa3-121">IHostedService interface</span></span>

<span data-ttu-id="37aa3-122">I servizi ospitati implementano l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="37aa3-122">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="37aa3-123">L'interfaccia definisce due metodi riservati agli oggetti gestiti dall'host:</span><span class="sxs-lookup"><span data-stu-id="37aa3-123">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="37aa3-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) - `StartAsync` contiene la logica per avviare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="37aa3-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) - `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="37aa3-125">Quando si usa l'[Host Web](xref:fundamentals/host/web-host), `StartAsync` viene chiamato dopo l'avvio del server e l'attivazione di [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="37aa3-125">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="37aa3-126">Quando si usa l'[Host generico](xref:fundamentals/host/generic-host), `StartAsync` viene chiamato prima dell'attivazione di `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="37aa3-126">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="37aa3-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*): metodo attivato quando l'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="37aa3-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="37aa3-128">`StopAsync` contiene la logica per porre fine all'attività in background ed eliminare le eventuali risorse non gestite.</span><span class="sxs-lookup"><span data-stu-id="37aa3-128">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="37aa3-129">Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="37aa3-129">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="37aa3-130">Il servizio ospitato viene attivato una volta all'avvio dell'app e all'arresto normale al momento della chiusura dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="37aa3-130">The hosted service is activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="37aa3-131">Se viene implementato <xref:System.IDisposable>, le risorse possono essere eliminate quando viene eliminato il contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="37aa3-131">When <xref:System.IDisposable> is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="37aa3-132">Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="37aa3-132">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="37aa3-133">Attività in background programmate</span><span class="sxs-lookup"><span data-stu-id="37aa3-133">Timed background tasks</span></span>

<span data-ttu-id="37aa3-134">Un'attività programmata in background utilizza la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="37aa3-134">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="37aa3-135">Il timer attiva il metodo `DoWork` dell'attività.</span><span class="sxs-lookup"><span data-stu-id="37aa3-135">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="37aa3-136">Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="37aa3-136">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="37aa3-137">Il servizio registrato in `Startup.ConfigureServices` con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="37aa3-137">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="37aa3-138">Utilizzo di un servizio con ambito in un'attività in background</span><span class="sxs-lookup"><span data-stu-id="37aa3-138">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="37aa3-139">Per utilizzare i servizi con ambito all'interno di un `IHostedService`, creare un ambito.</span><span class="sxs-lookup"><span data-stu-id="37aa3-139">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="37aa3-140">Non viene creato automaticamente alcun ambito per un servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="37aa3-140">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="37aa3-141">Il servizio dell'attività in background con ambito contiene la logica dell'attività in background.</span><span class="sxs-lookup"><span data-stu-id="37aa3-141">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="37aa3-142">Nell'esempio seguente, <xref:Microsoft.Extensions.Logging.ILogger> viene inserito nel servizio:</span><span class="sxs-lookup"><span data-stu-id="37aa3-142">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="37aa3-143">Il servizio ospitato crea un ambito per risolvere il servizio dell'attività in background con ambito e chiamare il relativo metodo `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="37aa3-143">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="37aa3-144">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="37aa3-144">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="37aa3-145">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="37aa3-145">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="37aa3-146">Attività in background in coda</span><span class="sxs-lookup"><span data-stu-id="37aa3-146">Queued background tasks</span></span>

<span data-ttu-id="37aa3-147">Una coda di attività in background si basa su .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([provvisoriamente pianificato per essere incorporato in ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="37aa3-147">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="37aa3-148">In `QueueHostedService`, le attività in background in coda vengono rimosse dalla coda ed eseguite come <xref:Microsoft.Extensions.Hosting.BackgroundService>, ovvero una classe di base per l'implementazione di un `IHostedService` a esecuzione prolungata:</span><span class="sxs-lookup"><span data-stu-id="37aa3-148">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="37aa3-149">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="37aa3-149">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="37aa3-150">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="37aa3-150">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="37aa3-151">Nella classe modello della pagina di indice `IBackgroundTaskQueue` viene inserito nel costruttore e assegnato a `Queue`:</span><span class="sxs-lookup"><span data-stu-id="37aa3-151">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="37aa3-152">Quando si seleziona il pulsante **Aggiungi attività** nella pagina di indice, viene eseguito il metodo `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="37aa3-152">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="37aa3-153">`QueueBackgroundWorkItem` viene chiamato per accodare l'elemento di lavoro:</span><span class="sxs-lookup"><span data-stu-id="37aa3-153">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="37aa3-154">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="37aa3-154">Additional resources</span></span>

* [<span data-ttu-id="37aa3-155">Implementare attività in background in microservizi con IHostedService e la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="37aa3-155">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="37aa3-156">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="37aa3-156">System.Threading.Timer</span></span>](xref:System.Threading.Timer)
