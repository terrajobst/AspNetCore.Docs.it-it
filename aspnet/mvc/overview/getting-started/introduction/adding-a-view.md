---
title: Aggiunta di una vista a un'applicazione MVC
author: Rick-Anderson
description: Aggiunta di una vista a un'applicazione MVC
ms.author: riande
manager: wpickett
ms.date: 09/1721/2017
ms.topic: article
ms.technology: dotnet-mvc
ms.prod: .net-framework
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 21db97e635b5db580df31f46ca7f8b60a80d6f94
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "30873430"
---
<a name="adding-a-view"></a>Aggiunta di una vista
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In questa sezione si intende modificare il `HelloWorldController` classe per utilizzare la visualizzazione dei file di modello per correttamente incapsulano il processo di generazione di risposte HTML a un client. 

Si creerà un file modello di visualizzazione utilizzando il [motore di visualizzazione Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). I modelli di visualizzazione in base Razor presentano un *. cshtml* estensione di file e fornire un modo elegante di creazione di un output che utilizza c# HTML. Razor riduce al minimo il numero di caratteri e sequenze di tasti richieste durante la scrittura di un modello di visualizzazione e consente un fluido veloce, la codifica del flusso di lavoro.

Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller. Modifica il `Index` per restituire un `View` dell'oggetto, come illustrato nel codice seguente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

Il `Index` metodo precedente Usa un modello di visualizzazione per generare una risposta HTML al browser. I metodi del controller (noto anche come [metodi di azione](http://rachelappel.com/asp.net-mvc-actionresults-explained)), ad esempio il `Index` dei metodi descritti sopra, restituiscono in genere un [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (o una classe derivata da [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), i tipi non primitivi come stringa.

Fare clic destro la *Views\HelloWorld* cartella e fare clic su **Aggiungi**, quindi fare clic su **pagina visualizzazione MVC 5 con Layout (Razor)**.
  
![](adding-a-view/_static/image1.png)   
  
Nel **Specifica nome per l'elemento** finestra di dialogo immettere *indice*, quindi fare clic su **OK**.  
  
![](adding-a-view/_static/image2.png)  
  
Nel **selezionare una pagina di Layout** finestra di dialogo, accettare il valore predefinito  **\_cshtml** e fare clic su **OK**.  
  
![](adding-a-view/_static/image3.png)  
  
Nella finestra di dialogo precedente, il *Views\Shared* cartella sia selezionata nel riquadro a sinistra. Se si dispone di un file di layout personalizzato in un'altra cartella, è possibile selezionarlo. Più avanti nell'esercitazione verrà descritto come il file di layout

Il *MvcMovie\Views\HelloWorld\Index.cshtml* file viene creato.

![](adding-a-view/_static/image4.png)

Aggiungere il seguente markup evidenziato.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Fare clic destro la *cshtml* file e selezionare **Visualizza nel Browser**.

![PI](adding-a-view/_static/image5.png)

È possibile anche con il pulsante destro scegliere il *cshtml* del file e selezionare **Visualizza in controllo pagina.** Vedere il [esercitazione controllo pagina](../../views/using-page-inspector-in-aspnet-mvc.md) per ulteriori informazioni.

In alternativa, eseguire l'applicazione e individuare il `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`). Il `Index` metodo nel controller di non eseguire la quantità di lavoro, sufficiente eseguita l'istruzione `return View()`, quale specificato che il metodo deve usare un file di modello di visualizzazione per il rendering di una risposta nel browser. Poiché è stato specificato in modo esplicito il nome del file modello di visualizzazione da utilizzare, ASP.NET MVC impostato sul valore predefinito utilizzando il *cshtml* Visualizza file nel *\Views\HelloWorld* cartella. L'immagine seguente mostra la stringa &quot;Hello da questo modello di visualizzazione!&quot; hardcoded nella vista.

![](adding-a-view/_static/image6.png)

È buona. Si noti tuttavia che Mostra barra del titolo del browser &quot;Index - My Appli ASP.NET "e il collegamento grande nella parte superiore della pagina è indicato"Nome applicazione". A seconda di come ridotto si apporta la finestra del browser, potrebbe essere necessario fare clic su tre barre in alto a destra per visualizzare il per il **Home**, **su**, **contatto**, **Registrare** e **Accedi** collegamenti.

## <a name="changing-views-and-layout-pages"></a>Modifica delle visualizzazioni e pagine di Layout

In primo luogo, si desidera modificare il &quot;nome applicazione&quot; collegamento nella parte superiore della pagina. Il testo è comune a ogni pagina. Viene effettivamente implementata in un'unica posizione nel progetto, anche se è presente in ogni pagina nell'applicazione. Passare al */visualizzazioni/Shared* cartella **Esplora** e aprire il  *\_cshtml* file. Questo file viene chiamato un *pagina layout* sia nella cartella condivisa che utilizzano tutte le altre pagine.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Modelli di layout consentono di specificare il layout di contenitore HTML del sito in un'unica posizione e quindi applicarlo in più pagine del sito. Trovare la riga `@RenderBody()`. `RenderBody` è un segnaposto dove tutte le pagine specifiche della vista vengono presentate, &quot;incapsulate&quot; nella pagina di layout. Ad esempio, se si seleziona il **su** collegamento, il *Views\Home\About.cshtml* vista viene eseguita all'interno di `RenderBody` (metodo).

Modificare il contenuto dell'elemento titolo. Modifica il [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) nel modello di layout da &quot;nome applicazione&quot; a &quot;MVC film&quot; e il controller da `Home` a `Movies`. Il file di layout completo è illustrato di seguito:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Eseguire l'applicazione e si noti che ora dichiara &quot;MVC film &quot;. Fare clic su di **su** collegamento e vedere la pagina viene &quot;MVC film&quot;, troppo. Abbiamo potuto apportare la modifica di una volta nel modello di layout e dispone di tutte le pagine nel sito di riflettere il nuovo titolo.

![](adding-a-view/_static/image8.png)

Quando abbiamo creato prima il *Views\HelloWorld\Index.cshtml* file contiene il codice seguente:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Il codice Razor precedente in modo esplicito nell'impostazione di pagina di layout. Esaminare il *viste\\viewstart* file, che contiene gli stessi tag Razor esatto. Il *[viste\\viewstart](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* file definisce il layout comune che utilizzeranno tutte le visualizzazioni, pertanto è possibile commentare quel codice da rimuovere o viene disattivata la *Views\HelloWorld\ Cshtml* file.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

È possibile usare la proprietà `Layout` per impostare una vista di layout differente oppure impostarlo su `null` e quindi non verrà usato alcun file di layout.

A questo punto, è necessario modificare il titolo della visualizzazione dell'indice.

Aprire *MvcMovie\Views\HelloWorld\Index.cshtml*. Esistono due posizioni di apportare una modifica: in primo luogo, il testo che viene visualizzato nel titolo del browser, quindi nell'intestazione secondaria (la `<h2>` elemento). Renderli leggermente diversi in modo da poter esaminare la parte specifica di app modificata dal frammento specifico di codice.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Per indicare il titolo HTML da visualizzare, il codice di sopra di set di un `Title` proprietà del `ViewBag` oggetto (che è il *cshtml* modello di visualizzazione). Si noti che il modello di layout ( *Views\Shared\\layout. cshtml* ) utilizza questo valore nel `<title>` come parte dell'elemento di `<head>` sezione del codice HTML che ha in precedenza.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Usando questa `ViewBag` approccio, è possibile facilmente passare altri parametri tra il modello di visualizzazione e il file di layout.

Eseguire l'applicazione. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server. Il titolo del browser viene creato con il `ViewBag.Title` è impostato *cshtml* visualizzare modelli e gli altri &quot;-App cinematografica&quot; aggiunto nel file di layout.

Si noti inoltre come il contenuto nel *cshtml* vista modello è stato unito il  *\_cshtml* modello di visualizzazione e una singola risposta HTML è stata inviata al browser. I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione.

![](adding-a-view/_static/image9.png)

Il numero minimo di &quot;dati&quot; (in questo caso il &quot;Hello da questo modello di visualizzazione!&quot; messaggio) è hardcoded, tuttavia. L'applicazione MVC con un &quot;V&quot; (visualizzazione) e hai una &quot;C&quot; (controller), ma non &quot;M&quot; (modello) ancora. Più avanti, verranno esaminati come creare un database e recuperare i dati del modello.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Prima di passare a un database e descrivere i modelli, tuttavia, esaminiamo innanzitutto il passaggio di informazioni a una visualizzazione dal controller. Classi controller vengono richiamate in risposta a una richiesta URL in ingresso. Una classe controller viene scritto il codice che gestisce il browser in arrivo delle richieste, recupera i dati da un database e infine decide il tipo di risposta da inviare al browser. Modelli di visualizzazione quindi utilizzabile da un controller per generare e formattare una risposta HTML al browser.

I controller sono responsabili di fornire qualsiasi dati o gli oggetti necessari affinché un modello di visualizzazione per il rendering di una risposta nel browser. Una procedura consigliata: **un modello di visualizzazione non deve mai eseguire la logica di business o interagire direttamente con un database**. Al contrario, un modello di visualizzazione dovrebbe funzionare solo con i dati che vengano forniti dal controller. Gestione di questo &quot;separazione delle problematiche&quot; consente di mantenere il codice pulito, testabile e più gestibili.

Attualmente, il `Welcome` metodo di azione il `HelloWorldController` classe accetta un `name` e `numTimes` parametro e quindi i valori direttamente al browser di output. Per evitare che il controller di eseguire il rendering questa risposta sotto forma di stringa, è necessario modificare il controller per utilizzare invece un modello di visualizzazione. Il modello di vista genererà una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta. È possibile farlo con il controller di inserire i dati dinamici (parametri) necessarie per il modello di visualizzazione in un `ViewBag` oggetto che può quindi accedere il modello di visualizzazione.

Restituito per il *HelloWorldController.cs* file e modificare il `Welcome` metodo per aggiungere un `Message` e `NumTimes` valore per il `ViewBag` oggetto. `ViewBag` è un oggetto dinamico, ovvero che è possibile inserire desiderati. il `ViewBag` oggetto dispone di alcuna proprietà definito fino a quando non si inserisce un elemento all'interno. Il [sistema di associazione del modello MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) esegue automaticamente il mapping di parametri denominati (`name` e `numTimes`) dalla stringa di query nella barra degli indirizzi per i parametri del metodo. Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

A questo punto il `ViewBag` oggetto contiene i dati che verranno passati alla visualizzazione automaticamente. Successivamente, è necessario un modello di visualizzazione iniziale. Nel **compilare** dal menu **Compila soluzione** (o Ctrl + MAIUSC + B) per assicurarsi la compilazione del progetto. Fare clic destro la *Views\HelloWorld* cartella e fare clic su **Aggiungi**, quindi fare clic su **pagina visualizzazione MVC 5 con Layout (Razor)**.
  
![](adding-a-view/_static/image10.png)   
  
Nel **Specifica nome per l'elemento** finestra di dialogo immettere *iniziale*, quindi fare clic su **OK**.   
  
Nel **selezionare una pagina di Layout** finestra di dialogo, accettare il valore predefinito  **\_cshtml** e fare clic su **OK**.  
  
![](adding-a-view/_static/image11.png)   

Il *MvcMovie\Views\HelloWorld\Welcome.cshtml* file viene creato.

Sostituire il markup di *Welcome.cshtml* file. Si creerà un ciclo che afferma &quot;Hello&quot; le volte che l'utente è indicato come previsto. L'intero *Welcome.cshtml* file è illustrato di seguito.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Eseguire l'applicazione e passare all'URL seguente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ora i dati vengono eseguiti dall'URL e passati al controller utilizzando il [dello strumento di associazione del modello](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Il controller di pacchetti di dati in un `ViewBag` oggetto e passa tale oggetto nella vista. La vista visualizza quindi i dati in formato HTML per l'utente.

![](adding-a-view/_static/image12.png)

Nell'esempio precedente, è stato usato un `ViewBag` oggetto per passare i dati dal controller per una vista. Più avanti nell'esercitazione si userà un modello di vista per passare i dati da un controller a una vista. L'approccio di modello di visualizzazione per il passaggio di dati è in genere consigliabile rispetto all'approccio di visualizzazione elenco. Vedere il post di blog [V fortemente tipizzato visualizzazioni dinamiche](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) per ulteriori informazioni. 

Che è un tipo di un &quot;M&quot; per modello, ma non il tipo di database. Creare un database di film con i concetti appresi.

> [!div class="step-by-step"]
> [Precedente](adding-a-controller.md)
> [Successivo](adding-a-model.md)
