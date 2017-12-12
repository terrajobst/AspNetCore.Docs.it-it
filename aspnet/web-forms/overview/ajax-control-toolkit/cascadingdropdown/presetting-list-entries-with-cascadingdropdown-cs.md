---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Preimpostazione le voci dell'elenco con CascadingDropDown (c#) | Documenti Microsoft
author: wenz
description: Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: d86c5887a8b175a60c32a0970482266b7d690162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a>Voci dell'elenco preimpostazione con CascadingDropDown (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)

> Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList. Con un minimo di codice è possibile che un elemento di elenco è preselezionato una volta caricati in modo dinamico i dati.


## <a name="overview"></a>Panoramica

Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList. (Ad esempio, un unico elenco fornisce un elenco degli stati ed elenco successivo viene riempita con città importanti in tale stato.) Con un minimo di codice è possibile che un elemento di elenco è preselezionato una volta caricati in modo dinamico i dati.

## <a name="steps"></a>Passaggi

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

Quindi, è necessario un controllo di DropDownList:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

Per questo elenco, viene aggiunto un extender CascadingDropDown, fornendo informazioni sull'URL e metodo servizio web:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

Quindi il programma di estensione CascadingDropDown chiama in modo asincrono un servizio web con la firma del metodo seguente:

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

Il metodo restituisce una matrice di tipo valore CascadingDropDown. Il costruttore del tipo prevede innanzitutto la didascalia della voce di elenco e quindi il valore (HTML `value` attributo). Se il terzo argomento è impostato su true, l'elenco viene automaticamente selezionato nel browser.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

Il caricamento della pagina nel browser riempirà nell'elenco a discesa con tre fornitori, la seconda fase preselezionata.


[![L'elenco viene compilato e preselezionato automaticamente](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

L'elenco viene compilato e preselezionato automaticamente ([fare clic per visualizzare l'immagine ingrandita](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))

>[!div class="step-by-step"]
[Precedente](using-cascadingdropdown-with-a-database-cs.md)
[Successivo](using-auto-postback-with-cascadingdropdown-cs.md)
