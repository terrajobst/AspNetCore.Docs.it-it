---
title: Collegamento del browser in ASP.NET Core
author: ncarandini
description: Viene illustrato come il collegamento del Browser è una funzionalità di Visual Studio che si collega l'ambiente di sviluppo con uno o più web browser.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 452ba5149563c186750466f471c7b950f0017614
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894708"
---
# <a name="browser-link-in-aspnet-core"></a>Collegamento del browser in ASP.NET Core

Dal [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), e [Tom Dykstra](https://github.com/tdykstra)

Collegamento del browser è una funzionalità di Visual Studio che crea un canale di comunicazione tra l'ambiente di sviluppo e uno o più web browser. È possibile usare il collegamento del Browser per aggiornare l'applicazione web in diversi browser in una sola volta, è utile per i test tra browser.

## <a name="browser-link-setup"></a>Programma di installazione di browser Link

::: moniker range=">= aspnetcore-2.1"

Durante la conversione di un progetto ASP.NET Core 2.0 in ASP.NET Core 2.1 e in fase di transizione per il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), installare il [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) creare un pacchetto per Funzionalità BrowserLink. Usano i modelli di progetto ASP.NET Core 2.1 il `Microsoft.AspNetCore.App` metapacchetto per impostazione predefinita.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ASP.NET Core 2.0 **applicazione Web**, **vuota**, e **API Web** utilizzo di modelli di progetto di [metapacchetto Microsoft. aspnetcore](xref:fundamentals/metapackage) , che contiene un riferimento al pacchetto per [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/). Pertanto, l'utilizzo di `Microsoft.AspNetCore.All` metapacchetto non richiede alcuna azione aggiuntiva per rendere disponibili per l'uso di collegamento del Browser.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

ASP.NET Core 1.x **applicazione Web** modello di progetto contiene un riferimento al pacchetto per il [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pacchetto. Il **vuote** oppure **API Web** progetti di modello richiedono è quindi necessario aggiungere un riferimento al pacchetto `Microsoft.VisualStudio.Web.BrowserLink`.

Poiché si tratta di una funzionalità di Visual Studio, il modo più semplice per aggiungere il pacchetto a un **vuota** o **API Web** progetto del modello consiste nell'aprire il **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) ed eseguire il comando seguente:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

In alternativa, è possibile usare **Gestisci pacchetti NuGet**. Fare clic sul nome del progetto in **Esplora soluzioni** e scegliere **Gestisci pacchetti NuGet**:

![Gestione pacchetti NuGet Open](using-browserlink/_static/open-nuget-package-manager.png)

Trovare e installare il pacchetto:

![Aggiungere il pacchetto con Gestione pacchetti NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a>Configurazione

Nel metodo `Startup.Configure`:

```csharp
app.UseBrowserLink();
```

È in genere il codice all'interno di un `if` blocco che consente solo il collegamento del Browser nell'ambiente di sviluppo, come illustrato di seguito:

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

Per altre informazioni, vedere [Usare più ambienti](xref:fundamentals/environments).

## <a name="how-to-use-browser-link"></a>Come usare il collegamento del Browser

Quando è aperto un progetto ASP.NET Core, Visual Studio visualizza il controllo di collegamento del Browser sulla barra degli strumenti accanto al **destinazione di Debug** toolbar (controllo):

![Menu a discesa collegamento browser](using-browserlink/_static/browserLink-dropdown-menu.png)

Dal controllo della barra degli strumenti di collegamento del Browser, è possibile:

* Aggiornare l'applicazione web in diversi browser in una sola volta.
* Aprire il **Dashboard Browser Link**.
* Abilitare o disabilitare **collegamento del Browser**. Nota: Collegamento del browser è disabilitato per impostazione predefinita in Visual Studio 2017 (versione 15.3).
* Abilitare o disabilitare [sincronizzazione automatica CSS](#enable-or-disable-css-auto-sync).

> [!NOTE]
> Alcuni plug-in Visual Studio, in particolare *Web estensione Pack 2015* e *Web estensione Pack 2017*, offrono funzionalità estese per il collegamento del Browser, ma alcune funzionalità aggiuntive non funzionano con ASP. Progetti .NET Core.

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>Aggiornare l'app web in diversi browser in una sola volta

Per scegliere un browser web singola da avviare quando si avvia il progetto, usare il menu a discesa nel **destinazione di Debug** toolbar (controllo):

![Menu di scelta rapida F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Per aprire più browser in una sola volta, scegliere **Esplora con...**  dalla stessa elenco a discesa. Tenere premuto il tasto CTRL per selezionare il browser desiderato e quindi fare clic su **esplorare**:

![Apri molti browser in una sola volta](using-browserlink/_static/open-many-browsers-at-once.png)

Ecco uno screenshot che mostra Visual Studio con la visualizzazione dell'indice open e due i browser aperti:

![La sincronizzazione con esempio di due browser](using-browserlink/_static/sync-with-two-browsers-example.png)

Passare il mouse sopra il controllo della barra degli strumenti di collegamento del Browser per visualizzare i browser connessi al progetto:

![Suggerimento al passaggio del mouse](using-browserlink/_static/hoover-tip.png)

Modificare la visualizzazione dell'indice e tutti i browser collegati vengono aggiornati quando si fa clic sul pulsante Aggiorna il collegamento del Browser:

![browser-sync-a-changes](using-browserlink/_static/browsers-sync-to-changes.png)

Collegamento del browser funziona anche con i browser che avvia da esterno a Visual Studio e passare all'URL dell'applicazione.

### <a name="the-browser-link-dashboard"></a>Dashboard Browser Link

Aprire il Dashboard Browser Link nell'elenco di Browser Link a discesa di menu per gestire la connessione con il browser aperto:

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

Se non è connesso alcun browser, è possibile avviare una sessione di debug non selezionando il *Visualizza nel Browser* collegamento:

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

In caso contrario, il browser connessi vengono visualizzati con il percorso della pagina che mostra ogni browser:

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Se si desidera, è possibile fare clic sul nome di un browser elencati per aggiornare il browser singolo.

### <a name="enable-or-disable-browser-link"></a>Abilitare o disabilitare il collegamento del Browser

Quando si riabilita il collegamento del Browser dopo la sua disabilitazione, è necessario aggiornare il browser per riconnettersi a essi.

### <a name="enable-or-disable-css-auto-sync"></a>Abilitare o disabilitare la sincronizzazione automatica CSS

Quando è abilitato sincronizzazione automatica CSS, browser connessi vengono aggiornati automaticamente quando si apportano modifiche al file CSS.

## <a name="how-it-works"></a>Come funziona

Collegamento del browser Usa SignalR per creare un canale di comunicazione tra Visual Studio e il browser. Quando è abilitato il collegamento del Browser, Visual Studio funge da un server di SignalR in grado di connettersi a più client (browser). Collegamento del browser registra anche un componente del middleware nella pipeline delle richieste ASP.NET Core. Il componente inserisce speciali `<script>` riferimenti in ogni richiesta di pagina dal server. È possibile visualizzare i riferimenti a script selezionando **Visualizza origine** nel browser e lo scorrimento al fine del `<body>` contrassegnare il contenuto:

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

I file di origine non verranno più modificati. Il componente del middleware inserisce in modo dinamico i riferimenti a script.

Dal momento che il codice del browser-side è il supporto di JavaScript, funziona in tutti i browser che supporta SignalR senza richiedere un plug-in del browser.
