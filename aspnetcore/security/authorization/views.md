---
title: Autorizzazione basata su Vista in ASP.NET MVC di base
author: rick-anderson
description: Questo documento viene illustrato come inserire e utilizzare il servizio di autorizzazione all'interno di una visualizzazione Razor di ASP.NET Core.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: f25bab61afc93ff14bfd9c36d95a6d2e54b06dfb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277817"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="3b01d-103">Autorizzazione basata su Vista in ASP.NET MVC di base</span><span class="sxs-lookup"><span data-stu-id="3b01d-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="3b01d-104">Uno sviluppatore desidera spesso da mostrare, nascondere o modificare un'interfaccia utente in base all'identità utente corrente.</span><span class="sxs-lookup"><span data-stu-id="3b01d-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="3b01d-105">È possibile accedere al servizio di autorizzazione all'interno di visualizzazioni MVC tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3b01d-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="3b01d-106">Per inserire il servizio di autorizzazione in una visualizzazione Razor, utilizzare il `@inject` direttiva:</span><span class="sxs-lookup"><span data-stu-id="3b01d-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="3b01d-107">Se si desidera che il servizio di autorizzazione in ogni visualizzazione, inserire il `@inject` direttiva nel *viewimports. cshtml* file il *viste* directory.</span><span class="sxs-lookup"><span data-stu-id="3b01d-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="3b01d-108">Per altre informazioni, vedere [Inserimento di dipendenze in visualizzazioni](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3b01d-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="3b01d-109">Utilizzare il servizio di autorizzazione inserito per richiamare `AuthorizeAsync` esattamente nello stesso modo controllare durante [autorizzazione basata sulle risorse](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="3b01d-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b01d-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b01d-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b01d-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b01d-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="3b01d-112">In alcuni casi, la risorsa sarà il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="3b01d-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="3b01d-113">Richiamare `AuthorizeAsync` esattamente nello stesso modo controllare durante [autorizzazione basata sulle risorse](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="3b01d-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3b01d-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3b01d-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3b01d-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3b01d-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="3b01d-116">Nel codice precedente, il modello viene passato come risorsa che deve eseguire la valutazione dei criteri in considerazione.</span><span class="sxs-lookup"><span data-stu-id="3b01d-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="3b01d-117">Non fare affidamento sul Toggle visibilità degli elementi di interfaccia utente dell'applicazione come il controllo delle autorizzazioni solo.</span><span class="sxs-lookup"><span data-stu-id="3b01d-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="3b01d-118">Nascondere un elemento dell'interfaccia utente potrebbe non completamente impedire l'accesso per l'azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="3b01d-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="3b01d-119">Si consideri ad esempio il pulsante nel frammento di codice precedente.</span><span class="sxs-lookup"><span data-stu-id="3b01d-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="3b01d-120">Un utente può richiamare il `Edit` il metodo di azione se egli conosce risorsa relativa di URL è */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="3b01d-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="3b01d-121">Per questo motivo, il `Edit` metodo di azione deve eseguire il proprio controllo delle autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="3b01d-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
