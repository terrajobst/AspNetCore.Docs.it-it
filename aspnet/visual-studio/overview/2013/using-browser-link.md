---
uid: visual-studio/overview/2013/using-browser-link
title: Utilizzo di collegamento del Browser in Visual Studio 2013 | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: e5a13405a303580ec8c1d4cdacafc26c6f8ff34a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044110"
---
<a name="using-browser-link-in-visual-studio-2013"></a>Utilizzo di collegamento del Browser in Visual Studio 2013
====================
da [Mike Wasson](https://github.com/MikeWasson)

Collegamento del browser è una nuova funzionalità di Visual Studio 2013 che crea un canale di comunicazione tra l'ambiente di sviluppo e uno o più web browser. È possibile utilizzare il collegamento del Browser per aggiornare l'applicazione web nel browser diverse contemporaneamente, che è utile per il testing e browser.

- [Aggiornamento del browser](#browser-refresh)
- [Visualizzazione Dashboard del collegamento Browser](#dashboard)
- [Abilitando il collegamento del Browser per i file HTML statico](#static-html)
- [La disabilitazione di collegamento del Browser](#disabling)
- [Come funziona?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Aggiornamento del browser

Con l'aggiornamento del Browser, è possibile aggiornare più browser connessi a Visual Studio tramite il collegamento del Browser.

Per utilizzare l'aggiornamento del Browser, creare innanzitutto un'applicazione ASP.NET, utilizzando uno dei modelli di progetto. Eseguire il debug dell'applicazione premendo F5 o facendo clic sull'icona della freccia sulla barra degli strumenti:

![](using-browser-link/_static/image1.png)

È anche possibile utilizzare l'elenco a discesa per selezionare un browser specifico per il debug.

![](using-browser-link/_static/image2.png)

Per eseguire il debug con più browser, selezionare **Esplora con**. Nel **Esplora con** finestra di dialogo, tenere premuto il tasto CTRL per selezionare più di un browser. Fare clic su **Sfoglia** per eseguire il debug con i browser selezionati. Collegamento del browser funziona anche se si avvia un browser dall'esterno di Visual Studio e accedere all'URL dell'applicazione.

![](using-browser-link/_static/image3.png)

I controlli collegamento Browser si trovano nell'elenco a discesa con l'icona freccia circolare. L'icona freccia è il **aggiornamento** pulsante.

![](using-browser-link/_static/image4.png)

Per vedere quali browser connessi, posizionare il mouse sopra il **aggiornamento** pulsante durante il debug. I browser connessi vengono visualizzati in una finestra della descrizione comando.

![](using-browser-link/_static/image5.png)

Per aggiornare il browser connesso, fare clic su di **aggiornamento** oppure premere CTRL + ALT + INVIO. Ad esempio, nella schermata seguente mostra un progetto ASP.NET, che è stato creato utilizzando il modello di progetto MVC 5. È possibile visualizzare l'applicazione in esecuzione nei due browser nella parte superiore. Nella parte inferiore, il progetto è aperto in Visual Studio.

![](using-browser-link/_static/image6.png)

In Visual Studio, in seguito alla modifica di &lt;h1&gt; titolo per la home page:

![](using-browser-link/_static/image7.png)

Quando fa clic su di **aggiornamento** pulsante, la modifica è presente in entrambe le finestre del browser:

![](using-browser-link/_static/image8.png)

**Note**

- Per abilitare il collegamento del Browser, impostare `debug=true` nel [ &lt;compilazione&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) elemento nel file Web. config del progetto.
- L'applicazione deve essere in esecuzione in localhost.
- L'applicazione deve avere come destinazione .NET 4.0 o versione successiva.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Visualizzazione Dashboard del collegamento Browser

Il dashboard di collegamento del Browser Mostra informazioni sulle connessioni di collegamento del Browser. Per visualizzare il dashboard, selezionare il menu a discesa di collegamento del Browser (la piccola freccia accanto al **aggiornamento** pulsante). Quindi fare clic su **Dashboard del collegamento Browser**.

![](using-browser-link/_static/image9.png)

I dashboard sono elencati i browser connessi e l'URL a cui si è spostato ogni browser.

![](using-browser-link/_static/image10.png)

Il **prerequisiti** sezione Mostra tutti i passaggi necessari per abilitare il collegamento del Browser per il progetto. Ad esempio, nella schermata seguente viene illustrato un progetto in cui "debug" è impostato su false nel file Web. config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Abilitando il collegamento del Browser per i file HTML statico

Per abilitare il collegamento del Browser per i file HTML statici, aggiungere quanto segue al file Web. config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Per motivi di prestazioni, è possibile rimuovere questa impostazione quando si pubblica il progetto.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>La disabilitazione di collegamento del Browser

Collegamento del browser è abilitata per impostazione predefinita. Esistono diversi modi per disabilitare la funzionalità:

- Nel menu a discesa collegamento del Browser, deselezionare **abilitare collegamento Browser**. 

    ![](using-browser-link/_static/image12.png)
- Nel file Web. config, aggiungere una chiave denominata "vs: EnableBrowserLink" con valore "false" nella sezione appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- Nel file Web. config impostare debug su false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Come funziona?

Collegamento del browser utilizza [SignalR](../../../signalr/index.md) per creare un canale di comunicazione tra Visual Studio e il browser. Quando il collegamento del Browser è abilitato, Visual Studio funziona come un server di SignalR in grado di connettersi a più client (browser). Collegamento del browser registra anche un modulo HTTP con ASP.NET. Questo modulo inserisce speciali &lt;script&gt; riferimenti in ogni richiesta di pagina dal server. È possibile visualizzare i riferimenti a script selezionando "HTML" nel browser.

![](using-browser-link/_static/image13.png)

I file di origine non vengono modificati. Il modulo HTTP inserisce i riferimenti a script in modo dinamico.

Dal momento che il codice lato browser è il supporto di JavaScript, funziona per tutti i browser che [SignalR supporta](../../../signalr/overview/getting-started/supported-platforms.md), senza richiedere alcun plug-in browser.
