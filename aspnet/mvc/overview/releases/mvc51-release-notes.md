---
uid: mvc/overview/releases/mvc51-release-notes
title: Novità di ASP.NET MVC 5.1 | Documenti Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
ms.locfileid: "26504050"
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="90377-102">Novità di ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="90377-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="90377-103">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="90377-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="90377-104">In questo argomento vengono descritte le novità di ASP.NET Web MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="90377-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="90377-105">Requisiti software</span><span class="sxs-lookup"><span data-stu-id="90377-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="90377-106">Download</span><span class="sxs-lookup"><span data-stu-id="90377-106">Download</span></span>](#download)
- [<span data-ttu-id="90377-107">Documentazione</span><span class="sxs-lookup"><span data-stu-id="90377-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="90377-108">Nuove funzionalità in ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="90377-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="90377-109">Attributo di miglioramenti di routing</span><span class="sxs-lookup"><span data-stu-id="90377-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="90377-110">Bootstrap supporto per i modelli di editor</span><span class="sxs-lookup"><span data-stu-id="90377-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="90377-111">Supporto di enum nelle viste</span><span class="sxs-lookup"><span data-stu-id="90377-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="90377-112">Convalida non intrusiva per gli attributi di MinLength/MaxLength</span><span class="sxs-lookup"><span data-stu-id="90377-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="90377-113">Supporto di contesto 'this' in non intrusivo Ajax</span><span class="sxs-lookup"><span data-stu-id="90377-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="90377-114">[Problemi noti e modifiche di rilievo](#KnownBreakingChanges)- [correzioni di Bug](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="90377-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="90377-115">Requisiti software</span><span class="sxs-lookup"><span data-stu-id="90377-115">Software Requirements</span></span>

- <span data-ttu-id="90377-116">Visual Studio 2012: Download [2013.1 per Visual Studio 2012 degli strumenti Web ASP.NET e](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="90377-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="90377-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="90377-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="90377-118">Questo aggiornamento è necessario per la modifica delle visualizzazioni Razor di ASP.NET MVC 5.1.</span><span class="sxs-lookup"><span data-stu-id="90377-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="90377-119">Download</span><span class="sxs-lookup"><span data-stu-id="90377-119">Download</span></span>

<span data-ttu-id="90377-120">Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in Raccolta NuGet.</span><span class="sxs-lookup"><span data-stu-id="90377-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="90377-121">Seguono tutti i pacchetti di runtime di [controllo delle versioni semantico](http://semver.org/) specifica.</span><span class="sxs-lookup"><span data-stu-id="90377-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="90377-122">Il pacchetto di ASP.NET MVC 5.1 RTM più recente è la seguente versione: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="90377-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="90377-123">È possibile installare o aggiornare i pacchetti tramite [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="90377-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="90377-124">Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.</span><span class="sxs-lookup"><span data-stu-id="90377-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="90377-125">È possibile installare o aggiornare i pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:</span><span class="sxs-lookup"><span data-stu-id="90377-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="90377-126">Documentazione</span><span class="sxs-lookup"><span data-stu-id="90377-126">Documentation</span></span>

<span data-ttu-id="90377-127">Esercitazioni e altre informazioni su ASP.NET MVC 5.1 RTM sono disponibili dal sito web ASP.NET ( https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="90377-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="90377-128">Nuove funzionalità in ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="90377-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="90377-129">Attributo di miglioramenti di routing</span><span class="sxs-lookup"><span data-stu-id="90377-129">Attribute routing improvements</span></span>

 <span data-ttu-id="90377-130">Selezione delle route basata su attributo routing ora supporta i vincoli, abilitando il controllo delle versioni e intestazione.</span><span class="sxs-lookup"><span data-stu-id="90377-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="90377-131">Molti aspetti delle route di attributo sono ora personalizzabili tramite il `IDirectRouteFactory` interfaccia e `RouteFactoryAttribute` classe.</span><span class="sxs-lookup"><span data-stu-id="90377-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="90377-132">Il prefisso della route è estensibile tramite il `IRoutePrefix` interfaccia e `RoutePrefixAttribute` classe.</span><span class="sxs-lookup"><span data-stu-id="90377-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="90377-133">Supporto di enum nelle viste</span><span class="sxs-lookup"><span data-stu-id="90377-133">Enum support in views</span></span>

1. <span data-ttu-id="90377-134">Nuovo `@Html.EnumDropDownListFor()` metodi di supporto.</span><span class="sxs-lookup"><span data-stu-id="90377-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="90377-135">Che dovrebbero essere utilizzate come la maggior parte degli helper HTML, tenendo però che l'espressione deve restituire un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo o un [Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) dove `T` è un [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) tipo.</span><span class="sxs-lookup"><span data-stu-id="90377-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="90377-136">Utilizzare `EnumHelper.IsValidForEnumHelper()` per verificare i requisiti seguenti.</span><span class="sxs-lookup"><span data-stu-id="90377-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="90377-137">Nuovo `EnumHelper.GetSelectList()` metodi che restituiscono un `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="90377-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="90377-138">Ciò è utile quando è necessario modificare un elenco di selezione prima di chiamare, ad esempio, `@Html.DropDownListFor()`, o quando si desidera visualizzare i nomi che `@Html.EnumDropDownListFor()` Mostra.</span><span class="sxs-lookup"><span data-stu-id="90377-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="90377-139">Il codice seguente illustra queste API.</span><span class="sxs-lookup"><span data-stu-id="90377-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="90377-140">È possibile visualizzare un esempio completo [qui](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="90377-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="90377-141">Bootstrap supporto per i modelli di editor</span><span class="sxs-lookup"><span data-stu-id="90377-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="90377-142">È ora consente il passaggio negli attributi HTML nel [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) come un [oggetto anonimo](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="90377-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="90377-143">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="90377-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="90377-144">Convalida non intrusiva per MinLengthAttribute e MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="90377-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="90377-145">Convalida lato client per i tipi stringa e matrice sarà ora supportata per le proprietà decorata con il [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) e [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributi.</span><span class="sxs-lookup"><span data-stu-id="90377-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="90377-146">Supporto di contesto 'this' in non intrusivo Ajax</span><span class="sxs-lookup"><span data-stu-id="90377-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="90377-147">Le funzioni di callback (`OnBegin, OnComplete, OnFailure, OnSuccess`) saranno in grado di individuare l'elemento chiamata tramite il `this` contesto.</span><span class="sxs-lookup"><span data-stu-id="90377-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="90377-148">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="90377-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="90377-149">Problemi noti e modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="90377-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="90377-150">Routing degli attributi</span><span class="sxs-lookup"><span data-stu-id="90377-150">Attribute Routing</span></span>

<span data-ttu-id="90377-151">Ambiguità di corrispondenze di routing attributo ora segnalerà un errore anziché scelta la prima corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="90377-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="90377-152">Route di attributi vietate l'utilizzo di `{controller}` parametro e dall'utilizzo di `{action}` parametro route inserito in azioni.</span><span class="sxs-lookup"><span data-stu-id="90377-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="90377-153">Utilizzi di questi parametri molto probabilmente potrebbe causare ambiguità.</span><span class="sxs-lookup"><span data-stu-id="90377-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="90377-154">Lo scaffolding di API Web MVC e in un progetto con risultati pacchetti 5.1 nei 5.0 pacchetti per quelle che non esistono già nel progetto</span><span class="sxs-lookup"><span data-stu-id="90377-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="90377-155">L'aggiornamento di pacchetti NuGet per ASP.NET MVC 5.1 RTM non aggiornare gli strumenti di Visual Studio, ad esempio lo scaffolding di ASP.NET o il modello di progetto applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="90377-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="90377-156">Usano la versione precedente dei pacchetti di runtime ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="90377-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="90377-157">Di conseguenza, lo scaffolding di ASP.NET installerà la versione precedente (5.0.0.0) dei pacchetti richiesti, se non sono già disponibili nei progetti.</span><span class="sxs-lookup"><span data-stu-id="90377-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="90377-158">Tuttavia, lo scaffolding di ASP.NET in Visual Studio 2013 RTM o Update 1 non sovrascrive i pacchetti nei progetti più recenti.</span><span class="sxs-lookup"><span data-stu-id="90377-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="90377-159">Se si usa lo scaffolding di ASP.NET dopo l'aggiornamento di pacchetti dei progetti Web API 2.1 o ASP.NET MVC 5.1, assicurarsi che le versioni di API Web e ASP.NET MVC sono coerenti.</span><span class="sxs-lookup"><span data-stu-id="90377-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="90377-160">Evidenziazione della sintassi per le visualizzazioni Razor in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="90377-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="90377-161">Se l'aggiornamento alla versione RTM di ASP.NET MVC 5.1 senza l'aggiornamento di Visual Studio 2013, non si otterrà il supporto di editor di Visual Studio per l'evidenziazione della sintassi durante la modifica di visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="90377-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="90377-162">È necessario aggiornare Visual Studio 2013 per ottenere questo supporto.</span><span class="sxs-lookup"><span data-stu-id="90377-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="90377-163">Ridenominazione di tipo</span><span class="sxs-lookup"><span data-stu-id="90377-163">Type Renames</span></span>

<span data-ttu-id="90377-164">Alcuni dei tipi utilizzati per l'estensibilità di routing di attributo vengono rinominati in RTM 5.1.</span><span class="sxs-lookup"><span data-stu-id="90377-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="90377-165">**Nome di tipo precedente (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="90377-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="90377-166">**Nuovo tipo di nome (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="90377-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="90377-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="90377-167">IDirectRouteProvider</span></span> | <span data-ttu-id="90377-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="90377-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="90377-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="90377-169">RouteProviderAttribute</span></span> | <span data-ttu-id="90377-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="90377-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="90377-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="90377-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="90377-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="90377-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="90377-173">Correzioni di bug</span><span class="sxs-lookup"><span data-stu-id="90377-173">Bug Fixes</span></span>

<span data-ttu-id="90377-174">Questa versione include inoltre diverse correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="90377-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="90377-175">È possibile trovare l'elenco completo di seguito:</span><span class="sxs-lookup"><span data-stu-id="90377-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="90377-176">5.1.0 pacchetto</span><span class="sxs-lookup"><span data-stu-id="90377-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="90377-177">5.1.1 pacchetto</span><span class="sxs-lookup"><span data-stu-id="90377-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="90377-178">Il 5.1.2 pacchetto contiene gli aggiornamenti di IntelliSense ma non correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="90377-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
