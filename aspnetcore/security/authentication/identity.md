---
title: Introduzione all'identità in ASP.NET Core
author: rick-anderson
description: Usare l'identità con un'app ASP.NET Core. Informazioni su come impostare i requisiti per le password (RequireDigit, RequiredLength, RequiredUniqueChars e altro).
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 979681cfc196aca9fb5097583d99a086e1c597ba
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082457"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="a99f4-104">Introduzione all'identità in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a99f4-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="a99f4-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a99f4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a99f4-106">ASP.NET Core identità è un sistema di appartenenza che aggiunge la funzionalità di accesso alle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a99f4-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="a99f4-107">Gli utenti possono creare un account con le informazioni di accesso archiviate in Identity oppure possono usare un provider di accesso esterno.</span><span class="sxs-lookup"><span data-stu-id="a99f4-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="a99f4-108">I provider di accesso esterni supportati includono [Facebook, Google, account Microsoft e Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="a99f4-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="a99f4-109">L'identità può essere configurata utilizzando un database SQL Server per archiviare i nomi utente, le password e i dati di profilo.</span><span class="sxs-lookup"><span data-stu-id="a99f4-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="a99f4-110">In alternativa, è possibile usare un altro archivio permanente, ad esempio archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="a99f4-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="a99f4-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([come scaricare)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a99f4-111">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="a99f4-112">In questo argomento si apprenderà come usare l'identità per la registrazione, l'accesso e la disconnessione di un utente.</span><span class="sxs-lookup"><span data-stu-id="a99f4-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="a99f4-113">Per istruzioni più dettagliate sulla creazione di app che usano l'identità, vedere la sezione passaggi successivi alla fine di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="a99f4-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="a99f4-114">AddDefaultIdentity e AddIdentity</span><span class="sxs-lookup"><span data-stu-id="a99f4-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="a99f4-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) è stato introdotto in ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="a99f4-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="a99f4-116">La `AddDefaultIdentity` chiamata a è simile alla chiamata a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="a99f4-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="a99f4-117">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="a99f4-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="a99f4-118">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="a99f4-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="a99f4-119">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="a99f4-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="a99f4-120">Per altre informazioni, vedere [origine AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="a99f4-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="a99f4-121">Creare un'app Web con l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="a99f4-121">Create a Web app with authentication</span></span>

<span data-ttu-id="a99f4-122">Creare un progetto di applicazione Web di ASP.NET Core con singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="a99f4-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a99f4-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a99f4-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a99f4-124">Selezionare **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="a99f4-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="a99f4-125">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a99f4-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a99f4-126">Denominare il progetto **app Web 1** in modo che abbia lo stesso spazio dei nomi del download del progetto.</span><span class="sxs-lookup"><span data-stu-id="a99f4-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="a99f4-127">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a99f4-127">Click **OK**.</span></span>
* <span data-ttu-id="a99f4-128">Selezionare un' **applicazione Web**ASP.NET Core, quindi selezionare **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="a99f4-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="a99f4-129">Selezionare **singoli account utente** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a99f4-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a99f4-130">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a99f4-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="a99f4-131">Il progetto generato fornisce [ASP.NET Core identità](xref:security/authentication/identity) come [libreria di classi Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="a99f4-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="a99f4-132">La libreria della classe Razor Identity espone gli endpoint con `Identity` l'area.</span><span class="sxs-lookup"><span data-stu-id="a99f4-132">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="a99f4-133">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a99f4-133">For example:</span></span>

* <span data-ttu-id="a99f4-134">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="a99f4-134">/Identity/Account/Login</span></span>
* <span data-ttu-id="a99f4-135">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="a99f4-135">/Identity/Account/Logout</span></span>
* <span data-ttu-id="a99f4-136">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="a99f4-136">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="a99f4-137">Applicare le migrazioni</span><span class="sxs-lookup"><span data-stu-id="a99f4-137">Apply migrations</span></span>

<span data-ttu-id="a99f4-138">Applicare le migrazioni per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="a99f4-138">Apply the migrations to initialise the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a99f4-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a99f4-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a99f4-140">Eseguire il comando seguente nella console di gestione pacchetti (PMC):</span><span class="sxs-lookup"><span data-stu-id="a99f4-140">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a99f4-141">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a99f4-141">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="a99f4-142">Registro di test e account di accesso</span><span class="sxs-lookup"><span data-stu-id="a99f4-142">Test Register and Login</span></span>

<span data-ttu-id="a99f4-143">Eseguire l'app e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="a99f4-143">Run the app and register a user.</span></span> <span data-ttu-id="a99f4-144">A seconda delle dimensioni dello schermo, potrebbe essere necessario selezionare l'interruttore di spostamento per visualizzare i collegamenti di **Registro** e di **accesso** .</span><span class="sxs-lookup"><span data-stu-id="a99f4-144">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="a99f4-145">Configurare i servizi di identità</span><span class="sxs-lookup"><span data-stu-id="a99f4-145">Configure Identity services</span></span>

<span data-ttu-id="a99f4-146">I servizi vengono aggiunti `ConfigureServices`in.</span><span class="sxs-lookup"><span data-stu-id="a99f4-146">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="a99f4-147">Il modello tipico consiste nel chiamare tutti i metodi `Add{Service}` e quindi chiamare tutti i metodi `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="a99f4-147">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="a99f4-148">Il codice precedente configura l'identità con i valori delle opzioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="a99f4-148">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="a99f4-149">I servizi vengono resi disponibili per l'applicazione tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a99f4-149">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="a99f4-150">L'identità viene abilitata chiamando [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="a99f4-150">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="a99f4-151">`UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="a99f4-151">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="a99f4-152">I servizi vengono resi disponibili per l'applicazione tramite l' [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a99f4-152">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="a99f4-153">Identità è abilitata per l'applicazione chiamando `UseAuthentication` nel metodo`Configure`.</span><span class="sxs-lookup"><span data-stu-id="a99f4-153">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="a99f4-154">`UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="a99f4-154">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="a99f4-155">Questi servizi vengono resi disponibili per l'applicazione tramite l' [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a99f4-155">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="a99f4-156">Identità è abilitata per l'applicazione chiamando `UseIdentity` nel metodo`Configure`.</span><span class="sxs-lookup"><span data-stu-id="a99f4-156">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="a99f4-157">`UseIdentity`aggiunge il [middleware](xref:fundamentals/middleware/index) di autenticazione basato su cookie alla pipeline di richieste.</span><span class="sxs-lookup"><span data-stu-id="a99f4-157">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="a99f4-158">Per ulteriori informazioni, vedere la [Classe IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) e l' [avvio dell'applicazione](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a99f4-158">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="a99f4-159">Registrazione, accesso e disconnessione del patibolo</span><span class="sxs-lookup"><span data-stu-id="a99f4-159">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="a99f4-160">Seguire l' [identità del patibolo in un progetto Razor con](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) le istruzioni di autorizzazione per generare il codice illustrato in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="a99f4-160">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a99f4-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a99f4-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a99f4-162">Aggiungere i file di registro, di accesso e di disconnessione.</span><span class="sxs-lookup"><span data-stu-id="a99f4-162">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a99f4-163">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a99f4-163">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a99f4-164">Se il progetto è stato creato con il nome **app Web 1**, eseguire i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="a99f4-164">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="a99f4-165">In caso contrario, utilizzare lo spazio dei `ApplicationDbContext`nomi corretto per:</span><span class="sxs-lookup"><span data-stu-id="a99f4-165">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="a99f4-166">PowerShell usa il punto e virgola come separatore di comandi.</span><span class="sxs-lookup"><span data-stu-id="a99f4-166">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="a99f4-167">Quando si usa PowerShell, usare il carattere di escape per il punto e virgola nell'elenco dei file o inserire l'elenco di file tra virgolette doppie, come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="a99f4-167">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="a99f4-168">Esaminare il registro</span><span class="sxs-lookup"><span data-stu-id="a99f4-168">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="a99f4-169">Quando un utente fa clic sul collegamento **Register** , `RegisterModel.OnPostAsync` viene richiamata l'azione.</span><span class="sxs-lookup"><span data-stu-id="a99f4-169">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="a99f4-170">L'utente viene creato da [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) sull' `_userManager` oggetto.</span><span class="sxs-lookup"><span data-stu-id="a99f4-170">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="a99f4-171">`_userManager`viene fornito dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="a99f4-171">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="a99f4-172">Quando un utente fa clic sul collegamento **Register** , `Register` l'azione viene richiamata in. `AccountController`</span><span class="sxs-lookup"><span data-stu-id="a99f4-172">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="a99f4-173">L'azione`Register` crea l'utente chiamandoconsente all'utente chiamando  `CreateAsync` sull'oggetto  `_userManager`(fornito all `AccountController` tramite dependency injection):</span><span class="sxs-lookup"><span data-stu-id="a99f4-173">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="a99f4-174">Se l'utente è stato creato correttamente, l'utente è connesso tramite la chiamata al metodo `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="a99f4-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="a99f4-175">**Nota:** Vedere la [conferma dell'account](xref:security/authentication/accconfirm#prevent-login-at-registration) per i passaggi per impedire l'accesso immediato alla registrazione.</span><span class="sxs-lookup"><span data-stu-id="a99f4-175">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="a99f4-176">Accedi</span><span class="sxs-lookup"><span data-stu-id="a99f4-176">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a99f4-177">Il modulo di accesso viene visualizzato quando:</span><span class="sxs-lookup"><span data-stu-id="a99f4-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="a99f4-178">Il collegamento **Accedi** è selezionato.</span><span class="sxs-lookup"><span data-stu-id="a99f4-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="a99f4-179">Un utente tenta di accedere a una pagina con restrizioni che non è autorizzata ad accedere **o** quando non è stata autenticata dal sistema.</span><span class="sxs-lookup"><span data-stu-id="a99f4-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="a99f4-180">Quando viene inviato il modulo nella pagina di accesso, viene `OnPostAsync` chiamata l'azione.</span><span class="sxs-lookup"><span data-stu-id="a99f4-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="a99f4-181">`PasswordSignInAsync`viene chiamato sull' `_signInManager` oggetto, fornito dall'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="a99f4-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="a99f4-182">La classe base `Controller` espone una proprietà  `User` a cui è possibile accedere dai metodi del controller.</span><span class="sxs-lookup"><span data-stu-id="a99f4-182">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="a99f4-183">Ad esempio, è possibile enumerare `User.Claims` e prendere decisioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="a99f4-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="a99f4-184">Per altre informazioni, vedere <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="a99f4-184">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a99f4-185">Il modulo di accesso viene visualizzato quando gli utenti selezionano il collegamento **Accedi** o vengono reindirizzati quando si accede a una pagina che richiede l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="a99f4-185">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="a99f4-186">Quando l'utente invia il form nella pagina di accesso, il metodo di azione`AccountController`nell `Login`viene invocato.</span><span class="sxs-lookup"><span data-stu-id="a99f4-186">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="a99f4-187">L'azione `Login` chiama `PasswordSignInAsync` sul `_signInManager` oggetto (fornito all `AccountController` tramite dependency injection).</span><span class="sxs-lookup"><span data-stu-id="a99f4-187">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="a99f4-188">La classe di`Controller` base `PageModel`(o) espone `User` una proprietà.</span><span class="sxs-lookup"><span data-stu-id="a99f4-188">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="a99f4-189">Ad esempio, `User.Claims` può essere enumerato per prendere decisioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="a99f4-189">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="a99f4-190">Disconnetti</span><span class="sxs-lookup"><span data-stu-id="a99f4-190">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a99f4-191">Il collegamento **Disconnetti** richiama l' `LogoutModel.OnPost` azione.</span><span class="sxs-lookup"><span data-stu-id="a99f4-191">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="a99f4-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) Cancella le attestazioni dell'utente archiviate in un cookie.</span><span class="sxs-lookup"><span data-stu-id="a99f4-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="a99f4-193">Post è specificato in *pages/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a99f4-193">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="a99f4-194">Fare clic sul collegamento **disconnettersi** per chiamare l'azione `LogOut`.</span><span class="sxs-lookup"><span data-stu-id="a99f4-194">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="a99f4-195">Il codice precedente chiama il `_signInManager.SignOutAsync` metodo.</span><span class="sxs-lookup"><span data-stu-id="a99f4-195">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="a99f4-196">Il metodo `SignOutAsync` cancella attestazioni dell'utente archiviate in un cookie.</span><span class="sxs-lookup"><span data-stu-id="a99f4-196">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="a99f4-197">Identità del test</span><span class="sxs-lookup"><span data-stu-id="a99f4-197">Test Identity</span></span>

<span data-ttu-id="a99f4-198">I modelli di progetto Web predefiniti consentono l'accesso anonimo alle Home page.</span><span class="sxs-lookup"><span data-stu-id="a99f4-198">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="a99f4-199">Per verificare l'identità, [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) aggiungere alla pagina privacy.</span><span class="sxs-lookup"><span data-stu-id="a99f4-199">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

<span data-ttu-id="a99f4-200">Se è stato eseguito l'accesso, disconnettersi. Eseguire l'app e selezionare il collegamento per la **privacy** .</span><span class="sxs-lookup"><span data-stu-id="a99f4-200">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="a99f4-201">Si verrà reindirizzati alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="a99f4-201">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="a99f4-202">Esplora identità</span><span class="sxs-lookup"><span data-stu-id="a99f4-202">Explore Identity</span></span>

<span data-ttu-id="a99f4-203">Per esplorare l'identità in modo più dettagliato:</span><span class="sxs-lookup"><span data-stu-id="a99f4-203">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="a99f4-204">Crea origine interfaccia utente con identità completa</span><span class="sxs-lookup"><span data-stu-id="a99f4-204">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="a99f4-205">Esaminare l'origine di ogni pagina ed eseguire un'istruzione alla volta nel debugger.</span><span class="sxs-lookup"><span data-stu-id="a99f4-205">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="a99f4-206">Componenti Identity</span><span class="sxs-lookup"><span data-stu-id="a99f4-206">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a99f4-207">Tutti i pacchetti NuGet dipendenti dall'identità sono inclusi nel [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a99f4-207">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="a99f4-208">Il pacchetto primario per Identity è [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="a99f4-208">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="a99f4-209">Questo pacchetto contiene il set principale di interfacce per ASP.NET Core identità ed è incluso in `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="a99f4-209">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="a99f4-210">Migrazione a identità ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a99f4-210">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="a99f4-211">Per altre informazioni e indicazioni sulla migrazione dell'archivio identità esistente, vedere [eseguire la migrazione dell'autenticazione e dell'identità](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="a99f4-211">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="a99f4-212">Impostazione della complessità della password</span><span class="sxs-lookup"><span data-stu-id="a99f4-212">Setting password strength</span></span>

<span data-ttu-id="a99f4-213">Vedere [configurazione](#pw) per un esempio che consente di impostare i requisiti minimi per le password.</span><span class="sxs-lookup"><span data-stu-id="a99f4-213">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a99f4-214">Fasi successive</span><span class="sxs-lookup"><span data-stu-id="a99f4-214">Next Steps</span></span>

* [<span data-ttu-id="a99f4-215">Configurare Identity</span><span class="sxs-lookup"><span data-stu-id="a99f4-215">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
