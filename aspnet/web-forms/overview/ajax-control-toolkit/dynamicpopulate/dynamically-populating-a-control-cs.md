---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Popolamento dinamico di un controllo (c#) | Microsoft Docs
author: wenz
description: Il controllo di DynamicPopulate di ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione in t...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: a78e2800b119db61965f9922ba99a2f90e6be948
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827327"
---
<a name="dynamically-populating-a-control-c"></a>Popolamento dinamico di un controllo (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)

> Il controllo di DynamicPopulate di ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina.


## <a name="overview"></a>Panoramica

Il `DynamicPopulate` chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina di controllo in ASP.NET AJAX Control Toolkit. Questa esercitazione viene illustrato come impostare questa funzionalità.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessario un servizio Web ASP.NET che implementa il metodo che verrà chiamata da `DynamicPopulate`. Classe del servizio web richiede la `ScriptService` attributo di cui è definito all'interno `Microsoft.Web.Script.Services`; in caso contrario, ASP.NET AJAX non è possibile creare il proxy di JavaScript lato client per il servizio web che a sua volta è richiesta da `DynamicPopulate`.

Il metodo web deve prevedere un unico argomento di tipo string, denominato `contextKey`, poiché il `DynamicPopulate` controllo Invia un'informazione rapida con ogni chiamata al servizio web. Il seguente servizio web restituisce la data corrente in un formato rappresentato dal `contextKey` argomento:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

Il servizio web viene quindi salvato come `DynamicPopulate.cs.asmx`. In alternativa, è possibile implementare il `getDate()` metodo come metodo di pagina all'interno della pagina ASP.NET effettivo con il `DynamicPopulate` controllo.

Nel passaggio successivo, creare un nuovo file ASP.NET. Come sempre, il primo passaggio per includere il `ScriptManager` nella tabella corrente per caricare la libreria ASP.NET AJAX e rendere il lavoro Control Toolkit:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

Quindi, aggiungere un controllo etichetta (ad esempio usando il controllo HTML lo stesso nome, o la &lt; `asp:Label`  / &gt; controllo web) che in un secondo momento verrà visualizzato il risultato della chiamata al servizio web.

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

Verrà usato un pulsante HTML (come un controllo HTML, perché non sono necessari un postback al server) per attivare la compilazione dinamica:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

Infine, è necessario il `DynamicPopulateExtender` controllo per le operazioni wire. Verranno impostati gli attributi seguenti (oltre a quelli ovvie, `ID` e `runat` = `"server"`):

- `TargetControlID` posizione in cui inserire il risultato dalla chiamata al servizio web
- `ServicePath` percorso del servizio web (omettere se si desidera utilizzare un metodo di pagina)
- `ServiceMethod` nome del metodo web o del metodo di pagina
- `ContextKey` informazioni di contesto da inviare al servizio web
- `PopulateTriggerControlID` elemento che attiva la chiamata al servizio web
- `ClearContentsDuringUpdate` Se su un valore vuoto l'elemento di destinazione durante la chiamata al servizio web

Come si può notare, il controllo richiede alcune informazioni ma l'inserimento di tutti gli elementi nella posizione corretta è piuttosto semplice. Ecco il markup per il `DynamicPopulateExtender` controllo nello scenario corrente:

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

Eseguire la pagina ASP.NET nel browser e fare clic sul pulsante. si riceverà la data corrente nel formato mese-giorno-anno.


[![Un clic sul pulsante Recupera la data dal server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)

Un clic sul pulsante Recupera la data dal server ([fare clic per visualizzare l'immagine con dimensioni normali](dynamically-populating-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](dynamically-populating-a-control-using-javascript-code-cs.md)
