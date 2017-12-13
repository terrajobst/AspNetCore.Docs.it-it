---
title: Convenzioni di autorizzazione di pagine Razor in ASP.NET Core
author: guardrex
description: Informazioni su come controllare l'accesso alle pagine con le convenzioni all'avvio di autorizzano gli utenti e consentono agli utenti anonimi di accedere a singole pagine o le cartelle di pagine.
ms.author: riande
manager: wpickett
ms.date: 10/27/2017
ms.topic: article
ms.assetid: f65ad22d-9472-478a-856c-c59c8681fa71
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 36acf3c06a462882972c5f389d544d98cadc35f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="aa2cb-103">Convenzioni di autorizzazione di pagine Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa2cb-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="aa2cb-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="aa2cb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="aa2cb-105">Un modo per controllare l'accesso nell'app pagine Razor consiste nell'utilizzare le convenzioni di autorizzazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="aa2cb-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="aa2cb-106">Queste convenzioni consentono di autorizzare gli utenti e consentire agli utenti anonimi di accedere a singole pagine o le cartelle di pagine.</span><span class="sxs-lookup"><span data-stu-id="aa2cb-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="aa2cb-107">Applicano le convenzioni descritte in questo argomento automaticamente [filtri di autorizzazione](xref:mvc/controllers/filters#authorization-filters) per controllare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="aa2cb-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="aa2cb-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aa2cb-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="aa2cb-109">Richiedere l'autorizzazione per accedere a una pagina</span><span class="sxs-lookup"><span data-stu-id="aa2cb-109">Require authorization to access a page</span></span>

<span data-ttu-id="aa2cb-110">Utilizzare il [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="aa2cb-110">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="aa2cb-111">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo della directory radice di pagine Razor senza un'estensione e contenente solo le barre.</span><span class="sxs-lookup"><span data-stu-id="aa2cb-111">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="aa2cb-112">Un [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) è disponibile se è necessario specificare un criterio di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="aa2cb-112">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="aa2cb-113">Richiedere l'autorizzazione per accedere alla cartella delle pagine</span><span class="sxs-lookup"><span data-stu-id="aa2cb-113">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="aa2cb-114">Utilizzare il [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) per tutte le pagine in una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="aa2cb-114">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="aa2cb-115">Il percorso specificato è il percorso di motore di visualizzazione, ovvero il percorso radice pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="aa2cb-115">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="aa2cb-116">Un [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) è disponibile se è necessario specificare un criterio di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="aa2cb-116">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="aa2cb-117">Consenti accesso anonimo a una pagina</span><span class="sxs-lookup"><span data-stu-id="aa2cb-117">Allow anonymous access to a page</span></span>

<span data-ttu-id="aa2cb-118">Utilizzare il [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una pagina nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="aa2cb-118">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="aa2cb-119">Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo della directory radice di pagine Razor senza un'estensione e contenente solo le barre.</span><span class="sxs-lookup"><span data-stu-id="aa2cb-119">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="aa2cb-120">Consenti accesso anonimo in una cartella delle pagine</span><span class="sxs-lookup"><span data-stu-id="aa2cb-120">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="aa2cb-121">Utilizzare il [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) per tutte le pagine in una cartella nel percorso specificato:</span><span class="sxs-lookup"><span data-stu-id="aa2cb-121">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="aa2cb-122">Il percorso specificato è il percorso di motore di visualizzazione, ovvero il percorso radice pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="aa2cb-122">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="aa2cb-123">Nota sulla combinazione autorizzato e l'accesso anonimo</span><span class="sxs-lookup"><span data-stu-id="aa2cb-123">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="aa2cb-124">È perfettamente valido per specificare che una cartella delle pagine richiedono l'autorizzazione e specificare che una pagina all'interno di tale cartella consente l'accesso anonimo:</span><span class="sxs-lookup"><span data-stu-id="aa2cb-124">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="aa2cb-125">L'operazione inversa, tuttavia, non è true.</span><span class="sxs-lookup"><span data-stu-id="aa2cb-125">The reverse, however, isn't true.</span></span> <span data-ttu-id="aa2cb-126">Non è possibile dichiarare una cartella delle pagine per l'accesso anonimo e specificare una pagina all'interno di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="aa2cb-126">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="aa2cb-127">Richiede l'autorizzazione nella pagina privata non funzionerà perché quando sia il `AllowAnonymousFilter` e `AuthorizeFilter` i filtri vengono applicati alla pagina, il `AllowAnonymousFilter` wins e controlla l'accesso.</span><span class="sxs-lookup"><span data-stu-id="aa2cb-127">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="see-also"></a><span data-ttu-id="aa2cb-128">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="aa2cb-128">See also</span></span>

* [<span data-ttu-id="aa2cb-129">Razor pagine route e pagina modello provider personalizzati</span><span class="sxs-lookup"><span data-stu-id="aa2cb-129">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* <span data-ttu-id="aa2cb-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe</span><span class="sxs-lookup"><span data-stu-id="aa2cb-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
