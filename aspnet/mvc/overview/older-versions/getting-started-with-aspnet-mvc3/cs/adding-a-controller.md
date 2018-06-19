---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Aggiunta di un Controller (c#) | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express corrisponde Service Pack 1, che si...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 963d3bbbadf408d7045c50bfd693069e4097d45d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868295"
---
<a name="adding-a-controller-c"></a>Aggiunta di un Controller (c#)
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


MVC è l'acronimo di *model-view-controller*. MVC è un modello per lo sviluppo di applicazioni che sono ben progettate e facili da gestire. Applicazioni basate su MVC contengono:

- Controller: Le classi che gestiscono le richieste in ingresso all'applicazione, recuperare i dati del modello e quindi specificare i modelli di visualizzazione che restituiscono una risposta al client.
- Modelli: Le classi che rappresentano i dati dell'applicazione e che usano la logica di convalida per applicare le regole di business per tali dati.
- Visualizzazioni: File di modello utilizzati dall'applicazione per generare dinamicamente le risposte HTML.

Si verrà tutti questi concetti in questa serie di esercitazioni di copertura e viene illustrato come usarle per compilare un'applicazione.

Si inizierà creando una classe controller. In **Esplora**, fare doppio clic su di *controller* cartella e quindi selezionare **Aggiungi Controller**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Denominare il nuovo controller "HelloWorldController". Lasciare il modello predefinito come **controller vuoto** e fare clic su **Aggiungi**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Si noti che nel **Esplora** che un nuovo file è stato creato denominato *HelloWorldController.cs*. Il file è aperto nell'IDE.

![](adding-a-controller/_static/image5.png)

All'interno di `public class HelloWorldController` di blocco, creare due metodi che è simile al codice seguente. Il controller verrà restituita una stringa HTML come esempio.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Il controller è denominato `HelloWorldController` e sopra il primo metodo è denominato `Index`. Verrà richiamato da un browser. Eseguire l'applicazione (premere F5 o Ctrl + F5). Nel browser, aggiungere "HelloWorld" al percorso nella barra degli indirizzi. (Ad esempio, nella figura seguente, la `http://localhost:43246/HelloWorld.`) la pagina nel browser apparirà la schermata seguente. Nel metodo, il codice restituito direttamente una stringa. È indicato il sistema da restituire solo il codice HTML e aveva!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC richiama classi controller diverso (e i metodi di azione diversi all'interno di essi) a seconda dell'URL in ingresso. La logica di mapping predefinito utilizzata da MVC ASP.NET utilizza un formato simile al seguente per determinare quale codice per richiamare:

`/[Controller]/[ActionName]/[Parameters]`

La prima parte dell'URL determina la classe controller da eseguire. Pertanto, */HelloWorld* esegue il mapping alla `HelloWorldController` classe. La seconda parte dell'URL determina il metodo di azione della classe per l'esecuzione. Così */HelloWorld/indice* causerebbe il `Index` metodo il `HelloWorldController` classe da eseguire. Si noti che abbiamo solo passare a */HelloWorld* e `Index` metodo utilizzato per impostazione predefinita. In questo modo un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller non è specificata in modo esplicito.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il metodo `Welcome` viene eseguito e restituisce la stringa "This is the Welcome action method...". Il mapping di MVC predefinita è `/[Controller]/[ActionName]/[Parameters]`. Per questo URL, il controller è `HelloWorld` e il metodo di azione è `Welcome`. Non è stata ancora usata la parte `[Parameters]` dell'URL.

![](adding-a-controller/_static/image7.png)

Modifichiamo l'esempio leggermente, in modo che è possibile passare alcune informazioni sui parametri dall'URL per il controller (ad esempio, */HelloWorld/iniziale? name = Scott&amp;numtimes = 4*). Modifica il `Welcome` metodo includere due parametri, come illustrato di seguito. Si noti che il codice Usa la funzionalità di c# parametro facoltativo per indicare che il `numTimes` parametro deve essere predefinito su 1 se per tale parametro viene passato alcun valore.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Eseguire l'applicazione e passare all'URL di esempio (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. È possibile provare diversi valori per `name` e `numtimes` nell'URL. Il sistema esegue il mapping automaticamente i parametri denominati dalla stringa di query nella barra degli indirizzi per i parametri del metodo.

![](adding-a-controller/_static/image8.png)

In entrambi questi esempi il controller di operazioni la parte "VC" di MVC, vale a dire, il lavoro di visualizzazione e controller. Il controller ha restituito HTML direttamente. In genere non si desidera controller restituzione HTML direttamente, dal momento che diventa molto complessa al codice. Invece in genere si userà un file di modello di visualizzazione separato per generare la risposta HTML. Verrà ora esaminato il successivo come possiamo farlo.

> [!div class="step-by-step"]
> [Precedente](intro-to-aspnet-mvc-3.md)
> [Successivo](adding-a-view.md)
