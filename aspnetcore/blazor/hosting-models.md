---
title: ASP.NET Core Blazor modelli di hosting
author: guardrex
description: Informazioni sui modelli di hosting Blazor webassembly e Blazor server.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
- blazor.webassembly.js
uid: blazor/hosting-models
ms.openlocfilehash: c9521acf40317c90d1197660bfa516710263cfc9
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160041"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a>ASP.NET Core Blazor modelli di hosting

Di [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor è un framework web progettato per l'esecuzione sul lato client nel browser in un Runtime .NET basato su [webassembly](https://webassembly.org/)( *Blazor webassembly*) o sul lato server in ASP.NET Core ( *Blazor server*). Indipendentemente dal modello di hosting, i modelli di app e componenti *sono gli stessi*.

Per creare un progetto per i modelli di hosting descritti in questo articolo, vedere <xref:blazor/get-started>.

## <a name="opno-locblazor-webassembly"></a>Blazor webassembly

Il modello di hosting principale per Blazor è in esecuzione sul lato client nel browser del webassembly. L'app Blazor, le relative dipendenze e il Runtime .NET vengono scaricati nel browser. L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser. Gli aggiornamenti dell'interfaccia utente e la gestione degli eventi si verificano nello stesso processo. Le risorse dell'app vengono distribuite come file statici in un server Web o in un servizio in grado di servire contenuto statico ai client.

![[! OP. NO-LOC (Blazer)] webassembly: [! OP. L'app NO-LOC (Blazer)] viene eseguita su un thread dell'interfaccia utente all'interno del browser.](hosting-models/_static/blazor-webassembly.png)

Per creare un'app Blazor usando il modello di hosting lato client, usare il modello di **app webassemblyBlazor** ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)).

Dopo aver selezionato il modello di **app webassemblyBlazor** , è possibile configurare l'app per l'uso di un back-end ASP.NET Core selezionando la casella di controllo **ASP.NET Core Hosted** ([DotNet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)). L'app ASP.NET Core serve l'app Blazor ai client. L'app webassembly Blazor può interagire con il server tramite la rete usando chiamate API Web o [SignalR](xref:signalr/introduction).

I modelli includono lo script `blazor.webassembly.js` che gestisce:

* Download del runtime .NET, dell'app e delle dipendenze dell'app.
* Inizializzazione del runtime per l'esecuzione dell'app.

Il modello di hosting webassembly Blazor offre diversi vantaggi:

* Non esiste alcuna dipendenza lato server .NET. L'app funziona completamente dopo il download nel client.
* Le risorse e le funzionalità client sono completamente sfruttate.
* Il lavoro viene scaricato dal server al client.
* Non è necessario un server Web ASP.NET Core per ospitare l'app. Gli scenari di distribuzione senza server sono possibili, ad esempio per servire l'app da una rete CDN.

Ci sono svantaggi per Blazor l'hosting di webassembly:

* L'app è limitata alle funzionalità del browser.
* È necessario disporre di hardware e software client idonei (ad esempio, il supporto di webassembly).
* Le dimensioni del download sono maggiori e le app importano più tempo per il caricamento.
* Il supporto di runtime e strumenti .NET è meno maturo. Ad esempio, esistono limitazioni in [.NET standard](/dotnet/standard/net-standard) supporto e debug.

## <a name="opno-locblazor-server"></a>Server Blazor

Con il modello di hosting del server di Blazor, l'app viene eseguita nel server dall'interno di un'app ASP.NET Core. Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestiti tramite una connessione [SignalR](xref:signalr/introduction) .

![Il browser interagisce con l'app (ospitata all'interno di un'app ASP.NET Core) sul server tramite un [! OP. Connessione NO-LOC (SignalR)].](hosting-models/_static/blazor-server.png)

Per creare un'app Blazor usando il modello di hosting del server Blazor, usare il modello di **app ASP.NET CoreBlazor server** ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)). L'app ASP.NET Core ospita l'app Blazor server e crea l'endpoint SignalR a cui si connettono i client.

L'app ASP.NET Core fa riferimento alla classe `Startup` dell'app per aggiungere:

* Servizi lato server.
* App per la pipeline di gestione delle richieste.

Lo script `blazor.server.js`&dagger; stabilisce la connessione client. È responsabilità dell'app salvare in modo permanente e ripristinare lo stato dell'app come richiesto, ad esempio in caso di perdita di una connessione di rete.

Il modello di hosting del server Blazor offre diversi vantaggi:

* Le dimensioni del download sono notevolmente inferiori rispetto a un'app webassembly Blazor e l'app viene caricata molto più velocemente.
* L'app sfrutta appieno le funzionalità del server, incluso l'uso di tutte le API compatibili con .NET Core.
* .NET Core nel server viene usato per eseguire l'app, in modo che gli strumenti .NET esistenti, ad esempio il debug, funzionino come previsto.
* Sono supportati i thin client. Ad esempio, le app Server Blazor funzionano con i browser che non supportano webassembly e nei dispositivi con vincoli di risorse.
* La codebase .NET/C# code dell'app, incluso il codice componente dell'app, non viene servita ai client.

Ci sono svantaggi per l'hosting di Blazor server:

* In genere esiste una latenza più elevata. Ogni interazione dell'utente comporta un hop di rete.
* Non è disponibile alcun supporto offline. Se la connessione client non riesce, l'app smette di funzionare.
* La scalabilità è complessa per le app con molti utenti. Il server deve gestire più connessioni client e gestire lo stato del client.
* Per gestire l'app, è necessario un server ASP.NET Core. Gli scenari di distribuzione senza server non sono possibili, ad esempio per servire l'app da una rete CDN.

&dagger;lo script di `blazor.server.js` viene servito da una risorsa incorporata nel framework ASP.NET Core Shared.

### <a name="comparison-to-server-rendered-ui"></a>Confronto con interfaccia utente sottoposta a rendering server

Un modo per comprendere Blazor app Server consiste nel comprendere come si differenzia dai modelli tradizionali per il rendering dell'interfaccia utente nelle app ASP.NET Core con visualizzazioni Razor o Razor Pages. Entrambi i modelli utilizzano il linguaggio Razor per descrivere il contenuto HTML, ma presentano differenze significative per il rendering del markup.

Quando viene eseguito il rendering di una pagina o di una visualizzazione Razor, ogni riga di codice Razor emette HTML in formato testo. Dopo il rendering, il server Elimina l'istanza della pagina o della vista, incluso qualsiasi stato prodotto. Quando si verifica un'altra richiesta della pagina, ad esempio quando la convalida del server non riesce e viene visualizzato il riepilogo di convalida:

* Viene nuovamente eseguito il rendering dell'intera pagina in testo HTML.
* La pagina viene inviata al client.

Un'app Blazor è costituita da elementi riutilizzabili di un'interfaccia utente denominata *Components*. Un componente contiene C# codice, markup e altri componenti. Quando viene eseguito il rendering di un componente, Blazor produce un grafico dei componenti inclusi in modo analogo a un HTML o XML Document Object Model (DOM). Questo grafico include lo stato del componente contenuto in proprietà e campi. Blazor valuta il grafico dei componenti per produrre una rappresentazione binaria del markup. Il formato binario può essere:

* Trasformato in testo HTML (durante il prerendering).
* Utilizzato per aggiornare in modo efficiente il markup durante il normale rendering.

Un aggiornamento dell'interfaccia utente in Blazor viene attivato da:

* Interazione dell'utente, ad esempio la selezione di un pulsante.
* Trigger dell'app, ad esempio un timer.

Viene eseguito il rendering del grafico e viene calcolata *una differenza dell'interfaccia utente* (differenza). Questa differenza è il set più piccolo di modifiche DOM necessarie per aggiornare l'interfaccia utente nel client. Il diff viene inviato al client in un formato binario e applicato dal browser.

Un componente viene eliminato dopo che l'utente si è spostato dal client. Mentre un utente interagisce con un componente, lo stato del componente (servizi, risorse) deve essere mantenuto nella memoria del server. Poiché lo stato di molti componenti può essere gestito simultaneamente dal server, l'esaurimento della memoria è un problema che deve essere risolto. Per istruzioni su come creare un'app Server Blazor per garantire l'utilizzo ottimale della memoria del server, vedere <xref:security/blazor/server>.

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a>Integrare i componenti Razor in app Razor Pages e MVC

#### <a name="use-components-in-pages-and-views"></a>Usare i componenti in pagine e visualizzazioni

Un'app Razor Pages o MVC esistente può integrare i componenti Razor in pagine e visualizzazioni:

1. Nel file di layout dell'app ( *_Layout. cshtml*):

   * Aggiungere il tag di `<base>` seguente all'elemento `<head>`:

     ```html
     <base href="~/" />
     ```

     Il valore `href` (il *percorso di base dell'app*) nell'esempio precedente presuppone che l'app si trovi nel percorso URL radice (`/`). Se l'app è un'applicazione secondaria, seguire le istruzioni nella sezione *percorso di base dell'app* dell'articolo <xref:host-and-deploy/blazor/index#app-base-path>.

     Il file *_Layout. cshtml* si trova nella cartella *pages/Shared* in un'app Razor Pages o in una cartella *Views/Shared* in un'app MVC.

   * Aggiungere un tag `<script>` per lo script *blazer. Server. js* all'interno del tag di chiusura `</body>`:

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     Il Framework aggiunge lo script *blazer. Server. js* all'app. Non è necessario aggiungere manualmente lo script all'app.

1. Aggiungere un file *_Imports. Razor* alla cartella radice del progetto con il contenuto seguente (modificare l'ultimo spazio dei nomi, `MyAppNamespace`, nello spazio dei nomi dell'app):

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. In `Startup.ConfigureServices`aggiungere il servizio Blazor server:

   ```csharp
   services.AddServerSideBlazor();
   ```

1. In `Startup.Configure`aggiungere l'endpoint dell'hub Blazor a `app.UseEndpoints`:

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. Integrare i componenti in qualsiasi pagina o visualizzazione. Per ulteriori informazioni, vedere la sezione *integrare componenti in Razor Pages e app MVC* dell'articolo <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-a-razor-pages-app"></a>Usare componenti instradabili in un'app Razor Pages

Per supportare componenti Razor instradabili nelle app Razor Pages:

1. Seguire le istruzioni riportate nella sezione [usare i componenti in pagine e viste](#use-components-in-pages-and-views) .

1. Aggiungere un file *app. Razor* alla radice del progetto con il contenuto seguente:

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. Aggiungere un file *_Host. cshtml* alla cartella *pages* con il contenuto seguente:

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   I componenti usano il file Shared *_Layout. cshtml* per il layout.

1. Aggiungere una route con priorità bassa per la pagina *_Host. cshtml* alla configurazione dell'endpoint in `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. Aggiungere componenti instradabili all'app. Ad esempio:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Quando si usa una cartella personalizzata per archiviare i componenti dell'app, aggiungere lo spazio dei nomi che rappresenta la cartella al file *pages/_ViewImports. cshtml* . Per ulteriori informazioni, vedere <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

#### <a name="use-routable-components-in-an-mvc-app"></a>Usare componenti instradabili in un'app MVC

Per supportare i componenti Razor instradabili nelle app MVC:

1. Seguire le istruzioni riportate nella sezione [usare i componenti in pagine e viste](#use-components-in-pages-and-views) .

1. Aggiungere un file *app. Razor* alla radice del progetto con il contenuto seguente:

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. Aggiungere un file *_Host. cshtml* alla cartella *views/Home* con il contenuto seguente:

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   I componenti usano il file Shared *_Layout. cshtml* per il layout.

1. Aggiungere un'azione al controller Home:

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. Aggiungere una route con priorità bassa per l'azione del controller che restituisce la vista *_Host. cshtml* alla configurazione dell'endpoint in `Startup.Configure`:

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. Creare una cartella *pages* e aggiungere componenti instradabili all'app. Ad esempio:

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   Quando si usa una cartella personalizzata per archiviare i componenti dell'app, aggiungere lo spazio dei nomi che rappresenta la cartella al file *views/_ViewImports. cshtml* . Per ulteriori informazioni, vedere <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

### <a name="circuits"></a>Circuiti

Un'app Server Blazor è basata sull' [SignalRASP.NET Core ](xref:signalr/introduction). Ogni client comunica al server tramite una o più connessioni di SignalR chiamate *circuito*. Un circuito viene Blazorastrazione su SignalR connessioni che possono tollerare interruzioni di rete temporanee. Quando un client Blazor rileva che la connessione SignalR è disconnessa, tenta di riconnettersi al server utilizzando una nuova connessione SignalR.

Ogni schermata del browser (scheda del browser o iframe) connessa a un'app del server Blazor usa una connessione SignalR. Si tratta ancora di un'altra distinzione importante rispetto alle app tipiche sottoposte a rendering server. In un'app sottoposta a rendering server, l'apertura della stessa app in più schermate del browser in genere non viene convertita in richieste di risorse aggiuntive sul server. In un'app Server Blazor, ogni schermata del browser richiede un circuito separato e istanze separate dello stato dei componenti che devono essere gestite dal server.

Blazor considera la chiusura di una scheda del browser o l'esplorazione di un URL esterno a una chiusura *normale* . In caso di chiusura normale, il circuito e le risorse associate vengono immediatamente rilasciati. Un client può anche disconnettersi in modo non corretto, ad esempio a causa di un'interruzione della rete. Blazor Server archivia circuiti disconnessi per un intervallo configurabile in modo da consentire la riconnessione del client. Per ulteriori informazioni, vedere la sezione [riconnessione allo stesso server](#reconnection-to-the-same-server) .

### <a name="ui-latency"></a>Latenza interfaccia utente

La latenza dell'interfaccia utente è il tempo necessario da un'azione iniziata al momento dell'aggiornamento dell'interfaccia utente. I valori più bassi per la latenza dell'interfaccia utente sono imperativi perché un'app risponda a un utente. In un'app Server Blazor, ogni azione viene inviata al server, elaborata e viene restituita una diff dell'interfaccia utente. Di conseguenza, la latenza dell'interfaccia utente è la somma della latenza di rete e la latenza del server nell'elaborazione dell'azione.

Per un'app line-of-business limitata a una rete aziendale privata, l'effetto sulle percezioni utente della latenza a causa della latenza di rete è in genere impercettibile. Per un'app distribuita su Internet, la latenza può diventare evidente per gli utenti, in particolare se gli utenti sono ampiamente distribuiti geograficamente.

L'utilizzo della memoria può anche contribuire alla latenza dell'app. Un aumento dell'utilizzo della memoria comporta un frequente Garbage Collection o il paging della memoria su disco, che comportano una riduzione delle prestazioni dell'app e quindi aumentano la latenza dell'interfaccia Per ulteriori informazioni, vedere <xref:security/blazor/server>.

Blazor le app Server devono essere ottimizzate per ridurre al minimo la latenza dell'interfaccia utente riducendo la latenza di rete e l'utilizzo Per un approccio alla misurazione della latenza di rete, vedere <xref:host-and-deploy/blazor/server#measure-network-latency>. Per ulteriori informazioni su SignalR e Blazor, vedere:

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a>Connessione al server

Blazor app Server richiedono una connessione SignalR attiva al server. Se la connessione viene persa, l'app tenta di riconnettersi al server. Fino a quando lo stato del client è ancora in memoria, la sessione client riprende senza perdere lo stato.

#### <a name="reconnection-to-the-same-server"></a>Riconnessione allo stesso server

Viene eseguito il prerendering di un'app Blazor server in risposta alla prima richiesta client, che imposta lo stato dell'interfaccia utente sul server. Quando il client tenta di creare una connessione SignalR, il client deve riconnettersi allo stesso server. Blazor app server che usano più di un server back-end devono implementare *sessioni permanenti* per le connessioni SignalR.

È consigliabile usare il [servizio SignalR di Azure](/azure/azure-signalr) per le app Blazor server. Il servizio consente di aumentare le prestazioni di un'app Server Blazor per un numero elevato di connessioni SignalR simultanee. Le sessioni permanenti sono abilitate per il servizio Azure SignalR impostando l'opzione di `ServerStickyMode` del servizio o il valore di configurazione su `Required`. Per ulteriori informazioni, vedere <xref:host-and-deploy/blazor/server#signalr-configuration>.

Quando si usa IIS, le sessioni permanenti sono abilitate con il routing delle richieste di applicazioni. Per altre informazioni, vedere [bilanciamento del carico http usando il routing delle richieste di applicazioni](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).

#### <a name="reflect-the-connection-state-in-the-ui"></a>Riflette lo stato di connessione nell'interfaccia utente

Quando il client rileva che la connessione è stata persa, viene visualizzata un'interfaccia utente predefinita quando il client tenta di riconnettersi. Se la riconnessione non riesce, all'utente viene offerta l'opzione per riprovare.

Per personalizzare l'interfaccia utente, definire un elemento con un `id` di `components-reconnect-modal` nel `<body>` della pagina Razor *_Host. cshtml* :

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

Nella tabella seguente vengono descritte le classi CSS applicate all'elemento `components-reconnect-modal`.

| Classe CSS                       | Indica&hellip; |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | Connessione persa. È in corso un tentativo di riconnessione del client. Mostra il modale. |
| `components-reconnect-hide`     | Viene ristabilita una connessione attiva al server. Nascondere il modale. |
| `components-reconnect-failed`   | Riconnessione non riuscita. probabilmente a causa di un errore di rete. Per tentare la riconnessione, chiamare `window.Blazor.reconnect()`. |
| `components-reconnect-rejected` | Riconnessione rifiutata. Il server è stato raggiunto ma ha rifiutato la connessione e lo stato dell'utente nel server è andato perso. Per ricaricare l'app, chiamare `location.reload()`. Questo stato di connessione può verificarsi nei casi seguenti:<ul><li>Si verifica un arresto anomalo del circuito sul lato server.</li><li>Il client viene disconnesso abbastanza a lungo da consentire al server di eliminare lo stato dell'utente. Le istanze dei componenti con cui l'utente interagisce sono state eliminate.</li><li>Il server viene riavviato oppure il processo di lavoro dell'app viene riciclato.</li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a>Riconnessione con stato dopo il rendering preliminare

per impostazione predefinita, le app di Blazor server sono impostate per eseguire il prerendering dell'interfaccia utente nel server prima che venga stabilita la connessione client al server. Questa impostazione è configurata nella pagina Razor *_Host. cshtml* :

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

`RenderMode` configura se il componente:

* Viene preeseguito nella pagina.
* Viene sottoposto a rendering come HTML statico nella pagina o se include le informazioni necessarie per avviare un'app Blazor dall'agente utente.

| `RenderMode`        | Descrizione |
| ------------------- | ----------- |
| `ServerPrerendered` | Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor. Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor. |
| `Server`            | Esegue il rendering di un marcatore per un'app Server Blazor. L'output del componente non è incluso. Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor. |
| `Static`            | Esegue il rendering del componente in HTML statico. |

Il rendering dei componenti server da una pagina HTML statica non è supportato.

Quando `RenderMode` viene `ServerPrerendered`, il componente viene inizialmente sottoposto a rendering statico come parte della pagina. Quando il browser stabilisce una connessione al server, viene *nuovamente*eseguito il rendering del componente e il componente è ora interattivo. Se è presente il metodo [{Async} Lifecycle OnInitialized](xref:blazor/lifecycle#component-initialization-methods) per l'inizializzazione del componente, il metodo viene eseguito *due volte*:

* Quando il componente viene preeseguito in modo statico.
* Una volta stabilita la connessione al server.

Ciò può comportare una modifica evidente nei dati visualizzati nell'interfaccia utente quando il componente viene sottoposto a rendering.

Per evitare lo scenario di doppio rendering in un'app Server Blazor:

* Passare un identificatore che può essere usato per memorizzare nella cache lo stato durante il prerendering e recuperare lo stato dopo il riavvio dell'app.
* Utilizzare l'identificatore durante il prerendering per salvare lo stato del componente.
* Utilizzare l'identificatore dopo il prerendering per recuperare lo stato memorizzato nella cache.

Il codice seguente illustra un `WeatherForecastService` aggiornato in un'app Server Blazor basata su modelli che evita il doppio rendering:

```csharp
public class WeatherForecastService
{
    private static readonly string[] Summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = Summaries[rng.Next(Summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a>Eseguire il rendering di componenti interattivi con stato da pagine e visualizzazioni Razor

I componenti interattivi con stato possono essere aggiunti a una pagina o a una visualizzazione Razor.

Quando viene eseguito il rendering della pagina o della visualizzazione:

* Il componente viene preeseguito con la pagina o la visualizzazione.
* Lo stato iniziale del componente utilizzato per il prerendering viene perso.
* Quando viene stabilita la connessione SignalR viene creato il nuovo stato del componente.

La pagina Razor seguente esegue il rendering di un componente `Counter`:

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a>Eseguire il rendering di componenti non interattivi da pagine e visualizzazioni Razor

Nella pagina Razor seguente il componente `Counter` viene sottoposto a rendering statico con un valore iniziale specificato utilizzando un form:

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

Poiché `MyComponent` viene sottoposto a rendering statico, il componente non può essere interattivo.

### <a name="detect-when-the-app-is-prerendering"></a>Rilevare il momento in cui viene eseguito il prerendering dell'app

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a>Configurare il client di SignalR per le app Blazor server

In alcuni casi, è necessario configurare il client SignalR usato dalle app Blazor server. È ad esempio possibile configurare la registrazione nel client di SignalR per diagnosticare un problema di connessione.

Per configurare il client di SignalR nel file *pages/_Host. cshtml* :

* Aggiungere un attributo `autostart="false"` al tag `<script>` per lo script `blazor.server.js`.
* Chiamare `Blazor.start` e passare un oggetto di configurazione che specifichi il generatore di SignalR.

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:blazor/get-started>
* <xref:signalr/introduction>
