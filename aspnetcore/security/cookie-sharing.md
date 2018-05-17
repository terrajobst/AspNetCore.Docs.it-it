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
ms.openlocfilehash: 5f77377f168993d48686217adac54a75313766ec
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2018
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a>Condividere i cookie tra le app con ASP.NET e ASP.NET Core

Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)

Siti Web sono spesso costituite da un'App web singoli interagiscono. Per fornire un'esperienza single sign-on (SSO), le applicazioni web all'interno di un sito devono condividere i cookie di autenticazione. Per supportare questo scenario, lo stack di protezione dati consente la condivisione di autenticazione dei cookie Katana e il ticket di autenticazione ASP.NET Core cookie.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

L'esempio illustra la condivisione tra tre applicazioni che utilizzano l'autenticazione dei cookie cookie:

* App ASP.NET Core 2.0 Razor pagine senza utilizzare [ASP.NET Identity Core](xref:security/authentication/identity)
* Applicazione MVC ASP.NET Core 2.0 con identità di ASP.NET Core
* Applicazione MVC ASP.NET Framework 4.6.1 con identità di ASP.NET

Negli esempi che seguono:

* Il nome del cookie di autenticazione è impostata su un valore comune di `.AspNet.SharedCookie`.
* Il `AuthenticationType` è impostato su `Identity.Application` in modo esplicito o per impostazione predefinita.
* Viene utilizzato un nome di applicazione comune per abilitare il sistema di protezione dati condividere le chiavi di protezione dati (`SharedCookieApp`).
* `Identity.Application` viene utilizzato come schema di autenticazione. Qualsiasi schema viene utilizzato, sarà necessario utilizzarlo in modo coerente *all'interno e al* le app condivise cookie come lo schema predefinito o impostandolo in modo esplicito. Lo schema viene utilizzato quando crittografare e decrittografare i cookie, pertanto deve essere utilizzato uno schema coerente tra app.
* Un comune [chiave di protezione dati](xref:security/data-protection/implementation/key-management) viene utilizzato il percorso di archiviazione. L'app di esempio utilizza una cartella denominata *KeyRing* alla radice della soluzione per contenere le chiavi di protezione dati.
* Nelle app di ASP.NET Core [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) utilizzato per impostare il percorso di archiviazione chiavi. [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) consente di configurare un nome di app condiviso comune.
* Nell'app .NET Framework, il middleware di autenticazione cookie utilizza un'implementazione di [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider). `DataProtectionProvider` fornisce servizi di protezione dei dati per la crittografia e decrittografia dei dati di payload cookie di autenticazione. Il `DataProtectionProvider` istanza è isolata dal sistema di protezione di dati utilizzato da altre parti dell'app.
  * [DataProtectionProvider.Create (DirectoryInfo, azione\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accetta una [DirectoryInfo](/dotnet/api/system.io.directoryinfo) per specificare il percorso di archiviazione chiavi di protezione dati. L'app di esempio fornisce il percorso del *KeyRing* cartella `DirectoryInfo`. [DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) imposta il nome dell'applicazione comune.
  * [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) richiede il [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pacchetto NuGet. Per ottenere il pacchetto per ASP.NET 2.0 Core App e versioni successive, fare riferimento il [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage. Quando la destinazione è .NET Framework, aggiungere un riferimento pacchetto `Microsoft.AspNetCore.DataProtection.Extensions`.

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a>Condividere i cookie di autenticazione tra le applicazioni ASP.NET Core

Quando si usa ASP.NET Identity Core:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Nel `ConfigureServices` metodo, utilizzare il [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) metodo di estensione per configurare il servizio di protezione dati per i cookie.

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

Chiavi di protezione dati e il nome dell'applicazione devono essere condivise tra le app. Nelle app di esempio `GetKeyRingDirInfo` restituisce la posizione di archiviazione chiavi comuni per il [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metodo. Uso [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) per configurare un nome di app condiviso comune (`SharedCookieApp` nell'esempio). Per altre informazioni, vedere [configurazione della protezione dati](xref:security/data-protection/configuration/overview).

Vedere il *CookieAuthWithIdentity.Core* nel progetto di [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Nel `Configure` metodo, utilizzare il [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) impostare:

* Il servizio di protezione dati per i cookie.
* Il `AuthenticationScheme` in modo che corrisponda ASP.NET 4. x.

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

Quando si utilizzano direttamente i cookie:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

Chiavi di protezione dati e il nome dell'applicazione devono essere condivise tra le app. Nelle app di esempio `GetKeyRingDirInfo` restituisce la posizione di archiviazione chiavi comuni per il [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) metodo. Uso [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) per configurare un nome di app condiviso comune (`SharedCookieApp` nell'esempio). Per altre informazioni, vedere [configurazione della protezione dati](xref:security/data-protection/configuration/overview). 

Vedere il *CookieAuth.Core* nel progetto di [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).

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

## <a name="encrypting-data-protection-keys-at-rest"></a>La crittografia delle chiavi di protezione dati inattivi

Per le distribuzioni di produzione, è possibile configurare il `DataProtectionProvider` per crittografare le chiavi inattivi con DPAPI o un X509Certificate. Vedere [chiave di crittografia](xref:security/data-protection/implementation/key-encryption-at-rest) per ulteriori informazioni.

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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a>Condividere i cookie di autenticazione tra ASP.NET 4. x e ASP.NET Core App

ASP.NET 4. x App che usano Katana cookie middleware di autenticazione può essere configurata per generare i cookie di autenticazione che sono compatibili con il middleware di autenticazione ASP.NET Core cookie. In questo modo l'aggiornamento a fasi di singole app di un sito di grandi dimensioni fornendo un'esperienza SSO senza problemi tra il sito.

> [!TIP]
> Quando un'app Usa Katana cookie middleware di autenticazione, chiama `UseCookieAuthentication` del progetto *Startup.Auth.cs* file. Progetti di app web ASP.NET 4. x creati con Visual Studio 2013 e versioni successive, utilizzare il middleware di autenticazione del cookie Katana per impostazione predefinita.

> [!NOTE]
> Un'app ASP.NET 4. x deve avere come destinazione .NET Framework 4.5.1 o versione successiva. In caso contrario, i pacchetti NuGet necessari non essere installato.

Per condividere i cookie di autenticazione tra le app ASP.NET 4. x e ASP.NET Core, configurare l'applicazione ASP.NET di base, come descritto in precedenza, quindi configurare le app ASP.NET 4. x seguendo la procedura seguente.

1. Installare il pacchetto [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) in ogni app ASP.NET 4. x.

2. In *Startup.Auth.cs*, individuare la chiamata a `UseCookieAuthentication` e modificarla come segue. Modificare il nome del cookie per corrispondere al nome utilizzato dal middleware di autenticazione del cookie di ASP.NET Core. Fornire un'istanza di un `DataProtectionProvider` inizializzato al percorso di archiviazione chiavi protezione dati comune. Verificare che il nome dell'applicazione è impostato il nome dell'app comuni utilizzati da tutte le app che condividono i cookie, `SharedCookieApp` nell'app di esempio.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

Vedere il *CookieAuthWithIdentity.NETFramework* nel progetto di [codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([come scaricare](xref:tutorials/index#how-to-download-a-sample)).

Quando si genera un'identità utente, il tipo di autenticazione deve corrispondere al tipo definito in `AuthenticationType` impostata con `UseCookieAuthentication`.

*Models/IdentityModels.cs*:

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Impostare il `CookieManager` all'interoperabilità `ChunkingCookieManager` pertanto il formato di suddivisione in blocchi è compatibile.

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

## <a name="use-a-common-user-database"></a>Utilizzare un database utente comuni

Verificare che il sistema di identità per ogni app fa riferimento allo stesso database utente. In caso contrario, il sistema di identità genera errori in fase di esecuzione durante il tentativo di associare le informazioni nel cookie di autenticazione e le informazioni sul proprio database.
