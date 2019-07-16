---
title: Condividere i cookie di autenticazione tra le app ASP.NET
author: rick-anderson
description: Informazioni su come condividere i cookie di autenticazione tra ASP.NET 4.x e le app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/15/2019
uid: security/cookie-sharing
ms.openlocfilehash: b2f906ac97fe79b2a66a5ab709bcbcb03ab8cc39
ms.sourcegitcommit: 1bf80f4acd62151ff8cce517f03f6fa891136409
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223919"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a>Condividere i cookie di autenticazione tra le app ASP.NET

Dal [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)

Siti Web è spesso costituita da singole App web che interagiscono. Per offrire un'esperienza single sign-on (SSO), App web all'interno di un sito devono condividere i cookie di autenticazione. Per supportare questo scenario, lo stack di protezione dati consente di condividere l'autenticazione tramite cookie Katana e i ticket di autenticazione di cookie di ASP.NET Core.

Negli esempi che seguono:

* Il nome del cookie di autenticazione è impostata su un valore comune di `.AspNet.SharedCookie`.
* Il `AuthenticationType` è impostata su `Identity.Application` in modo esplicito o per impostazione predefinita.
* Un nome comune dell'app viene usato per abilitare il sistema di protezione dati condividere le chiavi di protezione dati (`SharedCookieApp`).
* `Identity.Application` viene usato come lo schema di autenticazione. Qualsiasi schema viene usato, sarà necessario utilizzarlo in modo coerente *all'interno e tra* le app condivise cookie come lo schema predefinito o impostandolo in modo esplicito. Lo schema viene utilizzato quando crittografare e decrittografare i cookie, pertanto è necessario utilizzare uno schema coerente tra le app.
* Un comune [chiave di protezione dei dati](xref:security/data-protection/implementation/key-management) viene usato il percorso di archiviazione.
  * Nelle App ASP.NET Core, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> consente di impostare il percorso di archiviazione delle chiavi.
  * In .NET Framework applicazioni, Middleware di autenticazione del Cookie Usa un'implementazione di <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>. `DataProtectionProvider` fornisce servizi di protezione dati per la crittografia e decrittografia dei dati di payload di cookie di autenticazione. Il `DataProtectionProvider` istanza è isolata dal sistema di protezione di dati usato da altre parti dell'app. [DataProtectionProvider.Create (azione, DirectoryInfo\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accetta una <xref:System.IO.DirectoryInfo> per specificare il percorso di archiviazione chiavi di protezione dati.
* `DataProtectionProvider` richiede la [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pacchetto NuGet:
  * Nelle App ASP.NET Core 2.x, fare riferimento il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
  * Nelle app .NET Framework, aggiungere un riferimento al pacchetto [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).
* <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> Imposta il nome dell'app più comuni.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Condivisione di cookie di autenticazione tra le app ASP.NET Core

Quando si usa ASP.NET Core Identity:

* Chiavi di protezione dati e il nome dell'app deve essere condivisa tra le app. Viene fornito un percorso di archiviazione chiavi comuni per la <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> metodo negli esempi seguenti. Uso <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> per configurare un nome di app condivisa comune (`SharedCookieApp` negli esempi seguenti). Per altre informazioni, vedere <xref:security/data-protection/configuration/overview>.
* Usare il <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> metodo di estensione per configurare il servizio di protezione dati per i cookie.
* Nell'esempio seguente, il tipo di autenticazione è impostato su `Identity.Application` per impostazione predefinita.

In `Startup.ConfigureServices`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

Quando si usano i cookie direttamente senza ASP.NET Core Identity, configurare la protezione dei dati e l'autenticazione in `Startup.ConfigureServices`. Nell'esempio seguente, il tipo di autenticazione è impostato su `Identity.Application`:

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.AddAuthentication("Identity.Application")
    .AddCookie("Identity.Application", options =>
    {
        options.Cookie.Name = ".AspNet.SharedCookie";
    });
```

Quando si ospitano le app che condividono i cookie in sottodomini, specificare un dominio in comune il [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) proprietà. Per condividere i cookie tra App su `contoso.com`, ad esempio `first_subdomain.contoso.com` e `second_subdomain.contoso.com`, specificare il `Cookie.Domain` come `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a>Crittografare le chiavi di protezione dei dati inattivi

Per le distribuzioni di produzione, configurare il `DataProtectionProvider` per crittografare le chiavi a riposo con DPAPI o un X509Certificate. Per altre informazioni, vedere <xref:security/data-protection/implementation/key-encryption-at-rest>. Nell'esempio seguente, un'identificazione personale del certificato viene fornito per <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Condividere i cookie di autenticazione tra ASP.NET 4.x e le app ASP.NET Core

App ASP.NET 4.x che usano il Middleware di autenticazione di Katana Cookie può essere configurata per generare i cookie di autenticazione che sono compatibili con il Middleware di autenticazione Cookie di ASP.NET Core. In questo modo l'aggiornamento di App di singoli di un sito di grandi dimensioni in diversi passaggi, fornendo un'esperienza SSO ottimale tra il sito.

Quando un'app Usa il Middleware di autenticazione Cookie Katana, chiama `UseCookieAuthentication` del progetto *Startup.Auth.cs* file. Progetti di app web ASP.NET 4.x creati con Visual Studio 2013 e versioni successive usano il Middleware di autenticazione del Cookie Katana per impostazione predefinita. Sebbene `UseCookieAuthentication` è obsoleto e non supportati per le app ASP.NET Core, la chiamata `UseCookieAuthentication` in ASP.NET 4.x app che usa il Middleware di autenticazione di Katana Cookie è valido.

Un'app ASP.NET 4.x deve avere come destinazione .NET Framework 4.5.1 o versioni successive. In caso contrario, i pacchetti NuGet necessari esito negativo per l'installazione.

Per condividere i cookie di autenticazione tra un'app ASP.NET 4.x e un'app ASP.NET Core, configurare l'app ASP.NET Core come indicato nella [condivisione di cookie di autenticazione tra le app ASP.NET Core](#share-authentication-cookies-among-aspnet-core-apps) sezione e quindi configurare l'app ASP.NET 4.x come segue.

Verificare che i pacchetti dell'app vengono aggiornati a versioni più recenti. Installare il [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package in ogni app ASP.NET 4.x.

Individuare e modificare la chiamata a `UseCookieAuthentication`:

* Modificare il nome del cookie per corrispondere al nome usato per il Middleware di autenticazione Cookie di ASP.NET Core (`.AspNet.SharedCookie` nell'esempio).
* Nell'esempio seguente, il tipo di autenticazione è impostato su `Identity.Application`.
* Fornire un'istanza di un `DataProtectionProvider` inizializzati per il percorso di archiviazione delle chiavi di protezione dati comune.
* Verificare che il nome dell'app è impostato per il nome dell'app più comuni usato da tutte le app che condividono i cookie di autenticazione (`SharedCookieApp` nell'esempio).

Se non si imposta `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` e `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, impostare <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> per un'attestazione che consente di distinguere gli utenti univoci.

*App_Start/Startup.Auth.cs*:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = "Identity.Application",
    CookieName = ".AspNet.SharedCookie",
    LoginPath = new PathString("/Account/Login"),
    Provider = new CookieAuthenticationProvider
    {
        OnValidateIdentity =
            SecurityStampValidator
                .OnValidateIdentity<ApplicationUserManager, ApplicationUser>(
                    validateInterval: TimeSpan.FromMinutes(30),
                    regenerateIdentity: (manager, user) =>
                        user.GenerateUserIdentityAsync(manager))
    },
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create({PATH TO COMMON KEY RING FOLDER},
                (builder) => { builder.SetApplicationName("SharedCookieApp"); })
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies." +
                    "CookieAuthenticationMiddleware",
                "Identity.Application",
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});

System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier =
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name";
```

Durante la generazione di un'identità utente, il tipo di autenticazione (`Identity.Application`) deve corrispondere al tipo definito in `AuthenticationType` impostati con `UseCookieAuthentication` nelle *App_Start/Startup.Auth.cs*.

*Models/IdentityModels.cs*:

```csharp
public class ApplicationUser : IdentityUser
{
    public async Task<ClaimsIdentity> GenerateUserIdentityAsync(
        UserManager<ApplicationUser> manager)
    {
        // The authenticationType must match the one defined in 
        // CookieAuthenticationOptions.AuthenticationType
        var userIdentity = 
            await manager.CreateIdentityAsync(this, "Identity.Application");

        // Add custom user claims here

        return userIdentity;
    }
}
```

## <a name="use-a-common-user-database"></a>Usare un database utente comuni

Quando le app usano la stessa identità dello schema (stessa versione dell'identità), verificare che il sistema di identità per ogni app fa riferimento a livello di database utente stesso. In caso contrario, il sistema di identità genera errori in fase di esecuzione quando tenta di ottenere le informazioni nel cookie di autenticazione con le informazioni nel proprio database.

Quando lo schema di identità è diverso tra le app, in genere perché le app usino versioni diverse di identità, la condivisione di un database comune in base alla versione più recente dell'identità non è possibile senza modifica del mapping e l'aggiunta di colonne negli schemi di identità dell'app con altri. È spesso più efficiente aggiornare le altre App per usare la versione più recente di identità in modo che un database comune può essere condiviso dalle app.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:host-and-deploy/web-farm>
