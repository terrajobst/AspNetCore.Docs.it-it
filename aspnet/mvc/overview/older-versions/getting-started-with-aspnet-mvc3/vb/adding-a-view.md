---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Aggiunta di una visualizzazione (VB) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 0d6311cac4fe01b2e21b300ff3841b3f2ca6fcf5
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577063"
---
<a name="adding-a-view-vb"></a>Aggiunta di una visualizzazione (VB)
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente Visual Basic.NET è disponibile a complemento di questo argomento. [Scaricare la versione VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce c#, passare al [c# versione](../cs/adding-a-view.md) di questa esercitazione.


In questa sezione verranno per modificare il `HelloWorldController` classe da utilizzare un file di modello di visualizzazione per correttamente incapsulare il processo di generazione di risposte HTML a un client.

Iniziamo con un modello di visualizzazione con il `Index` nel metodo il `HelloWorldController` classe. Attualmente il `Index` metodo restituisce una stringa con un messaggio hardcoded nella classe controller. Modifica il `Index` per restituire un `View` dell'oggetto, come illustrato nell'esempio seguente:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Questo punto, aggiungere un modello di visualizzazione per il progetto che possiamo richiamare con la `Index` (metodo). A tale scopo, fare doppio clic all'interno di `Index` (metodo) e fare clic su **Aggiungi visualizzazione**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

Il **Aggiungi visualizzazione** verrà visualizzata la finestra di dialogo. Lasciare le voci predefinite, quindi scegliere il **Add** pulsante.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

Il *MvcMovie\Views\HelloWorld* cartella e il *MvcMovie\Views\HelloWorld\Index.vbhtml* file vengono creati. È possibile visualizzarli nella **Esplora soluzioni**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Aggiungere del codice HTML nel `<h2>` tag. Modificato *MvcMovie\Views\HelloWorld\Index.vbhtml* file è illustrato di seguito.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Eseguire l'applicazione e selezionare il &quot;HelloWorld&quot; controller (`http://localhost:xxxx/HelloWorld`). Il `Index` metodo nel controller di non esegue la quantità di lavoro, sufficiente eseguita l'istruzione `return View()`, per indicare che si desidera utilizzare un file di modello di visualizzazione per il rendering di una risposta al client. Perché non si ha specificato il nome del file del modello di visualizzazione da usare in modo esplicito, per impostazione predefinita MVC ASP.NET all'utilizzo il *Index.vbhtml* file di visualizzazione all'interno di *\Views\HelloWorld* cartella. L'immagine seguente mostra la stringa hardcoded nella vista.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Sembra abbastanza positivo. Si noti tuttavia che barra del titolo del browser indica &quot;indice&quot; , il titolo della pagina big dice &quot;applicazione MVC personale.&quot; È possibile modificare quelle.

## <a name="changing-views-and-layout-pages"></a>Modifica delle viste e delle pagine di layout

In primo luogo, è possibile modificare il testo &quot;applicazione MVC personale.&quot; Tale testo viene condivisa e viene visualizzato in ogni pagina. Viene effettivamente visualizzato in un'unica posizione nel progetto, anche se è in ogni pagina nell'applicazione. Passare al */viste/Shared* cartella **Esplora soluzioni** e aprire il  *\_Layout.vbhtml* file. Questo file viene chiamato una pagina di layout ed è condiviso &quot;shell&quot; che utilizzano tutte le altre pagine.

Si noti il `@RenderBody()` riga di codice nella parte inferiore del file. `RenderBody` è un segnaposto dove tutte le pagine create visualizzati &quot;incapsulati&quot; nella pagina di layout. Modifica il `<h1>` titolo da **&quot;** applicazione MVC personale&quot; al &quot;App MVC Movie&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Eseguire l'applicazione e prendere nota viene ora indicato &quot;MVC Movie App&quot;. Fare clic sui **sulle** collegamento e tale pagina viene mostrato &quot;MVC Movie App&quot;anche.

L'intero  *\_Layout.vbhtml* file è illustrato di seguito:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

A questo punto, è possibile modificare il titolo della pagina di indice (visualizzazione).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Aprire *MvcMovie\Views\HelloWorld\Index.vbhtml*. Esistono due modi per apportare una modifica: prima di tutto il testo che viene visualizzato nel titolo del browser, quindi nell'intestazione secondaria (la `<h2>` elemento). Si renderà li leggermente diversa in modo da visualizzare il frammento di codice viene modificato quale parte dell'app.

Eseguire l'applicazione e passare a`http://localhost:xx/HelloWorld`. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. È facile apportare modifiche big nell'applicazione con piccole modifiche a una visualizzazione. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server.

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

La minima &quot;dati&quot; (in questo caso il &quot;Hello World!&quot; messaggio) è hardcoded, tuttavia. Applicazione MVC ha V (viste) e abbiamo C (controller), ma ancora alcun M (modello). A breve verrà illustrato come creare un database e recuperare i dati del modello da quest'ultimo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Prima di passare a un database e comunicare con informazioni sui modelli, tuttavia, esaminiamo innanzitutto il passaggio di informazioni dal Controller a una visualizzazione. Si vuole passare ciò che richiede un modello di vista per eseguire il rendering di una risposta HTML a un client. Questi oggetti vengono in genere creati e passati da una classe controller da un modello di vista e devono contenere solo i dati che richiede il modello di vista e non è più.

In precedenza con il `HelloWorldController` (classe), il `Welcome` metodo di azione ha avuto un `name` e un `numTimes` parametro e quindi restituire i valori dei parametri nel browser. Invece dispone di controller di continuare a eseguire il rendering di questa risposta direttamente, è possibile invece è verrà inclusa che i dati in un contenitore per la visualizzazione. Controller e visualizzazioni è possono usare un `ViewBag` oggetto per contenere i dati. Che verrà essere passato a un modello di vista automaticamente e usato per eseguire il rendering nella risposta HTML utilizzando il contenuto del contenitore sotto forma di dati. In questo modo il controller si occupa di un aspetto e il modello di visualizzazione con un altro, ovvero ci permette di mantenere pulita &quot;la separazione dei compiti&quot; all'interno dell'applicazione.

In alternativa, è potremmo definire una classe personalizzata, quindi creare un'istanza di tale oggetto da soli, inserimento dei dati e passarlo alla visualizzazione. Che viene spesso definito un elemento ViewModel, perché si tratta di un modello personalizzato per la visualizzazione. Per piccole quantità di dati, tuttavia, ViewBag funziona perfettamente.

Tornare al *HelloWorldController.vb* una modifica ai file di `Welcome` metodo all'interno di un controller di inserire il messaggio e NumTimes in ViewBag. ViewBag è un oggetto dinamico. Ciò significa che è possibile inserire gli elementi desiderati a esso. ViewBag dispone di alcuna proprietà definite fino a quando non si inserisce un elemento al suo interno.

L'intero `HelloWorldController.vb` con la nuova classe nello stesso file.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

A questo punto il ViewBag contiene dati che verranno passati la visualizzazione automaticamente. Anche in questo caso, in alternativa è potremmo avere passati in un oggetto simile al seguente se apprezzavamo il:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

A questo punto è necessario un `WelcomeView` modello! Eseguire l'applicazione in modo che il nuovo codice viene compilato. Chiudere il browser, fare doppio clic all'interno di `Welcome` metodo e quindi fare clic su **Aggiungi visualizzazione**.

Questa figura viene illustrata il **Aggiungi visualizzazione** nella finestra di dialogo è simile.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Aggiungere il codice seguente sotto il `<h2>` nel nuovo elemento <em>iniziale.</em> file vbhtml. Illustreremo apportare un ciclo e pronunciare &quot;Hello&quot; come tutte le volte che l'utente indicato si deve!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Eseguire l'applicazione e passare a `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

A questo punto i dati vengono prelevati dall'URL e passati automaticamente al controller. Il controller in pacchetti i dati in un `Model` oggetto e passa tale oggetto alla vista. La vista che consente di visualizzare i dati in formato HTML all'utente.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Era un tipo di un' &quot;M&quot; per modello, ma non il tipo di database. Creare un database di film con i concetti appresi.

> [!div class="step-by-step"]
> [Precedente](adding-a-controller.md)
> [Successivo](adding-a-model.md)
