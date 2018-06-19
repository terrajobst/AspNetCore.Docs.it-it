---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Creazione di caselle di controllo si escludono a vicenda (VB) | Documenti Microsoft
author: wenz
description: 'Quando può essere selezionato solo uno di un set di opzioni, pulsanti di opzione in genere vengono utilizzati. È uno svantaggio, tuttavia: dopo aver selezionato un pulsante di opzione in un gruppo,...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: cdb93a080fb62cfdc7e3ff0604a1447e2bb99071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874252"
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a>Creazione di caselle di controllo si escludono a vicenda (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Quando può essere selezionato solo uno di un set di opzioni, pulsanti di opzione in genere vengono utilizzati. È uno svantaggio, tuttavia: dopo aver selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione. Caselle di controllo può essere deselezionate in qualsiasi momento, tuttavia non si escludono a vicenda. In questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.


## <a name="overview"></a>Panoramica

Quando può essere selezionato solo uno di un set di opzioni, pulsanti di opzione in genere vengono utilizzati. È uno svantaggio, tuttavia: dopo aver selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione. Caselle di controllo può essere deselezionate in qualsiasi momento, tuttavia non si escludono a vicenda. In questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.

## <a name="steps"></a>Passaggi

ASP.NET AJAX Control Toolkit contiene l'estensione MutuallyExclusiveCheckBox. Questo consente ai programmatori di assegnare un nome di gruppo di qualsiasi casella di controllo (`Key` attributo). Da tutte le caselle di controllo nello stesso gruppo, è possibile selezionare solo una alla volta.

Per iniziare l'inserimento di due caselle di controllo in una nuova pagina ASP.NET. Possono esistere più, ma due di essi sono sufficienti per illustrare il principio:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Per entrambe le caselle di controllo, un controllo MutuallyExclusiveCheckBoxExtender deve essere inserito nella pagina. Entrambi gli attributi di chiave necessario hanno lo stesso valore, come il valore degli attributi degli elementi di pulsante di opzione HTML devono essere identici per indicare il gruppo che cui appartengono. La proprietà TargetControlID dei punti di estensione per l'ID della casella di controllo.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Infine, includere ASP.NET AJAX `ScriptManager` che sono richieste da tutti gli elementi di ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Salvare ed eseguire la pagina: È possibile selezionare e deselezionare caselle di controllo, tuttavia non entrambe le caselle di controllo controllare.


[![Solo una casella di controllo può essere verificata in un momento](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Solo una casella di controllo può essere verificata in un momento ([fare clic per visualizzare l'immagine ingrandita](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](creating-mutually-exclusive-checkboxes-cs.md)
