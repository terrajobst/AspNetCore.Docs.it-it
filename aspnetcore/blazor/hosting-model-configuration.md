---
title: ASP.NET Core Blazor configurazione del modello di hosting
author: guardrex
description: Informazioni su Blazor la configurazione del modello di hosting, inclusa la procedura per integrare i componenti Razor in app Razor Pages e MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 91dd1aa32bb5165845eb4794377a50ccbd01adc3
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215060"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>Configurazione del modello di hosting di ASP.NET Core Blazer

Di [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Questo articolo illustra la configurazione del modello di hosting.

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a>Server Blazor

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

   * Aggiungere un tag `<script>` per lo script *blazer. Server. js* immediatamente prima del tag di chiusura `</body>`:

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

1. In `Startup.ConfigureServices`registrare il servizio Server Blazer:

   ```csharp
   services.AddServerSideBlazor();
   ```

1. In `Startup.Configure`aggiungere l'endpoint dell'hub blazer per `app.UseEndpoints`:

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

   Quando si usa una cartella personalizzata per archiviare i componenti dell'app, aggiungere lo spazio dei nomi che rappresenta la cartella al file *pages/_ViewImports. cshtml* . Per altre informazioni, vedere <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

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

   Quando si usa una cartella personalizzata per archiviare i componenti dell'app, aggiungere lo spazio dei nomi che rappresenta la cartella al file *views/_ViewImports. cshtml* . Per altre informazioni, vedere <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.

### <a name="reflect-the-connection-state-in-the-ui"></a>Riflette lo stato di connessione nell'interfaccia utente

Quando il client rileva che la connessione è stata persa, viene visualizzata un'interfaccia utente predefinita quando il client tenta di riconnettersi. Se la riconnessione non riesce, all'utente viene offerta l'opzione per riprovare.

Per personalizzare l'interfaccia utente, definire un elemento con un `id` di `components-reconnect-modal` nel `<body>` della pagina Razor *_Host. cshtml* :

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

Nella tabella seguente vengono descritte le classi CSS applicate all'elemento `components-reconnect-modal`.

| Classe CSS                       | Indica&hellip; |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | Connessione persa. È in corso un tentativo di riconnessione del client. Mostra il modale. |
| `components-reconnect-hide`     | Viene ristabilita una connessione attiva al server. Nascondere il modale. |
| `components-reconnect-failed`   | Riconnessione non riuscita. probabilmente a causa di un errore di rete. Per tentare la riconnessione, chiamare `window.Blazor.reconnect()`. |
| `components-reconnect-rejected` | Riconnessione rifiutata. Il server è stato raggiunto ma ha rifiutato la connessione e lo stato dell'utente nel server è andato perso. Per ricaricare l'app, chiamare `location.reload()`. Questo stato di connessione può verificarsi nei casi seguenti:<ul><li>Si verifica un arresto anomalo del circuito sul lato server.</li><li>Il client viene disconnesso abbastanza a lungo da consentire al server di eliminare lo stato dell'utente. Le istanze dei componenti con cui l'utente interagisce sono state eliminate.</li><li>Il server viene riavviato oppure il processo di lavoro dell'app viene riciclato.</li></ul> |

### <a name="render-mode"></a>Modalità di rendering

Per impostazione predefinita, le app del server Blazor sono configurate per eseguire il prerendering dell'interfaccia utente nel server prima che venga stabilita la connessione client al server. Questa impostazione è configurata nella pagina Razor *_Host. cshtml* :

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
* Viene visualizzato come HTML statico nella pagina o se include le informazioni necessarie per eseguire il bootstrap di un'app Blazor dall'agente utente.

| `RenderMode`        | Descrizione |
| ------------------- | ----------- |
| `ServerPrerendered` | Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor. Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor. |
| `Server`            | Esegue il rendering di un marcatore per un'app Server Blazor. L'output del componente non è incluso. Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor. |
| `Static`            | Esegue il rendering del componente in HTML statico. |

Il rendering dei componenti server da una pagina HTML statica non è supportato.

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
