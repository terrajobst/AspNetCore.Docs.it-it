---
title: Usare l'autenticazione tramite cookie senza ASP.NET Core Identity
author: rick-anderson
description: Informazioni su come usare l'autenticazione tramite cookie senza ASP.NET Core Identity.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622740"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>Usare l'autenticazione tramite cookie senza ASP.NET Core Identity

Dal [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)

ASP.NET Core Identity è un provider di autenticazione completa e completo per creare e gestire gli account di accesso. Tuttavia, un provider di autenticazione l'autenticazione basata su cookie senza ASP.NET Core Identity può essere utilizzato. Per altre informazioni, vedere <xref:security/authentication/identity>.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

A scopo dimostrativo nell'app di esempio, l'account utente per l'utente ipotetica, Maria Rodriguez, è hardcoded nell'app. Usare la **messaggio di posta elettronica** nome utente `maria.rodriguez@contoso.com` e qualsiasi password di accesso dell'utente. L'utente viene autenticato nel `AuthenticateUser` metodo nella *Pages/Account/Login.cshtml.cs* file. In un esempio reale, l'utente viene autenticato in un database.

## <a name="configuration"></a>Configurazione

Se l'app non usa la [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), creare un riferimento al pacchetto nel file di progetto per il [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacchetto.

Nel `Startup.ConfigureServices` metodo, creare il servizio di Middleware di autenticazione con il <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> e <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> metodi:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passato a `AddAuthentication` imposta lo schema di autenticazione predefinito per l'app. `AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione dei cookie e si desidera [autorizzare con uno schema specifico](xref:security/authorization/limitingidentitybyscheme). Impostando il `AuthenticationScheme` al [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) fornisce un valore di "Cookie" per lo schema. È possibile specificare qualsiasi valore stringa che distingue lo schema.

Schema di autenticazione dell'app è diverso dallo schema di autenticazione cookie dell'app. Quando non viene fornito uno schema di autenticazione del cookie a <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, Usa `CookieAuthenticationDefaults.AuthenticationScheme` ("cookie").

Il cookie di autenticazione <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> è impostata su `true` per impostazione predefinita. I cookie di autenticazione sono consentiti quando un visitatore non ha fornito il consenso alla raccolta di dati. Per altre informazioni, vedere <xref:security/gdpr#essential-cookies>.

Nel `Startup.Configure` metodo, chiamare il `UseAuthentication` metodo da richiamare il Middleware di autenticazione che consente di impostare il `HttpContext.User` proprietà. Chiamare il `UseAuthentication` metodo prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

Il <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> classe viene utilizzata per configurare le opzioni del provider di autenticazione.

Impostare `CookieAuthenticationOptions` nella configurazione del servizio per l'autenticazione nel `Startup.ConfigureServices` metodo:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Middleware cookie criteri

[Middleware cookie criteri](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) Abilita le funzionalità dei criteri di cookie. Aggiungere il middleware alla pipeline di elaborazione delle app è ordine sensibili&mdash;riguarda solo i componenti a valle registrati nella pipeline.

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

Usare <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> fornito per il Middleware dei Cookie criteri per controllare caratteristiche globali del cookie elaborazione e di hook in elaborazione cookie gestori quando i cookie vengono aggiunti o eliminati.

Il valore predefinito <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> valore è `SameSiteMode.Lax` per consentire l'autenticazione OAuth2. Per applicare rigorosamente criteri nello stesso sito di `SameSiteMode.Strict`, impostare il `MinimumSameSitePolicy`. Anche se questa impostazione interrompe OAuth2 e altri schemi di autenticazione tra le origini, eleva il livello di protezione dei cookie per altri tipi di App che non richiedono l'elaborazione della richiesta multiorigine.

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

L'impostazione di criteri Middleware dei Cookie per `MinimumSameSitePolicy` l'impostazione di `Cookie.SameSite` in `CookieAuthenticationOptions` impostazioni a seconda della matrice riportata di seguito.

| MinimumSameSitePolicy | Cookie.SameSite | Impostazione Cookie.SameSite risultante |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>Creare un cookie di autenticazione

Per creare un cookie che contiene informazioni sull'utente, costruire un <xref:System.Security.Claims.ClaimsPrincipal>. Le informazioni dell'utente vengano serializzate e archiviate nel cookie. 

Creare un <xref:System.Security.Claims.ClaimsIdentity> con le operazioni <xref:System.Security.Claims.Claim>s e chiamata <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> per l'accesso dell'utente:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` Crea un cookie crittografato e lo aggiunge alla risposta corrente. Se `AuthenticationScheme` non è specificato, viene usato lo schema predefinito.

ASP.NET Core [protezione dati](xref:security/data-protection/using-data-protection) sistema viene utilizzato per la crittografia. Per un'app ospitata in più computer, di caricamento tramite una web farm, o bilanciamento del carico tra le app [configurare la protezione dati](xref:security/data-protection/configuration/overview) usare il Keyring stesso e l'identificatore dell'app.

## <a name="sign-out"></a>Esci

Per disconnettere l'utente corrente ed eliminare i cookie, chiamare <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

Se `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookie") non viene usato come lo schema (ad esempio, "ContosoCookie"), specificare lo schema utilizzato durante la configurazione del provider di autenticazione. In caso contrario, viene usato lo schema predefinito.

## <a name="react-to-back-end-changes"></a>Reagire alle modifiche di back-end

Dopo aver creato un cookie, il cookie è l'unica origine di identità. Se un account utente è disabilitato nei sistemi back-end:

* Sistema di autenticazione cookie dell'app continua a elaborare le richieste basate su cookie di autenticazione.
* L'utente rimane connesso all'App, purché il cookie di autenticazione è valido.

Il <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> evento può essere utilizzato per intercettare ed eseguire l'override di convalida dell'identità del cookie. Convalida il cookie a ogni richiesta riduce il rischio di revocato agli utenti l'accesso all'app.

Un approccio alla convalida dei cookie si basa su tenere traccia di quando il database utente viene modificato. Se il database non è stato modificato perché è stato emesso il cookie dell'utente, non è necessario autenticare nuovamente l'utente se il cookie sia ancora valido. Nell'app di esempio, il database viene implementato in `IUserRepository` e archivia un `LastChanged` valore. Quando un utente viene aggiornato nel database, il `LastChanged` valore è impostato sull'ora corrente.

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

Per implementare una sostituzione per il `ValidatePrincipal` evento, scrivere un metodo con la firma seguente in una classe che deriva da <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

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

Registrare l'istanza di eventi durante la registrazione del servizio nel cookie di `Startup.ConfigureServices` (metodo). Fornire una [scoped la registrazione del servizio](xref:fundamentals/dependency-injection#service-lifetimes) per il `CustomCookieAuthenticationEvents` classe:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

Si consideri una situazione in cui viene aggiornato il nome dell'utente&mdash;una decisione che non influiscono sulla sicurezza in alcun modo. Se si desidera aggiornare in modo non distruttivo l'entità utente, chiamare `context.ReplacePrincipal` e impostare il `context.ShouldRenew` proprietà `true`.

> [!WARNING]
> L'approccio descritto di seguito viene attivato a ogni richiesta. Convalida i cookie di autenticazione per tutti gli utenti a ogni richiesta può comportare una riduzione delle prestazioni di grandi dimensioni per l'app.

## <a name="persistent-cookies"></a>Cookie permanenti

È possibile il cookie per rendere persistenti le sessioni del browser. La persistenza deve essere abilitata solo con il consenso dell'utente esplicita con una "Memorizza password" casella di controllo di accesso in o un meccanismo simile. 

Il frammento di codice seguente crea un'identità e un corrispondente cookie in cui viene conservata tramite le chiusure del browser. Vengono rispettate le impostazioni di scadenza scorrevole configurate in precedenza. Se il cookie scade mentre il browser viene chiuso, il browser Cancella il cookie dopo il riavvio.

Impostare <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> al `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

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

## <a name="absolute-cookie-expiration"></a>Scadenza del cookie assoluto

Un'ora di scadenza assoluto può essere impostata con <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>. Per creare un cookie permanente, `IsPersistent` deve essere impostata. In caso contrario, il cookie viene creato con un ciclo di vita basati su sessione e far scadere prima o dopo il ticket di autenticazione che contiene. Quando `ExpiresUtc` è impostato, sostituisce il valore della <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> opzione di <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, se impostata.

Il frammento di codice seguente crea un'identità e un corrispondente cookie che dura per 20 minuti. Ignora eventuali impostazioni di scadenza scorrevole configurati in precedenza.

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

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [Controlli di ruolo basata su criteri](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
