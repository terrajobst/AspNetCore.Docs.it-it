---
title: "Introduzione all'identità su ASP.NET Core"
author: rick-anderson
description: "Utilizzando l'identità con un'applicazione ASP.NET di base"
keywords: "Se l'autorizzazione ASP.NET Core, identità, sicurezza"
ms.author: riande
manager: wpickett
ms.date: 7/7/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 72802830660ddcf479e540de7cfc33a07c49dc23
ms.sourcegitcommit: b02db6da115e55140da91b67355aaf56aae1703f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="d68a9-104">Introduzione all'identità su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d68a9-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="d68a9-105">Da [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), e [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="d68a9-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="d68a9-106">Identità di ASP.NET Core è un sistema di appartenenze che consente di aggiungere funzionalità di accesso all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d68a9-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="d68a9-107">Gli utenti possono creare un account e l'account di accesso con un nome utente e password o che è possibile utilizzare un provider di accesso esterno, ad esempio Facebook, Google, Account Microsoft, Twitter o di altri utenti.</span><span class="sxs-lookup"><span data-stu-id="d68a9-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="d68a9-108">È possibile configurare ASP.NET Identity Core per l'utilizzo di un database di SQL Server per archiviare i nomi utente, password e i dati di profilo.</span><span class="sxs-lookup"><span data-stu-id="d68a9-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="d68a9-109">In alternativa, è possibile utilizzare un archivio permanente, ad esempio archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="d68a9-109">Alternatively, you can use your own persistent store, for example Azure Table Storage.</span></span> <span data-ttu-id="d68a9-110">Questo documento contiene istruzioni per Visual Studio e per usando l'interfaccia CLI.</span><span class="sxs-lookup"><span data-stu-id="d68a9-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="d68a9-111">Panoramica dell'identità</span><span class="sxs-lookup"><span data-stu-id="d68a9-111">Overview of Identity</span></span>

<span data-ttu-id="d68a9-112">In questo argomento, verranno imparare a usare ASP.NET Identity Core per aggiungere funzionalità a registrare, accedere e disconnettersi da un utente.</span><span class="sxs-lookup"><span data-stu-id="d68a9-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="d68a9-113">Per istruzioni più dettagliate sulla creazione di App scritte in ASP.NET Identity Core, vedere la sezione passaggi successivi alla fine di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="d68a9-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="d68a9-114">Creare un progetto di applicazione Web di ASP.NET Core con singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="d68a9-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d68a9-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d68a9-115">Visual Studio</span></span>](#tab/visual-studio)
    <span data-ttu-id="d68a9-116">In Visual Studio, selezionare **File** -> **New** -> **progetto**.</span><span class="sxs-lookup"><span data-stu-id="d68a9-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="d68a9-117">Selezionare il **applicazione Web ASP.NET** dal **nuovo progetto** la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="d68a9-117">Select the **ASP.NET Web Application** from the **New Project** dialog box.</span></span> <span data-ttu-id="d68a9-118">Selezionando un ASP.NET Core **applicazione Web** con **singoli account utente di** come metodo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d68a9-118">Selecting an ASP.NET Core **Web Application** with **Individual User Accounts** as the authentication method.</span></span>

    <span data-ttu-id="d68a9-119">Nota: È necessario selezionare **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="d68a9-119">Note: You must select **Individual User Accounts**.</span></span>
 
    ![Finestra di dialogo Nuovo progetto](identity/_static/01-mvc.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d68a9-121">.NET core CLI</span><span class="sxs-lookup"><span data-stu-id="d68a9-121">.NET Core CLI</span></span>](#tab/netcore-cli)
    <span data-ttu-id="d68a9-122">Se si utilizza l'interfaccia CLI Core .NET, creare il nuovo progetto utilizzando ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="d68a9-122">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="d68a9-123">Si creerà un nuovo progetto con lo stesso codice di modello di identità che Visual Studio crea.</span><span class="sxs-lookup"><span data-stu-id="d68a9-123">This will create a new project with the same Identity template code Visual Studio creates.</span></span>
 
    <span data-ttu-id="d68a9-124">Il progetto creato contiene il `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pacchetto, che verrà mantenuti i dati di identità e schema a SQL Server utilizzando [Entity Framework Core](https://docs.efproject.net).</span><span class="sxs-lookup"><span data-stu-id="d68a9-124">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which will persist the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.efproject.net).</span></span>
    
    ---
 
2.  <span data-ttu-id="d68a9-125">Configurare i servizi di identità e aggiungere middleware `Startup`.</span><span class="sxs-lookup"><span data-stu-id="d68a9-125">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="d68a9-126">I servizi di identità vengono aggiunti all'applicazione nel `ConfigureServices` metodo la `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="d68a9-126">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d68a9-127">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="d68a9-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="d68a9-128">[!code-csharp[Principale](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]</span><span class="sxs-lookup"><span data-stu-id="d68a9-128">[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]</span></span>
    
    <span data-ttu-id="d68a9-129">Questi servizi vengono resi disponibili per l'applicazione tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d68a9-129">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="d68a9-130">Identità è abilitata per l'applicazione chiamando `UseAuthentication` nel `Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="d68a9-130">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="d68a9-131">`UseAuthentication`Aggiunge l'autenticazione [middleware](xref:fundamentals/middleware) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="d68a9-131">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    <span data-ttu-id="d68a9-132">[!code-csharp[Principale](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]</span><span class="sxs-lookup"><span data-stu-id="d68a9-132">[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]</span></span>
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d68a9-133">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="d68a9-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="d68a9-134">[!code-csharp[Principale](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]</span><span class="sxs-lookup"><span data-stu-id="d68a9-134">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]</span></span>
    
    <span data-ttu-id="d68a9-135">Questi servizi vengono resi disponibili per l'applicazione tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d68a9-135">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="d68a9-136">Identità è abilitata per l'applicazione chiamando `UseIdentity` nel `Configure` metodo.</span><span class="sxs-lookup"><span data-stu-id="d68a9-136">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="d68a9-137">`UseIdentity`Aggiunge l'autenticazione basata su cookie [middleware](xref:fundamentals/middleware) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="d68a9-137">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    <span data-ttu-id="d68a9-138">[!code-csharp[Principale](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]</span><span class="sxs-lookup"><span data-stu-id="d68a9-138">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]</span></span>
    
    ---
     
    <span data-ttu-id="d68a9-139">Per ulteriori informazioni sull'avvio dell'applicazione, vedere [avvio dell'applicazione](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="d68a9-139">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="d68a9-140">Creare un utente.</span><span class="sxs-lookup"><span data-stu-id="d68a9-140">Create a user.</span></span>

    <span data-ttu-id="d68a9-141">Avviare l'applicazione e quindi fare clic su di **registrare** collegamento.</span><span class="sxs-lookup"><span data-stu-id="d68a9-141">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="d68a9-142">Se questa è la prima volta che si sta eseguendo questa azione, potrebbe essere necessario eseguire le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="d68a9-142">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="d68a9-143">L'applicazione viene richiesto di **applicare le migrazioni**:</span><span class="sxs-lookup"><span data-stu-id="d68a9-143">The application prompts you to **Apply Migrations**:</span></span>
    
    ![Applicare le migrazioni pagina](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="d68a9-145">In alternativa, è possibile verificare l'utilizzo di ASP.NET Identity Core con l'app senza un database persistente utilizzando un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="d68a9-145">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="d68a9-146">Per utilizzare un database in memoria, aggiungere il ``Microsoft.EntityFrameworkCore.InMemory`` pacchetto all'App e modificare la chiamata dell'applicazione a ``AddDbContext`` in ``ConfigureServices`` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="d68a9-146">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="d68a9-147">Quando l'utente sceglie il **registrare** collegamento, il ``Register`` azione viene richiamata sul ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="d68a9-147">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="d68a9-148">Il ``Register`` azione consente all'utente chiamando `CreateAsync` sul `_userManager` oggetto (fornito a ``AccountController`` dall'inserimento di dipendenze):</span><span class="sxs-lookup"><span data-stu-id="d68a9-148">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    <span data-ttu-id="d68a9-149">[!code-csharp[Principale](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]</span><span class="sxs-lookup"><span data-stu-id="d68a9-149">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]</span></span>

    <span data-ttu-id="d68a9-150">Se l'utente è stato creato correttamente, l'utente è connesso tramite la chiamata di ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="d68a9-150">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="d68a9-151">**Nota:** vedere [account conferma](xref:security/authentication/accconfirm#prevent-login-at-registration) per la procedura impedire l'accesso immediato al momento della registrazione.</span><span class="sxs-lookup"><span data-stu-id="d68a9-151">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="d68a9-152">Accedi.</span><span class="sxs-lookup"><span data-stu-id="d68a9-152">Log in.</span></span>
 
    <span data-ttu-id="d68a9-153">Gli utenti possono accedere facendo clic di **Accedi** collegamento nella parte superiore del sito, o può essere reindirizzati alla pagina di accesso se si tenta di accedere a una parte del sito che richiede l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="d68a9-153">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="d68a9-154">Quando l'utente invia il form nella pagina di accesso, il ``AccountController`` ``Login`` azione viene chiamata.</span><span class="sxs-lookup"><span data-stu-id="d68a9-154">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="d68a9-155">[!code-csharp[Principale](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="d68a9-155">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]</span></span>
 
    <span data-ttu-id="d68a9-156">Il ``Login`` azione chiama ``PasswordSignInAsync`` sul ``_signInManager`` oggetto (fornito a ``AccountController`` dall'inserimento di dipendenze).</span><span class="sxs-lookup"><span data-stu-id="d68a9-156">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>
 
    <span data-ttu-id="d68a9-157">La base ``Controller`` classe espone un ``User`` proprietà che è possibile accedere dai metodi di controller.</span><span class="sxs-lookup"><span data-stu-id="d68a9-157">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="d68a9-158">Ad esempio, è possibile enumerare `User.Claims` e prendere decisioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="d68a9-158">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="d68a9-159">Per ulteriori informazioni, vedere [autorizzazione](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="d68a9-159">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="d68a9-160">Effettuare la disconnessione.</span><span class="sxs-lookup"><span data-stu-id="d68a9-160">Log out.</span></span>
 
    <span data-ttu-id="d68a9-161">Fare clic su di **disconnettersi** collegamento chiamate il `LogOut` azione.</span><span class="sxs-lookup"><span data-stu-id="d68a9-161">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    <span data-ttu-id="d68a9-162">[!code-csharp[Principale](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="d68a9-162">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]</span></span>
 
    <span data-ttu-id="d68a9-163">Il codice precedente le chiamate di sopra di `_signInManager.SignOutAsync` metodo.</span><span class="sxs-lookup"><span data-stu-id="d68a9-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="d68a9-164">Il `SignOutAsync` metodo cancella attestazioni dell'utente archiviate in un cookie.</span><span class="sxs-lookup"><span data-stu-id="d68a9-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="d68a9-165">Configurazione.</span><span class="sxs-lookup"><span data-stu-id="d68a9-165">Configuration.</span></span>

    <span data-ttu-id="d68a9-166">Identità dispone di alcuni comportamenti predefiniti che è possibile eseguire l'override in una classe di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d68a9-166">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="d68a9-167">Non è necessario configurare ``IdentityOptions`` se si usano i comportamenti predefiniti.</span><span class="sxs-lookup"><span data-stu-id="d68a9-167">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d68a9-168">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="d68a9-168">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="d68a9-169">[!code-csharp[Principale](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]</span><span class="sxs-lookup"><span data-stu-id="d68a9-169">[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]</span></span>
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d68a9-170">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="d68a9-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="d68a9-171">[!code-csharp[Principale](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]</span><span class="sxs-lookup"><span data-stu-id="d68a9-171">[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]</span></span>

    ---
    
    <span data-ttu-id="d68a9-172">Per ulteriori informazioni su come configurare l'identità, vedere [configurare identità](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="d68a9-172">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="d68a9-173">È anche possibile configurare il tipo di dati della chiave primaria, vedere [tipo di dati di identità di configurare le chiavi primarie](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="d68a9-173">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="d68a9-174">Visualizzare il database.</span><span class="sxs-lookup"><span data-stu-id="d68a9-174">View the database.</span></span>

    <span data-ttu-id="d68a9-175">Se l'app Usa un database di SQL Server (impostazione predefinita in Windows e per gli utenti di Visual Studio), è possibile visualizzare il database dell'applicazione creata.</span><span class="sxs-lookup"><span data-stu-id="d68a9-175">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="d68a9-176">È possibile utilizzare **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="d68a9-176">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="d68a9-177">In alternativa, da Visual Studio, selezionare **vista** -> **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="d68a9-177">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="d68a9-178">Connettersi a **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="d68a9-178">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="d68a9-179">Il database con un nome corrispondente * *aspnet - <*nome del progetto*>-<*stringa data*> * * viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="d68a9-179">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Menu di scelta rapida nella tabella di database AspNetUsers](identity/_static/04-db.png)
    
    <span data-ttu-id="d68a9-181">Espandere il database e il relativo **tabelle**, quindi fare doppio clic su di **dbo. AspNetUsers** tabella e selezionare **Visualizza dati**.</span><span class="sxs-lookup"><span data-stu-id="d68a9-181">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

## <a name="identity-components"></a><span data-ttu-id="d68a9-182">Componenti di identità</span><span class="sxs-lookup"><span data-stu-id="d68a9-182">Identity Components</span></span>

<span data-ttu-id="d68a9-183">L'assembly di riferimento principale per il sistema di identità è `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="d68a9-183">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="d68a9-184">Questo pacchetto contiene il set di base di interfacce per ASP.NET Identity Core e viene incluso per `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="d68a9-184">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="d68a9-185">Queste dipendenze sono necessarie per utilizzare il sistema di identità nelle applicazioni ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="d68a9-185">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="d68a9-186">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Contiene i tipi necessari per l'uso di identità con Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d68a9-186">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="d68a9-187">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core è una tecnologia di accesso ai dati consigliati di Microsoft per i database relazionali, ad esempio SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d68a9-187">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="d68a9-188">Per i test, è possibile utilizzare `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="d68a9-188">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="d68a9-189">`Microsoft.AspNetCore.Authentication.Cookies`-Middleware che consente a un'applicazione utilizzare l'autenticazione basata su cookie.</span><span class="sxs-lookup"><span data-stu-id="d68a9-189">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="d68a9-190">La migrazione a identità ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d68a9-190">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="d68a9-191">Per ulteriori informazioni e istruzioni sulla migrazione dell'identità esistenti dell'archivio vedere [la migrazione di autenticazione e identità](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="d68a9-191">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d68a9-192">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="d68a9-192">Next Steps</span></span>

* [<span data-ttu-id="d68a9-193">La migrazione di autenticazione e identità</span><span class="sxs-lookup"><span data-stu-id="d68a9-193">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="d68a9-194">La conferma dell'account e il recupero della Password</span><span class="sxs-lookup"><span data-stu-id="d68a9-194">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="d68a9-195">Autenticazione a due fattori con SMS</span><span class="sxs-lookup"><span data-stu-id="d68a9-195">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="d68a9-196">Abilitazione dell'autenticazione con Facebook, Google e altri provider esterni</span><span class="sxs-lookup"><span data-stu-id="d68a9-196">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
