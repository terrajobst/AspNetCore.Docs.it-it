---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Aggiunta di un Controller (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express corrisponde Service Pack 1, cui ho...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 0c13248960384e8d773f9c5ba3d927b6a8502938
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805859"
---
<a name="adding-a-controller-c"></a>Aggiunta di un Controller (c#)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

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


MVC è l'acronimo *model-view-controller*. MVC è un modello per lo sviluppo di applicazioni che sono ben strutturata e facile da gestire. Le applicazioni basate su MVC contengono:

- I controller: Classi che gestiscono le richieste in ingresso all'applicazione, recuperare i dati del modello e quindi specificare i modelli di vista che restituiscono una risposta al client.
- I modelli: Classi che rappresentano i dati dell'applicazione e che usano la logica di convalida per applicare le regole di business per tali dati.
- Visualizzazioni: File modello l'applicazione usa per generare in modo dinamico le risposte HTML.

Si verrà che coprono tutti questi concetti in questa serie di esercitazioni e mostrerò come usarle per compilare un'applicazione.

È innanzitutto necessario creare una classe controller. Nella **Esplora soluzioni**, fare doppio clic il *controller* cartella e quindi selezionare **Aggiungi Controller**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Assegnare un nome al controller "HelloWorldController". Lasciare il modello predefinito come **controller vuoto** e fare clic su **Add**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Si noti che nel **Esplora soluzioni** che un nuovo file creato denominato *HelloWorldController.cs*. Il file è aperto nell'IDE.

![](adding-a-controller/_static/image5.png)

All'interno di `public class HelloWorldController` blocca, creare due metodi con un aspetto simile al codice seguente. Il controller verrà restituita una stringa del codice HTML come esempio.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Il controller è denominato `HelloWorldController` e il primo metodo riportato sopra è denominato `Index`. È possibile richiamarla da un browser. Eseguire l'applicazione (premere F5 o CTRL+F5). Nel browser, aggiungere "HelloWorld" al percorso nella barra degli indirizzi. (Ad esempio, nell'illustrazione seguente, relativo `http://localhost:43246/HelloWorld.`) la pagina nel browser avrà un aspetto simile allo screenshot seguente. Nel metodo precedente, il codice restituito direttamente una stringa. È indicato il sistema deve restituire solo del codice HTML e ha sempre fatto!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC richiama le classi controller diverso (e i metodi di azione diverso in esse contenute) a seconda dell'URL in ingresso. La logica di mapping predefinito usata da ASP.NET MVC Usa un formato simile al seguente per determinare quale codice per richiamare:

`/[Controller]/[ActionName]/[Parameters]`

La prima parte dell'URL determina la classe controller da eseguire. Così */HelloWorld* esegue il mapping al `HelloWorldController` classe. La seconda parte dell'URL determina il metodo di azione per la classe da eseguire. Così */HelloWorld/Index* causerebbe il `Index` metodo del `HelloWorldController` classe da eseguire. Si noti che è necessario solo passare a */HelloWorld* e il `Index` metodo utilizzato per impostazione predefinita. Infatti, un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller se non ne viene specificato in modo esplicito.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il metodo `Welcome` viene eseguito e restituisce la stringa "This is the Welcome action method...". Il mapping di MVC predefinita è `/[Controller]/[ActionName]/[Parameters]`. Per questo URL, il controller è `HelloWorld` e il metodo di azione è `Welcome`. Non è stata ancora usata la parte `[Parameters]` dell'URL.

![](adding-a-controller/_static/image7.png)

È possibile modificare leggermente l'esempio in modo che è possibile passare le informazioni dei parametri dall'URL al controller (ad esempio, */HelloWorld/Welcome? name = Scott&amp;numtimes = 4*). Modifica il `Welcome` metodo da includere due parametri, come illustrato di seguito. Si noti che il codice Usa la funzionalità di c#-parametro facoltativo per indicare che il `numTimes` parametro deve essere predefinito su 1 se per tale parametro viene passato alcun valore.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Eseguire l'applicazione e passare all'URL di esempio (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. È possibile provare diversi valori per `name` e `numtimes` nell'URL. Il sistema esegue automaticamente il mapping di parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.

![](adding-a-controller/_static/image8.png)

In entrambi questi esempi il controller è in linea la parte "VC" di MVC, vale a dire, il lavoro di visualizzazione e controller. Il controller restituisce direttamente l'HTML. In genere è preferibile non controller restituisce direttamente, l'HTML dal momento che diventa molto complessa al codice. Ma in genere si userà un file di modello di visualizzazione separato per generare la risposta HTML. Si esaminerà successivo al modo in cui è possibile farlo.

> [!div class="step-by-step"]
> [Precedente](intro-to-aspnet-mvc-3.md)
> [Successivo](adding-a-view.md)
