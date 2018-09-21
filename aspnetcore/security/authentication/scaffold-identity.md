---
title: Identità di scaffolding in progetti ASP.NET Core
author: rick-anderson
description: Informazioni su come eseguire lo scaffolding di identità in un progetto ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 37ad9897fbc5eb1822ed2413334b4fce9050296b
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523038"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Identità di scaffolding in progetti ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 2.1 e versioni successive offre [ASP.NET Core Identity](xref:security/authentication/identity) come una [libreria di classi Razor](xref:razor-pages/ui-class). Applicazioni che includono l'identità è possono applicare l'utilità di scaffolding per aggiungere in modo selettivo il codice sorgente contenuto in libreria di classe di Razor l'identità (RCL). Se si vuole generare un codice sorgente, è possibile modificare il codice e modificarne il comportamento. Ad esempio, è possibile indicare allo scaffolder di generare il codice usato nella registrazione. Il codice generato ha la precedenza rispetto allo stesso codice nella libreria di classi Razor per Identity. Per esercitare il pieno controllo dell'interfaccia utente e non usare il valore predefinito RCL, vedere la sezione [origine dell'interfaccia utente di creare identità completa](#full).

Le applicazioni che hanno **non** includono l'autenticazione è possibile applicare l'utilità di scaffolding per aggiungere il pacchetto RCL identità. È possibile selezionare il codice Identity da generare.

Anche se l'utilità di scaffolding genera la maggior parte del codice necessario, è possibile aggiornare il progetto per completare il processo. Questo documento illustra i passaggi necessari per completare un aggiornamento di scaffolding di identità.

Quando si esegue l'utilità di scaffolding di identità, un *Scaffoldingreadme* file viene creato nella directory del progetto. Il *Scaffoldingreadme* file contiene istruzioni generali sul cosa è necessario per completare l'aggiornamento di scaffolding di identità. Questo documento contiene istruzioni più complete rispetto al *Scaffoldingreadme* file.

È consigliabile usare un sistema di controllo di origine che mostra le differenze tra file e consente di eseguire l'annullamento delle modifiche. Esaminare le modifiche dopo aver eseguito l'utilità di scaffolding di identità.

> [!NOTE]
> Servizi sono necessari quando si usa [autenticazione a due fattori](xref:security/authentication/identity-enable-qrcodes), [ripristino conferma e la password dell'Account](xref:security/authentication/accconfirm)e altre funzionalità di sicurezza con identità. I servizi o stub di servizio non viene generato quando lo scaffolding di identità. Servizi per abilitare queste funzionalità devono essere aggiunti manualmente. Ad esempio, vedere [richiedere conferma tramite posta elettronica](xref:security/authentication/accconfirm#require-email-confirmation).

## <a name="scaffold-identity-into-an-empty-project"></a>Identità di scaffolding in un progetto vuoto

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Aggiungere le chiamate seguenti evidenziate per il `Startup` classe:

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Identità di scaffolding in un progetto Razor senza autorizzazione esistenti

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

Identità è configurata nel *Areas/Identity/IdentityHostingStartup.cs*. per altre informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>Le migrazioni, UseAuthentication e layout

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a>Abilitare l'autenticazione

Nel `Configure` metodo per il `Startup` classe, chiamare [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dopo `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Modifiche del layout

Facoltativo: Aggiungere l'account di accesso parziale (`_LoginPartial`) per il file di layout:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Identità di scaffolding in un progetto Razor con autorizzazione

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth

dotnet new webapp -au Individual -o RPauth

dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Alcune opzioni di identità configurati nella *Areas/Identity/IdentityHostingStartup.cs*. per altre informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Identità di scaffolding in un progetto MVC senza autorizzazione esistenti

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

Facoltativo: Aggiungere l'account di accesso parziale (`_LoginPartial`) per il *Views/Shared/_Layout.cshtml* file:

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Spostare il *Pages/Shared/_LoginPartial.cshtml* file *Views/Shared/_LoginPartial.cshtml*

Identità è configurata nel *Areas/Identity/IdentityHostingStartup.cs*. Per altre informazioni, vedere IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Chiamare [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dopo `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Identità di scaffolding in un progetto MVC con autorizzazione

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Eliminare il *Pages/Shared* cartella e i file in tale cartella.

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>Creare identità completa dell'interfaccia utente origine

Per mantenere il controllo completo dell'interfaccia utente identità, eseguire l'utilità di scaffolding di identità e selezionare **eseguire l'Override di tutti i file**.

Il codice evidenziato seguente illustra le modifiche apportate a sostituire il valore predefinito di identità dell'interfaccia utente con identità in un'app web ASP.NET Core 2.1. Si potrebbe voler eseguire questa operazione per disporre del controllo completo dell'interfaccia utente di identità.

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

Il valore predefinito di identità viene sostituito nel codice seguente:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

Nell'esempio il codice imposta la [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [proprietà logoutpath dal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), e [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

Registrare un `IEmailSender` implementazione, ad esempio:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Modifiche al codice di autenticazione per ASP.NET Core 2.1 e versioni successive](xref:migration/20_21#changes-to-authentication-code)