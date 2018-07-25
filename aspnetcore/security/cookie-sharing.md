---
title: Condividere cookie tra le app con ASP.NET e ASP.NET Core
author: rick-anderson
description: Informazioni su come condividere i cookie di autenticazione tra ASP.NET 4.x e le app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
uid: security/cookie-sharing
ms.openlocfilehash: ed3496db3f7a63a704f0e57faef6b2f6c085a8bd
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228598"
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="f12d4-103">Condividere cookie tra le app con ASP.NET e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f12d4-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="f12d4-104">Dal [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f12d4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f12d4-105">Siti Web è spesso costituita da singole App web che interagiscono.</span><span class="sxs-lookup"><span data-stu-id="f12d4-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="f12d4-106">Per offrire un'esperienza single sign-on (SSO), App web all'interno di un sito devono condividere i cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="f12d4-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="f12d4-107">Per supportare questo scenario, lo stack di protezione dati consente di condividere l'autenticazione tramite cookie Katana e i ticket di autenticazione di cookie di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f12d4-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="f12d4-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f12d4-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f12d4-109">L'esempio illustra cookie condivisione tra le tre App che usano l'autenticazione tramite cookie:</span><span class="sxs-lookup"><span data-stu-id="f12d4-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="f12d4-110">App ASP.NET Core 2.0 Razor Pages senza usare [ASP.NET Core Identity](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="f12d4-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="f12d4-111">App ASP.NET Core 2.0 MVC con ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="f12d4-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="f12d4-112">App MVC ASP.NET Framework 4.6.1 con ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="f12d4-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="f12d4-113">Negli esempi che seguono:</span><span class="sxs-lookup"><span data-stu-id="f12d4-113">In the examples that follow:</span></span>

* <span data-ttu-id="f12d4-114">Il nome del cookie di autenticazione è impostata su un valore comune di `.AspNet.SharedCookie`.</span><span class="sxs-lookup"><span data-stu-id="f12d4-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="f12d4-115">Il `AuthenticationType` è impostata su `Identity.Application` in modo esplicito o per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f12d4-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="f12d4-116">Un nome comune dell'app viene usato per abilitare il sistema di protezione dati condividere le chiavi di protezione dati (`SharedCookieApp`).</span><span class="sxs-lookup"><span data-stu-id="f12d4-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="f12d4-117">`Identity.Application` viene usato come lo schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="f12d4-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="f12d4-118">Qualsiasi schema viene usato, sarà necessario utilizzarlo in modo coerente *all'interno e tra* le app condivise cookie come lo schema predefinito o impostandolo in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="f12d4-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="f12d4-119">Lo schema viene utilizzato quando crittografare e decrittografare i cookie, pertanto è necessario utilizzare uno schema coerente tra le app.</span><span class="sxs-lookup"><span data-stu-id="f12d4-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="f12d4-120">Un comune [chiave di protezione dei dati](xref:security/data-protection/implementation/key-management) viene usato il percorso di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="f12d4-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="f12d4-121">L'app di esempio Usa una cartella denominata *KeyRing* alla radice della soluzione per contenere le chiavi di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="f12d4-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="f12d4-122">Nelle App ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) consente di impostare il percorso di archiviazione delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="f12d4-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="f12d4-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) viene usato per configurare un nome di app condiviso comune.</span><span class="sxs-lookup"><span data-stu-id="f12d4-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="f12d4-124">Nell'app .NET Framework, middleware di autenticazione dei cookie Usa un'implementazione del [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span><span class="sxs-lookup"><span data-stu-id="f12d4-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="f12d4-125">`DataProtectionProvider` fornisce servizi di protezione dati per la crittografia e decrittografia dei dati di payload di cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="f12d4-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="f12d4-126">Il `DataProtectionProvider` istanza è isolata dal sistema di protezione di dati usato da altre parti dell'app.</span><span class="sxs-lookup"><span data-stu-id="f12d4-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="f12d4-127">[DataProtectionProvider.Create (azione, DirectoryInfo\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accetta una [DirectoryInfo](/dotnet/api/system.io.directoryinfo) per specificare il percorso di archiviazione chiavi di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="f12d4-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="f12d4-128">L'app di esempio fornisce il percorso dei *KeyRing* cartella `DirectoryInfo`.</span><span class="sxs-lookup"><span data-stu-id="f12d4-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="f12d4-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) imposta il nome dell'app più comuni.</span><span class="sxs-lookup"><span data-stu-id="f12d4-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="f12d4-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) richiede la [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="f12d4-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="f12d4-131">Per ottenere questo pacchetto per ASP.NET Core 2.1 e versioni successive delle App, fare riferimento il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="f12d4-131">To obtain this package for ASP.NET Core 2.1 and later apps, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="f12d4-132">Quando la destinazione è .NET Framework, aggiungere un riferimento al pacchetto `Microsoft.AspNetCore.DataProtection.Extensions`.</span><span class="sxs-lookup"><span data-stu-id="f12d4-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="f12d4-133">Condivisione di cookie di autenticazione tra le app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f12d4-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="f12d4-134">Quando si usa ASP.NET Core Identity:</span><span class="sxs-lookup"><span data-stu-id="f12d4-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f12d4-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f12d4-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="f12d4-136">Nel `ConfigureServices` metodo, usare il [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) metodo di estensione per configurare il servizio di protezione dati per i cookie.</span><span class="sxs-lookup"><span data-stu-id="f12d4-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="f12d4-137">Chiavi di protezione dati e il nome dell'app deve essere condivisa tra le app.</span><span class="sxs-lookup"><span data-stu-id="f12d4-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="f12d4-138">Nelle app di esempio, `GetKeyRingDirInfo` restituisce la posizione di archiviazione chiavi comuni per il [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) (metodo).</span><span class="sxs-lookup"><span data-stu-id="f12d4-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="f12d4-139">Uso [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) per configurare un nome di app condivisa comune (`SharedCookieApp` nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="f12d4-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="f12d4-140">Per altre informazioni, vedere [configurazione della protezione dati](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="f12d4-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="f12d4-141">Quando si ospitano le app che condividono i cookie in sottodomini, specificare un dominio in comune il [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) proprietà.</span><span class="sxs-lookup"><span data-stu-id="f12d4-141">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="f12d4-142">Per condividere i cookie tra App su `contoso.com`, ad esempio `first_subdomain.contoso.com` e `second_subdomain.contoso.com`, specificare il `Cookie.Domain` come `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="f12d4-142">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="f12d4-143">Vedere le *CookieAuthWithIdentity.Core* del progetto nel [esempi di codice](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f12d4-143">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f12d4-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f12d4-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="f12d4-145">Nel `Configure` metodo, usare il [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) configurare:</span><span class="sxs-lookup"><span data-stu-id="f12d4-145">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="f12d4-146">Il servizio di protezione dati per i cookie.</span><span class="sxs-lookup"><span data-stu-id="f12d4-146">The data protection service for cookies.</span></span>
* <span data-ttu-id="f12d4-147">Il `AuthenticationScheme` in modo che corrispondano ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="f12d4-147">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

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

<span data-ttu-id="f12d4-148">Quando si usa direttamente i cookie:</span><span class="sxs-lookup"><span data-stu-id="f12d4-148">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f12d4-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f12d4-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="f12d4-150">Chiavi di protezione dati e il nome dell'app deve essere condivisa tra le app.</span><span class="sxs-lookup"><span data-stu-id="f12d4-150">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="f12d4-151">Nelle app di esempio, `GetKeyRingDirInfo` restituisce la posizione di archiviazione chiavi comuni per il [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) (metodo).</span><span class="sxs-lookup"><span data-stu-id="f12d4-151">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="f12d4-152">Uso [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) per configurare un nome di app condivisa comune (`SharedCookieApp` nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="f12d4-152">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="f12d4-153">Per altre informazioni, vedere [configurazione della protezione dati](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="f12d4-153">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="f12d4-154">Quando si ospitano le app che condividono i cookie in sottodomini, specificare un dominio in comune il [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) proprietà.</span><span class="sxs-lookup"><span data-stu-id="f12d4-154">When hosting apps that share cookies across subdomains, specify a common domain in the [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) property.</span></span> <span data-ttu-id="f12d4-155">Per condividere i cookie tra App su `contoso.com`, ad esempio `first_subdomain.contoso.com` e `second_subdomain.contoso.com`, specificare il `Cookie.Domain` come `.contoso.com`:</span><span class="sxs-lookup"><span data-stu-id="f12d4-155">To share cookies across apps at `contoso.com`, such as `first_subdomain.contoso.com` and `second_subdomain.contoso.com`, specify the `Cookie.Domain` as `.contoso.com`:</span></span>

```csharp
options.Cookie.Domain = ".contoso.com";
```

<span data-ttu-id="f12d4-156">Vedere le *CookieAuth.Core* del progetto nel [esempi di codice](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f12d4-156">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f12d4-157">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f12d4-157">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="f12d4-158">Crittografare le chiavi di protezione dei dati inattivi</span><span class="sxs-lookup"><span data-stu-id="f12d4-158">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="f12d4-159">Per le distribuzioni di produzione, configurare il `DataProtectionProvider` per crittografare le chiavi a riposo con DPAPI o un X509Certificate.</span><span class="sxs-lookup"><span data-stu-id="f12d4-159">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="f12d4-160">Visualizzare [chiave di crittografia dei dati](xref:security/data-protection/implementation/key-encryption-at-rest) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="f12d4-160">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f12d4-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f12d4-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f12d4-162">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f12d4-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="f12d4-163">Condivisione dei cookie di autenticazione tra ASP.NET 4.x e le app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f12d4-163">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="f12d4-164">App ASP.NET 4.x che usano il middleware di autenticazione di Katana cookie può essere configurata per generare i cookie di autenticazione che sono compatibili con il middleware di autenticazione di cookie di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f12d4-164">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="f12d4-165">In questo modo l'aggiornamento a fasi le singole app di un sito di grandi dimensioni, fornendo un'esperienza SSO ottimale tra il sito.</span><span class="sxs-lookup"><span data-stu-id="f12d4-165">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

<span data-ttu-id="f12d4-166">Quando un'app Usa il middleware di autenticazione cookie Katana, chiama `UseCookieAuthentication` del progetto *Startup.Auth.cs* file.</span><span class="sxs-lookup"><span data-stu-id="f12d4-166">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="f12d4-167">Progetti di app web ASP.NET 4.x creati con Visual Studio 2013 e versioni successive usano il middleware di autenticazione del cookie Katana per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f12d4-167">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span> <span data-ttu-id="f12d4-168">Sebbene `UseCookieAuthentication` è obsoleto e non supportati per le app ASP.NET Core, la chiamata `UseCookieAuthentication` in un'app ASP.NET 4.x Usa Katana middleware di autenticazione del cookie è valido.</span><span class="sxs-lookup"><span data-stu-id="f12d4-168">Although `UseCookieAuthentication` is obsolete and unsupported for ASP.NET Core apps, calling `UseCookieAuthentication` in an ASP.NET 4.x app that uses Katana cookie authentication middleware is valid.</span></span>

<span data-ttu-id="f12d4-169">Un'app ASP.NET 4.x deve avere come destinazione .NET Framework 4.5.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="f12d4-169">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="f12d4-170">In caso contrario, i pacchetti NuGet necessari esito negativo per l'installazione.</span><span class="sxs-lookup"><span data-stu-id="f12d4-170">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="f12d4-171">Per condividere i cookie di autenticazione tra un'app ASP.NET 4.x e un'app ASP.NET Core, configurare l'app ASP.NET Core, come indicato in precedenza, quindi configurare l'app ASP.NET 4.x seguendo questa procedura:</span><span class="sxs-lookup"><span data-stu-id="f12d4-171">To share authentication cookies between an ASP.NET 4.x app and an ASP.NET Core app, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x app by following these steps:</span></span>

1. <span data-ttu-id="f12d4-172">Installare il pacchetto [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) in ogni app ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="f12d4-172">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="f12d4-173">Nelle *Startup.Auth.cs*, individuare la chiamata a `UseCookieAuthentication` e modificarla come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f12d4-173">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="f12d4-174">Modificare il nome del cookie per corrispondere al nome usato dal middleware di autenticazione dei cookie di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f12d4-174">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="f12d4-175">Fornire un'istanza di un `DataProtectionProvider` inizializzati per il percorso di archiviazione delle chiavi di protezione dati comune.</span><span class="sxs-lookup"><span data-stu-id="f12d4-175">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="f12d4-176">Assicurarsi che il nome dell'app è impostato per il nome dell'app più comuni usato da tutte le app che condividono i cookie, `SharedCookieApp` nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="f12d4-176">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="f12d4-177">Vedere le *CookieAuthWithIdentity.NETFramework* del progetto nel [esempi di codice](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f12d4-177">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="f12d4-178">Durante la generazione di un'identità utente, il tipo di autenticazione deve corrispondere al tipo definito in `AuthenticationType` impostati con `UseCookieAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="f12d4-178">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="f12d4-179">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="f12d4-179">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a><span data-ttu-id="f12d4-180">Usare un database utente comuni</span><span class="sxs-lookup"><span data-stu-id="f12d4-180">Use a common user database</span></span>

<span data-ttu-id="f12d4-181">Verificare che il sistema di identità per ogni app fa riferimento a livello di database utente stesso.</span><span class="sxs-lookup"><span data-stu-id="f12d4-181">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="f12d4-182">In caso contrario, il sistema di identità genera errori in fase di esecuzione quando tenta di ottenere le informazioni nel cookie di autenticazione con le informazioni nel proprio database.</span><span class="sxs-lookup"><span data-stu-id="f12d4-182">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f12d4-183">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f12d4-183">Additional resources</span></span>

<xref:host-and-deploy/web-farm>
