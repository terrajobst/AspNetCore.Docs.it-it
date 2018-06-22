---
title: Identità Scaffold nei progetti ASP.NET Core
author: rick-anderson
description: Informazioni su come eseguire lo scaffolding di identità in un progetto ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: cf6544d8b671f026c8466fa8dff506027b64cf1f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276318"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="8e564-103">Identità Scaffold nei progetti ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e564-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="8e564-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8e564-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8e564-105">Fornisce ASP.NET Core 2.1 e versioni successive [ASP.NET Identity Core](xref:security/authentication/identity) come una [libreria di classi Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="8e564-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="8e564-106">Le applicazioni che includono l'identità possono applicare scaffolder per aggiungere in modo selettivo il codice sorgente incluso in Identity Razor classe libreria (RCL).</span><span class="sxs-lookup"><span data-stu-id="8e564-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="8e564-107">È possibile generare codice sorgente, pertanto è possibile modificare il codice e modificare il comportamento.</span><span class="sxs-lookup"><span data-stu-id="8e564-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="8e564-108">Ad esempio, si può indicare scaffolder per generare il codice usato nella registrazione.</span><span class="sxs-lookup"><span data-stu-id="8e564-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="8e564-109">Codice generato ha la precedenza sullo stesso codice in RCL l'identità.</span><span class="sxs-lookup"><span data-stu-id="8e564-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="8e564-110">Per ottenere il completo controllo dell'interfaccia utente e non usa il valore predefinito RCL, vedere la sezione [origine dell'interfaccia utente di creare identità completa](#full).</span><span class="sxs-lookup"><span data-stu-id="8e564-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="8e564-111">Le applicazioni che hanno **non** includono l'autenticazione è possibile applicare scaffolder per aggiungere il pacchetto RCL identità.</span><span class="sxs-lookup"><span data-stu-id="8e564-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="8e564-112">È la possibilità di selezionare il codice di identità da generare.</span><span class="sxs-lookup"><span data-stu-id="8e564-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="8e564-113">Sebbene il scaffolder genera la maggior parte del codice necessario, sarà necessario aggiornare il progetto per completare il processo.</span><span class="sxs-lookup"><span data-stu-id="8e564-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="8e564-114">Questo documento illustra i passaggi necessari per completare un aggiornamento che lo scaffolding di identità.</span><span class="sxs-lookup"><span data-stu-id="8e564-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="8e564-115">Quando viene eseguito il scaffolder Identity, una *ScaffoldingReadme.txt* file viene creato nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="8e564-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="8e564-116">Il *ScaffoldingReadme.txt* file contiene istruzioni generali sul cosa è necessario per completare l'aggiornamento che lo scaffolding di identità.</span><span class="sxs-lookup"><span data-stu-id="8e564-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="8e564-117">Questo documento contiene istruzioni più complete rispetto ai *ScaffoldingReadme.txt* file.</span><span class="sxs-lookup"><span data-stu-id="8e564-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="8e564-118">È consigliabile utilizzare un sistema di controllo di origine che mostra le differenze tra file e consente di annullare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="8e564-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="8e564-119">Controllare le modifiche apportate dopo l'esecuzione di scaffolder di identità.</span><span class="sxs-lookup"><span data-stu-id="8e564-119">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="8e564-120">Lo scaffolding di identità in un progetto vuoto</span><span class="sxs-lookup"><span data-stu-id="8e564-120">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="8e564-121">Aggiungere il seguente chiama evidenziato per il `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="8e564-121">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="8e564-122">Lo scaffolding di identità in un progetto Razor senza autorizzazione esistente</span><span class="sxs-lookup"><span data-stu-id="8e564-122">Scaffold identity into a Razor project without existing authorization</span></span>

<!--
set projNam=RPnoAuth
set projType=razor
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

<span data-ttu-id="8e564-123">Identità sia configurata nel *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8e564-123">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="8e564-124">Per altre informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="8e564-124">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="8e564-125">Le migrazioni, UseAuthentication e layout</span><span class="sxs-lookup"><span data-stu-id="8e564-125">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="8e564-126">Nel `Configure` metodo per il `Startup` classe, chiamare [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dopo `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="8e564-126">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="8e564-127">Modifiche di layout</span><span class="sxs-lookup"><span data-stu-id="8e564-127">Layout changes</span></span>

<span data-ttu-id="8e564-128">Facoltativo: Aggiungere l'account di accesso parziale (`_LoginPartial`) per il file di layout:</span><span class="sxs-lookup"><span data-stu-id="8e564-128">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="8e564-129">Lo scaffolding di identità in un progetto Razor con l'autorizzazione di</span><span class="sxs-lookup"><span data-stu-id="8e564-129">Scaffold identity into a Razor project with authorization</span></span>

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="8e564-130">Alcune opzioni di identità configurati nel *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8e564-130">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="8e564-131">Per altre informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="8e564-131">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="8e564-132">Lo scaffolding di identità in un progetto MVC senza autorizzazione esistente</span><span class="sxs-lookup"><span data-stu-id="8e564-132">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="8e564-133">Facoltativo: Aggiungere l'account di accesso parziale (`_LoginPartial`) per il *Views/Shared/_Layout.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="8e564-133">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="8e564-134">Spostare il *Pages/Shared/_LoginPartial.cshtml* file *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8e564-134">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="8e564-135">Identità sia configurata nel *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8e564-135">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="8e564-136">Per altre informazioni, vedere IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="8e564-136">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="8e564-137">Chiamare [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dopo `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="8e564-137">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="8e564-138">Lo scaffolding di identità in un progetto MVC con autorizzazione</span><span class="sxs-lookup"><span data-stu-id="8e564-138">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="8e564-139">Eliminare il *pagine/Shared* cartella e i file in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="8e564-139">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="8e564-140">Creare identità completa dell'interfaccia utente origine</span><span class="sxs-lookup"><span data-stu-id="8e564-140">Create full identity UI source</span></span>

<span data-ttu-id="8e564-141">Per mantenere il controllo completo dell'interfaccia utente identità, eseguire il scaffolder di identità e selezionare **eseguire l'Override di tutti i file**.</span><span class="sxs-lookup"><span data-stu-id="8e564-141">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="8e564-142">Il codice evidenziato di seguito mostra le modifiche per sostituire il valore predefinito dell'interfaccia utente di identità con identità in un'app web ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="8e564-142">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="8e564-143">È possibile effettuare questa operazione per avere il pieno controllo dell'interfaccia utente di identità.</span><span class="sxs-lookup"><span data-stu-id="8e564-143">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="8e564-144">Il valore predefinito di identità viene sostituito nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8e564-144">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="8e564-145">Nell'esempio il codice imposta il [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [proprietà logoutpath dal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), e [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="8e564-145">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="8e564-146">Registrare un `IEmailSender` implementazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="8e564-146">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
