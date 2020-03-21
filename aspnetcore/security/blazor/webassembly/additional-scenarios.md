---
title: Scenari di sicurezza aggiuntivi ASP.NET Core Blazor webassembly
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/additional-scenarios
ms.openlocfilehash: ccb512392341e3eea33f4ab45740b7900f7b63f9
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989475"
---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a>Scenari di sicurezza aggiuntivi del webassembly ASP.NET Core Blazer

Di [Javier Calvarro Nelson](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="handle-token-request-errors"></a>Gestione degli errori di richiesta di token

Quando un'applicazione a pagina singola (SPA) autentica un utente usando Open ID Connect (OIDC), lo stato di autenticazione viene gestito localmente all'interno della SPA e nel provider di identità (IP) sotto forma di cookie di sessione impostato come risultato dell'utente che fornisce le credenziali.

I token che l'IP emette per l'utente in genere sono validi per brevi periodi di tempo, circa un'ora in modo normale, quindi l'app client deve recuperare periodicamente nuovi token. In caso contrario, l'utente verrà disconnesso dopo la scadenza dei token concessi. Nella maggior parte dei casi, i client OIDC sono in grado di effettuare il provisioning di nuovi token senza richiedere all'utente di eseguire di nuovo l'autenticazione grazie allo stato di autenticazione o alla "sessione" mantenuta all'interno dell'IP.

In alcuni casi il client non è in grado di ottenere un token senza interazione dell'utente, ad esempio quando per qualche motivo l'utente si disconnette esplicitamente dall'IP. Questo scenario si verifica se un utente visita `https://login.microsoftonline.com` e si disconnette. In questi scenari, l'app non sa immediatamente che l'utente si è disconnesso. Tutti i token che il client include potrebbero non essere più validi. Inoltre, il client non è in grado di effettuare il provisioning di un nuovo token senza interazione dell'utente dopo la scadenza del token corrente.

Questi scenari non sono specifici dell'autenticazione basata su token. Sono parte della natura delle Spa. Una SPA che usa i cookie non riesce anche a chiamare un'API server se il cookie di autenticazione viene rimosso.

Quando un'app esegue chiamate API a risorse protette, è necessario tenere presente quanto segue:

* Per eseguire il provisioning di un nuovo token di accesso per chiamare l'API, l'utente potrebbe essere necessario eseguire di nuovo l'autenticazione.
* Anche se il client dispone di un token che sembra essere valido, la chiamata al server potrebbe non riuscire perché il token è stato revocato dall'utente.

Quando l'app richiede un token, esistono due possibili risultati:

* La richiesta ha esito positivo e l'app ha un token valido.
* La richiesta ha esito negativo e l'app deve autenticare di nuovo l'utente per ottenere un nuovo token.

Quando una richiesta di token ha esito negativo, è necessario decidere se si desidera salvare lo stato corrente prima di eseguire un reindirizzamento. Esistono diversi approcci con livelli di complessità crescenti:

* Archiviare lo stato della pagina corrente nell'archiviazione della sessione. Durante `OnInitializeAsync`, controllare se è possibile ripristinare lo stato prima di continuare.
* Aggiungere un parametro della stringa di query e usarlo come metodo per segnalare all'app che è necessario riattivare lo stato salvato in precedenza.
* Aggiungere un parametro della stringa di query con un identificatore univoco per archiviare i dati nell'archiviazione della sessione senza rischiare conflitti con altri elementi.

L'esempio seguente illustra come:

* Mantenere lo stato prima di reindirizzare alla pagina di accesso.
* Ripristinare lo stato precedente in seguito all'autenticazione utilizzando il parametro della stringa di query.

```razor
<EditForm Model="User" @onsubmit="OnSaveAsync">
    <label>User
        <InputText @bind-Value="User.Name" />
    </label>
    <label>Last name
        <InputText @bind-Value="User.LastName" />
    </label>
</EditForm>

@code {
    public class Profile
    {
        public string Name { get; set; }
        public string LastName { get; set; }
    }

    public Profile User { get; set; } = new Profile();

    protected async override Task OnInitializedAsync()
    {
        var currentQuery = new Uri(Navigation.Uri).Query;

        if (currentQuery.Contains("state=resumeSavingProfile"))
        {
            User = await JS.InvokeAsync<Profile>("sessionStorage.getState", 
                "resumeSavingProfile");
        }
    }

    public async Task OnSaveAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var resumeUri = Navigation.Uri + $"?state=resumeSavingProfile";

        var tokenResult = await AuthenticationService.RequestAccessToken(
            new AccessTokenRequestOptions
            {
                ReturnUrl = resumeUri
            });

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            await httpClient.PostJsonAsync("Save", User);
        }
        else
        {
            await JS.InvokeVoidAsync("sessionStorage.setState", 
                "resumeSavingProfile", User);
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }
    }
}
```

## <a name="save-app-state-before-an-authentication-operation"></a>Salva lo stato dell'app prima di un'operazione di autenticazione

Durante un'operazione di autenticazione, esistono casi in cui si vuole salvare lo stato dell'app prima che il browser venga reindirizzato all'indirizzo IP. Questo può succedere quando si usa un elemento come un contenitore di stato e si vuole ripristinare lo stato dopo l'autenticazione. È possibile usare un oggetto stato di autenticazione personalizzato per mantenere lo stato specifico dell'app o un riferimento a esso e ripristinare lo stato dopo che l'operazione di autenticazione è stata completata correttamente.

componente `Authentication` (*pages/Authentication. Razor*):

```razor
@page "/authentication/{action}"
@inject JSRuntime JS
@inject StateContainer State
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorViewCore Action="@Action" 
    AuthenticationState="AuthenticationState" OnLoginSucceded="RestoreState" 
    OnLogoutSucceded="RestoreState" />

@code {
    [Parameter]
    public string Action { get; set; }

    public class ApplicationAuthenticationState : RemoteAuthenticationState
    {
        public string Id { get; set; }
    }

    protected async override Task OnInitializedAsync()
    {
        if (RemoteAuthenticationActions.IsAction(RemoteAuthenticationActions.LogIn, 
            Action))
        {
            AuthenticationState.Id = Guid.NewGuid().ToString();
            await JS.InvokeVoidAsync("sessionStorage.setKey", 
                AuthenticationState.Id, State.Store());
        }
    }

    public async Task RestoreState(ApplicationAuthenticationState state)
    {
        var stored = await JS.InvokeAsync<string>("sessionStorage.getKey", 
            state.Id);
        State.FromStore(stored);
    }

    public ApplicationAuthenticationState AuthenticationState { get; set; } = 
        new ApplicationAuthenticationState();
}
```

## <a name="request-additional-access-tokens"></a>Richiedere token di accesso aggiuntivi

La maggior parte delle app richiede solo un token di accesso per interagire con le risorse protette che usano. In alcuni scenari, un'app potrebbe richiedere più di un token per interagire con due o più risorse. Il metodo `IAccessTokenProvider.RequestToken` fornisce un overload che consente a un'app di eseguire il provisioning di un token con un determinato set di ambiti, come illustrato nell'esempio seguente:

```csharp
var tokenResult = await AuthenticationService.RequestAccessToken(
    new AccessTokenRequestOptions
    {
        Scopes = new[] { "https://graph.microsoft.com/Mail.Send", 
            "https://graph.microsoft.com/User.Read" }
    });
```

## <a name="customize-app-routes"></a>Personalizzare le route delle app

Per impostazione predefinita, la libreria `Microsoft.AspNetCore.Components.WebAssembly.Authentication` usa le route mostrate nella tabella seguente per la rappresentazione di Stati di autenticazione diversi.

| Route                            | Scopo |
| -------------------------------- | ------- |
| `authentication/login`           | Attiva un'operazione di accesso. |
| `authentication/login-callback`  | Gestisce il risultato di qualsiasi operazione di accesso. |
| `authentication/login-failed`    | Visualizza i messaggi di errore quando l'operazione di accesso non riesce per qualche motivo. |
| `authentication/logout`          | Attiva un'operazione di disconnessione. |
| `authentication/logout-callback` | Gestisce il risultato di un'operazione di disconnessione. |
| `authentication/logout-failed`   | Visualizza i messaggi di errore quando l'operazione di disconnessione non riesce per qualche motivo. |
| `authentication/logged-out`      | Indica che l'utente ha eseguito correttamente la disconnessione. |
| `authentication/profile`         | Attiva un'operazione per modificare il profilo utente. |
| `authentication/register`        | Attiva un'operazione per registrare un nuovo utente. |

Le route visualizzate nella tabella precedente sono configurabili tramite `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`. Quando si impostano le opzioni per fornire route personalizzate, verificare che l'app disponga di una route che gestisce ogni percorso.

Nell'esempio seguente tutti i percorsi sono preceduti dal prefisso `/security`.

componente `Authentication` (*pages/Authentication. Razor*):

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

`Program.Main` (*Program.cs*):

```csharp
builder.Services.AddApiAuthorization(options => { 
    options.AuthenticationPaths.LogInPath = "security/login";
    options.AuthenticationPaths.LogInCallbackPath = "security/login-callback";
    options.AuthenticationPaths.LogInFailedPath = "security/login-failed";
    options.AuthenticationPaths.LogOutPath = "security/logout";
    options.AuthenticationPaths.LogOutCallbackPath = "security/logout-callback";
    options.AuthenticationPaths.LogOutFailedPath = "security/logout-failed";
    options.AuthenticationPaths.LogOutSucceededPath = "security/logged-out";
    options.AuthenticationPaths.ProfilePath = "security/profile";
    options.AuthenticationPaths.RegisterPath = "security/register";
});
```

Se il requisito richiede percorsi completamente diversi, impostare le route come descritto in precedenza ed eseguire il rendering del `RemoteAuthenticatorView` con un parametro di azione esplicito:

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

Se si sceglie di eseguire questa operazione, è possibile suddividere l'interfaccia utente in pagine diverse.

## <a name="customize-the-authentication-user-interface"></a>Personalizzare l'interfaccia utente di autenticazione

`RemoteAuthenticatorView` include un set predefinito di elementi dell'interfaccia utente per ogni stato di autenticazione. Ogni stato può essere personalizzato passando una `RenderFragment`personalizzata. Per personalizzare il testo visualizzato durante il processo di accesso iniziale, può modificare il `RemoteAuthenticatorView` come indicato di seguito.

componente `Authentication` (*pages/Authentication. Razor*):

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action">
    <LoggingIn>
        You are about to be redirected to https://login.microsoftonline.com.
    </LoggingIn>
</RemoteAuthenticatorView>

@code{
    [Parameter]
    public string Action { get; set; }
}
```

Il `RemoteAuthenticatorView` dispone di un frammento che può essere utilizzato per ogni route di autenticazione illustrato nella tabella seguente.

| Route                            | Fragment                |
| -------------------------------- | ----------------------- |
| `authentication/login`           | `<LoggingIn>`           |
| `authentication/login-callback`  | `<CompletingLoggingIn>` |
| `authentication/login-failed`    | `<LogInFailed>`         |
| `authentication/logout`          | `<LogOut>`              |
| `authentication/logout-callback` | `<CompletingLogOut>`    |
| `authentication/logout-failed`   | `<LogOutFailed>`        |
| `authentication/logged-out`      | `<LogOutSucceeded>`     |
| `authentication/profile`         | `<UserProfile>`         |
| `authentication/register`        | `<Registering>`         |
