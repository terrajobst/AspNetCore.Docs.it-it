---
title: Proteggere un'app ospitata ASP.NET Core Blazor webassembly con Azure Active Directory B2C
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/22/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: 0083f179f85371d4751fb179194417681fc1a01d
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219064"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a>Proteggere un'app ospitata ASP.NET Core Blazor webassembly con Azure Active Directory B2C

Di [Javier Calvarro Nelson](https://github.com/javiercn) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Questo articolo descrive come creare un'app Blazor webassembly autonoma che usa [Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview) per l'autenticazione.

## <a name="register-apps-in-aad-b2c-and-create-solution"></a>Registrare le app in AAD B2C e creare la soluzione

### <a name="create-a-tenant"></a>Creare un tenant

Seguire le istruzioni in [esercitazione: creare un tenant di Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-create-tenant) per creare un tenant di AAD B2C e registrare le informazioni seguenti:

* AAD B2C istanza (ad esempio, `https://contoso.b2clogin.com/`, che include la barra finale)
* AAD B2C dominio tenant, ad esempio `contoso.onmicrosoft.com`

### <a name="register-a-server-api-app"></a>Registrare un'app per le API server

Seguire le istruzioni riportate in [esercitazione: registrare un'applicazione in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) per registrare un'app AAD per l'app per le *API Server* nell'area **Azure Active Directory** > **registrazioni app** del portale di Azure:

1. Selezionare **Nuova registrazione**.
1. Specificare un **nome** per l'app (ad esempio, **Blazor AAD B2C server**).
1. Per i **tipi di account supportati**, selezionare **account in qualsiasi directory organizzativa o provider di identità. Per l'autenticazione degli utenti con Azure AD B2C.** (multi-tenant) per questa esperienza.
1. L' *app* per le API del server non richiede un **URI di reindirizzamento** in questo scenario, quindi lasciare l'elenco a discesa impostato su **Web** e non immettere un URI di reindirizzamento.
1. Verificare che siano abilitate le **autorizzazioni** > concedere l'autorizzazione **concent per l'amministratore a OpenID e offline_access** .
1. Selezionare **Registra**.

In **esporre un'API**:

1. Selezionare **Aggiungi un ambito**.
1. Selezionare **Salva e continua**.
1. Specificare un **nome di ambito** , ad esempio `API.Access`.
1. Fornire un **nome visualizzato** per il consenso dell'amministratore, ad esempio `Access API`.
1. Fornire una **Descrizione del consenso dell'amministratore** , ad esempio `Allows the app to access server app API endpoints.`.
1. Verificare che lo **stato** sia impostato su **abilitato**.
1. Selezionare **Aggiungi ambito**.

Registrare le seguenti informazioni:

* *App per le API server* ID applicazione (ID client) (ad esempio, `11111111-1111-1111-1111-111111111111`)
* ID directory (ID tenant) (ad esempio, `222222222-2222-2222-2222-222222222222`)
* *App per le API server* URI ID app (ad esempio `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`, il portale di Azure potrebbe avere come valore predefinito l'ID client)
* Ambito predefinito (ad esempio, `API.Access`)

### <a name="register-a-client-app"></a>Registrare un'app client

Seguire le istruzioni riportate in [esercitazione: registrare un'applicazione in Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-register-applications) di nuovo per registrare un'app AAD per l' *app Client* nell'area **Azure Active Directory** > **registrazioni app** del portale di Azure:

1. Selezionare **Nuova registrazione**.
1. Specificare un **nome** per l'app, ad esempio **Blazor AAD B2C client**.
1. Per i **tipi di account supportati**, selezionare **account in qualsiasi directory organizzativa o provider di identità. Per l'autenticazione degli utenti con Azure AD B2C.** (multi-tenant) per questa esperienza.
1. Lasciare l'elenco a discesa **URI di reindirizzamento** impostato su **Web**e specificare un uri di reindirizzamento di `https://localhost:5001/authentication/login-callback`.
1. Verificare che siano abilitate le **autorizzazioni** > concedere l'autorizzazione **concent per l'amministratore a OpenID e offline_access** .
1. Selezionare **Registra**.

In **Authentication** > **configurazioni della piattaforma** > **Web**:

1. Verificare che sia presente l' **URI di reindirizzamento** del `https://localhost:5001/authentication/login-callback`.
1. Per **concessione implicita**, selezionare le caselle di controllo per i token di **accesso** e i **token ID**.
1. Per questa esperienza sono accettabili le impostazioni predefinite rimanenti per l'app.
1. Fare clic sul pulsante **Salva**.

In **autorizzazioni API**:

1. Verificare che l'app disponga **Microsoft Graph** autorizzazione > **utente. Read** .
1. Selezionare **Aggiungi un'autorizzazione** seguita da **API personali**.
1. Selezionare l' *app per le API server* dalla colonna **nome** , ad esempio **Blazor AAD B2C server**.
1. Aprire l'elenco di **API** .
1. Abilitare l'accesso all'API, ad esempio `API.Access`.
1. Selezionare **Aggiungi autorizzazioni**.
1. Selezionare il pulsante **Concedi contenuto amministratore per {tenant Name}** . Selezionare **Sì** per confermare.

In **Home** > **Azure ad B2C** > **flussi utente**:

[Creare un flusso utente di iscrizione e accesso](/azure/active-directory-b2c/tutorial-create-user-flows)

Selezionare come minimo l'attributo **Application claims** > **Display Name** user per popolare il `context.User.Identity.Name` nel componente `LoginDisplay` (*Shared/LoginDisplay. Razor*).

Registrare le seguenti informazioni:

* Registrare l'ID applicazione dell' *app client* (ID client), ad esempio `33333333-3333-3333-3333-333333333333`.
* Registrare il nome del flusso utente di iscrizione e accesso creato per l'app, ad esempio `B2C_1_signupsignin`.

### <a name="create-the-app"></a>Creare l'app

Sostituire i segnaposto nel comando seguente con le informazioni registrate in precedenza ed eseguire il comando in una shell dei comandi:

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho -ssp "{SIGN UP OR SIGN IN POLICY}" --tenant-id "{TENANT ID}"
```

Per specificare il percorso di output, che crea una cartella di progetto, se non esiste, includere l'opzione di output nel comando con un percorso, ad esempio `-o BlazorSample`. Il nome della cartella diventa anche parte del nome del progetto.

## <a name="server-app-configuration"></a>Configurazione dell'app Server

*Questa sezione è relativa all'app **Server** della soluzione.*

### <a name="authentication-package"></a>Pacchetto di autenticazione

Il supporto per l'autenticazione e l'autorizzazione delle chiamate a ASP.NET Core API Web viene fornito dal `Microsoft.AspNetCore.Authentication.AzureAD.UI`:

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a>Supporto del servizio di autenticazione

Il metodo `AddAuthentication` configura i servizi di autenticazione all'interno dell'app e configura il gestore di JWT Bearer come metodo di autenticazione predefinito. Il metodo `AddAzureADBearer` imposta i parametri specifici nel gestore di JWT Bearer necessario per convalidare i token emessi dall'Azure Active Directory:

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

`UseAuthentication` e `UseAuthorization` assicurarsi che:

* L'app tenta di analizzare e convalidare i token nelle richieste in ingresso.
* Eventuali richieste che tentano di accedere a una risorsa protetta senza credenziali appropriate hanno esito negativo.

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a>Impostazioni app

Il file *appSettings. JSON* contiene le opzioni per configurare il gestore di connessione JWT usato per convalidare i token di accesso.

```json
{
  "AzureAd": {
    "Instance": "https://{ORGANIZATION}.b2clogin.com/",
    "ClientId": "{API CLIENT ID}",
    "Domain": "{DOMAIN}",
    "SignUpSignInPolicyId": "{SIGN UP OR SIGN IN POLICY}"
  }
}
```

### <a name="weatherforecast-controller"></a>Controller WeatherForecast

Il controller WeatherForecast (*Controllers/WeatherForecastController. cs*) espone un'API protetta con l'attributo `[Authorize]` applicato al controller. È **importante** comprendere che:

* L'attributo `[Authorize]` in questo controller API è l'unico elemento che protegge questa API da accessi non autorizzati.
* L'attributo `[Authorize]` usato nell'app Blazor webassembly funge solo da hint per l'app che l'utente deve essere autorizzato affinché l'app funzioni correttamente.

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

## <a name="client-app-configuration"></a>Configurazione dell'app client

*Questa sezione riguarda l'app **client** della soluzione.*

### <a name="authentication-package"></a>Pacchetto di autenticazione

Quando viene creata un'app per usare un singolo account B2C (`IndividualB2C`), l'app riceve automaticamente un riferimento al pacchetto per [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`). Il pacchetto fornisce un set di primitive che consentono all'app di autenticare gli utenti e ottenere i token per chiamare le API protette.

Se si aggiunge l'autenticazione a un'app, aggiungere manualmente il pacchetto al file di progetto dell'app:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

Sostituire `{VERSION}` nel riferimento al pacchetto precedente con la versione del pacchetto `Microsoft.AspNetCore.Blazor.Templates` illustrato nell'articolo <xref:blazor/get-started>.

Il pacchetto di `Microsoft.Authentication.WebAssembly.Msal` aggiunge in modo transitivo il pacchetto di `Microsoft.AspNetCore.Components.WebAssembly.Authentication` all'app.

### <a name="authentication-service-support"></a>Supporto del servizio di autenticazione

Il supporto per l'autenticazione degli utenti viene registrato nel contenitore del servizio con il metodo di estensione `AddMsalAuthentication` fornito dal pacchetto di `Microsoft.Authentication.WebAssembly.Msal`. Questo metodo configura tutti i servizi necessari per l'interazione dell'app con il provider di identità (IP).

*Program.cs*:

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

Il metodo `AddMsalAuthentication` accetta un callback per configurare i parametri necessari per autenticare un'app. Quando si registra l'app, è possibile ottenere i valori necessari per la configurazione dell'app dalla configurazione di AAD del portale di Azure.

Il modello di Blazor webassembly configura automaticamente l'app in modo da richiedere un token di accesso per un'API protetta per l'ambito predefinito fornito al comando `dotnet new` (`{APP ID URI}/{DEFAULT SCOPE}`).

Gli ambiti dei token di accesso predefiniti rappresentano l'elenco degli ambiti dei token di accesso:

* Incluso per impostazione predefinita nella richiesta di accesso.
* Utilizzato per eseguire il provisioning di un token di accesso immediatamente dopo l'autenticazione.

Tutti gli ambiti devono appartenere alla stessa app per ogni regola di Azure Active Directory. Per altre app per le API è possibile aggiungere altri ambiti, se necessario:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{APP ID URI}/{SCOPE}");
});
```

### <a name="index-page"></a>Pagina di indice

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a>Componente app

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>Componente RedirectToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>Componente LoginDisplay

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a>Componente di autenticazione

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>Componente FetchData

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>Eseguire l'app

Eseguire l'app dal progetto server. Quando si usa Visual Studio, selezionare il progetto server in **Esplora soluzioni** e selezionare il pulsante **Esegui** sulla barra degli strumenti o avviare l'app dal menu **debug** .

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:security/authentication/azure-ad-b2c>
* [Esercitazione: Creare un tenant di Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-create-tenant)
