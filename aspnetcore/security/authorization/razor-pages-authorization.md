---
title: Convenzioni di autorizzazione Razor Pages in ASP.NET Core
author: guardrex
description: Informazioni su come controllare l'accesso alle pagine con convenzioni che autorizzano gli utenti e consentono agli utenti anonimi di accedere a pagine o cartelle di pagine.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: e0102ff64921a83f0330acb6f5d9bfd90f64ca7a
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994030"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Convenzioni di autorizzazione Razor Pages in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

Un modo per controllare l'accesso all'app Razor Pages consiste nell'usare le convenzioni di autorizzazione all'avvio. Queste convenzioni consentono di autorizzare gli utenti e consentire agli utenti anonimi di accedere a singole pagine o cartelle di pagine. Le convenzioni descritte in questo argomento applicano automaticamente i [filtri di autorizzazione](xref:mvc/controllers/filters#authorization-filters) per controllare l'accesso.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

L'app di esempio usa [l'autenticazione basata su cookie senza ASP.NET Core identità](xref:security/authentication/cookie). I concetti e gli esempi illustrati in questo argomento si applicano ugualmente alle app che usano ASP.NET Core identità. Per usare ASP.NET Core identità, seguire le istruzioni riportate in <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Richiedi autorizzazione per accedere a una pagina

Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> oggetto alla pagina nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice senza un'estensione e contenente solo barre.

Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> oggetto può essere applicato a una classe del modello di `[Authorize]` pagina con l'attributo Filter. Per ulteriori informazioni, vedere [autorizzazione filtro attributo](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Richiedi autorizzazione per accedere a una cartella di pagine

Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le pagine di una cartella nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice.

Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Richiedi autorizzazione per accedere a una pagina di area

Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> alla pagina area nel percorso specificato:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Il nome della pagina è il percorso del file senza estensione rispetto alla directory radice delle pagine per l'area specificata. Ad esempio, il nome della pagina per il file *areas/Identity/Pages/Manage/accounts. cshtml* è */Manage/accounts*.

Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Richiedi autorizzazione per accedere a una cartella di aree

Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le aree di una cartella nel percorso specificato:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Il percorso della cartella è il percorso della cartella relativa alla directory radice delle pagine per l'area specificata. Ad esempio, il percorso della cartella per i file in *areas/Identity/pages/manage/* is */Manage*.

Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Consenti accesso anonimo a una pagina

Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> oggetto a una pagina nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice senza un'estensione e contenente solo barre.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Consenti accesso anonimo a una cartella di pagine

Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a tutte le pagine di una cartella nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Nota sulla combinazione dell'accesso autorizzato e anonimo

È possibile specificare che una cartella di pagine che richiede l'autorizzazione e che specifica che una pagina all'interno di tale cartella consenta l'accesso anonimo:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Il contrario, tuttavia, non è valido. Non è possibile dichiarare una cartella di pagine per l'accesso anonimo e quindi specificare una pagina all'interno della cartella che richiede l'autorizzazione:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

La richiesta di autorizzazione nella pagina privata non riesce. <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> Quando e <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> vengono applicati alla pagina, ha la <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> precedenza e controlla l'accesso.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Un modo per controllare l'accesso all'app Razor Pages consiste nell'usare le convenzioni di autorizzazione all'avvio. Queste convenzioni consentono di autorizzare gli utenti e consentire agli utenti anonimi di accedere a singole pagine o cartelle di pagine. Le convenzioni descritte in questo argomento applicano automaticamente i [filtri di autorizzazione](xref:mvc/controllers/filters#authorization-filters) per controllare l'accesso.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

L'app di esempio usa [l'autenticazione basata su cookie senza ASP.NET Core identità](xref:security/authentication/cookie). I concetti e gli esempi illustrati in questo argomento si applicano ugualmente alle app che usano ASP.NET Core identità. Per usare ASP.NET Core identità, seguire le istruzioni riportate in <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Richiedi autorizzazione per accedere a una pagina

Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> oggetto alla pagina nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice senza un'estensione e contenente solo barre.

Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> oggetto può essere applicato a una classe del modello di `[Authorize]` pagina con l'attributo Filter. Per ulteriori informazioni, vedere [autorizzazione filtro attributo](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Richiedi autorizzazione per accedere a una cartella di pagine

Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le pagine di una cartella nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice.

Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Richiedi autorizzazione per accedere a una pagina di area

Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> alla pagina area nel percorso specificato:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Il nome della pagina è il percorso del file senza estensione rispetto alla directory radice delle pagine per l'area specificata. Ad esempio, il nome della pagina per il file *areas/Identity/Pages/Manage/accounts. cshtml* è */Manage/accounts*.

Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Richiedi autorizzazione per accedere a una cartella di aree

Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> a tutte le aree di una cartella nel percorso specificato:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Il percorso della cartella è il percorso della cartella relativa alla directory radice delle pagine per l'area specificata. Ad esempio, il percorso della cartella per i file in *areas/Identity/pages/manage/* is */Manage*.

Per specificare un [criterio di autorizzazione](xref:security/authorization/policies), usare un [Overload AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Consenti accesso anonimo a una pagina

Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> oggetto a una pagina nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice senza un'estensione e contenente solo barre.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Consenti accesso anonimo a una cartella di pagine

Utilizzare la <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convenzione tramite <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> per aggiungere un <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> a tutte le pagine di una cartella nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo Razor Pages radice.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Nota sulla combinazione dell'accesso autorizzato e anonimo

È possibile specificare che una cartella di pagine che richiede l'autorizzazione e che specifica che una pagina all'interno di tale cartella consenta l'accesso anonimo:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Il contrario, tuttavia, non è valido. Non è possibile dichiarare una cartella di pagine per l'accesso anonimo e quindi specificare una pagina all'interno della cartella che richiede l'autorizzazione:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

La richiesta di autorizzazione nella pagina privata non riesce. <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> Quando e <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> vengono applicati alla pagina, ha la <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> precedenza e controlla l'accesso.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
