---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Aggiunta di una vista (c#) | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 50ce4a2024ffd9e2bbb5526717052d486689ff38
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view-c"></a>Aggiunta di una vista (c#)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013. È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.
> 
> 
> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)
> 
> Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con il codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare il [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.


In questa sezione si intende modificare il `HelloWorldController` classe per utilizzare la visualizzazione dei file di modello per correttamente incapsulano il processo di generazione di risposte HTML a un client.

Si creerà un file di modello di visualizzazione utilizzando il nuovo [motore di visualizzazione Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introdotto con ASP.NET MVC 3. I modelli di visualizzazione in base Razor presentano un *. cshtml* estensione di file e fornire un modo elegante di creazione di un output che utilizza c# HTML. Razor riduce al minimo il numero di caratteri e sequenze di tasti richieste durante la scrittura di un modello di visualizzazione e consente un fluido veloce, la codifica del flusso di lavoro.

Iniziare a usare un modello di visualizzazione con il `Index` metodo la `HelloWorldController` classe. Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller. Modifica il `Index` per restituire un `View` dell'oggetto, come illustrato nell'esempio seguente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Questo codice Usa un modello di visualizzazione per generare una risposta HTML al browser. Nel progetto, aggiungere un modello di visualizzazione che è possibile utilizzare con il `Index` metodo. A tale scopo, fare doppio clic all'interno di `Index` (metodo) e fare clic su **Aggiungi visualizzazione**.

![](adding-a-view/_static/image1.png)

Il **Aggiungi visualizzazione** viene visualizzata la finestra di dialogo. Lasciare le impostazioni predefinite modo vengono e fare clic su di **Aggiungi** pulsante:

![](adding-a-view/_static/image2.png)

Il *MvcMovie\Views\HelloWorld* cartella e *MvcMovie\Views\HelloWorld\Index.cshtml* vengono creati i file. È possibile visualizzarli in **Esplora**:

![](adding-a-view/_static/image3.png)

Nella seguente il *cshtml* file che è stato creato:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Aggiungere il codice HTML con il `<h2>` tag. Modificato *MvcMovie\Views\HelloWorld\Index.cshtml* file è illustrato di seguito.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Eseguire l'applicazione e individuare il `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`). Il `Index` metodo nel controller di non eseguire la quantità di lavoro, sufficiente eseguita l'istruzione `return View()`, quale specificato che il metodo deve usare un file di modello di visualizzazione per il rendering di una risposta nel browser. Poiché è stato specificato in modo esplicito il nome del file modello di visualizzazione da utilizzare, ASP.NET MVC impostato sul valore predefinito utilizzando il *cshtml* Visualizza file nel *\Views\HelloWorld* cartella. L'immagine seguente mostra la stringa hardcoded nella vista.

![](adding-a-view/_static/image6.png)

È buona. Si noti tuttavia che barra del titolo del browser indica "Index" e il titolo di grandi dimensioni nella pagina è indicato "Applicazione MVC." È necessario modificare quelle.

## <a name="changing-views-and-layout-pages"></a>Modifica delle visualizzazioni e pagine di Layout

In primo luogo, si desidera modificare il titolo "My Application MVC" nella parte superiore della pagina. Il testo è comune a ogni pagina. In realtà viene implementato in un'unica posizione nel progetto, anche se è presente in ogni pagina nell'applicazione. Passare al */visualizzazioni/Shared* cartella **Esplora** e aprire il  *\_cshtml* file. Questo file viene chiamato un *pagina layout* ed è condivisa "shell" che utilizzano tutte le altre pagine.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Modelli di layout consentono di specificare il layout di contenitore HTML del sito in un'unica posizione e quindi applicarlo in più pagine del sito. Si noti il `@RenderBody()` nella parte inferiore del file. `RenderBody` è un segnaposto in cui tutte le pagine di visualizzazione specifica che crei visualizzati, "wrapping" nella pagina di layout. Modificare l'intestazione del titolo nel modello di layout da "My MVC Application" a "MVC film App".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Eseguire l'applicazione e si noti che verrà ora visualizzato il numero "MVC film App". Fare clic su di **su** collegamento e vedere come tale pagina viene visualizzato "App" film MVC, troppo. Abbiamo potuto apportare la modifica di una volta nel modello di layout e dispone di tutte le pagine nel sito di riflettere il nuovo titolo.

![](adding-a-view/_static/image9.png)

L'intero  *\_cshtml* file è illustrato di seguito:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

A questo punto, è necessario modificare il titolo della pagina di indice (visualizzazione).

Aprire *MvcMovie\Views\HelloWorld\Index.cshtml*. Esistono due posizioni di apportare una modifica: in primo luogo, il testo che viene visualizzato nel titolo del browser, quindi nell'intestazione secondaria (la `<h2>` elemento). Renderli leggermente diversi in modo da poter esaminare la parte specifica di app modificata dal frammento specifico di codice.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Per indicare il titolo HTML da visualizzare, il codice di sopra di set di un `Title` proprietà del `ViewBag` oggetto (che è il *cshtml* modello di visualizzazione). Se si esamina il codice sorgente del modello di layout, si noterà che il modello utilizza questo valore di `<title>` come parte dell'elemento di `<head>` sezione del codice HTML. Questo approccio, è possibile passare facilmente altri parametri tra il modello di visualizzazione e il file di layout.

Eseguire l'applicazione e passare a `http://localhost:xx/HelloWorld`. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server.

Si noti inoltre come il contenuto nel *cshtml* vista modello è stato unito il  *\_cshtml* modello di visualizzazione e una singola risposta HTML è stata inviata al browser. I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione.

![](adding-a-view/_static/image10.png)

Il breve testo di "dati", in questo caso il messaggio "Hello from our View Template!", è tuttavia hardcoded. L'applicazione MVC ha un elemento "V" (vista) ed è stato ottenuto un elemento "C" (controller), ma non ancora un elemento "M" (modello). Più avanti, verranno esaminati come creare un database e recuperare i dati del modello.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Prima di passare a un database e descrivere i modelli, tuttavia, esaminiamo innanzitutto il passaggio di informazioni a una visualizzazione dal controller. Classi controller vengono richiamate in risposta a una richiesta URL in ingresso. Una classe controller viene scritto il codice che gestisce i parametri in ingresso, recupera i dati da un database e infine decide il tipo di risposta da inviare al browser. Modelli di visualizzazione quindi utilizzabile da un controller per generare e formattare una risposta HTML al browser.

I controller sono responsabili di fornire qualsiasi dati o gli oggetti necessari affinché un modello di visualizzazione per il rendering di una risposta nel browser. Un modello di visualizzazione deve eseguire la logica di business o mai interagire direttamente con un database. Invece, dovrebbe funzionare solo con i dati che vengano forniti dal controller. Mantenere questa "separazione delle problematiche" consente di mantenere il codice pulita e più gestibili.

Attualmente, il `Welcome` metodo di azione il `HelloWorldController` classe accetta un `name` e `numTimes` parametro e quindi i valori direttamente al browser di output. Per evitare che il controller di eseguire il rendering questa risposta sotto forma di stringa, è necessario modificare il controller per utilizzare invece un modello di visualizzazione. Il modello di vista genererà una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta. È possibile farlo con il controller di inserire i dati dinamici che richiede il modello di visualizzazione di un `ViewBag` oggetto che può quindi accedere il modello di visualizzazione.

Restituito per il *HelloWorldController.cs* file e modificare il `Welcome` metodo per aggiungere un `Message` e `NumTimes` valore per il `ViewBag` oggetto. `ViewBag` è un oggetto dinamico, ovvero che è possibile inserire desiderati. il `ViewBag` oggetto dispone di alcuna proprietà definito fino a quando non si inserisce un elemento all'interno. Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

A questo punto il `ViewBag` oggetto contiene i dati che verranno passati alla visualizzazione automaticamente.

Successivamente, è necessario un modello di visualizzazione iniziale. Nel **Debug** dal menu **MvcMovie compilare** per assicurarsi la compilazione del progetto.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Quindi fare doppio clic all'interno di `Welcome` (metodo) e fare clic su **Aggiungi visualizzazione**. Ecco il **Aggiungi visualizzazione** la finestra di dialogo simile:

![](adding-a-view/_static/image13.png)

Fare clic su **Aggiungi**, quindi aggiungere il codice seguente sotto il `<h2>` elemento nel nuovo *Welcome.cshtml* file. Si creerà un ciclo che afferma "Hello" come tutte le volte che l'utente è indicato che come previsto. L'intero *Welcome.cshtml* file è illustrato di seguito.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Eseguire l'applicazione e passare all'URL seguente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

A questo punto dati viene portati dall'URL e passati automaticamente al controller. Il controller di pacchetti di dati in un `ViewBag` oggetto e passa tale oggetto nella vista. La vista visualizza quindi i dati in formato HTML per l'utente.

![](adding-a-view/_static/image14.png)

Queste operazioni hanno riguardato un tipo di "M" per modello, ma non il tipo di database. Creare un database di film con i concetti appresi.

> [!div class="step-by-step"]
> [Precedente](adding-a-controller.md)
> [Successivo](adding-a-model.md)
