---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: La compilazione di un elenco utilizzando CascadingDropDown (c#) | Documenti Microsoft
author: wenz
description: Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: c9e47f6484e49013004bf15084f98440ee67558e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="1b489-103">La compilazione di un elenco utilizzando CascadingDropDown (c#)</span><span class="sxs-lookup"><span data-stu-id="1b489-103">Filling a List Using CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="1b489-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1b489-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1b489-105">[Scaricare codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1b489-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="1b489-106">Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="1b489-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="1b489-107">(Ad esempio, un unico elenco fornisce un elenco degli stati ed elenco successivo viene riempita con città importanti in tale stato.) Il primo problema da risolvere consiste nel compilare effettivamente un elenco a discesa tramite questo controllo.</span><span class="sxs-lookup"><span data-stu-id="1b489-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="1b489-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="1b489-108">Overview</span></span>

<span data-ttu-id="1b489-109">Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="1b489-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="1b489-110">(Ad esempio, un unico elenco fornisce un elenco degli stati ed elenco successivo viene riempita con città importanti in tale stato.) Il primo problema da risolvere consiste nel compilare effettivamente un elenco a discesa tramite questo controllo.</span><span class="sxs-lookup"><span data-stu-id="1b489-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="1b489-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="1b489-111">Steps</span></span>

<span data-ttu-id="1b489-112">Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="1b489-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="1b489-113">Quindi, è necessario un controllo di DropDownList:</span><span class="sxs-lookup"><span data-stu-id="1b489-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="1b489-114">Per questo elenco, viene aggiunta un'estensione CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="1b489-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="1b489-115">Questo invierà una richiesta asincrona a un servizio web che verrà quindi restituito un elenco di voci da visualizzare nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="1b489-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="1b489-116">Per il funzionamento, è necessario impostare gli attributi CascadingDropDown seguenti:</span><span class="sxs-lookup"><span data-stu-id="1b489-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="1b489-117">`ServicePath`: URL di un servizio web recapitare le voci dell'elenco</span><span class="sxs-lookup"><span data-stu-id="1b489-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="1b489-118">`ServiceMethod`: Metodo web recapito le voci dell'elenco</span><span class="sxs-lookup"><span data-stu-id="1b489-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="1b489-119">`TargetControlID`: ID dell'elenco a discesa</span><span class="sxs-lookup"><span data-stu-id="1b489-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="1b489-120">`Category`: Informazioni sulle categorie viene inviati al metodo web quando viene chiamato</span><span class="sxs-lookup"><span data-stu-id="1b489-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="1b489-121">`PromptText`: Testo visualizzato quando si caricano in modo asincrono i dati elenco dal server</span><span class="sxs-lookup"><span data-stu-id="1b489-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="1b489-122">Ecco il markup per il `CascadingDropDown` elemento.</span><span class="sxs-lookup"><span data-stu-id="1b489-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="1b489-123">L'unica differenza tra i linguaggi c# e Visual Basic è il nome del servizio web associato:</span><span class="sxs-lookup"><span data-stu-id="1b489-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="1b489-124">Il codice JavaScript proviene il `CascadingDropDown` extender chiama un metodo di servizio web con la firma seguente:</span><span class="sxs-lookup"><span data-stu-id="1b489-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="1b489-125">Pertanto l'aspetto importante è che il metodo deve restituire una matrice di tipo `CascadingDropDownNameValue` (definito da ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="1b489-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="1b489-126">Nel `CascadingDropDownNameValue` costruttore, prima è necessario specificare il testo della voce di elenco e quindi il relativo valore, come `<option value="VALUE">NAME</option>` farebbe in formato HTML.</span><span class="sxs-lookup"><span data-stu-id="1b489-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="1b489-127">Ecco alcuni dati di esempio:</span><span class="sxs-lookup"><span data-stu-id="1b489-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="1b489-128">Il caricamento della pagina nel browser verrà attivata l'elenco da popolare con tre fornitori.</span><span class="sxs-lookup"><span data-stu-id="1b489-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="1b489-129">[![L'elenco viene compilato automaticamente](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1b489-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="1b489-130">L'elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine ingrandita](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1b489-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1b489-131">avanti</span><span class="sxs-lookup"><span data-stu-id="1b489-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
