---
title: Collegamento del browser in ASP.NET Core
author: ncarandini
description: "Una funzionalità di Visual Studio che collega l'ambiente di sviluppo con uno o più web browser"
keywords: ASP.NET Core, il collegamento del browser, sincronizzazione CSS
ms.author: riande
manager: wpickett
ms.date: 12/28/2016
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 211dd5d03e6b8414e0b2ed3234d8970c92f72452
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-browser-link-in-aspnet-core"></a>Introduzione al collegamento del Browser ASP.NET Core 

Da [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), e [Tom Dykstra](https://github.com/tdykstra)

Collegamento del browser è una funzionalità di Visual Studio che crea un canale di comunicazione tra l'ambiente di sviluppo e uno o più web browser. È possibile utilizzare il collegamento del Browser per aggiornare l'applicazione web nel browser diverse contemporaneamente, che è utile per il testing e browser.

## <a name="browser-link-setup"></a>Installazione del collegamento browser

ASP.NET Core **applicazione Web** modelli di progetto nella finestra di Visual Studio 2015 e versioni successive sono disponibili tutti gli elementi necessari per il collegamento del Browser.

Per aggiungere il collegamento del Browser a un progetto creato utilizzando ASP.NET Core **vuoto** o **API Web** modello, seguire questi passaggi:

1. Aggiungere il *Microsoft.VisualStudio.Web.BrowserLink.Loader* pacchetto 
2. Aggiungere il codice di configurazione nel *Startup.cs* file.

### <a name="add-the-package"></a>Aggiungere il pacchetto

Poiché si tratta di una funzionalità di Visual Studio, il modo più semplice per aggiungere il pacchetto è per aprire la **Package Manager Console** (**Vista > altre finestre > Console di gestione pacchetti**) ed eseguire il comando seguente:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink.Loader
```

In alternativa, è possibile utilizzare **Gestione pacchetti NuGet**.  Fare doppio clic sul nome del progetto in **Esplora**e scegliere **Gestisci pacchetti NuGet**. 

![Aprire Gestisci pacchetti NuGet](using-browserlink/_static/open-nuget-package-manager.png)

Quindi, individuare e installare il pacchetto.

![Aggiungere il pacchetto con Gestione pacchetti NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

### <a name="add-configuration-code"></a>Aggiungere il codice di configurazione

Aprire il *Startup.cs* file e il `Configure` metodo aggiungere il codice seguente:

```csharp
app.UseBrowserLink();
```

In genere che il codice è all'interno di un `if` blocco che consente il collegamento Browser solo nell'ambiente di sviluppo, come illustrato di seguito:

[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]

Per altre informazioni, vedere [Uso di più ambienti](../fundamentals/environments.md).

## <a name="how-to-use-browser-link"></a>Come utilizzare il collegamento del Browser

Quando è aperto un progetto ASP.NET Core, Visual Studio Mostra il controllo barra degli strumenti di collegamento del Browser accanto al **destinazione di Debug** controllo barra degli strumenti:

![Menu a discesa collegamento browser](using-browserlink/_static/browserLink-dropdown-menu.png)

Dal controllo della barra degli strumenti di collegamento del Browser, è possibile:

- Aggiornare l'applicazione web browser diversi in una sola volta
- Aprire il **Dashboard del collegamento Browser**
- Abilitare o disabilitare **collegamento Browser**
- Abilitare o disabilitare la sincronizzazione automatica CSS

> [!NOTE]
> Alcuni plug-in Visual Studio, in particolare *Web estensione Pack 2015* e *Web estensione Pack 2017*, offrono funzionalità estese per il collegamento del Browser, ma alcune funzionalità aggiuntive non funzionano con ASP. Progetti di componenti di base NET.

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Aggiornare l'applicazione web browser diversi in una sola volta

Per scegliere un singolo web browser per avviare all'avvio del progetto, utilizzare il menu di scelta rapida nel **destinazione di Debug** controllo barra degli strumenti:

![Menu di scelta rapida F5](using-browserlink/_static/debug-target-dropdown-menu.png)

Per aprire contemporaneamente più browser, scegliere **Esplora con... ** dalla stessa elenco a discesa.  Tenere premuto il tasto CTRL per selezionare i browser desiderato e quindi fare clic su **Sfoglia**:

![Aprire contemporaneamente molti browser](using-browserlink/_static/open-many-browsers-at-once.png)

Ecco una schermata di esempio con Visual Studio con la visualizzazione dell'indice aperto e due i browser aperti:

![Sincronizzazione con un esempio di due browser](using-browserlink/_static/sync-with-two-browsers-example.png)

Passare il mouse sopra il controllo barra degli strumenti di collegamento del Browser per visualizzare i browser che sono connessi al progetto:

![Suggerimento al passaggio del mouse](using-browserlink/_static/hoover-tip.png)

Modificare la visualizzazione dell'indice e tutti i browser connessi vengono aggiornati quando si fa clic sul pulsante Aggiorna collegamento Browser:

![browser sincronizzazione di modifiche](using-browserlink/_static/browsers-sync-to-changes.png)

Collegamento del browser funziona anche con i browser che si avvia dall'esterno di Visual Studio e accedere all'URL dell'applicazione.

### <a name="the-browser-link-dashboard"></a>Dashboard del collegamento Browser

Aprire il Dashboard di collegamento Browser dal menu per gestire la connessione con i browser aperte a discesa collegamento Browser:

![dashboard di browserslink Apri](using-browserlink/_static/open-browserlink-dashboard.png)

Se non è connesso alcun browser, è possibile avviare una sessione non debug scegliendo il _Visualizza nel Browser_ collegamento:

![browserlink-dashboard-no-connessioni](using-browserlink/_static/browserlink-dashboard-no-connections.png)

In caso contrario, vengono visualizzati i browser connessi, con il percorso della pagina da cui viene visualizzata ogni browser:

![browserlink-dashboard-due connessioni](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Se si desidera, è possibile fare clic sul nome di un browser elencati per aggiornare il browser singolo.

### <a name="enable-or-disable-browser-link"></a>Abilitare o disabilitare il collegamento del Browser

Quando si riabilita collegamento del Browser dopo la disabilitazione, è necessario aggiornare il browser per riconnettersi a essi.

### <a name="enable-or-disable-css-auto-sync"></a>Abilitare o disabilitare la sincronizzazione automatica CSS

Quando è abilitata sincronizzazione automatica CSS, i browser connessi vengono aggiornati automaticamente quando si apporta modifiche al file CSS.

## <a name="how-does-it-work"></a>Funzionamento

Collegamento del browser utilizza SignalR per creare un canale di comunicazione tra Visual Studio e il browser. Quando il collegamento del Browser è abilitato, Visual Studio funziona come un server di SignalR in grado di connettersi a più client (browser). Collegamento del browser registra anche un componente del middleware nella pipeline delle richieste ASP.NET. Il componente inserisce speciali `<script>` riferimenti in ogni richiesta di pagina dal server. È possibile visualizzare i riferimenti a script selezionando **Visualizza origine** nel browser e lo scorrimento alla fine del `<body>` tag al contenuto:

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

I file di origine non vengono modificati. Il componente middleware inserisce i riferimenti a script in modo dinamico. 

Dal momento che il codice lato browser è il supporto di JavaScript, funziona per tutti i browser che supporta SignalR, senza richiedere alcun plug-in browser.
