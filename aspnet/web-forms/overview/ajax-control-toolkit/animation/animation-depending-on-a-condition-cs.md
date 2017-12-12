---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animazione in base a una condizione (c#) | Documenti Microsoft
author: wenz
description: "Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Se è un'animazione..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 13366a86be01f41e27db1869b93192520190387a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="animation-depending-on-a-condition-c"></a>Animazione in base a una condizione (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Se un'animazione viene eseguita o non può dipendere anche una condizione in forma di codice JavaScript.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Se un'animazione viene eseguita o non può dipendere anche una condizione in forma di codice JavaScript.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria`runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

All'interno di `<Animations>` nodo, utilizzare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente. Invece di una delle animazioni, regolare il `<Condition>` elemento entra in gioco. Il codice JavaScript fornito come valore della `ConditionScript` attributo viene eseguito in fase di esecuzione. Se restituisce true, l'animazione viene eseguita, in caso contrario non. Il markup seguente fornisce due animazioni, ognuno di essi in esecuzione al 50% dei case al momento casuale. Poiché potrebbe essere presente solo un'animazione all'interno di `<OnLoad>`, i due `<Condition>` animazioni vengono unite tramite un insieme di `<Sequence>` elemento:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Si noti che il segno di minore di (`<`) nei `ConditionScript` attributo deve essere sottoposto a escape (). Quando si esegue questo script viene eseguito alcuna animazione, o uno dei due non oppure eseguire entrambe.


[![Il pannello è dissolvenza in uscita senza alcun ridimensionamento, pertanto l'esecuzione del secondo animazione, il primo non](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

Il pannello è dissolvenza in uscita senza alcun ridimensionamento, in modo non ha l'esecuzione del secondo animazione, il primo ([fare clic per visualizzare l'immagine ingrandita](animation-depending-on-a-condition-cs/_static/image3.png))

>[!div class="step-by-step"]
[Precedente](executing-several-animations-after-each-other-cs.md)
[Successivo](picking-one-animation-out-of-a-list-cs.md)
