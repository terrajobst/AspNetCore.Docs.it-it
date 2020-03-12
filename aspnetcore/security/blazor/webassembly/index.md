---
title: Sicurezza ASP.NET Core Blazor webassembly
author: guardrex
description: Informazioni su come proteggere Blazor app WebAssemlby come applicazioni a pagina singola (Spa).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/index
ms.openlocfilehash: 48136d0717998df3311dd5177688e062d0009176
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083839"
---
# <a name="secure-aspnet-core-opno-locblazor-webassembly"></a>Sicurezza ASP.NET Core Blazor webassembly

Di [Javier Calvarro Nelson](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Blazor le app webassembly sono protette in modo analogo alle applicazioni a pagina singola (Spa). Esistono diversi approcci per l'autenticazione degli utenti in Spa, ma l'approccio più comune e completo consiste nell'usare un'implementazione basata sul [protocollo oAuth 2,0](https://oauth.net/), ad esempio [Open ID Connect (OIDC)](https://openid.net/connect/).

## <a name="authentication-library"></a>Libreria di autenticazione

Blazor webassembly supporta l'autenticazione e l'autorizzazione delle app usando OIDC tramite la libreria `Microsoft.AspNetCore.Components.WebAssembly.Authentication`. La libreria fornisce un set di primitive per l'autenticazione uniforme rispetto a ASP.NET Core backend. La libreria integra ASP.NET Core identità con il supporto dell'autorizzazione API basato su [Identity Server](https://identityserver.io/). La libreria può eseguire l'autenticazione con qualsiasi provider di identità di terze parti (IP) che supporta OIDC, detti provider OpenID (OP).

Il supporto per l'autenticazione in Blazor webassembly è basato sulla libreria *oidc-client. js* , che consente di gestire i dettagli del protocollo di autenticazione sottostante.

Sono disponibili altre opzioni per l'autenticazione di Spa, ad esempio l'uso di cookie navigava sullostesso sito. Tuttavia, la progettazione di Blazor webassembly viene stabilita in oAuth e OIDC come opzione migliore per l'autenticazione nelle app Blazor webassembly. È stata scelta [l'autenticazione](xref:security/anti-request-forgery#token-based-authentication) basata su token basata su [token Web JSON (token JWT)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) tramite [l'autenticazione basata su cookie](xref:security/anti-request-forgery#cookie-based-authentication) per motivi funzionali e di sicurezza:

* L'uso di un protocollo basato su token offre una superficie di attacco più piccola, poiché i token non vengono inviati in tutte le richieste.
* Gli endpoint server non richiedono la protezione da [richieste intersito falsificazione (CSRF)](xref:security/anti-request-forgery) perché i token vengono inviati in modo esplicito. Ciò consente di ospitare Blazor app webassembly insieme alle app MVC o Razor Pages.
* I token hanno autorizzazioni più strette rispetto ai cookie. Ad esempio, non è possibile usare i token per gestire l'account utente o modificare la password di un utente, a meno che tale funzionalità non venga implementata in modo esplicito.
* I token hanno una durata breve, un'ora per impostazione predefinita, che limita la finestra di attacco. I token possono anche essere revocati in qualsiasi momento.
* Il token JWT autonomo offre garanzie al client e al server per il processo di autenticazione. Un client, ad esempio, è in grado di rilevare e verificare che i token ricevuti siano legittimi e che siano stati emessi come parte di un determinato processo di autenticazione. Se una terza parte tenta di cambiare un token durante il processo di autenticazione, il client può rilevare il token cambiato ed evitare di usarlo.
* I token con oAuth e OIDC non si basano sulla corretta presenza dell'agente utente per assicurarsi che l'applicazione sia protetta.
* I protocolli basati su token, ad esempio oAuth e OIDC, consentono di autenticare e autorizzare app ospitate e autonome con lo stesso set di caratteristiche di sicurezza.

## <a name="authentication-process-with-oidc"></a>Processo di autenticazione con OIDC

La libreria `Microsoft.AspNetCore.Components.WebAssembly.Authentication` offre diverse primitive per implementare l'autenticazione e l'autorizzazione usando OIDC. In termini generali, l'autenticazione funziona nel modo seguente:

* Quando un utente anonimo seleziona il pulsante di accesso o richiede una pagina con l'attributo [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) applicato, l'utente viene reindirizzato alla pagina di accesso dell'app (`/authentication/login`).
* Nella pagina di accesso, la libreria di autenticazione prepara un reindirizzamento all'endpoint di autorizzazione. L'endpoint di autorizzazione è esterno all'app webassembly Blazor e può essere ospitato in un'origine separata. L'endpoint è responsabile per determinare se l'utente è autenticato e per emettere uno o più token in risposta. La libreria di autenticazione fornisce un callback di accesso per ricevere la risposta di autenticazione.
  * Se l'utente non è autenticato, l'utente viene reindirizzato al sistema di autenticazione sottostante, che in genere è ASP.NET Core identità.
  * Se l'utente è già stato autenticato, l'endpoint di autorizzazione genera i token appropriati e reindirizza di nuovo il browser all'endpoint di callback dell'account di accesso (`/authentication/login-callback`).
* Quando l'app webassembly Blazor carica l'endpoint di callback di accesso (`/authentication/login-callback`), viene elaborata la risposta di autenticazione.
  * Se il processo di autenticazione viene completato correttamente, l'utente viene autenticato e, facoltativamente, restituito all'URL protetto originale richiesto dall'utente.
  * Se il processo di autenticazione ha esito negativo per qualsiasi motivo, l'utente viene inviato alla pagina di accesso non riuscita (`/authentication/login-failed`) e viene visualizzato un errore.
