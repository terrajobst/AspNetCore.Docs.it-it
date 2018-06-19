---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modifica le animazioni dal lato Server (VB) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Le animazioni potrebbero inoltre...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b9ce85fc5040b2318233b3c553c2cf53dd03555
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869270"
---
<a name="modifying-animations-from-the-server-side-vb"></a>Modifica le animazioni dal lato Server (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Le animazioni possono anche essere modificate sul lato server


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Le animazioni possono anche essere modificate sul lato server

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

Il resto del codice viene eseguito sul lato server e non viene utilizzato markup. al contrario, viene utilizzato codice per creare il `AnimationExtender` controllo:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Tuttavia, il Toolkit di controllo attualmente non fornisce un accesso all'API per creare le animazioni singoli. È tuttavia possibile impostare il `AnimationExtender`proprietà animazioni in una stringa contenente il markup XML utilizzato quando si assegnano le animazioni in modo dichiarativo. Per creare il codice XML che non deve contenere il `<Animations>` elemento è possibile usare il XML di .NET Framework supporta o, come illustrato nel codice seguente, è sufficiente fornire la stringa:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Aggiungere infine il `AnimationExtender` controllo alla pagina corrente, all'interno di `<form runat="server">` elemento, assicurandosi che l'animazione è incluso e viene eseguito:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


[![L'animazione viene creata usando codice c# /VB C lato server](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

L'animazione viene creato utilizzando il codice c# /VB sul lato server C ([fare clic per visualizzare l'immagine ingrandita](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](triggering-an-animation-in-another-control-vb.md)
> [Successivo](executing-animations-using-client-side-code-vb.md)
