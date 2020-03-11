---
title: Usa autenticazione cookie senza ASP.NET Core identità
author: rick-anderson
description: Informazioni su come usare l'autenticazione tramite cookie senza ASP.NET Core identità.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/11/2020
uid: security/authentication/cookie
ms.openlocfilehash: 64f881441a7a7f9a5529cb6ee5ce81142ccd69e6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663002"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Usa autenticazione cookie senza ASP.NET Core identità

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core identità è un provider di autenticazione completo e completo per la creazione e la gestione degli account di accesso. Tuttavia, è possibile usare un provider di autenticazione basato su cookie senza ASP.NET Core identità. Per altre informazioni, vedere <xref:security/authentication/identity>.

[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

A scopo dimostrativo nell'app di esempio, l'account utente per l'utente ipotetico, Maria Rodriguez, è hardcoded nell'app. Usare l'indirizzo di **posta elettronica** `maria.rodriguez@contoso.com` e qualsiasi password per accedere all'utente. L'utente viene autenticato nel metodo `AuthenticateUser` nel file *pages/account/login. cshtml. cs* . In un esempio reale, l'utente viene autenticato a fronte di un database.

## <a name="configuration"></a>Configurazione

Se l'app non usa il [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), creare un riferimento al pacchetto nel file di progetto per il pacchetto [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .

Nel metodo `Startup.ConfigureServices` creare i servizi del middleware di autenticazione con i metodi <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> e <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passato a `AddAuthentication` imposta lo schema di autenticazione predefinito per l'app. `AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione dei cookie e si desidera [autorizzare con uno schema specifico](xref:security/authorization/limitingidentitybyscheme). Impostando il `AuthenticationScheme` su [CookieAuthenticationDefaults. AuthenticationScheme,](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) viene fornito il valore "cookies" per lo schema. È possibile specificare qualsiasi valore stringa che distingua lo schema.

Lo schema di autenticazione dell'app è diverso dallo schema di autenticazione dei cookie dell'app. Quando non viene fornito uno schema di autenticazione dei cookie per <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, viene usato `CookieAuthenticationDefaults.AuthenticationScheme` ("cookie").

Per impostazione predefinita, la proprietà <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> del cookie di autenticazione è impostata su `true`. I cookie di autenticazione sono consentiti quando un visitatore del sito non ha acconsentito alla raccolta dei dati. Per altre informazioni, vedere <xref:security/gdpr#essential-cookies>.

In `Startup.Configure`chiamare `UseAuthentication` e `UseAuthorization` per impostare la proprietà `HttpContext.User` ed eseguire il middleware di autorizzazione per le richieste. Chiamare i metodi `UseAuthentication` e `UseAuthorization` prima di chiamare `UseEndpoints`:

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

La classe <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> viene utilizzata per configurare le opzioni del provider di autenticazione.

Impostare `CookieAuthenticationOptions` nella configurazione del servizio per l'autenticazione nel metodo `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Middleware dei criteri dei cookie

Il [middleware dei criteri](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) dei cookie Abilita le funzionalità dei criteri dei cookie. L'aggiunta del middleware alla pipeline di elaborazione delle app è sensibile agli ordini&mdash;interessa solo i componenti downstream registrati nella pipeline.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Usare <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> fornite al middleware dei criteri dei cookie per controllare le caratteristiche globali dell'elaborazione dei cookie e l'hook nei gestori di elaborazione dei cookie quando i cookie vengono aggiunti o eliminati.

Il valore predefinito <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> è `SameSiteMode.Lax` per consentire l'autenticazione OAuth2. Per applicare rigorosamente un criterio dello stesso sito di `SameSiteMode.Strict`, impostare il `MinimumSameSitePolicy`. Sebbene questa impostazione interrompa OAuth2 e altri schemi di autenticazione tra le origini, eleva il livello di sicurezza dei cookie per altri tipi di app che non si basano sull'elaborazione di richieste tra origini.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

L'impostazione del middleware per i criteri dei cookie per `MinimumSameSitePolicy` può influire sull'impostazione di `Cookie.SameSite` nelle impostazioni `CookieAuthenticationOptions` in base alla matrice riportata di seguito.

| MinimumSameSitePolicy | Cookie.SameSite | Impostazione cookie. navigava sullostesso sito risultante |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Creare un cookie di autenticazione

Per creare un cookie che contiene informazioni sull'utente, costruire un <xref:System.Security.Claims.ClaimsPrincipal>. Le informazioni utente vengono serializzate e archiviate nel cookie. 

Creare una <xref:System.Security.Claims.ClaimsIdentity> con qualsiasi <xref:System.Security.Claims.Claim>obbligatorio e chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> per accedere all'utente:

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` crea un cookie crittografato e lo aggiunge alla risposta corrente. Se `AuthenticationScheme` non è specificato, viene usato lo schema predefinito.

Per la crittografia viene usato il sistema di [protezione dei dati](xref:security/data-protection/using-data-protection) di ASP.NET Core. Per un'app ospitata in più computer, bilanciamento del carico tra app o uso di un Web farm, [configurare la protezione dei dati](xref:security/data-protection/configuration/overview) in modo da usare lo stesso anello chiave e l'identificatore dell'app.

## <a name="sign-out"></a>Disconnessione

Per disconnettersi dall'utente corrente ed eliminare il cookie, chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Se `CookieAuthenticationDefaults.AuthenticationScheme` (o "cookie") non viene utilizzato come schema (ad esempio, "ContosoCookie"), fornire lo schema utilizzato durante la configurazione del provider di autenticazione. In caso contrario, verrà utilizzato lo schema predefinito.

## <a name="react-to-back-end-changes"></a>Reagire alle modifiche del back-end

Una volta creato un cookie, il cookie è l'unica origine di identità. Se un account utente è disabilitato nei sistemi back-end:

* Il sistema di autenticazione dei cookie dell'app continua a elaborare le richieste in base al cookie di autenticazione.
* L'utente rimane connesso all'app fino a quando il cookie di autenticazione è valido.

È possibile utilizzare l'evento <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> per intercettare ed eseguire l'override della convalida dell'identità del cookie. La convalida del cookie a ogni richiesta attenua il rischio di revocare gli utenti che accedono all'app.

Un approccio alla convalida dei cookie si basa sul tenere traccia del momento in cui il database utente viene modificato. Se il database non è stato modificato dopo l'emissione del cookie dell'utente, non è necessario autenticare di nuovo l'utente se il cookie è ancora valido. Nell'app di esempio, il database viene implementato in `IUserRepository` e archivia un valore `LastChanged`. Quando un utente viene aggiornato nel database, il valore `LastChanged` viene impostato sull'ora corrente.

Per invalidare un cookie quando il database viene modificato in base al valore `LastChanged`, creare il cookie con un'attestazione `LastChanged` contenente il valore di `LastChanged` corrente dal database:

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

Per implementare una sostituzione per l'evento `ValidatePrincipal`, scrivere un metodo con la firma seguente in una classe che deriva da <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Di seguito è riportato un esempio di implementazione di `CookieAuthenticationEvents`:

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

Registrare l'istanza degli eventi durante la registrazione del servizio cookie nel metodo `Startup.ConfigureServices`. Fornire una [registrazione del servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes) per la classe `CustomCookieAuthenticationEvents`:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Si consideri una situazione in cui il nome dell'utente viene aggiornato&mdash;una decisione che non influisce sulla sicurezza in alcun modo. Se si desidera aggiornare l'entità utente in modo non distruttivo, chiamare `context.ReplacePrincipal` e impostare la proprietà `context.ShouldRenew` su `true`.

> [!WARNING]
> L'approccio descritto qui viene attivato a ogni richiesta. La convalida dei cookie di autenticazione per tutti gli utenti in ogni richiesta può comportare un notevole calo delle prestazioni per l'app.

## <a name="persistent-cookies"></a>Cookie permanenti

È possibile che il cookie venga mantenuto tra le sessioni del browser. Questa persistenza deve essere abilitata solo con il consenso esplicito dell'utente con una casella di controllo "Remember me" all'accesso o un meccanismo simile. 

Il frammento di codice seguente crea un'identità e un cookie corrispondente che sopravvivono attraverso le chiusure del browser. Vengono rispettate tutte le impostazioni di scadenza scorrevoli configurate in precedenza. Se il cookie scade quando il browser viene chiuso, il cookie viene cancellato dopo il riavvio.

Impostare <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> su `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a>Scadenza assoluta del cookie

È possibile impostare un'ora di scadenza assoluta con <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>. Per creare un cookie persistente, è necessario impostare anche `IsPersistent`. In caso contrario, il cookie viene creato con una durata basata sulla sessione e potrebbe scadere prima o dopo il ticket di autenticazione che possiede. Quando si imposta `ExpiresUtc`, viene eseguito l'override del valore dell'opzione <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> di <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, se impostata.

Il frammento di codice seguente crea un'identità e un cookie corrispondente che dura 20 minuti. Questa operazione ignora le impostazioni di scadenza scorrevoli configurate in precedenza.

```csharp
// using Microsoft.AspNetCore.Authentication;

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

::: moniker range="< aspnetcore-3.0"

ASP.NET Core identità è un provider di autenticazione completo e completo per la creazione e la gestione degli account di accesso. Tuttavia, è possibile usare un provider di autenticazione basata su cookie senza ASP.NET Core identità. Per altre informazioni, vedere <xref:security/authentication/identity>.

[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

A scopo dimostrativo nell'app di esempio, l'account utente per l'utente ipotetico, Maria Rodriguez, è hardcoded nell'app. Usare l'indirizzo di **posta elettronica** `maria.rodriguez@contoso.com` e qualsiasi password per accedere all'utente. L'utente viene autenticato nel metodo `AuthenticateUser` nel file *pages/account/login. cshtml. cs* . In un esempio reale, l'utente viene autenticato a fronte di un database.

## <a name="configuration"></a>Configurazione

Se l'app non usa il [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), creare un riferimento al pacchetto nel file di progetto per il pacchetto [Microsoft. AspNetCore. Authentication. cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) .

Nel metodo `Startup.ConfigureServices` creare il servizio middleware di autenticazione con i metodi <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> e <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passato a `AddAuthentication` imposta lo schema di autenticazione predefinito per l'app. `AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione dei cookie e si desidera [autorizzare con uno schema specifico](xref:security/authorization/limitingidentitybyscheme). Impostando il `AuthenticationScheme` su [CookieAuthenticationDefaults. AuthenticationScheme,](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) viene fornito il valore "cookies" per lo schema. È possibile specificare qualsiasi valore stringa che distingua lo schema.

Lo schema di autenticazione dell'app è diverso dallo schema di autenticazione dei cookie dell'app. Quando non viene fornito uno schema di autenticazione dei cookie per <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, viene usato `CookieAuthenticationDefaults.AuthenticationScheme` ("cookie").

Per impostazione predefinita, la proprietà <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> del cookie di autenticazione è impostata su `true`. I cookie di autenticazione sono consentiti quando un visitatore del sito non ha acconsentito alla raccolta dei dati. Per altre informazioni, vedere <xref:security/gdpr#essential-cookies>.

Nel metodo `Startup.Configure` chiamare il metodo `UseAuthentication` per richiamare il middleware di autenticazione che imposta la proprietà `HttpContext.User`. Chiamare il metodo `UseAuthentication` prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

La classe <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> viene utilizzata per configurare le opzioni del provider di autenticazione.

Impostare `CookieAuthenticationOptions` nella configurazione del servizio per l'autenticazione nel metodo `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Middleware dei criteri dei cookie

Il [middleware dei criteri](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) dei cookie Abilita le funzionalità dei criteri dei cookie. L'aggiunta del middleware alla pipeline di elaborazione delle app è sensibile agli ordini&mdash;interessa solo i componenti downstream registrati nella pipeline.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Usare <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> fornite al middleware dei criteri dei cookie per controllare le caratteristiche globali dell'elaborazione dei cookie e l'hook nei gestori di elaborazione dei cookie quando i cookie vengono aggiunti o eliminati.

Il valore predefinito <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> è `SameSiteMode.Lax` per consentire l'autenticazione OAuth2. Per applicare rigorosamente un criterio dello stesso sito di `SameSiteMode.Strict`, impostare il `MinimumSameSitePolicy`. Sebbene questa impostazione interrompa OAuth2 e altri schemi di autenticazione tra le origini, eleva il livello di sicurezza dei cookie per altri tipi di app che non si basano sull'elaborazione di richieste tra origini.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

L'impostazione del middleware per i criteri dei cookie per `MinimumSameSitePolicy` può influire sull'impostazione di `Cookie.SameSite` nelle impostazioni `CookieAuthenticationOptions` in base alla matrice riportata di seguito.

| MinimumSameSitePolicy | Cookie.SameSite | Impostazione cookie. navigava sullostesso sito risultante |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Creare un cookie di autenticazione

Per creare un cookie che contiene informazioni sull'utente, costruire un <xref:System.Security.Claims.ClaimsPrincipal>. Le informazioni utente vengono serializzate e archiviate nel cookie. 

Creare una <xref:System.Security.Claims.ClaimsIdentity> con qualsiasi <xref:System.Security.Claims.Claim>obbligatorio e chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> per accedere all'utente:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` crea un cookie crittografato e lo aggiunge alla risposta corrente. Se `AuthenticationScheme` non è specificato, viene usato lo schema predefinito.

Per la crittografia viene usato il sistema di [protezione dei dati](xref:security/data-protection/using-data-protection) di ASP.NET Core. Per un'app ospitata in più computer, bilanciamento del carico tra app o uso di un Web farm, [configurare la protezione dei dati](xref:security/data-protection/configuration/overview) in modo da usare lo stesso anello chiave e l'identificatore dell'app.

## <a name="sign-out"></a>Disconnessione

Per disconnettersi dall'utente corrente ed eliminare il cookie, chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Se `CookieAuthenticationDefaults.AuthenticationScheme` (o "cookie") non viene utilizzato come schema (ad esempio, "ContosoCookie"), fornire lo schema utilizzato durante la configurazione del provider di autenticazione. In caso contrario, verrà utilizzato lo schema predefinito.

## <a name="react-to-back-end-changes"></a>Reagire alle modifiche del back-end

Una volta creato un cookie, il cookie è l'unica origine di identità. Se un account utente è disabilitato nei sistemi back-end:

* Il sistema di autenticazione dei cookie dell'app continua a elaborare le richieste in base al cookie di autenticazione.
* L'utente rimane connesso all'app fino a quando il cookie di autenticazione è valido.

È possibile utilizzare l'evento <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> per intercettare ed eseguire l'override della convalida dell'identità del cookie. La convalida del cookie a ogni richiesta attenua il rischio di revocare gli utenti che accedono all'app.

Un approccio alla convalida dei cookie si basa sul tenere traccia del momento in cui il database utente viene modificato. Se il database non è stato modificato dopo l'emissione del cookie dell'utente, non è necessario autenticare di nuovo l'utente se il cookie è ancora valido. Nell'app di esempio, il database viene implementato in `IUserRepository` e archivia un valore `LastChanged`. Quando un utente viene aggiornato nel database, il valore `LastChanged` viene impostato sull'ora corrente.

Per invalidare un cookie quando il database viene modificato in base al valore `LastChanged`, creare il cookie con un'attestazione `LastChanged` contenente il valore di `LastChanged` corrente dal database:

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

Per implementare una sostituzione per l'evento `ValidatePrincipal`, scrivere un metodo con la firma seguente in una classe che deriva da <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

Di seguito è riportato un esempio di implementazione di `CookieAuthenticationEvents`:

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

Registrare l'istanza degli eventi durante la registrazione del servizio cookie nel metodo `Startup.ConfigureServices`. Fornire una [registrazione del servizio con ambito](xref:fundamentals/dependency-injection#service-lifetimes) per la classe `CustomCookieAuthenticationEvents`:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Si consideri una situazione in cui il nome dell'utente viene aggiornato&mdash;una decisione che non influisce sulla sicurezza in alcun modo. Se si desidera aggiornare l'entità utente in modo non distruttivo, chiamare `context.ReplacePrincipal` e impostare la proprietà `context.ShouldRenew` su `true`.

> [!WARNING]
> L'approccio descritto qui viene attivato a ogni richiesta. La convalida dei cookie di autenticazione per tutti gli utenti in ogni richiesta può comportare un notevole calo delle prestazioni per l'app.

## <a name="persistent-cookies"></a>Cookie permanenti

È possibile che il cookie venga mantenuto tra le sessioni del browser. Questa persistenza deve essere abilitata solo con il consenso esplicito dell'utente con una casella di controllo "Remember me" all'accesso o un meccanismo simile. 

Il frammento di codice seguente crea un'identità e un cookie corrispondente che sopravvivono attraverso le chiusure del browser. Vengono rispettate tutte le impostazioni di scadenza scorrevoli configurate in precedenza. Se il cookie scade quando il browser viene chiuso, il cookie viene cancellato dopo il riavvio.

Impostare <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> su `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a>Scadenza assoluta del cookie

È possibile impostare un'ora di scadenza assoluta con <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>. Per creare un cookie persistente, è necessario impostare anche `IsPersistent`. In caso contrario, il cookie viene creato con una durata basata sulla sessione e potrebbe scadere prima o dopo il ticket di autenticazione che possiede. Quando si imposta `ExpiresUtc`, viene eseguito l'override del valore dell'opzione <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> di <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, se impostata.

Il frammento di codice seguente crea un'identità e un cookie corrispondente che dura 20 minuti. Questa operazione ignora le impostazioni di scadenza scorrevoli configurate in precedenza.

```csharp
// using Microsoft.AspNetCore.Authentication;

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

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Controlli dei ruoli basati su criteri](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
