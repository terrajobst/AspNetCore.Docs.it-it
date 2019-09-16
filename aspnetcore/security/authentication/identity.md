---
title: Introduzione all'identità in ASP.NET Core
author: rick-anderson
description: Usare l'identità con un'app ASP.NET Core. Informazioni su come impostare i requisiti per le password (RequireDigit, RequiredLength, RequiredUniqueChars e altro).
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 325a61e6038e79b9a0db72c8360a5cbff2c8ddae
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011202"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Introduzione all'identità in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core identità è un sistema di appartenenza che aggiunge la funzionalità di accesso alle app ASP.NET Core. Gli utenti possono creare un account con le informazioni di accesso archiviate in Identity oppure possono usare un provider di accesso esterno. I provider di accesso esterni supportati includono [Facebook, Google, account Microsoft e Twitter](xref:security/authentication/social/index).

L'identità può essere configurata utilizzando un database SQL Server per archiviare i nomi utente, le password e i dati di profilo. In alternativa, è possibile usare un altro archivio permanente, ad esempio archiviazione tabelle di Azure.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([come scaricare)](xref:index#how-to-download-a-sample)).

In questo argomento si apprenderà come usare l'identità per la registrazione, l'accesso e la disconnessione di un utente. Per istruzioni più dettagliate sulla creazione di app che usano l'identità, vedere la sezione passaggi successivi alla fine di questo articolo.

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a>AddDefaultIdentity e AddIdentity

[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) è stato introdotto in ASP.NET Core 2,1. La `AddDefaultIdentity` chiamata a è simile alla chiamata a quanto segue:

* [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [AddDefaultUI](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [AddDefaultTokenProviders](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

Per altre informazioni, vedere [origine AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a>Creare un'app Web con l'autenticazione

Creare un progetto di applicazione Web di ASP.NET Core con singoli account utente.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Selezionare **File** > **Nuovo** > **Progetto**.
* Selezionare **Applicazione Web ASP.NET Core**. Denominare il progetto **app Web 1** in modo che abbia lo stesso spazio dei nomi del download del progetto. Fare clic su **OK**.
* Selezionare un' **applicazione Web**ASP.NET Core, quindi selezionare **Modifica autenticazione**.
* Selezionare **singoli account utente** e fare clic su **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

Il progetto generato fornisce [ASP.NET Core identità](xref:security/authentication/identity) come [libreria di classi Razor](xref:razor-pages/ui-class). La libreria della classe Razor Identity espone gli endpoint con `Identity` l'area. Ad esempio:

* /Identity/Account/Login
* /Identity/Account/Logout
* /Identity/Account/Manage

### <a name="apply-migrations"></a>Applicare le migrazioni

Applicare le migrazioni per inizializzare il database.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Eseguire il comando seguente nella console di gestione pacchetti (PMC):

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```cli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a>Registro di test e account di accesso

Eseguire l'app e registrare un utente. A seconda delle dimensioni dello schermo, potrebbe essere necessario selezionare l'interruttore di spostamento per visualizzare i collegamenti di **Registro** e di **accesso** .

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a>Configurare i servizi di identità

I servizi vengono aggiunti `ConfigureServices`in. Il modello tipico consiste nel chiamare tutti i metodi `Add{Service}` e quindi chiamare tutti i metodi `services.Configure{Service}`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

Il codice precedente configura l'identità con i valori delle opzioni predefinite. I servizi vengono resi disponibili per l'applicazione tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).

   L'identità viene abilitata chiamando [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_). `UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   I servizi vengono resi disponibili per l'applicazione tramite l' [inserimento delle dipendenze](xref:fundamentals/dependency-injection).

   Identità è abilitata per l'applicazione chiamando `UseAuthentication` nel metodo`Configure`. `UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Questi servizi vengono resi disponibili per l'applicazione tramite l' [inserimento delle dipendenze](xref:fundamentals/dependency-injection).

   Identità è abilitata per l'applicazione chiamando `UseIdentity` nel metodo`Configure`. `UseIdentity`aggiunge il [middleware](xref:fundamentals/middleware/index) di autenticazione basato su cookie alla pipeline di richieste.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

Per ulteriori informazioni, vedere la [Classe IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) e l' [avvio dell'applicazione](xref:fundamentals/startup).

## <a name="scaffold-register-login-and-logout"></a>Registrazione, accesso e disconnessione del patibolo

Seguire l' [identità del patibolo in un progetto Razor con](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) le istruzioni di autorizzazione per generare il codice illustrato in questa sezione.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Aggiungere i file di registro, di accesso e di disconnessione.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Se il progetto è stato creato con il nome **app Web 1**, eseguire i comandi seguenti. In caso contrario, utilizzare lo spazio dei `ApplicationDbContext`nomi corretto per:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

PowerShell usa il punto e virgola come separatore di comandi. Quando si usa PowerShell, usare il carattere di escape per il punto e virgola nell'elenco dei file o inserire l'elenco di file tra virgolette doppie, come illustrato nell'esempio precedente.

---

### <a name="examine-register"></a>Esaminare il registro

::: moniker range=">= aspnetcore-2.1"

   Quando un utente fa clic sul collegamento **Register** , `RegisterModel.OnPostAsync` viene richiamata l'azione. L'utente viene creato da [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) sull' `_userManager` oggetto. `_userManager`viene fornito dall'inserimento delle dipendenze:

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Quando un utente fa clic sul collegamento **Register** , `Register` l'azione viene richiamata in. `AccountController` L'azione`Register` crea l'utente chiamandoconsente all'utente chiamando  `CreateAsync` sull'oggetto  `_userManager`(fornito all `AccountController` tramite dependency injection):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   Se l'utente è stato creato correttamente, l'utente è connesso tramite la chiamata al metodo `_signInManager.SignInAsync`.

   **Nota:** Vedere la [conferma dell'account](xref:security/authentication/accconfirm#prevent-login-at-registration) per i passaggi per impedire l'accesso immediato alla registrazione.

### <a name="log-in"></a>Accedi

::: moniker range=">= aspnetcore-2.1"

Il modulo di accesso viene visualizzato quando:

* Il collegamento **Accedi** è selezionato.
* Un utente tenta di accedere a una pagina con restrizioni che non è autorizzata ad accedere **o** quando non è stata autenticata dal sistema.

Quando viene inviato il modulo nella pagina di accesso, viene `OnPostAsync` chiamata l'azione. `PasswordSignInAsync`viene chiamato sull' `_signInManager` oggetto, fornito dall'inserimento delle dipendenze.

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   La classe base `Controller` espone una proprietà  `User` a cui è possibile accedere dai metodi del controller. Ad esempio, è possibile enumerare `User.Claims` e prendere decisioni di autorizzazione. Per altre informazioni, vedere <xref:security/authorization/introduction>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Il modulo di accesso viene visualizzato quando gli utenti selezionano il collegamento **Accedi** o vengono reindirizzati quando si accede a una pagina che richiede l'autenticazione. Quando l'utente invia il form nella pagina di accesso, il metodo di azione`AccountController`nell `Login`viene invocato.

L'azione `Login` chiama `PasswordSignInAsync` sul `_signInManager` oggetto (fornito all `AccountController` tramite dependency injection).

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

La classe di`Controller` base `PageModel`(o) espone `User` una proprietà. Ad esempio, `User.Claims` può essere enumerato per prendere decisioni di autorizzazione.

::: moniker-end

### <a name="log-out"></a>Disconnetti

::: moniker range=">= aspnetcore-2.1"

Il collegamento **Disconnetti** richiama l' `LogoutModel.OnPost` azione. 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) Cancella le attestazioni dell'utente archiviate in un cookie.

Post è specificato in *pages/Shared/_LoginPartial. cshtml*:

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Fare clic sul collegamento **disconnettersi** per chiamare l'azione `LogOut`.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Il codice precedente chiama il `_signInManager.SignOutAsync` metodo. Il metodo `SignOutAsync` cancella attestazioni dell'utente archiviate in un cookie.

::: moniker-end

## <a name="test-identity"></a>Identità del test

I modelli di progetto Web predefiniti consentono l'accesso anonimo alle Home page. Per verificare l'identità, [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) aggiungere alla pagina privacy.

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

Se è stato eseguito l'accesso, disconnettersi. Eseguire l'app e selezionare il collegamento per la **privacy** . Si verrà reindirizzati alla pagina di accesso.

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>Esplora identità

Per esplorare l'identità in modo più dettagliato:

* [Crea origine interfaccia utente con identità completa](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* Esaminare l'origine di ogni pagina ed eseguire un'istruzione alla volta nel debugger.

::: moniker-end

## <a name="identity-components"></a>Componenti Identity

::: moniker range=">= aspnetcore-2.1"

Tutti i pacchetti NuGet dipendenti dall'identità sono inclusi nel [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

::: moniker-end

Il pacchetto primario per Identity è [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/). Questo pacchetto contiene il set principale di interfacce per ASP.NET Core identità ed è incluso in `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

## <a name="migrating-to-aspnet-core-identity"></a>Migrazione a identità ASP.NET Core

Per altre informazioni e indicazioni sulla migrazione dell'archivio identità esistente, vedere [eseguire la migrazione dell'autenticazione e dell'identità](xref:migration/identity).

## <a name="setting-password-strength"></a>Impostazione della complessità della password

Vedere [configurazione](#pw) per un esempio che consente di impostare i requisiti minimi per le password.

## <a name="next-steps"></a>Fasi successive

* [Configurare Identity](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
