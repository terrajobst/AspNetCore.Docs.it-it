---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Consentire solo determinati caratteri in una casella di testo (c#) | Microsoft Docs
author: wenz
description: I controlli di convalida di ASP.NET è possono garantire che solo alcuni caratteri sono consentiti nell'input dell'utente. Questo comunque non impedisce tuttavia agli utenti dalla digitazione non è validi...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: d8a1e792c9cd854591fc434f28afe98e4d91dfbe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837389"
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a>Consentire solo determinati caratteri in una casella di testo (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)

> I controlli di convalida di ASP.NET è possono garantire che solo alcuni caratteri sono consentiti nell'input dell'utente. Tuttavia questo comunque non impedisce agli utenti di digitare i caratteri non validi e provando a inviare il modulo.


## <a name="overview"></a>Panoramica

I controlli di convalida di ASP.NET è possono garantire che solo alcuni caratteri sono consentiti nell'input dell'utente. Tuttavia questo comunque non impedisce agli utenti di digitare i caratteri non validi e provando a inviare il modulo.

## <a name="steps"></a>Passaggi

ASP.NET AJAX Control Toolkit contiene il `FilteredTextBox` controllo che estende una casella di testo. Una volta attivato, solo un determinato set di caratteri possono essere immessi nel campo.

A questo scopo, è innanzitutto necessario come di consueto ASP.NET AJAX `ScriptManager` che carica le librerie JavaScript che vengono anche usate da ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

Quindi, abbiamo bisogno di una casella di testo:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

Infine, il `FilteredTextBoxExtender` controllo si occupa di limitare i caratteri che l'utente è autorizzato nel tipo. Innanzitutto, impostare il `TargetControlID` attributo la `ID` del `TextBox` controllo. Quindi, scegliere uno dei contatori `FilterType` valori:

- `Custom` impostazione predefinita. è necessario specificare un elenco di caratteri validi
- `LowercaseLetters` solo lettere minuscole
- `Numbers` solo cifre
- `UppercaseLetters` solo lettere maiuscole

Se il `Custom FilterType` viene usato il `ValidChars` proprietà deve essere configurata e fornire un elenco di caratteri che possono essere digitati. A proposito: se si tenta di incollare il testo nella casella di testo, tutti i caratteri non validi vengono rimossi.

Ecco il markup per il `FilteredTextBoxExtender` controllo che consente solo cifre (qualcosa che anche risulterebbe possibili con `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

Eseguire la pagina e riprovare l'immissione di una lettera di JavaScript è abilitato, non funzionerà; Nella pagina vengono tuttavia visualizzate cifre. Si noti tuttavia che la protezione `FilteredTextBox` fornisce non valide: JavaScript se è abilitata, tutti i dati possono essere immessi nella casella di testo, quindi è necessario usare una convalida aggiuntiva significa che, ad esempio ASP. Controlli di convalida della rete.


[![È possibile immettere solo cifre](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)

È possibile immettere solo cifre ([fare clic per visualizzare l'immagine con dimensioni normali](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](allowing-only-certain-characters-in-a-text-box-vb.md)
