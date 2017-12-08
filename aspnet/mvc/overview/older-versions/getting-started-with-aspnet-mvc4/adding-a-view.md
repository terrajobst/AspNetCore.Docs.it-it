---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Aggiunta di una vista | Documenti Microsoft
author: Rick-Anderson
description: "Nota: Una versione aggiornata di questa esercitazione è disponibile qui che utilizza ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice seguire e demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 4309290294b28d4c177e0193719bcff4b3f2a8cf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view"></a>Aggiunta di una vista
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013. È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.


In questa sezione si intende modificare il `HelloWorldController` classe per utilizzare la visualizzazione dei file di modello per correttamente incapsulano il processo di generazione di risposte HTML a un client.

Si creerà un file modello di visualizzazione utilizzando il [motore di visualizzazione Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introdotto con ASP.NET MVC 3. I modelli di visualizzazione in base Razor presentano un *. cshtml* estensione di file e fornire un modo elegante di creazione di un output che utilizza c# HTML. Razor riduce al minimo il numero di caratteri e sequenze di tasti richieste durante la scrittura di un modello di visualizzazione e consente un fluido veloce, la codifica del flusso di lavoro.

Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller. Modifica il `Index` per restituire un `View` dell'oggetto, come illustrato nel codice seguente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Il `Index` metodo precedente Usa un modello di visualizzazione per generare una risposta HTML al browser. I metodi del controller (noto anche come [metodi di azione](http://rachelappel.com/asp.net-mvc-actionresults-explained)), ad esempio il `Index` dei metodi descritti sopra, restituiscono in genere un [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx) (o una classe derivata da [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx)), i tipi non primitivi come stringa.

Nel progetto, aggiungere un modello di visualizzazione che è possibile utilizzare con il `Index` metodo. A tale scopo, fare doppio clic all'interno di `Index` (metodo) e fare clic su **Aggiungi visualizzazione**.

![](adding-a-view/_static/image1.png)

Il **Aggiungi visualizzazione** viene visualizzata la finestra di dialogo. Lasciare le impostazioni predefinite modo vengono e fare clic su di **Aggiungi** pulsante:

![](adding-a-view/_static/image2.png)

Il *MvcMovie\Views\HelloWorld* cartella e *MvcMovie\Views\HelloWorld\Index.cshtml* vengono creati i file. È possibile visualizzarli in **Esplora**:

![](adding-a-view/_static/image3.png)

Nella seguente il *cshtml* file che è stato creato:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Aggiungere il codice HTML seguente sotto il `<h2>` tag.

[!code-html[Main](adding-a-view/samples/sample2.html)]

L'intero *MvcMovie\Views\HelloWorld\Index.cshtml* file è illustrato di seguito.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Se si utilizza Visual Studio 2012, in Esplora soluzioni, fare clic destro la *cshtml* file e selezionare **Visualizza in controllo pagina**.

![PI](adding-a-view/_static/image5.png)

Il [esercitazione controllo pagina](../../views/using-page-inspector-in-aspnet-mvc.md) per ulteriori informazioni su questo nuovo strumento.

In alternativa, eseguire l'applicazione e individuare il `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`). Il `Index` metodo nel controller di non eseguire la quantità di lavoro, sufficiente eseguita l'istruzione `return View()`, quale specificato che il metodo deve usare un file di modello di visualizzazione per il rendering di una risposta nel browser. Poiché è stato specificato in modo esplicito il nome del file modello di visualizzazione da utilizzare, ASP.NET MVC impostato sul valore predefinito utilizzando il *cshtml* Visualizza file nel *\Views\HelloWorld* cartella. L'immagine seguente mostra la stringa &quot;Hello da questo modello di visualizzazione!&quot; hardcoded nella vista.

![](adding-a-view/_static/image6.png)

È buona. Si noti tuttavia che Mostra barra del titolo del browser &quot;indice personali A ASP.NET&quot; e il collegamento grande nella parte superiore della pagina è indicato &quot;inserire qui il logo.&quot; Di sotto di &quot;qui il logo.&quot; collegamento sono registrazione e log in collegamenti e seguito che si collega alla home page, circa e contattare pagine. È necessario modificare alcune di queste.

## <a name="changing-views-and-layout-pages"></a>Modifica delle visualizzazioni e pagine di Layout

In primo luogo, si desidera modificare il &quot;qui il logo.&quot; titolo nella parte superiore della pagina. Il testo è comune a ogni pagina. Viene effettivamente implementata in un'unica posizione nel progetto, anche se è presente in ogni pagina nell'applicazione. Passare al */visualizzazioni/Shared* cartella **Esplora** e aprire il  *\_cshtml* file. Questo file viene chiamato un *pagina layout* ed è condiviso &quot;shell&quot; che utilizzano tutte le altre pagine.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Modelli di layout consentono di specificare il layout di contenitore HTML del sito in un'unica posizione e quindi applicarlo in più pagine del sito. Trovare la riga `@RenderBody()`. `RenderBody` è un segnaposto dove tutte le pagine specifiche della vista vengono presentate, &quot;incapsulate&quot; nella pagina di layout. Ad esempio, se si seleziona il collegamento a informazioni su, il *Views\Home\About.cshtml* vista viene eseguita all'interno di `RenderBody` (metodo).

Modificare l'intestazione del titolo del sito nel modello di layout da &quot;inserire qui il logo&quot; a &quot;MVC film&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Sostituire il contenuto dell'elemento title con il markup seguente:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Eseguire l'applicazione e si noti che ora dichiara &quot;MVC film &quot;. Fare clic su di **su** collegamento e vedere la pagina viene &quot;MVC film&quot;, troppo. Abbiamo potuto apportare la modifica di una volta nel modello di layout e dispone di tutte le pagine nel sito di riflettere il nuovo titolo.

![](adding-a-view/_static/image8.png)

A questo punto, è necessario modificare il titolo della visualizzazione dell'indice.

Aprire *MvcMovie\Views\HelloWorld\Index.cshtml*. Esistono due posizioni di apportare una modifica: in primo luogo, il testo che viene visualizzato nel titolo del browser, quindi nell'intestazione secondaria (la `<h2>` elemento). Renderli leggermente diversi in modo da poter esaminare la parte specifica di app modificata dal frammento specifico di codice.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

Per indicare il titolo HTML da visualizzare, il codice di sopra di set di un `Title` proprietà del `ViewBag` oggetto (che è il *cshtml* modello di visualizzazione). Se si esamina il codice sorgente del modello di layout, si noterà che il modello utilizza questo valore nel `<title>` come parte dell'elemento di `<head>` sezione del codice HTML che ha in precedenza. Usando questa `ViewBag` approccio, è possibile facilmente passare altri parametri tra il modello di visualizzazione e il file di layout.

Eseguire l'applicazione e passare a `http://localhost:xx/HelloWorld`. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server. Il titolo del browser viene creato con il `ViewBag.Title` è impostato *cshtml* visualizzare modelli e gli altri &quot;-App cinematografica&quot; aggiunto nel file di layout.

Si noti inoltre come il contenuto nel *cshtml* vista modello è stato unito il  *\_cshtml* modello di visualizzazione e una singola risposta HTML è stata inviata al browser. I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione.

![](adding-a-view/_static/image9.png)

Il numero minimo di &quot;dati&quot; (in questo caso il &quot;Hello da questo modello di visualizzazione!&quot; messaggio) è hardcoded, tuttavia. L'applicazione MVC con un &quot;V&quot; (visualizzazione) e hai una &quot;C&quot; (controller), ma non &quot;M&quot; (modello) ancora. Più avanti, verranno esaminati come creare un database e recuperare i dati del modello.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Prima di passare a un database e descrivere i modelli, tuttavia, esaminiamo innanzitutto il passaggio di informazioni a una visualizzazione dal controller. Classi controller vengono richiamate in risposta a una richiesta URL in ingresso. Una classe controller viene scritto il codice che gestisce il browser in arrivo delle richieste, recupera i dati da un database e infine decide il tipo di risposta da inviare al browser. Modelli di visualizzazione quindi utilizzabile da un controller per generare e formattare una risposta HTML al browser.

I controller sono responsabili di fornire qualsiasi dati o gli oggetti necessari affinché un modello di visualizzazione per il rendering di una risposta nel browser. Una procedura consigliata: **un modello di visualizzazione non deve mai eseguire la logica di business o interagire direttamente con un database**. Al contrario, un modello di visualizzazione dovrebbe funzionare solo con i dati che vengano forniti dal controller. Gestione di questo &quot;separazione delle problematiche&quot; consente di mantenere il codice pulito, testabile e più gestibili.

Attualmente, il `Welcome` metodo di azione il `HelloWorldController` classe accetta un `name` e `numTimes` parametro e quindi i valori direttamente al browser di output. Per evitare che il controller di eseguire il rendering questa risposta sotto forma di stringa, è necessario modificare il controller per utilizzare invece un modello di visualizzazione. Il modello di vista genererà una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta. È possibile farlo con il controller di inserire i dati dinamici (parametri) necessarie per il modello di visualizzazione in un `ViewBag` oggetto che può quindi accedere il modello di visualizzazione.

Restituito per il *HelloWorldController.cs* file e modificare il `Welcome` metodo per aggiungere un `Message` e `NumTimes` valore per il `ViewBag` oggetto. `ViewBag`è un oggetto dinamico, ovvero che è possibile inserire elementi desiderati. il `ViewBag` oggetto dispone di alcuna proprietà definito fino a quando non si inserisce un elemento all'interno. Il [sistema di associazione del modello MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) esegue automaticamente il mapping di parametri denominati (`name` e `numTimes`) dalla stringa di query nella barra degli indirizzi per i parametri del metodo. Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

A questo punto il `ViewBag` oggetto contiene i dati che verranno passati alla visualizzazione automaticamente.

Successivamente, è necessario un modello di visualizzazione iniziale. Nel **compilare** dal menu **MvcMovie compilare** per assicurarsi la compilazione del progetto.

Quindi fare doppio clic all'interno di `Welcome` (metodo) e fare clic su **Aggiungi visualizzazione**.

![](adding-a-view/_static/image10.png)

Ecco il **Aggiungi visualizzazione** la finestra di dialogo simile:

![](adding-a-view/_static/image11.png)

Fare clic su **Aggiungi**, quindi aggiungere il codice seguente sotto il `<h2>` elemento nel nuovo *Welcome.cshtml* file. Si creerà un ciclo che afferma &quot;Hello&quot; le volte che l'utente è indicato come previsto. L'intero *Welcome.cshtml* file è illustrato di seguito.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Eseguire l'applicazione e passare all'URL seguente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ora i dati vengono eseguiti dall'URL e passati al controller utilizzando il [dello strumento di associazione del modello](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Il controller di pacchetti di dati in un `ViewBag` oggetto e passa tale oggetto nella vista. La vista visualizza quindi i dati in formato HTML per l'utente.

![](adding-a-view/_static/image12.png)

Nell'esempio precedente, è stato usato un `ViewBag` oggetto per passare i dati dal controller per una vista. Quest'ultimo nell'esercitazione si utilizzerà un modello di visualizzazione per passare dati da un controller di una vista. L'approccio di modello di visualizzazione per il passaggio di dati è in genere consigliabile rispetto all'approccio di visualizzazione elenco. Vedere il post di blog [V fortemente tipizzato visualizzazioni dinamiche](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) per ulteriori informazioni.

Che è un tipo di un &quot;M&quot; per modello, ma non il tipo di database. Creare un database di film con i concetti appresi.

>[!div class="step-by-step"]
[Precedente](adding-a-controller.md)
[Successivo](adding-a-model.md)
