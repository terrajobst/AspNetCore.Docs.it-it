---
title: Controlli di integrità in ASP.NET Core
author: guardrex
description: Informazioni su come configurare i controlli di integrità per l'infrastruttura ASP.NET Core, ad esempio database e app.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 11/03/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: c7cf1c432d2186f0e2f9f5082e8a2229d8a5ef8f
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463027"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="09693-103">Controlli di integrità in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09693-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="09693-104">Di [Luke Latham](https://github.com/guardrex) e [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="09693-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="09693-105">ASP.NET Core offre middleware e librerie di controlli di integrità per segnalare l'integrità dei componenti dell'infrastruttura di app.</span><span class="sxs-lookup"><span data-stu-id="09693-105">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="09693-106">I controlli di integrità vengono esposti da un'app come endpoint HTTP.</span><span class="sxs-lookup"><span data-stu-id="09693-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="09693-107">È possibile configurare endpoint dei controlli di integrità per svariati scenari di monitoraggio in tempo reale:</span><span class="sxs-lookup"><span data-stu-id="09693-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="09693-108">I probe di integrità possono essere usati dagli agenti di orchestrazione e dai servizi di bilanciamento del carico per controllare lo stato di un'app.</span><span class="sxs-lookup"><span data-stu-id="09693-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="09693-109">Ad esempio, un agente di orchestrazione potrebbe rispondere a controllo di integrità non superato arrestando una distribuzione in sequenza o riavviando un contenitore.</span><span class="sxs-lookup"><span data-stu-id="09693-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="09693-110">Un servizio di bilanciamento del carico potrebbe reagire a un'app non integra trasferendo il traffico dall'istanza con errori a un'istanza integra.</span><span class="sxs-lookup"><span data-stu-id="09693-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="09693-111">È possibile monitorare lo stato di integrità di memoria, disco e altre risorse fisiche del server.</span><span class="sxs-lookup"><span data-stu-id="09693-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="09693-112">I controlli di integrità possono testare le dipendenze di un'app, ad esempio i database e gli endpoint di servizio esterni, per verificare la disponibilità e il normale funzionamento.</span><span class="sxs-lookup"><span data-stu-id="09693-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="09693-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="09693-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="09693-114">L'app di esempio include esempi degli scenari descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="09693-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="09693-115">Per eseguire l'app di esempio per un determinato scenario, usare il comando [dotnet run](/dotnet/core/tools/dotnet-run) dalla cartella del progetto in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="09693-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="09693-116">Per informazioni dettagliate su come usare l'app di esempio, vedere il file *README.md* dell'app di esempio e le descrizioni degli scenari in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="09693-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09693-117">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="09693-117">Prerequisites</span></span>

<span data-ttu-id="09693-118">I controlli di integrità vengono in genere usati con un servizio di monitoraggio esterno o un agente di orchestrazione per controllare lo stato di un'app.</span><span class="sxs-lookup"><span data-stu-id="09693-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="09693-119">Prima di aggiungere i controlli di integrità a un'app, decidere quale sistema di monitoraggio usare.</span><span class="sxs-lookup"><span data-stu-id="09693-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="09693-120">Il sistema di monitoraggio determina quali tipi di controlli di integrità creare e come configurare i relativi endpoint.</span><span class="sxs-lookup"><span data-stu-id="09693-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="09693-121">Si fa riferimento al pacchetto [Microsoft. AspNetCore. Diagnostics. HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) in modo implicito per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09693-121">The [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package is referenced implicitly for ASP.NET Core apps.</span></span> <span data-ttu-id="09693-122">Per eseguire i controlli di integrità usando Entity Framework Core, aggiungere un riferimento al pacchetto [Microsoft. Extensions. Diagnostics. HealthChecks. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) .</span><span class="sxs-lookup"><span data-stu-id="09693-122">To perform health checks using Entity Framework Core, add a package reference to the [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) package.</span></span>

<span data-ttu-id="09693-123">L'app di esempio include il codice di avvio per illustrare i controlli di integrità per diversi scenari.</span><span class="sxs-lookup"><span data-stu-id="09693-123">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="09693-124">Lo scenario di [probe del database](#database-probe) controlla l'integrità di una connessione di database usando [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="09693-124">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="09693-125">Lo scenario [Probe DbContext](#entity-framework-core-dbcontext-probe) verifica un database usando un elemento `DbContext` di EF Core.</span><span class="sxs-lookup"><span data-stu-id="09693-125">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="09693-126">Per esplorare gli scenari relativi ai database, l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="09693-126">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="09693-127">Crea un database e specifica la stringa di connessione nel file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="09693-127">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="09693-128">Dispone dei riferimenti al pacchetto seguenti nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="09693-128">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="09693-129">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="09693-129">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="09693-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="09693-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="09693-131">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) non è gestito né supportato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="09693-131">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="09693-132">Un altro scenario relativo ai controlli di integrità illustra come filtrare i controlli di integrità per una porta di gestione.</span><span class="sxs-lookup"><span data-stu-id="09693-132">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="09693-133">L'app di esempio richiede di creare un file *Properties/launchSettings.json* che include l'URL di gestione e la porta di gestione.</span><span class="sxs-lookup"><span data-stu-id="09693-133">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="09693-134">Per altre informazioni, vedere la sezione [Filtrare in base alla porta](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="09693-134">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="09693-135">Probe di integrità di base</span><span class="sxs-lookup"><span data-stu-id="09693-135">Basic health probe</span></span>

<span data-ttu-id="09693-136">Per molte app, una configurazione del probe di integrità di base, che segnala la disponibilità dell'app per elaborare le richieste (*attività*), è sufficiente per individuare lo stato dell'app.</span><span class="sxs-lookup"><span data-stu-id="09693-136">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="09693-137">La configurazione di base registra i servizi di controllo integrità e chiama il middleware dei controlli di integrità per rispondere a un endpoint URL con una risposta di integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-137">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="09693-138">Per impostazione predefinita, non vengono registrati controlli di integrità specifici per testare particolari dipendenze o sottosistemi.</span><span class="sxs-lookup"><span data-stu-id="09693-138">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="09693-139">L'app viene considerata integra se riesce a rispondere all'URL dell'endpoint di integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-139">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="09693-140">Il writer di risposta predefinito scrive lo stato (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) come risposta di testo non crittografato per il client, indicando uno stato [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) o [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).</span><span class="sxs-lookup"><span data-stu-id="09693-140">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="09693-141">Registrare i servizi dei controlli di integrità con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="09693-141">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="09693-142">Creare un endpoint di controllo di integrità chiamando `MapHealthChecks` in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="09693-142">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="09693-143">Nell'app di esempio l'endpoint di controllo di integrità viene creato in `/health` (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-143">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

```csharp
public class BasicStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks();
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHealthChecks("/health");
        });
    }
}
```

<span data-ttu-id="09693-144">Per eseguire lo scenario di configurazione di base usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="09693-144">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="09693-145">Esempio Docker</span><span class="sxs-lookup"><span data-stu-id="09693-145">Docker example</span></span>

<span data-ttu-id="09693-146">[Docker](xref:host-and-deploy/docker/index) offre una direttiva `HEALTHCHECK` predefinita che può essere usata per controllare lo stato di un'app che usa la configurazione dei controlli di integrità di base:</span><span class="sxs-lookup"><span data-stu-id="09693-146">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="09693-147">Creare controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-147">Create health checks</span></span>

<span data-ttu-id="09693-148">I controlli di integrità vengono creati implementando l'interfaccia <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck>.</span><span class="sxs-lookup"><span data-stu-id="09693-148">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="09693-149">Il metodo <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> restituisce <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> che indica l'integrità come `Healthy`, `Degraded` o `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="09693-149">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="09693-150">Il risultato viene scritto come risposta di testo non crittografato con un codice di stato configurabile. La configurazione viene descritta nella sezione [Opzioni dei controlli di integrità](#health-check-options).</span><span class="sxs-lookup"><span data-stu-id="09693-150">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="09693-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> può anche restituire coppie chiave-valore facoltative.</span><span class="sxs-lookup"><span data-stu-id="09693-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

<span data-ttu-id="09693-152">Nella classe `ExampleHealthCheck` seguente viene illustrato il layout di un controllo di integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="09693-153">La logica dei controlli di integrità viene inserita nel metodo `CheckHealthAsync`.</span><span class="sxs-lookup"><span data-stu-id="09693-153">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="09693-154">Nell'esempio seguente viene impostata una variabile fittizia, `healthCheckResultHealthy`, `true`.</span><span class="sxs-lookup"><span data-stu-id="09693-154">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="09693-155">Se il valore di `healthCheckResultHealthy` è impostato su `false`, viene restituito lo stato [HealthCheckResult. unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) .</span><span class="sxs-lookup"><span data-stu-id="09693-155">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("A healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("An unhealthy result."));
    }
}
```

## <a name="register-health-check-services"></a><span data-ttu-id="09693-156">Registrare i servizi di controllo dell'integrità</span><span class="sxs-lookup"><span data-stu-id="09693-156">Register health check services</span></span>

<span data-ttu-id="09693-157">Il tipo di `ExampleHealthCheck` viene aggiunto ai servizi di controllo integrità con <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="09693-157">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="09693-158">L'overload <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> illustrato nell'esempio seguente imposta lo stato di errore (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) per segnalare quando il controllo di integrità indica un errore.</span><span class="sxs-lookup"><span data-stu-id="09693-158">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="09693-159">Se lo stato di errore è impostato su `null` (impostazione predefinita), viene segnalato [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).</span><span class="sxs-lookup"><span data-stu-id="09693-159">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="09693-160">Questo overload è uno scenario utile per gli autori di librerie, in cui lo stato di errore indicato dalla libreria viene applicato dall'app quando si verifica un errore di controllo dell'integrità se l'implementazione del controllo dell'integrità rispetta l'impostazione.</span><span class="sxs-lookup"><span data-stu-id="09693-160">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="09693-161">Si possono usare *tag* per filtrare i controlli di integrità, come illustrato più avanti nella sezione [Filtrare i controlli di integrità](#filter-health-checks).</span><span class="sxs-lookup"><span data-stu-id="09693-161">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="09693-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> può anche eseguire una funzione lambda.</span><span class="sxs-lookup"><span data-stu-id="09693-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="09693-163">Nell'esempio seguente il nome del controllo di integrità viene specificato come `Example` e il controllo restituisce sempre uno stato integro:</span><span class="sxs-lookup"><span data-stu-id="09693-163">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

## <a name="use-health-checks-routing"></a><span data-ttu-id="09693-164">Usare i controlli di integrità routing</span><span class="sxs-lookup"><span data-stu-id="09693-164">Use Health Checks Routing</span></span>

<span data-ttu-id="09693-165">In `Startup.Configure` chiamare `MapHealthChecks` nel generatore di endpoint con l'URL dell'endpoint o il percorso relativo:</span><span class="sxs-lookup"><span data-stu-id="09693-165">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a><span data-ttu-id="09693-166">Richiedi host</span><span class="sxs-lookup"><span data-stu-id="09693-166">Require host</span></span>

<span data-ttu-id="09693-167">Chiamare `RequireHost` per specificare uno o più host consentiti per l'endpoint di controllo integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-167">Call `RequireHost` to specify one or more permitted hosts for the health check endpoint.</span></span> <span data-ttu-id="09693-168">Gli host devono essere Unicode anziché Punycode e possono includere una porta.</span><span class="sxs-lookup"><span data-stu-id="09693-168">Hosts should be Unicode rather than punycode and may include a port.</span></span> <span data-ttu-id="09693-169">Se non viene specificata una raccolta, viene accettato qualsiasi host.</span><span class="sxs-lookup"><span data-stu-id="09693-169">If a collection isn't supplied, any host is accepted.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

<span data-ttu-id="09693-170">Per altre informazioni, vedere la sezione [Filtrare in base alla porta](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="09693-170">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

### <a name="require-authorization"></a><span data-ttu-id="09693-171">Richiedi autorizzazione</span><span class="sxs-lookup"><span data-stu-id="09693-171">Require authorization</span></span>

<span data-ttu-id="09693-172">Chiamare `RequireAuthorization` per eseguire il middleware di autorizzazione nell'endpoint della richiesta di controllo integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-172">Call `RequireAuthorization` to run Authorization Middleware on the health check request endpoint.</span></span> <span data-ttu-id="09693-173">Un overload `RequireAuthorization` accetta uno o più criteri di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="09693-173">A `RequireAuthorization` overload accepts one or more authorization policies.</span></span> <span data-ttu-id="09693-174">Se un criterio non viene specificato, vengono usati i criteri di autorizzazione predefiniti.</span><span class="sxs-lookup"><span data-stu-id="09693-174">If a policy isn't provided, the default authorization policy is used.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a><span data-ttu-id="09693-175">Abilitare richieste tra le origini (CORS)</span><span class="sxs-lookup"><span data-stu-id="09693-175">Enable Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="09693-176">Sebbene i controlli di integrità eseguiti manualmente da un browser non siano uno scenario di utilizzo comune, il middleware CORS può essere abilitato chiamando `RequireCors` sugli endpoint di controllo dell'integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-176">Although performing health checks manually from a browser isn't a common use scenario, CORS Middleware can be enabled by calling `RequireCors` on health checks endpoints.</span></span> <span data-ttu-id="09693-177">Un overload di `RequireCors` accetta un delegato del generatore di criteri CORS (`CorsPolicyBuilder`) o un nome di criterio.</span><span class="sxs-lookup"><span data-stu-id="09693-177">A `RequireCors` overload accepts a CORS policy builder delegate (`CorsPolicyBuilder`) or a policy name.</span></span> <span data-ttu-id="09693-178">Se non viene specificato alcun criterio, viene usato il criterio CORS predefinito.</span><span class="sxs-lookup"><span data-stu-id="09693-178">If a policy isn't provided, the default CORS policy is used.</span></span> <span data-ttu-id="09693-179">Per ulteriori informazioni, vedere <xref:security/cors>.</span><span class="sxs-lookup"><span data-stu-id="09693-179">For more information, see <xref:security/cors>.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="09693-180">Opzioni dei controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-180">Health check options</span></span>

<span data-ttu-id="09693-181"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> consente di personalizzare il comportamento dei controlli di integrità:</span><span class="sxs-lookup"><span data-stu-id="09693-181"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="09693-182">Filtrare i controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-182">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="09693-183">Personalizzare il codice di stato HTTP</span><span class="sxs-lookup"><span data-stu-id="09693-183">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="09693-184">Eliminare le intestazioni della cache</span><span class="sxs-lookup"><span data-stu-id="09693-184">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="09693-185">Personalizzare l'output</span><span class="sxs-lookup"><span data-stu-id="09693-185">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="09693-186">Filtrare i controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-186">Filter health checks</span></span>

<span data-ttu-id="09693-187">Per impostazione predefinita, il middleware controlli integrità esegue tutti i controlli di integrità registrati.</span><span class="sxs-lookup"><span data-stu-id="09693-187">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="09693-188">Per eseguire un subset dei controlli di integrità, specificare una funzione che restituisce un valore booleano all'opzione <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate>.</span><span class="sxs-lookup"><span data-stu-id="09693-188">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="09693-189">Nell'esempio seguente il controllo di integrità `Bar` viene filtrato in base al tag (`bar_tag`) nell'istruzione condizionale della funzione, dove `true` viene restituito solo se la proprietà <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> del controllo di integrità corrisponde a `foo_tag` o a `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="09693-189">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

<span data-ttu-id="09693-190">In `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="09693-190">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

<span data-ttu-id="09693-191">In `Startup.Configure`, il `Predicate` filtra il controllo integrità "barra".</span><span class="sxs-lookup"><span data-stu-id="09693-191">In `Startup.Configure`, the `Predicate` filters out the 'Bar' health check.</span></span> <span data-ttu-id="09693-192">Solo foo e Baz Execute.:</span><span class="sxs-lookup"><span data-stu-id="09693-192">Only Foo and Baz execute.:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
});
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="09693-193">Personalizzare il codice di stato HTTP</span><span class="sxs-lookup"><span data-stu-id="09693-193">Customize the HTTP status code</span></span>

<span data-ttu-id="09693-194">Usare <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> per personalizzare il mapping dello stato di integrità dei codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="09693-194">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="09693-195">Le assegnazioni <xref:Microsoft.AspNetCore.Http.StatusCodes> seguenti sono i valori predefiniti usati dal middleware.</span><span class="sxs-lookup"><span data-stu-id="09693-195">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="09693-196">Modificare i valori dei codici di stato in base ai requisiti.</span><span class="sxs-lookup"><span data-stu-id="09693-196">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="09693-197">In `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="09693-197">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
});
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="09693-198">Eliminare le intestazioni della cache</span><span class="sxs-lookup"><span data-stu-id="09693-198">Suppress cache headers</span></span>

<span data-ttu-id="09693-199"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controlla se il middleware dei controlli di integrità aggiunge intestazioni HTTP a una risposta Probe per evitare la memorizzazione nella cache delle risposte.</span><span class="sxs-lookup"><span data-stu-id="09693-199"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="09693-200">Se il valore è `false` (impostazione predefinita), il middleware imposta le intestazioni `Cache-Control`, `Expires` e `Pragma` o ne esegue l'override per impedire la memorizzazione della risposta nella cache.</span><span class="sxs-lookup"><span data-stu-id="09693-200">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="09693-201">Se il valore è `true`, il middleware non modifica le intestazioni della risposta nella cache.</span><span class="sxs-lookup"><span data-stu-id="09693-201">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="09693-202">In `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="09693-202">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a><span data-ttu-id="09693-203">Personalizzare l'output</span><span class="sxs-lookup"><span data-stu-id="09693-203">Customize output</span></span>

<span data-ttu-id="09693-204">L'opzione <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> ottiene o imposta un delegato usato per scrivere la risposta.</span><span class="sxs-lookup"><span data-stu-id="09693-204">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span>

<span data-ttu-id="09693-205">In `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="09693-205">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

<span data-ttu-id="09693-206">Il delegato predefinito scrive una risposta in testo non crittografato minima con il valore di stringa [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="09693-206">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="09693-207">Il delegato personalizzato seguente, `WriteResponse`, restituisce una risposta JSON personalizzata:</span><span class="sxs-lookup"><span data-stu-id="09693-207">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

```csharp
private static Task WriteResponse(HttpContext httpContext, HealthReport result)
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

<span data-ttu-id="09693-208">Il sistema di controlli di integrità non fornisce il supporto incorporato per i formati restituiti JSON complessi, perché il formato è specifico del sistema di monitoraggio scelto.</span><span class="sxs-lookup"><span data-stu-id="09693-208">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="09693-209">È possibile personalizzare il `JObject` nell'esempio precedente, in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="09693-209">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="09693-210">Probe del database</span><span class="sxs-lookup"><span data-stu-id="09693-210">Database probe</span></span>

<span data-ttu-id="09693-211">Un controllo di integrità può specificare una query di database da eseguire come test booleano per indicare se il database sta rispondendo normalmente.</span><span class="sxs-lookup"><span data-stu-id="09693-211">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="09693-212">L'app di esempio usa [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), una libreria di controlli di integrità per app ASP.NET Core, per eseguire un controllo di integrità su un database di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="09693-212">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="09693-213">`AspNetCore.Diagnostics.HealthChecks` esegue una query `SELECT 1` sul database per confermare che la connessione al database sia integra.</span><span class="sxs-lookup"><span data-stu-id="09693-213">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="09693-214">Quando si controlla una connessione di database con una query, scegliere una query che viene restituita rapidamente.</span><span class="sxs-lookup"><span data-stu-id="09693-214">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="09693-215">Dall'approccio alla query possono derivare il sovraccarico del database e il degrado delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="09693-215">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="09693-216">Nella maggior parte dei casi, l'esecuzione di una query di test non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="09693-216">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="09693-217">È sufficiente stabilire una connessione al database.</span><span class="sxs-lookup"><span data-stu-id="09693-217">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="09693-218">Se invece è necessario eseguire una query, scegliere una semplice query SELECT, ad esempio `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="09693-218">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="09693-219">Includere un riferimento al pacchetto [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="09693-219">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="09693-220">Fornire una stringa di connessione di database valida nel file *appsettings.json* dell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="09693-220">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="09693-221">L'app usa una database di SQL Server denominato `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="09693-221">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="09693-222">Registrare i servizi dei controlli di integrità con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="09693-222">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="09693-223">L'app di esempio chiama il metodo `AddSqlServer` di con la stringa di connessione del database (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-223">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="09693-224">Un endpoint di controllo dell'integrità viene creato chiamando `MapHealthChecks` in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="09693-224">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="09693-225">Per eseguire lo scenario di probe del database di base usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="09693-225">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="09693-226">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) non è gestito né supportato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="09693-226">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="09693-227">Probe DbContext di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="09693-227">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="09693-228">Il controllo `DbContext` verifica che l'app possa comunicare con il database configurato per un `DbContext` di EF Core.</span><span class="sxs-lookup"><span data-stu-id="09693-228">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="09693-229">Il controllo `DbContext` è supportato nelle app che:</span><span class="sxs-lookup"><span data-stu-id="09693-229">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="09693-230">Usano [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="09693-230">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="09693-231">Includono un riferimento del pacchetto a [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="09693-231">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="09693-232">`AddDbContextCheck<TContext>` registra un controllo di integrità per un `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="09693-232">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="09693-233">`DbContext` viene reso disponibile come `TContext` al metodo.</span><span class="sxs-lookup"><span data-stu-id="09693-233">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="09693-234">È disponibile un overload per configurare lo stato di errore, i tag e una query di test personalizzata.</span><span class="sxs-lookup"><span data-stu-id="09693-234">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="09693-235">Per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="09693-235">By default:</span></span>

* <span data-ttu-id="09693-236">`DbContextHealthCheck` chiama il metodo `CanConnectAsync` di EF Core.</span><span class="sxs-lookup"><span data-stu-id="09693-236">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="09693-237">È possibile determinare l'operazione da eseguire quando si controlla l'integrità usando gli overload del metodo `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="09693-237">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="09693-238">Il nome del controllo di integrità è il nome del tipo `TContext`.</span><span class="sxs-lookup"><span data-stu-id="09693-238">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="09693-239">Nell'app di esempio, viene fornito `AppDbContext` per `AddDbContextCheck` e registrato come servizio in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-239">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="09693-240">Un endpoint di controllo dell'integrità viene creato chiamando `MapHealthChecks` in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="09693-240">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="09693-241">Per eseguire lo scenario di probe `DbContext` usando l'app di esempio, verificare che il database specificato dalla stringa di connessione non esista nell'istanza di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="09693-241">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="09693-242">Se il database esiste, eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="09693-242">If the database exists, delete it.</span></span>

<span data-ttu-id="09693-243">Eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="09693-243">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="09693-244">Dopo che l'app è in esecuzione, controllare lo stato di integrità effettuando una richiesta all'endpoint `/health` in un browser.</span><span class="sxs-lookup"><span data-stu-id="09693-244">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="09693-245">Il database e `AppDbContext` non esistono, quindi l'app fornisce la risposta seguente:</span><span class="sxs-lookup"><span data-stu-id="09693-245">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="09693-246">Attivare l'app di esempio per creare il database.</span><span class="sxs-lookup"><span data-stu-id="09693-246">Trigger the sample app to create the database.</span></span> <span data-ttu-id="09693-247">Effettuare una richiesta a `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="09693-247">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="09693-248">L'app risponde:</span><span class="sxs-lookup"><span data-stu-id="09693-248">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="09693-249">Effettuare una richiesta all'endpoint `/health`.</span><span class="sxs-lookup"><span data-stu-id="09693-249">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="09693-250">Il database e il contesto esistono, quindi l'app risponde:</span><span class="sxs-lookup"><span data-stu-id="09693-250">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="09693-251">Attivare l'app di esempio per eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="09693-251">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="09693-252">Effettuare una richiesta a `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="09693-252">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="09693-253">L'app risponde:</span><span class="sxs-lookup"><span data-stu-id="09693-253">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="09693-254">Effettuare una richiesta all'endpoint `/health`.</span><span class="sxs-lookup"><span data-stu-id="09693-254">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="09693-255">L'app fornisce una risposta non integra:</span><span class="sxs-lookup"><span data-stu-id="09693-255">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="09693-256">Separare i probe di idoneità e di attività</span><span class="sxs-lookup"><span data-stu-id="09693-256">Separate readiness and liveness probes</span></span>

<span data-ttu-id="09693-257">In alcuni scenari di hosting vengono usati due controlli di integrità che distinguono due stati dell'app:</span><span class="sxs-lookup"><span data-stu-id="09693-257">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="09693-258">L'app è funzionante, ma non ancora pronta a ricevere le richieste.</span><span class="sxs-lookup"><span data-stu-id="09693-258">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="09693-259">Questo stato è l'*idoneità* dell'app.</span><span class="sxs-lookup"><span data-stu-id="09693-259">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="09693-260">L'app è funzionante e risponde alle richieste.</span><span class="sxs-lookup"><span data-stu-id="09693-260">The app is functioning and responding to requests.</span></span> <span data-ttu-id="09693-261">Questo stato è l'*attività* dell'app.</span><span class="sxs-lookup"><span data-stu-id="09693-261">This state is the app's *liveness*.</span></span>

<span data-ttu-id="09693-262">Il controllo di idoneità in genere esegue un set di controlli più completo e dispendioso in termini di tempo per determinare se tutti i sottosistemi e le risorse dell'app sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="09693-262">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="09693-263">Un controllo di attività esegue semplicemente un rapido controllo per determinare se l'app è disponibile per elaborare le richieste.</span><span class="sxs-lookup"><span data-stu-id="09693-263">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="09693-264">Dopo che l'app ha superato il controllo di idoneità, non è necessario caricare ulteriormente l'app con il dispendioso set di controlli di idoneità. Sarà sufficiente controllare solo l'attività.</span><span class="sxs-lookup"><span data-stu-id="09693-264">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="09693-265">L'app di esempio contiene un controllo di integrità per segnalare il completamento dell'attività di avvio con esecuzione prolungata in un [servizio ospitato](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="09693-265">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="09693-266">`StartupHostedServiceHealthCheck` espone una proprietà, `StartupTaskCompleted`, che il servizio ospitato può impostare su `true` al termine dell'attività con esecuzione prolungata (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-266">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="09693-267">L'attività in background con esecuzione prolungata viene avviata da un [servizio ospitato](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="09693-267">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="09693-268">Al termine dell'attività, `StartupHostedServiceHealthCheck.StartupTaskCompleted` viene impostata su `true`:</span><span class="sxs-lookup"><span data-stu-id="09693-268">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="09693-269">Il controllo di integrità viene registrato con <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` insieme al servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="09693-269">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="09693-270">Poiché il servizio ospitato deve impostare la proprietà sul controllo di integrità, il controllo di integrità viene anche registrato nel contenitore del servizio (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-270">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="09693-271">Un endpoint di controllo dell'integrità viene creato chiamando `MapHealthChecks` in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="09693-271">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="09693-272">Nell'app di esempio gli endpoint di controllo integrità vengono creati in:</span><span class="sxs-lookup"><span data-stu-id="09693-272">In the sample app, the health check endpoints are created at:</span></span>

* <span data-ttu-id="09693-273">`/health/ready` per il controllo di conformità.</span><span class="sxs-lookup"><span data-stu-id="09693-273">`/health/ready` for the readiness check.</span></span> <span data-ttu-id="09693-274">Il controllo di idoneità filtra i controlli di integrità per il controllo di integrità con il tag `ready`.</span><span class="sxs-lookup"><span data-stu-id="09693-274">The readiness check filters health checks to the health check with the `ready` tag.</span></span>
* <span data-ttu-id="09693-275">`/health/live` per il controllo dell'anima.</span><span class="sxs-lookup"><span data-stu-id="09693-275">`/health/live` for the liveness check.</span></span> <span data-ttu-id="09693-276">Il controllo dell'integrità filtra il `StartupHostedServiceHealthCheck` restituendo `false` in [HealthCheckOptions. Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (per altre informazioni, vedere [filtrare i controlli di integrità](#filter-health-checks))</span><span class="sxs-lookup"><span data-stu-id="09693-276">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks))</span></span>

<span data-ttu-id="09693-277">Nell'esempio di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="09693-277">In the following example code:</span></span>

* <span data-ttu-id="09693-278">Il controllo di conformità utilizza tutti i controlli registrati con il tag "Ready".</span><span class="sxs-lookup"><span data-stu-id="09693-278">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="09693-279">Il `Predicate` esclude tutti i controlli e restituisce 200-OK.</span><span class="sxs-lookup"><span data-stu-id="09693-279">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health/ready", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("ready"),
    });

    endpoints.MapHealthChecks("/health/live", new HealthCheckOptions()
    {
        Predicate = (_) => false
    });
}
```

<span data-ttu-id="09693-280">Per eseguire lo scenario di configurazione di idoneità/attività usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="09693-280">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="09693-281">In a browser visitare più volte `/health/ready` per 15 secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-281">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="09693-282">Il controllo di integrità segnala *Unhealthy* per i primi 15 secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-282">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="09693-283">Dopo 15 secondi, l'endpoint segnala *Healthy*, che indica il completamento dell'attività con esecuzione prolungata da parte del servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="09693-283">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="09693-284">Questo esempio crea anche un server di pubblicazione dei controlli di integrità (implementazione di <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>) che esegue il primo controllo di conformità con un ritardo di due secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-284">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="09693-285">Per altre informazioni, vedere la sezione [Server di pubblicazione dei controlli di integrità](#health-check-publisher).</span><span class="sxs-lookup"><span data-stu-id="09693-285">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="09693-286">Esempio di Kubernetes</span><span class="sxs-lookup"><span data-stu-id="09693-286">Kubernetes example</span></span>

<span data-ttu-id="09693-287">L'uso di controlli di idoneità e attività separati è utile in un ambiente come [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="09693-287">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="09693-288">In Kubernetes un'app potrebbe dover eseguire un'operazione di avvio con esecuzione prolungata prima di accettare le richieste, ad esempio un test della disponibilità del database sottostante.</span><span class="sxs-lookup"><span data-stu-id="09693-288">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="09693-289">L'uso di controlli separati consente all'agente di orchestrazione di distinguere se l'app è funzionante, ma non è ancora pronta o se l'app non è stata avviata.</span><span class="sxs-lookup"><span data-stu-id="09693-289">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="09693-290">Per altre informazioni sui probe di idoneità e di attività in Kubernetes, vedere [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (Configurare i probe di attività e di idoneità in Kubernetes) nella documentazione di Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="09693-290">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="09693-291">L'esempio seguente illustra la configurazione di un probe di idoneità di Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="09693-291">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="09693-292">Probe basato sulle metriche con un writer di risposta personalizzata</span><span class="sxs-lookup"><span data-stu-id="09693-292">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="09693-293">L'app di esempio illustra un controllo di integrità della memoria con un writer di risposta personalizzata.</span><span class="sxs-lookup"><span data-stu-id="09693-293">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="09693-294">`MemoryHealthCheck` segnala uno stato danneggiato se l'app usa più di una determinata soglia di memoria (1 GB nell'app di esempio).</span><span class="sxs-lookup"><span data-stu-id="09693-294">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="09693-295"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> include informazioni sul Garbage Collector (GC) per l'app (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-295">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="09693-296">Registrare i servizi dei controlli di integrità con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="09693-296">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="09693-297">Invece di abilitare il controllo di integrità passandolo ad <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, `MemoryHealthCheck` viene registrato come servizio.</span><span class="sxs-lookup"><span data-stu-id="09693-297">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="09693-298">Tutti i servizi registrati da <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> sono disponibili per il middleware e i servizi di controllo dell'integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-298">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="09693-299">È consigliabile registrare i servizi di controllo dell'integrità come servizi Singleton.</span><span class="sxs-lookup"><span data-stu-id="09693-299">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="09693-300">Nell'app di esempio (*CustomWriterStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-300">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="09693-301">Un endpoint di controllo dell'integrità viene creato chiamando `MapHealthChecks` in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="09693-301">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="09693-302">Viene fornito un delegato `WriteResponse` alla proprietà `ResponseWriter` per visualizzare una risposta JSON personalizzata quando il controllo di integrità viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="09693-302">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="09693-303">Il metodo `WriteResponse` formatta `CompositeHealthCheckResult` in un oggetto JSON e genera l'output JSON per la risposta del controllo di integrità:</span><span class="sxs-lookup"><span data-stu-id="09693-303">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="09693-304">Per eseguire lo scenario di probe basato sulle metriche con il writer di risposta personalizzata usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="09693-304">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="09693-305">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) include scenari di controllo dell'integrità basati su metriche, tra cui i controlli dell'archiviazione su disco e dell'attività dei valori massimi.</span><span class="sxs-lookup"><span data-stu-id="09693-305">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="09693-306">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) non è gestito né supportato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="09693-306">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="09693-307">Filtro per porta</span><span class="sxs-lookup"><span data-stu-id="09693-307">Filter by port</span></span>

<span data-ttu-id="09693-308">Chiamare `RequireHost` su `MapHealthChecks` con un modello di URL che specifica una porta per limitare le richieste di controllo integrità alla porta specificata.</span><span class="sxs-lookup"><span data-stu-id="09693-308">Call `RequireHost` on `MapHealthChecks` with a URL pattern that specifies a port to restrict health check requests to the port specified.</span></span> <span data-ttu-id="09693-309">Questa chiamata viene in genere usata nell'ambiente di un contenitore per esporre una porta per il monitoraggio dei servizi.</span><span class="sxs-lookup"><span data-stu-id="09693-309">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="09693-310">L'app di esempio configura la porta usando il [provider di configurazione della variabile di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="09693-310">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="09693-311">La porta viene impostata nel file *launchSettings.json* e passata al provider di configurazione tramite una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="09693-311">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="09693-312">È anche necessario configurare il server per l'ascolto di richieste sulla porta di gestione.</span><span class="sxs-lookup"><span data-stu-id="09693-312">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="09693-313">Per usare l'app di esempio per illustrare la configurazione della porta di gestione, creare il file *launchSettings.json* in una cartella *Properties*.</span><span class="sxs-lookup"><span data-stu-id="09693-313">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="09693-314">Il file *Properties/launchSettings. JSON* seguente nell'app di esempio non è incluso nei file di progetto dell'app di esempio e deve essere creato manualmente:</span><span class="sxs-lookup"><span data-stu-id="09693-314">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="09693-315">Registrare i servizi dei controlli di integrità con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="09693-315">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="09693-316">Creare un endpoint di controllo di integrità chiamando `MapHealthChecks` in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="09693-316">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="09693-317">Nell'app di esempio una chiamata a `RequireHost` sull'endpoint in `Startup.Configure` specifica la porta di gestione dalla configurazione:</span><span class="sxs-lookup"><span data-stu-id="09693-317">In the sample app, a call to `RequireHost` on the endpoint in `Startup.Configure` specifies the management port from configuration:</span></span>

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

<span data-ttu-id="09693-318">Gli endpoint vengono creati nell'app di esempio in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="09693-318">Endpoints are created in the sample app in `Startup.Configure`.</span></span> <span data-ttu-id="09693-319">Nell'esempio di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="09693-319">In the following example code:</span></span>

* <span data-ttu-id="09693-320">Il controllo di conformità utilizza tutti i controlli registrati con il tag "Ready".</span><span class="sxs-lookup"><span data-stu-id="09693-320">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="09693-321">Il `Predicate` esclude tutti i controlli e restituisce 200-OK.</span><span class="sxs-lookup"><span data-stu-id="09693-321">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health/ready", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("ready"),
    });

    endpoints.MapHealthChecks("/health/live", new HealthCheckOptions()
    {
        Predicate = (_) => false
    });
}
```

> [!NOTE]
> <span data-ttu-id="09693-322">È possibile evitare di creare il file *launchSettings. JSON* nell'app di esempio impostando la porta di gestione in modo esplicito nel codice.</span><span class="sxs-lookup"><span data-stu-id="09693-322">You can avoid creating the *launchSettings.json* file in the sample app by setting the management port explicitly in code.</span></span> <span data-ttu-id="09693-323">In *Program.cs* in cui viene creata la <xref:Microsoft.Extensions.Hosting.HostBuilder> aggiungere una chiamata a <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> e fornire l'endpoint della porta di gestione dell'app.</span><span class="sxs-lookup"><span data-stu-id="09693-323">In *Program.cs* where the <xref:Microsoft.Extensions.Hosting.HostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> and provide the app's management port endpoint.</span></span> <span data-ttu-id="09693-324">In `Configure` di *ManagementPortStartup.cs*specificare la porta di gestione con `RequireHost`:</span><span class="sxs-lookup"><span data-stu-id="09693-324">In `Configure` of *ManagementPortStartup.cs*, specify the management port with `RequireHost`:</span></span>
>
> <span data-ttu-id="09693-325">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="09693-325">*Program.cs*:</span></span>
>
> ```csharp
> return new HostBuilder()
>     .ConfigureWebHostDefaults(webBuilder =>
>     {
>         webBuilder.UseKestrel()
>             .ConfigureKestrel(serverOptions =>
>             {
>                 serverOptions.ListenAnyIP(5001);
>             })
>             .UseStartup(startupType);
>     })
>     .Build();
> ```
>
> <span data-ttu-id="09693-326">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="09693-326">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

<span data-ttu-id="09693-327">Per eseguire lo scenario di configurazione della porta di gestione usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="09693-327">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="09693-328">Distribuire una libreria di controllo di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-328">Distribute a health check library</span></span>

<span data-ttu-id="09693-329">Per distribuire un controllo di integrità come libreria:</span><span class="sxs-lookup"><span data-stu-id="09693-329">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="09693-330">Scrivere un controllo di integrità che implementa l'interfaccia <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> come classe autonoma.</span><span class="sxs-lookup"><span data-stu-id="09693-330">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="09693-331">La classe può basarsi su [inserimento delle dipendenze](xref:fundamentals/dependency-injection), attivazione del tipo e [opzioni denominate](xref:fundamentals/configuration/options) per accedere ai dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="09693-331">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="09693-332">Nella logica dei controlli di integrità di `CheckHealthAsync`:</span><span class="sxs-lookup"><span data-stu-id="09693-332">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="09693-333">`data1` e `data2` vengono utilizzati nel metodo per eseguire la logica del controllo di integrità del probe.</span><span class="sxs-lookup"><span data-stu-id="09693-333">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="09693-334">`AccessViolationException` è gestito.</span><span class="sxs-lookup"><span data-stu-id="09693-334">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="09693-335">Quando si verifica un <xref:System.AccessViolationException>, viene restituito il <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> con la <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> per consentire agli utenti di configurare lo stato di errore dei controlli di integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-335">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="09693-336">Scrivere un metodo di estensione con i parametri che l'app chiama nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="09693-336">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="09693-337">Nell'esempio seguente si presuma la firma del metodo di controllo dell'integrità seguente:</span><span class="sxs-lookup"><span data-stu-id="09693-337">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="09693-338">La firma precedente indica che `ExampleHealthCheck` richiede dati aggiuntivi per elaborare la logica del probe di controllo dell'integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-338">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="09693-339">I dati vengono forniti al delegato usato per creare l'istanza del controllo di integrità quando il controllo di integrità viene registrato con un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="09693-339">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="09693-340">Nell'esempio seguente il chiamante specifica facoltativamente:</span><span class="sxs-lookup"><span data-stu-id="09693-340">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="09693-341">nome del controllo integrità (`name`).</span><span class="sxs-lookup"><span data-stu-id="09693-341">health check name (`name`).</span></span> <span data-ttu-id="09693-342">Se `null`, viene usato `example_health_check`.</span><span class="sxs-lookup"><span data-stu-id="09693-342">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="09693-343">punto dati stringa per il controllo di integrità (`data1`).</span><span class="sxs-lookup"><span data-stu-id="09693-343">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="09693-344">punto dati Integer per il controllo di integrità (`data2`).</span><span class="sxs-lookup"><span data-stu-id="09693-344">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="09693-345">Se `null`, viene usato `1`.</span><span class="sxs-lookup"><span data-stu-id="09693-345">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="09693-346">stato dell'errore (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="09693-346">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="09693-347">Il valore predefinito è `null`.</span><span class="sxs-lookup"><span data-stu-id="09693-347">The default is `null`.</span></span> <span data-ttu-id="09693-348">Se `null`, viene segnalato [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) per uno stato di errore.</span><span class="sxs-lookup"><span data-stu-id="09693-348">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="09693-349">tag (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="09693-349">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string DefaultName = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? DefaultName,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="09693-350">Server di pubblicazione dei controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-350">Health Check Publisher</span></span>

<span data-ttu-id="09693-351">Quando <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> viene aggiunto al contenitore del servizio, il sistema di controllo dell'integrità esegue periodicamente i controlli di integrità e chiama `PublishAsync` con il risultato.</span><span class="sxs-lookup"><span data-stu-id="09693-351">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="09693-352">Ciò è utile in uno scenario relativo a un sistema di monitoraggio dell'integrità basato su push, che prevede che ogni processo chiami periodicamente il sistema di monitoraggio per determinare l'integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-352">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="09693-353">L'interfaccia <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> ha un solo metodo:</span><span class="sxs-lookup"><span data-stu-id="09693-353">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="09693-354"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> consente di impostare:</span><span class="sxs-lookup"><span data-stu-id="09693-354"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="09693-355"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; Ritardo iniziale applicato dopo l'avvio dell'app prima dell'esecuzione delle istanze di <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="09693-355"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="09693-356">Il ritardo viene applicato una sola volta all'avvio e non si applica alle iterazioni successive.</span><span class="sxs-lookup"><span data-stu-id="09693-356">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="09693-357">Il valore predefinito è cinque secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-357">The default value is five seconds.</span></span>
* <span data-ttu-id="09693-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; Periodo di esecuzione di <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="09693-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="09693-359">Il valore predefinito è 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-359">The default value is 30 seconds.</span></span>
* <span data-ttu-id="09693-360"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; Se <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> è `null` (impostazione predefinita), il servizio del server di pubblicazione dei controlli di integrità esegue tutti i controlli di integrità registrati.</span><span class="sxs-lookup"><span data-stu-id="09693-360"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="09693-361">Per eseguire un subset dei controlli di integrità, specificare una funzione che filtra il set di controlli.</span><span class="sxs-lookup"><span data-stu-id="09693-361">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="09693-362">Il predicato viene valutato per ogni periodo.</span><span class="sxs-lookup"><span data-stu-id="09693-362">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="09693-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; Timeout per l'esecuzione dei controlli di integrità per tutte le istanze di <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="09693-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="09693-364">Usare <xref:System.Threading.Timeout.InfiniteTimeSpan> per l'esecuzione senza un timeout.</span><span class="sxs-lookup"><span data-stu-id="09693-364">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="09693-365">Il valore predefinito è 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-365">The default value is 30 seconds.</span></span>

<span data-ttu-id="09693-366">Nell'app di esempio, `ReadinessPublisher` è un'implementazione di <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="09693-366">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="09693-367">Lo stato del controllo di integrità viene registrato per ogni controllo a livello di log di:</span><span class="sxs-lookup"><span data-stu-id="09693-367">The health check status is logged for each check at a log level of:</span></span>

* <span data-ttu-id="09693-368">Informazioni (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) se lo stato dei controlli di integrità è <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span><span class="sxs-lookup"><span data-stu-id="09693-368">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="09693-369">Errore (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) se lo stato è <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> o <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span><span class="sxs-lookup"><span data-stu-id="09693-369">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="09693-370">Nell'esempio di `LivenessProbeStartup` dell'app di esempio, il controllo di conformità `StartupHostedService` ha un ritardo di avvio di due secondi e il controllo viene eseguito ogni 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-370">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="09693-371">Per attivare l'implementazione <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>, l'esempio registra `ReadinessPublisher` come servizio singleton nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="09693-371">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> <span data-ttu-id="09693-372">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) include i server di pubblicazione per diversi sistemi, tra cui [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="09693-372">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="09693-373">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) non è gestito né supportato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="09693-373">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="09693-374">Limitare i controlli integrità con MapWhen</span><span class="sxs-lookup"><span data-stu-id="09693-374">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="09693-375">Usare <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> per creare un ramo condizionale della pipeline delle richieste per gli endpoint di controllo integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-375">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="09693-376">Nell'esempio seguente `MapWhen` esegue il branching della pipeline di richiesta per attivare il middleware dei controlli di integrità se viene ricevuta una richiesta GET per l'endpoint `api/HealthCheck`:</span><span class="sxs-lookup"><span data-stu-id="09693-376">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseEndpoints(endpoints =>
{
    endpoints.MapRazorPages();
});
```

<span data-ttu-id="09693-377">Per ulteriori informazioni, vedere <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="09693-377">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="09693-378">ASP.NET Core offre middleware e librerie di controlli di integrità per segnalare l'integrità dei componenti dell'infrastruttura di app.</span><span class="sxs-lookup"><span data-stu-id="09693-378">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="09693-379">I controlli di integrità vengono esposti da un'app come endpoint HTTP.</span><span class="sxs-lookup"><span data-stu-id="09693-379">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="09693-380">È possibile configurare endpoint dei controlli di integrità per svariati scenari di monitoraggio in tempo reale:</span><span class="sxs-lookup"><span data-stu-id="09693-380">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="09693-381">I probe di integrità possono essere usati dagli agenti di orchestrazione e dai servizi di bilanciamento del carico per controllare lo stato di un'app.</span><span class="sxs-lookup"><span data-stu-id="09693-381">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="09693-382">Ad esempio, un agente di orchestrazione potrebbe rispondere a controllo di integrità non superato arrestando una distribuzione in sequenza o riavviando un contenitore.</span><span class="sxs-lookup"><span data-stu-id="09693-382">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="09693-383">Un servizio di bilanciamento del carico potrebbe reagire a un'app non integra trasferendo il traffico dall'istanza con errori a un'istanza integra.</span><span class="sxs-lookup"><span data-stu-id="09693-383">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="09693-384">È possibile monitorare lo stato di integrità di memoria, disco e altre risorse fisiche del server.</span><span class="sxs-lookup"><span data-stu-id="09693-384">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="09693-385">I controlli di integrità possono testare le dipendenze di un'app, ad esempio i database e gli endpoint di servizio esterni, per verificare la disponibilità e il normale funzionamento.</span><span class="sxs-lookup"><span data-stu-id="09693-385">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="09693-386">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="09693-386">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="09693-387">L'app di esempio include esempi degli scenari descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="09693-387">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="09693-388">Per eseguire l'app di esempio per un determinato scenario, usare il comando [dotnet run](/dotnet/core/tools/dotnet-run) dalla cartella del progetto in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="09693-388">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="09693-389">Per informazioni dettagliate su come usare l'app di esempio, vedere il file *README.md* dell'app di esempio e le descrizioni degli scenari in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="09693-389">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09693-390">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="09693-390">Prerequisites</span></span>

<span data-ttu-id="09693-391">I controlli di integrità vengono in genere usati con un servizio di monitoraggio esterno o un agente di orchestrazione per controllare lo stato di un'app.</span><span class="sxs-lookup"><span data-stu-id="09693-391">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="09693-392">Prima di aggiungere i controlli di integrità a un'app, decidere quale sistema di monitoraggio usare.</span><span class="sxs-lookup"><span data-stu-id="09693-392">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="09693-393">Il sistema di monitoraggio determina quali tipi di controlli di integrità creare e come configurare i relativi endpoint.</span><span class="sxs-lookup"><span data-stu-id="09693-393">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="09693-394">Fare riferimento al [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) oppure aggiungere un riferimento al pacchetto [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="09693-394">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="09693-395">L'app di esempio include il codice di avvio per illustrare i controlli di integrità per diversi scenari.</span><span class="sxs-lookup"><span data-stu-id="09693-395">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="09693-396">Lo scenario di [probe del database](#database-probe) controlla l'integrità di una connessione di database usando [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="09693-396">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="09693-397">Lo scenario [Probe DbContext](#entity-framework-core-dbcontext-probe) verifica un database usando un elemento `DbContext` di EF Core.</span><span class="sxs-lookup"><span data-stu-id="09693-397">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="09693-398">Per esplorare gli scenari relativi ai database, l'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="09693-398">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="09693-399">Crea un database e specifica la stringa di connessione nel file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="09693-399">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="09693-400">Dispone dei riferimenti al pacchetto seguenti nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="09693-400">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="09693-401">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="09693-401">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="09693-402">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="09693-402">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="09693-403">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) non è gestito né supportato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="09693-403">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="09693-404">Un altro scenario relativo ai controlli di integrità illustra come filtrare i controlli di integrità per una porta di gestione.</span><span class="sxs-lookup"><span data-stu-id="09693-404">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="09693-405">L'app di esempio richiede di creare un file *Properties/launchSettings.json* che include l'URL di gestione e la porta di gestione.</span><span class="sxs-lookup"><span data-stu-id="09693-405">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="09693-406">Per altre informazioni, vedere la sezione [Filtrare in base alla porta](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="09693-406">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="09693-407">Probe di integrità di base</span><span class="sxs-lookup"><span data-stu-id="09693-407">Basic health probe</span></span>

<span data-ttu-id="09693-408">Per molte app, una configurazione del probe di integrità di base, che segnala la disponibilità dell'app per elaborare le richieste (*attività*), è sufficiente per individuare lo stato dell'app.</span><span class="sxs-lookup"><span data-stu-id="09693-408">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="09693-409">La configurazione di base registra i servizi di controllo integrità e chiama il middleware dei controlli di integrità per rispondere a un endpoint URL con una risposta di integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-409">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="09693-410">Per impostazione predefinita, non vengono registrati controlli di integrità specifici per testare particolari dipendenze o sottosistemi.</span><span class="sxs-lookup"><span data-stu-id="09693-410">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="09693-411">L'app viene considerata integra se riesce a rispondere all'URL dell'endpoint di integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-411">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="09693-412">Il writer di risposta predefinito scrive lo stato (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) come risposta di testo non crittografato per il client, indicando uno stato [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) o [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).</span><span class="sxs-lookup"><span data-stu-id="09693-412">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="09693-413">Registrare i servizi dei controlli di integrità con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="09693-413">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="09693-414">Aggiungere un endpoint per il middleware dei controlli di integrità con <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> nella pipeline di elaborazione delle richieste di `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="09693-414">Add an endpoint for Health Checks Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="09693-415">Nell'app di esempio l'endpoint di controllo di integrità viene creato in `/health` (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-415">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

```csharp
public class BasicStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks();
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseHealthChecks("/health");
    }
}
```

<span data-ttu-id="09693-416">Per eseguire lo scenario di configurazione di base usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="09693-416">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="09693-417">Esempio Docker</span><span class="sxs-lookup"><span data-stu-id="09693-417">Docker example</span></span>

<span data-ttu-id="09693-418">[Docker](xref:host-and-deploy/docker/index) offre una direttiva `HEALTHCHECK` predefinita che può essere usata per controllare lo stato di un'app che usa la configurazione dei controlli di integrità di base:</span><span class="sxs-lookup"><span data-stu-id="09693-418">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="09693-419">Creare controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-419">Create health checks</span></span>

<span data-ttu-id="09693-420">I controlli di integrità vengono creati implementando l'interfaccia <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck>.</span><span class="sxs-lookup"><span data-stu-id="09693-420">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="09693-421">Il metodo <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> restituisce <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> che indica l'integrità come `Healthy`, `Degraded` o `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="09693-421">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="09693-422">Il risultato viene scritto come risposta di testo non crittografato con un codice di stato configurabile. La configurazione viene descritta nella sezione [Opzioni dei controlli di integrità](#health-check-options).</span><span class="sxs-lookup"><span data-stu-id="09693-422">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="09693-423"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> può anche restituire coppie chiave-valore facoltative.</span><span class="sxs-lookup"><span data-stu-id="09693-423"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="09693-424">Controllo di integrità di esempio</span><span class="sxs-lookup"><span data-stu-id="09693-424">Example health check</span></span>

<span data-ttu-id="09693-425">Nella classe `ExampleHealthCheck` seguente viene illustrato il layout di un controllo di integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-425">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="09693-426">La logica dei controlli di integrità viene inserita nel metodo `CheckHealthAsync`.</span><span class="sxs-lookup"><span data-stu-id="09693-426">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="09693-427">Nell'esempio seguente viene impostata una variabile fittizia, `healthCheckResultHealthy`, `true`.</span><span class="sxs-lookup"><span data-stu-id="09693-427">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="09693-428">Se il valore di `healthCheckResultHealthy` è impostato su `false`, viene restituito lo stato [HealthCheckResult. unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) .</span><span class="sxs-lookup"><span data-stu-id="09693-428">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
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

### <a name="register-health-check-services"></a><span data-ttu-id="09693-429">Registrare i servizi di controllo dell'integrità</span><span class="sxs-lookup"><span data-stu-id="09693-429">Register health check services</span></span>

<span data-ttu-id="09693-430">Il tipo di `ExampleHealthCheck` viene aggiunto ai servizi di controllo integrità in `Startup.ConfigureServices` con <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span><span class="sxs-lookup"><span data-stu-id="09693-430">The `ExampleHealthCheck` type is added to health check services in `Startup.ConfigureServices` with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="09693-431">L'overload <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> illustrato nell'esempio seguente imposta lo stato di errore (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) per segnalare quando il controllo di integrità indica un errore.</span><span class="sxs-lookup"><span data-stu-id="09693-431">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="09693-432">Se lo stato di errore è impostato su `null` (impostazione predefinita), viene segnalato [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).</span><span class="sxs-lookup"><span data-stu-id="09693-432">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="09693-433">Questo overload è uno scenario utile per gli autori di librerie, in cui lo stato di errore indicato dalla libreria viene applicato dall'app quando si verifica un errore di controllo dell'integrità se l'implementazione del controllo dell'integrità rispetta l'impostazione.</span><span class="sxs-lookup"><span data-stu-id="09693-433">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="09693-434">Si possono usare *tag* per filtrare i controlli di integrità, come illustrato più avanti nella sezione [Filtrare i controlli di integrità](#filter-health-checks).</span><span class="sxs-lookup"><span data-stu-id="09693-434">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="09693-435"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> può anche eseguire una funzione lambda.</span><span class="sxs-lookup"><span data-stu-id="09693-435"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="09693-436">Nell'esempio di `Startup.ConfigureServices` seguente il nome del controllo integrità viene specificato come `Example` e il controllo restituisce sempre uno stato integro:</span><span class="sxs-lookup"><span data-stu-id="09693-436">In the following `Startup.ConfigureServices` example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="09693-437">Usare il middleware per i controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-437">Use Health Checks Middleware</span></span>

<span data-ttu-id="09693-438">In `Startup.Configure` chiamare <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> nella pipeline di elaborazione con l'URL dell'endpoint o il percorso relativo:</span><span class="sxs-lookup"><span data-stu-id="09693-438">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="09693-439">Se i controlli di integrità devono essere in ascolto su una porta specifica, usare un overload di <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> per impostare la porta, come illustrato più avanti nella sezione [Filtrare in base alla porta](#filter-by-port):</span><span class="sxs-lookup"><span data-stu-id="09693-439">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a><span data-ttu-id="09693-440">Opzioni dei controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-440">Health check options</span></span>

<span data-ttu-id="09693-441"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> consente di personalizzare il comportamento dei controlli di integrità:</span><span class="sxs-lookup"><span data-stu-id="09693-441"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="09693-442">Filtrare i controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-442">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="09693-443">Personalizzare il codice di stato HTTP</span><span class="sxs-lookup"><span data-stu-id="09693-443">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="09693-444">Eliminare le intestazioni della cache</span><span class="sxs-lookup"><span data-stu-id="09693-444">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="09693-445">Personalizzare l'output</span><span class="sxs-lookup"><span data-stu-id="09693-445">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="09693-446">Filtrare i controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-446">Filter health checks</span></span>

<span data-ttu-id="09693-447">Per impostazione predefinita, il middleware controlli integrità esegue tutti i controlli di integrità registrati.</span><span class="sxs-lookup"><span data-stu-id="09693-447">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="09693-448">Per eseguire un subset dei controlli di integrità, specificare una funzione che restituisce un valore booleano all'opzione <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate>.</span><span class="sxs-lookup"><span data-stu-id="09693-448">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="09693-449">Nell'esempio seguente il controllo di integrità `Bar` viene filtrato in base al tag (`bar_tag`) nell'istruzione condizionale della funzione, dove `true` viene restituito solo se la proprietà <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> del controllo di integrità corrisponde a `foo_tag` o a `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="09693-449">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), 
                tags: new[] { "bar_tag" })
        .AddCheck("Baz", () =>
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="09693-450">Personalizzare il codice di stato HTTP</span><span class="sxs-lookup"><span data-stu-id="09693-450">Customize the HTTP status code</span></span>

<span data-ttu-id="09693-451">Usare <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> per personalizzare il mapping dello stato di integrità dei codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="09693-451">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="09693-452">Le assegnazioni <xref:Microsoft.AspNetCore.Http.StatusCodes> seguenti sono i valori predefiniti usati dal middleware.</span><span class="sxs-lookup"><span data-stu-id="09693-452">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="09693-453">Modificare i valori dei codici di stato in base ai requisiti.</span><span class="sxs-lookup"><span data-stu-id="09693-453">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="09693-454">In `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="09693-454">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResultStatusCodes =
    {
        [HealthStatus.Healthy] = StatusCodes.Status200OK,
        [HealthStatus.Degraded] = StatusCodes.Status200OK,
        [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
    }
});
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="09693-455">Eliminare le intestazioni della cache</span><span class="sxs-lookup"><span data-stu-id="09693-455">Suppress cache headers</span></span>

<span data-ttu-id="09693-456"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controlla se il middleware dei controlli di integrità aggiunge intestazioni HTTP a una risposta Probe per evitare la memorizzazione nella cache delle risposte.</span><span class="sxs-lookup"><span data-stu-id="09693-456"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="09693-457">Se il valore è `false` (impostazione predefinita), il middleware imposta le intestazioni `Cache-Control`, `Expires` e `Pragma` o ne esegue l'override per impedire la memorizzazione della risposta nella cache.</span><span class="sxs-lookup"><span data-stu-id="09693-457">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="09693-458">Se il valore è `true`, il middleware non modifica le intestazioni della risposta nella cache.</span><span class="sxs-lookup"><span data-stu-id="09693-458">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="09693-459">In `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="09693-459">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a><span data-ttu-id="09693-460">Personalizzare l'output</span><span class="sxs-lookup"><span data-stu-id="09693-460">Customize output</span></span>

<span data-ttu-id="09693-461">L'opzione <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> ottiene o imposta un delegato usato per scrivere la risposta.</span><span class="sxs-lookup"><span data-stu-id="09693-461">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="09693-462">Il delegato predefinito scrive una risposta in testo non crittografato minima con il valore di stringa [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="09693-462">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

<span data-ttu-id="09693-463">In `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="09693-463">In `Startup.Configure`:</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

<span data-ttu-id="09693-464">Il delegato predefinito scrive una risposta in testo non crittografato minima con il valore di stringa [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="09693-464">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="09693-465">Il delegato personalizzato seguente, `WriteResponse`, restituisce una risposta JSON personalizzata:</span><span class="sxs-lookup"><span data-stu-id="09693-465">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

```csharp
private static Task WriteResponse(HttpContext httpContext, HealthReport result)
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

<span data-ttu-id="09693-466">Il sistema di controlli di integrità non fornisce il supporto incorporato per i formati restituiti JSON complessi, perché il formato è specifico del sistema di monitoraggio scelto.</span><span class="sxs-lookup"><span data-stu-id="09693-466">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="09693-467">È possibile personalizzare il `JObject` nell'esempio precedente, in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="09693-467">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="09693-468">Probe del database</span><span class="sxs-lookup"><span data-stu-id="09693-468">Database probe</span></span>

<span data-ttu-id="09693-469">Un controllo di integrità può specificare una query di database da eseguire come test booleano per indicare se il database sta rispondendo normalmente.</span><span class="sxs-lookup"><span data-stu-id="09693-469">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="09693-470">L'app di esempio usa [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), una libreria di controlli di integrità per app ASP.NET Core, per eseguire un controllo di integrità su un database di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="09693-470">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="09693-471">`AspNetCore.Diagnostics.HealthChecks` esegue una query `SELECT 1` sul database per confermare che la connessione al database sia integra.</span><span class="sxs-lookup"><span data-stu-id="09693-471">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="09693-472">Quando si controlla una connessione di database con una query, scegliere una query che viene restituita rapidamente.</span><span class="sxs-lookup"><span data-stu-id="09693-472">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="09693-473">Dall'approccio alla query possono derivare il sovraccarico del database e il degrado delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="09693-473">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="09693-474">Nella maggior parte dei casi, l'esecuzione di una query di test non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="09693-474">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="09693-475">È sufficiente stabilire una connessione al database.</span><span class="sxs-lookup"><span data-stu-id="09693-475">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="09693-476">Se invece è necessario eseguire una query, scegliere una semplice query SELECT, ad esempio `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="09693-476">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="09693-477">Includere un riferimento al pacchetto [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="09693-477">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="09693-478">Fornire una stringa di connessione di database valida nel file *appsettings.json* dell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="09693-478">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="09693-479">L'app usa una database di SQL Server denominato `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="09693-479">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="09693-480">Registrare i servizi dei controlli di integrità con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="09693-480">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="09693-481">L'app di esempio chiama il metodo `AddSqlServer` di con la stringa di connessione del database (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-481">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="09693-482">Chiamare il middleware verifica integrità nella pipeline di elaborazione delle app in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="09693-482">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="09693-483">Per eseguire lo scenario di probe del database di base usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="09693-483">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="09693-484">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) non è gestito né supportato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="09693-484">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="09693-485">Probe DbContext di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="09693-485">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="09693-486">Il controllo `DbContext` verifica che l'app possa comunicare con il database configurato per un `DbContext` di EF Core.</span><span class="sxs-lookup"><span data-stu-id="09693-486">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="09693-487">Il controllo `DbContext` è supportato nelle app che:</span><span class="sxs-lookup"><span data-stu-id="09693-487">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="09693-488">Usano [Entity Framework (EF) Core](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="09693-488">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="09693-489">Includono un riferimento del pacchetto a [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="09693-489">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="09693-490">`AddDbContextCheck<TContext>` registra un controllo di integrità per un `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="09693-490">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="09693-491">`DbContext` viene reso disponibile come `TContext` al metodo.</span><span class="sxs-lookup"><span data-stu-id="09693-491">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="09693-492">È disponibile un overload per configurare lo stato di errore, i tag e una query di test personalizzata.</span><span class="sxs-lookup"><span data-stu-id="09693-492">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="09693-493">Per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="09693-493">By default:</span></span>

* <span data-ttu-id="09693-494">`DbContextHealthCheck` chiama il metodo `CanConnectAsync` di EF Core.</span><span class="sxs-lookup"><span data-stu-id="09693-494">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="09693-495">È possibile determinare l'operazione da eseguire quando si controlla l'integrità usando gli overload del metodo `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="09693-495">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="09693-496">Il nome del controllo di integrità è il nome del tipo `TContext`.</span><span class="sxs-lookup"><span data-stu-id="09693-496">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="09693-497">Nell'app di esempio, viene fornito `AppDbContext` per `AddDbContextCheck` e registrato come servizio in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-497">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="09693-498">Nell'app di esempio `UseHealthChecks` aggiunge il middleware dei controlli di integrità in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="09693-498">In the sample app, `UseHealthChecks` adds the Health Checks Middleware in `Startup.Configure`.</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="09693-499">Per eseguire lo scenario di probe `DbContext` usando l'app di esempio, verificare che il database specificato dalla stringa di connessione non esista nell'istanza di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="09693-499">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="09693-500">Se il database esiste, eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="09693-500">If the database exists, delete it.</span></span>

<span data-ttu-id="09693-501">Eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="09693-501">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="09693-502">Dopo che l'app è in esecuzione, controllare lo stato di integrità effettuando una richiesta all'endpoint `/health` in un browser.</span><span class="sxs-lookup"><span data-stu-id="09693-502">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="09693-503">Il database e `AppDbContext` non esistono, quindi l'app fornisce la risposta seguente:</span><span class="sxs-lookup"><span data-stu-id="09693-503">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="09693-504">Attivare l'app di esempio per creare il database.</span><span class="sxs-lookup"><span data-stu-id="09693-504">Trigger the sample app to create the database.</span></span> <span data-ttu-id="09693-505">Effettuare una richiesta a `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="09693-505">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="09693-506">L'app risponde:</span><span class="sxs-lookup"><span data-stu-id="09693-506">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="09693-507">Effettuare una richiesta all'endpoint `/health`.</span><span class="sxs-lookup"><span data-stu-id="09693-507">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="09693-508">Il database e il contesto esistono, quindi l'app risponde:</span><span class="sxs-lookup"><span data-stu-id="09693-508">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="09693-509">Attivare l'app di esempio per eliminare il database.</span><span class="sxs-lookup"><span data-stu-id="09693-509">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="09693-510">Effettuare una richiesta a `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="09693-510">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="09693-511">L'app risponde:</span><span class="sxs-lookup"><span data-stu-id="09693-511">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="09693-512">Effettuare una richiesta all'endpoint `/health`.</span><span class="sxs-lookup"><span data-stu-id="09693-512">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="09693-513">L'app fornisce una risposta non integra:</span><span class="sxs-lookup"><span data-stu-id="09693-513">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="09693-514">Separare i probe di idoneità e di attività</span><span class="sxs-lookup"><span data-stu-id="09693-514">Separate readiness and liveness probes</span></span>

<span data-ttu-id="09693-515">In alcuni scenari di hosting vengono usati due controlli di integrità che distinguono due stati dell'app:</span><span class="sxs-lookup"><span data-stu-id="09693-515">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="09693-516">L'app è funzionante, ma non ancora pronta a ricevere le richieste.</span><span class="sxs-lookup"><span data-stu-id="09693-516">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="09693-517">Questo stato è l'*idoneità* dell'app.</span><span class="sxs-lookup"><span data-stu-id="09693-517">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="09693-518">L'app è funzionante e risponde alle richieste.</span><span class="sxs-lookup"><span data-stu-id="09693-518">The app is functioning and responding to requests.</span></span> <span data-ttu-id="09693-519">Questo stato è l'*attività* dell'app.</span><span class="sxs-lookup"><span data-stu-id="09693-519">This state is the app's *liveness*.</span></span>

<span data-ttu-id="09693-520">Il controllo di idoneità in genere esegue un set di controlli più completo e dispendioso in termini di tempo per determinare se tutti i sottosistemi e le risorse dell'app sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="09693-520">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="09693-521">Un controllo di attività esegue semplicemente un rapido controllo per determinare se l'app è disponibile per elaborare le richieste.</span><span class="sxs-lookup"><span data-stu-id="09693-521">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="09693-522">Dopo che l'app ha superato il controllo di idoneità, non è necessario caricare ulteriormente l'app con il dispendioso set di controlli di idoneità. Sarà sufficiente controllare solo l'attività.</span><span class="sxs-lookup"><span data-stu-id="09693-522">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="09693-523">L'app di esempio contiene un controllo di integrità per segnalare il completamento dell'attività di avvio con esecuzione prolungata in un [servizio ospitato](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="09693-523">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="09693-524">`StartupHostedServiceHealthCheck` espone una proprietà, `StartupTaskCompleted`, che il servizio ospitato può impostare su `true` al termine dell'attività con esecuzione prolungata (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-524">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="09693-525">L'attività in background con esecuzione prolungata viene avviata da un [servizio ospitato](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="09693-525">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="09693-526">Al termine dell'attività, `StartupHostedServiceHealthCheck.StartupTaskCompleted` viene impostata su `true`:</span><span class="sxs-lookup"><span data-stu-id="09693-526">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="09693-527">Il controllo di integrità viene registrato con <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` insieme al servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="09693-527">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="09693-528">Poiché il servizio ospitato deve impostare la proprietà sul controllo di integrità, il controllo di integrità viene anche registrato nel contenitore del servizio (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-528">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="09693-529">Il middleware controlla l'integrità della chiamata nella pipeline di elaborazione delle app `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="09693-529">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="09693-530">Nell'app di esempio gli endpoint dei controlli di integrità vengono creati in `/health/ready` per il controllo di idoneità e in `/health/live` per il controllo di attività.</span><span class="sxs-lookup"><span data-stu-id="09693-530">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="09693-531">Il controllo di idoneità filtra i controlli di integrità per il controllo di integrità con il tag `ready`.</span><span class="sxs-lookup"><span data-stu-id="09693-531">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="09693-532">Il controllo di attività filtra `StartupHostedServiceHealthCheck` restituendo `false` in [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (per altre informazioni, vedere [Filtrare i controlli di integrità](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="09693-532">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

```csharp
app.UseHealthChecks("/health/ready", new HealthCheckOptions()
{
    Predicate = (check) => check.Tags.Contains("ready"), 
});

app.UseHealthChecks("/health/live", new HealthCheckOptions()
{
    Predicate = (_) => false
});
```

<span data-ttu-id="09693-533">Per eseguire lo scenario di configurazione di idoneità/attività usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="09693-533">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="09693-534">In a browser visitare più volte `/health/ready` per 15 secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-534">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="09693-535">Il controllo di integrità segnala *Unhealthy* per i primi 15 secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-535">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="09693-536">Dopo 15 secondi, l'endpoint segnala *Healthy*, che indica il completamento dell'attività con esecuzione prolungata da parte del servizio ospitato.</span><span class="sxs-lookup"><span data-stu-id="09693-536">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="09693-537">Questo esempio crea anche un server di pubblicazione dei controlli di integrità (implementazione di <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>) che esegue il primo controllo di conformità con un ritardo di due secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-537">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="09693-538">Per altre informazioni, vedere la sezione [Server di pubblicazione dei controlli di integrità](#health-check-publisher).</span><span class="sxs-lookup"><span data-stu-id="09693-538">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="09693-539">Esempio di Kubernetes</span><span class="sxs-lookup"><span data-stu-id="09693-539">Kubernetes example</span></span>

<span data-ttu-id="09693-540">L'uso di controlli di idoneità e attività separati è utile in un ambiente come [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="09693-540">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="09693-541">In Kubernetes un'app potrebbe dover eseguire un'operazione di avvio con esecuzione prolungata prima di accettare le richieste, ad esempio un test della disponibilità del database sottostante.</span><span class="sxs-lookup"><span data-stu-id="09693-541">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="09693-542">L'uso di controlli separati consente all'agente di orchestrazione di distinguere se l'app è funzionante, ma non è ancora pronta o se l'app non è stata avviata.</span><span class="sxs-lookup"><span data-stu-id="09693-542">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="09693-543">Per altre informazioni sui probe di idoneità e di attività in Kubernetes, vedere [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (Configurare i probe di attività e di idoneità in Kubernetes) nella documentazione di Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="09693-543">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="09693-544">L'esempio seguente illustra la configurazione di un probe di idoneità di Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="09693-544">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="09693-545">Probe basato sulle metriche con un writer di risposta personalizzata</span><span class="sxs-lookup"><span data-stu-id="09693-545">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="09693-546">L'app di esempio illustra un controllo di integrità della memoria con un writer di risposta personalizzata.</span><span class="sxs-lookup"><span data-stu-id="09693-546">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="09693-547">`MemoryHealthCheck` segnala uno stato di tipo non integro se l'applicazione usa più di una determinata soglia di memoria (1 GB nell'app di esempio).</span><span class="sxs-lookup"><span data-stu-id="09693-547">`MemoryHealthCheck` reports an unhealthy status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="09693-548"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> include informazioni sul Garbage Collector (GC) per l'app (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-548">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="09693-549">Registrare i servizi dei controlli di integrità con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="09693-549">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="09693-550">Invece di abilitare il controllo di integrità passandolo ad <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, `MemoryHealthCheck` viene registrato come servizio.</span><span class="sxs-lookup"><span data-stu-id="09693-550">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="09693-551">Tutti i servizi registrati da <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> sono disponibili per il middleware e i servizi di controllo dell'integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-551">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="09693-552">È consigliabile registrare i servizi di controllo dell'integrità come servizi Singleton.</span><span class="sxs-lookup"><span data-stu-id="09693-552">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="09693-553">Nell'app di esempio (*CustomWriterStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-553">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="09693-554">Il middleware controlla l'integrità della chiamata nella pipeline di elaborazione delle app `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="09693-554">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="09693-555">Viene fornito un delegato `WriteResponse` alla proprietà `ResponseWriter` per visualizzare una risposta JSON personalizzata quando il controllo di integrità viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="09693-555">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // This custom writer formats the detailed status as JSON.
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="09693-556">Il metodo `WriteResponse` formatta `CompositeHealthCheckResult` in un oggetto JSON e genera l'output JSON per la risposta del controllo di integrità:</span><span class="sxs-lookup"><span data-stu-id="09693-556">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="09693-557">Per eseguire lo scenario di probe basato sulle metriche con il writer di risposta personalizzata usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="09693-557">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="09693-558">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) include scenari di controllo dell'integrità basati su metriche, tra cui i controlli dell'archiviazione su disco e dell'attività dei valori massimi.</span><span class="sxs-lookup"><span data-stu-id="09693-558">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="09693-559">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) non è gestito né supportato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="09693-559">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="09693-560">Filtro per porta</span><span class="sxs-lookup"><span data-stu-id="09693-560">Filter by port</span></span>

<span data-ttu-id="09693-561">La chiamata a <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> con una porta limita le richieste di controllo dell'integrità alla porta specificata.</span><span class="sxs-lookup"><span data-stu-id="09693-561">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="09693-562">Questa chiamata viene in genere usata nell'ambiente di un contenitore per esporre una porta per il monitoraggio dei servizi.</span><span class="sxs-lookup"><span data-stu-id="09693-562">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="09693-563">L'app di esempio configura la porta usando il [provider di configurazione della variabile di ambiente](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="09693-563">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="09693-564">La porta viene impostata nel file *launchSettings.json* e passata al provider di configurazione tramite una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="09693-564">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="09693-565">È anche necessario configurare il server per l'ascolto di richieste sulla porta di gestione.</span><span class="sxs-lookup"><span data-stu-id="09693-565">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="09693-566">Per usare l'app di esempio per illustrare la configurazione della porta di gestione, creare il file *launchSettings.json* in una cartella *Properties*.</span><span class="sxs-lookup"><span data-stu-id="09693-566">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="09693-567">Il file *Properties/launchSettings. JSON* seguente nell'app di esempio non è incluso nei file di progetto dell'app di esempio e deve essere creato manualmente:</span><span class="sxs-lookup"><span data-stu-id="09693-567">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="09693-568">Registrare i servizi dei controlli di integrità con <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="09693-568">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="09693-569">La chiamata a <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifica la porta di gestione (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="09693-569">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> <span data-ttu-id="09693-570">È possibile evitare di creare il file *launchSettings.json* nell'app di esempio impostando gli URL e la porta di gestione in modo esplicito nel codice.</span><span class="sxs-lookup"><span data-stu-id="09693-570">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="09693-571">In *Program.cs*, dove viene creato <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, aggiungere una chiamata a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> e fornire l'endpoint della porta di gestione e l'endpoint di risposta normale dell'app.</span><span class="sxs-lookup"><span data-stu-id="09693-571">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="09693-572">In *ManagementPortStartup.cs*, dove viene chiamato <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>, specificare la porta di gestione in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="09693-572">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="09693-573">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="09693-573">*Program.cs*:</span></span>
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
> <span data-ttu-id="09693-574">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="09693-574">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="09693-575">Per eseguire lo scenario di configurazione della porta di gestione usando l'app di esempio, eseguire il comando seguente dalla cartella del progetto in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="09693-575">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="09693-576">Distribuire una libreria di controllo di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-576">Distribute a health check library</span></span>

<span data-ttu-id="09693-577">Per distribuire un controllo di integrità come libreria:</span><span class="sxs-lookup"><span data-stu-id="09693-577">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="09693-578">Scrivere un controllo di integrità che implementa l'interfaccia <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> come classe autonoma.</span><span class="sxs-lookup"><span data-stu-id="09693-578">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="09693-579">La classe può basarsi su [inserimento delle dipendenze](xref:fundamentals/dependency-injection), attivazione del tipo e [opzioni denominate](xref:fundamentals/configuration/options) per accedere ai dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="09693-579">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="09693-580">Nella logica dei controlli di integrità di `CheckHealthAsync`:</span><span class="sxs-lookup"><span data-stu-id="09693-580">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="09693-581">`data1` e `data2` vengono utilizzati nel metodo per eseguire la logica del controllo di integrità del probe.</span><span class="sxs-lookup"><span data-stu-id="09693-581">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="09693-582">`AccessViolationException` è gestito.</span><span class="sxs-lookup"><span data-stu-id="09693-582">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="09693-583">Quando si verifica un <xref:System.AccessViolationException>, viene restituito il <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> con la <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> per consentire agli utenti di configurare lo stato di errore dei controlli di integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-583">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

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
   ```

1. <span data-ttu-id="09693-584">Scrivere un metodo di estensione con i parametri che l'app chiama nel metodo `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="09693-584">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="09693-585">Nell'esempio seguente si presuma la firma del metodo di controllo dell'integrità seguente:</span><span class="sxs-lookup"><span data-stu-id="09693-585">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="09693-586">La firma precedente indica che `ExampleHealthCheck` richiede dati aggiuntivi per elaborare la logica del probe di controllo dell'integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-586">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="09693-587">I dati vengono forniti al delegato usato per creare l'istanza del controllo di integrità quando il controllo di integrità viene registrato con un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="09693-587">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="09693-588">Nell'esempio seguente il chiamante specifica facoltativamente:</span><span class="sxs-lookup"><span data-stu-id="09693-588">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="09693-589">nome del controllo integrità (`name`).</span><span class="sxs-lookup"><span data-stu-id="09693-589">health check name (`name`).</span></span> <span data-ttu-id="09693-590">Se `null`, viene usato `example_health_check`.</span><span class="sxs-lookup"><span data-stu-id="09693-590">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="09693-591">punto dati stringa per il controllo di integrità (`data1`).</span><span class="sxs-lookup"><span data-stu-id="09693-591">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="09693-592">punto dati Integer per il controllo di integrità (`data2`).</span><span class="sxs-lookup"><span data-stu-id="09693-592">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="09693-593">Se `null`, viene usato `1`.</span><span class="sxs-lookup"><span data-stu-id="09693-593">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="09693-594">stato dell'errore (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="09693-594">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="09693-595">Il valore predefinito è `null`.</span><span class="sxs-lookup"><span data-stu-id="09693-595">The default is `null`.</span></span> <span data-ttu-id="09693-596">Se `null`, viene segnalato [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) per uno stato di errore.</span><span class="sxs-lookup"><span data-stu-id="09693-596">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="09693-597">tag (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="09693-597">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string DefaultName = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? DefaultName,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="09693-598">Server di pubblicazione dei controlli di integrità</span><span class="sxs-lookup"><span data-stu-id="09693-598">Health Check Publisher</span></span>

<span data-ttu-id="09693-599">Quando <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> viene aggiunto al contenitore del servizio, il sistema di controllo dell'integrità esegue periodicamente i controlli di integrità e chiama `PublishAsync` con il risultato.</span><span class="sxs-lookup"><span data-stu-id="09693-599">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="09693-600">Ciò è utile in uno scenario relativo a un sistema di monitoraggio dell'integrità basato su push, che prevede che ogni processo chiami periodicamente il sistema di monitoraggio per determinare l'integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-600">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="09693-601">L'interfaccia <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> ha un solo metodo:</span><span class="sxs-lookup"><span data-stu-id="09693-601">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="09693-602"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> consente di impostare:</span><span class="sxs-lookup"><span data-stu-id="09693-602"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="09693-603"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; Ritardo iniziale applicato dopo l'avvio dell'app prima dell'esecuzione delle istanze di <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="09693-603"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="09693-604">Il ritardo viene applicato una sola volta all'avvio e non si applica alle iterazioni successive.</span><span class="sxs-lookup"><span data-stu-id="09693-604">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="09693-605">Il valore predefinito è cinque secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-605">The default value is five seconds.</span></span>
* <span data-ttu-id="09693-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; Periodo di esecuzione di <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="09693-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="09693-607">Il valore predefinito è 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-607">The default value is 30 seconds.</span></span>
* <span data-ttu-id="09693-608"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; Se <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> è `null` (impostazione predefinita), il servizio del server di pubblicazione dei controlli di integrità esegue tutti i controlli di integrità registrati.</span><span class="sxs-lookup"><span data-stu-id="09693-608"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="09693-609">Per eseguire un subset dei controlli di integrità, specificare una funzione che filtra il set di controlli.</span><span class="sxs-lookup"><span data-stu-id="09693-609">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="09693-610">Il predicato viene valutato per ogni periodo.</span><span class="sxs-lookup"><span data-stu-id="09693-610">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="09693-611"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; Timeout per l'esecuzione dei controlli di integrità per tutte le istanze di <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="09693-611"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="09693-612">Usare <xref:System.Threading.Timeout.InfiniteTimeSpan> per l'esecuzione senza un timeout.</span><span class="sxs-lookup"><span data-stu-id="09693-612">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="09693-613">Il valore predefinito è 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-613">The default value is 30 seconds.</span></span>

> [!WARNING]
> <span data-ttu-id="09693-614">Nella versione 2.2 di ASP.NET Core, l'impostazione <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> non viene rispettata dall'implementazione <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>. Viene impostato il valore <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span><span class="sxs-lookup"><span data-stu-id="09693-614">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="09693-615">Questo problema è stato risolto in ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="09693-615">This issue has been addressed in ASP.NET Core 3.0.</span></span>

<span data-ttu-id="09693-616">Nell'app di esempio, `ReadinessPublisher` è un'implementazione di <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="09693-616">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="09693-617">Lo stato di controllo integrità viene registrato per ogni controllo come:</span><span class="sxs-lookup"><span data-stu-id="09693-617">The health check status is logged for each check as either:</span></span>

* <span data-ttu-id="09693-618">Informazioni (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) se lo stato dei controlli di integrità è <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span><span class="sxs-lookup"><span data-stu-id="09693-618">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="09693-619">Errore (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) se lo stato è <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> o <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span><span class="sxs-lookup"><span data-stu-id="09693-619">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="09693-620">Nell'esempio di `LivenessProbeStartup` dell'app di esempio, il controllo di conformità `StartupHostedService` ha un ritardo di avvio di due secondi e il controllo viene eseguito ogni 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="09693-620">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="09693-621">Per attivare l'implementazione <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>, l'esempio registra `ReadinessPublisher` come servizio singleton nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="09693-621">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> <span data-ttu-id="09693-622">La soluzione alternativa seguente consente di aggiungere un'istanza <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> al contenitore del servizio quando uno o più servizi ospitati sono già stati aggiunti all'app.</span><span class="sxs-lookup"><span data-stu-id="09693-622">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="09693-623">Questa soluzione alternativa non è necessaria in ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="09693-623">This workaround won't isn't required in ASP.NET Core 3.0.</span></span>
>
> ```csharp
> private const string HealthCheckServiceAssembly =
>     "Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherHostedService";
>
> services.TryAddEnumerable(
>     ServiceDescriptor.Singleton(typeof(IHostedService),
>         typeof(HealthCheckPublisherOptions).Assembly
>             .GetType(HealthCheckServiceAssembly)));
> ```

> [!NOTE]
> <span data-ttu-id="09693-624">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) include i server di pubblicazione per diversi sistemi, tra cui [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="09693-624">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="09693-625">[AspNetCore. Diagnostics. HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) non è gestito né supportato da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="09693-625">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="09693-626">Limitare i controlli integrità con MapWhen</span><span class="sxs-lookup"><span data-stu-id="09693-626">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="09693-627">Usare <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> per creare un ramo condizionale della pipeline delle richieste per gli endpoint di controllo integrità.</span><span class="sxs-lookup"><span data-stu-id="09693-627">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="09693-628">Nell'esempio seguente `MapWhen` esegue il branching della pipeline di richiesta per attivare il middleware dei controlli di integrità se viene ricevuta una richiesta GET per l'endpoint `api/HealthCheck`:</span><span class="sxs-lookup"><span data-stu-id="09693-628">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="09693-629">Per ulteriori informazioni, vedere <xref:fundamentals/middleware/index#use-run-and-map>.</span><span class="sxs-lookup"><span data-stu-id="09693-629">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end
