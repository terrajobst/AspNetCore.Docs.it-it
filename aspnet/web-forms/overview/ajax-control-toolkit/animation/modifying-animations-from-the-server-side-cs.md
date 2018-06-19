---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Modifica le animazioni dal lato Server (c#) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Le animazioni potrebbero inoltre...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 6946875552c885ffb1f2a2eb7e728b85d7dd3973
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869010"
---
<a name="modifying-animations-from-the-server-side-c"></a>Modifica le animazioni dal lato Server (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Le animazioni possono anche essere modificate sul lato server


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Le animazioni possono anche essere modificate sul lato server

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

Il resto del codice viene eseguito sul lato server e non viene utilizzato markup. al contrario, viene utilizzato codice per creare il `AnimationExtender` controllo:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

Tuttavia, il Toolkit di controllo attualmente non fornisce un accesso all'API per creare le animazioni singoli. È tuttavia possibile impostare il `AnimationExtender`proprietà animazioni in una stringa contenente il markup XML utilizzato quando si assegnano le animazioni in modo dichiarativo. Per creare il codice XML che non deve contenere il `<Animations>` elemento è possibile usare il XML di .NET Framework supporta o, come illustrato nel codice seguente, è sufficiente fornire la stringa:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Aggiungere infine il `AnimationExtender` controllo alla pagina corrente, all'interno di `<form runat="server">` elemento, assicurandosi che l'animazione è incluso e viene eseguito:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![L'animazione viene creata usando codice c# /VB C lato server](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

L'animazione viene creato utilizzando il codice c# /VB sul lato server C ([fare clic per visualizzare l'immagine ingrandita](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](triggering-an-animation-in-another-control-cs.md)
> [Successivo](executing-animations-using-client-side-code-cs.md)
