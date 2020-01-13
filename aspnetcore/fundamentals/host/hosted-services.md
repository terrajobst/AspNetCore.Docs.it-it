---
title: Attività in background con servizi ospitati in ASP.NET Core
author: guardrex
description: Informazioni su come implementare attività in background con servizi ospitati in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2020
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 49229b5db4d58f25f86425f8622d12c9107262bd
ms.sourcegitcommit: 57b85708f4cded99b8f008a69830cb104cd8e879
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2020
ms.locfileid: "75914218"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="9e728-103">Attività in background con servizi ospitati in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e728-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="9e728-104">Di [Luke Latham](https://github.com/guardrex) e [Jeow li Huan](https://github.com/huan086)</span><span class="sxs-lookup"><span data-stu-id="9e728-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9e728-105">In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="9e728-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="9e728-106">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="9e728-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="9e728-107">In questo argomento vengono forniti tre esempi di servizio ospitato:</span><span class="sxs-lookup"><span data-stu-id="9e728-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="9e728-108">Attività in background eseguita su un timer.</span><span class="sxs-lookup"><span data-stu-id="9e728-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="9e728-109">Servizio ospitato che attiva un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="9e728-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="9e728-110">Il servizio con ambito può usare l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9e728-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="9e728-111">Attività in background in coda che vengono eseguite in sequenza.</span><span class="sxs-lookup"><span data-stu-id="9e728-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="9e728-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9e728-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="9e728-113">Modello di servizio di ruolo di lavoro</span><span class="sxs-lookup"><span data-stu-id="9e728-113">Worker Service template</span></span>

<span data-ttu-id="9e728-114">Il modello di servizio di ruolo di lavoro di ASP.NET Core rappresenta un punto di partenza per la scrittura di app di servizi a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="9e728-114">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="9e728-115">Un'app creata dal modello del servizio Worker specifica l'SDK del ruolo di lavoro nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="9e728-115">An app created from the Worker Service template specifies the Worker SDK in its project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

<span data-ttu-id="9e728-116">Per usare il modello come base per un'app di servizi ospitati:</span><span class="sxs-lookup"><span data-stu-id="9e728-116">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a><span data-ttu-id="9e728-117">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="9e728-117">Package</span></span>

<span data-ttu-id="9e728-118">Un'app basata sul modello del servizio Worker usa l'SDK `Microsoft.NET.Sdk.Worker` e contiene un riferimento al pacchetto esplicito al pacchetto [Microsoft. Extensions. host](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) .</span><span class="sxs-lookup"><span data-stu-id="9e728-118">An app based on the Worker Service template uses the `Microsoft.NET.Sdk.Worker` SDK and has an explicit package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span> <span data-ttu-id="9e728-119">Ad esempio, vedere il file di progetto dell'app di esempio (*BackgroundTasksSample. csproj*).</span><span class="sxs-lookup"><span data-stu-id="9e728-119">For example, see the sample app's project file (*BackgroundTasksSample.csproj*).</span></span>

<span data-ttu-id="9e728-120">Per le app Web che usano l'SDK `Microsoft.NET.Sdk.Web`, viene fatto riferimento in modo implicito al pacchetto [Microsoft. Extensions. host](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) dal Framework condiviso.</span><span class="sxs-lookup"><span data-stu-id="9e728-120">For web apps that use the `Microsoft.NET.Sdk.Web` SDK, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is referenced implicitly from the shared framework.</span></span> <span data-ttu-id="9e728-121">Un riferimento esplicito al pacchetto nel file di progetto dell'app non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="9e728-121">An explicit package reference in the app's project file isn't required.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="9e728-122">Interfaccia IHostedService</span><span class="sxs-lookup"><span data-stu-id="9e728-122">IHostedService interface</span></span>

<span data-ttu-id="9e728-123">L'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService> definisce due metodi per gli oggetti gestiti dall'host:</span><span class="sxs-lookup"><span data-stu-id="9e728-123">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="9e728-124">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contiene la logica per avviare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="9e728-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="9e728-125">`StartAsync` viene chiamato *prima*:</span><span class="sxs-lookup"><span data-stu-id="9e728-125">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="9e728-126">La pipeline di elaborazione delle richieste dell'app è configurata (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="9e728-126">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="9e728-127">Il server viene avviato e viene attivato [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="9e728-127">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="9e728-128">Il comportamento predefinito può essere modificato in modo che l'`StartAsync` del servizio ospitato venga eseguito dopo la configurazione della pipeline dell'app e che venga chiamato `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="9e728-128">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="9e728-129">Per modificare il comportamento predefinito, aggiungere il servizio ospitato (`VideosWatcher` nell'esempio seguente) dopo aver chiamato `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="9e728-129">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="9e728-130">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; attivata quando l'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="9e728-130">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="9e728-131">`StopAsync` contiene la logica per terminare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="9e728-131">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="9e728-132">Implementare <xref:System.IDisposable> e i [finalizzatori (distruttori)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) per eliminare tutte le risorse non gestite.</span><span class="sxs-lookup"><span data-stu-id="9e728-132">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="9e728-133">Il token di annullamento ha un timeout predefinito di cinque secondi che indica che il processo di arresto non è più normale.</span><span class="sxs-lookup"><span data-stu-id="9e728-133">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="9e728-134">Quando viene richiesto l'annullamento sul token:</span><span class="sxs-lookup"><span data-stu-id="9e728-134">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="9e728-135">Tutte le operazioni in background rimanenti che sta eseguendo l'app devono essere interrotte.</span><span class="sxs-lookup"><span data-stu-id="9e728-135">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="9e728-136">Tutti i metodi eventuali chiamati in `StopAsync` devono essere completati rapidamente.</span><span class="sxs-lookup"><span data-stu-id="9e728-136">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="9e728-137">Tuttavia, dopo la richiesta di annullamento le attività non vengono abbandonate: il chiamante attende il completamento di tutte le attività.</span><span class="sxs-lookup"><span data-stu-id="9e728-137">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="9e728-138">Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="9e728-138">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="9e728-139">Pertanto è possibile che i metodi chiamati o le operazioni effettuate in `StopAsync` non vengano eseguiti.</span><span class="sxs-lookup"><span data-stu-id="9e728-139">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="9e728-140">Per estendere il timeout di arresto predefinito di cinque secondi, impostare:</span><span class="sxs-lookup"><span data-stu-id="9e728-140">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="9e728-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quando si usa l'host generico.</span><span class="sxs-lookup"><span data-stu-id="9e728-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="9e728-142">Per ulteriori informazioni, vedere <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="9e728-142">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="9e728-143">Impostazione di configurazione dell'host del timeout di arresto quando si usa l'host Web.</span><span class="sxs-lookup"><span data-stu-id="9e728-143">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="9e728-144">Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="9e728-144">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="9e728-145">Il servizio ospitato viene attivato una volta all'avvio dell'app e arrestato normalmente all'arresto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9e728-145">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="9e728-146">Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="9e728-146">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice-base-class"></a><span data-ttu-id="9e728-147">Classe di base BackgroundService</span><span class="sxs-lookup"><span data-stu-id="9e728-147">BackgroundService base class</span></span>

<span data-ttu-id="9e728-148"><xref:Microsoft.Extensions.Hosting.BackgroundService> è una classe di base per l'implementazione di una <xref:Microsoft.Extensions.Hosting.IHostedService>con esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="9e728-148"><xref:Microsoft.Extensions.Hosting.BackgroundService> is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span>

<span data-ttu-id="9e728-149">[ExecuteAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) viene chiamato per eseguire il servizio in background.</span><span class="sxs-lookup"><span data-stu-id="9e728-149">[ExecuteAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) is called to run the background service.</span></span> <span data-ttu-id="9e728-150">L'implementazione restituisce un <xref:System.Threading.Tasks.Task> che rappresenta l'intera durata del servizio in background.</span><span class="sxs-lookup"><span data-stu-id="9e728-150">The implementation returns a <xref:System.Threading.Tasks.Task> that represents the entire lifetime of the background service.</span></span> <span data-ttu-id="9e728-151">Non vengono avviati altri servizi fino a quando [ExecuteAsync non diventa asincrono](https://github.com/dotnet/extensions/issues/2149), ad esempio chiamando `await`.</span><span class="sxs-lookup"><span data-stu-id="9e728-151">No further services are started until [ExecuteAsync becomes asynchronous](https://github.com/dotnet/extensions/issues/2149), such as by calling `await`.</span></span> <span data-ttu-id="9e728-152">Evitare di eseguire operazioni di inizializzazione lunghe e bloccate in `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="9e728-152">Avoid performing long, blocking initialization work in `ExecuteAsync`.</span></span> <span data-ttu-id="9e728-153">Blocchi host in [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) in attesa del completamento `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="9e728-153">The host blocks in [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) waiting for `ExecuteAsync` to complete.</span></span>

<span data-ttu-id="9e728-154">Il token di annullamento viene attivato quando viene chiamato [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) .</span><span class="sxs-lookup"><span data-stu-id="9e728-154">The cancellation token is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="9e728-155">L'implementazione di `ExecuteAsync` deve terminare tempestivamente quando viene generato il token di annullamento per arrestare correttamente il servizio.</span><span class="sxs-lookup"><span data-stu-id="9e728-155">Your implementation of `ExecuteAsync` should finish promptly when the cancellation token is fired in order to gracefully shut down the service.</span></span> <span data-ttu-id="9e728-156">In caso contrario, il servizio si arresta normalmente in corrispondenza del timeout di arresto.</span><span class="sxs-lookup"><span data-stu-id="9e728-156">Otherwise, the service ungracefully shuts down at the shutdown timeout.</span></span> <span data-ttu-id="9e728-157">Per ulteriori informazioni, vedere la sezione [interfaccia IHostedService](#ihostedservice-interface) .</span><span class="sxs-lookup"><span data-stu-id="9e728-157">For more information, see the [IHostedService interface](#ihostedservice-interface) section.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="9e728-158">Attività in background programmate</span><span class="sxs-lookup"><span data-stu-id="9e728-158">Timed background tasks</span></span>

<span data-ttu-id="9e728-159">Un'attività programmata in background utilizza la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="9e728-159">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="9e728-160">Il timer attiva il metodo `DoWork` dell'attività.</span><span class="sxs-lookup"><span data-stu-id="9e728-160">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="9e728-161">Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="9e728-161">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-17,34,41)]

<span data-ttu-id="9e728-162">Il <xref:System.Threading.Timer> non attende il completamento delle esecuzioni precedenti di `DoWork`, pertanto l'approccio illustrato potrebbe non essere adatto per ogni scenario.</span><span class="sxs-lookup"><span data-stu-id="9e728-162">The <xref:System.Threading.Timer> doesn't wait for previous executions of `DoWork` to finish, so the approach shown might not be suitable for every scenario.</span></span> <span data-ttu-id="9e728-163">[Interlocked. Increment](xref:System.Threading.Interlocked.Increment*) viene usato per incrementare il contatore di esecuzione come operazione atomica, in modo da garantire che più thread non aggiornino `executionCount` simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="9e728-163">[Interlocked.Increment](xref:System.Threading.Interlocked.Increment*) is used to increment the execution counter as an atomic operation, which ensures that multiple threads don't update `executionCount` concurrently.</span></span>

<span data-ttu-id="9e728-164">Il servizio è registrato nel `IHostBuilder.ConfigureServices` (*Program.cs*) con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="9e728-164">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="9e728-165">Utilizzo di un servizio con ambito in un'attività in background</span><span class="sxs-lookup"><span data-stu-id="9e728-165">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="9e728-166">Per usare i [servizi con ambito](xref:fundamentals/dependency-injection#service-lifetimes) all'interno di un [BackgroundService](#backgroundservice-base-class), creare un ambito.</span><span class="sxs-lookup"><span data-stu-id="9e728-166">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a [BackgroundService](#backgroundservice-base-class), create a scope.</span></span> <span data-ttu-id="9e728-167">Non viene creato automaticamente alcun ambito per un servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="9e728-167">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="9e728-168">Il servizio dell'attività in background con ambito contiene la logica dell'attività in background.</span><span class="sxs-lookup"><span data-stu-id="9e728-168">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="9e728-169">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="9e728-169">In the following example:</span></span>

* <span data-ttu-id="9e728-170">Il servizio è asincrono.</span><span class="sxs-lookup"><span data-stu-id="9e728-170">The service is asynchronous.</span></span> <span data-ttu-id="9e728-171">Il metodo `DoWork` restituisce un tipo `Task`.</span><span class="sxs-lookup"><span data-stu-id="9e728-171">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="9e728-172">A scopo dimostrativo, è atteso un ritardo di dieci secondi nel metodo `DoWork`.</span><span class="sxs-lookup"><span data-stu-id="9e728-172">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="9e728-173">Un <xref:Microsoft.Extensions.Logging.ILogger> viene inserito nel servizio.</span><span class="sxs-lookup"><span data-stu-id="9e728-173">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="9e728-174">Il servizio ospitato crea un ambito per risolvere il servizio attività in background con ambito per chiamare il relativo metodo `DoWork`.</span><span class="sxs-lookup"><span data-stu-id="9e728-174">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="9e728-175">`DoWork` restituisce un `Task`, atteso in `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="9e728-175">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="9e728-176">I servizi vengono registrati nel `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="9e728-176">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="9e728-177">Il servizio ospitato viene registrato con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="9e728-177">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="9e728-178">Attività in background in coda</span><span class="sxs-lookup"><span data-stu-id="9e728-178">Queued background tasks</span></span>

<span data-ttu-id="9e728-179">Una coda di attività in background si basa su .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([programma provvisoriamente pianificato per essere incorporato per ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="9e728-179">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="9e728-180">Nell'esempio di `QueueHostedService` seguente:</span><span class="sxs-lookup"><span data-stu-id="9e728-180">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="9e728-181">Il metodo `BackgroundProcessing` restituisce un `Task`, atteso in `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="9e728-181">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="9e728-182">Le attività in background nella coda vengono rimossi dalla coda ed eseguite in `BackgroundProcessing`.</span><span class="sxs-lookup"><span data-stu-id="9e728-182">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="9e728-183">Gli elementi di lavoro sono in attesa prima che il servizio venga arrestato in `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="9e728-183">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="9e728-184">Un servizio `MonitorLoop` gestisce le attività di Accodamento per il servizio ospitato ogni volta che viene selezionata la chiave di `w` in un dispositivo di input:</span><span class="sxs-lookup"><span data-stu-id="9e728-184">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="9e728-185">Il `IBackgroundTaskQueue` viene inserito nel servizio `MonitorLoop`.</span><span class="sxs-lookup"><span data-stu-id="9e728-185">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="9e728-186">`IBackgroundTaskQueue.QueueBackgroundWorkItem` viene chiamato per accodare un elemento di lavoro.</span><span class="sxs-lookup"><span data-stu-id="9e728-186">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="9e728-187">L'elemento di lavoro simula un'attività in background con esecuzione prolungata:</span><span class="sxs-lookup"><span data-stu-id="9e728-187">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="9e728-188">Vengono eseguiti tre ritardi di 5 secondi (`Task.Delay`).</span><span class="sxs-lookup"><span data-stu-id="9e728-188">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="9e728-189">Un'istruzione `try-catch` intrappola <xref:System.OperationCanceledException> se l'attività è stata annullata.</span><span class="sxs-lookup"><span data-stu-id="9e728-189">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="9e728-190">I servizi vengono registrati nel `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="9e728-190">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="9e728-191">Il servizio ospitato viene registrato con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="9e728-191">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9e728-192">In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="9e728-192">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="9e728-193">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="9e728-193">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="9e728-194">In questo argomento vengono forniti tre esempi di servizio ospitato:</span><span class="sxs-lookup"><span data-stu-id="9e728-194">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="9e728-195">Attività in background eseguita su un timer.</span><span class="sxs-lookup"><span data-stu-id="9e728-195">Background task that runs on a timer.</span></span>
* <span data-ttu-id="9e728-196">Servizio ospitato che attiva un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="9e728-196">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="9e728-197">Il servizio con ambito può usare l' [inserimento di dipendenze](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="9e728-197">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="9e728-198">Attività in background in coda che vengono eseguite in sequenza.</span><span class="sxs-lookup"><span data-stu-id="9e728-198">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="9e728-199">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9e728-199">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="9e728-200">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="9e728-200">Package</span></span>

<span data-ttu-id="9e728-201">Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="9e728-201">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="9e728-202">Interfaccia IHostedService</span><span class="sxs-lookup"><span data-stu-id="9e728-202">IHostedService interface</span></span>

<span data-ttu-id="9e728-203">I servizi ospitati implementano l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="9e728-203">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="9e728-204">L'interfaccia definisce due metodi riservati agli oggetti gestiti dall'host:</span><span class="sxs-lookup"><span data-stu-id="9e728-204">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="9e728-205">[StartAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contiene la logica per avviare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="9e728-205">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="9e728-206">Quando si usa l'[Host Web](xref:fundamentals/host/web-host), `StartAsync` viene chiamato dopo l'avvio del server e l'attivazione di [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="9e728-206">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="9e728-207">Quando si usa l'[Host generico](xref:fundamentals/host/generic-host), `StartAsync` viene chiamato prima dell'attivazione di `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="9e728-207">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="9e728-208">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; attivata quando l'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="9e728-208">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="9e728-209">`StopAsync` contiene la logica per terminare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="9e728-209">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="9e728-210">Implementare <xref:System.IDisposable> e i [finalizzatori (distruttori)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) per eliminare tutte le risorse non gestite.</span><span class="sxs-lookup"><span data-stu-id="9e728-210">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="9e728-211">Il token di annullamento ha un timeout predefinito di cinque secondi che indica che il processo di arresto non è più normale.</span><span class="sxs-lookup"><span data-stu-id="9e728-211">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="9e728-212">Quando viene richiesto l'annullamento sul token:</span><span class="sxs-lookup"><span data-stu-id="9e728-212">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="9e728-213">Tutte le operazioni in background rimanenti che sta eseguendo l'app devono essere interrotte.</span><span class="sxs-lookup"><span data-stu-id="9e728-213">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="9e728-214">Tutti i metodi eventuali chiamati in `StopAsync` devono essere completati rapidamente.</span><span class="sxs-lookup"><span data-stu-id="9e728-214">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="9e728-215">Tuttavia, dopo la richiesta di annullamento le attività non vengono abbandonate: il chiamante attende il completamento di tutte le attività.</span><span class="sxs-lookup"><span data-stu-id="9e728-215">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="9e728-216">Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="9e728-216">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="9e728-217">Pertanto è possibile che i metodi chiamati o le operazioni effettuate in `StopAsync` non vengano eseguiti.</span><span class="sxs-lookup"><span data-stu-id="9e728-217">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="9e728-218">Per estendere il timeout di arresto predefinito di cinque secondi, impostare:</span><span class="sxs-lookup"><span data-stu-id="9e728-218">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="9e728-219"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quando si usa l'host generico.</span><span class="sxs-lookup"><span data-stu-id="9e728-219"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="9e728-220">Per ulteriori informazioni, vedere <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="9e728-220">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="9e728-221">Impostazione di configurazione dell'host del timeout di arresto quando si usa l'host Web.</span><span class="sxs-lookup"><span data-stu-id="9e728-221">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="9e728-222">Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="9e728-222">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="9e728-223">Il servizio ospitato viene attivato una volta all'avvio dell'app e arrestato normalmente all'arresto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9e728-223">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="9e728-224">Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="9e728-224">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="9e728-225">Attività in background programmate</span><span class="sxs-lookup"><span data-stu-id="9e728-225">Timed background tasks</span></span>

<span data-ttu-id="9e728-226">Un'attività programmata in background utilizza la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="9e728-226">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="9e728-227">Il timer attiva il metodo `DoWork` dell'attività.</span><span class="sxs-lookup"><span data-stu-id="9e728-227">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="9e728-228">Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="9e728-228">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="9e728-229">Il <xref:System.Threading.Timer> non attende il completamento delle esecuzioni precedenti di `DoWork`, pertanto l'approccio illustrato potrebbe non essere adatto per ogni scenario.</span><span class="sxs-lookup"><span data-stu-id="9e728-229">The <xref:System.Threading.Timer> doesn't wait for previous executions of `DoWork` to finish, so the approach shown might not be suitable for every scenario.</span></span>

<span data-ttu-id="9e728-230">Il servizio registrato in `Startup.ConfigureServices` con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="9e728-230">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="9e728-231">Utilizzo di un servizio con ambito in un'attività in background</span><span class="sxs-lookup"><span data-stu-id="9e728-231">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="9e728-232">Per usare i [servizi con ambito](xref:fundamentals/dependency-injection#service-lifetimes) all'interno di un `IHostedService`, creare un ambito.</span><span class="sxs-lookup"><span data-stu-id="9e728-232">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="9e728-233">Non viene creato automaticamente alcun ambito per un servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="9e728-233">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="9e728-234">Il servizio dell'attività in background con ambito contiene la logica dell'attività in background.</span><span class="sxs-lookup"><span data-stu-id="9e728-234">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="9e728-235">Nell'esempio seguente, <xref:Microsoft.Extensions.Logging.ILogger> viene inserito nel servizio:</span><span class="sxs-lookup"><span data-stu-id="9e728-235">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="9e728-236">Il servizio ospitato crea un ambito per risolvere il servizio dell'attività in background con ambito e chiamare il relativo metodo `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="9e728-236">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="9e728-237">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9e728-237">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9e728-238">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="9e728-238">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="9e728-239">Attività in background in coda</span><span class="sxs-lookup"><span data-stu-id="9e728-239">Queued background tasks</span></span>

<span data-ttu-id="9e728-240">Una coda di attività in background si basa su .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([programma provvisoriamente da incorporare per ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="9e728-240">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="9e728-241">In `QueueHostedService`, le attività in background in coda vengono rimosse dalla coda ed eseguite come [BackgroundService](#backgroundservice-base-class), ovvero una classe di base per l'implementazione di un `IHostedService` a esecuzione prolungata:</span><span class="sxs-lookup"><span data-stu-id="9e728-241">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a [BackgroundService](#backgroundservice-base-class), which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="9e728-242">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9e728-242">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9e728-243">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="9e728-243">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="9e728-244">Nella classe modello della pagina di indice:</span><span class="sxs-lookup"><span data-stu-id="9e728-244">In the Index page model class:</span></span>

* <span data-ttu-id="9e728-245">`IBackgroundTaskQueue` viene inserito nel costruttore e assegnato a `Queue`.</span><span class="sxs-lookup"><span data-stu-id="9e728-245">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="9e728-246">Un'interfaccia <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> viene inserita e assegnata a `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="9e728-246">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="9e728-247">La factory viene usata per creare istanze di <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, che consente di creare servizi all'interno di un ambito.</span><span class="sxs-lookup"><span data-stu-id="9e728-247">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="9e728-248">Viene creato un ambito per usare la classe `AppDbContext` dell'app (un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes)) per scrivere record di database in `IBackgroundTaskQueue` (un servizio singleton).</span><span class="sxs-lookup"><span data-stu-id="9e728-248">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="9e728-249">Quando si seleziona il pulsante **Aggiungi attività** nella pagina di indice, viene eseguito il metodo `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="9e728-249">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="9e728-250">`QueueBackgroundWorkItem` viene chiamato per accodare un elemento di lavoro:</span><span class="sxs-lookup"><span data-stu-id="9e728-250">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9e728-251">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9e728-251">Additional resources</span></span>

* [<span data-ttu-id="9e728-252">Implementare attività in background in microservizi con IHostedService e la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="9e728-252">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
