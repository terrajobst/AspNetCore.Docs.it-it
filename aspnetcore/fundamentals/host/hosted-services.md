---
title: Attività in background con servizi ospitati in ASP.NET Core
author: guardrex
description: Informazioni su come implementare attività in background con servizi ospitati in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 0eaa3a62370c1e413840bb65f597dc664adafc38
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71688111"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="c9ae2-103">Attività in background con servizi ospitati in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9ae2-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="c9ae2-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c9ae2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c9ae2-105">In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="c9ae2-106">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="c9ae2-107">In questo argomento vengono forniti tre esempi di servizio ospitato:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="c9ae2-108">Attività in background eseguita su un timer.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="c9ae2-109">Servizio ospitato che attiva un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="c9ae2-110">Il servizio con ambito può usare l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="c9ae2-111">Attività in background in coda che vengono eseguite in sequenza.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="c9ae2-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c9ae2-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c9ae2-113">L'app di esempio è disponibile in due versioni:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="c9ae2-114">Host Web &ndash; L'host Web è utile per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="c9ae2-115">Il codice di esempio in questo argomento è tratto dalla versione host Web dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="c9ae2-116">Per altre informazioni, vedere l'argomento [Host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="c9ae2-117">Host generico &ndash; L'host generico è una novità di ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="c9ae2-118">Per altre informazioni, vedere l'argomento [Host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="c9ae2-119">Modello di servizio di ruolo di lavoro</span><span class="sxs-lookup"><span data-stu-id="c9ae2-119">Worker Service template</span></span>

<span data-ttu-id="c9ae2-120">Il modello di servizio di ruolo di lavoro di ASP.NET Core rappresenta un punto di partenza per la scrittura di app di servizi a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="c9ae2-121">Per usare il modello come base per un'app di servizi ospitati:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-121">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a><span data-ttu-id="c9ae2-122">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="c9ae2-122">Package</span></span>

<span data-ttu-id="c9ae2-123">Un riferimento al pacchetto [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) viene aggiunto in modo implicito per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-123">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="c9ae2-124">Interfaccia IHostedService</span><span class="sxs-lookup"><span data-stu-id="c9ae2-124">IHostedService interface</span></span>

<span data-ttu-id="c9ae2-125">L' <xref:Microsoft.Extensions.Hosting.IHostedService> interfaccia definisce due metodi per gli oggetti gestiti dall'host:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-125">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="c9ae2-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contiene la logica per avviare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="c9ae2-127">`StartAsync`viene chiamato *prima*di:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-127">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="c9ae2-128">La pipeline di elaborazione delle richieste dell'app è`Startup.Configure`configurata ().</span><span class="sxs-lookup"><span data-stu-id="c9ae2-128">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="c9ae2-129">Il server viene avviato e viene attivato [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="c9ae2-129">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="c9ae2-130">Il comportamento predefinito può essere modificato in modo che il servizio `StartAsync` ospitato venga eseguito dopo che la pipeline dell'app è stata configurata e `ApplicationStarted` chiamata.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-130">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="c9ae2-131">Per modificare il comportamento predefinito, aggiungere il servizio ospitato (`VideosWatcher` nell'esempio seguente) dopo la chiamata `ConfigureWebHostDefaults`a:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-131">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Hosting;
  using Microsoft.Extensions.DependencyInjection;
  using Microsoft.Extensions.Hosting;

  public class Program
  {
      public static void Main(string[] args)
      {
          CreateHostBuilder(args).Build().Run();
      }

      public static IHostBuilder CreateHostBuilder(string[] args) =>
          Host.CreateDefaultBuilder(args)
              .ConfigureWebHostDefaults(webBuilder =>
              {
                  webBuilder.UseStartup<Startup>();
              })
              .ConfigureServices(services =>
              {
                  services.AddHostedService<VideosWatcher>();
              });
  }
  ```

* <span data-ttu-id="c9ae2-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Attivato quando l'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="c9ae2-133">`StopAsync` contiene la logica per terminare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-133">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="c9ae2-134">Implementare <xref:System.IDisposable> e i [finalizzatori (distruttori)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) per eliminare tutte le risorse non gestite.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-134">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="c9ae2-135">Il token di annullamento ha un timeout predefinito di cinque secondi che indica che il processo di arresto non è più normale.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-135">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="c9ae2-136">Quando viene richiesto l'annullamento sul token:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-136">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="c9ae2-137">Tutte le operazioni in background rimanenti che sta eseguendo l'app devono essere interrotte.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-137">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="c9ae2-138">Tutti i metodi eventuali chiamati in `StopAsync` devono essere completati rapidamente.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-138">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="c9ae2-139">Tuttavia, dopo la richiesta di annullamento le attività non vengono abbandonate: il chiamante attende il completamento di tutte le attività.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-139">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="c9ae2-140">Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-140">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="c9ae2-141">Pertanto è possibile che i metodi chiamati o le operazioni effettuate in `StopAsync` non vengano eseguiti.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-141">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="c9ae2-142">Per estendere il timeout di arresto predefinito di cinque secondi, impostare:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-142">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="c9ae2-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quando si usa l'host generico.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="c9ae2-144">Per altre informazioni, vedere <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-144">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="c9ae2-145">Impostazione di configurazione dell'host del timeout di arresto quando si usa l'host Web.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-145">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="c9ae2-146">Per altre informazioni, vedere <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-146">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="c9ae2-147">Il servizio ospitato viene attivato una volta all'avvio dell'app e arrestato normalmente all'arresto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-147">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="c9ae2-148">Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-148">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="c9ae2-149">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="c9ae2-149">BackgroundService</span></span>

<span data-ttu-id="c9ae2-150">`BackgroundService`è una classe di base per l'implementazione di <xref:Microsoft.Extensions.Hosting.IHostedService>un oggetto a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-150">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="c9ae2-151">`BackgroundService`fornisce il `ExecuteAsync(CancellationToken stoppingToken)` metodo astratto per contenere la logica del servizio.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-151">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="c9ae2-152">Viene attivato quando viene chiamato [IHostedService. StopAsync.](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) `stoppingToken`</span><span class="sxs-lookup"><span data-stu-id="c9ae2-152">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="c9ae2-153">L'implementazione di questo metodo restituisce un `Task` oggetto che rappresenta l'intera durata del servizio in background.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-153">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="c9ae2-154">Inoltre, *facoltativamente* , eseguire l'override dei metodi `IHostedService` definiti in per eseguire il codice di avvio e arresto per il servizio:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-154">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="c9ae2-155">`StopAsync(CancellationToken cancellationToken)`&ndash; vienechiamatoquandol'hostdell'applicazionesta`StopAsync` eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="c9ae2-156">`cancellationToken` Viene segnalato quando l'host decide di terminare forzatamente il servizio.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-156">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="c9ae2-157">Se viene eseguito l'override di questo metodo, è **necessario** chiamare `await`(e) il metodo della classe di base per verificare che il servizio venga arrestato correttamente.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-157">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="c9ae2-158">`StartAsync(CancellationToken cancellationToken)`&ndash; vienechiamatoperavviare`StartAsync` il servizio in background.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-158">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="c9ae2-159">`cancellationToken` Viene segnalato se il processo di avvio viene interrotto.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-159">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="c9ae2-160">L'implementazione restituisce un `Task` oggetto che rappresenta il processo di avvio per il servizio.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-160">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="c9ae2-161">Non vengono avviati altri servizi fino `Task` a quando l'operazione non viene completata.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-161">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="c9ae2-162">Se viene eseguito l'override di questo metodo, è **necessario** chiamare `await`(e) il metodo della classe di base per verificare che il servizio venga avviato correttamente.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-162">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="c9ae2-163">Attività in background programmate</span><span class="sxs-lookup"><span data-stu-id="c9ae2-163">Timed background tasks</span></span>

<span data-ttu-id="c9ae2-164">Un'attività programmata in background utilizza la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-164">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="c9ae2-165">Il timer attiva il metodo `DoWork` dell'attività.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-165">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="c9ae2-166">Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-166">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="c9ae2-167">Il servizio è registrato in `IHostBuilder.ConfigureServices` (*Program.cs*) con il `AddHostedService` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-167">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="c9ae2-168">Utilizzo di un servizio con ambito in un'attività in background</span><span class="sxs-lookup"><span data-stu-id="c9ae2-168">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="c9ae2-169">Per usare i [servizi con ambito](xref:fundamentals/dependency-injection#service-lifetimes) all' `BackgroundService`interno di un, creare un ambito.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-169">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="c9ae2-170">Non viene creato automaticamente alcun ambito per un servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-170">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="c9ae2-171">Il servizio dell'attività in background con ambito contiene la logica dell'attività in background.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-171">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="c9ae2-172">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-172">In the following example:</span></span>

* <span data-ttu-id="c9ae2-173">Il servizio è asincrono.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-173">The service is asynchronous.</span></span> <span data-ttu-id="c9ae2-174">Il metodo `DoWork` restituisce un tipo `Task`.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-174">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="c9ae2-175">A scopo dimostrativo, nel `DoWork` metodo è atteso un ritardo di dieci secondi.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-175">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="c9ae2-176">Un <xref:Microsoft.Extensions.Logging.ILogger> oggetto viene inserito nel servizio.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-176">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="c9ae2-177">Il servizio ospitato crea un ambito per risolvere il servizio attività in background con ambito per chiamare `DoWork` il relativo metodo.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-177">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="c9ae2-178">`DoWork`Restituisce un `Task`oggetto, che è atteso `ExecuteAsync`in:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-178">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="c9ae2-179">I servizi sono registrati in `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-179">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="c9ae2-180">Il servizio ospitato viene registrato con il `AddHostedService` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-180">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="c9ae2-181">Attività in background in coda</span><span class="sxs-lookup"><span data-stu-id="c9ae2-181">Queued background tasks</span></span>

<span data-ttu-id="c9ae2-182">Una coda delle attività in background è basata su .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([programma provvisoriamente per essere incorporato per ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="c9ae2-182">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="c9ae2-183">Nell'esempio seguente `QueueHostedService` :</span><span class="sxs-lookup"><span data-stu-id="c9ae2-183">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="c9ae2-184">Il `BackgroundProcessing` metodo restituisce un `Task`oggetto, che è atteso `ExecuteAsync`in.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-184">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="c9ae2-185">Le attività in background nella coda vengono rimesse in coda ed `BackgroundProcessing`eseguite in.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-185">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="c9ae2-186">Un `MonitorLoop` servizio gestisce le attività di Accodamento per il servizio ospitato ogni `w` volta che la chiave viene selezionata in un dispositivo di input:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-186">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="c9ae2-187">`IBackgroundTaskQueue` Viene inserito`MonitorLoop` nel servizio.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-187">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="c9ae2-188">`IBackgroundTaskQueue.QueueBackgroundWorkItem`viene chiamato per accodare un elemento di lavoro.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-188">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="c9ae2-189">I servizi sono registrati in `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-189">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="c9ae2-190">Il servizio ospitato viene registrato con il `AddHostedService` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-190">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c9ae2-191">In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-191">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="c9ae2-192">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-192">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="c9ae2-193">In questo argomento vengono forniti tre esempi di servizio ospitato:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-193">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="c9ae2-194">Attività in background eseguita su un timer.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-194">Background task that runs on a timer.</span></span>
* <span data-ttu-id="c9ae2-195">Servizio ospitato che attiva un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-195">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="c9ae2-196">Il servizio con ambito può usare l' [inserimento di dipendenze](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="c9ae2-196">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="c9ae2-197">Attività in background in coda che vengono eseguite in sequenza.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-197">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="c9ae2-198">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c9ae2-198">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c9ae2-199">L'app di esempio è disponibile in due versioni:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-199">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="c9ae2-200">Host Web &ndash; L'host Web è utile per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-200">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="c9ae2-201">Il codice di esempio in questo argomento è tratto dalla versione host Web dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-201">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="c9ae2-202">Per altre informazioni, vedere l'argomento [Host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-202">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="c9ae2-203">Host generico &ndash; L'host generico è una novità di ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-203">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="c9ae2-204">Per altre informazioni, vedere l'argomento [Host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-204">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="c9ae2-205">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="c9ae2-205">Package</span></span>

<span data-ttu-id="c9ae2-206">Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-206">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="c9ae2-207">Interfaccia IHostedService</span><span class="sxs-lookup"><span data-stu-id="c9ae2-207">IHostedService interface</span></span>

<span data-ttu-id="c9ae2-208">I servizi ospitati implementano l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-208">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="c9ae2-209">L'interfaccia definisce due metodi riservati agli oggetti gestiti dall'host:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-209">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="c9ae2-210">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contiene la logica per avviare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-210">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="c9ae2-211">Quando si usa l'[Host Web](xref:fundamentals/host/web-host), `StartAsync` viene chiamato dopo l'avvio del server e l'attivazione di [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-211">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="c9ae2-212">Quando si usa l'[Host generico](xref:fundamentals/host/generic-host), `StartAsync` viene chiamato prima dell'attivazione di `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-212">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="c9ae2-213">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Attivato quando l'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-213">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="c9ae2-214">`StopAsync` contiene la logica per terminare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-214">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="c9ae2-215">Implementare <xref:System.IDisposable> e i [finalizzatori (distruttori)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) per eliminare tutte le risorse non gestite.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-215">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="c9ae2-216">Il token di annullamento ha un timeout predefinito di cinque secondi che indica che il processo di arresto non è più normale.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-216">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="c9ae2-217">Quando viene richiesto l'annullamento sul token:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-217">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="c9ae2-218">Tutte le operazioni in background rimanenti che sta eseguendo l'app devono essere interrotte.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-218">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="c9ae2-219">Tutti i metodi eventuali chiamati in `StopAsync` devono essere completati rapidamente.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-219">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="c9ae2-220">Tuttavia, dopo la richiesta di annullamento le attività non vengono abbandonate: il chiamante attende il completamento di tutte le attività.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-220">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="c9ae2-221">Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-221">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="c9ae2-222">Pertanto è possibile che i metodi chiamati o le operazioni effettuate in `StopAsync` non vengano eseguiti.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-222">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="c9ae2-223">Per estendere il timeout di arresto predefinito di cinque secondi, impostare:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-223">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="c9ae2-224"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quando si usa l'host generico.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-224"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="c9ae2-225">Per altre informazioni, vedere <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-225">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="c9ae2-226">Impostazione di configurazione dell'host del timeout di arresto quando si usa l'host Web.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-226">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="c9ae2-227">Per altre informazioni, vedere <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-227">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="c9ae2-228">Il servizio ospitato viene attivato una volta all'avvio dell'app e arrestato normalmente all'arresto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-228">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="c9ae2-229">Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-229">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="c9ae2-230">Attività in background programmate</span><span class="sxs-lookup"><span data-stu-id="c9ae2-230">Timed background tasks</span></span>

<span data-ttu-id="c9ae2-231">Un'attività programmata in background utilizza la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-231">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="c9ae2-232">Il timer attiva il metodo `DoWork` dell'attività.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-232">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="c9ae2-233">Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-233">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="c9ae2-234">Il servizio registrato in `Startup.ConfigureServices` con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-234">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="c9ae2-235">Utilizzo di un servizio con ambito in un'attività in background</span><span class="sxs-lookup"><span data-stu-id="c9ae2-235">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="c9ae2-236">Per usare i [servizi con ambito](xref:fundamentals/dependency-injection#service-lifetimes) all'interno di un `IHostedService`, creare un ambito.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-236">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="c9ae2-237">Non viene creato automaticamente alcun ambito per un servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-237">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="c9ae2-238">Il servizio dell'attività in background con ambito contiene la logica dell'attività in background.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-238">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="c9ae2-239">Nell'esempio seguente, <xref:Microsoft.Extensions.Logging.ILogger> viene inserito nel servizio:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-239">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="c9ae2-240">Il servizio ospitato crea un ambito per risolvere il servizio dell'attività in background con ambito e chiamare il relativo metodo `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-240">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="c9ae2-241">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-241">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c9ae2-242">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-242">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="c9ae2-243">Attività in background in coda</span><span class="sxs-lookup"><span data-stu-id="c9ae2-243">Queued background tasks</span></span>

<span data-ttu-id="c9ae2-244">Una coda delle attività in background è basata su .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([programma provvisoriamente per essere incorporato per ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="c9ae2-244">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="c9ae2-245">In `QueueHostedService`, le attività in background in coda vengono rimosse dalla coda ed eseguite come <xref:Microsoft.Extensions.Hosting.BackgroundService>, ovvero una classe di base per l'implementazione di un `IHostedService` a esecuzione prolungata:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-245">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="c9ae2-246">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-246">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c9ae2-247">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-247">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="c9ae2-248">Nella classe modello della pagina di indice:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-248">In the Index page model class:</span></span>

* <span data-ttu-id="c9ae2-249">`IBackgroundTaskQueue` viene inserito nel costruttore e assegnato a `Queue`.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-249">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="c9ae2-250">Un'interfaccia <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> viene inserita e assegnata a `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-250">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="c9ae2-251">La factory viene usata per creare istanze di <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, che consente di creare servizi all'interno di un ambito.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-251">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="c9ae2-252">Viene creato un ambito per usare la classe `AppDbContext` dell'app (un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes)) per scrivere record di database in `IBackgroundTaskQueue` (un servizio singleton).</span><span class="sxs-lookup"><span data-stu-id="c9ae2-252">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="c9ae2-253">Quando si seleziona il pulsante **Aggiungi attività** nella pagina di indice, viene eseguito il metodo `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="c9ae2-253">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="c9ae2-254">`QueueBackgroundWorkItem`viene chiamato per accodare un elemento di lavoro:</span><span class="sxs-lookup"><span data-stu-id="c9ae2-254">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="c9ae2-255">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c9ae2-255">Additional resources</span></span>

* [<span data-ttu-id="c9ae2-256">Implementare attività in background in microservizi con IHostedService e la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="c9ae2-256">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
