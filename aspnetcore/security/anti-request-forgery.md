---
title: Evitare attacchi Cross-Site Request Forgery (XSRF/CSRF) in ASP.NET Core
author: steve-smith
description: Informazioni su come prevenire gli attacchi contro le app web in cui un sito Web dannoso può influenzare l'interazione tra un browser client e l'app.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 6e140717834b901e12ef7863fd07b983b0c55107
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667661"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Evitare attacchi Cross-Site Request Forgery (XSRF/CSRF) in ASP.NET Core

Dal [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), e [Rick Anderson](https://twitter.com/RickAndMSFT)

Richiesta intersito falsa (nota anche come XSRF o CSRF, pronunciato *vedere entrando*) è un attacco contro applicazioni ospitate sul web in base al quale un'app web dannoso può influenzare l'interazione tra un browser client e un'app web che considera attendibile che Browser. Questi attacchi sono possibili in quanto i browser web inviano alcuni tipi di token di autenticazione automaticamente con ogni richiesta a un sito Web. Questa forma di attacco è noto anche come un *attacco di un solo clic* oppure *sessione puntato* perché l'attacco sfrutta l'utente autenticato del precedentemente sessione.

Un esempio di un attacco CSRF:

1. Un utente accede a `www.good-banking-site.com` tramite autenticazione basata su form. Il server autentica l'utente e invia una risposta che include un cookie di autenticazione. Il sito è vulnerabile agli attacchi perché considera attendibili tutte le richieste che riceve con un cookie di autenticazione valido.
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

   Si noti che il form `action` post per il sito vulnerabile, non per il sito dannoso. Questa è la parte "intersito" di CSRF.

1. L'utente seleziona il pulsante di invio. Il browser invia la richiesta e include automaticamente i cookie di autenticazione per il dominio richiesto, `www.good-banking-site.com`.
1. La richiesta viene eseguita di `www.good-banking-site.com` server con il contesto di autenticazione dell'utente e può eseguire qualsiasi azione che un utente autenticato è autorizzato a eseguire.

Oltre allo scenario in cui l'utente seleziona il pulsante per inviare il modulo, il sito dannoso è stato possibile:

* Eseguire uno script che invia automaticamente il form.
* Inviare l'invio del modulo come una richiesta AJAX.
* Nascondere il modulo usando lo stile CSS.

Questi scenari alternativi non richiedono qualsiasi azione e l'input dell'utente diverse da inizialmente visitando il sito dannoso.

L'uso di HTTPS non impedire un attacco CSRF. Il sito dannoso può inviare un `https://www.good-banking-site.com/` richiesta con la stessa semplicità può inviare una richiesta non sicura.

Alcuni attacchi colpiscono che rispondono alle richieste GET, nel qual caso un tag di immagine può essere utilizzato per eseguire l'azione. Questa forma di attacco è comune nei siti di forum che consentono le immagini, ma Blocca JavaScript. Le app che modificano lo stato per le richieste GET, in cui le variabili o le risorse vengono modificate, sono vulnerabili ad attacchi dannosi. **Le richieste GET che modificano lo stato sono non sicure. Una procedura consigliata consiste nel non modificare mai lo stato in una richiesta GET.**

Attacchi CSRF, sono possibili con App web che usano i cookie per l'autenticazione perché:

* Browser archiviare i cookie emessi da un'app web.
* I cookie stored includono i cookie di sessione per gli utenti autenticati.
* I browser inviano che tutti i cookie associati a un dominio all'app web di tutte le richieste indipendentemente dal modo in cui è stata generata la richiesta all'app all'interno del browser.

Tuttavia, gli attacchi CSRF non sono limitati a sfruttare i cookie. Ad esempio, l'autenticazione di base e classificata sono anche vulnerabili. Dopo che un utente accede con l'autenticazione di base o Digest, il browser invia automaticamente le credenziali finché la sessione&dagger; termina.

&dagger;In questo contesto *sessione* fa riferimento alla sessione dal lato client durante il quale l'utente viene autenticato. È non correlata alle sessioni sul lato server oppure [Middleware di ASP.NET Core sessione](xref:fundamentals/app-state).

Gli utenti possono proteggersi contro vulnerabilità CSRF adottando delle precauzioni:

* Firma dell'App web di termine del loro utilizzo.
* Periodicamente i cookie del browser non crittografato.

Tuttavia, le vulnerabilità CSRF sono fondamentalmente un problema dell'app web, non dell'utente finale.

## <a name="authentication-fundamentals"></a>Nozioni fondamentali di autenticazione

L'autenticazione basata su cookie è una forma più diffusi di autenticazione. Sistemi di autenticazione basata su token sono sempre più diffusi, in particolare per le applicazioni a pagina singola (SPAs).

### <a name="cookie-based-authentication"></a>Autenticazione basata su cookie

Quando un utente esegue l'autenticazione con nome utente e password, che siano emessi un token, contenente un ticket di autenticazione che può essere utilizzato per l'autenticazione e autorizzazione. Il token viene archiviato come rende un cookie associato a ogni richiesta del client. Generazione e la convalida questo cookie viene eseguita dal Middleware di autenticazione dei Cookie. Il [middleware](xref:fundamentals/middleware/index) serializza un'entità utente in un cookie crittografato. Nelle richieste successive, il middleware convalida il cookie, ricrea l'entità e assegna l'entità per il [utente](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) proprietà di [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Autenticazione basata su token

Quando un utente viene autenticato, che siano emessi token (non un token anti falsificazione). Il token contiene informazioni sull'utente sotto forma di [attestazioni](/dotnet/framework/security/claims-based-identity-model) o un token di riferimento che punta all'app per lo stato utente gestito nell'app. Quando un utente tenta di accedere a una risorsa che richiede l'autenticazione, il token viene inviato all'app con un'intestazione di autorizzazione aggiuntive sotto forma di token di connessione. In questo modo l'app senza stato. In ogni richiesta successiva, il token viene passato nella richiesta per la convalida sul lato server. Questo token non è *crittografato*; ha *codificato*. Nel server, il token viene decodificato per accedere alle relative informazioni. Per inviare il token nelle richieste successive, archiviare il token nell'archiviazione locale del browser. Non c'è da preoccuparsi delle vulnerabilità CSRF se il token viene memorizzato nell'archivio locale del browser. CSRF costituisce un problema quando il token viene archiviato in un cookie.

### <a name="multiple-apps-hosted-at-one-domain"></a>Più App ospitata in un dominio

Ambienti di hosting condivisi sono vulnerabili a Hijack della sessione, account di accesso CSRF e altri attacchi.

Sebbene `example1.contoso.net` e `example2.contoso.net` sono diversi host, c'è una relazione di trust implicita tra gli host sotto il `*.contoso.net` dominio. Questa relazione di trust implicita consente agli host potenzialmente non attendibili influire sul reciproci cookie (i criteri di corrispondenza dell'origine che regolano le richieste AJAX non vengono necessariamente applicano per i cookie HTTP).

Per impedire gli attacchi che sfruttano i cookie attendibili tra le app ospitate nello stesso dominio, è possono non condivide i domini. Quando ogni app è ospitata nel proprio dominio, non vi è alcuna relazione di trust implicita cookie da sfruttare.

## <a name="aspnet-core-antiforgery-configuration"></a>Configurazione anti falsificazione di ASP.NET Core

> [!WARNING]
> ASP.NET Core implementa tramite anti falsificazione [protezione di dati di ASP.NET Core](xref:security/data-protection/introduction). Lo stack di protezione dati deve essere configurato per funzionare in una server farm. Visualizzare [configurazione della protezione dati](xref:security/data-protection/configuration/overview) per altre informazioni.

In ASP.NET Core 2.0 o versioni successive, il [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) inserisce il token anti falsificazione in elementi del form HTML. Il markup seguente in un file Razor genera automaticamente i token anti falsificazione:

```cshtml
<form method="post">
    ...
</form>
```

Analogamente, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) genera token antifalsificazione per impostazione predefinita, se il metodo del form non GET.

La generazione automatica dei token antifalsificazione per elementi del form HTML si verifica quando la `<form>` tag contiene il `method="post"` attributo e uno dei modi seguenti sono vere:

  * L'attributo action è vuoto (`action=""`).
  * Non è specificato l'attributo di azione (`<form method="post">`).

È possibile disabilitare la generazione automatica dei token antifalsificazione per elementi del form HTML:

* Disabilitare in modo esplicito i token antifalsificazione con il `asp-antiforgery` attributo:

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* L'elemento del form viene scelto esplicitamente di helper Tag usando l'Helper Tag [! esclusione simboli](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* Rimuovere il `FormTagHelper` dalla vista. Il `FormTagHelper` può essere rimosso da una vista aggiungendo la seguente direttiva per la visualizzazione Razor:

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor Pages](xref:razor-pages/index) vengono protetti automaticamente dalle XSRF/CSRF. Per altre informazioni, vedere [XSRF/CSRF e Razor Pages](xref:razor-pages/index#xsrf).

L'approccio più comune per difendersi dagli attacchi CSRF consiste nell'usare la *modello di sincronizzazione del Token* (STP). STP viene usato quando l'utente richiede una pagina con i dati del modulo:

1. Il server invia un token associato all'identità dell'utente corrente al client.
1. Il client invia il token al server per la verifica.
1. Se il server riceve un token che non corrisponde all'identità dell'utente autenticato, la richiesta viene rifiutata.

Il token è univoco e imprevedibili. Il token è anche utilizzabile per verificare che la sequenza appropriata di una serie di richieste (ad esempio, garantendo la sequenza di richiesta di: pagina 1 &ndash; pagina 2 &ndash; pagina 3). Tutti i moduli nei modelli di ASP.NET Core MVC e Razor Pages di generare token antifalsificazione. La seguente coppia di Visualizza esempi genera token anti falsificazione:

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Aggiungere in modo esplicito un token anti falsificazione a un `<form>` elemento senza usare gli helper Tag con l'helper HTML [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

In ognuno dei casi precedenti, ASP.NET Core aggiunge un campo del form nascosto simile al seguente:

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core include tre [filtri](xref:mvc/controllers/filters) per l'utilizzo di token anti falsificazione:

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Opzioni di anti falsificazione

Personalizzare [opzioni di anti falsificazione](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

&dagger;Impostare l'anti falsificazione `Cookie` delle proprietà usando la proprietà dell'oggetto di [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) classe.

| Opzione | Descrizione |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Determina le impostazioni utilizzate per creare i cookie antifalsificazione. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Il nome del campo del form nascosto utilizzato dal sistema antifalsificazione per il rendering di un token anti falsificazione in viste. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Il nome dell'intestazione utilizzato dal sistema antifalsificazione. Se `null`, il sistema prende in considerazione solo i dati del modulo. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Specifica se eliminare la generazione del `X-Frame-Options` intestazione. Per impostazione predefinita, l'intestazione viene generato con un valore di "SAMEORIGIN". Il valore predefinito è `false`. |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

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
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Determina le impostazioni utilizzate per creare i cookie antifalsificazione. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Il dominio del cookie. Il valore predefinito è `null`. Questa proprietà è obsoleta e verrà rimossa in una versione futura. L'alternativa consigliata è Cookie.Domain. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Il nome del cookie. Se non impostato, il sistema genera un nome univoco che inizia con la [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery."). Questa proprietà è obsoleta e verrà rimossa in una versione futura. L'alternativa consigliata è Cookie.Name. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Il percorso impostato nel cookie. Questa proprietà è obsoleta e verrà rimossa in una versione futura. L'alternativa consigliata è Cookie.Path. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Il nome del campo del form nascosto utilizzato dal sistema antifalsificazione per il rendering di un token anti falsificazione in viste. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Il nome dell'intestazione utilizzato dal sistema antifalsificazione. Se `null`, il sistema prende in considerazione solo i dati del modulo. |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | Specifica se il sistema antifalsificazione richiede HTTPS. Se `true`, non HTTPS richieste hanno esito negativo. Il valore predefinito è `false`. Questa proprietà è obsoleta e verrà rimossa in una versione futura. L'alternativa consigliata consiste nell'impostare Cookie.SecurePolicy. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Specifica se eliminare la generazione del `X-Frame-Options` intestazione. Per impostazione predefinita, l'intestazione viene generato con un valore di "SAMEORIGIN". Il valore predefinito è `false`. |

::: moniker-end

Per altre informazioni, vedere [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>Configurare le funzionalità anti falsificazione con IAntiforgery

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) fornisce l'API per configurare le funzionalità di anti falsificazione. `IAntiforgery` può essere richiesta nel `Configure` metodo di `Startup` classe. L'esempio seguente usa middleware dalla home page dell'app per generare un token anti falsificazione e inviarlo nella risposta sotto forma di cookie (tramite l'impostazione predefinita Angular convenzione di denominazione descritta più avanti in questo argomento):

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

### <a name="require-antiforgery-validation"></a>Richiedere la convalida antifalsificazione

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) è un filtro azione che può essere applicato a una singola azione, un controller o a livello globale. Le richieste effettuate alle azioni che hanno applicato questo filtro vengono bloccate a meno che la richiesta include un token anti falsificazione valido.

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

Il `ValidateAntiForgeryToken` attributo richiede un token per le richieste ai metodi di azione che decora, tra cui le richieste HTTP GET. Se il `ValidateAntiForgeryToken` attributo è applicato tra controller dell'app, che può essere sostituito con il `IgnoreAntiforgeryToken` attributo.

> [!NOTE]
> ASP.NET Core non supporta l'aggiunta di token anti falsificazione alle richieste GET automaticamente.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Convalidare automaticamente i token antifalsificazione per solo i metodi HTTP non sicuri

Le app ASP.NET Core non generano i token antifalsificazione per safe metodi HTTP (GET, HEAD, opzioni e traccia). Anziché applicare ampiamente la `ValidateAntiForgeryToken` attributo e quindi si esegue l'override con `IgnoreAntiforgeryToken` attributi, la [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attributo può essere utilizzato. Questo attributo funziona in modo identico al `ValidateAntiForgeryToken` attributo, ad eccezione del fatto che non richiede token per le richieste effettuate usando i metodi HTTP seguenti:

* GET
* HEAD
* OPZIONI
* TRACE

È consigliabile usare `AutoValidateAntiforgeryToken` su vasta scala per gli scenari non-API. In questo modo le azioni POST sono protetti per impostazione predefinita. In alternativa è possibile ignorare i token antifalsificazione per impostazione predefinita, a meno che non `ValidateAntiForgeryToken` viene applicata a singoli metodi di azione. È più probabile che in questo scenario per un metodo di azione POST deve rimanere non protetto per errore, uscire dall'app vulnerabile agli attacchi CSRF. Tutti i post devono inviare il token antifalsificazione.

Le API non sono un meccanismo automatico per inviare la parte non cookie del token. L'implementazione dipende probabilmente dall'implementazione del codice client. Di seguito sono riportati alcuni esempi:

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

### <a name="override-global-or-controller-antiforgery-attributes"></a>Override globale o controller anti falsificazione attributi

Il [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filtro viene utilizzato per eliminare la necessità di un token anti falsificazione per una determinata azione (o controller). Quando applicata, esegue l'override di questo filtro `ValidateAntiForgeryToken` e `AutoValidateAntiforgeryToken` filtri specificati a un livello superiore (a livello globale o in un controller).

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

I token devono essere aggiornati dopo che l'utente viene autenticato, reindirizzare l'utente a una pagina Razor Pages o una vista.

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX e SPA (Single Page Application)

Nelle App basate su HTML tradizionale, i token anti falsificazione vengono passati al server usando i campi modulo nascosti. Nell'App moderne basate su JavaScript e SPAs, molte richieste vengono effettuate a livello di codice. Queste richieste AJAX possono utilizzare altre tecniche, quali intestazioni della richiesta o i cookie, inviare il token.

Se i cookie vengono usati per archiviare i token di autenticazione e per autenticare le richieste di API sul server, CSRF è un potenziale problema. Se l'archiviazione locale viene usata per archiviare il token, CSRF vulnerabilità potrebbe essere ridotta perché i valori dall'archiviazione locale non vengono inviati automaticamente al server con ogni richiesta. Di conseguenza, usando un'archiviazione locale per archiviare il token anti falsificazione sul client che invia il token come intestazione della richiesta è un approccio consigliato.

### <a name="javascript"></a>JavaScript

Utilizzo di JavaScript con visualizzazioni, il token è possibile creare usando un servizio dall'interno della visualizzazione. Inserire il [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) servizio nella visualizzazione e chiamare [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

Questo approccio elimina la necessità di interagire direttamente con l'impostazione di cookie dal server o la relativa lettura dai client.

L'esempio precedente Usa JavaScript per leggere il valore del campo nascosto per l'intestazione di AJAX POST.

JavaScript è possibile anche i token nei cookie di accesso e usare il contenuto del cookie per creare un'intestazione con il valore del token.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Supponendo che lo script richiede di inviare il token in un'intestazione denominata `X-CSRF-TOKEN`, configurare il servizio anti falsificazione per cercare il `X-CSRF-TOKEN` intestazione:

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

AngularJS Usa una convenzione all'indirizzo CSRF. Se il server invia un cookie con il nome `XSRF-TOKEN`, AngularJS `$http` servizio aggiunge il valore del cookie a un'intestazione quando viene inviata una richiesta al server. Questo processo è automatico. L'intestazione non deve essere impostata in modo esplicito nel client. È il nome dell'intestazione `X-XSRF-TOKEN`. Il server deve rilevare questa intestazione e convalidare il contenuto.

Per ASP.NET Core API da usare con questa convenzione nell'avvio dell'applicazione:

* Configurare l'app per fornire un token in un cookie denominato `XSRF-TOKEN`.
* Configurare il servizio per individuare un'intestazione denominata anti falsificazione `X-XSRF-TOKEN`.

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

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Estendere anti falsificazione

Il [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo consente agli sviluppatori di estendere il comportamento del sistema Intersito, i dati aggiuntivi di andata e ritorno in ogni token. Il [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) viene chiamato ogni volta che viene generato un token di campo e il valore restituito è incorporato all'interno del token generato. Un implementatore potrebbe restituire un timestamp, un parametro nonce o qualsiasi altro valore, quindi chiamare [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) per convalidare i dati quando il token viene convalidato. Nome utente del client è già incorporato nei token generati, pertanto non è necessario includere queste informazioni. Se un token include dati aggiuntivi, ma non `IAntiForgeryAdditionalDataProvider` è configurato, non vero convalidati dati aggiuntivi.

## <a name="additional-resources"></a>Risorse aggiuntive

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) sul [aprire Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).
* <xref:host-and-deploy/web-farm>
