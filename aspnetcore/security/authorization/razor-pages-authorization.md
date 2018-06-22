---
title: Convenzioni di autorizzazione di pagine Razor in ASP.NET Core
author: guardrex
description: Informazioni su come controllare l'accesso alle pagine con informazioni sulle convenzioni autorizzare gli utenti e consentire agli utenti anonimi di accedere a pagine o le cartelle di pagine.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 8856520bf43f2f62cc12c7e883485babdb43fb3e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272675"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Convenzioni di autorizzazione di pagine Razor in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

Un modo per controllare l'accesso nell'app pagine Razor consiste nell'utilizzare le convenzioni di autorizzazione all'avvio. Queste convenzioni consentono di autorizzare gli utenti e consentire agli utenti anonimi di accedere a singole pagine o le cartelle di pagine. Applicano le convenzioni descritte in questo argomento automaticamente [filtri di autorizzazione](xref:mvc/controllers/filters#authorization-filters) per controllare l'accesso.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

Nell'app di esempio vengono utilizzate [autenticazione dei Cookie senza ASP.NET Identity Core](xref:security/authentication/cookie). L'account utente per l'utente ipotetica, Maria Rodriguez, è hardcoded nell'app. Utilizzare il nome utente di posta elettronica "maria.rodriguez@contoso.com" e indicare una password di accesso dell'utente. L'utente viene autenticato nel `AuthenticateUser` metodo il *Pages/Account/Login.cshtml.cs* file. In un esempio reale, l'utente viene autenticato un database. Per utilizzare ASP.NET Identity Core, seguire le istruzioni riportate nel [Introduzione all'identità su ASP.NET Core](xref:security/authentication/identity) argomento. I concetti e gli esempi illustrati in questo argomento si applicano anche alle App che usano ASP.NET Identity Core.

## <a name="require-authorization-to-access-a-page"></a>Richiedere l'autorizzazione per accedere a una pagina

Utilizzare il [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo della directory radice di pagine Razor senza un'estensione e contenente solo le barre.

Un [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) è disponibile se è necessario specificare un criterio di autorizzazione.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> Un' `AuthorizeFilter` può essere applicato a una classe di modello di pagina con il `[Authorize]` attributo di filtro. Per altre informazioni, vedere [attributo di filtro Authorize](xref:razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Richiedere l'autorizzazione per accedere alla cartella delle pagine

Utilizzare il [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) per tutte le pagine in una cartella nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Il percorso specificato è il percorso di motore di visualizzazione, ovvero il percorso radice pagine Razor.

Un [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) è disponibile se è necessario specificare un criterio di autorizzazione.

## <a name="allow-anonymous-access-to-a-page"></a>Consenti accesso anonimo a una pagina

Utilizzare il [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) a una pagina nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Il percorso specificato è il percorso del motore di visualizzazione, ovvero il percorso relativo della directory radice di pagine Razor senza un'estensione e contenente solo le barre.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Consenti accesso anonimo in una cartella delle pagine

Utilizzare il [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convenzione tramite [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) per aggiungere un [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) per tutte le pagine in una cartella nel percorso specificato:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Il percorso specificato è il percorso di motore di visualizzazione, ovvero il percorso radice pagine Razor.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Nota sulla combinazione autorizzato e l'accesso anonimo

È perfettamente valido per specificare che una cartella delle pagine richiedono l'autorizzazione e specificare che una pagina all'interno di tale cartella consente l'accesso anonimo:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

L'operazione inversa, tuttavia, non è true. Non è possibile dichiarare una cartella delle pagine per l'accesso anonimo e specificare una pagina all'interno di autorizzazione:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Richiede l'autorizzazione nella pagina privata non funzionerà perché quando sia il `AllowAnonymousFilter` e `AuthorizeFilter` i filtri vengono applicati alla pagina, il `AllowAnonymousFilter` wins e controlla l'accesso.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Route personalizzata di Razor Pages e provider di modelli di pagina](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe
