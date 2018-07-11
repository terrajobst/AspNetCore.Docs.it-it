---
title: Aggiunta di una vista a un'app MVC
author: Rick-Anderson
description: Aggiunta di una vista a un'app MVC
ms.author: riande
ms.date: 09/1721/2017
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 8b9ef79d630623019b22414ef730edffa5a83a09
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38166398"
---
<a name="adding-a-view"></a>Aggiunta di una vista
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In questa sezione si intende modificare il `HelloWorldController` classe utilizzare file di modello per correttamente il processo di generazione di risposte HTML a un client di incapsulare la visualizzazione. 

Si creerà un file modello di visualizzazione usando la [motore di visualizzazione Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). I modelli di vista basati su Razor hanno una *cshtml* estensione di file e fornire un modo elegante per creare HTML di output usando c#. Riduce al minimo il numero di caratteri e sequenze di tasti richieste durante la scrittura di un modello di vista Razor e consente a un veloce, fluido codifica del flusso di lavoro.

Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller. Modifica il `Index` per restituire un `View` dell'oggetto, come illustrato nel codice seguente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

Il `Index` metodo precedente Usa un modello di vista per generare una risposta HTML al browser. I metodi del controller (noto anche come [metodi di azione](http://rachelappel.com/asp.net-mvc-actionresults-explained)), ad esempio il `Index` metodo precedente, restituiscono in genere un' [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (o una classe derivata da [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), i tipi non primitivi come stringa.

Fare clic il *Views\HelloWorld* cartella e fare clic su **Add**, quindi fare clic su **pagina visualizzazione MVC 5 con Layout (Razor)**.
  
![](adding-a-view/_static/image1.png)   
  
Nel **Specifica nome per l'elemento** finestra di dialogo immettere *indice*, quindi fare clic su **OK**.  
  
![](adding-a-view/_static/image2.png)  
  
Nel **seleziona una pagina di Layout** finestra di dialogo, accettare il valore predefinito  **\_layout. cshtml** e fare clic su **OK**.  
  
![](adding-a-view/_static/image3.png)  
  
Nella finestra di dialogo precedente, il *Views\Shared* cartella sia selezionata nel riquadro sinistro. Se si dispone di un file di layout personalizzati in un'altra cartella, è possibile selezionarlo. Esaminiamo ora il file di layout in un secondo momento nell'esercitazione

Il *MvcMovie\Views\HelloWorld\Index.cshtml* file viene creato.

![](adding-a-view/_static/image4.png)

Aggiungere il markup evidenziato seguente.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Fare clic il *index. cshtml* del file e selezionare **Visualizza nel Browser**.

![PI](adding-a-view/_static/image5.png)

È anche possibile con il pulsante destro scegliere il *index. cshtml* del file e selezionare **Visualizza in controllo pagina.** Vedere le [esercitazione di controllo pagina](../../views/using-page-inspector-in-aspnet-mvc.md) per altre informazioni.

In alternativa, eseguire l'applicazione e selezionare il `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`). Il `Index` metodo nel controller di non esegue la quantità di lavoro, sufficiente eseguita l'istruzione `return View()`, quale specificato che il metodo deve usare un file di modello di visualizzazione per il rendering di una risposta nel browser. Perché è stato specificato in modo esplicito il nome del file del modello di visualizzazione da usare, ASP.NET MVC impostato sul valore predefinito usando il *index. cshtml* file di visualizzazione nel *\Views\HelloWorld* cartella. L'immagine seguente mostra la stringa &quot;Hello from our View Template!&quot; hardcoded nella vista.

![](adding-a-view/_static/image6.png)

Sembra abbastanza positivo. Si noti tuttavia che barra del titolo del browser mostra &quot;indice - My Appli ASP.NET "e il collegamento grande nella parte superiore della pagina indica"Application name". A seconda di come piccolo rendere la finestra del browser, potrebbe essere necessario fare clic sulle tre barre in alto a destra per visualizzare il per il **Home**, **sulle**, **contatto**, **Register** e **Accedi** collegamenti.

## <a name="changing-views-and-layout-pages"></a>Modifica delle viste e le pagine di Layout

In primo luogo, si desidera modificare il &quot;nome dell'applicazione&quot; collegamento nella parte superiore della pagina. Tale testo è comune a tutte le pagine. Viene effettivamente implementato in un'unica posizione nel progetto, anche se è presente in ogni pagina dell'applicazione. Passare al */viste/Shared* cartella **Esplora soluzioni** e aprire il  *\_layout. cshtml* file. Questo file viene chiamato un *pagina layout* e si trova nella cartella condivisa che utilizzano tutte le altre pagine.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Modelli di layout consentono di specificare il layout del contenitore HTML del sito in un'unica posizione e quindi applicarlo in più pagine del sito. Trovare la riga `@RenderBody()`. `RenderBody` è un segnaposto dove tutte le pagine specifiche della vista vengono presentate, &quot;incapsulate&quot; nella pagina di layout. Ad esempio, se si seleziona il **sulle** collegamento, il *Views\Home\About.cshtml* vista viene eseguita all'interno il `RenderBody` (metodo).

Modificare il contenuto dell'elemento titolo. Modifica il [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) nel modello di layout da &quot;nome applicazione&quot; al &quot;MVC Movie&quot; e il controller da `Home` a `Movies`. Il file di layout completo è illustrato di seguito:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Eseguire l'applicazione e si noti che ora dichiara &quot;MVC Movie &quot;. Fare clic sui **sulle** collegamento verrà visualizzato come pagina mostra &quot;MVC Movie&quot;anche. Abbiamo potuto apportare la modifica di una volta nel modello di layout e avere tutte le pagine nel sito riflettono il nuovo titolo.

![](adding-a-view/_static/image8.png)

Quando viene creato il *Views\HelloWorld\Index.cshtml* file, contenente il codice seguente:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Il codice Razor precedente è l'impostazione in modo esplicito la pagina di layout. Esaminare i *viste\\viewstart. cshtml* file, contiene il markup Razor stesso esatto. Il *[viste\\viewstart. cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* file definisce il layout comune che tutte le visualizzazioni useranno, pertanto è possibile inserire commenti out oppure rimuovere tale codice dal *Views\HelloWorld\ Index. cshtml* file.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

È possibile usare la proprietà `Layout` per impostare una vista di layout differente oppure impostarlo su `null` e quindi non verrà usato alcun file di layout.

A questo punto, è possibile modificare il titolo della visualizzazione Index.

Aprire *MvcMovie\Views\HelloWorld\Index.cshtml*. Esistono due modi per apportare una modifica: prima di tutto il testo che viene visualizzato nel titolo del browser, quindi nell'intestazione secondaria (la `<h2>` elemento). Renderli leggermente diversi in modo da poter esaminare la parte specifica di app modificata dal frammento specifico di codice.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Per indicare il titolo HTML da visualizzare, il codice precedente imposta una `Title` proprietà del `ViewBag` oggetto (che è nel *index. cshtml* modello di vista). Si noti che il modello di layout ( *Views\Shared\\layout. cshtml* ) viene utilizzato questo valore nel `<title>` come parte dell'elemento il `<head>` sezione del codice HTML che è stata modificata in precedenza.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Usando questo `ViewBag` approccio, è possibile facilmente passare altri parametri tra il modello di visualizzazione e il file di layout.

Eseguire l'applicazione. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server. Il titolo del browser viene creato con il `ViewBag.Title` impostato nel *index. cshtml* visualizzare modelli e gli altri &quot;-Movie App&quot; aggiunto nel file di layout.

Si noti inoltre come il contenuto nel *index. cshtml* modello di visualizzazione dopo il merge con il  *\_layout. cshtml* visualizzare il modello e una singola risposta HTML è stato inviato al browser. I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione.

![](adding-a-view/_static/image9.png)

La minima &quot;dati&quot; (in questo caso il &quot;Hello from our View Template!&quot; messaggio) è hardcoded, tuttavia. L'applicazione MVC ha un &quot;V&quot; (visualizzazione) e hai una &quot;C&quot; (controller), ma nessun &quot;M&quot; (model) ancora. A breve verrà illustrato come creare un database e recuperare i dati del modello da quest'ultimo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Prima di passare a un database e comunicare con informazioni sui modelli, tuttavia, esaminiamo innanzitutto il passaggio di informazioni dal controller a una visualizzazione. Le classi controller vengono richiamate in risposta a una richiesta URL in ingresso. Una classe controller è in cui viene scritto il codice che gestisce il browser in ingresso richieste, recupera i dati da un database e in definitiva determina il tipo di risposta da inviare al browser. I modelli di visualizzazione sono quindi utilizzabile per generare e formattare una risposta HTML al browser da un controller.

I controller sono responsabili di fornire i dati o oggetti sono necessari affinché un modello di vista eseguire il rendering di una risposta nel browser. Una procedura consigliata: **un modello di vista non deve mai eseguire logica di business o interagire direttamente con un database**. Al contrario, un modello di vista dovrebbe lavorare solo con i dati forniti dal controller. Mantenendo ciò &quot;la separazione dei compiti&quot; consente inoltre di mantenere il codice pulito, testabile e gestibile.

Attualmente, il `Welcome` metodo di azione il `HelloWorldController` classe accetta un `name` e un `numTimes` parametro e quindi genera i valori direttamente al browser. Anziché ottenere il rendering di questa risposta come stringa, cambiare il controller per usare invece un modello di vista. Il modello di vista genererà una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta. È possibile farlo facendo in modo che il controller inserisca i dati dinamici (parametri) che il modello di vista necessita in un `ViewBag` oggetto che può quindi accedere il modello di visualizzazione.

Tornare al *HelloWorldController.cs* di file e modificare le `Welcome` metodo per aggiungere un `Message` e `NumTimes` valore per il `ViewBag` oggetto. `ViewBag` è un oggetto dinamico, ovvero che è possibile inserire gli elementi desiderati da esso. il `ViewBag` oggetto non dispone di alcuna proprietà definite fino a quando non si inserisce un elemento al suo interno. Il [sistema di associazione di modelli ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticamente viene eseguito il mapping di parametri denominati (`name` e `numTimes`) dalla stringa di query nella barra degli indirizzi ai parametri nel metodo. Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

A questo punto il `ViewBag` oggetto contiene i dati che verranno passati automaticamente alla visualizzazione. Successivamente, è necessario un modello di vista Welcome! Nel **compilare** dal menu **Compila soluzione** (o Ctrl + MAIUSC + B) per assicurarsi che la compilazione del progetto. Fare clic il *Views\HelloWorld* cartella e fare clic su **Add**, quindi fare clic su **pagina visualizzazione MVC 5 con Layout (Razor)**.
  
![](adding-a-view/_static/image10.png)   
  
Nel **Specifica nome per l'elemento** finestra di dialogo immettere *benvenuto*, quindi fare clic su **OK**.   
  
Nel **seleziona una pagina di Layout** finestra di dialogo, accettare il valore predefinito  **\_layout. cshtml** e fare clic su **OK**.  
  
![](adding-a-view/_static/image11.png)   

Il *MvcMovie\Views\HelloWorld\Welcome.cshtml* file viene creato.

Sostituire il markup nel *Welcome* file. Si creerà un ciclo con la dicitura &quot;Hello&quot; come tutte le volte che l'utente afferma quanto previsto. L'intero *Welcome* file è illustrato di seguito.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Eseguire l'applicazione e passare all'URL seguente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

A questo punto i dati vengono prelevati dall'URL e passati al controller usando il [dello strumento di associazione del modello](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Il controller inserisce i dati in un `ViewBag` oggetto e passa tale oggetto alla vista. La vista visualizza quindi i dati in formato HTML all'utente.

![](adding-a-view/_static/image12.png)

Nell'esempio precedente, abbiamo utilizzato un `ViewBag` oggetto per passare i dati dal controller a una visualizzazione. Più avanti nell'esercitazione si userà un modello di vista per passare i dati da un controller a una vista. L'approccio di modello di vista per passare i dati è generalmente molto preferito rispetto all'approccio di contenitore di visualizzazione. Vedere il post di blog [V fortemente tipizzate viste dinamiche](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) per altre informazioni. 

Era un tipo di un' &quot;M&quot; per modello, ma non il tipo di database. Creare un database di film con i concetti appresi.

> [!div class="step-by-step"]
> [Precedente](adding-a-controller.md)
> [Successivo](adding-a-model.md)
