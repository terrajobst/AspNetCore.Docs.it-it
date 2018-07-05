---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
title: Uso di TextBoxWatermark in un controllo FormView (c#) | Microsoft Docs
author: wenz
description: Il controllo di TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, lo posso...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e6ee90bf-32a5-4987-a384-15cc7dd30c8a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
msc.type: authoredcontent
ms.openlocfilehash: 1e646b685680071501d45f7454920def2b15a7db
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817038"
---
<a name="using-textboxwatermark-in-a-formview-c"></a>Uso di TextBoxWatermark in un controllo FormView (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)

> Il controllo di TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, viene svuotato. Se l'utente lascia la casella senza dover immettere testo, viene nuovamente visualizzato il testo precompilato. Questo è anche possibile all'interno di un controllo FormView.


## <a name="overview"></a>Panoramica

Il `TextBoxWatermark` controllo in AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, viene svuotato. Se l'utente lascia la casella senza dover immettere testo, viene nuovamente visualizzato il testo precompilato. Questo è anche possibile all'interno di un `FormView` controllo.

## <a name="steps"></a>Passaggi

Prima di tutto, è richiesta un'origine dati. Questo esempio Usa il database AdventureWorks e Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione express) ed è anche disponibile come download separato in [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks è parte di SQL Server 2005 Samples and Sample Databases (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.

In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è anche l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.

Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample1.aspx)]

Quindi, aggiungere un'origine dati alla pagina che supporta il `DELETE`, `INSERT` e `UPDATE` istruzioni SQL. Se si usa l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente non prefisso il nome della tabella (`Vendor`) con `Purchasing`. Il markup seguente mostra la sintassi corretta:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample2.aspx)]

Ricordare il nome (`ID`) dell'origine dati, poiché verrà usato nel `DataSourceID` proprietà del `FormView` controllo. Il `<InsertItemTemplate>` sezione il `FormView` contiene una casella di testo che viene esteso in base il `TextBoxWatermarkExtender` controllo. Assicurarsi che il dispositivo extender si trova all'interno del modello e non di fuori.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample3.aspx)]

A questo punto quando l'utente modifica la modalità di inserimento del `FormView` controllare, il campo di testo per il nuovo fornitore è precompilato grazie alla `TextBoxWatermarkExtender` controllo. Un clic nella casella di testo consente il testo di riempimento non vengono più visualizzati.


[![Il limite nel campo proviene il dispositivo extender](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)

Il limite nel campo proviene il dispositivo extender ([fare clic per visualizzare l'immagine con dimensioni normali](using-textboxwatermark-in-a-formview-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](using-textboxwatermark-with-validation-controls-cs.md)
