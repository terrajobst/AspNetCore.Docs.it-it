---
title: Proteggere un'app ASP.NET Core Blazor webassembly autonomamente con Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: b4d32e91b4013cbea37baecb972a535d2874d3d1
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434460"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a><span data-ttu-id="8b043-102">Proteggere un'app ASP.NET Core Blazor webassembly autonomamente con Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="8b043-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory B2C</span></span>

<span data-ttu-id="8b043-103">Di [Javier Calvarro Nelson](https://github.com/javiercn) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8b043-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="8b043-104">Per creare un'app Blazor webassembly autonoma che usa [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="8b043-104">To create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication:</span></span>

1. <span data-ttu-id="8b043-105">Per creare un tenant e registrare un'app Web nel portale di Azure, seguire le istruzioni riportate negli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="8b043-105">Follow the guidance in the following topics to create a tenant and register a web app in the Azure Portal:</span></span>

   * <span data-ttu-id="8b043-106">[Creare un tenant AAD B2C](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; registrare le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="8b043-106">[Create an AAD B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; Record the following information:</span></span>

     <span data-ttu-id="8b043-107">1 \.</span><span class="sxs-lookup"><span data-stu-id="8b043-107">1\.</span></span> <span data-ttu-id="8b043-108">AAD B2C istanza (ad esempio, `https://contoso.b2clogin.com/`, che include la barra finale)</span><span class="sxs-lookup"><span data-stu-id="8b043-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span><br>
     <span data-ttu-id="8b043-109">2 \.</span><span class="sxs-lookup"><span data-stu-id="8b043-109">2\.</span></span> <span data-ttu-id="8b043-110">AAD B2C dominio tenant, ad esempio `contoso.onmicrosoft.com`</span><span class="sxs-lookup"><span data-stu-id="8b043-110">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

   * <span data-ttu-id="8b043-111">[Registrare un'applicazione web](/azure/active-directory-b2c/tutorial-register-applications) &ndash; effettuare le selezioni seguenti durante la registrazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="8b043-111">[Register a web application](/azure/active-directory-b2c/tutorial-register-applications) &ndash; Make the following selections during app registration:</span></span>

     <span data-ttu-id="8b043-112">1 \.</span><span class="sxs-lookup"><span data-stu-id="8b043-112">1\.</span></span> <span data-ttu-id="8b043-113">Impostare **app Web/API Web** su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="8b043-113">Set **Web App / Web API** to **Yes**.</span></span><br>
     <span data-ttu-id="8b043-114">2 \.</span><span class="sxs-lookup"><span data-stu-id="8b043-114">2\.</span></span> <span data-ttu-id="8b043-115">Impostare **Consenti flusso implicito** su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="8b043-115">Set **Allow implicit flow** to **Yes**.</span></span><br>
     <span data-ttu-id="8b043-116">3 \.</span><span class="sxs-lookup"><span data-stu-id="8b043-116">3\.</span></span> <span data-ttu-id="8b043-117">Aggiungere un **URL di risposta** di `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="8b043-117">Add a **Reply URL** of `https://localhost:5001/authentication/login-callback`.</span></span>

     <span data-ttu-id="8b043-118">Registrare l'ID applicazione (ID client), ad esempio `11111111-1111-1111-1111-111111111111`.</span><span class="sxs-lookup"><span data-stu-id="8b043-118">Record the Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`).</span></span>

   * <span data-ttu-id="8b043-119">[Creare flussi utente](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; creare un flusso utente di iscrizione e accesso.</span><span class="sxs-lookup"><span data-stu-id="8b043-119">[Create user flows](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash; Create a sign-up and sign-in user flow.</span></span>

     <span data-ttu-id="8b043-120">Selezionare come minimo l'attributo **Application claims** > **Display Name** user per popolare il `context.User.Identity.Name` nel componente `LoginDisplay` (*Shared/LoginDisplay. Razor*).</span><span class="sxs-lookup"><span data-stu-id="8b043-120">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

     <span data-ttu-id="8b043-121">Registrare il nome del flusso utente di iscrizione e accesso creato per l'app, ad esempio `B2C_1_signupsignin`.</span><span class="sxs-lookup"><span data-stu-id="8b043-121">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

1. <span data-ttu-id="8b043-122">Sostituire i segnaposto nel comando seguente con le informazioni registrate in precedenza ed eseguire il comando in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="8b043-122">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   <span data-ttu-id="8b043-123">Per specificare il percorso di output, che crea una cartella di progetto, se non esiste, includere l'opzione di output nel comando con un percorso, ad esempio `-o BlazorSample`.</span><span class="sxs-lookup"><span data-stu-id="8b043-123">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="8b043-124">Il nome della cartella diventa anche parte del nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="8b043-124">The folder name also becomes part of the project's name.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="8b043-125">Pacchetto di autenticazione</span><span class="sxs-lookup"><span data-stu-id="8b043-125">Authentication package</span></span>

<span data-ttu-id="8b043-126">Quando viene creata un'app per usare un singolo account B2C (`IndividualB2C`), l'app riceve automaticamente un riferimento al pacchetto per [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="8b043-126">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="8b043-127">Il pacchetto fornisce un set di primitive che consentono all'app di autenticare gli utenti e ottenere i token per chiamare le API protette.</span><span class="sxs-lookup"><span data-stu-id="8b043-127">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="8b043-128">Se si aggiunge l'autenticazione a un'app, aggiungere manualmente il pacchetto al file di progetto dell'app:</span><span class="sxs-lookup"><span data-stu-id="8b043-128">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="8b043-129">Sostituire `{VERSION}` nel riferimento al pacchetto precedente con la versione del pacchetto `Microsoft.AspNetCore.Blazor.Templates` illustrato nell'articolo <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="8b043-129">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="8b043-130">Il pacchetto di `Microsoft.Authentication.WebAssembly.Msal` aggiunge in modo transitivo il pacchetto di `Microsoft.AspNetCore.Components.WebAssembly.Authentication` all'app.</span><span class="sxs-lookup"><span data-stu-id="8b043-130">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="8b043-131">Supporto del servizio di autenticazione</span><span class="sxs-lookup"><span data-stu-id="8b043-131">Authentication service support</span></span>

<span data-ttu-id="8b043-132">Il supporto per l'autenticazione degli utenti viene registrato nel contenitore del servizio con il metodo di estensione `AddMsalAuthentication` fornito dal pacchetto di `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="8b043-132">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="8b043-133">Questo metodo configura tutti i servizi necessari per l'interazione dell'app con il provider di identità (IP).</span><span class="sxs-lookup"><span data-stu-id="8b043-133">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="8b043-134">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="8b043-134">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
});
```

<span data-ttu-id="8b043-135">Il metodo `AddMsalAuthentication` accetta un callback per configurare i parametri necessari per autenticare un'app.</span><span class="sxs-lookup"><span data-stu-id="8b043-135">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="8b043-136">Quando si registra l'app, è possibile ottenere i valori necessari per la configurazione dell'app dalla configurazione di AAD del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="8b043-136">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="8b043-137">Il modello di Blazor webassembly non configura automaticamente l'app per richiedere un token di accesso per un'API protetta.</span><span class="sxs-lookup"><span data-stu-id="8b043-137">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="8b043-138">Per eseguire il provisioning di un token come parte del flusso di accesso, aggiungere l'ambito agli ambiti dei token di accesso predefiniti della `MsalProviderOptions`:</span><span class="sxs-lookup"><span data-stu-id="8b043-138">To provision a token as part of the sign-in flow, add the scope to the default access token scopes of the `MsalProviderOptions`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{API ID URI}/{SCOPE}");
});
```

## <a name="index-page"></a><span data-ttu-id="8b043-139">Pagina di indice</span><span class="sxs-lookup"><span data-stu-id="8b043-139">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a><span data-ttu-id="8b043-140">Componente app</span><span class="sxs-lookup"><span data-stu-id="8b043-140">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="8b043-141">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="8b043-141">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="8b043-142">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="8b043-142">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="8b043-143">Componente di autenticazione</span><span class="sxs-lookup"><span data-stu-id="8b043-143">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="8b043-144">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8b043-144">Additional resources</span></span>

* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="8b043-145">Esercitazione: Creare un tenant di Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="8b043-145">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
