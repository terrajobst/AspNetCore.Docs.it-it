---
title: Identità di impalcatura nei progetti ASP.NET Core
author: rick-anderson
description: Informazioni su come eseguire l'impalcatura dell'identità in un progetto ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2020
uid: security/authentication/scaffold-identity
ms.openlocfilehash: b3e077aeac11e62d9e992884100476f7be35b59a
ms.sourcegitcommit: 990a4c2e623c202a27f60bdf3902f250359c13be
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/03/2020
ms.locfileid: "76972038"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="5422b-103">Identità di impalcatura nei progetti ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5422b-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="5422b-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5422b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5422b-105">ASP.NET Core fornisce [ASP.NET Core identità](xref:security/authentication/identity) come [libreria di classi Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="5422b-105">ASP.NET Core provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="5422b-106">Le applicazioni che includono l'identità possono applicare l'impalcatura per aggiungere selettivamente il codice sorgente contenuto nella libreria di classi Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="5422b-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="5422b-107">Se si vuole generare un codice sorgente, è possibile modificare il codice e modificarne il comportamento.</span><span class="sxs-lookup"><span data-stu-id="5422b-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="5422b-108">Ad esempio, è possibile indicare allo scaffolder di generare il codice usato nella registrazione.</span><span class="sxs-lookup"><span data-stu-id="5422b-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="5422b-109">Il codice generato ha la precedenza rispetto allo stesso codice nella libreria di classi Razor per Identity.</span><span class="sxs-lookup"><span data-stu-id="5422b-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="5422b-110">Per ottenere il controllo completo dell'interfaccia utente e non usare il RCL predefinito, vedere la sezione [creare l'origine dell'interfaccia utente Full Identity](#full).</span><span class="sxs-lookup"><span data-stu-id="5422b-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="5422b-111">Le applicazioni che **non** includono l'autenticazione possono applicare l'impalcatura per aggiungere il pacchetto di identità RCL.</span><span class="sxs-lookup"><span data-stu-id="5422b-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="5422b-112">È possibile selezionare il codice Identity da generare.</span><span class="sxs-lookup"><span data-stu-id="5422b-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="5422b-113">Sebbene l'impalcatura generi la maggior parte del codice necessario, è necessario aggiornare il progetto per completare il processo.</span><span class="sxs-lookup"><span data-stu-id="5422b-113">Although the scaffolder generates most of the necessary code, you need to update your project to complete the process.</span></span> <span data-ttu-id="5422b-114">Questo documento illustra i passaggi necessari per completare un aggiornamento dell'impalcatura delle identità.</span><span class="sxs-lookup"><span data-stu-id="5422b-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="5422b-115">È consigliabile usare un sistema di controllo del codice sorgente che mostra le differenze dei file e consente di eseguire il backup delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="5422b-115">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="5422b-116">Controllare le modifiche dopo l'esecuzione dell'impalcatura di identità.</span><span class="sxs-lookup"><span data-stu-id="5422b-116">Inspect the changes after running the Identity scaffolder.</span></span>

<span data-ttu-id="5422b-117">I servizi sono necessari quando si usa [l'autenticazione a due fattori](xref:security/authentication/identity-enable-qrcodes), la [conferma dell'account e il recupero della password](xref:security/authentication/accconfirm)e altre funzionalità di sicurezza con identità.</span><span class="sxs-lookup"><span data-stu-id="5422b-117">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="5422b-118">I servizi o gli stub di servizio non vengono generati durante l'identità di ponteggi.</span><span class="sxs-lookup"><span data-stu-id="5422b-118">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="5422b-119">I servizi per abilitare queste funzionalità devono essere aggiunti manualmente.</span><span class="sxs-lookup"><span data-stu-id="5422b-119">Services to enable these features must be added manually.</span></span> <span data-ttu-id="5422b-120">Vedere ad esempio [Richiedi conferma tramite posta elettronica](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="5422b-120">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

<span data-ttu-id="5422b-121">Questo documento contiene istruzioni più complete rispetto al file *ScaffoldingReadme. txt* che viene generato quando si esegue l'impalcatura.</span><span class="sxs-lookup"><span data-stu-id="5422b-121">This document contains more complete instructions than the *ScaffoldingReadme.txt* file which is generated when running the scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="5422b-122">Identità del patibolo in un progetto vuoto</span><span class="sxs-lookup"><span data-stu-id="5422b-122">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="5422b-123">Aggiornare la classe `Startup` con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="5422b-123">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="5422b-124">Identità del patibolo in un progetto Razor senza autorizzazione esistente</span><span class="sxs-lookup"><span data-stu-id="5422b-124">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="5422b-125">L'identità è configurata in *areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="5422b-125">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="5422b-126">Per ulteriori informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="5422b-126">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="5422b-127">Migrazioni, UseAuthentication e layout</span><span class="sxs-lookup"><span data-stu-id="5422b-127">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="5422b-128">Abilita autenticazione</span><span class="sxs-lookup"><span data-stu-id="5422b-128">Enable authentication</span></span>

<span data-ttu-id="5422b-129">Aggiornare la classe `Startup` con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="5422b-129">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupRP.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="5422b-130">Modifiche del layout</span><span class="sxs-lookup"><span data-stu-id="5422b-130">Layout changes</span></span>

<span data-ttu-id="5422b-131">Facoltativo: aggiungere l'account di accesso parziale (`_LoginPartial`) al file di layout:</span><span class="sxs-lookup"><span data-stu-id="5422b-131">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="5422b-132">Identità del patibolo in un progetto Razor con autorizzazione</span><span class="sxs-lookup"><span data-stu-id="5422b-132">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="5422b-133">Alcune opzioni di identità sono configurate in *areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="5422b-133">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="5422b-134">Per ulteriori informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="5422b-134">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="5422b-135">Identità del patibolo in un progetto MVC senza autorizzazione esistente</span><span class="sxs-lookup"><span data-stu-id="5422b-135">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="5422b-136">Facoltativo: aggiungere l'account di accesso parziale (`_LoginPartial`) al file *Views/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="5422b-136">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/3.1sample/_Layout.cshtml?highlight=20)]

* <span data-ttu-id="5422b-137">Spostare il file *pages/Shared/_LoginPartial. cshtml* in *Views/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5422b-137">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="5422b-138">L'identità è configurata in *areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="5422b-138">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="5422b-139">Per ulteriori informazioni, vedere IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="5422b-139">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="5422b-140">Aggiornare la classe `Startup` con codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="5422b-140">Update the `Startup` class with code similar to the following:</span></span>

[!code-csharp[](scaffold-identity/3.1sample/StartupMVC.cs?name=snippet)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="5422b-141">Identità del patibolo in un progetto MVC con autorizzazione</span><span class="sxs-lookup"><span data-stu-id="5422b-141">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="5422b-142">Crea origine interfaccia utente con identità completa</span><span class="sxs-lookup"><span data-stu-id="5422b-142">Create full identity UI source</span></span>

<span data-ttu-id="5422b-143">Per mantenere il controllo completo dell'interfaccia utente di identità, eseguire l'impalcatura delle identità e selezionare **Sostituisci tutti i file**.</span><span class="sxs-lookup"><span data-stu-id="5422b-143">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="5422b-144">Il codice evidenziato seguente mostra le modifiche per sostituire l'interfaccia utente di identità predefinita con Identity in un'app Web ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="5422b-144">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="5422b-145">Questa operazione può essere eseguita per avere il controllo completo dell'interfaccia utente di identità.</span><span class="sxs-lookup"><span data-stu-id="5422b-145">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="5422b-146">L'identità predefinita viene sostituita dal codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5422b-146">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="5422b-147">Il codice seguente imposta [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)e [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="5422b-147">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="5422b-148">Registrare un'implementazione di `IEmailSender`, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="5422b-148">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="5422b-149">Disabilita pagina Registro</span><span class="sxs-lookup"><span data-stu-id="5422b-149">Disable register page</span></span>

<span data-ttu-id="5422b-150">Per disabilitare la registrazione dell'utente:</span><span class="sxs-lookup"><span data-stu-id="5422b-150">To disable user registration:</span></span>

* <span data-ttu-id="5422b-151">Identità del patibolo.</span><span class="sxs-lookup"><span data-stu-id="5422b-151">Scaffold Identity.</span></span> <span data-ttu-id="5422b-152">Includere account. Register, account. login e account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="5422b-152">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="5422b-153">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="5422b-153">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="5422b-154">Aggiornare le *aree/Identity/Pages/account/Register. cshtml. cs,* in modo che gli utenti non possano effettuare la registrazione da questo endpoint:</span><span class="sxs-lookup"><span data-stu-id="5422b-154">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="5422b-155">Aggiornare le *aree/Identity/Pages/account/Register. cshtml* in modo che siano coerenti con le modifiche precedenti:</span><span class="sxs-lookup"><span data-stu-id="5422b-155">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="5422b-156">Impostare come commento o rimuovere il collegamento di registrazione da *aree/Identity/Pages/account/login. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5422b-156">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="5422b-157">Aggiornare la pagina *aree/identità/pagine/account/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="5422b-157">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="5422b-158">Rimuovere il codice e i collegamenti dal file cshtml.</span><span class="sxs-lookup"><span data-stu-id="5422b-158">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="5422b-159">Rimuovere il codice di conferma dal `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="5422b-159">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="5422b-160">Usare un'altra app per aggiungere utenti</span><span class="sxs-lookup"><span data-stu-id="5422b-160">Use another app to add users</span></span>

<span data-ttu-id="5422b-161">Fornire un meccanismo per aggiungere utenti all'esterno dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="5422b-161">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="5422b-162">Le opzioni per aggiungere utenti includono:</span><span class="sxs-lookup"><span data-stu-id="5422b-162">Options to add users include:</span></span>

* <span data-ttu-id="5422b-163">Un'app Web amministrativa dedicata.</span><span class="sxs-lookup"><span data-stu-id="5422b-163">A dedicated admin web app.</span></span>
* <span data-ttu-id="5422b-164">Un'app console.</span><span class="sxs-lookup"><span data-stu-id="5422b-164">A console app.</span></span>

<span data-ttu-id="5422b-165">Il codice seguente illustra un approccio per l'aggiunta di utenti:</span><span class="sxs-lookup"><span data-stu-id="5422b-165">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="5422b-166">Un elenco di utenti viene letto in memoria.</span><span class="sxs-lookup"><span data-stu-id="5422b-166">A list of users is read into memory.</span></span>
* <span data-ttu-id="5422b-167">Per ogni utente viene generata una password univoca complessa.</span><span class="sxs-lookup"><span data-stu-id="5422b-167">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="5422b-168">L'utente viene aggiunto al database di identità.</span><span class="sxs-lookup"><span data-stu-id="5422b-168">The user is added to the Identity database.</span></span>
* <span data-ttu-id="5422b-169">Viene inviata una notifica all'utente e viene chiesto di modificare la password.</span><span class="sxs-lookup"><span data-stu-id="5422b-169">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="5422b-170">Il codice seguente illustra l'aggiunta di un utente:</span><span class="sxs-lookup"><span data-stu-id="5422b-170">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="5422b-171">Per gli scenari di produzione è possibile seguire un approccio simile.</span><span class="sxs-lookup"><span data-stu-id="5422b-171">A similar approach can be followed for production scenarios.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="5422b-172">Impedisci la pubblicazione di asset di identità statici</span><span class="sxs-lookup"><span data-stu-id="5422b-172">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="5422b-173">Per evitare la pubblicazione di asset di identità statici nella radice Web, vedere <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span><span class="sxs-lookup"><span data-stu-id="5422b-173">To prevent publishing static Identity assets to the web root, see <xref:security/authentication/identity#prevent-publish-of-static-identity-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5422b-174">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5422b-174">Additional resources</span></span>

* [<span data-ttu-id="5422b-175">Modifiche al codice di autenticazione in ASP.NET Core 2,1 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="5422b-175">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5422b-176">ASP.NET Core 2,1 e versioni successive fornisce [ASP.NET Core identità](xref:security/authentication/identity) come [libreria di classi Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="5422b-176">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="5422b-177">Le applicazioni che includono l'identità possono applicare l'impalcatura per aggiungere selettivamente il codice sorgente contenuto nella libreria di classi Razor (RCL).</span><span class="sxs-lookup"><span data-stu-id="5422b-177">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="5422b-178">Se si vuole generare un codice sorgente, è possibile modificare il codice e modificarne il comportamento.</span><span class="sxs-lookup"><span data-stu-id="5422b-178">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="5422b-179">Ad esempio, è possibile indicare allo scaffolder di generare il codice usato nella registrazione.</span><span class="sxs-lookup"><span data-stu-id="5422b-179">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="5422b-180">Il codice generato ha la precedenza rispetto allo stesso codice nella libreria di classi Razor per Identity.</span><span class="sxs-lookup"><span data-stu-id="5422b-180">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="5422b-181">Per ottenere il controllo completo dell'interfaccia utente e non usare il RCL predefinito, vedere la sezione [creare l'origine dell'interfaccia utente Full Identity](#full).</span><span class="sxs-lookup"><span data-stu-id="5422b-181">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="5422b-182">Le applicazioni che **non** includono l'autenticazione possono applicare l'impalcatura per aggiungere il pacchetto di identità RCL.</span><span class="sxs-lookup"><span data-stu-id="5422b-182">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="5422b-183">È possibile selezionare il codice Identity da generare.</span><span class="sxs-lookup"><span data-stu-id="5422b-183">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="5422b-184">Sebbene l'impalcatura generi la maggior parte del codice necessario, sarà necessario aggiornare il progetto per completare il processo.</span><span class="sxs-lookup"><span data-stu-id="5422b-184">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="5422b-185">Questo documento illustra i passaggi necessari per completare un aggiornamento dell'impalcatura delle identità.</span><span class="sxs-lookup"><span data-stu-id="5422b-185">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="5422b-186">Quando si esegue l'impalcatura delle identità, viene creato un file *ScaffoldingReadme. txt* nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="5422b-186">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="5422b-187">Il file *ScaffoldingReadme. txt* contiene istruzioni generali sugli elementi necessari per completare l'aggiornamento dell'impalcatura delle identità.</span><span class="sxs-lookup"><span data-stu-id="5422b-187">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="5422b-188">Questo documento contiene istruzioni più complete rispetto al file *ScaffoldingReadme. txt* .</span><span class="sxs-lookup"><span data-stu-id="5422b-188">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="5422b-189">È consigliabile usare un sistema di controllo del codice sorgente che mostra le differenze dei file e consente di eseguire il backup delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="5422b-189">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="5422b-190">Controllare le modifiche dopo l'esecuzione dell'impalcatura di identità.</span><span class="sxs-lookup"><span data-stu-id="5422b-190">Inspect the changes after running the Identity scaffolder.</span></span>

> [!NOTE]
> <span data-ttu-id="5422b-191">I servizi sono necessari quando si usa [l'autenticazione a due fattori](xref:security/authentication/identity-enable-qrcodes), la [conferma dell'account e il recupero della password](xref:security/authentication/accconfirm)e altre funzionalità di sicurezza con identità.</span><span class="sxs-lookup"><span data-stu-id="5422b-191">Services are required when using [Two Factor Authentication](xref:security/authentication/identity-enable-qrcodes), [Account confirmation and password recovery](xref:security/authentication/accconfirm), and other security features with Identity.</span></span> <span data-ttu-id="5422b-192">I servizi o gli stub di servizio non vengono generati durante l'identità di ponteggi.</span><span class="sxs-lookup"><span data-stu-id="5422b-192">Services or service stubs aren't generated when scaffolding Identity.</span></span> <span data-ttu-id="5422b-193">I servizi per abilitare queste funzionalità devono essere aggiunti manualmente.</span><span class="sxs-lookup"><span data-stu-id="5422b-193">Services to enable these features must be added manually.</span></span> <span data-ttu-id="5422b-194">Vedere ad esempio [Richiedi conferma tramite posta elettronica](xref:security/authentication/accconfirm#require-email-confirmation).</span><span class="sxs-lookup"><span data-stu-id="5422b-194">For example, see [Require Email Confirmation](xref:security/authentication/accconfirm#require-email-confirmation).</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="5422b-195">Identità del patibolo in un progetto vuoto</span><span class="sxs-lookup"><span data-stu-id="5422b-195">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="5422b-196">Aggiungere le seguenti chiamate evidenziate alla classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="5422b-196">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="5422b-197">Identità del patibolo in un progetto Razor senza autorizzazione esistente</span><span class="sxs-lookup"><span data-stu-id="5422b-197">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="5422b-198">L'identità è configurata in *areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="5422b-198">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="5422b-199">Per ulteriori informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="5422b-199">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="5422b-200">Migrazioni, UseAuthentication e layout</span><span class="sxs-lookup"><span data-stu-id="5422b-200">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a><span data-ttu-id="5422b-201">Abilita autenticazione</span><span class="sxs-lookup"><span data-stu-id="5422b-201">Enable authentication</span></span>

<span data-ttu-id="5422b-202">Nel metodo `Configure` della classe `Startup`, chiamare [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dopo `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="5422b-202">In the `Configure` method of the `Startup` class, call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="5422b-203">Modifiche del layout</span><span class="sxs-lookup"><span data-stu-id="5422b-203">Layout changes</span></span>

<span data-ttu-id="5422b-204">Facoltativo: aggiungere l'account di accesso parziale (`_LoginPartial`) al file di layout:</span><span class="sxs-lookup"><span data-stu-id="5422b-204">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="5422b-205">Identità del patibolo in un progetto Razor con autorizzazione</span><span class="sxs-lookup"><span data-stu-id="5422b-205">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="5422b-206">Alcune opzioni di identità sono configurate in *areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="5422b-206">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="5422b-207">Per ulteriori informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="5422b-207">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="5422b-208">Identità del patibolo in un progetto MVC senza autorizzazione esistente</span><span class="sxs-lookup"><span data-stu-id="5422b-208">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="5422b-209">Facoltativo: aggiungere l'account di accesso parziale (`_LoginPartial`) al file *Views/Shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="5422b-209">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="5422b-210">Spostare il file *pages/Shared/_LoginPartial. cshtml* in *Views/Shared/_LoginPartial. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5422b-210">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="5422b-211">L'identità è configurata in *areas/Identity/IdentityHostingStartup. cs*.</span><span class="sxs-lookup"><span data-stu-id="5422b-211">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="5422b-212">Per ulteriori informazioni, vedere IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="5422b-212">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="5422b-213">Chiamare [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dopo `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="5422b-213">Call [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="5422b-214">Identità del patibolo in un progetto MVC con autorizzazione</span><span class="sxs-lookup"><span data-stu-id="5422b-214">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext  --files "Account.Login;Account.Register"
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="5422b-215">Eliminare le *pagine o* la cartella condivisa e i file in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="5422b-215">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="5422b-216">Crea origine interfaccia utente con identità completa</span><span class="sxs-lookup"><span data-stu-id="5422b-216">Create full identity UI source</span></span>

<span data-ttu-id="5422b-217">Per mantenere il controllo completo dell'interfaccia utente di identità, eseguire l'impalcatura delle identità e selezionare **Sostituisci tutti i file**.</span><span class="sxs-lookup"><span data-stu-id="5422b-217">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="5422b-218">Il codice evidenziato seguente mostra le modifiche per sostituire l'interfaccia utente di identità predefinita con Identity in un'app Web ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="5422b-218">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="5422b-219">Questa operazione può essere eseguita per avere il controllo completo dell'interfaccia utente di identità.</span><span class="sxs-lookup"><span data-stu-id="5422b-219">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="5422b-220">L'identità predefinita viene sostituita dal codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5422b-220">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="5422b-221">Il codice seguente imposta [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)e [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="5422b-221">The following code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="5422b-222">Registrare un'implementazione di `IEmailSender`, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="5422b-222">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

<!--
uld option: Use Local DB, not SQLite

dotnet new webapp -au Individual -uld -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
-->
## <a name="disable-register-page"></a><span data-ttu-id="5422b-223">Disabilita pagina Registro</span><span class="sxs-lookup"><span data-stu-id="5422b-223">Disable register page</span></span>

<span data-ttu-id="5422b-224">Per disabilitare la registrazione dell'utente:</span><span class="sxs-lookup"><span data-stu-id="5422b-224">To disable user registration:</span></span>

* <span data-ttu-id="5422b-225">Identità del patibolo.</span><span class="sxs-lookup"><span data-stu-id="5422b-225">Scaffold Identity.</span></span> <span data-ttu-id="5422b-226">Includere account. Register, account. login e account. RegisterConfirmation.</span><span class="sxs-lookup"><span data-stu-id="5422b-226">Include Account.Register, Account.Login, and Account.RegisterConfirmation.</span></span> <span data-ttu-id="5422b-227">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="5422b-227">For example:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.RegisterConfirmation"
  ```

* <span data-ttu-id="5422b-228">Aggiornare le *aree/Identity/Pages/account/Register. cshtml. cs,* in modo che gli utenti non possano effettuare la registrazione da questo endpoint:</span><span class="sxs-lookup"><span data-stu-id="5422b-228">Update *Areas/Identity/Pages/Account/Register.cshtml.cs* so users can't register from this endpoint:</span></span>

  [!code-csharp[](scaffold-identity/sample/Register.cshtml.cs?name=snippet)]

* <span data-ttu-id="5422b-229">Aggiornare le *aree/Identity/Pages/account/Register. cshtml* in modo che siano coerenti con le modifiche precedenti:</span><span class="sxs-lookup"><span data-stu-id="5422b-229">Update *Areas/Identity/Pages/Account/Register.cshtml* to be consistent with the preceding changes:</span></span>

  [!code-cshtml[](scaffold-identity/sample/Register.cshtml)]

* <span data-ttu-id="5422b-230">Impostare come commento o rimuovere il collegamento di registrazione da *aree/Identity/Pages/account/login. cshtml*</span><span class="sxs-lookup"><span data-stu-id="5422b-230">Comment out or remove the registration link from *Areas/Identity/Pages/Account/Login.cshtml*</span></span>

```cshtml
@*
<p>
    <a asp-page="./Register" asp-route-returnUrl="@Model.ReturnUrl">Register as a new user</a>
</p>
*@
```

* <span data-ttu-id="5422b-231">Aggiornare la pagina *aree/identità/pagine/account/RegisterConfirmation* .</span><span class="sxs-lookup"><span data-stu-id="5422b-231">Update the *Areas/Identity/Pages/Account/RegisterConfirmation* page.</span></span>

  * <span data-ttu-id="5422b-232">Rimuovere il codice e i collegamenti dal file cshtml.</span><span class="sxs-lookup"><span data-stu-id="5422b-232">Remove the code and links from the cshtml file.</span></span>
  * <span data-ttu-id="5422b-233">Rimuovere il codice di conferma dal `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="5422b-233">Remove the confirmation code from the `PageModel`:</span></span>

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
  
### <a name="use-another-app-to-add-users"></a><span data-ttu-id="5422b-234">Usare un'altra app per aggiungere utenti</span><span class="sxs-lookup"><span data-stu-id="5422b-234">Use another app to add users</span></span>

<span data-ttu-id="5422b-235">Fornire un meccanismo per aggiungere utenti all'esterno dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="5422b-235">Provide a mechanism to add users outside the web app.</span></span> <span data-ttu-id="5422b-236">Le opzioni per aggiungere utenti includono:</span><span class="sxs-lookup"><span data-stu-id="5422b-236">Options to add users include:</span></span>

* <span data-ttu-id="5422b-237">Un'app Web amministrativa dedicata.</span><span class="sxs-lookup"><span data-stu-id="5422b-237">A dedicated admin web app.</span></span>
* <span data-ttu-id="5422b-238">Un'app console.</span><span class="sxs-lookup"><span data-stu-id="5422b-238">A console app.</span></span>

<span data-ttu-id="5422b-239">Il codice seguente illustra un approccio per l'aggiunta di utenti:</span><span class="sxs-lookup"><span data-stu-id="5422b-239">The following code outlines one approach to adding users:</span></span>

* <span data-ttu-id="5422b-240">Un elenco di utenti viene letto in memoria.</span><span class="sxs-lookup"><span data-stu-id="5422b-240">A list of users is read into memory.</span></span>
* <span data-ttu-id="5422b-241">Per ogni utente viene generata una password univoca complessa.</span><span class="sxs-lookup"><span data-stu-id="5422b-241">A strong unique password is generated for each user.</span></span>
* <span data-ttu-id="5422b-242">L'utente viene aggiunto al database di identità.</span><span class="sxs-lookup"><span data-stu-id="5422b-242">The user is added to the Identity database.</span></span>
* <span data-ttu-id="5422b-243">Viene inviata una notifica all'utente e viene chiesto di modificare la password.</span><span class="sxs-lookup"><span data-stu-id="5422b-243">The user is notified and told to change the password.</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Program.cs?name=snippet)]

<span data-ttu-id="5422b-244">Il codice seguente illustra l'aggiunta di un utente:</span><span class="sxs-lookup"><span data-stu-id="5422b-244">The following code outlines adding a user:</span></span>

[!code-csharp[](scaffold-identity/consoleAddUser/Data/SeedData.cs?name=snippet)]

<span data-ttu-id="5422b-245">Per gli scenari di produzione è possibile seguire un approccio simile.</span><span class="sxs-lookup"><span data-stu-id="5422b-245">A similar approach can be followed for production scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5422b-246">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5422b-246">Additional resources</span></span>

* [<span data-ttu-id="5422b-247">Modifiche al codice di autenticazione in ASP.NET Core 2,1 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="5422b-247">Changes to authentication code to ASP.NET Core 2.1 and later</span></span>](xref:migration/20_21#changes-to-authentication-code)

::: moniker-end