---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: Utilizzo ModalPopup con un controllo Repeater (c#) | Documenti Microsoft
author: wenz
description: Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. È anche possibile usare questo Contr....
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7124ca3fc346d78c3b235d1756695b008afb47aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872946"
---
<a name="using-modalpopup-with-a-repeater-control-c"></a>Utilizzo ModalPopup con un controllo Repeater (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)

> Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. È inoltre possibile utilizzare il controllo all'interno di un controllo repeater.


## <a name="overview"></a>Panoramica

Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. È inoltre possibile utilizzare il controllo all'interno di un controllo repeater.

## <a name="steps"></a>Passaggi

Prima di tutto, un'origine dati è obbligatoria. Questo esempio viene utilizzato il database AdventureWorks e Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa express edition) ed è anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks fa parte di database di esempio e gli esempi di SQL Server 2005 (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database. In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita. Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database. Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

Quindi, aggiungere un'origine dati per la pagina. Per utilizzare una quantità limitata di dati, selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks. Se si utilizza l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente come prefisso il nome della tabella (`Vendor`) con `Purchasing`. Il markup seguente viene illustrata la sintassi corretta:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup modale. Contiene un `Button` controllo a chiudere la finestra popup:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

Per fare in modo che la finestra popup all'interno di repeater, il `ModalPopupExtender` controllo deve essere inserito all'interno di `<ItemTemplate>` sezione del ripetitore. Il pannello è di fuori di repeater, ma il programma di estensione si trova all'interno. Di seguito è riportato il markup per il controllo repeater:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

Quindi, ogni elemento nell'origine dati viene visualizzato con un pulsante accanto che attiva il popup modale.


[![La finestra popup modale può essere attivato per ogni voce relativa all'origine dati](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)

Il popup modale può essere attivato per ogni voce relativa all'origine dati ([fare clic per visualizzare l'immagine ingrandita](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](launching-a-modal-popup-window-from-server-code-cs.md)
> [Successivo](handling-postbacks-from-a-modalpopup-cs.md)
