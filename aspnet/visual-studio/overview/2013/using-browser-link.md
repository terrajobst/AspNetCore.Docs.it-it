---
uid: visual-studio/overview/2013/using-browser-link
title: Uso di Browser Link in Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 9da93c279bfa2af614733e3234ba62429abf688a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822195"
---
<a name="using-browser-link-in-visual-studio-2013"></a>Uso di Browser Link in Visual Studio 2013
====================
da [Mike Wasson](https://github.com/MikeWasson)

Collegamento del browser è una nuova funzionalità di Visual Studio 2013 che consente di creare un canale di comunicazione tra l'ambiente di sviluppo e uno o più web browser. È possibile usare il collegamento del Browser per aggiornare l'applicazione web in diversi browser in una sola volta, è utile per i test tra browser.

- [Aggiornamento del browser](#browser-refresh)
- [Visualizzazione Dashboard del collegamento Browser](#dashboard)
- [Abilitazione di collegamento del Browser per i file di codice HTML statico](#static-html)
- [La disabilitazione di collegamento del Browser](#disabling)
- [Come funziona?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Aggiornamento del browser

Con l'aggiornamento del Browser, è possibile aggiornare più browser connessi a Visual Studio tramite il collegamento del Browser.

Per usare l'aggiornamento del Browser, creare innanzitutto un'applicazione ASP.NET, usando uno dei modelli di progetto. Eseguire il debug dell'applicazione premendo F5 o facendo clic sull'icona della freccia sulla barra degli strumenti:

![](using-browser-link/_static/image1.png)

È anche possibile usare l'elenco a discesa per selezionare un browser specifico per il debug.

![](using-browser-link/_static/image2.png)

Per eseguire il debug con più browser, selezionare **Esplora con**. Nel **Esplora con** finestra di dialogo, tenere premuto il tasto CTRL per selezionare più di un browser. Fare clic su **esplorare** per eseguire il debug con i browser selezionati. Collegamento del browser funziona anche se si avvia un browser da esterno a Visual Studio e passare all'URL dell'applicazione.

![](using-browser-link/_static/image3.png)

I controlli collegamento del Browser si trovano nell'elenco a discesa con l'icona di freccia circolare. L'icona della freccia è il **Aggiorna** pulsante.

![](using-browser-link/_static/image4.png)

Per vedere quali browser sono connessi, posizionare il mouse sopra il **Aggiorna** pulsante durante il debug. Il browser connessi vengono visualizzati in una finestra della descrizione comando.

![](using-browser-link/_static/image5.png)

Per aggiornare il browser connessi, fare clic sui **Aggiorna** oppure premere CTRL + ALT + INVIO. Lo screenshot seguente mostra ad esempio, un progetto ASP.NET, che è stato creato usando il modello di progetto MVC 5. È possibile visualizzare l'applicazione in esecuzione nei due browser nella parte superiore. Nella parte inferiore, il progetto è aperto in Visual Studio.

![](using-browser-link/_static/image6.png)

In Visual Studio dopo aver modificato il &lt;h1&gt; intestazione per la home page:

![](using-browser-link/_static/image7.png)

Quando fa clic sui **Aggiorna** pulsante, la modifica è presente in entrambe le finestre del browser:

![](using-browser-link/_static/image8.png)

**Note**

- Per abilitare il collegamento del Browser, impostare `debug=true` nella [ &lt;compilazione&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) elemento nel file Web. config del progetto.
- L'applicazione deve essere in esecuzione in localhost.
- L'applicazione deve avere come destinazione .NET 4.0 o versione successiva.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Visualizzazione Dashboard del collegamento Browser

Il dashboard di collegamento del Browser Mostra informazioni sulle connessioni collegamento del Browser. Per visualizzare il dashboard, selezionare il menu a discesa di collegamento del Browser (la piccola freccia avanti per la **Aggiorna** pulsante). Quindi fare clic su **Dashboard Browser Link**.

![](using-browser-link/_static/image9.png)

Il dashboard Elenca i browser connessi e l'URL a cui si è spostato ogni browser.

![](using-browser-link/_static/image10.png)

Il **prerequisiti** sezione Mostra tutti i passaggi necessari per abilitare il collegamento del Browser per il progetto. Lo screenshot seguente mostra ad esempio, un progetto in cui "debug" è impostato su false nel file Web. config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Abilitazione di collegamento del Browser per i file di codice HTML statico

Per abilitare il collegamento del Browser per i file HTML statici, aggiungere quanto segue al file Web. config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Per motivi di prestazioni, rimuovere questa impostazione quando si pubblica il progetto.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>La disabilitazione di collegamento del Browser

Collegamento del browser è abilitato per impostazione predefinita. Esistono diversi modi per disabilitarlo:

- Nel menu a discesa collegamento del Browser, deselezionare **Abilita il collegamento del Browser**. 

    ![](using-browser-link/_static/image12.png)
- Aggiungere una chiave denominata "vs: EnableBrowserLink" con il valore "false" nella sezione appSettings nel file Web. config. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- Nel file Web. config, impostare debug su false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Come funziona?

Usa il collegamento del browser [SignalR](../../../signalr/index.md) per creare un canale di comunicazione tra Visual Studio e il browser. Quando è abilitato il collegamento del Browser, Visual Studio funge da un server di SignalR in grado di connettersi a più client (browser). Collegamento del browser registra inoltre un modulo HTTP con ASP.NET. Questo modulo inserisce speciali &lt;script&gt; riferimenti in ogni richiesta di pagina dal server. È possibile visualizzare i riferimenti a script selezionando "HTML" nel browser.

![](using-browser-link/_static/image13.png)

I file di origine non vengono modificati. Il modulo HTTP inserisce in modo dinamico i riferimenti a script.

Dal momento che il codice del browser-side è il supporto di JavaScript, funziona in tutti i browser che [SignalR supporta](../../../signalr/overview/getting-started/supported-platforms.md), senza richiedere alcun plug-in del browser.
