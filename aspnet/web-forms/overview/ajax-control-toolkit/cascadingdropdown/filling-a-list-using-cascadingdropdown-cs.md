---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Compilazione di un elenco tramite CascadingDropDown (c#) | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichi associati i valori in anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 795b2a2ee80d3d2bed6f752887d5f9d974151a56
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827194"
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>Compilazione di un elenco tramite CascadingDropDown (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList Carica valori in un altro controllo DropDownList associati. (Ad esempio, un unico elenco fornisce un elenco di stati degli Stati Uniti e il successivo elenco viene riempito con città più importanti di tale stato.) Il primo problema da risolvere è riempire effettivamente un elenco a discesa tramite questo controllo.


## <a name="overview"></a>Panoramica

Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList Carica valori in un altro controllo DropDownList associati. (Ad esempio, un unico elenco fornisce un elenco di stati degli Stati Uniti e il successivo elenco viene riempito con città più importanti di tale stato.) Il primo problema da risolvere è riempire effettivamente un elenco a discesa tramite questo controllo.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Quindi, è necessario un controllo DropDownList:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Per questo elenco, viene aggiunto un controllo extender CascadingDropDown. Invia una richiesta asincrona a un servizio web che quindi restituirà un elenco di voci da visualizzare nell'elenco. Per il corretto funzionamento, è necessario impostare gli attributi di CascadingDropDown seguenti:

- `ServicePath`: URL di un servizio web distribuisce le voci dell'elenco
- `ServiceMethod`: Metodo web fornire le voci dell'elenco
- `TargetControlID`: ID dell'elenco a discesa
- `Category`: Informazioni categoria in cui viene inviate al metodo web quando viene chiamato
- `PromptText`: Testo visualizzato quando si caricano in modo asincrono i dati dell'elenco dal server

Ecco il markup per il `CascadingDropDown` elemento. L'unica differenza tra c# e Visual Basic è il nome del servizio web associato:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Il codice JavaScript provenienti dal `CascadingDropDown` extender chiama un metodo del servizio web con la firma seguente:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

In modo che l'aspetto importante è che il metodo deve restituire una matrice di tipo `CascadingDropDownNameValue` (definito da ASP.NET AJAX Control Toolkit). Nel `CascadingDropDownNameValue` costruttore, prima è necessario specificare il testo della voce di elenco e quindi il relativo valore, proprio come `<option value="VALUE">NAME</option>` verrebbero impiegati in HTML. Ecco alcuni dati di esempio:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Il caricamento della pagina nel browser verrà attivata l'elenco deve essere compilato con tre fornitori.


[![L'elenco viene compilato automaticamente](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

L'elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine con dimensioni normali](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](using-cascadingdropdown-with-a-database-cs.md)
