---
title: Proteggere un'app ospitata ASP.NET Core Blazor webassembly con Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 2ddbc9791ec9b31d55c9c6017d9d6d5be5c8dec8
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434486"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a><span data-ttu-id="1f545-102">Proteggere un'app ospitata ASP.NET Core Blazor webassembly con Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1f545-102">Secure an ASP.NET Core Blazor WebAssembly hosted app with Azure Active Directory</span></span>

<span data-ttu-id="1f545-103">Di [Javier Calvarro Nelson](https://github.com/javiercn) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1f545-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



<span data-ttu-id="1f545-104">Questo articolo descrive come creare un' [app ospitata da webassemblyBlazor](xref:blazor/hosting-models#blazor-webassembly) che usa [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="1f545-104">This article describes how to create a [Blazor WebAssembly hosted app](xref:blazor/hosting-models#blazor-webassembly) that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication.</span></span>

## <a name="register-apps-in-aad-b2c-and-create-solution"></a><span data-ttu-id="1f545-105">Registrare le app in AAD B2C e creare la soluzione</span><span class="sxs-lookup"><span data-stu-id="1f545-105">Register apps in AAD B2C and create solution</span></span>

### <a name="create-a-tenant"></a><span data-ttu-id="1f545-106">Creare un tenant</span><span class="sxs-lookup"><span data-stu-id="1f545-106">Create a tenant</span></span>

<span data-ttu-id="1f545-107">Seguire le istruzioni riportate nella [Guida introduttiva: configurare un tenant](/azure/active-directory/develop/quickstart-create-new-tenant) per creare un tenant in AAD.</span><span class="sxs-lookup"><span data-stu-id="1f545-107">Follow the guidance in [Quickstart: Set up a tenant](/azure/active-directory/develop/quickstart-create-new-tenant) to create a tenant in AAD.</span></span>

### <a name="register-a-server-api-app"></a><span data-ttu-id="1f545-108">Registrare un'app per le API server</span><span class="sxs-lookup"><span data-stu-id="1f545-108">Register a server API app</span></span>

<span data-ttu-id="1f545-109">Seguire le istruzioni disponibili in [Guida introduttiva: registrare un'applicazione con la piattaforma di identità Microsoft](/azure/active-directory/develop/quickstart-register-app) e gli argomenti di Azure AAD successivi per registrare un'app AAD per l'app per le *API Server* nell'area **Azure Active Directory** > **registrazioni app** del portale di Azure:</span><span class="sxs-lookup"><span data-stu-id="1f545-109">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register an AAD app for the *Server API app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="1f545-110">Selezionare **Nuova registrazione**.</span><span class="sxs-lookup"><span data-stu-id="1f545-110">Select **New registration**.</span></span>
1. <span data-ttu-id="1f545-111">Specificare un **nome** per l'app (ad esempio, **Blazor server AAD**).</span><span class="sxs-lookup"><span data-stu-id="1f545-111">Provide a **Name** for the app (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="1f545-112">Scegliere i **tipi di conto supportati**.</span><span class="sxs-lookup"><span data-stu-id="1f545-112">Choose a **Supported account types**.</span></span> <span data-ttu-id="1f545-113">Per questa esperienza è possibile selezionare **solo gli account in questa directory aziendale** (tenant singolo).</span><span class="sxs-lookup"><span data-stu-id="1f545-113">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="1f545-114">L' *app* per le API del server non richiede un **URI di reindirizzamento** in questo scenario, quindi lasciare l'elenco a discesa impostato su **Web** e non immettere un URI di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="1f545-114">The *Server API app* doesn't require a **Redirect URI** in this scenario, so leave the drop down set to **Web** and don't enter a redirect URI.</span></span>
1. <span data-ttu-id="1f545-115">Disabilitare le **autorizzazioni** > la casella **di controllo Concedi a OpenID e offline_access le autorizzazioni** .</span><span class="sxs-lookup"><span data-stu-id="1f545-115">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="1f545-116">Selezionare **Registra**.</span><span class="sxs-lookup"><span data-stu-id="1f545-116">Select **Register**.</span></span>

<span data-ttu-id="1f545-117">In **autorizzazioni API**rimuovere l'autorizzazione **Microsoft Graph** > **utente. Read** , perché l'app non richiede l'accesso al profilo di accesso o di UER.</span><span class="sxs-lookup"><span data-stu-id="1f545-117">In **API permissions**, remove the **Microsoft Graph** > **User.Read** permission, as the app doesn't require sign in or uer profile access.</span></span>

<span data-ttu-id="1f545-118">In **esporre un'API**:</span><span class="sxs-lookup"><span data-stu-id="1f545-118">In **Expose an API**:</span></span>

1. <span data-ttu-id="1f545-119">Selezionare **Aggiungi un ambito**.</span><span class="sxs-lookup"><span data-stu-id="1f545-119">Select **Add a scope**.</span></span>
1. <span data-ttu-id="1f545-120">Selezionare **Salva e continua**.</span><span class="sxs-lookup"><span data-stu-id="1f545-120">Select **Save and continue**.</span></span>
1. <span data-ttu-id="1f545-121">Specificare un **nome di ambito** , ad esempio `API.Access`.</span><span class="sxs-lookup"><span data-stu-id="1f545-121">Provide a **Scope name** (for example, `API.Access`).</span></span>
1. <span data-ttu-id="1f545-122">Fornire un **nome visualizzato** per il consenso dell'amministratore, ad esempio `Access API`.</span><span class="sxs-lookup"><span data-stu-id="1f545-122">Provide an **Admin consent display name** (for example, `Access API`).</span></span>
1. <span data-ttu-id="1f545-123">Fornire una **Descrizione del consenso dell'amministratore** , ad esempio `Allows the app to access server app API endpoints.`.</span><span class="sxs-lookup"><span data-stu-id="1f545-123">Provide an **Admin consent description** (for example, `Allows the app to access server app API endpoints.`).</span></span>
1. <span data-ttu-id="1f545-124">Verificare che lo **stato** sia impostato su **abilitato**.</span><span class="sxs-lookup"><span data-stu-id="1f545-124">Confirm that the **State** is set to **Enabled**.</span></span>
1. <span data-ttu-id="1f545-125">Selezionare **Aggiungi ambito**.</span><span class="sxs-lookup"><span data-stu-id="1f545-125">Select **Add scope**.</span></span>

<span data-ttu-id="1f545-126">Registrare le seguenti informazioni:</span><span class="sxs-lookup"><span data-stu-id="1f545-126">Record the following information:</span></span>

* <span data-ttu-id="1f545-127">*App per le API server* ID applicazione (ID client) (ad esempio, `11111111-1111-1111-1111-111111111111`)</span><span class="sxs-lookup"><span data-stu-id="1f545-127">*Server API app* Application ID (Client ID) (for example, `11111111-1111-1111-1111-111111111111`)</span></span>
* <span data-ttu-id="1f545-128">ID directory (ID tenant) (ad esempio, `222222222-2222-2222-2222-222222222222`)</span><span class="sxs-lookup"><span data-stu-id="1f545-128">Directory ID (Tenant ID) (for example, `222222222-2222-2222-2222-222222222222`)</span></span>
* <span data-ttu-id="1f545-129">Dominio del tenant AAD (ad esempio, `contoso.onmicrosoft.com`)</span><span class="sxs-lookup"><span data-stu-id="1f545-129">AAD Tenant domain (for example, `contoso.onmicrosoft.com`)</span></span>
* <span data-ttu-id="1f545-130">Ambito predefinito (ad esempio, `API.Access`)</span><span class="sxs-lookup"><span data-stu-id="1f545-130">Default scope (for example, `API.Access`)</span></span>

### <a name="register-a-client-app"></a><span data-ttu-id="1f545-131">Registrare un'app client</span><span class="sxs-lookup"><span data-stu-id="1f545-131">Register a client app</span></span>

<span data-ttu-id="1f545-132">Seguire le istruzioni disponibili in [Guida introduttiva: registrare un'applicazione con la piattaforma di identità Microsoft](/azure/active-directory/develop/quickstart-register-app) e gli argomenti di Azure AAD successivi per registrare un'app AAD per l' *app Client* nell'area **Azure Active Directory** > **registrazioni app** del portale di Azure:</span><span class="sxs-lookup"><span data-stu-id="1f545-132">Follow the guidance in [Quickstart: Register an application with the Microsoft identity platform](/azure/active-directory/develop/quickstart-register-app) and subsequent Azure AAD topics to register a AAD app for the *Client app* in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="1f545-133">Selezionare **Nuova registrazione**.</span><span class="sxs-lookup"><span data-stu-id="1f545-133">Select **New registration**.</span></span>
1. <span data-ttu-id="1f545-134">Specificare un **nome** per l'app (ad esempio, **Blazor AAD client**).</span><span class="sxs-lookup"><span data-stu-id="1f545-134">Provide a **Name** for the app (for example, **Blazor Client AAD**).</span></span>
1. <span data-ttu-id="1f545-135">Scegliere i **tipi di conto supportati**.</span><span class="sxs-lookup"><span data-stu-id="1f545-135">Choose a **Supported account types**.</span></span> <span data-ttu-id="1f545-136">Per questa esperienza è possibile selezionare **solo gli account in questa directory aziendale** (tenant singolo).</span><span class="sxs-lookup"><span data-stu-id="1f545-136">You may select **Accounts in this organizational directory only** (single tenant) for this experience.</span></span>
1. <span data-ttu-id="1f545-137">Lasciare l'elenco a discesa **URI di reindirizzamento** impostato su **Web**e specificare un uri di reindirizzamento di `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="1f545-137">Leave the **Redirect URI** drop down set to **Web**, and provide a redirect URI of `https://localhost:5001/authentication/login-callback`.</span></span>
1. <span data-ttu-id="1f545-138">Disabilitare le **autorizzazioni** > la casella **di controllo Concedi a OpenID e offline_access le autorizzazioni** .</span><span class="sxs-lookup"><span data-stu-id="1f545-138">Disable the **Permissions** > **Grant admin concent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="1f545-139">Selezionare **Registra**.</span><span class="sxs-lookup"><span data-stu-id="1f545-139">Select **Register**.</span></span>

<span data-ttu-id="1f545-140">In **Authentication** > **configurazioni della piattaforma** > **Web**:</span><span class="sxs-lookup"><span data-stu-id="1f545-140">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="1f545-141">Verificare che sia presente l' **URI di reindirizzamento** del `https://localhost:5001/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="1f545-141">Confirm the **Redirect URI** of `https://localhost:5001/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="1f545-142">Per **concessione implicita**, selezionare le caselle di controllo per i token di **accesso** e i **token ID**.</span><span class="sxs-lookup"><span data-stu-id="1f545-142">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="1f545-143">Per questa esperienza sono accettabili le impostazioni predefinite rimanenti per l'app.</span><span class="sxs-lookup"><span data-stu-id="1f545-143">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="1f545-144">Selezionare il pulsante **Salva**.</span><span class="sxs-lookup"><span data-stu-id="1f545-144">Select the **Save** button.</span></span>

<span data-ttu-id="1f545-145">In **autorizzazioni API**:</span><span class="sxs-lookup"><span data-stu-id="1f545-145">In **API permissions**:</span></span>

1. <span data-ttu-id="1f545-146">Verificare che l'app disponga **Microsoft Graph** autorizzazione > **utente. Read** .</span><span class="sxs-lookup"><span data-stu-id="1f545-146">Confirm that the app has **Microsoft Graph** > **User.Read** permission.</span></span>
1. <span data-ttu-id="1f545-147">Selezionare **Aggiungi un'autorizzazione** seguita da **API personali**.</span><span class="sxs-lookup"><span data-stu-id="1f545-147">Select **Add a permission** followed by **My APIs**.</span></span>
1. <span data-ttu-id="1f545-148">Selezionare l' *app per le API server* dalla colonna **nome** (ad esempio, **Blazor server AAD**).</span><span class="sxs-lookup"><span data-stu-id="1f545-148">Select the *Server API app* from the **Name** column (for example, **Blazor Server AAD**).</span></span>
1. <span data-ttu-id="1f545-149">Aprire l'elenco di **API** .</span><span class="sxs-lookup"><span data-stu-id="1f545-149">Open the **API** list.</span></span>
1. <span data-ttu-id="1f545-150">Abilitare l'accesso all'API, ad esempio `API.Access`.</span><span class="sxs-lookup"><span data-stu-id="1f545-150">Enable access to the API (for example, `API.Access`).</span></span>
1. <span data-ttu-id="1f545-151">Selezionare **Aggiungi autorizzazioni**.</span><span class="sxs-lookup"><span data-stu-id="1f545-151">Select **Add permissions**.</span></span>
1. <span data-ttu-id="1f545-152">Selezionare il pulsante **Concedi contenuto amministratore per {tenant Name}** .</span><span class="sxs-lookup"><span data-stu-id="1f545-152">Select the **Grant admin content for {TENANT NAME}** button.</span></span> <span data-ttu-id="1f545-153">Selezionare **Sì** per confermare.</span><span class="sxs-lookup"><span data-stu-id="1f545-153">Select **Yes** to confirm.</span></span>

<span data-ttu-id="1f545-154">Registrare l'ID applicazione dell' *app client* (ID client), ad esempio `33333333-3333-3333-3333-333333333333`.</span><span class="sxs-lookup"><span data-stu-id="1f545-154">Record the *Client app* Application ID (Client ID) (for example, `33333333-3333-3333-3333-333333333333`).</span></span>

### <a name="create-the-app"></a><span data-ttu-id="1f545-155">Creare l'app</span><span class="sxs-lookup"><span data-stu-id="1f545-155">Create the app</span></span>

<span data-ttu-id="1f545-156">Sostituire i segnaposto nel comando seguente con le informazioni registrate in precedenza ed eseguire il comando in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="1f545-156">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

<span data-ttu-id="1f545-157">Per specificare il percorso di output, che crea una cartella di progetto, se non esiste, includere l'opzione di output nel comando con un percorso, ad esempio `-o BlazorSample`.</span><span class="sxs-lookup"><span data-stu-id="1f545-157">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="1f545-158">Il nome della cartella diventa anche parte del nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="1f545-158">The folder name also becomes part of the project's name.</span></span>

> [!NOTE]
> <span data-ttu-id="1f545-159">Per un'importante modifica alla configurazione dell'ambito del token di accesso predefinito, vedere la sezione [supporto del servizio di autenticazione](#Authentication service support) .</span><span class="sxs-lookup"><span data-stu-id="1f545-159">See the [Authentication service support](#Authentication service support) section for an important configuration change to the default access token scope.</span></span> <span data-ttu-id="1f545-160">Il valore fornito dal modello di Blazor webassembly deve essere modificato manualmente dopo la creazione dell' *app client* dal modello.</span><span class="sxs-lookup"><span data-stu-id="1f545-160">The value provided by the Blazor WebAssembly template must be manually changed after the *Client app* is created from the template.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="1f545-161">Configurazione dell'app Server</span><span class="sxs-lookup"><span data-stu-id="1f545-161">Server app configuration</span></span>

<span data-ttu-id="1f545-162">*Questa sezione è relativa all'app **Server** della soluzione.*</span><span class="sxs-lookup"><span data-stu-id="1f545-162">*This section pertains to the solution's **Server** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="1f545-163">Pacchetto di autenticazione</span><span class="sxs-lookup"><span data-stu-id="1f545-163">Authentication package</span></span>

<span data-ttu-id="1f545-164">Il supporto per l'autenticazione e l'autorizzazione delle chiamate a ASP.NET Core API Web viene fornito dal `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span><span class="sxs-lookup"><span data-stu-id="1f545-164">The support for authenticating and authorizing calls to ASP.NET Core Web APIs is provided by the `Microsoft.AspNetCore.Authentication.AzureAD.UI`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a><span data-ttu-id="1f545-165">Supporto del servizio di autenticazione</span><span class="sxs-lookup"><span data-stu-id="1f545-165">Authentication service support</span></span>

<span data-ttu-id="1f545-166">Il metodo `AddAuthentication` configura i servizi di autenticazione all'interno dell'app e configura il gestore di JWT Bearer come metodo di autenticazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="1f545-166">The `AddAuthentication` method sets up authentication services within the app and configures the JWT Bearer handler as the default authentication method.</span></span> <span data-ttu-id="1f545-167">Il metodo `AddAzureADBearer` imposta i parametri specifici nel gestore di JWT Bearer necessario per convalidare i token emessi dall'Azure Active Directory:</span><span class="sxs-lookup"><span data-stu-id="1f545-167">The `AddAzureADBearer` method sets up the specific parameters in the JWT Bearer handler required to validate tokens emitted by the Azure Active Directory:</span></span>

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<span data-ttu-id="1f545-168">`UseAuthentication` e `UseAuthorization` assicurarsi che:</span><span class="sxs-lookup"><span data-stu-id="1f545-168">`UseAuthentication` and `UseAuthorization` ensure that:</span></span>

* <span data-ttu-id="1f545-169">L'app tenta di analizzare e convalidare i token nelle richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="1f545-169">The app attempts to parse and validate tokens on incoming requests.</span></span>
* <span data-ttu-id="1f545-170">Eventuali richieste che tentano di accedere a una risorsa protetta senza credenziali appropriate hanno esito negativo.</span><span class="sxs-lookup"><span data-stu-id="1f545-170">Any request attempting to access a protected resource without proper credentials fails.</span></span>

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a><span data-ttu-id="1f545-171">Impostazioni app</span><span class="sxs-lookup"><span data-stu-id="1f545-171">App settings</span></span>

<span data-ttu-id="1f545-172">Il file *appSettings. JSON* contiene le opzioni per configurare il gestore di connessione JWT usato per convalidare i token di accesso.</span><span class="sxs-lookup"><span data-stu-id="1f545-172">The *appsettings.json* file contains the options to configure the JWT bearer handler used to validate access tokens.</span></span>

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
  }
}
```

### <a name="weatherforecast-controller"></a><span data-ttu-id="1f545-173">Controller WeatherForecast</span><span class="sxs-lookup"><span data-stu-id="1f545-173">WeatherForecast controller</span></span>

<span data-ttu-id="1f545-174">Il controller WeatherForecast (*Controllers/WeatherForecastController. cs*) espone un'API protetta con l'attributo `[Authorize]` applicato al controller.</span><span class="sxs-lookup"><span data-stu-id="1f545-174">The WeatherForecast controller (*Controllers/WeatherForecastController.cs*) exposes a protected API with the `[Authorize]` attribute applied to the controller.</span></span> <span data-ttu-id="1f545-175">È **importante** comprendere che:</span><span class="sxs-lookup"><span data-stu-id="1f545-175">It's **important** to understand that:</span></span>

* <span data-ttu-id="1f545-176">L'attributo `[Authorize]` in questo controller API è l'unico elemento che protegge questa API da accessi non autorizzati.</span><span class="sxs-lookup"><span data-stu-id="1f545-176">The `[Authorize]` attribute in this API controller is the only thing that protect this API from unauthorized access.</span></span>
* <span data-ttu-id="1f545-177">L'attributo `[Authorize]` usato nell'app Blazor webassembly funge solo da hint per l'app che l'utente deve essere autorizzato affinché l'app funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="1f545-177">The `[Authorize]` attribute used in the Blazor WebAssembly app only serves as a hint to the app that the user should be authorized for the app to work correctly.</span></span>

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

## <a name="client-app-configuration"></a><span data-ttu-id="1f545-178">Configurazione dell'app client</span><span class="sxs-lookup"><span data-stu-id="1f545-178">Client app configuration</span></span>

<span data-ttu-id="1f545-179">*Questa sezione riguarda l'app **client** della soluzione.*</span><span class="sxs-lookup"><span data-stu-id="1f545-179">*This section pertains to the solution's **Client** app.*</span></span>

### <a name="authentication-package"></a><span data-ttu-id="1f545-180">Pacchetto di autenticazione</span><span class="sxs-lookup"><span data-stu-id="1f545-180">Authentication package</span></span>

<span data-ttu-id="1f545-181">Quando viene creata un'app per usare gli account aziendali o dell'Istituto di istruzione (`SingleOrg`), l'app riceve automaticamente un riferimento al pacchetto per [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span><span class="sxs-lookup"><span data-stu-id="1f545-181">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`).</span></span> <span data-ttu-id="1f545-182">Il pacchetto fornisce un set di primitive che consentono all'app di autenticare gli utenti e ottenere i token per chiamare le API protette.</span><span class="sxs-lookup"><span data-stu-id="1f545-182">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="1f545-183">Se si aggiunge l'autenticazione a un'app, aggiungere manualmente il pacchetto al file di progetto dell'app:</span><span class="sxs-lookup"><span data-stu-id="1f545-183">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

<span data-ttu-id="1f545-184">Sostituire `{VERSION}` nel riferimento al pacchetto precedente con la versione del pacchetto `Microsoft.AspNetCore.Blazor.Templates` illustrato nell'articolo <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="1f545-184">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

<span data-ttu-id="1f545-185">Il pacchetto di `Microsoft.Authentication.WebAssembly.Msal` aggiunge in modo transitivo il pacchetto di `Microsoft.AspNetCore.Components.WebAssembly.Authentication` all'app.</span><span class="sxs-lookup"><span data-stu-id="1f545-185">The `Microsoft.Authentication.WebAssembly.Msal` package transitively adds the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package to the app.</span></span>

### <a name="authentication-service-support"></a><span data-ttu-id="1f545-186">Supporto del servizio di autenticazione</span><span class="sxs-lookup"><span data-stu-id="1f545-186">Authentication service support</span></span>

<span data-ttu-id="1f545-187">Il supporto per l'autenticazione degli utenti viene registrato nel contenitore del servizio con il metodo di estensione `AddMsalAuthentication` fornito dal pacchetto di `Microsoft.Authentication.WebAssembly.Msal`.</span><span class="sxs-lookup"><span data-stu-id="1f545-187">Support for authenticating users is registered in the service container with the `AddMsalAuthentication` extension method provided by the `Microsoft.Authentication.WebAssembly.Msal` package.</span></span> <span data-ttu-id="1f545-188">Questo metodo configura tutti i servizi necessari per l'interazione dell'app con il provider di identità (IP).</span><span class="sxs-lookup"><span data-stu-id="1f545-188">This method sets up all of the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="1f545-189">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1f545-189">*Program.cs*:</span></span>

<span data-ttu-id="1f545-190">Quando viene generata l' *app client* , l'ambito del token di accesso predefinito è nel formato `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`.</span><span class="sxs-lookup"><span data-stu-id="1f545-190">When the *Client app* is generated, the default access token scope is of the format `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`.</span></span> <span data-ttu-id="1f545-191">**Rimuovere la parte `api://` del valore dell'ambito.**</span><span class="sxs-lookup"><span data-stu-id="1f545-191">**Remove the `api://` portion of the scope value.**</span></span> <span data-ttu-id="1f545-192">Questo problema verrà risolto in una versione di anteprima futura.</span><span class="sxs-lookup"><span data-stu-id="1f545-192">This issue will be addressed in a future preview release.</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> <span data-ttu-id="1f545-193">L'ambito del token di accesso predefinito deve essere nel formato `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`, ad esempio `11111111-1111-1111-1111-111111111111/API.Access`.</span><span class="sxs-lookup"><span data-stu-id="1f545-193">The default access token scope must be in the format `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` (for example, `11111111-1111-1111-1111-111111111111/API.Access`).</span></span> <span data-ttu-id="1f545-194">Se per l'impostazione dell'ambito vengono forniti uno schema o uno schema e un host, come illustrato nel portale di Azure, l' *app client* genera un'eccezione non gestita quando riceve una risposta *401 non autorizzata* dall'app per le *API del server*.</span><span class="sxs-lookup"><span data-stu-id="1f545-194">If a scheme or scheme and host is provided to the scope setting (as shown in the Azure Portal), the *Client app* throws an unhandled exception when it receives a *401 Unauthorized* response from the *Server API app*.</span></span>

<span data-ttu-id="1f545-195">Il metodo `AddMsalAuthentication` accetta un callback per configurare i parametri necessari per autenticare un'app.</span><span class="sxs-lookup"><span data-stu-id="1f545-195">The `AddMsalAuthentication` method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="1f545-196">Quando si registra l'app, è possibile ottenere i valori necessari per la configurazione dell'app dalla configurazione di AAD del portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="1f545-196">The values required for configuring the app can be obtained from the Azure Portal AAD configuration when you register the app.</span></span>

<span data-ttu-id="1f545-197">Gli ambiti dei token di accesso predefiniti rappresentano l'elenco degli ambiti dei token di accesso:</span><span class="sxs-lookup"><span data-stu-id="1f545-197">The default access token scopes represent the list of access token scopes that are:</span></span>

* <span data-ttu-id="1f545-198">Incluso per impostazione predefinita nella richiesta di accesso.</span><span class="sxs-lookup"><span data-stu-id="1f545-198">Included by default in the sign in request.</span></span>
* <span data-ttu-id="1f545-199">Utilizzato per eseguire il provisioning di un token di accesso immediatamente dopo l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="1f545-199">Used to provision an access token immediately after authentication.</span></span>

<span data-ttu-id="1f545-200">Tutti gli ambiti devono appartenere alla stessa app per ogni regola di Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1f545-200">All scopes must belong to the same app per Azure Active Directory rules.</span></span> <span data-ttu-id="1f545-201">Per altre app per le API è possibile aggiungere altri ambiti, se necessario:</span><span class="sxs-lookup"><span data-stu-id="1f545-201">Additional scopes can be added for additional API apps as needed:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a><span data-ttu-id="1f545-202">Pagina di indice</span><span class="sxs-lookup"><span data-stu-id="1f545-202">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a><span data-ttu-id="1f545-203">Componente app</span><span class="sxs-lookup"><span data-stu-id="1f545-203">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="1f545-204">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="1f545-204">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="1f545-205">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="1f545-205">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a><span data-ttu-id="1f545-206">Componente di autenticazione</span><span class="sxs-lookup"><span data-stu-id="1f545-206">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="1f545-207">Componente FetchData</span><span class="sxs-lookup"><span data-stu-id="1f545-207">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="1f545-208">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="1f545-208">Run the app</span></span>

<span data-ttu-id="1f545-209">Eseguire l'app dal progetto server.</span><span class="sxs-lookup"><span data-stu-id="1f545-209">Run the app from the Server project.</span></span> <span data-ttu-id="1f545-210">Quando si usa Visual Studio, selezionare il progetto server in **Esplora soluzioni** e selezionare il pulsante **Esegui** sulla barra degli strumenti o avviare l'app dal menu **debug** .</span><span class="sxs-lookup"><span data-stu-id="1f545-210">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="1f545-211">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1f545-211">Additional resources</span></span>

* <xref:security/authentication/azure-active-directory/index>
