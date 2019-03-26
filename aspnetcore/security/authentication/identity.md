---
title: Introduzione all'identità in ASP.NET Core
author: rick-anderson
description: Usare l'identità con un'app ASP.NET Core. Informazioni su come impostare i requisiti di password (RequireDigit RequiredLength, RequiredUniqueChars e altro ancora).
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: efe8119c2d18db50dce3df57fa0e300d0d0df2cd
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488673"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Introduzione all'identità in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core Identity è un sistema di appartenenza che aggiunge funzionalità di accesso per le app ASP.NET Core. Gli utenti possono creare un account con le informazioni di accesso archiviate nell'identità o è possibile usare un provider di accesso esterno. Provider di accesso esterni supportati includono [Facebook, Google, Account Microsoft e Twitter](xref:security/authentication/social/index).

Identità può essere configurato usando un database di SQL Server per archiviare nomi utente, password e i dati del profilo. In alternativa, un altro archivio permanente utilizzabile, ad esempio, archiviazione tabelle di Azure.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([come scaricare)](xref:index#how-to-download-a-sample)).

In questo argomento, informazioni su come usare l'identità per registrare, accedere e disconnettersi da un utente. Per istruzioni più dettagliate sulla creazione di App che usano l'identità, vedere la sezione passaggi successivi alla fine di questo articolo.

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a>AddDefaultIdentity e AddIdentity

[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) è stato introdotto in ASP.NET Core 2.1. La chiamata `AddDefaultIdentity` è simile alla chiamata seguente:

* [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [AddDefaultUI](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [AddDefaultTokenProviders](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

Visualizzare [AddDefaultIdentity origine](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) per altre informazioni.

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a>Creare un'app Web con l'autenticazione

Creare un progetto di applicazione Web ASP.NET Core con account utente individuali.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Selezionare **File** > **Nuovo** > **Progetto**.
* Selezionare **Applicazione Web ASP.NET Core**. Denominare il progetto **App Web 1** avere lo stesso spazio dei nomi come il download del progetto. Fare clic su **OK**.
* Selezionare un ASP.NET Core **applicazione Web**, quindi selezionare **Modifica autenticazione**.
* Selezionare **account utente individuali** e fare clic su **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

Fornisce il progetto generato [ASP.NET Core Identity](xref:security/authentication/identity) come una [libreria di classi Razor](xref:razor-pages/ui-class). La libreria di classi di identità Razor espone gli endpoint con il `Identity` area. Ad esempio:

* / Identità/Account/Login
* / Identità/Account/disconnessione
* / Identità/Account/gestire

### <a name="apply-migrations"></a>Applicare le migrazioni

Applicare le migrazioni per inizializzare il database.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Eseguire il comando seguente in Package Manager Console (console di gestione pacchetti):

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```cli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a>Registrazione di test e account di accesso

Eseguire l'app e registrare un utente. A seconda delle dimensioni dello schermo, potrebbe essere necessario selezionare il pulsante Attiva/Disattiva navigazione per vedere le **registrare** e **Login** collegamenti.

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a>Configurare i servizi di identità

Aggiunta di servizi `ConfigureServices`. Il modello tipico consiste nel chiamare tutti i metodi `Add{Service}` e quindi chiamare tutti i metodi `services.Configure{Service}`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

Il codice precedente configura l'identità con valori predefiniti dell'opzione. Servizi sono resi disponibili per l'app tramite il [inserimento delle dipendenze](xref:fundamentals/dependency-injection).

   Vengono abilitate le identità chiamando [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_). `UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Servizi sono resi disponibili per l'applicazione attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).

   Identità è abilitata per l'applicazione chiamando `UseAuthentication` nel metodo`Configure`. `UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Questi servizi sono resi disponibili per l'applicazione attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).

   Identità è abilitata per l'applicazione chiamando `UseIdentity` nel metodo`Configure`. `UseIdentity` Aggiunge l'autenticazione basata su cookie [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

Per altre informazioni, vedere la [classe IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) e [avvio dell'applicazione](xref:fundamentals/startup).

## <a name="scaffold-register-login-and-logout"></a>Eseguire lo scaffolding di registrazione, l'account di accesso e disconnessione

Seguire le [lo scaffolding di identità in un progetto Razor con autorizzazione](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) istruzioni per generare il codice riportato in questa sezione.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Aggiungere i file di registrazione, l'account di accesso e disconnessione.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Se è stato creato il progetto con nome **App Web 1**, eseguire i comandi seguenti. In caso contrario, usare lo spazio dei nomi corretto per il `ApplicationDbContext`:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

PowerShell Usa un punto e virgola come separatore di comandi. Quando si usa PowerShell, eseguire l'escape di punti e virgola nell'elenco di file o inserire l'elenco di file tra virgolette doppie, come illustrato nell'esempio precedente.

---

### <a name="examine-register"></a>Esaminare Register

::: moniker range=">= aspnetcore-2.1"

   Quando un utente sceglie il **registrare** collegamento, il `RegisterModel.OnPostAsync` azione viene richiamata. L'utente viene creato mediante [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) nel `_userManager` oggetto. `_userManager` viene fornito dall'inserimento delle dipendenze):

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Quando un utente sceglie il **registrare** collegamento, il `Register` azione viene richiamata in `AccountController`. L'azione`Register` crea l'utente chiamandoconsente all'utente chiamando  `CreateAsync` sull'oggetto  `_userManager`(fornito all `AccountController` tramite dependency injection):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   Se l'utente è stato creato correttamente, l'utente è connesso tramite la chiamata al metodo `_signInManager.SignInAsync`.

   **Nota:** Visualizzare [conferma dell'account](xref:security/authentication/accconfirm#prevent-login-at-registration) per i passaggi per impedire l'accesso immediato al momento della registrazione.

### <a name="log-in"></a>Accedi

::: moniker range=">= aspnetcore-2.1"

Il modulo di accesso viene visualizzato quando:

* Il **Accedi** Seleziona collegamento.
* Un utente prova ad accedere a una pagina con restrizioni che non sono autorizzati ad accedere **o** quando non sono stati autenticati dal sistema.

Quando viene inviato il modulo nella pagina di accesso, il `OnPostAsync` viene chiamata azione. `PasswordSignInAsync` viene chiamato sul `_signInManager` oggetto (fornito dall'inserimento di dipendenze).

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   La classe base `Controller` espone una proprietà  `User` a cui è possibile accedere dai metodi del controller. Ad esempio, è possibile enumerare `User.Claims` e prendere decisioni di autorizzazione. Per altre informazioni, vedere <xref:security/authorization/introduction>.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Il modulo di accesso viene visualizzato quando gli utenti selezionano il **Accedi** collegare o vengono reindirizzati quando si accede a una pagina che richiede l'autenticazione. Quando l'utente invia il form nella pagina di accesso, il metodo di azione`AccountController`nell `Login`viene invocato.

L'azione `Login` chiama `PasswordSignInAsync` sul `_signInManager` oggetto (fornito all `AccountController` tramite dependency injection).

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

La base (`Controller` oppure `PageModel`) classe espone un `User` proprietà. Ad esempio, `User.Claims` può essere enumerata per prendere decisioni di autorizzazione.

::: moniker-end

### <a name="log-out"></a>Effettuare la disconnessione

::: moniker range=">= aspnetcore-2.1"

Il **disconnettersi** collegamento richiama il `LogoutModel.OnPost` azione. 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) Cancella le attestazioni dell'utente archiviate in un cookie. Dopo la chiamata non di reindirizzamento `SignOutAsync` o l'utente verrà **non** disconnessione.

Post viene specificato nella *Pages/Shared/_LoginPartial.cshtml*:

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   Fare clic sul collegamento **disconnettersi** per chiamare l'azione `LogOut`.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Il codice precedente chiama il `_signInManager.SignOutAsync` (metodo). Il metodo `SignOutAsync` cancella attestazioni dell'utente archiviate in un cookie.

::: moniker-end

## <a name="test-identity"></a>Verificare l'identità

I modelli di progetto web predefinita è consentire l'accesso anonimo alle pagine home. Per verificare l'identità, aggiungere [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) alla pagina di Privacy.

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

Se si è connessi, disconnettersi. Eseguire l'app e selezionare il **Privacy** collegamento. Si verrà reindirizzati alla pagina di accesso.

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>Esplorare identità

Per ulteriori dettagli sulle identità:

* [Creare identità completa dell'interfaccia utente origine](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* Esaminare l'origine di ogni pagina e procedere con il debugger.

::: moniker-end

## <a name="identity-components"></a>Componenti delle identità

::: moniker range=">= aspnetcore-2.1"

Tutte le identità NuGet pacchetti dipendenti sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

Il pacchetto per l'identità primario viene [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/). Questo pacchetto contiene il set principale di interfacce per ASP.NET Core Identity e viene incluso per `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

## <a name="migrating-to-aspnet-core-identity"></a>Migrazione ad ASP.NET Core Identity

Per altre informazioni e istruzioni sulla migrazione archivio di identità esistente, vedere [eseguire la migrazione di autenticazione e identità](xref:migration/identity).

## <a name="setting-password-strength"></a>Impostare la complessità della password

Visualizzare [configurazione](#pw) per un esempio che imposta i requisiti di lunghezza minima della password.

## <a name="next-steps"></a>Passaggi successivi

* [Configurare Identity](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
