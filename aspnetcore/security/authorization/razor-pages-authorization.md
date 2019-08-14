---
title: Convenzioni di autorizzazione Razor Pages in ASP.NET Core
author: guardrex
description: Informazioni su come controllare l'accesso alle pagine con convenzioni che autorizzano gli utenti e consentono agli utenti anonimi di accedere a pagine o cartelle di pagine.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: e0102ff64921a83f0330acb6f5d9bfd90f64ca7a
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994030"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="e4b66-103">Convenzioni di autorizzazione Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4b66-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="e4b66-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e4b66-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e4b66-105">Un modo per controllare l'accesso all'app Razor Pages consiste nell'usare le convenzioni di autorizzazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="e4b66-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="e4b66-106">Queste convenzioni consentono di autorizzare gli utenti e consentire agli utenti anonimi di accedere a singole pagine o cartelle di pagine.</span><span class="sxs-lookup"><span data-stu-id="e4b66-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="e4b66-107">Le convenzioni descritte in questo argomento applicano automaticamente i [filtri di autorizzazione](xref:mvc/controllers/filters#authorization-filters) per controllare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="e4b66-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="e4b66-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e4b66-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e4b66-109">L'app di esempio usa [l'autenticazione basata su cookie senza ASP.NET Core identità](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="e4b66-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="e4b66-110">I concetti e gli esempi illustrati in questo argomento si applicano ugualmente alle app che usano ASP.NET Core identità.</span><span class="sxs-lookup"><span data-stu-id="e4b66-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="e4b66-111">Per usare ASP.NET Core identità, seguire le istruzioni riportate in <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="e4b66-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="e4b66-112">Richiedi autorizzazione per accedere a una pagina</span><span class="sxs-lookup"><span data-stu-id="e4b66-112">Require authorization to access a page</span></span>

<span data-ttu-id="e4b66-113">Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> oggetto alla pagina nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4b66-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="e4b66-114">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice senza un'estensione e contenente solo barre.</span><span class="sxs-lookup"><span data-stu-id="e4b66-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="e4b66-115">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="e4b66-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="e4b66-116">Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> oggetto può essere applicato a una classe del modello di `[Authorize]` pagina con l'attributo Filter.</span><span class="sxs-lookup"><span data-stu-id="e4b66-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="e4b66-117">Per ulteriori informazioni, vedere [autorizzazione filtro attributo](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="e4b66-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="e4b66-118">Richiedi autorizzazione per accedere a una cartella di pagine</span><span class="sxs-lookup"><span data-stu-id="e4b66-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="e4b66-119">Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le pagine di una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4b66-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="e4b66-120">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice.</span><span class="sxs-lookup"><span data-stu-id="e4b66-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="e4b66-121">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="e4b66-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="e4b66-122">Richiedi autorizzazione per accedere a una pagina di area</span><span class="sxs-lookup"><span data-stu-id="e4b66-122">Require authorization to access an area page</span></span>

<span data-ttu-id="e4b66-123">Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> alla pagina area nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4b66-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="e4b66-124">Il nome della pagina è il percorso del file senza estensione rispetto alla directory radice delle pagine per l'area specificata.</span><span class="sxs-lookup"><span data-stu-id="e4b66-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="e4b66-125">Ad esempio, il nome della pagina per il file *areas/Identity/Pages/Manage/accounts. cshtml* è */Manage/accounts*.</span><span class="sxs-lookup"><span data-stu-id="e4b66-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="e4b66-126">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="e4b66-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="e4b66-127">Richiedi autorizzazione per accedere a una cartella di aree</span><span class="sxs-lookup"><span data-stu-id="e4b66-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="e4b66-128">Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le aree di una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4b66-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="e4b66-129">Il percorso della cartella è il percorso della cartella relativa alla directory radice delle pagine per l'area specificata.</span><span class="sxs-lookup"><span data-stu-id="e4b66-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="e4b66-130">Ad esempio, il percorso della cartella per i file in *areas/Identity/pages/manage/* is */Manage*.</span><span class="sxs-lookup"><span data-stu-id="e4b66-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="e4b66-131">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="e4b66-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="e4b66-132">Consenti accesso anonimo a una pagina</span><span class="sxs-lookup"><span data-stu-id="e4b66-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="e4b66-133">Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> oggetto a una pagina nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4b66-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="e4b66-134">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice senza un'estensione e contenente solo barre.</span><span class="sxs-lookup"><span data-stu-id="e4b66-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="e4b66-135">Consenti accesso anonimo a una cartella di pagine</span><span class="sxs-lookup"><span data-stu-id="e4b66-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="e4b66-136">Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a tutte le pagine di una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4b66-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="e4b66-137">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice.</span><span class="sxs-lookup"><span data-stu-id="e4b66-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="e4b66-138">Nota sulla combinazione dell'accesso autorizzato e anonimo</span><span class="sxs-lookup"><span data-stu-id="e4b66-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="e4b66-139">È possibile specificare che una cartella di pagine che richiede l'autorizzazione e che specifica che una pagina all'interno di tale cartella consenta l'accesso anonimo:</span><span class="sxs-lookup"><span data-stu-id="e4b66-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="e4b66-140">Il contrario, tuttavia, non è valido.</span><span class="sxs-lookup"><span data-stu-id="e4b66-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="e4b66-141">Non è possibile dichiarare una cartella di pagine per l'accesso anonimo e quindi specificare una pagina all'interno della cartella che richiede l'autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="e4b66-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="e4b66-142">La richiesta di autorizzazione nella pagina privata non riesce.</span><span class="sxs-lookup"><span data-stu-id="e4b66-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="e4b66-143"><xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> Quando e <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> vengono applicati alla pagina, ha la <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> precedenza e controlla l'accesso.</span><span class="sxs-lookup"><span data-stu-id="e4b66-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4b66-144">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e4b66-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e4b66-145">Un modo per controllare l'accesso all'app Razor Pages consiste nell'usare le convenzioni di autorizzazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="e4b66-145">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="e4b66-146">Queste convenzioni consentono di autorizzare gli utenti e consentire agli utenti anonimi di accedere a singole pagine o cartelle di pagine.</span><span class="sxs-lookup"><span data-stu-id="e4b66-146">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="e4b66-147">Le convenzioni descritte in questo argomento applicano automaticamente i [filtri di autorizzazione](xref:mvc/controllers/filters#authorization-filters) per controllare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="e4b66-147">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="e4b66-148">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e4b66-148">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e4b66-149">L'app di esempio usa [l'autenticazione basata su cookie senza ASP.NET Core identità](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="e4b66-149">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="e4b66-150">I concetti e gli esempi illustrati in questo argomento si applicano ugualmente alle app che usano ASP.NET Core identità.</span><span class="sxs-lookup"><span data-stu-id="e4b66-150">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="e4b66-151">Per usare ASP.NET Core identità, seguire le istruzioni riportate in <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="e4b66-151">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="e4b66-152">Richiedi autorizzazione per accedere a una pagina</span><span class="sxs-lookup"><span data-stu-id="e4b66-152">Require authorization to access a page</span></span>

<span data-ttu-id="e4b66-153">Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> oggetto alla pagina nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4b66-153">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="e4b66-154">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice senza un'estensione e contenente solo barre.</span><span class="sxs-lookup"><span data-stu-id="e4b66-154">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="e4b66-155">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="e4b66-155">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="e4b66-156">Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> oggetto può essere applicato a una classe del modello di `[Authorize]` pagina con l'attributo Filter.</span><span class="sxs-lookup"><span data-stu-id="e4b66-156">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="e4b66-157">Per ulteriori informazioni, vedere [autorizzazione filtro attributo](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="e4b66-157">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="e4b66-158">Richiedi autorizzazione per accedere a una cartella di pagine</span><span class="sxs-lookup"><span data-stu-id="e4b66-158">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="e4b66-159">Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le pagine di una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4b66-159">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="e4b66-160">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice.</span><span class="sxs-lookup"><span data-stu-id="e4b66-160">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="e4b66-161">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="e4b66-161">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="e4b66-162">Richiedi autorizzazione per accedere a una pagina di area</span><span class="sxs-lookup"><span data-stu-id="e4b66-162">Require authorization to access an area page</span></span>

<span data-ttu-id="e4b66-163">Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> alla pagina area nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4b66-163">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="e4b66-164">Il nome della pagina è il percorso del file senza estensione rispetto alla directory radice delle pagine per l'area specificata.</span><span class="sxs-lookup"><span data-stu-id="e4b66-164">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="e4b66-165">Ad esempio, il nome della pagina per il file *areas/Identity/Pages/Manage/accounts. cshtml* è */Manage/accounts*.</span><span class="sxs-lookup"><span data-stu-id="e4b66-165">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="e4b66-166">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="e4b66-166">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="e4b66-167">Richiedi autorizzazione per accedere a una cartella di aree</span><span class="sxs-lookup"><span data-stu-id="e4b66-167">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="e4b66-168">Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le aree di una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4b66-168">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="e4b66-169">Il percorso della cartella è il percorso della cartella relativa alla directory radice delle pagine per l'area specificata.</span><span class="sxs-lookup"><span data-stu-id="e4b66-169">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="e4b66-170">Ad esempio, il percorso della cartella per i file in *areas/Identity/pages/manage/* is */Manage*.</span><span class="sxs-lookup"><span data-stu-id="e4b66-170">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="e4b66-171">Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="e4b66-171">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="e4b66-172">Consenti accesso anonimo a una pagina</span><span class="sxs-lookup"><span data-stu-id="e4b66-172">Allow anonymous access to a page</span></span>

<span data-ttu-id="e4b66-173">Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> oggetto a una pagina nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4b66-173">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="e4b66-174">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice senza un'estensione e contenente solo barre.</span><span class="sxs-lookup"><span data-stu-id="e4b66-174">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="e4b66-175">Consenti accesso anonimo a una cartella di pagine</span><span class="sxs-lookup"><span data-stu-id="e4b66-175">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="e4b66-176">Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a tutte le pagine di una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="e4b66-176">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="e4b66-177">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice.</span><span class="sxs-lookup"><span data-stu-id="e4b66-177">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="e4b66-178">Nota sulla combinazione dell'accesso autorizzato e anonimo</span><span class="sxs-lookup"><span data-stu-id="e4b66-178">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="e4b66-179">È possibile specificare che una cartella di pagine che richiede l'autorizzazione e che specifica che una pagina all'interno di tale cartella consenta l'accesso anonimo:</span><span class="sxs-lookup"><span data-stu-id="e4b66-179">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="e4b66-180">Il contrario, tuttavia, non è valido.</span><span class="sxs-lookup"><span data-stu-id="e4b66-180">The reverse, however, isn't valid.</span></span> <span data-ttu-id="e4b66-181">Non è possibile dichiarare una cartella di pagine per l'accesso anonimo e quindi specificare una pagina all'interno della cartella che richiede l'autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="e4b66-181">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="e4b66-182">La richiesta di autorizzazione nella pagina privata non riesce.</span><span class="sxs-lookup"><span data-stu-id="e4b66-182">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="e4b66-183"><xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> Quando e <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> vengono applicati alla pagina, ha la <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> precedenza e controlla l'accesso.</span><span class="sxs-lookup"><span data-stu-id="e4b66-183">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4b66-184">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e4b66-184">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
