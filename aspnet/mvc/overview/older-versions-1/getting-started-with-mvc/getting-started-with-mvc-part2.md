---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Aggiunta di un Controller | Documenti Microsoft
author: shanselman
description: Una versione aggiornata, se in questa esercitazione è disponibile qui con Visual Studio 2013. Nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti rispetto t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: c6ecd1ffdd53a629d0079d57b85c7f6db2f316ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller"></a>Aggiunta di un controller
====================
da [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Una versione aggiornata se è disponibile in questa esercitazione [qui](../../getting-started/introduction/getting-started.md) utilizzando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti introdotti in questa esercitazione.
> 
> 
> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà un'applicazione web semplice la lettura e scrittura da un database. Visitare il [area risorse di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


MVC è l'acronimo di modello, visualizzazione e Controller. MVC è un modello per lo sviluppo di applicazioni in modo che ogni parte è una responsabilità che è diversa da un altro.

- Del modello: Dati dell'applicazione
- Visualizzazioni: I file di modello che dell'applicazione verrà utilizzati per generare dinamicamente le risposte HTML.
- Controller: Le classi che gestiscono le richieste di URL in ingresso all'applicazione, recuperare i dati del modello e quindi specificare i modelli di visualizzazione che eseguono il rendering di una risposta al client

Si verrà che coprono tutti questi concetti in questa esercitazione e viene illustrato come usarle per compilare un'applicazione.

Di seguito è possibile creare un nuovo controller facendo clic sulla cartella controller in Esplora soluzioni e scegliendo Aggiungi Controller.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Denominare il nuovo controller "HelloWorldController" e fare clic su Aggiungi.

[![Aggiungere una finestra di dialogo Controller](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Si noti che in a destra di un nuovo file è stato creato per è stato chiamato HelloWorldController.cs e tale file è ora aperto in Esplora soluzioni di **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Creare due nuovi metodi simili all'interno della nuova classe pubblica HelloWorldController. Ad esempio, si verrà restituire una stringa di HTML direttamente da questo controller.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Il Controller è denominato HelloWorldController e il nuovo metodo viene chiamato l'indice. Esecuzione dell'applicazione, esattamente come prima (fare clic sul pulsante play o premere F5 per eseguire questa operazione). Una volta che è stata avviata nel browser, modificare il percorso nella barra degli indirizzi per `http://localhost:xx/HelloWorld` dove xx rappresenta il numero di computer selezionato. Ora il browser dovrebbe essere analogo la schermata riportata di seguito. Nel metodo di precedente è restituita una stringa passata a un metodo denominato "Contenuto". Il sistema restituisce solo il codice HTML e aveva davvero!

ASP.NET MVC richiama diverse classi Controller (e diversi metodi di azione all'interno di essi) a seconda dell'URL in ingresso. Per controllare il codice viene eseguito, la logica di mapping predefinito utilizzata da ASP.NET MVC utilizza un formato simile al seguente:

/ [Controller] / [ActionName] / [parametri]

La prima parte dell'URL determina la classe Controller da eseguire. Pertanto, /HelloWorld esegue il mapping alla classe HelloWorldController. La seconda parte dell'URL determina il metodo di azione della classe per l'esecuzione. Operazione /HelloWorld/Index comporta il metodo Index () della classe HelloWorldcontroller da eseguire. Si noti che è stato solo da visitare /HelloWorld precedente e il metodo di che indice è implicito. In questo modo un metodo denominato "Index" è il metodo predefinito che verrà chiamato su un controller non è specificata in modo esplicito.

[![Si tratta di azione predefinito](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

A questo punto, si visitare `http://localhost:xx/HelloWorld/Welcome.` ora il metodo di completamento dell'installazione è eseguito e ha restituito la stringa HTML.

Nuovo / [Controller] / [ActionName] / [parametri] Controller è HelloWorld e iniziale è il metodo in questo caso. È stata ancora eseguita ancora parametri.

[![Si tratta del metodo di azione iniziale](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Modifichiamo esempio leggermente, in modo che è possibile passare alcune informazioni dall'URL per il controller, ad esempio simile al seguente: / HelloWorld/iniziale? name = Scott&amp;numtimes = 4. Modificare il metodo per includere i due parametri e come il seguente aggiornamento iniziale. Si noti che è stata utilizzata la funzionalità di c# parametro facoltativo per indicare che numTimes il parametro deve essere predefinito su 1 se non viene passato.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Eseguire l'applicazione e visitare `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` la modifica del valore del nome e numtimes nel modo desiderato. Il sistema mappato automaticamente i parametri denominati dalla stringa di query nella barra degli indirizzi per i parametri del metodo.

In entrambi questi esempi il controller è stato esegue tutto il lavoro e la restituzione direttamente HTML. In genere non vogliamo il controller di restituzione HTML direttamente - dal momento che finisce per essere molto complessa al codice. Invece in genere si userà un file di modello di visualizzazione separato per generare la risposta HTML. Vediamo come è possibile farlo. Chiudere il browser e tornare all'IDE.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part1.md)
> [Successivo](getting-started-with-mvc-part3.md)
