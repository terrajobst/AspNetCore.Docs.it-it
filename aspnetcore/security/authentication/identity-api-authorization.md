---
title: Introduzione all'autenticazione per App a pagina singola in ASP.NET Core
author: javiercn
description: Usa l'identità con un'App a singola pagina ospitate all'interno di un'app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 4afc9ac0a3c54b452c6a1b23e4de31d7e2fc5284
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894148"
---
# <a name="authentication-and-authorization-for-spas"></a>Autenticazione e autorizzazione per SPAs

ASP.NET Core 3.0 o versioni successive offre l'autenticazione nell'App a pagina singola (SPAs) utilizzando il supporto per l'autorizzazione API. ASP.NET Core Identity per l'autenticazione e l'archiviazione agli utenti viene combinata con [IdentityServer](https://identityserver.io/) per l'implementazione di Openid Connect.

È stato aggiunto un parametro di autenticazione per il **Angular** e **React** modelli che è simile al parametro di autenticazione nel progetto di **applicazione Web (Model-View-Controller)**  (MVC) e **applicazione Web** modelli di progetto (pagine Razor). I valori di parametri consentiti sono **None** e **singoli**. Il **React. js e Redux** modello di progetto non supporta il parametro di autenticazione in questo momento.

## <a name="create-an-app-with-api-authorization-support"></a>Creare un'app con supporto per l'autorizzazione API

Autenticazione e autorizzazione è utilizzabile con Angular e React SPAs. Aprire una shell dei comandi ed eseguire il comando seguente:

**Angular**:

```console
dotnet new angular -o <output_directory_name> -au Individual
```

**Reagire**:

```console
dotnet new react -o <output_directory_name> -au Individual
```

Il comando precedente crea un'app ASP.NET Core con un *ClientApp* directory contenente l'applicazione a singola pagina.

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>Descrizione generale dei componenti dell'app ASP.NET Core

Le sezioni seguenti descrivono le aggiunte al progetto quando è incluso il supporto di autenticazione:

### <a name="startup-class"></a>Classe di avvio

Il `Startup` classe ha le aggiunte seguenti:

* All'interno di `Startup.ConfigureServices` metodo:
  * Identità con l'interfaccia utente predefinita:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * IdentityServer con un'ulteriore `AddApiAuthorization` metodo helper che configurazioni alcuni predefinito nella parte superiore IdentityServer le convenzioni di ASP.NET Core:

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * L'autenticazione con un'ulteriore `AddIdentityServerJwt` metodo helper che consente di configurare l'app per convalidare i token JWT generato da IdentityServer:

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* All'interno di `Startup.Configure` metodo:
  * Il middleware di autenticazione che è responsabile per la convalida delle credenziali richieste e impostando l'utente per il contesto della richiesta:

    ```csharp
    app.UseAuthentication();
    ```

  * Il middleware IdentityServer che espone gli endpoint Openid Connect:

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

Questo metodo di supporto Configura IdentityServer per usare la configurazione supportata. IdentityServer è un framework potente ed estendibile per la gestione di problemi di protezione delle app. Allo stesso tempo, che espone inutili complessità per gli scenari più comuni. Di conseguenza, viene fornito un set di convenzioni e le opzioni di configurazione per l'utente che sono considerati un buon punto di partenza. Dopo l'autenticazione delle esigenze, tutta la potenza di IdentityServer è ancora disponibile per personalizzare l'autenticazione in base alle esigenze.

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt

Questo metodo di supporto consente di configurare una combinazione di criteri per l'app come il gestore di autenticazione predefinito. Il criterio è configurato per consentire a gestire tutte le richieste indirizzate a qualsiasi percorso secondario nel campo URL di identità dell'identità "/ Identity". Il `JwtBearerHandler` gestisce tutte le altre richieste. Inoltre, questo metodo registra un `<<ApplicationName>>API` della risorsa API con IdentityServer con un ambito predefinito di `<<ApplicationName>>API` e configura il middleware del token Bearer token JWT per convalidare i token emessi da IdentityServer per l'app.

### <a name="sampledatacontroller"></a>SampleDataController

Nel *Controllers\SampleDataController.cs* del file, si noti il `[Authorize]` attributo applicato alla classe che indica che l'utente deve essere autorizzato in base i criteri predefiniti per accedere alla risorsa. I criteri di autorizzazione predefinita accade per essere configurato per usare lo schema di autenticazione predefinito, che viene impostato in modo da `AddIdentityServerJwt` rispetto allo schema dei criteri è stato indicato in precedenza, rendendo il `JwtBearerHandler` configurate da tale metodo di supporto del gestore predefinito per richieste all'app.

### <a name="applicationdbcontext"></a>ApplicationDbContext

Nel *Data\ApplicationDbContext.cs* del file, si noti la stessa `DbContext` viene usato nell'identità con l'eccezione che estende `ApiAuthorizationDbContext` (più derivato dalla classe `IdentityDbContext`) da includere lo schema per IdentityServer.

Per ottenere il controllo completo dello schema del database, ereditare da una delle identità disponibili `DbContext` classi e configurare il contesto per includere lo schema di identità chiamando `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` nel `OnModelCreating` (metodo).

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController

Nel *Controllers\OidcConfigurationController.cs* file, si noti che l'endpoint che viene eseguito il provisioning per servire i parametri OIDC che il client deve utilizzare.

### <a name="appsettingsjson"></a>appsettings.json

Nel *appSettings. JSON* file principale del progetto, c'è un nuovo `IdentityServer` sezione che descrive l'elenco di configurata i client. Nell'esempio seguente, è disponibile un singolo client. Il nome del client corrisponde al nome dell'app e viene eseguito il mapping per convenzione per OAuth `ClientId` parametro. Il profilo indica il tipo di app da configurare. Viene usato internamente per le convenzioni di unità che semplificano il processo di configurazione per il server. Esistono diversi profili disponibili, come spiegato nel [profili applicazione](#application-profiles) sezione.

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a>appsettings.Development.json

Nel *appsettings. Development.JSON* file principale del progetto, è presente un `IdentityServer` sezione che descrive la chiave usata per firmare i token. Durante la distribuzione nell'ambiente di produzione, una chiave deve essere eseguito il provisioning e distribuire insieme all'app, come spiegato nel [Distribuisci in produzione](#deploy-to-production) sezione.

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a>Descrizione generale dell'app Angular

L'autenticazione e l'autorizzazione API supportano in Angular modello si trova in un proprio modulo Angular nel *ClientApp\src\api-authorization* directory. Il modulo è costituito dagli elementi seguenti:

* 3 componenti:
  * *login.component.ts*: Gestisce il flusso di accesso dell'app.
  * *logout.component.ts*: Gestisce il flusso di disconnessione dell'app.
  * *login-menu.component.ts*: Un widget che visualizza uno dei set di collegamenti seguenti:
    * Gestione dei profili utente e la disconnessione collegamenti quando l'utente è autenticato.
    * Registrazione e log nei collegamenti quando l'utente non è autenticato.
* Una clausola guard route `AuthorizeGuard` che possono essere aggiunti alla route e richiede all'utente di essere autenticati prima di visitare la route.
* Un intercettore HTTP `AuthorizeInterceptor` che associa il token di accesso alle richieste HTTP in uscita come destinazione le API quando l'utente è autenticato.
* Un servizio `AuthorizeService` che gestisce i dettagli di basso livello del processo di autenticazione ed espone le informazioni relative all'utente autenticato al resto dell'app per l'utilizzo.
* Un modulo Angular che definisce le route associate alle parti di autenticazione dell'app. Espone il componente di menu account di accesso, l'intercettore, la protezione e il servizio per l'utilizzo dal resto dell'app.

## <a name="general-description-of-the-react-app"></a>Descrizione generale dell'app React

Il supporto per l'autenticazione e autorizzazione di API del modello di React si trova nel *ClientApp\src\components\api-authorization* directory. Si è costituito dagli elementi seguenti:

* 4 componenti:
  * *Login.js*: Gestisce il flusso di accesso dell'app.
  * *Logout.js*: Gestisce il flusso di disconnessione dell'app.
  * *LoginMenu.js*: Un widget che visualizza uno dei set di collegamenti seguenti:
    * Gestione dei profili utente e la disconnessione collegamenti quando l'utente è autenticato.
    * Registrazione e log nei collegamenti quando l'utente non è autenticato.
  * *AuthorizeRoute.js*: Un componente di route che richiede all'utente venga autenticato prima del componente di rendering indicate nel `Component` parametro.
* Una raccolta esportat `authService` istanza della classe `AuthorizeService` che gestisce i dettagli di basso livello del processo di autenticazione ed espone le informazioni relative all'utente autenticato al resto dell'app per l'utilizzo.

Ora che si sono visto i componenti principali della soluzione, è possibile esaminare più profondo singoli scenari per l'app.

## <a name="require-authorization-on-a-new-api"></a>Richiedere l'autorizzazione in una nuova API

Per impostazione predefinita, il sistema è configurato per richiedere facilmente l'autorizzazione per le nuove API. A tale scopo, creare un nuovo controller e aggiungere il `[Authorize]` attributo alla classe controller o a qualsiasi azione all'interno del controller.

## <a name="protect-a-client-side-route-angular"></a>Proteggere una route lato client (Angular)

La protezione di una route lato client viene eseguita aggiungendo la clausola guard authorize all'elenco di protezioni per l'esecuzione quando si configura una route. Ad esempio, è possibile vedere come il `fetch-data` route è stata configurata all'interno del modulo Angular app principale:

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

È importante ricordare che la protezione di una route non protegge l'endpoint effettivo (che richiede comunque un `[Authorize]` applicato l'attributo), ma che solo impedisce all'utente passando alla route del lato client specificata quando la si non è autenticato.

## <a name="authenticate-api-requests-angular"></a>Autenticare le richieste di API (Angular)

L'autenticazione delle richieste alle API ospitate insieme con l'app viene eseguita automaticamente tramite l'intercettore di client HTTP definito dall'app.

## <a name="protect-a-client-side-route-react"></a>Proteggere una route lato client (React)

Proteggere una route lato client usando il `AuthorizeRoute` componente anziché il normale `Route` componente. Ad esempio, si noti come il `fetch-data` route è stata configurata all'interno di `App` componente:

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

La protezione di una route:

* Non protegge l'endpoint effettivo (che richiede comunque un `[Authorize]` applicato l'attributo).
* Impedisce solo l'utente lo spostamento alla route del lato client specificata quando la si non è autenticato.

## <a name="authenticate-api-requests-react"></a>Autenticare le richieste di API (React)

L'autenticazione delle richieste con React viene eseguita importando il `authService` dell'istanza dal `AuthorizeService`. Il token di accesso viene recuperato dal `authService` e viene associato alla richiesta come illustrato di seguito. Nei componenti di React, questa operazione avviene in genere `componentDidMount` metodo del ciclo di vita o come risultato di alcune interazioni dell'utente.

### <a name="import-the-authservice-into-your-component"></a>Importare il authService nel componente

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>Recuperare e allegare il token di accesso alla risposta

```javascript
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-to-production"></a>Distribuire nell'ambiente di produzione

Per distribuire l'app nell'ambiente di produzione, è necessario eseguire il provisioning le risorse seguenti:

* Un database per archiviare gli account utente di identità e i privilegi concessi IdentityServer.
* Un certificato di produzione da usare per firmare i token.
  * Non sono previsti requisiti specifici per questo certificato. può trattarsi di un certificato autofirmato o un certificato di provisioning tramite un'autorità di certificazione.
  * Può essere generato tramite gli strumenti standard, ad esempio PowerShell oppure OpenSSL.
  * Può essere installato nell'archivio certificati nei computer di destinazione o distribuito come un *PFX* file con una password complessa.

### <a name="example-deploy-to-azure-websites"></a>Esempio: Distribuire siti Web di Azure

Questa sezione descrive la distribuzione dell'app per siti Web di Azure usando un certificato archiviato nell'archivio certificati. Per modificare l'app per caricare un certificato dall'archivio certificati, il piano di servizio App deve essere in esecuzione almeno il livello Standard quando si configurano in un passaggio successivo. Dell'app *appSettings. JSON* del file, modificare il `IdentityServer` sezione per includere i dettagli chiave:

```json
"IdentityServer": {
  "Key": {
    "Type": "Store",
    "StoreName": "My",
    "StoreLocation": "CurrentUser",
    "Name": "CN=MyApplication"
  }
}
```

* La proprietà nome sul certificato corrispondente con l'oggetto distinto per il certificato.
* Il percorso dell'archivio rappresenta la posizione in cui caricare il certificato da (`CurrentUser` o `LocalMachine`).
* Il nome dell'archivio rappresenta il nome dell'archivio certificati in cui è archiviato il certificato. In questo caso, fa riferimento all'archivio dell'utente personale.

Per distribuire siti Web di Azure, distribuire l'app seguendo la procedura descritta in [distribuire l'app in Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) per creare le risorse di Azure necessarie e distribuire l'app nell'ambiente di produzione.

Dopo aver seguito le istruzioni precedenti, l'app viene distribuita in Azure ma non è ancora attiva. Il certificato usato dall'app deve comunque essere configurati. Individuare l'identificazione personale del certificato da usare e seguire i passaggi descritti [caricare i certificati](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).

Mentre questi passaggi menzionare SSL, non vi è una **certificati privati** sezione nel portale in cui è possibile caricare il certificato di provisioning da usare con l'app.

Dopo questo passaggio, riavviare l'app e deve essere funzionale.

## <a name="other-configuration-options"></a>Altre opzioni di configurazione

Il supporto per l'autorizzazione API si basa IdentityServer con un set di convenzioni, i valori predefiniti e miglioramenti per semplificare l'esperienza per SPAs. Inutile a dirsi, la potenza completa di IdentityServer è disponibile dietro le quinte se integrazioni di ASP.NET Core non coprono dello scenario. Il supporto di ASP.NET Core è incentrato sulle app "cookie", in cui tutte le app vengono create e distribuite dalla nostra organizzazione. Di conseguenza, il supporto non è disponibile per operazioni come il consenso o la federazione. Per tali scenari, usare IdentityServer e seguire la relativa documentazione.

### <a name="application-profiles"></a>Profili applicazione

I profili dell'applicazione sono le configurazioni predefinite per le app che definiscono ulteriormente i relativi parametri. A questo punto, sono supportati i profili seguenti:

* `IdentityServerSPA`: Rappresenta un'applicazione a singola pagina ospitato insieme con IdentityServer come singola unità.
  * Il `redirect_uri` per impostazione predefinita `/authentication/login-callback`.
  * Il `post_logout_redirect_uri` per impostazione predefinita `/authentication/logout-callback`.
  * Il set di ambiti include il `openid`, `profile`e tutti gli ambiti definiti per le API nell'app.
  * È il set di tipi di risposta consentiti OIDC `id_token token` o ognuno di essi singolarmente (`id_token`, `token`).
  * La modalità di risposta consentite `fragment`.
* `SPA`: Rappresenta un'applicazione a singola pagina che non è ospitato con IdentityServer.
  * Il set di ambiti include il `openid`, `profile`e tutti gli ambiti definiti per le API nell'app.
  * È il set di tipi di risposta consentiti OIDC `id_token token` o ognuno di essi singolarmente (`id_token`, `token`).
  * La modalità di risposta consentite `fragment`.
* `IdentityServerJwt`: Rappresenta un'API che è ospitata insieme con IdentityServer.
  * L'app è configurata per avere un ambito singolo valore predefinito è il nome dell'app.
* `API`: Rappresenta un'API che non è ospitata con IdentityServer.
  * L'app è configurata per avere un ambito singolo valore predefinito è il nome dell'app.

### <a name="configuration-through-appsettings"></a>Configurazione tramite AppSettings

Configurare le app tramite il sistema di configurazione aggiungendoli all'elenco degli `Clients` o `Resources`.

Configurare ogni client `redirect_uri` e `post_logout_redirect_uri` proprietà, come illustrato nell'esempio seguente:

```json
"IdentityServer": {
  "Clients": {
    "MySPA": {
      "Profile": "SPA",
      "RedirectUri": "https://www.example.com/authentication/login-callback",
      "LogoutUri": "https://www.example.com/authentication/logout-callback"
    }
  }
}
```

Quando si configurano le risorse, è possibile configurare gli ambiti per la risorsa, come illustrato di seguito:

```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c"
    }
  }
}
```

### <a name="configuration-through-code"></a>Configurazione tramite codice

È anche possibile configurare i client e le risorse mediante il codice che usa un overload di `AddApiAuthorization` che accetta un'azione per configurare le opzioni.

```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA", spa =>
        spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
           .WithLogoutRedirectUri(
               "http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource =>
        resource.WithScopes("a", "b", "c"));
});
```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:spa/angular>
* <xref:spa/react>
