---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Preimpostazione le voci dell'elenco con CascadingDropDown (VB) | Documenti Microsoft
author: wenz
description: Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f74e6ac80b756240870d9406a03db11c610093aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869894"
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a>Voci dell'elenco preimpostazione con CascadingDropDown (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList. Con un minimo di codice è possibile che un elemento di elenco è preselezionato una volta caricati in modo dinamico i dati.


## <a name="overview"></a>Panoramica

Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList. (Ad esempio, un unico elenco fornisce un elenco degli stati ed elenco successivo viene riempita con città importanti in tale stato.) Con un minimo di codice è possibile che un elemento di elenco è preselezionato una volta caricati in modo dinamico i dati.

## <a name="steps"></a>Passaggi

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

Quindi, è necessario un controllo di DropDownList:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

Per questo elenco, viene aggiunto un extender CascadingDropDown, fornendo informazioni sull'URL e metodo servizio web:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

Quindi il programma di estensione CascadingDropDown chiama in modo asincrono un servizio web con la firma del metodo seguente:

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

Il metodo restituisce una matrice di tipo valore CascadingDropDown. Il costruttore del tipo prevede innanzitutto la didascalia della voce di elenco e quindi il valore (HTML `value` attributo). Se il terzo argomento è impostato su true, l'elenco viene automaticamente selezionato nel browser.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

Il caricamento della pagina nel browser riempirà nell'elenco a discesa con tre fornitori, la seconda fase preselezionata.


[![L'elenco viene compilato e preselezionato automaticamente](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

L'elenco viene compilato e preselezionato automaticamente ([fare clic per visualizzare l'immagine ingrandita](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-cascadingdropdown-with-a-database-vb.md)
> [Successivo](using-auto-postback-with-cascadingdropdown-vb.md)
