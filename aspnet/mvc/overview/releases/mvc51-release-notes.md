---
uid: mvc/overview/releases/mvc51-release-notes
title: What ' s New in ASP.NET MVC 5.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: d2e67f64e725e73c3bf9021295da3fe870079a45
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825648"
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="44d3d-102">What ' s New in ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="44d3d-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="44d3d-103">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="44d3d-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="44d3d-104">Questo argomento descrive cosa sono le novità di ASP.NET Web MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="44d3d-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="44d3d-105">Requisiti software</span><span class="sxs-lookup"><span data-stu-id="44d3d-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="44d3d-106">Download</span><span class="sxs-lookup"><span data-stu-id="44d3d-106">Download</span></span>](#download)
- [<span data-ttu-id="44d3d-107">Documentazione</span><span class="sxs-lookup"><span data-stu-id="44d3d-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="44d3d-108">Nuove funzionalità in ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="44d3d-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="44d3d-109">Miglioramenti di routing dell'attributo</span><span class="sxs-lookup"><span data-stu-id="44d3d-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="44d3d-110">Supporto di bootstrap per i modelli di editor</span><span class="sxs-lookup"><span data-stu-id="44d3d-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="44d3d-111">Supporto di enumerazione in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="44d3d-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="44d3d-112">Convalida discreta per gli attributi di MinLength/MaxLength</span><span class="sxs-lookup"><span data-stu-id="44d3d-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="44d3d-113">Supporto di contesto 'this' in Ajax Unobtrusive</span><span class="sxs-lookup"><span data-stu-id="44d3d-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="44d3d-114">[Problemi noti e modifiche di rilievo](#KnownBreakingChanges)- [correzioni di Bug](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="44d3d-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="44d3d-115">Requisiti software</span><span class="sxs-lookup"><span data-stu-id="44d3d-115">Software Requirements</span></span>

- <span data-ttu-id="44d3d-116">Visual Studio 2012: Download [ASP.NET e Web Tools 2013.1 per Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="44d3d-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="44d3d-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="44d3d-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="44d3d-118">Questo aggiornamento è necessaria per modificare le visualizzazioni Razor di ASP.NET MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="44d3d-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="44d3d-119">Download</span><span class="sxs-lookup"><span data-stu-id="44d3d-119">Download</span></span>

<span data-ttu-id="44d3d-120">Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in NuGet gallery.</span><span class="sxs-lookup"><span data-stu-id="44d3d-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="44d3d-121">Seguono tutti i pacchetti di runtime di [Versionamento semantico](http://semver.org/) specifica.</span><span class="sxs-lookup"><span data-stu-id="44d3d-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="44d3d-122">Il pacchetto di ASP.NET MVC 5.1 RTM più recente è la seguente versione: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="44d3d-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="44d3d-123">È possibile installare o aggiornare i pacchetti attraverso [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="44d3d-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="44d3d-124">Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.</span><span class="sxs-lookup"><span data-stu-id="44d3d-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="44d3d-125">È possibile installare o aggiornare ai pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:</span><span class="sxs-lookup"><span data-stu-id="44d3d-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="44d3d-126">Documentazione</span><span class="sxs-lookup"><span data-stu-id="44d3d-126">Documentation</span></span>

<span data-ttu-id="44d3d-127">Esercitazioni e altre informazioni su ASP.NET MVC 5.1 RTM sono disponibili dal sito web ASP.NET ( https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="44d3d-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="44d3d-128">Nuove funzionalità in ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="44d3d-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="44d3d-129">Miglioramenti di routing dell'attributo</span><span class="sxs-lookup"><span data-stu-id="44d3d-129">Attribute routing improvements</span></span>

 <span data-ttu-id="44d3d-130">Selezione delle route basata su attributi routing ora supporta i vincoli, abilitare il controllo delle versioni e l'intestazione.</span><span class="sxs-lookup"><span data-stu-id="44d3d-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="44d3d-131">Molti aspetti della route con attributi sono ora personalizzabili tramite il `IDirectRouteFactory` interfaccia e `RouteFactoryAttribute` classe.</span><span class="sxs-lookup"><span data-stu-id="44d3d-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="44d3d-132">Il prefisso della route è ora estendibile tramite le `IRoutePrefix` interfaccia e `RoutePrefixAttribute` classe.</span><span class="sxs-lookup"><span data-stu-id="44d3d-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="44d3d-133">Supporto di enumerazione in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="44d3d-133">Enum support in views</span></span>

1. <span data-ttu-id="44d3d-134">Nuovo `@Html.EnumDropDownListFor()` metodi helper.</span><span class="sxs-lookup"><span data-stu-id="44d3d-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="44d3d-135">Devono essere usati come la maggior parte degli helper HTML precisando che l'espressione deve restituire un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo o un' [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) dove `T` è un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo.</span><span class="sxs-lookup"><span data-stu-id="44d3d-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="44d3d-136">Usare `EnumHelper.IsValidForEnumHelper()` di verificare questi requisiti.</span><span class="sxs-lookup"><span data-stu-id="44d3d-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="44d3d-137">Nuove `EnumHelper.GetSelectList()` metodi che restituiscono un `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="44d3d-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="44d3d-138">Ciò è utile quando occorre modificare prima della chiamata, ad esempio, un elenco di selezione `@Html.DropDownListFor()`, o quando si vogliono visualizzare i nomi di cui `@Html.EnumDropDownListFor()` Mostra.</span><span class="sxs-lookup"><span data-stu-id="44d3d-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="44d3d-139">Il codice seguente illustra queste API.</span><span class="sxs-lookup"><span data-stu-id="44d3d-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="44d3d-140">È possibile vedere un esempio completo [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="44d3d-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="44d3d-141">Supporto di bootstrap per i modelli di editor</span><span class="sxs-lookup"><span data-stu-id="44d3d-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="44d3d-142">È ora consentito il passaggio negli attributi HTML nella [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) come un' [oggetto anonimo](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="44d3d-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="44d3d-143">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="44d3d-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="44d3d-144">Convalida discreta per MinLengthAttribute e MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="44d3d-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="44d3d-145">La convalida lato client per i tipi stringa e matrice sarà ora supportata per le proprietà decorata con il [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) e [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributi.</span><span class="sxs-lookup"><span data-stu-id="44d3d-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="44d3d-146">Supporto di contesto 'this' in Ajax Unobtrusive</span><span class="sxs-lookup"><span data-stu-id="44d3d-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="44d3d-147">Le funzioni di callback (`OnBegin, OnComplete, OnFailure, OnSuccess`) a questo punto sarà in grado di individuare l'elemento chiamante tramite la `this` contesto.</span><span class="sxs-lookup"><span data-stu-id="44d3d-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="44d3d-148">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="44d3d-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="44d3d-149">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="44d3d-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="44d3d-150">Routing con attributi</span><span class="sxs-lookup"><span data-stu-id="44d3d-150">Attribute Routing</span></span>

<span data-ttu-id="44d3d-151">Ambiguità corrisponda al routing attributo ora segnalerà un errore anziché scelta la prima corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="44d3d-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="44d3d-152">Le route con attributi non possono utilizzare il `{controller}` parametro e dall'uso di `{action}` parametro sulle route inserito in azioni.</span><span class="sxs-lookup"><span data-stu-id="44d3d-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="44d3d-153">Usa questi parametri molto probabile che potrebbe causare ambiguità.</span><span class="sxs-lookup"><span data-stu-id="44d3d-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="44d3d-154">Lo scaffolding di MVC o Web API in un progetto con risultati pacchetti 5.1 nei 5.0 pacchetti per quelle che non esistono già nel progetto</span><span class="sxs-lookup"><span data-stu-id="44d3d-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="44d3d-155">Aggiornamento di pacchetti NuGet per ASP.NET MVC 5.1 RTM non aggiorna il modello di progetto applicazione Web ASP.NET o gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="44d3d-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="44d3d-156">Usano la versione precedente dei pacchetti di runtime ASP.NET (versione=5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="44d3d-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="44d3d-157">Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (versione=5.0.0.0) dei pacchetti necessari, se non sono già disponibili nei progetti.</span><span class="sxs-lookup"><span data-stu-id="44d3d-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="44d3d-158">Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti più recenti nei progetti.</span><span class="sxs-lookup"><span data-stu-id="44d3d-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="44d3d-159">Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento dei pacchetti dei progetti Web API 2.1 o ASP.NET MVC 5.1, assicurarsi che le versioni dell'API Web e ASP.NET MVC sono coerenti.</span><span class="sxs-lookup"><span data-stu-id="44d3d-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="44d3d-160">L'evidenziazione della sintassi per le visualizzazioni Razor in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="44d3d-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="44d3d-161">Se si aggiorna alla versione RTM di ASP.NET MVC 5.1 senza aggiornare Visual Studio 2013, non si otterrà supporto all'editor di Visual Studio per l'evidenziazione della sintassi durante la modifica le visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="44d3d-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="44d3d-162">È necessario aggiornare Visual Studio 2013, per ottenere questo supporto.</span><span class="sxs-lookup"><span data-stu-id="44d3d-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="44d3d-163">Operazioni di ridenominazione di tipo</span><span class="sxs-lookup"><span data-stu-id="44d3d-163">Type Renames</span></span>

<span data-ttu-id="44d3d-164">Alcuni dei tipi usati per l'estensibilità di routing di attributi sono stati rinominati in 5.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="44d3d-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="44d3d-165">**Vecchio nome del tipo (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="44d3d-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="44d3d-166">**Nuovo tipo di nome (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="44d3d-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="44d3d-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="44d3d-167">IDirectRouteProvider</span></span> | <span data-ttu-id="44d3d-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="44d3d-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="44d3d-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="44d3d-169">RouteProviderAttribute</span></span> | <span data-ttu-id="44d3d-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="44d3d-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="44d3d-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="44d3d-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="44d3d-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="44d3d-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="44d3d-173">Correzioni di bug</span><span class="sxs-lookup"><span data-stu-id="44d3d-173">Bug Fixes</span></span>

<span data-ttu-id="44d3d-174">Questa versione include anche diverse correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="44d3d-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="44d3d-175">È possibile trovare l'elenco completo di seguito:</span><span class="sxs-lookup"><span data-stu-id="44d3d-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="44d3d-176">5.1.0 pacchetto</span><span class="sxs-lookup"><span data-stu-id="44d3d-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="44d3d-177">5.1.1 pacchetto</span><span class="sxs-lookup"><span data-stu-id="44d3d-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="44d3d-178">5.1.2 il pacchetto contiene gli aggiornamenti di IntelliSense, ma non le correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="44d3d-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
