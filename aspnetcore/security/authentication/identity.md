---
title: Introduzione all'identità in ASP.NET Core
author: rick-anderson
description: Usare l'identità con un'app ASP.NET Core. Informazioni su come impostare i requisiti per le password (RequireDigit, RequiredLength, RequiredUniqueChars e altro).
ms.author: riande
ms.date: 12/05/2019
uid: security/authentication/identity
ms.openlocfilehash: d4cda43c417a0d8e1b78f8b59b7050debf017269
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828230"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="a54c7-104">Introduzione all'identità in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a54c7-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a54c7-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a54c7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a54c7-106">Identità ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a54c7-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="a54c7-107">È un'API che supporta la funzionalità di accesso dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="a54c7-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="a54c7-108">Gestisce utenti, password, dati di profilo, ruoli, attestazioni, token, conferma tramite posta elettronica e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="a54c7-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="a54c7-109">Gli utenti possono creare un account con le informazioni di accesso archiviate in Identity oppure possono usare un provider di accesso esterno.</span><span class="sxs-lookup"><span data-stu-id="a54c7-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="a54c7-110">I provider di accesso esterni supportati includono [Facebook, Google, account Microsoft e Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="a54c7-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="a54c7-111">Il [codice sorgente di identità](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) è disponibile in GitHub.</span><span class="sxs-lookup"><span data-stu-id="a54c7-111">The [Identity source code](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="a54c7-112">[Identità del patibolo](xref:security/authentication/scaffold-identity) e visualizzare i file generati per esaminare l'interazione del modello con l'identità.</span><span class="sxs-lookup"><span data-stu-id="a54c7-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

<span data-ttu-id="a54c7-113">L'identità viene in genere configurata utilizzando un database SQL Server per archiviare i nomi utente, le password e i dati di profilo.</span><span class="sxs-lookup"><span data-stu-id="a54c7-113">Identity is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="a54c7-114">In alternativa, è possibile usare un altro archivio permanente, ad esempio archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="a54c7-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="a54c7-115">In questo argomento si apprenderà come usare l'identità per la registrazione, l'accesso e la disconnessione di un utente.</span><span class="sxs-lookup"><span data-stu-id="a54c7-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="a54c7-116">Per istruzioni più dettagliate sulla creazione di app che usano l'identità, vedere la sezione passaggi successivi alla fine di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="a54c7-116">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<span data-ttu-id="a54c7-117">[Piattaforma di identità Microsoft](/azure/active-directory/develop/) :</span><span class="sxs-lookup"><span data-stu-id="a54c7-117">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="a54c7-118">Evoluzione della piattaforma per sviluppatori Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a54c7-118">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="a54c7-119">Non correlato all'identità del ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a54c7-119">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="a54c7-120">Consente di [visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([come scaricare)](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="a54c7-120">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="a54c7-121">Creare un'app Web con l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="a54c7-121">Create a Web app with authentication</span></span>

<span data-ttu-id="a54c7-122">Creare un progetto di applicazione Web di ASP.NET Core con singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="a54c7-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a54c7-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a54c7-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a54c7-124">Selezionare **File** > **nuovo** **progetto**>.</span><span class="sxs-lookup"><span data-stu-id="a54c7-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="a54c7-125">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a54c7-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a54c7-126">Denominare il progetto **app Web 1** in modo che abbia lo stesso spazio dei nomi del download del progetto.</span><span class="sxs-lookup"><span data-stu-id="a54c7-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="a54c7-127">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a54c7-127">Click **OK**.</span></span>
* <span data-ttu-id="a54c7-128">Selezionare un' **applicazione Web**ASP.NET Core, quindi selezionare **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="a54c7-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="a54c7-129">Selezionare **singoli account utente** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a54c7-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a54c7-130">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a54c7-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="a54c7-131">Il comando precedente crea un'app Web Razor con SQLite.</span><span class="sxs-lookup"><span data-stu-id="a54c7-131">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="a54c7-132">Per creare l'app Web con il database locale, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a54c7-132">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="a54c7-133">Il progetto generato fornisce [ASP.NET Core identità](xref:security/authentication/identity) come [libreria di classi Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="a54c7-133">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="a54c7-134">La libreria della classe Razor Identity espone gli endpoint con l'area `Identity`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-134">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="a54c7-135">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a54c7-135">For example:</span></span>

* <span data-ttu-id="a54c7-136">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="a54c7-136">/Identity/Account/Login</span></span>
* <span data-ttu-id="a54c7-137">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="a54c7-137">/Identity/Account/Logout</span></span>
* <span data-ttu-id="a54c7-138">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="a54c7-138">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="a54c7-139">Applicare le migrazioni</span><span class="sxs-lookup"><span data-stu-id="a54c7-139">Apply migrations</span></span>

<span data-ttu-id="a54c7-140">Applicare le migrazioni per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="a54c7-140">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a54c7-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a54c7-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a54c7-142">Eseguire il comando seguente nella console di gestione pacchetti (PMC):</span><span class="sxs-lookup"><span data-stu-id="a54c7-142">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a54c7-143">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a54c7-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a54c7-144">Le migrazioni non sono necessarie in questo passaggio quando si usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="a54c7-144">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="a54c7-145">Per il database locale, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a54c7-145">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="a54c7-146">Registro di test e account di accesso</span><span class="sxs-lookup"><span data-stu-id="a54c7-146">Test Register and Login</span></span>

<span data-ttu-id="a54c7-147">Eseguire l'app e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="a54c7-147">Run the app and register a user.</span></span> <span data-ttu-id="a54c7-148">A seconda delle dimensioni dello schermo, potrebbe essere necessario selezionare l'interruttore di spostamento per visualizzare i collegamenti di **Registro** e di **accesso** .</span><span class="sxs-lookup"><span data-stu-id="a54c7-148">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="a54c7-149">Configurare i servizi di identità</span><span class="sxs-lookup"><span data-stu-id="a54c7-149">Configure Identity services</span></span>

<span data-ttu-id="a54c7-150">I servizi vengono aggiunti in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-150">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="a54c7-151">Il modello tipico consiste nel chiamare tutti i metodi `Add{Service}` e quindi chiamare tutti i metodi `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-151">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="a54c7-152">Il codice evidenziato precedente configura l'identità con i valori delle opzioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="a54c7-152">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="a54c7-153">I servizi vengono resi disponibili per l'applicazione tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a54c7-153">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="a54c7-154">L'identità viene abilitata chiamando <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span><span class="sxs-lookup"><span data-stu-id="a54c7-154">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="a54c7-155">`UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="a54c7-155">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="a54c7-156">L'app generata dal modello non usa l' [autorizzazione](xref:security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="a54c7-156">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="a54c7-157">`app.UseAuthorization` è incluso per assicurarsi che venga aggiunto nell'ordine corretto se l'app aggiunge l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="a54c7-157">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="a54c7-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`e `UseEndpoints` devono essere chiamati nell'ordine indicato nel codice precedente.</span><span class="sxs-lookup"><span data-stu-id="a54c7-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="a54c7-159">Per ulteriori informazioni su `IdentityOptions` e `Startup`, vedere <xref:Microsoft.AspNetCore.Identity.IdentityOptions> e [avvio dell'applicazione](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a54c7-159">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="a54c7-160">Registrazione, accesso e disconnessione del patibolo</span><span class="sxs-lookup"><span data-stu-id="a54c7-160">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a54c7-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a54c7-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a54c7-162">Aggiungere i file di registro, di accesso e di disconnessione.</span><span class="sxs-lookup"><span data-stu-id="a54c7-162">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="a54c7-163">Seguire l' [identità del patibolo in un progetto Razor con](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) le istruzioni di autorizzazione per generare il codice illustrato in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="a54c7-163">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a54c7-164">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a54c7-164">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a54c7-165">Se il progetto è stato creato con il nome **app Web 1**, eseguire i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="a54c7-165">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="a54c7-166">In caso contrario, utilizzare lo spazio dei nomi corretto per il `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="a54c7-166">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="a54c7-167">PowerShell usa il punto e virgola come separatore di comandi.</span><span class="sxs-lookup"><span data-stu-id="a54c7-167">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="a54c7-168">Quando si usa PowerShell, usare il carattere di escape per il punto e virgola nell'elenco dei file o inserire l'elenco di file tra virgolette doppie, come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="a54c7-168">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="a54c7-169">Per altre informazioni sull'identità di impalcatura, vedere la pagina relativa all' [identità di impalcatura in un progetto Razor con autorizzazione](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span><span class="sxs-lookup"><span data-stu-id="a54c7-169">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="a54c7-170">Esaminare il registro</span><span class="sxs-lookup"><span data-stu-id="a54c7-170">Examine Register</span></span>

<span data-ttu-id="a54c7-171">Quando un utente fa clic sul collegamento **Register** , viene richiamata l'azione `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-171">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="a54c7-172">L'utente viene creato da [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) nell'oggetto `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-172">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="a54c7-173">`_userManager` viene fornito dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="a54c7-173">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="a54c7-174">Se l'utente è stato creato correttamente, l'utente è connesso tramite la chiamata al metodo `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="a54c7-175">Vedere la [conferma dell'account](xref:security/authentication/accconfirm#prevent-login-at-registration) per i passaggi per impedire l'accesso immediato alla registrazione.</span><span class="sxs-lookup"><span data-stu-id="a54c7-175">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="a54c7-176">Accedi</span><span class="sxs-lookup"><span data-stu-id="a54c7-176">Log in</span></span>

<span data-ttu-id="a54c7-177">Il modulo di accesso viene visualizzato quando:</span><span class="sxs-lookup"><span data-stu-id="a54c7-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="a54c7-178">Il collegamento **Accedi** è selezionato.</span><span class="sxs-lookup"><span data-stu-id="a54c7-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="a54c7-179">Un utente tenta di accedere a una pagina con restrizioni che non è autorizzata ad accedere **o** quando non è stata autenticata dal sistema.</span><span class="sxs-lookup"><span data-stu-id="a54c7-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="a54c7-180">Quando viene inviato il modulo nella pagina di accesso, viene chiamata l'azione `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="a54c7-181">`PasswordSignInAsync` viene chiamato sull'oggetto `_signInManager` (fornito dall'inserimento delle dipendenze).</span><span class="sxs-lookup"><span data-stu-id="a54c7-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="a54c7-182">La classe di base `Controller` espone una proprietà `User` a cui è possibile accedere dai metodi del controller.</span><span class="sxs-lookup"><span data-stu-id="a54c7-182">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="a54c7-183">Ad esempio, è possibile enumerare `User.Claims` e prendere decisioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="a54c7-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="a54c7-184">Per ulteriori informazioni, vedere <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="a54c7-184">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="a54c7-185">Disconnetti</span><span class="sxs-lookup"><span data-stu-id="a54c7-185">Log out</span></span>

<span data-ttu-id="a54c7-186">Il collegamento **Disconnetti** richiama l'azione `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-186">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="a54c7-187">Nel codice precedente, il `return RedirectToPage();` del codice deve essere un reindirizzamento in modo che il browser esegua una nuova richiesta e l'identità per l'utente venga aggiornata.</span><span class="sxs-lookup"><span data-stu-id="a54c7-187">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="a54c7-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) Cancella le attestazioni dell'utente archiviate in un cookie.</span><span class="sxs-lookup"><span data-stu-id="a54c7-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="a54c7-189">Post viene specificato nelle *pagine/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a54c7-189">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="a54c7-190">Identità del test</span><span class="sxs-lookup"><span data-stu-id="a54c7-190">Test Identity</span></span>

<span data-ttu-id="a54c7-191">I modelli di progetto Web predefiniti consentono l'accesso anonimo alle Home page.</span><span class="sxs-lookup"><span data-stu-id="a54c7-191">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="a54c7-192">Per verificare l'identità, aggiungere [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span><span class="sxs-lookup"><span data-stu-id="a54c7-192">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="a54c7-193">Se è stato eseguito l'accesso, disconnettersi. Eseguire l'app e selezionare il collegamento per la **privacy** .</span><span class="sxs-lookup"><span data-stu-id="a54c7-193">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="a54c7-194">Si verrà reindirizzati alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="a54c7-194">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="a54c7-195">Esplora identità</span><span class="sxs-lookup"><span data-stu-id="a54c7-195">Explore Identity</span></span>

<span data-ttu-id="a54c7-196">Per esplorare l'identità in modo più dettagliato:</span><span class="sxs-lookup"><span data-stu-id="a54c7-196">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="a54c7-197">Crea origine interfaccia utente con identità completa</span><span class="sxs-lookup"><span data-stu-id="a54c7-197">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="a54c7-198">Esaminare l'origine di ogni pagina ed eseguire un'istruzione alla volta nel debugger.</span><span class="sxs-lookup"><span data-stu-id="a54c7-198">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="a54c7-199">Componenti Identity</span><span class="sxs-lookup"><span data-stu-id="a54c7-199">Identity Components</span></span>

<span data-ttu-id="a54c7-200">Tutti i pacchetti NuGet dipendenti dall'identità sono inclusi nel [Framework condiviso ASP.NET Core](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span><span class="sxs-lookup"><span data-stu-id="a54c7-200">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="a54c7-201">Il pacchetto primario per Identity è [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="a54c7-201">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="a54c7-202">Questo pacchetto contiene il set principale di interfacce per ASP.NET Core identità ed è incluso `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-202">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="a54c7-203">Migrazione a identità ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a54c7-203">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="a54c7-204">Per altre informazioni e indicazioni sulla migrazione dell'archivio identità esistente, vedere [eseguire la migrazione dell'autenticazione e dell'identità](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="a54c7-204">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="a54c7-205">Impostazione della complessità della password</span><span class="sxs-lookup"><span data-stu-id="a54c7-205">Setting password strength</span></span>

<span data-ttu-id="a54c7-206">Vedere [configurazione](#pw) per un esempio che consente di impostare i requisiti minimi per le password.</span><span class="sxs-lookup"><span data-stu-id="a54c7-206">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="a54c7-207">AddDefaultIdentity e AddIdentity</span><span class="sxs-lookup"><span data-stu-id="a54c7-207">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="a54c7-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> è stato introdotto in ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="a54c7-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="a54c7-209">La chiamata di `AddDefaultIdentity` è simile alla chiamata a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="a54c7-209">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="a54c7-210">Per altre informazioni, vedere [origine AddDefaultIdentity](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="a54c7-210">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a54c7-211">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="a54c7-211">Next Steps</span></span>

* <span data-ttu-id="a54c7-212">Per informazioni sulla configurazione dell'identità con SQLite, vedere [questo problema di GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/5131) .</span><span class="sxs-lookup"><span data-stu-id="a54c7-212">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* [<span data-ttu-id="a54c7-213">Configurare Identity</span><span class="sxs-lookup"><span data-stu-id="a54c7-213">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a54c7-214">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a54c7-214">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a54c7-215">ASP.NET Core identità è un sistema di appartenenza che aggiunge la funzionalità di accesso alle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a54c7-215">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="a54c7-216">Gli utenti possono creare un account con le informazioni di accesso archiviate in Identity oppure possono usare un provider di accesso esterno.</span><span class="sxs-lookup"><span data-stu-id="a54c7-216">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="a54c7-217">I provider di accesso esterni supportati includono [Facebook, Google, account Microsoft e Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="a54c7-217">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="a54c7-218">L'identità può essere configurata utilizzando un database SQL Server per archiviare i nomi utente, le password e i dati di profilo.</span><span class="sxs-lookup"><span data-stu-id="a54c7-218">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="a54c7-219">In alternativa, è possibile usare un altro archivio permanente, ad esempio archiviazione tabelle di Azure.</span><span class="sxs-lookup"><span data-stu-id="a54c7-219">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="a54c7-220">Consente di [visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([come scaricare)](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="a54c7-220">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="a54c7-221">In questo argomento si apprenderà come usare l'identità per la registrazione, l'accesso e la disconnessione di un utente.</span><span class="sxs-lookup"><span data-stu-id="a54c7-221">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="a54c7-222">Per istruzioni più dettagliate sulla creazione di app che usano l'identità, vedere la sezione passaggi successivi alla fine di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="a54c7-222">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="a54c7-223">AddDefaultIdentity e AddIdentity</span><span class="sxs-lookup"><span data-stu-id="a54c7-223">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="a54c7-224"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> è stato introdotto in ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="a54c7-224"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="a54c7-225">La chiamata di `AddDefaultIdentity` è simile alla chiamata a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="a54c7-225">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="a54c7-226">Per altre informazioni, vedere [origine AddDefaultIdentity](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="a54c7-226">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="a54c7-227">Creare un'app Web con l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="a54c7-227">Create a Web app with authentication</span></span>

<span data-ttu-id="a54c7-228">Creare un progetto di applicazione Web di ASP.NET Core con singoli account utente.</span><span class="sxs-lookup"><span data-stu-id="a54c7-228">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a54c7-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a54c7-229">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a54c7-230">Selezionare **File** > **nuovo** **progetto**>.</span><span class="sxs-lookup"><span data-stu-id="a54c7-230">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="a54c7-231">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a54c7-231">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a54c7-232">Denominare il progetto **app Web 1** in modo che abbia lo stesso spazio dei nomi del download del progetto.</span><span class="sxs-lookup"><span data-stu-id="a54c7-232">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="a54c7-233">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a54c7-233">Click **OK**.</span></span>
* <span data-ttu-id="a54c7-234">Selezionare un' **applicazione Web**ASP.NET Core, quindi selezionare **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="a54c7-234">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="a54c7-235">Selezionare **singoli account utente** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a54c7-235">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a54c7-236">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a54c7-236">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="a54c7-237">Il progetto generato fornisce [ASP.NET Core identità](xref:security/authentication/identity) come [libreria di classi Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="a54c7-237">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="a54c7-238">La libreria della classe Razor Identity espone gli endpoint con l'area `Identity`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-238">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="a54c7-239">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a54c7-239">For example:</span></span>

* <span data-ttu-id="a54c7-240">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="a54c7-240">/Identity/Account/Login</span></span>
* <span data-ttu-id="a54c7-241">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="a54c7-241">/Identity/Account/Logout</span></span>
* <span data-ttu-id="a54c7-242">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="a54c7-242">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="a54c7-243">Applicare le migrazioni</span><span class="sxs-lookup"><span data-stu-id="a54c7-243">Apply migrations</span></span>

<span data-ttu-id="a54c7-244">Applicare le migrazioni per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="a54c7-244">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a54c7-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a54c7-245">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a54c7-246">Eseguire il comando seguente nella console di gestione pacchetti (PMC):</span><span class="sxs-lookup"><span data-stu-id="a54c7-246">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a54c7-247">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a54c7-247">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="a54c7-248">Registro di test e account di accesso</span><span class="sxs-lookup"><span data-stu-id="a54c7-248">Test Register and Login</span></span>

<span data-ttu-id="a54c7-249">Eseguire l'app e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="a54c7-249">Run the app and register a user.</span></span> <span data-ttu-id="a54c7-250">A seconda delle dimensioni dello schermo, potrebbe essere necessario selezionare l'interruttore di spostamento per visualizzare i collegamenti di **Registro** e di **accesso** .</span><span class="sxs-lookup"><span data-stu-id="a54c7-250">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="a54c7-251">Configurare i servizi di identità</span><span class="sxs-lookup"><span data-stu-id="a54c7-251">Configure Identity services</span></span>

<span data-ttu-id="a54c7-252">I servizi vengono aggiunti in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-252">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="a54c7-253">Il modello tipico consiste nel chiamare tutti i metodi `Add{Service}` e quindi chiamare tutti i metodi `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-253">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="a54c7-254">Il codice precedente configura l'identità con i valori delle opzioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="a54c7-254">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="a54c7-255">I servizi vengono resi disponibili per l'applicazione tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a54c7-255">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="a54c7-256">L'identità viene abilitata chiamando [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="a54c7-256">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="a54c7-257">`UseAuthentication` aggiunge l'autenticazione [middleware](xref:fundamentals/middleware/index) alla pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="a54c7-257">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="a54c7-258">Per ulteriori informazioni, vedere la [Classe IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) e l' [avvio dell'applicazione](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a54c7-258">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="a54c7-259">Registrazione, accesso e disconnessione del patibolo</span><span class="sxs-lookup"><span data-stu-id="a54c7-259">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="a54c7-260">Seguire l' [identità del patibolo in un progetto Razor con](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) le istruzioni di autorizzazione per generare il codice illustrato in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="a54c7-260">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a54c7-261">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a54c7-261">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a54c7-262">Aggiungere i file di registro, di accesso e di disconnessione.</span><span class="sxs-lookup"><span data-stu-id="a54c7-262">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a54c7-263">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a54c7-263">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a54c7-264">Se il progetto è stato creato con il nome **app Web 1**, eseguire i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="a54c7-264">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="a54c7-265">In caso contrario, utilizzare lo spazio dei nomi corretto per il `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="a54c7-265">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="a54c7-266">PowerShell usa il punto e virgola come separatore di comandi.</span><span class="sxs-lookup"><span data-stu-id="a54c7-266">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="a54c7-267">Quando si usa PowerShell, usare il carattere di escape per il punto e virgola nell'elenco dei file o inserire l'elenco di file tra virgolette doppie, come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="a54c7-267">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="a54c7-268">Esaminare il registro</span><span class="sxs-lookup"><span data-stu-id="a54c7-268">Examine Register</span></span>

<span data-ttu-id="a54c7-269">Quando un utente fa clic sul collegamento **Register** , viene richiamata l'azione `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-269">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="a54c7-270">L'utente viene creato da [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) nell'oggetto `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-270">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="a54c7-271">`_userManager` viene fornito dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="a54c7-271">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="a54c7-272">Se l'utente è stato creato correttamente, l'utente è connesso tramite la chiamata al metodo `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-272">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="a54c7-273">**Nota:** Vedere la [conferma dell'account](xref:security/authentication/accconfirm#prevent-login-at-registration) per i passaggi per impedire l'accesso immediato alla registrazione.</span><span class="sxs-lookup"><span data-stu-id="a54c7-273">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="a54c7-274">Accedi</span><span class="sxs-lookup"><span data-stu-id="a54c7-274">Log in</span></span>

<span data-ttu-id="a54c7-275">Il modulo di accesso viene visualizzato quando:</span><span class="sxs-lookup"><span data-stu-id="a54c7-275">The Login form is displayed when:</span></span>

* <span data-ttu-id="a54c7-276">Il collegamento **Accedi** è selezionato.</span><span class="sxs-lookup"><span data-stu-id="a54c7-276">The **Log in** link is selected.</span></span>
* <span data-ttu-id="a54c7-277">Un utente tenta di accedere a una pagina con restrizioni che non è autorizzata ad accedere **o** quando non è stata autenticata dal sistema.</span><span class="sxs-lookup"><span data-stu-id="a54c7-277">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="a54c7-278">Quando viene inviato il modulo nella pagina di accesso, viene chiamata l'azione `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-278">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="a54c7-279">`PasswordSignInAsync` viene chiamato sull'oggetto `_signInManager` (fornito dall'inserimento delle dipendenze).</span><span class="sxs-lookup"><span data-stu-id="a54c7-279">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="a54c7-280">La classe base `Controller` espone una proprietà  `User` a cui è possibile accedere dai metodi del controller.</span><span class="sxs-lookup"><span data-stu-id="a54c7-280">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="a54c7-281">Ad esempio, è possibile enumerare `User.Claims` e prendere decisioni di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="a54c7-281">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="a54c7-282">Per ulteriori informazioni, vedere <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="a54c7-282">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="a54c7-283">Disconnetti</span><span class="sxs-lookup"><span data-stu-id="a54c7-283">Log out</span></span>

<span data-ttu-id="a54c7-284">Il collegamento **Disconnetti** richiama l'azione `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-284">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="a54c7-285">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) Cancella le attestazioni dell'utente archiviate in un cookie.</span><span class="sxs-lookup"><span data-stu-id="a54c7-285">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="a54c7-286">Post viene specificato nelle *pagine/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a54c7-286">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="a54c7-287">Identità del test</span><span class="sxs-lookup"><span data-stu-id="a54c7-287">Test Identity</span></span>

<span data-ttu-id="a54c7-288">I modelli di progetto Web predefiniti consentono l'accesso anonimo alle Home page.</span><span class="sxs-lookup"><span data-stu-id="a54c7-288">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="a54c7-289">Per verificare l'identità, aggiungere [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) alla pagina privacy.</span><span class="sxs-lookup"><span data-stu-id="a54c7-289">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="a54c7-290">Se è stato eseguito l'accesso, disconnettersi. Eseguire l'app e selezionare il collegamento per la **privacy** .</span><span class="sxs-lookup"><span data-stu-id="a54c7-290">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="a54c7-291">Si verrà reindirizzati alla pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="a54c7-291">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="a54c7-292">Esplora identità</span><span class="sxs-lookup"><span data-stu-id="a54c7-292">Explore Identity</span></span>

<span data-ttu-id="a54c7-293">Per esplorare l'identità in modo più dettagliato:</span><span class="sxs-lookup"><span data-stu-id="a54c7-293">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="a54c7-294">Crea origine interfaccia utente con identità completa</span><span class="sxs-lookup"><span data-stu-id="a54c7-294">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="a54c7-295">Esaminare l'origine di ogni pagina ed eseguire un'istruzione alla volta nel debugger.</span><span class="sxs-lookup"><span data-stu-id="a54c7-295">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="a54c7-296">Componenti Identity</span><span class="sxs-lookup"><span data-stu-id="a54c7-296">Identity Components</span></span>

<span data-ttu-id="a54c7-297">Tutti i pacchetti NuGet dipendenti dall'identità sono inclusi nel [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a54c7-297">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a54c7-298">Il pacchetto primario per Identity è [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="a54c7-298">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="a54c7-299">Questo pacchetto contiene il set principale di interfacce per ASP.NET Core identità ed è incluso `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="a54c7-299">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="a54c7-300">Migrazione a identità ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a54c7-300">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="a54c7-301">Per altre informazioni e indicazioni sulla migrazione dell'archivio identità esistente, vedere [eseguire la migrazione dell'autenticazione e dell'identità](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="a54c7-301">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="a54c7-302">Impostazione della complessità della password</span><span class="sxs-lookup"><span data-stu-id="a54c7-302">Setting password strength</span></span>

<span data-ttu-id="a54c7-303">Vedere [configurazione](#pw) per un esempio che consente di impostare i requisiti minimi per le password.</span><span class="sxs-lookup"><span data-stu-id="a54c7-303">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a54c7-304">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="a54c7-304">Next Steps</span></span>

* <span data-ttu-id="a54c7-305">Per informazioni sulla configurazione dell'identità con SQLite, vedere [questo problema di GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/5131) .</span><span class="sxs-lookup"><span data-stu-id="a54c7-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* [<span data-ttu-id="a54c7-306">Configurare Identity</span><span class="sxs-lookup"><span data-stu-id="a54c7-306">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
