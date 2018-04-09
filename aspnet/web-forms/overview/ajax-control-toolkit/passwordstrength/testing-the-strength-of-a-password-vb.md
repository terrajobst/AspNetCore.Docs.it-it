---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Test di complessità di una Password (VB) | Documenti Microsoft
author: wenz
description: Le password sono necessarie ovunque, in modo che gli utenti lazy tendono a scegliere password semplici, facili da interrompere. Il controllo PasswordStrength nella pagina ASP. N....
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d46026535f3f5cf82944359599464e8a4725280
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="testing-the-strength-of-a-password-vb"></a>Test di complessità di una Password (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)

> Le password sono necessarie ovunque, in modo che gli utenti lazy tendono a scegliere password semplici, facili da interrompere. Il controllo PasswordStrength in ASP.NET AJAX Control Toolkit possibile verificare la qualità una password.


## <a name="overview"></a>Panoramica

Le password sono necessarie ovunque, in modo che gli utenti lazy tendono a scegliere password semplici, facili da interrompere. Il `PasswordStrength` controllo ASP.NET AJAX Control Toolkit è possibile verificare la qualità è di una password.

## <a name="steps"></a>Passaggi

Il `PasswordStrength` controllo estende una casella di testo e controlla se la password è sufficiente. Offre una vasta gamma di opzioni tramite gli attributi. Ecco solo alcuni di essi:

- `MinimumNumericCharacters` numero minimo di caratteri numerici richiesti nella password
- `MinimumSymbolCharacters` numero minimo di simboli (non lettere e cifre) richiesto nella password
- `PreferredPasswordLength` lunghezza minima della password
- `RequiresUpperAndLowerCaseCharacters` indica se la password deve usare caratteri maiuscoli e minuscoli

Il `StrengthIndicatorType` fornisce le informazioni come presentare la complessità della password, come testo (valore `"Text"`) o come un tipo di indicatore di stato (valore `"BarIndicator"`). Nel `DisplayPosition` attributo, configurare dove vengono visualizzate le informazioni. Ecco un esempio completo, incluso ASP.NET AJAX `ScriptManager` (controllo), il `PasswordStrength` controllo e, naturalmente, una casella di testo in cui l'utente può immettere una password. Ai fini di dimostrazione, il campo modulo quest'ultimo è un campo di testo normale e non un campo della password in modo da visualizzare durante lo sviluppo che si sta digitando.

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

Eseguire la pagina e digitare lontano: solo dopo avere immesso le lettere minuscole, lettere maiuscole, cifre e simboli, la password viene considerata come non interrompibile.


[![La password non è valida (piuttosto)](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)

A questo punto la password () buone ([fare clic per visualizzare l'immagine ingrandita](testing-the-strength-of-a-password-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](testing-the-strength-of-a-password-cs.md)
