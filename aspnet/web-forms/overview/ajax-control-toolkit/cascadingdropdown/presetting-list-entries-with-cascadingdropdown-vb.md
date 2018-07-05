---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Preimpostazione delle voci di elenco con CascadingDropDown (VB) del | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichi associati i valori in anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 5133516311478d0a4faab45721c6b1d0a251b4b0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817752"
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="478a6-103">Preimpostazione delle voci dell'elenco con CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="478a6-103">Presetting List Entries with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="478a6-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="478a6-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="478a6-105">[Scaricare il codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="478a6-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="478a6-106">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList Carica valori in un altro controllo DropDownList associati.</span><span class="sxs-lookup"><span data-stu-id="478a6-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="478a6-107">Con un po' di codice è possibile che un elemento di elenco è preselezionata l'opzione quando i dati sono stati caricati in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="478a6-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="478a6-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="478a6-108">Overview</span></span>

<span data-ttu-id="478a6-109">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList Carica valori in un altro controllo DropDownList associati.</span><span class="sxs-lookup"><span data-stu-id="478a6-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="478a6-110">(Ad esempio, un unico elenco fornisce un elenco di stati degli Stati Uniti e il successivo elenco viene riempito con città più importanti di tale stato.) Con un po' di codice è possibile che un elemento di elenco è preselezionata l'opzione quando i dati sono stati caricati in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="478a6-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="478a6-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="478a6-111">Steps</span></span>

<span data-ttu-id="478a6-112">Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="478a6-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="478a6-113">Quindi, è necessario un controllo DropDownList:</span><span class="sxs-lookup"><span data-stu-id="478a6-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="478a6-114">Per questo elenco, viene aggiunto un controllo extender CascadingDropDown, che fornisce informazioni di URL e il metodo di servizio web:</span><span class="sxs-lookup"><span data-stu-id="478a6-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="478a6-115">Il dispositivo extender CascadingDropDown chiama quindi in modo asincrono un servizio web con la firma del metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="478a6-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="478a6-116">Il metodo restituisce una matrice di tipo valore CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="478a6-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="478a6-117">Il costruttore del tipo prevede innanzitutto la didascalia della voce di elenco e quindi il valore (HTML `value` attributo).</span><span class="sxs-lookup"><span data-stu-id="478a6-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="478a6-118">Se il terzo argomento è impostato su true, l'elenco viene automaticamente selezionato nel browser.</span><span class="sxs-lookup"><span data-stu-id="478a6-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="478a6-119">Il caricamento della pagina nel browser riempirà l'elenco a discesa con i fornitori di tre, il secondo viene preselezionato.</span><span class="sxs-lookup"><span data-stu-id="478a6-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="478a6-120">[![L'elenco viene compilato e preselezionata automaticamente](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="478a6-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="478a6-121">L'elenco viene compilato e preselezionata automaticamente ([fare clic per visualizzare l'immagine con dimensioni normali](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="478a6-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="478a6-122">[Precedente](using-cascadingdropdown-with-a-database-vb.md)
> [Successivo](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="478a6-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>
