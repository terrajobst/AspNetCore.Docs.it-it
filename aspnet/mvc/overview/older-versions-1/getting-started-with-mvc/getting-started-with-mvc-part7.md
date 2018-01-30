---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Aggiunta della convalida per il modello | Documenti Microsoft
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice la lettura e scrittura da un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 5616c3c3bc77be0a770540d04cc2ae48ba9eedff
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
<a name="adding-validation-to-the-model"></a>Aggiunta della convalida per il modello
====================
da [Scott Hanselman](https://github.com/shanselman)

> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà un'applicazione web semplice la lettura e scrittura da un database. Visitare il [area risorse di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


In questa sezione verrà per implementare il supporto necessario per abilitare la convalida dell'input interno dell'applicazione. Si farà in modo che il contenuto del database è sempre corretto e fornire utili messaggi di errore per gli utenti finali quando immettere dati filmato che non sono validi e riprovare. Inizieremo aggiungendo un po' logica di convalida per la classe di film.

Fare clic con il pulsante destro sulla cartella del modello e selezionare Aggiungi classe. Nome classe del film.

Quando abbiamo creato il modello di entità film in precedenza, l'IDE creata una classe di film. In effetti, parte della classe film può essere in un file e una parte in un altro. Si tratta di una classe parziale. Si intende estendere la classe di film da un altro file.

Verrà creata una classe parziale filmato che punta a una "classe buddy" con alcuni attributi che genererà l'hint per la convalida per il sistema. Verrà contrassegnare il titolo e il prezzo come richiesto e insistere anche che il prezzo deve essere all'interno di un determinato intervallo. Fare clic sulla cartella modelli e selezionare Aggiungi classe. Nome classe del filmato e fare clic sul pulsante OK. Di seguito è presentato il nostro parziale film classe.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Eseguire nuovamente l'applicazione e provare a immettere un filmato con un prezzo maggiori di 100. Dopo avere inviato il form viene visualizzato un errore. L'errore viene intercettato sul lato server e si verifica dopo l'invio del Form. Si noti come helper HTML incorporato di ASP.NET MVC sono stati abbastanza per visualizzare il messaggio di errore e gestire i valori per noi all'interno gli elementi della casella di testo:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Questo è particolarmente efficace, ma sarebbe utile se è stato possibile indicare all'utente sul lato client, immediatamente, prima che il server ottiene coinvolto.

Consente di abilitare una convalida sul lato client con JavaScript.

## <a name="adding-client-side-validation"></a>Aggiunta della convalida lato Client

Poiché la classe di film dispone già di alcuni attributi di convalida, è necessario solo aggiungere alcuni file JavaScript per il modello di visualizzazione Create.aspx e aggiungere una riga di codice per abilitare la convalida lato client da eseguire sul posto.

Dall'interno VWD passare la cartella viste o film e aprire Create.aspx.

Aprire la cartella di script in Esplora soluzioni e trascinare gli script seguenti tre all'interno di &lt;head&gt; tag.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Si desidera questi file di script vengono visualizzati nell'ordine indicato.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Inoltre, aggiungere quest'unica riga sopra il Html.BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Di seguito è riportato il codice mostrato all'interno dell'IDE.

[![Filmati - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Eseguire l'applicazione e visitare di nuovo /Movies/Create e fare clic su Crea senza dover immettere tutti i dati. I messaggi di errore vengono visualizzati immediatamente senza la pagina flash che è associato l'invio di dati completamente al server. Si tratta poiché ASP.NET MVC ora convalida l'input sia il client (tramite JavaScript) e nel server.

[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Questa ricerca buona! Aggiungere ora una colonna aggiuntiva per il database.

>[!div class="step-by-step"]
[Precedente](getting-started-with-mvc-part6.md)
[Successivo](getting-started-with-mvc-part8.md)
