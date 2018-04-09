---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Compilazione dinamica di un controllo (c#) | Documenti Microsoft
author: wenz
description: Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 113b8c043c14e4ebc476b021884dd1430757452a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-populating-a-control-c"></a>Compilazione dinamica di un controllo (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)

> Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e riempie il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina.


## <a name="overview"></a>Panoramica

Il `DynamicPopulate` controllo ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e riempie il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina. In questa esercitazione viene illustrato come specificare questa impostazione.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessario un servizio Web ASP.NET che implementa il metodo da chiamare `DynamicPopulate`. La classe del servizio web richiede il `ScriptService` attributo definito all'interno di `Microsoft.Web.Script.Services`; in caso contrario AJAX ASP.NET non è possibile creare il proxy di JavaScript sul lato client per il servizio web che a sua volta è necessario per `DynamicPopulate`.

Il metodo web deve prevedere un unico argomento di tipo string, denominato `contextKey`, poiché il `DynamicPopulate` controllo invia una parte delle informazioni di contesto con ogni chiamata al servizio web. Il servizio web seguente restituisce la data corrente in un formato rappresentato dal `contextKey` argomento:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

Il servizio web viene quindi salvato come `DynamicPopulate.cs.asmx`. In alternativa, è possibile implementare il `getDate()` metodo come metodo di pagina all'interno della pagina ASP.NET effettivo con il `DynamicPopulate` controllo.

Nel passaggio successivo, creare un nuovo file ASP.NET. Come sempre, il primo passaggio per includere il `ScriptManager` nella pagina corrente per caricare la libreria ASP.NET AJAX e il lavoro Control Toolkit:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

Quindi, aggiungere un controllo etichetta (ad esempio mediante il controllo HTML con lo stesso nome, o &lt; `asp:Label`  / &gt; controllo web) che in un secondo momento visualizzerà il risultato della chiamata al servizio web.

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

Verrà quindi usato per attivare la compilazione dinamica di un pulsante HTML (come un controllo HTML, poiché non è necessario un postback al server):

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

Infine, è necessario il `DynamicPopulateExtender` controllo per le operazioni transito. I seguenti attributi verranno impostati (oltre a quelli ovvi, `ID` e `runat` = `"server"`):

- `TargetControlID` posizione in cui inserire il risultato dalla chiamata al servizio web
- `ServicePath` percorso del servizio web (omettere se si desidera utilizzare un metodo di pagina)
- `ServiceMethod` nome del metodo web o del metodo di pagina
- `ContextKey` informazioni sul contesto da inviare al servizio web
- `PopulateTriggerControlID` elemento che attiva la chiamata del servizio web
- `ClearContentsDuringUpdate` Se su un valore vuoto l'elemento di destinazione durante la chiamata al servizio web

Come si può notare, il controllo richiede alcune informazioni, ma tutti gli elementi sul posto è piuttosto semplice. Ecco il markup per il `DynamicPopulateExtender` controllo nello scenario corrente:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

Eseguire la pagina ASP.NET nel browser e fare clic sul pulsante. si riceverà la data corrente nel formato mese-giorno-anno.


[![Un clic sul pulsante Recupera la data dal server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

Un clic sul pulsante Recupera la data dal server ([fare clic per visualizzare l'immagine ingrandita](dynamically-populating-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](dynamically-populating-a-control-using-javascript-code-cs.md)
