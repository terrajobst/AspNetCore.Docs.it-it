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
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>Condividere cookie tra le app con ASP.NET e ASP.NET Core

Dal [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)

Siti Web è spesso costituita da singole App web che interagiscono. Per offrire un'esperienza single sign-on (SSO), App web all'interno di un sito devono condividere i cookie di autenticazione. Per supportare questo scenario, lo stack di protezione dati consente di condividere l'autenticazione tramite cookie Katana e i ticket di autenticazione di cookie di ASP.NET Core.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

L'esempio illustra cookie condivisione tra le tre App che usano l'autenticazione tramite cookie:

* App ASP.NET Core 2.0 Razor Pages senza usare [ASP.NET Core Identity](xref:security/authentication/identity)
* App ASP.NET Core 2.0 MVC con ASP.NET Core Identity
* App MVC ASP.NET Framework 4.6.1 con ASP.NET Identity

Negli esempi che seguono:

* Il nome del cookie di autenticazione è impostata su un valore comune di `.AspNet.SharedCookie`.
* Il `AuthenticationType` è impostata su `Identity.Application` in modo esplicito o per impostazione predefinita.
* Un nome comune dell'app viene usato per abilitare il sistema di protezione dati condividere le chiavi di protezione dati (`SharedCookieApp`).
* `Identity.Application` viene usato come lo schema di autenticazione. Qualsiasi schema viene usato, sarà necessario utilizzarlo in modo coerente *all'interno e tra* le app condivise cookie come lo schema predefinito o impostandolo in modo esplicito. Lo schema viene utilizzato quando crittografare e decrittografare i cookie, pertanto è necessario utilizzare uno schema coerente tra le app.
* Un comune [chiave di protezione dei dati](xref:security/data-protection/implementation/key-management) viene usato il percorso di archiviazione. L'app di esempio Usa una cartella denominata *KeyRing* alla radice della soluzione per contenere le chiavi di protezione dati.
* Nelle App ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) consente di impostare il percorso di archiviazione delle chiavi. [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) viene usato per configurare un nome di app condiviso comune.
* Nell'app .NET Framework, middleware di autenticazione dei cookie Usa un'implementazione del [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` fornisce servizi di protezione dati per la crittografia e decrittografia dei dati di payload di cookie di autenticazione. Il `DataProtectionProvider` istanza è isolata dal sistema di protezione di dati usato da altre parti dell'app.
  * [DataProtectionProvider.Create (azione, DirectoryInfo\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accetta una [DirectoryInfo](/dotnet/api/system.io.directoryinfo) per specificare il percorso di archiviazione chiavi di protezione dati. L'app di esempio fornisce il percorso dei *KeyRing* cartella `DirectoryInfo`. [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) imposta il nome dell'app più comuni.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) richiede la [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pacchetto NuGet. Per ottenere questo pacchetto per ASP.NET Core 2.1 e versioni successive delle App, fare riferimento il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Quando la destinazione è .NET Framework, aggiungere un riferimento al pacchetto `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Condivisione di cookie di autenticazione tra le app ASP.NET Core

Quando si usa ASP.NET Core Identity:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Nel `ConfigureServices` metodo, usare il [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) metodo di estensione per configurare il servizio di protezione dati per i cookie.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Chiavi di protezione dati e il nome dell'app deve essere condivisa tra le app. Nelle app di esempio, `GetKeyRingDirInfo` restituisce la posizione di archiviazione chiavi comuni per il [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) (metodo). Uso [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) per configurare un nome di app condivisa comune (`SharedCookieApp` nell'esempio). Per altre informazioni, vedere [configurazione della protezione dati](xref:security/data-protection/configuration/overview).

Quando si ospitano le app che condividono i cookie in sottodomini, specificare un dominio in comune il [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) proprietà. Per condividere i cookie tra App su `contoso.com`, ad esempio `first_subdomain.contoso.com` e `second_subdomain.contoso.com`, specificare il `Cookie.Domain` come `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

Vedere le *CookieAuthWithIdentity.Core* del progetto nel [esempi di codice](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Nel `Configure` metodo, usare il [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) configurare:

* Il servizio di protezione dati per i cookie.
* Il `AuthenticationScheme` in modo che corrispondano ASP.NET 4.x.

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

Quando si usa direttamente i cookie:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

Chiavi di protezione dati e il nome dell'app deve essere condivisa tra le app. Nelle app di esempio, `GetKeyRingDirInfo` restituisce la posizione di archiviazione chiavi comuni per il [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) (metodo). Uso [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) per configurare un nome di app condivisa comune (`SharedCookieApp` nell'esempio). Per altre informazioni, vedere [configurazione della protezione dati](xref:security/data-protection/configuration/overview).

Quando si ospitano le app che condividono i cookie in sottodomini, specificare un dominio in comune il [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain) proprietà. Per condividere i cookie tra App su `contoso.com`, ad esempio `first_subdomain.contoso.com` e `second_subdomain.contoso.com`, specificare il `Cookie.Domain` come `.contoso.com`:

```csharp
options.Cookie.Domain = ".contoso.com";
```

Vedere le *CookieAuth.Core* del progetto nel [esempi di codice](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a>Crittografare le chiavi di protezione dei dati inattivi

Per le distribuzioni di produzione, configurare il `DataProtectionProvider` per crittografare le chiavi a riposo con DPAPI o un X509Certificate. Visualizzare [chiave di crittografia dei dati](xref:security/data-protection/implementation/key-encryption-at-rest) per altre informazioni.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Condivisione dei cookie di autenticazione tra ASP.NET 4.x e le app ASP.NET Core

App ASP.NET 4.x che usano il middleware di autenticazione di Katana cookie può essere configurata per generare i cookie di autenticazione che sono compatibili con il middleware di autenticazione di cookie di ASP.NET Core. In questo modo l'aggiornamento a fasi le singole app di un sito di grandi dimensioni, fornendo un'esperienza SSO ottimale tra il sito.

Quando un'app Usa il middleware di autenticazione cookie Katana, chiama `UseCookieAuthentication` del progetto *Startup.Auth.cs* file. Progetti di app web ASP.NET 4.x creati con Visual Studio 2013 e versioni successive usano il middleware di autenticazione del cookie Katana per impostazione predefinita. Sebbene `UseCookieAuthentication` è obsoleto e non supportati per le app ASP.NET Core, la chiamata `UseCookieAuthentication` in un'app ASP.NET 4.x Usa Katana middleware di autenticazione del cookie è valido.

Un'app ASP.NET 4.x deve avere come destinazione .NET Framework 4.5.1 o versione successiva. In caso contrario, i pacchetti NuGet necessari esito negativo per l'installazione.

Per condividere i cookie di autenticazione tra un'app ASP.NET 4.x e un'app ASP.NET Core, configurare l'app ASP.NET Core, come indicato in precedenza, quindi configurare l'app ASP.NET 4.x seguendo questa procedura:

1. Installare il pacchetto [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) in ogni app ASP.NET 4.x.

2. Nelle *Startup.Auth.cs*, individuare la chiamata a `UseCookieAuthentication` e modificarla come indicato di seguito. Modificare il nome del cookie per corrispondere al nome usato dal middleware di autenticazione dei cookie di ASP.NET Core. Fornire un'istanza di un `DataProtectionProvider` inizializzati per il percorso di archiviazione delle chiavi di protezione dati comune. Assicurarsi che il nome dell'app è impostato per il nome dell'app più comuni usato da tutte le app che condividono i cookie, `SharedCookieApp` nell'app di esempio.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

Vedere le *CookieAuthWithIdentity.NETFramework* del progetto nel [esempi di codice](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).

Durante la generazione di un'identità utente, il tipo di autenticazione deve corrispondere al tipo definito in `AuthenticationType` impostati con `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

## <a name="use-a-common-user-database"></a>Usare un database utente comuni

Verificare che il sistema di identità per ogni app fa riferimento a livello di database utente stesso. In caso contrario, il sistema di identità genera errori in fase di esecuzione quando tenta di ottenere le informazioni nel cookie di autenticazione con le informazioni nel proprio database.

## <a name="additional-resources"></a>Risorse aggiuntive

<xref:host-and-deploy/web-farm>
