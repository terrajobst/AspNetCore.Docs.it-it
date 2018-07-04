---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Test della complessità di una Password (VB) | Microsoft Docs
author: wenz
description: Le password sono necessarie, praticamente ovunque, in modo che gli utenti lazy tendono a sceglie le password semplici, facili da interrompere. Il controllo PasswordStrength nella pagina ASP. N....
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 3faf9996c73fb5aaa427b515d396f36663cf1801
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379702"
---
<a name="testing-the-strength-of-a-password-vb"></a>Test della complessità di una Password (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)

> Le password sono necessarie, praticamente ovunque, in modo che gli utenti lazy tendono a sceglie le password semplici, facili da interrompere. Il controllo PasswordStrength di ASP.NET AJAX Control Toolkit può controllare è davvero una password.


## <a name="overview"></a>Panoramica

Le password sono necessarie, praticamente ovunque, in modo che gli utenti lazy tendono a sceglie le password semplici, facili da interrompere. Il `PasswordStrength` controllo in ASP.NET AJAX Control Toolkit possibile verificare la qualità una password.

## <a name="steps"></a>Passaggi

Il `PasswordStrength` controllo si estende una casella di testo e verifica se la password è sufficiente. Offre un'ampia gamma di opzioni tramite gli attributi. Ecco solo alcuni di essi:

- `MinimumNumericCharacters` numero minimo di caratteri numerici richiesti nella password
- `MinimumSymbolCharacters` numero minimo di simboli (non lettere e cifre) necessario includere nella password
- `PreferredPasswordLength` lunghezza minima della password
- `RequiresUpperAndLowerCaseCharacters` Se la password deve usare caratteri maiuscoli e minuscoli

Il `StrengthIndicatorType` vengono fornite le informazioni come presentare la complessità della password, come testo (valore `"Text"`) o come un tipo di indicatore di stato (valore `"BarIndicator"`). Nel `DisplayPosition` attributo, si configura in cui vengono visualizzate le informazioni. Ecco un esempio completo, tra cui ASP.NET AJAX `ScriptManager` (controllo), il `PasswordStrength` controllo e ovviamente una casella di testo in cui l'utente può immettere una password. Per fini dimostrativi, il campo del form quest'ultimo è un campo di testo normale e non un campo della password in modo che durante lo sviluppo è possibile visualizzare ciò che si sta digitando.

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

Eseguire la pagina e digitare da subito: solo dopo aver immesso le lettere minuscole, lettere maiuscole, cifre e simboli, la password viene considerata come non interrompibile.


[![A questo punto la password non è valida (ancora)](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)

A questo punto la password non è valida (ancora) ([fare clic per visualizzare l'immagine con dimensioni normali](testing-the-strength-of-a-password-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](testing-the-strength-of-a-password-cs.md)
