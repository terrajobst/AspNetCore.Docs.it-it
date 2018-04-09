---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Aggiunta di un Controller | Documenti Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 3864bab284661b0c44f9e4cb363c2d60eccc7c66
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller"></a>Aggiunta di un controller
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC è l'acronimo di *model-view-controller*. MVC è un modello per lo sviluppo di applicazioni che sono ben strutturato, testabile e facili da gestire. Applicazioni basate su MVC contengono:

- **M** odels: le classi che rappresentano i dati dell'applicazione e che usano la logica di convalida per applicare le regole di business per tali dati.
- **V** iews: file di modello che l'applicazione usa per generare in modo dinamico le risposte HTML.
- **C** ontrollers: le classi che gestiscono le richieste del browser in ingresso, recuperare i dati del modello e quindi specificare i modelli di visualizzazione che restituiscono una risposta nel browser.

Si verrà tutti questi concetti in questa serie di esercitazioni di copertura e viene illustrato come usarle per compilare un'applicazione.

> [!NOTE]
> Il modello è stato selezionato nel passaggio precedente il MVC predefinito. Crea Home, Account e gestire i controller per impostazione predefinita.

Si inizierà creando una classe controller. In **Esplora**, fare doppio clic su di *controller* cartella e quindi fare clic su **Aggiungi**, quindi **Controller**.


![](adding-a-controller/_static/image1.png)

Nel **aggiungere lo scaffolding** nella finestra di dialogo fare clic su **Controller MVC 5 - vuoto**, quindi fare clic su **Aggiungi**.

![](adding-a-controller/_static/image2.png)  
 

Il nuovo controller "HelloWorldController" e fare clic su **Aggiungi**.

![Aggiungi controller](adding-a-controller/_static/image3.png)

Si noti che nel **Esplora** che un nuovo file è stato creato denominato *HelloWorldController.cs* e una nuova cartella *Views\HelloWorld*. Il controller è aperto nell'IDE.

![](adding-a-controller/_static/image4.png)

Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

I metodi di controller verranno restituita una stringa HTML come esempio. Il controller è denominato `HelloWorldController` e il primo metodo è denominato `Index`. Verrà richiamato da un browser. Eseguire l'applicazione (premere F5 o Ctrl + F5). Nel browser, aggiungere &quot;HelloWorld&quot; per il percorso nella barra degli indirizzi. (Ad esempio, nella figura seguente, la `http://localhost:1234/HelloWorld.`) la pagina nel browser apparirà la schermata seguente. Nel metodo, il codice restituito direttamente una stringa. È indicato il sistema da restituire solo il codice HTML e aveva!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC richiama classi controller diverso (e i metodi di azione diversi all'interno di essi) a seconda dell'URL in ingresso. La logica di routing URL predefinito utilizzata da MVC ASP.NET utilizza un formato simile al seguente per determinare quale codice per richiamare:

`/[Controller]/[ActionName]/[Parameters]`

Impostare il formato per il routing di *App\_Start/RouteConfig.cs* file.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Quando si esegue l'applicazione e non forniscono eventuali segmenti di URL, il valore predefinito è il controller "Home" e il metodo di azione "Index" specificato nella sezione Impostazioni predefinite del codice precedente.

La prima parte dell'URL determina la classe controller da eseguire. Pertanto, */HelloWorld* esegue il mapping alla `HelloWorldController` classe. La seconda parte dell'URL determina il metodo di azione della classe per l'esecuzione. Così */HelloWorld/indice* causerebbe il `Index` metodo il `HelloWorldController` classe da eseguire. Si noti che abbiamo solo passare a */HelloWorld* e `Index` metodo utilizzato per impostazione predefinita. In questo modo un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller non è specificata in modo esplicito. La terza parte del segmento di URL ( `Parameters`) è relativa ai dati di route. Dati della route verrà illustrata più avanti in questa esercitazione.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il `Welcome` metodo viene eseguito e restituisce la stringa &quot;si tratta del metodo di azione di completamento dell'installazione... &quot;. Il mapping di MVC predefinita è `/[Controller]/[ActionName]/[Parameters]`. Per questo URL, il controller è `HelloWorld` e il metodo di azione è `Welcome`. Non è stata ancora usata la parte `[Parameters]` dell'URL.

![](adding-a-controller/_static/image6.png)

Modifichiamo l'esempio leggermente, in modo che è possibile passare alcune informazioni sui parametri dall'URL per il controller (ad esempio, */HelloWorld/iniziale? name = Scott&amp;numtimes = 4*). Modifica il `Welcome` metodo includere due parametri, come illustrato di seguito. Si noti che il codice Usa la funzionalità di c# parametro facoltativo per indicare che il `numTimes` parametro deve essere predefinito su 1 se per tale parametro viene passato alcun valore.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Nota sulla sicurezza: Il codice precedente viene [HttpUtility](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) per proteggere l'applicazione da input dannosi (vale a dire JavaScript). Per ulteriori informazioni vedere [procedura: proteggere dagli attacchi tramite Script in un'applicazione Web da applicare la codifica HTML alle stringhe](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Eseguire l'applicazione e passare all'URL di esempio (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). È possibile provare diversi valori per `name` e `numtimes` nell'URL. Il [sistema di associazione del modello MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) associa automaticamente i parametri denominati dalla stringa di query nella barra degli indirizzi per i parametri del metodo.

![](adding-a-controller/_static/image7.png)

Nell'esempio precedente, il segmento URL ( `Parameters`) non viene utilizzato il `name` e `numTimes` i parametri vengono passati come [delle stringhe di query](http://en.wikipedia.org/wiki/Query_string). Il carattere jolly ? (punto interrogativo) nell'URL precedente è un separatore e seguono le stringhe di query. Il carattere &amp; separa le stringhe di query.

Sostituire il metodo di completamento dell'installazione con il codice seguente:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Eseguire l'applicazione e immettere l'URL seguente: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Questa volta il terzo segmento di URL corrispondente parametro di route `ID.` il `Welcome` il metodo di azione contiene un parametro (`ID`) corrispondenti della specifica URL di `RegisterRoutes` (metodo).

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

Nelle applicazioni ASP.NET MVC, è più comune di comunicazione per passare i parametri come dati di route (come abbiamo visto con ID precedente) di essere passate come stringhe di query. È inoltre possibile aggiungere una route per soddisfare entrambi i `name` e `numtimes` nei parametri come dati di route nell'URL. Nel *App\_Start\RouteConfig.cs* file, aggiungere la route "Hello":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Eseguire l'applicazione e passare a `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Per molte applicazioni MVC, la route predefinita funziona correttamente. Verrà illustrato più avanti in questa esercitazione per passare i dati utilizzando lo strumento di associazione del modello e non sarà necessario modificare la route predefinita per tale.

In questi esempi di operazioni il controller del &quot;VC&quot; parte di MVC, vale a dire, il lavoro di visualizzazione e controller. Il controller ha restituito HTML direttamente. In genere non si desidera controller restituzione HTML direttamente, dal momento che diventa molto complessa al codice. Invece in genere si userà un file di modello di visualizzazione separato per generare la risposta HTML. Verrà ora esaminato il successivo come possiamo farlo.

> [!div class="step-by-step"]
> [Precedente](getting-started.md)
> [Successivo](adding-a-view.md)
