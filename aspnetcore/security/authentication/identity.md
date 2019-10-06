---
title: Introduzione all'identità in ASP.NET Core
author: rick-anderson
description: Usare l'identità con un'app ASP.NET Core. Informazioni su come impostare i requisiti per le password (RequireDigit, RequiredLength, RequiredUniqueChars e altro).
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 6701eb0ac1b1abb8699a5a529bcc29f295e5c7c9
ms.sourcegitcommit: 4115bf0e850c13d4e655beb5ab5e8ff431173cb6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/06/2019
ms.locfileid: "71981919"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="c1fab-104">Introduzione all'identità in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1fab-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="c1fab-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c1fab-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c1fab-106">ASP.NET Core identità è un sistema di appartenenza che aggiunge la funzionalità di accesso alle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1fab-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="c1fab-107">Gli utenti possono creare un account con le informazioni di accesso archiviate in Identity oppure possono usare un provider di accesso esterno.</span><span class="sxs-lookup"><span data-stu-id="c1fab-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="c1fab-108">I provider di accesso esterni supportati includono [Facebook, Google, account Microsoft e Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="c1fab-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="c1fab-109">L'identità può essere configurata utilizzando un database SQL Server per archiviare i nomi utente, le password e i dati di profilo.</span><span class="sxs-lookup"><span data-stu-id="c1fab-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="c1fab-110">In alternativa, è possibile usare un altro archivio permanente, ad esempio archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="c1fab-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="c1fab-111">Consente di [visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([come scaricare)](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="c1fab-111">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="c1fab-112">In questo argomento si apprenderà come usare l'identità per la registrazione, l'accesso e la disconnessione di un utente.</span><span class="sxs-lookup"><span data-stu-id="c1fab-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="c1fab-113">Per istruzioni più dettagliate sulla creazione di app che usano l'identità, vedere la sezione passaggi successivi alla fine di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="c1fab-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="c1fab-114">AddDefaultIdentity e AddIdentity</span><span class="sxs-lookup"><span data-stu-id="c1fab-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="c1fab-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) è stato introdotto in ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="c1fab-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="c1fab-116">La chiamata a `AddDefaultIdentity` è simile alla chiamata a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c1fab-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="c1fab-117">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="c1fab-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="c1fab-118">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="c1fab-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="c1fab-119">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="c1fab-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="c1fab-120">Per altre informazioni, vedere [origine AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="c1fab-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="c1fab-121">Creare un'app Web con l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="c1fab-121">Create a Web app with authentication</span></span>

<span data-ttu-id="c1fab-122">Creare un progetto di applicazione Web di ASP.NET Core con singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="c1fab-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c1fab-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c1fab-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c1fab-124">Selezionare **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="c1fab-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="c1fab-125">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c1fab-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="c1fab-126">Denominare il progetto **app Web 1** in modo che abbia lo stesso spazio dei nomi del download del progetto.</span><span class="sxs-lookup"><span data-stu-id="c1fab-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="c1fab-127">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1fab-127">Click **OK**.</span></span>
* <span data-ttu-id="c1fab-128">Selezionare un' **applicazione Web**ASP.NET Core, quindi selezionare **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="c1fab-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="c1fab-129">Selezionare **singoli account utente** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1fab-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c1fab-130">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c1fab-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="c1fab-131">Il progetto generato fornisce [ASP.NET Core identità](xref:security/authentication/identity) come [libreria di classi Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="c1fab-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="c1fab-132">La libreria della classe Razor Identity espone gli endpoint con l'area `Identity`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-132">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="c1fab-133">Esempio:</span><span class="sxs-lookup"><span data-stu-id="c1fab-133">For example:</span></span>

* <span data-ttu-id="c1fab-134">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="c1fab-134">/Identity/Account/Login</span></span>
* <span data-ttu-id="c1fab-135">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="c1fab-135">/Identity/Account/Logout</span></span>
* <span data-ttu-id="c1fab-136">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="c1fab-136">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="c1fab-137">Applicare le migrazioni</span><span class="sxs-lookup"><span data-stu-id="c1fab-137">Apply migrations</span></span>

<span data-ttu-id="c1fab-138">Applicare le migrazioni per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="c1fab-138">Apply the migrations to initialise the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c1fab-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c1fab-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c1fab-140">Eseguire il comando seguente nella console di gestione pacchetti (PMC):</span><span class="sxs-lookup"><span data-stu-id="c1fab-140">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c1fab-141">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c1fab-141">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="c1fab-142">Registro di test e account di accesso</span><span class="sxs-lookup"><span data-stu-id="c1fab-142">Test Register and Login</span></span>

<span data-ttu-id="c1fab-143">Eseguire l'app e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="c1fab-143">Run the app and register a user.</span></span> <span data-ttu-id="c1fab-144">A seconda delle dimensioni dello schermo, potrebbe essere necessario selezionare l'interruttore di spostamento per visualizzare i collegamenti di **Registro** e di **accesso** .</span><span class="sxs-lookup"><span data-stu-id="c1fab-144">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="c1fab-145">Configurare i servizi di identità</span><span class="sxs-lookup"><span data-stu-id="c1fab-145">Configure Identity services</span></span>

<span data-ttu-id="c1fab-146">I servizi vengono aggiunti in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-146">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="c1fab-147">Il modello tipico consiste nel chiamare tutti i metodi `Add{Service}` e quindi chiamare tutti i metodi `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-147">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="c1fab-148">Il codice precedente configura l'identità con i valori delle opzioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="c1fab-148">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="c1fab-149">I servizi vengono resi disponibili per l'applicazione tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c1fab-149">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="c1fab-150">L'identità viene abilitata chiamando [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="c1fab-150">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="c1fab-151">`UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="c1fab-151">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="c1fab-152">I servizi vengono resi disponibili per l'applicazione tramite l' [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c1fab-152">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="c1fab-153">Identità è abilitata per l'applicazione chiamando `UseAuthentication` nel metodo`Configure`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-153">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="c1fab-154">`UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="c1fab-154">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="c1fab-155">Questi servizi vengono resi disponibili per l'applicazione tramite l' [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c1fab-155">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="c1fab-156">Identità è abilitata per l'applicazione chiamando `UseIdentity` nel metodo`Configure`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-156">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="c1fab-157">`UseIdentity` aggiunge il [middleware](xref:fundamentals/middleware/index) di autenticazione basato su cookie alla pipeline di richieste.</span><span class="sxs-lookup"><span data-stu-id="c1fab-157">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="c1fab-158">Per ulteriori informazioni, vedere la [Classe IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) e l' [avvio dell'applicazione](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="c1fab-158">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="c1fab-159">Registrazione, accesso e disconnessione del patibolo</span><span class="sxs-lookup"><span data-stu-id="c1fab-159">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="c1fab-160">Seguire l' [identità del patibolo in un progetto Razor con](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) le istruzioni di autorizzazione per generare il codice illustrato in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="c1fab-160">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c1fab-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c1fab-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c1fab-162">Aggiungere i file di registro, di accesso e di disconnessione.</span><span class="sxs-lookup"><span data-stu-id="c1fab-162">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c1fab-163">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c1fab-163">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c1fab-164">Se il progetto è stato creato con il nome **app Web 1**, eseguire i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="c1fab-164">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="c1fab-165">In caso contrario, utilizzare lo spazio dei nomi corretto per il `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="c1fab-165">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="c1fab-166">PowerShell usa il punto e virgola come separatore di comandi.</span><span class="sxs-lookup"><span data-stu-id="c1fab-166">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="c1fab-167">Quando si usa PowerShell, usare il carattere di escape per il punto e virgola nell'elenco dei file o inserire l'elenco di file tra virgolette doppie, come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="c1fab-167">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="c1fab-168">Esaminare il registro</span><span class="sxs-lookup"><span data-stu-id="c1fab-168">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="c1fab-169">Quando un utente fa clic sul collegamento **Register** , viene richiamata l'azione `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-169">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="c1fab-170">L'utente viene creato da [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) nell'oggetto `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-170">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="c1fab-171">`_userManager` è fornito dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="c1fab-171">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="c1fab-172">Quando un utente fa clic sul collegamento **Register** , viene richiamata l'azione `Register` in `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-172">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="c1fab-173">L'azione`Register` crea l'utente chiamandoconsente all'utente chiamando  `CreateAsync` sull'oggetto  `_userManager`(fornito all `AccountController` tramite dependency injection):</span><span class="sxs-lookup"><span data-stu-id="c1fab-173">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="c1fab-174">Se l'utente è stato creato correttamente, l'utente è connesso tramite la chiamata al metodo `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="c1fab-175">**Nota:** Vedere la [conferma dell'account](xref:security/authentication/accconfirm#prevent-login-at-registration) per i passaggi per impedire l'accesso immediato alla registrazione.</span><span class="sxs-lookup"><span data-stu-id="c1fab-175">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="c1fab-176">Accedi</span><span class="sxs-lookup"><span data-stu-id="c1fab-176">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c1fab-177">Il modulo di accesso viene visualizzato quando:</span><span class="sxs-lookup"><span data-stu-id="c1fab-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="c1fab-178">Il collegamento **Accedi** è selezionato.</span><span class="sxs-lookup"><span data-stu-id="c1fab-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="c1fab-179">Un utente tenta di accedere a una pagina con restrizioni che non è autorizzata ad accedere **o** quando non è stata autenticata dal sistema.</span><span class="sxs-lookup"><span data-stu-id="c1fab-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="c1fab-180">Quando viene inviato il modulo nella pagina di accesso, viene chiamata l'azione `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="c1fab-181">`PasswordSignInAsync` viene chiamato sull'oggetto `_signInManager` (fornito dall'inserimento delle dipendenze).</span><span class="sxs-lookup"><span data-stu-id="c1fab-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="c1fab-182">La classe base `Controller` espone una proprietà  `User` a cui è possibile accedere dai metodi del controller.</span><span class="sxs-lookup"><span data-stu-id="c1fab-182">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="c1fab-183">Ad esempio, è possibile enumerare `User.Claims` e prendere decisioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="c1fab-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="c1fab-184">Per altre informazioni, vedere <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="c1fab-184">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="c1fab-185">Il modulo di accesso viene visualizzato quando gli utenti selezionano il collegamento **Accedi** o vengono reindirizzati quando si accede a una pagina che richiede l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="c1fab-185">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="c1fab-186">Quando l'utente invia il form nella pagina di accesso, il metodo di azione`AccountController`nell `Login`viene invocato.</span><span class="sxs-lookup"><span data-stu-id="c1fab-186">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="c1fab-187">L'azione `Login` chiama `PasswordSignInAsync` sul `_signInManager` oggetto (fornito all `AccountController` tramite dependency injection).</span><span class="sxs-lookup"><span data-stu-id="c1fab-187">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="c1fab-188">La classe base (`Controller` o `PageModel`) espone una proprietà `User`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-188">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="c1fab-189">È ad esempio possibile enumerare `User.Claims` per prendere decisioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="c1fab-189">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="c1fab-190">Disconnetti</span><span class="sxs-lookup"><span data-stu-id="c1fab-190">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c1fab-191">Il collegamento **Disconnetti** richiama l'azione `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-191">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="c1fab-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) Cancella le attestazioni dell'utente archiviate in un cookie.</span><span class="sxs-lookup"><span data-stu-id="c1fab-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="c1fab-193">Post è specificato in *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c1fab-193">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="c1fab-194">Fare clic sul collegamento **disconnettersi** per chiamare l'azione `LogOut`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-194">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="c1fab-195">Il codice precedente chiama il metodo `_signInManager.SignOutAsync`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-195">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="c1fab-196">Il metodo `SignOutAsync` cancella attestazioni dell'utente archiviate in un cookie.</span><span class="sxs-lookup"><span data-stu-id="c1fab-196">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="c1fab-197">Identità del test</span><span class="sxs-lookup"><span data-stu-id="c1fab-197">Test Identity</span></span>

<span data-ttu-id="c1fab-198">I modelli di progetto Web predefiniti consentono l'accesso anonimo alle Home page.</span><span class="sxs-lookup"><span data-stu-id="c1fab-198">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="c1fab-199">Per verificare l'identità, aggiungere [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) alla pagina privacy.</span><span class="sxs-lookup"><span data-stu-id="c1fab-199">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

<span data-ttu-id="c1fab-200">Se è stato eseguito l'accesso, disconnettersi. Eseguire l'app e selezionare il collegamento per la **privacy** .</span><span class="sxs-lookup"><span data-stu-id="c1fab-200">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="c1fab-201">Si verrà reindirizzati alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="c1fab-201">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="c1fab-202">Esplora identità</span><span class="sxs-lookup"><span data-stu-id="c1fab-202">Explore Identity</span></span>

<span data-ttu-id="c1fab-203">Per esplorare l'identità in modo più dettagliato:</span><span class="sxs-lookup"><span data-stu-id="c1fab-203">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="c1fab-204">Crea origine interfaccia utente con identità completa</span><span class="sxs-lookup"><span data-stu-id="c1fab-204">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="c1fab-205">Esaminare l'origine di ogni pagina ed eseguire un'istruzione alla volta nel debugger.</span><span class="sxs-lookup"><span data-stu-id="c1fab-205">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="c1fab-206">Componenti Identity</span><span class="sxs-lookup"><span data-stu-id="c1fab-206">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="c1fab-207">Tutti i pacchetti NuGet dipendenti dall'identità sono inclusi nel [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c1fab-207">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="c1fab-208">Il pacchetto primario per Identity è [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="c1fab-208">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="c1fab-209">Questo pacchetto contiene il set principale di interfacce per ASP.NET Core identità ed è incluso `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="c1fab-209">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="c1fab-210">Migrazione a identità ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1fab-210">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="c1fab-211">Per altre informazioni e indicazioni sulla migrazione dell'archivio identità esistente, vedere [eseguire la migrazione dell'autenticazione e dell'identità](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="c1fab-211">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="c1fab-212">Impostazione della complessità della password</span><span class="sxs-lookup"><span data-stu-id="c1fab-212">Setting password strength</span></span>

<span data-ttu-id="c1fab-213">Vedere [configurazione](#pw) per un esempio che consente di impostare i requisiti minimi per le password.</span><span class="sxs-lookup"><span data-stu-id="c1fab-213">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1fab-214">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c1fab-214">Next Steps</span></span>

* [<span data-ttu-id="c1fab-215">Configurare Identity</span><span class="sxs-lookup"><span data-stu-id="c1fab-215">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
