---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
title: Utilizzo di TextBoxWatermark in un controllo FormView (c#) | Documenti Microsoft
author: wenz
description: Il controllo TextBoxWatermark AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, è possibile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6ee90bf-32a5-4987-a384-15cc7dd30c8a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
msc.type: authoredcontent
ms.openlocfilehash: 90e532d33756a995a85994254c48889517c5bb8d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-textboxwatermark-in-a-formview-c"></a>Utilizzo di TextBoxWatermark in un controllo FormView (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)

> Il controllo TextBoxWatermark AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, viene svuotato. Se l'utente lascia la casella senza l'immissione di testo, viene nuovamente visualizzato il testo precompilato. Questo vale anche all'interno di un controllo FormView.


## <a name="overview"></a>Panoramica

Il `TextBoxWatermark` controllo AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, viene svuotato. Se l'utente lascia la casella senza l'immissione di testo, viene nuovamente visualizzato il testo precompilato. Questo vale anche all'interno di un `FormView` controllo.

## <a name="steps"></a>Passaggi

Prima di tutto, un'origine dati è obbligatoria. Questo esempio viene utilizzato il database AdventureWorks e Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa express edition) ed è anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks fa parte di database di esempio e gli esempi di SQL Server 2005 (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.

In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample1.aspx)]

Quindi, aggiungere un'origine dati che supporta il `DELETE`, `INSERT` e `UPDATE` istruzioni SQL. Se si utilizza l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente come prefisso il nome della tabella (`Vendor`) con `Purchasing`. Il markup seguente viene illustrata la sintassi corretta:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample2.aspx)]

Ricordare il nome (`ID`) dell'origine dati, poiché verrà utilizzato nel `DataSourceID` proprietà del `FormView` controllo. Il `<InsertItemTemplate>` sezione del `FormView` contiene una casella di testo che viene esteso per il `TextBoxWatermarkExtender` controllo. Assicurarsi che il programma di estensione si trova all'interno del modello e non di fuori.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample3.aspx)]

A questo punto quando l'utente modifica in modalità di inserimento del `FormView` controllare, il campo di testo viene inserito il nuovo fornitore grazie al `TextBoxWatermarkExtender` controllo. Un clic nella casella di testo consente il testo di riempimento scomparire.


[![La filigrana nel campo proviene dal programma di estensione](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)

La filigrana nel campo viene dell'estensione ([fare clic per visualizzare l'immagine ingrandita](using-textboxwatermark-in-a-formview-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](using-textboxwatermark-with-validation-controls-cs.md)
