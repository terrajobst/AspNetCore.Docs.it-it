---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Aggiunta di una visualizzazione (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 9fc8c6cad44016511c462b4206c28ea3449ff97e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576612"
---
<a name="adding-a-view-c"></a>Aggiunta di una visualizzazione (c#)
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.
> 
> 
> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare al [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.


In questa sezione si intende modificare il `HelloWorldController` classe utilizzare file di modello per correttamente il processo di generazione di risposte HTML a un client di incapsulare la visualizzazione.

Si creerà un file di modello di visualizzazione usando le nuove [motore di visualizzazione Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introdotte con ASP.NET MVC 3. I modelli di vista basati su Razor hanno una *cshtml* estensione di file e fornire un modo elegante per creare HTML di output usando c#. Riduce al minimo il numero di caratteri e sequenze di tasti richieste durante la scrittura di un modello di vista Razor e consente a un veloce, fluido codifica del flusso di lavoro.

Iniziare con un modello di visualizzazione con il `Index` metodo nel `HelloWorldController` classe. Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller. Modifica il `Index` per restituire un `View` dell'oggetto, come illustrato nell'esempio seguente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Questo codice Usa un modello di vista per generare una risposta HTML al browser. Nel progetto, aggiungere un modello di visualizzazione che è possibile usare con il `Index` (metodo). A tale scopo, fare doppio clic all'interno di `Index` (metodo) e fare clic su **Aggiungi visualizzazione**.

![](adding-a-view/_static/image1.png)

Il **Aggiungi visualizzazione** verrà visualizzata la finestra di dialogo. Lasciare le impostazioni predefinite di quelle fornite sono e fare clic sui **Add** pulsante:

![](adding-a-view/_static/image2.png)

Il *MvcMovie\Views\HelloWorld* cartella e il *MvcMovie\Views\HelloWorld\Index.cshtml* file vengono creati. È possibile visualizzarli nella **Esplora soluzioni**:

![](adding-a-view/_static/image3.png)

Il seguente viene illustrato il *index. cshtml* file creato:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Aggiungere del codice HTML nel `<h2>` tag. Modificato *MvcMovie\Views\HelloWorld\Index.cshtml* file è illustrato di seguito.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Eseguire l'applicazione e selezionare il `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`). Il `Index` metodo nel controller di non esegue la quantità di lavoro, sufficiente eseguita l'istruzione `return View()`, quale specificato che il metodo deve usare un file di modello di visualizzazione per il rendering di una risposta nel browser. Perché è stato specificato in modo esplicito il nome del file del modello di visualizzazione da usare, ASP.NET MVC impostato sul valore predefinito usando il *index. cshtml* file di visualizzazione nel *\Views\HelloWorld* cartella. L'immagine seguente mostra la stringa hardcoded nella vista.

![](adding-a-view/_static/image6.png)

Sembra abbastanza positivo. Si noti tuttavia che barra del titolo del browser indica "Index", il titolo della pagina big dice "Applicazione MVC." È possibile modificare quelle.

## <a name="changing-views-and-layout-pages"></a>Modifica delle viste e le pagine di Layout

In primo luogo, si desidera modificare il titolo "My Application MVC" nella parte superiore della pagina. Tale testo è comune a tutte le pagine. In realtà viene implementato in un'unica posizione nel progetto, anche se è presente in ogni pagina dell'applicazione. Passare al */viste/Shared* cartella **Esplora soluzioni** e aprire il  *\_layout. cshtml* file. Questo file viene chiamato un *pagina layout* ed è condivisa "shell" che utilizzano tutte le altre pagine.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Modelli di layout consentono di specificare il layout del contenitore HTML del sito in un'unica posizione e quindi applicarlo in più pagine del sito. Si noti il `@RenderBody()` riga nella parte inferiore del file. `RenderBody` è un segnaposto dove tutte le pagine di visualizzazione specifici che crei vengono visualizzati, "wrapping" nella pagina di layout. Modificare l'intestazione del titolo nel modello di layout da "My MVC Application" a "MVC Movie App".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Eseguire l'applicazione e si noti che viene ora visualizzato "MVC Movie App". Fare clic sui **sulle** collegamento verrà visualizzato come pagina Mostra "MVC Movie App", troppo. Abbiamo potuto apportare la modifica di una volta nel modello di layout e avere tutte le pagine nel sito riflettono il nuovo titolo.

![](adding-a-view/_static/image9.png)

L'intero  *\_layout. cshtml* file è illustrato di seguito:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

A questo punto, è possibile modificare il titolo della pagina di indice (visualizzazione).

Aprire *MvcMovie\Views\HelloWorld\Index.cshtml*. Esistono due modi per apportare una modifica: prima di tutto il testo che viene visualizzato nel titolo del browser, quindi nell'intestazione secondaria (la `<h2>` elemento). Renderli leggermente diversi in modo da poter esaminare la parte specifica di app modificata dal frammento specifico di codice.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Per indicare il titolo HTML da visualizzare, il codice precedente imposta una `Title` proprietà del `ViewBag` oggetto (che è nel *index. cshtml* modello di vista). Se si osserva nuovamente il codice sorgente del modello di layout, si noterà che il modello Usa questo valore nel `<title>` come parte dell'elemento di `<head>` sezione del codice HTML. Con questo approccio, è possibile passare facilmente altri parametri tra il modello di visualizzazione e il file di layout.

Eseguire l'applicazione e passare a `http://localhost:xx/HelloWorld`. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server.

Si noti inoltre come il contenuto nel *index. cshtml* modello di visualizzazione dopo il merge con il  *\_layout. cshtml* visualizzare il modello e una singola risposta HTML è stato inviato al browser. I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione.

![](adding-a-view/_static/image10.png)

Il breve testo di "dati", in questo caso il messaggio "Hello from our View Template!", è tuttavia hardcoded. L'applicazione MVC ha un elemento "V" (vista) ed è stato ottenuto un elemento "C" (controller), ma non ancora un elemento "M" (modello). A breve verrà illustrato come creare un database e recuperare i dati del modello da quest'ultimo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Prima di passare a un database e comunicare con informazioni sui modelli, tuttavia, esaminiamo innanzitutto il passaggio di informazioni dal controller a una visualizzazione. Le classi controller vengono richiamate in risposta a una richiesta URL in ingresso. Una classe controller è in cui viene scritto il codice che gestisce i parametri in arrivo, recupera i dati da un database e in definitiva determina il tipo di risposta da inviare al browser. I modelli di visualizzazione sono quindi utilizzabile per generare e formattare una risposta HTML al browser da un controller.

I controller sono responsabili di fornire i dati o oggetti sono necessari affinché un modello di vista eseguire il rendering di una risposta nel browser. Un modello di vista deve mai eseguire la logica di business o interagire direttamente con un database. Al contrario, dovrebbe funzionare solo con i dati forniti dal controller. Questa "separazione delle problematiche" consente di mantenere il codice pulito e più facilmente gestibile.

Attualmente, il `Welcome` metodo di azione il `HelloWorldController` classe accetta un `name` e un `numTimes` parametro e quindi genera i valori direttamente al browser. Anziché ottenere il rendering di questa risposta come stringa, cambiare il controller per usare invece un modello di vista. Il modello di vista genererà una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta. È possibile farlo facendo in modo che il controller inserisca i dati dinamici che il modello di vista necessita in un `ViewBag` oggetto che può quindi accedere il modello di visualizzazione.

Tornare al *HelloWorldController.cs* di file e modificare le `Welcome` metodo per aggiungere un `Message` e `NumTimes` valore per il `ViewBag` oggetto. `ViewBag` è un oggetto dinamico, ovvero che è possibile inserire gli elementi desiderati da esso. il `ViewBag` oggetto non dispone di alcuna proprietà definite fino a quando non si inserisce un elemento al suo interno. Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

A questo punto il `ViewBag` oggetto contiene i dati che verranno passati automaticamente alla visualizzazione.

Successivamente, è necessario un modello di vista Welcome! Nel **Debug** dal menu **compilazione MvcMovie** per assicurarsi che la compilazione del progetto.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Quindi fare doppio clic all'interno di `Welcome` (metodo) e fare clic su **Aggiungi visualizzazione**. Questa figura viene illustrata la **Aggiungi visualizzazione** nella finestra di dialogo è simile a:

![](adding-a-view/_static/image13.png)

Fare clic su **Add**e quindi aggiungere il codice seguente sotto il `<h2>` nuovo elemento *Welcome* file. Si creerà un ciclo con la dicitura "Hello" come tutte le volte che l'utente afferma che quanto previsto. L'intero *Welcome* file è illustrato di seguito.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Eseguire l'applicazione e passare all'URL seguente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

A questo punto i dati vengono prelevati dall'URL e passati automaticamente al controller. Il controller inserisce i dati in un `ViewBag` oggetto e passa tale oggetto alla vista. La vista visualizza quindi i dati in formato HTML all'utente.

![](adding-a-view/_static/image14.png)

Queste operazioni hanno riguardato un tipo di "M" per modello, ma non il tipo di database. Creare un database di film con i concetti appresi.

> [!div class="step-by-step"]
> [Precedente](adding-a-controller.md)
> [Successivo](adding-a-model.md)
