---
title: Introduzione all'identità in ASP.NET Core
author: rick-anderson
description: Usare l'identità con un'app ASP.NET Core. Include, i requisiti di password di impostazione (RequireDigit, RequiredLength, RequiredUniqueChars e altro ancora).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: 50ddb96000e6a3f9e1762e9bb3e1f215f20d4356
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095639"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="3a2aa-104">Introduzione all'identità in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3a2aa-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="3a2aa-105">Dal [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="3a2aa-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3a2aa-106">ASP.NET Core Identity è un sistema di membership che ti consente di aggiungere funzionalità di accesso all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="3a2aa-107">li utenti possono creare un account ed effettuare il login con username e password o possono utilizzare un provider di accesso esterno come ad esempio Facebook, Google, Account Microsoft, Twitter o altri.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="3a2aa-108">Puoi configurare ASP.NET Identity Core per l'utilizzo di un database SQL Server per archiviare i nomi utente, password e i dati di profilo.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="3a2aa-109">In alternativa, è possibile usare il proprio archivio permanente, ad esempio, un'archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="3a2aa-110">Questo documento contiene istruzioni per Visual Studio e per l'utilizzo dell'interfaccia a riga di comando CLI.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="3a2aa-111">Visualizzare o scaricare il codice di esempio.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="3a2aa-112">(Come scaricare)</span><span class="sxs-lookup"><span data-stu-id="3a2aa-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="3a2aa-113">Panoramica di Identity</span><span class="sxs-lookup"><span data-stu-id="3a2aa-113">Overview of Identity</span></span>

<span data-ttu-id="3a2aa-114">In questo argomento, imparerai ad usare ASP.NET Identity Core per aggiungere le funzionalità di registrazione, accesso e disconnessione di un utente.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="3a2aa-115">Per istruzioni più dettagliate sulla creazione di App tramite ASP.NET Core Identity, vedere la sezione passaggi successivi alla fine di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1. <span data-ttu-id="3a2aa-116">Creare un progetto di applicazione Web ASP.NET Core con account utente individuali.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3a2aa-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3a2aa-117">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="3a2aa-118">In Visual Studio, selezionare **File** > **New** > **progetto**.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="3a2aa-119">Selezionare **applicazione Web ASP.NET Core** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

   ![Finestra di dialogo Nuovo progetto](identity/_static/01-new-project.png)

   <span data-ttu-id="3a2aa-121">Selezionare un ASP.NET Core **applicazione Web (Model-View-Controller)** per ASP.NET Core 2.x, quindi selezionare **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

   ![Finestra di dialogo Nuovo progetto](identity/_static/02-new-project.png)

   <span data-ttu-id="3a2aa-123">Appare una finestra di dialogo con le opzioni di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="3a2aa-124">Selezionare **account utente individuali** e fare clic su **OK** per tornare alla finestra di dialogo precedente.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

   ![Finestra di dialogo Nuovo progetto](identity/_static/03-new-project-auth.png)

   <span data-ttu-id="3a2aa-126">Selezionando **singoli account utente di** indica a Visual Studio per creare modelli, ViewModel, visualizzazioni, controller e gli altri asset necessari per l'autenticazione come parte del modello di progetto.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3a2aa-127">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="3a2aa-127">.NET Core CLI</span></span>](#tab/netcore-cli)

   <span data-ttu-id="3a2aa-128">Se si usa .NET Core CLI, creare il nuovo progetto usando `dotnet new mvc --auth Individual`.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-128">If using the .NET Core CLI, create the new project using `dotnet new mvc --auth Individual`.</span></span> <span data-ttu-id="3a2aa-129">Questo comando crea un nuovo progetto con lo stesso codice di modello di identità in Visual Studio creato.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

   <span data-ttu-id="3a2aa-130">Il progetto creato contiene il `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pacchetto, che rende persistenti i dati di identità e dello schema a SQL Server con [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="3a2aa-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

   ---

2. <span data-ttu-id="3a2aa-131">Configurare i servizi di identità e aggiungere middleware in `Startup`.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-131">Configure Identity services and add middleware in `Startup`.</span></span>

   <span data-ttu-id="3a2aa-132">I servizi di identità vengono aggiunti all'applicazione nel metodo `ConfigureServices` della classe `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="3a2aa-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3a2aa-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3a2aa-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="3a2aa-134">Questi servizi sono resi disponibili per l'applicazione attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3a2aa-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="3a2aa-135">Identità è abilitata per l'applicazione chiamando `UseAuthentication` nel metodo`Configure`.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="3a2aa-136">`UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3a2aa-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3a2aa-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="3a2aa-138">Questi servizi sono resi disponibili per l'applicazione attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3a2aa-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="3a2aa-139">Identità è abilitata per l'applicazione chiamando `UseIdentity` nel metodo`Configure`.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="3a2aa-140">`UseIdentity` Aggiunge l'autenticazione basata su cookie [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   <span data-ttu-id="3a2aa-141">Per altre informazioni sul processo di avvio applicazione, vedere [avvio dell'applicazione](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="3a2aa-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3. <span data-ttu-id="3a2aa-142">Creare un utente.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-142">Create a user.</span></span>

   <span data-ttu-id="3a2aa-143">Avviare l'applicazione e quindi fare clic sui **registrare** collegamento.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-143">Launch the application and then click on the **Register** link.</span></span>

   <span data-ttu-id="3a2aa-144">Se questa è la prima volta che si sta eseguendo questa azione, sarà necessario per eseguire le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="3a2aa-145">L'applicazione viene richiesto di **applicare le migrazioni**.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="3a2aa-146">Se necessario, aggiornare la pagina.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-146">Refresh the page if needed.</span></span>

   ![Applicare le migrazioni sender](identity/_static/apply-migrations.png)

   <span data-ttu-id="3a2aa-148">In alternativa, è possibile verificare l'utilizzo di ASP.NET Identity Core con l'app senza un database persistente utilizzando un database in memoria. </span><span class="sxs-lookup"><span data-stu-id="3a2aa-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="3a2aa-149">Per utilizzare un database in memoria, aggiungere il pacchetto `Microsoft.EntityFrameworkCore.InMemory` pall'App e modificare la chiamata dell'applicazione a `AddDbContext` in `ConfigureServices` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="3a2aa-149">To use an in-memory database, add the `Microsoft.EntityFrameworkCore.InMemory` package to your app and modify your app's call to `AddDbContext` in `ConfigureServices` as follows:</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   <span data-ttu-id="3a2aa-150">Quando l'utente sceglie il collegamento**registrare** il metodo azione`Register` viene richiamato nell `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-150">When the user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="3a2aa-151">L'azione`Register` crea l'utente chiamandoconsente all'utente chiamando  `CreateAsync` sull'oggetto  `_userManager`(fornito all `AccountController` tramite dependency injection):</span><span class="sxs-lookup"><span data-stu-id="3a2aa-151">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   <span data-ttu-id="3a2aa-152">Se l'utente è stato creato correttamente, l'utente è connesso tramite la chiamata al metodo `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-152">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="3a2aa-153">**Nota:** visualizzare [conferma dell'account](xref:security/authentication/accconfirm#prevent-login-at-registration) per i passaggi per impedire l'accesso immediato al momento della registrazione.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

4. <span data-ttu-id="3a2aa-154">Accedi.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-154">Log in.</span></span>

   <span data-ttu-id="3a2aa-155">Gli utenti possono accedere facendo clic sul collegamento**Accedi** nella parte superiore del sito, o possono essere reindirizzati alla pagina di accesso se si tenta di accedere ad una parte del sito che richiede l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="3a2aa-156">Quando l'utente invia il form nella pagina di accesso, il metodo di azione`AccountController`nell `Login`viene invocato.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-156">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

   <span data-ttu-id="3a2aa-157">L'azione `Login` chiama `PasswordSignInAsync` sul `_signInManager` oggetto (fornito all `AccountController` tramite dependency injection).</span><span class="sxs-lookup"><span data-stu-id="3a2aa-157">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   <span data-ttu-id="3a2aa-158">La classe base `Controller` espone una proprietà  `User` a cui è possibile accedere dai metodi del controller.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-158">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="3a2aa-159">Ad esempio, è possibile enumerare `User.Claims` e prendere decisioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="3a2aa-160">Per ulteriori informazioni, vedere [autorizzazione](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="3a2aa-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>

5. <span data-ttu-id="3a2aa-161">Effettuare la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-161">Log out.</span></span>

   <span data-ttu-id="3a2aa-162">Fare clic sul collegamento **disconnettersi** per chiamare l'azione `LogOut`.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="3a2aa-163">Il codice precedente chiama il metodo `_signInManager.SignOutAsync`.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="3a2aa-164">Il metodo `SignOutAsync` cancella attestazioni dell'utente archiviate in un cookie.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

<a name="pw"></a>
6. <span data-ttu-id="3a2aa-165">Configurazione.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-165">Configuration.</span></span>

   <span data-ttu-id="3a2aa-166">Identità dispone di alcuni comportamenti predefiniti che possono essere sostituiti nella classe di avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-166">Identity has some default behaviors that can be overridden in the app's startup class.</span></span> <span data-ttu-id="3a2aa-167">Le `IdentityOptions` non devono essere configurate quando si utilizzano i comportamenti predefiniti.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-167">`IdentityOptions` don't need to be configured when using the default behaviors.</span></span> <span data-ttu-id="3a2aa-168">Il codice seguente imposta diverse opzioni sul livello di sicurezza della password:</span><span class="sxs-lookup"><span data-stu-id="3a2aa-168">The following code sets several password strength options:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3a2aa-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="3a2aa-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3a2aa-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3a2aa-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   <span data-ttu-id="3a2aa-171">Per altre informazioni su come configurare l'identità, vedere [configurare identità](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="3a2aa-171">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>

   <span data-ttu-id="3a2aa-172">È anche possibile configurare il tipo di dati della chiave primaria, vedere [tipo di dati di identità di configurare le chiavi primarie](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="3a2aa-172">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>

7. <span data-ttu-id="3a2aa-173">Visualizzare il database.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-173">View the database.</span></span>

   <span data-ttu-id="3a2aa-174">Se l'app usa un database di SQL Server (impostazione predefinita in Windows e per gli utenti di Visual Studio), è possibile visualizzare il database dell'applicazione creata.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-174">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="3a2aa-175">È possibile utilizzare **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-175">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="3a2aa-176">In alternativa, da Visual Studio, selezionare **vista** > **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-176">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="3a2aa-177">Connettersi a **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-177">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="3a2aa-178">Il database con un nome corrispondente `aspnet-<name of your project>-<guid>` viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-178">The database with a name matching `aspnet-<name of your project>-<guid>` is displayed.</span></span>

   ![Menu di scelta rapida nella tabella AspNetUsers database](identity/_static/04-db.png)

   <span data-ttu-id="3a2aa-180">Espandere il database e le relative **tabelle**, quindi fare clic con il pulsante destro del mouse sulla tabella **dbo. AspNetUsers** e selezionare **Visualizza dati**.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-180">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="3a2aa-181">Verificare il funzionamento dell'identità</span><span class="sxs-lookup"><span data-stu-id="3a2aa-181">Verify Identity works</span></span>

    <span data-ttu-id="3a2aa-182">l template di progetto predefinito per l'*applicazione Web di ASP.NET Core* consente agli utenti di accedere a qualsiasi azione nell'applicazione senza effettuare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-182">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="3a2aa-183">Per verificare il funzionamento di ASP.NET Identity, aggiungere un attributo `[Authorize]` all'azione `About` del controller `Home`.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-183">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3a2aa-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3a2aa-184">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="3a2aa-185">Eseguire il progetto utilizzando **Ctrl** + **F5** e passare alla **sulle** pagina.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-185">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="3a2aa-186">Solo gli utenti autenticati possono accedere le **sulle** pagina a questo punto, in modo che ASP.NET si viene reindirizzati alla pagina di accesso per accedere o registrare.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-186">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="3a2aa-187">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="3a2aa-187">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="3a2aa-188">Aprire una finestra di comando e passare alla radice del progetto directory contenente il `.csproj` file.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-188">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="3a2aa-189">Eseguire la [dotnet eseguire](/dotnet/core/tools/dotnet-run) comando per eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="3a2aa-189">Run the [dotnet run](/dotnet/core/tools/dotnet-run) command to run the app:</span></span>

    ```csharp
    dotnet run 
    ```

    <span data-ttu-id="3a2aa-190">Passare l'URL specificato nell'output il [dotnet eseguire](/dotnet/core/tools/dotnet-run) comando.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-190">Browse the URL specified in the output from the [dotnet run](/dotnet/core/tools/dotnet-run) command.</span></span> <span data-ttu-id="3a2aa-191">L'URL deve puntare a `localhost` con un numero di porta generato.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-191">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="3a2aa-192">Passare il **sulle** pagina.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-192">Navigate to the **About** page.</span></span> <span data-ttu-id="3a2aa-193">Solo gli utenti autenticati possono accedere le **sulle** pagina a questo punto, in modo che ASP.NET si viene reindirizzati alla pagina di accesso per accedere o registrare.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-193">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="3a2aa-194">Componenti delle identità</span><span class="sxs-lookup"><span data-stu-id="3a2aa-194">Identity Components</span></span>

<span data-ttu-id="3a2aa-195">L'assembly di riferimento principale per il sistema di identità è `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-195">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="3a2aa-196">Questo pacchetto contiene il set principale di interfacce per ASP.NET Core Identity e viene incluso per `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="3a2aa-197">Queste dipendenze sono necessarie per usare il sistema di identità nelle applicazioni ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3a2aa-197">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="3a2aa-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Contiene i tipi necessari per l'uso di identità con Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="3a2aa-199">`Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core è una tecnologia di accesso ai dati consigliati di Microsoft per i database relazionali come SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-199">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="3a2aa-200">Per i test, è possibile usare `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-200">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="3a2aa-201">`Microsoft.AspNetCore.Authentication.Cookies` -Middleware che consente a un'app usare l'autenticazione basata su cookie.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-201">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="3a2aa-202">Migrazione ad ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="3a2aa-202">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="3a2aa-203">Per altre informazioni e indicazioni sulla migrazione di identità esistente vedere dei negozi [eseguire la migrazione di autenticazione e identità](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="3a2aa-203">For additional information and guidance on migrating your existing Identity store see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="3a2aa-204">Impostare la complessità della password</span><span class="sxs-lookup"><span data-stu-id="3a2aa-204">Setting password strength</span></span>

<span data-ttu-id="3a2aa-205">Visualizzare [configurazione](#pw) per un esempio che imposta i requisiti di lunghezza minima della password.</span><span class="sxs-lookup"><span data-stu-id="3a2aa-205">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a2aa-206">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="3a2aa-206">Next Steps</span></span>

* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:security/authentication/social/index>
* <xref:host-and-deploy/web-farm>
