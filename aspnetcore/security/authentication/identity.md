---
title: "Introduzione all'identità su ASP.NET Core"
author: rick-anderson
description: "Usare l'identità con un'applicazione ASP.NET di base. Include, impostazione dei requisiti delle password (RequireDigit, RequiredLength, RequiredUniqueChars e altro ancora)."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity
ms.openlocfilehash: a84c5f1d4cf802ee0c4116d2a02bdbfbab9aa72b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Introduzione all'identità su ASP.NET Core

Da [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), e [Steve Smith](https://ardalis.com/)

ASP.NET Core Identity è un sistema di membership che ti consente di aggiungere funzionalità di accesso all'applicazione. li utenti possono creare un account ed effettuare il login con username e password o possono utilizzare un provider di accesso esterno come ad esempio Facebook, Google, Account Microsoft, Twitter o altri.

Puoi configurare ASP.NET Identity Core per l'utilizzo di un database SQL Server per archiviare i nomi utente, password e i dati di profilo. In alternativa, è possibile utilizzare un archivio permanente, ad esempio, un archivio tabelle di Azure. Questo documento contiene istruzioni per Visual Studio e per l'utilizzo dell'interfaccia a riga di comando CLI.

[Consente di visualizzare o scaricare il codice di esempio.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Come scaricare)](https://docs.microsoft.com/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Panoramica di Identity

In questo argomento, imparerai ad usare ASP.NET Identity Core per aggiungere le funzionalità di registrazione, accesso e disconnessione di un utente. Per istruzioni più dettagliate sulla creazione di App scritte in ASP.NET Identity Core, vedere la sezione passaggi successivi alla fine di questo articolo.

1.  Creare un progetto di applicazione Web di ASP.NET Core con singoli account utente.

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    In Visual Studio, selezionare **File** > **New** > **progetto**. Selezionare **applicazione Web di ASP.NET Core** e fare clic su **OK**.

    ![Finestra di dialogo Nuovo progetto](identity/_static/01-new-project.png)

    Selezionare un ASP.NET Core **applicazione Web (Model-View-Controller)** per ASP.NET Core 2. x, quindi selezionare **Modifica autenticazione**.

    ![Finestra di dialogo Nuovo progetto](identity/_static/02-new-project.png)

    Appare una finestra di dialogo con le opzioni di autenticazione. Selezionare **singoli account utente di** e fare clic su **OK** per tornare alla finestra di dialogo precedente.

    ![Finestra di dialogo Nuovo progetto](identity/_static/03-new-project-auth.png)

    Selezione **singoli account utente di** indica a Visual Studio per creare modelli, ViewModel, visualizzazioni, controller e altre risorse necessarie per l'autenticazione come parte del modello di progetto.

    # <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

    Se si utilizza l'interfaccia CLI Core .NET, creare il nuovo progetto utilizzando ``dotnet new mvc --auth Individual``. Questo comando crea un nuovo progetto con lo stesso codice di modello di identità che Visual Studio crea.

    Il progetto creato contiene il `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pacchetto, che mantiene i dati di identità e schema a SQL Server utilizzando [Entity Framework Core](https://docs.microsoft.com/ef/).

    ---

2.  Configurare i servizi di identità e aggiungere middleware `Startup`.

    I servizi di identità vengono aggiunti all'applicazione nel metodo `ConfigureServices` della classe `Startup`:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]
    
    Questi servizi vengono resi disponibili per l'applicazione tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection).
    
    Identità è abilitata per l'applicazione chiamando `UseAuthentication` nel metodo `Configure`. `UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]
    
    Questi servizi vengono resi disponibili per l'applicazione tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection).
    
    Identità è abilitata per l'applicazione chiamando `UseIdentity` nel `Configure` metodo. `UseIdentity` Aggiunge l'autenticazione basata su cookie [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.
        
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    Per ulteriori informazioni sull'avvio dell'applicazione, vedere [avvio dell'applicazione](xref:fundamentals/startup).

3.  Creare un utente.

    Avviare l'applicazione e quindi fare clic su di **registrare** collegamento.

    Se questa è la prima volta che si sta eseguendo questa azione, potrebbe essere necessario eseguire le migrazioni. L'applicazione viene richiesto di **applicare le migrazioni**. Se necessario, aggiornare la pagina.
    
    ![Applicare le migrazioni pagina](identity/_static/apply-migrations.png)
    
    In alternativa, è possibile verificare l'utilizzo di ASP.NET Identity Core con l'app senza un database persistente utilizzando un database in memoria. Per utilizzare un database in memoria, aggiungere il pacchetto ``Microsoft.EntityFrameworkCore.InMemory`` all'App e modificare la chiamata dell'applicazione a ``AddDbContext`` in ``ConfigureServices`` come indicato di seguito:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    Quando l'utente sceglie il collegamento **registrare**, il metodo azione ``Register`` viene richiamato nell' ``AccountController``. L'azione ``Register``crea l'utente chiamando `CreateAsync` sull'oggetto `_userManager` (fornito all' ``AccountController`` tramite dependency injection):
 
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    Se l'utente è stato creato correttamente, l'utente è connesso tramite la chiamata al metodo ``_signInManager.SignInAsync``.

    **Nota:** vedere [account conferma](xref:security/authentication/accconfirm#prevent-login-at-registration) per la procedura impedire l'accesso immediato al momento della registrazione.
 
4.  Accedi.
 
    Gli utenti possono accedere facendo clic sul collegamento **Accedi** nella parte superiore del sito, o possono essere reindirizzati alla pagina di accesso se si tenta di accedere ad una parte del sito che richiede l'autorizzazione. Quando l'utente invia il form nella pagina di accesso, il metodo di azione ``Login`` nell'``AccountController`` viene invocato.

    L'azione ``Login`` chiama ``PasswordSignInAsync`` sull'oggetto ``_signInManager`` (fornito all' ``AccountController`` tramite dependency injection).

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    La classe base ``Controller`` espone una proprietà ``User`` a cui è possibile accedere dai metodi del controller. Ad esempio, è possibile enumerare `User.Claims` e prendere decisioni di autorizzazione. Per ulteriori informazioni, vedere [autorizzazione](xref:security/authorization/index).
 
5.  Effettuare la disconnessione.
 
    Fare clic sul collegamento **disconnettersi** chiama l'azione `LogOut`.
 
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    Il codice precedente qui sopra chiama il metodo `_signInManager.SignOutAsync`. Il metodo `SignOutAsync` cancella attestazioni dell'utente archiviate in un cookie.
 
<a name="pw"></a>
6.  Configurazione.

    Identità dispone di alcuni comportamenti predefiniti che possono essere sostituiti nella classe di avvio dell'app. Le `IdentityOptions` non devono essere configurate quando si utilizzano i comportamenti predefiniti. Il codice seguente imposta diverse opzioni sul livello di sicurezza della password:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

    ---
    
    Per ulteriori informazioni su come configurare l'identità, vedere [configurare identità](xref:security/authentication/identity-configuration).
    
    È anche possibile configurare il tipo di dati della chiave primaria, vedere [tipo di dati di identità di configurare le chiavi primarie](xref:security/authentication/identity-primary-key-configuration).
 
7.  Visualizzare il database.

    Se l'app usa un database di SQL Server (impostazione predefinita in Windows e per gli utenti di Visual Studio), è possibile visualizzare il database dell'applicazione creata. È possibile utilizzare **SQL Server Management Studio**. In alternativa, da Visual Studio, selezionare **vista** > **Esplora oggetti di SQL Server**. Connettersi a **(localdb) \MSSQLLocalDB**. Il database con un nome corrispondente **aspnet - <*nome del progetto*>-<*stringa data*>** viene visualizzato.

    ![Menu di scelta rapida nella tabella di database AspNetUsers](identity/_static/04-db.png)
    
    Espandere il database e il relativo **tabelle**, quindi tasto destro sulla tabella **dbo. AspNetUsers** e selezionare **Visualizza dati**.

8. Verificare il funzionamento dell'identità

    Il template di progetto predefinito per l'*applicazione Web di ASP.NET Core* consente agli utenti di accedere a qualsiasi azione nell'applicazione senza effettuare l'accesso. Per verificare il funzionamento di ASP.NET Identity, aggiungere un attributo `[Authorize]` all'azione `About` del controller `Home`.
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Eseguire il progetto utilizzando **Ctrl** + **F5** e passare il **su** pagina. Possono accedere solo gli utenti autenticati di **su** pagina ora, in modo ASP.NET si viene reindirizzati alla pagina di accesso per accedere o registrare.

    # <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

    Aprire una finestra di comando e passare alla radice del progetto directory contenente il `.csproj` file. Eseguire il [dotnet eseguire](/dotnet/core/tools/dotnet-run) comando per eseguire l'app:

    ```cs
    dotnet run 
    ```

    Individuare l'URL specificato nell'output del [dotnet eseguire](/dotnet/core/tools/dotnet-run) comando. L'URL deve puntare a `localhost` con un numero di porta generato. Passare il **su** pagina. Possono accedere solo gli utenti autenticati di **su** pagina ora, in modo ASP.NET si viene reindirizzati alla pagina di accesso per accedere o registrare.

    ---

## <a name="identity-components"></a>Componenti di identità

L'assembly di riferimento principale per il sistema di identità è `Microsoft.AspNetCore.Identity`. Questo pacchetto contiene il set di base di interfacce per ASP.NET Identity Core e viene incluso per `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Queste dipendenze sono necessarie per utilizzare il sistema di identità nelle applicazioni ASP.NET Core:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Contiene i tipi necessari per l'uso di identità con Entity Framework Core.

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core è una tecnologia di accesso ai dati consigliati di Microsoft per i database relazionali, ad esempio SQL Server. Per i test, è possibile utilizzare `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies` -Middleware che consente a un'applicazione utilizzare l'autenticazione basata su cookie.

## <a name="migrating-to-aspnet-core-identity"></a>La migrazione a identità ASP.NET Core

Per ulteriori informazioni e istruzioni sulla migrazione dell'identità esistenti dell'archivio vedere [la migrazione di autenticazione e identità](xref:migration/identity).

## <a name="setting-password-strength"></a>L'impostazione di complessità della password.

Vedere [configurazione](#pw) per un esempio che imposta i requisiti minimi.

## <a name="next-steps"></a>Passaggi successivi

* [Autenticazione della migrazione e identità](xref:migration/identity)
* [Conferma account e recupero password](xref:security/authentication/accconfirm)
* [Autenticazione a due fattori con SMS](xref:security/authentication/2fa)
* [Facebook, Google e autenticazione basata su provider esterni](xref:security/authentication/social/index)
