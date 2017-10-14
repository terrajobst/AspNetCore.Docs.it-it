---
title: Autorizzazione basata su Vista
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 58cafcfdc7946e82d1e0ea5de95e0e497b1b6bcf
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="view-based-authorization"></a>Autorizzazione basata su Vista

<a name="security-authorization-views"></a>

Spesso gli sviluppatori dovranno mostrare, nascondere o modificare un'interfaccia utente in base all'identità utente corrente. È possibile accedere al servizio di autorizzazione all'interno di visualizzazioni MVC tramite [inserimento di dipendenze](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection). Per inserire il servizio di autorizzazione in un utilizzo di visualizzazione Razor il `@inject` direttiva, ad esempio `@inject IAuthorizationService AuthorizationService` (richiede `@using Microsoft.AspNetCore.Authorization`). Se si desidera che il servizio di autorizzazione in ogni visualizzazione inserire il `@inject` direttiva nel `_ViewImports.cshtml` file nel `Views` directory. Per ulteriori informazioni sull'inserimento di dipendenze in visualizzazioni vedere [Dependency injection nelle viste](../../mvc/views/dependency-injection.md).

Dopo che sono state inserite il servizio di autorizzazione è utilizzare chiamando il `AuthorizeAsync` metodo esattamente come verrebbero archiviati durante [autorizzazione basata sulle risorse](resourcebased.md#security-authorization-resource-based-imperative).

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

In alcuni casi la risorsa sarà il modello di visualizzazione ed è possibile chiamare `AuthorizeAsync` in esattamente come verrebbero archiviati durante [autorizzazione basata sulle risorse](resourcebased.md#security-authorization-resource-based-imperative);

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

Qui è possibile visualizzare che il modello viene passato come l'autorizzazione per le risorse debba prendere in considerazione.

>[!WARNING]
>Non affidarsi mostrare o nascondere parti dell'interfaccia utente come unico metodo di autorizzazione. Nascondere un'interfaccia utente di elemento non significa che un utente non può accedervi. È anche necessario autorizzare l'utente all'interno del codice del controller.
