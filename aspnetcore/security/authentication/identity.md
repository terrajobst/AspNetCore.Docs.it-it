---
title: Introduzione all'identità in ASP.NET Core
author: rick-anderson
description: Usare l'identità con un'app ASP.NET Core. Informazioni su come impostare i requisiti di password (RequireDigit RequiredLength, RequiredUniqueChars e altro ancora).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: fd5fa2fd1e069bf10f3baea38b1fe9f951dc4a7d
ms.sourcegitcommit: fd461c60b5e36c7019f81da0138cc859d0fddaa2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/10/2018
ms.locfileid: "41826853"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="0d4e1-104">Introduzione all'identità in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d4e1-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="0d4e1-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0d4e1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0d4e1-106">ASP.NET Core Identity è un sistema di appartenenza che aggiunge funzionalità di accesso per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="0d4e1-107">Gli utenti possono creare un account con le informazioni di accesso archiviate nell'identità o è possibile usare un provider di accesso esterno.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="0d4e1-108">Provider di accesso esterni supportati includono [Facebook, Google, Account Microsoft e Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="0d4e1-109">Identità può essere configurato usando un database di SQL Server per archiviare nomi utente, password e i dati del profilo.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="0d4e1-110">In alternativa, un altro archivio permanente utilizzabile, ad esempio, archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

[<span data-ttu-id="0d4e1-111">Visualizzare o scaricare il codice di esempio.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="0d4e1-112">(Come scaricare)</span><span class="sxs-lookup"><span data-stu-id="0d4e1-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

<span data-ttu-id="0d4e1-113">In questo argomento, informazioni su come usare l'identità per registrare, accedere e disconnettersi da un utente.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-113">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="0d4e1-114">Per istruzioni più dettagliate sulla creazione di App che usano l'identità, vedere la sezione passaggi successivi alla fine di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-114">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="0d4e1-115">Creare un'app Web con l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="0d4e1-115">Create a Web app with authentication</span></span>

<span data-ttu-id="0d4e1-116">Creare un progetto di applicazione Web ASP.NET Core con account utente individuali.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d4e1-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d4e1-117">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0d4e1-118">Selezionare **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-118">Select **File** > **New** > **Project**.</span></span> 
* <span data-ttu-id="0d4e1-119">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-119">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="0d4e1-120">Denominare il progetto **App Web 1** avere lo stesso spazio dei nomi come il download del progetto.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-120">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="0d4e1-121">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-121">Click **OK**.</span></span>
* <span data-ttu-id="0d4e1-122">Selezionare un ASP.NET Core **applicazione Web** ASP.NET Core 2.1, quindi selezionare **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-122">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="0d4e1-123">Selezionare **account utente individuali** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-123">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0d4e1-124">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="0d4e1-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="0d4e1-125">Fornisce il progetto generato [ASP.NET Core Identity](xref:security/authentication/identity) come una [libreria di classi Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-125">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="0d4e1-126">Registrazione di test e account di accesso</span><span class="sxs-lookup"><span data-stu-id="0d4e1-126">Test Register and Login</span></span>

<span data-ttu-id="0d4e1-127">Eseguire l'app e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-127">Run the app and register a user.</span></span> <span data-ttu-id="0d4e1-128">A seconda delle dimensioni dello schermo, potrebbe essere necessario selezionare il pulsante Attiva/Disattiva navigazione per vedere le **registrare** e **Login** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-128">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![pulsante Mostra/Nascondi barra di spostamento](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="0d4e1-130">Configurare i servizi di identità</span><span class="sxs-lookup"><span data-stu-id="0d4e1-130">Configure Identity services</span></span>

<span data-ttu-id="0d4e1-131">Aggiunta di servizi `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-131">Services are added in `ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="0d4e1-132">Il codice precedente configura l'identità con valori predefiniti dell'opzione.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-132">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="0d4e1-133">Servizi sono resi disponibili per l'app tramite il [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-133">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="0d4e1-134">Vengono abilitate le identità chiamando [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-134">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="0d4e1-135">`UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-135">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="0d4e1-136">Servizi sono resi disponibili per l'applicazione attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-136">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="0d4e1-137">Identità è abilitata per l'applicazione chiamando `UseAuthentication` nel metodo`Configure`.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-137">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="0d4e1-138">`UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-138">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="0d4e1-139">Questi servizi sono resi disponibili per l'applicazione attraverso [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-139">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="0d4e1-140">Identità è abilitata per l'applicazione chiamando `UseIdentity` nel metodo`Configure`.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-140">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="0d4e1-141">`UseIdentity` Aggiunge l'autenticazione basata su cookie [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-141">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="0d4e1-142">Per altre informazioni, vedere la [classe IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) e [avvio dell'applicazione](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-142">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="0d4e1-143">Eseguire lo scaffolding di registrazione, l'account di accesso e disconnessione</span><span class="sxs-lookup"><span data-stu-id="0d4e1-143">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="0d4e1-144">Seguire le [lo scaffolding di identità in un progetto Razor con autorizzazione](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) istruzioni.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-144">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0d4e1-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d4e1-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0d4e1-146">Aggiungere i file di registrazione, l'account di accesso e disconnessione.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-146">Add the Register, Login, and LogOut files.</span></span>


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0d4e1-147">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="0d4e1-147">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="0d4e1-148">Se è stato creato il progetto con nome **App Web 1**, eseguire i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-148">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="0d4e1-149">In caso contrario, usare lo spazio dei nomi corretto per il `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="0d4e1-149">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>


```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"

```

<span data-ttu-id="0d4e1-150">PowerShell Usa un punto e virgola come separatore di comandi.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-150">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="0d4e1-151">Quando si usa PowerShell, eseguire l'escape di punti e virgola nell'elenco di file o inserire l'elenco di file tra virgolette doppie, come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-151">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="0d4e1-152">Esaminare Register</span><span class="sxs-lookup"><span data-stu-id="0d4e1-152">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="0d4e1-153">Quando un utente sceglie il **registrare** collegamento, il `RegisterModel.OnPostAsync` azione viene richiamata.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-153">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="0d4e1-154">L'utente viene creato mediante [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) nel `_userManager` oggetto.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-154">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="0d4e1-155">`_userManager` viene fornito dall'inserimento delle dipendenze):</span><span class="sxs-lookup"><span data-stu-id="0d4e1-155">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="0d4e1-156">Quando un utente sceglie il **registrare** collegamento, il `Register` azione viene richiamata in `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-156">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="0d4e1-157">L'azione`Register` crea l'utente chiamandoconsente all'utente chiamando  `CreateAsync` sull'oggetto  `_userManager`(fornito all `AccountController` tramite dependency injection):</span><span class="sxs-lookup"><span data-stu-id="0d4e1-157">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="0d4e1-158">Se l'utente è stato creato correttamente, l'utente è connesso tramite la chiamata al metodo `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-158">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="0d4e1-159">**Nota:** visualizzare [conferma dell'account](xref:security/authentication/accconfirm#prevent-login-at-registration) per i passaggi per impedire l'accesso immediato al momento della registrazione.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-159">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="0d4e1-160">Accedi</span><span class="sxs-lookup"><span data-stu-id="0d4e1-160">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0d4e1-161">Il modulo di accesso viene visualizzato quando:</span><span class="sxs-lookup"><span data-stu-id="0d4e1-161">The Login form is displayed when:</span></span>

* <span data-ttu-id="0d4e1-162">Il **Accedi** Seleziona collegamento.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-162">The **Log in** link  is selected.</span></span>
* <span data-ttu-id="0d4e1-163">Quando un utente accede a una pagina in cui non sono autenticati **o** autorizzato, viene reindirizzato alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-163">When a user accesses a page where they are not authenticated **or** authorized, they are redirected to the Login page.</span></span> 

<span data-ttu-id="0d4e1-164">Quando viene inviato il modulo nella pagina di accesso, il `OnPostAsync` viene chiamata azione.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-164">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="0d4e1-165">`PasswordSignInAsync` viene chiamato sul `_signInManager` oggetto (fornito dall'inserimento di dipendenze).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-165">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="0d4e1-166">La classe base `Controller` espone una proprietà  `User` a cui è possibile accedere dai metodi del controller.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-166">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="0d4e1-167">Ad esempio, è possibile enumerare `User.Claims` e prendere decisioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-167">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="0d4e1-168">Per ulteriori informazioni, vedere [autorizzazione](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-168">For more information, see [Authorization](xref:security/authorization/index).</span></span>

::: moniker-end
::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="0d4e1-169">Il modulo di accesso viene visualizzato quando gli utenti selezionano il **Accedi** collegare o vengono reindirizzati quando si accede a una pagina che richiede l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-169">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="0d4e1-170">Quando l'utente invia il form nella pagina di accesso, il metodo di azione`AccountController`nell `Login`viene invocato.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-170">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="0d4e1-171">L'azione `Login` chiama `PasswordSignInAsync` sul `_signInManager` oggetto (fornito all `AccountController` tramite dependency injection).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-171">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="0d4e1-172">La base (`Controller` oppure `PageModel`) classe espone un `User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-172">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="0d4e1-173">Ad esempio, `User.Claims` può essere enumerata per prendere decisioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-173">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="0d4e1-174">Effettuare la disconnessione</span><span class="sxs-lookup"><span data-stu-id="0d4e1-174">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0d4e1-175">Il **disconnettersi** collegamento richiama il `LogoutModel.OnPost` azione.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-175">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="0d4e1-176">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) Cancella le attestazioni dell'utente archiviate in un cookie.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-176">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="0d4e1-177">Dopo la chiamata non di reindirizzamento `SignOutAsync` o l'utente verrà **non** disconnessione.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-177">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="0d4e1-178">Post viene specificato nella *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0d4e1-178">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end
::: moniker range="= aspnetcore-2.0"
   <span data-ttu-id="0d4e1-179">Fare clic sul collegamento **disconnettersi** per chiamare l'azione `LogOut`.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-179">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="0d4e1-180">Il codice precedente chiama il `_signInManager.SignOutAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-180">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="0d4e1-181">Il metodo `SignOutAsync` cancella attestazioni dell'utente archiviate in un cookie.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-181">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="0d4e1-182">Verificare l'identità</span><span class="sxs-lookup"><span data-stu-id="0d4e1-182">Test Identity</span></span>

<span data-ttu-id="0d4e1-183">I modelli di progetto web predefinita è consentire l'accesso anonimo alle pagine home.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-183">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="0d4e1-184">Per verificare l'identità, aggiungere [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) alla pagina About.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-184">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="0d4e1-185">Se si è connessi, disconnettersi. Eseguire l'app e selezionare il **sulle** collegamento.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-185">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="0d4e1-186">Si verrà reindirizzati alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-186">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="0d4e1-187">Esplorare identità</span><span class="sxs-lookup"><span data-stu-id="0d4e1-187">Explore Identity</span></span>

<span data-ttu-id="0d4e1-188">Per ulteriori dettagli sulle identità:</span><span class="sxs-lookup"><span data-stu-id="0d4e1-188">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="0d4e1-189">Creare identità completa dell'interfaccia utente origine</span><span class="sxs-lookup"><span data-stu-id="0d4e1-189">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="0d4e1-190">Esaminare l'origine di ogni pagina e procedere con il debugger.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-190">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="0d4e1-191">Componenti delle identità</span><span class="sxs-lookup"><span data-stu-id="0d4e1-191">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="0d4e1-192">Tutte le identità NuGet pacchetti dipendenti sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-192">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
::: moniker-end

<span data-ttu-id="0d4e1-193">Il pacchetto per l'identità primario viene [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-193">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="0d4e1-194">Questo pacchetto contiene il set principale di interfacce per ASP.NET Core Identity e viene incluso per `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-194">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="0d4e1-195">Migrazione ad ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="0d4e1-195">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="0d4e1-196">Per altre informazioni e istruzioni sulla migrazione archivio di identità esistente, vedere [eseguire la migrazione di autenticazione e identità](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-196">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="0d4e1-197">Impostare la complessità della password</span><span class="sxs-lookup"><span data-stu-id="0d4e1-197">Setting password strength</span></span>

<span data-ttu-id="0d4e1-198">Visualizzare [configurazione](#pw) per un esempio che imposta i requisiti di lunghezza minima della password.</span><span class="sxs-lookup"><span data-stu-id="0d4e1-198">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d4e1-199">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="0d4e1-199">Next Steps</span></span>

* [<span data-ttu-id="0d4e1-200">Configurare Identity</span><span class="sxs-lookup"><span data-stu-id="0d4e1-200">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <span data-ttu-id="0d4e1-201">[Configurare il tipo di dati di identità chiavi primarie](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="0d4e1-201">[Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
