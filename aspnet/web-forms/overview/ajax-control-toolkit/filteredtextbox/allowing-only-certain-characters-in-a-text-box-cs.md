---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Consentire solo determinati caratteri in una casella di testo (c#) | Microsoft Docs
author: wenz
description: I controlli di convalida di ASP.NET è possono garantire che solo alcuni caratteri sono consentiti nell'input dell'utente. Questo comunque non impedisce tuttavia agli utenti dalla digitazione non è validi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: dd7203d7f367f275d2d80c86119edc9645c9d24c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400730"
---
<a name="allowing-only-certain-characters-in-a-text-box-c"></a><span data-ttu-id="796f1-104">Consentire solo determinati caratteri in una casella di testo (c#)</span><span class="sxs-lookup"><span data-stu-id="796f1-104">Allowing Only Certain Characters in a Text Box (C#)</span></span>
====================
<span data-ttu-id="796f1-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="796f1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="796f1-106">[Scaricare il codice](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="796f1-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)</span></span>

> <span data-ttu-id="796f1-107">I controlli di convalida di ASP.NET è possono garantire che solo alcuni caratteri sono consentiti nell'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="796f1-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="796f1-108">Tuttavia questo comunque non impedisce agli utenti di digitare i caratteri non validi e provando a inviare il modulo.</span><span class="sxs-lookup"><span data-stu-id="796f1-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="796f1-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="796f1-109">Overview</span></span>

<span data-ttu-id="796f1-110">I controlli di convalida di ASP.NET è possono garantire che solo alcuni caratteri sono consentiti nell'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="796f1-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="796f1-111">Tuttavia questo comunque non impedisce agli utenti di digitare i caratteri non validi e provando a inviare il modulo.</span><span class="sxs-lookup"><span data-stu-id="796f1-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="796f1-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="796f1-112">Steps</span></span>

<span data-ttu-id="796f1-113">ASP.NET AJAX Control Toolkit contiene il `FilteredTextBox` controllo che estende una casella di testo.</span><span class="sxs-lookup"><span data-stu-id="796f1-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="796f1-114">Una volta attivato, solo un determinato set di caratteri possono essere immessi nel campo.</span><span class="sxs-lookup"><span data-stu-id="796f1-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="796f1-115">A questo scopo, è innanzitutto necessario come di consueto ASP.NET AJAX `ScriptManager` che carica le librerie JavaScript che vengono anche usate da ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="796f1-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

<span data-ttu-id="796f1-116">Quindi, abbiamo bisogno di una casella di testo:</span><span class="sxs-lookup"><span data-stu-id="796f1-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

<span data-ttu-id="796f1-117">Infine, il `FilteredTextBoxExtender` controllo si occupa di limitare i caratteri che l'utente è autorizzato nel tipo.</span><span class="sxs-lookup"><span data-stu-id="796f1-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="796f1-118">Innanzitutto, impostare il `TargetControlID` attributo la `ID` del `TextBox` controllo.</span><span class="sxs-lookup"><span data-stu-id="796f1-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="796f1-119">Quindi, scegliere uno dei contatori `FilterType` valori:</span><span class="sxs-lookup"><span data-stu-id="796f1-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="796f1-120">`Custom` impostazione predefinita. è necessario specificare un elenco di caratteri validi</span><span class="sxs-lookup"><span data-stu-id="796f1-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="796f1-121">`LowercaseLetters` solo lettere minuscole</span><span class="sxs-lookup"><span data-stu-id="796f1-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="796f1-122">`Numbers` solo cifre</span><span class="sxs-lookup"><span data-stu-id="796f1-122">`Numbers` digits only</span></span>
- <span data-ttu-id="796f1-123">`UppercaseLetters` solo lettere maiuscole</span><span class="sxs-lookup"><span data-stu-id="796f1-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="796f1-124">Se il `Custom FilterType` viene usato il `ValidChars` proprietà deve essere configurata e fornire un elenco di caratteri che possono essere digitati.</span><span class="sxs-lookup"><span data-stu-id="796f1-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="796f1-125">A proposito: se si tenta di incollare il testo nella casella di testo, tutti i caratteri non validi vengono rimossi.</span><span class="sxs-lookup"><span data-stu-id="796f1-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="796f1-126">Ecco il markup per il `FilteredTextBoxExtender` controllo che consente solo cifre (qualcosa che anche risulterebbe possibili con `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="796f1-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

<span data-ttu-id="796f1-127">Eseguire la pagina e riprovare l'immissione di una lettera di JavaScript è abilitato, non funzionerà; Nella pagina vengono tuttavia visualizzate cifre.</span><span class="sxs-lookup"><span data-stu-id="796f1-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="796f1-128">Si noti tuttavia che la protezione `FilteredTextBox` fornisce non valide: JavaScript se è abilitata, tutti i dati possono essere immessi nella casella di testo, quindi è necessario usare una convalida aggiuntiva significa che, ad esempio ASP. Controlli di convalida della rete.</span><span class="sxs-lookup"><span data-stu-id="796f1-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="796f1-129">[![È possibile immettere solo cifre](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="796f1-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)</span></span>

<span data-ttu-id="796f1-130">È possibile immettere solo cifre ([fare clic per visualizzare l'immagine con dimensioni normali](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="796f1-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="796f1-131">avanti</span><span class="sxs-lookup"><span data-stu-id="796f1-131">Next</span></span>](allowing-only-certain-characters-in-a-text-box-vb.md)
