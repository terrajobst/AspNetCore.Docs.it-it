---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: "Test di complessità di una Password (c#) | Documenti Microsoft"
author: wenz
description: Le password sono necessarie ovunque, in modo che gli utenti lazy tendono a scegliere password semplici, facili da interrompere. Il controllo PasswordStrength nella pagina ASP. N....
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: eda7baae1833b074ba34d8f10fa434df14cc592e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="testing-the-strength-of-a-password-c"></a><span data-ttu-id="e7f85-104">Test di complessità di una Password (c#)</span><span class="sxs-lookup"><span data-stu-id="e7f85-104">Testing the Strength of a Password (C#)</span></span>
====================
<span data-ttu-id="e7f85-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e7f85-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e7f85-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e7f85-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span></span>

> <span data-ttu-id="e7f85-107">Le password sono necessarie ovunque, in modo che gli utenti lazy tendono a scegliere password semplici, facili da interrompere.</span><span class="sxs-lookup"><span data-stu-id="e7f85-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="e7f85-108">Il controllo PasswordStrength in ASP.NET AJAX Control Toolkit possibile verificare la qualità una password.</span><span class="sxs-lookup"><span data-stu-id="e7f85-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="e7f85-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e7f85-109">Overview</span></span>

<span data-ttu-id="e7f85-110">Le password sono necessarie ovunque, in modo che gli utenti lazy tendono a scegliere password semplici, facili da interrompere.</span><span class="sxs-lookup"><span data-stu-id="e7f85-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="e7f85-111">Il `PasswordStrength` controllo ASP.NET AJAX Control Toolkit è possibile verificare la qualità è di una password.</span><span class="sxs-lookup"><span data-stu-id="e7f85-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="e7f85-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="e7f85-112">Steps</span></span>

<span data-ttu-id="e7f85-113">Il `PasswordStrength` controllo estende una casella di testo e controlla se la password è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="e7f85-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="e7f85-114">Offre una vasta gamma di opzioni tramite gli attributi. Ecco solo alcuni di essi:</span><span class="sxs-lookup"><span data-stu-id="e7f85-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="e7f85-115">`MinimumNumericCharacters`numero minimo di caratteri numerici richiesti nella password</span><span class="sxs-lookup"><span data-stu-id="e7f85-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="e7f85-116">`MinimumSymbolCharacters`numero minimo di simboli (non lettere e cifre) richiesto nella password</span><span class="sxs-lookup"><span data-stu-id="e7f85-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="e7f85-117">`PreferredPasswordLength`lunghezza minima della password</span><span class="sxs-lookup"><span data-stu-id="e7f85-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="e7f85-118">`RequiresUpperAndLowerCaseCharacters`Se la password deve usare caratteri maiuscoli e minuscoli</span><span class="sxs-lookup"><span data-stu-id="e7f85-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="e7f85-119">Il `StrengthIndicatorType` fornisce le informazioni come presentare la complessità della password, come testo (valore `"Text"`) o come un tipo di indicatore di stato (valore `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="e7f85-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="e7f85-120">Nel `DisplayPosition` attributo, configurare dove vengono visualizzate le informazioni.</span><span class="sxs-lookup"><span data-stu-id="e7f85-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="e7f85-121">Ecco un esempio completo, incluso ASP.NET AJAX `ScriptManager` (controllo), il `PasswordStrength` controllo e, naturalmente, una casella di testo in cui l'utente può immettere una password.</span><span class="sxs-lookup"><span data-stu-id="e7f85-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="e7f85-122">Ai fini di dimostrazione, il campo modulo quest'ultimo è un campo di testo normale e non un campo della password in modo da visualizzare durante lo sviluppo che si sta digitando.</span><span class="sxs-lookup"><span data-stu-id="e7f85-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

<span data-ttu-id="e7f85-123">Eseguire la pagina e digitare lontano: solo dopo avere immesso le lettere minuscole, lettere maiuscole, cifre e simboli, la password viene considerata come non interrompibile.</span><span class="sxs-lookup"><span data-stu-id="e7f85-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="e7f85-124">[![La password non è valida (piuttosto)](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e7f85-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span></span>

<span data-ttu-id="e7f85-125">A questo punto la password () buone ([fare clic per visualizzare l'immagine ingrandita](testing-the-strength-of-a-password-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e7f85-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e7f85-126">Successivo</span><span class="sxs-lookup"><span data-stu-id="e7f85-126">Next</span></span>](testing-the-strength-of-a-password-vb.md)
