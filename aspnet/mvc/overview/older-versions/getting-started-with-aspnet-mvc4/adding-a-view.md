---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Aggiunta di una vista | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro e molto più semplice da seguire e demo...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7c7179419a126eaa998252e75ac38b3b1a2b23d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830983"
---
<a name="adding-a-view"></a>Aggiunta di una vista
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.


In questa sezione si intende modificare il `HelloWorldController` classe utilizzare file di modello per correttamente il processo di generazione di risposte HTML a un client di incapsulare la visualizzazione.

Si creerà un file modello di visualizzazione usando la [motore di visualizzazione Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introdotte con ASP.NET MVC 3. I modelli di vista basati su Razor hanno una *cshtml* estensione di file e fornire un modo elegante per creare HTML di output usando c#. Riduce al minimo il numero di caratteri e sequenze di tasti richieste durante la scrittura di un modello di vista Razor e consente a un veloce, fluido codifica del flusso di lavoro.

Attualmente il metodo `Index` restituisce una stringa con un messaggio hardcoded nella classe controller. Modifica il `Index` per restituire un `View` dell'oggetto, come illustrato nel codice seguente:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Il `Index` metodo precedente Usa un modello di vista per generare una risposta HTML al browser. I metodi del controller (noto anche come [metodi di azione](http://rachelappel.com/asp.net-mvc-actionresults-explained)), ad esempio il `Index` metodo precedente, restituiscono in genere un' [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (o una classe derivata da [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), i tipi non primitivi come stringa.

Nel progetto, aggiungere un modello di visualizzazione che è possibile usare con il `Index` (metodo). A tale scopo, fare doppio clic all'interno di `Index` (metodo) e fare clic su **Aggiungi visualizzazione**.

![](adding-a-view/_static/image1.png)

Il **Aggiungi visualizzazione** verrà visualizzata la finestra di dialogo. Lasciare le impostazioni predefinite di quelle fornite sono e fare clic sui **Add** pulsante:

![](adding-a-view/_static/image2.png)

Il *MvcMovie\Views\HelloWorld* cartella e il *MvcMovie\Views\HelloWorld\Index.cshtml* file vengono creati. È possibile visualizzarli nella **Esplora soluzioni**:

![](adding-a-view/_static/image3.png)

Il seguente viene illustrato il *index. cshtml* file creato:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Aggiungere il codice HTML seguente sotto il `<h2>` tag.

[!code-html[Main](adding-a-view/samples/sample2.html)]

L'intero *MvcMovie\Views\HelloWorld\Index.cshtml* file è illustrato di seguito.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Se si usa Visual Studio 2012, in Esplora soluzioni, fare clic il *index. cshtml* del file e selezionare **Visualizza in controllo pagina**.

![PI](adding-a-view/_static/image5.png)

Il [esercitazione di controllo pagina](../../views/using-page-inspector-in-aspnet-mvc.md) include altre informazioni su questo nuovo strumento.

In alternativa, eseguire l'applicazione e selezionare il `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`). Il `Index` metodo nel controller di non esegue la quantità di lavoro, sufficiente eseguita l'istruzione `return View()`, quale specificato che il metodo deve usare un file di modello di visualizzazione per il rendering di una risposta nel browser. Perché è stato specificato in modo esplicito il nome del file del modello di visualizzazione da usare, ASP.NET MVC impostato sul valore predefinito usando il *index. cshtml* file di visualizzazione nel *\Views\HelloWorld* cartella. L'immagine seguente mostra la stringa &quot;Hello from our View Template!&quot; hardcoded nella vista.

![](adding-a-view/_static/image6.png)

Sembra abbastanza positivo. Si noti tuttavia che barra del titolo del browser mostra &quot;personali A ASP.NET di indice&quot; e il collegamento grande nella parte superiore della pagina indica &quot;inserire qui il logo.&quot; Di seguito il &quot;qui il logo.&quot; collegamento registrazione e log nei collegamenti e di seguito che si collega a casa, circa e contatto pagine. È possibile modificare alcune di queste.

## <a name="changing-views-and-layout-pages"></a>Modifica delle viste e le pagine di Layout

In primo luogo, si desidera modificare il &quot;qui il logo.&quot; titolo nella parte superiore della pagina. Tale testo è comune a tutte le pagine. Viene effettivamente implementato in un'unica posizione nel progetto, anche se è presente in ogni pagina dell'applicazione. Passare al */viste/Shared* cartella **Esplora soluzioni** e aprire il  *\_layout. cshtml* file. Questo file viene chiamato un *pagina layout* ed è condiviso &quot;shell&quot; che utilizzano tutte le altre pagine.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Modelli di layout consentono di specificare il layout del contenitore HTML del sito in un'unica posizione e quindi applicarlo in più pagine del sito. Trovare la riga `@RenderBody()`. `RenderBody` è un segnaposto dove tutte le pagine specifiche della vista vengono presentate, &quot;incapsulate&quot; nella pagina di layout. Ad esempio, se si seleziona il collegamento About, il *Views\Home\About.cshtml* vista viene eseguita all'interno di `RenderBody` (metodo).

Modificare l'intestazione del titolo del sito nel modello di layout da &quot;inserire qui il logo&quot; al &quot;MVC Movie&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Sostituire il contenuto dell'elemento title con il markup seguente:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Eseguire l'applicazione e si noti che ora dichiara &quot;MVC Movie &quot;. Fare clic sui **sulle** collegamento verrà visualizzato come pagina mostra &quot;MVC Movie&quot;anche. Abbiamo potuto apportare la modifica di una volta nel modello di layout e avere tutte le pagine nel sito riflettono il nuovo titolo.

![](adding-a-view/_static/image8.png)

A questo punto, è possibile modificare il titolo della visualizzazione Index.

Aprire *MvcMovie\Views\HelloWorld\Index.cshtml*. Esistono due modi per apportare una modifica: prima di tutto il testo che viene visualizzato nel titolo del browser, quindi nell'intestazione secondaria (la `<h2>` elemento). Renderli leggermente diversi in modo da poter esaminare la parte specifica di app modificata dal frammento specifico di codice.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

Per indicare il titolo HTML da visualizzare, il codice precedente imposta una `Title` proprietà del `ViewBag` oggetto (che è nel *index. cshtml* modello di vista). Se si osserva nuovamente il codice sorgente del modello di layout, si noterà che il modello Usa questo valore nel `<title>` come parte dell'elemento di `<head>` sezione del codice HTML che è stata modificata in precedenza. Usando questo `ViewBag` approccio, è possibile facilmente passare altri parametri tra il modello di visualizzazione e il file di layout.

Eseguire l'applicazione e passare a `http://localhost:xx/HelloWorld`. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. Se le modifiche non sono visibili nel browser, è possibile che si stia visualizzando il contenuto memorizzato nella cache. Premere CTRL + F5 nel browser per forzare il caricamento della risposta dal server. Il titolo del browser viene creato con il `ViewBag.Title` impostato nel *index. cshtml* visualizzare modelli e gli altri &quot;-Movie App&quot; aggiunto nel file di layout.

Si noti inoltre come il contenuto nel *index. cshtml* modello di visualizzazione dopo il merge con il  *\_layout. cshtml* visualizzare il modello e una singola risposta HTML è stato inviato al browser. I modelli di layout rendono molto semplice apportare modifiche che si applicano a tutte le pagine dell'applicazione.

![](adding-a-view/_static/image9.png)

La minima &quot;dati&quot; (in questo caso il &quot;Hello from our View Template!&quot; messaggio) è hardcoded, tuttavia. L'applicazione MVC ha un &quot;V&quot; (visualizzazione) e hai una &quot;C&quot; (controller), ma nessun &quot;M&quot; (model) ancora. A breve verrà illustrato come creare un database e recuperare i dati del modello da quest'ultimo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passaggio di dati dal controller alla vista

Prima di passare a un database e comunicare con informazioni sui modelli, tuttavia, esaminiamo innanzitutto il passaggio di informazioni dal controller a una visualizzazione. Le classi controller vengono richiamate in risposta a una richiesta URL in ingresso. Una classe controller è in cui viene scritto il codice che gestisce il browser in ingresso richieste, recupera i dati da un database e in definitiva determina il tipo di risposta da inviare al browser. I modelli di visualizzazione sono quindi utilizzabile per generare e formattare una risposta HTML al browser da un controller.

I controller sono responsabili di fornire i dati o oggetti sono necessari affinché un modello di vista eseguire il rendering di una risposta nel browser. Una procedura consigliata: **un modello di vista non deve mai eseguire logica di business o interagire direttamente con un database**. Al contrario, un modello di vista dovrebbe lavorare solo con i dati forniti dal controller. Mantenendo ciò &quot;la separazione dei compiti&quot; consente inoltre di mantenere il codice pulito, testabile e gestibile.

Attualmente, il `Welcome` metodo di azione il `HelloWorldController` classe accetta un `name` e un `numTimes` parametro e quindi genera i valori direttamente al browser. Anziché ottenere il rendering di questa risposta come stringa, cambiare il controller per usare invece un modello di vista. Il modello di vista genererà una risposta dinamica, il che significa che è necessario passare i bit di dati appropriati dal controller alla vista per generare la risposta. È possibile farlo facendo in modo che il controller inserisca i dati dinamici (parametri) che il modello di vista necessita in un `ViewBag` oggetto che può quindi accedere il modello di visualizzazione.

Tornare al *HelloWorldController.cs* di file e modificare le `Welcome` metodo per aggiungere un `Message` e `NumTimes` valore per il `ViewBag` oggetto. `ViewBag` è un oggetto dinamico, ovvero che è possibile inserire gli elementi desiderati da esso. il `ViewBag` oggetto non dispone di alcuna proprietà definite fino a quando non si inserisce un elemento al suo interno. Il [sistema di associazione di modelli ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticamente viene eseguito il mapping di parametri denominati (`name` e `numTimes`) dalla stringa di query nella barra degli indirizzi ai parametri nel metodo. Il file *HelloWorldController.cs* completo avrà un aspetto simile al seguente:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

A questo punto il `ViewBag` oggetto contiene i dati che verranno passati automaticamente alla visualizzazione.

Successivamente, è necessario un modello di vista Welcome! Nel **compilare** dal menu **compilazione MvcMovie** per assicurarsi che la compilazione del progetto.

Quindi fare doppio clic all'interno di `Welcome` (metodo) e fare clic su **Aggiungi visualizzazione**.

![](adding-a-view/_static/image10.png)

Questa figura viene illustrata la **Aggiungi visualizzazione** nella finestra di dialogo è simile a:

![](adding-a-view/_static/image11.png)

Fare clic su **Add**e quindi aggiungere il codice seguente sotto il `<h2>` nuovo elemento *Welcome* file. Si creerà un ciclo con la dicitura &quot;Hello&quot; come tutte le volte che l'utente afferma quanto previsto. L'intero *Welcome* file è illustrato di seguito.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Eseguire l'applicazione e passare all'URL seguente:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

A questo punto i dati vengono prelevati dall'URL e passati al controller usando il [dello strumento di associazione del modello](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Il controller inserisce i dati in un `ViewBag` oggetto e passa tale oggetto alla vista. La vista visualizza quindi i dati in formato HTML all'utente.

![](adding-a-view/_static/image12.png)

Nell'esempio precedente, abbiamo utilizzato un `ViewBag` oggetto per passare i dati dal controller a una visualizzazione. Se quest'ultimo, in questa esercitazione, si userà un modello di visualizzazione per passare i dati da un controller a una visualizzazione. L'approccio di modello di vista per passare i dati è generalmente molto preferito rispetto all'approccio di contenitore di visualizzazione. Vedere il post di blog [V fortemente tipizzate viste dinamiche](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) per altre informazioni.

Era un tipo di un' &quot;M&quot; per modello, ma non il tipo di database. Creare un database di film con i concetti appresi.

> [!div class="step-by-step"]
> [Precedente](adding-a-controller.md)
> [Successivo](adding-a-model.md)
