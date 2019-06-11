---
title: Attività in background con servizi ospitati in ASP.NET Core
author: guardrex
description: Informazioni su come implementare attività in background con servizi ospitati in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/03/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 2dbb1a84a380ab06a4be7ecf628799a070afc9e3
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692509"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="4cb5b-103">Attività in background con servizi ospitati in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cb5b-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="4cb5b-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4cb5b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4cb5b-105">In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="4cb5b-106">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="4cb5b-107">In questo argomento vengono forniti tre esempi di servizio ospitato:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="4cb5b-108">Attività in background eseguita su un timer.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="4cb5b-109">Servizio ospitato che attiva un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="4cb5b-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="4cb5b-110">Il servizio con ambito può utilizzare l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="4cb5b-111">Attività in background in coda che vengono eseguite in sequenza.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="4cb5b-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4cb5b-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4cb5b-113">L'app di esempio è disponibile in due versioni:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="4cb5b-114">Host Web &ndash; L'host Web è utile per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="4cb5b-115">Il codice di esempio in questo argomento è tratto dalla versione host Web dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="4cb5b-116">Per altre informazioni, vedere l'argomento [Host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="4cb5b-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="4cb5b-117">Host generico &ndash; L'host generico è una novità di ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="4cb5b-118">Per altre informazioni, vedere l'argomento [Host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="4cb5b-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="4cb5b-119">Modello di servizio di ruolo di lavoro</span><span class="sxs-lookup"><span data-stu-id="4cb5b-119">Worker Service template</span></span>

<span data-ttu-id="4cb5b-120">Il modello di servizio di ruolo di lavoro di ASP.NET Core rappresenta un punto di partenza per la scrittura di app di servizi a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="4cb5b-121">Per usare il modello come base per un'app di servizi ospitati:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4cb5b-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4cb5b-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="4cb5b-123">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-123">Create a new project.</span></span>
1. <span data-ttu-id="4cb5b-124">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="4cb5b-125">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-125">Select **Next**.</span></span>
1. <span data-ttu-id="4cb5b-126">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="4cb5b-127">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-127">Select **Create**.</span></span>
1. <span data-ttu-id="4cb5b-128">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="4cb5b-129">Selezionare il modello del **Servizio di ruolo di lavoro**.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="4cb5b-130">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-130">Select **Create**.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="4cb5b-131">Visual Studio Code/Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="4cb5b-131">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="4cb5b-132">Usare il servizio di ruolo di lavoro (`worker`) con il comando[dotnet new](/dotnet/core/tools/dotnet-new) da una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-132">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="4cb5b-133">Nell'esempio seguente viene creata un'app del servizio di ruolo di lavoro denominata `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-133">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="4cb5b-134">Una cartella per l'app `ContosoWorkerService` viene creata automaticamente quando viene eseguito il comando.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-134">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="package"></a><span data-ttu-id="4cb5b-135">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="4cb5b-135">Package</span></span>

<span data-ttu-id="4cb5b-136">Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="4cb5b-136">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="4cb5b-137">Interfaccia IHostedService</span><span class="sxs-lookup"><span data-stu-id="4cb5b-137">IHostedService interface</span></span>

<span data-ttu-id="4cb5b-138">I servizi ospitati implementano l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-138">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="4cb5b-139">L'interfaccia definisce due metodi riservati agli oggetti gestiti dall'host:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-139">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="4cb5b-140">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contiene la logica per avviare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-140">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="4cb5b-141">Quando si usa l'[Host Web](xref:fundamentals/host/web-host), `StartAsync` viene chiamato dopo l'avvio del server e l'attivazione di [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="4cb5b-141">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="4cb5b-142">Quando si usa l'[Host generico](xref:fundamentals/host/generic-host), `StartAsync` viene chiamato prima dell'attivazione di `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-142">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="4cb5b-143">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Attivato quando l'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-143">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="4cb5b-144">`StopAsync` contiene la logica per terminare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-144">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="4cb5b-145">Implementare <xref:System.IDisposable> e i [finalizzatori (distruttori)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) per eliminare tutte le risorse non gestite.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-145">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="4cb5b-146">Il token di annullamento ha un timeout predefinito di cinque secondi che indica che il processo di arresto non è più normale.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-146">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="4cb5b-147">Quando viene richiesto l'annullamento sul token:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-147">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="4cb5b-148">Tutte le operazioni in background rimanenti che sta eseguendo l'app devono essere interrotte.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-148">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="4cb5b-149">Tutti i metodi eventuali chiamati in `StopAsync` devono essere completati rapidamente.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-149">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="4cb5b-150">Tuttavia, dopo la richiesta di annullamento le attività non vengono abbandonate: il chiamante attende il completamento di tutte le attività.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-150">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="4cb5b-151">Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-151">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="4cb5b-152">Pertanto è possibile che i metodi chiamati o le operazioni effettuate in `StopAsync` non vengano eseguiti.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-152">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="4cb5b-153">Per estendere il timeout di arresto predefinito di cinque secondi, impostare:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-153">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="4cb5b-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quando si usa l'host generico.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="4cb5b-155">Per ulteriori informazioni, vedere <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-155">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="4cb5b-156">Impostazione di configurazione dell'host del timeout di arresto quando si usa l'host Web.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-156">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="4cb5b-157">Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-157">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="4cb5b-158">Il servizio ospitato viene attivato una volta all'avvio dell'app e arrestato normalmente all'arresto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-158">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="4cb5b-159">Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-159">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="4cb5b-160">Attività in background programmate</span><span class="sxs-lookup"><span data-stu-id="4cb5b-160">Timed background tasks</span></span>

<span data-ttu-id="4cb5b-161">Un'attività programmata in background utilizza la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="4cb5b-161">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="4cb5b-162">Il timer attiva il metodo `DoWork` dell'attività.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-162">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="4cb5b-163">Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-163">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="4cb5b-164">Il servizio registrato in `Startup.ConfigureServices` con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-164">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="4cb5b-165">Utilizzo di un servizio con ambito in un'attività in background</span><span class="sxs-lookup"><span data-stu-id="4cb5b-165">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="4cb5b-166">Per usare i [servizi con ambito](xref:fundamentals/dependency-injection#service-lifetimes) all'interno di un `IHostedService`, creare un ambito.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-166">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="4cb5b-167">Non viene creato automaticamente alcun ambito per un servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-167">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="4cb5b-168">Il servizio dell'attività in background con ambito contiene la logica dell'attività in background.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-168">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="4cb5b-169">Nell'esempio seguente, <xref:Microsoft.Extensions.Logging.ILogger> viene inserito nel servizio:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-169">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="4cb5b-170">Il servizio ospitato crea un ambito per risolvere il servizio dell'attività in background con ambito e chiamare il relativo metodo `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-170">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="4cb5b-171">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-171">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="4cb5b-172">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-172">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="4cb5b-173">Attività in background in coda</span><span class="sxs-lookup"><span data-stu-id="4cb5b-173">Queued background tasks</span></span>

<span data-ttu-id="4cb5b-174">Una coda di attività in background si basa su .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([provvisoriamente pianificato per essere incorporato in ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="4cb5b-174">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="4cb5b-175">In `QueueHostedService`, le attività in background in coda vengono rimosse dalla coda ed eseguite come <xref:Microsoft.Extensions.Hosting.BackgroundService>, ovvero una classe di base per l'implementazione di un `IHostedService` a esecuzione prolungata:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-175">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="4cb5b-176">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-176">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="4cb5b-177">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-177">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="4cb5b-178">Nella classe modello della pagina di indice:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-178">In the Index page model class:</span></span>

* <span data-ttu-id="4cb5b-179">`IBackgroundTaskQueue` viene inserito nel costruttore e assegnato a `Queue`.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-179">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="4cb5b-180">Un'interfaccia <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> viene inserita e assegnata a `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-180">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="4cb5b-181">La factory viene usata per creare istanze di <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, che consente di creare servizi all'interno di un ambito.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-181">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="4cb5b-182">Viene creato un ambito per usare la classe `AppDbContext` dell'app (un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes)) per scrivere record di database in `IBackgroundTaskQueue` (un servizio singleton).</span><span class="sxs-lookup"><span data-stu-id="4cb5b-182">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="4cb5b-183">Quando si seleziona il pulsante **Aggiungi attività** nella pagina di indice, viene eseguito il metodo `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="4cb5b-183">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="4cb5b-184">`QueueBackgroundWorkItem` viene chiamato per accodare l'elemento di lavoro:</span><span class="sxs-lookup"><span data-stu-id="4cb5b-184">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="4cb5b-185">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4cb5b-185">Additional resources</span></span>

* [<span data-ttu-id="4cb5b-186">Implementare attività in background in microservizi con IHostedService e la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="4cb5b-186">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
