---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Aggiunta di un metodo di creazione e creare vista | Documenti Microsoft
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice la lettura e scrittura da un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 48e656a0c394b9db5baaec9c557ec38c4020d41b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-create-method-and-create-view"></a>Aggiunta di un metodo di creazione e creare vista
====================
da [Scott Hanselman](https://github.com/shanselman)

> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà un'applicazione web semplice la lettura e scrittura da un database. Visitare il [area risorse di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


In questa sezione verrà per implementare il supporto necessario per consentire agli utenti di creare nuovi film nel database. Faremo implementando l'azione URL filmati/Create.

Implementare l'URL di filmati/Create è un processo in due fasi. Quando un utente visita innanzitutto l'URL di filmati/Create desideriamo mostrare un form HTML che è possibile compilare per immettere un nuovo filmato. Quando l'utente invia il form e gli invii che i dati di postback al server, si desidera recuperare il contenuto registrato e salvarlo nel database.

Questi due passaggi all'interno di due metodi di metodo di creazione verrà implementato all'interno della classe MoviesController. Verrà visualizzato un metodo di &lt;modulo&gt; che l'utente deve compilare per creare un nuovo film. Il secondo metodo gestisce l'elaborazione dei dati inviati quando l'utente invia il &lt;modulo&gt; il postback al server e salvare un nuovo filmato all'interno del database.

Di seguito è riportato il codice verrà aggiunto alla classe MoviesController per implementare questa:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Il codice sopra riportato contiene tutto il codice che è necessario all'interno di questo Controller.

Verrà ora implementata il modello Create View che verranno utilizzate per visualizzare un form all'utente. Si sarà fare clic con il primo metodo di creazione e selezionare "Aggiungi visualizzazione" per creare il modello di visualizzazione per il modulo di film.

Si selezionerà si decide di passare il modello di visualizzazione di un filmato"" come la classe di visualizzazione dati, e indicare che si desidera "lo scaffolding" modello "Creazione".

[![Aggiungi visualizzazione](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Dopo aver fatto clic sul pulsante Aggiungi modello di visualizzazione \Movies\Create.aspx verrà creato automaticamente. Poiché è selezionata l'opzione "Crea" nell'elenco a discesa "Visualizza contenuto", la finestra di dialogo Aggiungi visualizzazione automaticamente "scaffolding" del contenuto predefinito per noi. Lo scaffolding creato HTML &lt;modulo&gt;, per passare una posizione per errore di convalida dei messaggi e poiché lo scaffolding conoscenza di filmati, creata etichetta e i campi per ogni proprietà di classe.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Poiché il database offre un filmato automaticamente un ID, rimuovere tali campi tale modello di riferimento. ID dalla visualizzazione di creazione. Rimuovere le righe dopo 7 &lt;legenda&gt;campi&lt;/legend&gt; come visualizzano il campo ID non vogliamo.

Verrà ora creare un nuovo filmato e aggiungerlo al database. Verrà questo scopo, eseguire nuovamente l'applicazione e visitare il "/ filmati" URL e fare clic su "Crea" collegamento per aggiungere un nuovo film.

[![Create - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Quando si fa clic sul pulsante Crea, verranno resi disponibili (tramite HTTP POST) i dati del form per il metodo /Movies/Create che abbiamo appena creato. Analogamente a quando automaticamente il sistema ha impiegato il parametro "numTimes" e "name" all'URL e li mappati ai parametri su un metodo in precedenza, il sistema automaticamente richiedere i campi del modulo da un POST e associarli a un oggetto. In questo caso, i valori dei campi in formato HTML come "ReleaseDate" e "Title" verranno automaticamente messe nelle proprietà corrette di una nuova istanza di un film.

Esaminiamo il secondo metodo Create dal nostro MoviesController nuovamente. Si noti come accetta un oggetto di "Filmato" come argomento:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Questo oggetto film quindi passato alla versione [HttpPost] del metodo di azione Create e viene salvato nel database e quindi l'utente viene reindirizzato al metodo di azione Index () che consente di visualizzare il risultato salvato nell'elenco di film:

[![Elenco di film - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

È non verifica se i filmati siano corretti, tuttavia, e il database non consente di salvare un filmato con nessun titolo. Sarebbe utile se è stato possibile indicare all'utente di database ha generato un errore. Successiva attività verranno eseguite tramite l'aggiunta di supporto della convalida per l'applicazione.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part5.md)
> [Successivo](getting-started-with-mvc-part7.md)
