---
title: Attività in background con servizi ospitati in ASP.NET Core
author: guardrex
description: Informazioni su come implementare attività in background con servizi ospitati in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 5a29952c4e50edb953fa03c6ea1a1ae27b728bb0
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317727"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="2540d-103">Attività in background con servizi ospitati in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2540d-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="2540d-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2540d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2540d-105">In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="2540d-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="2540d-106">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="2540d-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2540d-107">In questo argomento vengono forniti tre esempi di servizio ospitato:</span><span class="sxs-lookup"><span data-stu-id="2540d-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="2540d-108">Attività in background eseguita su un timer.</span><span class="sxs-lookup"><span data-stu-id="2540d-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="2540d-109">Servizio ospitato che attiva un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="2540d-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="2540d-110">Il servizio con ambito può usare l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2540d-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="2540d-111">Attività in background in coda che vengono eseguite in sequenza.</span><span class="sxs-lookup"><span data-stu-id="2540d-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="2540d-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2540d-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2540d-113">L'app di esempio è disponibile in due versioni:</span><span class="sxs-lookup"><span data-stu-id="2540d-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="2540d-114">Host Web &ndash; L'host Web è utile per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="2540d-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="2540d-115">Il codice di esempio in questo argomento è tratto dalla versione host Web dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="2540d-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="2540d-116">Per altre informazioni, vedere l'argomento [Host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="2540d-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="2540d-117">Host generico &ndash; L'host generico è una novità di ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2540d-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="2540d-118">Per altre informazioni, vedere l'argomento [Host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="2540d-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="2540d-119">Modello di servizio di ruolo di lavoro</span><span class="sxs-lookup"><span data-stu-id="2540d-119">Worker Service template</span></span>

<span data-ttu-id="2540d-120">Il modello di servizio di ruolo di lavoro di ASP.NET Core rappresenta un punto di partenza per la scrittura di app di servizi a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="2540d-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="2540d-121">Per usare il modello come base per un'app di servizi ospitati:</span><span class="sxs-lookup"><span data-stu-id="2540d-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2540d-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2540d-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="2540d-123">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="2540d-123">Create a new project.</span></span>
1. <span data-ttu-id="2540d-124">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2540d-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="2540d-125">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2540d-125">Select **Next**.</span></span>
1. <span data-ttu-id="2540d-126">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="2540d-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="2540d-127">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="2540d-127">Select **Create**.</span></span>
1. <span data-ttu-id="2540d-128">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="2540d-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="2540d-129">Selezionare il modello del **Servizio di ruolo di lavoro**.</span><span class="sxs-lookup"><span data-stu-id="2540d-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="2540d-130">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="2540d-130">Select **Create**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2540d-131">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2540d-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="2540d-132">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="2540d-132">Create a new project.</span></span>
1. <span data-ttu-id="2540d-133">Selezionare **app** in **.NET Core** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="2540d-133">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="2540d-134">Selezionare **Worker** in **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2540d-134">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="2540d-135">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2540d-135">Select **Next**.</span></span>
1. <span data-ttu-id="2540d-136">Selezionare **.NET Core 3,0** per il **Framework di destinazione**.</span><span class="sxs-lookup"><span data-stu-id="2540d-136">Select **.NET Core 3.0** for the **Target Framework**.</span></span> <span data-ttu-id="2540d-137">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2540d-137">Select **Next**.</span></span>
1. <span data-ttu-id="2540d-138">Specificare un nome nel campo **nome progetto** .</span><span class="sxs-lookup"><span data-stu-id="2540d-138">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="2540d-139">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="2540d-139">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2540d-140">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="2540d-140">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2540d-141">Usare il servizio di ruolo di lavoro (`worker`) con il comando[dotnet new](/dotnet/core/tools/dotnet-new) da una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="2540d-141">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="2540d-142">Nell'esempio seguente viene creata un'app del servizio di ruolo di lavoro denominata `ContosoWorker`.</span><span class="sxs-lookup"><span data-stu-id="2540d-142">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="2540d-143">Una cartella per l'app `ContosoWorker` viene creata automaticamente quando viene eseguito il comando.</span><span class="sxs-lookup"><span data-stu-id="2540d-143">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---

## <a name="package"></a><span data-ttu-id="2540d-144">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="2540d-144">Package</span></span>

<span data-ttu-id="2540d-145">Un riferimento al pacchetto [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) viene aggiunto in modo implicito per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2540d-145">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="2540d-146">Interfaccia IHostedService</span><span class="sxs-lookup"><span data-stu-id="2540d-146">IHostedService interface</span></span>

<span data-ttu-id="2540d-147">L' <xref:Microsoft.Extensions.Hosting.IHostedService> interfaccia definisce due metodi per gli oggetti gestiti dall'host:</span><span class="sxs-lookup"><span data-stu-id="2540d-147">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="2540d-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contiene la logica per avviare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="2540d-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="2540d-149">`StartAsync`viene chiamato *prima*di:</span><span class="sxs-lookup"><span data-stu-id="2540d-149">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="2540d-150">La pipeline di elaborazione delle richieste dell'app è`Startup.Configure`configurata ().</span><span class="sxs-lookup"><span data-stu-id="2540d-150">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="2540d-151">Il server viene avviato e viene attivato [IApplicationLifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) .</span><span class="sxs-lookup"><span data-stu-id="2540d-151">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="2540d-152">Il comportamento predefinito può essere modificato in modo che il servizio `StartAsync` ospitato venga eseguito dopo che la pipeline dell'app è stata configurata e `ApplicationStarted` chiamata.</span><span class="sxs-lookup"><span data-stu-id="2540d-152">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="2540d-153">Per modificare il comportamento predefinito, aggiungere il servizio ospitato (`VideosWatcher` nell'esempio seguente) dopo la chiamata `ConfigureWebHostDefaults`a:</span><span class="sxs-lookup"><span data-stu-id="2540d-153">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="2540d-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Attivato quando l'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="2540d-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="2540d-155">`StopAsync` contiene la logica per terminare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="2540d-155">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="2540d-156">Implementare <xref:System.IDisposable> e i [finalizzatori (distruttori)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) per eliminare tutte le risorse non gestite.</span><span class="sxs-lookup"><span data-stu-id="2540d-156">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="2540d-157">Il token di annullamento ha un timeout predefinito di cinque secondi che indica che il processo di arresto non è più normale.</span><span class="sxs-lookup"><span data-stu-id="2540d-157">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="2540d-158">Quando viene richiesto l'annullamento sul token:</span><span class="sxs-lookup"><span data-stu-id="2540d-158">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="2540d-159">Tutte le operazioni in background rimanenti che sta eseguendo l'app devono essere interrotte.</span><span class="sxs-lookup"><span data-stu-id="2540d-159">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="2540d-160">Tutti i metodi eventuali chiamati in `StopAsync` devono essere completati rapidamente.</span><span class="sxs-lookup"><span data-stu-id="2540d-160">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="2540d-161">Tuttavia, dopo la richiesta di annullamento le attività non vengono abbandonate: il chiamante attende il completamento di tutte le attività.</span><span class="sxs-lookup"><span data-stu-id="2540d-161">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="2540d-162">Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="2540d-162">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="2540d-163">Pertanto è possibile che i metodi chiamati o le operazioni effettuate in `StopAsync` non vengano eseguiti.</span><span class="sxs-lookup"><span data-stu-id="2540d-163">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="2540d-164">Per estendere il timeout di arresto predefinito di cinque secondi, impostare:</span><span class="sxs-lookup"><span data-stu-id="2540d-164">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="2540d-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quando si usa l'host generico.</span><span class="sxs-lookup"><span data-stu-id="2540d-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="2540d-166">Per altre informazioni, vedere <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="2540d-166">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="2540d-167">Impostazione di configurazione dell'host del timeout di arresto quando si usa l'host Web.</span><span class="sxs-lookup"><span data-stu-id="2540d-167">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="2540d-168">Per altre informazioni, vedere <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="2540d-168">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="2540d-169">Il servizio ospitato viene attivato una volta all'avvio dell'app e arrestato normalmente all'arresto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2540d-169">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="2540d-170">Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="2540d-170">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="2540d-171">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="2540d-171">BackgroundService</span></span>

<span data-ttu-id="2540d-172">`BackgroundService`è una classe di base per l'implementazione di <xref:Microsoft.Extensions.Hosting.IHostedService>un oggetto a esecuzione prolungata.</span><span class="sxs-lookup"><span data-stu-id="2540d-172">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="2540d-173">`BackgroundService`fornisce il `ExecuteAsync(CancellationToken stoppingToken)` metodo astratto per contenere la logica del servizio.</span><span class="sxs-lookup"><span data-stu-id="2540d-173">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="2540d-174">Viene attivato quando viene chiamato [IHostedService. StopAsync.](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) `stoppingToken`</span><span class="sxs-lookup"><span data-stu-id="2540d-174">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="2540d-175">L'implementazione di questo metodo restituisce un `Task` oggetto che rappresenta l'intera durata del servizio in background.</span><span class="sxs-lookup"><span data-stu-id="2540d-175">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="2540d-176">Inoltre, *facoltativamente* , eseguire l'override dei metodi `IHostedService` definiti in per eseguire il codice di avvio e arresto per il servizio:</span><span class="sxs-lookup"><span data-stu-id="2540d-176">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="2540d-177">`StopAsync(CancellationToken cancellationToken)`&ndash; vienechiamatoquandol'hostdell'applicazionesta`StopAsync` eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="2540d-177">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="2540d-178">`cancellationToken` Viene segnalato quando l'host decide di terminare forzatamente il servizio.</span><span class="sxs-lookup"><span data-stu-id="2540d-178">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="2540d-179">Se viene eseguito l'override di questo metodo, è **necessario** chiamare `await`(e) il metodo della classe di base per verificare che il servizio venga arrestato correttamente.</span><span class="sxs-lookup"><span data-stu-id="2540d-179">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="2540d-180">`StartAsync(CancellationToken cancellationToken)`&ndash; vienechiamatoperavviare`StartAsync` il servizio in background.</span><span class="sxs-lookup"><span data-stu-id="2540d-180">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="2540d-181">`cancellationToken` Viene segnalato se il processo di avvio viene interrotto.</span><span class="sxs-lookup"><span data-stu-id="2540d-181">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="2540d-182">L'implementazione restituisce un `Task` oggetto che rappresenta il processo di avvio per il servizio.</span><span class="sxs-lookup"><span data-stu-id="2540d-182">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="2540d-183">Non vengono avviati altri servizi fino `Task` a quando l'operazione non viene completata.</span><span class="sxs-lookup"><span data-stu-id="2540d-183">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="2540d-184">Se viene eseguito l'override di questo metodo, è **necessario** chiamare `await`(e) il metodo della classe di base per verificare che il servizio venga avviato correttamente.</span><span class="sxs-lookup"><span data-stu-id="2540d-184">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="2540d-185">Attività in background programmate</span><span class="sxs-lookup"><span data-stu-id="2540d-185">Timed background tasks</span></span>

<span data-ttu-id="2540d-186">Un'attività programmata in background utilizza la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="2540d-186">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="2540d-187">Il timer attiva il metodo `DoWork` dell'attività.</span><span class="sxs-lookup"><span data-stu-id="2540d-187">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="2540d-188">Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="2540d-188">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="2540d-189">Il servizio è registrato in `IHostBuilder.ConfigureServices` (*Program.cs*) con il `AddHostedService` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="2540d-189">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="2540d-190">Utilizzo di un servizio con ambito in un'attività in background</span><span class="sxs-lookup"><span data-stu-id="2540d-190">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="2540d-191">Per usare i [servizi con ambito](xref:fundamentals/dependency-injection#service-lifetimes) all' `BackgroundService`interno di un, creare un ambito.</span><span class="sxs-lookup"><span data-stu-id="2540d-191">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="2540d-192">Non viene creato automaticamente alcun ambito per un servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="2540d-192">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="2540d-193">Il servizio dell'attività in background con ambito contiene la logica dell'attività in background.</span><span class="sxs-lookup"><span data-stu-id="2540d-193">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="2540d-194">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="2540d-194">In the following example:</span></span>

* <span data-ttu-id="2540d-195">Il servizio è asincrono.</span><span class="sxs-lookup"><span data-stu-id="2540d-195">The service is asynchronous.</span></span> <span data-ttu-id="2540d-196">Il metodo `DoWork` restituisce un tipo `Task`.</span><span class="sxs-lookup"><span data-stu-id="2540d-196">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="2540d-197">A scopo dimostrativo, nel `DoWork` metodo è atteso un ritardo di dieci secondi.</span><span class="sxs-lookup"><span data-stu-id="2540d-197">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="2540d-198">Un <xref:Microsoft.Extensions.Logging.ILogger> oggetto viene inserito nel servizio.</span><span class="sxs-lookup"><span data-stu-id="2540d-198">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="2540d-199">Il servizio ospitato crea un ambito per risolvere il servizio attività in background con ambito per chiamare `DoWork` il relativo metodo.</span><span class="sxs-lookup"><span data-stu-id="2540d-199">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="2540d-200">`DoWork`Restituisce un `Task`oggetto, che è atteso `ExecuteAsync`in:</span><span class="sxs-lookup"><span data-stu-id="2540d-200">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="2540d-201">I servizi sono registrati in `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="2540d-201">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="2540d-202">Il servizio ospitato viene registrato con il `AddHostedService` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="2540d-202">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="2540d-203">Attività in background in coda</span><span class="sxs-lookup"><span data-stu-id="2540d-203">Queued background tasks</span></span>

<span data-ttu-id="2540d-204">Una coda delle attività in background è basata su .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([programma provvisoriamente per essere incorporato per ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="2540d-204">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="2540d-205">Nell'esempio seguente `QueueHostedService` :</span><span class="sxs-lookup"><span data-stu-id="2540d-205">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="2540d-206">Il `BackgroundProcessing` metodo restituisce un `Task`oggetto, che è atteso `ExecuteAsync`in.</span><span class="sxs-lookup"><span data-stu-id="2540d-206">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="2540d-207">Le attività in background nella coda vengono rimesse in coda ed `BackgroundProcessing`eseguite in.</span><span class="sxs-lookup"><span data-stu-id="2540d-207">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="2540d-208">Un `MonitorLoop` servizio gestisce le attività di Accodamento per il servizio ospitato ogni `w` volta che la chiave viene selezionata in un dispositivo di input:</span><span class="sxs-lookup"><span data-stu-id="2540d-208">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="2540d-209">`IBackgroundTaskQueue` Viene inserito`MonitorLoop` nel servizio.</span><span class="sxs-lookup"><span data-stu-id="2540d-209">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="2540d-210">`IBackgroundTaskQueue.QueueBackgroundWorkItem`viene chiamato per accodare un elemento di lavoro.</span><span class="sxs-lookup"><span data-stu-id="2540d-210">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="2540d-211">I servizi sono registrati in `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="2540d-211">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="2540d-212">Il servizio ospitato viene registrato con il `AddHostedService` metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="2540d-212">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2540d-213">In ASP.NET Core le attività in background possono essere implementate come *servizi ospitati*.</span><span class="sxs-lookup"><span data-stu-id="2540d-213">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="2540d-214">Un servizio ospitato è una classe con logica di attività in background che implementa l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="2540d-214">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2540d-215">In questo argomento vengono forniti tre esempi di servizio ospitato:</span><span class="sxs-lookup"><span data-stu-id="2540d-215">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="2540d-216">Attività in background eseguita su un timer.</span><span class="sxs-lookup"><span data-stu-id="2540d-216">Background task that runs on a timer.</span></span>
* <span data-ttu-id="2540d-217">Servizio ospitato che attiva un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="2540d-217">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="2540d-218">Il servizio con ambito può usare l' [inserimento di dipendenze](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="2540d-218">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="2540d-219">Attività in background in coda che vengono eseguite in sequenza.</span><span class="sxs-lookup"><span data-stu-id="2540d-219">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="2540d-220">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2540d-220">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2540d-221">L'app di esempio è disponibile in due versioni:</span><span class="sxs-lookup"><span data-stu-id="2540d-221">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="2540d-222">Host Web &ndash; L'host Web è utile per l'hosting di app Web.</span><span class="sxs-lookup"><span data-stu-id="2540d-222">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="2540d-223">Il codice di esempio in questo argomento è tratto dalla versione host Web dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="2540d-223">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="2540d-224">Per altre informazioni, vedere l'argomento [Host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="2540d-224">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="2540d-225">Host generico &ndash; L'host generico è una novità di ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2540d-225">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="2540d-226">Per altre informazioni, vedere l'argomento [Host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="2540d-226">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="2540d-227">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="2540d-227">Package</span></span>

<span data-ttu-id="2540d-228">Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="2540d-228">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="2540d-229">Interfaccia IHostedService</span><span class="sxs-lookup"><span data-stu-id="2540d-229">IHostedService interface</span></span>

<span data-ttu-id="2540d-230">I servizi ospitati implementano l'interfaccia <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="2540d-230">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2540d-231">L'interfaccia definisce due metodi riservati agli oggetti gestiti dall'host:</span><span class="sxs-lookup"><span data-stu-id="2540d-231">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="2540d-232">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contiene la logica per avviare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="2540d-232">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="2540d-233">Quando si usa l'[Host Web](xref:fundamentals/host/web-host), `StartAsync` viene chiamato dopo l'avvio del server e l'attivazione di [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="2540d-233">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="2540d-234">Quando si usa l'[Host generico](xref:fundamentals/host/generic-host), `StartAsync` viene chiamato prima dell'attivazione di `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="2540d-234">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="2540d-235">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Attivato quando l'host sta eseguendo un arresto normale.</span><span class="sxs-lookup"><span data-stu-id="2540d-235">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="2540d-236">`StopAsync` contiene la logica per terminare l'attività in background.</span><span class="sxs-lookup"><span data-stu-id="2540d-236">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="2540d-237">Implementare <xref:System.IDisposable> e i [finalizzatori (distruttori)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) per eliminare tutte le risorse non gestite.</span><span class="sxs-lookup"><span data-stu-id="2540d-237">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="2540d-238">Il token di annullamento ha un timeout predefinito di cinque secondi che indica che il processo di arresto non è più normale.</span><span class="sxs-lookup"><span data-stu-id="2540d-238">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="2540d-239">Quando viene richiesto l'annullamento sul token:</span><span class="sxs-lookup"><span data-stu-id="2540d-239">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="2540d-240">Tutte le operazioni in background rimanenti che sta eseguendo l'app devono essere interrotte.</span><span class="sxs-lookup"><span data-stu-id="2540d-240">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="2540d-241">Tutti i metodi eventuali chiamati in `StopAsync` devono essere completati rapidamente.</span><span class="sxs-lookup"><span data-stu-id="2540d-241">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="2540d-242">Tuttavia, dopo la richiesta di annullamento le attività non vengono abbandonate: il chiamante attende il completamento di tutte le attività.</span><span class="sxs-lookup"><span data-stu-id="2540d-242">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="2540d-243">Se l'app si arresta in modo imprevisto, ad esempio, il processo dell'app ha esito negativo, il metodo `StopAsync` potrebbe non essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="2540d-243">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="2540d-244">Pertanto è possibile che i metodi chiamati o le operazioni effettuate in `StopAsync` non vengano eseguiti.</span><span class="sxs-lookup"><span data-stu-id="2540d-244">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="2540d-245">Per estendere il timeout di arresto predefinito di cinque secondi, impostare:</span><span class="sxs-lookup"><span data-stu-id="2540d-245">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="2540d-246"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> quando si usa l'host generico.</span><span class="sxs-lookup"><span data-stu-id="2540d-246"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="2540d-247">Per altre informazioni, vedere <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="2540d-247">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="2540d-248">Impostazione di configurazione dell'host del timeout di arresto quando si usa l'host Web.</span><span class="sxs-lookup"><span data-stu-id="2540d-248">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="2540d-249">Per altre informazioni, vedere <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="2540d-249">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="2540d-250">Il servizio ospitato viene attivato una volta all'avvio dell'app e arrestato normalmente all'arresto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2540d-250">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="2540d-251">Se viene generato un errore durante l'esecuzione dell'attività in background, deve essere chiamato `Dispose` anche se `StopAsync` non viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="2540d-251">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="2540d-252">Attività in background programmate</span><span class="sxs-lookup"><span data-stu-id="2540d-252">Timed background tasks</span></span>

<span data-ttu-id="2540d-253">Un'attività programmata in background utilizza la classe [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="2540d-253">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="2540d-254">Il timer attiva il metodo `DoWork` dell'attività.</span><span class="sxs-lookup"><span data-stu-id="2540d-254">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="2540d-255">Il timer viene disabilitato con `StopAsync` ed eliminato quando il contenitore dei servizi è eliminato con `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="2540d-255">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="2540d-256">Il servizio registrato in `Startup.ConfigureServices` con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="2540d-256">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="2540d-257">Utilizzo di un servizio con ambito in un'attività in background</span><span class="sxs-lookup"><span data-stu-id="2540d-257">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="2540d-258">Per usare i [servizi con ambito](xref:fundamentals/dependency-injection#service-lifetimes) all'interno di un `IHostedService`, creare un ambito.</span><span class="sxs-lookup"><span data-stu-id="2540d-258">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="2540d-259">Non viene creato automaticamente alcun ambito per un servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="2540d-259">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="2540d-260">Il servizio dell'attività in background con ambito contiene la logica dell'attività in background.</span><span class="sxs-lookup"><span data-stu-id="2540d-260">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="2540d-261">Nell'esempio seguente, <xref:Microsoft.Extensions.Logging.ILogger> viene inserito nel servizio:</span><span class="sxs-lookup"><span data-stu-id="2540d-261">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="2540d-262">Il servizio ospitato crea un ambito per risolvere il servizio dell'attività in background con ambito e chiamare il relativo metodo `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="2540d-262">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="2540d-263">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2540d-263">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2540d-264">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="2540d-264">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="2540d-265">Attività in background in coda</span><span class="sxs-lookup"><span data-stu-id="2540d-265">Queued background tasks</span></span>

<span data-ttu-id="2540d-266">Una coda delle attività in background è basata su .NET 4. <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> x ([programma provvisoriamente per essere incorporato per ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="2540d-266">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="2540d-267">In `QueueHostedService`, le attività in background in coda vengono rimosse dalla coda ed eseguite come <xref:Microsoft.Extensions.Hosting.BackgroundService>, ovvero una classe di base per l'implementazione di un `IHostedService` a esecuzione prolungata:</span><span class="sxs-lookup"><span data-stu-id="2540d-267">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="2540d-268">I servizi vengono registrati in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2540d-268">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2540d-269">L'implementazione di `IHostedService`viene registrata con il metodo di estensione `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="2540d-269">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="2540d-270">Nella classe modello della pagina di indice:</span><span class="sxs-lookup"><span data-stu-id="2540d-270">In the Index page model class:</span></span>

* <span data-ttu-id="2540d-271">`IBackgroundTaskQueue` viene inserito nel costruttore e assegnato a `Queue`.</span><span class="sxs-lookup"><span data-stu-id="2540d-271">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="2540d-272">Un'interfaccia <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> viene inserita e assegnata a `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="2540d-272">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="2540d-273">La factory viene usata per creare istanze di <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, che consente di creare servizi all'interno di un ambito.</span><span class="sxs-lookup"><span data-stu-id="2540d-273">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="2540d-274">Viene creato un ambito per usare la classe `AppDbContext` dell'app (un [servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes)) per scrivere record di database in `IBackgroundTaskQueue` (un servizio singleton).</span><span class="sxs-lookup"><span data-stu-id="2540d-274">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="2540d-275">Quando si seleziona il pulsante **Aggiungi attività** nella pagina di indice, viene eseguito il metodo `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="2540d-275">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="2540d-276">`QueueBackgroundWorkItem`viene chiamato per accodare un elemento di lavoro:</span><span class="sxs-lookup"><span data-stu-id="2540d-276">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="2540d-277">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2540d-277">Additional resources</span></span>

* [<span data-ttu-id="2540d-278">Implementare attività in background in microservizi con IHostedService e la classe BackgroundService</span><span class="sxs-lookup"><span data-stu-id="2540d-278">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
