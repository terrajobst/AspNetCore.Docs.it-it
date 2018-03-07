---
title: Evitare Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core
author: steve-smith
description: "Informazioni su come prevenire gli attacchi contro le app web in un sito Web dannoso può influenzare l'interazione tra un browser del client e l'app."
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 80651a3c3e4c722e0cb96d7cc07de366819f8d1d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>Evitare Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core

[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), e [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-attack-does-anti-forgery-prevent"></a>Quali attacco impedire antifalsificazione?

Richieste intersito false (noto anche come XSRF o CSRF, pronuncia *tra superfici vedere*) è un attacco contro applicazioni ospitate da web in base al quale un sito web in grado di influenzare l'interazione tra un browser del client e un sito web che considera attendibile tale browser. Questi attacchi sono possibili in quanto web browser invia alcuni tipi di token di autenticazione automaticamente con ogni richiesta a un sito web. Questa forma di attacco del noto anche come un *attacco con un clic* o come *sessione riding*, perché l'attacco sfrutta i vantaggi dell'utente dell'autenticazione in precedenza sessione.

Un esempio di un attacco di tipo CSRF:

1. Un utente accede a `www.example.com`, con autenticazione basata su form.
2. Il server autentica l'utente e invia una risposta che include un cookie di autenticazione.
3. L'utente visita un sito dannoso.

   Il sito dannoso contiene un form HTML simile al seguente:

   ```html
   <h1>You Are a Winner!</h1>
   <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click Me">
   </form>
   ```

Si noti che le azioni form invia al sito vulnerabile, non per il sito dannoso. Questa è la parte "cross-site" di CSRF.

4. L'utente fa clic sul pulsante Invia. Il visualizzatore include automaticamente il cookie di autenticazione per il dominio richiesto (sito vulnerabile in questo caso) con la richiesta.
5. La richiesta viene eseguita nel server con il contesto di autenticazione dell'utente e può eseguire qualsiasi operazione che è consentito un utente autenticato.

In questo esempio richiede all'utente di fare clic sul pulsante del form. La pagina è possibile:

* Eseguire uno script che invia automaticamente il form.
* Invia l'invio di un form come una richiesta AJAX. 
* Utilizzare un form nascosto con CSS. 

Un attacco di tipo CSRF non impedisce l'uso di SSL, il sito dannoso può inviare un `https://` richiesta. 

Alcuni attacchi di destinazione gli endpoint del sito che rispondono alle `GET` richieste, in cui un tag di immagine case consente di eseguire l'azione (questa forma di attacco è comune nei siti di forum che consentono di immagini ma bloccare JavaScript). Le applicazioni che modificano lo stato con `GET` richieste sono vulnerabili da attacchi dannosi.

Attacchi CSRF sono possibili contro i siti web che usano i cookie per l'autenticazione, in quanto browser invia tutti i cookie pertinenti al sito web di destinazione. Tuttavia, gli attacchi CSRF non sono limitati per sfruttare i cookie. Ad esempio, l'autenticazione di base e classificata sono anche vulnerabili. Dopo che un utente accede con l'autenticazione di base o Digest, il browser invia automaticamente le credenziali, fino al termine della sessione.

Nota: In questo contesto, *sessione* fa riferimento alla sessione sul lato client durante i quali l'utente viene autenticato. È non correlati alle sessioni sul lato server o [sessione middleware](xref:fundamentals/app-state).

Gli utenti possono impedire vulnerabilità CSRF da:
* Disconnettersi da siti web quando hanno finito di usarli.
* Cancellare periodicamente i cookie del browser.

Tuttavia, le vulnerabilità CSRF sono fondamentalmente un problema con l'app web, non l'utente finale.

## <a name="how-does-aspnet-core-mvc-address-csrf"></a>Modalità di ASP.NET MVC di base indirizzi CSRF?

> [!WARNING]
> ASP.NET Core implementa request anti-falsificazione usando il [dello stack di protezione dati di ASP.NET Core](xref:security/data-protection/introduction). La protezione dei dati di ASP.NET Core deve essere configurato per funzionare in una server farm. Vedere [la configurazione di protezione dei dati](xref:security/data-protection/configuration/overview) per ulteriori informazioni.

Configurazione di protezione predefinito request anti-falsificazione ASP.NET Core 

In ASP.NET Core MVC 2.0 il [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) inserisce i token antifalsificazione per elementi del form HTML. Ad esempio, il markup seguente in un file Razor genererà automaticamente i token antifalsificazione:

```html
<form method="post">
  <!-- form markup -->
</form>
```

La generazione automatica di token antifalsificazione per elementi del form HTML si verifica quando:

* Il `form` tag contiene il `method="post"` attributo AND

  * L'attributo action è vuoto. ( `action=""`) O
  * Non è specificato l'attributo dell'azione. (`<form method="post">`)

È possibile disabilitare la generazione automatica di token antifalsificazione per elementi del form HTML da:

* Disabilita in modo esplicito `asp-antiforgery`. Esempio:

  ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* Scegliere l'elemento form fuori gli helper di Tag utilizzando l'Helper di Tag [! simbolo rinuncia](xref:mvc/views/tag-helpers/intro#opt-out).

  ```html
  <!form method="post">
  </!form>
  ```

* Rimuovere il `FormTagHelper` dalla vista. È possibile rimuovere il `FormTagHelper` da una vista aggiungendo la seguente direttiva alla visualizzazione Razor:

  ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Pagine Razor](xref:mvc/razor-pages/index) vengono protetti automaticamente dalle XSRF/CSRF. Non è necessario scrivere codice aggiuntivo. Vedere [XSRF/CSRF e pagine Razor](xref:mvc/razor-pages/index#xsrf) per ulteriori informazioni.

L'approccio più comune di difesa contro gli attacchi CSRF è il modello di token del programma di sincronizzazione (STP). STP è una tecnica usata quando l'utente richiede una pagina di dati del form. Il server invia un token associato all'identità dell'utente corrente al client. Il client invia il token per il server per la verifica. Se il server riceve un token che non corrisponde all'identità dell'utente autenticato, la richiesta viene rifiutata. Il token è univoco e imprevedibili. Il token può essere utilizzato anche per garantire la corretta sequenza di una serie di richieste (pagina verifica 1 precede pagina 2 che precede pagina 3). Tutti i moduli nei modelli ASP.NET MVC Core generano token non riproducibili. I due esempi seguenti di logica di visualizzazione generano token non riproducibili:

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

È possibile aggiungere in modo esplicito un token non riproducibili a un `<form>` elemento senza utilizzare gli helper di tag con l'helper HTML `@Html.AntiForgeryToken`:


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

In ognuno dei casi precedenti, ASP.NET Core verranno aggiunti a un campo modulo nascosto simile al seguente:
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw">
```

ASP.NET Core include tre [filtri](xref:mvc/controllers/filters) per l'utilizzo di token non riproducibili: `ValidateAntiForgeryToken`, `AutoValidateAntiforgeryToken`, e `IgnoreAntiforgeryToken`.

### <a name="validateantiforgerytoken"></a>ValidateAntiForgeryToken

Il `ValidateAntiForgeryToken` è un filtro azione che può essere applicato a una singola azione, un controller, o a livello globale. Le richieste effettuate alle azioni che è sono applicato il filtro verranno bloccate a meno che la richiesta include un token valido non riproducibili.

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

Il `ValidateAntiForgeryToken` attributo richiede un token per le richieste a metodi di azione decora, tra cui `HTTP GET` richieste. Se si applica su vasta scala, è possibile eseguirne l'override con il `IgnoreAntiforgeryToken` attributo.

### <a name="autovalidateantiforgerytoken"></a>AutoValidateAntiforgeryToken

Le applicazioni ASP.NET Core in genere non generano token non riproducibili per i metodi HTTP sicuro (GET, HEAD, opzioni e traccia). Anziché applicare ampiamente la `ValidateAntiForgeryToken` attributo e quindi si esegue l'override con `IgnoreAntiforgeryToken` gli attributi, è possibile utilizzare il ``AutoValidateAntiforgeryToken`` attributo. Questo attributo funziona esattamente come il `ValidateAntiForgeryToken` attributo, ad eccezione del fatto che non richiede token per le richieste effettuate utilizzando i metodi HTTP seguenti:

* GET
* HEAD
* OPZIONI
* TRACE

Si consiglia di usare `AutoValidateAntiforgeryToken` su vasta scala per gli scenari non API. In questo modo che le azioni di POST sono protetti per impostazione predefinita. L'alternativa consiste nell'ignorare i token non riproducibili per impostazione predefinita, a meno che non `ValidateAntiForgeryToken` viene applicato al metodo di azione singola. È più probabile che in questo scenario per un metodo di azione di POST di sinistra non è protetta, lasciando vulnerabile agli attacchi CSRF l'app. POST anche anonimo deve inviare il token non riproducibili.

Nota: Le API non disporre di un meccanismo automatico per l'invio la parte non cookie del token. l'implementazione dipenderà probabilmente l'implementazione del codice client. Di seguito sono illustrati alcuni esempi.

Esempio (livello di classe):

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Esempio (globale):

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a>IgnoreAntiforgeryToken

Il `IgnoreAntiforgeryToken` filtro viene utilizzato per eliminare la necessità di un token di essere presenti per una determinata azione o controller, non riproducibili. Quando viene applicato, sostituirà questo filtro `ValidateAntiForgeryToken` e/o `AutoValidateAntiforgeryToken` i filtri specificati a un livello superiore (a livello globale o in un controller).

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

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX e SPA (Single Page Application)

Nelle applicazioni basate su HTML tradizionale, i token non riproducibili vengono passati al server utilizzando i campi del form nascosto. In App moderne basate su JavaScript e applicazioni a pagina singola (SPA), numero di richieste viene eseguite a livello di codice. Queste richieste AJAX possono utilizzare altre tecniche (ad esempio le intestazioni di richiesta o i cookie) per inviare il token. Se i cookie vengono usati per archiviare i token di autenticazione e per autenticare le richieste API nel server, CSRF sarà un potenziale problema. Tuttavia, se l'archiviazione locale viene utilizzato per memorizzare il token, CSRF vulnerabilità può essere contrastata, poiché i valori dall'archivio locale non vengono inviati automaticamente al server con ogni nuova richiesta. Pertanto, utilizzare l'archiviazione locale per archiviare il token non riproducibili sul client che invia il token come intestazione della richiesta è un approccio consigliato.

### <a name="angularjs"></a>AngularJS

AngularJS viene utilizzata una convenzione all'indirizzo CSRF. Se il server invia un cookie con il nome `XSRF-TOKEN`, l'angolare `$http` servizio aggiungerà il valore da questo cookie a un'intestazione quando invia una richiesta al server. Questo processo è automatico; non è necessario impostare in modo esplicito l'intestazione. Il nome di intestazione è `X-XSRF-TOKEN`. Il server deve rilevare questa intestazione e convalidare il contenuto.

Utilizzare questa convenzione per ASP.NET Core API:

* Configurare l'app per fornire un token di un cookie denominato `XSRF-TOKEN`
* Configurare il servizio non riproducibili per cercare un'intestazione denominata `X-XSRF-TOKEN`

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Visualizzare l'esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).

### <a name="javascript"></a>JavaScript

Utilizzo di JavaScript con visualizzazioni, è possibile creare il token utilizzando un servizio dall'interno della visualizzazione. A tale scopo, inserisce il `Microsoft.AspNetCore.Antiforgery.IAntiforgery` servizio nella vista e chiamata `GetAndStoreTokens`, come illustrato:

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,28)]

Questo approccio elimina la necessità di interagire direttamente con l'impostazione di cookie dal server o la relativa lettura dai client.

Nell'esempio precedente utilizza jQuery per leggere il valore del campo nascosto per l'intestazione AJAX POST. Per utilizzare JavaScript per ottenere il valore del token, utilizzare `document.getElementById('RequestVerificationToken').value`.

JavaScript è possibile anche i token forniti nei cookie di accesso e quindi utilizzare il contenuto del cookie per creare un'intestazione con il valore del token, come illustrato di seguito.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Quindi, presupponendo che si crea lo script richiede per inviare il token in un'intestazione denominata `X-CSRF-TOKEN`, configurare il servizio non riproducibili per cercare il `X-CSRF-TOKEN` intestazione:

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

L'esempio seguente usa jQuery per effettuare una richiesta AJAX con l'intestazione appropriata:

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a>Configurazione Antiforgery

`IAntiforgery` fornisce l'API per configurare il sistema non riproducibili. Può essere richiesta nel `Configure` metodo la `Startup` classe. L'esempio seguente usa middleware dalla home page dell'app per generare un token non riproducibili e inviarlo nella risposta sotto forma di cookie (impostazione predefinita angolare convenzioni di denominazione descritte in precedenza):


```csharp
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a>Opzioni

È possibile personalizzare [opzioni non riproducibili](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "mydomain.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|Opzione        | Descrizione |
|------------- | ----------- |
|CookieDomain  | Il dominio del cookie. Il valore predefinito è `null`. |
|CookieName    | Il nome del cookie. Se non è impostata, il sistema genererà un nome univoco che inizia con il `DefaultCookiePrefix` (". AspNetCore.Antiforgery."). |
|CookiePath    | Il percorso impostato nel cookie. |
|FormFieldName | Il nome del campo del form nascosto usato dal sistema non riproducibili per il rendering di un token non riproducibili nelle viste. |
|HeaderName    | Il nome dell'intestazione utilizzato dal sistema non riproducibili. Se `null`, il sistema prenderà in considerazione solo i dati del modulo. |
|RequireSsl    | Specifica se SSL è richiesto dal sistema non riproducibili. Il valore predefinito è `false`. Se `true`, le richieste non SSL avrà esito negativo. |
|SuppressXFrameOptionsHeader | Specifica se eliminare la generazione del `X-Frame-Options` intestazione. Per impostazione predefinita, viene generata l'intestazione con un valore di "SAMEORIGIN". Il valore predefinito è `false`. |

Https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions per altre informazioni, vedere.

### <a name="extending-antiforgery"></a>Estensione Antiforgery

Il [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) tipo consente agli sviluppatori di estendere il comportamento del sistema anti-XSRF, dati aggiuntivi di andata e ritorno in ogni token. Il [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) metodo viene chiamato ogni volta che viene generato un token di campo e il valore restituito è incorporato all'interno del token generato. Un responsabile dell'implementazione potrebbe restituire un timestamp, un parametro nonce o qualsiasi altro valore e quindi chiamare [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) per la convalida dei dati quando il token viene convalidato. Nome utente del client è già incorporato nei token generati, pertanto non è necessario includere queste informazioni. Se un token include dati supplementari, ma non `IAntiForgeryAdditionalDataProvider` è stato configurato, i dati supplementari non convalidati.

## <a name="fundamentals"></a>Concetti fondamentali

Attacchi CSRF si basano sul comportamento di invio di cookie associati a un dominio con ogni richiesta effettuata per quel dominio browser predefinito. Questi cookie vengono archiviati all'interno del browser. Spesso includono i cookie di sessione per gli utenti autenticati. L'autenticazione basata su cookie è una forma comune di autenticazione. Sistemi di autenticazione basata su token sono sempre più diffusi, soprattutto per SPA (Single Page Applications, applicazioni a pagina singola) e altri scenari "smart client".

### <a name="cookie-based-authentication"></a>Autenticazione basata su cookie

Una volta che un utente è autenticato tramite nome utente e password, si sta rilasciati un token che può essere utilizzato per identificare in modo sicuro e convalidare che sono stati autenticati. Il token viene memorizzato come rende un cookie associato a ogni richiesta client. Generazione e convalida il cookie viene eseguita dal middleware di autenticazione del cookie. ASP.NET Core fornisce il cookie [middleware](xref:fundamentals/middleware/index) che serializza un'entità utente in un cookie crittografato e nelle richieste successive convalida quindi il cookie, ricrea l'entità e assegna al `User` proprietà `HttpContext`.

Quando viene utilizzato un cookie, il cookie di autenticazione è semplicemente un contenitore per il ticket di autenticazione form. Il ticket viene passato come valore del cookie di autenticazione form, con ogni richiesta e viene utilizzato dall'autenticazione basata su form, nel server, per identificare un utente autenticato.

Quando un utente è connesso a un sistema, una sessione utente viene creata sul lato server e viene archiviata in un database o un altro archivio persistente. Il sistema genera una chiave di sessione che punta alla sessione effettiva nell'archivio dati e viene inviato come cookie sul lato client. Il server web verifica la chiave di sessione ogni volta che un utente richiede una risorsa che richiede l'autorizzazione. Il sistema verifica se la sessione utente associata dispone del privilegio per accedere alla risorsa richiesta. In questo caso, la richiesta continua. La richiesta in caso contrario, restituisce come non autorizzati. In questo approccio, i cookie vengono usati per rendere l'applicazione di stato, perché è in grado di ricordare "" che l'utente è autenticato in precedenza con il server.

### <a name="user-tokens"></a>Token dell'utente

L'autenticazione basata su token non archivia sessione sul server. Quando un utente è connesso, cui è assegnati un token (non un token non riproducibili). Il token contiene i dati necessari per convalidare il token. Sono inoltre contenute informazioni utente sotto forma di [attestazioni](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model). Quando un utente tenta di accedere a una risorsa del server che richiede l'autenticazione, il token viene inviato al server con un'intestazione di autorizzazione aggiuntive sotto forma di connessione {token}. In questo modo, l'applicazione senza stato poiché in ogni richiesta successiva il token viene passato nella richiesta per la convalida sul lato server. Questo token non *crittografati*; si tratta piuttosto di *codificato*. Sul lato server, il token può essere decodificato per accedere alle informazioni non elaborate all'interno del token. Per inviare il token nelle richieste successive, di memorizzarlo nella memoria locale del browser o in un cookie. Non preoccuparti delle vulnerabilità XSRF se il token viene archiviato nell'archiviazione locale, ma si tratta di un problema se il token viene archiviato in un cookie.

### <a name="multiple-applications-are-hosted-in-one-domain"></a>Più applicazioni sono ospitate in un dominio

Sebbene `example1.cloudapp.net` e `example2.cloudapp.net` sono host diversi, c'è una relazione di trust implicita tra gli host sotto il `*.cloudapp.net` dominio. Questa relazione di trust implicita consente agli host potenzialmente non attendibili determinare i cookie di altro (lo stessa origine i criteri che regolano le richieste AJAX non applicano necessariamente ai cookie HTTP). Il runtime di ASP.NET Core fornisce alcuni attenuazione in quanto il nome utente è incorporato nel token di campo. Anche se un sottodominio dannoso è in grado di sovrascrivere un token di sessione, non può generare un token di campo valido per l'utente. Quando sono ospitati in un simile ambiente, le routine di anti-XSRF incorporato ancora non è possibile difendersi Hijack della sessione o l'account di accesso CSRF attacchi. Ambienti di hosting condivisi sono vunerable Hijack della sessione, account di accesso CSRF e altri attacchi.

### <a name="additional-resources"></a>Risorse aggiuntive

* [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) su [aprire il progetto di sicurezza dell'applicazione Web](https://www.owasp.org/index.php/Main_Page) (OWASP).
