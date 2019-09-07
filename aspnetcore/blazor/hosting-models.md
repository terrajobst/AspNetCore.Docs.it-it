---
title: Modelli di hosting di ASP.NET Core Blazer
author: guardrex
description: Informazioni sui modelli di hosting lato client e lato server di Blazer.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: blazor/hosting-models
ms.openlocfilehash: f7a16d64e1f874a4f6b3c8db5217810b13c7c6ff
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800434"
---
# <a name="aspnet-core-blazor-hosting-models"></a>Modelli di hosting di ASP.NET Core Blazer

Di [Daniel Roth](https://github.com/danroth27)

Blazer è un framework web progettato per l'esecuzione sul lato client nel browser in un Runtime .NET basato su [webassembly](https://webassembly.org/) (*lato client Blazer*) o sul lato server in ASP.NET Core (*lato server Blaze*). Indipendentemente dal modello di hosting, i modelli di app e componenti *sono gli stessi*.

Per creare un progetto per i modelli di hosting descritti in questo articolo, <xref:blazor/get-started>vedere.

## <a name="client-side"></a>Lato client

Il modello di hosting principale per blazer è in esecuzione sul lato client nel browser del webassembly. L'app Blazor, le relative dipendenze e il runtime .NET vengono scaricati nel browser. L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser. Gli aggiornamenti dell'interfaccia utente e la gestione degli eventi si verificano nello stesso processo. Le risorse dell'app vengono distribuite come file statici in un server Web o in un servizio in grado di servire contenuto statico ai client.

![Lato client Blazer: L'app Blazer viene eseguita su un thread dell'interfaccia utente all'interno del browser.](hosting-models/_static/client-side.png)

Per creare un'app Blazer usando il modello di hosting lato client, usare il modello di **app Webassembly Blazer** ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)).

Dopo aver selezionato il modello di **applicazione Webassembly Blazer** , è possibile configurare l'app per l'uso di un back-end ASP.NET Core selezionando la casella di controllo **ASP.NET Core Hosted** ([DotNet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)). L'app ASP.NET Core serve l'app Blazer ai client. L'app del lato client blazer può interagire con il server tramite la rete usando chiamate API Web o [SignalR](xref:signalr/introduction).

I modelli includono lo script *blazer. webassembly. js* che gestisce:

* Download del runtime .NET, dell'app e delle dipendenze dell'app.
* Inizializzazione del runtime per l'esecuzione dell'app.

Il modello di hosting lato client offre diversi vantaggi:

* Non esiste alcuna dipendenza lato server .NET. L'app funziona completamente dopo il download nel client.
* Le risorse e le funzionalità client sono completamente sfruttate.
* Il lavoro viene scaricato dal server al client.
* Non è necessario un server Web ASP.NET Core per ospitare l'app. Gli scenari di distribuzione senza server sono possibili, ad esempio per servire l'app da una rete CDN.

Ci sono svantaggi per l'hosting lato client:

* L'app è limitata alle funzionalità del browser.
* È necessario disporre di hardware e software client idonei (ad esempio, il supporto di webassembly).
* Le dimensioni del download sono maggiori e le app importano più tempo per il caricamento.
* Il supporto di runtime e strumenti .NET è meno maturo. Ad esempio, esistono limitazioni in [.NET standard](/dotnet/standard/net-standard) supporto e debug.

## <a name="server-side"></a>Lato server

Con il modello di hosting lato server, l'app viene eseguita nel server dall'interno di un'app ASP.NET Core. Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione [SignalR](xref:signalr/introduction).

![Il browser interagisce con l'app (ospitata all'interno di un'app ASP.NET Core) sul server tramite una connessione SignalR.](hosting-models/_static/server-side.png)

Per creare un'app Blazer usando il modello di hosting lato server, usare il modello di **app del server ASP.NET Core Blazer** ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)). L'app ASP.NET Core ospita l'app sul lato server e crea l'endpoint SignalR a cui si connettono i client.

L'app ASP.NET Core fa riferimento alla `Startup` classe dell'app per aggiungere:

* Servizi lato server.
* App per la pipeline di gestione delle richieste.

Lo script&dagger; *blazer. Server. js* stabilisce la connessione client. È responsabilità dell'app salvare in modo permanente e ripristinare lo stato dell'app come richiesto, ad esempio in caso di perdita di una connessione di rete.

Il modello di hosting lato server offre diversi vantaggi:

* Le dimensioni del download sono notevolmente inferiori rispetto a un'app sul lato client e l'app viene caricata molto più velocemente.
* L'app sfrutta appieno le funzionalità del server, incluso l'uso di tutte le API compatibili con .NET Core.
* .NET Core nel server viene usato per eseguire l'app, in modo che gli strumenti .NET esistenti, ad esempio il debug, funzionino come previsto.
* Sono supportati i thin client. Ad esempio, le app sul lato server funzionano con i browser che non supportano webassembly e nei dispositivi con vincoli di risorse.
* La codebase .NET/C# code dell'app, incluso il codice componente dell'app, non viene servita ai client.

Ci sono svantaggi per l'hosting lato server:

* In genere esiste una latenza più elevata. Ogni interazione dell'utente comporta un hop di rete.
* Non è disponibile alcun supporto offline. Se la connessione client non riesce, l'app smette di funzionare.
* La scalabilità è complessa per le app con molti utenti. Il server deve gestire più connessioni client e gestire lo stato del client.
* Per gestire l'app, è necessario un server ASP.NET Core. Gli scenari di distribuzione senza server non sono possibili, ad esempio per servire l'app da una rete CDN.

&dagger;Lo script *blazer. Server. js* viene servito da una risorsa incorporata nel framework condiviso ASP.NET Core.

### <a name="reconnection-to-the-same-server"></a>Riconnessione allo stesso server

Le app sul lato server di Blazer richiedono una connessione SignalR attiva al server. Se la connessione viene persa, l'app tenta di riconnettersi al server. Fino a quando lo stato del client è ancora in memoria, la sessione client riprende senza perdere lo stato.
 
Quando il client rileva che la connessione è stata persa, viene visualizzata un'interfaccia utente predefinita quando il client tenta di riconnettersi. Se la riconnessione non riesce, all'utente viene offerta l'opzione per riprovare. Per personalizzare l'interfaccia utente, definire un elemento `components-reconnect-modal` con `id` come nella pagina Razor *_Host. cshtml* . Il client aggiorna questo elemento con una delle seguenti classi CSS in base allo stato della connessione:
 
* `components-reconnect-show`&ndash; Mostra l'interfaccia utente per indicare che la connessione è stata persa e il client sta tentando di riconnettersi.
* `components-reconnect-hide`&ndash; Il client ha una connessione attiva e nasconde l'interfaccia utente.
* `components-reconnect-failed`&ndash; Riconnessione non riuscita. Per ritentare la riconnessione `window.Blazor.reconnect()`, chiamare.

### <a name="stateful-reconnection-after-prerendering"></a>Riconnessione con stato dopo il rendering preliminare
 
Per impostazione predefinita, le app lato server di Blaze sono impostate per eseguire il prerendering dell'interfaccia utente nel server prima che venga stabilita la connessione client al server. Questa impostazione è configurata nella pagina Razor *_Host. cshtml* :
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode`Configura se il componente:

* Viene preeseguito nella pagina.
* Viene visualizzato come HTML statico nella pagina o se include le informazioni necessarie per eseguire il bootstrap di un'app Blazer dall'agente utente.

| `RenderMode`        | Descrizione |
| ------------------- | ----------- |
| `ServerPrerendered` | Esegue il rendering del componente in HTML statico e include un marcatore per un'app Blazer sul lato server. Quando l'agente utente viene avviato, questo marcatore viene usato per il bootstrap di un'app blazer. I parametri non sono supportati. |
| `Server`            | Esegue il rendering di un marcatore per un'app del lato server Blaze. L'output del componente non è incluso. Quando l'agente utente viene avviato, questo marcatore viene usato per il bootstrap di un'app blazer. I parametri non sono supportati. |
| `Static`            | Esegue il rendering del componente in HTML statico. I parametri sono supportati. |

Il rendering dei componenti server da una pagina HTML statica non è supportato.
 
Il client si riconnette al server con lo stesso stato usato per eseguire il prerendering dell'app. Se lo stato dell'app è ancora in memoria, lo stato del componente non viene sottoposto a rendering dopo che è stata stabilita la connessione SignalR.

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Eseguire il rendering di componenti interattivi con stato da pagine e visualizzazioni Razor
 
I componenti interattivi con stato possono essere aggiunti a una pagina o a una visualizzazione Razor.

Quando viene eseguito il rendering della pagina o della visualizzazione:

* Il componente viene preeseguito con la pagina o la visualizzazione.
* Lo stato iniziale del componente utilizzato per il prerendering viene perso.
* Quando viene stabilita la connessione SignalR, viene creato il nuovo stato del componente.
 
La pagina Razor seguente esegue il rendering `Counter` di un componente:

```cshtml
<h1>My Razor Page</h1>
 
@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Eseguire il rendering di componenti non interattivi da pagine e visualizzazioni Razor

Nella pagina Razor seguente il `MyComponent` componente viene sottoposto a rendering statico con un valore iniziale specificato usando un form:
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>
 
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.Static, 
    new { InitialValue = InitialValue }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

Poiché `MyComponent` viene sottoposto a rendering statico, il componente non può essere interattivo.

### <a name="detect-when-the-app-is-prerendering"></a>Rilevare il momento in cui viene eseguito il prerendering dell'app
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a>Configurare il client SignalR per le app del lato server di Blaze
 
In alcuni casi, è necessario configurare il client SignalR usato dalle app del lato server di Blazer. Ad esempio, potrebbe essere necessario configurare la registrazione nel client SignalR per diagnosticare un problema di connessione.
 
Per configurare il client SignalR nel file *pages/_Host. cshtml* :

* Aggiungere un `autostart="false"` attributo `<script>` al tag per lo script *blazer. Server. js* .
* Chiamare `Blazor.start` e passare un oggetto di configurazione che specifichi il generatore SignalR.
 
```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging(2); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:blazor/get-started>
* <xref:signalr/introduction>
