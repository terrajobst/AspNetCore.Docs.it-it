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
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="26f25-103">Autenticazione e autorizzazione per SPAs</span><span class="sxs-lookup"><span data-stu-id="26f25-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="26f25-104">Nella versione 3.0 di ASP.NET viene introdotto il supporto per l'autenticazione in applicazioni a pagina singola con il nuovo supporto per l'autorizzazione API.</span><span class="sxs-lookup"><span data-stu-id="26f25-104">In ASP.NET 3.0 we are introducing support for authentication in single page applications using our new support for API authorization.</span></span> <span data-ttu-id="26f25-105">Questo supporto è basato su una combinazione di ASP.NET Core Identity per l'autenticazione e l'archiviazione agli utenti e il Server di identità per l'implementazione di Openid Connect.</span><span class="sxs-lookup"><span data-stu-id="26f25-105">This support is based on a combination of ASP.NET Core Identity for authenticating and storing users and Identity Server for implementing Open ID Connect.</span></span>

<span data-ttu-id="26f25-106">È stato aggiunto un nuovo parametro di autenticazione al nostro Angular e React modelli che è simile al parametro di autenticazione nei nostri modelli di pagine mvc e razor con consentiti i valori 'None' e 'Individuale'.</span><span class="sxs-lookup"><span data-stu-id="26f25-106">We have added a new authentication parameter to our Angular and React templates that is similar to the authentication parameter in our mvc and razor pages templates with allowed values 'None' and 'Individual'.</span></span>

## <a name="create-an-angular-app-with-api-authorization-support"></a><span data-ttu-id="26f25-107">Creare un'app Angular con supporto per l'autorizzazione API</span><span class="sxs-lookup"><span data-stu-id="26f25-107">Create an Angular app with API authorization support</span></span>

<span data-ttu-id="26f25-108">Per creare una nuova app Angular con supporto per l'autenticazione e autorizzazione degli utenti, aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="26f25-108">To create a new Angular app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="26f25-109">Il comando precedente crea un'app ASP.NET Core con un *ClientApp* directory contenente l'app Angular.</span><span class="sxs-lookup"><span data-stu-id="26f25-109">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the Angular app.</span></span>

## <a name="create-a-react-app-with-api-authorization-support"></a><span data-ttu-id="26f25-110">Creare un'app React con supporto per l'autorizzazione API</span><span class="sxs-lookup"><span data-stu-id="26f25-110">Create a React app with API authorization support</span></span>

<span data-ttu-id="26f25-111">Per creare una nuova app React con supporto per l'autenticazione e autorizzazione degli utenti, aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="26f25-111">To create a new React app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="26f25-112">Il comando precedente crea un'app ASP.NET Core con un *ClientApp* directory contenente l'app React.</span><span class="sxs-lookup"><span data-stu-id="26f25-112">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the React app.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="26f25-113">Descrizione generale dei componenti dell'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26f25-113">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="26f25-114">Sono disponibili diverse funzionalità per il progetto è incluso il supporto per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="26f25-114">There are several additions to the project when we include support for authentication:</span></span>

### <a name="startup-class"></a><span data-ttu-id="26f25-115">Classe di avvio</span><span class="sxs-lookup"><span data-stu-id="26f25-115">Startup class</span></span>

<span data-ttu-id="26f25-116">Se si esamina il codice nella classe Startup seguente apprezziamo possiamo le inclusioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="26f25-116">If we look at the code in the Startup class below we can appreciate the following inclusions:</span></span>
* <span data-ttu-id="26f25-117">All'interno di `public void ConfigureServices(IServiceCollection services)`:</span><span class="sxs-lookup"><span data-stu-id="26f25-117">Inside `public void ConfigureServices(IServiceCollection services)`:</span></span>
  * <span data-ttu-id="26f25-118">Identità con l'interfaccia utente predefinita.</span><span class="sxs-lookup"><span data-stu-id="26f25-118">Identity with the default UI.</span></span>
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * <span data-ttu-id="26f25-119">Identity Server con un altro metodo helper AddApiAuthorization tale configurazioni alcuni predefinito le convenzioni di ASP.NET in Identity Server.</span><span class="sxs-lookup"><span data-stu-id="26f25-119">Identity Server with an additional AddApiAuthorization helper method that setups some default ASP.NET Conventions on top of Identity Server.</span></span>
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * <span data-ttu-id="26f25-120">Autenticazione con un metodo helper AddIdentityServerJwt aggiuntivo che consente di configurare l'applicazione per convalidare i token Jwt generati da Identity Server.</span><span class="sxs-lookup"><span data-stu-id="26f25-120">Authentication with an additional AddIdentityServerJwt helper method that configures the application to validate Jwt tokens produced by Identity Server.</span></span> 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* <span data-ttu-id="26f25-121">All'interno di `public void Configure(IApplicationBuilder app)`:</span><span class="sxs-lookup"><span data-stu-id="26f25-121">Inside `public void Configure(IApplicationBuilder app)`:</span></span>
  * <span data-ttu-id="26f25-122">Il middleware di autenticazione che è responsabile per la convalida delle credenziali nella richiesta in ingresso e impostando l'utente per il contesto della richiesta.</span><span class="sxs-lookup"><span data-stu-id="26f25-122">The authentication middleware that is responsible for validating the credentials in the incoming request and setting the user on the request context.</span></span>
    ```csharp
    app.UseAuthentication();
    ```
  * <span data-ttu-id="26f25-123">Il middleware di server di identità che espone gli endpoint Openid Connect.</span><span class="sxs-lookup"><span data-stu-id="26f25-123">The identity server middleware that exposes the Open ID Connect endpoints.</span></span>
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="26f25-124">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="26f25-124">AddApiAuthorization</span></span> 
<span data-ttu-id="26f25-125">Questo metodo di supporto consente di configurare il Server di identità per usare la configurazione supportata.</span><span class="sxs-lookup"><span data-stu-id="26f25-125">This helper method configures Identity Server to use our supported configuration.</span></span> <span data-ttu-id="26f25-126">Server di identità è un framework molto potente ed estendibile per la gestione di problemi di sicurezza dell'applicazione, ma allo stesso tempo che espone molte complessità che non è necessario conoscere per gli scenari più comuni, quindi si sceglie un set di convenzioni e opzioni di configurazione per l'utente sono considerate sono un buon punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="26f25-126">Identity Server is a very powerful and extensible framework for handling application security concerns but at the same time that exposes a lot of complexity that we don't need to know about for the most common scenarios, so we choose a set of conventions and configuration options for you that we consider are a good starting point.</span></span> <span data-ttu-id="26f25-127">Dopo l'autenticazione delle esigenze tutta la potenza di Identity Server è ancora disponibile per l'utente in modo che è possibile personalizzarlo in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="26f25-127">Once your authentication needs change the full power of Identity Server is still available to you so you can customize it to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="26f25-128">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="26f25-128">AddIdentityServerJwt</span></span>
<span data-ttu-id="26f25-129">Questo metodo di supporto consente di configurare una combinazione di criteri per l'applicazione come il gestore di autenticazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="26f25-129">This helper method configures a policy scheme for the application as the default authentication handler.</span></span> <span data-ttu-id="26f25-130">Il criterio è configurato per consentire a gestire tutte le richieste che vanno da qualsiasi percorso secondario nel campo URL di identità dell'identità "/ identità" e che il JwtBearerHandler gestisca tutte le altre richieste.</span><span class="sxs-lookup"><span data-stu-id="26f25-130">The policy is configured to let identity handle all the requests that go to any subpath in the Identity url space "/Identity" and to let the JwtBearerHandler handle all other requests.</span></span>
<span data-ttu-id="26f25-131">Questo metodo registra Addionally un' `<<ApplicationName>>API` della risorsa Api con il server di identità con un ambito predefinito di `<<ApplicationName>>API` e configura il middleware del token Bearer token JWT per convalidare i token rilasciati dal Server di identità per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="26f25-131">Addionally this method registers an `<<ApplicationName>>API` Api resource with identity server with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by Identity Server for the application.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="26f25-132">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="26f25-132">SampleDataController</span></span>
<span data-ttu-id="26f25-133">Se si esamina il file Controllers\SampleDataController.cs è possibile osservare la `[Authorize]` attributo applicato alla classe che indica che l'utente deve essere autorizzato in base i criteri predefiniti per accedere alla risorsa.</span><span class="sxs-lookup"><span data-stu-id="26f25-133">If we look at the file Controllers\SampleDataController.cs we can observe the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="26f25-134">I criteri di autorizzazione predefinita accade per essere configurato per usare lo schema di autenticazione predefinito che viene impostato in modo da `AddIdentityServerJwt` rispetto allo schema dei criteri accennato in precedenza, rendendo il gestore JwtBearer configurate da tale metodo di supporto del gestore predefinito per richieste all'app.</span><span class="sxs-lookup"><span data-stu-id="26f25-134">The default authorization policy happens to be configured to use the default authentication scheme which is set up by `AddIdentityServerJwt` to the policy scheme that we mentioned above, making the JwtBearer handler configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="26f25-135">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="26f25-135">ApplicationDbContext</span></span>
<span data-ttu-id="26f25-136">Se si esamina il file in Data\ApplicationDbContext.cs noteremo stesso DbContext usiamo in identità con l'eccezione che estende ApiAuthorizationDbContext (una classe più derivata da IdentityDbContext) da includere lo schema per il Server di identità.</span><span class="sxs-lookup"><span data-stu-id="26f25-136">If we look at the file in Data\ApplicationDbContext.cs we can see the same DbContext we use in identity with the exception that it extends ApiAuthorizationDbContext (a more derived class from IdentityDbContext) to include the schema for Identity Server.</span></span>
<span data-ttu-id="26f25-137">Si vuole il controllo completo dello schema del database ora è sufficiente ereditare da una delle classi di DbContext di identità disponibili, per configurare il contesto per includere lo schema di identità tramite la chiamata `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` nella `OnModelCreating` (metodo).</span><span class="sxs-lookup"><span data-stu-id="26f25-137">If we want full control of the database schema we can simply inherit from one of the available Identity DbContext classes and configure the context to include the identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="26f25-138">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="26f25-138">OidcConfigurationController</span></span>
<span data-ttu-id="26f25-139">Se si esamina il file Controllers\OidcConfigurationController.cs possiamo vedere l'endpoint che sono rapide giornaliere per servire i parametri OIDC che il client deve utilizzare.</span><span class="sxs-lookup"><span data-stu-id="26f25-139">If we look at the file Controllers\OidcConfigurationController.cs we can see the endpoint that we stand-up to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="26f25-140">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="26f25-140">appsettings.json</span></span>
<span data-ttu-id="26f25-141">Se si esamina il file appSettings. JSON nella radice del progetto, si noterà un nuovo `IdentityServer` sezione che descrive l'elenco di configurata i client e possiamo vedere che vi sia un singolo client.</span><span class="sxs-lookup"><span data-stu-id="26f25-141">If we look at the appsettings.json file on the root of the project, we can see a new `IdentityServer` section that describes the list of configured clients and we can see that there is a single client.</span></span> <span data-ttu-id="26f25-142">Il nome del client corrisponde al nome dell'applicazione e viene eseguito il mapping per convenzione per il parametro ID client oAuth.</span><span class="sxs-lookup"><span data-stu-id="26f25-142">The name of the client corresponds to the name of the application and is mapped by convention to the oAuth ClientId parameter.</span></span> <span data-ttu-id="26f25-143">Il profilo indica il tipo di applicazione viene configurato e viene usata internamente per le convenzioni di unità che semplificano il processo di configurazione per il server.</span><span class="sxs-lookup"><span data-stu-id="26f25-143">The profile indicates what type of application we are configuring, and we use it internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="26f25-144">Esistono diversi profili descritti nella sezione seguente.</span><span class="sxs-lookup"><span data-stu-id="26f25-144">There are several profiles available explained in the section below.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="26f25-145">appsettings.Development.json</span><span class="sxs-lookup"><span data-stu-id="26f25-145">appsettings.Development.json</span></span>
<span data-ttu-id="26f25-146">Se osserviamo appsettings. Development.JSON file nella radice del progetto, si noterà un nuovo `IdentityServer` sezione che descrive la chiave viene usata per firmare i token.</span><span class="sxs-lookup"><span data-stu-id="26f25-146">If we look at the appsettings.Development.json file on the root of the project, we can see a new `IdentityServer` section that describes the key we are using to sign tokens.</span></span> <span data-ttu-id="26f25-147">Durante la distribuzione nell'ambiente di produzione una chiave deve essere eseguito il provisioning e distribuire insieme all'applicazione, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="26f25-147">When deploying to production a key needs to be provisioned and deployed alongside the application as explained below.</span></span>

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a><span data-ttu-id="26f25-148">Descrizione generale dell'applicazione Angular</span><span class="sxs-lookup"><span data-stu-id="26f25-148">General description of the Angular application</span></span>
<span data-ttu-id="26f25-149">Il supporto per l'autenticazione e autorizzazione di API del modello Angular si trova in un proprio modulo Angular.</span><span class="sxs-lookup"><span data-stu-id="26f25-149">The support for authentication and API authorization in the Angular template lives in its own Angular module.</span></span> <span data-ttu-id="26f25-150">Nella sezione ClientApp\src\api-authorization e lo è composto da degli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="26f25-150">Under ClientApp\src\api-authorization and it is composed of the following elements:</span></span>
* <span data-ttu-id="26f25-151">3 componenti:</span><span class="sxs-lookup"><span data-stu-id="26f25-151">3 Components:</span></span>
  * <span data-ttu-id="26f25-152">Componente di accesso: Gestisce il flusso di accesso per l'app.</span><span class="sxs-lookup"><span data-stu-id="26f25-152">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="26f25-153">Componenti di disconnessione: Gestisce il flusso di disconnessione per l'app.</span><span class="sxs-lookup"><span data-stu-id="26f25-153">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="26f25-154">Componente di menu account di accesso: Un widget che visualizza l'utente autenticato corrente con collegamenti per gestire il profilo utente e la disconnessione o i collegamenti per accedere o registrare quando l'utente non è autenticata.</span><span class="sxs-lookup"><span data-stu-id="26f25-154">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
* <span data-ttu-id="26f25-155">Una clausola guard route `AuthorizeGuard` che possono essere aggiunti alla route e richiede all'utente di essere autenticati prima di visitare la route.</span><span class="sxs-lookup"><span data-stu-id="26f25-155">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="26f25-156">Un intercettore http `AuthorizeInterceptor` che associa il token di accesso alle richieste HTTP in uscita come destinazione le API quando l'utente è autenticato.</span><span class="sxs-lookup"><span data-stu-id="26f25-156">An http interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="26f25-157">Un servizio `AuthorizeService` che gestisce i dettagli del livello inferiore del processo di autenticazione ed espone le informazioni relative all'utente autenticato al resto dell'applicazione per l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="26f25-157">A service `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>
* <span data-ttu-id="26f25-158">Un modulo angular che definisce le route associate alle parti di autenticazione dell'applicazione e ne espone il componente di menu account di accesso, l'intercettore, la protezione e il servizio per l'utilizzo dal resto dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="26f25-158">An angular module that defines routes associated with the authentication parts of the application and exposes the login menu component, the interceptor, the guard and the service for consumption from the rest of the application.</span></span>

## <a name="general-description-of-the-react-application"></a><span data-ttu-id="26f25-159">Descrizione generale dell'applicazione React</span><span class="sxs-lookup"><span data-stu-id="26f25-159">General description of the React application</span></span>
<span data-ttu-id="26f25-160">Il supporto per l'autenticazione e l'autorizzazione API nella vita di modello di React sotto ClientApp\src\components\api authorization\ e è costituito dagli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="26f25-160">The support for authentication and API authorization in the React template lives under ClientApp\src\components\api-authorization\ and it is composed of the following elements:</span></span>
* <span data-ttu-id="26f25-161">4 componenti:</span><span class="sxs-lookup"><span data-stu-id="26f25-161">4 Components:</span></span>
  * <span data-ttu-id="26f25-162">Componente di accesso: Gestisce il flusso di accesso per l'app.</span><span class="sxs-lookup"><span data-stu-id="26f25-162">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="26f25-163">Componenti di disconnessione: Gestisce il flusso di disconnessione per l'app.</span><span class="sxs-lookup"><span data-stu-id="26f25-163">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="26f25-164">Componente di menu account di accesso: Un widget che visualizza l'utente autenticato corrente con collegamenti per gestire il profilo utente e la disconnessione o i collegamenti per accedere o registrare quando l'utente non è autenticata.</span><span class="sxs-lookup"><span data-stu-id="26f25-164">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
  * <span data-ttu-id="26f25-165">AuthorizeRoute: Un componente di route che richiede all'utente venga autenticato prima del componente indicato nel parametro del componente di rendering.</span><span class="sxs-lookup"><span data-stu-id="26f25-165">AuthorizeRoute: A route component that requires a user to be authenticated before rendering the component indicated in the Component parameter.</span></span>
  * <span data-ttu-id="26f25-166">Una raccolta esportat `authService` istanza della classe `AuthorizeService` che gestisce i dettagli del livello inferiore del processo di autenticazione ed espone le informazioni relative all'utente autenticato al resto dell'applicazione per l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="26f25-166">An exported `authService` instance of class `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>

<span data-ttu-id="26f25-167">Ora che abbiamo visto i componenti principali della soluzione, è possibile esaminare specifico singoli scenari per l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="26f25-167">Now that we've seen the main components of the solution, we can take a specific look at individual scenarios for the application:</span></span>

## <a name="requiring-authorization-on-a-new-api"></a><span data-ttu-id="26f25-168">Richiesta di autorizzazione in una nuova API</span><span class="sxs-lookup"><span data-stu-id="26f25-168">Requiring authorization on a new API</span></span>
<span data-ttu-id="26f25-169">Il sistema è configurato predefiniti per renderlo semplice per richiedere l'autorizzazione per nuove API.</span><span class="sxs-lookup"><span data-stu-id="26f25-169">The system is configured out of the box to make it trivial to require authorization for new APIs.</span></span> <span data-ttu-id="26f25-170">A questo scopo, è sufficiente creare un nuovo controller e aggiungere il `[Authorize]` attributo alla classe controller o a qualsiasi azione all'interno del controller.</span><span class="sxs-lookup"><span data-stu-id="26f25-170">In order to do so, simply create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protecting-a-client-side-route-angular"></a><span data-ttu-id="26f25-171">La protezione di una route lato client (Angular)</span><span class="sxs-lookup"><span data-stu-id="26f25-171">Protecting a client-side route (Angular)</span></span>
<span data-ttu-id="26f25-172">La protezione di una route sul lato client viene eseguita aggiungendo la clausola guard authorize all'elenco di protezioni per l'esecuzione quando si configura una route.</span><span class="sxs-lookup"><span data-stu-id="26f25-172">Protecting a client side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="26f25-173">Ad esempio è possibile vedere come viene configurata la route di recuperare dati all'interno del modulo angular app principale:</span><span class="sxs-lookup"><span data-stu-id="26f25-173">As an example you can see how the fetch-data route is configured within the main app angular module:</span></span>

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="26f25-174">È importante ricordare che la protezione di una route non protegge l'endpoint effettivo (che richiede comunque un `[Authorize]` applicato l'attributo), ma che solo impedisce all'utente passando alla route del lato client specifico quando non è autenticato.</span><span class="sxs-lookup"><span data-stu-id="26f25-174">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="26f25-175">Autenticare le richieste di API (Angular)</span><span class="sxs-lookup"><span data-stu-id="26f25-175">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="26f25-176">Autenticazione delle richieste alle API ospitate sul lato che dell'applicazione viene eseguita automaticamente tramite l'intercettore di client HTTP definito dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="26f25-176">Authenticating requests to APIs hosted along side the application is done automatically through the use of the HTTP client interceptor defined by the application.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="26f25-177">Proteggere una route lato client (React)</span><span class="sxs-lookup"><span data-stu-id="26f25-177">Protect a client-side route (React)</span></span>

<span data-ttu-id="26f25-178">La protezione di una route sul lato client viene eseguita tramite il componente AuthorizeRoute anziché il componente Route semplice.</span><span class="sxs-lookup"><span data-stu-id="26f25-178">Protecting a client side route is done by using the AuthorizeRoute component instead of the plain Route component.</span></span> <span data-ttu-id="26f25-179">Ad esempio è possibile vedere come viene configurata la route di recuperare dati all'interno del componente di App:</span><span class="sxs-lookup"><span data-stu-id="26f25-179">As an example you can see how the fetch-data route is configured within the App component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="26f25-180">È importante ricordare che la protezione di una route non protegge l'endpoint effettivo (che richiede comunque un `[Authorize]` applicato l'attributo), ma che solo impedisce all'utente passando alla route del lato client specifico quando non è autenticato.</span><span class="sxs-lookup"><span data-stu-id="26f25-180">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="26f25-181">Autenticare le richieste di API (React)</span><span class="sxs-lookup"><span data-stu-id="26f25-181">Authenticate API requests (React)</span></span>

<span data-ttu-id="26f25-182">L'autenticazione delle richieste con react viene eseguita importando il `authService` dell'istanza dal `AuthorizeService` quindi recuperando il token di accesso dal authService e collegarlo alla richiesta, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="26f25-182">Authenticating requests with react is done by first importing the `authService` instance from the `AuthorizeService` and then retrieving the access token from the authService and attaching it to the request as shown below.</span></span> <span data-ttu-id="26f25-183">Nei componenti di react ciò avviene in genere nel metodo del ciclo di vita componentDidMount o come risultato dall'interazione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="26f25-183">In react components this is typically done in the componentDidMount lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="26f25-184">Importare il authService nel componente</span><span class="sxs-lookup"><span data-stu-id="26f25-184">Import the authService into your component</span></span>

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="26f25-185">Recuperare e allegare il token di accesso alla risposta</span><span class="sxs-lookup"><span data-stu-id="26f25-185">Retrieve and attach the access token to the response</span></span>

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

## <a name="deploy-into-production"></a><span data-ttu-id="26f25-186">Distribuire in produzione</span><span class="sxs-lookup"><span data-stu-id="26f25-186">Deploy into production</span></span>

<span data-ttu-id="26f25-187">Per distribuire l'applicazione nell'ambiente di produzione è necessario effettuare il provisioning delle risorse:</span><span class="sxs-lookup"><span data-stu-id="26f25-187">In order to deploy the application into production we need to provision several resources:</span></span>
* <span data-ttu-id="26f25-188">Concede a un database per archiviare gli account utente di identità e il server di identità.</span><span class="sxs-lookup"><span data-stu-id="26f25-188">A database to store the Identity user accounts and the identity server grants.</span></span>
* <span data-ttu-id="26f25-189">Un certificato di produzione da usare per firmare i token.</span><span class="sxs-lookup"><span data-stu-id="26f25-189">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="26f25-190">Non sono previsti requisiti specifici per questo certificato. può trattarsi di un certificato autofirmato o un certificato di provisioning tramite un'autorità di certificazione.</span><span class="sxs-lookup"><span data-stu-id="26f25-190">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="26f25-191">Può essere generato tramite gli strumenti standard, ad esempio powershell oppure openssl.</span><span class="sxs-lookup"><span data-stu-id="26f25-191">It can be generated through standard tools like powershell or openssl.</span></span>
  * <span data-ttu-id="26f25-192">Può essere installato nell'archivio certificati nei computer di destinazione o distribuito come file pfx con una password complessa.</span><span class="sxs-lookup"><span data-stu-id="26f25-192">It can be installed into the certificate store on the target machines or deployed as a pfx file with a strong password.</span></span>

### <a name="example-deploy-into-azure-websites"></a><span data-ttu-id="26f25-193">Esempio: Distribuzione in siti Web di Azure</span><span class="sxs-lookup"><span data-stu-id="26f25-193">Example: Deploy into Azure Websites</span></span>

<span data-ttu-id="26f25-194">In questa sezione si intende distribuire l'applicazione in siti Web di Azure usando un certificato archiviato nell'archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="26f25-194">In this section we are going to deploy the application to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="26f25-195">È necessario modificare l'applicazione per caricare un certificato dall'archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="26f25-195">We need to modify the application to load a certicate from the certificate store.</span></span> <span data-ttu-id="26f25-196">A tale scopo, il piano di servizio app deve essere presente almeno nel livello standard quando si configurano in un passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="26f25-196">To do so, our app service plan needs to be at least on the standard tier when we configure in a later step.</span></span> <span data-ttu-id="26f25-197">Nell'applicazione è sufficiente modificare la sezione IdentityServer in appSettings. JSON per includere i dettagli chiave:</span><span class="sxs-lookup"><span data-stu-id="26f25-197">In our application we simply need to modify the IdentityServer section on appsettings.json to include the key details:</span></span>
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
* <span data-ttu-id="26f25-198">La proprietà nome sul certificato corrispondente con l'oggetto distinto per il certificato.</span><span class="sxs-lookup"><span data-stu-id="26f25-198">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="26f25-199">Il percorso dell'archivio rappresenta la posizione in cui caricare il certificato da (CurrentUrser o LocalMachine).</span><span class="sxs-lookup"><span data-stu-id="26f25-199">The store location represents where to load the certificate from (CurrentUrser or LocalMachine).</span></span>
* <span data-ttu-id="26f25-200">Il nome dell'archivio rappresenta il nome dell'archivio certificati in cui è archiviato il certificato, in questo caso che punta all'archivio utente personali.</span><span class="sxs-lookup"><span data-stu-id="26f25-200">The store name represents the name of the certificate store where the certificate is stored, in this case it points to the personal user store.</span></span>

<span data-ttu-id="26f25-201">Per distribuire siti Web di Azure, distribuire l'app seguendo la procedura descritta in [distribuire l'app in Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) per creare le risorse di Azure necessarie e distribuire l'app nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="26f25-201">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="26f25-202">Dopo questa operazione, l'applicazione viene distribuita in Azure ma non è ancora completamente funzionale quanto occorre comunque configurare il certificato da usare per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="26f25-202">After doing this, the application is deployed into Azure but is not yet completely functional as we still need to setup the certificate to be used by the application.</span></span> <span data-ttu-id="26f25-203">A tale scopo, è necessario disporre dell'identificazione personale del certificato che si intende usare e seguire i passaggi descritti in [caricare i certificati](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span><span class="sxs-lookup"><span data-stu-id="26f25-203">To do so, we need to have the thumbprint for the certificate we are going to use and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span></span>

<span data-ttu-id="26f25-204">Quando questi passaggi cita SSL, è disponibile una sezione "Certificati privati" nel portale in cui è possibile caricare il certificato con provisioning da usare con l'App nell'app.</span><span class="sxs-lookup"><span data-stu-id="26f25-204">While these steps mention SSL, there is a "Private certificates" section on the portal where we can upload our provisioned certificate to use with our app.</span></span>

<span data-ttu-id="26f25-205">Dopo questo passaggio, dovrebbe essere possibile riavviare l'applicazione e deve essere completamente funzionale.</span><span class="sxs-lookup"><span data-stu-id="26f25-205">After this step, we should be able to restart our application and it should be completely functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="26f25-206">Altre opzioni di configurazione</span><span class="sxs-lookup"><span data-stu-id="26f25-206">Other configuration options</span></span>
<span data-ttu-id="26f25-207">Il supporto per l'autorizzazione API si basa Identity Server con un set di convenzioni, i valori predefiniti e i miglioramenti per semplificare l'esperienza per le applicazioni a pagina singola.</span><span class="sxs-lookup"><span data-stu-id="26f25-207">Our support for API authorization builds on top of Identity Server with a set of conventions, default values and enhancements to simplify the experience for Single Page Applications.</span></span> <span data-ttu-id="26f25-208">Inutile a dirsi, tutta la potenza di Identity Server è disponibile dietro le quinte se le integrazioni che offriamo non coprono dello scenario.</span><span class="sxs-lookup"><span data-stu-id="26f25-208">Needless to say, the full power of Identity Server is available behind the scenes if the integrations that we offer don't cover your scenario.</span></span> <span data-ttu-id="26f25-209">Il supporto è incentrato su quelle che chiamiamo "proprietaria" applicazioni, in cui tutte le applicazioni vengono create e distribuite da dell'organizzazione.</span><span class="sxs-lookup"><span data-stu-id="26f25-209">Our support is focused on what we call "first-party" applications, where all the applications are created and deployed by our organization.</span></span> <span data-ttu-id="26f25-210">Di conseguenza non offriamo il supporto per elementi quali il consenso o la federazione.</span><span class="sxs-lookup"><span data-stu-id="26f25-210">As such we don't offer support for things like consent or federation.</span></span> <span data-ttu-id="26f25-211">Per questi scenari il Consiglio è usare identità del Server e seguire la relativa documentazione.</span><span class="sxs-lookup"><span data-stu-id="26f25-211">For those scenarios our recommendation is to use Identity Server and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="26f25-212">Profili applicazione</span><span class="sxs-lookup"><span data-stu-id="26f25-212">Application profiles</span></span>
<span data-ttu-id="26f25-213">I profili dell'applicazione sono le configurazioni predefinite per le applicazioni che definiscono ulteriormente i relativi parametri.</span><span class="sxs-lookup"><span data-stu-id="26f25-213">Application profiles are predefined configurations for applications that further define their parameters.</span></span> <span data-ttu-id="26f25-214">In questo momento sono supportati due profili:</span><span class="sxs-lookup"><span data-stu-id="26f25-214">At this time we support two profiles:</span></span>
* <span data-ttu-id="26f25-215">IdentityServerSPA: Rappresenta un'applicazione a singola pagina ospitata insieme a Identity Server come singola unità.</span><span class="sxs-lookup"><span data-stu-id="26f25-215">IdentityServerSPA: Represents a single page application hosted alongside Identity Server as a single unit.</span></span>
  * <span data-ttu-id="26f25-216">Per impostazione predefinita al valore di redirect_uri `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="26f25-216">The redirect_uri defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="26f25-217">Per impostazione predefinita il post_logout_redirect_uri `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="26f25-217">The post_logout_redirect_uri defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="26f25-218">Il set di ambiti include il `openid`, `profile`e tutti gli ambiti definiti per le API nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="26f25-218">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="26f25-219">È il set di tipi di risposta consentiti OIDC `id_token token` o ognuno di essi singolarmente (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="26f25-219">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="26f25-220">La modalità di risposta consentite `fragment`.</span><span class="sxs-lookup"><span data-stu-id="26f25-220">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="26f25-221">SPA: Rappresenta un'applicazione a pagina singola che non è ospitata in Identity Server.</span><span class="sxs-lookup"><span data-stu-id="26f25-221">SPA: Represents a single page application that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="26f25-222">Il set di ambiti include il `openid`, `profile`e tutti gli ambiti definiti per le API nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="26f25-222">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="26f25-223">È il set di tipi di risposta consentiti OIDC `id_token token` o ognuno di essi singolarmente (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="26f25-223">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="26f25-224">La modalità di risposta consentite `fragment`.</span><span class="sxs-lookup"><span data-stu-id="26f25-224">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="26f25-225">IdentityServerJwt: Rappresenta un'API che è ospitata insieme con Identity Server.</span><span class="sxs-lookup"><span data-stu-id="26f25-225">IdentityServerJwt: Represents an API that is hosted alongside with Identity Server.</span></span>
  * <span data-ttu-id="26f25-226">L'applicazione è configurata per utilizzare un ambito singolo che viene impostato sul nome dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="26f25-226">The application is configured to have a single scope that defaults to the application name.</span></span>
* <span data-ttu-id="26f25-227">API: Rappresenta un'API che non è ospitata con Server di identità.</span><span class="sxs-lookup"><span data-stu-id="26f25-227">API: Represents an API that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="26f25-228">L'applicazione è configurata per utilizzare un ambito singolo che viene impostato sul nome dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="26f25-228">The application is configured to have a single scope that defaults to the application name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="26f25-229">Configurazione tramite AppSettings</span><span class="sxs-lookup"><span data-stu-id="26f25-229">Configuration through AppSettings</span></span>
<span data-ttu-id="26f25-230">È possibile configurare le applicazioni tramite il nostro sistema di configurazione aggiungendoli rispettivamente all'elenco di client o risorse.</span><span class="sxs-lookup"><span data-stu-id="26f25-230">We can configure the applications through our configuration system by adding them to the list of Clients or Resources respectively.</span></span> 

<span data-ttu-id="26f25-231">Quando si configurano i client è possibile configurare il `redirect_uri` e il `post_logout_redirect_uri` come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="26f25-231">When configuring clients we can configure the `redirect_uri` and the `post_logout_redirect_uri` as shown below:</span></span>
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

<span data-ttu-id="26f25-232">Quando si configurano le risorse è possibile configurare gli ambiti per la risorsa, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="26f25-232">When configuring resources we can configure the scopes for the resource as shown below:</span></span>
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

### <a name="configuration-through-code"></a><span data-ttu-id="26f25-233">Configurazione tramite codice</span><span class="sxs-lookup"><span data-stu-id="26f25-233">Configuration through code</span></span>
<span data-ttu-id="26f25-234">Possiamo anche configurare i client e le risorse mediante il codice che usa un overload di AddApiAuthorization che accetta un'azione per configurare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="26f25-234">We can also configure the clients and resources through code using an overload of AddApiAuthorization that takes an action to configure options.</span></span>
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
