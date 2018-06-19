---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
title: Informazioni su modelli, visualizzazioni e controller (VB) | Documenti Microsoft
author: StephenWalther
description: Ulteriori informazioni sui modelli, visualizzazioni e controller? In questa esercitazione, Stephen Walther vengono descritte le diverse parti di un'applicazione MVC ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: a106374a-5e74-4fd0-9ac0-1a32280e5d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
msc.type: authoredcontent
ms.openlocfilehash: 36f70503f7e96210e5fb8a2d361da6759c18c85b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26500990"
---
<a name="understanding-models-views-and-controllers-vb"></a>Informazioni su modelli, visualizzazioni e controller (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Ulteriori informazioni sui modelli, visualizzazioni e controller? In questa esercitazione, Stephen Walther vengono descritte le diverse parti di un'applicazione MVC ASP.NET.


In questa esercitazione fornisce una panoramica di ASP.NET MVC modelli, visualizzazioni e controller. In altre parole, viene spiegato il M', V "e C' in ASP.NET MVC.

Dopo avere letto questa esercitazione, è necessario comprendere dell'interazione tra le diverse parti di un'applicazione MVC ASP.NET. È inoltre necessario comprendere le differenze tra l'architettura di un'applicazione MVC ASP.NET da un'applicazione Web Form ASP.NET o Active Server Pages.

## <a name="the-sample-aspnet-mvc-application"></a>L'applicazione ASP.NET MVC di esempio

Il modello di Visual Studio predefinito per la creazione di applicazioni Web ASP.NET MVC include un'applicazione di esempio molto semplice che può essere usata per comprendere le diverse parti di un'applicazione MVC ASP.NET. Possiamo usufruire di questa semplice applicazione in questa esercitazione.

Si crea una nuova applicazione MVC ASP.NET con il modello MVC avviando Visual Studio 2008 e selezionando l'opzione di menu File, nuovo progetto (vedere la figura 1). Nella finestra di dialogo Nuovo progetto, selezionare il linguaggio di programmazione preferito in tipi di progetto (Visual Basic o c#) e selezionare **applicazione Web ASP.NET MVC** in modelli. Fare clic sul pulsante OK.


[![Finestra di dialogo Nuovo progetto](understanding-models-views-and-controllers-vb/_static/image1.jpg)](understanding-models-views-and-controllers-vb/_static/image1.png)

**Figura 01**: finestra di dialogo Nuovo progetto ([fare clic per visualizzare l'immagine ingrandita](understanding-models-views-and-controllers-vb/_static/image2.png))


Quando si crea una nuova applicazione ASP.NET MVC, il **Crea progetto Unit Test** (vedere la figura 2) viene visualizzata la finestra. Questa finestra di dialogo consente di creare un progetto separato nella soluzione per il test dell'applicazione ASP.NET MVC. Selezionare l'opzione **No, non creare un progetto di unit test** e fare clic su di **OK** pulsante.


[![Creare una finestra di dialogo di Unit Test](understanding-models-views-and-controllers-vb/_static/image2.jpg)](understanding-models-views-and-controllers-vb/_static/image3.png)

**Figura 02**: creare Unit Test finestra ([fare clic per visualizzare l'immagine ingrandita](understanding-models-views-and-controllers-vb/_static/image4.png))


Dopo il nuovo MVC ASP.NET viene creata l'applicazione. Si noterà diverse cartelle e file nella finestra Esplora soluzioni. In particolare, si noterà tre cartelle denominate modelli, visualizzazioni e controller. Come può immaginare dai nomi di cartella, queste cartelle contengono i file per l'implementazione di modelli, visualizzazioni e controller.

Se si espande la cartella Controllers, si noterà un file denominato AccountController.vb e un file denominato HomeController.vb. Se si espande la cartella Views, vedrai tre sottocartelle denominate Account, Home e condiviso. Se si espande la cartella Home, si noterà altri due file denominati About e Index.aspx (vedere la figura 3). Questi file è costituiscono l'applicazione di esempio incluso con il modello di MVC ASP.NET predefinito.


[![Finestra Esplora soluzioni](understanding-models-views-and-controllers-vb/_static/image3.jpg)](understanding-models-views-and-controllers-vb/_static/image5.png)

**Figura 03**: la finestra Esplora soluzioni ([fare clic per visualizzare l'immagine ingrandita](understanding-models-views-and-controllers-vb/_static/image6.png))


È possibile eseguire l'applicazione di esempio selezionando l'opzione di menu **Debug, Avvia debug**. In alternativa, è possibile premere il tasto F5.

Quando si esegue innanzitutto un'applicazione ASP.NET, viene visualizzata la finestra di dialogo nella figura 4 che consiglia di abilitare la modalità di debug. Fare clic sul pulsante OK e l'applicazione verrà eseguita.


[![Finestra di debug non abilitato](understanding-models-views-and-controllers-vb/_static/image4.jpg)](understanding-models-views-and-controllers-vb/_static/image7.png)

**Figura 04**: finestra di dialogo Debug non attivato ([fare clic per visualizzare l'immagine ingrandita](understanding-models-views-and-controllers-vb/_static/image8.png))


Quando si esegue un'applicazione ASP.NET MVC, Visual Studio avvia l'applicazione nel browser web. L'applicazione di esempio è costituito da due pagine: la pagina di indice e la pagina di informazioni. Avvio dell'applicazione prima di tutto, viene visualizzata la pagina di indice (vedere Figura 5). È possibile passare alla pagina informazioni facendo clic sul collegamento del menu nella parte superiore destra dell'applicazione.


[![La pagina di indice](understanding-models-views-and-controllers-vb/_static/image5.jpg)](understanding-models-views-and-controllers-vb/_static/image9.png)

**Figura 05**: la pagina di indice ([fare clic per visualizzare l'immagine ingrandita](understanding-models-views-and-controllers-vb/_static/image10.png))


Si noti l'URL nella barra degli indirizzi del browser. Ad esempio, quando si fa clic sul collegamento di menu, modifica l'URL nella barra degli indirizzi del browser per **/Home/About**.

Se si chiude la finestra del browser e tornare a Visual Studio, non sarà in grado di trovare un file con il percorso Home/circa. I file non esistano. Come è possibile?

## <a name="a-url-does-not-equal-a-page"></a>Un URL non è uguale a una pagina

Quando si compila un'applicazione Web Form ASP.NET tradizionale o un'applicazione Active Server Pages, sussiste una corrispondenza tra un URL e una pagina. Se richiesta una pagina denominata SomePage.aspx dal server, è meglio essere presente una pagina su disco denominato SomePage.aspx. Se il file SomePage.aspx non esiste, è possibile ottenere un'inutile **404 - pagina non trovata** errore.

Quando si compila un'applicazione ASP.NET MVC, al contrario, non è nessuna corrispondenza tra l'URL digitato nella barra degli indirizzi del browser e i file che trova nell'applicazione. In un'applicazione ASP.NET MVC, un URL corrisponde a un'azione del controller anziché una pagina su disco.

In un'applicazione ASP o ASP.NET tradizionale, alle richieste del browser vengono eseguito il mapping a pagine. In un'applicazione ASP.NET MVC, al contrario, vengono eseguito il mapping di richieste del browser per le azioni del controller. Un'applicazione Web Form ASP.NET è incentrato sul contenuto. Un'applicazione ASP.NET MVC, è invece la logica dell'applicazione basato su.

## <a name="understanding-aspnet-routing"></a>Informazioni sul Routing di ASP.NET

Viene eseguito il mapping di una richiesta del browser per un'azione del controller tramite una funzionalità del framework di ASP.NET denominato *Routing ASP.NET*. Routing ASP.NET viene usato dal framework ASP.NET MVC per *route* richieste in ingresso per le azioni del controller.

Routing ASP.NET utilizza una tabella di routing per gestire le richieste in ingresso. Questa tabella di route viene creata al primo avvio dell'applicazione web. La tabella di routing è configurato nel file Global. asax. Il file Global. asax MVC predefinito è contenuto in elenco 1.

**Elenco 1 - Global. asax**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample1.vb)]

Quando un'applicazione ASP.NET innanzitutto avvia, l'applicazione\_viene chiamato il metodo Start (). Nel listato 1, questo metodo chiama il metodo RegisterRoutes() e il metodo RegisterRoutes() crea la tabella di route predefinita.

La tabella di route predefinita è costituito da una route. Questa route predefinita interrompe tutte le richieste in ingresso in tre segmenti (un segmento URL è diverso tra le barre). Il primo segmento è mappato a un nome del controller, il secondo segmento è mappato a un nome di azione e il segmento finale viene eseguito il mapping a un parametro passato all'azione denominato ID.

Si consideri ad esempio l'URL seguente:

/ Prodotti/dettagli/3

Questo URL viene analizzato in tre parametri simile al seguente:

Controller = prodotto

Azione = dettagli

ID = 3

La route predefinita definita nel file Global. asax includa valori predefiniti per tutti e tre i parametri. Il valore predefinito è Home Controller, il valore predefinito di azione è l'indice e l'Id predefinito è una stringa vuota. Con queste impostazioni predefinite presenti, prendere in considerazione la modalità di analisi l'URL seguente:

Dipendente

Questo URL viene analizzato in tre parametri simile al seguente:

Controller = dipendente

Azione = indice

ID =

Infine, se si apre un'applicazione MVC ASP.NET senza fornire un URL (ad esempio, `http://localhost`) viene analizzato l'URL simile al seguente:

Controller Home =

Azione = indice

ID =

La richiesta viene indirizzata all'azione Index () sulla classe HomeController.

## <a name="understanding-controllers"></a>Informazioni sui controller

Un controller è responsabile per controllare il modo in cui un utente interagisce con un'applicazione MVC. Un controller contiene la logica di controllo di flusso per un'applicazione MVC ASP.NET. Un controller determina il tipo di risposta da inviare a un utente quando un utente effettua una richiesta del browser.

Un controller è semplicemente una classe (ad esempio, una classe c# o Visual Basic). L'esempio di applicazione MVC ASP.NET include un controller denominato HomeController.vb si trova nella cartella controller. Il contenuto del file HomeController.vb è riprodotta nel listato 2.

**Elenco di 2 - HomeController. cs**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample2.vb)]

Si noti che la classe HomeController sono disponibili due metodi denominati Index () e About (). Questi due metodi corrispondono alle due azioni esposte dal controller. Indice/URL /Home richiama il metodo HomeController.Index() e /Home URL/informazioni su richiama il metodo HomeController.About().

Qualsiasi metodo pubblico in un controller viene esposta come un'azione del controller. È necessario prestare attenzione a questo. Ciò significa che qualsiasi metodo pubblico contenuto in un controller può essere richiamato da chiunque abbia accesso a Internet immettendo l'URL corretto in un browser.

## <a name="understanding-views"></a>Informazioni sulle viste

Le azioni di due controller esposte dalla classe HomeController, Index () e About (), restituiscono entrambi una vista. Una vista contiene il markup HTML e il contenuto che viene inviato al browser. Una vista è l'equivalente di una pagina quando si lavora con un'applicazione MVC ASP.NET.

È necessario creare le visualizzazioni nella posizione corretta. L'azione HomeController.Index() restituisce una visualizzazione che si trova nel percorso seguente:

\Views\Home\Index.aspx

L'azione HomeController.About() restituisce una visualizzazione che si trova nel percorso seguente:

\Views\Home\About.aspx

In generale, se si desidera restituire una visualizzazione per un'azione del controller, quindi è necessario creare una sottocartella nella cartella viste con lo stesso nome del controller. All'interno della sottocartella, è necessario creare un file aspx con lo stesso nome di azione del controller.

Il file di listato 3 contiene la vista About.

**Elenco di 3 - About**

[!code-aspx[Main](understanding-models-views-and-controllers-vb/samples/sample3.aspx)]

Se si ignora la prima riga nel listato 3, la maggior parte del resto della vista è costituita da codice HTML standard. È possibile modificare il contenuto della visualizzazione tramite l'immissione di qualsiasi codice HTML da qui.

Una vista è molto simile a una pagina ASP o ASP.NET Web Forms. Una vista può contenere contenuto HTML e script. È possibile scrivere gli script del .NET preferito (ad esempio, c# o Visual Basic .NET) di linguaggio di programmazione. Gli script consentono di visualizzare il contenuto dinamico, ad esempio i dati del database.

## <a name="understanding-models"></a>Informazioni su modelli

Sono stati illustrati i controller e sono state illustrate le visualizzazioni. Nell'ultimo argomento che è necessario per discutere è modelli. Che cos'è un modello MVC?

Un modello MVC contiene tutta la logica dell'applicazione che non è inclusa in una vista o a un controller. Il modello deve contenere tutta la logica di business dell'applicazione, la logica di convalida e logica di accesso ai database. Ad esempio, se si utilizza Entity Framework Microsoft di accedere al database, quindi creare le classi di Entity Framework (file con estensione edmx) nella cartella Models.

Una vista deve contenere solo la logica relative alla generazione dell'interfaccia utente. Un controller deve contenere solo il livello minimo di logica necessaria per restituire la vista a destra o reindirizzare l'utente a un'altra azione (controllo di flusso). Tutti gli altri devono essere contenuti nel modello.

In generale, deve cercare modelli fat e skinny controller. I metodi del controller devono contenere solo poche righe di codice. Se un'azione del controller Ottiene troppo fat, quindi è consigliabile spostare la logica di per una nuova classe nella cartella Models.

## <a name="summary"></a>Riepilogo

In questa esercitazione viene fornita una panoramica delle diverse parti di MVC ASP.NET applicazione web. Si è appreso come il Routing di ASP.NET esegue il mapping in ingresso alle richieste del browser per le azioni del controller specifico. Si è appreso come controller di orchestrare come viste vengono restituite al browser. Infine, si è appreso come i modelli contengono business dell'applicazione, la convalida e logica di accesso ai database.
