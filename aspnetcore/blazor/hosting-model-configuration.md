---
title: ASP.NET Core Blazor configurazione del modello di hosting
author: guardrex
description: Informazioni su Blazor la configurazione del modello di hosting, inclusa la procedura per integrare i componenti Razor in app Razor Pages e MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 1f71ac63bbe9dc9d56cfca2ded19a5b863be828f
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306430"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a>Configurazione del modello di hosting di ASP.NET Core Blazer

Di [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Questo articolo illustra la configurazione del modello di hosting.

## <a name="blazor-webassembly"></a>WebAssembly Blazor

A partire dalla versione di ASP.NET Core 3,2 Preview 3, il webassembly Blazer supporta la configurazione da:

* *Wwwroot/appSettings. JSON*
* *Wwwroot/appSettings. {ENVIRONMENT}. JSON*

In un'app ospitata da Blazer, l' [ambiente di runtime](xref:fundamentals/environments) è uguale al valore dell'app Server.

Quando si esegue l'app in locale, l'ambiente USA per impostazione predefinita lo sviluppo. Quando l'app viene pubblicata, per impostazione predefinita viene impostato l'ambiente di produzione. Per ulteriori informazioni, tra cui come configurare l'ambiente, vedere <xref:fundamentals/environments>.

> [!WARNING]
> La configurazione in un'app webassembly blazer è visibile agli utenti. **Non archiviare i segreti dell'app o le credenziali nella configurazione.**

I file di configurazione vengono memorizzati nella cache per l'uso offline. Con [le applicazioni Web progressive (PWA)](xref:blazor/progressive-web-app)è possibile aggiornare solo i file di configurazione quando si crea una nuova distribuzione. La modifica dei file di configurazione tra le distribuzioni non ha effetto perché:

* Gli utenti dispongono di versioni memorizzate nella cache dei file che continuano a usare.
* I file *service-worker. js* e *service-worker-assets. js* di PWA devono essere ricompilati durante la compilazione, che segnalano all'app la prossima visita online dell'utente che l'app è stata ridistribuita.

Per ulteriori informazioni sulla gestione degli aggiornamenti in background da PWA, vedere <xref:blazor/progressive-web-app#background-updates>.

## <a name="blazor-server"></a>Server Blazor

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
