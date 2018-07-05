---
title: Introduzione all'identità in ASP.NET Core
author: rick-anderson
description: Usare l'identità con un'app ASP.NET Core. Include, i requisiti di password di impostazione (RequireDigit, RequiredLength, RequiredUniqueChars e altro ancora).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: c231a7619a4433ce004342ce68564e4c3892e702
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829302"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Introduzione all'identità in ASP.NET Core

Dal [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), e [Steve Smith](https://ardalis.com/)

ASP.NET Core Identity è un sistema di membership che ti consente di aggiungere funzionalità di accesso all'applicazione. li utenti possono creare un account ed effettuare il login con username e password o possono utilizzare un provider di accesso esterno come ad esempio Facebook, Google, Account Microsoft, Twitter o altri.

Puoi configurare ASP.NET Identity Core per l'utilizzo di un database SQL Server per archiviare i nomi utente, password e i dati di profilo. In alternativa, è possibile usare il proprio archivio permanente, ad esempio, un'archiviazione tabelle di Azure. Questo documento contiene istruzioni per Visual Studio e per l'utilizzo dell'interfaccia a riga di comando CLI.

[Visualizzare o scaricare il codice di esempio.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Come scaricare)](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Panoramica di Identity

In questo argomento, imparerai ad usare ASP.NET Identity Core per aggiungere le funzionalità di registrazione, accesso e disconnessione di un utente. Per istruzioni più dettagliate sulla creazione di App tramite ASP.NET Core Identity, vedere la sezione passaggi successivi alla fine di questo articolo.

1. Creare un progetto di applicazione Web ASP.NET Core con account utente individuali.

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   In Visual Studio, selezionare **File** > **New** > **progetto**. Selezionare **applicazione Web ASP.NET Core** e fare clic su **OK**.

   ![Finestra di dialogo Nuovo progetto](identity/_static/01-new-project.png)

   Selezionare un ASP.NET Core **applicazione Web (Model-View-Controller)** per ASP.NET Core 2.x, quindi selezionare **Modifica autenticazione**.

   ![Finestra di dialogo Nuovo progetto](identity/_static/02-new-project.png)

   Appare una finestra di dialogo con le opzioni di autenticazione. Selezionare **account utente individuali** e fare clic su **OK** per tornare alla finestra di dialogo precedente.

   ![Finestra di dialogo Nuovo progetto](identity/_static/03-new-project-auth.png)

   Selezionando **singoli account utente di** indica a Visual Studio per creare modelli, ViewModel, visualizzazioni, controller e gli altri asset necessari per l'autenticazione come parte del modello di progetto.

   # <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

   Se si usa .NET Core CLI, creare il nuovo progetto usando `dotnet new mvc --auth Individual`. Questo comando crea un nuovo progetto con lo stesso codice di modello di identità in Visual Studio creato.

   Il progetto creato contiene il `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pacchetto, che rende persistenti i dati di identità e dello schema a SQL Server con [Entity Framework Core](https://docs.microsoft.com/ef/).

   ---

2. Configurare i servizi di identità e aggiungere middleware in `Startup`.

   I servizi di identità vengono aggiunti all'applicazione nel metodo `ConfigureServices` della classe `Startup` classe:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Questi servizi sono resi disponibili per l'applicazione attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).

   Identità è abilitata per l'applicazione chiamando `UseAuthentication` nel metodo`Configure`. `UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Questi servizi sono resi disponibili per l'applicazione attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).

   Identità è abilitata per l'applicazione chiamando `UseIdentity` nel metodo`Configure`. `UseIdentity` Aggiunge l'autenticazione basata su cookie [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   Per altre informazioni sul processo di avvio applicazione, vedere [avvio dell'applicazione](xref:fundamentals/startup).

3. Creare un utente.

   Avviare l'applicazione e quindi fare clic sui **registrare** collegamento.

   Se questa è la prima volta che si sta eseguendo questa azione, sarà necessario per eseguire le migrazioni. L'applicazione viene richiesto di **applicare le migrazioni**. Se necessario, aggiornare la pagina.

   ![Applicare le migrazioni sender](identity/_static/apply-migrations.png)

   In alternativa, è possibile verificare l'utilizzo di ASP.NET Identity Core con l'app senza un database persistente utilizzando un database in memoria.  Per utilizzare un database in memoria, aggiungere il pacchetto `Microsoft.EntityFrameworkCore.InMemory` pall'App e modificare la chiamata dell'applicazione a `AddDbContext` in `ConfigureServices` come indicato di seguito:

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   Quando l'utente sceglie il collegamento**registrare** il metodo azione`Register` viene richiamato nell `AccountController`. L'azione`Register` crea l'utente chiamandoconsente all'utente chiamando  `CreateAsync` sull'oggetto  `_userManager`(fornito all `AccountController` tramite dependency injection):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   Se l'utente è stato creato correttamente, l'utente è connesso tramite la chiamata al metodo `_signInManager.SignInAsync`.

   **Nota:** visualizzare [conferma dell'account](xref:security/authentication/accconfirm#prevent-login-at-registration) per i passaggi per impedire l'accesso immediato al momento della registrazione.

4. Accedi.

   Gli utenti possono accedere facendo clic sul collegamento**Accedi** nella parte superiore del sito, o possono essere reindirizzati alla pagina di accesso se si tenta di accedere ad una parte del sito che richiede l'autorizzazione. Quando l'utente invia il form nella pagina di accesso, il metodo di azione`AccountController`nell `Login`viene invocato.

   L'azione `Login` chiama `PasswordSignInAsync` sul `_signInManager` oggetto (fornito all `AccountController` tramite dependency injection).

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   La classe base `Controller` espone una proprietà  `User` a cui è possibile accedere dai metodi del controller. Ad esempio, è possibile enumerare `User.Claims` e prendere decisioni di autorizzazione. Per ulteriori informazioni, vedere [autorizzazione](xref:security/authorization/index).

5. Effettuare la disconnessione.

   Fare clic sul collegamento **disconnettersi** per chiamare l'azione `LogOut`.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Il codice precedente chiama il metodo `_signInManager.SignOutAsync`. Il metodo `SignOutAsync` cancella attestazioni dell'utente archiviate in un cookie.

<a name="pw"></a>
6. Configurazione.

   Identità dispone di alcuni comportamenti predefiniti che possono essere sostituiti nella classe di avvio dell'app. Le `IdentityOptions` non devono essere configurate quando si utilizzano i comportamenti predefiniti. Il codice seguente imposta diverse opzioni sul livello di sicurezza della password:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   Per altre informazioni su come configurare l'identità, vedere [configurare identità](xref:security/authentication/identity-configuration).

   È anche possibile configurare il tipo di dati della chiave primaria, vedere [tipo di dati di identità di configurare le chiavi primarie](xref:security/authentication/identity-primary-key-configuration).

7. Visualizzare il database.

   Se l'app usa un database di SQL Server (impostazione predefinita in Windows e per gli utenti di Visual Studio), è possibile visualizzare il database dell'applicazione creata. È possibile utilizzare **SQL Server Management Studio**. In alternativa, da Visual Studio, selezionare **vista** > **Esplora oggetti di SQL Server**. Connettersi a **(localdb) \MSSQLLocalDB**. Il database con un nome corrispondente `aspnet-<name of your project>-<guid>` viene visualizzato.

   ![Menu di scelta rapida nella tabella AspNetUsers database](identity/_static/04-db.png)

   Espandere il database e le relative **tabelle**, quindi fare clic con il pulsante destro del mouse sulla tabella **dbo. AspNetUsers** e selezionare **Visualizza dati**.

8. Verificare il funzionamento dell'identità

    l template di progetto predefinito per l'*applicazione Web di ASP.NET Core* consente agli utenti di accedere a qualsiasi azione nell'applicazione senza effettuare l'accesso. Per verificare il funzionamento di ASP.NET Identity, aggiungere un attributo `[Authorize]` all'azione `About` del controller `Home`.

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Eseguire il progetto utilizzando **Ctrl** + **F5** e passare alla **sulle** pagina. Solo gli utenti autenticati possono accedere le **sulle** pagina a questo punto, in modo che ASP.NET si viene reindirizzati alla pagina di accesso per accedere o registrare.

    # <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

    Aprire una finestra di comando e passare alla radice del progetto directory contenente il `.csproj` file. Eseguire la [dotnet eseguire](/dotnet/core/tools/dotnet-run) comando per eseguire l'app:

    ```csharp
    dotnet run 
    ```

    Passare l'URL specificato nell'output il [dotnet eseguire](/dotnet/core/tools/dotnet-run) comando. L'URL deve puntare a `localhost` con un numero di porta generato. Passare il **sulle** pagina. Solo gli utenti autenticati possono accedere le **sulle** pagina a questo punto, in modo che ASP.NET si viene reindirizzati alla pagina di accesso per accedere o registrare.

    ---

## <a name="identity-components"></a>Componenti delle identità

L'assembly di riferimento principale per il sistema di identità è `Microsoft.AspNetCore.Identity`. Questo pacchetto contiene il set principale di interfacce per ASP.NET Core Identity e viene incluso per `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Queste dipendenze sono necessarie per usare il sistema di identità nelle applicazioni ASP.NET Core:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Contiene i tipi necessari per l'uso di identità con Entity Framework Core.

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core è una tecnologia di accesso ai dati consigliati di Microsoft per i database relazionali come SQL Server. Per i test, è possibile usare `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies` -Middleware che consente a un'app usare l'autenticazione basata su cookie.

## <a name="migrating-to-aspnet-core-identity"></a>Migrazione ad ASP.NET Core Identity

Per altre informazioni e indicazioni sulla migrazione di identità esistente vedere dei negozi [eseguire la migrazione di autenticazione e identità](xref:migration/identity).

## <a name="setting-password-strength"></a>Impostare la complessità della password

Visualizzare [configurazione](#pw) per un esempio che imposta i requisiti di lunghezza minima della password.

## <a name="next-steps"></a>Passaggi successivi

* [Eseguire la migrazione di autenticazione e identità](xref:migration/identity)
* [Conferma account e recupero password](xref:security/authentication/accconfirm)
* [Autenticazione a due fattori con SMS](xref:security/authentication/2fa)
* [Facebook, Google e autenticazione basata su provider esterni](xref:security/authentication/social/index)
