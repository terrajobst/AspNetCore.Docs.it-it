---
title: Usare l'autenticazione tramite cookie senza ASP.NET Core Identity
author: rick-anderson
description: Una spiegazione dell'uso l'autenticazione tramite cookie senza ASP.NET Core Identity
ms.author: riande
ms.date: 02/25/2019
uid: security/authentication/cookie
ms.openlocfilehash: f3e02b357a83cf5fc4b9fcdc79b2fbe80da98507
ms.sourcegitcommit: 9691b742134563b662948b0ed63f54ef7186801e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/10/2019
ms.locfileid: "66824754"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Usare l'autenticazione tramite cookie senza ASP.NET Core Identity

Dal [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)

Come si è visto negli argomenti precedenti authentication [ASP.NET Core Identity](xref:security/authentication/identity) è un provider di autenticazione completa e completo per creare e gestire gli account di accesso. Tuttavia, è possibile usare la propria logica di autenticazione personalizzato con autenticazione basata su cookie in alcuni casi. È possibile usare l'autenticazione basata su cookie come un provider di autenticazione autonomo senza ASP.NET Core Identity.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

A scopo dimostrativo nell'app di esempio, l'account utente per l'utente ipotetica, Maria Rodriguez, è hardcoded nell'app. Usare il nome utente di posta elettronica "maria.rodriguez@contoso.com" e qualsiasi password di accesso dell'utente. L'utente viene autenticato nel `AuthenticateUser` metodo nella *Pages/Account/Login.cshtml.cs* file. In un esempio reale, l'utente viene autenticato in un database.

Per informazioni sulla migrazione di autenticazione basata su cookie da ASP.NET Core 1.x a 2.0, vedere [eseguire la migrazione di autenticazione e identità ad ASP.NET Core 2.0 argomento (autenticazione basata su Cookie)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).

Per usare ASP.NET Core Identity, vedere la [Introduzione all'identità](xref:security/authentication/identity) argomento.

## <a name="configuration"></a>Configurazione

::: moniker range=">= aspnetcore-2.0"

Se l'app non usa la [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), creare un riferimento al pacchetto nel file di progetto per il [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (versione 2.1.0 o in un secondo momento).

Nel `ConfigureServices` metodo, creare il servizio di Middleware di autenticazione con il `AddAuthentication` e `AddCookie` metodi:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

`AuthenticationScheme` passato a `AddAuthentication` imposta lo schema di autenticazione predefinito per l'app. `AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione dei cookie e si desidera [autorizzare con uno schema specifico](xref:security/authorization/limitingidentitybyscheme). Impostando il `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` fornisce un valore di "Cookie" per lo schema. È possibile specificare qualsiasi valore stringa che distingue lo schema.

Schema di autenticazione dell'app è diverso dallo schema di autenticazione cookie dell'app. Quando non viene fornito uno schema di autenticazione del cookie a <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, viene utilizzato [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("cookie").

Il cookie di autenticazione <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> è impostata su `true` per impostazione predefinita. I cookie di autenticazione sono consentiti quando un visitatore non ha fornito il consenso alla raccolta di dati. Per altre informazioni, vedere <xref:security/gdpr#essential-cookies>.

Nel `Configure` metodo, usare il `UseAuthentication` metodo da richiamare il Middleware di autenticazione che consente di impostare il `HttpContext.User` proprietà. Chiamare il `UseAuthentication` metodo prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

**Opzioni AddCookie**

Il [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) classe viene utilizzata per configurare le opzioni del provider di autenticazione.

| Opzione | Descrizione |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | Fornisce il percorso per fornire un 302 trovato (reindirizzamento URL) quando viene attivata da `HttpContext.ForbidAsync`. Il valore predefinito è `/Account/AccessDenied`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | L'autorità emittente da usare per la [Issuer](/dotnet/api/system.security.claims.claim.issuer) proprietà in tutte le attestazioni creato dal servizio di autenticazione del cookie. |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | Il nome di dominio in cui viene servito il cookie. Per impostazione predefinita, questo è il nome host della richiesta. Il browser invia solo il cookie nelle richieste a un nome host corrispondente. È possibile modificare questa opzione per disporre i cookie disponibili a tutti gli host nel dominio. Ad esempio, l'impostazione del dominio del cookie `.contoso.com` renderli disponibili agli `contoso.com`, `www.contoso.com`, e `staging.www.contoso.com`. |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | Un flag che indica se il cookie deve essere accessibile solo ai server. Se si modifica questo valore per `false` consente a script lato client per accedere al cookie e può aprire l'app al furto di cookie che l'app deve avere una [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerabilità. Il valore predefinito è `true`. |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | Imposta il nome del cookie. |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | Consente di isolare le App in esecuzione sullo stesso nome host. Se si dispone di un'app in esecuzione in `/app1` e si desidera limitare i cookie per tale app, impostare il `CookiePath` proprietà `/app1`. In questo modo, il cookie è disponibile solo nelle richieste al `/app1` e a qualsiasi app in esso contenuti. |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | Indica se il browser deve consentire i cookie da allegare al solo le richieste nello stesso sito (`SameSiteMode.Strict`) o le richieste intersito tramite i metodi HTTP sicuro e nello stesso sito richieste (`SameSiteMode.Lax`). Se impostato su `SameSiteMode.None`, non è impostato il valore dell'intestazione cookie. Si noti che [Middleware dei Cookie criteri](#cookie-policy-middleware) potrebbe sovrascrivere il valore fornito dall'utente. Per supportare l'autenticazione OAuth, il valore predefinito è `SameSiteMode.Lax`. Per altre informazioni, vedere [l'autenticazione OAuth interrotto a causa di criteri per i cookie SameSite](https://github.com/aspnet/Security/issues/1231). |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | Un flag che indica se il cookie creato deve essere limitato a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o lo stesso protocollo della richiesta (`CookieSecurePolicy.SameAsRequest`). Il valore predefinito è `CookieSecurePolicy.SameAsRequest`. |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | Imposta il `DataProtectionProvider` che consente di creare il valore predefinito `TicketDataFormat`. Se il `TicketDataFormat` è impostata, il `DataProtectionProvider` non viene usata l'opzione. Se non specificato, viene utilizzato provider di protezione dati predefinito dell'app. |
| [Eventi](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | Il gestore chiama i metodi sul provider che offrono il controllo delle app in determinati punti di elaborazione. Se `Events` non vengono forniti, viene fornita un'istanza predefinita che non esegue alcuna operazione quando i metodi vengono chiamati. |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | Utilizzato come tipo di servizio per ottenere il `Events` istanza anziché la proprietà. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | Il `TimeSpan` dopo che scade il ticket di autenticazione archiviato all'interno del cookie. `ExpireTimeSpan` viene aggiunto all'ora corrente per creare l'ora di scadenza per il ticket. Il `ExpiredTimeSpan` valore di passi sempre nei ticket di autenticazione crittografato verificati dal server. È anche possibile che passi nel [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) intestazione, ma solo se `IsPersistent` è impostata. Per impostare `IsPersistent` al `true`, configurare il [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passato a `SignInAsync`. Il valore predefinito di `ExpireTimeSpan` è 14 giorni. |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | Fornisce il percorso per fornire un 302 trovato (reindirizzamento URL) quando viene attivata da `HttpContext.ChallengeAsync`. L'URL corrente che ha generato il codice 401 viene aggiunto per il `LoginPath` come parametro di stringa di query denominato dal `ReturnUrlParameter`. Una volta una richiesta per il `LoginPath` concede una nuova identità di accesso, il `ReturnUrlParameter` valore viene usato per reindirizzare il browser all'URL che ha causato il codice di stato unauthorized originale. Il valore predefinito è `/Account/Login`. |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | Se il `LogoutPath` viene fornito al gestore, quindi reindirizza una richiesta a tale percorso in base al valore della `ReturnUrlParameter`. Il valore predefinito è `/Account/Logout`. |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | Determina il nome del parametro della stringa di query che viene aggiunto dal gestore per una risposta 302 trovato (reindirizzamento URL). `ReturnUrlParameter` viene usato quando una richiesta arriva attraverso il `LoginPath` o `LogoutPath` per restituire il browser all'URL originale dopo che viene eseguita l'azione di connessione o disconnessione. Il valore predefinito è `ReturnUrl`. |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | Contenitore facoltativo usato per archiviare l'identità tra le richieste. Quando usato, solo un identificatore di sessione viene inviato al client. `SessionStore` Consente di attenuare i potenziali problemi con le identità di grandi dimensioni. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | Un flag che indica se un nuovo cookie con una scadenza aggiornato deve essere visualizzato in modo dinamico. Questa situazione può verificarsi in qualsiasi richiesta in cui il periodo di scadenza cookie corrente è superiore al 50% scaduto. La nuova data di scadenza viene spostata in avanti è la data corrente più il `ExpireTimespan`. Un' [data di scadenza assoluto cookie](xref:security/authentication/cookie#absolute-cookie-expiration) può essere impostata tramite il `AuthenticationProperties` classe quando si chiama `SignInAsync`. Un'ora di scadenza assoluto può migliorare la sicurezza dell'app, limitando la quantità di tempo in cui il cookie di autenticazione è valido. Il valore predefinito è `true`. |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | Il `TicketDataFormat` viene usato per proteggere e rimuovere la protezione dell'identità e altre proprietà che vengono memorizzate nel valore del cookie. Se non specificato, un `TicketDataFormat` viene creato usando il [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0). |
| [Validate](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | Metodo che verifica che le opzioni sono valide. |

Impostare `CookieAuthenticationOptions` nella configurazione del servizio per l'autenticazione nel `ConfigureServices` metodo:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

ASP.NET Core 1.x Usa cookie [middleware](xref:fundamentals/middleware/index) che serializza un'entità utente in un cookie crittografato. Nelle richieste successive, il cookie viene convalidato e l'entità viene ricreata e assegnato al `HttpContext.User` proprietà.

Installare il [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacchetto NuGet nel progetto. Questo pacchetto contiene il middleware dei cookie.

Usare il `UseCookieAuthentication` metodo nel `Configure` metodo nel *Startup.cs* file prima `UseMvc` o `UseMvcWithDefaultRoute`:

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
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | Imposta lo schema di autenticazione. `AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione e si desidera [autorizzare con uno schema specifico](xref:security/authorization/limitingidentitybyscheme). Impostando il `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` fornisce un valore di "Cookie" per lo schema. È possibile specificare qualsiasi valore stringa che distingue lo schema. |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | Imposta un valore per indicare che l'autenticazione dei cookie debba eseguire in ogni richiesta e tenta di convalidare e ricostruire qualsiasi entità serializzato, che creato. |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | Se true, il middleware di autenticazione gestisce le sfide automatica. Se false, il middleware di autenticazione altera solo le risposte quando richiesto esplicitamente dal `AuthenticationScheme`. |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | L'autorità emittente da usare per la [Issuer](/dotnet/api/system.security.claims.claim.issuer) proprietà in tutte le attestazioni creato dal middleware di autenticazione dei cookie. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | Il nome di dominio in cui viene servito il cookie. Per impostazione predefinita, questo è il nome host della richiesta. Il browser viene usato solo il cookie a un nome host corrispondente. È possibile modificare questa opzione per disporre i cookie disponibili a tutti gli host nel dominio. Ad esempio, l'impostazione del dominio del cookie `.contoso.com` renderli disponibili agli `contoso.com`, `www.contoso.com`, e `staging.www.contoso.com`. |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | Un flag che indica se il cookie deve essere accessibile solo ai server. Se si modifica questo valore per `false` consente a script lato client per accedere al cookie e può aprire l'app al furto di cookie che l'app deve avere una [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerabilità. Il valore predefinito è `true`. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | Consente di isolare le App in esecuzione sullo stesso nome host. Se si dispone di un'app in esecuzione in `/app1` e si desidera limitare i cookie per tale app, impostare il `CookiePath` proprietà `/app1`. In questo modo, il cookie è disponibile solo nelle richieste al `/app1` e a qualsiasi app in esso contenuti. |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | Un flag che indica se il cookie creato deve essere limitato a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o lo stesso protocollo della richiesta (`CookieSecurePolicy.SameAsRequest`). Il valore predefinito è `CookieSecurePolicy.SameAsRequest`. |
| [Descrizione](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | Informazioni aggiuntive sul tipo di autenticazione che viene reso disponibile per l'app. |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | Il `TimeSpan` dopo che scade il ticket di autenticazione. Viene aggiunto all'ora corrente per creare l'ora di scadenza per il ticket. Per utilizzare `ExpireTimeSpan`, è necessario impostare `IsPersistent` a `true` nel `AuthenticationProperties` passato a `SignInAsync`. Il valore predefinito è 14 giorni. |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | Un flag che indica se la data di scadenza del cookie viene reimpostato quando più della metà del `ExpireTimeSpan` superamento dell'intervallo. La nuova ora di scadenza viene spostata in avanti è la data corrente più il `ExpireTimespan`. Un' [data di scadenza assoluto cookie](xref:security/authentication/cookie#absolute-cookie-expiration) può essere impostata tramite il `AuthenticationProperties` classe quando si chiama `SignInAsync`. Un'ora di scadenza assoluto può migliorare la sicurezza dell'app, limitando la quantità di tempo in cui il cookie di autenticazione è valido. Il valore predefinito è `true`. |

Impostare `CookieAuthenticationOptions` per il Middleware di autenticazione del Cookie nel `Configure` metodo:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a>Middleware cookie criteri

[Middleware cookie criteri](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) offre funzionalità di criteri di cookie in un'app. Aggiungere il middleware alla pipeline di elaborazione delle app è ordine sensibili; riguarda solo i componenti registrati dopo di esso nella pipeline.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 Il [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) fornito per il Middleware dei Cookie criteri consentono di controllare caratteristiche globali del cookie elaborazione e di hook in elaborazione cookie gestori quando i cookie vengono aggiunti o eliminati.

| Proprietà | Descrizione |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | Interessa se i cookie devono essere HttpOnly, che è un flag che indica se il cookie deve essere accessibile solo ai server. Il valore predefinito è `HttpOnlyPolicy.None`. |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | Interessa attributo SameSite del cookie (vedere sotto). Il valore predefinito è `SameSiteMode.Lax`. Questa opzione è disponibile per ASP.NET Core 2.0 +. |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Chiamato quando viene aggiunto un cookie. |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | Chiamato quando viene eliminato un cookie. |
| [Proteggere](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | Determina se i cookie devono essere sicura. Il valore predefinito è `CookieSecurePolicy.None`. |

**MinimumSameSitePolicy** (ASP.NET Core 2.0 + solo)

Il valore predefinito `MinimumSameSitePolicy` valore è `SameSiteMode.Lax` per consentire l'autenticazione OAuth2. Per applicare rigorosamente criteri nello stesso sito di `SameSiteMode.Strict`, impostare il `MinimumSameSitePolicy`. Anche se questa impostazione interrompe OAuth2 e altri schemi di autenticazione tra le origini, eleva il livello di protezione dei cookie per altri tipi di App che non richiedono l'elaborazione della richiesta multiorigine.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

L'impostazione di criteri Middleware dei Cookie per `MinimumSameSitePolicy` può influire sull'impostazione di `Cookie.SameSite` in `CookieAuthenticationOptions` impostazioni a seconda della matrice riportata di seguito.

| MinimumSameSitePolicy | Cookie.SameSite | Impostazione Cookie.SameSite risultante |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Creare un cookie di autenticazione

Per creare un cookie che contiene informazioni sull'utente, è necessario creare un [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal). Le informazioni dell'utente vengano serializzate e archiviate nel cookie. 

::: moniker range=">= aspnetcore-2.0"

Creare un [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) con le operazioni [attestazione](/dotnet/api/system.security.claims.claim)s e chiamare [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) per l'accesso dell'utente:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Chiamare [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) per l'accesso dell'utente:

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

`SignInAsync` Crea un cookie crittografato e lo aggiunge alla risposta corrente. Se non si specifica un `AuthenticationScheme`, viene usato lo schema predefinito.

Dietro le quinte, la crittografia usata è ASP.NET Core [protezione dati](xref:security/data-protection/using-data-protection) sistema. Se si ospita app su più computer, il bilanciamento del carico tra le app o utilizzando una web farm, quindi devi [configurare la protezione dati](xref:security/data-protection/configuration/overview) usare il Keyring stesso e l'identificatore dell'app.

## <a name="sign-out"></a>Esci

::: moniker range=">= aspnetcore-2.0"

Per disconnettere l'utente corrente ed eliminare i cookie, chiamare [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Per disconnettere l'utente corrente ed eliminare i cookie, chiamare [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

Se non si usa `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookie") come lo schema (ad esempio, "ContosoCookie"), specificare lo schema usato durante la configurazione del provider di autenticazione. In caso contrario, viene usato lo schema predefinito.

## <a name="react-to-back-end-changes"></a>Reagire alle modifiche di back-end

Dopo aver creato un cookie, diventa l'unica origine di identità. Anche se si disabilita un utente nei sistemi back-end, il sistema di autenticazione del cookie non è a conoscenza di questo oggetto e un utente rimane connesso, purché il cookie è valido.

Il [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) evento in ASP.NET Core 2.x o il [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metodo in ASP.NET Core 1.x è utilizzabile per intercettare ed eseguire l'override di convalida dell'identità del cookie. Questo approccio riduce il rischio di revocato agli utenti l'accesso all'app.

Un approccio alla convalida dei cookie si basa su tenere traccia di quando il database utente è stato modificato. Se il database non è stato modificato perché è stato emesso il cookie dell'utente, non è necessario autenticare nuovamente l'utente se il cookie sia ancora valido. Per implementare questo scenario, il database, che viene implementato in `IUserRepository` in questo esempio archivia un `LastChanged` valore. Quando un utente viene aggiornato nel database, il `LastChanged` valore è impostato sull'ora corrente.

Per poter invalidare un cookie durante le modifiche del database in base il `LastChanged` valore, creare il cookie con un `LastChanged` attestazione contenente l'oggetto corrente `LastChanged` valore dal database:

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

::: moniker range=">= aspnetcore-2.0"

Per implementare una sostituzione per il `ValidatePrincipal` evento, scrivere un metodo con la firma seguente in una classe derivata da [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

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

Registrare l'istanza di eventi durante la registrazione del servizio nel cookie di `ConfigureServices` (metodo). Fornire una registrazione del servizio con ambito per il `CustomCookieAuthenticationEvents` classe:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Per implementare una sostituzione per il `ValidateAsync` evento, scrivere un metodo con la firma seguente:

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Core Identity implementa questo controllo come parte del relativo [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync). Un esempio è simile al seguente:

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

::: moniker-end

Si consideri una situazione in cui viene aggiornato il nome dell'utente &mdash; una decisione che non influiscono sulla sicurezza in alcun modo. Se si desidera aggiornare in modo non distruttivo l'entità utente, chiamare `context.ReplacePrincipal` e impostare il `context.ShouldRenew` proprietà `true`.

> [!WARNING]
> L'approccio descritto di seguito viene attivato a ogni richiesta. Ciò può comportare una riduzione delle prestazioni di grandi dimensioni per l'app.

## <a name="persistent-cookies"></a>Cookie permanenti

È possibile il cookie per rendere persistenti le sessioni del browser. La persistenza deve essere abilitata solo con il consenso dell'utente esplicita con una "Memorizza password" casella di controllo in account di accesso o un meccanismo simile. 

Il frammento di codice seguente crea un'identità e un corrispondente cookie in cui viene conservata tramite le chiusure del browser. Vengono rispettate le impostazioni di scadenza scorrevole configurate in precedenza. Se il cookie scade mentre il browser viene chiuso, il browser Cancella il cookie dopo il riavvio.

::: moniker range=">= aspnetcore-2.0"

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

## <a name="absolute-cookie-expiration"></a>Scadenza del cookie assoluto

È possibile impostare un'ora di scadenza assoluto con `ExpiresUtc`. Per creare un cookie permanente, è necessario impostare anche `IsPersistent`; in caso contrario, il cookie viene creato con un ciclo di vita basati su sessione e far scadere prima o dopo l'autenticazione del ticket che lo contiene. Quando `ExpiresUtc` è nastavit `SignInAsync`, sostituisce il valore della `ExpireTimeSpan` opzione di `CookieAuthenticationOptions`, se impostata.

Il frammento di codice seguente crea un'identità e un corrispondente cookie che dura per 20 minuti. Ignora eventuali impostazioni di scadenza scorrevole configurati in precedenza.

::: moniker range=">= aspnetcore-2.0"

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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

::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

* [Modifiche di autorizzazione 2.0 / migrazione annuncio](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Controlli di ruolo basata su criteri](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
