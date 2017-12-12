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
# <a name="view-based-authorization"></a>Autorizzazione basata su Vista

Uno sviluppatore desidera spesso da mostrare, nascondere o modificare un'interfaccia utente in base all'identità utente corrente. È possibile accedere al servizio di autorizzazione all'interno di visualizzazioni MVC tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection#fundamentals-dependency-injection). Per inserire il servizio di autorizzazione in una visualizzazione Razor, utilizzare il `@inject` direttiva:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Se si desidera che il servizio di autorizzazione in ogni visualizzazione, inserire il `@inject` direttiva nel *viewimports. cshtml* file il *viste* directory. Per ulteriori informazioni, vedere [Dependency injection nelle viste](xref:mvc/views/dependency-injection).

Utilizzare il servizio di autorizzazione inserito per richiamare `AuthorizeAsync` esattamente nello stesso modo controllare durante [autorizzazione basata sulle risorse](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

In alcuni casi, la risorsa sarà il modello di visualizzazione. Richiamare `AuthorizeAsync` esattamente nello stesso modo controllare durante [autorizzazione basata sulle risorse](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

Nel codice precedente, il modello viene passato come risorsa che deve eseguire la valutazione dei criteri in considerazione.

> [!WARNING]
> Non fare affidamento sul Toggle visibilità degli elementi di interfaccia utente dell'applicazione come il controllo delle autorizzazioni solo. Nascondere un elemento dell'interfaccia utente potrebbe non completamente impedire l'accesso per l'azione del controller associato. Si consideri ad esempio il pulsante nel frammento di codice precedente. Un utente può richiamare il `Edit` il metodo di azione se egli conosce risorsa relativa di URL è */Document/Edit/1*. Per questo motivo, il `Edit` metodo di azione deve eseguire il proprio controllo delle autorizzazioni.
