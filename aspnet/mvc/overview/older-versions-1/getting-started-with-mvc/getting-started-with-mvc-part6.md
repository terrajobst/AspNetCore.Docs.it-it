---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Aggiunta di un creare metodo e vista | Microsoft Docs
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 546c3e0a24ecd0d916c79e9ad12f62b926c760c5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832159"
---
<a name="adding-a-create-method-and-create-view"></a>Aggiunta di un creare metodo e vista
====================
da [Scott Hanselman](https://github.com/shanselman)

> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà una semplice applicazione web che legge e scrive da un database. Visitare il [centro di formazione di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


In questa sezione si intende implementare il supporto necessario per consentire agli utenti di creare nuovi film nel database. Faremo implementando l'azione URL Movies/Create.

L'URL di Movies/Create l'implementazione è un processo in due passaggi. Quando un utente visita prima di tutto l'URL di Movies/Create si vuole visualizzare in un form HTML che è possibile compilare per immettere un nuovo film. Quindi, quando l'utente invia il form e gli invii che i dati di postback al server, si vuole recuperare il contenuto registrato e salvarlo nel database.

Che verrà implementato questi due passaggi all'interno di due metodi Create () all'interno della classe MoviesController. Un metodo consente di visualizzare il &lt;form&gt; che l'utente deve compilare per creare un nuovo film. Il secondo metodo gestirà l'elaborazione dei dati inviati quando l'utente invia il &lt;form&gt; il postback al server e salvare un nuovo film nel database.

Di seguito è riportato il codice verrà aggiunto alla nostra classe MoviesController per implementare questa:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Il codice sopra riportato contiene tutto il codice che avremo bisogno all'interno di Controller.

Verrà ora implementata il modello Create View che verranno utilizzate per visualizzare un form all'utente. Si sarà fare clic con il pulsante destro nel primo metodo di creazione e selezionare "Aggiungi visualizzazione" per creare il modello di visualizzazione per il modulo di film.

Sceglieremo ora che abbiamo si decide di passare il modello di vista di un film"" come la classe di visualizzazione dei dati e indicare che desideriamo "lo scaffolding di" un modello di "Crea".

[![Aggiungi visualizzazione](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Dopo aver fatto clic sul pulsante Aggiungi modello di vista \Movies\Create.aspx verrà creato automaticamente. Poiché sono stati selezionati nell'elenco a discesa "visualizzare il contenuto" "Crea", la finestra di dialogo Aggiungi visualizzazione automaticamente "sottoposto a scaffolding" del contenuto predefinito per noi. Lo scaffolding creato un elemento HTML &lt;form&gt;, per passare una posizione per errore di convalida dei messaggi e poiché lo scaffolding SA sui film, creata etichetta e i campi per ogni proprietà della nostra classe.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Poiché il nostro database fornisce automaticamente un film un ID, è possibile rimuovere tali campi di tale modello di riferimento. ID dalla visualizzazione Create. Rimuovere le 7 righe dopo &lt;legenda&gt;campi&lt;/legend&gt; che mostrano il campo ID non vogliamo.

È possibile ora creare un nuovo film e aggiungerlo al database. Verrà effettuare questa operazione eseguendo nuovamente l'applicazione e visitare il "/ film" URL e fare clic sul collegamento "Crea" per aggiungere un nuovo film.

[![Create - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Quando si fa clic sul pulsante Crea, verranno resi disponibili (tramite HTTP POST) i dati in questo modulo per il metodo /Movies/Create appena creato. Proprio come quando automaticamente il sistema ha richiesto il parametro "numTimes" e "name" all'URL esterno e li mappati ai parametri in un metodo in precedenza, il sistema verrà automaticamente portare i campi del modulo da una richiesta POST ed eseguirne il mapping a un oggetto. In questo caso, i valori dei campi in formato HTML, ad esempio "ReleaseDate" e "Title" automaticamente essere inseriti nelle proprietà del corretta di una nuova istanza di un film.

Verrà ora esaminato il secondo metodo Create dal nostro MoviesController nuovamente. Si noti come richiede un oggetto "Film" come argomento:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Questo oggetto è stato quindi passato alla versione [HttpPost] del nostro metodo di azione Create e salvato nel database e quindi l'utente viene reindirizzato al metodo di azione Index () che consente di visualizzare il risultato salvato nell'elenco di film:

[![Elenco di film - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

È non verifica se il film siano corretti, tuttavia, e il database non consentirà di salvare un film con nessun titolo. Sarebbe bello se potremmo l'utente viene informato che prima il database ha generato un errore. Faremo successiva mediante l'aggiunta di supporto per la convalida per l'applicazione.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part5.md)
> [Successivo](getting-started-with-mvc-part7.md)
