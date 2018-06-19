---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Aggiunta di una colonna al modello | Documenti Microsoft
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice la lettura e scrittura da un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
ms.locfileid: "30879257"
---
<a name="adding-a-column-to-the-model"></a>Aggiunta di una colonna al modello
====================
da [Scott Hanselman](https://github.com/shanselman)

> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà un'applicazione web semplice la lettura e scrittura da un database. Visitare il [area risorse di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


In questa sezione verrà illustrata come è possibile apportare modifiche allo schema del database e gestire le modifiche all'interno dell'applicazione.

Aggiungere una colonna "Rating" alla tabella dei film. Tornare all'IDE e fare clic su Esplora Database. Fare clic con il pulsante destro nella tabella di film e selezionare Apri definizione tabella.

Aggiungere una colonna di "Livello", come illustrato di seguito. Poiché non sono ora le classificazioni, la colonna consente valori null. Fare clic su Salva.

[![Modifica tabella filmati](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

Successivamente, tornare a Esplora soluzioni e aprire il file Movies.edmx (che si trova nella cartella \Models). Fare clic con il pulsante destro nell'area di progettazione (area bianca) e selezionare il modello di aggiornamento dal Database.

[![Filmati - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Verrà avviata il "aggiornamento guidato". Fare clic sulla scheda di aggiornamento all'interno di esso e fare clic su Fine. La classe modello film verrà quindi aggiornata con la nuova colonna.

![Aggiornamento guidato (2)](getting-started-with-mvc-part8/_static/image5.png)

Dopo aver fatto clic su Fine, è possibile visualizzare che la nuova colonna di classificazione è stato aggiunto all'entità nel modello in esame film.

[![Entità film](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

È stato aggiunto a una colonna nel modello di database, ma le visualizzazioni non sa su di esso.

## <a name="update-views-with-model-changes"></a>Aggiornamento delle visualizzazioni con le modifiche al modello

Esistono diversi modi, è possibile aggiornare i modelli di visualizzazione per riflettere la nuova colonna di classificazione. Queste viste è stato creato da generarli tramite la finestra di dialogo Aggiungi visualizzazione, è possibile eliminarli e ricrearli nuovamente. Tuttavia, in genere gli utenti saranno già apportate le modifiche apportate per i modelli di visualizzazione dalla generazione di scaffolding iniziale e saranno necessario aggiungere o eliminare i campi manualmente, esattamente come è stato fatto con il campo ID per la creazione.

Aprire il modello \Views\Movies\Index.aspx e aggiungere un &lt;th&gt;classificazione&lt;/th&gt; all'intestazione della tabella di film. Il mio aggiunto dopo il genere. Quindi, nella stessa posizione di colonna, ma più in basso, aggiungere una riga per la nuova classificazione di output.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Il nostro modello Index.aspx finale sarà simile al seguente:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Verrà quindi aprire il modello \Views\Movies\Create.aspx e aggiungere un'etichetta e una casella di testo per la nuova proprietà di classificazione:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Il nostro modello Create.aspx finale sarà simile al seguente e modificare titolo e secondaria del browser &lt;h2&gt; titolo simile al seguente "Creare un filmato" mentre ci troviamo in!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Eseguire l'app e a questo punto si dispone di un nuovo campo nel database che è stato aggiunto alla pagina di creazione. Aggiungere un nuovo filmato - questa volta con una classificazione uguale a - e fare clic su Crea.

[![Creare un filmato - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Dopo aver fatto clic su Crea, è inviata alla pagina di indice in cui si nuovi film sia elencato con la nuova colonna nel database di classificazione

[![Elenco di film - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

In questa esercitazione di base ottenuto è iniziato le controller, associarli a viste e al passaggio dati hardcoded. Quindi è creato e progettato un Database e inserire alcuni dati in. Abbiamo recuperati i dati dal database e viene visualizzato in una tabella HTML. Quindi è stato aggiunto un modulo di creazione per consentire all'utente di aggiungere dati al database stessi da all'interno dell'applicazione Web. È l'aggiunta di convalida, quindi effettuata la convalida di utilizzare JavaScript sul lato client. Infine, è modificato il database per includere una nuova colonna di dati, quindi aggiornare le due pagine per creare e visualizzare questi nuovi dati.

È possibile ora invita gli utenti per continuare con l'esercitazione di livello intermedio "[negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", nonché numerosi video e risorse [ https://asp.net/mvc ](https://asp.net/mvc) per ulteriori informazioni su ASP.NET MVC!

Buona lettura.

- Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) e [ @shanselman ](http://twitter.com/shanselman) su Twitter.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part7.md)
