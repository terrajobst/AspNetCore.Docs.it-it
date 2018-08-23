---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Test della complessità di una Password (VB) | Microsoft Docs
author: wenz
description: Le password sono necessarie, praticamente ovunque, in modo che gli utenti lazy tendono a sceglie le password semplici, facili da interrompere. Il controllo PasswordStrength nella pagina ASP. N....
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: db4b2a6bbdb0716442b104c03d0c4138bf60f9be
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835810"
---
<a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="158ac-104">Test della complessità di una Password (VB)</span><span class="sxs-lookup"><span data-stu-id="158ac-104">Testing the Strength of a Password (VB)</span></span>
====================
<span data-ttu-id="158ac-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="158ac-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="158ac-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="158ac-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="158ac-107">Le password sono necessarie, praticamente ovunque, in modo che gli utenti lazy tendono a sceglie le password semplici, facili da interrompere.</span><span class="sxs-lookup"><span data-stu-id="158ac-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="158ac-108">Il controllo PasswordStrength di ASP.NET AJAX Control Toolkit può controllare è davvero una password.</span><span class="sxs-lookup"><span data-stu-id="158ac-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="158ac-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="158ac-109">Overview</span></span>

<span data-ttu-id="158ac-110">Le password sono necessarie, praticamente ovunque, in modo che gli utenti lazy tendono a sceglie le password semplici, facili da interrompere.</span><span class="sxs-lookup"><span data-stu-id="158ac-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="158ac-111">Il `PasswordStrength` controllo in ASP.NET AJAX Control Toolkit possibile verificare la qualità una password.</span><span class="sxs-lookup"><span data-stu-id="158ac-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="158ac-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="158ac-112">Steps</span></span>

<span data-ttu-id="158ac-113">Il `PasswordStrength` controllo si estende una casella di testo e verifica se la password è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="158ac-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="158ac-114">Offre un'ampia gamma di opzioni tramite gli attributi. Ecco solo alcuni di essi:</span><span class="sxs-lookup"><span data-stu-id="158ac-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="158ac-115">`MinimumNumericCharacters` numero minimo di caratteri numerici richiesti nella password</span><span class="sxs-lookup"><span data-stu-id="158ac-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="158ac-116">`MinimumSymbolCharacters` numero minimo di simboli (non lettere e cifre) necessario includere nella password</span><span class="sxs-lookup"><span data-stu-id="158ac-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="158ac-117">`PreferredPasswordLength` lunghezza minima della password</span><span class="sxs-lookup"><span data-stu-id="158ac-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="158ac-118">`RequiresUpperAndLowerCaseCharacters` Se la password deve usare caratteri maiuscoli e minuscoli</span><span class="sxs-lookup"><span data-stu-id="158ac-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="158ac-119">Il `StrengthIndicatorType` vengono fornite le informazioni come presentare la complessità della password, come testo (valore `"Text"`) o come un tipo di indicatore di stato (valore `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="158ac-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="158ac-120">Nel `DisplayPosition` attributo, si configura in cui vengono visualizzate le informazioni.</span><span class="sxs-lookup"><span data-stu-id="158ac-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="158ac-121">Ecco un esempio completo, tra cui ASP.NET AJAX `ScriptManager` (controllo), il `PasswordStrength` controllo e ovviamente una casella di testo in cui l'utente può immettere una password.</span><span class="sxs-lookup"><span data-stu-id="158ac-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="158ac-122">Per fini dimostrativi, il campo del form quest'ultimo è un campo di testo normale e non un campo della password in modo che durante lo sviluppo è possibile visualizzare ciò che si sta digitando.</span><span class="sxs-lookup"><span data-stu-id="158ac-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="158ac-123">Eseguire la pagina e digitare da subito: solo dopo aver immesso le lettere minuscole, lettere maiuscole, cifre e simboli, la password viene considerata come non interrompibile.</span><span class="sxs-lookup"><span data-stu-id="158ac-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="158ac-124">[![A questo punto la password non è valida (ancora)](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="158ac-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="158ac-125">A questo punto la password non è valida (ancora) ([fare clic per visualizzare l'immagine con dimensioni normali](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="158ac-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="158ac-126">Precedente</span><span class="sxs-lookup"><span data-stu-id="158ac-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
