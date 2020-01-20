---
title: Usare i cookie navigava sullostesso sito in ASP.NET Core
author: rick-anderson
description: Informazioni su come usare per navigava sullostesso sito i cookie in ASP.NET Core
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
uid: security/samesite
ms.openlocfilehash: b344ed8f539979210980b3421659207edd513f32
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146433"
---
# <a name="work-with-samesite-cookies-in-aspnet-core"></a>Usare i cookie navigava sullostesso sito in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

[Navigava sullostesso sito](https://tools.ietf.org/html/draft-west-first-party-cookies-07) è una bozza [IETF](https://ietf.org/about/) progettata per garantire una protezione contro gli attacchi di richiesta intersito falsificazione (CSRF). [Bozza di navigava sullostesso sito 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):

* Considera i cookie come `SameSite=Lax` per impostazione predefinita.
* Indica che i cookie che asseriscono in modo esplicito `SameSite=None` per abilitare il recapito tra siti devono essere contrassegnati come `Secure`.

`Lax` funziona per la maggior parte dei cookie dell'app. Alcune forme di autenticazione come [OpenID Connect](https://openid.net/connect/) (OIDC) e l'impostazione predefinita di [WS-Federation](https://auth0.com/docs/protocols/ws-fed) sono i reindirizzamenti basati su post. I reindirizzamenti basati su POST attivano le protezioni del browser navigava sullostesso sito, pertanto navigava sullostesso sito è disabilitato per questi componenti. La maggior parte degli accessi [OAuth](https://oauth.net/) non è interessata a causa di differenze nel modo in cui la richiesta fluisce.

Il parametro `None` causa problemi di compatibilità con i client che hanno implementato lo standard Draft precedente 2016 (ad esempio, iOS 12). Vedere [supporto di browser meno recenti](#sob) in questo documento.

Ogni componente ASP.NET Core che emette cookie deve decidere se navigava sullostesso sito è appropriato.

## <a name="api-usage-with-samesite"></a>Utilizzo delle API con navigava sullostesso sito

Il valore predefinito di [HttpContext. Response. cookies. Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*) è `Unspecified`, ovvero nessun attributo navigava sullostesso sito aggiunto al cookie e il client utilizzerà il comportamento predefinito (LAX per i nuovi browser, None per quelli precedenti). Il codice seguente illustra come modificare il valore di navigava sullostesso sito del cookie in `SameSiteMode.Lax`:

[!code-csharp[](samesite/sample/Pages/Index.cshtml.cs?name=snippet)]

Tutti i componenti ASP.NET Core che emettono cookie sostituiscono i valori predefiniti precedenti con le impostazioni appropriate per gli scenari. L'override dei valori predefiniti precedenti non è stato modificato.

| Componente | cookie | Default |
| ------------- | ------------- |
| <xref:Microsoft.AspNetCore.Http.CookieBuilder> | <xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite> | `Unspecified` |
| <xref:Microsoft.AspNetCore.Http.HttpContext.Session>  | [SessionOptions. cookie](xref:Microsoft.AspNetCore.Builder.SessionOptions.Cookie) |`Lax` |
| <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.CookieTempDataProvider>  | [CookieTempDataProviderOptions. cookie](xref:Microsoft.AspNetCore.Mvc.CookieTempDataProviderOptions.Cookie) | `Lax` |
| <xref:Microsoft.AspNetCore.Antiforgery.IAntiforgery> | [AntiforgeryOptions. cookie](xref:Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions.Cookie)| `Strict` |
| [Autenticazione cookie](xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*) | [CookieAuthenticationOptions. cookie](xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.CookieName) | `Lax` |
| <xref:Microsoft.Extensions.DependencyInjection.TwitterExtensions.AddTwitter*> | [TwitterOptions.StateCookie](xref:Microsoft.AspNetCore.Authentication.Twitter.TwitterOptions.StateCookie) | `Lax`  |
| <xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationHandler`1> | [RemoteAuthenticationOptions. CorrelationCookie](xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.CorrelationCookie)  | `None` |
| <xref:Microsoft.Extensions.DependencyInjection.OpenIdConnectExtensions.AddOpenIdConnect*> | [OpenIdConnectOptions.NonceCookie](xref:Microsoft.AspNetCore.Authentication.OpenIdConnect.OpenIdConnectOptions.NonceCookie)| `None` |
| [HttpContext. Response. cookies. Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*) | <xref:Microsoft.AspNetCore.Http.CookieOptions> | `Unspecified` |

::: moniker range=">= aspnetcore-3.1"

ASP.NET Core 3,1 e versioni successive fornisce il supporto navigava sullostesso sito seguente:

* Ridefinisce il comportamento di `SameSiteMode.None` per emettere `SameSite=None`
* Aggiunge un nuovo valore `SameSiteMode.Unspecified` per omettere l'attributo navigava sullostesso sito.
* Per impostazione predefinita, tutte le API cookie `Unspecified`. Alcuni componenti che usano cookie impostano valori più specifici per gli scenari. Vedere la tabella precedente per gli esempi.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

In ASP.NET Core 3,0 e versioni successive i valori predefiniti di navigava sullostesso sito sono stati modificati per evitare conflitti con impostazioni predefinite client incoerenti. Le API seguenti hanno modificato l'impostazione predefinita da `SameSiteMode.Lax ` a `-1` per evitare di emettere un attributo navigava sullostesso sito per questi cookie:

* <xref:Microsoft.AspNetCore.Http.CookieOptions> usato con [HttpContext. Response. cookies. Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*)
* <xref:Microsoft.AspNetCore.Http.CookieBuilder> usato come factory per `CookieOptions`
* [CookiePolicyOptions.MinimumSameSitePolicy](xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy)

::: moniker-end

## <a name="history-and-changes"></a>Cronologia e modifiche

Il supporto di navigava sullostesso sito è stato implementato per la prima volta nel ASP.NET Core 2,0 usando lo [standard 2016 Draft](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1). Lo standard 2016 è stato acconsentito esplicitamente. Per impostazione predefinita, ASP.NET Core si è scelto di impostare diversi cookie da `Lax`. Dopo avere riscontrato diversi [problemi](https://github.com/aspnet/Announcements/issues/318) di autenticazione, la maggior parte dell'utilizzo di navigava sullostesso sito è stata [disabilitata](https://github.com/aspnet/Announcements/issues/348).

Le [patch](https://devblogs.microsoft.com/dotnet/net-core-November-2019/) sono state rilasciate nel 2019 novembre per l'aggiornamento dallo standard 2016 allo standard 2019. [Bozza 2019 della specifica navigava sullostesso sito](https://github.com/aspnet/Announcements/issues/390):

* **Non** è compatibile con le versioni precedenti della bozza 2016. Per ulteriori informazioni, vedere [supporto dei browser meno recenti](#sob) in questo documento.
* Specifica che i cookie vengono considerati come `SameSite=Lax` per impostazione predefinita.
* Specifica i cookie che asseriscono in modo esplicito `SameSite=None` per consentire il recapito tra siti deve essere contrassegnato come `Secure`. `None` è una nuova voce da rifiutare esplicitamente.
* È supportato dalle patch rilasciate per ASP.NET Core 2,1, 2,2 e 3,0. ASP.NET Core 3,1 dispone di supporto navigava sullostesso sito aggiuntivo.
* Viene pianificata per essere abilitata per impostazione predefinita da [Chrome](https://chromestatus.com/feature/5088147346030592) in [feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Il passaggio a questo standard nei browser è stato avviato in 2019.

## <a name="apis-impacted-by-the-change-from-the-2016-samesite-draft-standard-to-the-2019-draft-standard"></a>API interessate dal passaggio dallo standard Draft 2016 navigava sullostesso sito allo standard 2019 Draft

* [Http. SameSiteMode](xref:Microsoft.AspNetCore.Http.SameSiteMode)
* [CookieOptions. navigava sullostesso sito](xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite)
* [CookieBuilder. navigava sullostesso sito](xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite)
* [CookiePolicyOptions.MinimumSameSitePolicy](xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy)
* <xref:Microsoft.Net.Http.Headers.SameSiteMode?displayProperty=fullName>
* <xref:Microsoft.Net.Http.Headers.SetCookieHeaderValue.SameSite?displayProperty=fullName>

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Supporto di browser meno recenti

Lo standard 2016 navigava sullostesso sito ha richiesto che i valori sconosciuti debbano essere considerati come valori `SameSite=Strict`. Le app a cui è stato eseguito l'accesso da browser meno recenti che supportano lo standard navigava sullostesso sito 2016 possono interrompersi quando ottengono una proprietà navigava sullostesso sito con un valore di `None`. Le app Web devono implementare il rilevamento del browser se intendono supportare browser meno recenti. ASP.NET Core non implementa il rilevamento del browser perché i valori degli agenti utente sono altamente volatili e cambiano di frequente. Un punto di estensione in <xref:Microsoft.AspNetCore.CookiePolicy> consente di collegare la logica specifica dell'agente utente.

In `Startup.Configure`aggiungere il codice che chiama <xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*> prima di chiamare <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> o qualsiasi metodo che scrive *i* cookie:

[!code-csharp[](samesite/sample/Startup.cs?name=snippet5&highlight=18-19)]

In `Startup.ConfigureServices`aggiungere codice simile al seguente:

::: moniker range="= aspnetcore-3.1"

[!code-csharp[](samesite/sample/Startup31.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.1"

[!code-csharp[](samesite/sample/Startup.cs?name=snippet)]

::: moniker-end

Nell'esempio precedente, `MyUserAgentDetectionLib.DisallowsSameSiteNone` è una libreria fornita dall'utente che rileva se l'agente utente non supporta navigava sullostesso sito `None`:

[!code-csharp[](samesite/sample/Startup31.cs?name=snippet2)]

Il codice seguente illustra un metodo di `DisallowsSameSiteNone` di esempio:

> [!WARNING]
> Il codice seguente è solo a scopo dimostrativo:
> * Non deve essere considerato completo.
> * Non è gestita né supportata.

[!code-csharp[](samesite/sample/Startup31.cs?name=snippetX)]

## <a name="test-apps-for-samesite-problems"></a>Testare le app per i problemi di navigava sullostesso sito

Le app che interagiscono con i siti remoti, ad esempio tramite l'accesso di terze parti, devono:

* Testare l'interazione su più browser.
* Applicare il [rilevamento e la mitigazione del browser CookiePolicy](#sob) descritti in questo documento.

Testare le app Web usando una versione client che può acconsentire esplicitamente al nuovo comportamento del navigava sullostesso sito. Chrome, Firefox e Chromium Edge hanno tutti nuovi flag di funzionalità di consenso esplicito che possono essere usati per i test. Quando l'app applica le patch di navigava sullostesso sito, testarla con versioni client precedenti, in particolare Safari. Per ulteriori informazioni, vedere [supporto dei browser meno recenti](#sob) in questo documento.

### <a name="test-with-chrome"></a>Eseguire test con Chrome

Chrome 78 + fornisce risultati fuorvianti perché prevede una mitigazione temporanea. La mitigazione temporanea di Chrome 78 + consente ai cookie meno di due minuti di età. Chrome 76 o 77 con i flag di test appropriati abilitati fornisce risultati più accurati. Per testare il nuovo comportamento di **navigava sullostesso sito, attivare**o disabilitare `chrome://flags/#same-site-by-default-cookies`. Le versioni precedenti di Chrome (75 e versioni precedenti) vengono segnalate per avere esito negativo con la nuova impostazione `None`. Vedere [supporto di browser meno recenti](#sob) in questo documento.

Google non rende disponibili versioni precedenti di Chrome. Seguire le istruzioni riportate in [scaricare Chromium](https://www.chromium.org/getting-involved/download-chromium) per testare le versioni precedenti di Chrome. Non **scaricare Chrome** dai collegamenti forniti cercando le versioni precedenti di Chrome.

* [Cromo 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Cromo 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Eseguire test con Safari

Safari 12 ha implementato rigorosamente la bozza precedente e ha esito negativo quando il nuovo valore `None` si trova in un cookie. `None` viene evitata tramite il codice di rilevamento del browser che [supporta i browser meno recenti](#sob) in questo documento. Testare gli accessi basati su stile sistema operativo Safari 12, Safari 13 e WebKit usando MSAL, ADAL o qualsiasi libreria in uso. Il problema dipende dalla versione del sistema operativo sottostante. I problemi di compatibilità con il nuovo comportamento navigava sullostesso sito sono noti per OSX Mojave (10,14) e iOS 12. L'aggiornamento del sistema operativo a OSX Catalina (10,15) o iOS 13 corregge il problema. Safari attualmente non dispone di un flag di consenso esplicito per il test del nuovo comportamento delle specifiche.

### <a name="test-with-firefox"></a>Eseguire test con Firefox

Il supporto di Firefox per il nuovo standard può essere testato nella versione 68 + scegliendo nella pagina `about:config` con il flag funzionalità `network.cookie.sameSite.laxByDefault`. Non sono stati segnalati problemi di compatibilità con le versioni precedenti di Firefox.

### <a name="test-with-edge-browser"></a>Eseguire test con il browser Microsoft Edge

Microsoft Edge supporta lo standard navigava sullostesso sito precedente. La versione perimetrale 44 non presenta problemi di compatibilità noti con il nuovo standard.

### <a name="test-with-edge-chromium"></a>Test con Microsoft Edge (cromo)

I flag navigava sullostesso sito sono impostati nella pagina `edge://flags/#same-site-by-default-cookies`. Nessun problema di compatibilità rilevato con cromo perimetrale.

### <a name="test-with-electron"></a>Test con elettrone

Le versioni di Electron includono versioni precedenti di cromo. Ad esempio, la versione di Electron usata dai team è Chromium 66, che presenta il comportamento precedente. È necessario eseguire test di compatibilità personalizzati con la versione di Electron utilizzata dal prodotto. Vedere [supporto dei browser precedenti](#sob) nella sezione seguente.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Blog di Chromium: sviluppatori: prepararsi per la nuova navigava sullostesso sito = None; Impostazioni sicure del cookie](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Spiegazione dei cookie navigava sullostesso sito](https://web.dev/samesite-cookies-explained/)
* [Patch di novembre 2019](https://devblogs.microsoft.com/dotnet/net-core-November-2019/)
