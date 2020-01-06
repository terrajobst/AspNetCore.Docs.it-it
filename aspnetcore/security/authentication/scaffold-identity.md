---
title: Identità di impalcatura nei progetti ASP.NET Core
author: rick-anderson
description: Informazioni su come eseguire l'impalcatura dell'identità in un progetto ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 2432d346d9678157848a38fa01d9057cdd7503ff
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75356271"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="30982-103">Identità di impalcatura nei progetti ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="30982-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="30982-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="30982-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="30982-105">ASP.NET Core fornisce [ASP.NET Core identità](xref:security/authentication/identity) come [libreria di classi Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="30982-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="30982-106">Le applicazioni che includono l'identità possono applicare l'impalcatura per aggiungere selettivamente il codice sorgente contenuto nella libreria di classi Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="30982-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="30982-107">Se si vuole generare un codice sorgente, è possibile modificare il codice e modificarne il comportamento.</span><span class="sxs-lookup"><span data-stu-id="30982-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="30982-108">Ad esempio, è possibile indicare allo scaffolder di generare il codice usato nella registrazione.</span><span class="sxs-lookup"><span data-stu-id="30982-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="30982-109">Il codice generato ha la precedenza rispetto allo stesso codice nella libreria di classi Razor per Identity.</span><span class="sxs-lookup"><span data-stu-id="30982-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="30982-110">Per ottenere il controllo completo dell'interfaccia utente e non usare il RCL predefinito, vedere la sezione [creare l'origine dell'interfaccia utente Full Identity](#full).</span><span class="sxs-lookup"><span data-stu-id="30982-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="30982-111">Le applicazioni che **non** includono l'autenticazione possono applicare l'impalcatura per aggiungere il pacchetto di identità RCL.</span><span class="sxs-lookup"><span data-stu-id="30982-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="30982-112">È possibile selezionare il codice Identity da generare.</span><span class="sxs-lookup"><span data-stu-id="30982-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="30982-113">Sebbene l'impalcatura generi la maggior parte del codice necessario, è necessario aggiornare il progetto per completare il processo.</span><span class="sxs-lookup"><span data-stu-id="30982-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="30982-114">Questo documento illustra i passaggi necessari per completare un aggiornamento dell'impalcatura delle identità.</span><span class="sxs-lookup"><span data-stu-id="30982-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="30982-115">È consigliabile usare un sistema di controllo del codice sorgente che mostra le differenze dei file e consente di eseguire il backup delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="30982-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="30982-116">Controllare le modifiche dopo l'esecuzione dell'impalcatura di identità.</span><span class="sxs-lookup"><span data-stu-id="30982-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="30982-117">I servizi sono necessari quando si usa [l'autenticazione a due fattori](xref:security/authentication/identity-enable-qrcodes), la [conferma dell'account e il recupero della password](xref:security/authentication/accconfirm)e altre funzionalità di sicurezza con identità.</span><span class="sxs-lookup"><span data-stu-id="30982-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="30982-118">I servizi o gli stub di servizio non vengono generati durante l'identità di ponteggi.</span><span class="sxs-lookup"><span data-stu-id="30982-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="30982-119">I servizi per abilitare queste funzionalità devono essere aggiunti manualmente.</span><span class="sxs-lookup"><span data-stu-id="30982-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="30982-120">Vedere ad esempio [Richiedi conferma tramite posta elettronica](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="30982-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="30982-121">Questo documento contiene istruzioni più complete rispetto al file *ScaffoldingReadme. txt* che viene generato quando si esegue l'impalcatura.</span><span class="sxs-lookup"><span data-stu-id="30982-121">This document contains more complete instructions than the *ScaffoldingReadme.txt* file which is generated when running the the scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="30982-122">Identità del patibolo in un progetto vuoto</span><span class="sxs-lookup"><span data-stu-id="30982-122">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="30982-123">Aggiornare la classe `Startup` con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="30982-123">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="30982-124">Identità del patibolo in un progetto Razor senza autorizzazione esistente</span><span class="sxs-lookup"><span data-stu-id="30982-124">Scaffold identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef database drop
dotnet ef migrations add CreateIdentitySchema0
dotnet ef database update
-->

<!-- ERROR
There is already an object named 'AspNetRoles' in the database.

Fixed via dotnet ef database drop
before dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="30982-125">L'identità è configurata in *areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="30982-125">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="30982-126">Per ulteriori informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="30982-126">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="30982-127">Migrazioni, UseAuthentication e layout</span><span class="sxs-lookup"><span data-stu-id="30982-127">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="30982-128">Abilitare l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="30982-128">Enable authentication</span></span>

<span data-ttu-id="30982-129">Aggiornare la classe `Startup` con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="30982-129">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="30982-130">Modifiche del layout</span><span class="sxs-lookup"><span data-stu-id="30982-130">Layout changes</span></span>

<span data-ttu-id="30982-131">Facoltativo: aggiungere l'account di accesso parziale (`_LoginPartial`) al file di layout:</span><span class="sxs-lookup"><span data-stu-id="30982-131">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="30982-132">Identità del patibolo in un progetto Razor con autorizzazione</span><span class="sxs-lookup"><span data-stu-id="30982-132">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="30982-133">Alcune opzioni di identità sono configurate in *areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="30982-133">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="30982-134">Per ulteriori informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="30982-134">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="30982-135">Identità del patibolo in un progetto MVC senza autorizzazione esistente</span><span class="sxs-lookup"><span data-stu-id="30982-135">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="30982-136">Facoltativo: aggiungere l'account di accesso parziale (`_LoginPartial`) al file *Views/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="30982-136">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="30982-137">Spostare il file *pages/Shared/_LoginPartial. cshtml* in *Views/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="30982-137">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="30982-138">L'identità è configurata in *areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="30982-138">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="30982-139">Per ulteriori informazioni, vedere IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="30982-139">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="30982-140">Aggiornare la classe `Startup` con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="30982-140">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="30982-141">Identità del patibolo in un progetto MVC con autorizzazione</span><span class="sxs-lookup"><span data-stu-id="30982-141">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="30982-142">Crea origine interfaccia utente con identità completa</span><span class="sxs-lookup"><span data-stu-id="30982-142">Create full identity UI source</span></span>

<span data-ttu-id="30982-143">Per mantenere il controllo completo dell'interfaccia utente di identità, eseguire l'impalcatura delle identità e selezionare **Sostituisci tutti i file**.</span><span class="sxs-lookup"><span data-stu-id="30982-143">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="30982-144">Il codice evidenziato seguente mostra le modifiche per sostituire l'interfaccia utente di identità predefinita con Identity in un'app Web ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="30982-144">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="30982-145">Questa operazione può essere eseguita per avere il controllo completo dell'interfaccia utente di identità.</span><span class="sxs-lookup"><span data-stu-id="30982-145">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="30982-146">L'identità predefinita viene sostituita dal codice seguente:</span><span class="sxs-lookup"><span data-stu-id="30982-146">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="30982-147">Il codice seguente imposta [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)e [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="30982-147">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="30982-148">Registrare un'implementazione di `IEmailSender`, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="30982-148">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="30982-149">Disabilita pagina Registro</span><span class="sxs-lookup"><span data-stu-id="30982-149">Disable register page</span></span>

<span data-ttu-id="30982-150">Per disabilitare la registrazione dell'utente:</span><span class="sxs-lookup"><span data-stu-id="30982-150">To disable user registration:</span></span>

* <span data-ttu-id="30982-151">Identità del patibolo.</span><span class="sxs-lookup"><span data-stu-id="30982-151">Scaffold Identity.</span></span> <span data-ttu-id="30982-152">Includere account. Register, account. login e account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="30982-152">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="30982-153">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="30982-153">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="30982-154">Aggiornare le *aree/Identity/Pages/account/Register. cshtml. cs,* in modo che gli utenti non possano effettuare la registrazione da questo endpoint:</span><span class="sxs-lookup"><span data-stu-id="30982-154">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="30982-155">Aggiornare le *aree/Identity/Pages/account/Register. cshtml* in modo che siano coerenti con le modifiche precedenti:</span><span class="sxs-lookup"><span data-stu-id="30982-155">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="30982-156">Impostare come commento o rimuovere il collegamento di registrazione da *aree/Identity/Pages/account/login. cshtml*</span><span class="sxs-lookup"><span data-stu-id="30982-156">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="30982-157">Aggiornare la pagina *aree/identità/pagine/account/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="30982-157">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="30982-158">Rimuovere il codice e i collegamenti dal file cshtml.</span><span class="sxs-lookup"><span data-stu-id="30982-158">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="30982-159">Rimuovere il codice di conferma dal `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="30982-159">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="30982-160">Usare un'altra app per aggiungere utenti</span><span class="sxs-lookup"><span data-stu-id="30982-160">Use another app to add users</span></span>

<span data-ttu-id="30982-161">Fornire un meccanismo per aggiungere utenti all'esterno dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="30982-161">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="30982-162">Le opzioni per aggiungere utenti includono:</span><span class="sxs-lookup"><span data-stu-id="30982-162">Options to add users include:</span></span>

* <span data-ttu-id="30982-163">Un'app Web amministrativa dedicata.</span><span class="sxs-lookup"><span data-stu-id="30982-163">A dedicated admin web app.</span></span>
* <span data-ttu-id="30982-164">Un'app console.</span><span class="sxs-lookup"><span data-stu-id="30982-164">A console app.</span></span>

<span data-ttu-id="30982-165">Il codice seguente illustra un approccio per l'aggiunta di utenti:</span><span class="sxs-lookup"><span data-stu-id="30982-165">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="30982-166">Un elenco di utenti viene letto in memoria.</span><span class="sxs-lookup"><span data-stu-id="30982-166">A list of users is read into memory.</span></span>
* <span data-ttu-id="30982-167">Per ogni utente viene generata una password univoca complessa.</span><span class="sxs-lookup"><span data-stu-id="30982-167">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="30982-168">L'utente viene aggiunto al database di identità.</span><span class="sxs-lookup"><span data-stu-id="30982-168">The user is added to the Identity database.</span></span>
* <span data-ttu-id="30982-169">Viene inviata una notifica all'utente e viene chiesto di modificare la password.</span><span class="sxs-lookup"><span data-stu-id="30982-169">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="30982-170">Il codice seguente illustra l'aggiunta di un utente:</span><span class="sxs-lookup"><span data-stu-id="30982-170">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="30982-171">Per gli scenari di produzione è possibile seguire un approccio simile.</span><span class="sxs-lookup"><span data-stu-id="30982-171">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="30982-172">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="30982-172">Additional resources</span></span>

* [<span data-ttu-id="30982-173">Modifiche al codice di autenticazione in ASP.NET Core 2,1 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="30982-173">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="30982-174">ASP.NET Core 2,1 e versioni successive fornisce [ASP.NET Core identità](xref:security/authentication/identity) come [libreria di classi Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="30982-174">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="30982-175">Le applicazioni che includono l'identità possono applicare l'impalcatura per aggiungere selettivamente il codice sorgente contenuto nella libreria di classi Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="30982-175">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="30982-176">Se si vuole generare un codice sorgente, è possibile modificare il codice e modificarne il comportamento.</span><span class="sxs-lookup"><span data-stu-id="30982-176">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="30982-177">Ad esempio, è possibile indicare allo scaffolder di generare il codice usato nella registrazione.</span><span class="sxs-lookup"><span data-stu-id="30982-177">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="30982-178">Il codice generato ha la precedenza rispetto allo stesso codice nella libreria di classi Razor per Identity.</span><span class="sxs-lookup"><span data-stu-id="30982-178">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="30982-179">Per ottenere il controllo completo dell'interfaccia utente e non usare il RCL predefinito, vedere la sezione [creare l'origine dell'interfaccia utente Full Identity](#full).</span><span class="sxs-lookup"><span data-stu-id="30982-179">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="30982-180">Le applicazioni che **non** includono l'autenticazione possono applicare l'impalcatura per aggiungere il pacchetto di identità RCL.</span><span class="sxs-lookup"><span data-stu-id="30982-180">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="30982-181">È possibile selezionare il codice Identity da generare.</span><span class="sxs-lookup"><span data-stu-id="30982-181">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="30982-182">Sebbene l'impalcatura generi la maggior parte del codice necessario, sarà necessario aggiornare il progetto per completare il processo.</span><span class="sxs-lookup"><span data-stu-id="30982-182">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="30982-183">Questo documento illustra i passaggi necessari per completare un aggiornamento dell'impalcatura delle identità.</span><span class="sxs-lookup"><span data-stu-id="30982-183">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="30982-184">Quando si esegue l'impalcatura delle identità, viene creato un file *ScaffoldingReadme. txt* nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="30982-184">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="30982-185">Il file *ScaffoldingReadme. txt* contiene istruzioni generali sugli elementi necessari per completare l'aggiornamento dell'impalcatura delle identità.</span><span class="sxs-lookup"><span data-stu-id="30982-185">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="30982-186">Questo documento contiene istruzioni più complete rispetto al file *ScaffoldingReadme. txt* .</span><span class="sxs-lookup"><span data-stu-id="30982-186">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="30982-187">È consigliabile usare un sistema di controllo del codice sorgente che mostra le differenze dei file e consente di eseguire il backup delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="30982-187">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="30982-188">Controllare le modifiche dopo l'esecuzione dell'impalcatura di identità.</span><span class="sxs-lookup"><span data-stu-id="30982-188">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="30982-189">I servizi sono necessari quando si usa [l'autenticazione a due fattori](xref:security/authentication/identity-enable-qrcodes), la [conferma dell'account e il recupero della password](xref:security/authentication/accconfirm)e altre funzionalità di sicurezza con identità.</span><span class="sxs-lookup"><span data-stu-id="30982-189">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="30982-190">I servizi o gli stub di servizio non vengono generati durante l'identità di ponteggi.</span><span class="sxs-lookup"><span data-stu-id="30982-190">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="30982-191">I servizi per abilitare queste funzionalità devono essere aggiunti manualmente.</span><span class="sxs-lookup"><span data-stu-id="30982-191">Services to enable these features must be added manually.</span></span> <span data-ttu-id="30982-192">Vedere ad esempio [Richiedi conferma tramite posta elettronica](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="30982-192">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="30982-193">Identità del patibolo in un progetto vuoto</span><span class="sxs-lookup"><span data-stu-id="30982-193">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="30982-194">Aggiungere le seguenti chiamate evidenziate alla classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="30982-194">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="30982-195">Identità del patibolo in un progetto Razor senza autorizzazione esistente</span><span class="sxs-lookup"><span data-stu-id="30982-195">Scaffold identity into a Razor project without existing authorization</span></span>

<!--  Updated for 3.0
set projNam=RPnoAuth
set projType=webapp

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="30982-196">L'identità è configurata in *areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="30982-196">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="30982-197">Per ulteriori informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="30982-197">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="30982-198">Migrazioni, UseAuthentication e layout</span><span class="sxs-lookup"><span data-stu-id="30982-198">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="30982-199">Abilitare l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="30982-199">Enable authentication</span></span>

<span data-ttu-id="30982-200">Nel metodo `Configure` della classe `Startup`, chiamare [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dopo `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="30982-200">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="30982-201">Modifiche del layout</span><span class="sxs-lookup"><span data-stu-id="30982-201">Layout changes</span></span>

<span data-ttu-id="30982-202">Facoltativo: aggiungere l'account di accesso parziale (`_LoginPartial`) al file di layout:</span><span class="sxs-lookup"><span data-stu-id="30982-202">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="30982-203">Identità del patibolo in un progetto Razor con autorizzazione</span><span class="sxs-lookup"><span data-stu-id="30982-203">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="30982-204">Alcune opzioni di identità sono configurate in *areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="30982-204">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="30982-205">Per ulteriori informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="30982-205">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="30982-206">Identità del patibolo in un progetto MVC senza autorizzazione esistente</span><span class="sxs-lookup"><span data-stu-id="30982-206">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="30982-207">Facoltativo: aggiungere l'account di accesso parziale (`_LoginPartial`) al file *Views/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="30982-207">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="30982-208">Spostare il file *pages/Shared/_LoginPartial. cshtml* in *Views/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="30982-208">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="30982-209">L'identità è configurata in *areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="30982-209">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="30982-210">Per ulteriori informazioni, vedere IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="30982-210">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="30982-211">Chiamare [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dopo `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="30982-211">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="30982-212">Identità del patibolo in un progetto MVC con autorizzazione</span><span class="sxs-lookup"><span data-stu-id="30982-212">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="30982-213">Eliminare le *pagine o* la cartella condivisa e i file in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="30982-213">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="30982-214">Crea origine interfaccia utente con identità completa</span><span class="sxs-lookup"><span data-stu-id="30982-214">Create full identity UI source</span></span>

<span data-ttu-id="30982-215">Per mantenere il controllo completo dell'interfaccia utente di identità, eseguire l'impalcatura delle identità e selezionare **Sostituisci tutti i file**.</span><span class="sxs-lookup"><span data-stu-id="30982-215">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="30982-216">Il codice evidenziato seguente mostra le modifiche per sostituire l'interfaccia utente di identità predefinita con Identity in un'app Web ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="30982-216">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="30982-217">Questa operazione può essere eseguita per avere il controllo completo dell'interfaccia utente di identità.</span><span class="sxs-lookup"><span data-stu-id="30982-217">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="30982-218">L'identità predefinita viene sostituita dal codice seguente:</span><span class="sxs-lookup"><span data-stu-id="30982-218">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="30982-219">Il codice seguente imposta [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)e [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="30982-219">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="30982-220">Registrare un'implementazione di `IEmailSender`, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="30982-220">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="30982-221">Disabilita pagina Registro</span><span class="sxs-lookup"><span data-stu-id="30982-221">Disable register page</span></span>

<span data-ttu-id="30982-222">Per disabilitare la registrazione dell'utente:</span><span class="sxs-lookup"><span data-stu-id="30982-222">To disable user registration:</span></span>

* <span data-ttu-id="30982-223">Identità del patibolo.</span><span class="sxs-lookup"><span data-stu-id="30982-223">Scaffold Identity.</span></span> <span data-ttu-id="30982-224">Includere account. Register, account. login e account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="30982-224">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="30982-225">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="30982-225">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="30982-226">Aggiornare le *aree/Identity/Pages/account/Register. cshtml. cs,* in modo che gli utenti non possano effettuare la registrazione da questo endpoint:</span><span class="sxs-lookup"><span data-stu-id="30982-226">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="30982-227">Aggiornare le *aree/Identity/Pages/account/Register. cshtml* in modo che siano coerenti con le modifiche precedenti:</span><span class="sxs-lookup"><span data-stu-id="30982-227">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="30982-228">Impostare come commento o rimuovere il collegamento di registrazione da *aree/Identity/Pages/account/login. cshtml*</span><span class="sxs-lookup"><span data-stu-id="30982-228">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="30982-229">Aggiornare la pagina *aree/identità/pagine/account/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="30982-229">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="30982-230">Rimuovere il codice e i collegamenti dal file cshtml.</span><span class="sxs-lookup"><span data-stu-id="30982-230">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="30982-231">Rimuovere il codice di conferma dal `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="30982-231">Remove the confirmation code from the `PageModel`:</span></span>

  ```csharp
   [AllowAnonymous]
    public class RegisterConfirmationModel : PageModel
    {
        public IActionResult OnGet()
        {  
            return Page();
        }
    }
  ```
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="30982-232">Usare un'altra app per aggiungere utenti</span><span class="sxs-lookup"><span data-stu-id="30982-232">Use another app to add users</span></span>

<span data-ttu-id="30982-233">Fornire un meccanismo per aggiungere utenti all'esterno dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="30982-233">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="30982-234">Le opzioni per aggiungere utenti includono:</span><span class="sxs-lookup"><span data-stu-id="30982-234">Options to add users include:</span></span>

* <span data-ttu-id="30982-235">Un'app Web amministrativa dedicata.</span><span class="sxs-lookup"><span data-stu-id="30982-235">A dedicated admin web app.</span></span>
* <span data-ttu-id="30982-236">Un'app console.</span><span class="sxs-lookup"><span data-stu-id="30982-236">A console app.</span></span>

<span data-ttu-id="30982-237">Il codice seguente illustra un approccio per l'aggiunta di utenti:</span><span class="sxs-lookup"><span data-stu-id="30982-237">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="30982-238">Un elenco di utenti viene letto in memoria.</span><span class="sxs-lookup"><span data-stu-id="30982-238">A list of users is read into memory.</span></span>
* <span data-ttu-id="30982-239">Per ogni utente viene generata una password univoca complessa.</span><span class="sxs-lookup"><span data-stu-id="30982-239">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="30982-240">L'utente viene aggiunto al database di identità.</span><span class="sxs-lookup"><span data-stu-id="30982-240">The user is added to the Identity database.</span></span>
* <span data-ttu-id="30982-241">Viene inviata una notifica all'utente e viene chiesto di modificare la password.</span><span class="sxs-lookup"><span data-stu-id="30982-241">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="30982-242">Il codice seguente illustra l'aggiunta di un utente:</span><span class="sxs-lookup"><span data-stu-id="30982-242">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="30982-243">Per gli scenari di produzione è possibile seguire un approccio simile.</span><span class="sxs-lookup"><span data-stu-id="30982-243">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="30982-244">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="30982-244">Additional resources</span></span>

* [<span data-ttu-id="30982-245">Modifiche al codice di autenticazione in ASP.NET Core 2,1 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="30982-245">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end