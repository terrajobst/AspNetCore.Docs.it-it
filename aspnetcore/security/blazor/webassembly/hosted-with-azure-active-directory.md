---
title: Proteggere un'app ospitata ASP.NET Core Blazor webassembly con Azure Active Directory
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 0803e436d66ef7df3c68739e674a8652fde11166
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083846"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a>Proteggere un'app ospitata ASP.NET Core Blazor webassembly con Azure Active Directory

Di [Javier Calvarro Nelson](https://github.com/javiercn) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



Questo articolo descrive come creare un' [app ospitata da webassemblyBlazor](xref:blazor/hosting-models#blazor-webassembly) che usa [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) per l'autenticazione.

## <a name="register-apps-in-aad-b2c-and-create-solution"></a>Registrare le app in AAD B2C e creare la soluzione

### <a name="create-a-tenant"></a>Creare un tenant

Seguire le istruzioni riportate nella [Guida introduttiva: configurare un tenant](/azure/active-directory/develop/quickstart-create-new-tenant) per creare un tenant in AAD.

### <a name="register-a-server-api-app"></a>Registrare un'app per le API server

Seguire le istruzioni disponibili in [Guida introduttiva: registrare un'applicazione con la piattaforma di identità Microsoft](/azure/active-directory/develop/quickstart-register-app) e gli argomenti di Azure AAD successivi per registrare un'app AAD per l'app per le *API Server* nell'area **Azure Active Directory** > **registrazioni app** del portale di Azure:

1. Selezionare **Nuova registrazione**.
1. Specificare un **nome** per l'app (ad esempio, **Blazor server AAD**).
1. Scegliere i **tipi di conto supportati**. Per questa esperienza è possibile selezionare **solo gli account in questa directory aziendale** (tenant singolo).
1. L' *app* per le API del server non richiede un **URI di reindirizzamento** in questo scenario, quindi lasciare l'elenco a discesa impostato su **Web** e non immettere un URI di reindirizzamento.
1. Disabilitare le **autorizzazioni** > la casella **di controllo Concedi a OpenID e offline_access le autorizzazioni** .
1. Selezionare **Registra**.

In **autorizzazioni API**rimuovere l'autorizzazione **Microsoft Graph** > **utente. Read** , perché l'app non richiede l'accesso al profilo di accesso o di UER.

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
* Dominio del tenant AAD (ad esempio, `contoso.onmicrosoft.com`)
* Ambito predefinito (ad esempio, `API.Access`)

### <a name="register-a-client-app"></a>Registrare un'app client

Seguire le istruzioni disponibili in [Guida introduttiva: registrare un'applicazione con la piattaforma di identità Microsoft](/azure/active-directory/develop/quickstart-register-app) e gli argomenti di Azure AAD successivi per registrare un'app AAD per l' *app Client* nell'area **Azure Active Directory** > **registrazioni app** del portale di Azure:

1. Selezionare **Nuova registrazione**.
1. Specificare un **nome** per l'app (ad esempio, **Blazor AAD client**).
1. Scegliere i **tipi di conto supportati**. Per questa esperienza è possibile selezionare **solo gli account in questa directory aziendale** (tenant singolo).
1. Lasciare l'elenco a discesa **URI di reindirizzamento** impostato su **Web**e specificare un uri di reindirizzamento di `https://localhost:5001/authentication/login-callback`.
1. Disabilitare le **autorizzazioni** > la casella **di controllo Concedi a OpenID e offline_access le autorizzazioni** .
1. Selezionare **Registra**.

In **Authentication** > **configurazioni della piattaforma** > **Web**:

1. Verificare che sia presente l' **URI di reindirizzamento** del `https://localhost:5001/authentication/login-callback`.
1. Per **concessione implicita**, selezionare le caselle di controllo per i token di **accesso** e i **token ID**.
1. Per questa esperienza sono accettabili le impostazioni predefinite rimanenti per l'app.
1. Selezionare il pulsante **Salva**.

In **autorizzazioni API**:

1. Verificare che l'app disponga **Microsoft Graph** autorizzazione > **utente. Read** .
1. Selezionare **Aggiungi un'autorizzazione** seguita da **API personali**.
1. Selezionare l' *app per le API server* dalla colonna **nome** (ad esempio, **Blazor server AAD**).
1. Aprire l'elenco di **API** .
1. Abilitare l'accesso all'API, ad esempio `API.Access`.
1. Selezionare **Aggiungi autorizzazioni**.
1. Selezionare il pulsante **Concedi contenuto amministratore per {tenant Name}** . Selezionare **Sì** per confermare.

Registrare l'ID applicazione dell' *app client* (ID client), ad esempio `33333333-3333-3333-3333-333333333333`.

### <a name="create-the-app"></a>Creare l'app

Sostituire i segnaposto nel comando seguente con le informazioni registrate in precedenza ed eseguire il comando in una shell dei comandi:

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

Per specificare il percorso di output, che crea una cartella di progetto, se non esiste, includere l'opzione di output nel comando con un percorso, ad esempio `-o BlazorSample`. Il nome della cartella diventa anche parte del nome del progetto.

> [!NOTE]
> Per un'importante modifica alla configurazione dell'ambito del token di accesso predefinito, vedere la sezione [supporto del servizio di autenticazione](#Authentication service support) . Il valore fornito dal modello di Blazor webassembly deve essere modificato manualmente dopo la creazione dell' *app client* dal modello.

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
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
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

Quando viene creata un'app per usare gli account aziendali o dell'Istituto di istruzione (`SingleOrg`), l'app riceve automaticamente un riferimento al pacchetto per [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) (`Microsoft.Authentication.WebAssembly.Msal`). Il pacchetto fornisce un set di primitive che consentono all'app di autenticare gli utenti e ottenere i token per chiamare le API protette.

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

Quando viene generata l' *app client* , l'ambito del token di accesso predefinito è nel formato `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`. **Rimuovere la parte `api://` del valore dell'ambito.** Questo problema verrà risolto in una versione di anteprima futura.

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
> L'ambito del token di accesso predefinito deve essere nel formato `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`, ad esempio `11111111-1111-1111-1111-111111111111/API.Access`. Se per l'impostazione dell'ambito vengono forniti uno schema o uno schema e un host, come illustrato nel portale di Azure, l' *app client* genera un'eccezione non gestita quando riceve una risposta *401 non autorizzata* dall'app per le *API del server*.

Il metodo `AddMsalAuthentication` accetta un callback per configurare i parametri necessari per autenticare un'app. Quando si registra l'app, è possibile ottenere i valori necessari per la configurazione dell'app dalla configurazione di AAD del portale di Azure.

Gli ambiti dei token di accesso predefiniti rappresentano l'elenco degli ambiti dei token di accesso:

* Incluso per impostazione predefinita nella richiesta di accesso.
* Utilizzato per eseguire il provisioning di un token di accesso immediatamente dopo l'autenticazione.

Tutti gli ambiti devono appartenere alla stessa app per ogni regola di Azure Active Directory. Per altre app per le API è possibile aggiungere altri ambiti, se necessario:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a>Pagina di indice

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

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

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:security/authentication/azure-active-directory/index>
