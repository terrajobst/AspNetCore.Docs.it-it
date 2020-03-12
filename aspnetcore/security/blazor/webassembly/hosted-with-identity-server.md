---
title: Proteggere un ASP.NET Core Blazor app ospitata tramite webassembly con Identity Server
author: guardrex
description: Per creare una nuova app ospitata Blazor con autenticazione da Visual Studio che usa un back-end [IdentityServer](https://identityserver.io/)
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-identity-server
ms.openlocfilehash: 98eb126ab3c483e0a6dc2274db8ffcfd9d5bc59a
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083776"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-identity-server"></a><span data-ttu-id="3fb1f-103">Proteggere un ASP.NET Core Blazor app ospitata tramite webassembly con Identity Server</span><span class="sxs-lookup"><span data-stu-id="3fb1f-103">Secure an ASP.NET Core Blazor WebAssembly hosted app with Identity Server</span></span>

<span data-ttu-id="3fb1f-104">Di [Javier Calvarro Nelson](https://github.com/javiercn) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3fb1f-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="3fb1f-105">Per creare una nuova Blazor app ospitata in Visual Studio che usa [IdentityServer](https://identityserver.io/) per autenticare gli utenti e le chiamate API:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-105">To create a new Blazor hosted app in Visual Studio that uses [IdentityServer](https://identityserver.io/) to authenticate users and API calls:</span></span>

1. <span data-ttu-id="3fb1f-106">Usare Visual Studio per creare una nuova app **Blazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="3fb1f-106">Use Visual Studio to create a new **Blazor WebAssembly** app.</span></span> <span data-ttu-id="3fb1f-107">Per altre informazioni, vedere <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-107">For more information, see <xref:blazor/get-started>.</span></span>
1. <span data-ttu-id="3fb1f-108">Nella finestra di dialogo **Crea una nuova app Blazor** selezionare **modifica** nella sezione **autenticazione** .</span><span class="sxs-lookup"><span data-stu-id="3fb1f-108">In the **Create a new Blazor app** dialog, select **Change** in the **Authentication** section.</span></span>
1. <span data-ttu-id="3fb1f-109">Selezionare **account utente singoli** e quindi **OK**.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-109">Select **Individual User Accounts** followed by **OK**.</span></span>
1. <span data-ttu-id="3fb1f-110">Nella sezione **Avanzate** Selezionare la casella di controllo **ASP.NET Core Hosted** .</span><span class="sxs-lookup"><span data-stu-id="3fb1f-110">Select the **ASP.NET Core hosted** checkbox in the **Advanced** section.</span></span>
1. <span data-ttu-id="3fb1f-111">Selezionare il pulsante **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-111">Select the **Create** button.</span></span>

<span data-ttu-id="3fb1f-112">Per creare l'app in una shell dei comandi, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-112">To create the app in a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

<span data-ttu-id="3fb1f-113">Per specificare il percorso di output, che crea una cartella di progetto, se non esiste, includere l'opzione di output nel comando con un percorso, ad esempio `-o BlazorSample`.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-113">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="3fb1f-114">Il nome della cartella diventa anche parte del nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-114">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="3fb1f-115">Configurazione dell'app Server</span><span class="sxs-lookup"><span data-stu-id="3fb1f-115">Server app configuration</span></span>

<span data-ttu-id="3fb1f-116">Le sezioni seguenti descrivono le aggiunte al progetto quando è incluso il supporto per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-116">The following sections describe additions to the project when authentication support is included.</span></span>

### <a name="startup-class"></a><span data-ttu-id="3fb1f-117">Classe di avvio</span><span class="sxs-lookup"><span data-stu-id="3fb1f-117">Startup class</span></span>

<span data-ttu-id="3fb1f-118">La classe `Startup` presenta le aggiunte seguenti:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="3fb1f-119">In `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-119">In `Startup.ConfigureServices`:</span></span>

  * <span data-ttu-id="3fb1f-120">Identità con l'interfaccia utente predefinita:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="3fb1f-121">IdentityServer con un metodo helper <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> aggiuntivo che configura alcune convenzioni di ASP.NET Core predefinite in IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-121">IdentityServer with an additional <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method that sets up some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="3fb1f-122">Autenticazione con un metodo helper <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> aggiuntivo che configura l'app per convalidare i token JWT prodotti da IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-122">Authentication with an additional <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="3fb1f-123">In `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-123">In `Startup.Configure`:</span></span>

  * <span data-ttu-id="3fb1f-124">Il middleware di autenticazione responsabile della convalida delle credenziali della richiesta e dell'impostazione dell'utente nel contesto della richiesta:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="3fb1f-125">Il middleware IdentityServer che espone gli endpoint Open ID Connect (OIDC):</span><span class="sxs-lookup"><span data-stu-id="3fb1f-125">The IdentityServer middleware that exposes the Open ID Connect (OIDC) endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="3fb1f-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="3fb1f-126">AddApiAuthorization</span></span>

<span data-ttu-id="3fb1f-127">Il metodo helper <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> Configura [IdentityServer](https://identityserver.io/) per gli scenari di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-127">The <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method configures [IdentityServer](https://identityserver.io/) for ASP.NET Core scenarios.</span></span> <span data-ttu-id="3fb1f-128">IdentityServer è un Framework potente ed estendibile per la gestione dei problemi di sicurezza delle app.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="3fb1f-129">IdentityServer espone una complessità superflua per gli scenari più comuni.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-129">IdentityServer exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="3fb1f-130">Di conseguenza, viene fornito un set di convenzioni e opzioni di configurazione che consideriamo un valido punto di partenza.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-130">Consequently, a set of conventions and configuration options is provided that we consider a good starting point.</span></span> <span data-ttu-id="3fb1f-131">Una volta modificate le esigenze di autenticazione, è ancora disponibile la potenza completa di IdentityServer per personalizzare l'autenticazione per soddisfare i requisiti di un'app.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit an app's requirements.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="3fb1f-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="3fb1f-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="3fb1f-133">Il metodo helper <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> configura uno schema di criteri per l'app come gestore di autenticazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-133">The <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="3fb1f-134">Il criterio è configurato in modo da consentire all'identità di gestire tutte le richieste indirizzate a qualsiasi sottopercorso nello spazio URL identità `/Identity`.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-134">The policy is configured to allow Identity to handle all requests routed to any subpath in the Identity URL space `/Identity`.</span></span> <span data-ttu-id="3fb1f-135">Il <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> gestisce tutte le altre richieste.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-135">The <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> handles all other requests.</span></span> <span data-ttu-id="3fb1f-136">Inoltre, questo metodo:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-136">Additionally, this method:</span></span>

* <span data-ttu-id="3fb1f-137">Registra una risorsa API `{APPLICATION NAME}API` con IdentityServer con un ambito predefinito di `{APPLICATION NAME}API`.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-137">Registers an `{APPLICATION NAME}API` API resource with IdentityServer with a default scope of `{APPLICATION NAME}API`.</span></span>
* <span data-ttu-id="3fb1f-138">Configura il middleware del token di porta JWT per convalidare i token emessi da IdentityServer per l'app.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-138">Configures the JWT Bearer Token Middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="3fb1f-139">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="3fb1f-139">WeatherForecastController</span></span>

<span data-ttu-id="3fb1f-140">Nel `WeatherForecastController` (*Controllers/WeatherForecastController. cs*), l'attributo [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) viene applicato alla classe.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-140">In the `WeatherForecastController` (*Controllers/WeatherForecastController.cs*), the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute is applied to the class.</span></span> <span data-ttu-id="3fb1f-141">L'attributo indica che l'utente deve essere autorizzato in base ai criteri predefiniti per accedere alla risorsa.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-141">The attribute indicates that the user must be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="3fb1f-142">I criteri di autorizzazione predefiniti sono configurati per l'utilizzo dello schema di autenticazione predefinito, configurato dal <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> allo schema dei criteri indicato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-142">The default authorization policy is configured to use the default authentication scheme, which is set up by <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> to the policy scheme that was mentioned earlier.</span></span> <span data-ttu-id="3fb1f-143">Il metodo helper configura <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> come gestore predefinito per le richieste all'app.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-143">The helper method configures <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> as the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="3fb1f-144">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="3fb1f-144">ApplicationDbContext</span></span>

<span data-ttu-id="3fb1f-145">Nel `ApplicationDbContext` (*Data/ApplicationDbContext. cs*), lo stesso <xref:Microsoft.EntityFrameworkCore.DbContext> viene usato nell'identità con l'eccezione che estende <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> per includere lo schema per IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-145">In the `ApplicationDbContext` (*Data/ApplicationDbContext.cs*), the same <xref:Microsoft.EntityFrameworkCore.DbContext> is used in Identity with the exception that it extends <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> to include the schema for IdentityServer.</span></span> <span data-ttu-id="3fb1f-146">L'oggetto <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> è derivato da <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-146"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> is derived from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span></span>

<span data-ttu-id="3fb1f-147">Per ottenere il controllo completo dello schema del database, ereditare da una delle classi Identity <xref:Microsoft.EntityFrameworkCore.DbContext> disponibili e configurare il contesto per includere lo schema di identità chiamando `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` nel metodo `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-147">To gain full control of the database schema, inherit from one of the available Identity <xref:Microsoft.EntityFrameworkCore.DbContext> classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` in the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="3fb1f-148">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="3fb1f-148">OidcConfigurationController</span></span>

<span data-ttu-id="3fb1f-149">Nel `OidcConfigurationController` (*Controllers/OidcConfigurationController. cs*), viene eseguito il provisioning dell'endpoint client per gestire i parametri di OIDC.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-149">In the `OidcConfigurationController` (*Controllers/OidcConfigurationController.cs*), the client endpoint is provisioned to serve OIDC parameters.</span></span>

### <a name="app-settings-files"></a><span data-ttu-id="3fb1f-150">File di impostazioni dell'app</span><span class="sxs-lookup"><span data-stu-id="3fb1f-150">App settings files</span></span>

<span data-ttu-id="3fb1f-151">Nel file di impostazioni dell'app (*appSettings. JSON*) nella radice del progetto, nella sezione `IdentityServer` viene descritto l'elenco dei client configurati.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-151">In the app settings file (*appsettings.json*) at the project root, the `IdentityServer` section describes the list of configured clients.</span></span> <span data-ttu-id="3fb1f-152">Nell'esempio seguente è presente un singolo client.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-152">In the following example, there's a single client.</span></span> <span data-ttu-id="3fb1f-153">Il nome del client corrisponde al nome dell'app e viene mappato per convenzione al parametro OAuth `ClientId`.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-153">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="3fb1f-154">Il profilo indica il tipo di app da configurare.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-154">The profile indicates the app type being configured.</span></span> <span data-ttu-id="3fb1f-155">Il profilo viene utilizzato internamente per guidare le convenzioni che semplificano il processo di configurazione per il server.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-155">The profile is used internally to drive conventions that simplify the configuration process for the server.</span></span> <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "BlazorApplicationWithAuthentication.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

<span data-ttu-id="3fb1f-156">Nel file di impostazioni dell'app dell'ambiente di sviluppo (*appSettings. Development. JSON*) alla radice del progetto, nella sezione `IdentityServer` viene descritta la chiave usata per firmare i token.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-156">In the Development environment app settings file (*appsettings.Development.json*) at the project root, the `IdentityServer` section describes the key used to sign tokens.</span></span> <!-- When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section. -->

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="3fb1f-157">Configurazione dell'app client</span><span class="sxs-lookup"><span data-stu-id="3fb1f-157">Client app configuration</span></span>

### <a name="authentication-package"></a><span data-ttu-id="3fb1f-158">Pacchetto di autenticazione</span><span class="sxs-lookup"><span data-stu-id="3fb1f-158">Authentication package</span></span>

<span data-ttu-id="3fb1f-159">Quando viene creata un'app per l'uso di singoli account utente (`Individual`), l'app riceve automaticamente un riferimento al pacchetto per il `Microsoft.AspNetCore.Components.WebAssembly.Authentication` pacchetto nel file di progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-159">When an app is created to use Individual User Accounts (`Individual`), the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="3fb1f-160">Il pacchetto fornisce un set di primitive che consentono all'app di autenticare gli utenti e ottenere i token per chiamare le API protette.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-160">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="3fb1f-161">Se si aggiunge l'autenticazione a un'app, aggiungere manualmente il pacchetto al file di progetto dell'app:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-161">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="3fb1f-162">Sostituire `{VERSION}` nel riferimento al pacchetto precedente con la versione del pacchetto `Microsoft.AspNetCore.Blazor.Templates` illustrato nell'articolo <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-162">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

### <a name="api-authorization-support"></a><span data-ttu-id="3fb1f-163">Supporto dell'autorizzazione API</span><span class="sxs-lookup"><span data-stu-id="3fb1f-163">API authorization support</span></span>

<span data-ttu-id="3fb1f-164">Il supporto per l'autenticazione degli utenti è collegato al contenitore del servizio mediante il metodo di estensione fornito nel pacchetto di `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-164">The support for authenticating users is plugged into the service container by the extension method provided inside the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="3fb1f-165">Questo metodo configura tutti i servizi necessari per l'interazione dell'app con il sistema di autorizzazione esistente.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-165">This method sets up all the services needed for the app to interact with the existing authorization system.</span></span>

```csharp
builder.Services.AddApiAuthorization();
```

<span data-ttu-id="3fb1f-166">Per impostazione predefinita, carica la configurazione per l'app per convenzione da `_configuration/{client-id}`.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-166">By default, it loads the configuration for the app by convention from `_configuration/{client-id}`.</span></span> <span data-ttu-id="3fb1f-167">Per convenzione, l'ID client è impostato sul nome dell'assembly dell'app.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-167">By convention, the client ID is set to the app's assembly name.</span></span> <span data-ttu-id="3fb1f-168">Questo URL può essere modificato in modo che punti a un endpoint separato chiamando l'overload con options.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-168">This URL can be changed to point to a separate endpoint by calling the overload with options.</span></span>

### <a name="index-page"></a><span data-ttu-id="3fb1f-169">Pagina di indice</span><span class="sxs-lookup"><span data-stu-id="3fb1f-169">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a><span data-ttu-id="3fb1f-170">Componente app</span><span class="sxs-lookup"><span data-stu-id="3fb1f-170">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="3fb1f-171">Componente RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="3fb1f-171">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="3fb1f-172">Componente LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="3fb1f-172">LoginDisplay component</span></span>

<span data-ttu-id="3fb1f-173">Il componente `LoginDisplay` (*Shared/LoginDisplay. Razor*) viene sottoposto a rendering nel componente `MainLayout` (*Shared/MainLayout. Razor*) e gestisce i comportamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-173">The `LoginDisplay` component (*Shared/LoginDisplay.razor*) is rendered in the `MainLayout` component (*Shared/MainLayout.razor*) and manages the following behaviors:</span></span>

* <span data-ttu-id="3fb1f-174">Per utenti autenticati:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-174">For authenticated users:</span></span>
  * <span data-ttu-id="3fb1f-175">Visualizza il nome utente corrente.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-175">Displays the current user name.</span></span>
  * <span data-ttu-id="3fb1f-176">Offre un collegamento alla pagina del profilo utente nell'identità del ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-176">Offers a link to the user profile page in ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="3fb1f-177">Offre un pulsante per disconnettersi dall'app.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-177">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="3fb1f-178">Per utenti anonimi:</span><span class="sxs-lookup"><span data-stu-id="3fb1f-178">For anonymous users:</span></span>
  * <span data-ttu-id="3fb1f-179">Offre la possibilità di effettuare la registrazione.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-179">Offers the option to register.</span></span>
  * <span data-ttu-id="3fb1f-180">Offre la possibilità di effettuare l'accesso.</span><span class="sxs-lookup"><span data-stu-id="3fb1f-180">Offers the option to log in.</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        <a href="authentication/profile">Hello, @context.User.Identity.Name!</a>
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/register">Register</a>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```

### <a name="authentication-component"></a><span data-ttu-id="3fb1f-181">Componente di autenticazione</span><span class="sxs-lookup"><span data-stu-id="3fb1f-181">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="3fb1f-182">Componente FetchData</span><span class="sxs-lookup"><span data-stu-id="3fb1f-182">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
