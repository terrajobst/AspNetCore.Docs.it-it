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
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>Autorizzazione basata su visualizzazione in ASP.NET Core MVC

Uno sviluppatore desidera spesso da mostrare, nascondere o modificare in altro modo un'interfaccia utente basata sull'identità dell'utente corrente. È possibile accedere al servizio di autorizzazione all'interno di visualizzazioni MVC tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection). Per inserire il servizio di autorizzazione in una visualizzazione Razor, usare il `@inject` direttiva:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Se si desidera che il servizio di autorizzazione in ogni visualizzazione, il `@inject` direttiva nel *viewimports. cshtml* file del *viste* directory. Per altre informazioni, vedere [Inserimento di dipendenze in visualizzazioni](xref:mvc/views/dependency-injection).

Usare il servizio di autorizzazione inserito per richiamare `AuthorizeAsync` in esattamente come si controlla durante [autorizzazione basata sulle risorse](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

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

In alcuni casi, la risorsa sarà il modello di visualizzazione. Richiamare `AuthorizeAsync` in esattamente come si controlla durante [autorizzazione basata sulle risorse](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

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

Nel codice precedente, il modello viene passato come una risorsa che dovrebbe richiedere la valutazione dei criteri in considerazione.

> [!WARNING]
> Non fare affidamento sulla visibilità attivata e disattivata di elementi dell'interfaccia utente dell'app come il controllo dell'autorizzazione esclusiva. Se si nasconde un elemento dell'interfaccia utente possono completamente impedisce l'accesso per l'azione del controller associato. Si consideri, ad esempio, il pulsante nel frammento di codice precedente. Un utente può richiamare il `Edit` metodo di azione se ne conosca la risorsa relativa URL viene */Document/Edit/1*. Per questo motivo, il `Edit` metodo di azione deve eseguire il proprio controllo di autorizzazione.
