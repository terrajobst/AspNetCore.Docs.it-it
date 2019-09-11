---
title: Modelli di hosting di ASP.NET Core Blazer
author: guardrex
description: Informazioni sui modelli di hosting lato client e lato server di Blazer.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/07/2019
uid: blazor/hosting-models
ms.openlocfilehash: 7880affa59af1fa4fc47aee3dc98ae9aa53729af
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878339"
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

### <a name="comparison-to-server-rendered-ui"></a>Confronto con interfaccia utente sottoposta a rendering server

Un modo per comprendere le app del server blazer è comprendere come si differenzia dai modelli tradizionali per il rendering dell'interfaccia utente nelle app ASP.NET Core usando le visualizzazioni Razor o Razor Pages. Entrambi i modelli utilizzano il linguaggio Razor per descrivere il contenuto HTML, ma presentano differenze significative per il rendering del markup.

Quando viene eseguito il rendering di una pagina o di una visualizzazione Razor, ogni riga di codice Razor emette HTML in formato testo. Dopo il rendering, il server Elimina l'istanza della pagina o della vista, incluso qualsiasi stato prodotto. Quando si verifica un'altra richiesta della pagina, ad esempio quando la convalida del server non riesce e viene visualizzato il riepilogo di convalida:

* Viene nuovamente eseguito il rendering dell'intera pagina in testo HTML.
* La pagina viene inviata al client.

Un'app blazer è costituita da elementi riutilizzabili di un'interfaccia utente denominata *Components*. Un componente contiene C# codice, markup e altri componenti. Quando viene eseguito il rendering di un componente, blazer produce un grafico dei componenti inclusi in modo simile a un codice HTML o XML Document Object Model (DOM). Questo grafico include lo stato del componente contenuto in proprietà e campi. Blazer valuta il grafico dei componenti per produrre una rappresentazione binaria del markup. Il formato binario può essere:

* Trasformato in testo HTML (durante il prerendering).
* Utilizzato per aggiornare in modo efficiente il markup durante il normale rendering.

Un aggiornamento dell'interfaccia utente in blazer viene attivato da:

* Interazione dell'utente, ad esempio la selezione di un pulsante.
* Trigger dell'app, ad esempio un timer.

Viene eseguito il rendering del grafico e viene calcolata *una differenza dell'interfaccia utente* (differenza). Questa differenza è il set più piccolo di modifiche DOM necessarie per aggiornare l'interfaccia utente nel client. Il diff viene inviato al client in un formato binario e applicato dal browser.

Un componente viene eliminato dopo che l'utente si è spostato dal client. Mentre un utente interagisce con un componente, lo stato del componente (servizi, risorse) deve essere mantenuto nella memoria del server. Poiché lo stato di molti componenti può essere gestito simultaneamente dal server, l'esaurimento della memoria è un problema che deve essere risolto. Per istruzioni su come creare un'app del server blazer per garantire l'utilizzo ottimale della memoria del server, <xref:security/blazor/server-side>vedere.

### <a name="circuits"></a>Circuiti

Un'app Server Blazer si basa su [ASP.NET Core SignalR](xref:signalr/introduction). Ogni client comunica al server tramite una o più connessioni SignalR dette *circuito*. Un circuito è l'astrazione di Blazer sulle connessioni SignalR che possono tollerare interruzioni di rete temporanee. Quando un client Blaze rileva che la connessione SignalR è disconnessa, tenta di riconnettersi al server usando una nuova connessione SignalR.

Ogni schermata del browser (scheda del browser o iframe) connessa a un'app del server Blazer usa una connessione SignalR. Si tratta ancora di un'altra distinzione importante rispetto alle app tipiche sottoposte a rendering server. In un'app sottoposta a rendering server, l'apertura della stessa app in più schermate del browser in genere non viene convertita in richieste di risorse aggiuntive sul server. In un'app Server blazer, ogni schermata del browser richiede un circuito separato e istanze separate dello stato dei componenti che devono essere gestite dal server.

Blazer considera la chiusura di una scheda del browser o l'esplorazione di un URL esterno a una chiusura *normale* . In caso di chiusura normale, il circuito e le risorse associate vengono immediatamente rilasciati. Un client può anche disconnettersi in modo non corretto, ad esempio a causa di un'interruzione della rete. Il server Blazer archivia circuiti disconnessi per un intervallo configurabile in modo da consentire la riconnessione del client. Per ulteriori informazioni, vedere la sezione [riconnessione allo stesso server](#reconnection-to-the-same-server) .

### <a name="ui-latency"></a>Latenza interfaccia utente

La latenza dell'interfaccia utente è il tempo necessario da un'azione iniziata al momento dell'aggiornamento dell'interfaccia utente. I valori più bassi per la latenza dell'interfaccia utente sono imperativi perché un'app risponda a un utente. In un'app Server Blazer ogni azione viene inviata al server, elaborata e viene restituita una diff dell'interfaccia utente. Di conseguenza, la latenza dell'interfaccia utente è la somma della latenza di rete e la latenza del server nell'elaborazione dell'azione.

Per un'app line-of-business limitata a una rete aziendale privata, l'effetto sulle percezioni utente della latenza a causa della latenza di rete è in genere impercettibile. Per un'app distribuita su Internet, la latenza può diventare evidente per gli utenti, in particolare se gli utenti sono ampiamente distribuiti geograficamente.

L'utilizzo della memoria può anche contribuire alla latenza dell'app. Un aumento dell'utilizzo della memoria comporta un frequente Garbage Collection o il paging della memoria su disco, che comportano una riduzione delle prestazioni dell'app e quindi aumentano la latenza dell'interfaccia Per altre informazioni, vedere <xref:security/blazor/server-side>.

Le app del server Blazer devono essere ottimizzate per ridurre la latenza dell'interfaccia utente riducendo la latenza di rete e l'utilizzo della memoria Per un approccio alla misurazione della latenza di <xref:host-and-deploy/blazor/server-side#measure-network-latency>rete, vedere. Per ulteriori informazioni su SignalR e blazer, vedere:

* <xref:host-and-deploy/blazor/server-side>
* <xref:security/blazor/server-side>

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
