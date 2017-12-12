---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Utilizzo di Postback automatico con CascadingDropDown (VB) | Documenti Microsoft
author: wenz
description: Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f8059f44b4efbf59ebe7b3d2fd5400e886642a90
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a>Utilizzo di Postback automatico con CascadingDropDown (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)

> Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList. Tuttavia quando si utilizza il controllo CascadingDropDown, ASP. Funzionalità di un postback automatico del controllo DropDownList della rete non funziona, poiché in modo asincrono il caricamento dei dati nell'elenco genera un postback (non necessarie). Con un codice JavaScript, è possibile evitare questo effetto.


## <a name="overview"></a>Panoramica

Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList. (Ad esempio, un unico elenco fornisce un elenco degli stati ed elenco successivo viene riempita con città importanti in tale stato.) Tuttavia quando si utilizza il controllo CascadingDropDown, ASP. Funzionalità di un postback automatico del controllo DropDownList della rete non funziona, poiché in modo asincrono il caricamento dei dati nell'elenco genera un postback (non necessarie). Con un codice JavaScript, è possibile evitare questo effetto.

## <a name="steps"></a>Passaggi

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il &lt; `form` &gt; elemento):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

Quindi, è necessario un controllo di DropDownList:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

Per questo elenco, viene aggiunto un extender CascadingDropDown, fornendo informazioni sull'URL e metodo servizio web:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

Quindi il programma di estensione CascadingDropDown chiama in modo asincrono un servizio web con la firma del metodo seguente:

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

Il metodo restituisce una matrice di tipo valore CascadingDropDown. Il costruttore del tipo prevede innanzitutto la didascalia della voce di elenco e quindi il valore (HTML `value` attributo).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

Il caricamento della pagina nel browser riempirà nell'elenco a discesa con tre fornitori, la seconda fase preselezionata. Inoltre, ASP.NET definisce il `__doPostBack()` metodo JavaScript. Dopo la pagina è stata caricata, la chiamata di JavaScript viene aggiunto all'elenco a discesa, ma solo se sono presenti elementi in essa contenuti. Se non sono presenti elementi nell'elenco, il Toolkit di controllo sta caricando, in modo che il codice JavaScript viene utilizzato un timeout e tenta nuovamente di mezzo secondo.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

In questo modo, un postback viene eseguito solo quando vi sono effettivamente gli elementi nell'elenco e l'utente seleziona una voce.


[![La selezione di un elemento di elenco provoca un postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)

La selezione di un elemento di elenco provoca un postback ([fare clic per visualizzare l'immagine ingrandita](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))

>[!div class="step-by-step"]
[Precedente](presetting-list-entries-with-cascadingdropdown-vb.md)
