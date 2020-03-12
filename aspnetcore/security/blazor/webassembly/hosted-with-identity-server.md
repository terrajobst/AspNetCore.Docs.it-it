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
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-identity-server"></a>Proteggere un ASP.NET Core Blazor app ospitata tramite webassembly con Identity Server

Di [Javier Calvarro Nelson](https://github.com/javiercn) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Per creare una nuova Blazor app ospitata in Visual Studio che usa [IdentityServer](https://identityserver.io/) per autenticare gli utenti e le chiamate API:

1. Usare Visual Studio per creare una nuova app **Blazor webassembly** . Per altre informazioni, vedere <xref:blazor/get-started>.
1. Nella finestra di dialogo **Crea una nuova app Blazor** selezionare **modifica** nella sezione **autenticazione** .
1. Selezionare **account utente singoli** e quindi **OK**.
1. Nella sezione **Avanzate** Selezionare la casella di controllo **ASP.NET Core Hosted** .
1. Selezionare il pulsante **Crea**.

Per creare l'app in una shell dei comandi, eseguire il comando seguente:

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

Per specificare il percorso di output, che crea una cartella di progetto, se non esiste, includere l'opzione di output nel comando con un percorso, ad esempio `-o BlazorSample`. Il nome della cartella diventa anche parte del nome del progetto.

## <a name="server-app-configuration"></a>Configurazione dell'app Server

Le sezioni seguenti descrivono le aggiunte al progetto quando è incluso il supporto per l'autenticazione.

### <a name="startup-class"></a>Classe di avvio

La classe `Startup` presenta le aggiunte seguenti:

* In `Startup.ConfigureServices`:

  * Identità con l'interfaccia utente predefinita:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * IdentityServer con un metodo helper <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> aggiuntivo che configura alcune convenzioni di ASP.NET Core predefinite in IdentityServer:

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * Autenticazione con un metodo helper <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> aggiuntivo che configura l'app per convalidare i token JWT prodotti da IdentityServer:

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* In `Startup.Configure`:

  * Il middleware di autenticazione responsabile della convalida delle credenziali della richiesta e dell'impostazione dell'utente nel contesto della richiesta:

    ```csharp
    app.UseAuthentication();
    ```

  * Il middleware IdentityServer che espone gli endpoint Open ID Connect (OIDC):

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

Il metodo helper <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> Configura [IdentityServer](https://identityserver.io/) per gli scenari di ASP.NET Core. IdentityServer è un Framework potente ed estendibile per la gestione dei problemi di sicurezza delle app. IdentityServer espone una complessità superflua per gli scenari più comuni. Di conseguenza, viene fornito un set di convenzioni e opzioni di configurazione che consideriamo un valido punto di partenza. Una volta modificate le esigenze di autenticazione, è ancora disponibile la potenza completa di IdentityServer per personalizzare l'autenticazione per soddisfare i requisiti di un'app.

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt

Il metodo helper <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> configura uno schema di criteri per l'app come gestore di autenticazione predefinito. Il criterio è configurato in modo da consentire all'identità di gestire tutte le richieste indirizzate a qualsiasi sottopercorso nello spazio URL identità `/Identity`. Il <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> gestisce tutte le altre richieste. Inoltre, questo metodo:

* Registra una risorsa API `{APPLICATION NAME}API` con IdentityServer con un ambito predefinito di `{APPLICATION NAME}API`.
* Configura il middleware del token di porta JWT per convalidare i token emessi da IdentityServer per l'app.

### <a name="weatherforecastcontroller"></a>WeatherForecastController

Nel `WeatherForecastController` (*Controllers/WeatherForecastController. cs*), l'attributo [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) viene applicato alla classe. L'attributo indica che l'utente deve essere autorizzato in base ai criteri predefiniti per accedere alla risorsa. I criteri di autorizzazione predefiniti sono configurati per l'utilizzo dello schema di autenticazione predefinito, configurato dal <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> allo schema dei criteri indicato in precedenza. Il metodo helper configura <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> come gestore predefinito per le richieste all'app.

### <a name="applicationdbcontext"></a>ApplicationDbContext

Nel `ApplicationDbContext` (*Data/ApplicationDbContext. cs*), lo stesso <xref:Microsoft.EntityFrameworkCore.DbContext> viene usato nell'identità con l'eccezione che estende <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> per includere lo schema per IdentityServer. L'oggetto <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> è derivato da <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.

Per ottenere il controllo completo dello schema del database, ereditare da una delle classi Identity <xref:Microsoft.EntityFrameworkCore.DbContext> disponibili e configurare il contesto per includere lo schema di identità chiamando `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` nel metodo `OnModelCreating`.

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController

Nel `OidcConfigurationController` (*Controllers/OidcConfigurationController. cs*), viene eseguito il provisioning dell'endpoint client per gestire i parametri di OIDC.

### <a name="app-settings-files"></a>File di impostazioni dell'app

Nel file di impostazioni dell'app (*appSettings. JSON*) nella radice del progetto, nella sezione `IdentityServer` viene descritto l'elenco dei client configurati. Nell'esempio seguente è presente un singolo client. Il nome del client corrisponde al nome dell'app e viene mappato per convenzione al parametro OAuth `ClientId`. Il profilo indica il tipo di app da configurare. Il profilo viene utilizzato internamente per guidare le convenzioni che semplificano il processo di configurazione per il server. <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "BlazorApplicationWithAuthentication.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

Nel file di impostazioni dell'app dell'ambiente di sviluppo (*appSettings. Development. JSON*) alla radice del progetto, nella sezione `IdentityServer` viene descritta la chiave usata per firmare i token. <!-- When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section. -->

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="client-app-configuration"></a>Configurazione dell'app client

### <a name="authentication-package"></a>Pacchetto di autenticazione

Quando viene creata un'app per l'uso di singoli account utente (`Individual`), l'app riceve automaticamente un riferimento al pacchetto per il `Microsoft.AspNetCore.Components.WebAssembly.Authentication` pacchetto nel file di progetto dell'app. Il pacchetto fornisce un set di primitive che consentono all'app di autenticare gli utenti e ottenere i token per chiamare le API protette.

Se si aggiunge l'autenticazione a un'app, aggiungere manualmente il pacchetto al file di progetto dell'app:

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

Sostituire `{VERSION}` nel riferimento al pacchetto precedente con la versione del pacchetto `Microsoft.AspNetCore.Blazor.Templates` illustrato nell'articolo <xref:blazor/get-started>.

### <a name="api-authorization-support"></a>Supporto dell'autorizzazione API

Il supporto per l'autenticazione degli utenti è collegato al contenitore del servizio mediante il metodo di estensione fornito nel pacchetto di `Microsoft.AspNetCore.Components.WebAssembly.Authentication`. Questo metodo configura tutti i servizi necessari per l'interazione dell'app con il sistema di autorizzazione esistente.

```csharp
builder.Services.AddApiAuthorization();
```

Per impostazione predefinita, carica la configurazione per l'app per convenzione da `_configuration/{client-id}`. Per convenzione, l'ID client è impostato sul nome dell'assembly dell'app. Questo URL può essere modificato in modo che punti a un endpoint separato chiamando l'overload con options.

### <a name="index-page"></a>Pagina di indice

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a>Componente app

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>Componente RedirectToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>Componente LoginDisplay

Il componente `LoginDisplay` (*Shared/LoginDisplay. Razor*) viene sottoposto a rendering nel componente `MainLayout` (*Shared/MainLayout. Razor*) e gestisce i comportamenti seguenti:

* Per utenti autenticati:
  * Visualizza il nome utente corrente.
  * Offre un collegamento alla pagina del profilo utente nell'identità del ASP.NET Core.
  * Offre un pulsante per disconnettersi dall'app.
* Per utenti anonimi:
  * Offre la possibilità di effettuare la registrazione.
  * Offre la possibilità di effettuare l'accesso.

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

### <a name="authentication-component"></a>Componente di autenticazione

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>Componente FetchData

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
