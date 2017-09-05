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
ms.openlocfilehash: 3b7fa6025d766da80ba92ee27af20bf9bfe0dcf4
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2017
---
# <a name="view-based-authorization"></a><span data-ttu-id="745eb-103">Autorizzazione basata su Vista</span><span class="sxs-lookup"><span data-stu-id="745eb-103">View Based Authorization</span></span>

<a name=security-authorization-views></a>

<span data-ttu-id="745eb-104">Spesso gli sviluppatori dovranno mostrare, nascondere o modificare un'interfaccia utente in base all'identità utente corrente.</span><span class="sxs-lookup"><span data-stu-id="745eb-104">Often a developer will want to show, hide or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="745eb-105">È possibile accedere al servizio di autorizzazione all'interno di visualizzazioni MVC tramite [inserimento di dipendenze](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="745eb-105">You can access the authorization service within MVC views via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection).</span></span> <span data-ttu-id="745eb-106">Per inserire il servizio di autorizzazione in un utilizzo di visualizzazione Razor il `@inject` direttiva, ad esempio `@inject IAuthorizationService AuthorizationService` (richiede `@using Microsoft.AspNetCore.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="745eb-106">To inject the authorization service into a Razor view use the `@inject` directive, for example `@inject IAuthorizationService AuthorizationService` (requires `@using Microsoft.AspNetCore.Authorization`).</span></span> <span data-ttu-id="745eb-107">Se si desidera che il servizio di autorizzazione in ogni visualizzazione inserire il `@inject` direttiva nel `_ViewImports.cshtml` file nel `Views` directory.</span><span class="sxs-lookup"><span data-stu-id="745eb-107">If you want the authorization service in every view then place the `@inject` directive into the `_ViewImports.cshtml` file in the `Views` directory.</span></span> <span data-ttu-id="745eb-108">Per ulteriori informazioni sull'inserimento di dipendenze in visualizzazioni vedere [Dependency injection nelle viste](../../mvc/views/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="745eb-108">For more information on dependency injection into views see [Dependency injection into views](../../mvc/views/dependency-injection.md).</span></span>

<span data-ttu-id="745eb-109">Dopo che sono state inserite il servizio di autorizzazione è utilizzare chiamando il `AuthorizeAsync` metodo esattamente come verrebbero archiviati durante [autorizzazione basata sulle risorse](resourcebased.md#security-authorization-resource-based-imperative).</span><span class="sxs-lookup"><span data-stu-id="745eb-109">Once you have injected the authorization service you use it by calling the `AuthorizeAsync` method in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative).</span></span>

```csharp
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

<span data-ttu-id="745eb-110">In alcuni casi la risorsa sarà il modello di visualizzazione ed è possibile chiamare `AuthorizeAsync` in esattamente come verrebbero archiviati durante [autorizzazione basata sulle risorse](resourcebased.md#security-authorization-resource-based-imperative);</span><span class="sxs-lookup"><span data-stu-id="745eb-110">In some cases the resource will be your view model, and you can call `AuthorizeAsync` in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative);</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="745eb-111">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="745eb-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
   @if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="745eb-112">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="745eb-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
   @if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```
---

<span data-ttu-id="745eb-113">Qui è possibile visualizzare che il modello viene passato come l'autorizzazione per le risorse debba prendere in considerazione.</span><span class="sxs-lookup"><span data-stu-id="745eb-113">Here you can see the model is passed as the resource authorization should take into consideration.</span></span>

>[!WARNING]
><span data-ttu-id="745eb-114">Non affidarsi mostrare o nascondere parti dell'interfaccia utente come unico metodo di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="745eb-114">Do not rely on showing or hiding parts of your UI as your only authorization method.</span></span> <span data-ttu-id="745eb-115">Nascondere un'interfaccia utente di elemento non significa che un utente non può accedervi.</span><span class="sxs-lookup"><span data-stu-id="745eb-115">Hiding a UI element does not mean a user cannot access it.</span></span> <span data-ttu-id="745eb-116">È anche necessario autorizzare l'utente all'interno del codice del controller.</span><span class="sxs-lookup"><span data-stu-id="745eb-116">You must also authorize the user within your controller code.</span></span>
