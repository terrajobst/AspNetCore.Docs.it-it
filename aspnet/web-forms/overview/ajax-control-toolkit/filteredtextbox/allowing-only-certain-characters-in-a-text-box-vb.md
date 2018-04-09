---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Consentire solo il numero di caratteri specifici in una casella di testo (VB) | Documenti Microsoft
author: wenz
description: I controlli di convalida di ASP.NET è possono garantire che solo alcuni caratteri sono consentiti nell'input dell'utente. Questo ancora non impedisce tuttavia agli utenti di digitare non validi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b63a3582c09e08310c97d4adfc7b8273458a723
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="c5067-104">Consentire solo il numero di caratteri specifici in una casella di testo (VB)</span><span class="sxs-lookup"><span data-stu-id="c5067-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>
====================
<span data-ttu-id="c5067-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c5067-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c5067-106">[Scaricare codice](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c5067-106">[Download Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="c5067-107">I controlli di convalida di ASP.NET è possono garantire che solo alcuni caratteri sono consentiti nell'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="c5067-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="c5067-108">Questo tuttavia non impedisce tuttavia agli utenti di digitare i caratteri non validi e durante il tentativo di inviare il modulo.</span><span class="sxs-lookup"><span data-stu-id="c5067-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>


## <a name="overview"></a><span data-ttu-id="c5067-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c5067-109">Overview</span></span>

<span data-ttu-id="c5067-110">I controlli di convalida di ASP.NET è possono garantire che solo alcuni caratteri sono consentiti nell'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="c5067-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="c5067-111">Questo tuttavia non impedisce tuttavia agli utenti di digitare i caratteri non validi e durante il tentativo di inviare il modulo.</span><span class="sxs-lookup"><span data-stu-id="c5067-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="c5067-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="c5067-112">Steps</span></span>

<span data-ttu-id="c5067-113">ASP.NET AJAX Control Toolkit contiene il `FilteredTextBox` controllo che estende una casella di testo.</span><span class="sxs-lookup"><span data-stu-id="c5067-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="c5067-114">Una volta attivato, è possibile immettere solo un determinato set di caratteri nel campo.</span><span class="sxs-lookup"><span data-stu-id="c5067-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="c5067-115">Per il funzionamento, è necessario innanzitutto come di consueto ASP.NET AJAX `ScriptManager` che carica le librerie JavaScript che sono utilizzate anche da ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="c5067-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="c5067-116">Quindi, abbiamo bisogno di una casella di testo:</span><span class="sxs-lookup"><span data-stu-id="c5067-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="c5067-117">Infine, il `FilteredTextBoxExtender` controllo si occupa di limitare i caratteri che l'utente è autorizzato a digitare.</span><span class="sxs-lookup"><span data-stu-id="c5067-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="c5067-118">Innanzitutto, impostare il `TargetControlID` attributo la `ID` del `TextBox` controllo.</span><span class="sxs-lookup"><span data-stu-id="c5067-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="c5067-119">Quindi, scegliere una delle `FilterType` valori:</span><span class="sxs-lookup"><span data-stu-id="c5067-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="c5067-120">`Custom` impostazione predefinita. è necessario fornire un elenco di caratteri validi</span><span class="sxs-lookup"><span data-stu-id="c5067-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="c5067-121">`LowercaseLetters` solo lettere minuscole</span><span class="sxs-lookup"><span data-stu-id="c5067-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="c5067-122">`Numbers` solo cifre</span><span class="sxs-lookup"><span data-stu-id="c5067-122">`Numbers` digits only</span></span>
- <span data-ttu-id="c5067-123">`UppercaseLetters` solo le lettere maiuscole</span><span class="sxs-lookup"><span data-stu-id="c5067-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="c5067-124">Se il `Custom FilterType` viene utilizzato il `ValidChars` proprietà deve essere impostata e fornire un elenco di caratteri che possono essere digitati.</span><span class="sxs-lookup"><span data-stu-id="c5067-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="c5067-125">Dalla modalità: se si tenta di incollare il testo nella casella di testo, tutti i caratteri non validi vengono rimossi.</span><span class="sxs-lookup"><span data-stu-id="c5067-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="c5067-126">Ecco il markup per il `FilteredTextBoxExtender` controllo che consente solo cifre (qualcosa che inoltre sono stati possibili con `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="c5067-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="c5067-127">Eseguire la pagina e provare a immettere una lettera se JavaScript è abilitato, non funzionerà; cifre vengono tuttavia visualizzate nella pagina.</span><span class="sxs-lookup"><span data-stu-id="c5067-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="c5067-128">Tuttavia, si noti che la protezione `FilteredTextBox` fornisce non valide: se JavaScript è abilitato, i dati possono essere immessi nella casella di testo, è necessario utilizzare metodi di convalida aggiuntivi, ad esempio ASP. Controlli di convalida della rete.</span><span class="sxs-lookup"><span data-stu-id="c5067-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>


<span data-ttu-id="c5067-129">[![È possibile immettere solo cifre](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c5067-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="c5067-130">È possibile immettere solo cifre ([fare clic per visualizzare l'immagine ingrandita](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c5067-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c5067-131">Precedente</span><span class="sxs-lookup"><span data-stu-id="c5067-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
