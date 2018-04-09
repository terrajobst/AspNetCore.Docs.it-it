---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Utilizzo di DynamicPopulate con un controllo utente e JavaScript (VB) | Documenti Microsoft
author: wenz
description: Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 715973ef4923e635ec2a860d00d55f13a5f8b2d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a>Utilizzo di DynamicPopulate con un controllo utente e JavaScript (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)

> Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e riempie il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina. È inoltre possibile attivare la popolazione utilizzando codice JavaScript sul lato client personalizzato. Tuttavia particolare attenzione deve essere eseguita quando il programma di estensione si trova in un controllo utente.


## <a name="overview"></a>Panoramica

Il `DynamicPopulate` controllo ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e riempie il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina. È inoltre possibile attivare la popolazione utilizzando codice JavaScript sul lato client personalizzato. Tuttavia particolare attenzione deve essere eseguita quando il programma di estensione si trova in un controllo utente.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessario un servizio Web ASP.NET che implementa il metodo da chiamare `DynamicPopulateExtender` controllo. Il servizio web implementa il metodo `getDate()` che prevede un argomento di tipo string, denominato `contextKey`, poiché il `DynamicPopulate` controllo invia una parte delle informazioni di contesto con ogni chiamata al servizio web. Ecco il codice (file `DynamicPopulate.vb.asmx`) che consente di recuperare la data corrente in uno dei tre formati:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

Nel passaggio successivo, creare un nuovo controllo utente (`.ascx` file), indicati dalla dichiarazione seguente nella prima riga:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

Oggetto &lt; `label` &gt; elemento verrà utilizzato per visualizzare i dati provenienti dal server.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

Anche nel file di controllo utente, si utilizzerà tre pulsanti di opzione, ogni uno che rappresenta uno dei tre formati di data supportati dal servizio web. Quando l'utente fa clic su uno dei pulsanti di opzione, il browser verrà eseguito il codice JavaScript che è simile al seguente:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

Questo codice consente di accedere il `DynamicPopulateExtender` (non è necessario l'ID strano ancora, questo argomento verrà descritto più avanti) e attiva la popolazione con dati dinamica. Nel contesto del pulsante di opzione corrente `this.value` fa riferimento al valore, ovvero `format1`, `format2` o `format3` esattamente ciò che prevede il metodo web.

È l'unico elemento mancante nel controllo utente ancora il `DynamicPopulateExtender` controllo che collega i pulsanti di opzione per il servizio web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

Nuovo si può notare l'ID strano utilizzata per il controllo: `mcd1$myDate` anziché `myDate`. In precedenza, il codice JavaScript utilizzato `mcd1_dpe1` per l'accesso di `DynamicPopulateExtender` anziché `dpe1`. Questa strategia di denominazione è necessario quando si utilizza `DynamicPopulateExtender` all'interno di un controllo utente. Inoltre, è necessario incorporare il controllo utente in modo specifico per il funzionamento. Creare una nuova pagina ASP.NET e registrare un prefisso di tag per il controllo utente che appena implementato:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

Quindi, includere ASP.NET AJAX `ScriptManager` controllo nella nuova pagina:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

Infine, aggiungere il controllo utente alla pagina. È necessario impostare il relativo `ID` attributo (e `runat="server"`, naturalmente), ma è anche necessario impostare un nome specifico: `mcd1` poiché si tratta del prefisso utilizzato all'interno del controllo utente per l'accesso utilizzando JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

L'operazione è ora completata. La pagina si comporti come previsto: un utente fa clic su uno dei pulsanti di opzione, il controllo in Toolkit chiama il servizio web e Visualizza la data corrente nel formato desiderato.


[![I pulsanti di opzione si trovano in un controllo utente](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)

I pulsanti di opzione si trovano in un controllo utente ([fare clic per visualizzare l'immagine ingrandita](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](dynamically-populating-a-control-using-javascript-code-vb.md)
