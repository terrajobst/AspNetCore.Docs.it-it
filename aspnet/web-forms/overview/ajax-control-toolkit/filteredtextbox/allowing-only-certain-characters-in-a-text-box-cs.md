---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Consentire solo il numero di caratteri specifici in una casella di testo (c#) | Documenti Microsoft
author: wenz
description: "I controlli di convalida di ASP.NET è possono garantire che solo alcuni caratteri sono consentiti nell'input dell'utente. Questo ancora non impedisce tuttavia agli utenti di digitare non validi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: 246c3b5dd55ceb0f47ad1f4982ae5b3bf855e747
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a>Consentire solo il numero di caratteri specifici in una casella di testo (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)

> I controlli di convalida di ASP.NET è possono garantire che solo alcuni caratteri sono consentiti nell'input dell'utente. Questo tuttavia non impedisce tuttavia agli utenti di digitare i caratteri non validi e durante il tentativo di inviare il modulo.


## <a name="overview"></a>Panoramica

I controlli di convalida di ASP.NET è possono garantire che solo alcuni caratteri sono consentiti nell'input dell'utente. Questo tuttavia non impedisce tuttavia agli utenti di digitare i caratteri non validi e durante il tentativo di inviare il modulo.

## <a name="steps"></a>Passaggi

ASP.NET AJAX Control Toolkit contiene il `FilteredTextBox` controllo che estende una casella di testo. Una volta attivato, è possibile immettere solo un determinato set di caratteri nel campo.

Per il funzionamento, è necessario innanzitutto come di consueto ASP.NET AJAX `ScriptManager` che carica le librerie JavaScript che sono utilizzate anche da ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

Quindi, abbiamo bisogno di una casella di testo:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

Infine, il `FilteredTextBoxExtender` controllo si occupa di limitare i caratteri che l'utente è autorizzato a digitare. Innanzitutto, impostare il `TargetControlID` attributo la `ID` del `TextBox` controllo. Quindi, scegliere una delle `FilterType` valori:

- `Custom`impostazione predefinita. è necessario fornire un elenco di caratteri valide
- `LowercaseLetters`solo lettere minuscole
- `Numbers`solo cifre
- `UppercaseLetters`solo le lettere maiuscole

Se il `Custom FilterType` viene utilizzato il `ValidChars` proprietà deve essere impostata e fornire un elenco di caratteri che possono essere digitati. Dalla modalità: se si tenta di incollare il testo nella casella di testo, tutti i caratteri non validi vengono rimossi.

Ecco il markup per il `FilteredTextBoxExtender` controllo che consente solo cifre (qualcosa che inoltre sono stati possibili con `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

Eseguire la pagina e provare a immettere una lettera se JavaScript è abilitato, non funzionerà; cifre vengono tuttavia visualizzate nella pagina. Tuttavia, si noti che la protezione `FilteredTextBox` fornisce non valide: se JavaScript è abilitato, i dati possono essere immessi nella casella di testo, è necessario utilizzare metodi di convalida aggiuntivi, ad esempio ASP. Controlli di convalida della rete.


[![È possibile immettere solo cifre.](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)

È possibile immettere solo cifre ([fare clic per visualizzare l'immagine ingrandita](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))

>[!div class="step-by-step"]
[Successivo](allowing-only-certain-characters-in-a-text-box-vb.md)
