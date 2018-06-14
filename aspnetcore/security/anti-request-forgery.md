---
title: Evitare attacchi Cross-Site Request Forgery (XSRF/CSRF) in ASP.NET Core
author: steve-smith
description: Informazioni su come prevenire gli attacchi contro le app web in un sito Web dannoso può influenzare l'interazione tra un browser del client e l'app.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 3bca96f4a2e247eeeb93140df93221371d88d4d3
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341860"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Evitare attacchi Cross-Site Request Forgery (XSRF/CSRF) in ASP.NET Core

Dal [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), e [Rick Anderson](https://twitter.com/RickAndMSFT)

Falsificazione della richiesta tra siti (noto anche come XSRF o CSRF, accentuato *tra superfici vedere*) è un attacco contro App web ospitate in base al quale un'app web dannoso può influenzare l'interazione tra un browser del client e un'app web che considera attendibile che Browser. Questi attacchi sono possibili in quanto i web browser invia alcuni tipi di token di autenticazione automaticamente con tutte le richieste a un sito Web. Questa forma di attacco è noto anche come un *con un clic attacco* oppure *sessione da* perché l'attacco sfrutta l'utente autenticato del precedentemente sessione.

Un esempio di un attacco di tipo CSRF:

1. Un utente accede a `www.good-banking-site.com` tramite autenticazione basata su form. Il server autentica l'utente e invia una risposta che include un cookie di autenticazione. Il sito è vulnerabile agli attacchi poiché si affida qualsiasi richiesta che riceve con un cookie di autenticazione valido.
1. L'utente visita un sito dannoso, `www.bad-crook-site.com`.

   Il sito dannoso, `www.bad-crook-site.com`, contiene un form HTML simile al seguente:

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   Si noti che il modulo `action` post per il sito vulnerabile, non per il sito dannoso. Questa è la parte "cross-site" di CSRF.

1. L'utente seleziona il pulsante di invio. Il browser effettua la richiesta e include automaticamente il cookie di autenticazione per il dominio richiesto `www.good-banking-site.com`.
1. La richiesta viene eseguita nel `www.good-banking-site.com` server con il contesto di autenticazione dell'utente e possono eseguire qualsiasi azione che un utente autenticato è autorizzato a eseguire.

Oltre a uno scenario in cui l'utente seleziona il pulsante di invio del form, il sito dannoso è stato possibile:

* Eseguire uno script che invia automaticamente il form.
* Inviare l'invio del modulo come una richiesta AJAX.
* Nascondere il form utilizzando CSS.

Questi scenari alternativi non richiedono un'azione o l'input dell'utente diverso da inizialmente visitare il sito dannoso.

L'uso di HTTPS non impedire un attacco di tipo CSRF. Il sito dannoso può inviare un `https://www.good-banking-site.com/` richiedere la stessa facilità con cui può inviare una richiesta non protetta.

Alcuni attacchi di destinazione che rispondono alle richieste GET, nel qual caso un tag di immagine può essere utilizzato per eseguire l'azione. Questa forma di attacco è comune nei siti di forum che consentono le immagini ma bloccano JavaScript. Le app che modificano lo stato per le richieste GET, in cui le variabili o le risorse vengono modificate, sono vulnerabili ad attacchi dannosi. **Le richieste GET che modificano lo stato sono non protette. Una procedura consigliata consiste nel non modificare mai stato in una richiesta GET.**

Attacchi CSRF sono possibili con le app web che usano i cookie per l'autenticazione perché:

* Browser memorizzano i cookie emessi da un'app web.
* Cookie stored includono i cookie di sessione per gli utenti autenticati.
* Browser inviano che tutti i cookie associati a un dominio all'app web tutte le richieste indipendentemente dal modo in cui è stata generata la richiesta all'app all'interno del browser.

Tuttavia, non sono limitati CSRF attacchi per sfruttare i cookie. Ad esempio, l'autenticazione di base e classificata sono anche vulnerabili. Dopo che un utente accede con l'autenticazione di base o Digest, il browser invia automaticamente le credenziali finché la sessione&dagger; termina.

&dagger;In questo contesto *sessione* fa riferimento alla sessione dal lato client durante i quali l'autenticazione dell'utente. È non correlati alle sessioni sul lato server o [ASP.NET Core sessione Middleware](xref:fundamentals/app-state).

Gli utenti possono proteggersi dal vulnerabilità CSRF adottando delle precauzioni:

* Firma dell'App web al termine di usarli.
* Periodicamente i cookie del browser non crittografato.

Tuttavia, le vulnerabilità CSRF sono fondamentalmente un problema dell'app web, non dell'utente finale.

## <a name="authentication-fundamentals"></a>Nozioni fondamentali di autenticazione

L'autenticazione basata su cookie è una forma comune di autenticazione. Sistemi di autenticazione basata su token sono sempre più diffusi, soprattutto per applicazioni a pagina singola (SPAs).

### <a name="cookie-based-authentication"></a>Autenticazione basata su cookie

Quando un utente viene autenticato mediante il nome utente e password, che sta emessi un token contenente un ticket di autenticazione che può essere utilizzato per l'autenticazione e autorizzazione. Il token viene memorizzato come rende un cookie associato a ogni richiesta client. Generazione e la convalida di questo cookie viene eseguita dal Middleware del Cookie di autenticazione. Il [middleware](xref:fundamentals/middleware/index) serializza un'entità utente in un cookie crittografato. Nelle richieste successive, il middleware convalida il cookie, ricrea l'entità e assegna all'entità del [utente](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) proprietà di [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Autenticazione basata su token

Quando un utente viene autenticato, essi sta rilasciati un token (non un token non riproducibili). Il token contiene le informazioni utente nel formato [attestazioni](/dotnet/framework/security/claims-based-identity-model) o un token di riferimento che punta all'app lo stato utente gestito nell'app. Quando un utente tenta di accedere a una risorsa che richiede l'autenticazione, il token viene inviato all'app con un'intestazione di autorizzazione aggiuntive sotto forma di token di connessione. In questo modo l'app senza stato. In tutte le richieste successive, il token viene passato nella richiesta per la convalida lato server. Questo token non è *crittografati*; ha *codificato*. Nel server, il token viene decodificato per accedere alle relative informazioni. Per inviare il token nelle richieste successive, memorizzare il token nel servizio di archiviazione locale del browser. Non occorre preoccuparsi sulle vulnerabilità CSRF se il token viene memorizzato nell'archivio locale del browser. CSRF costituisce un problema quando il token viene archiviato in un cookie.

### <a name="multiple-apps-hosted-at-one-domain"></a>Più App ospitate in un dominio

Ambienti di hosting condivisi sono vulnerabili a Hijack della sessione, account di accesso CSRF e altri attacchi.

Sebbene `example1.contoso.net` e `example2.contoso.net` sono host diversi, c'è una relazione di trust implicita tra gli host sotto il `*.contoso.net` dominio. Questa relazione di trust implicita consente agli host potenzialmente non attendibili determinare i cookie di altro (lo stessa origine i criteri che regolano le richieste AJAX non applicano necessariamente ai cookie HTTP).

Per impedire gli attacchi che sfruttano i cookie attendibili tra le applicazioni ospitate nello stesso dominio, è possono non condivide i domini. Quando ogni app è ospitata nel proprio dominio, non vi è alcuna relazione di trust implicita cookie da sfruttare.

## <a name="aspnet-core-antiforgery-configuration"></a>Configurazione di ASP.NET Core non riproducibili

> [!WARNING]
> ASP.NET Core implementa usando non riproducibili [protezione dei dati di ASP.NET Core](xref:security/data-protection/introduction). Lo stack di protezione dati deve essere configurato per l'utilizzo in una server farm. Vedere [la configurazione di protezione dei dati](xref:security/data-protection/configuration/overview) per ulteriori informazioni.

In ASP.NET Core 2.0 o versione successiva, il [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) inserisce il token non riproducibili in elementi del form HTML. Il markup seguente in un file Razor genera automaticamente i token non riproducibili:

```cshtml
<form method="post">
    ...
</form>
```

Analogamente, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) genera i token non riproducibili per impostazione predefinita, se il metodo del form non è GET.

La generazione automatica di token non riproducibili per elementi del form HTML si verifica quando il `<form>` tag contiene il `method="post"` attributi e agli elementi seguenti sono vere:

  * L'attributo action è vuoto (`action=""`).
  * Non è specificato l'attributo action (`<form method="post">`).

È possibile disabilitare la generazione automatica di token non riproducibili per elementi del form HTML:

* Disabilitare in modo esplicito i token non riproducibili con il `asp-antiforgery` attributo:

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* L'elemento di modulo viene scelto esplicitamente gli helper di Tag utilizzando l'Helper di Tag [! simbolo opt-out](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* Rimuovere il `FormTagHelper` dalla vista. Il `FormTagHelper` può essere rimosso da una vista aggiungendo la seguente direttiva alla visualizzazione Razor:

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Pagine Razor](xref:mvc/razor-pages/index) vengono protetti automaticamente dalle XSRF/CSRF. Per altre informazioni, vedere [XSRF/CSRF e pagine Razor](xref:mvc/razor-pages/index#xsrf).

L'approccio più comune per difendersi da attacchi di tipo CSRF consiste nell'utilizzare il *modello di Token sincronizzazione* (STP). STP viene utilizzato quando l'utente richiede una pagina di dati del form:

1. Il server invia un token associato all'identità dell'utente corrente al client.
1. Il client invia il token per il server per la verifica.
1. Se il server riceve un token che non corrisponde all'identità dell'utente autenticato, la richiesta viene rifiutata.

Il token è univoco e imprevedibili. Il token anche utilizzabile per verificare che la sequenza corretta di una serie di richieste (ad esempio, assicurando la sequenza di richiesta di: pagina 1 &ndash; pagina 2 &ndash; pagina 3). Tutti i moduli nei modelli di MVC ASP.NET Core e le pagine Razor genera i token non riproducibili. La seguente coppia di esempi di vista genera i token non riproducibili:

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Aggiungere in modo esplicito un token non riproducibili a un `<form>` elemento senza utilizzare gli helper di Tag con l'helper HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

In ognuno dei casi precedenti, ASP.NET Core aggiunge un campo modulo nascosto simile al seguente:

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core include tre [filtri](xref:mvc/controllers/filters) per l'utilizzo di token non riproducibili:

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Opzioni non riproducibili

Personalizzare [opzioni non riproducibili](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| Opzione | Descrizione |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Determina le impostazioni utilizzate per creare i cookie non riproducibili. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Il dominio del cookie. Il valore predefinito è `null`. Questa proprietà è obsoleta e verrà rimossa in una versione futura. L'alternativa consigliata è Cookie.Domain. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Il nome del cookie. Se non è impostata, il sistema genera un nome univoco che inizia con il [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery."). Questa proprietà è obsoleta e verrà rimossa in una versione futura. L'alternativa consigliata è Cookie.Name. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Il percorso impostato nel cookie. Questa proprietà è obsoleta e verrà rimossa in una versione futura. L'alternativa consigliata è Cookie.Path. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Il nome del campo del form nascosto usato dal sistema non riproducibili per il rendering di un token non riproducibili nelle viste. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Il nome dell'intestazione utilizzato dal sistema non riproducibili. Se `null`, il sistema prende in considerazione solo i dati del modulo. |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | Specifica se SSL è richiesto dal sistema non riproducibili. Se `true`, le richieste non SSL esito negativo. Il valore predefinito è `false`. Questa proprietà è obsoleta e verrà rimossa in una versione futura. L'alternativa consigliata consiste nell'impostare Cookie.SecurePolicy. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Specifica se eliminare la generazione del `X-Frame-Options` intestazione. Per impostazione predefinita, viene generata l'intestazione con un valore di "SAMEORIGIN". Il valore predefinito è `false`. |

Per altre informazioni, vedere [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>Configurare le funzionalità non riproducibili con IAntiforgery

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) fornisce l'API per configurare le funzionalità non riproducibili. `IAntiforgery` può essere richiesta nel `Configure` metodo la `Startup` classe. L'esempio seguente usa middleware dalla home page dell'app per generare un token non riproducibili e inviarlo nella risposta sotto forma di cookie (tramite l'impostazione predefinita Angular convenzione di denominazione descritta più avanti in questo argomento):

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a>Richiedere la convalida non riproducibili

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) è un filtro azione che può essere applicato a una singola azione, un controller o a livello globale. Le richieste effettuate alle azioni che è sono applicato questo filtro vengono bloccate, a meno che la richiesta include un token non riproducibili valido.

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

Il `ValidateAntiForgeryToken` attributo richiede un token per le richieste ai metodi di azione decora, incluse le richieste HTTP GET. Se il `ValidateAntiForgeryToken` attributo viene applicato tra i controller dell'app, è possibile sostituirlo con il `IgnoreAntiforgeryToken` attributo.

> [!NOTE]
> ASP.NET Core non supporta l'aggiunta automatica di token non riproducibili alle richieste GET.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Convalidare automaticamente i token non riproducibili per solo i metodi HTTP non sicuri

Le app ASP.NET Core non generano token non riproducibili provvisoria metodi HTTP (GET, HEAD, opzioni e traccia). Anziché applicare su vasta scala il `ValidateAntiForgeryToken` attributo e quindi si esegue l'override con `IgnoreAntiforgeryToken` attributi, la [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attributo può essere utilizzato. Questo attributo funziona esattamente come il `ValidateAntiForgeryToken` attributo, ad eccezione del fatto che non richiede token per le richieste effettuate utilizzando i metodi HTTP seguenti:

* GET
* HEAD
* OPZIONI
* TRACE

Si consiglia l'uso di `AutoValidateAntiforgeryToken` su vasta scala per gli scenari non API. In questo modo le azioni di POST sono protetti per impostazione predefinita. L'alternativa consiste nell'ignorare i token non riproducibili per impostazione predefinita, a meno che non `ValidateAntiForgeryToken` si riferisce a singoli metodi di azione. Non può essere più in questo scenario per un metodo di azione POST deve rimanere protetto per errore, lasciando vulnerabile agli attacchi CSRF l'app. Tutte le pubblicazioni devono inviare il token non riproducibili.

Le API non sono un meccanismo automatico per l'invio la parte non cookie del token. L'implementazione dipende probabilmente l'implementazione del codice client. Di seguito sono illustrati alcuni esempi:

Esempio a livello di classe:

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Esempio globale:

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a>Eseguire l'override globale o attributi non riproducibili controller

Il [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtro viene utilizzato per eliminare la necessità di un token non riproducibili per una determinata azione (o controller). Quando viene applicato, questo filtro esegue l'override `ValidateAntiForgeryToken` e `AutoValidateAntiforgeryToken` i filtri specificati a un livello superiore (a livello globale o in un controller).

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a>Token di aggiornamento dopo l'autenticazione

I token devono essere aggiornati dopo che l'utente viene autenticato mediante il reindirizzamento all'utente a una vista o una pagina di pagine Razor.

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX e SPA (Single Page Application)

In tradizionali App basate su HTML, i token non riproducibili vengono passati al server utilizzando i campi del form nascosto. In App moderne basate su JavaScript e SPAs, molte richieste vengono eseguite a livello di codice. Queste richieste AJAX possono utilizzare altre tecniche (ad esempio le intestazioni di richiesta o i cookie) per inviare il token.

Se i cookie vengono usati per archiviare i token di autenticazione e per autenticare le richieste API nel server, CSRF costituisce un potenziale problema. Se l'archiviazione locale viene usata per archiviare il token, CSRF vulnerabilità potrebbe essere ridotta perché i valori dall'archivio locale non vengono inviati automaticamente al server con ogni richiesta. Pertanto, utilizzare l'archiviazione locale per archiviare il token non riproducibili sul client che invia il token come intestazione della richiesta è un approccio consigliato.

### <a name="javascript"></a>JavaScript

Utilizzo di JavaScript con visualizzazioni, il token è possibile creare utilizzando un servizio dall'interno della visualizzazione. Inserire il [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) servizio nella vista e chiamata [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

Questo approccio elimina la necessità di interagire direttamente con l'impostazione di cookie dal server o la relativa lettura dai client.

Nell'esempio precedente Usa JavaScript per leggere il valore del campo nascosto per l'intestazione AJAX POST.

JavaScript è possibile anche i token di cookie di accesso e utilizzare il contenuto del cookie per creare un'intestazione con il valore del token.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Presupponendo che lo script richiede per inviare il token in un'intestazione denominata `X-CSRF-TOKEN`, configurare il servizio non riproducibili per cercare il `X-CSRF-TOKEN` intestazione:

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

L'esempio seguente usa JavaScript per eseguire una richiesta AJAX con l'intestazione appropriata:

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a>AngularJS

AngularJS viene utilizzata una convenzione all'indirizzo CSRF. Se il server invia un cookie con il nome `XSRF-TOKEN`, AngularJS `$http` servizio aggiunge il valore del cookie a un'intestazione quando invia una richiesta al server. Questo processo è automatico. L'intestazione non deve necessariamente essere impostata in modo esplicito. Il nome di intestazione è `X-XSRF-TOKEN`. Il server deve rilevare questa intestazione e convalidare il contenuto.

Utilizzare questa convenzione per ASP.NET Core API:

* Configurare l'app per fornire un token di un cookie denominato `XSRF-TOKEN`.
* Configurare il servizio non riproducibili per cercare un'intestazione denominata `X-XSRF-TOKEN`.

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Estendere antiforgery

Il [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo consente agli sviluppatori di estendere il comportamento del sistema anti-CSRF, i dati aggiuntivi di round trip in ogni token. Il [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) metodo viene chiamato ogni volta che viene generato un token di campo e il valore restituito è incorporato all'interno del token generato. Un responsabile dell'implementazione potrebbe restituire un timestamp, un parametro nonce o qualsiasi altro valore e quindi chiamare [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) per la convalida dei dati quando il token viene convalidato. Nome utente del client è già incorporato nei token generati, pertanto non è necessario includere queste informazioni. Se un token include dati supplementari ma nessun `IAntiForgeryAdditionalDataProvider` è configurato, i dati supplementari non convalidati.

## <a name="additional-resources"></a>Risorse aggiuntive

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) sul [aprire il progetto di sicurezza dell'applicazione Web](https://www.owasp.org/index.php/Main_Page) (OWASP).
