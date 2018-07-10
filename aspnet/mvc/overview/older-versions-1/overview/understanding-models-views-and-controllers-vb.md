---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
title: Informazioni su modelli, visualizzazioni e controller (VB) | Microsoft Docs
author: StephenWalther
description: Per informazioni dettagliate sugli modelli, visualizzazioni e controller? In questa esercitazione, Stephen Walther presenta le diverse parti di un'applicazione ASP.NET MVC.
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: a106374a-5e74-4fd0-9ac0-1a32280e5d0d
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
msc.type: authoredcontent
ms.openlocfilehash: 1c751972b0f59a3988e15eed0c6cff3bc566de0e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838728"
---
<a name="understanding-models-views-and-controllers-vb"></a>Informazioni su modelli, visualizzazioni e controller (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Per informazioni dettagliate sugli modelli, visualizzazioni e controller? In questa esercitazione, Stephen Walther presenta le diverse parti di un'applicazione ASP.NET MVC.


Questa esercitazione offre una panoramica generale di ASP.NET MVC modelli, visualizzazioni e controller. In altre parole, viene spiegato il valore M', V "e C' in ASP.NET MVC.

Dopo aver letto questa esercitazione, è necessario comprendere sull'interagiscono tra le diverse parti di un'applicazione ASP.NET MVC. È anche necessario conoscere le differenze tra l'architettura di un'applicazione MVC ASP.NET da un'applicazione di Active Server Pages o applicazione Web Form ASP.NET.

## <a name="the-sample-aspnet-mvc-application"></a>L'applicazione ASP.NET MVC di esempio

Il modello di Visual Studio predefinito per la creazione di applicazioni Web ASP.NET MVC include un'applicazione di esempio molto semplice che può essere utilizzata per comprendere le diverse parti di un'applicazione ASP.NET MVC. Possiamo usufruire di questa semplice applicazione in questa esercitazione.

Si crea una nuova applicazione MVC ASP.NET con il modello MVC, avviare Visual Studio 2008 e selezionando l'opzione di menu File, nuovo progetto (vedere la figura 1). Nella finestra di dialogo Nuovo progetto, selezionare il linguaggio di programmazione preferito in tipi di progetto (Visual Basic o c#) e selezionare **applicazione Web ASP.NET MVC** nei modelli. Fare clic sul pulsante OK.


[![Finestra di dialogo Nuovo progetto](understanding-models-views-and-controllers-vb/_static/image1.jpg)](understanding-models-views-and-controllers-vb/_static/image1.png)

**Figura 01**: finestra di dialogo Nuovo progetto ([fare clic per visualizzare l'immagine con dimensioni normali](understanding-models-views-and-controllers-vb/_static/image2.png))


Quando si crea una nuova applicazione ASP.NET MVC, il **Crea progetto Unit Test** (vedere la figura 2) viene visualizzata la finestra. Questa finestra di dialogo consente di creare un progetto separato nella soluzione per il test dell'applicazione ASP.NET MVC. Selezionare l'opzione **No, non creare un progetto unit test** e fare clic sui **OK** pulsante.


[![Creare una finestra di dialogo di Unit Test](understanding-models-views-and-controllers-vb/_static/image2.jpg)](understanding-models-views-and-controllers-vb/_static/image3.png)

**Figura 02**: creare Unit Test finestra ([fare clic per visualizzare l'immagine con dimensioni normali](understanding-models-views-and-controllers-vb/_static/image4.png))


Dopo il nuovo di ASP.NET MVC viene creata l'applicazione. Si noterà diversi file nella finestra Esplora soluzioni e cartelle. In particolare, si noterà tre cartelle denominate modelli, visualizzazioni e controller. Come può immaginare dai nomi delle cartelle, queste cartelle contengono i file per l'implementazione di modelli, visualizzazioni e controller.

Se si espande la cartella Controllers, si noterà un file denominato AccountController.vb e un file denominato HomeController.vb. Se si espande la cartella Views, si dovrebbero vedere tre sottocartelle denominate Account, Home e condiviso. Se si espande la cartella Home, verranno visualizzati due file aggiuntivi denominati index. aspx e About (vedere la figura 3). Tali file costituiscono l'applicazione di esempio incluso con il modello MVC di ASP.NET predefinito.


[![La finestra di Esplora soluzioni](understanding-models-views-and-controllers-vb/_static/image3.jpg)](understanding-models-views-and-controllers-vb/_static/image5.png)

**Figura 03**: la finestra di Esplora soluzioni ([fare clic per visualizzare l'immagine con dimensioni normali](understanding-models-views-and-controllers-vb/_static/image6.png))


È possibile eseguire l'applicazione di esempio, selezionando l'opzione di menu **eseguire il Debug, Avvia debug**. In alternativa, è possibile premere il tasto F5.

Quando si esegue prima un'applicazione ASP.NET, viene visualizzata la finestra di dialogo nella figura 4 che consiglia di abilitare la modalità di debug. Fare clic sul pulsante OK e verrà eseguita l'applicazione.


[![Finestra di debug non abilitato](understanding-models-views-and-controllers-vb/_static/image4.jpg)](understanding-models-views-and-controllers-vb/_static/image7.png)

**Figura 04**: finestra di dialogo Debug non abilitato ([fare clic per visualizzare l'immagine con dimensioni normali](understanding-models-views-and-controllers-vb/_static/image8.png))


Quando si esegue un'applicazione ASP.NET MVC, Visual Studio avvia l'applicazione nel web browser. L'applicazione di esempio è costituito da solo due pagine: la pagina di indice e la pagina di informazioni. All'avvio dell'applicazione prima di tutto, viene visualizzata la pagina di indice (vedere la figura 5). È possibile passare alla pagina About, selezionando il collegamento di menu in alto a destra dell'applicazione.


[![La pagina di indice](understanding-models-views-and-controllers-vb/_static/image5.jpg)](understanding-models-views-and-controllers-vb/_static/image9.png)

**Figura 05**: la pagina di indice ([fare clic per visualizzare l'immagine con dimensioni normali](understanding-models-views-and-controllers-vb/_static/image10.png))


Si noti che gli URL nella barra degli indirizzi del browser. Ad esempio, quando si fa clic sul collegamento del menu About, l'URL nella barra degli indirizzi del browser viene modificato da **/Home/About**.

Se si chiude la finestra del browser e tornare a Visual Studio, sarà in grado di trovare un file con la home page percorso/circa. I file non esistono. Come è possibile?

## <a name="a-url-does-not-equal-a-page"></a>Un URL diverso da una pagina

Quando si compila un'applicazione Web Form ASP.NET tradizionale o un'applicazione Active Server Pages, si verifica una corrispondenza uno a uno tra un URL e una pagina. Se si richiede una pagina denominata SomePage.aspx dal server, era meglio essere presente una pagina su disco denominato SomePage.aspx. Se il file SomePage.aspx non esiste, viene visualizzato un poco eleganti **404 - pagina non trovata** errore.

Quando si compila un'applicazione ASP.NET MVC, al contrario, non sia alcuna corrispondenza tra l'URL digitato nella barra degli indirizzi del browser e i file che trova nell'applicazione. In un'applicazione ASP.NET MVC, un URL corrisponde a un'azione del controller anziché una pagina su disco.

In un'applicazione ASP o ASP.NET tradizionale, le richieste del browser vengono eseguito il mapping a pagine. In un'applicazione ASP.NET MVC, al contrario, le richieste del browser sono mappate alle azioni del controller. Un'applicazione Web Form ASP.NET è incentrato sui contenuti. Un'applicazione ASP.NET MVC, è invece incentrato sugli elementi per la logica dell'applicazione.

## <a name="understanding-aspnet-routing"></a>Informazioni sul Routing di ASP.NET

Una richiesta del browser viene eseguito il mapping a un'azione del controller tramite una funzionalità del framework ASP.NET chiamato *Routing ASP.NET*. Il Routing ASP.NET viene utilizzato dal framework ASP.NET MVC per *route* richieste in ingresso per le azioni del controller.

Il Routing ASP.NET usa una tabella di route per gestire le richieste in ingresso. Questa tabella di route viene creata al primo avvio dell'applicazione web. La tabella di route è configurata nel file Global. asax. Il file Global. asax MVC predefinita è contenuto nel listato 1.

**Listato 1 - Global. asax**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample1.vb)]

Quando un'applicazione ASP.NET primo avvio, l'applicazione\_viene chiamato il metodo Start (). Nel listato 1, questo metodo chiama il metodo RegisterRoutes() e il metodo RegisterRoutes() crea la tabella di route predefiniti.

La tabella di route predefinito è costituito da una o più route. Questo route predefinito interrompe tutte le richieste in ingresso in tre segmenti (un segmento URL è diverso tra le barre). Il primo segmento è mappato a un nome di controller, il secondo segmento viene mappato a un nome di azione e il segmento finale viene eseguito il mapping a un parametro passato all'azione denominato ID.

Si consideri ad esempio l'URL seguente:

/ Prodotti/dettagli/3

Questo URL viene analizzato nei tre parametri simile al seguente:

Controller = prodotto

Azione: dettagli

ID = 3

La route predefinito definita nel file Global. asax includa valori predefiniti per tutti i tre parametri. Il valore predefinito è Home Controller, il valore predefinito di azione è l'indice e l'Id predefinito è una stringa vuota. Con queste impostazioni predefinite presenti, prendere in considerazione la modalità di analisi all'URL seguente:

Per dipendente

Questo URL viene analizzato nei tre parametri simile al seguente:

Controller = dipendente

Azione = indice

ID =

Infine, se si apre un'applicazione ASP.NET MVC senza fornire qualsiasi URL (ad esempio, `http://localhost`) l'URL viene analizzato come segue:

Controller = Home

Azione = indice

ID =

La richiesta viene indirizzata all'azione Index () sulla classe HomeController.

## <a name="understanding-controllers"></a>Informazioni sui controller

Un controller è responsabile del controllo il modo in cui un utente interagisce con un'applicazione MVC. Un controller contiene la logica di controllo di flusso per un'applicazione ASP.NET MVC. Un controller determina il tipo di risposta per inviare a un utente quando un utente effettua una richiesta del browser.

Un controller è semplicemente una classe (ad esempio, una classe Visual Basic o c#). L'esempio di applicazione MVC ASP.NET include un controller denominato HomeController.vb che si trova nella cartella controller. Il contenuto del file HomeController.vb viene riprodotto nel listato 2.

**Listato 2 - HomeController.cs**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample2.vb)]

Si noti che la classe HomeController ha due metodi denominati About () e Index (). Questi due metodi corrispondano per le due azioni esposte dal controller. Indice/URL avremo richiama il metodo HomeController.Index() e l'URL avremo su richiama il metodo HomeController.About().

Qualsiasi metodo pubblico in un controller viene esposta come un'azione del controller. È necessario prestare particolare attenzione a questo. Ciò significa che qualsiasi metodo pubblico contenuto in un controller può essere richiamata da chiunque abbia accesso a Internet, immettere l'URL giusto in un browser.

## <a name="understanding-views"></a>Informazioni sulle viste

Le azioni del due controller esposte dalla classe HomeController, About () e Index () restituiscono entrambi una vista. Una vista contiene il markup HTML e il contenuto che viene inviato al browser. Una vista è l'equivalente di una pagina quando si lavora con un'applicazione ASP.NET MVC.

È necessario creare le visualizzazioni nella posizione corretta. L'azione HomeController.Index() restituisce una vista che si trova nel percorso seguente:

\Views\Home\Index.aspx

L'azione HomeController.About() restituisce una vista che si trova nel percorso seguente:

\Views\Home\About.aspx

In generale, se si desidera restituire una visualizzazione per un'azione del controller, quindi è necessario creare una sottocartella nella cartella Views con lo stesso nome del controller. All'interno delle sottocartelle, è necessario creare un file con estensione aspx con lo stesso nome di azione del controller.

Il file nel listato 3 contiene la visualizzazione About. aspx.

**Listato 3 - About. aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-vb/samples/sample3.aspx)]

Se si ignora la prima riga nel listato 3, la maggior parte del resto della visualizzazione è costituito da codice HTML standard. È possibile modificare il contenuto della visualizzazione tramite l'immissione di qualsiasi codice HTML da qui.

Una vista è molto simile a una pagina nelle pagine ASP o ASP.NET Web Form. Una vista può contenere contenuto HTML e script. È possibile scrivere gli script nel .NET preferito (ad esempio, c# o Visual Basic .NET) di linguaggio di programmazione. È utilizzare script per visualizzare il contenuto dinamico, ad esempio i dati del database.

## <a name="understanding-models"></a>Informazioni su modelli

Abbiamo analizzato i controller e sono state illustrate le viste. Nell'ultimo argomento da discutere è rappresentata dai modelli. Che cos'è un modello di MVC?

Un modello di MVC contiene tutta la logica dell'applicazione che non è inclusa in una vista o a un controller. Il modello deve contenere tutte la logica di business dell'applicazione, la logica di convalida e logica di accesso ai database. Ad esempio, se si utilizza Microsoft Entity Framework per accedere al database, quindi si creerà le classi di Entity Framework (file. edmx) nella cartella Models.

Una vista deve contenere solo per la logica relative alla generazione dell'interfaccia utente. Un controller deve contenere solo il livello minimo della logica necessaria per restituire la visualizzazione a destra o reindirizzare l'utente a un'altra azione (controllo di flusso). Tutti gli altri elementi devono essere contenuti nel modello.

In generale, che si deve cercare modelli fat e skinny controller. I metodi del controller devono contenere solo poche righe di codice. Se un'azione del controller Ottiene troppo fat, quindi è consigliabile spostare la logica a una nuova classe nella cartella Models.

## <a name="summary"></a>Riepilogo

Questa esercitazione ha fornito una panoramica generale delle diverse parti di MVC ASP.NET dell'applicazione web. Si è appreso come il Routing ASP.NET esegue il mapping alle richieste del browser in ingresso alle azioni del controller specifico. Si è appreso come controller di orchestrare il modo in cui le visualizzazioni vengono restituite al browser. Infine, si è appreso come modelli contengono business dell'applicazione, la convalida e logica di accesso ai database.
