---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: La compilazione di un elenco utilizzando CascadingDropDown (c#) | Documenti Microsoft
author: wenz
description: Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: c9e47f6484e49013004bf15084f98440ee67558e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>La compilazione di un elenco utilizzando CascadingDropDown (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList. (Ad esempio, un unico elenco fornisce un elenco degli stati ed elenco successivo viene riempita con città importanti in tale stato.) Il primo problema da risolvere consiste nel compilare effettivamente un elenco a discesa tramite questo controllo.


## <a name="overview"></a>Panoramica

Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList. (Ad esempio, un unico elenco fornisce un elenco degli stati ed elenco successivo viene riempita con città importanti in tale stato.) Il primo problema da risolvere consiste nel compilare effettivamente un elenco a discesa tramite questo controllo.

## <a name="steps"></a>Passaggi

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Quindi, è necessario un controllo di DropDownList:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Per questo elenco, viene aggiunta un'estensione CascadingDropDown. Questo invierà una richiesta asincrona a un servizio web che verrà quindi restituito un elenco di voci da visualizzare nell'elenco. Per il funzionamento, è necessario impostare gli attributi CascadingDropDown seguenti:

- `ServicePath`: URL di un servizio web recapitare le voci dell'elenco
- `ServiceMethod`: Metodo web recapito le voci dell'elenco
- `TargetControlID`: ID dell'elenco a discesa
- `Category`: Informazioni sulle categorie viene inviati al metodo web quando viene chiamato
- `PromptText`: Testo visualizzato quando si caricano in modo asincrono i dati elenco dal server

Ecco il markup per il `CascadingDropDown` elemento. L'unica differenza tra i linguaggi c# e Visual Basic è il nome del servizio web associato:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Il codice JavaScript proviene il `CascadingDropDown` extender chiama un metodo di servizio web con la firma seguente:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Pertanto l'aspetto importante è che il metodo deve restituire una matrice di tipo `CascadingDropDownNameValue` (definito da ASP.NET AJAX Control Toolkit). Nel `CascadingDropDownNameValue` costruttore, prima è necessario specificare il testo della voce di elenco e quindi il relativo valore, come `<option value="VALUE">NAME</option>` farebbe in formato HTML. Ecco alcuni dati di esempio:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Il caricamento della pagina nel browser verrà attivata l'elenco da popolare con tre fornitori.


[![L'elenco viene compilato automaticamente](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

L'elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine ingrandita](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](using-cascadingdropdown-with-a-database-cs.md)
