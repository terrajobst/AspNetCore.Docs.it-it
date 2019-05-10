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
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="bef2f-103">Autenticazione e autorizzazione per SPAs</span><span class="sxs-lookup"><span data-stu-id="bef2f-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="bef2f-104">ASP.NET Core 3.0 o versioni successive offre l'autenticazione nell'App a pagina singola (SPAs) utilizzando il supporto per l'autorizzazione API.</span><span class="sxs-lookup"><span data-stu-id="bef2f-104">ASP.NET Core 3.0 or later offers authentication in Single Page Apps (SPAs) using the support for API authorization.</span></span> <span data-ttu-id="bef2f-105">ASP.NET Core Identity per l'autenticazione e l'archiviazione agli utenti viene combinata con [IdentityServer](https://identityserver.io/) per l'implementazione di Openid Connect.</span><span class="sxs-lookup"><span data-stu-id="bef2f-105">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="bef2f-106">È stato aggiunto un parametro di autenticazione per il **Angular** e **React** modelli che è simile al parametro di autenticazione nel progetto di **applicazione Web (Model-View-Controller)**  (MVC) e **applicazione Web** modelli di progetto (pagine Razor).</span><span class="sxs-lookup"><span data-stu-id="bef2f-106">An authentication parameter was added to the **Angular** and **React** project templates that is similar to the authentication parameter in the **Web Application (Model-View-Controller)** (MVC) and **Web Application** (Razor Pages) project templates.</span></span> <span data-ttu-id="bef2f-107">I valori di parametri consentiti sono **None** e **singoli**.</span><span class="sxs-lookup"><span data-stu-id="bef2f-107">The allowed parameter values are **None** and **Individual**.</span></span> <span data-ttu-id="bef2f-108">Il **React. js e Redux** modello di progetto non supporta il parametro di autenticazione in questo momento.</span><span class="sxs-lookup"><span data-stu-id="bef2f-108">The **React.js and Redux** project template doesn't support the authentication parameter at this time.</span></span>

## <a name="create-an-app-with-api-authorization-support"></a><span data-ttu-id="bef2f-109">Creare un'app con supporto per l'autorizzazione API</span><span class="sxs-lookup"><span data-stu-id="bef2f-109">Create an app with API authorization support</span></span>

<span data-ttu-id="bef2f-110">Autenticazione e autorizzazione è utilizzabile con Angular e React SPAs.</span><span class="sxs-lookup"><span data-stu-id="bef2f-110">User authentication and authorization can be used with both Angular and React SPAs.</span></span> <span data-ttu-id="bef2f-111">Aprire una shell dei comandi ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bef2f-111">Open a command shell, and run the following command:</span></span>

<span data-ttu-id="bef2f-112">**Angular**:</span><span class="sxs-lookup"><span data-stu-id="bef2f-112">**Angular**:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="bef2f-113">**Reagire**:</span><span class="sxs-lookup"><span data-stu-id="bef2f-113">**React**:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="bef2f-114">Il comando precedente crea un'app ASP.NET Core con un *ClientApp* directory contenente l'applicazione a singola pagina.</span><span class="sxs-lookup"><span data-stu-id="bef2f-114">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the SPA.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="bef2f-115">Descrizione generale dei componenti dell'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bef2f-115">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="bef2f-116">Le sezioni seguenti descrivono le aggiunte al progetto quando è incluso il supporto di autenticazione:</span><span class="sxs-lookup"><span data-stu-id="bef2f-116">The following sections describe additions to the project when authentication support is included:</span></span>

### <a name="startup-class"></a><span data-ttu-id="bef2f-117">Classe di avvio</span><span class="sxs-lookup"><span data-stu-id="bef2f-117">Startup class</span></span>

<span data-ttu-id="bef2f-118">Il `Startup` classe ha le aggiunte seguenti:</span><span class="sxs-lookup"><span data-stu-id="bef2f-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="bef2f-119">All'interno di `Startup.ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="bef2f-119">Inside the `Startup.ConfigureServices` method:</span></span>
  * <span data-ttu-id="bef2f-120">Identità con l'interfaccia utente predefinita:</span><span class="sxs-lookup"><span data-stu-id="bef2f-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="bef2f-121">IdentityServer con un'ulteriore `AddApiAuthorization` metodo helper che configurazioni alcuni predefinito nella parte superiore IdentityServer le convenzioni di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="bef2f-121">IdentityServer with an additional `AddApiAuthorization` helper method that setups some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="bef2f-122">L'autenticazione con un'ulteriore `AddIdentityServerJwt` metodo helper che consente di configurare l'app per convalidare i token JWT generato da IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="bef2f-122">Authentication with an additional `AddIdentityServerJwt` helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="bef2f-123">All'interno di `Startup.Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="bef2f-123">Inside the `Startup.Configure` method:</span></span>
  * <span data-ttu-id="bef2f-124">Il middleware di autenticazione che è responsabile per la convalida delle credenziali richieste e impostando l'utente per il contesto della richiesta:</span><span class="sxs-lookup"><span data-stu-id="bef2f-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="bef2f-125">Il middleware IdentityServer che espone gli endpoint Openid Connect:</span><span class="sxs-lookup"><span data-stu-id="bef2f-125">The IdentityServer middleware that exposes the Open ID Connect endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="bef2f-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="bef2f-126">AddApiAuthorization</span></span>

<span data-ttu-id="bef2f-127">Questo metodo di supporto Configura IdentityServer per usare la configurazione supportata.</span><span class="sxs-lookup"><span data-stu-id="bef2f-127">This helper method configures IdentityServer to use our supported configuration.</span></span> <span data-ttu-id="bef2f-128">IdentityServer è un framework potente ed estendibile per la gestione di problemi di protezione delle app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="bef2f-129">Allo stesso tempo, che espone inutili complessità per gli scenari più comuni.</span><span class="sxs-lookup"><span data-stu-id="bef2f-129">At the same time, that exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="bef2f-130">Di conseguenza, viene fornito un set di convenzioni e le opzioni di configurazione per l'utente che sono considerati un buon punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="bef2f-130">Consequently, a set of conventions and configuration options is provided to you that are considered a good starting point.</span></span> <span data-ttu-id="bef2f-131">Dopo l'autenticazione delle esigenze, tutta la potenza di IdentityServer è ancora disponibile per personalizzare l'autenticazione in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="bef2f-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="bef2f-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="bef2f-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="bef2f-133">Questo metodo di supporto consente di configurare una combinazione di criteri per l'app come il gestore di autenticazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="bef2f-133">This helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="bef2f-134">Il criterio è configurato per consentire a gestire tutte le richieste indirizzate a qualsiasi percorso secondario nel campo URL di identità dell'identità "/ Identity".</span><span class="sxs-lookup"><span data-stu-id="bef2f-134">The policy is configured to let Identity handle all requests routed to any subpath in the Identity URL space "/Identity".</span></span> <span data-ttu-id="bef2f-135">Il `JwtBearerHandler` gestisce tutte le altre richieste.</span><span class="sxs-lookup"><span data-stu-id="bef2f-135">The `JwtBearerHandler` handles all other requests.</span></span> <span data-ttu-id="bef2f-136">Inoltre, questo metodo registra un `<<ApplicationName>>API` della risorsa API con IdentityServer con un ambito predefinito di `<<ApplicationName>>API` e configura il middleware del token Bearer token JWT per convalidare i token emessi da IdentityServer per l'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-136">Additionally, this method registers an `<<ApplicationName>>API` API resource with IdentityServer with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="bef2f-137">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="bef2f-137">SampleDataController</span></span>

<span data-ttu-id="bef2f-138">Nel *Controllers\SampleDataController.cs* del file, si noti il `[Authorize]` attributo applicato alla classe che indica che l'utente deve essere autorizzato in base i criteri predefiniti per accedere alla risorsa.</span><span class="sxs-lookup"><span data-stu-id="bef2f-138">In the *Controllers\SampleDataController.cs* file, notice the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="bef2f-139">I criteri di autorizzazione predefinita accade per essere configurato per usare lo schema di autenticazione predefinito, che viene impostato in modo da `AddIdentityServerJwt` rispetto allo schema dei criteri è stato indicato in precedenza, rendendo il `JwtBearerHandler` configurate da tale metodo di supporto del gestore predefinito per richieste all'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-139">The default authorization policy happens to be configured to use the default authentication scheme, which is set up by `AddIdentityServerJwt` to the policy scheme that was mentioned above, making the `JwtBearerHandler` configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="bef2f-140">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="bef2f-140">ApplicationDbContext</span></span>

<span data-ttu-id="bef2f-141">Nel *Data\ApplicationDbContext.cs* del file, si noti la stessa `DbContext` viene usato nell'identità con l'eccezione che estende `ApiAuthorizationDbContext` (più derivato dalla classe `IdentityDbContext`) da includere lo schema per IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="bef2f-141">In the *Data\ApplicationDbContext.cs* file, notice the same `DbContext` is used in Identity with the exception that it extends `ApiAuthorizationDbContext` (a more derived class from `IdentityDbContext`) to include the schema for IdentityServer.</span></span>

<span data-ttu-id="bef2f-142">Per ottenere il controllo completo dello schema del database, ereditare da una delle identità disponibili `DbContext` classi e configurare il contesto per includere lo schema di identità chiamando `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` nel `OnModelCreating` (metodo).</span><span class="sxs-lookup"><span data-stu-id="bef2f-142">To gain full control of the database schema, inherit from one of the available Identity `DbContext` classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="bef2f-143">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="bef2f-143">OidcConfigurationController</span></span>

<span data-ttu-id="bef2f-144">Nel *Controllers\OidcConfigurationController.cs* file, si noti che l'endpoint che viene eseguito il provisioning per servire i parametri OIDC che il client deve utilizzare.</span><span class="sxs-lookup"><span data-stu-id="bef2f-144">In the *Controllers\OidcConfigurationController.cs* file, notice the endpoint that's provisioned to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="bef2f-145">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="bef2f-145">appsettings.json</span></span>

<span data-ttu-id="bef2f-146">Nel *appSettings. JSON* file principale del progetto, c'è un nuovo `IdentityServer` sezione che descrive l'elenco di configurata i client.</span><span class="sxs-lookup"><span data-stu-id="bef2f-146">In the *appsettings.json* file of the project root, there's a new `IdentityServer` section that describes the list of configured clients.</span></span> <span data-ttu-id="bef2f-147">Nell'esempio seguente, è disponibile un singolo client.</span><span class="sxs-lookup"><span data-stu-id="bef2f-147">In the following example, there's a single client.</span></span> <span data-ttu-id="bef2f-148">Il nome del client corrisponde al nome dell'app e viene eseguito il mapping per convenzione per OAuth `ClientId` parametro.</span><span class="sxs-lookup"><span data-stu-id="bef2f-148">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="bef2f-149">Il profilo indica il tipo di app da configurare.</span><span class="sxs-lookup"><span data-stu-id="bef2f-149">The profile indicates the app type being configured.</span></span> <span data-ttu-id="bef2f-150">Viene usato internamente per le convenzioni di unità che semplificano il processo di configurazione per il server.</span><span class="sxs-lookup"><span data-stu-id="bef2f-150">It's used internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="bef2f-151">Esistono diversi profili disponibili, come spiegato nel [profili applicazione](#application-profiles) sezione.</span><span class="sxs-lookup"><span data-stu-id="bef2f-151">There are several profiles available, as explained in the [Application profiles](#application-profiles) section.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="bef2f-152">appsettings.Development.json</span><span class="sxs-lookup"><span data-stu-id="bef2f-152">appsettings.Development.json</span></span>

<span data-ttu-id="bef2f-153">Nel *appsettings. Development.JSON* file principale del progetto, è presente un `IdentityServer` sezione che descrive la chiave usata per firmare i token.</span><span class="sxs-lookup"><span data-stu-id="bef2f-153">In the *appsettings.Development.json* file of the project root, there's an `IdentityServer` section that describes the key used to sign tokens.</span></span> <span data-ttu-id="bef2f-154">Durante la distribuzione nell'ambiente di produzione, una chiave deve essere eseguito il provisioning e distribuire insieme all'app, come spiegato nel [Distribuisci in produzione](#deploy-to-production) sezione.</span><span class="sxs-lookup"><span data-stu-id="bef2f-154">When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section.</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a><span data-ttu-id="bef2f-155">Descrizione generale dell'app Angular</span><span class="sxs-lookup"><span data-stu-id="bef2f-155">General description of the Angular app</span></span>

<span data-ttu-id="bef2f-156">L'autenticazione e l'autorizzazione API supportano in Angular modello si trova in un proprio modulo Angular nel *ClientApp\src\api-authorization* directory.</span><span class="sxs-lookup"><span data-stu-id="bef2f-156">The authentication and API authorization support in the Angular template resides in its own Angular module in the *ClientApp\src\api-authorization* directory.</span></span> <span data-ttu-id="bef2f-157">Il modulo è costituito dagli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="bef2f-157">The module is composed of the following elements:</span></span>

* <span data-ttu-id="bef2f-158">3 componenti:</span><span class="sxs-lookup"><span data-stu-id="bef2f-158">3 components:</span></span>
  * <span data-ttu-id="bef2f-159">*login.component.ts*: Gestisce il flusso di accesso dell'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-159">*login.component.ts*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="bef2f-160">*logout.component.ts*: Gestisce il flusso di disconnessione dell'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-160">*logout.component.ts*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="bef2f-161">*login-menu.component.ts*: Un widget che visualizza uno dei set di collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="bef2f-161">*login-menu.component.ts*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="bef2f-162">Gestione dei profili utente e la disconnessione collegamenti quando l'utente è autenticato.</span><span class="sxs-lookup"><span data-stu-id="bef2f-162">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="bef2f-163">Registrazione e log nei collegamenti quando l'utente non è autenticato.</span><span class="sxs-lookup"><span data-stu-id="bef2f-163">Registration and log in links when the user isn't authenticated.</span></span>
* <span data-ttu-id="bef2f-164">Una clausola guard route `AuthorizeGuard` che possono essere aggiunti alla route e richiede all'utente di essere autenticati prima di visitare la route.</span><span class="sxs-lookup"><span data-stu-id="bef2f-164">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="bef2f-165">Un intercettore HTTP `AuthorizeInterceptor` che associa il token di accesso alle richieste HTTP in uscita come destinazione le API quando l'utente è autenticato.</span><span class="sxs-lookup"><span data-stu-id="bef2f-165">An HTTP interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="bef2f-166">Un servizio `AuthorizeService` che gestisce i dettagli di basso livello del processo di autenticazione ed espone le informazioni relative all'utente autenticato al resto dell'app per l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="bef2f-166">A service `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>
* <span data-ttu-id="bef2f-167">Un modulo Angular che definisce le route associate alle parti di autenticazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-167">An Angular module that defines routes associated with the authentication parts of the app.</span></span> <span data-ttu-id="bef2f-168">Espone il componente di menu account di accesso, l'intercettore, la protezione e il servizio per l'utilizzo dal resto dell'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-168">It exposes the login menu component, the interceptor, the guard, and the service for consumption from the rest of the app.</span></span>

## <a name="general-description-of-the-react-app"></a><span data-ttu-id="bef2f-169">Descrizione generale dell'app React</span><span class="sxs-lookup"><span data-stu-id="bef2f-169">General description of the React app</span></span>

<span data-ttu-id="bef2f-170">Il supporto per l'autenticazione e autorizzazione di API del modello di React si trova nel *ClientApp\src\components\api-authorization* directory.</span><span class="sxs-lookup"><span data-stu-id="bef2f-170">The support for authentication and API authorization in the React template resides in the *ClientApp\src\components\api-authorization* directory.</span></span> <span data-ttu-id="bef2f-171">Si è costituito dagli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="bef2f-171">It's composed of the following elements:</span></span>

* <span data-ttu-id="bef2f-172">4 componenti:</span><span class="sxs-lookup"><span data-stu-id="bef2f-172">4 components:</span></span>
  * <span data-ttu-id="bef2f-173">*Login.js*: Gestisce il flusso di accesso dell'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-173">*Login.js*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="bef2f-174">*Logout.js*: Gestisce il flusso di disconnessione dell'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-174">*Logout.js*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="bef2f-175">*LoginMenu.js*: Un widget che visualizza uno dei set di collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="bef2f-175">*LoginMenu.js*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="bef2f-176">Gestione dei profili utente e la disconnessione collegamenti quando l'utente è autenticato.</span><span class="sxs-lookup"><span data-stu-id="bef2f-176">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="bef2f-177">Registrazione e log nei collegamenti quando l'utente non è autenticato.</span><span class="sxs-lookup"><span data-stu-id="bef2f-177">Registration and log in links when the user isn't authenticated.</span></span>
  * <span data-ttu-id="bef2f-178">*AuthorizeRoute.js*: Un componente di route che richiede all'utente venga autenticato prima del componente di rendering indicate nel `Component` parametro.</span><span class="sxs-lookup"><span data-stu-id="bef2f-178">*AuthorizeRoute.js*: A route component that requires a user to be authenticated before rendering the component indicated in the `Component` parameter.</span></span>
* <span data-ttu-id="bef2f-179">Una raccolta esportat `authService` istanza della classe `AuthorizeService` che gestisce i dettagli di basso livello del processo di autenticazione ed espone le informazioni relative all'utente autenticato al resto dell'app per l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="bef2f-179">An exported `authService` instance of class `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>

<span data-ttu-id="bef2f-180">Ora che si sono visto i componenti principali della soluzione, è possibile esaminare più profondo singoli scenari per l'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-180">Now that you've seen the main components of the solution, you can take a deeper look at individual scenarios for the app.</span></span>

## <a name="require-authorization-on-a-new-api"></a><span data-ttu-id="bef2f-181">Richiedere l'autorizzazione in una nuova API</span><span class="sxs-lookup"><span data-stu-id="bef2f-181">Require authorization on a new API</span></span>

<span data-ttu-id="bef2f-182">Per impostazione predefinita, il sistema è configurato per richiedere facilmente l'autorizzazione per le nuove API.</span><span class="sxs-lookup"><span data-stu-id="bef2f-182">By default, the system is configured to easily require authorization for new APIs.</span></span> <span data-ttu-id="bef2f-183">A tale scopo, creare un nuovo controller e aggiungere il `[Authorize]` attributo alla classe controller o a qualsiasi azione all'interno del controller.</span><span class="sxs-lookup"><span data-stu-id="bef2f-183">To do so, create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protect-a-client-side-route-angular"></a><span data-ttu-id="bef2f-184">Proteggere una route lato client (Angular)</span><span class="sxs-lookup"><span data-stu-id="bef2f-184">Protect a client-side route (Angular)</span></span>

<span data-ttu-id="bef2f-185">La protezione di una route lato client viene eseguita aggiungendo la clausola guard authorize all'elenco di protezioni per l'esecuzione quando si configura una route.</span><span class="sxs-lookup"><span data-stu-id="bef2f-185">Protecting a client-side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="bef2f-186">Ad esempio, è possibile vedere come il `fetch-data` route è stata configurata all'interno del modulo Angular app principale:</span><span class="sxs-lookup"><span data-stu-id="bef2f-186">As an example, you can see how the `fetch-data` route is configured within the main app Angular module:</span></span>

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="bef2f-187">È importante ricordare che la protezione di una route non protegge l'endpoint effettivo (che richiede comunque un `[Authorize]` applicato l'attributo), ma che solo impedisce all'utente passando alla route del lato client specificata quando la si non è autenticato.</span><span class="sxs-lookup"><span data-stu-id="bef2f-187">It's important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="bef2f-188">Autenticare le richieste di API (Angular)</span><span class="sxs-lookup"><span data-stu-id="bef2f-188">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="bef2f-189">L'autenticazione delle richieste alle API ospitate insieme con l'app viene eseguita automaticamente tramite l'intercettore di client HTTP definito dall'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-189">Authenticating requests to APIs hosted alongside the app is done automatically through the use of the HTTP client interceptor defined by the app.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="bef2f-190">Proteggere una route lato client (React)</span><span class="sxs-lookup"><span data-stu-id="bef2f-190">Protect a client-side route (React)</span></span>

<span data-ttu-id="bef2f-191">Proteggere una route lato client usando il `AuthorizeRoute` componente anziché il normale `Route` componente.</span><span class="sxs-lookup"><span data-stu-id="bef2f-191">Protect a client-side route by using the `AuthorizeRoute` component instead of the plain `Route` component.</span></span> <span data-ttu-id="bef2f-192">Ad esempio, si noti come il `fetch-data` route è stata configurata all'interno di `App` componente:</span><span class="sxs-lookup"><span data-stu-id="bef2f-192">For example, notice how the `fetch-data` route is configured within the `App` component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="bef2f-193">La protezione di una route:</span><span class="sxs-lookup"><span data-stu-id="bef2f-193">Protecting a route:</span></span>

* <span data-ttu-id="bef2f-194">Non protegge l'endpoint effettivo (che richiede comunque un `[Authorize]` applicato l'attributo).</span><span class="sxs-lookup"><span data-stu-id="bef2f-194">Doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it).</span></span>
* <span data-ttu-id="bef2f-195">Impedisce solo l'utente lo spostamento alla route del lato client specificata quando la si non è autenticato.</span><span class="sxs-lookup"><span data-stu-id="bef2f-195">Only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="bef2f-196">Autenticare le richieste di API (React)</span><span class="sxs-lookup"><span data-stu-id="bef2f-196">Authenticate API requests (React)</span></span>

<span data-ttu-id="bef2f-197">L'autenticazione delle richieste con React viene eseguita importando il `authService` dell'istanza dal `AuthorizeService`.</span><span class="sxs-lookup"><span data-stu-id="bef2f-197">Authenticating requests with React is done by first importing the `authService` instance from the `AuthorizeService`.</span></span> <span data-ttu-id="bef2f-198">Il token di accesso viene recuperato dal `authService` e viene associato alla richiesta come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="bef2f-198">The access token is retrieved from the `authService` and is attached to the request as shown below.</span></span> <span data-ttu-id="bef2f-199">Nei componenti di React, questa operazione avviene in genere `componentDidMount` metodo del ciclo di vita o come risultato di alcune interazioni dell'utente.</span><span class="sxs-lookup"><span data-stu-id="bef2f-199">In React components, this work is typically done in the `componentDidMount` lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="bef2f-200">Importare il authService nel componente</span><span class="sxs-lookup"><span data-stu-id="bef2f-200">Import the authService into your component</span></span>

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="bef2f-201">Recuperare e allegare il token di accesso alla risposta</span><span class="sxs-lookup"><span data-stu-id="bef2f-201">Retrieve and attach the access token to the response</span></span>

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

## <a name="deploy-to-production"></a><span data-ttu-id="bef2f-202">Distribuire nell'ambiente di produzione</span><span class="sxs-lookup"><span data-stu-id="bef2f-202">Deploy to production</span></span>

<span data-ttu-id="bef2f-203">Per distribuire l'app nell'ambiente di produzione, è necessario eseguire il provisioning le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="bef2f-203">To deploy the app to production, the following resources need to be provisioned:</span></span>

* <span data-ttu-id="bef2f-204">Un database per archiviare gli account utente di identità e i privilegi concessi IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="bef2f-204">A database to store the Identity user accounts and the IdentityServer grants.</span></span>
* <span data-ttu-id="bef2f-205">Un certificato di produzione da usare per firmare i token.</span><span class="sxs-lookup"><span data-stu-id="bef2f-205">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="bef2f-206">Non sono previsti requisiti specifici per questo certificato. può trattarsi di un certificato autofirmato o un certificato di provisioning tramite un'autorità di certificazione.</span><span class="sxs-lookup"><span data-stu-id="bef2f-206">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="bef2f-207">Può essere generato tramite gli strumenti standard, ad esempio PowerShell oppure OpenSSL.</span><span class="sxs-lookup"><span data-stu-id="bef2f-207">It can be generated through standard tools like PowerShell or OpenSSL.</span></span>
  * <span data-ttu-id="bef2f-208">Può essere installato nell'archivio certificati nei computer di destinazione o distribuito come un *PFX* file con una password complessa.</span><span class="sxs-lookup"><span data-stu-id="bef2f-208">It can be installed into the certificate store on the target machines or deployed as a *.pfx* file with a strong password.</span></span>

### <a name="example-deploy-to-azure-websites"></a><span data-ttu-id="bef2f-209">Esempio: Distribuire siti Web di Azure</span><span class="sxs-lookup"><span data-stu-id="bef2f-209">Example: Deploy to Azure Websites</span></span>

<span data-ttu-id="bef2f-210">Questa sezione descrive la distribuzione dell'app per siti Web di Azure usando un certificato archiviato nell'archivio certificati.</span><span class="sxs-lookup"><span data-stu-id="bef2f-210">This section describes deploying the app to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="bef2f-211">Per modificare l'app per caricare un certificato dall'archivio certificati, il piano di servizio App deve essere in esecuzione almeno il livello Standard quando si configurano in un passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="bef2f-211">To modify the app to load a certificate from the certificate store, the App Service plan needs to be on at least the Standard tier when you configure in a later step.</span></span> <span data-ttu-id="bef2f-212">Dell'app *appSettings. JSON* del file, modificare il `IdentityServer` sezione per includere i dettagli chiave:</span><span class="sxs-lookup"><span data-stu-id="bef2f-212">In the app's *appsettings.json* file, modify the `IdentityServer` section to include the key details:</span></span>

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

* <span data-ttu-id="bef2f-213">La proprietà nome sul certificato corrispondente con l'oggetto distinto per il certificato.</span><span class="sxs-lookup"><span data-stu-id="bef2f-213">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="bef2f-214">Il percorso dell'archivio rappresenta la posizione in cui caricare il certificato da (`CurrentUser` o `LocalMachine`).</span><span class="sxs-lookup"><span data-stu-id="bef2f-214">The store location represents where to load the certificate from (`CurrentUser` or `LocalMachine`).</span></span>
* <span data-ttu-id="bef2f-215">Il nome dell'archivio rappresenta il nome dell'archivio certificati in cui è archiviato il certificato.</span><span class="sxs-lookup"><span data-stu-id="bef2f-215">The store name represents the name of the certificate store where the certificate is stored.</span></span> <span data-ttu-id="bef2f-216">In questo caso, fa riferimento all'archivio dell'utente personale.</span><span class="sxs-lookup"><span data-stu-id="bef2f-216">In this case, it points to the personal user store.</span></span>

<span data-ttu-id="bef2f-217">Per distribuire siti Web di Azure, distribuire l'app seguendo la procedura descritta in [distribuire l'app in Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) per creare le risorse di Azure necessarie e distribuire l'app nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="bef2f-217">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="bef2f-218">Dopo aver seguito le istruzioni precedenti, l'app viene distribuita in Azure ma non è ancora attiva.</span><span class="sxs-lookup"><span data-stu-id="bef2f-218">After following the preceding instructions, the app is deployed to Azure but isn't yet functional.</span></span> <span data-ttu-id="bef2f-219">Il certificato usato dall'app deve comunque essere configurati.</span><span class="sxs-lookup"><span data-stu-id="bef2f-219">The certificate used by the app still needs to be set up.</span></span> <span data-ttu-id="bef2f-220">Individuare l'identificazione personale del certificato da usare e seguire i passaggi descritti [caricare i certificati](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span><span class="sxs-lookup"><span data-stu-id="bef2f-220">Locate the thumbprint for the certificate to be used, and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span></span>

<span data-ttu-id="bef2f-221">Mentre questi passaggi menzionare SSL, non vi è una **certificati privati** sezione nel portale in cui è possibile caricare il certificato di provisioning da usare con l'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-221">While these steps mention SSL, there's a **Private certificates** section on the portal where you can upload the provisioned certificate to use with the app.</span></span>

<span data-ttu-id="bef2f-222">Dopo questo passaggio, riavviare l'app e deve essere funzionale.</span><span class="sxs-lookup"><span data-stu-id="bef2f-222">After this step, restart the app and it should be functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="bef2f-223">Altre opzioni di configurazione</span><span class="sxs-lookup"><span data-stu-id="bef2f-223">Other configuration options</span></span>

<span data-ttu-id="bef2f-224">Il supporto per l'autorizzazione API si basa IdentityServer con un set di convenzioni, i valori predefiniti e miglioramenti per semplificare l'esperienza per SPAs.</span><span class="sxs-lookup"><span data-stu-id="bef2f-224">The support for API authorization builds on top of IdentityServer with a set of conventions, default values, and enhancements to simplify the experience for SPAs.</span></span> <span data-ttu-id="bef2f-225">Inutile a dirsi, la potenza completa di IdentityServer è disponibile dietro le quinte se integrazioni di ASP.NET Core non coprono dello scenario.</span><span class="sxs-lookup"><span data-stu-id="bef2f-225">Needless to say, the full power of IdentityServer is available behind the scenes if the ASP.NET Core integrations don't cover your scenario.</span></span> <span data-ttu-id="bef2f-226">Il supporto di ASP.NET Core è incentrato sulle app "cookie", in cui tutte le app vengono create e distribuite dalla nostra organizzazione.</span><span class="sxs-lookup"><span data-stu-id="bef2f-226">The ASP.NET Core support is focused on "first-party" apps, where all the apps are created and deployed by our organization.</span></span> <span data-ttu-id="bef2f-227">Di conseguenza, il supporto non è disponibile per operazioni come il consenso o la federazione.</span><span class="sxs-lookup"><span data-stu-id="bef2f-227">As such, support isn't offered for things like consent or federation.</span></span> <span data-ttu-id="bef2f-228">Per tali scenari, usare IdentityServer e seguire la relativa documentazione.</span><span class="sxs-lookup"><span data-stu-id="bef2f-228">For those scenarios, use IdentityServer and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="bef2f-229">Profili applicazione</span><span class="sxs-lookup"><span data-stu-id="bef2f-229">Application profiles</span></span>

<span data-ttu-id="bef2f-230">I profili dell'applicazione sono le configurazioni predefinite per le app che definiscono ulteriormente i relativi parametri.</span><span class="sxs-lookup"><span data-stu-id="bef2f-230">Application profiles are predefined configurations for apps that further define their parameters.</span></span> <span data-ttu-id="bef2f-231">A questo punto, sono supportati i profili seguenti:</span><span class="sxs-lookup"><span data-stu-id="bef2f-231">At this time, the following profiles are supported:</span></span>

* <span data-ttu-id="bef2f-232">`IdentityServerSPA`: Rappresenta un'applicazione a singola pagina ospitato insieme con IdentityServer come singola unità.</span><span class="sxs-lookup"><span data-stu-id="bef2f-232">`IdentityServerSPA`: Represents a SPA hosted alongside IdentityServer as a single unit.</span></span>
  * <span data-ttu-id="bef2f-233">Il `redirect_uri` per impostazione predefinita `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="bef2f-233">The `redirect_uri` defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="bef2f-234">Il `post_logout_redirect_uri` per impostazione predefinita `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="bef2f-234">The `post_logout_redirect_uri` defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="bef2f-235">Il set di ambiti include il `openid`, `profile`e tutti gli ambiti definiti per le API nell'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-235">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="bef2f-236">È il set di tipi di risposta consentiti OIDC `id_token token` o ognuno di essi singolarmente (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="bef2f-236">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="bef2f-237">La modalità di risposta consentite `fragment`.</span><span class="sxs-lookup"><span data-stu-id="bef2f-237">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="bef2f-238">`SPA`: Rappresenta un'applicazione a singola pagina che non è ospitato con IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="bef2f-238">`SPA`: Represents a SPA that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="bef2f-239">Il set di ambiti include il `openid`, `profile`e tutti gli ambiti definiti per le API nell'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-239">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="bef2f-240">È il set di tipi di risposta consentiti OIDC `id_token token` o ognuno di essi singolarmente (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="bef2f-240">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="bef2f-241">La modalità di risposta consentite `fragment`.</span><span class="sxs-lookup"><span data-stu-id="bef2f-241">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="bef2f-242">`IdentityServerJwt`: Rappresenta un'API che è ospitata insieme con IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="bef2f-242">`IdentityServerJwt`: Represents an API that is hosted alongside with IdentityServer.</span></span>
  * <span data-ttu-id="bef2f-243">L'app è configurata per avere un ambito singolo valore predefinito è il nome dell'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-243">The app is configured to have a single scope that defaults to the app name.</span></span>
* <span data-ttu-id="bef2f-244">`API`: Rappresenta un'API che non è ospitata con IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="bef2f-244">`API`: Represents an API that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="bef2f-245">L'app è configurata per avere un ambito singolo valore predefinito è il nome dell'app.</span><span class="sxs-lookup"><span data-stu-id="bef2f-245">The app is configured to have a single scope that defaults to the app name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="bef2f-246">Configurazione tramite AppSettings</span><span class="sxs-lookup"><span data-stu-id="bef2f-246">Configuration through AppSettings</span></span>

<span data-ttu-id="bef2f-247">Configurare le app tramite il sistema di configurazione aggiungendoli all'elenco degli `Clients` o `Resources`.</span><span class="sxs-lookup"><span data-stu-id="bef2f-247">Configure the apps through the configuration system by adding them to the list of `Clients` or `Resources`.</span></span>

<span data-ttu-id="bef2f-248">Configurare ogni client `redirect_uri` e `post_logout_redirect_uri` proprietà, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="bef2f-248">Configure each client's `redirect_uri` and `post_logout_redirect_uri` property, as shown in the following example:</span></span>

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

<span data-ttu-id="bef2f-249">Quando si configurano le risorse, è possibile configurare gli ambiti per la risorsa, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="bef2f-249">When configuring resources, you can configure the scopes for the resource as shown below:</span></span>

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

### <a name="configuration-through-code"></a><span data-ttu-id="bef2f-250">Configurazione tramite codice</span><span class="sxs-lookup"><span data-stu-id="bef2f-250">Configuration through code</span></span>

<span data-ttu-id="bef2f-251">È anche possibile configurare i client e le risorse mediante il codice che usa un overload di `AddApiAuthorization` che accetta un'azione per configurare le opzioni.</span><span class="sxs-lookup"><span data-stu-id="bef2f-251">You can also configure the clients and resources through code using an overload of `AddApiAuthorization` that takes an action to configure options.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="bef2f-252">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="bef2f-252">Additional resources</span></span>

* <xref:spa/angular>
* <xref:spa/react>
