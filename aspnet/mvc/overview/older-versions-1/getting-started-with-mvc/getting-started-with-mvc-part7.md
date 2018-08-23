---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Aggiunta della convalida al modello | Microsoft Docs
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 4c7867587ba0610f0f1c23d9a0b9fbdc4040de7c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827502"
---
<a name="adding-validation-to-the-model"></a>Aggiunta della convalida al modello
====================
da [Scott Hanselman](https://github.com/shanselman)

> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà una semplice applicazione web che legge e scrive da un database. Visitare il [centro di formazione di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


In questa sezione si intende implementare il supporto necessario per abilitare la convalida dell'input all'interno dell'applicazione. Si farà in modo che il contenuto del database sia sempre corretto e fornire messaggi di errore utili agli utenti finali quando si prova a immettere dati dei film che non è validi. Inizieremo mediante l'aggiunta di un piccolo per la logica di convalida per la classe di film.

Fare clic con il pulsante destro sulla cartella del modello e selezionare Add Class. Assegnare un nome classe del film.

Quando abbiamo creato il modello di entità film in precedenza, l'IDE creato una classe di film. Infatti, può essere parte della classe di film in un file e parte in un altro. Si tratta di una classe parziale. Dobbiamo estende la classe di film da un altro file.

Si creerà una classe parziale film che punta a una "classe buddy" con alcuni attributi contenenti hint per la convalida al sistema. Verranno contrassegnare il titolo e il prezzo come obbligatorio e anche insistere che il prezzo di essere all'interno di un determinato intervallo. Fare clic con il pulsante destro della cartella Models e scegliere Aggiungi classe. Denominare la classe film e fare clic sul pulsante OK. Ecco l'aspetto di classe film parziale come.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Eseguire nuovamente l'applicazione e provare a immettere un film con un prezzo maggiori di 100. Si otterrà un errore dopo aver inviato il form. L'errore viene intercettato sul lato server e si verifica dopo l'invio del Form. Si noti come helper HTML predefiniti di ASP.NET MVC sono stati in grado di visualizzare il messaggio di errore e gestire i valori per noi all'interno di elementi nella casella di testo:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Questo è particolarmente efficace, ma sarebbe bello se potremmo l'utente viene informato sul lato client, immediatamente, prima che il server viene coinvolto.

È possibile abilitare alcune convalide sul lato client con JavaScript.

## <a name="adding-client-side-validation"></a>Aggiunta della convalida lato Client

Poiché la classe di film dispone già di alcuni attributi di convalida, è necessario solo aggiungere alcuni file JavaScript per il modello di vista di tornarci e aggiungere una riga di codice per abilitare la convalida lato client per l'implementazione.

Dall'interno VWD passare la cartella visualizzazioni/film e apriamo tornarci.

Aprire la cartella degli script in Esplora soluzioni e trascinare tre gli script seguenti all'interno di &lt;head&gt; tag.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Si desidera che questi file script vengono visualizzati nell'ordine indicato.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Inoltre, aggiungere quest'unica riga sopra il Html.BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Ecco il codice illustrato all'interno dell'IDE.

[![Film - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Eseguire l'applicazione e accedere di nuovo /Movies/Create e fare clic su Crea senza dover immettere tutti i dati. I messaggi di errore vengono visualizzati immediatamente senza la pagina flash che viene associato l'invio di dati fino al server. Si tratta poiché ASP.NET MVC è ora la convalida dell'input sia il client (tramite JavaScript) e nel server.

[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Ciò è bene Questo punto, aggiungere una colonna aggiuntiva al database.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part6.md)
> [Successivo](getting-started-with-mvc-part8.md)
