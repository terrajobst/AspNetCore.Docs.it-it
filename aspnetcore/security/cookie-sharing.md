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
# <a name="share-authentication-cookies-among-aspnet-apps"></a><span data-ttu-id="e3636-103">Condividere i cookie di autenticazione tra le app ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e3636-103">Share authentication cookies among ASP.NET apps</span></span>

<span data-ttu-id="e3636-104">Dal [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e3636-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e3636-105">Siti Web è spesso costituita da singole App web che interagiscono.</span><span class="sxs-lookup"><span data-stu-id="e3636-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="e3636-106">Per offrire un'esperienza single sign-on (SSO), App web all'interno di un sito devono condividere i cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e3636-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="e3636-107">Per supportare questo scenario, lo stack di protezione dati consente di condividere l'autenticazione tramite cookie Katana e i ticket di autenticazione di cookie di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e3636-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="e3636-108">Negli esempi che seguono:</span><span class="sxs-lookup"><span data-stu-id="e3636-108">In the examples that follow:</span></span>

* <span data-ttu-id="e3636-109">Il nome del cookie di autenticazione è impostata su un valore comune di `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="e3636-109">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="e3636-110">Il `AuthenticationType` è impostata su `Identity.Application` in modo esplicito o per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e3636-110">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="e3636-111">Un nome comune dell'app viene usato per abilitare il sistema di protezione dati condividere le chiavi di protezione dati (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="e3636-111">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="e3636-112">`Identity.Application` viene usato come lo schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e3636-112">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="e3636-113">Qualsiasi schema viene usato, sarà necessario utilizzarlo in modo coerente *all'interno e tra* le app condivise cookie come lo schema predefinito o impostandolo in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="e3636-113">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="e3636-114">Lo schema viene utilizzato quando crittografare e decrittografare i cookie, pertanto è necessario utilizzare uno schema coerente tra le app.</span><span class="sxs-lookup"><span data-stu-id="e3636-114">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="e3636-115">Un comune [chiave di protezione dei dati](xref:security/data-protection/implementation/key-management) viene usato il percorso di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="e3636-115">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span>
  * <span data-ttu-id="e3636-116">Nelle App ASP.NET Core, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> consente di impostare il percorso di archiviazione delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="e3636-116">In ASP.NET Core apps, <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> is used to set the key storage location.</span></span>
  * <span data-ttu-id="e3636-117">In .NET Framework applicazioni, Middleware di autenticazione del Cookie Usa un'implementazione di <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span><span class="sxs-lookup"><span data-stu-id="e3636-117">In .NET Framework apps, Cookie Authentication Middleware uses an implementation of <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>.</span></span> <span data-ttu-id="e3636-118">`DataProtectionProvider` fornisce servizi di protezione dati per la crittografia e decrittografia dei dati di payload di cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e3636-118">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="e3636-119">Il `DataProtectionProvider` istanza è isolata dal sistema di protezione di dati usato da altre parti dell'app.</span><span class="sxs-lookup"><span data-stu-id="e3636-119">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span> <span data-ttu-id="e3636-120">[DataProtectionProvider.Create (azione, DirectoryInfo\<IDataProtectionBuilder >)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accetta una <xref:System.IO.DirectoryInfo> per specificare il percorso di archiviazione chiavi di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="e3636-120">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider.Create*) accepts a <xref:System.IO.DirectoryInfo> to specify the location for data protection key storage.</span></span>
* <span data-ttu-id="e3636-121">`DataProtectionProvider` richiede la [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pacchetto NuGet:</span><span class="sxs-lookup"><span data-stu-id="e3636-121">`DataProtectionProvider` requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package:</span></span>
  * <span data-ttu-id="e3636-122">Nelle App ASP.NET Core 2.x, fare riferimento il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e3636-122">In ASP.NET Core 2.x apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="e3636-123">Nelle app .NET Framework, aggiungere un riferimento al pacchetto [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span><span class="sxs-lookup"><span data-stu-id="e3636-123">In .NET Framework apps, add a package reference to [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/).</span></span>
* <span data-ttu-id="e3636-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> Imposta il nome dell'app più comuni.</span><span class="sxs-lookup"><span data-stu-id="e3636-124"><xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> sets the common app name.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="e3636-125">Condivisione di cookie di autenticazione tra le app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3636-125">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="e3636-126">Quando si usa ASP.NET Core Identity:</span><span class="sxs-lookup"><span data-stu-id="e3636-126">When using ASP.NET Core Identity:</span></span>

* <span data-ttu-id="e3636-127">Chiavi di protezione dati e il nome dell'app deve essere condivisa tra le app.</span><span class="sxs-lookup"><span data-stu-id="e3636-127">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="e3636-128">Viene fornito un percorso di archiviazione chiavi comuni per la <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> metodo negli esempi seguenti.</span><span class="sxs-lookup"><span data-stu-id="e3636-128">A common key storage location is provided to the <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.PersistKeysToFileSystem*> method in the following examples.</span></span> <span data-ttu-id="e3636-129">Uso <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> per configurare un nome di app condivisa comune (`SharedCookieApp` negli esempi seguenti).</span><span class="sxs-lookup"><span data-stu-id="e3636-129">Use <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> to configure a common shared app name (`SharedCookieApp` in the following examples).</span></span> <span data-ttu-id="e3636-130">Per altre informazioni, vedere <xref:security/data-protection/configuration/overview>.</span><span class="sxs-lookup"><span data-stu-id="e3636-130">For more information, see <xref:security/data-protection/configuration/overview>.</span></span>
* <span data-ttu-id="e3636-131">Usare il <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> metodo di estensione per configurare il servizio di protezione dati per i cookie.</span><span class="sxs-lookup"><span data-stu-id="e3636-131">Use the <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.ConfigureApplicationCookie*> extension method to set up the data protection service for cookies.</span></span>
* <span data-ttu-id="e3636-132">Nell'esempio seguente, il tipo di autenticazione è impostato su `Identity.Application` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e3636-132">In the following example, the authentication type is set to `Identity.Application` by default.</span></span>

<span data-ttu-id="e3636-133">In `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e3636-133">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddDataProtection()
    .PersistKeysToFileSystem({PATH TO COMMON KEY RING FOLDER})
    .SetApplicationName("SharedCookieApp");

services.ConfigureApplicationCookie(options => {
    options.Cookie.Name = ".AspNet.SharedCookie";
});
```

<span data-ttu-id="e3636-134">Quando si usano i cookie direttamente senza ASP.NET Core Identity, configurare la protezione dei dati e l'autenticazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e3636-134">When using cookies directly without ASP.NET Core Identity, configure data protection and authentication in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="e3636-135">Nell'esempio seguente, il tipo di autenticazione è impostato su `Identity.Application`:</span><span class="sxs-lookup"><span data-stu-id="e3636-135">In the following example, the authentication type is set to `Identity.Application`:</span></span>

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

<span data-ttu-id="e3636-136">Quando si ospitano le app che condividono i cookie in sottodomini, specificare un dominio in comune il [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) proprietà.</span><span class="sxs-lookup"><span data-stu-id="e3636-136">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](xref:Microsoft.AspNetCore.Http.CookieBuilder.Domain) property.</span></span> <span data-ttu-id="e3636-137">Per condividere i cookie tra App su `contoso.com`, ad esempio `first_subdomain.contoso.com` e `second_subdomain.contoso.com`, specificare il `Cookie.Domain` come `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="e3636-137">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

## <a name="encrypt-data-protection-keys-at-rest"></a><span data-ttu-id="e3636-138">Crittografare le chiavi di protezione dei dati inattivi</span><span class="sxs-lookup"><span data-stu-id="e3636-138">Encrypt data protection keys at rest</span></span>

<span data-ttu-id="e3636-139">Per le distribuzioni di produzione, configurare il `DataProtectionProvider` per crittografare le chiavi a riposo con DPAPI o un X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="e3636-139">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="e3636-140">Per altre informazioni, vedere <xref:security/data-protection/implementation/key-encryption-at-rest>.</span><span class="sxs-lookup"><span data-stu-id="e3636-140">For more information, see <xref:security/data-protection/implementation/key-encryption-at-rest>.</span></span> <span data-ttu-id="e3636-141">Nell'esempio seguente, un'identificazione personale del certificato viene fornito per <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span><span class="sxs-lookup"><span data-stu-id="e3636-141">In the following example, a certificate thumbprint is provided to <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.ProtectKeysWithCertificate*>:</span></span>

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("{CERTIFICATE THUMBPRINT}");
```

## <a name="share-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="e3636-142">Condividere i cookie di autenticazione tra ASP.NET 4.x e le app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3636-142">Share authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="e3636-143">App ASP.NET 4.x che usano il Middleware di autenticazione di Katana Cookie può essere configurata per generare i cookie di autenticazione che sono compatibili con il Middleware di autenticazione Cookie di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e3636-143">ASP.NET 4.x apps that use Katana Cookie Authentication Middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core Cookie Authentication Middleware.</span></span> <span data-ttu-id="e3636-144">In questo modo l'aggiornamento di App di singoli di un sito di grandi dimensioni in diversi passaggi, fornendo un'esperienza SSO ottimale tra il sito.</span><span class="sxs-lookup"><span data-stu-id="e3636-144">This allows upgrading a large site's individual apps in several steps while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="e3636-145">Quando un'app Usa il Middleware di autenticazione Cookie Katana, chiama `UseCookieAuthentication` del progetto *Startup.Auth.cs* file.</span><span class="sxs-lookup"><span data-stu-id="e3636-145">When an app uses Katana Cookie Authentication Middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="e3636-146">Progetti di app web ASP.NET 4.x creati con Visual Studio 2013 e versioni successive usano il Middleware di autenticazione del Cookie Katana per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e3636-146">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana Cookie Authentication Middleware by default.</span></span> <span data-ttu-id="e3636-147">Sebbene `UseCookieAuthentication` è obsoleto e non supportati per le app ASP.NET Core, la chiamata `UseCookieAuthentication` in ASP.NET 4.x app che usa il Middleware di autenticazione di Katana Cookie è valido.</span><span class="sxs-lookup"><span data-stu-id="e3636-147">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana Cookie Authentication Middleware is valid.</span></span>

<span data-ttu-id="e3636-148">Un'app ASP.NET 4.x deve avere come destinazione .NET Framework 4.5.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="e3636-148">An ASP.NET 4.x app must target .NET Framework 4.5.1 or later.</span></span> <span data-ttu-id="e3636-149">In caso contrario, i pacchetti NuGet necessari esito negativo per l'installazione.</span><span class="sxs-lookup"><span data-stu-id="e3636-149">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="e3636-150">Per condividere i cookie di autenticazione tra un'app ASP.NET 4.x e un'app ASP.NET Core, configurare l'app ASP.NET Core come indicato nella [condivisione di cookie di autenticazione tra le app ASP.NET Core](#share-authentication-cookies-among-aspnet-core-apps) sezione e quindi configurare l'app ASP.NET 4.x come segue.</span><span class="sxs-lookup"><span data-stu-id="e3636-150">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated in the [Share authentication cookies among ASP.NET Core apps](#share-authentication-cookies-among-aspnet-core-apps) section, then configure the ASP.NET 4.x app as follows.</span></span>

<span data-ttu-id="e3636-151">Verificare che i pacchetti dell'app vengono aggiornati a versioni più recenti.</span><span class="sxs-lookup"><span data-stu-id="e3636-151">Confirm that the app's packages are updated to the latest releases.</span></span> <span data-ttu-id="e3636-152">Installare il [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package in ogni app ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="e3636-152">Install the [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) package into each ASP.NET 4.x app.</span></span>

<span data-ttu-id="e3636-153">Individuare e modificare la chiamata a `UseCookieAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e3636-153">Locate and modify the call to `UseCookieAuthentication`:</span></span>

* <span data-ttu-id="e3636-154">Modificare il nome del cookie per corrispondere al nome usato per il Middleware di autenticazione Cookie di ASP.NET Core (`.AspNet.SharedCookie` nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="e3636-154">Change the cookie name to match the name used by the ASP.NET Core Cookie Authentication Middleware (`.AspNet.SharedCookie` in the example).</span></span>
* <span data-ttu-id="e3636-155">Nell'esempio seguente, il tipo di autenticazione è impostato su `Identity.Application`.</span><span class="sxs-lookup"><span data-stu-id="e3636-155">In the following example, the authentication type is set to `Identity.Application`.</span></span>
* <span data-ttu-id="e3636-156">Fornire un'istanza di un `DataProtectionProvider` inizializzati per il percorso di archiviazione delle chiavi di protezione dati comune.</span><span class="sxs-lookup"><span data-stu-id="e3636-156">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>
* <span data-ttu-id="e3636-157">Verificare che il nome dell'app è impostato per il nome dell'app più comuni usato da tutte le app che condividono i cookie di autenticazione (`SharedCookieApp` nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="e3636-157">Confirm that the app name is set to the common app name used by all apps that share authentication cookies (`SharedCookieApp` in the example).</span></span>

<span data-ttu-id="e3636-158">Se non si imposta `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` e `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, impostare <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> per un'attestazione che consente di distinguere gli utenti univoci.</span><span class="sxs-lookup"><span data-stu-id="e3636-158">If not setting `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` and `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`, set <xref:System.Web.Helpers.AntiForgeryConfig.UniqueClaimTypeIdentifier> to a claim that distinguishes unique users.</span></span>

<span data-ttu-id="e3636-159">*App_Start/Startup.Auth.cs*:</span><span class="sxs-lookup"><span data-stu-id="e3636-159">*App_Start/Startup.Auth.cs*:</span></span>

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

<span data-ttu-id="e3636-160">Durante la generazione di un'identità utente, il tipo di autenticazione (`Identity.Application`) deve corrispondere al tipo definito in `AuthenticationType` impostati con `UseCookieAuthentication` nelle *App_Start/Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="e3636-160">When generating a user identity, the authentication type (`Identity.Application`) must match the type defined in `AuthenticationType` set with `UseCookieAuthentication` in *App_Start/Startup.Auth.cs*.</span></span>

<span data-ttu-id="e3636-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="e3636-161">*Models/IdentityModels.cs*:</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="e3636-162">Usare un database utente comuni</span><span class="sxs-lookup"><span data-stu-id="e3636-162">Use a common user database</span></span>

<span data-ttu-id="e3636-163">Quando le app usano la stessa identità dello schema (stessa versione dell'identità), verificare che il sistema di identità per ogni app fa riferimento a livello di database utente stesso.</span><span class="sxs-lookup"><span data-stu-id="e3636-163">When apps use the same Identity schema (same version of Identity), confirm that the Identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="e3636-164">In caso contrario, il sistema di identità genera errori in fase di esecuzione quando tenta di ottenere le informazioni nel cookie di autenticazione con le informazioni nel proprio database.</span><span class="sxs-lookup"><span data-stu-id="e3636-164">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

<span data-ttu-id="e3636-165">Quando lo schema di identità è diverso tra le app, in genere perché le app usino versioni diverse di identità, la condivisione di un database comune in base alla versione più recente dell'identità non è possibile senza modifica del mapping e l'aggiunta di colonne negli schemi di identità dell'app con altri.</span><span class="sxs-lookup"><span data-stu-id="e3636-165">When the Identity schema is different among apps, usually because apps are using different Identity versions, sharing a common database based on the latest version of Identity isn't possible without remapping and adding columns in other app's Identity schemas.</span></span> <span data-ttu-id="e3636-166">È spesso più efficiente aggiornare le altre App per usare la versione più recente di identità in modo che un database comune può essere condiviso dalle app.</span><span class="sxs-lookup"><span data-stu-id="e3636-166">It's often more efficient to upgrade the other apps to use the latest Identity version so that a common database can be shared by the apps.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e3636-167">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e3636-167">Additional resources</span></span>

* <xref:host-and-deploy/web-farm>
