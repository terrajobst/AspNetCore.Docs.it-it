---
title: Convenzioni di autorizzazione di Razor Pages in ASP.NET Core
author: guardrex
description: Informazioni su come controllare l'accesso alle pagine con le convenzioni di autorizzano gli utenti e consentono agli utenti anonimi di accedere alle pagine o le cartelle di pagine.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: ff061f96f30cd893b903403de760a172c924cf06
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895418"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Convenzioni di autorizzazione di Razor Pages in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Un modo per controllare l'accesso nell'app Razor Pages è usare convenzioni di autorizzazione all'avvio. Queste convenzioni consentono di autorizzare gli utenti e consentire agli utenti anonimi di accedere alle cartelle di pagine o le singole pagine. Applicano le convenzioni descritte in questo argomento automaticamente [filtri di autorizzazione](xref:mvc/controllers/filters#authorization-filters) per controllare l'accesso.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

L'app di esempio Usa [l'autenticazione tramite cookie senza ASP.NET Core Identity](xref:security/authentication/cookie). I concetti e gli esempi illustrati in questo argomento sono ugualmente applicabili alle App che usano ASP.NET Core Identity. Per usare ASP.NET Core Identity, seguire le indicazioni fornite in <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Richiedere l'autorizzazione per accedere a una pagina

Usare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> alla pagina nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Il percorso specificato è il percorso di motore di visualizzazione, che è il percorso relativo della radice di Razor Pages senza un'estensione e contenente solo barre.

Per specificare una [criteri di autorizzazione](xref:security/authorization/policies), usare un' [overload AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Un' <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> può essere applicato a una classe di modello di pagina con il `[Authorize]` attributo di filtro. Per altre informazioni, vedere [attributo di filtro Authorize](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Richiedere l'autorizzazione per accedere a una cartella delle pagine

Usare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> per tutte le pagine in una cartella nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Il percorso specificato è il percorso di motore di visualizzazione, che è il percorso relativo radice di Razor Pages.

Per specificare una [criteri di autorizzazione](xref:security/authorization/policies), usare un' [overload AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Richiedere l'autorizzazione per accedere a una pagina di area

Usare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> alla pagina area nel percorso specificato:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Il nome della pagina è il percorso del file senza estensione relativo alla directory radice di pagine per l'area specificata. Ad esempio, il nome della pagina per il file *Areas/Identity/Pages/Manage/Accounts.cshtml* viene */Gestisci/account*.

Per specificare una [criteri di autorizzazione](xref:security/authorization/policies), usare un' [overload AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Richiedere l'autorizzazione per accedere a una cartella delle aree

Usare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le aree in una cartella nel percorso specificato:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Il percorso della cartella è il percorso della cartella relativo alla directory radice di pagine per l'area specificata. Ad esempio, il percorso della cartella per i file sotto *aree/identità/pagine/Gestisci/* viene */gestire*.

Per specificare una [criteri di autorizzazione](xref:security/authorization/policies), usare un' [overload AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Consenti accesso anonimo a una pagina

Usare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a una pagina nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Il percorso specificato è il percorso di motore di visualizzazione, che è il percorso relativo della radice di Razor Pages senza un'estensione e contenente solo barre.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Consenti accesso anonimo in una cartella delle pagine

Usare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> per tutte le pagine in una cartella nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Il percorso specificato è il percorso di motore di visualizzazione, che è il percorso relativo radice di Razor Pages.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Nota sulla combinazione autorizzato e l'accesso anonimo

È possibile specificare che una cartella delle pagine che richiedono l'autorizzazione e di specificare che una pagina all'interno della cartella consente l'accesso anonimo:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

L'operazione inversa, tuttavia, non è valido. Non è possibile dichiarare una cartella delle pagine per l'accesso anonimo e quindi specificare una pagina all'interno della cartella che richiede l'autorizzazione:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Autorizzazione che richiede la pagina privato ha esito negativo. Quando sia la <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> e <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> vengono applicate alla pagina, il <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ha la precedenza e controlla l'accesso.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
