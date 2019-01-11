---
title: Controlli di integrità in ASP.NET Core
author: guardrex
description: Informazioni su come configurare i controlli di integrità per l'infrastruttura ASP.NET Core, ad esempio database e app.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/12/2018
uid: host-and-deploy/health-checks
ms.openlocfilehash: cf2aea91221887dad5646604214f810493d4b175
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329146"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="2cc6a-103">Controlli di integrità in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2cc6a-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="2cc6a-104">Di [Luke Latham](https://github.com/guardrex) e [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="2cc6a-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

<span data-ttu-id="2cc6a-105">ASP.NET Core offre il middleware per i controlli di integrità e librerie per la creazione di report sull'integrità dei componenti dell'infrastruttura di app.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-105">ASP.NET Core offers Health Check Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="2cc6a-106">I controlli di integrità vengono esposti da un'app come endpoint HTTP.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="2cc6a-107">È possibile configurare endpoint dei controlli di integrità per svariati scenari di monitoraggio in tempo reale:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="2cc6a-108">I probe di integrità possono essere usati dagli agenti di orchestrazione e dai servizi di bilanciamento del carico per controllare lo stato di un'app.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="2cc6a-109">Ad esempio, un agente di orchestrazione potrebbe rispondere a controllo di integrità non superato arrestando una distribuzione in sequenza o riavviando un contenitore.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="2cc6a-110">Un servizio di bilanciamento del carico potrebbe reagire a un'app non integra trasferendo il traffico dall'istanza con errori a un'istanza integra.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="2cc6a-111">È possibile monitorare lo stato di integrità di memoria, disco e altre risorse fisiche del server.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="2cc6a-112">I controlli di integrità possono testare le dipendenze di un'app, ad esempio i database e gli endpoint di servizio esterni, per verificare la disponibilità e il normale funzionamento.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="2cc6a-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2cc6a-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2cc6a-114">L'app di esempio include esempi degli scenari descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="2cc6a-115">Per eseguire l'app di esempio per un determinato scenario, usare il comando [dotnet run](/dotnet/core/tools/dotnet-run) dalla cartella del progetto in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="2cc6a-116">Per informazioni dettagliate su come usare l'app di esempio, vedere il file *README.md* dell'app di esempio e le descrizioni degli scenari in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2cc6a-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="2cc6a-117">Prerequisites</span></span>

<span data-ttu-id="2cc6a-118">I controlli di integrità vengono in genere usati con un servizio di monitoraggio esterno o un agente di orchestrazione per controllare lo stato di un'app.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="2cc6a-119">Prima di aggiungere i controlli di integrità a un'app, decidere quale sistema di monitoraggio usare.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="2cc6a-120">Il sistema di monitoraggio determina quali tipi di controlli di integrità creare e come configurare i relativi endpoint.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="2cc6a-121">Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-121">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="2cc6a-122">L'app di esempio include il codice di avvio per illustrare i controlli di integrità per diversi scenari.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-122">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="2cc6a-123">Lo scenario [Probe del database](#database-probe) verifica l'integrità di una connessione di database usando [BeatPulse](https://github.com/Xabaril/BeatPulse).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-123">The [database probe](#database-probe) scenario checks the health of a database connection using [BeatPulse](https://github.com/Xabaril/BeatPulse).</span></span> <span data-ttu-id="2cc6a-124">Lo scenario [Probe DbContext](#entity-framework-core-dbcontext-probe) verifica un database usando un elemento `DbContext` di EF Core.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-124">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="2cc6a-125">Per esplorare gli scenari relativi ai database, l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-125">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="2cc6a-126">Crea un database e specifica la stringa di connessione nel file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-126">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="2cc6a-127">Dispone dei riferimenti al pacchetto seguenti nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-127">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="2cc6a-128">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="2cc6a-128">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="2cc6a-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="2cc6a-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="2cc6a-130">[BeatPulse](https://github.com/Xabaril/BeatPulse) non viene gestito né supportato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-130">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="2cc6a-131">Un altro scenario relativo ai controlli di integrità illustra come filtrare i controlli di integrità per una porta di gestione.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-131">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="2cc6a-132">L'app di esempio richiede di creare un file *Properties/launchSettings.json* che include l'URL di gestione e la porta di gestione.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-132">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="2cc6a-133">Per altre informazioni, vedere la sezione [Filtrare in base alla porta](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-133">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="2cc6a-134">Probe di integrità di base</span><span class="sxs-lookup"><span data-stu-id="2cc6a-134">Basic health probe</span></span>

<span data-ttu-id="2cc6a-135">Per molte app, una configurazione del probe di integrità di base, che segnala la disponibilità dell'app per elaborare le richieste (*attività*), è sufficiente per individuare lo stato dell'app.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-135">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="2cc6a-136">La configurazione di base registra i servizi dei controlli di integrità e chiama il middleware per i controlli di integrità per rispondere a un endpoint di URL con una risposta di integrità.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-136">The basic configuration registers health check services and calls the Health Check Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="2cc6a-137">Per impostazione predefinita, non vengono registrati controlli di integrità specifici per testare particolari dipendenze o sottosistemi.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-137">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="2cc6a-138">L'app viene considerata integra se riesce a rispondere all'URL dell'endpoint di integrità.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-138">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="2cc6a-139">Il writer di risposta predefinito scrive lo stato (`HealthStatus`) come risposta di testo non crittografato per il client, indicando uno stato `HealthStatus.Healthy`, `HealthStatus.Degraded` o `HealthStatus.Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-139">The default response writer writes the status (`HealthStatus`) as a plaintext response back to the client, indicating either a `HealthStatus.Healthy`, `HealthStatus.Degraded` or `HealthStatus.Unhealthy` status.</span></span>

<span data-ttu-id="2cc6a-140">Registrare i servizi dei controlli di integrità con `AddHealthChecks` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-140">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2cc6a-141">Aggiungere il middleware per i controlli di integrità con `UseHealthChecks` nella pipeline di elaborazione della richiesta di `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-141">Add Health Check Middleware with `UseHealthChecks` in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="2cc6a-142">Nell'app di esempio l'endpoint di controllo di integrità viene creato in `/health` (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="2cc6a-142">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

<span data-ttu-id="2cc6a-143">Per eseguire lo scenario di configurazione di base usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-143">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="2cc6a-144">Esempio Docker</span><span class="sxs-lookup"><span data-stu-id="2cc6a-144">Docker example</span></span>

<span data-ttu-id="2cc6a-145">[Docker](xref:host-and-deploy/docker/index) offre una direttiva `HEALTHCHECK` predefinita che può essere usata per controllare lo stato di un'app che usa la configurazione dei controlli di integrità di base:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-145">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="2cc6a-146">Creare controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="2cc6a-146">Create health checks</span></span>

<span data-ttu-id="2cc6a-147">I controlli di integrità vengono creati implementando l'interfaccia `IHealthCheck`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-147">Health checks are created by implementing the `IHealthCheck` interface.</span></span> <span data-ttu-id="2cc6a-148">Il metodo `IHealthCheck.CheckHealthAsync` restituisce `Task<HealthCheckResult>` che indica l'integrità come `Healthy`, `Degraded` o `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-148">The `IHealthCheck.CheckHealthAsync` method returns a `Task<HealthCheckResult>` that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="2cc6a-149">Il risultato viene scritto come risposta di testo non crittografato con un codice di stato configurabile. La configurazione viene descritta nella sezione [Opzioni dei controlli di integrità](#health-check-options).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-149">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="2cc6a-150">`HealthCheckResult` può anche restituire coppie chiave-valore facoltative.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-150">`HealthCheckResult` can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="2cc6a-151">Controllo di integrità di esempio</span><span class="sxs-lookup"><span data-stu-id="2cc6a-151">Example health check</span></span>

<span data-ttu-id="2cc6a-152">La classe `ExampleHealthCheck` seguente illustra il layout di un controllo di integrità:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check:</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public ExampleHealthCheck()
    {
        // Use dependency injection (DI) to supply any required services to the
        // health check.
    }

    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context, 
        CancellationToken cancellationToken = default(CancellationToken))
    {
        // Execute health check logic here. This example sets a dummy
        // variable to true.
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("The check indicates a healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("The check indicates an unhealthy result."));
    }
}
```

### <a name="register-health-check-services"></a><span data-ttu-id="2cc6a-153">Registrare i servizi di controllo dell'integrità</span><span class="sxs-lookup"><span data-stu-id="2cc6a-153">Register health check services</span></span>

<span data-ttu-id="2cc6a-154">Il tipo `ExampleHealthCheck` viene aggiunto ai servizi di controllo dell'integrità con `AddCheck`:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-154">The `ExampleHealthCheck` type is added to health check services with `AddCheck`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

<span data-ttu-id="2cc6a-155">L'overload `AddCheck` illustrato nell'esempio seguente imposta lo stato di errore (`HealthStatus`) per segnalare quando il controllo di integrità indica un errore.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-155">The `AddCheck` overload shown in the following example sets the failure status (`HealthStatus`) to report when the health check reports a failure.</span></span> <span data-ttu-id="2cc6a-156">Se lo stato di errore è impostato su `null` (impostazione predefinita), viene segnalato `HealthStatus.Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-156">If the failure status is set to `null` (default), `HealthStatus.Unhealthy` is reported.</span></span> <span data-ttu-id="2cc6a-157">Questo overload è uno scenario utile per gli autori di librerie, in cui lo stato di errore indicato dalla libreria viene applicato dall'app quando si verifica un errore di controllo dell'integrità se l'implementazione del controllo dell'integrità rispetta l'impostazione.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-157">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="2cc6a-158">Si possono usare *tag* per filtrare i controlli di integrità, come illustrato più avanti nella sezione [Filtrare i controlli di integrità](#filter-health-checks).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-158">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" });
```

<span data-ttu-id="2cc6a-159">`AddCheck` può anche eseguire una funzione lambda.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-159">`AddCheck` can also execute a lambda function.</span></span> <span data-ttu-id="2cc6a-160">Nell'esempio seguente il nome del controllo di integrità viene specificato come `Example` e il controllo restituisce sempre uno stato integro:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-160">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () => 
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="2cc6a-161">Usare il middleware per i controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="2cc6a-161">Use Health Checks Middleware</span></span>

<span data-ttu-id="2cc6a-162">In `Startup.Configure` chiamare `UseHealthChecks` nella pipeline di elaborazione con l'URL dell'endpoint o il percorso relativo:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-162">In `Startup.Configure`, call `UseHealthChecks` in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

<span data-ttu-id="2cc6a-163">Se i controlli di integrità devono essere in ascolto su una porta specifica, usare un overload di `UseHealthChecks` per impostare la porta, come illustrato più avanti nella sezione [Filtrare in base alla porta](#filter-by-port):</span><span class="sxs-lookup"><span data-stu-id="2cc6a-163">If the health checks should listen on a specific port, use an overload of `UseHealthChecks` to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

<span data-ttu-id="2cc6a-164">Il middleware per i controlli di integrità è un *middleware terminale* nella pipeline di elaborazione della richiesta dell'app.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-164">Health Checks Middleware is a *terminal middleware* in the app's request processing pipeline.</span></span> <span data-ttu-id="2cc6a-165">Il primo endpoint di controllo dell'integrità ha rilevato che viene eseguita una corrispondenza esatta con l'URL della richiesta, che blocca il resto della pipeline del middleware.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-165">The first health check endpoint encountered that's an exact match to the request URL executes and short-circuits the rest of the middleware pipeline.</span></span> <span data-ttu-id="2cc6a-166">In caso di corto circuito, non viene eseguito alcun middleware che segue il controllo di integrità corrispondente.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-166">When short-circuiting occurs, no middleware following the matched health check executes.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="2cc6a-167">Opzioni dei controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="2cc6a-167">Health check options</span></span>

<span data-ttu-id="2cc6a-168">`HealthCheckOptions` consente di personalizzare il comportamento dei controlli di integrità:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-168">`HealthCheckOptions` provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="2cc6a-169">Filtrare i controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="2cc6a-169">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="2cc6a-170">Personalizzare il codice di stato HTTP</span><span class="sxs-lookup"><span data-stu-id="2cc6a-170">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="2cc6a-171">Eliminare le intestazioni della cache</span><span class="sxs-lookup"><span data-stu-id="2cc6a-171">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="2cc6a-172">Personalizzare l'output</span><span class="sxs-lookup"><span data-stu-id="2cc6a-172">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="2cc6a-173">Filtrare i controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="2cc6a-173">Filter health checks</span></span>

<span data-ttu-id="2cc6a-174">Per impostazione predefinita, il middleware per i controlli di integrità esegue tutti i controlli di integrità registrati.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-174">By default, Health Check Middleware runs all registered health checks.</span></span> <span data-ttu-id="2cc6a-175">Per eseguire un subset dei controlli di integrità, specificare una funzione che restituisce un valore booleano all'opzione `Predicate`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-175">To run a subset of health checks, provide a function that returns a boolean to the `Predicate` option.</span></span> <span data-ttu-id="2cc6a-176">Nell'esempio seguente il controllo di integrità `Bar` viene filtrato in base al tag (`bar_tag`) nell'istruzione condizionale della funzione, dove `true` viene restituito solo se la proprietà `Tag` del controllo di integrità corrisponde a `foo_tag` o a `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-176">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's `Tag` property matches `foo_tag` or `baz_tag`:</span></span>

```csharp
using System.Threading.Tasks;
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Foo", () => 
            HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
        .AddCheck("Bar", () => 
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
        .AddCheck("Baz", () => 
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // Filter out the 'Bar' health check. Only Foo and Baz execute.
        Predicate = (check) => check.Tags.Contains("foo_tag") || 
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="2cc6a-177">Personalizzare il codice di stato HTTP</span><span class="sxs-lookup"><span data-stu-id="2cc6a-177">Customize the HTTP status code</span></span>

<span data-ttu-id="2cc6a-178">Usare `ResultStatusCodes` per personalizzare il mapping dello stato di integrità dei codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-178">Use `ResultStatusCodes` to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="2cc6a-179">Le assegnazioni `StatusCode` seguenti sono i valori predefiniti usati dal middleware.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-179">The following `StatusCode` assignments are the default values used by the middleware.</span></span> <span data-ttu-id="2cc6a-180">Modificare i valori dei codici di stato in base ai requisiti.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-180">Change the status code values to meet your requirements.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The following StatusCodes are the default assignments for
        // the HealthStatus properties.
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
}
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="2cc6a-181">Eliminare le intestazioni della cache</span><span class="sxs-lookup"><span data-stu-id="2cc6a-181">Suppress cache headers</span></span>

<span data-ttu-id="2cc6a-182">`AllowCachingResponses` controlla se il middleware per i controlli di integrità aggiunge intestazioni HTTP a una risposta di probe per impedire la memorizzazione della risposta nella cache.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-182">`AllowCachingResponses` controls whether the Health Check Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="2cc6a-183">Se il valore è `false` (impostazione predefinita), il middleware imposta le intestazioni `Cache-Control`, `Expires` e `Pragma` o ne esegue l'override per impedire la memorizzazione della risposta nella cache.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-183">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="2cc6a-184">Se il valore è `true`, il middleware non modifica le intestazioni della risposta nella cache.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-184">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The default value is false.
        AllowCachingResponses = false
    });
}
```

### <a name="customize-output"></a><span data-ttu-id="2cc6a-185">Personalizzare l'output</span><span class="sxs-lookup"><span data-stu-id="2cc6a-185">Customize output</span></span>

<span data-ttu-id="2cc6a-186">L'opzione `ResponseWriter` ottiene o imposta un delegato usato per scrivere la risposta.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-186">The `ResponseWriter` option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="2cc6a-187">Il delegato predefinito scrive una risposta in testo non crittografato minima con il valore di stringa `HealthReport.Status`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-187">The default delegate writes a minimal plaintext response with the string value of `HealthReport.Status`.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // WriteResponse is a delegate used to write the response.
        ResponseWriter = WriteResponse
    });
}

private static Task WriteResponse(HttpContext httpContext, 
    HealthReport result)
{
    httpContext.Response.ContentType = "application/json";

    var json = new JObject(
        new JProperty("status", result.Status.ToString()),
        new JProperty("results", new JObject(result.Entries.Select(pair =>
            new JProperty(pair.Key, new JObject(
                new JProperty("status", pair.Value.Status.ToString()),
                new JProperty("description", pair.Value.Description),
                new JProperty("data", new JObject(pair.Value.Data.Select(
                    p => new JProperty(p.Key, p.Value))))))))));
    return httpContext.Response.WriteAsync(
        json.ToString(Formatting.Indented));
}
```

## <a name="database-probe"></a><span data-ttu-id="2cc6a-188">Probe del database</span><span class="sxs-lookup"><span data-stu-id="2cc6a-188">Database probe</span></span>

<span data-ttu-id="2cc6a-189">Un controllo di integrità può specificare una query di database da eseguire come test booleano per indicare se il database sta rispondendo normalmente.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-189">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="2cc6a-190">L'app di esempio usa [BeatPulse](https://github.com/Xabaril/BeatPulse), una libreria di controlli di integrità per le app ASP.NET Core, per eseguire un controllo di integrità su un database di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-190">The sample app uses [BeatPulse](https://github.com/Xabaril/BeatPulse), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="2cc6a-191">BeatPulse esegue una query `SELECT 1` sul database per verificare che la connessione al database sia integra.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-191">BeatPulse executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="2cc6a-192">Quando si controlla una connessione di database con una query, scegliere una query che viene restituita rapidamente.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-192">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="2cc6a-193">Dall'approccio alla query possono derivare il sovraccarico del database e il degrado delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-193">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="2cc6a-194">Nella maggior parte dei casi, l'esecuzione di una query di test non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-194">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="2cc6a-195">È sufficiente stabilire una connessione al database.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-195">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="2cc6a-196">Se invece è necessario eseguire una query, scegliere una semplice query SELECT, ad esempio `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-196">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="2cc6a-197">Per usare la libreria BeatPulse, includere un riferimento al pacchetto [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-197">In order to use the BeatPulse library, include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="2cc6a-198">Fornire una stringa di connessione di database valida nel file *appsettings.json* dell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-198">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="2cc6a-199">L'app usa una database di SQL Server denominato `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-199">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="2cc6a-200">Registrare i servizi dei controlli di integrità con `AddHealthChecks` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-200">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2cc6a-201">L'app di esempio chiama il metodo `AddSqlServer` di BeatPulse con la stringa di connessione del database (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="2cc6a-201">The sample app calls BeatPulse's `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="2cc6a-202">Chiamare il middleware per i controlli di integrità nella pipeline di elaborazione dell'app in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-202">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="2cc6a-203">Per eseguire lo scenario di probe del database di base usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-203">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="2cc6a-204">[BeatPulse](https://github.com/Xabaril/BeatPulse) non viene gestito né supportato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-204">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="2cc6a-205">Probe DbContext di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2cc6a-205">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="2cc6a-206">Il controllo `DbContext` verifica che l'app possa comunicare con il database configurato per un `DbContext` di EF Core.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-206">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="2cc6a-207">Il controllo `DbContext` è supportato nelle app che:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-207">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="2cc6a-208">Usano [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-208">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="2cc6a-209">Includono un riferimento del pacchetto a [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-209">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="2cc6a-210">`AddDbContextCheck<TContext>` registra un controllo di integrità per un `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-210">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="2cc6a-211">`DbContext` viene reso disponibile come `TContext` al metodo.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-211">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="2cc6a-212">È disponibile un overload per configurare lo stato di errore, i tag e una query di test personalizzata.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-212">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="2cc6a-213">Per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-213">By default:</span></span>

* <span data-ttu-id="2cc6a-214">`DbContextHealthCheck` chiama il metodo `CanConnectAsync` di EF Core.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-214">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="2cc6a-215">È possibile determinare l'operazione da eseguire quando si controlla l'integrità usando gli overload del metodo `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-215">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="2cc6a-216">Il nome del controllo di integrità è il nome del tipo `TContext`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-216">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="2cc6a-217">Nell'app di esempio `AppDbContext` viene fornito a `AddDbContextCheck` e registrato come servizio in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-217">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="2cc6a-218">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-218">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="2cc6a-219">Nell'app di esempio `UseHealthChecks` aggiunge il middleware per i controlli di integrità in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-219">In the sample app, `UseHealthChecks` adds the Health Check Middleware in `Startup.Configure`.</span></span>

<span data-ttu-id="2cc6a-220">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-220">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="2cc6a-221">Per eseguire lo scenario di probe `DbContext` usando l'app di esempio, verificare che il database specificato dalla stringa di connessione non esista nell'istanza di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-221">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="2cc6a-222">Se il database esiste, eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-222">If the database exists, delete it.</span></span>

<span data-ttu-id="2cc6a-223">Eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-223">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="2cc6a-224">Dopo che l'app è in esecuzione, controllare lo stato di integrità effettuando una richiesta all'endpoint `/health` in un browser.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-224">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="2cc6a-225">Il database e `AppDbContext` non esistono, quindi l'app fornisce la risposta seguente:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-225">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="2cc6a-226">Attivare l'app di esempio per creare il database.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-226">Trigger the sample app to create the database.</span></span> <span data-ttu-id="2cc6a-227">Effettuare una richiesta a `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-227">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="2cc6a-228">L'app risponde:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-228">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="2cc6a-229">Effettuare una richiesta all'endpoint `/health`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-229">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="2cc6a-230">Il database e il contesto esistono, quindi l'app risponde:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-230">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="2cc6a-231">Attivare l'app di esempio per eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-231">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="2cc6a-232">Effettuare una richiesta a `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-232">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="2cc6a-233">L'app risponde:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-233">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="2cc6a-234">Effettuare una richiesta all'endpoint `/health`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-234">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="2cc6a-235">L'app fornisce una risposta non integra:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-235">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="2cc6a-236">Separare i probe di idoneità e di attività</span><span class="sxs-lookup"><span data-stu-id="2cc6a-236">Separate readiness and liveness probes</span></span>

<span data-ttu-id="2cc6a-237">In alcuni scenari di hosting vengono usati due controlli di integrità che distinguono due stati dell'app:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-237">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="2cc6a-238">L'app è funzionante, ma non ancora pronta a ricevere le richieste.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-238">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="2cc6a-239">Questo stato è l'*idoneità* dell'app.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-239">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="2cc6a-240">L'app è funzionante e risponde alle richieste.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-240">The app is functioning and responding to requests.</span></span> <span data-ttu-id="2cc6a-241">Questo stato è l'*attività* dell'app.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-241">This state is the app's *liveness*.</span></span>

<span data-ttu-id="2cc6a-242">Il controllo di idoneità in genere esegue un set di controlli più completo e dispendioso in termini di tempo per determinare se tutti i sottosistemi e le risorse dell'app sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-242">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="2cc6a-243">Un controllo di attività esegue semplicemente un rapido controllo per determinare se l'app è disponibile per elaborare le richieste.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-243">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="2cc6a-244">Dopo che l'app ha superato il controllo di idoneità, non è necessario caricare ulteriormente l'app con il dispendioso set di controlli di idoneità. Sarà sufficiente controllare solo l'attività.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-244">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="2cc6a-245">L'app di esempio contiene un controllo di integrità per segnalare il completamento dell'attività di avvio con esecuzione prolungata in un [servizio ospitato](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-245">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="2cc6a-246">`StartupHostedServiceHealthCheck` espone una proprietà, `StartupTaskCompleted`, che il servizio ospitato può impostare su `true` al termine dell'attività con esecuzione prolungata (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="2cc6a-246">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=5)]

<span data-ttu-id="2cc6a-247">L'attività in background con esecuzione prolungata viene avviata da un [servizio ospitato](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-247">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="2cc6a-248">Al termine dell'attività, `StartupHostedServiceHealthCheck.StartupTaskCompleted` viene impostata su `true`:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-248">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="2cc6a-249">Il controllo di integrità viene registrato con `AddCheck` in `Startup.ConfigureServices` insieme al servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-249">The health check is registered with `AddCheck` in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="2cc6a-250">Poiché il servizio ospitato deve impostare la proprietà sul controllo di integrità, il controllo di integrità viene anche registrato nel contenitore del servizio (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="2cc6a-250">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="2cc6a-251">Chiamare il middleware per i controlli di integrità nella pipeline di elaborazione dell'app in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-251">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="2cc6a-252">Nell'app di esempio gli endpoint dei controlli di integrità vengono creati in `/health/ready` per il controllo di idoneità e in `/health/live` per il controllo di attività.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-252">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="2cc6a-253">Il controllo di idoneità filtra i controlli di integrità per il controllo di integrità con il tag `ready`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-253">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="2cc6a-254">Il controllo di attività filtra `StartupHostedServiceHealthCheck` restituendo `false` in `HealthCheckOptions.Predicate` (per altre informazioni, vedere [Filtrare i controlli di integrità](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="2cc6a-254">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the `HealthCheckOptions.Predicate` (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

<span data-ttu-id="2cc6a-255">Per eseguire lo scenario di configurazione di idoneità/attività usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-255">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="2cc6a-256">In a browser visitare più volte `/health/ready` per 15 secondi.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-256">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="2cc6a-257">Il controllo di integrità segnala `Unhealthy` per i primi 15 secondi.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-257">The health check reports `Unhealthy` for the first 15 seconds.</span></span> <span data-ttu-id="2cc6a-258">Dopo 15 secondi, l'endpoint segnala `Healthy`, che indica il completamento dell'attività con esecuzione prolungata da parte del servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-258">After 15 seconds, the endpoint reports `Healthy`, which reflects the completion of the long-running task by the hosted service.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="2cc6a-259">Esempio di Kubernetes</span><span class="sxs-lookup"><span data-stu-id="2cc6a-259">Kubernetes example</span></span>

<span data-ttu-id="2cc6a-260">L'uso di controlli di idoneità e attività separati è utile in un ambiente come [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-260">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="2cc6a-261">In Kubernetes un'app potrebbe dover eseguire un'operazione di avvio con esecuzione prolungata prima di accettare le richieste, ad esempio un test della disponibilità del database sottostante.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-261">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="2cc6a-262">L'uso di controlli separati consente all'agente di orchestrazione di distinguere se l'app è funzionante, ma non è ancora pronta o se l'app non è stata avviata.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-262">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="2cc6a-263">Per altre informazioni sui probe di idoneità e di attività in Kubernetes, vedere [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (Configurare i probe di attività e di idoneità in Kubernetes) nella documentazione di Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-263">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="2cc6a-264">L'esempio seguente illustra la configurazione di un probe di idoneità di Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-264">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

```
spec:
  template:
  spec:
    readinessProbe:
      # an http probe
      httpGet:
        path: /health/ready
        port: 80
      # length of time to wait for a pod to initialize
      # after pod startup, before applying health checking
      initialDelaySeconds: 30
      timeoutSeconds: 1
    ports:
      - containerPort: 80
```

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="2cc6a-265">Probe basato sulle metriche con un writer di risposta personalizzata</span><span class="sxs-lookup"><span data-stu-id="2cc6a-265">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="2cc6a-266">L'app di esempio illustra un controllo di integrità della memoria con un writer di risposta personalizzata.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-266">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="2cc6a-267">`MemoryHealthCheck` segnala uno stato danneggiato se l'app usa più di una determinata soglia di memoria (1 GB nell'app di esempio).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-267">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="2cc6a-268">`HealthCheckResult` include informazioni sul Garbage Collector (GC) per l'app (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="2cc6a-268">The `HealthCheckResult` includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="2cc6a-269">Registrare i servizi dei controlli di integrità con `AddHealthChecks` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-269">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2cc6a-270">Invece di abilitare il controllo di integrità passandolo ad `AddCheck`, `MemoryHealthCheck` viene registrato come servizio.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-270">Instead of enabling the health check by passing it to `AddCheck`, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="2cc6a-271">Tutti i servizi registrati da `IHealthCheck` sono disponibili per il middleware e i servizi di controllo dell'integrità.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-271">All `IHealthCheck` registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="2cc6a-272">È consigliabile registrare i servizi di controllo dell'integrità come servizi Singleton.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-272">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="2cc6a-273">*CustomWriterStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-273">*CustomWriterStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="2cc6a-274">Chiamare il middleware per i controlli di integrità nella pipeline di elaborazione dell'app in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-274">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="2cc6a-275">Viene fornito un delegato `WriteResponse` alla proprietà `ResponseWriter` per visualizzare una risposta JSON personalizzata quando il controllo di integrità viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-275">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

<span data-ttu-id="2cc6a-276">Il metodo `WriteResponse` formatta `CompositeHealthCheckResult` in un oggetto JSON e genera l'output JSON per la risposta del controllo di integrità:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-276">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="2cc6a-277">Per eseguire lo scenario di probe basato sulle metriche con il writer di risposta personalizzata usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-277">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="2cc6a-278">[BeatPulse](https://github.com/Xabaril/BeatPulse) include gli scenari di controllo dell'integrità basati sulle metriche, inclusi i controlli di attività del valore massimo e di archiviazione su disco.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-278">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="2cc6a-279">[BeatPulse](https://github.com/Xabaril/BeatPulse) non viene gestito né supportato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-279">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="2cc6a-280">Filtrare in base alla porta</span><span class="sxs-lookup"><span data-stu-id="2cc6a-280">Filter by port</span></span>

<span data-ttu-id="2cc6a-281">La chiamata a `UseHealthChecks` con una porta limita le richieste di controllo dell'integrità alla porta specificata.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-281">Calling `UseHealthChecks` with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="2cc6a-282">Questa chiamata viene in genere usata nell'ambiente di un contenitore per esporre una porta per il monitoraggio dei servizi.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-282">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="2cc6a-283">L'app di esempio configura la porta usando il [provider di configurazione della variabile di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-283">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="2cc6a-284">La porta viene impostata nel file *launchSettings.json* e passata al provider di configurazione tramite una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-284">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="2cc6a-285">È anche necessario configurare il server per l'ascolto di richieste sulla porta di gestione.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-285">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="2cc6a-286">Per usare l'app di esempio per illustrare la configurazione della porta di gestione, creare il file *launchSettings.json* in una cartella *Properties*.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-286">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="2cc6a-287">Il file *launchSettings.json* di esempio non è incluso nei file di progetto dell'app di esempio e deve essere creato manualmente.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-287">The following *launchSettings.json* file isn't included in the sample app's project files and must be created manually.</span></span>

<span data-ttu-id="2cc6a-288">*Properties/launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-288">*Properties/launchSettings.json*:</span></span>

```json
{
  "profiles": {
    "SampleApp": {
      "commandName": "Project",
      "commandLineArgs": "",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development",
        "ASPNETCORE_URLS": "http://localhost:5000/;http://localhost:5001/",
        "ASPNETCORE_MANAGEMENTPORT": "5001"
      },
      "applicationUrl": "http://localhost:5000/"
    }
  }
}
```

<span data-ttu-id="2cc6a-289">Registrare i servizi dei controlli di integrità con `AddHealthChecks` in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-289">Register health check services with `AddHealthChecks` in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2cc6a-290">La chiamata a `UseHealthChecks` specifica la porta di gestione (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="2cc6a-290">The call to `UseHealthChecks` specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> <span data-ttu-id="2cc6a-291">È possibile evitare di creare il file *launchSettings.json* nell'app di esempio impostando gli URL e la porta di gestione in modo esplicito nel codice.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-291">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="2cc6a-292">In *Program.cs*, dove viene creato `WebHostBuilder`, aggiungere una chiamata a `UseUrls` e fornire l'endpoint della porta di gestione e l'endpoint di risposta normale dell'app.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-292">In *Program.cs* where the `WebHostBuilder` is created, add a call to `UseUrls` and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="2cc6a-293">In *ManagementPortStartup.cs*, dove viene chiamato `UseHealthChecks`, specificare la porta di gestione in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-293">In *ManagementPortStartup.cs* where `UseHealthChecks` is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="2cc6a-294">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-294">*Program.cs*:</span></span>
>
> ```csharp
> return new WebHostBuilder()
>     .UseConfiguration(config)
>     .UseUrls("http://localhost:5000/;http://localhost:5001/")
>     .ConfigureLogging(builder =>
>     {
>         builder.SetMinimumLevel(LogLevel.Trace);
>         builder.AddConfiguration(config);
>         builder.AddConsole();
>     })
>     .UseKestrel()
>     .UseStartup(startupType)
>     .Build();
> ```
>
> <span data-ttu-id="2cc6a-295">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-295">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="2cc6a-296">Per eseguire lo scenario di configurazione della porta di gestione usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-296">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="2cc6a-297">Distribuire una libreria di controllo di integrità</span><span class="sxs-lookup"><span data-stu-id="2cc6a-297">Distribute a health check library</span></span>

<span data-ttu-id="2cc6a-298">Per distribuire un controllo di integrità come libreria:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-298">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="2cc6a-299">Scrivere un controllo di integrità che implementa l'interfaccia `IHealthCheck` come classe autonoma.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-299">Write a health check that implements the `IHealthCheck` interface as a standalone class.</span></span> <span data-ttu-id="2cc6a-300">La classe può basarsi su [inserimento delle dipendenze](xref:fundamentals/dependency-injection), attivazione del tipo e [opzioni denominate](xref:fundamentals/configuration/options) per accedere ai dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-300">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   namespace SampleApp
   {
       public class ExampleHealthCheck : IHealthCheck
       {
           private readonly string _data1;
           private readonly int? _data2;

           public ExampleHealthCheck(string data1, int? data2)
           {
               _data1 = data1 ?? throw new ArgumentNullException(nameof(data1));
               _data2 = data2 ?? throw new ArgumentNullException(nameof(data2));
           }

           public async Task<HealthCheckResult> CheckHealthAsync(
               HealthCheckContext context, CancellationToken cancellationToken)
           {
               try
               {
                   // Health check logic
                   //
                   // data1 and data2 are used in the method to
                   // run the probe's health check logic.

                   // Assume that it's possible for this health check
                   // to throw an AccessViolationException.

                   return HealthCheckResult.Healthy();
               }
               catch (AccessViolationException ex)
               {
                   return new HealthCheckResult(
                       context.Registration.FailureStatus,
                       description: "An access violation occurred during the check.",
                       exception: ex,
                       data: null);
               }
           }
       }
   }
   ```

1. <span data-ttu-id="2cc6a-301">Scrivere un metodo di estensione con i parametri che l'app chiama nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-301">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="2cc6a-302">Nell'esempio seguente si presuma la firma del metodo di controllo dell'integrità seguente:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-302">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="2cc6a-303">La firma precedente indica che `ExampleHealthCheck` richiede dati aggiuntivi per elaborare la logica del probe di controllo dell'integrità.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-303">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="2cc6a-304">I dati vengono forniti al delegato usato per creare l'istanza del controllo di integrità quando il controllo di integrità viene registrato con un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-304">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="2cc6a-305">Nell'esempio seguente il chiamante specifica facoltativamente:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-305">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="2cc6a-306">nome del controllo integrità (`name`).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-306">health check name (`name`).</span></span> <span data-ttu-id="2cc6a-307">Se `null`, viene usato `example_health_check`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-307">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="2cc6a-308">punto dati stringa per il controllo di integrità (`data1`).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-308">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="2cc6a-309">punto dati Integer per il controllo di integrità (`data2`).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-309">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="2cc6a-310">Se `null`, viene usato `1`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-310">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="2cc6a-311">stato dell'errore (`HealthStatus`).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-311">failure status (`HealthStatus`).</span></span> <span data-ttu-id="2cc6a-312">Il valore predefinito è `null`.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-312">The default is `null`.</span></span> <span data-ttu-id="2cc6a-313">Se `null`, viene segnalato `HealthStatus.Unhealthy` come stato di errore.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-313">If `null`, `HealthStatus.Unhealthy` is reported for a failure status.</span></span>
   * <span data-ttu-id="2cc6a-314">tag (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-314">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string NAME = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder, 
           string name = default, 
           string data1, 
           int data2 = 1, 
           HealthStatus? failureStatus = default, 
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? NAME,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="2cc6a-315">Server di pubblicazione dei controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="2cc6a-315">Health Check Publisher</span></span>

<span data-ttu-id="2cc6a-316">Quando `IHealthCheckPublisher` viene aggiunto al contenitore del servizio, il sistema di controllo dell'integrità esegue periodicamente i controlli di integrità e chiama `PublishAsync` con il risultato.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-316">When an `IHealthCheckPublisher` is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="2cc6a-317">Ciò è utile in uno scenario relativo a un sistema di monitoraggio dell'integrità basato su push, che prevede che ogni processo chiami periodicamente il sistema di monitoraggio per determinare l'integrità.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-317">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="2cc6a-318">L'interfaccia `IHealthCheckPublisher` ha un solo metodo:</span><span class="sxs-lookup"><span data-stu-id="2cc6a-318">The `IHealthCheckPublisher` interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

> [!NOTE]
> <span data-ttu-id="2cc6a-319">[BeatPulse](https://github.com/Xabaril/BeatPulse) include i server di pubblicazione per diversi sistemi, incluso [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="2cc6a-319">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="2cc6a-320">[BeatPulse](https://github.com/Xabaril/BeatPulse) non viene gestito né supportato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2cc6a-320">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>
