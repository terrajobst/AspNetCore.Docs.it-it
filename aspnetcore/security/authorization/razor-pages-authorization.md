---
title: Convenzioni di autorizzazione Razor Pages in ASP.NET Core
author: rick-anderson
description: Informazioni su come controllare l'accesso alle pagine con convenzioni che autorizzano gli utenti e consentono agli utenti anonimi di accedere a pagine o cartelle di pagine.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 00fc487c6ac802f213bcf83994ecc2b1a1468589
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662057"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="e4f89-103">Convenzioni di autorizzazione Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4f89-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e4f89-104">Un modo per controllare l'accesso all'app Razor Pages consiste nell'usare le convenzioni di autorizzazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="e4f89-104">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="e4f89-105">Queste convenzioni consentono di autorizzare gli utenti e consentire agli utenti anonimi di accedere a singole pagine o cartelle di pagine.</span><span class="sxs-lookup"><span data-stu-id="e4f89-105">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="e4f89-106">Le convenzioni descritte in questo argomento applicano automaticamente i [filtri di autorizzazione](xref:mvc/controllers/filters#authorization-filters) per controllare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="e4f89-106">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="e4f89-107">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e4f89-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e4f89-108">L'app di esempio usa [l'autenticazione basata su cookie senza ASP.NET Core identità](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="e4f89-108">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="e4f89-109">I concetti e gli esempi illustrati in questo argomento si applicano ugualmente alle app che usano ASP.NET Core identità.</span><span class="sxs-lookup"><span data-stu-id="e4f89-109">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="e4f89-110">Per usare ASP.NET Core identità, seguire le istruzioni riportate in <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="e4f89-110">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="e4f89-111">Richiedi autorizzazione per accedere a una pagina</span><span class="sxs-lookup"><span data-stu-id="e4f89-111">Require authorization to access a page</span></span>

<span data-ttu-id="e4f89-112">Usare la convenzione di <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> alla pagina nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4f89-112">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="e4f89-113">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice senza un'estensione e contenente solo barre.</span><span class="sxs-lookup"><span data-stu-id="e4f89-113">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="e4f89-114">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="e4f89-114">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="e4f89-115">È possibile applicare una <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a una classe del modello di pagina con l'attributo di filtro `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="e4f89-115">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="e4f89-116">Per ulteriori informazioni, vedere [autorizzazione filtro attributo](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="e4f89-116">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="e4f89-117">Richiedi autorizzazione per accedere a una cartella di pagine</span><span class="sxs-lookup"><span data-stu-id="e4f89-117">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="e4f89-118">Usare la convenzione di <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le pagine di una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4f89-118">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="e4f89-119">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice.</span><span class="sxs-lookup"><span data-stu-id="e4f89-119">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="e4f89-120">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="e4f89-120">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="e4f89-121">Richiedi autorizzazione per accedere a una pagina di area</span><span class="sxs-lookup"><span data-stu-id="e4f89-121">Require authorization to access an area page</span></span>

<span data-ttu-id="e4f89-122">Usare la convenzione di <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> alla pagina area nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4f89-122">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="e4f89-123">Il nome della pagina è il percorso del file senza estensione rispetto alla directory radice delle pagine per l'area specificata.</span><span class="sxs-lookup"><span data-stu-id="e4f89-123">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="e4f89-124">Ad esempio, il nome della pagina per il file *areas/Identity/Pages/Manage/accounts. cshtml* è */Manage/accounts*.</span><span class="sxs-lookup"><span data-stu-id="e4f89-124">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="e4f89-125">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="e4f89-125">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="e4f89-126">Richiedi autorizzazione per accedere a una cartella di aree</span><span class="sxs-lookup"><span data-stu-id="e4f89-126">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="e4f89-127">Usare la convenzione di <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le aree di una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4f89-127">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="e4f89-128">Il percorso della cartella è il percorso della cartella relativa alla directory radice delle pagine per l'area specificata.</span><span class="sxs-lookup"><span data-stu-id="e4f89-128">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="e4f89-129">Ad esempio, il percorso della cartella per i file in *areas/Identity/pages/manage/* is */Manage*.</span><span class="sxs-lookup"><span data-stu-id="e4f89-129">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="e4f89-130">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="e4f89-130">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="e4f89-131">Consenti accesso anonimo a una pagina</span><span class="sxs-lookup"><span data-stu-id="e4f89-131">Allow anonymous access to a page</span></span>

<span data-ttu-id="e4f89-132">Usare la convenzione di <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a una pagina nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4f89-132">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="e4f89-133">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice senza un'estensione e contenente solo barre.</span><span class="sxs-lookup"><span data-stu-id="e4f89-133">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="e4f89-134">Consenti accesso anonimo a una cartella di pagine</span><span class="sxs-lookup"><span data-stu-id="e4f89-134">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="e4f89-135">Usare la convenzione di <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a tutte le pagine di una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4f89-135">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="e4f89-136">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice.</span><span class="sxs-lookup"><span data-stu-id="e4f89-136">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="e4f89-137">Nota sulla combinazione dell'accesso autorizzato e anonimo</span><span class="sxs-lookup"><span data-stu-id="e4f89-137">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="e4f89-138">È possibile specificare che una cartella di pagine richiede l'autorizzazione e quindi specificare che una pagina all'interno di tale cartella consenta l'accesso anonimo:</span><span class="sxs-lookup"><span data-stu-id="e4f89-138">It's valid to specify that a folder of pages requires authorization and then specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="e4f89-139">Il contrario, tuttavia, non è valido.</span><span class="sxs-lookup"><span data-stu-id="e4f89-139">The reverse, however, isn't valid.</span></span> <span data-ttu-id="e4f89-140">Non è possibile dichiarare una cartella di pagine per l'accesso anonimo e quindi specificare una pagina all'interno della cartella che richiede l'autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="e4f89-140">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="e4f89-141">La richiesta di autorizzazione nella pagina privata non riesce.</span><span class="sxs-lookup"><span data-stu-id="e4f89-141">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="e4f89-142">Quando si applicano sia la <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> che la <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> alla pagina, il <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ha la precedenza e controlla l'accesso.</span><span class="sxs-lookup"><span data-stu-id="e4f89-142">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4f89-143">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e4f89-143">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e4f89-144">Un modo per controllare l'accesso all'app Razor Pages consiste nell'usare le convenzioni di autorizzazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="e4f89-144">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="e4f89-145">Queste convenzioni consentono di autorizzare gli utenti e consentire agli utenti anonimi di accedere a singole pagine o cartelle di pagine.</span><span class="sxs-lookup"><span data-stu-id="e4f89-145">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="e4f89-146">Le convenzioni descritte in questo argomento applicano automaticamente i [filtri di autorizzazione](xref:mvc/controllers/filters#authorization-filters) per controllare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="e4f89-146">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="e4f89-147">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e4f89-147">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e4f89-148">L'app di esempio usa [l'autenticazione basata su cookie senza ASP.NET Core identità](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="e4f89-148">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="e4f89-149">I concetti e gli esempi illustrati in questo argomento si applicano ugualmente alle app che usano ASP.NET Core identità.</span><span class="sxs-lookup"><span data-stu-id="e4f89-149">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="e4f89-150">Per usare ASP.NET Core identità, seguire le istruzioni riportate in <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="e4f89-150">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="e4f89-151">Richiedi autorizzazione per accedere a una pagina</span><span class="sxs-lookup"><span data-stu-id="e4f89-151">Require authorization to access a page</span></span>

<span data-ttu-id="e4f89-152">Usare la convenzione di <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> alla pagina nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4f89-152">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="e4f89-153">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice senza un'estensione e contenente solo barre.</span><span class="sxs-lookup"><span data-stu-id="e4f89-153">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="e4f89-154">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="e4f89-154">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="e4f89-155">È possibile applicare una <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a una classe del modello di pagina con l'attributo di filtro `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="e4f89-155">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="e4f89-156">Per ulteriori informazioni, vedere [autorizzazione filtro attributo](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="e4f89-156">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="e4f89-157">Richiedi autorizzazione per accedere a una cartella di pagine</span><span class="sxs-lookup"><span data-stu-id="e4f89-157">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="e4f89-158">Usare la convenzione di <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le pagine di una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4f89-158">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="e4f89-159">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice.</span><span class="sxs-lookup"><span data-stu-id="e4f89-159">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="e4f89-160">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="e4f89-160">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="e4f89-161">Richiedi autorizzazione per accedere a una pagina di area</span><span class="sxs-lookup"><span data-stu-id="e4f89-161">Require authorization to access an area page</span></span>

<span data-ttu-id="e4f89-162">Usare la convenzione di <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> alla pagina area nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4f89-162">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="e4f89-163">Il nome della pagina è il percorso del file senza estensione rispetto alla directory radice delle pagine per l'area specificata.</span><span class="sxs-lookup"><span data-stu-id="e4f89-163">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="e4f89-164">Ad esempio, il nome della pagina per il file *areas/Identity/Pages/Manage/accounts. cshtml* è */Manage/accounts*.</span><span class="sxs-lookup"><span data-stu-id="e4f89-164">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="e4f89-165">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="e4f89-165">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="e4f89-166">Richiedi autorizzazione per accedere a una cartella di aree</span><span class="sxs-lookup"><span data-stu-id="e4f89-166">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="e4f89-167">Usare la convenzione di <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le aree di una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4f89-167">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="e4f89-168">Il percorso della cartella è il percorso della cartella relativa alla directory radice delle pagine per l'area specificata.</span><span class="sxs-lookup"><span data-stu-id="e4f89-168">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="e4f89-169">Ad esempio, il percorso della cartella per i file in *areas/Identity/pages/manage/* is */Manage*.</span><span class="sxs-lookup"><span data-stu-id="e4f89-169">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="e4f89-170">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="e4f89-170">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="e4f89-171">Consenti accesso anonimo a una pagina</span><span class="sxs-lookup"><span data-stu-id="e4f89-171">Allow anonymous access to a page</span></span>

<span data-ttu-id="e4f89-172">Usare la convenzione di <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a una pagina nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4f89-172">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="e4f89-173">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice senza un'estensione e contenente solo barre.</span><span class="sxs-lookup"><span data-stu-id="e4f89-173">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="e4f89-174">Consenti accesso anonimo a una cartella di pagine</span><span class="sxs-lookup"><span data-stu-id="e4f89-174">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="e4f89-175">Usare la convenzione di <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a tutte le pagine di una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4f89-175">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="e4f89-176">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice.</span><span class="sxs-lookup"><span data-stu-id="e4f89-176">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="e4f89-177">Nota sulla combinazione dell'accesso autorizzato e anonimo</span><span class="sxs-lookup"><span data-stu-id="e4f89-177">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="e4f89-178">È possibile specificare che una cartella di pagine che richiede l'autorizzazione e che specifica che una pagina all'interno di tale cartella consenta l'accesso anonimo:</span><span class="sxs-lookup"><span data-stu-id="e4f89-178">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="e4f89-179">Il contrario, tuttavia, non è valido.</span><span class="sxs-lookup"><span data-stu-id="e4f89-179">The reverse, however, isn't valid.</span></span> <span data-ttu-id="e4f89-180">Non è possibile dichiarare una cartella di pagine per l'accesso anonimo e quindi specificare una pagina all'interno della cartella che richiede l'autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="e4f89-180">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="e4f89-181">La richiesta di autorizzazione nella pagina privata non riesce.</span><span class="sxs-lookup"><span data-stu-id="e4f89-181">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="e4f89-182">Quando si applicano sia la <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> che la <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> alla pagina, il <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ha la precedenza e controlla l'accesso.</span><span class="sxs-lookup"><span data-stu-id="e4f89-182">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4f89-183">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e4f89-183">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
