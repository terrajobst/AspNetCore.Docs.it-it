---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: Uso di ASP.NET MVC con versioni diverse di IIS (c#) | Microsoft Docs
author: microsoft
description: In questa esercitazione descrive come usare ASP.NET MVC e Routing degli URL, con diverse versioni di Internet Information Services. Descrive diverse strategie...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: aa7d00c0f54212d495f48929ed2a453942a1ed7d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835663"
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>Uso di ASP.NET MVC con versioni diverse di IIS (c#)
====================
da [Microsoft](https://github.com/microsoft)

> In questa esercitazione descrive come usare ASP.NET MVC e Routing degli URL, con diverse versioni di Internet Information Services. Illustra diverse strategie per l'utilizzo di ASP.NET MVC con versioni precedenti di IIS, IIS 6.0 e IIS 7.0 (modalità classica).


Il framework ASP.NET MVC dipende dal Routing ASP.NET per indirizzare le richieste del browser per le azioni del controller. Per poter sfruttare i vantaggi di Routing di ASP.NET, potrebbe essere necessario eseguire ulteriori passaggi di configurazione nel server web. Tutto dipende dalla versione di Internet Information Services (IIS) e la modalità per l'applicazione di elaborazione delle richieste.

Ecco un riepilogo delle versioni diverse di IIS:

- IIS 7.0 (modalità integrata) - alcuna configurazione speciale necessaria per usare il Routing di ASP.NET.
- IIS 7.0 (modalità classica): È necessario eseguire alcuna configurazione specifica per il routing ASP.NET.
- IIS 6.0 o seguito - è necessario eseguire alcuna configurazione specifica per il routing ASP.NET.

La versione più recente di IIS è versione 7.5 (in Windows 7). IIS 7 di IIS è incluso con Windows Server 2008 AND VISTA SP1 e versioni successive. È anche possibile installare IIS 7.0 in qualsiasi versione del sistema operativo Vista eccetto Home Basic (vedere [ https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx ](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

IIS 7.0 supporta due modalità per l'elaborazione delle richieste. È possibile usare la modalità integrata o in modalità classica. Non devi eseguire alcuna configurazione speciale quando si utilizza IIS 7.0 in modalità integrata. Tuttavia, è necessario eseguire un'ulteriore configurazione quando si utilizza IIS 7.0 in modalità classica.

Microsoft Windows Server 2003 include IIS 6.0. Quando si usa il sistema operativo Windows Server 2003 non è possibile aggiornare IIS 6.0 a IIS 7.0. Quando si utilizza IIS 6.0, è necessario eseguire ulteriori passaggi di configurazione.

Microsoft Windows XP Professional include IIS 5.1. Quando si utilizza IIS 5.1, è necessario eseguire ulteriori passaggi di configurazione.

Infine, Microsoft Windows 2000 e Microsoft Windows 2000 Professional include IIS 5.0. Quando si utilizza IIS 5.0, è necessario eseguire ulteriori passaggi di configurazione.

## <a name="integrated-versus-classic-mode"></a>Integrato e la modalità classica

IIS 7.0 possa elaborare le richieste utilizzando due modalità di elaborazione richiesta diversi: integrata e classica. La modalità integrata fornisce prestazioni migliori e più funzionalità. La modalità classica è inclusa per ragioni di compatibilità con le versioni precedenti di IIS.

La modalità di elaborazione della richiesta è determinata dal pool di applicazioni. È possibile determinare la modalità di elaborazione viene usata da un'applicazione web specifica determinando il pool di applicazioni associato all'applicazione. Attenersi ai passaggi riportati di seguito.

1. Avviare Gestione Internet Information Services
2. Nella finestra di connessioni, selezionare un'applicazione
3. Nella finestra di azioni, scegliere il **impostazioni di base** collegamento per aprire la finestra di dialogo Modifica applicazione (vedere la figura 1)
4. Prendere nota del pool di applicazioni selezionato.

Per impostazione predefinita, IIS è configurato per supportare due pool di applicazioni: **DefaultAppPool** e **Classic .NET AppPool**. Se DefaultAppPool è selezionata, quindi l'applicazione è in esecuzione in modalità di elaborazione della richiesta integrata. Se Classic .NET AppPool è selezionata, l'applicazione è in esecuzione in modalità di elaborazione della richiesta classico.

[![La finestra di dialogo Nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**Figura 1**: il rilevamento della modalità di elaborazione della richiesta ([fare clic per visualizzare l'immagine con dimensioni normali](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

Si noti che è possibile modificare la modalità di elaborazione delle richieste nella finestra di dialogo Modifica applicazione. Scegliere il pulsante di selezione e modificare il pool di applicazioni associato all'applicazione. Tenere presente che esistono problemi di compatibilità quando si modifica un'applicazione ASP.NET dalla distribuzione classica alla modalità integrata. Per altre informazioni, vedere i seguenti articoli:

- L'aggiornamento di ASP.NET 1.1 a IIS 7.0 in Windows Vista e Windows Server 2008: [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- Integrazione di ASP.NET con IIS 7.0: [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Se un'applicazione ASP.NET utilizza DefaultAppPool, quindi non devi eseguire ulteriori passaggi per ottenere il Routing di ASP.NET (e pertanto ASP.NET MVC) per lavorare. Tuttavia, se l'applicazione ASP.NET è configurato per usare il Classic .NET AppPool quindi continuare a leggere, è necessario eseguire altre operazioni.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Uso di ASP.NET MVC con versioni precedenti di IIS

Se è necessario usare ASP.NET MVC con una versione precedente di IIS a IIS 7.0 o è necessario utilizzare IIS 7.0 in modalità classica, sono disponibili due opzioni. In primo luogo, è possibile modificare la tabella di route per usare le estensioni di file. Ad esempio, anziché richiedere un URL come /Store/Details, si richiede un URL come /Store.aspx/Details.

La seconda opzione consiste nel creare un elemento chiamato un' *mapping di script con caratteri jolly*. Un mapping di script con caratteri jolly consente di eseguire il mapping di tutte le richieste nel framework di ASP.NET.

Se non si ha accesso al server web (ad esempio, ASP.NET MVC dell'applicazione è ospitata da un Provider di servizi Internet) è necessario usare la prima opzione. Se non si vuole modificare l'aspetto degli URL e si ha accesso al server web, è possibile utilizzare la seconda opzione.

Verrà esaminata ogni opzione in modo dettagliato nelle sezioni seguenti.

## <a name="adding-extensions-to-the-route-table"></a>Aggiunta di estensioni per la tabella di Route

Il modo più semplice per ottenere il Routing di ASP.NET per lavorare con le versioni precedenti di IIS consiste nel modificare la tabella di route nel file Global. asax. Il valore predefinito e non modificata file Global. asax nel listato 1 consente di configurare una route denominata la route predefinita.

**Listato 1 - Global. asax (non modificato)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

La route predefinita configurata nel listato 1 consente di accedere agli URL di route con un aspetto simile al seguente:

/ Home/Index

/ Prodotti/dettagli/3

/ Prodotto

Sfortunatamente, le versioni precedenti di IIS non passare tali richieste per il framework ASP.NET. Pertanto, queste richieste non vengono instradate a un controller. Ad esempio, se si esegue una richiesta del browser per l'URL avremo/indice si verrà visualizzata la pagina di errore nella figura 2.

[![La finestra di dialogo Nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**Figura 2**: ricezione di un errore 404 non trovato ([fare clic per visualizzare l'immagine con dimensioni normali](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

Le versioni precedenti di IIS eseguire il mapping solo determinate richieste per il framework ASP.NET. La richiesta deve essere un URL con l'estensione di file a destra. Ad esempio, una richiesta per /SomePage.aspx viene mappata al framework di ASP.NET. Tuttavia, una richiesta per /SomePage.htm non lo consente.

Pertanto, per ottenere il Routing di ASP.NET per lavorare, è necessario modificare la route predefinita in modo che includa un'estensione di file che viene eseguito il mapping per il framework ASP.NET.

Questa operazione viene eseguita tramite uno script denominato `registermvc.wsf`. È stato incluso nella versione 1 di ASP.NET MVC nel `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, ma a partire da ASP.NET 2 in questo script è stato spostato in ASP.NET Futures, disponibile all'indirizzo [ http://aspnet.codeplex.com/releases/view/39978 ](http://aspnet.codeplex.com/releases/view/39978).

Eseguire lo script registra una nuova estensione di MVC con IIS. Dopo aver registrato l'estensione di MVC, è possibile modificare le route nel file Global. asax in modo che le route di usano l'estensione di MVC.

Il file Global. asax modificato nel listato 2 funziona con le versioni precedenti di IIS.

**Listato 2 - Global. asax (modificato con le estensioni)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**Importante**: ricordare di compilare l'applicazione ASP.NET MVC dopo aver modificato il file Global. asax.

Esistono due importanti modifiche nel file Global. asax nel listato 2. Sono ora disponibili due route definite in Global. asax. Simile a questo punto il modello di URL per la route predefinita, la prima route:

{controller}.mvc/{action}/{id}

L'aggiunta dell'estensione MVC modifica il tipo di file che intercetta il modulo di Routing di ASP.NET. Con questa modifica, a questo punto l'applicazione ASP.NET MVC indirizza le richieste simile al seguente:

/Home.mvc/Index/

/Product.Mvc/Details/3

/Product.Mvc/

La seconda route, la route radice, è una novità. Questo modello di URL per la route radice è una stringa vuota. Questa route è necessaria per la corrispondenza di richieste effettuate per la radice dell'applicazione. Ad esempio, la route radice corrisponderà una richiesta simile alla seguente:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Dopo aver apportato queste modifiche alla tabella di route, è necessario assicurarsi che tutti i collegamenti nell'applicazione siano compatibili con questi nuovi modelli di URL. In altre parole, assicurarsi che tutti i collegamenti includere l'estensione di MVC. Se si usa il metodo helper Html.ActionLink() per generare i collegamenti, quindi non dovrebbe essere necessario apportare alcuna modifica.

Invece di usare lo script registermvc.wcf, è possibile aggiungere una nuova estensione per IIS in cui viene eseguito il mapping per il framework ASP.NET manualmente. Quando si aggiunge una nuova estensione, assicurarsi che la casella di controllo etichettato **verifica esistenza del file** non è selezionata.

## <a name="hosted-server"></a>Server ospitato

Sempre autorizzati ad accedere al server web. Ad esempio, se si ospita l'applicazione ASP.NET MVC Usa un Provider di Hosting Internet, quindi si necessariamente avrà accesso a IIS.

In tal caso, è necessario usare una delle estensioni di file esistenti che vengono mappate ai framework di ASP.NET. Sono esempi di estensioni di file mappate ad ASP.NET con estensione aspx, axd e. ashx estensioni.

Ad esempio, il file Global. asax modificato nel listato 3 Usa l'estensione aspx anziché l'estensione di MVC.

**Listato 3 - Global. asax (modificato con estensioni aspx)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

Il file Global. asax nel listato 3 è esattamente lo stesso di quello del file Global. asax precedente ad eccezione del fatto che usa l'estensione aspx anziché l'estensione di MVC. Si è autorizzati a eseguire qualsiasi programma di installazione nel server web remoto per usare l'estensione aspx.

## <a name="creating-a-wildcard-script-map"></a>Creazione di una mappa di Script con caratteri jolly

Se non si vuole modificare l'URL per l'applicazione ASP.NET MVC e si ha accesso al server web, è presente un'opzione aggiuntiva. È possibile creare un mapping di script con caratteri jolly che esegue il mapping di tutte le richieste al server web per il framework ASP.NET. In questo modo, è possibile utilizzare l'impostazione predefinita la tabella di route MVC ASP.NET con IIS 7.0 (in modalità classica) o IIS 6.0.

Tenere presente che questa opzione consente di intercettare ogni richiesta effettuata per il server web IIS. Questo include le richieste per le immagini, le pagine ASP classiche e pagine HTML. Pertanto, l'abilitazione di un carattere jolly mapping di script ad ASP.NET avere implicazioni sulle prestazioni.

Ecco la procedura per attivare un mapping di script con caratteri jolly per IIS 7.0:

1. Selezionare l'applicazione nella finestra di connessioni
2. Assicurarsi che il **funzionalità** è selezionata la visualizzazione
3. Fare doppio clic il **Mapping gestori** pulsante
4. Scegliere il **Aggiungi mapping di Script con caratteri jolly** link (vedere la figura 3)
5. Immettere il percorso di aspnet\_ISAPI. dll file (è possibile copiare questo percorso dalla mappa PageHandlerFactory script)
6. Immettere il nome MVC
7. Scegliere il **OK** pulsante

[![La finestra di dialogo Nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**Figura 3**: la creazione di un mapping di script con caratteri jolly con IIS 7.0 ([fare clic per visualizzare l'immagine con dimensioni normali](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

Seguire questi passaggi per creare un mapping di script con caratteri jolly con IIS 6.0:

1. Fare doppio clic su un sito Web e selezionare proprietà
2. Selezionare il **Home Directory** scheda
3. Scegliere il **configurazione** pulsante
4. Selezionare il **mapping** scheda
5. Scegliere il **Inserisci** pulsante (vedere la figura 4)
6. Incollare il percorso aspnet\_ISAPI. dll nel campo eseguibile (è possibile copiare questo percorso dal mapping di script per i file con estensione aspx)
7. Deselezionare la casella di controllo **verifica esistenza del file**
8. Scegliere il **OK** pulsante

[![La finestra di dialogo Nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**Figura 4**: la creazione di un mapping di script con caratteri jolly con IIS 6.0 ([fare clic per visualizzare l'immagine con dimensioni normali](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

Dopo aver abilitato il mapping di script con caratteri jolly, è necessario modificare la tabella di route nel file Global. asax in modo che includa una route radice. In caso contrario, si otterrà la pagina di errore nella figura 5 quando si effettua una richiesta per la pagina principale dell'applicazione. È possibile usare il file Global. asax modificato nel listato 4.

[![La finestra di dialogo Nuovo progetto](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**Figura 5**: errore di route radice mancante ([fare clic per visualizzare l'immagine con dimensioni normali](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**Listato 4 - Global. asax (modificato con la route radice)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

Dopo aver abilitato un mapping di script con caratteri jolly per IIS 7.0 o IIS 6.0, è possibile effettuare richieste che funzionano con la tabella di route predefinito con un aspetto simile al seguente:

/

/ Home/Index

/ Prodotti/dettagli/3

/ Prodotto

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è stata per spiegare come usare ASP.NET MVC quando si usa una versione precedente di IIS (o IIS 7.0 in modalità classica). Abbiamo parlato di due metodi per ottenere il Routing di ASP.NET per lavorare con le versioni precedenti di IIS: modificare la tabella di route predefinite o creare un mapping di script con caratteri jolly.

La prima opzione richiede di modificare gli URL usati nell'applicazione ASP.NET MVC. Uno dei vantaggi molto significativo di questa opzione prima è che non è necessario l'accesso a un server web per modificare la tabella di route. Ciò significa che è possibile usare questa opzione prima anche quando si ospita l'applicazione ASP.NET MVC con un Internet società di hosting.

La seconda opzione consiste nel creare un mapping di script con caratteri jolly. Il vantaggio di questa seconda opzione è che non occorre modificare gli URL. Lo svantaggio di questa seconda opzione è che può ridurre le prestazioni dell'applicazione ASP.NET MVC.

> [!div class="step-by-step"]
> [avanti](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
