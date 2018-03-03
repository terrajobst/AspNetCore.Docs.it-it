---
title: I cookie tra le app di condivisione
author: rick-anderson
description: Informazioni su come condividere i cookie di autenticazione tra ASP.NET 4. x e le applicazioni ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: f026a287601a51e544482b95c5283e8ee960d656
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="sharing-cookies-among-apps"></a><span data-ttu-id="97ce6-103">I cookie tra le app di condivisione</span><span class="sxs-lookup"><span data-stu-id="97ce6-103">Sharing cookies among apps</span></span>

<span data-ttu-id="97ce6-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="97ce6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="97ce6-105">Siti Web sono spesso costituite da un'App web singoli interagiscono.</span><span class="sxs-lookup"><span data-stu-id="97ce6-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="97ce6-106">Per fornire un'esperienza single sign-on (SSO), le applicazioni web all'interno di un sito devono condividere i cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="97ce6-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="97ce6-107">Per supportare questo scenario, lo stack di protezione dati consente la condivisione di autenticazione dei cookie Katana e il ticket di autenticazione ASP.NET Core cookie.</span><span class="sxs-lookup"><span data-stu-id="97ce6-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="97ce6-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="97ce6-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="97ce6-109">L'esempio illustra la condivisione tra tre applicazioni che utilizzano l'autenticazione dei cookie cookie:</span><span class="sxs-lookup"><span data-stu-id="97ce6-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="97ce6-110">App ASP.NET Core 2.0 Razor pagine senza utilizzare [ASP.NET Identity Core](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="97ce6-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="97ce6-111">Applicazione MVC ASP.NET Core 2.0 con identità di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97ce6-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="97ce6-112">Applicazione MVC ASP.NET Framework 4.6.1 con identità di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="97ce6-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="97ce6-113">Negli esempi che seguono:</span><span class="sxs-lookup"><span data-stu-id="97ce6-113">In the examples that follow:</span></span>

* <span data-ttu-id="97ce6-114">Il nome del cookie di autenticazione è impostata su un valore comune di `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="97ce6-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="97ce6-115">Il `AuthenticationType` è impostato su `Identity.Application` in modo esplicito o per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="97ce6-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="97ce6-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) viene utilizzato lo schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="97ce6-116">[CookieAuthenticationDefaults.AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationdefaults.authenticationscheme) is used as the authentication scheme.</span></span> <span data-ttu-id="97ce6-117">Restituisce un valore di costante `Cookies`.</span><span class="sxs-lookup"><span data-stu-id="97ce6-117">The constant resolves to a value of `Cookies`.</span></span>
* <span data-ttu-id="97ce6-118">Il middleware di autenticazione cookie utilizza un'implementazione di [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="97ce6-118">The cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="97ce6-119">`DataProtectionProvider` fornisce servizi di protezione dei dati per la crittografia e decrittografia dei dati di payload cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="97ce6-119">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="97ce6-120">Il `DataProtectionProvider` istanza è isolata dal sistema di protezione di dati utilizzato da altre parti dell'app.</span><span class="sxs-lookup"><span data-stu-id="97ce6-120">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="97ce6-121">Un comune [chiave di protezione dati](xref:security/data-protection/implementation/key-management) viene utilizzato il percorso di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="97ce6-121">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="97ce6-122">L'app di esempio utilizza una cartella denominata *KeyRing* alla radice della soluzione per contenere le chiavi di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="97ce6-122">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
  * <span data-ttu-id="97ce6-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) accetta un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) per l'utilizzo con i cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="97ce6-123">[DataProtectionProvider.Create(System.IO.DirectoryInfo)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) for use with authentication cookies.</span></span> <span data-ttu-id="97ce6-124">L'app di esempio fornisce il percorso del *KeyRing* cartella `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="97ce6-124">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span>
  * <span data-ttu-id="97ce6-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) richiede il [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="97ce6-125">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="97ce6-126">Per ottenere il pacchetto per ASP.NET 2.0 Core App e versioni successive, fare riferimento il [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span><span class="sxs-lookup"><span data-stu-id="97ce6-126">To obtain this package for ASP.NET Core 2.0 and later apps, reference the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="97ce6-127">Quando la destinazione è .NET Framework, aggiungere un riferimento pacchetto `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="97ce6-127">When targeting .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="97ce6-128">Condividere i cookie di autenticazione tra le applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="97ce6-128">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="97ce6-129">Quando si usa ASP.NET Identity Core:</span><span class="sxs-lookup"><span data-stu-id="97ce6-129">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="97ce6-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="97ce6-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="97ce6-131">Nel `ConfigureServices` metodo, utilizzare il [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) metodo di estensione per configurare il servizio di protezione dati per i cookie.</span><span class="sxs-lookup"><span data-stu-id="97ce6-131">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="97ce6-132">Vedere il *CookieAuthWithIdentity.Core* nel progetto di [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="97ce6-132">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="97ce6-133">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="97ce6-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="97ce6-134">Nel `Configure` metodo, utilizzare il [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) impostare:</span><span class="sxs-lookup"><span data-stu-id="97ce6-134">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="97ce6-135">Il servizio di protezione dati per i cookie.</span><span class="sxs-lookup"><span data-stu-id="97ce6-135">The data protection service for cookies.</span></span>
* <span data-ttu-id="97ce6-136">Il `AuthenticationScheme` in modo che corrisponda ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="97ce6-136">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = 
        "ApplicationCookie";

    var protectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"));

    options.Cookies.ApplicationCookie.DataProtectionProvider = 
        protectionProvider;

    options.Cookies.ApplicationCookie.TicketDataFormat = 
        new TicketDataFormat(protectionProvider.CreateProtector(
            "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", 
            "Cookies", 
            "v2"));
});
```

---

<span data-ttu-id="97ce6-137">Quando si utilizzano direttamente i cookie:</span><span class="sxs-lookup"><span data-stu-id="97ce6-137">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="97ce6-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="97ce6-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="97ce6-139">Vedere il *CookieAuth.Core* nel progetto di [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="97ce6-139">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="97ce6-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="97ce6-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="97ce6-141">La crittografia delle chiavi di protezione dati inattivi</span><span class="sxs-lookup"><span data-stu-id="97ce6-141">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="97ce6-142">Per le distribuzioni di produzione, è possibile configurare il `DataProtectionProvider` per crittografare le chiavi inattivi con DPAPI o un X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="97ce6-142">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="97ce6-143">Vedere [chiave di crittografia](xref:security/data-protection/implementation/key-encryption-at-rest) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="97ce6-143">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="97ce6-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="97ce6-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="97ce6-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="97ce6-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(
        new DirectoryInfo(@"PATH_TO_KEY_RING"),
        configure =>
        {
            configure.ProtectKeysWithCertificate("thumbprint");
        })
});
```

---

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="97ce6-146">Condividere i cookie di autenticazione tra ASP.NET 4. x e ASP.NET Core App</span><span class="sxs-lookup"><span data-stu-id="97ce6-146">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="97ce6-147">ASP.NET 4. x App che usano Katana cookie middleware di autenticazione può essere configurata per generare i cookie di autenticazione che sono compatibili con il middleware di autenticazione ASP.NET Core cookie.</span><span class="sxs-lookup"><span data-stu-id="97ce6-147">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="97ce6-148">In questo modo l'aggiornamento a fasi di singole app di un sito di grandi dimensioni fornendo un'esperienza SSO senza problemi tra il sito.</span><span class="sxs-lookup"><span data-stu-id="97ce6-148">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="97ce6-149">Quando un'app Usa Katana cookie middleware di autenticazione, chiama `UseCookieAuthentication` del progetto *Startup.Auth.cs* file.</span><span class="sxs-lookup"><span data-stu-id="97ce6-149">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="97ce6-150">Progetti di app web ASP.NET 4. x creati con Visual Studio 2013 e versioni successive, utilizzare il middleware di autenticazione del cookie Katana per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="97ce6-150">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="97ce6-151">Un'app ASP.NET 4. x deve avere come destinazione .NET Framework 4.5.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="97ce6-151">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="97ce6-152">In caso contrario, i pacchetti NuGet necessari non essere installato.</span><span class="sxs-lookup"><span data-stu-id="97ce6-152">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="97ce6-153">Per condividere i cookie di autenticazione tra le app ASP.NET 4. x e ASP.NET Core, configurare l'applicazione ASP.NET di base, come descritto in precedenza, quindi configurare le app ASP.NET 4. x seguendo la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="97ce6-153">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="97ce6-154">Installare il pacchetto [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) in ogni app ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="97ce6-154">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="97ce6-155">In *Startup.Auth.cs*, individuare la chiamata a `UseCookieAuthentication` e modificarla come segue.</span><span class="sxs-lookup"><span data-stu-id="97ce6-155">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="97ce6-156">Modificare il nome del cookie per corrispondere al nome utilizzato dal middleware di autenticazione del cookie di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="97ce6-156">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="97ce6-157">Fornire un'istanza di un `DataProtectionProvider` inizializzato al percorso di archiviazione chiavi protezione dati comune.</span><span class="sxs-lookup"><span data-stu-id="97ce6-157">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="97ce6-158">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="97ce6-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="97ce6-159">Vedere il *CookieAuthWithIdentity.NETFramework* nel progetto di [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="97ce6-159">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/data-protection/compatibility/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="97ce6-160">Quando si genera un'identità utente, il tipo di autenticazione deve corrispondere al tipo definito in `AuthenticationType` impostata con `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="97ce6-160">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="97ce6-161">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="97ce6-161">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="97ce6-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="97ce6-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="97ce6-163">Impostare il `CookieManager` all'interoperabilità `ChunkingCookieManager` pertanto il formato di suddivisione in blocchi è compatibile.</span><span class="sxs-lookup"><span data-stu-id="97ce6-163">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
    CookieName = ".AspNetCore.Cookies",
    // CookieName = ".AspNetCore.ApplicationCookie", (if using ASP.NET Identity)
    // CookiePath = "...", (if necessary)
    // ...
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create(
                new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", 
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});
```

---

## <a name="use-a-common-user-database"></a><span data-ttu-id="97ce6-164">Utilizzare un database utente comuni</span><span class="sxs-lookup"><span data-stu-id="97ce6-164">Use a common user database</span></span>

<span data-ttu-id="97ce6-165">Verificare che il sistema di identità per ogni app fa riferimento allo stesso database utente.</span><span class="sxs-lookup"><span data-stu-id="97ce6-165">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="97ce6-166">In caso contrario, il sistema di identità genera errori in fase di esecuzione durante il tentativo di associare le informazioni nel cookie di autenticazione e le informazioni sul proprio database.</span><span class="sxs-lookup"><span data-stu-id="97ce6-166">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
