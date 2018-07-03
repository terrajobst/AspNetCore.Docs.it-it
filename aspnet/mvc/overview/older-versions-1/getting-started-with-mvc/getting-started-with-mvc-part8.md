---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Aggiunta di una colonna al modello. | Microsoft Docs
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice che legge e scrive da un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 241f9108394561fecb15db5b6efa2661fdb128d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361760"
---
<a name="adding-a-column-to-the-model"></a>Aggiunta di una colonna al modello
====================
da [Scott Hanselman](https://github.com/shanselman)

> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà una semplice applicazione web che legge e scrive da un database. Visitare il [centro di formazione di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


In questa sezione verrà illustrato come è possibile apportare modifiche allo schema del database e gestire le modifiche all'interno dell'applicazione.

Aggiungere una colonna "Rating" alla tabella dei film. Tornare all'IDE e fare clic su Esplora Database. Fare clic con il pulsante destro tabella Movie e selezionare Apri definizione tabella.

Aggiungere una colonna di "Rating" come indicato di seguito. Poiché ora non c'è alcun classificazioni, è possibile la colonna ammette valori null. Fare clic su Salva.

[![Modifica tabella film](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Successivamente, tornare a Esplora soluzioni e aprire il file Movies.edmx (che si trova nella cartella \Models). Fare clic con il pulsante destro sull'area di progettazione (area bianca) e selezionare il modello di aggiornamento dal Database.

[![Film - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Verrà avviata la "procedura guidata Aggiorna". Scegliere la scheda aggiornamento all'interno di esso e fare clic su Fine. La classe del modello Movie verrà quindi aggiornata con la nuova colonna.

![Aggiornamento guidato (2)](getting-started-with-mvc-part8/_static/image5.png)

Dopo aver fatto clic su Fine, è possibile visualizzare che la nuova colonna Rating è stata aggiunta all'entità film nel modello.

[![Entità film](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

È stata aggiunta una colonna nel modello di database, ma le visualizzazioni non sa su di esso.

## <a name="update-views-with-model-changes"></a>Aggiornare le viste con le modifiche al modello

Esistono alcuni modi è stato possibile aggiornare i modelli di visualizzazione in modo da riflettere la nuova colonna di punteggio. Poiché abbiamo creato queste visualizzazioni da generarli tramite la finestra di dialogo Aggiungi visualizzazione, è possibile eliminarli e ricrearli nuovamente. Tuttavia, in genere le persone saranno già state apportate delle modifiche ai propri modelli di visualizzazione dalla generazione iniziale sottoposto a scaffolding e saranno necessario aggiungere o eliminare i campi manualmente, esattamente come è stato fatto con il campo ID per la creazione.

Aprire il modello di \Views\Movies\Index.aspx e aggiungere una &lt;th&gt;Rating&lt;/th&gt; nell'intestazione della tabella Movie. È stato aggiunto il mio dopo Genre. Quindi, nella stessa posizione di colonna, ma basso, aggiungere una riga per restituire il nuovo controllo Rating.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Il modello di index. aspx finale avrà un aspetto simile al seguente:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

È possibile quindi aprire il modello di \Views\Movies\Create.aspx e aggiungere un'etichetta e una casella di testo per la nuova proprietà di classificazione:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Il modello di tornarci finale sarà simile al seguente e modificare titolo e replica secondaria del browser &lt;h2&gt; title su un valore come "Creazione di un film" mentre ci troviamo qui!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Eseguire l'app e a questo punto si ha un nuovo campo nel database che è stato aggiunto alla pagina Create. Aggiungere un nuovo film - questa volta con una classificazione uguale a - e fare clic su Crea.

[![Creare un filmato - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Dopo che si fa clic su Crea, si viene reindirizzati alla pagina di indice dove si nuovo film sia elencato con la nuova colonna di classificazione nel database

[![Elenco di film - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

Questa esercitazione di base tutto quello che ti a rendere i controller, associandole con viste e passano dati hardcoded. Quindi viene creato e progettato un Database e inserire alcuni dati in. Abbiamo i dati recuperati dal database e viene visualizzato in una tabella HTML. Quindi è stato aggiunto un modulo di creazione che consentono di aggiungere dati al database se stessi da all'interno dell'applicazione Web. Abbiamo aggiunto la convalida, verrà visualizzata la convalida usare JavaScript sul lato client. Infine, abbiamo modificato il database in modo da includere una nuova colonna di dati e successivamente aggiornare alle due pagine per creare e visualizzare questi nuovi dati.

Ora ti suggeriamo per proseguire con l'esercitazione di livello intermedio "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", nonché i video e le risorse a molti [ https://asp.net/mvc ](https://asp.net/mvc) per altre informazioni su ASP.NET MVC.

Buona lettura.

- Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) e [ @shanselman ](http://twitter.com/shanselman) su Twitter.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part7.md)
