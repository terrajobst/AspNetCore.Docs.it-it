---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Creazione di caselle di controllo che si escludono a vicenda (c#) | Microsoft Docs
author: wenz
description: "Quando è possibile selezionare solo uno di un set di opzioni, pulsanti di opzione in genere vengono usati. C'è però uno svantaggio: dopo aver selezionato un pulsante di opzione in un gruppo,..."
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d80771081a7aefefa115e68827c52092c6559bb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838021"
---
<a name="creating-mutually-exclusive-checkboxes-c"></a>Creazione di caselle di controllo che si escludono a vicenda (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Quando è possibile selezionare solo uno di un set di opzioni, pulsanti di opzione in genere vengono usati. C'è però uno svantaggio: dopo aver selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione. Le caselle di controllo può essere deselezionate in qualsiasi momento, tuttavia non si escludono a vicenda. Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.


## <a name="overview"></a>Panoramica

Quando è possibile selezionare solo uno di un set di opzioni, pulsanti di opzione in genere vengono usati. C'è però uno svantaggio: dopo aver selezionato un pulsante di opzione in un gruppo, non è possibile deselezionare tutti i pulsanti di opzione. Le caselle di controllo può essere deselezionate in qualsiasi momento, tuttavia non si escludono a vicenda. Questa esercitazione offre il meglio di entrambi gli approcci: caselle di controllo che si escludono a vicenda.

## <a name="steps"></a>Passaggi

ASP.NET AJAX Control Toolkit contiene il dispositivo extender MutuallyExclusiveCheckBox. Ciò consente ai programmatori di assegnare qualsiasi casella di controllo a un nome di gruppo (`Key` attributo). Da tutte le caselle di controllo nello stesso gruppo, è possibile selezionare solo uno alla volta.

Iniziamo con l'inserimento di due caselle di controllo in una nuova pagina ASP.NET. Possono esserci più, ma due di essi sono sufficienti per illustrare il principio:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Per entrambe le caselle di controllo, è necessario inserire un controllo MutuallyExclusiveCheckBoxExtender nella pagina. Entrambi gli attributi chiave devono avere lo stesso valore, proprio come il valore degli attributi degli elementi di pulsante di opzione HTML devono essere identici per indicare il gruppo che a cui appartengono. La proprietà TargetControlID dell'extender punta all'ID della casella di controllo.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Infine, includere ASP.NET AJAX `ScriptManager` necessaria per tutti gli elementi di ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Salvare ed eseguire la pagina: È possibile selezionare e deselezionare caselle di controllo, tuttavia in nessuna circostanza entrambe le caselle di controllo controllare.


[![Solo una casella di controllo può essere controllato in un momento](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Solo una casella di controllo può essere controllato in un momento ([fare clic per visualizzare l'immagine con dimensioni normali](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](creating-mutually-exclusive-checkboxes-vb.md)
