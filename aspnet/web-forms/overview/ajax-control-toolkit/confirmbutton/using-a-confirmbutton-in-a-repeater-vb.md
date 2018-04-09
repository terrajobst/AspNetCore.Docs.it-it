---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Utilizzo di un ConfirmButton In un ripetitore (VB) | Documenti Microsoft
author: wenz
description: L'estensione ConfirmButton AJAX Control Toolkit crea Sì/popup quando l'utente fa clic su un pulsante (inclusi controllo LinkButton). Solo se è Sì...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 89f412c242a3a5bcd10b72b7f0cbfb23705edb51
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a>Utilizzo di un ConfirmButton In un ripetitore (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> L'estensione ConfirmButton AJAX Control Toolkit crea Sì/popup quando l'utente fa clic su un pulsante (inclusi controllo LinkButton). Solo se si sceglie Sì, azione del pulsante viene eseguito, annullato in caso contrario. È anche possibile in un controllo repeater.


## <a name="overview"></a>Panoramica

L'estensione ConfirmButton AJAX Control Toolkit crea Sì/popup quando l'utente fa clic su un pulsante (inclusi controllo LinkButton). Solo se si sceglie Sì, azione del pulsante viene eseguito, annullato in caso contrario. È anche possibile in un controllo repeater.

## <a name="steps"></a>Passaggi

Prima di tutto, un'origine dati è obbligatoria. Questo esempio viene utilizzato il database AdventureWorks e Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa express edition) ed è anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks fa parte di database di esempio e gli esempi di SQL Server 2005 (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.

In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

Quindi, un'origine dati è obbligatoria. Per ragioni di semplicità, vengono recuperate solo i primi cinque voci nella tabella di fornitori di AdventureWorks. Si noti che quando si utilizza la procedura guidata di Visual Studio per creare l'origine dati, il nome della tabella (`Vendors`) è attualmente non correttamente preceduto da `Purchasing`. Il markup seguente è quello corretto:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

Questa origine dati può essere utilizzata quindi all'interno di un controllo repeater. Come al solito, la `DataBinder.Eval()` metodo recupera i dati dall'origine dati. Il `ConfirmButtonExtender` controllo deve quindi essere inserito all'interno di `<ItemTemplate>` sezione del ripetitore in modo che venga visualizzato per ogni voce nell'origine dati.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


[![Accanto a ogni voce dall'origine dati viene visualizzato il pulsante di conferma](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

Il pulsante di conferma viene visualizzato accanto a ogni voce dell'origine dati ([fare clic per visualizzare l'immagine ingrandita](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-a-confirmbutton-in-a-repeater-cs.md)
