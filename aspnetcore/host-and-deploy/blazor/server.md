---
title: Ospitare e distribuire ASP.NET Core Server Blazer
author: guardrex
description: Informazioni su come ospitare e distribuire un'app Server Blazer usando ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: host-and-deploy/blazor/server
ms.openlocfilehash: 693d7ff67bad3a0c5bd050b795833763056ed511
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378819"
---
# <a name="host-and-deploy-blazor-server"></a>Ospitare e distribuire il server Blazer

Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valori di configurazione dell'host

Le [app del server Blazer](xref:blazor/hosting-models#blazor-server) possono accettare [valori di configurazione host generici](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Distribuzione

Con il [modello di hosting del server Blazer](xref:blazor/hosting-models#blazor-server), Blazer viene eseguito sul server dall'interno di un'app ASP.NET Core. Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione [SignalR](xref:signalr/introduction).

È necessario un server Web in grado di ospitare un'app ASP.NET Core. Visual Studio include il modello di progetto **App server Blazor** (modello `blazorserverside` quando si usa il comando [dotnet new](/dotnet/core/tools/dotnet-new)).

## <a name="scalability"></a>Scalabilità

Pianificare una distribuzione per sfruttare al meglio l'infrastruttura disponibile per un'app del server blazer. Vedere le risorse seguenti per risolvere la scalabilità delle app del server Blazer:

* [Nozioni fondamentali sulle app del server Blazer](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a>Server di distribuzione

Quando si considera la scalabilità di un singolo server (scalabilità verticale), la memoria disponibile per un'app è probabilmente la prima risorsa che verrà esaurita dall'app quando le richieste degli utenti aumentano. La memoria disponibile sul server influiscono su:

* Numero di circuiti attivi che un server è in grado di supportare.
* Latenza dell'interfaccia utente nel client.

Per istruzioni sulla creazione di app Server Blazer sicure e scalabili, vedere <xref:security/blazor/server>.

Ogni circuito utilizza circa 250 KB di memoria per un'app di tipo *Hello World*minima. La dimensione di un circuito dipende dal codice dell'app e dai requisiti di manutenzione dello stato associati a ogni componente. Si consiglia di misurare le richieste di risorse durante lo sviluppo dell'applicazione e dell'infrastruttura, ma la linea di base seguente può essere un punto di partenza per la pianificazione della destinazione di distribuzione: se si prevede che l'app supporti 5.000 utenti simultanei, prendere in considerazione il budget almeno 1,3 GB di memoria del server per l'app (o circa 273 KB per utente).

### <a name="signalr-configuration"></a>Configurazione di SignalR

Le app del server Blazer usano ASP.NET Core SignalR per comunicare con il browser. [Le condizioni di hosting e scalabilità di SignalR](xref:signalr/publish-to-azure-web-app) si applicano alle app del server blazer.

Blazer funziona meglio quando si usano WebSocket come trasporto SignalR grazie a latenza, affidabilità e [sicurezza](xref:signalr/security)inferiori. Il polling prolungato viene usato da SignalR quando WebSocket non è disponibile o quando l'app è configurata in modo esplicito per l'uso del polling prolungato. Quando si esegue la distribuzione nel servizio app Azure, configurare l'app per l'uso di WebSocket nelle impostazioni portale di Azure per il servizio. Per informazioni dettagliate sulla configurazione dell'app per il servizio app Azure, vedere le [linee guida sulla pubblicazione di SignalR](xref:signalr/publish-to-azure-web-app).

È consigliabile usare il [servizio Azure SignalR](/azure/azure-signalr) per le app del server blazer. Il servizio consente la scalabilità verticale di un'app del server blazer a un numero elevato di connessioni di SignalR simultanee. Inoltre, la portata globale del servizio SignalR e i Data Center a prestazioni elevate consentono di ridurre significativamente la latenza a causa della geografia. Per configurare un'app e, facoltativamente, effettuare il provisioning del servizio Azure SignalR:

* Creare un profilo di pubblicazione per le app di Azure in Visual Studio per l'app Server blazer.
* Aggiungere la dipendenza del **servizio Azure SignalR** al profilo. Se la sottoscrizione di Azure non dispone di un'istanza del servizio Azure SignalR esistente da assegnare all'app, selezionare **Crea una nuova istanza del servizio SignalR di Azure** per eseguire il provisioning di una nuova istanza del servizio.
* Pubblicare l'app in Azure.

### <a name="measure-network-latency"></a>Misurare la latenza di rete

L' [interoperabilità JS](xref:blazor/javascript-interop) può essere usata per misurare la latenza di rete, come illustrato nell'esempio seguente:

```cshtml
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

Per un'esperienza dell'interfaccia utente ragionevole, è consigliabile una latenza di interfaccia utente prolungata di 250ms o meno.
