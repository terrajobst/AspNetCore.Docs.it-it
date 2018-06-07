---
title: Convenzioni di autorizzazione di pagine Razor in ASP.NET Core
author: guardrex
description: Informazioni su come controllare l'accesso alle pagine con informazioni sulle convenzioni autorizzare gli utenti e consentire agli utenti anonimi di accedere a pagine o le cartelle di pagine.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 35a21156c001d8703e09e604129c4c2c500fe25f
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734653"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="3bab0-103">Convenzioni di autorizzazione di pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3bab0-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="3bab0-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3bab0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3bab0-105">Un modo per controllare l'accesso nell'app pagine Razor consiste nell'utilizzare le convenzioni di autorizzazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="3bab0-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="3bab0-106">Queste convenzioni consentono di autorizzare gli utenti e consentire agli utenti anonimi di accedere a singole pagine o le cartelle di pagine.</span><span class="sxs-lookup"><span data-stu-id="3bab0-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="3bab0-107">Applicano le convenzioni descritte in questo argomento automaticamente [filtri di autorizzazione](xref:mvc/controllers/filters#authorization-filters) per controllare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="3bab0-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="3bab0-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3bab0-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3bab0-109">Nell'app di esempio vengono utilizzate [autenticazione dei Cookie senza ASP.NET Identity Core](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="3bab0-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="3bab0-110">L'account utente per l'utente ipotetica, Maria Rodriguez, è hardcoded nell'app.</span><span class="sxs-lookup"><span data-stu-id="3bab0-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="3bab0-111">Utilizzare il nome utente di posta elettronica "maria.rodriguez@contoso.com" e indicare una password di accesso dell'utente.</span><span class="sxs-lookup"><span data-stu-id="3bab0-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="3bab0-112">L'utente viene autenticato nel `AuthenticateUser` metodo il *Pages/Account/Login.cshtml.cs* file.</span><span class="sxs-lookup"><span data-stu-id="3bab0-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="3bab0-113">In un esempio reale, l'utente viene autenticato un database.</span><span class="sxs-lookup"><span data-stu-id="3bab0-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="3bab0-114">Per utilizzare ASP.NET Identity Core, seguire le istruzioni riportate nel [Introduzione all'identità su ASP.NET Core](xref:security/authentication/identity) argomento.</span><span class="sxs-lookup"><span data-stu-id="3bab0-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="3bab0-115">I concetti e gli esempi illustrati in questo argomento si applicano anche alle App che usano ASP.NET Identity Core.</span><span class="sxs-lookup"><span data-stu-id="3bab0-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="3bab0-116">Richiedere l'autorizzazione per accedere a una pagina</span><span class="sxs-lookup"><span data-stu-id="3bab0-116">Require authorization to access a page</span></span>

<span data-ttu-id="3bab0-117">Utilizzare il [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="3bab0-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="3bab0-118">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo della directory radice di pagine Razor senza un'estensione e contenente solo le barre.</span><span class="sxs-lookup"><span data-stu-id="3bab0-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="3bab0-119">Un [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) è disponibile se è necessario specificare un criterio di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="3bab0-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="3bab0-120">Un' `AuthorizeFilter` può essere applicato a una classe di modello di pagina con il `[Authorize]` attributo di filtro.</span><span class="sxs-lookup"><span data-stu-id="3bab0-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="3bab0-121">Per altre informazioni, vedere [attributo di filtro Authorize](xref:mvc/razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="3bab0-121">For more information, see [Authorize filter attribute](xref:mvc/razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="3bab0-122">Richiedere l'autorizzazione per accedere alla cartella delle pagine</span><span class="sxs-lookup"><span data-stu-id="3bab0-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="3bab0-123">Utilizzare il [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) per tutte le pagine in una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="3bab0-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="3bab0-124">Il percorso specificato è il percorso di motore di visualizzazione, ovvero il percorso radice pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="3bab0-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="3bab0-125">Un [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) è disponibile se è necessario specificare un criterio di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="3bab0-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="3bab0-126">Consenti accesso anonimo a una pagina</span><span class="sxs-lookup"><span data-stu-id="3bab0-126">Allow anonymous access to a page</span></span>

<span data-ttu-id="3bab0-127">Utilizzare il [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una pagina nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="3bab0-127">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="3bab0-128">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo della directory radice di pagine Razor senza un'estensione e contenente solo le barre.</span><span class="sxs-lookup"><span data-stu-id="3bab0-128">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="3bab0-129">Consenti accesso anonimo in una cartella delle pagine</span><span class="sxs-lookup"><span data-stu-id="3bab0-129">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="3bab0-130">Utilizzare il [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) per tutte le pagine in una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="3bab0-130">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="3bab0-131">Il percorso specificato è il percorso di motore di visualizzazione, ovvero il percorso radice pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="3bab0-131">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="3bab0-132">Nota sulla combinazione autorizzato e l'accesso anonimo</span><span class="sxs-lookup"><span data-stu-id="3bab0-132">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="3bab0-133">È perfettamente valido per specificare che una cartella delle pagine richiedono l'autorizzazione e specificare che una pagina all'interno di tale cartella consente l'accesso anonimo:</span><span class="sxs-lookup"><span data-stu-id="3bab0-133">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="3bab0-134">L'operazione inversa, tuttavia, non è true.</span><span class="sxs-lookup"><span data-stu-id="3bab0-134">The reverse, however, isn't true.</span></span> <span data-ttu-id="3bab0-135">Non è possibile dichiarare una cartella delle pagine per l'accesso anonimo e specificare una pagina all'interno di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="3bab0-135">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="3bab0-136">Richiede l'autorizzazione nella pagina privata non funzionerà perché quando sia il `AllowAnonymousFilter` e `AuthorizeFilter` i filtri vengono applicati alla pagina, il `AllowAnonymousFilter` wins e controlla l'accesso.</span><span class="sxs-lookup"><span data-stu-id="3bab0-136">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3bab0-137">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3bab0-137">Additional resources</span></span>

* [<span data-ttu-id="3bab0-138">Route personalizzata di Razor Pages e provider di modelli di pagina</span><span class="sxs-lookup"><span data-stu-id="3bab0-138">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-conventions)
* <span data-ttu-id="3bab0-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe</span><span class="sxs-lookup"><span data-stu-id="3bab0-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
