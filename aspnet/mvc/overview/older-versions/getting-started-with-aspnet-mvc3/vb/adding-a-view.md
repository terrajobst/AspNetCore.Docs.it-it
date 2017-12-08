---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Aggiunta di una vista (VB) | Documenti Microsoft
author: Rick-Anderson
description: "In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7e8564c743510780b93d56bc1215f4c5b1faeb43
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view-vb"></a>Aggiunta di una vista (VB)
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)
> 
> Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente VB.NET complemento è disponibile in questo argomento. [Scaricare la versione di Visual Basic.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce c#, passare il [c# versione](../cs/adding-a-view.md) di questa esercitazione.


In questa sezione si intende modificare il `HelloWorldController` incapsulare di classe per utilizzare un file di modello di visualizzazione per correttamente il processo di generazione di risposte HTML a un client.

Iniziamo con un modello di visualizzazione con il `Index` metodo la `HelloWorldController` classe. Attualmente il `Index` metodo restituisce una stringa con un messaggio a livello di codice all'interno della classe controller. Modifica il `Index` per restituire un `View` dell'oggetto, come illustrato nell'esempio seguente:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Aggiungere ora un modello di visualizzazione per il progetto che è possibile richiamare con il `Index` metodo. A tale scopo, fare doppio clic all'interno di `Index` (metodo) e fare clic su **Aggiungi visualizzazione**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

Il **Aggiungi visualizzazione** viene visualizzata la finestra di dialogo. Lasciare le voci predefinite, quindi scegliere il **Aggiungi** pulsante.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

Il *MvcMovie\Views\HelloWorld* cartella e *MvcMovie\Views\HelloWorld\Index.vbhtml* vengono creati i file. È possibile visualizzarli in **Esplora**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Aggiungere il codice HTML con il `<h2>` tag. Modificato *MvcMovie\Views\HelloWorld\Index.vbhtml* file è illustrato di seguito.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Eseguire l'applicazione e individuare il &quot;HelloWorld&quot; controller (`http://localhost:xxxx/HelloWorld`). Il `Index` metodo nel controller di non eseguire la quantità di lavoro, sufficiente eseguita l'istruzione `return View()`, per indicare che si desidera utilizzare un file di modello di visualizzazione per il rendering di una risposta al client. Perché non è stato specificato il nome del file modello di visualizzazione da utilizzare in modo esplicito, ASP.NET MVC impostato sul valore predefinito utilizzando il *Index.vbhtml* Visualizza file all'interno di *\Views\HelloWorld* cartella. L'immagine seguente mostra la stringa hardcoded nella vista.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

È buona. Si noti tuttavia che barra del titolo del browser indica &quot;indice&quot; e il titolo della pagina big afferma &quot;applicazione MVC.&quot; È necessario modificare quelle.

## <a name="changing-views-and-layout-pages"></a>Modifica delle viste e delle pagine di layout

In primo luogo, è necessario modificare il testo &quot;applicazione MVC.&quot; Tale testo è condivisa e verrà visualizzato in ogni pagina. In realtà solo in una posizione nel progetto, anche se è presente in ogni pagina nell'applicazione. Passare al */visualizzazioni/Shared* cartella **Esplora** e aprire il  *\_Layout.vbhtml* file. Questo file è denominato a una pagina di layout ed è condiviso &quot;shell&quot; che utilizzano tutte le altre pagine.

Si noti il `@RenderBody()` riga di codice nella parte inferiore del file. `RenderBody`è un segnaposto in cui tutte le pagine create visualizzati &quot;incapsulati&quot; nella pagina di layout. Modifica il `<h1>` intestazione da  **&quot;**  applicazione MVC personale&quot; a &quot;App cinematografica MVC&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Eseguire l'applicazione e si noti ora è indicato &quot;App cinematografica MVC&quot;. Fare clic su di **su** collegamento e tale pagina vengono visualizzati &quot;App cinematografica MVC&quot;anche.

L'intero  *\_Layout.vbhtml* file è illustrato di seguito:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

A questo punto, è necessario modificare il titolo della pagina di indice (visualizzazione).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Aprire *MvcMovie\Views\HelloWorld\Index.vbhtml*. Esistono due posizioni di apportare una modifica: in primo luogo, il testo che viene visualizzato nel titolo del browser, quindi nell'intestazione secondaria (la `<h2>` elemento). Ci accerteremo li leggermente diversa in modo da visualizzare il frammento di codice modifica la parte dell'app.

Eseguire l'applicazione e passare a`http://localhost:xx/HelloWorld`. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. È facile apportare modifiche nell'applicazione con piccole modifiche a una visualizzazione. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server.

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Il numero minimo di &quot;dati&quot; (in questo caso il &quot;Hello World!&quot; messaggio) è hardcoded, tuttavia. L'applicazione MVC è V (views) e abbiamo C (controller), ma ancora Nessun M (modello). Più avanti, verranno esaminati come creare un database e recuperare i dati del modello.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Prima di passare a un database e descrivere i modelli, tuttavia, esaminiamo innanzitutto il passaggio di informazioni a una visualizzazione dal Controller. Si desidera passare le richieste per eseguire il rendering di una risposta HTML a un client da un modello di visualizzazione. Questi oggetti vengono in genere creati e passati da una classe controller per un modello di visualizzazione e devono contenere solo i dati che richiede il modello di visualizzazione, ovvero non maggiori.

In precedenza con il `HelloWorldController` (classe), il `Welcome` metodo di azione ha avuto un `name` e un `numTimes` parametro e quindi restituire i valori dei parametri per il browser. Invece dispone di controller di continuare a eseguire il rendering la risposta direttamente, verrà invece è verrà inclusa che i dati in un contenitore per la visualizzazione. Controller e visualizzazioni è possono utilizzare un `ViewBag` oggetto che contenga i dati. Che verranno trasmesse automaticamente un modello di visualizzazione ed usato per il rendering nella risposta HTML utilizzando il contenuto del contenitore come dati. In questo modo è interessato il controller con un aspetto e il modello di visualizzazione con un altro, consentono di mantenere pulita &quot;separazione delle problematiche&quot; all'interno dell'applicazione.

In alternativa, è Impossibile definire una classe personalizzata, quindi creare un'istanza dell'oggetto nel nostro, inserirvi dati e passare alla visualizzazione. Che viene chiamato un ViewModel, spesso perché è un modello personalizzato per la visualizzazione. Per piccole quantità di dati, tuttavia, ViewBag è efficace.

Restituito per il *HelloWorldController.vb* una modifica ai file di `Welcome` metodo all'interno di un controller di inserire il messaggio e NumTimes in ViewBag. ViewBag è un oggetto dinamico. Ciò significa che è possibile inserire elementi desiderati a esso. ViewBag dispone di alcuna proprietà definito fino a quando non si inserisce un elemento all'interno.

L'intero `HelloWorldController.vb` con la nuova classe nello stesso file.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Il nostro ViewBag contiene dati che verranno passati la vista automaticamente. Nuovamente, in alternativa è possibile superati in un oggetto simile al seguente se è stato apprezzato:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Ora è necessario un `WelcomeView` modello! Eseguire l'applicazione in modo il nuovo codice viene compilato. Chiudere il browser, fare doppio clic all'interno di `Welcome` (metodo) e quindi fare clic su **Aggiungi visualizzazione**.

Ecco il **Aggiungi visualizzazione** la finestra di dialogo è simile.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Aggiungere il codice seguente sotto il `<h2>` elemento nel nuovo *iniziale.* file vbhtml. Verrà verificare un ciclo e pronunciare &quot;Hello&quot; le volte che l'utente indicato si deve!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Eseguire l'applicazione e passare a`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

A questo punto dati viene portati dall'URL e passati automaticamente al controller. Il controller di pacchetti di dati in un `Model` oggetto e passa tale oggetto nella vista. La vista che consente di visualizzare i dati in formato HTML per l'utente.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Che è un tipo di un &quot;M&quot; per modello, ma non il tipo di database. Creare un database di film con i concetti appresi.

>[!div class="step-by-step"]
[Precedente](adding-a-controller.md)
[Successivo](adding-a-model.md)
