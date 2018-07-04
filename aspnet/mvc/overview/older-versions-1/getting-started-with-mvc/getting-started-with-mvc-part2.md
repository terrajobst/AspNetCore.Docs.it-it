---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Aggiunta di un Controller | Microsoft Docs
author: shanselman
description: Una versione aggiornata se questa esercitazione è disponibile qui utilizzando Visual Studio 2013. La nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti rispetto t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: f20889cf1c1cd9a2a69f0008d879b518c38334d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395669"
---
<a name="adding-a-controller"></a>Aggiunta di un controller
====================
da [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Una versione aggiornata se è disponibile in questa esercitazione [Ecco](../../getting-started/introduction/getting-started.md) utilizzando [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). La nuova esercitazione Usa ASP.NET MVC 5, che offre numerosi miglioramenti in questa esercitazione.
> 
> 
> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà una semplice applicazione web che legge e scrive da un database. Visitare il [centro di formazione di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


MVC è l'acronimo di modello, visualizzazione e Controller. MVC è un modello per lo sviluppo di applicazioni in modo che ogni parte è una responsabilità che è diversa da un altro.

- Modello: I dati dell'applicazione
- Visualizzazioni: I file di modello l'applicazione userà per generare dinamicamente risposte HTML.
- I controller: Classi che gestiscono le richieste di URL in ingresso all'applicazione, recuperare i dati del modello e quindi specificare i modelli di vista che eseguono il rendering di una risposta al client

Si verrà che coprono tutti questi concetti in questa esercitazione e mostrerò come usarle per compilare un'applicazione.

È possibile creare un nuovo controller facendo clic sulla cartella controller in Esplora soluzione e selezionando Aggiungi Controller.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Assegnare un nome al controller "HelloWorldController" e fare clic su Aggiungi.

[![Aggiungi finestra di dialogo Controller](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Si noti che in sul lato destro che è stato creato un nuovo file per è stato chiamato HelloWorldController.cs e tale file viene ora aperta Esplora soluzioni il **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Creare due nuovi metodi con un aspetto simile al seguente all'interno della nuova classe pubblica HelloWorldController. Si verrà restituita una stringa del codice HTML direttamente dal controller come esempio.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Il Controller è denominato HelloWorldController e il nuovo metodo e viene definito l'indice. Eseguire nuovamente, l'applicazione esattamente come in precedenza (fare clic sul pulsante di riproduzione o premere F5 per eseguire questa operazione). Dopo che è stata avviata nel browser, modificare il percorso nella barra degli indirizzi a `http://localhost:xx/HelloWorld` dove xx è qualsiasi numero di computer in uso ha scelto. A questo punto il browser dovrebbe essere simile allo screenshot seguente. Nel nostro metodo precedente viene restituita una stringa passata in un metodo chiamato "Contenuto". L'avevamo detto il sistema restituisce solo tag HTML e ha sempre fatto!

ASP.NET MVC richiama le classi Controller diverse (e diversi metodi di azione in esse contenute) a seconda dell'URL in ingresso. Per controllare il tipo di codice viene eseguito, la logica di mapping predefinito usata da ASP.NET MVC Usa un formato simile al seguente:

/ [Controller] / [ActionName] / [parametri]

La prima parte dell'URL determina la classe Controller da eseguire. Quindi, /HelloWorld esegue il mapping alla classe HelloWorldController. La seconda parte dell'URL determina il metodo di azione per la classe da eseguire. Quindi, /HelloWorld/Index provocherebbe il metodo Index () della classe HelloWorldcontroller da eseguire. Si noti che è stato solo necessario visitare /HelloWorld precedente e il metodo che indice definita in modo implicito. Ciò avviene perché un metodo denominato "Index" è il metodo predefinito che verrà chiamato su un controller se non ne viene specificato in modo esplicito.

[![Questa è l'azione predefinita](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

A questo punto, è possibile visitare `http://localhost:xx/HelloWorld/Welcome.` ora il metodo iniziale ha eseguito e ha restituito la stringa HTML.

Anche in questo caso / [Controller] / [ActionName] / [parametri] Controller è HelloWorld e Benvenuti in questo caso sono il metodo. È stata ancora eseguita ancora parametri.

[![Si tratta del metodo di azione iniziale](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

È possibile modificare leggermente l'esempio in modo che che possiamo fornire alcune informazioni dall'URL al controller, ad esempio simile al seguente: / HelloWorld/Welcome? name = Scott&amp;numtimes = 4. Impostare il metodo iniziale per includere due parametri e aggiornamento simile al seguente. Si noti che è stato utilizzato la funzionalità di C#-parametro facoltativo per indicare che il parametro numTimes deve valore predefinito è 1 se non viene passato.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Eseguire l'applicazione e visitare `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` la modifica del valore del nome e numtimes nel modo desiderato. Il sistema mappate automaticamente i parametri denominati dalla stringa di query nella barra degli indirizzi ai parametri nel metodo.

In entrambi questi esempi il controller è stata esegue tutto il lavoro e ha restituito HTML direttamente. In genere non si vuole che il controller restituisce direttamente - l'HTML dal momento che finisce per essere molto complessa al codice. Ma in genere si userà un file di modello di visualizzazione separato per generare la risposta HTML. Si esaminerà come è possibile farlo. Chiudere il browser e tornare all'IDE.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part1.md)
> [Successivo](getting-started-with-mvc-part3.md)
