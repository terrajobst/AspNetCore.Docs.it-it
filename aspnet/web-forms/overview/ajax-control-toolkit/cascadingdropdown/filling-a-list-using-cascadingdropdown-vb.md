---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: La compilazione di un elenco utilizzando CascadingDropDown (VB) | Documenti Microsoft
author: wenz
description: Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 1c5cb23be4366365c73ce4774e6a53e452e2a6c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a>La compilazione di un elenco utilizzando CascadingDropDown (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList. (Ad esempio, un unico elenco fornisce un elenco degli stati ed elenco successivo viene riempita con città importanti in tale stato.) Il primo problema da risolvere consiste nel compilare effettivamente un elenco a discesa tramite questo controllo.


## <a name="overview"></a>Panoramica

Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList. (Ad esempio, un unico elenco fornisce un elenco degli stati ed elenco successivo viene riempita con città importanti in tale stato.) Il primo problema da risolvere consiste nel compilare effettivamente un elenco a discesa tramite questo controllo.

## <a name="steps"></a>Passaggi

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

Quindi, è necessario un controllo di DropDownList:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

Per questo elenco, viene aggiunta un'estensione CascadingDropDown. Questo invierà una richiesta asincrona a un servizio web che verrà quindi restituito un elenco di voci da visualizzare nell'elenco. Per il funzionamento, è necessario impostare gli attributi CascadingDropDown seguenti:

- `ServicePath`: URL di un servizio web, offrendo le voci dell'elenco
- `ServiceMethod`: Metodo web le voci dell'elenco di distribuzione
- `TargetControlID`: ID dell'elenco a discesa
- `Category`: Le informazioni sulle categorie viene inviati al metodo web quando viene chiamato
- `PromptText`: Testo visualizzato quando si caricano in modo asincrono i dati di elenco dal server

Ecco il markup per il `CascadingDropDown` elemento. L'unica differenza tra i linguaggi c# e Visual Basic è il nome del servizio web associato:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

Il codice JavaScript proviene il `CascadingDropDown` extender chiama un metodo di servizio web con la firma seguente:

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

Pertanto l'aspetto importante è che il metodo deve restituire una matrice di tipo `CascadingDropDownNameValue` (definito da ASP.NET AJAX Control Toolkit). Nel `CascadingDropDownNameValue` costruttore, prima è necessario specificare il testo della voce di elenco e quindi il relativo valore, come `<option value="VALUE">NAME</option>` farebbe in formato HTML. Ecco alcuni dati di esempio:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Il caricamento della pagina nel browser verrà attivata l'elenco da popolare con tre fornitori.


[![L'elenco viene compilato automaticamente](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

L'elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine ingrandita](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

>[!div class="step-by-step"]
[Precedente](using-auto-postback-with-cascadingdropdown-cs.md)
[Successivo](using-cascadingdropdown-with-a-database-vb.md)
