---
title: Identità Scaffold nei progetti ASP.NET Core
author: rick-anderson
description: Informazioni su come eseguire lo scaffolding di identità in un progetto ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 80cd39af61e856d3ce92db1c26e70788bcdca83d
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725819"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Identità Scaffold nei progetti ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Fornisce ASP.NET Core 2.1 e versioni successive [ASP.NET Identity Core](xref:security/authentication/identity) come una [libreria di classi Razor](xref:mvc/razor-pages/ui-class). Le applicazioni che includono l'identità possono applicare scaffolder per aggiungere in modo selettivo il codice sorgente incluso in Identity Razor classe libreria (RCL). È possibile generare codice sorgente, pertanto è possibile modificare il codice e modificare il comportamento. Ad esempio, si può indicare scaffolder per generare il codice usato nella registrazione. Codice generato ha la precedenza sullo stesso codice in RCL l'identità. Per ottenere il completo controllo dell'interfaccia utente e non usa il valore predefinito RCL, vedere la sezione [origine dell'interfaccia utente di creare identità completa](#full).

Le applicazioni che hanno **non** includono l'autenticazione è possibile applicare scaffolder per aggiungere il pacchetto RCL identità. È la possibilità di selezionare il codice di identità da generare.

Sebbene il scaffolder genera la maggior parte del codice necessario, sarà necessario aggiornare il progetto per completare il processo. Questo documento illustra i passaggi necessari per completare un aggiornamento che lo scaffolding di identità.

Quando viene eseguito il scaffolder Identity, una *ScaffoldingReadme.txt* file viene creato nella directory del progetto. Il *ScaffoldingReadme.txt* file contiene istruzioni generali sul cosa è necessario per completare l'aggiornamento che lo scaffolding di identità. Questo documento contiene istruzioni più complete rispetto ai *ScaffoldingReadme.txt* file.

È consigliabile utilizzare un sistema di controllo di origine che mostra le differenze tra file e consente di annullare le modifiche. Controllare le modifiche apportate dopo l'esecuzione di scaffolder di identità.

## <a name="scaffold-identity-into-an-empty-project"></a>Lo scaffolding di identità in un progetto vuoto

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Aggiungere il seguente chiama evidenziato per il `Startup` classe:

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Lo scaffolding di identità in un progetto Razor senza autorizzazione esistente

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

Identità sia configurata nel *Areas/Identity/IdentityHostingStartup.cs*. Per altre informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>Le migrazioni, UseAuthentication e layout

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Nel `Configure` metodo per il `Startup` classe, chiamare [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dopo `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Modifiche di layout

Facoltativo: Aggiungere l'account di accesso parziale (`_LoginPartial`) per il file di layout:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Lo scaffolding di identità in un progetto Razor con l'autorizzazione di

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
Alcune opzioni di identità configurati nel *Areas/Identity/IdentityHostingStartup.cs*. Per altre informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Lo scaffolding di identità in un progetto MVC senza autorizzazione esistente

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

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Spostare il *Pages/Shared/_LoginPartial.cshtml* file *Views/Shared/_LoginPartial.cshtml*

Identità sia configurata nel *Areas/Identity/IdentityHostingStartup.cs*. Per altre informazioni, vedere IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Chiamare [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dopo `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Lo scaffolding di identità in un progetto MVC con autorizzazione

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Eliminare il *pagine/Shared* cartella e i file in tale cartella.

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>Creare identità completa dell'interfaccia utente origine

Per mantenere il controllo completo dell'interfaccia utente identità, eseguire il scaffolder di identità e selezionare **eseguire l'Override di tutti i file**.

Il codice evidenziato di seguito mostra le modifiche per sostituire il valore predefinito dell'interfaccia utente di identità con identità in un'app web ASP.NET Core 2.1. È possibile effettuare questa operazione per avere il pieno controllo dell'interfaccia utente di identità.

[!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

Il valore predefinito di identità viene sostituito nel codice seguente: [!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

Il codice seguente consente di configurare ASP.NET Core per autorizzare le pagine di identità che richiedono autorizzazione: [!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

Nell'esempio di codice imposta il cookie di identità da utilizzare il percorso di pagine di identità corretto.
[!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

Registrare un `IEmailSender` implementazione, ad esempio:

[!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet4)]