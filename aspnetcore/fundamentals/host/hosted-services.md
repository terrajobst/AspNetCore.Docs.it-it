---
title: Attività in background con servizi ospitati in ASP.NET Core
author: guardrex
description: Informazioni su come implementare attività in background con servizi ospitati in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/18/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 8df86b10d7ba853edb3265df0e02eabbf8a2c058
ms.sourcegitcommit: fa61d882be9d0c48bd681f2efcb97e05522051d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71205706"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="2b32d-103">Attività in background con servizi ospitati in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b32d-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="2b32d-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2b32d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2b32d-105">In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="2b32d-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="2b32d-106">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="2b32d-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2b32d-107">In questo argomento vengono forniti tre esempi di servizio ospitato:</span><span class="sxs-lookup"><span data-stu-id="2b32d-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="2b32d-108">Attività in background eseguita su un timer.</span><span class="sxs-lookup"><span data-stu-id="2b32d-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="2b32d-109">Servizio ospitato che attiva un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="2b32d-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="2b32d-110">Il servizio con ambito può usare l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2b32d-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="2b32d-111">Attività in background in coda che vengono eseguite in sequenza.</span><span class="sxs-lookup"><span data-stu-id="2b32d-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="2b32d-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2b32d-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2b32d-113">L'app di esempio è disponibile in due versioni:</span><span class="sxs-lookup"><span data-stu-id="2b32d-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="2b32d-114">Host Web &ndash; L'host Web è utile per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="2b32d-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="2b32d-115">Il codice di esempio in questo argomento è tratto dalla versione host Web dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="2b32d-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="2b32d-116">Per altre informazioni, vedere l'argomento [Host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="2b32d-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="2b32d-117">Host generico &ndash; L'host generico è una novità di ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2b32d-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="2b32d-118">Per altre informazioni, vedere l'argomento [Host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="2b32d-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="2b32d-119">Modello di servizio di ruolo di lavoro</span><span class="sxs-lookup"><span data-stu-id="2b32d-119">Worker Service template</span></span>

<span data-ttu-id="2b32d-120">Il modello di servizio di ruolo di lavoro di ASP.NET Core rappresenta un punto di partenza per la scrittura di app di servizi a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="2b32d-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="2b32d-121">Per usare il modello come base per un'app di servizi ospitati:</span><span class="sxs-lookup"><span data-stu-id="2b32d-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2b32d-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b32d-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="2b32d-123">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="2b32d-123">Create a new project.</span></span>
1. <span data-ttu-id="2b32d-124">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2b32d-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="2b32d-125">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2b32d-125">Select **Next**.</span></span>
1. <span data-ttu-id="2b32d-126">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="2b32d-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="2b32d-127">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2b32d-127">Select **Create**.</span></span>
1. <span data-ttu-id="2b32d-128">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="2b32d-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="2b32d-129">Selezionare il modello del **Servizio di ruolo di lavoro**.</span><span class="sxs-lookup"><span data-stu-id="2b32d-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="2b32d-130">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2b32d-130">Select **Create**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2b32d-131">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2b32d-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="2b32d-132">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="2b32d-132">Create a new project.</span></span>
1. <span data-ttu-id="2b32d-133">Selezionare **app** in **.NET Core** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="2b32d-133">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="2b32d-134">Selezionare **Worker** in **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2b32d-134">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="2b32d-135">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2b32d-135">Select **Next**.</span></span>
1. <span data-ttu-id="2b32d-136">Selezionare **.NET Core 3,0** per il **Framework di destinazione**.</span><span class="sxs-lookup"><span data-stu-id="2b32d-136">Select **.NET Core 3.0** for the **Target Framework**.</span></span> <span data-ttu-id="2b32d-137">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2b32d-137">Select **Next**.</span></span>
1. <span data-ttu-id="2b32d-138">Specificare un nome nel campo **nome progetto** .</span><span class="sxs-lookup"><span data-stu-id="2b32d-138">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="2b32d-139">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2b32d-139">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2b32d-140">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="2b32d-140">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2b32d-141">Usare il servizio di ruolo di lavoro (`worker`) con il comando[dotnet new](/dotnet/core/tools/dotnet-new) da una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="2b32d-141">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="2b32d-142">Nell'esempio seguente viene creata un'app del servizio di ruolo di lavoro denominata `ContosoWorker`.</span><span class="sxs-lookup"><span data-stu-id="2b32d-142">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="2b32d-143">Una cartella per l'app `ContosoWorker` viene creata automaticamente quando viene eseguito il comando.</span><span class="sxs-lookup"><span data-stu-id="2b32d-143">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---

## <a name="package"></a><span data-ttu-id="2b32d-144">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="2b32d-144">Package</span></span>

<span data-ttu-id="2b32d-145">Un riferimento al pacchetto [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) viene aggiunto in modo implicito per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2b32d-145">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="2b32d-146">Interfaccia IHostedService</span><span class="sxs-lookup"><span data-stu-id="2b32d-146">IHostedService interface</span></span>

<span data-ttu-id="2b32d-147">L' <xref:Microsoft.Extensions.Hosting.IHostedService> interfaccia definisce due metodi per gli oggetti gestiti dall'host:</span><span class="sxs-lookup"><span data-stu-id="2b32d-147">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="2b32d-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contiene la logica per avviare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="2b32d-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="2b32d-149">`StartAsync`viene chiamato *prima*di:</span><span class="sxs-lookup"><span data-stu-id="2b32d-149">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="2b32d-150">La pipeline di elaborazione delle richieste dell'app è`Startup.Configure`configurata ().</span><span class="sxs-lookup"><span data-stu-id="2b32d-150">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="2b32d-151">Il server viene avviato e viene attivato [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="2b32d-151">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="2b32d-152">Il comportamento predefinito può essere modificato in modo che il servizio `StartAsync` ospitato venga eseguito dopo che la pipeline dell'app è stata configurata e `ApplicationStarted` chiamata.</span><span class="sxs-lookup"><span data-stu-id="2b32d-152">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="2b32d-153">Per modificare il comportamento predefinito, aggiungere il servizio ospitato (`VideosWatcher` nell'esempio seguente) dopo la chiamata `ConfigureWebHostDefaults`a:</span><span class="sxs-lookup"><span data-stu-id="2b32d-153">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="2b32d-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Attivato quando l'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="2b32d-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="2b32d-155">`StopAsync` contiene la logica per terminare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="2b32d-155">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="2b32d-156">Implementare <xref:System.IDisposable> e i [finalizzatori (distruttori)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) per eliminare tutte le risorse non gestite.</span><span class="sxs-lookup"><span data-stu-id="2b32d-156">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="2b32d-157">Il token di annullamento ha un timeout predefinito di cinque secondi che indica che il processo di arresto non è più normale.</span><span class="sxs-lookup"><span data-stu-id="2b32d-157">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="2b32d-158">Quando viene richiesto l'annullamento sul token:</span><span class="sxs-lookup"><span data-stu-id="2b32d-158">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="2b32d-159">Tutte le operazioni in background rimanenti che sta eseguendo l'app devono essere interrotte.</span><span class="sxs-lookup"><span data-stu-id="2b32d-159">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="2b32d-160">Tutti i metodi eventuali chiamati in `StopAsync` devono essere completati rapidamente.</span><span class="sxs-lookup"><span data-stu-id="2b32d-160">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="2b32d-161">Tuttavia, dopo la richiesta di annullamento le attività non vengono abbandonate: il chiamante attende il completamento di tutte le attività.</span><span class="sxs-lookup"><span data-stu-id="2b32d-161">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="2b32d-162">Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="2b32d-162">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="2b32d-163">Pertanto è possibile che i metodi chiamati o le operazioni effettuate in `StopAsync` non vengano eseguiti.</span><span class="sxs-lookup"><span data-stu-id="2b32d-163">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="2b32d-164">Per estendere il timeout di arresto predefinito di cinque secondi, impostare:</span><span class="sxs-lookup"><span data-stu-id="2b32d-164">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="2b32d-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quando si usa l'host generico.</span><span class="sxs-lookup"><span data-stu-id="2b32d-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="2b32d-166">Per altre informazioni, vedere <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="2b32d-166">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="2b32d-167">Impostazione di configurazione dell'host del timeout di arresto quando si usa l'host Web.</span><span class="sxs-lookup"><span data-stu-id="2b32d-167">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="2b32d-168">Per altre informazioni, vedere <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="2b32d-168">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="2b32d-169">Il servizio ospitato viene attivato una volta all'avvio dell'app e arrestato normalmente all'arresto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2b32d-169">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="2b32d-170">Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="2b32d-170">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="2b32d-171">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="2b32d-171">BackgroundService</span></span>

<span data-ttu-id="2b32d-172">`BackgroundService`è una classe di base per l'implementazione di <xref:Microsoft.Extensions.Hosting.IHostedService>un oggetto a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="2b32d-172">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="2b32d-173">`BackgroundService`definisce due metodi per le operazioni in background:</span><span class="sxs-lookup"><span data-stu-id="2b32d-173">`BackgroundService` defines two methods for background operations:</span></span>

* <span data-ttu-id="2b32d-174">`ExecuteAsync(CancellationToken stoppingToken)`Viene chiamato all' <xref:Microsoft.Extensions.Hosting.IHostedService> avvio di. &ndash; `ExecuteAsync`</span><span class="sxs-lookup"><span data-stu-id="2b32d-174">`ExecuteAsync(CancellationToken stoppingToken)` &ndash; `ExecuteAsync` Called when the <xref:Microsoft.Extensions.Hosting.IHostedService> starts.</span></span> <span data-ttu-id="2b32d-175">L'implementazione deve restituire un `Task` oggetto che rappresenta la durata delle operazioni a esecuzione prolungata eseguite.</span><span class="sxs-lookup"><span data-stu-id="2b32d-175">The implementation should return a `Task` that represents the lifetime of the long running operations performed.</span></span> <span data-ttu-id="2b32d-176">Oggetto `stoppingToken` attivato quando viene chiamato [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) .</span><span class="sxs-lookup"><span data-stu-id="2b32d-176">The `stoppingToken` Triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span>
* <span data-ttu-id="2b32d-177">`StopAsync(CancellationToken stoppingToken)`&ndash; vieneattivatoquandol'hostdell'applicazionesta`StopAsync` eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="2b32d-177">`StopAsync(CancellationToken stoppingToken)` &ndash; `StopAsync` is triggered when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="2b32d-178">`stoppingToken` Indica che il processo di arresto non dovrebbe essere più normale.</span><span class="sxs-lookup"><span data-stu-id="2b32d-178">The `stoppingToken` indicates that the shutdown process should no longer be graceful.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="2b32d-179">Attività in background programmate</span><span class="sxs-lookup"><span data-stu-id="2b32d-179">Timed background tasks</span></span>

<span data-ttu-id="2b32d-180">Un'attività programmata in background utilizza la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="2b32d-180">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="2b32d-181">Il timer attiva il metodo `DoWork` dell'attività.</span><span class="sxs-lookup"><span data-stu-id="2b32d-181">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="2b32d-182">Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="2b32d-182">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="2b32d-183">Il servizio è registrato in `IHostBuilder.ConfigureServices` (*Program.cs*) con il `AddHostedService` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="2b32d-183">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="2b32d-184">Utilizzo di un servizio con ambito in un'attività in background</span><span class="sxs-lookup"><span data-stu-id="2b32d-184">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="2b32d-185">Per usare i [servizi con ambito](xref:fundamentals/dependency-injection#service-lifetimes) all' `BackgroundService`interno di un, creare un ambito.</span><span class="sxs-lookup"><span data-stu-id="2b32d-185">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="2b32d-186">Non viene creato automaticamente alcun ambito per un servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="2b32d-186">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="2b32d-187">Il servizio dell'attività in background con ambito contiene la logica dell'attività in background.</span><span class="sxs-lookup"><span data-stu-id="2b32d-187">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="2b32d-188">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="2b32d-188">In the following example:</span></span>

* <span data-ttu-id="2b32d-189">Il servizio è asincrono.</span><span class="sxs-lookup"><span data-stu-id="2b32d-189">The service is asynchronous.</span></span> <span data-ttu-id="2b32d-190">Il metodo `DoWork` restituisce un tipo `Task`.</span><span class="sxs-lookup"><span data-stu-id="2b32d-190">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="2b32d-191">A scopo dimostrativo, nel `DoWork` metodo è atteso un ritardo di dieci secondi.</span><span class="sxs-lookup"><span data-stu-id="2b32d-191">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="2b32d-192">Un <xref:Microsoft.Extensions.Logging.ILogger> oggetto viene inserito nel servizio.</span><span class="sxs-lookup"><span data-stu-id="2b32d-192">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="2b32d-193">Il servizio ospitato crea un ambito per risolvere il servizio attività in background con ambito per chiamare `DoWork` il relativo metodo.</span><span class="sxs-lookup"><span data-stu-id="2b32d-193">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="2b32d-194">`DoWork`Restituisce un `Task`oggetto, che è atteso `ExecuteAsync`in:</span><span class="sxs-lookup"><span data-stu-id="2b32d-194">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="2b32d-195">I servizi sono registrati in `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="2b32d-195">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="2b32d-196">Il servizio ospitato viene registrato con il `AddHostedService` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="2b32d-196">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="2b32d-197">Attività in background in coda</span><span class="sxs-lookup"><span data-stu-id="2b32d-197">Queued background tasks</span></span>

<span data-ttu-id="2b32d-198">Una coda delle attività in background è basata su .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([programma provvisoriamente per essere incorporato per ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="2b32d-198">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="2b32d-199">Nell'esempio seguente `QueueHostedService` :</span><span class="sxs-lookup"><span data-stu-id="2b32d-199">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="2b32d-200">Il `BackgroundProcessing` metodo restituisce un `Task`oggetto, che è atteso `ExecuteAsync`in.</span><span class="sxs-lookup"><span data-stu-id="2b32d-200">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="2b32d-201">Le attività in background nella coda vengono rimesse in coda ed `BackgroundProcessing`eseguite in.</span><span class="sxs-lookup"><span data-stu-id="2b32d-201">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="2b32d-202">Un `MonitorLoop` servizio gestisce le attività di Accodamento per il servizio ospitato ogni `w` volta che la chiave viene selezionata in un dispositivo di input:</span><span class="sxs-lookup"><span data-stu-id="2b32d-202">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="2b32d-203">`IBackgroundTaskQueue` Viene inserito`MonitorLoop` nel servizio.</span><span class="sxs-lookup"><span data-stu-id="2b32d-203">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="2b32d-204">`IBackgroundTaskQueue.QueueBackgroundWorkItem`viene chiamato per accodare un elemento di lavoro.</span><span class="sxs-lookup"><span data-stu-id="2b32d-204">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="2b32d-205">I servizi sono registrati in `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="2b32d-205">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="2b32d-206">Il servizio ospitato viene registrato con il `AddHostedService` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="2b32d-206">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2b32d-207">In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="2b32d-207">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="2b32d-208">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="2b32d-208">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2b32d-209">In questo argomento vengono forniti tre esempi di servizio ospitato:</span><span class="sxs-lookup"><span data-stu-id="2b32d-209">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="2b32d-210">Attività in background eseguita su un timer.</span><span class="sxs-lookup"><span data-stu-id="2b32d-210">Background task that runs on a timer.</span></span>
* <span data-ttu-id="2b32d-211">Servizio ospitato che attiva un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="2b32d-211">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="2b32d-212">Il servizio con ambito può usare l' [inserimento di dipendenze](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="2b32d-212">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="2b32d-213">Attività in background in coda che vengono eseguite in sequenza.</span><span class="sxs-lookup"><span data-stu-id="2b32d-213">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="2b32d-214">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2b32d-214">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2b32d-215">L'app di esempio è disponibile in due versioni:</span><span class="sxs-lookup"><span data-stu-id="2b32d-215">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="2b32d-216">Host Web &ndash; L'host Web è utile per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="2b32d-216">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="2b32d-217">Il codice di esempio in questo argomento è tratto dalla versione host Web dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="2b32d-217">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="2b32d-218">Per altre informazioni, vedere l'argomento [Host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="2b32d-218">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="2b32d-219">Host generico &ndash; L'host generico è una novità di ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2b32d-219">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="2b32d-220">Per altre informazioni, vedere l'argomento [Host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="2b32d-220">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="2b32d-221">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="2b32d-221">Package</span></span>

<span data-ttu-id="2b32d-222">Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="2b32d-222">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="2b32d-223">Interfaccia IHostedService</span><span class="sxs-lookup"><span data-stu-id="2b32d-223">IHostedService interface</span></span>

<span data-ttu-id="2b32d-224">I servizi ospitati implementano l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="2b32d-224">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2b32d-225">L'interfaccia definisce due metodi riservati agli oggetti gestiti dall'host:</span><span class="sxs-lookup"><span data-stu-id="2b32d-225">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="2b32d-226">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contiene la logica per avviare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="2b32d-226">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="2b32d-227">Quando si usa l'[Host Web](xref:fundamentals/host/web-host), `StartAsync` viene chiamato dopo l'avvio del server e l'attivazione di [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="2b32d-227">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="2b32d-228">Quando si usa l'[Host generico](xref:fundamentals/host/generic-host), `StartAsync` viene chiamato prima dell'attivazione di `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="2b32d-228">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="2b32d-229">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Attivato quando l'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="2b32d-229">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="2b32d-230">`StopAsync` contiene la logica per terminare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="2b32d-230">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="2b32d-231">Implementare <xref:System.IDisposable> e i [finalizzatori (distruttori)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) per eliminare tutte le risorse non gestite.</span><span class="sxs-lookup"><span data-stu-id="2b32d-231">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="2b32d-232">Il token di annullamento ha un timeout predefinito di cinque secondi che indica che il processo di arresto non è più normale.</span><span class="sxs-lookup"><span data-stu-id="2b32d-232">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="2b32d-233">Quando viene richiesto l'annullamento sul token:</span><span class="sxs-lookup"><span data-stu-id="2b32d-233">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="2b32d-234">Tutte le operazioni in background rimanenti che sta eseguendo l'app devono essere interrotte.</span><span class="sxs-lookup"><span data-stu-id="2b32d-234">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="2b32d-235">Tutti i metodi eventuali chiamati in `StopAsync` devono essere completati rapidamente.</span><span class="sxs-lookup"><span data-stu-id="2b32d-235">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="2b32d-236">Tuttavia, dopo la richiesta di annullamento le attività non vengono abbandonate: il chiamante attende il completamento di tutte le attività.</span><span class="sxs-lookup"><span data-stu-id="2b32d-236">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="2b32d-237">Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="2b32d-237">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="2b32d-238">Pertanto è possibile che i metodi chiamati o le operazioni effettuate in `StopAsync` non vengano eseguiti.</span><span class="sxs-lookup"><span data-stu-id="2b32d-238">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="2b32d-239">Per estendere il timeout di arresto predefinito di cinque secondi, impostare:</span><span class="sxs-lookup"><span data-stu-id="2b32d-239">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="2b32d-240"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quando si usa l'host generico.</span><span class="sxs-lookup"><span data-stu-id="2b32d-240"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="2b32d-241">Per altre informazioni, vedere <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="2b32d-241">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="2b32d-242">Impostazione di configurazione dell'host del timeout di arresto quando si usa l'host Web.</span><span class="sxs-lookup"><span data-stu-id="2b32d-242">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="2b32d-243">Per altre informazioni, vedere <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="2b32d-243">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="2b32d-244">Il servizio ospitato viene attivato una volta all'avvio dell'app e arrestato normalmente all'arresto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2b32d-244">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="2b32d-245">Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="2b32d-245">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="2b32d-246">Attività in background programmate</span><span class="sxs-lookup"><span data-stu-id="2b32d-246">Timed background tasks</span></span>

<span data-ttu-id="2b32d-247">Un'attività programmata in background utilizza la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="2b32d-247">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="2b32d-248">Il timer attiva il metodo `DoWork` dell'attività.</span><span class="sxs-lookup"><span data-stu-id="2b32d-248">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="2b32d-249">Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="2b32d-249">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="2b32d-250">Il servizio registrato in `Startup.ConfigureServices` con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="2b32d-250">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="2b32d-251">Utilizzo di un servizio con ambito in un'attività in background</span><span class="sxs-lookup"><span data-stu-id="2b32d-251">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="2b32d-252">Per usare i [servizi con ambito](xref:fundamentals/dependency-injection#service-lifetimes) all'interno di un `IHostedService`, creare un ambito.</span><span class="sxs-lookup"><span data-stu-id="2b32d-252">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="2b32d-253">Non viene creato automaticamente alcun ambito per un servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="2b32d-253">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="2b32d-254">Il servizio dell'attività in background con ambito contiene la logica dell'attività in background.</span><span class="sxs-lookup"><span data-stu-id="2b32d-254">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="2b32d-255">Nell'esempio seguente, <xref:Microsoft.Extensions.Logging.ILogger> viene inserito nel servizio:</span><span class="sxs-lookup"><span data-stu-id="2b32d-255">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="2b32d-256">Il servizio ospitato crea un ambito per risolvere il servizio dell'attività in background con ambito e chiamare il relativo metodo `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="2b32d-256">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="2b32d-257">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2b32d-257">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2b32d-258">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="2b32d-258">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="2b32d-259">Attività in background in coda</span><span class="sxs-lookup"><span data-stu-id="2b32d-259">Queued background tasks</span></span>

<span data-ttu-id="2b32d-260">Una coda delle attività in background è basata su .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([programma provvisoriamente per essere incorporato per ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="2b32d-260">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="2b32d-261">In `QueueHostedService`, le attività in background in coda vengono rimosse dalla coda ed eseguite come <xref:Microsoft.Extensions.Hosting.BackgroundService>, ovvero una classe di base per l'implementazione di un `IHostedService` a esecuzione prolungata:</span><span class="sxs-lookup"><span data-stu-id="2b32d-261">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="2b32d-262">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2b32d-262">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2b32d-263">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="2b32d-263">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="2b32d-264">Nella classe modello della pagina di indice:</span><span class="sxs-lookup"><span data-stu-id="2b32d-264">In the Index page model class:</span></span>

* <span data-ttu-id="2b32d-265">`IBackgroundTaskQueue` viene inserito nel costruttore e assegnato a `Queue`.</span><span class="sxs-lookup"><span data-stu-id="2b32d-265">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="2b32d-266">Un'interfaccia <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> viene inserita e assegnata a `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="2b32d-266">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="2b32d-267">La factory viene usata per creare istanze di <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, che consente di creare servizi all'interno di un ambito.</span><span class="sxs-lookup"><span data-stu-id="2b32d-267">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="2b32d-268">Viene creato un ambito per usare la classe `AppDbContext` dell'app (un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes)) per scrivere record di database in `IBackgroundTaskQueue` (un servizio singleton).</span><span class="sxs-lookup"><span data-stu-id="2b32d-268">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="2b32d-269">Quando si seleziona il pulsante **Aggiungi attività** nella pagina di indice, viene eseguito il metodo `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="2b32d-269">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="2b32d-270">`QueueBackgroundWorkItem`viene chiamato per accodare un elemento di lavoro:</span><span class="sxs-lookup"><span data-stu-id="2b32d-270">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="2b32d-271">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2b32d-271">Additional resources</span></span>

* [<span data-ttu-id="2b32d-272">Implementare attività in background in microservizi con IHostedService e la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="2b32d-272">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
