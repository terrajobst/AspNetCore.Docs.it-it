---
title: Autorizzazione basata su Vista in ASP.NET MVC di base
author: rick-anderson
description: Questo documento viene illustrato come inserire e utilizzare il servizio di autorizzazione all'interno di una visualizzazione Razor di ASP.NET Core.
keywords: ASP.NET Core, autorizzazione Razor IAuthorizationService, l'autorizzazione,
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 756431f398c29376ab0ecd6c4f4d1db4f8022b0b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="view-based-authorization"></a><span data-ttu-id="4a7bb-104">Autorizzazione basata su Vista</span><span class="sxs-lookup"><span data-stu-id="4a7bb-104">View-based authorization</span></span>

<span data-ttu-id="4a7bb-105">Uno sviluppatore desidera spesso da mostrare, nascondere o modificare un'interfaccia utente in base all'identità utente corrente.</span><span class="sxs-lookup"><span data-stu-id="4a7bb-105">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="4a7bb-106">È possibile accedere al servizio di autorizzazione all'interno di visualizzazioni MVC tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4a7bb-106">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="4a7bb-107">Per inserire il servizio di autorizzazione in una visualizzazione Razor, utilizzare il `@inject` direttiva:</span><span class="sxs-lookup"><span data-stu-id="4a7bb-107">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="4a7bb-108">Se si desidera che il servizio di autorizzazione in ogni visualizzazione, inserire il `@inject` direttiva nel *viewimports. cshtml* file il *viste* directory.</span><span class="sxs-lookup"><span data-stu-id="4a7bb-108">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="4a7bb-109">Per ulteriori informazioni, vedere [Dependency injection nelle viste](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4a7bb-109">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="4a7bb-110">Utilizzare il servizio di autorizzazione inserito per richiamare `AuthorizeAsync` esattamente nello stesso modo controllare durante [autorizzazione basata sulle risorse](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="4a7bb-110">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4a7bb-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4a7bb-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4a7bb-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4a7bb-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="4a7bb-113">In alcuni casi, la risorsa sarà il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4a7bb-113">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="4a7bb-114">Richiamare `AuthorizeAsync` esattamente nello stesso modo controllare durante [autorizzazione basata sulle risorse](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="4a7bb-114">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4a7bb-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4a7bb-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4a7bb-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4a7bb-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="4a7bb-117">Nel codice precedente, il modello viene passato come risorsa che deve eseguire la valutazione dei criteri in considerazione.</span><span class="sxs-lookup"><span data-stu-id="4a7bb-117">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="4a7bb-118">Non fare affidamento sul Toggle visibilità degli elementi di interfaccia utente dell'applicazione come il controllo delle autorizzazioni solo.</span><span class="sxs-lookup"><span data-stu-id="4a7bb-118">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="4a7bb-119">Nascondere un elemento dell'interfaccia utente potrebbe non completamente impedire l'accesso per l'azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="4a7bb-119">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="4a7bb-120">Si consideri ad esempio il pulsante nel frammento di codice precedente.</span><span class="sxs-lookup"><span data-stu-id="4a7bb-120">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="4a7bb-121">Un utente può richiamare il `Edit` il metodo di azione se egli conosce risorsa relativa di URL è */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="4a7bb-121">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="4a7bb-122">Per questo motivo, il `Edit` metodo di azione deve eseguire il proprio controllo delle autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="4a7bb-122">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
