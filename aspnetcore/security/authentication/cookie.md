---
title: "Utilizzo dell'autenticazione Cookie senza identità ASP.NET Core"
author: rick-anderson
description: Una spiegazione dell'utilizzo dell'autenticazione cookie senza ASP.NET Identity Core
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: bbc49a0d3ede66ad07ec3f1dea055cae5fec39ff
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>Utilizzo dell'autenticazione Cookie senza identità ASP.NET Core

Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)

Come si è visto negli argomenti precedenti autenticazione [ASP.NET Identity Core](xref:security/authentication/identity) è un provider di autenticazione completa e completa per creare e gestire gli account di accesso. Tuttavia, si consiglia di utilizzare la logica di autenticazione personalizzato con l'autenticazione basata su cookie in alcuni casi. È possibile utilizzare l'autenticazione basata su cookie come un provider di autenticazione autonomo senza ASP.NET Identity Core.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

Per informazioni sulla migrazione di autenticazione basato su cookie da ASP.NET Core 1. x a 2.0, vedere [la migrazione di autenticazione e identità per ASP.NET 2.0 Core argomento (basato su Cookie di autenticazione)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).

## <a name="configuration"></a>Configurazione

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se non si utilizza il [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), installare la versione 2.0 di [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacchetto NuGet.

Nel `ConfigureServices` (metodo), creare il servizio di Middleware di autenticazione con il `AddAuthentication` e `AddCookie` metodi:

[!code-csharp[](cookie/sample/Startup.cs?name=snippet1)]

`AuthenticationScheme` passato a `AddAuthentication` imposta lo schema di autenticazione predefinito per l'app. `AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione dei cookie e si desidera [autorizzazione con uno schema specifico](xref:security/authorization/limitingidentitybyscheme). L'impostazione di `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` fornisce un valore pari a "Cookie" per lo schema. È possibile fornire qualsiasi valore stringa che distingue lo schema.

Nel `Configure` metodo, utilizzare il `UseAuthentication` metodo da richiamare il Middleware di autenticazione che imposta il `HttpContext.User` proprietà. Chiamare il `UseAuthentication` metodo prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:

[!code-csharp[](cookie/sample/Startup.cs?name=snippet2)]

**Opzioni AddCookie**

Il [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) classe viene utilizzata per configurare le opzioni del provider di autenticazione.

| Opzione | Descrizione |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | Fornisce il percorso di fornire un 302 trovato (reindirizzamento dell'URL) quando sono attivati da `HttpContext.ForbidAsync`. Il valore predefinito è `/Account/AccessDenied`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | L'autorità emittente da utilizzare per il [dell'autorità di certificazione](/dotnet/api/system.security.claims.claim.issuer) proprietà in tutte le attestazioni creata dal servizio di autenticazione del cookie. |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | Il nome di dominio in cui il cookie viene servito. Per impostazione predefinita, questo è il nome host della richiesta. Il browser invia solo il cookie nelle richieste inviate a un nome host corrispondente. Si potrebbe voler modificare questa opzione per disporre i cookie disponibili per tutti gli host nel dominio. Ad esempio, impostando il dominio del cookie `.contoso.com` rende disponibili agli `contoso.com`, `www.contoso.com`, e `staging.www.contoso.com`. |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | Ottiene o imposta la durata di un cookie. Attualmente, questa opzione no ops e diventerà obsoleta in ASP.NET Core 2.1 +. Utilizzare il `ExpireTimeSpan` opzione per impostare la scadenza del cookie. Per ulteriori informazioni, vedere [chiarire il comportamento di CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293). |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | Flag che indica se il cookie deve essere accessibile solo ai server. Modifica questo valore su `false` consenta script lato client per accedere al cookie e possono aprire l'app al rischio di furto di cookie che l'app deve avere un [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerabilità. Il valore predefinito è `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Imposta il nome del cookie. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | Utilizzato per isolare le app eseguono lo stesso nome host. Se si dispone di un'app in esecuzione in `/app1` e desidera limitare i cookie per tale app, impostare il `CookiePath` proprietà `/app1`. In questo modo, il cookie è disponibile solo per le richieste per `/app1` e tutte le app disponibili al suo interno. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | Indica se il browser deve consentire i cookie da collegare alle richieste di nello stesso sito solo (`SameSiteMode.Strict`) o le richieste tra siti tramite i metodi HTTP sicuro e richieste di nello stesso sito (`SameSiteMode.Lax`). Se impostato su `SameSiteMode.None`, non è impostato il valore dell'intestazione cookie. Si noti che [Middleware criteri Cookie](#cookie-policy-middleware) potrebbe sovrascrivere il valore fornito dall'utente. Per supportare l'autenticazione OAuth, il valore predefinito è `SameSiteMode.Lax`. Per ulteriori informazioni, vedere [autenticazione OAuth interrotto a causa dei criteri di cookie SameSite](https://github.com/aspnet/Security/issues/1231). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | Flag che indica se il cookie creato deve essere limitato a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o lo stesso protocollo della richiesta (`CookieSecurePolicy.SameAsRequest`). Il valore predefinito è `CookieSecurePolicy.SameAsRequest`. |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | Imposta il `DataProtectionProvider` che viene utilizzato per creare il valore predefinito `TicketDataFormat`. Se il `TicketDataFormat` è impostata, il `DataProtectionProvider` non viene usata l'opzione. Se omesso, viene utilizzato provider di protezione dati predefinito dell'app. |
| [Eventi](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | Il gestore chiama i metodi nel provider che offrono il controllo di app in alcune fasi di elaborazione. Se `Events` non fornito, viene fornita un'istanza predefinita che non esegue alcuna operazione quando i metodi vengono chiamati. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | Utilizzato come tipo di servizio per ottenere il `Events` istanza anziché la proprietà. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | Il `TimeSpan` dopo che il ticket di autenticazione archiviato il cookie scade. `ExpireTimeSpan` viene aggiunto all'ora corrente per creare l'ora di scadenza per il ticket. Il `ExpiredTimeSpan` valore diventa sempre il ticket di autenticazione crittografato verificati dal server. Può inoltre analizzare le [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) intestazione, ma solo se `IsPersistent` è impostata. Per impostare `IsPersistent` a `true`, configurare il [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passato a `SignInAsync`. Il valore predefinito di `ExpireTimeSpan` è 14 giorni. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | Fornisce il percorso di fornire un 302 trovato (reindirizzamento dell'URL) quando sono attivati da `HttpContext.ChallengeAsync`. L'URL corrente che ha generato il codice 401 viene aggiunto per il `LoginPath` come parametro di stringa di query denominato dal `ReturnUrlParameter`. Una volta una richiesta per il `LoginPath` concede una nuova identità di accesso, il `ReturnUrlParameter` valore viene utilizzato per reindirizzare il browser all'URL che ha causato il codice di stato unauthorized originale. Il valore predefinito è `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | Se il `LogoutPath` viene fornito per il gestore, quindi reindirizza una richiesta per il percorso in base al valore del `ReturnUrlParameter`. Il valore predefinito è `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | Determina il nome del parametro della stringa di query che viene aggiunto dal gestore di risposta 302 trovato (reindirizzamento dell'URL). `ReturnUrlParameter` viene utilizzato quando arriva una richiesta di `LoginPath` o `LogoutPath` per restituire il browser all'URL originale al termine dell'operazione di connessione o disconnessione. Il valore predefinito è `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | Un contenitore facoltativo utilizzato per archiviare l'identità in tutte le richieste. Se utilizzato, solo un identificatore di sessione viene inviato al client. `SessionStore` Consente di attenuare i potenziali problemi con le identità di grandi dimensioni. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | Un flag che indica se un nuovo cookie con una scadenza aggiornato deve essere visualizzato in modo dinamico. Questa situazione può verificarsi in qualsiasi richiesta in cui il periodo di scadenza cookie corrente è superiore al 50% scaduto. La nuova data di scadenza viene spostata in avanti la data corrente più il `ExpireTimespan`. Un [data di scadenza assoluto cookie](xref:security/authentication/cookie#absolute-cookie-expiration) può essere impostato utilizzando il `AuthenticationProperties` classe quando si chiama `SignInAsync`. Un'ora di scadenza assoluto può migliorare la sicurezza dell'app, limitando il periodo di tempo che il cookie di autenticazione è valido. Il valore predefinito è `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | Il `TicketDataFormat` viene utilizzata per proteggere e annullare la protezione dell'identità e altre proprietà che vengono archiviate nel valore del cookie. Se non specificato, un `TicketDataFormat` viene creato utilizzando il [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0). |
| [Convalidare](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | Metodo che verifica che le opzioni sono valide. |

Impostare `CookieAuthenticationOptions` nella configurazione del servizio per l'autenticazione nel `ConfigureServices` metodo:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 1. x utilizza cookie [middleware](xref:fundamentals/middleware/index) che serializza un'entità utente in un cookie crittografato. Nelle richieste successive, il cookie viene convalidato e ricreato e assegnata l'entità di `HttpContext.User` proprietà.

Installare il [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacchetto NuGet nel progetto. Questo pacchetto contiene middleware del cookie.

Utilizzare il `UseCookieAuthentication` metodo nel `Configure` metodo i *Startup.cs* file prima di `UseMvc` o `UseMvcWithDefaultRoute`:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

**Opzioni di CookieAuthenticationOptions**

Il [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) classe viene utilizzata per configurare le opzioni del provider di autenticazione.

| Opzione | Descrizione |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | Imposta lo schema di autenticazione. `AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione e si desidera [autorizzazione con uno schema specifico](xref:security/authorization/limitingidentitybyscheme). L'impostazione di `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` fornisce un valore pari a "Cookie" per lo schema. È possibile fornire qualsiasi valore stringa che distingue lo schema. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Imposta un valore per indicare che l'autenticazione dei cookie di eseguire in ogni richiesta e tenta di convalidare e ricostruire qualsiasi entità serializzato che è creato. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | Se true, il middleware di autenticazione gestisce sfide automatico. Se false, il middleware di autenticazione altera solo le risposte quando richiesto esplicitamente dal `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | L'autorità emittente da utilizzare per il [dell'autorità di certificazione](/dotnet/api/system.security.claims.claim.issuer) proprietà in tutte le attestazioni creato dal middleware di autenticazione del cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | Il nome di dominio in cui il cookie viene servito. Per impostazione predefinita, questo è il nome host della richiesta. Il browser opera solo il cookie a un nome host corrispondente. Si potrebbe voler modificare questa opzione per disporre i cookie disponibili per tutti gli host nel dominio. Ad esempio, impostando il dominio del cookie `.contoso.com` rende disponibili agli `contoso.com`, `www.contoso.com`, e `staging.www.contoso.com`. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | Flag che indica se il cookie deve essere accessibile solo ai server. Modifica questo valore su `false` consenta script lato client per accedere al cookie e possono aprire l'app al rischio di furto di cookie che l'app deve avere un [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerabilità. Il valore predefinito è `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | Utilizzato per isolare le app eseguono lo stesso nome host. Se si dispone di un'app in esecuzione in `/app1` e desidera limitare i cookie per tale app, impostare il `CookiePath` proprietà `/app1`. In questo modo, il cookie è disponibile solo per le richieste per `/app1` e tutte le app disponibili al suo interno. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | Flag che indica se il cookie creato deve essere limitato a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o lo stesso protocollo della richiesta (`CookieSecurePolicy.SameAsRequest`). Il valore predefinito è `CookieSecurePolicy.SameAsRequest`. |
| [Descrizione](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | Informazioni aggiuntive sul tipo di autenticazione viene resa disponibile per l'app. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | Il `TimeSpan` dopo cui scade il ticket di autenticazione. Viene aggiunto all'ora corrente per creare l'ora di scadenza per il ticket. Per utilizzare `ExpireTimeSpan`, è necessario impostare `IsPersistent` a `true` nel `AuthenticationProperties` passato a `SignInAsync`. Il valore predefinito è 14 giorni. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | Un flag che indica se la data di scadenza del cookie viene reimpostato quando più di semestre di `ExpireTimeSpan` trascorso l'intervallo. La nuova ora exipiration viene spostata in avanti la data corrente più il `ExpireTimespan`. Un [data di scadenza assoluto cookie](xref:security/authentication/cookie#absolute-cookie-expiration) può essere impostato utilizzando il `AuthenticationProperties` classe quando si chiama `SignInAsync`. Un'ora di scadenza assoluto può migliorare la sicurezza dell'app, limitando il periodo di tempo che il cookie di autenticazione è valido. Il valore predefinito è `true`. |

Impostare `CookieAuthenticationOptions` per il Middleware di autenticazione nel Cookie di `Configure` metodo:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a>Middleware criteri cookie

[Cookie criteri Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) consente le funzionalità di criteri di cookie in un'applicazione. L'aggiunta di middleware alla pipeline di elaborazione app è ordine sensibili; riguarda solo i componenti registrati dopo di esso nella pipeline.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 Il [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) fornito per il Middleware di criteri di Cookie consentono di controllare caratteristiche globali del cookie elaborazione e l'hook in elaborazione cookie gestori quando i cookie vengono aggiunti o eliminati.

| Proprietà | Descrizione |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Interessa se i cookie devono essere HttpOnly, che è un flag che indica se il cookie deve essere accessibile solo ai server. Il valore predefinito è `HttpOnlyPolicy.None`. |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Interessa attributo nello stesso sito del cookie (vedere sotto). Il valore predefinito è `SameSiteMode.Lax`. Questa opzione è disponibile per i componenti di base di ASP.NET 2.0 +. |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Chiamato quando viene aggiunto un cookie. |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Chiamato quando viene eliminato un cookie. |
| [Proteggere](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Determina se i cookie devono essere protetto. Il valore predefinito è `CookieSecurePolicy.None`. |

**MinimumSameSitePolicy** (ASP.NET Core 2.0 + solo)

Il valore predefinito `MinimumSameSitePolicy` valore `SameSiteMode.Lax` per consentire l'autenticazione OAuth2. Per applicare rigorosamente criteri nello stesso sito di `SameSiteMode.Strict`, impostare il `MinimumSameSitePolicy`. Anche se questa impostazione di interruzioni di OAuth2 e altri schemi di autenticazione tra le origini, eleva il livello di sicurezza dei cookie per altri tipi di App che non richiedono l'elaborazione della richiesta multiorigine.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

L'impostazione di Middleware di criteri di Cookie per `MinimumSameSitePolicy` possono influenzare l'impostazione di `Cookie.SameSite` in `CookieAuthenticationOptions` impostazioni in base alla matrice di seguito.

| MinimumSameSitePolicy | Cookie.SameSite | Impostazione Cookie.SameSite risultante |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="creating-an-authentication-cookie"></a>Creazione di un cookie di autenticazione

Per creare un cookie contenente le informazioni sull'utente, è necessario costruire un [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal). Le informazioni utente vengano serializzate e memorizzate nel cookie. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Creare un [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) con le necessarie [attestazione](/dotnet/api/system.security.claims.claim)s e chiamare [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) per l'accesso dell'utente:

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Chiamare [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) per l'accesso dell'utente:

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

`SignInAsync` Crea un cookie crittografato e lo aggiunge alla risposta corrente. Se non si specifica un `AuthenticationScheme`, viene utilizzato lo schema predefinito.

Dietro le quinte, la crittografia utilizzata è ASP.NET Core [la protezione dei dati](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema. Se si ospita l'applicazione in più computer, il bilanciamento del carico tra App o utilizzando una web farm, quindi è necessario [configurare la protezione dati](xref:security/data-protection/configuration/overview) per utilizzare la stessa chiave anello e identificatore dell'app.

## <a name="signing-out"></a>La disconnessione

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Per effettuare la disconnessione dell'utente corrente ed eliminare i cookie, chiamare [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Per effettuare la disconnessione dell'utente corrente ed eliminare i cookie, chiamare [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

Se non si utilizza `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookie") per lo schema (ad esempio, "ContosoCookie"), specificare lo schema usato durante la configurazione del provider di autenticazione. In caso contrario, viene utilizzato lo schema predefinito.

## <a name="reacting-to-back-end-changes"></a>Reazione alle modifiche di back-end

Dopo aver creato un cookie, diventa l'unica origine di identità. Anche se si disabilita un utente nei sistemi back-end, il sistema di autenticazione cookie non ha alcuna conoscenza di questo oggetto e un utente rimane connesso, purché i cookie è valido.

Il [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) evento in ASP.NET Core 2. x o [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metodo in ASP.NET Core 1. x consente di intercettare ed eseguire l'override di convalida dell'identità del cookie. Questo approccio riduce il rischio di utenti revocati l'accesso all'app.

Un approccio alla convalida dei cookie è basato su tenere traccia di quando il database utente è stato modificato. Se il database non è stato modificato perché è stato emesso il cookie dell'utente, non è necessario autenticare nuovamente l'utente se il cookie è ancora valido. Implementare questo scenario, il database, che viene implementato in `IUserRepository` per questo esempio, archivia un `LastChanged` valore. Quando un utente viene aggiornato nel database, il `LastChanged` è impostato sull'ora corrente.

Per invalidare un cookie quando le modifiche del database basano sul `LastChanged` valore, creare il cookie con una `LastChanged` attestazione contenente corrente `LastChanged` valore dal database:

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Per implementare un override per il `ValidatePrincipal` evento, scrivere un metodo con la firma seguente in una classe derivata da [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Un esempio è simile al seguente:

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Registrare l'istanza di eventi durante la registrazione del servizio nel cookie di `ConfigureServices` metodo. Fornire una registrazione di servizio con ambito per il `CustomCookieAuthenticationEvents` classe:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Per implementare un override per il `ValidateAsync` eventi, scrivere un metodo con la firma seguente:

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Identity Core implementa questo controllo come parte del relativo [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync). Un esempio è simile al seguente:

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

Registrare l'evento durante la configurazione dell'autenticazione nel cookie di `Configure` metodo:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

Si consideri una situazione in cui viene aggiornato il nome dell'utente &mdash; una decisione che non influisce sulla sicurezza in alcun modo. Se si desidera aggiornare distruttivo l'entità utente, chiamare `context.ReplacePrincipal` e impostare il `context.ShouldRenew` proprietà `true`.

> [!WARNING]
> L'approccio descritto di seguito viene attivata ogni richiesta. Ciò può comportare una riduzione delle prestazioni di grandi dimensioni per l'app.

## <a name="persistent-cookies"></a>Cookie permanenti

È possibile che il cookie per rendere persistenti le sessioni del browser. Questo tipo di persistenza deve essere abilitata solo con il consenso esplicito utente con un controllo checkbox "Memorizza dati" su account di accesso o un meccanismo simile. 

Il seguente frammento di codice crea un'identità e i cookie corrispondente che resta tramite chiusure browser. Le impostazioni di scadenza variabile configurate in precedenza vengono rispettate. Se il cookie scade mentre il browser viene chiuso, il browser Cancella cookie quando viene riavviato.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

Il [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) risiede nella classe di `Microsoft.AspNetCore.Authentication` dello spazio dei nomi.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

Il [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) risiede nella classe di `Microsoft.AspNetCore.Http.Authentication` dello spazio dei nomi.

---

## <a name="absolute-cookie-expiration"></a>Scadenza del cookie assoluto

È possibile impostare un'ora di scadenza assoluto con `ExpiresUtc`. È inoltre necessario impostare `IsPersistent`; in caso contrario, `ExpiresUtc` viene ignorato e viene creato un cookie di sessione singola. Quando `ExpiresUtc` è impostato su `SignInAsync`, sostituisce il valore del `ExpireTimeSpan` opzione di `CookieAuthenticationOptions`, se impostata.

Il seguente frammento di codice crea un'identità e i cookie corrispondente che la durata è di 20 minuti. Questo ignora eventuali impostazioni di scadenza variabile configurati in precedenza.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="see-also"></a>Vedere anche

* [Modifiche di autenticazione 2.0 / migrazione annuncio](https://github.com/aspnet/Announcements/issues/262)
* [Limitazione dell'identità tramite schema](xref:security/authorization/limitingidentitybyscheme)
* [Autorizzazione basata sulle attestazioni](xref:security/authorization/claims)
* [Controlli del ruolo basata su criteri](xref:security/authorization/roles#policy-based-role-checks)
