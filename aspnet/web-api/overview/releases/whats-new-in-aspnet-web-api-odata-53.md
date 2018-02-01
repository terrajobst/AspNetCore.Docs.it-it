---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: "Novità di ASP.NET Web API OData 5.3 | Documenti Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="64279-102">Novità di ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="64279-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="64279-103">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="64279-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="64279-104">In questo argomento vengono descritte le novità di ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="64279-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="64279-105">Download</span><span class="sxs-lookup"><span data-stu-id="64279-105">Download</span></span>](#download)
- [<span data-ttu-id="64279-106">Documentazione</span><span class="sxs-lookup"><span data-stu-id="64279-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="64279-107">Librerie di base di OData</span><span class="sxs-lookup"><span data-stu-id="64279-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="64279-108">Nuove funzionalità</span><span class="sxs-lookup"><span data-stu-id="64279-108">New Features</span></span>](#newf)
- [<span data-ttu-id="64279-109">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="64279-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="64279-110">Correzioni di bug</span><span class="sxs-lookup"><span data-stu-id="64279-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="64279-111">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="64279-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="64279-112">Download</span><span class="sxs-lookup"><span data-stu-id="64279-112">Download</span></span>

<span data-ttu-id="64279-113">Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in Raccolta NuGet.</span><span class="sxs-lookup"><span data-stu-id="64279-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="64279-114">È possibile installare o aggiornare i pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:</span><span class="sxs-lookup"><span data-stu-id="64279-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="64279-115">Documentazione</span><span class="sxs-lookup"><span data-stu-id="64279-115">Documentation</span></span>

<span data-ttu-id="64279-116">È possibile trovare le esercitazioni e altra documentazione sulle ASP.NET Web API OData nel [sito web ASP.NET](../odata-support-in-aspnet-web-api/index.md).</span><span class="sxs-lookup"><span data-stu-id="64279-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="64279-117">Librerie di base di OData</span><span class="sxs-lookup"><span data-stu-id="64279-117">OData Core Libraries</span></span>

<span data-ttu-id="64279-118">Per OData v4, API Web viene ora utilizzato ODataLib versione 6.5.0</span><span class="sxs-lookup"><span data-stu-id="64279-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="64279-119">Nuove funzionalità di ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="64279-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="64279-120">Espandere il supporto per $levels in $</span><span class="sxs-lookup"><span data-stu-id="64279-120">Support for $levels in $expand</span></span>

<span data-ttu-id="64279-121">È possibile utilizzare il $levels query opzione in $espandere le query.</span><span class="sxs-lookup"><span data-stu-id="64279-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="64279-122">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="64279-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="64279-123">Questa query equivale a:</span><span class="sxs-lookup"><span data-stu-id="64279-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="64279-124">Supporto per tipi di entità aperto</span><span class="sxs-lookup"><span data-stu-id="64279-124">Support for Open Entity Types</span></span>

<span data-ttu-id="64279-125">Un *aprire tipo* è un tipo stuctured che contiene le proprietà dinamiche, oltre a tutte le proprietà che vengono dichiarate nella definizione del tipo.</span><span class="sxs-lookup"><span data-stu-id="64279-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="64279-126">I tipi aperti consentono di aggiungere la flessibilità per i modelli di data.</span><span class="sxs-lookup"><span data-stu-id="64279-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="64279-127">Per ulteriori informazioni, vedere xxxx.</span><span class="sxs-lookup"><span data-stu-id="64279-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="64279-128">Supporto per le proprietà di raccolta dinamica in tipi aperti</span><span class="sxs-lookup"><span data-stu-id="64279-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="64279-129">Una proprietà dinamica in precedenza, era necessario un solo valore.</span><span class="sxs-lookup"><span data-stu-id="64279-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="64279-130">Versione 5.3, le proprietà dinamiche possono avere valori della raccolta.</span><span class="sxs-lookup"><span data-stu-id="64279-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="64279-131">Ad esempio, nel payload JSON seguente, la `Emails` proprietà è una proprietà dinamica ed è di raccolta di tipo stringa:</span><span class="sxs-lookup"><span data-stu-id="64279-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="64279-132">Supporto per l'ereditarietà di tipi complessi</span><span class="sxs-lookup"><span data-stu-id="64279-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="64279-133">Ora i tipi complessi possono ereditare da un tipo di base.</span><span class="sxs-lookup"><span data-stu-id="64279-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="64279-134">Ad esempio, un servizio OData è possibile definire tipi complessi seguenti:</span><span class="sxs-lookup"><span data-stu-id="64279-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="64279-135">Di seguito è riportato il modello EDM per questo esempio:</span><span class="sxs-lookup"><span data-stu-id="64279-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="64279-136">Per ulteriori informazioni, vedere [esempio ereditarietà di tipo complesso OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="64279-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="64279-137">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="64279-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="64279-138">In questa sezione vengono descritti problemi noti e modifiche di rilievo nel 5.3 di ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="64279-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="64279-139">OData v4</span><span class="sxs-lookup"><span data-stu-id="64279-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="64279-140">Opzioni di query</span><span class="sxs-lookup"><span data-stu-id="64279-140">Query Options</span></span>

<span data-ttu-id="64279-141">Problema: Usando $ nidificata espandere con $levels = max risultati in una profondità di espansione non corretto.</span><span class="sxs-lookup"><span data-stu-id="64279-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="64279-142">Ad esempio, poiché la richiesta seguente:</span><span class="sxs-lookup"><span data-stu-id="64279-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="64279-143">Se `MaxExpansionDepth` è 5, la query darà origine a una profondità di espansione di 6.</span><span class="sxs-lookup"><span data-stu-id="64279-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="64279-144">Correzioni di bug e aggiornamenti delle funzionalità secondarie</span><span class="sxs-lookup"><span data-stu-id="64279-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="64279-145">Questa versione include inoltre diverse correzioni di bug e le funzionalità secondarie gli aggiornamenti.</span><span class="sxs-lookup"><span data-stu-id="64279-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="64279-146">È possibile trovare l'elenco completo di seguito:</span><span class="sxs-lookup"><span data-stu-id="64279-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="64279-147">Correzioni di bug</span><span class="sxs-lookup"><span data-stu-id="64279-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="64279-148">ASP.NET Web API OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="64279-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="64279-149">In questa versione sono stati apportati un [correzione di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) ad alcune delle enumerazioni AllowedFunctions.</span><span class="sxs-lookup"><span data-stu-id="64279-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="64279-150">Questa versione non dispone di qualsiasi altra funzionalità nuove o correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="64279-150">This release doesn't have any other bug fixes or new features.</span></span>
