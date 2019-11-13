---
title: Ospitare e distribuire ASP.NET Core Server Blazor
author: guardrex
description: Informazioni su come ospitare e distribuire un'app Server Blazor usando ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/server
ms.openlocfilehash: ad83c57f55c75d4a49656fa5d7046c32b0f049ff
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963567"
---
# <a name="host-and-deploy-opno-locblazor-server"></a>Ospitare e distribuire Blazor server

Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>Valori di configurazione dell'host

[Blazor app Server](xref:blazor/hosting-models#blazor-server) possono accettare [valori di configurazione host generici](xref:fundamentals/host/generic-host#host-configuration).

## <a name="deployment"></a>Distribuzione

Utilizzando il [modello di hosting del server diBlazor](xref:blazor/hosting-models#blazor-server), Blazor viene eseguito sul server dall'interno di un'app di ASP.NET Core. Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestiti tramite una connessione [SignalR](xref:signalr/introduction) .

È necessario un server Web in grado di ospitare un'app ASP.NET Core. Visual Studio include il modello di progetto **app ServerBlazor** (`blazorserverside` modello quando si usa il comando [DotNet New](/dotnet/core/tools/dotnet-new) ).

## <a name="scalability"></a>Scalabilità

Pianificare una distribuzione per sfruttare al meglio l'infrastruttura disponibile per un'app Server Blazor. Per informazioni sulla scalabilità delle app Server Blazor, vedere le risorse seguenti:

* [Nozioni fondamentali sulle app Server Blazor](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a>Server di distribuzione

Quando si considera la scalabilità di un singolo server (scalabilità verticale), la memoria disponibile per un'app è probabilmente la prima risorsa che verrà esaurita dall'app quando le richieste degli utenti aumentano. La memoria disponibile sul server influiscono su:

* Numero di circuiti attivi che un server è in grado di supportare.
* Latenza dell'interfaccia utente nel client.

Per istruzioni sulla creazione di app Server Blazor sicure e scalabili, vedere <xref:security/blazor/server>.

Ogni circuito utilizza circa 250 KB di memoria per un'app di tipo *Hello World*minima. La dimensione di un circuito dipende dal codice dell'app e dai requisiti di manutenzione dello stato associati a ogni componente. Si consiglia di misurare le richieste di risorse durante lo sviluppo dell'applicazione e dell'infrastruttura, ma la linea di base seguente può essere un punto di partenza per la pianificazione della destinazione di distribuzione: se si prevede che l'app supporti 5.000 utenti simultanei, prendere in considerazione il budget almeno 1,3 GB di memoria del server per l'app (o circa 273 KB per utente).

### <a name="opno-locsignalr-configuration"></a>configurazione di SignalR

le app Server Blazor usano SignalR ASP.NET Core per comunicare con il browser. [le condizioni di hosting e scalabilità diSignalR](xref:signalr/publish-to-azure-web-app) si applicano alle app Blazor server.

Blazor funziona al meglio quando si usano WebSocket come il trasporto SignalR a causa di latenza, affidabilità e [sicurezza](xref:signalr/security)inferiori. Il polling prolungato viene usato da SignalR quando WebSocket non è disponibile o quando l'app è configurata in modo esplicito per l'uso del polling prolungato. Quando si esegue la distribuzione nel servizio app Azure, configurare l'app per l'uso di WebSocket nelle impostazioni portale di Azure per il servizio. Per informazioni dettagliate sulla configurazione dell'app per il servizio app Azure, vedere le [linee guida](xref:signalr/publish-to-azure-web-app)per la pubblicazione diSignalR.

È consigliabile usare il [servizio SignalR di Azure](/azure/azure-signalr) per le app Blazor server. Il servizio consente di aumentare le prestazioni di un'app Server Blazor per un numero elevato di connessioni SignalR simultanee. Inoltre, la portata globale del servizio SignalR e i Data Center a prestazioni elevate consentono di ridurre significativamente la latenza a causa della geografia. Per configurare un'app e, facoltativamente, effettuare il provisioning del servizio SignalR di Azure:

* Creare un profilo di pubblicazione per le app di Azure in Visual Studio per l'app Server Blazor.
* Aggiungere la dipendenza del **servizio SignalR di Azure** al profilo. Se la sottoscrizione di Azure non dispone di un'istanza del servizio SignalR di Azure esistente da assegnare all'app, selezionare **Crea una nuova istanza del servizio SignalR** di Azure per eseguire il provisioning di una nuova istanza del servizio.
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
