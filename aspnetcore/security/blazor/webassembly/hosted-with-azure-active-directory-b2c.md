---
title: Proteggere un'app ospitata ASP.NET Core Blazor webassembly con Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: 12e09cf7e27f85473d84f42564d13e1c0ed5dff1
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434447"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a><span data-ttu-id="14e77-102">Proteggere un'app ospitata ASP.NET Core Blazor webassembly con Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="14e77-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory B2C</span></span>

<span data-ttu-id="14e77-103">Di [Javier Calvarro Nelson](https://github.com/javiercn) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="14e77-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="14e77-104">Questo articolo descrive come creare un'app Blazor webassembly autonoma che usa [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="14e77-104">This article describes how to create a Blazor WebAssembly standalone app that uses [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="14e77-105">Registrare le app in AAD B2C e creare la soluzione</span><span class="sxs-lookup"><span data-stu-id="14e77-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="14e77-106">Creare un tenant</span><span class="sxs-lookup"><span data-stu-id="14e77-106">Create a tenant</span></span>

<span data-ttu-id="14e77-107">Seguire le istruzioni in [esercitazione: creare un tenant di Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-create-tenant) per creare un tenant di AAD B2C e registrare le informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="14e77-107">Follow the guidance in [Tutorial: Create an Azure Active Directory B2C tenant](/azure/active-directory-b2c/tutorial-create-tenant) to create an AAD B2C tenant and record the following information:</span></span>

* <span data-ttu-id="14e77-108">AAD B2C istanza (ad esempio, `https://contoso.b2clogin.com/`, che include la barra finale)</span><span class="sxs-lookup"><span data-stu-id="14e77-108">AAD B2C instance (for example, `https://contoso.b2clogin.com/`, which includes the trailing slash)</span></span>
* <span data-ttu-id="14e77-109">AAD B2C dominio tenant, ad esempio `contoso.onmicrosoft.com`</span><span class="sxs-lookup"><span data-stu-id="14e77-109">AAD B2C Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="14e77-110">Registrare un'app per le API server</span><span class="sxs-lookup"><span data-stu-id="14e77-110">Register a server API app</span></span>

<span data-ttu-id="14e77-111">Seguire le istruzioni riportate in [esercitazione: registrare un'applicazione in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) per registrare un'app AAD per l'app per le *API Server* nell'area **Azure Active Directory** > **registrazioni app** del portale di Azure:</span><span class="sxs-lookup"><span data-stu-id="14e77-111">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="14e77-112">Selezionare **Nuova registrazione**.</span><span class="sxs-lookup"><span data-stu-id="14e77-112">Select **New registration**.</span></span>
1. <span data-ttu-id="14e77-113">Specificare un **nome** per l'app (ad esempio, **Blazor AAD B2C server**).</span><span class="sxs-lookup"><span data-stu-id="14e77-113">Provide a **Name** for the app (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="14e77-114">Per i **tipi di account supportati**, selezionare **account in qualsiasi directory organizzativa o provider di identità. Per l'autenticazione degli utenti con Azure AD B2C.**</span><span class="sxs-lookup"><span data-stu-id="14e77-114">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="14e77-115">(multi-tenant) per questa esperienza.</span><span class="sxs-lookup"><span data-stu-id="14e77-115">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="14e77-116">L' *app* per le API del server non richiede un **URI di reindirizzamento** in questo scenario, quindi lasciare l'elenco a discesa impostato su **Web** e non immettere un URI di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="14e77-116">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="14e77-117">Verificare che siano abilitate le **autorizzazioni** > concedere l'autorizzazione **concent per l'amministratore a OpenID e offline_access** .</span><span class="sxs-lookup"><span data-stu-id="14e77-117">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="14e77-118">Selezionare **Registra**.</span><span class="sxs-lookup"><span data-stu-id="14e77-118">Select **Register**.</span></span>

<span data-ttu-id="14e77-119">In **esporre un'API**:</span><span class="sxs-lookup"><span data-stu-id="14e77-119">In **Expose an API**:</span></span>

1. <span data-ttu-id="14e77-120">Selezionare **Aggiungi un ambito**.</span><span class="sxs-lookup"><span data-stu-id="14e77-120">Select **Add a scope**.</span></span>
1. <span data-ttu-id="14e77-121">Selezionare **Salva e continua**.</span><span class="sxs-lookup"><span data-stu-id="14e77-121">Select **Save and continue**.</span></span>
1. <span data-ttu-id="14e77-122">Specificare un **nome di ambito** , ad esempio `API.Access`.</span><span class="sxs-lookup"><span data-stu-id="14e77-122">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="14e77-123">Fornire un **nome visualizzato** per il consenso dell'amministratore, ad esempio `Access API`.</span><span class="sxs-lookup"><span data-stu-id="14e77-123">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="14e77-124">Fornire una **Descrizione del consenso dell'amministratore** , ad esempio `Allows the app to access server app API endpoints.`.</span><span class="sxs-lookup"><span data-stu-id="14e77-124">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="14e77-125">Verificare che lo **stato** sia impostato su **abilitato**.</span><span class="sxs-lookup"><span data-stu-id="14e77-125">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="14e77-126">Selezionare **Aggiungi ambito**.</span><span class="sxs-lookup"><span data-stu-id="14e77-126">Select **Add scope**.</span></span>

<span data-ttu-id="14e77-127">Registrare le seguenti informazioni:</span><span class="sxs-lookup"><span data-stu-id="14e77-127">Record the following information:</span></span>

* <span data-ttu-id="14e77-128">*App per le API server* ID applicazione (ID client) (ad esempio, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="14e77-128">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="14e77-129">ID directory (ID tenant) (ad esempio, `222222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="14e77-129">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="14e77-130">*App per le API server* URI ID app (ad esempio `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, il portale di Azure potrebbe avere come valore predefinito l'ID client)</span><span class="sxs-lookup"><span data-stu-id="14e77-130">*Server API app* App ID URI (for example, `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, the Azure portal might default the value to the Client ID)</span></span>
* <span data-ttu-id="14e77-131">Ambito predefinito (ad esempio, `API.Access`)</span><span class="sxs-lookup"><span data-stu-id="14e77-131">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="14e77-132">Registrare un'app client</span><span class="sxs-lookup"><span data-stu-id="14e77-132">Register a client app</span></span>

<span data-ttu-id="14e77-133">Seguire le istruzioni riportate in [esercitazione: registrare un'applicazione in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) di nuovo per registrare un'app AAD per l' *app Client* nell'area **Azure Active Directory** > **registrazioni app** del portale di Azure:</span><span class="sxs-lookup"><span data-stu-id="14e77-133">Follow the guidance in [Tutorial: Register an application in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) again to register an AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="14e77-134">Selezionare **Nuova registrazione**.</span><span class="sxs-lookup"><span data-stu-id="14e77-134">Select **New registration**.</span></span>
1. <span data-ttu-id="14e77-135">Specificare un **nome** per l'app, ad esempio **Blazor AAD B2C client**.</span><span class="sxs-lookup"><span data-stu-id="14e77-135">Provide a **Name** for the app (for example, **Blazor Client AAD B2C**).</span></span>
1. <span data-ttu-id="14e77-136">Per i **tipi di account supportati**, selezionare **account in qualsiasi directory organizzativa o provider di identità. Per l'autenticazione degli utenti con Azure AD B2C.**</span><span class="sxs-lookup"><span data-stu-id="14e77-136">For **Supported account types**, select **Accounts in any organizational directory or any identity provider. For authenticating users with Azure AD B2C.**</span></span> <span data-ttu-id="14e77-137">(multi-tenant) per questa esperienza.</span><span class="sxs-lookup"><span data-stu-id="14e77-137">(multi-tenant) for this experience.</span></span>
1. <span data-ttu-id="14e77-138">Lasciare l'elenco a discesa **URI di reindirizzamento** impostato su **Web**e specificare un uri di reindirizzamento di `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="14e77-138">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="14e77-139">Verificare che siano abilitate le **autorizzazioni** > concedere l'autorizzazione **concent per l'amministratore a OpenID e offline_access** .</span><span class="sxs-lookup"><span data-stu-id="14e77-139">Confirm that **Permissions** > **Grant admin concent to openid and offline_access permissions** is enabled.</span></span>
1. <span data-ttu-id="14e77-140">Selezionare **Registra**.</span><span class="sxs-lookup"><span data-stu-id="14e77-140">Select **Register**.</span></span>

<span data-ttu-id="14e77-141">In **Authentication** > **configurazioni della piattaforma** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="14e77-141">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="14e77-142">Verificare che sia presente l' **URI di reindirizzamento** del `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="14e77-142">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="14e77-143">Per **concessione implicita**, selezionare le caselle di controllo per i token di **accesso** e i **token ID**.</span><span class="sxs-lookup"><span data-stu-id="14e77-143">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="14e77-144">Per questa esperienza sono accettabili le impostazioni predefinite rimanenti per l'app.</span><span class="sxs-lookup"><span data-stu-id="14e77-144">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="14e77-145">Selezionare il pulsante **Salva**.</span><span class="sxs-lookup"><span data-stu-id="14e77-145">Select the **Save** button.</span></span>

<span data-ttu-id="14e77-146">In **autorizzazioni API**:</span><span class="sxs-lookup"><span data-stu-id="14e77-146">In **API permissions**:</span></span>

1. <span data-ttu-id="14e77-147">Verificare che l'app disponga **Microsoft Graph** autorizzazione > **utente. Read** .</span><span class="sxs-lookup"><span data-stu-id="14e77-147">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="14e77-148">Selezionare **Aggiungi un'autorizzazione** seguita da **API personali**.</span><span class="sxs-lookup"><span data-stu-id="14e77-148">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="14e77-149">Selezionare l' *app per le API server* dalla colonna **nome** , ad esempio **Blazor AAD B2C server**.</span><span class="sxs-lookup"><span data-stu-id="14e77-149">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD B2C**).</span></span>
1. <span data-ttu-id="14e77-150">Aprire l'elenco di **API** .</span><span class="sxs-lookup"><span data-stu-id="14e77-150">Open the **API** list.</span></span>
1. <span data-ttu-id="14e77-151">Abilitare l'accesso all'API, ad esempio `API.Access`.</span><span class="sxs-lookup"><span data-stu-id="14e77-151">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="14e77-152">Selezionare **Aggiungi autorizzazioni**.</span><span class="sxs-lookup"><span data-stu-id="14e77-152">Select **Add permissions**.</span></span>
1. <span data-ttu-id="14e77-153">Selezionare il pulsante **Concedi contenuto amministratore per {tenant Name}** .</span><span class="sxs-lookup"><span data-stu-id="14e77-153">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="14e77-154">Selezionare **Sì** per confermare.</span><span class="sxs-lookup"><span data-stu-id="14e77-154">Select **Yes** to confirm.</span></span>

<span data-ttu-id="14e77-155">In **Home** > **Azure ad B2C** > **flussi utente**:</span><span class="sxs-lookup"><span data-stu-id="14e77-155">In **Home** > **Azure AD B2C** > **User flows**:</span></span>

[<span data-ttu-id="14e77-156">Creare un flusso utente di iscrizione e accesso</span><span class="sxs-lookup"><span data-stu-id="14e77-156">Create a sign-up and sign-in user flow</span></span>](/azure/active-directory-b2c/tutorial-create-user-flows)

<span data-ttu-id="14e77-157">Selezionare come minimo l'attributo **Application claims** > **Display Name** user per popolare il `context.User.Identity.Name` nel componente `LoginDisplay` (*Shared/LoginDisplay. Razor*).</span><span class="sxs-lookup"><span data-stu-id="14e77-157">At a minimum, select the **Application claims** > **Display Name** user attribute to populate the `context.User.Identity.Name` in the `LoginDisplay` component (*Shared/LoginDisplay.razor*).</span></span>

<span data-ttu-id="14e77-158">Registrare le seguenti informazioni:</span><span class="sxs-lookup"><span data-stu-id="14e77-158">Record the following information:</span></span>

* <span data-ttu-id="14e77-159">Registrare l'ID applicazione dell' *app client* (ID client), ad esempio `33333333-3333-3333-3333-333333333333`.</span><span class="sxs-lookup"><span data-stu-id="14e77-159">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>
* <span data-ttu-id="14e77-160">Registrare il nome del flusso utente di iscrizione e accesso creato per l'app, ad esempio `B2C_1_signupsignin`.</span><span class="sxs-lookup"><span data-stu-id="14e77-160">Record the sign-up and sign-in user flow name created for the app (for example, `B2C_1_signupsignin`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="14e77-161">Creare l'app</span><span class="sxs-lookup"><span data-stu-id="14e77-161">Create the app</span></span>

<span data-ttu-id="14e77-162">Sostituire i segnaposto nel comando seguente con le informazioni registrate in precedenza ed eseguire il comando in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="14e77-162">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho -ssp "{SIGN UP OR SIGN IN POLICY}" --tenant-id "{TENANT ID}"
```

<span data-ttu-id="14e77-163">Per specificare il percorso di output, che crea una cartella di progetto, se non esiste, includere l'opzione di output nel comando con un percorso, ad esempio `-o BlazorSample`.</span><span class="sxs-lookup"><span data-stu-id="14e77-163">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="14e77-164">Il nome della cartella diventa anche parte del nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="14e77-164">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="14e77-165">Configurazione dell'app Server</span><span class="sxs-lookup"><span data-stu-id="14e77-165">Server app configuration</span></span>

<span data-ttu-id="14e77-166">*Questa sezione è relativa all'app **Server** della soluzione.*</span><span class="sxs-lookup"><span data-stu-id="14e77-166">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="14e77-167">Pacchetto di autenticazione</span><span class="sxs-lookup"><span data-stu-id="14e77-167">Authentication package</span></span>

<span data-ttu-id="14e77-168">Il supporto per l'autenticazione e l'autorizzazione delle chiamate a ASP.NET Core API Web viene fornito dal `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span><span class="sxs-lookup"><span data-stu-id="14e77-168">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="14e77-169">Supporto del servizio di autenticazione</span><span class="sxs-lookup"><span data-stu-id="14e77-169">Authentication service support</span></span>

<span data-ttu-id="14e77-170">Il metodo `AddAuthentication` configura i servizi di autenticazione all'interno dell'app e configura il gestore di JWT Bearer come metodo di autenticazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="14e77-170">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="14e77-171">Il metodo `AddAzureADBearer` imposta i parametri specifici nel gestore di JWT Bearer necessario per convalidare i token emessi dall'Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="14e77-171">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="14e77-172">`UseAuthentication` e `UseAuthorization` assicurarsi che:</span><span class="sxs-lookup"><span data-stu-id="14e77-172">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="14e77-173">L'app tenta di analizzare e convalidare i token nelle richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="14e77-173">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="14e77-174">Eventuali richieste che tentano di accedere a una risorsa protetta senza credenziali appropriate hanno esito negativo.</span><span class="sxs-lookup"><span data-stu-id="14e77-174">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="14e77-175">Impostazioni app</span><span class="sxs-lookup"><span data-stu-id="14e77-175">App settings</span></span>

<span data-ttu-id="14e77-176">Il file *appSettings. JSON* contiene le opzioni per configurare il gestore di connessione JWT usato per convalidare i token di accesso.</span><span class="sxs-lookup"><span data-stu-id="14e77-176">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "{API CLIENT ID}",
    "Domain": "{DOMAIN}",
    "SignUpSignInPolicyId": "{SIGN UP OR SIGN IN POLICY}"
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="14e77-177">Controller WeatherForecast</span><span class="sxs-lookup"><span data-stu-id="14e77-177">WeatherForecast controller</span></span>

<span data-ttu-id="14e77-178">Il controller WeatherForecast (*Controllers/WeatherForecastController. cs*) espone un'API protetta con l'attributo `[Authorize]` applicato al controller.</span><span class="sxs-lookup"><span data-stu-id="14e77-178">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="14e77-179">È **importante** comprendere che:</span><span class="sxs-lookup"><span data-stu-id="14e77-179">It's **important** to understand that:</span></span>

* <span data-ttu-id="14e77-180">L'attributo `[Authorize]` in questo controller API è l'unico elemento che protegge questa API da accessi non autorizzati.</span><span class="sxs-lookup"><span data-stu-id="14e77-180">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="14e77-181">L'attributo `[Authorize]` usato nell'app Blazor webassembly funge solo da hint per l'app che l'utente deve essere autorizzato affinché l'app funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="14e77-181">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="14e77-182">Configurazione dell'app client</span><span class="sxs-lookup"><span data-stu-id="14e77-182">Client app configuration</span></span>

<span data-ttu-id="14e77-183">*Questa sezione riguarda l'app **client** della soluzione.*</span><span class="sxs-lookup"><span data-stu-id="14e77-183">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="14e77-184">Pacchetto di autenticazione</span><span class="sxs-lookup"><span data-stu-id="14e77-184">Authentication package</span></span>

<span data-ttu-id="14e77-185">Quando viene creata un'app per usare un singolo account B2C (`IndividualB2C`), l'app riceve automaticamente un riferimento al pacchetto per [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="14e77-185">When an app is created to use an Individual B2C Account (`IndividualB2C`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="14e77-186">Il pacchetto fornisce un set di primitive che consentono all'app di autenticare gli utenti e ottenere i token per chiamare le API protette.</span><span class="sxs-lookup"><span data-stu-id="14e77-186">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="14e77-187">Se si aggiunge l'autenticazione a un'app, aggiungere manualmente il pacchetto al file di progetto dell'app:</span><span class="sxs-lookup"><span data-stu-id="14e77-187">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="14e77-188">Sostituire `{VERSION}` nel riferimento al pacchetto precedente con la versione del pacchetto `Microsoft.AspNetCore.Blazor.Templates` illustrato nell'articolo <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="14e77-188">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="14e77-189">Il pacchetto di `Microsoft.Authentication.WebAssembly.Msal` aggiunge in modo transitivo il pacchetto di `Microsoft.AspNetCore.Components.WebAssembly.Authentication` all'app.</span><span class="sxs-lookup"><span data-stu-id="14e77-189">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="14e77-190">Supporto del servizio di autenticazione</span><span class="sxs-lookup"><span data-stu-id="14e77-190">Authentication service support</span></span>

<span data-ttu-id="14e77-191">Il supporto per l'autenticazione degli utenti viene registrato nel contenitore del servizio con il metodo di estensione `AddMsalAuthentication` fornito dal pacchetto di `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="14e77-191">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="14e77-192">Questo metodo configura tutti i servizi necessari per l'interazione dell'app con il provider di identità (IP).</span><span class="sxs-lookup"><span data-stu-id="14e77-192">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="14e77-193">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="14e77-193">*Program.cs*:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{DEFAULT SCOPE}");
});
```

<span data-ttu-id="14e77-194">Il metodo `AddMsalAuthentication` accetta un callback per configurare i parametri necessari per autenticare un'app.</span><span class="sxs-lookup"><span data-stu-id="14e77-194">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="14e77-195">Quando si registra l'app, è possibile ottenere i valori necessari per la configurazione dell'app dalla configurazione di AAD del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="14e77-195">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="14e77-196">Il modello di Blazor webassembly configura automaticamente l'app in modo da richiedere un token di accesso per un'API protetta per l'ambito predefinito fornito al comando `dotnet new` (`{APP ID URI}/{DEFAULT SCOPE}`).</span><span class="sxs-lookup"><span data-stu-id="14e77-196">The Blazor WebAssembly template automatically configures the app to request an access token for a secure API for the default scope provided to the `dotnet new` command (`{APP ID URI}/{DEFAULT SCOPE}`).</span></span>

<span data-ttu-id="14e77-197">Gli ambiti dei token di accesso predefiniti rappresentano l'elenco degli ambiti dei token di accesso:</span><span class="sxs-lookup"><span data-stu-id="14e77-197">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="14e77-198">Incluso per impostazione predefinita nella richiesta di accesso.</span><span class="sxs-lookup"><span data-stu-id="14e77-198">Included by default in the sign in request.</span></span>
* <span data-ttu-id="14e77-199">Utilizzato per eseguire il provisioning di un token di accesso immediatamente dopo l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="14e77-199">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="14e77-200">Tutti gli ambiti devono appartenere alla stessa app per ogni regola di Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="14e77-200">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="14e77-201">Per altre app per le API è possibile aggiungere altri ambiti, se necessario:</span><span class="sxs-lookup"><span data-stu-id="14e77-201">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="14e77-202">Pagina di indice</span><span class="sxs-lookup"><span data-stu-id="14e77-202">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a><span data-ttu-id="14e77-203">Componente app</span><span class="sxs-lookup"><span data-stu-id="14e77-203">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="14e77-204">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="14e77-204">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="14e77-205">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="14e77-205">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="14e77-206">Componente di autenticazione</span><span class="sxs-lookup"><span data-stu-id="14e77-206">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="14e77-207">Componente FetchData</span><span class="sxs-lookup"><span data-stu-id="14e77-207">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="14e77-208">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="14e77-208">Run the app</span></span>

<span data-ttu-id="14e77-209">Eseguire l'app dal progetto server.</span><span class="sxs-lookup"><span data-stu-id="14e77-209">Run the app from the Server project.</span></span> <span data-ttu-id="14e77-210">Quando si usa Visual Studio, selezionare il progetto server in **Esplora soluzioni** e selezionare il pulsante **Esegui** sulla barra degli strumenti o avviare l'app dal menu **debug** .</span><span class="sxs-lookup"><span data-stu-id="14e77-210">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="14e77-211">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="14e77-211">Additional resources</span></span>

* <xref:security/authentication/azure-ad-b2c>
* [<span data-ttu-id="14e77-212">Esercitazione: Creare un tenant di Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="14e77-212">Tutorial: Create an Azure Active Directory B2C tenant</span></span>](/azure/active-directory-b2c/tutorial-create-tenant)
