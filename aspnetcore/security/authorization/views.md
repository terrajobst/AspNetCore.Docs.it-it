---
title: Autorizzazione basata sulla visualizzazione in ASP.NET Core MVC
author: rick-anderson
description: In questo documento viene illustrato come inserire e utilizzare il servizio di autorizzazione all'interno di un ASP.NET Core visualizzazione Razor.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/views
ms.openlocfilehash: fc03da9eb98d36ffdda932ee5b16f327c2be9f83
ms.sourcegitcommit: 4818385c3cfe0805e15138a2c1785b62deeaab90
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896985"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="f7137-103">Autorizzazione basata sulla visualizzazione in ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="f7137-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="f7137-104">Spesso uno sviluppatore desidera visualizzare, nascondere o modificare un'interfaccia utente in base all'identità dell'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="f7137-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="f7137-105">È possibile accedere al servizio di autorizzazione all'interno delle visualizzazioni MVC tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f7137-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f7137-106">Per inserire il servizio di autorizzazione in una visualizzazione Razor, usare la direttiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="f7137-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="f7137-107">Se si vuole che il servizio di autorizzazione sia in ogni visualizzazione, inserire la direttiva `@inject` nel file *_ViewImports. cshtml* della directory *views* .</span><span class="sxs-lookup"><span data-stu-id="f7137-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="f7137-108">Per altre informazioni, vedere [Inserimento di dipendenze in visualizzazioni](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f7137-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="f7137-109">Usare il servizio di autorizzazione inserito per richiamare `AuthorizeAsync` esattamente nello stesso modo in cui si verifica durante l' [autorizzazione basata sulle risorse](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="f7137-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

<span data-ttu-id="f7137-110">In alcuni casi, la risorsa sarà il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f7137-110">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="f7137-111">Richiamare `AuthorizeAsync` esattamente nello stesso modo in cui si verificherà durante l' [autorizzazione basata sulle risorse](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="f7137-111">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

<span data-ttu-id="f7137-112">Nel codice precedente, il modello viene passato come una risorsa che deve prendere in considerazione la valutazione dei criteri.</span><span class="sxs-lookup"><span data-stu-id="f7137-112">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="f7137-113">Non fare affidamento sulla visibilità degli elementi dell'interfaccia utente dell'app come unico controllo di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f7137-113">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="f7137-114">Nascondere un elemento dell'interfaccia utente potrebbe non impedire completamente l'accesso all'azione del controller associato.</span><span class="sxs-lookup"><span data-stu-id="f7137-114">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="f7137-115">Si consideri ad esempio il pulsante nel frammento di codice precedente.</span><span class="sxs-lookup"><span data-stu-id="f7137-115">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="f7137-116">Un utente può richiamare il metodo di azione `Edit` se conosce l'URL relativo della risorsa è */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="f7137-116">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="f7137-117">Per questo motivo, il metodo di azione `Edit` deve eseguire il proprio controllo di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f7137-117">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
