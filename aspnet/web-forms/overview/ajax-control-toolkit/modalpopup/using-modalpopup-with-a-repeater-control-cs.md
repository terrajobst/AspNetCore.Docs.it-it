---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: Uso di ModalPopup con un controllo Repeater (c#) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. È anche possibile usare questo Contr....
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7e6136bac48b7c1b0931cada66501d303be8e927
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371077"
---
<a name="using-modalpopup-with-a-repeater-control-c"></a>Uso di ModalPopup con un controllo Repeater (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)

> Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. È anche possibile usare questo controllo all'interno di un controllo repeater.


## <a name="overview"></a>Panoramica

Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. È anche possibile usare questo controllo all'interno di un controllo repeater.

## <a name="steps"></a>Passaggi

Prima di tutto, è richiesta un'origine dati. Questo esempio Usa il database AdventureWorks e Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione express) ed è anche disponibile come download separato in [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks è parte di SQL Server 2005 Samples and Sample Databases (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per configurare il database è di utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database. In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è anche l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database. Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

Quindi, aggiungere un'origine dati alla pagina. Per usare una quantità limitata di dati, selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks. Se si usa l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente non prefisso il nome della tabella (`Vendor`) con `Purchasing`. Il markup seguente mostra la sintassi corretta:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup modale. Contiene un `Button` controllo a chiudere la finestra popup:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

Per rendere il popup di lavorare all'interno di repeater, il `ModalPopupExtender` controllo deve essere inserito all'interno di `<ItemTemplate>` sezione del ripetitore. Il pannello non è compreso il repeater, ma il dispositivo extender si trova all'interno. Ecco il markup per il controllo repeater:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

Quindi, ogni elemento nell'origine dati viene visualizzato con un pulsante accanto a esso che attiva la finestra popup modale.


[![La finestra popup modale può essere attivato per ogni voce di origine dati](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)

La finestra popup modale può essere attivato per ogni voce di origine dati ([fare clic per visualizzare l'immagine con dimensioni normali](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](launching-a-modal-popup-window-from-server-code-cs.md)
> [Successivo](handling-postbacks-from-a-modalpopup-cs.md)
