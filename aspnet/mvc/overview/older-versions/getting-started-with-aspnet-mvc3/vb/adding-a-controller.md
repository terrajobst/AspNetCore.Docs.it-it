---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Aggiunta di un Controller (VB) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b38f3c4051af426e471568b8a25a471986f07209
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576229"
---
<a name="adding-a-controller-vb"></a>Aggiunta di un Controller (VB)
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente Visual Basic.NET è disponibile a complemento di questo argomento. [Scaricare la versione VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce c#, passare al [c# versione](../cs/adding-a-controller.md) di questa esercitazione.


MVC è l'acronimo *model-view-controller*. MVC è un modello per lo sviluppo di applicazioni in modo che ogni parte è una responsabilità separata:

- : I dati del modello Per l'applicazione.
- Visualizzazioni: I file di modello l'applicazione userà per generare dinamicamente risposte HTML.
- I controller: Classi che gestiscono le richieste di URL in ingresso all'applicazione, recuperare i dati del modello e quindi specificare i modelli di vista che eseguono il rendering di una risposta al client.

Si verrà che coprono tutti questi concetti in questa esercitazione e mostrerò come usarle per compilare un'applicazione.

Creare un nuovo controller facendo clic con il *controller* cartella **Esplora soluzioni** e quindi selezionando **Aggiungi Controller**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Assegnare un nome al controller &quot;HelloWorldController&quot; e fare clic su **Add**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Si noti che nel **Esplora soluzioni** a destra che per l'utente è stato creato un nuovo file denominato *HelloWorldController.cs* e che il file è aperto nell'IDE.

All'interno di nuovo `public class HelloWorldController` blocca, creare due nuovi metodi con un aspetto simile al codice seguente. Si tornerà una stringa del codice HTML direttamente dal controller come esempio.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Il controller è denominato `HelloWorldController` e il nuovo metodo è denominato `Index`. Eseguire l'applicazione (premere F5 o CTRL+F5). Dopo che è stata avviata nel browser, accodare &quot;HelloWorld&quot; al percorso nella barra degli indirizzi. (Sul mio computer, ha `http://localhost:43246/HelloWorld`) del browser avrà un aspetto simile allo screenshot seguente. Nel metodo precedente, il codice restituito direttamente una stringa. L'avevamo detto il sistema deve restituire solo del codice HTML e ha sempre fatto!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC richiama le classi controller diverso (e i metodi di azione diverso in esse contenute) a seconda dell'URL in ingresso. La logica di mapping predefinito usata da ASP.NET MVC Usa un formato simile al seguente per controllare il codice richiamato:

`/[Controller]/[ActionName]/[Parameters]`

La prima parte dell'URL determina la classe controller da eseguire. Così */HelloWorld* esegue il mapping al `HelloWorldController` classe. La seconda parte dell'URL determina il metodo di azione per la classe da eseguire. Così */HelloWorld/Index* causerebbe il `Index` metodo del `HelloWorldController` classe da eseguire. Si noti che abbiamo dovuto solo visita */HelloWorld* sopra e `Index` metodo utilizzato per impostazione predefinita. Infatti, un metodo denominato `Index` è il metodo predefinito che verrà chiamato su un controller se non ne viene specificato in modo esplicito.

Passare a `http://localhost:xxxx/HelloWorld/Welcome`. Il `Welcome` metodo viene eseguito e restituisce la stringa &quot;si tratta del metodo di azione iniziale... &quot;. Il mapping di MVC predefinita è `/[Controller]/[ActionName]/[Parameters]`. Per questo URL, il controller si trova `HelloWorld` e `Welcome` è il metodo. Non è stato usato il `[Parameters]` fa parte dell'URL.

![](adding-a-controller/_static/image6.png)

È possibile modificare leggermente l'esempio in modo che è possibile passare le informazioni dei parametri dall'URL al controller (ad esempio, */HelloWorld/Welcome? name = Scott&amp;numtimes = 4*). Modifica il `Welcome` metodo da includere due parametri, come illustrato di seguito. Si noti che è stata usata la funzionalità di parametro facoltativo di Visual Basic per indicare che il `numTimes` parametro deve essere predefinito su 1 se per tale parametro viene passato alcun valore.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Eseguire l'applicazione e passare a `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** È possibile provare diversi valori per `name` e `numtimes`. Il sistema esegue automaticamente il mapping di parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.

![](adding-a-controller/_static/image7.png)

In entrambi questi esempi il controller è in linea la parte VC di MVC, ovvero le operazioni di visualizzazione e controller. Il controller restituisce direttamente l'HTML. In genere non vogliamo controller restituisce direttamente, l'HTML dal momento che diventa molto complessa al codice. Ma in genere si userà un file di modello di visualizzazione separato per generare la risposta HTML. Si esaminerà come è possibile farlo.

> [!div class="step-by-step"]
> [Precedente](intro-to-aspnet-mvc-3.md)
> [Successivo](adding-a-view.md)
