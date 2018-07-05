---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Accesso ai dati del modello da un Controller | Microsoft Docs
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice che legge e scrive da un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: fe5111e2f103e9b20f27385421a1985062acaac1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371613"
---
<a name="accessing-your-models-data-from-a-controller"></a>Accesso ai dati del modello da un Controller
====================
da [Scott Hanselman](https://github.com/shanselman)

> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà una semplice applicazione web che legge e scrive da un database. Visitare il [centro di formazione di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


In questa sezione si intende creare una nuova classe MoviesController e scrivere codice che recupera i dati dei film e lo visualizza al browser usando un modello di visualizzazione.

Fare clic con il pulsante destro sulla cartella controller e creare un nuovo MoviesController.

[![Aggiungi Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Si creerà un nuovo file "MoviesController.cs" sotto la cartella \Controllers nostro progetto. È possibile aggiornare lo scaffolding di MovieController per recuperare l'elenco di filmati dal database appena popolato.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Stiamo eseguendo una query LINQ in modo che abbiamo solo recuperare i film rilasciate dopo estate del 1984. È necessario un modello di vista per eseguire il rendering di questo elenco di film nuovamente, quindi fare doppio clic su nel metodo e selezionare Aggiungi visualizzazione per la sua creazione.

Nella finestra di dialogo Aggiungi visualizzazione si indicherà che si passa un elenco&lt;Movies.Models.Movie&gt; al nostro modello di visualizzazione. A differenza dei tempi precedenti abbiamo utilizzato la finestra di dialogo Aggiungi visualizzazione e ha scelto di creare un modello "Vuoto", questa volta che si indicherà che intendiamo Visual Studio eseguire automaticamente "lo scaffolding di" un modello di vista per noi contenuto predefinito. Microsoft. puoi farlo selezionando l'elemento "List" all'interno di "menu a discesa contenuto visualizzazione.

Tenere presente che, quando si dispone di una classe creata una nuova classe, è necessario compilare l'applicazione per essere visualizzato nella finestra di dialogo di visualizzazione aggiunta.

![Aggiungi visualizzazione](getting-started-with-mvc-part5/_static/image3.png)

Fare clic su Aggiungi e il sistema genererà automaticamente il codice per una visualizzazione per noi che consente di visualizzare l'elenco di film. Questo è un buon momento per modificare la &lt;h2&gt; intestazione a un elemento, ad esempio "My Movie List", come già visto in precedenza con la visualizzazione di Hello World.

[![Film - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Eseguire l'applicazione e visitare /Movies nella barra degli indirizzi. A questo punto abbiamo dati recuperati dal database di utilizzando una query di base all'interno del Controller e i dati restituiti da una vista che riconosce sui film. Tale visualizzazione, quindi attraversa l'elenco di film e crea una tabella di dati per noi.

[![Elenco di film - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

È non implementa la funzionalità di modifica, dettagli e l'eliminazione con questa applicazione - è necessario il collegamento predefinito che il modello di scaffolding ha creato automaticamente. Aprire il file /Movies/Index.aspx e rimuoverli.

Ecco il codice sorgente per il modello di visualizzazione aggiornato aspetto dopo aver effettuato tali modifiche:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Crea collegamenti che che non sia necessario, pertanto verranno eliminati in questo esempio. Quanto è successivo verranno fornite, tuttavia, il collegamento Crea nuovo! Di seguito viene visualizzata l'App nell'app, ad esempio con tale colonna rimossa.

[![Elenco di film - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

È ora disponibile un elenco semplice i dati dei film. Tuttavia, se si fa clic sul collegamento "Crea nuovo", si verrà visualizzato un errore perché non è immune! È possibile implementare un metodo di azione Create e consentire agli utenti di immettere nuovi film nel database.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part4.md)
> [Successivo](getting-started-with-mvc-part6.md)
