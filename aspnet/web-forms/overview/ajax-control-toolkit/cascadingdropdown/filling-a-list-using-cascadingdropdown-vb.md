---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Compilazione di un elenco tramite CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichi associati i valori in anoth...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 4a2e2440c0b93fc3945c466a2c60c6560c9e57a4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834177"
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="3e946-103">Compilazione di un elenco tramite CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="3e946-103">Filling a List Using CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="3e946-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3e946-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3e946-105">[Scaricare il codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3e946-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="3e946-106">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList Carica valori in un altro controllo DropDownList associati.</span><span class="sxs-lookup"><span data-stu-id="3e946-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="3e946-107">(Ad esempio, un unico elenco fornisce un elenco di stati degli Stati Uniti e il successivo elenco viene riempito con città più importanti di tale stato.) Il primo problema da risolvere è riempire effettivamente un elenco a discesa tramite questo controllo.</span><span class="sxs-lookup"><span data-stu-id="3e946-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="3e946-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="3e946-108">Overview</span></span>

<span data-ttu-id="3e946-109">Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList Carica valori in un altro controllo DropDownList associati.</span><span class="sxs-lookup"><span data-stu-id="3e946-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="3e946-110">(Ad esempio, un unico elenco fornisce un elenco di stati degli Stati Uniti e il successivo elenco viene riempito con città più importanti di tale stato.) Il primo problema da risolvere è riempire effettivamente un elenco a discesa tramite questo controllo.</span><span class="sxs-lookup"><span data-stu-id="3e946-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="3e946-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="3e946-111">Steps</span></span>

<span data-ttu-id="3e946-112">Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="3e946-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="3e946-113">Quindi, è necessario un controllo DropDownList:</span><span class="sxs-lookup"><span data-stu-id="3e946-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="3e946-114">Per questo elenco, viene aggiunto un controllo extender CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="3e946-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="3e946-115">Invia una richiesta asincrona a un servizio web che quindi restituirà un elenco di voci da visualizzare nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="3e946-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="3e946-116">Per il corretto funzionamento, è necessario impostare gli attributi di CascadingDropDown seguenti:</span><span class="sxs-lookup"><span data-stu-id="3e946-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="3e946-117">`ServicePath`: URL di un servizio web distribuisce le voci dell'elenco</span><span class="sxs-lookup"><span data-stu-id="3e946-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="3e946-118">`ServiceMethod`: Metodo web fornire le voci dell'elenco</span><span class="sxs-lookup"><span data-stu-id="3e946-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="3e946-119">`TargetControlID`: ID dell'elenco a discesa</span><span class="sxs-lookup"><span data-stu-id="3e946-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="3e946-120">`Category`: Informazioni categoria in cui viene inviate al metodo web quando viene chiamato</span><span class="sxs-lookup"><span data-stu-id="3e946-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="3e946-121">`PromptText`: Testo visualizzato quando si caricano in modo asincrono i dati dell'elenco dal server</span><span class="sxs-lookup"><span data-stu-id="3e946-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="3e946-122">Ecco il markup per il `CascadingDropDown` elemento.</span><span class="sxs-lookup"><span data-stu-id="3e946-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="3e946-123">L'unica differenza tra c# e Visual Basic è il nome del servizio web associato:</span><span class="sxs-lookup"><span data-stu-id="3e946-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="3e946-124">Il codice JavaScript provenienti dal `CascadingDropDown` extender chiama un metodo del servizio web con la firma seguente:</span><span class="sxs-lookup"><span data-stu-id="3e946-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="3e946-125">In modo che l'aspetto importante è che il metodo deve restituire una matrice di tipo `CascadingDropDownNameValue` (definito da ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="3e946-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="3e946-126">Nel `CascadingDropDownNameValue` costruttore, prima è necessario specificare il testo della voce di elenco e quindi il relativo valore, proprio come `<option value="VALUE">NAME</option>` verrebbero impiegati in HTML.</span><span class="sxs-lookup"><span data-stu-id="3e946-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="3e946-127">Ecco alcuni dati di esempio:</span><span class="sxs-lookup"><span data-stu-id="3e946-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="3e946-128">Il caricamento della pagina nel browser verrà attivata l'elenco deve essere compilato con tre fornitori.</span><span class="sxs-lookup"><span data-stu-id="3e946-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="3e946-129">[![L'elenco viene compilato automaticamente](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3e946-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="3e946-130">L'elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine con dimensioni normali](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3e946-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3e946-131">[Precedente](using-auto-postback-with-cascadingdropdown-cs.md)
> [Successivo](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3e946-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
