---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Aggiunta di un Controller (VB) | Documenti Microsoft
author: Rick-Anderson
description: "In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 74113d76a74b1da27a7f9a33a13038a0c36ad036
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller-vb"></a>Aggiunta di un Controller (VB)
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)
> 
> Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente VB.NET complemento è disponibile in questo argomento. [Scaricare la versione di Visual Basic.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce c#, passare il [c# versione](../cs/adding-a-controller.md) di questa esercitazione.


MVC è l'acronimo di *model-view-controller*. MVC è un modello per lo sviluppo di applicazioni in modo che ogni parte è una responsabilità separata:

- : I dati del modello Per l'applicazione.
- Visualizzazioni: I file di modello che dell'applicazione verrà utilizzati per generare dinamicamente le risposte HTML.
- Controller: Le classi che gestiscono le richieste di URL in ingresso all'applicazione, recuperare i dati del modello e quindi specificare i modelli di visualizzazione che eseguono il rendering di una risposta al client.

Si verrà che coprono tutti questi concetti in questa esercitazione e viene illustrato come usarle per compilare un'applicazione.

Creare un nuovo controller facendo clic con il *controller* cartella **Esplora** e quindi selezionando **Aggiungi Controller**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Nome del nuovo controller &quot;HelloWorldController&quot; e fare clic su **Aggiungi**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Si noti che nel **Esplora** a destra che per l'utente è stato creato un nuovo file denominato *HelloWorldController.cs* e che il file è aperto nell'IDE.

All'interno di nuovo `public class HelloWorldController` blocca, creare due nuovi metodi che è simile al codice seguente. Ad esempio, si sarà restituire una stringa di HTML direttamente dal controller di.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Il controller è denominato `HelloWorldController` e il nuovo metodo è denominato `Index`. Eseguire l'applicazione (premere F5 o Ctrl + F5). Una volta che è stata avviata nel browser, aggiungere &quot;HelloWorld&quot; per il percorso nella barra degli indirizzi. (Il computer ha `http://localhost:43246/HelloWorld`) nel browser apparirà la schermata riportata di seguito. Nel metodo, il codice restituito direttamente una stringa. È indicato il sistema da restituire solo il codice HTML e aveva!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC richiama classi controller diverso (e i metodi di azione diversi all'interno di essi) a seconda dell'URL in ingresso. Per controllare il codice richiamato, la logica di mapping predefinito utilizzata da ASP.NET MVC utilizza un formato simile al seguente:

`/[Controller]/[ActionName]/[Parameters]`

La prima parte dell'URL determina la classe controller da eseguire. Pertanto, */HelloWorld* esegue il mapping alla `HelloWorldController` classe. La seconda parte dell'URL determina il metodo di azione della classe per l'esecuzione. Così */HelloWorld/indice* causerebbe il `Index` metodo il `HelloWorldController` classe da eseguire. Si noti che abbiamo solo visitare */HelloWorld* sopra e `Index` metodo utilizzato per impostazione predefinita. In questo modo un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller non è specificata in modo esplicito.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il `Welcome` metodo viene eseguito e restituisce la stringa &quot;si tratta del metodo di azione di completamento dell'installazione... &quot;. Il mapping di MVC predefinita è `/[Controller]/[ActionName]/[Parameters]`. Per questo URL, il controller è `HelloWorld` e `Welcome` è il metodo. Non è stato usato il `[Parameters]` fa parte dell'URL ancora.

![](adding-a-controller/_static/image6.png)

Modifichiamo l'esempio leggermente, in modo che è possibile passare alcune informazioni di parametro in dall'URL per il controller (ad esempio, */HelloWorld/iniziale? name = Scott&amp;numtimes = 4*). Modifica il `Welcome` metodo includere due parametri, come illustrato di seguito. Si noti che è stata utilizzata la funzionalità di parametro facoltativo di Visual Basic per indicare che il `numTimes` parametro deve essere predefinito su 1 se per tale parametro viene passato alcun valore.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Eseguire l'applicazione e passare a `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** È possibile provare diversi valori per `name` e `numtimes`. Il sistema esegue il mapping automaticamente i parametri denominati dalla stringa di query nella barra degli indirizzi per i parametri del metodo.

![](adding-a-controller/_static/image7.png)

In entrambi questi esempi il controller di operazioni parte VC di MVC, che è la visualizzazione e controller. Il controller ha restituito HTML direttamente. In genere non vogliamo controller restituzione HTML direttamente, dal momento che diventa molto complessa al codice. Invece in genere si userà un file di modello di visualizzazione separato per generare la risposta HTML. Vediamo come è possibile farlo.

>[!div class="step-by-step"]
[Precedente](intro-to-aspnet-mvc-3.md)
[Successivo](adding-a-view.md)
