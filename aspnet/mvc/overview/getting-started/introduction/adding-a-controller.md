---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Aggiunta di un Controller | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 77389bfa4774857eb2a607b0a70e982826312e03
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38119263"
---
<a name="adding-a-controller"></a>Aggiunta di un controller
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC è l'acronimo *model-view-controller*. MVC è un modello per lo sviluppo di applicazioni che sono ben strutturata, testabile e facile da gestire. Le applicazioni basate su MVC contengono:

- **M** odelli: classi che rappresentano i dati dell'applicazione e che usano la logica di convalida per applicare le regole di business per tali dati.
- **V** iste: file di modello che l'applicazione usa per generare in modo dinamico le risposte HTML.
- **C** ontroller: classi che gestiscono le richieste in ingresso del browser, recuperare i dati del modello e quindi specificare i modelli di vista che restituiscono una risposta nel browser.

Si verrà che coprono tutti questi concetti in questa serie di esercitazioni e mostrerò come usarle per compilare un'applicazione.

> [!NOTE]
> Nel passaggio precedente MVC predefinita modello è stato selezionato. Ciò crea Home, Account e gestire i controller per impostazione predefinita.

È innanzitutto necessario creare una classe controller. In **Esplora soluzioni**, fare doppio clic il *controller* cartella e quindi fare clic su **Add**, quindi **Controller**.


![](adding-a-controller/_static/image1.png)

Nel **Add Scaffold** della finestra di dialogo fare clic su **Controller MVC 5 - vuoto**, quindi fare clic su **Aggiungi**.

![](adding-a-controller/_static/image2.png)  
 

Assegnare un nome al controller "HelloWorldController" e fare clic su **Add**.

![Aggiungi controller](adding-a-controller/_static/image3.png)

Si noti che nel **Esplora soluzioni** che un nuovo file creato denominato *HelloWorldController.cs* e una nuova cartella *Views\HelloWorld*. Il controller è aperto nell'IDE.

![](adding-a-controller/_static/image4.png)

Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

I metodi del controller restituirà una stringa del codice HTML come esempio. Il controller è denominato `HelloWorldController` e il primo metodo è denominato `Index`. È possibile richiamarla da un browser. Eseguire l'applicazione (premere F5 o CTRL+F5). Nel browser, accodare &quot;HelloWorld&quot; al percorso nella barra degli indirizzi. (Ad esempio, nell'illustrazione seguente, relativo `http://localhost:1234/HelloWorld.`) la pagina nel browser avrà un aspetto simile allo screenshot seguente. Nel metodo precedente, il codice restituito direttamente una stringa. È indicato il sistema deve restituire solo del codice HTML e ha sempre fatto!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC richiama le classi controller diverso (e i metodi di azione diverso in esse contenute) a seconda dell'URL in ingresso. La logica di routing URL predefinita utilizzata da ASP.NET MVC Usa un formato simile al seguente per determinare quale codice per richiamare:

`/[Controller]/[ActionName]/[Parameters]`

Il formato per il routing viene impostato il *App\_Start/RouteConfig.cs* file.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Quando si esegue l'applicazione e non si forniscono i segmenti di URL, l'impostazione predefinita è il controller "Home" e il metodo di azione "Index" specificati nella sezione Impostazioni predefinite del codice precedente.

La prima parte dell'URL determina la classe controller da eseguire. Così */HelloWorld* esegue il mapping al `HelloWorldController` classe. La seconda parte dell'URL determina il metodo di azione per la classe da eseguire. Così */HelloWorld/Index* causerebbe il `Index` metodo del `HelloWorldController` classe da eseguire. Si noti che è necessario solo passare a */HelloWorld* e il `Index` metodo utilizzato per impostazione predefinita. Infatti, un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller se non ne viene specificato in modo esplicito. La terza parte del segmento di URL ( `Parameters`) è relativa ai dati di route. Vedremo i dati della route più avanti in questa esercitazione.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il `Welcome` metodo viene eseguito e restituisce la stringa &quot;si tratta del metodo di azione iniziale... &quot;. Il mapping di MVC predefinita è `/[Controller]/[ActionName]/[Parameters]`. Per questo URL, il controller è `HelloWorld` e il metodo di azione è `Welcome`. Non è stata ancora usata la parte `[Parameters]` dell'URL.

![](adding-a-controller/_static/image6.png)

È possibile modificare leggermente l'esempio in modo che è possibile passare le informazioni dei parametri dall'URL al controller (ad esempio, */HelloWorld/Welcome? name = Scott&amp;numtimes = 4*). Modifica il `Welcome` metodo da includere due parametri, come illustrato di seguito. Si noti che il codice Usa la funzionalità di c#-parametro facoltativo per indicare che il `numTimes` parametro deve essere predefinito su 1 se per tale parametro viene passato alcun valore.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Nota sulla sicurezza: Il codice precedente Usa [HttpUtility](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) per proteggere l'applicazione da input dannoso (in particolare in JavaScript). Per altre informazioni, vedere [procedura: proteggere dagli attacchi tramite Script in un'applicazione Web per applicare la codifica HTML in stringhe](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Eseguire l'applicazione e passare all'URL di esempio (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). È possibile provare diversi valori per `name` e `numtimes` nell'URL. Il [sistema di associazione di modelli ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) esegue automaticamente il mapping di parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.

![](adding-a-controller/_static/image7.png)

Nell'esempio precedente, il segmento di URL ( `Parameters`) non viene utilizzato, il `name` e `numTimes` i parametri vengono passati come [stringhe di query](http://en.wikipedia.org/wiki/Query_string). Il carattere jolly ? (punto interrogativo) nell'URL precedente è un separatore e seguono le stringhe di query. Il carattere &amp; separa le stringhe di query.

Sostituire il metodo iniziale con il codice seguente:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Eseguire l'applicazione e immettere l'URL seguente: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Questa volta il terzo segmento di URL corrispondente parametro di route `ID.` il `Welcome` metodo di azione contiene un parametro (`ID`) corrispondenti della specifica URL di `RegisterRoutes` (metodo).

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

Nelle applicazioni ASP.NET MVC, è più comune per passare parametri come dati della route (come abbiamo fatto con ID precedente) rispetto a passarli come stringhe di query. È anche possibile aggiungere una route per il passaggio di entrambi i `name` e `numtimes` nei parametri come dati della route nell'URL. Nel *App\_Start\RouteConfig.cs* , aggiungere la route "Hello":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Eseguire l'applicazione e passare a `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Per molte applicazioni MVC, la route predefinita funziona correttamente. Si apprenderà più avanti in questa esercitazione per passare i dati usando lo strumento di associazione di modello e non sarà necessario modificare la route predefinita per tale.

In questi esempi il controller di elaborazione di operazioni il &quot;VC&quot; parte di MVC, vale a dire, il lavoro di visualizzazione e controller. Il controller restituisce direttamente l'HTML. In genere è preferibile non controller restituisce direttamente, l'HTML dal momento che diventa molto complessa al codice. Ma in genere si userà un file di modello di visualizzazione separato per generare la risposta HTML. Si esaminerà successivo al modo in cui è possibile farlo.

> [!div class="step-by-step"]
> [Precedente](getting-started.md)
> [Successivo](adding-a-view.md)
