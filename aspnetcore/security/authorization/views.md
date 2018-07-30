---
title: Autorizzazione basata su visualizzazione in ASP.NET Core MVC
author: rick-anderson
description: Questo documento illustra come inserire e usare il servizio di autorizzazione all'interno di una visualizzazione Razor di ASP.NET Core.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: e497c41d4dca29fed8733f18cf727804e3f06d8c
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342536"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="1ccf3-103">Autorizzazione basata su visualizzazione in ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="1ccf3-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="1ccf3-104">Uno sviluppatore desidera spesso da mostrare, nascondere o modificare in altro modo un'interfaccia utente basata sull'identità dell'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="1ccf3-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="1ccf3-105">È possibile accedere al servizio di autorizzazione all'interno di visualizzazioni MVC tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1ccf3-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1ccf3-106">Per inserire il servizio di autorizzazione in una visualizzazione Razor, usare il `@inject` direttiva:</span><span class="sxs-lookup"><span data-stu-id="1ccf3-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="1ccf3-107">Se si desidera che il servizio di autorizzazione in ogni visualizzazione, il `@inject` direttiva nel *viewimports. cshtml* file del *viste* directory.</span><span class="sxs-lookup"><span data-stu-id="1ccf3-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="1ccf3-108">Per altre informazioni, vedere [Inserimento di dipendenze in visualizzazioni](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1ccf3-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="1ccf3-109">Usare il servizio di autorizzazione inserito per richiamare `AuthorizeAsync` in esattamente come si controlla durante [autorizzazione basata sulle risorse](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="1ccf3-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ccf3-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1ccf3-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ccf3-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1ccf3-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="1ccf3-112">In alcuni casi, la risorsa sarà il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1ccf3-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="1ccf3-113">Richiamare `AuthorizeAsync` in esattamente come si controlla durante [autorizzazione basata sulle risorse](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="1ccf3-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1ccf3-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1ccf3-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1ccf3-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1ccf3-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="1ccf3-116">Nel codice precedente, il modello viene passato come una risorsa che dovrebbe richiedere la valutazione dei criteri in considerazione.</span><span class="sxs-lookup"><span data-stu-id="1ccf3-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="1ccf3-117">Non fare affidamento sulla visibilità attivata e disattivata di elementi dell'interfaccia utente dell'app come il controllo dell'autorizzazione esclusiva.</span><span class="sxs-lookup"><span data-stu-id="1ccf3-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="1ccf3-118">Se si nasconde un elemento dell'interfaccia utente possono completamente impedisce l'accesso per l'azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="1ccf3-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="1ccf3-119">Si consideri, ad esempio, il pulsante nel frammento di codice precedente.</span><span class="sxs-lookup"><span data-stu-id="1ccf3-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="1ccf3-120">Un utente può richiamare il `Edit` metodo di azione se ne conosca la risorsa relativa URL viene */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="1ccf3-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="1ccf3-121">Per questo motivo, il `Edit` metodo di azione deve eseguire il proprio controllo di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="1ccf3-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
