---
title: Condividere i cookie tra le app con ASP.NET e ASP.NET Core
author: rick-anderson
description: Informazioni su come condividere i cookie di autenticazione tra ASP.NET 4. x e le applicazioni ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cookie-sharing
ms.openlocfilehash: f6d62d5f6e446e3e2001ed6bde72a6c409aa2833
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734679"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="07281-103">Condividere i cookie tra le app con ASP.NET e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07281-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="07281-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="07281-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="07281-105">Siti Web sono spesso costituite da un'App web singoli interagiscono.</span><span class="sxs-lookup"><span data-stu-id="07281-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="07281-106">Per fornire un'esperienza single sign-on (SSO), le applicazioni web all'interno di un sito devono condividere i cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="07281-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="07281-107">Per supportare questo scenario, lo stack di protezione dati consente la condivisione di autenticazione dei cookie Katana e il ticket di autenticazione ASP.NET Core cookie.</span><span class="sxs-lookup"><span data-stu-id="07281-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="07281-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="07281-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="07281-109">L'esempio illustra la condivisione tra tre applicazioni che utilizzano l'autenticazione dei cookie cookie:</span><span class="sxs-lookup"><span data-stu-id="07281-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="07281-110">App ASP.NET Core 2.0 Razor pagine senza utilizzare [ASP.NET Identity Core](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="07281-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="07281-111">Applicazione MVC ASP.NET Core 2.0 con identità di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07281-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="07281-112">Applicazione MVC ASP.NET Framework 4.6.1 con identità di ASP.NET</span><span class="sxs-lookup"><span data-stu-id="07281-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="07281-113">Negli esempi che seguono:</span><span class="sxs-lookup"><span data-stu-id="07281-113">In the examples that follow:</span></span>

* <span data-ttu-id="07281-114">Il nome del cookie di autenticazione è impostata su un valore comune di `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="07281-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="07281-115">Il `AuthenticationType` è impostato su `Identity.Application` in modo esplicito o per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="07281-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="07281-116">Viene utilizzato un nome di applicazione comune per abilitare il sistema di protezione dati condividere le chiavi di protezione dati (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="07281-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="07281-117">`Identity.Application` viene utilizzato come schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="07281-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="07281-118">Qualsiasi schema viene utilizzato, sarà necessario utilizzarlo in modo coerente *all'interno e al* le app condivise cookie come lo schema predefinito o impostandolo in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="07281-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="07281-119">Lo schema viene utilizzato quando crittografare e decrittografare i cookie, pertanto deve essere utilizzato uno schema coerente tra app.</span><span class="sxs-lookup"><span data-stu-id="07281-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="07281-120">Un comune [chiave di protezione dati](xref:security/data-protection/implementation/key-management) viene utilizzato il percorso di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="07281-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="07281-121">L'app di esempio utilizza una cartella denominata *KeyRing* alla radice della soluzione per contenere le chiavi di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="07281-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="07281-122">Nelle app di ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) utilizzato per impostare il percorso di archiviazione chiavi.</span><span class="sxs-lookup"><span data-stu-id="07281-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="07281-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) consente di configurare un nome di app condiviso comune.</span><span class="sxs-lookup"><span data-stu-id="07281-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="07281-124">Nell'app .NET Framework, il middleware di autenticazione cookie utilizza un'implementazione di [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="07281-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="07281-125">`DataProtectionProvider` fornisce servizi di protezione dei dati per la crittografia e decrittografia dei dati di payload cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="07281-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="07281-126">Il `DataProtectionProvider` istanza è isolata dal sistema di protezione di dati utilizzato da altre parti dell'app.</span><span class="sxs-lookup"><span data-stu-id="07281-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="07281-127">[DataProtectionProvider.Create (DirectoryInfo, azione\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accetta una [DirectoryInfo](/dotnet/api/system.io.directoryinfo) per specificare il percorso di archiviazione chiavi di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="07281-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="07281-128">L'app di esempio fornisce il percorso del *KeyRing* cartella `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="07281-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="07281-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) imposta il nome dell'applicazione comune.</span><span class="sxs-lookup"><span data-stu-id="07281-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="07281-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) richiede il [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="07281-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="07281-131">Per ottenere questo pacchetto per ASP.NET Core 2.1 App e versioni successive, fare riferimento il [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="07281-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="07281-132">Quando la destinazione è .NET Framework, aggiungere un riferimento pacchetto `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="07281-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="07281-133">Condividere i cookie di autenticazione tra le applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07281-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="07281-134">Quando si usa ASP.NET Identity Core:</span><span class="sxs-lookup"><span data-stu-id="07281-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07281-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07281-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="07281-136">Nel `ConfigureServices` metodo, utilizzare il [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) metodo di estensione per configurare il servizio di protezione dati per i cookie.</span><span class="sxs-lookup"><span data-stu-id="07281-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="07281-137">Chiavi di protezione dati e il nome dell'applicazione devono essere condivise tra le app.</span><span class="sxs-lookup"><span data-stu-id="07281-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="07281-138">Nelle app di esempio `GetKeyRingDirInfo` restituisce la posizione di archiviazione chiavi comuni per il [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metodo.</span><span class="sxs-lookup"><span data-stu-id="07281-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="07281-139">Uso [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) per configurare un nome di app condiviso comune (`SharedCookieApp` nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="07281-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="07281-140">Per altre informazioni, vedere [configurazione della protezione dati](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="07281-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="07281-141">Vedere il *CookieAuthWithIdentity.Core* nel progetto di [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="07281-141">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07281-142">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07281-142">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="07281-143">Nel `Configure` metodo, utilizzare il [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) impostare:</span><span class="sxs-lookup"><span data-stu-id="07281-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="07281-144">Il servizio di protezione dati per i cookie.</span><span class="sxs-lookup"><span data-stu-id="07281-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="07281-145">Il `AuthenticationScheme` in modo che corrisponda ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="07281-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="07281-146">Quando si utilizzano direttamente i cookie:</span><span class="sxs-lookup"><span data-stu-id="07281-146">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07281-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07281-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="07281-148">Chiavi di protezione dati e il nome dell'applicazione devono essere condivise tra le app.</span><span class="sxs-lookup"><span data-stu-id="07281-148">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="07281-149">Nelle app di esempio `GetKeyRingDirInfo` restituisce la posizione di archiviazione chiavi comuni per il [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metodo.</span><span class="sxs-lookup"><span data-stu-id="07281-149">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="07281-150">Uso [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) per configurare un nome di app condiviso comune (`SharedCookieApp` nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="07281-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="07281-151">Per altre informazioni, vedere [configurazione della protezione dati](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="07281-151">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span> 

<span data-ttu-id="07281-152">Vedere il *CookieAuth.Core* nel progetto di [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="07281-152">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07281-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07281-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="07281-154">La crittografia delle chiavi di protezione dati inattivi</span><span class="sxs-lookup"><span data-stu-id="07281-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="07281-155">Per le distribuzioni di produzione, è possibile configurare il `DataProtectionProvider` per crittografare le chiavi inattivi con DPAPI o un X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="07281-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="07281-156">Vedere [chiave di crittografia](xref:security/data-protection/implementation/key-encryption-at-rest) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="07281-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07281-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07281-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07281-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07281-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="07281-159">Condividere i cookie di autenticazione tra ASP.NET 4. x e ASP.NET Core App</span><span class="sxs-lookup"><span data-stu-id="07281-159">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="07281-160">ASP.NET 4. x App che usano Katana cookie middleware di autenticazione può essere configurata per generare i cookie di autenticazione che sono compatibili con il middleware di autenticazione ASP.NET Core cookie.</span><span class="sxs-lookup"><span data-stu-id="07281-160">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="07281-161">In questo modo l'aggiornamento a fasi di singole app di un sito di grandi dimensioni fornendo un'esperienza SSO senza problemi tra il sito.</span><span class="sxs-lookup"><span data-stu-id="07281-161">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="07281-162">Quando un'app Usa Katana cookie middleware di autenticazione, chiama `UseCookieAuthentication` del progetto *Startup.Auth.cs* file.</span><span class="sxs-lookup"><span data-stu-id="07281-162">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="07281-163">Progetti di app web ASP.NET 4. x creati con Visual Studio 2013 e versioni successive, utilizzare il middleware di autenticazione del cookie Katana per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="07281-163">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="07281-164">Un'app ASP.NET 4. x deve avere come destinazione .NET Framework 4.5.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="07281-164">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="07281-165">In caso contrario, i pacchetti NuGet necessari non essere installato.</span><span class="sxs-lookup"><span data-stu-id="07281-165">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="07281-166">Per condividere i cookie di autenticazione tra le app ASP.NET 4. x e ASP.NET Core, configurare l'applicazione ASP.NET di base, come descritto in precedenza, quindi configurare le app ASP.NET 4. x seguendo la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="07281-166">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="07281-167">Installare il pacchetto [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) in ogni app ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="07281-167">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="07281-168">In *Startup.Auth.cs*, individuare la chiamata a `UseCookieAuthentication` e modificarla come segue.</span><span class="sxs-lookup"><span data-stu-id="07281-168">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="07281-169">Modificare il nome del cookie per corrispondere al nome utilizzato dal middleware di autenticazione del cookie di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07281-169">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="07281-170">Fornire un'istanza di un `DataProtectionProvider` inizializzato al percorso di archiviazione chiavi protezione dati comune.</span><span class="sxs-lookup"><span data-stu-id="07281-170">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="07281-171">Verificare che il nome dell'applicazione è impostato il nome dell'app comuni utilizzati da tutte le app che condividono i cookie, `SharedCookieApp` nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="07281-171">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="07281-172">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="07281-172">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="07281-173">Vedere il *CookieAuthWithIdentity.NETFramework* nel progetto di [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="07281-173">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="07281-174">Quando si genera un'identità utente, il tipo di autenticazione deve corrispondere al tipo definito in `AuthenticationType` impostata con `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="07281-174">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="07281-175">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="07281-175">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="07281-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="07281-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="07281-177">Impostare il `CookieManager` all'interoperabilità `ChunkingCookieManager` pertanto il formato di suddivisione in blocchi è compatibile.</span><span class="sxs-lookup"><span data-stu-id="07281-177">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

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

## <a name="use-a-common-user-database"></a><span data-ttu-id="07281-178">Utilizzare un database utente comuni</span><span class="sxs-lookup"><span data-stu-id="07281-178">Use a common user database</span></span>

<span data-ttu-id="07281-179">Verificare che il sistema di identità per ogni app fa riferimento allo stesso database utente.</span><span class="sxs-lookup"><span data-stu-id="07281-179">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="07281-180">In caso contrario, il sistema di identità genera errori in fase di esecuzione durante il tentativo di associare le informazioni nel cookie di autenticazione e le informazioni sul proprio database.</span><span class="sxs-lookup"><span data-stu-id="07281-180">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
