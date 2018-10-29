---
title: Convenzioni di autorizzazione di Razor Pages in ASP.NET Core
author: guardrex
description: Informazioni su come controllare l'accesso alle pagine con le convenzioni di autorizzano gli utenti e consentono agli utenti anonimi di accedere alle pagine o le cartelle di pagine.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 675dc8aa4bf00bb21981cc892a09a4acd0d53c15
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207264"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Convenzioni di autorizzazione di Razor Pages in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Un modo per controllare l'accesso nell'app Razor Pages è usare convenzioni di autorizzazione all'avvio. Queste convenzioni consentono di autorizzare gli utenti e consentire agli utenti anonimi di accedere alle cartelle di pagine o le singole pagine. Applicano le convenzioni descritte in questo argomento automaticamente [filtri di autorizzazione](xref:mvc/controllers/filters#authorization-filters) per controllare l'accesso.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

L'app di esempio Usa [l'autenticazione tramite Cookie senza ASP.NET Core Identity](xref:security/authentication/cookie). L'account utente per l'utente ipotetica, Maria Rodriguez, è hardcoded nell'app. Usare il nome utente di posta elettronica "maria.rodriguez@contoso.com" e qualsiasi password di accesso dell'utente. L'utente viene autenticato nel `AuthenticateUser` metodo nella *Pages/Account/Login.cshtml.cs* file. In un esempio reale, l'utente viene autenticato in un database. Per usare ASP.NET Core Identity, seguire le indicazioni fornite nel [Introduzione all'identità in ASP.NET Core](xref:security/authentication/identity) argomento. I concetti e gli esempi illustrati in questo argomento sono ugualmente applicabili alle App che usano ASP.NET Core Identity.

## <a name="require-authorization-to-access-a-page"></a>Richiedere l'autorizzazione per accedere a una pagina

Usare la [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un' [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Il percorso specificato è il percorso di motore di visualizzazione, che è il percorso relativo della radice di Razor Pages senza un'estensione e contenente solo barre.

Un' [overload AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) è disponibile se è necessario specificare un criterio di autorizzazione.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> Un' `AuthorizeFilter` può essere applicato a una classe di modello di pagina con il `[Authorize]` attributo di filtro. Per altre informazioni, vedere [attributo di filtro Authorize](xref:razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Richiedere l'autorizzazione per accedere a una cartella delle pagine

Usare la [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un' [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) per tutte le pagine in una cartella nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Il percorso specificato è il percorso di motore di visualizzazione, che è il percorso relativo radice di Razor Pages.

Un' [overload AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) è disponibile se è necessario specificare un criterio di autorizzazione.

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a>Richiedere l'autorizzazione per accedere a una pagina di area

Usare la [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un' [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina area nel percorso specificato:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Il nome della pagina è il percorso del file senza estensione relativo alla directory radice di pagine per l'area specificata. Ad esempio, il nome della pagina per il file *Areas/Identity/Pages/Manage/Accounts.cshtml* viene */Gestisci/account*.

Un' [overload AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) è disponibile se è necessario specificare un criterio di autorizzazione.

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Richiedere l'autorizzazione per accedere a una cartella delle aree

Usare la [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un' [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) a tutte le aree in una cartella nel percorso specificato:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Il percorso della cartella è il percorso della cartella relativo alla directory radice di pagine per l'area specificata. Ad esempio, il percorso della cartella per i file sotto *aree/identità/pagine/Gestisci/* viene */gestire*.

Un' [overload AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) è disponibile se è necessario specificare un criterio di autorizzazione.

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a>Consenti accesso anonimo a una pagina

Usare la [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un' [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una pagina nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Il percorso specificato è il percorso di motore di visualizzazione, che è il percorso relativo della radice di Razor Pages senza un'estensione e contenente solo barre.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Consenti accesso anonimo in una cartella delle pagine

Usare la [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un' [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) per tutte le pagine in una cartella nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Il percorso specificato è il percorso di motore di visualizzazione, che è il percorso relativo radice di Razor Pages.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Nota sulla combinazione autorizzato e l'accesso anonimo

È perfettamente valido per specificare che una cartella delle pagine richiedono l'autorizzazione e specificare che una pagina all'interno della cartella consente l'accesso anonimo:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Tuttavia, non viceversa. Non è possibile dichiarare una cartella delle pagine per l'accesso anonimo e specificare una pagina all'interno di autorizzazione:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Richiesta di autorizzazione nella pagina privata non funzionerà perché quando sia la `AllowAnonymousFilter` e `AuthorizeFilter` i filtri vengono applicati alla pagina, il `AllowAnonymousFilter` wins e controlla l'accesso.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Route personalizzata di Razor Pages e provider di modelli di pagina](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe
