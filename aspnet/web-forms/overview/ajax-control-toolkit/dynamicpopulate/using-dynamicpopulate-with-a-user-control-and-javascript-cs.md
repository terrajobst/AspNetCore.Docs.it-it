---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Uso di DynamicPopulate con un controllo utente e JavaScript (c#) | Microsoft Docs
author: wenz
description: Il controllo di DynamicPopulate di ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione in t...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 3a380f24a521153cba084de21014380df49efabd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835172"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>Uso di DynamicPopulate con un controllo utente e JavaScript (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> Il controllo di DynamicPopulate di ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina. È anche possibile attivare il popolamento usando codice JavaScript lato client personalizzato. Tuttavia particolare attenzione deve essere eseguita quando il dispositivo extender si trova in un controllo utente.


## <a name="overview"></a>Panoramica

Il `DynamicPopulate` chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina di controllo in ASP.NET AJAX Control Toolkit. È anche possibile attivare il popolamento usando codice JavaScript lato client personalizzato. Tuttavia particolare attenzione deve essere eseguita quando il dispositivo extender si trova in un controllo utente.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessario un servizio Web ASP.NET che implementa il metodo che verrà chiamata dal `DynamicPopulateExtender` controllo. Il servizio web implementa il metodo `getDate()` che prevede un argomento di tipo string, denominato `contextKey`, poiché il `DynamicPopulate` controllo Invia un'informazione rapida con ogni chiamata al servizio web. Ecco il codice (file `DynamicPopulate.cs.asmx`) che recupera la data corrente in uno dei tre formati:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

Nel passaggio successivo, creare un nuovo controllo utente (`.ascx` file), indicati dalla dichiarazione seguente nella prima riga:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

Oggetto &lt; `label` &gt; elemento verrà utilizzato per visualizzare i dati provenienti dal server.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

Anche nel file di controllo utente, si userà tre pulsanti di opzione, ognuno dei quali rappresenta uno dei tre formati data massima supportati dal servizio web. Quando l'utente fa clic su uno dei pulsanti di opzione, il browser eseguirà il codice JavaScript che presenta un aspetto simile al seguente:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Questo codice consente di accedere il `DynamicPopulateExtender` (senza doversi preoccupare ID strano ancora, questo argomento verrà trattato in un secondo momento) e attiva il popolamento con dati dinamico. Nel contesto del pulsante di opzione corrente `this.value` fa riferimento al relativo valore, vale a dire `format1`, `format2` o `format3` esattamente ciò che prevede il metodo web.

L'unica cosa manca ancora nel controllo utente è il `DynamicPopulateExtender` controllare quali collegamenti i pulsanti di opzione per il servizio web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

Anche in questo caso si può notare l'ID strano usato nel controllo: `mcd1$myDate` invece di `myDate`. In precedenza, il codice JavaScript usato `mcd1_dpe1` per l'accesso di `DynamicPopulateExtender` invece di `dpe1`. Questa strategia di denominazione è un requisito speciale quando si usa `DynamicPopulateExtender` all'interno di un controllo utente. Inoltre, è necessario incorporare il controllo utente in modo specifico per il funzionamento generale. Creare una nuova pagina ASP.NET e registrare un prefisso di tag per il controllo utente che è appena stata implementata:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

Quindi, includere ASP.NET AJAX `ScriptManager` controllo nella nuova pagina:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Infine, aggiungere il controllo utente alla pagina. È necessario impostare solo relativi `ID` attributo (e `runat="server"`, ovviamente), ma è necessario anche impostare un nome specifico: `mcd1` poiché si tratta del prefisso utilizzato all'interno del controllo utente per l'accesso usando JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

L'operazione è ora completata. La pagina si comporti come previsto: un utente fa clic su uno dei pulsanti di opzione, il controllo nel Toolkit chiama il servizio web e Visualizza la data corrente nel formato desiderato.


[![I pulsanti di opzione si trovano in un controllo utente](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

I pulsanti di opzione si trovano in un controllo utente ([fare clic per visualizzare l'immagine con dimensioni normali](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Successivo](dynamically-populating-a-control-vb.md)
