---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: Utilizzo di ASP.NET MVC con versioni diverse di IIS (c#) | Documenti Microsoft
author: microsoft
description: In questa esercitazione imparare a usare ASP.NET MVC e Routing degli URL, con diverse versioni di Internet Information Services. Informazioni su diverse strategie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: fdd024aba399f26e9ef7d01a00078cd3d5750d94
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>Utilizzo di ASP.NET MVC con versioni diverse di IIS (c#)
====================
da [Microsoft](https://github.com/microsoft)

> In questa esercitazione imparare a usare ASP.NET MVC e Routing degli URL, con diverse versioni di Internet Information Services. Informazioni su diverse strategie per l'utilizzo di MVC ASP.NET con IIS 7.0 (modalità classica), IIS 6.0 e versioni precedenti di IIS.


Il framework di MVC ASP.NET dipende dal Routing ASP.NET per indirizzare le richieste del browser per le azioni del controller. Per sfruttare il Routing di ASP.NET, si potrebbe essere necessario eseguire ulteriori passaggi di configurazione sul server web. Tutto dipende dalla versione di Internet Information Services (IIS) e la modalità per l'applicazione di elaborazione delle richieste.

Di seguito è riportato un riepilogo delle diverse versioni di IIS:

- IIS 7.0 (modalità integrata): nessuna configurazione speciale necessaria utilizzare il Routing di ASP.NET.
- IIS 7.0 (modalità classica): È necessario eseguire la configurazione speciale per il routing ASP.NET.
- IIS 6.0 o di seguito, è necessario eseguire la configurazione speciale per il routing ASP.NET.

La versione più recente di IIS è versione 7.5 (Win7). IIS 7 di IIS è incluso con Windows Server 2008 e VISTA SP1 e versioni successive. È anche possibile installare IIS 7.0 in qualsiasi versione del sistema operativo Vista eccetto Home Basic (vedere [https://technet.microsoft.com/en-us/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/en-us/library/cc731179%28WS.10%29.aspx)).

IIS 7.0 supporta due modalità per l'elaborazione delle richieste. È possibile utilizzare la modalità integrata o in modalità classica. Non è necessario eseguire alcuna procedura di configurazione speciali quando si utilizza IIS 7.0 in modalità integrata. Tuttavia, è necessario quando si usa IIS 7.0 in modalità classica, eseguire una configurazione aggiuntiva.

Microsoft Windows Server 2003 include IIS 6.0. Quando si utilizza il sistema operativo Windows Server 2003, è possibile aggiornare IIS 6.0 a IIS 7.0. Quando si utilizza IIS 6.0, è necessario eseguire ulteriori passaggi di configurazione.

Microsoft Windows XP Professional include IIS 5.1. Quando si utilizza IIS 5.1, è necessario eseguire ulteriori passaggi di configurazione.

Infine, Microsoft Windows 2000 e Microsoft Windows 2000 Professional include IIS 5.0. Quando si utilizza IIS 5.0, è necessario eseguire ulteriori passaggi di configurazione.

## <a name="integrated-versus-classic-mode"></a>Integrato e la modalità classica

IIS 7.0 può elaborare le richieste utilizzando due modalità di elaborazione diverso richiesta: classica e integrata. La modalità integrata fornisce prestazioni migliori e altre funzionalità. La modalità classica è inclusa per ragioni di compatibilità con le versioni precedenti di IIS.

La modalità di elaborazione della richiesta è determinata dal pool di applicazioni. È possibile determinare la modalità di elaborazione è usata da un'applicazione web particolare determinando il pool di applicazioni associato all'applicazione. Attenersi ai passaggi riportati di seguito.

1. Avviare Gestione Internet Information Services
2. Nella finestra di connessioni, selezionare un'applicazione
3. Nella finestra di azioni, fare clic su di **le impostazioni di base** collegamento per aprire la finestra di dialogo Modifica applicazione (vedere Figura 1)
4. Prendere nota del pool di applicazioni selezionato.

Per impostazione predefinita, IIS è configurato per supportare due pool di applicazioni: **DefaultAppPool** e **Classic .NET AppPool**. Se DefaultAppPool è selezionata, l'applicazione è in esecuzione in modalità di elaborazione della richiesta integrata. Se Classic .NET AppPool è selezionata, l'applicazione è in esecuzione in modalità di elaborazione della richiesta classico.

[![La finestra di dialogo Nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**Figura 1**: la modalità di elaborazione della richiesta di rilevamento ([fare clic per visualizzare l'immagine ingrandita](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

Si noti che è possibile modificare la modalità di elaborazione della richiesta nella finestra di dialogo Modifica applicazione. Fare clic sul pulsante Seleziona e modificare il pool di applicazioni associato all'applicazione. Tenere presente che non esistono problemi di compatibilità se si modifica un'applicazione ASP.NET dalla versione classica alla modalità integrata. Per altre informazioni, vedere i seguenti articoli:

- L'aggiornamento di ASP.NET 1.1 a IIS 7.0 in Windows Vista e Windows Server 2008 - [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- Integrazione di ASP.NET con IIS 7.0 - [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Se un'applicazione ASP.NET utilizza DefaultAppPool, non necessario eseguire operazioni aggiuntive per ottenere il Routing di ASP.NET (e pertanto ASP.NET MVC) per lavorare. Tuttavia, se l'applicazione ASP.NET è configurata per utilizzare il Classic .NET AppPool quindi continuare la lezione, è necessario eseguire altre operazioni.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Utilizzo di ASP.NET MVC con versioni precedenti di IIS

Se è necessario usare ASP.NET MVC con una versione precedente di IIS a IIS 7.0, oppure è necessario utilizzare IIS 7.0 in modalità classica, sono disponibili due opzioni. In primo luogo, è possibile modificare la tabella di route per l'utilizzo di estensioni di file. Ad esempio, anziché richiedere un URL come /Store/Details, si richiede un URL come /Store.aspx/Details.

La seconda opzione consiste nel creare il cosiddetto un *mapping di script con caratteri jolly*. Un mapping di script con caratteri jolly consente di eseguire il mapping di ogni richiesta, nel framework di ASP.NET.

Se non si dispone di accesso al server web (ad esempio, ASP.NET MVC è ospitata l'applicazione da un Provider di servizi Internet) è necessario utilizzare la prima opzione. Se non si desidera modificare l'aspetto degli URL e avere accesso al server web, è possibile utilizzare la seconda opzione.

Vengono illustrati in dettaglio nelle sezioni seguenti ogni opzione.

## <a name="adding-extensions-to-the-route-table"></a>Aggiunta di estensioni per la tabella di Route

Il modo più semplice per ottenere il Routing di ASP.NET per funzionare con le versioni precedenti di IIS è per modificare la tabella di routing nel file Global. asax. Il valore predefinito e il file Global. asax non modificato nel listato 1 consente di configurare una route denominata la route predefinita.

**Elenco 1 - Global. asax (non modificato)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

La route predefinita configurata nel listato 1 consente di accedere agli URL di route che è simile al seguente:

/ Home/Index

/ Prodotti/dettagli/3

/ Prodotto

Purtroppo, le versioni precedenti di IIS non passare queste richieste per il framework ASP.NET. Pertanto, queste richieste non instradate a un controller. Ad esempio, se si apporta una richiesta del browser per l'URL /Home/indice quindi si otterrà la pagina di errore nella figura 2.

[![La finestra di dialogo Nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**Figura 2**: ricezione di un errore 404 non trovato ([fare clic per visualizzare l'immagine ingrandita](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

Le versioni precedenti di IIS eseguire il mapping solo alcune richieste per il framework ASP.NET. La richiesta deve essere un URL con l'estensione di file corretti. Ad esempio, una richiesta per /SomePage.aspx viene eseguito il mapping per il framework ASP.NET. Tuttavia, una richiesta per /SomePage.htm non lo consente.

Pertanto, per ottenere il Routing di ASP.NET per funzionare, è necessario modificare la route predefinita in modo che includa un'estensione di file che viene eseguito il mapping per il framework ASP.NET.

Questa operazione viene eseguita tramite uno script denominato `registermvc.wsf`. È stato incluso nella versione 1 di ASP.NET MVC in `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, ma a partire da ASP.NET 2 questo script è stato spostato il ritardo di ASP.NET, disponibile all'indirizzo [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978).

Eseguire questo script per registrare una nuova estensione MVC con IIS. Dopo aver registrato l'estensione di MVC, è possibile modificare i percorsi nel file Global. asax in modo che le route utilizzano l'estensione per MVC.

Il file Global. asax modificato nel listato 2 funziona con le versioni precedenti di IIS.

**Elenco di 2 - Global. asax (modificato con estensioni)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**Importante**: è necessario compilare l'applicazione ASP.NET MVC dopo aver modificato il file Global. asax.

Esistono due importanti modifiche al file Global. asax listato 2. Esistono due route definite in Global. asax. Modello di URL per la route predefinita, il primo ciclo, è ora simile:

{controller}.mvc/{action}/{id}

L'aggiunta dell'estensione MVC modifica il tipo di file che intercetta il modulo di Routing di ASP.NET. Con questa modifica, l'applicazione ASP.NET MVC ora instrada le richieste simile al seguente:

/Home.Mvc/index/

/Product.Mvc/Details/3

/Product.Mvc/

La seconda route, la route di radice, è una novità. Questo modello di URL per la route radice è una stringa vuota. La route è necessaria per la corrispondenza delle richieste effettuate per la radice dell'applicazione. Ad esempio, la route radice corrisponderà a una richiesta simile al seguente:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Dopo aver apportato queste modifiche alla tabella di route, è necessario assicurarsi che tutti i collegamenti nell'applicazione siano compatibili con questi nuovi modelli di URL. In altre parole, assicurarsi che tutti i collegamenti includere l'estensione di MVC. Se si utilizza il metodo di supporto Html.ActionLink() per generare i collegamenti, quindi non è necessario apportare modifiche.

Anziché utilizzare lo script registermvc.wcf, è possibile aggiungere una nuova estensione per IIS in cui viene eseguito il mapping per il framework ASP.NET manualmente. Quando si aggiunge una nuova estensione, assicurarsi che la casella di controllo con etichetta **verifica esistenza del file** non è selezionata.

## <a name="hosted-server"></a>Server di hosting

Non è sempre necessario l'accesso al server web. Ad esempio, se si ospita l'applicazione MVC ASP.NET utilizzando un Provider di Hosting Internet, quindi si necessariamente avrà accesso a IIS.

In tal caso, utilizzare una delle estensioni di file esistente che vengono eseguito il mapping per il framework ASP.NET. L'estensione aspx, axd e. ashx estensioni sono esempi di estensioni di file mappate ad ASP.NET.

Ad esempio, il file Global. asax modificato nel listato 3 utilizza l'estensione aspx anziché dell'estensione di MVC.

**Elenco di 3 - Global. asax (modificato con estensioni aspx)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

Il file Global. asax listato 3 è esattamente uguale a quello del file Global. asax precedente, ad eccezione del fatto che usa l'estensione aspx anziché dell'estensione di MVC. Non è necessario eseguire alcuna installazione sul server web remoto per utilizzare l'estensione aspx.

## <a name="creating-a-wildcard-script-map"></a>Creazione di una mappa di Script con caratteri jolly

Se non si desidera modificare gli URL per l'applicazione ASP.NET MVC e avere accesso al server web, presente un'opzione aggiuntiva. È possibile creare un mapping di script con caratteri jolly che esegue il mapping di tutte le richieste al server web per il framework ASP.NET. In questo modo, è possibile utilizzare il valore predefinito di tabella di routing di ASP.NET MVC con IIS 7.0 (in modalità classica) o IIS 6.0.

Tenere presente che questa opzione consente di intercettare ogni richiesta effettuata per il server web IIS. Questo include le richieste per le immagini, le pagine ASP classiche e pagine HTML. Pertanto, l'abilitazione di un carattere jolly scriptmap di ASP.NET implicazioni sulle prestazioni.

Ecco la procedura per attivare un mapping di script con caratteri jolly per IIS 7.0:

1. Selezionare l'applicazione nella finestra di connessioni
2. Assicurarsi che il **funzionalità** è selezionata la visualizzazione
3. Fare doppio clic su di **Mapping gestori** pulsante
4. Fare clic su di **Aggiungi mapping di Script con caratteri jolly** collegamento (vedere la figura 3)
5. Immettere il percorso di aspnet\_file ISAPI. dll (è possibile copiare questo percorso dalla mappa PageHandlerFactory script)
6. Immettere il nome MVC
7. Fare clic su di **OK** pulsante

[![La finestra di dialogo Nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**Figura 3**: creazione di un mapping di script con caratteri jolly con IIS 7.0 ([fare clic per visualizzare l'immagine ingrandita](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

Seguire questi passaggi per creare un mapping di script con caratteri jolly con IIS 6.0:

1. Fare doppio clic su un sito Web e selezionare proprietà
2. Selezionare il **Home Directory** scheda
3. Fare clic su di **configurazione** pulsante
4. Selezionare il **mapping** scheda
5. Fare clic su di **inserire** pulsante (vedere la figura 4)
6. Incollare il percorso di aspnet\_ISAPI. dll nel campo eseguibile (è possibile copiare questo percorso di mapping di script per i file con estensione aspx)
7. Deselezionare la casella di controllo con etichettata **verifica esistenza del file**
8. Fare clic su di **OK** pulsante

[![La finestra di dialogo Nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**Figura 4**: creazione di un mapping di script con caratteri jolly con IIS 6.0 ([fare clic per visualizzare l'immagine ingrandita](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

Dopo aver abilitato il mapping di script con caratteri jolly, è necessario modificare la tabella di route nel file Global. asax in modo che includa una route di radice. In caso contrario, la pagina di errore viene visualizzato nella figura 5, quando si effettua una richiesta per la pagina principale dell'applicazione. È possibile utilizzare il file Global. asax modificato listato 4.

[![La finestra di dialogo Nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**Figura 5**: errore di route principale mancante ([fare clic per visualizzare l'immagine ingrandita](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**Elenco di 4 - Global. asax (modificato con route radice)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

Dopo aver abilitato un mapping di script con caratteri jolly per IIS 7.0 o IIS 6.0, è possibile effettuare richieste che utilizzano la tabella di route predefinita che è simile al seguente:

/

/ Home/Index

/ Prodotti/dettagli/3

/ Prodotto

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è illustrare come è possibile utilizzare ASP.NET MVC quando si utilizza una versione precedente di IIS (o IIS 7.0 in modalità classica). Esaminati i due metodi di acquisizione di Routing di ASP.NET per funzionare con le versioni precedenti di IIS: modificare la tabella di route predefinita oppure creare un mapping di script con caratteri jolly.

La prima opzione richiede di modificare gli URL usati nell'applicazione ASP.NET MVC. Un vantaggio significativo prima di questa opzione è che non è necessario accedere a un server web per modificare la tabella di route. Ciò significa che, anche quando si ospita l'applicazione ASP.NET MVC con un Internet, è possibile utilizzare questa opzione prima società di hosting.

La seconda opzione consiste nel creare un mapping di script con caratteri jolly. Il vantaggio di questa seconda opzione è che non è necessario modificare gli URL. Lo svantaggio di questa seconda opzione consiste nel fatto che può compromettere le prestazioni dell'applicazione ASP.NET MVC.

>[!div class="step-by-step"]
[Successivo](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
