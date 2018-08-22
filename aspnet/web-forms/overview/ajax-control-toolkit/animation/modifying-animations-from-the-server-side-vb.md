---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modifica delle animazioni dal lato Server (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Le animazioni possono inoltre...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: ef32c8f4846b18f11d816a64a3e4292b67b232e9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826946"
---
<a name="modifying-animations-from-the-server-side-vb"></a>Modifica delle animazioni dal lato Server (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Le animazioni possono anche essere modificate sul lato server


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Le animazioni possono anche essere modificate sul lato server

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

Il resto del codice viene eseguito sul lato server e non viene utilizzato markup. Usa invece codice per creare il `AnimationExtender` controllo:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Tuttavia, il Toolkit di controllo attualmente non fornisce un accesso all'API per creare le animazioni singoli. È tuttavia possibile impostare il `AnimationExtender`proprietà Animations in una stringa che contiene il tag XML utilizzato quando si assegnano le animazioni in modo dichiarativo. Per creare il codice XML che non deve contenere il `<Animations>` elemento è possibile usare il XML di .NET Framework supportano o, come nel codice seguente, è sufficiente fornire la stringa:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Infine, aggiungere il `AnimationExtender` controllo alla pagina corrente, all'interno di `<form runat="server">` elemento, assicurandosi che l'animazione è inclusa e viene eseguito:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![L'animazione viene creata usando codice c# /VB C sul lato server](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

L'animazione viene creata usando codice c# /VB C sul lato server ([fare clic per visualizzare l'immagine con dimensioni normali](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](triggering-an-animation-in-another-control-vb.md)
> [Successivo](executing-animations-using-client-side-code-vb.md)
