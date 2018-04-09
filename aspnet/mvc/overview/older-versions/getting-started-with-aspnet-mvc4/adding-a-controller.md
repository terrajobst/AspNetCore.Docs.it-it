---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Aggiunta di un Controller | Documenti Microsoft
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui che utilizza ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice seguire e demo...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: bb76c0a87d935322406b9d8e18fbdb3e41f327f5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller"></a>Aggiunta di un controller
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013. È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.


MVC è l'acronimo di *model-view-controller*. MVC è un modello per lo sviluppo di applicazioni che sono ben strutturato, testabile e facili da gestire. Applicazioni basate su MVC contengono:

- **M** odels: le classi che rappresentano i dati dell'applicazione e che usano la logica di convalida per applicare le regole di business per tali dati.
- **V** iews: file di modello che l'applicazione usa per generare in modo dinamico le risposte HTML.
- **C** ontrollers: le classi che gestiscono le richieste del browser in ingresso, recuperare i dati del modello e quindi specificare i modelli di visualizzazione che restituiscono una risposta nel browser.

Si verrà tutti questi concetti in questa serie di esercitazioni di copertura e viene illustrato come usarle per compilare un'applicazione.

Si inizierà creando una classe controller. In **Esplora**, fare doppio clic su di *controller* cartella e quindi selezionare **Aggiungi Controller**.

![](adding-a-controller/_static/image1.png)

Nome del nuovo controller &quot;HelloWorldController&quot;. Lasciare il modello predefinito come **controller MVC vuoto** e fare clic su **Aggiungi**.

![Aggiungi controller](adding-a-controller/_static/image2.png)

Si noti che nel **Esplora** che un nuovo file è stato creato denominato *HelloWorldController.cs*. Il file è aperto nell'IDE.

![](adding-a-controller/_static/image3.png)

Sostituire il contenuto del file con il codice seguente.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

I metodi di controller verranno restituita una stringa HTML come esempio. Il controller è denominato `HelloWorldController` e sopra il primo metodo è denominato `Index`. Verrà richiamato da un browser. Eseguire l'applicazione (premere F5 o Ctrl + F5). Nel browser, aggiungere &quot;HelloWorld&quot; per il percorso nella barra degli indirizzi. (Ad esempio, nella figura seguente, la `http://localhost:1234/HelloWorld.`) la pagina nel browser apparirà la schermata seguente. Nel metodo, il codice restituito direttamente una stringa. È indicato il sistema da restituire solo il codice HTML e aveva!

![](adding-a-controller/_static/image4.png)

ASP.NET MVC richiama classi controller diverso (e i metodi di azione diversi all'interno di essi) a seconda dell'URL in ingresso. La logica di routing URL predefinito utilizzata da MVC ASP.NET utilizza un formato simile al seguente per determinare quale codice per richiamare:

`/[Controller]/[ActionName]/[Parameters]`

La prima parte dell'URL determina la classe controller da eseguire. Pertanto, */HelloWorld* esegue il mapping alla `HelloWorldController` classe. La seconda parte dell'URL determina il metodo di azione della classe per l'esecuzione. Così */HelloWorld/indice* causerebbe il `Index` metodo il `HelloWorldController` classe da eseguire. Si noti che abbiamo solo passare a */HelloWorld* e `Index` metodo utilizzato per impostazione predefinita. In questo modo un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller non è specificata in modo esplicito.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il `Welcome` metodo viene eseguito e restituisce la stringa &quot;si tratta del metodo di azione di completamento dell'installazione... &quot;. Il mapping di MVC predefinita è `/[Controller]/[ActionName]/[Parameters]`. Per questo URL, il controller è `HelloWorld` e il metodo di azione è `Welcome`. Non è stata ancora usata la parte `[Parameters]` dell'URL.

![](adding-a-controller/_static/image5.png)

Modifichiamo l'esempio leggermente, in modo che è possibile passare alcune informazioni sui parametri dall'URL per il controller (ad esempio, */HelloWorld/iniziale? name = Scott&amp;numtimes = 4*). Modifica il `Welcome` metodo includere due parametri, come illustrato di seguito. Si noti che il codice Usa la funzionalità di c# parametro facoltativo per indicare che il `numTimes` parametro deve essere predefinito su 1 se per tale parametro viene passato alcun valore.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Eseguire l'applicazione e passare all'URL di esempio (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. È possibile provare diversi valori per `name` e `numtimes` nell'URL. Il [sistema di associazione del modello MVC ASP.NET](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) associa automaticamente i parametri denominati dalla stringa di query nella barra degli indirizzi per i parametri del metodo.

![](adding-a-controller/_static/image6.png)

In entrambi questi esempi il controller di operazioni il &quot;VC&quot; parte di MVC, vale a dire, il lavoro di visualizzazione e controller. Il controller ha restituito HTML direttamente. In genere non si desidera controller restituzione HTML direttamente, dal momento che diventa molto complessa al codice. Invece in genere si userà un file di modello di visualizzazione separato per generare la risposta HTML. Verrà ora esaminato il successivo come possiamo farlo.

> [!div class="step-by-step"]
> [Precedente](intro-to-aspnet-mvc-4.md)
> [Successivo](adding-a-view.md)
