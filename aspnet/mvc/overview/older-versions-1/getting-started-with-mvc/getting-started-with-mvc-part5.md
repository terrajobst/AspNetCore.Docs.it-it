---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Accesso ai dati del modello da un Controller | Documenti Microsoft
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice la lettura e scrittura da un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Accesso ai dati del modello da un Controller
====================
da [Scott Hanselman](https://github.com/shanselman)

> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà un'applicazione web semplice la lettura e scrittura da un database. Visitare il [area risorse di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


In questa sezione verrà per creare una nuova classe MoviesController e scrivere codice che recupera i dati di film e visualizzarlo nel browser utilizzando un modello di visualizzazione.

Fare clic con il pulsante destro sulla cartella controller e rendere un nuovo MoviesController.

[![Aggiungi Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Verrà creato un nuovo file "MoviesController.cs" sotto la cartella \Controllers all'interno del progetto. Si aggiorna il MovieController per recuperare l'elenco di film dal database appena compilato.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Stiamo eseguendo una query LINQ in modo che è possibile recuperare solo filmati rilasciati dopo estate del 1984. È necessario un modello di visualizzazione per il rendering di questo elenco di film nuovamente, pertanto nel metodo e scegliere Aggiungi visualizzazione per la sua creazione.

Nella finestra di dialogo Aggiungi visualizzazione è verrà indicato che viene passato un elenco&lt;Movies.Models.Movie&gt; per il modello di visualizzazione. A differenza di ore precedenti è utilizzata la finestra di dialogo Aggiungi visualizzazione e si sceglie di creare un modello "Vuoto", questa volta che è possibile indicare che si vuole che Visual Studio per automaticamente "lo scaffolding" un modello di visualizzazione per noi con contenuto predefinito. È questo scopo, selezionare l'elemento "List" in "menu a discesa di contenuto di visualizzazione.

Tenere presente che dopo avere creato una nuova classe, è necessario compilare l'applicazione per poter essere visualizzato nella finestra di dialogo Aggiungi visualizzazione.

![Aggiungi visualizzazione](getting-started-with-mvc-part5/_static/image3.png)

Fare clic su Aggiungi e il sistema genera automaticamente il codice per una vista per noi che consente di visualizzare l'elenco di film. Ciò è utile per modificare il &lt;h2&gt; diretto al simile al seguente "Elenco di film personali", come abbiamo fatto in precedenza con la visualizzazione di Hello World.

[![Filmati - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Eseguire l'applicazione e visitare /Movies nella barra degli indirizzi. Ora progettuali recuperati dati dal database di utilizzando una query di base all'interno del Controller e i dati restituiti a una vista che viene a conoscenza film. Tale visualizzazione attraversa l'elenco di film quindi crea una tabella di dati per Microsoft.

[![Elenco di film - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

È non implementa la funzionalità di modifica, dettagli e Delete con questa applicazione - è necessario il collegamento predefinito che il modello di scaffolding creato. Aprire il file /Movies/Index.aspx e rimuoverli.

Ecco il codice sorgente per il modello di visualizzazione aggiornato aspetto dopo aver effettuato le modifiche:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

La creazione collegamenti non necessari, pertanto è verranno eliminati in questo esempio. Si manterrà il nuovo collegamento, tuttavia, che è successiva! Ecco l'aspetto dell'app a tale colonna rimossa.

[![Elenco di film - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

È ora disponibile un elenco semplice dei dati del film. Tuttavia, se si fa clic sul collegamento "Crea nuovo", si verrà generato un errore perché non è collegato. Si implementa un metodo di azione Create e consentire agli utenti di immettere nuovi film nel database.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part4.md)
> [Successivo](getting-started-with-mvc-part6.md)
