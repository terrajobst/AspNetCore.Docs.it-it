---
title: Introduzione all'autenticazione per applicazioni a pagina singola in ASP.NET Core
author: ''
description: Usare l'identità con un'applicazione a singola pagina ospitata all'interno di un'app ASP.NET Core.
ms.author: ''
ms.date: 03/05/2018
uid: security/authentication/identity/spa
ms.openlocfilehash: cf04ec1ff0ae9afea066fd1864ab0a7956ace32c
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346752"
---
# <a name="authentication-and-authorization-for-spas"></a>Autenticazione e autorizzazione per SPAs

Nella versione 3.0 di ASP.NET viene introdotto il supporto per l'autenticazione in applicazioni a pagina singola con il nuovo supporto per l'autorizzazione API. Questo supporto è basato su una combinazione di ASP.NET Core Identity per l'autenticazione e l'archiviazione agli utenti e il Server di identità per l'implementazione di Openid Connect.

È stato aggiunto un nuovo parametro di autenticazione al nostro Angular e React modelli che è simile al parametro di autenticazione nei nostri modelli di pagine mvc e razor con consentiti i valori 'None' e 'Individuale'.

## <a name="create-an-angular-app-with-api-authorization-support"></a>Creare un'app Angular con supporto per l'autorizzazione API

Per creare una nuova app Angular con supporto per l'autenticazione e autorizzazione degli utenti, aprire una shell dei comandi ed eseguire il comando seguente:

```console
dotnet new angular -o <output_directory_name> -au Individual
```

Il comando precedente crea un'app ASP.NET Core con un *ClientApp* directory contenente l'app Angular.

## <a name="create-a-react-app-with-api-authorization-support"></a>Creare un'app React con supporto per l'autorizzazione API

Per creare una nuova app React con supporto per l'autenticazione e autorizzazione degli utenti, aprire una shell dei comandi ed eseguire il comando seguente:

```console
dotnet new react -o <output_directory_name> -au Individual
```

Il comando precedente crea un'app ASP.NET Core con un *ClientApp* directory contenente l'app React.

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>Descrizione generale dei componenti dell'app ASP.NET Core

Sono disponibili diverse funzionalità per il progetto è incluso il supporto per l'autenticazione:

### <a name="startup-class"></a>Classe di avvio

Se si esamina il codice nella classe Startup seguente apprezziamo possiamo le inclusioni seguenti:
* All'interno di `public void ConfigureServices(IServiceCollection services)`:
  * Identità con l'interfaccia utente predefinita.
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * Identity Server con un altro metodo helper AddApiAuthorization tale configurazioni alcuni predefinito le convenzioni di ASP.NET in Identity Server.
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * Autenticazione con un metodo helper AddIdentityServerJwt aggiuntivo che consente di configurare l'applicazione per convalidare i token Jwt generati da Identity Server. 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* All'interno di `public void Configure(IApplicationBuilder app)`:
  * Il middleware di autenticazione che è responsabile per la convalida delle credenziali nella richiesta in ingresso e impostando l'utente per il contesto della richiesta.
    ```csharp
    app.UseAuthentication();
    ```
  * Il middleware di server di identità che espone gli endpoint Openid Connect.
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization 
Questo metodo di supporto consente di configurare il Server di identità per usare la configurazione supportata. Server di identità è un framework molto potente ed estendibile per la gestione di problemi di sicurezza dell'applicazione, ma allo stesso tempo che espone molte complessità che non è necessario conoscere per gli scenari più comuni, quindi si sceglie un set di convenzioni e opzioni di configurazione per l'utente sono considerate sono un buon punto di partenza. Dopo l'autenticazione delle esigenze tutta la potenza di Identity Server è ancora disponibile per l'utente in modo che è possibile personalizzarlo in base alle esigenze.

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt
Questo metodo di supporto consente di configurare una combinazione di criteri per l'applicazione come il gestore di autenticazione predefinito. Il criterio è configurato per consentire a gestire tutte le richieste che vanno da qualsiasi percorso secondario nel campo URL di identità dell'identità "/ identità" e che il JwtBearerHandler gestisca tutte le altre richieste.
Questo metodo registra Addionally un' `<<ApplicationName>>API` della risorsa Api con il server di identità con un ambito predefinito di `<<ApplicationName>>API` e configura il middleware del token Bearer token JWT per convalidare i token rilasciati dal Server di identità per l'applicazione.

### <a name="sampledatacontroller"></a>SampleDataController
Se si esamina il file Controllers\SampleDataController.cs è possibile osservare la `[Authorize]` attributo applicato alla classe che indica che l'utente deve essere autorizzato in base i criteri predefiniti per accedere alla risorsa. I criteri di autorizzazione predefinita accade per essere configurato per usare lo schema di autenticazione predefinito che viene impostato in modo da `AddIdentityServerJwt` rispetto allo schema dei criteri accennato in precedenza, rendendo il gestore JwtBearer configurate da tale metodo di supporto del gestore predefinito per richieste all'app.

### <a name="applicationdbcontext"></a>ApplicationDbContext
Se si esamina il file in Data\ApplicationDbContext.cs noteremo stesso DbContext usiamo in identità con l'eccezione che estende ApiAuthorizationDbContext (una classe più derivata da IdentityDbContext) da includere lo schema per il Server di identità.
Si vuole il controllo completo dello schema del database ora è sufficiente ereditare da una delle classi di DbContext di identità disponibili, per configurare il contesto per includere lo schema di identità tramite la chiamata `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` nella `OnModelCreating` (metodo).

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController
Se si esamina il file Controllers\OidcConfigurationController.cs possiamo vedere l'endpoint che sono rapide giornaliere per servire i parametri OIDC che il client deve utilizzare.

### <a name="appsettingsjson"></a>appsettings.json
Se si esamina il file appSettings. JSON nella radice del progetto, si noterà un nuovo `IdentityServer` sezione che descrive l'elenco di configurata i client e possiamo vedere che vi sia un singolo client. Il nome del client corrisponde al nome dell'applicazione e viene eseguito il mapping per convenzione per il parametro ID client oAuth. Il profilo indica il tipo di applicazione viene configurato e viene usata internamente per le convenzioni di unità che semplificano il processo di configurazione per il server. Esistono diversi profili descritti nella sezione seguente.

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
Se osserviamo appsettings. Development.JSON file nella radice del progetto, si noterà un nuovo `IdentityServer` sezione che descrive la chiave viene usata per firmare i token. Durante la distribuzione nell'ambiente di produzione una chiave deve essere eseguito il provisioning e distribuire insieme all'applicazione, come illustrato di seguito.

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a>Descrizione generale dell'applicazione Angular
Il supporto per l'autenticazione e autorizzazione di API del modello Angular si trova in un proprio modulo Angular. Nella sezione ClientApp\src\api-authorization e lo è composto da degli elementi seguenti:
* 3 componenti:
  * Componente di accesso: Gestisce il flusso di accesso per l'app.
  * Componenti di disconnessione: Gestisce il flusso di disconnessione per l'app.
  * Componente di menu account di accesso: Un widget che visualizza l'utente autenticato corrente con collegamenti per gestire il profilo utente e la disconnessione o i collegamenti per accedere o registrare quando l'utente non è autenticata.
* Una clausola guard route `AuthorizeGuard` che possono essere aggiunti alla route e richiede all'utente di essere autenticati prima di visitare la route.
* Un intercettore http `AuthorizeInterceptor` che associa il token di accesso alle richieste HTTP in uscita come destinazione le API quando l'utente è autenticato.
* Un servizio `AuthorizeService` che gestisce i dettagli del livello inferiore del processo di autenticazione ed espone le informazioni relative all'utente autenticato al resto dell'applicazione per l'utilizzo.
* Un modulo angular che definisce le route associate alle parti di autenticazione dell'applicazione e ne espone il componente di menu account di accesso, l'intercettore, la protezione e il servizio per l'utilizzo dal resto dell'applicazione.

## <a name="general-description-of-the-react-application"></a>Descrizione generale dell'applicazione React
Il supporto per l'autenticazione e l'autorizzazione API nella vita di modello di React sotto ClientApp\src\components\api authorization\ e è costituito dagli elementi seguenti:
* 4 componenti:
  * Componente di accesso: Gestisce il flusso di accesso per l'app.
  * Componenti di disconnessione: Gestisce il flusso di disconnessione per l'app.
  * Componente di menu account di accesso: Un widget che visualizza l'utente autenticato corrente con collegamenti per gestire il profilo utente e la disconnessione o i collegamenti per accedere o registrare quando l'utente non è autenticata.
  * AuthorizeRoute: Un componente di route che richiede all'utente venga autenticato prima del componente indicato nel parametro del componente di rendering.
  * Una raccolta esportat `authService` istanza della classe `AuthorizeService` che gestisce i dettagli del livello inferiore del processo di autenticazione ed espone le informazioni relative all'utente autenticato al resto dell'applicazione per l'utilizzo.

Ora che abbiamo visto i componenti principali della soluzione, è possibile esaminare specifico singoli scenari per l'applicazione:

## <a name="requiring-authorization-on-a-new-api"></a>Richiesta di autorizzazione in una nuova API
Il sistema è configurato predefiniti per renderlo semplice per richiedere l'autorizzazione per nuove API. A questo scopo, è sufficiente creare un nuovo controller e aggiungere il `[Authorize]` attributo alla classe controller o a qualsiasi azione all'interno del controller.

## <a name="protecting-a-client-side-route-angular"></a>La protezione di una route lato client (Angular)
La protezione di una route sul lato client viene eseguita aggiungendo la clausola guard authorize all'elenco di protezioni per l'esecuzione quando si configura una route. Ad esempio è possibile vedere come viene configurata la route di recuperare dati all'interno del modulo angular app principale:

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

È importante ricordare che la protezione di una route non protegge l'endpoint effettivo (che richiede comunque un `[Authorize]` applicato l'attributo), ma che solo impedisce all'utente passando alla route del lato client specifico quando non è autenticato.

## <a name="authenticate-api-requests-angular"></a>Autenticare le richieste di API (Angular)

Autenticazione delle richieste alle API ospitate sul lato che dell'applicazione viene eseguita automaticamente tramite l'intercettore di client HTTP definito dall'applicazione.

## <a name="protect-a-client-side-route-react"></a>Proteggere una route lato client (React)

La protezione di una route sul lato client viene eseguita tramite il componente AuthorizeRoute anziché il componente Route semplice. Ad esempio è possibile vedere come viene configurata la route di recuperare dati all'interno del componente di App:

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

È importante ricordare che la protezione di una route non protegge l'endpoint effettivo (che richiede comunque un `[Authorize]` applicato l'attributo), ma che solo impedisce all'utente passando alla route del lato client specifico quando non è autenticato.

## <a name="authenticate-api-requests-react"></a>Autenticare le richieste di API (React)

L'autenticazione delle richieste con react viene eseguita importando il `authService` dell'istanza dal `AuthorizeService` quindi recuperando il token di accesso dal authService e collegarlo alla richiesta, come illustrato di seguito. Nei componenti di react ciò avviene in genere nel metodo del ciclo di vita componentDidMount o come risultato dall'interazione dell'utente.

### <a name="import-the-authservice-into-your-component"></a>Importare il authService nel componente

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>Recuperare e allegare il token di accesso alla risposta

```js
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-into-production"></a>Distribuire in produzione

Per distribuire l'applicazione nell'ambiente di produzione è necessario effettuare il provisioning delle risorse:
* Concede a un database per archiviare gli account utente di identità e il server di identità.
* Un certificato di produzione da usare per firmare i token.
  * Non sono previsti requisiti specifici per questo certificato. può trattarsi di un certificato autofirmato o un certificato di provisioning tramite un'autorità di certificazione.
  * Può essere generato tramite gli strumenti standard, ad esempio powershell oppure openssl.
  * Può essere installato nell'archivio certificati nei computer di destinazione o distribuito come file pfx con una password complessa.

### <a name="example-deploy-into-azure-websites"></a>Esempio: Distribuzione in siti Web di Azure

In questa sezione si intende distribuire l'applicazione in siti Web di Azure usando un certificato archiviato nell'archivio certificati. È necessario modificare l'applicazione per caricare un certificato dall'archivio certificati. A tale scopo, il piano di servizio app deve essere presente almeno nel livello standard quando si configurano in un passaggio successivo. Nell'applicazione è sufficiente modificare la sezione IdentityServer in appSettings. JSON per includere i dettagli chiave:
```json
  "IdentityServer": {
    "Key": {
      "Type": "Store",
      "StoreName": "My",
      "StoreLocation": "CurrentUser",
      "Name": "CN=MyApplication"
    }
  }
}
```
* La proprietà nome sul certificato corrispondente con l'oggetto distinto per il certificato.
* Il percorso dell'archivio rappresenta la posizione in cui caricare il certificato da (CurrentUrser o LocalMachine).
* Il nome dell'archivio rappresenta il nome dell'archivio certificati in cui è archiviato il certificato, in questo caso che punta all'archivio utente personali.

Per distribuire siti Web di Azure, distribuire l'app seguendo la procedura descritta in [distribuire l'app in Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) per creare le risorse di Azure necessarie e distribuire l'app nell'ambiente di produzione.

Dopo questa operazione, l'applicazione viene distribuita in Azure ma non è ancora completamente funzionale quanto occorre comunque configurare il certificato da usare per l'applicazione. A tale scopo, è necessario disporre dell'identificazione personale del certificato che si intende usare e seguire i passaggi descritti in [caricare i certificati](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).

Quando questi passaggi cita SSL, è disponibile una sezione "Certificati privati" nel portale in cui è possibile caricare il certificato con provisioning da usare con l'App nell'app.

Dopo questo passaggio, dovrebbe essere possibile riavviare l'applicazione e deve essere completamente funzionale.

## <a name="other-configuration-options"></a>Altre opzioni di configurazione
Il supporto per l'autorizzazione API si basa Identity Server con un set di convenzioni, i valori predefiniti e i miglioramenti per semplificare l'esperienza per le applicazioni a pagina singola. Inutile a dirsi, tutta la potenza di Identity Server è disponibile dietro le quinte se le integrazioni che offriamo non coprono dello scenario. Il supporto è incentrato su quelle che chiamiamo "proprietaria" applicazioni, in cui tutte le applicazioni vengono create e distribuite da dell'organizzazione. Di conseguenza non offriamo il supporto per elementi quali il consenso o la federazione. Per questi scenari il Consiglio è usare identità del Server e seguire la relativa documentazione.

### <a name="application-profiles"></a>Profili applicazione
I profili dell'applicazione sono le configurazioni predefinite per le applicazioni che definiscono ulteriormente i relativi parametri. In questo momento sono supportati due profili:
* IdentityServerSPA: Rappresenta un'applicazione a singola pagina ospitata insieme a Identity Server come singola unità.
  * Per impostazione predefinita al valore di redirect_uri `/authentication/login-callback`.
  * Per impostazione predefinita il post_logout_redirect_uri `/authentication/logout-callback`.
  * Il set di ambiti include il `openid`, `profile`e tutti gli ambiti definiti per le API nell'applicazione.
  * È il set di tipi di risposta consentiti OIDC `id_token token` o ognuno di essi singolarmente (`id_token`, `token`).
  * La modalità di risposta consentite `fragment`.
* SPA: Rappresenta un'applicazione a pagina singola che non è ospitata in Identity Server.
  * Il set di ambiti include il `openid`, `profile`e tutti gli ambiti definiti per le API nell'applicazione.
  * È il set di tipi di risposta consentiti OIDC `id_token token` o ognuno di essi singolarmente (`id_token`, `token`).
  * La modalità di risposta consentite `fragment`.
* IdentityServerJwt: Rappresenta un'API che è ospitata insieme con Identity Server.
  * L'applicazione è configurata per utilizzare un ambito singolo che viene impostato sul nome dell'applicazione.
* API: Rappresenta un'API che non è ospitata con Server di identità.
  * L'applicazione è configurata per utilizzare un ambito singolo che viene impostato sul nome dell'applicazione.

### <a name="configuration-through-appsettings"></a>Configurazione tramite AppSettings
È possibile configurare le applicazioni tramite il nostro sistema di configurazione aggiungendoli rispettivamente all'elenco di client o risorse. 

Quando si configurano i client è possibile configurare il `redirect_uri` e il `post_logout_redirect_uri` come illustrato di seguito:
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

Quando si configurano le risorse è possibile configurare gli ambiti per la risorsa, come illustrato di seguito:
```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c",
    }
  }
}
```

### <a name="configuration-through-code"></a>Configurazione tramite codice
Possiamo anche configurare i client e le risorse mediante il codice che usa un overload di AddApiAuthorization che accetta un'azione per configurare le opzioni.
```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA",
        spa => spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
            .WithLogoutRedirectUri("http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource => resource.WithScopes("a", "b", "c"));
});
```
