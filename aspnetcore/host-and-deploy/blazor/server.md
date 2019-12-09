---
title: Ospitare e distribuire ASP.NET Core Server Blazor
author: guardrex
description: Informazioni su come ospitare e distribuire un'app Server Blazor usando ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/server
ms.openlocfilehash: d45d355eabc53fc90bcda4cb7be22fb6a9f04541
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944334"
---
# <a name="host-and-deploy-opno-locblazor-server"></a><span data-ttu-id="12c93-103">Ospitare e distribuire Blazor server</span><span class="sxs-lookup"><span data-stu-id="12c93-103">Host and deploy Blazor Server</span></span>

<span data-ttu-id="12c93-104">Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="12c93-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="12c93-105">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="12c93-105">Host configuration values</span></span>

<span data-ttu-id="12c93-106">[Blazor app Server](xref:blazor/hosting-models#blazor-server) possono accettare [valori di configurazione host generici](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="12c93-106">[Blazor Server apps](xref:blazor/hosting-models#blazor-server) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="12c93-107">Distribuzione</span><span class="sxs-lookup"><span data-stu-id="12c93-107">Deployment</span></span>

<span data-ttu-id="12c93-108">Utilizzando il [modello di hosting del server diBlazor](xref:blazor/hosting-models#blazor-server), Blazor viene eseguito sul server dall'interno di un'app di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12c93-108">Using the [Blazor Server hosting model](xref:blazor/hosting-models#blazor-server), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="12c93-109">Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestiti tramite una connessione [SignalR](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="12c93-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="12c93-110">È necessario un server Web in grado di ospitare un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12c93-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="12c93-111">Visual Studio include il modello di progetto **app ServerBlazor** (`blazorserverside` modello quando si usa il comando [DotNet New](/dotnet/core/tools/dotnet-new) ).</span><span class="sxs-lookup"><span data-stu-id="12c93-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="12c93-112">Scalabilità</span><span class="sxs-lookup"><span data-stu-id="12c93-112">Scalability</span></span>

<span data-ttu-id="12c93-113">Pianificare una distribuzione per sfruttare al meglio l'infrastruttura disponibile per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="12c93-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="12c93-114">Per informazioni sulla scalabilità delle app Server Blazor, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="12c93-114">See the following resources to address Blazor Server app scalability:</span></span>

* <span data-ttu-id="12c93-115">[Nozioni fondamentali sulle app Server Blazor](xref:blazor/hosting-models#blazor-server)</span><span class="sxs-lookup"><span data-stu-id="12c93-115">[Fundamentals of Blazor Server apps](xref:blazor/hosting-models#blazor-server)</span></span>
* <xref:security/blazor/server>

### <a name="deployment-server"></a><span data-ttu-id="12c93-116">Server di distribuzione</span><span class="sxs-lookup"><span data-stu-id="12c93-116">Deployment server</span></span>

<span data-ttu-id="12c93-117">Quando si considera la scalabilità di un singolo server (scalabilità verticale), la memoria disponibile per un'app è probabilmente la prima risorsa che verrà esaurita dall'app quando le richieste degli utenti aumentano.</span><span class="sxs-lookup"><span data-stu-id="12c93-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="12c93-118">La memoria disponibile sul server influiscono su:</span><span class="sxs-lookup"><span data-stu-id="12c93-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="12c93-119">Numero di circuiti attivi che un server è in grado di supportare.</span><span class="sxs-lookup"><span data-stu-id="12c93-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="12c93-120">Latenza dell'interfaccia utente nel client.</span><span class="sxs-lookup"><span data-stu-id="12c93-120">UI latency on the client.</span></span>

<span data-ttu-id="12c93-121">Per istruzioni sulla creazione di app Server Blazor sicure e scalabili, vedere <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="12c93-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="12c93-122">Ogni circuito utilizza circa 250 KB di memoria per un'app di tipo *Hello World*minima.</span><span class="sxs-lookup"><span data-stu-id="12c93-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="12c93-123">La dimensione di un circuito dipende dal codice dell'app e dai requisiti di manutenzione dello stato associati a ogni componente.</span><span class="sxs-lookup"><span data-stu-id="12c93-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="12c93-124">Si consiglia di misurare le richieste di risorse durante lo sviluppo dell'applicazione e dell'infrastruttura, ma la linea di base seguente può essere un punto di partenza per la pianificazione della destinazione di distribuzione: se si prevede che l'app supporti 5.000 utenti simultanei, prendere in considerazione il budget almeno 1,3 GB di memoria del server per l'app (o circa 273 KB per utente).</span><span class="sxs-lookup"><span data-stu-id="12c93-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="opno-locsignalr-configuration"></a><span data-ttu-id="12c93-125">Configurazione di SignalR</span><span class="sxs-lookup"><span data-stu-id="12c93-125">SignalR configuration</span></span>

<span data-ttu-id="12c93-126">le app Server Blazor usano SignalR ASP.NET Core per comunicare con il browser.</span><span class="sxs-lookup"><span data-stu-id="12c93-126">Blazor Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="12c93-127">[le condizioni di hosting e scalabilità diSignalR](xref:signalr/publish-to-azure-web-app) si applicano alle app Blazor server.</span><span class="sxs-lookup"><span data-stu-id="12c93-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

Blazor<span data-ttu-id="12c93-128"> funziona al meglio quando si usano WebSocket come il trasporto SignalR a causa di latenza, affidabilità e [sicurezza](xref:signalr/security)inferiori.</span><span class="sxs-lookup"><span data-stu-id="12c93-128"> works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="12c93-129">Il polling prolungato viene usato da SignalR quando WebSocket non è disponibile o quando l'app è configurata in modo esplicito per l'uso del polling prolungato.</span><span class="sxs-lookup"><span data-stu-id="12c93-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="12c93-130">Quando si esegue la distribuzione nel servizio app Azure, configurare l'app per l'uso di WebSocket nelle impostazioni portale di Azure per il servizio.</span><span class="sxs-lookup"><span data-stu-id="12c93-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="12c93-131">Per informazioni dettagliate sulla configurazione dell'app per il servizio app Azure, vedere le [linee guida](xref:signalr/publish-to-azure-web-app)per la pubblicazione diSignalR.</span><span class="sxs-lookup"><span data-stu-id="12c93-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

#### <a name="azure-opno-locsignalr-service"></a><span data-ttu-id="12c93-132">Servizio SignalR di Azure</span><span class="sxs-lookup"><span data-stu-id="12c93-132">Azure SignalR Service</span></span>

<span data-ttu-id="12c93-133">È consigliabile usare il [servizio SignalR di Azure](/azure/azure-signalr) per le app Blazor server.</span><span class="sxs-lookup"><span data-stu-id="12c93-133">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="12c93-134">Il servizio consente di aumentare le prestazioni di un'app Server Blazor per un numero elevato di connessioni SignalR simultanee.</span><span class="sxs-lookup"><span data-stu-id="12c93-134">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="12c93-135">Inoltre, la portata globale del servizio SignalR e i Data Center a prestazioni elevate consentono di ridurre significativamente la latenza a causa della geografia.</span><span class="sxs-lookup"><span data-stu-id="12c93-135">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span> <span data-ttu-id="12c93-136">Per configurare un'app e, facoltativamente, effettuare il provisioning del servizio SignalR di Azure:</span><span class="sxs-lookup"><span data-stu-id="12c93-136">To configure an app (and optionally provision) the Azure SignalR Service:</span></span>

1. <span data-ttu-id="12c93-137">Consentire al servizio di supportare le *sessioni permanenti*, in cui i client vengono [reindirizzati allo stesso server durante il prerendering](xref:blazor/hosting-models#reconnection-to-the-same-server).</span><span class="sxs-lookup"><span data-stu-id="12c93-137">Enable the service to support *sticky sessions*, where clients are [redirected back to the same server when prerendering](xref:blazor/hosting-models#reconnection-to-the-same-server).</span></span> <span data-ttu-id="12c93-138">Impostare l'opzione `ServerStickyMode` o il valore di configurazione su `Required`.</span><span class="sxs-lookup"><span data-stu-id="12c93-138">Set the `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="12c93-139">In genere, un'app crea la configurazione usando **uno** degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="12c93-139">Typically, an app creates the configuration using **one** of the following approaches:</span></span>

   * <span data-ttu-id="12c93-140">`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="12c93-140">`Startup.ConfigureServices`:</span></span>
  
     ```csharp
     services.AddSignalR().AddAzureSignalR(options =>
     {
         options.ServerStickyMode = 
             Microsoft.Azure.SignalR.ServerStickyMode.Required;
     });
     ```

   * <span data-ttu-id="12c93-141">Configurazione (usare **uno** degli approcci seguenti):</span><span class="sxs-lookup"><span data-stu-id="12c93-141">Configuration (use **one** of the following approaches):</span></span>
  
     * <span data-ttu-id="12c93-142">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="12c93-142">*appsettings.json*:</span></span>

       ```json
       "Azure:SignalR:ServerStickyMode": "Required"
       ```

     * <span data-ttu-id="12c93-143">La **configurazione** del servizio app > **le impostazioni dell'applicazione** nella portale di Azure (**nome**: `Azure:SignalR:ServerStickyMode`, **valore**: `Required`).</span><span class="sxs-lookup"><span data-stu-id="12c93-143">The app service's **Configuration** > **Application settings** in the Azure portal (**Name**: `Azure:SignalR:ServerStickyMode`, **Value**: `Required`).</span></span>

1. <span data-ttu-id="12c93-144">Creare un profilo di pubblicazione per le app di Azure in Visual Studio per l'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="12c93-144">Create an Azure Apps publish profile in Visual Studio for the Blazor Server app.</span></span>
1. <span data-ttu-id="12c93-145">Aggiungere la dipendenza del **servizio SignalR di Azure** al profilo.</span><span class="sxs-lookup"><span data-stu-id="12c93-145">Add the **Azure SignalR Service** dependency to the profile.</span></span> <span data-ttu-id="12c93-146">Se la sottoscrizione di Azure non dispone di un'istanza del servizio SignalR di Azure esistente da assegnare all'app, selezionare **Crea una nuova istanza del servizio SignalR** di Azure per eseguire il provisioning di una nuova istanza del servizio.</span><span class="sxs-lookup"><span data-stu-id="12c93-146">If the Azure subscription doesn't have a pre-existing Azure SignalR Service instance to assign to the app, select **Create a new Azure SignalR Service instance** to provision a new service instance.</span></span>
1. <span data-ttu-id="12c93-147">Pubblicare l'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="12c93-147">Publish the app to Azure.</span></span>

#### <a name="iis"></a><span data-ttu-id="12c93-148">IIS</span><span class="sxs-lookup"><span data-stu-id="12c93-148">IIS</span></span>

<span data-ttu-id="12c93-149">Quando si usa IIS, le sessioni permanenti sono abilitate con il routing delle richieste di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="12c93-149">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="12c93-150">Per altre informazioni, vedere [bilanciamento del carico http usando il routing delle richieste di applicazioni](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="12c93-150">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="kubernetes"></a><span data-ttu-id="12c93-151">Kubernetes</span><span class="sxs-lookup"><span data-stu-id="12c93-151">Kubernetes</span></span>

<span data-ttu-id="12c93-152">Creare una definizione di ingresso con le [annotazioni Kubernetes seguenti per le sessioni permanenti](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/):</span><span class="sxs-lookup"><span data-stu-id="12c93-152">Create an ingress definition with the following [Kubernetes annotations for sticky sessions](https://kubernetes.github.io/ingress-nginx/examples/affinity/cookie/):</span></span>

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <ingress-name>
  annotations:
    nginx.ingress.kubernetes.io/affinity: "cookie"
    nginx.ingress.kubernetes.io/session-cookie-name: "affinity"
    nginx.ingress.kubernetes.io/session-cookie-expires: "14400"
    nginx.ingress.kubernetes.io/session-cookie-max-age: "14400"
```

### <a name="measure-network-latency"></a><span data-ttu-id="12c93-153">Misurare la latenza di rete</span><span class="sxs-lookup"><span data-stu-id="12c93-153">Measure network latency</span></span>

<span data-ttu-id="12c93-154">L' [interoperabilità JS](xref:blazor/javascript-interop) può essere usata per misurare la latenza di rete, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="12c93-154">[JS interop](xref:blazor/javascript-interop) can be used to measure network latency, as the following example demonstrates:</span></span>

```razor
@inject IJSRuntime JS

@if (latency is null)
{
    <span>Calculating...</span>
}
else
{
    <span>@(latency.Value.TotalMilliseconds)ms</span>
}

@code
{
    private DateTime startTime;
    private TimeSpan? latency;

    protected override async Task OnInitializedAsync()
    {
        startTime = DateTime.UtcNow;
        var _ = await JS.InvokeAsync<string>("toString");
        latency = DateTime.UtcNow - startTime;
    }
}
```

<span data-ttu-id="12c93-155">Per un'esperienza dell'interfaccia utente ragionevole, è consigliabile una latenza di interfaccia utente prolungata di 250ms o meno.</span><span class="sxs-lookup"><span data-stu-id="12c93-155">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
