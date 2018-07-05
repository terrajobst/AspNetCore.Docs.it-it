---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Creazione di caselle di controllo si escludono a vicenda (VB) | Microsoft Docs
author: wenz
description: "Quando è possibile selezionare solo uno di un set di opzioni, pulsanti di opzione in genere vengono usati. C'è però uno svantaggio: dopo aver selezionato un pulsante di opzione in un gruppo,..."
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 6badd005ed4775cb248784f3e9991132db1b3969
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828799"
---
<a name="creating-mutually-exclusive-checkboxes-vb"></a>Creazione di caselle di controllo si escludono a vicenda (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Quando è possibile selezionare solo uno di un set di opzioni, pulsanti di opzione in genere vengono usati. C'è però uno svantaggio: dopo aver selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione. Le caselle di controllo può essere deselezionate in qualsiasi momento, tuttavia non si escludono a vicenda. Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.


## <a name="overview"></a>Panoramica

Quando è possibile selezionare solo uno di un set di opzioni, pulsanti di opzione in genere vengono usati. C'è però uno svantaggio: dopo aver selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione. Le caselle di controllo può essere deselezionate in qualsiasi momento, tuttavia non si escludono a vicenda. Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.

## <a name="steps"></a>Passaggi

ASP.NET AJAX Control Toolkit contiene il dispositivo extender MutuallyExclusiveCheckBox. Ciò consente ai programmatori di assegnare qualsiasi casella di controllo a un nome di gruppo (`Key` attributo). Da tutte le caselle di controllo nello stesso gruppo, è possibile selezionare solo uno alla volta.

Iniziamo con l'inserimento di due caselle di controllo in una nuova pagina ASP.NET. Possono esserci più, ma due di essi sono sufficienti per illustrare il principio:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Per entrambe le caselle di controllo, è necessario inserire un controllo MutuallyExclusiveCheckBoxExtender nella pagina. Entrambi gli attributi chiave devono avere lo stesso valore, proprio come il valore degli attributi degli elementi di pulsante di opzione HTML devono essere identici per indicare il gruppo che a cui appartengono. La proprietà TargetControlID dell'extender punta all'ID della casella di controllo.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Infine, includere ASP.NET AJAX `ScriptManager` necessaria per tutti gli elementi di ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Salvare ed eseguire la pagina: È possibile selezionare e deselezionare caselle di controllo, tuttavia in nessuna circostanza entrambe le caselle di controllo controllare.


[![Solo una casella di controllo può essere controllato in un momento](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Solo una casella di controllo può essere controllato in un momento ([fare clic per visualizzare l'immagine con dimensioni normali](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](creating-mutually-exclusive-checkboxes-cs.md)
