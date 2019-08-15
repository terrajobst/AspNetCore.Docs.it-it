---
title: Condividere i cookie di autenticazione tra le app ASP.NET
author: rick-anderson
description: Informazioni su come condividere i cookie di autenticazione tra ASP.NET 4. x e le app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: security/cookie-sharing
ms.openlocfilehash: 1650afce5c371d0830bb207618b9c1495f0ce587
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022394"
---
# <a name="share-authentication-cookies-among-aspnet-apps"></a><span data-ttu-id="c8922-103">Condividere i cookie di autenticazione tra le app ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c8922-103">Share authentication cookies among ASP.NET apps</span></span>

<span data-ttu-id="c8922-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c8922-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c8922-105">I siti Web sono spesso costituiti da singole app Web che interagiscono.</span><span class="sxs-lookup"><span data-stu-id="c8922-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="c8922-106">Per offrire un'esperienza Single Sign-on (SSO), le app Web all'interno di un sito devono condividere i cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="c8922-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="c8922-107">Per supportare questo scenario, lo stack di protezione dei dati consente la condivisione dell'autenticazione del cookie Katana e dei ticket di autenticazione ASP.NET Core cookie.</span><span class="sxs-lookup"><span data-stu-id="c8922-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="c8922-108">Negli esempi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8922-108">In the examples that follow:</span></span>

* <span data-ttu-id="c8922-109">Il nome del cookie di autenticazione è impostato su un valore `.AspNet.SharedCookie`comune di.</span><span class="sxs-lookup"><span data-stu-id="c8922-109">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="c8922-110">L' `AuthenticationType` oggetto è impostato `Identity.Application` su in modo esplicito o per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c8922-110">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="c8922-111">Un nome di app comune viene usato per consentire al sistema di protezione dei dati di condividere le`SharedCookieApp`chiavi di protezione dei dati ().</span><span class="sxs-lookup"><span data-stu-id="c8922-111">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="c8922-112">`Identity.Application`viene usato come schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="c8922-112">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="c8922-113">Indipendentemente dallo schema usato, è necessario usarlo *in modo coerente all'interno e* nelle app di cookie condivise come schema predefinito o impostarlo in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="c8922-113">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="c8922-114">Lo schema viene usato durante la crittografia e la decrittografia dei cookie, quindi è necessario usare uno schema coerente tra le app.</span><span class="sxs-lookup"><span data-stu-id="c8922-114">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="c8922-115">Viene utilizzata una posizione di archiviazione della [chiave di protezione dati](xref:security/data-protection/implementation/key-management) comune.</span><span class="sxs-lookup"><span data-stu-id="c8922-115">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span>
  * <span data-ttu-id="c8922-116">In ASP.NET Core Apps <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> viene usato per impostare il percorso di archiviazione delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="c8922-116">In ASP.NET Core apps, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> is used to set the key storage location.</span></span>
  * <span data-ttu-id="c8922-117">Nelle app .NET Framework il middleware di autenticazione dei cookie usa un' <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>implementazione di.</span><span class="sxs-lookup"><span data-stu-id="c8922-117">In .NET Framework apps, Cookie Authentication Middleware uses an implementation of <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span></span> <span data-ttu-id="c8922-118">`DataProtectionProvider`fornisce servizi di protezione dei dati per la crittografia e la decrittografia dei dati di payload del cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="c8922-118">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="c8922-119">L' `DataProtectionProvider` istanza è isolata dal sistema di protezione dei dati usato da altre parti dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8922-119">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span> <span data-ttu-id="c8922-120">[DataProtectionProvider. Create (System. io. DirectoryInfo, Action\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accetta un <xref:System.IO.DirectoryInfo> oggetto per specificare il percorso per l'archiviazione delle chiavi di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="c8922-120">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accepts a <xref:System.IO.DirectoryInfo> to specify the location for data protection key storage.</span></span>
* <span data-ttu-id="c8922-121">`DataProtectionProvider`richiede il pacchetto NuGet [Microsoft. AspNetCore. dataprotection. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) :</span><span class="sxs-lookup"><span data-stu-id="c8922-121">`DataProtectionProvider` requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package:</span></span>
  * <span data-ttu-id="c8922-122">Nelle app ASP.NET Core 2. x fare riferimento al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c8922-122">In ASP.NET Core 2.x apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="c8922-123">In .NET Framework app aggiungere un riferimento al pacchetto a [Microsoft. AspNetCore. dataprotection. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span><span class="sxs-lookup"><span data-stu-id="c8922-123">In .NET Framework apps, add a package reference to [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span></span>
* <span data-ttu-id="c8922-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*>imposta il nome comune dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8922-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> sets the common app name.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="c8922-125">Condividere i cookie di autenticazione tra ASP.NET Core app</span><span class="sxs-lookup"><span data-stu-id="c8922-125">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="c8922-126">Quando si usa ASP.NET Core identità:</span><span class="sxs-lookup"><span data-stu-id="c8922-126">When using ASP.NET Core Identity:</span></span>

* <span data-ttu-id="c8922-127">Le chiavi di protezione dei dati e il nome dell'app devono essere condivise tra le app.</span><span class="sxs-lookup"><span data-stu-id="c8922-127">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="c8922-128">Un percorso di archiviazione delle <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> chiavi comune viene fornito al metodo negli esempi seguenti.</span><span class="sxs-lookup"><span data-stu-id="c8922-128">A common key storage location is provided to the <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> method in the following examples.</span></span> <span data-ttu-id="c8922-129">Usare <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> per configurare un nome comune per l'app`SharedCookieApp` condivisa (negli esempi seguenti).</span><span class="sxs-lookup"><span data-stu-id="c8922-129">Use <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> to configure a common shared app name (`SharedCookieApp` in the following examples).</span></span> <span data-ttu-id="c8922-130">Per altre informazioni, vedere <xref:security/data-protection/configuration/overview>.</span><span class="sxs-lookup"><span data-stu-id="c8922-130">For more information, see <xref:security/data-protection/configuration/overview>.</span></span>
* <span data-ttu-id="c8922-131">Usare il <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> metodo di estensione per configurare il servizio di protezione dei dati per i cookie.</span><span class="sxs-lookup"><span data-stu-id="c8922-131">Use the <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> extension method to set up the data protection service for cookies.</span></span>
* <span data-ttu-id="c8922-132">Il tipo di autenticazione predefinito `Identity.Application`è.</span><span class="sxs-lookup"><span data-stu-id="c8922-132">The default authentication type is `Identity.Application`.</span></span>

<span data-ttu-id="c8922-133">In `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c8922-133">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

<span data-ttu-id="c8922-134">Quando si usano i cookie direttamente senza ASP.NET Core identità, configurare la protezione dei `Startup.ConfigureServices`dati e l'autenticazione in.</span><span class="sxs-lookup"><span data-stu-id="c8922-134">When using cookies directly without ASP.NET Core Identity, configure data protection and authentication in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c8922-135">Nell'esempio seguente, il tipo di autenticazione è impostato su `Identity.Application`:</span><span class="sxs-lookup"><span data-stu-id="c8922-135">In the following example, the authentication type is set to `Identity.Application`:</span></span>

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

<span data-ttu-id="c8922-136">Quando si ospitano app che condividono cookie tra sottodomini, specificare un dominio comune nella proprietà [cookie. Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) .</span><span class="sxs-lookup"><span data-stu-id="c8922-136">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) property.</span></span> <span data-ttu-id="c8922-137">Per condividere i cookie tra le `contoso.com` `first_subdomain.contoso.com` `second_subdomain.contoso.com` `Cookie.Domain` app in, ad esempio e, specificare `.contoso.com`come:</span><span class="sxs-lookup"><span data-stu-id="c8922-137">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a><span data-ttu-id="c8922-138">Crittografare le chiavi di protezione dei dati inattivi</span><span class="sxs-lookup"><span data-stu-id="c8922-138">Encrypt data protection keys at rest</span></span>

<span data-ttu-id="c8922-139">Per le distribuzioni di produzione, `DataProtectionProvider` configurare per crittografare le chiavi inattive con DPAPI o un X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="c8922-139">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="c8922-140">Per altre informazioni, vedere <xref:security/data-protection/implementation/key-encryption-at-rest>.</span><span class="sxs-lookup"><span data-stu-id="c8922-140">For more information, see <xref:security/data-protection/implementation/key-encryption-at-rest>.</span></span> <span data-ttu-id="c8922-141">Nell'esempio seguente viene fornita un'identificazione personale del certificato per <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span><span class="sxs-lookup"><span data-stu-id="c8922-141">In the following example, a certificate thumbprint is provided to <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span></span>

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="c8922-142">Condividere i cookie di autenticazione tra ASP.NET 4. x e app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8922-142">Share authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="c8922-143">Le app ASP.NET 4. x che usano il middleware di autenticazione dei cookie Katana possono essere configurate per generare cookie di autenticazione compatibili con il middleware di autenticazione ASP.NET Core cookie.</span><span class="sxs-lookup"><span data-stu-id="c8922-143">ASP.NET 4.x apps that use Katana Cookie Authentication Middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core Cookie Authentication Middleware.</span></span> <span data-ttu-id="c8922-144">In questo modo è possibile aggiornare le singole app di un sito di grandi dimensioni in diversi passaggi garantendo al tempo stesso un'esperienza di accesso SSO uniforme nel sito.</span><span class="sxs-lookup"><span data-stu-id="c8922-144">This allows upgrading a large site's individual apps in several steps while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="c8922-145">Quando un'app usa il middleware di autenticazione dei cookie Katana `UseCookieAuthentication` , chiama nel file *Startup.auth.cs* del progetto.</span><span class="sxs-lookup"><span data-stu-id="c8922-145">When an app uses Katana Cookie Authentication Middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="c8922-146">I progetti di app Web ASP.NET 4. x creati con Visual Studio 2013 e versioni successive usano il middleware di autenticazione dei cookie katana per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c8922-146">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana Cookie Authentication Middleware by default.</span></span> <span data-ttu-id="c8922-147">Anche `UseCookieAuthentication` se è obsoleto e non supportato per le app ASP.NET Core `UseCookieAuthentication` , la chiamata in un'app ASP.NET 4. x che usa il middleware di autenticazione dei cookie Katana è valida.</span><span class="sxs-lookup"><span data-stu-id="c8922-147">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana Cookie Authentication Middleware is valid.</span></span>

<span data-ttu-id="c8922-148">Un'app ASP.NET 4. x deve avere come destinazione .NET Framework 4.5.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="c8922-148">An ASP.NET 4.x app must target .NET Framework 4.5.1 or later.</span></span> <span data-ttu-id="c8922-149">In caso contrario, non è possibile installare i pacchetti NuGet necessari.</span><span class="sxs-lookup"><span data-stu-id="c8922-149">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="c8922-150">Per condividere i cookie di autenticazione tra un'app ASP.NET 4. x e un'app ASP.NET Core, configurare l'app ASP.NET Core come indicato nella sezione [condividere i cookie di autenticazione tra ASP.NET Core app](#share-authentication-cookies-among-aspnet-core-apps) , quindi configurare l'app ASP.NET 4. x come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="c8922-150">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated in the [Share authentication cookies among ASP.NET Core apps](#share-authentication-cookies-among-aspnet-core-apps) section, then configure the ASP.NET 4.x app as follows.</span></span>

<span data-ttu-id="c8922-151">Verificare che i pacchetti dell'app vengano aggiornati alle versioni più recenti.</span><span class="sxs-lookup"><span data-stu-id="c8922-151">Confirm that the app's packages are updated to the latest releases.</span></span> <span data-ttu-id="c8922-152">Installare il pacchetto [Microsoft. Owin. Security. Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) in ogni app ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="c8922-152">Install the [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package into each ASP.NET 4.x app.</span></span>

<span data-ttu-id="c8922-153">Individuare e modificare la chiamata a `UseCookieAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="c8922-153">Locate and modify the call to `UseCookieAuthentication`:</span></span>

* <span data-ttu-id="c8922-154">Modificare il nome del cookie in modo che corrisponda al nome usato dal middleware di`.AspNet.SharedCookie` autenticazione del cookie ASP.NET Core (nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="c8922-154">Change the cookie name to match the name used by the ASP.NET Core Cookie Authentication Middleware (`.AspNet.SharedCookie` in the example).</span></span>
* <span data-ttu-id="c8922-155">Nell'esempio seguente, il tipo di autenticazione è impostato su `Identity.Application`.</span><span class="sxs-lookup"><span data-stu-id="c8922-155">In the following example, the authentication type is set to `Identity.Application`.</span></span>
* <span data-ttu-id="c8922-156">Fornire un'istanza di un `DataProtectionProvider` oggetto inizializzato al percorso di archiviazione della chiave di protezione dati comune.</span><span class="sxs-lookup"><span data-stu-id="c8922-156">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>
* <span data-ttu-id="c8922-157">Verificare che il nome dell'app sia impostato sul nome dell'app comune usato da tutte le app che condividono i`SharedCookieApp` cookie di autenticazione (nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="c8922-157">Confirm that the app name is set to the common app name used by all apps that share authentication cookies (`SharedCookieApp` in the example).</span></span>

<span data-ttu-id="c8922-158">Se non si `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` imposta `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`e, <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> impostare su un'attestazione che distingue gli utenti univoci.</span><span class="sxs-lookup"><span data-stu-id="c8922-158">If not setting `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` and `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, set <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> to a claim that distinguishes unique users.</span></span>

<span data-ttu-id="c8922-159">*App_start/Startup. auth. cs*:</span><span class="sxs-lookup"><span data-stu-id="c8922-159">*App_Start/Startup.Auth.cs*:</span></span>

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

<span data-ttu-id="c8922-160">Quando si genera un'identità utente, il tipo di`Identity.Application`autenticazione () deve corrispondere al tipo `AuthenticationType` definito in `UseCookieAuthentication` set with in *app_start/Startup. auth. cs*.</span><span class="sxs-lookup"><span data-stu-id="c8922-160">When generating a user identity, the authentication type (`Identity.Application`) must match the type defined in `AuthenticationType` set with `UseCookieAuthentication` in *App_Start/Startup.Auth.cs*.</span></span>

<span data-ttu-id="c8922-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="c8922-161">*Models/IdentityModels.cs*:</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="c8922-162">Usare un database utente comune</span><span class="sxs-lookup"><span data-stu-id="c8922-162">Use a common user database</span></span>

<span data-ttu-id="c8922-163">Quando le app usano lo stesso schema di identità (stessa versione di Identity), verificare che il sistema di identità per ogni app punti allo stesso database utente.</span><span class="sxs-lookup"><span data-stu-id="c8922-163">When apps use the same Identity schema (same version of Identity), confirm that the Identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="c8922-164">In caso contrario, il sistema di identità genera errori in fase di esecuzione quando tenta di trovare una corrispondenza tra le informazioni nel cookie di autenticazione e le informazioni contenute nel relativo database.</span><span class="sxs-lookup"><span data-stu-id="c8922-164">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

<span data-ttu-id="c8922-165">Quando lo schema di identità è diverso tra le app, in genere perché le app usano versioni diverse dell'identità, la condivisione di un database comune in base alla versione più recente dell'identità non è possibile senza rimappare e aggiungere colonne negli schemi di identità di altri app.</span><span class="sxs-lookup"><span data-stu-id="c8922-165">When the Identity schema is different among apps, usually because apps are using different Identity versions, sharing a common database based on the latest version of Identity isn't possible without remapping and adding columns in other app's Identity schemas.</span></span> <span data-ttu-id="c8922-166">È spesso più efficiente aggiornare le altre app per usare la versione più recente dell'identità, in modo che un database comune possa essere condiviso dalle app.</span><span class="sxs-lookup"><span data-stu-id="c8922-166">It's often more efficient to upgrade the other apps to use the latest Identity version so that a common database can be shared by the apps.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8922-167">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c8922-167">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
