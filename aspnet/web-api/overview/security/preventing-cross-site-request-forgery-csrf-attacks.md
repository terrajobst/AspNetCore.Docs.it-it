---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Prevenzione degli attacchi Forgery (CSRF) richiesta tra siti in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: Viene descritto l'attacco forgery (CSRF) richiesta tra siti e come implementare le misure di anti-CSRF nell'API Web ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 5e7b24c697e0bb37f388341abd89609c76f6b64c
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961237"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>Prevenzione degli attacchi Forgery (CSRF) richiesta tra siti in ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

Cross-Site richiesta Forgery (CSRF) è un attacco in cui un sito dannoso invia una richiesta a un sito vulnerabile in cui l'utente è connesso

Di seguito è riportato un esempio di un attacco di tipo CSRF:

1. Un utente accede a `www.example.com` tramite autenticazione basata su form.
2. Il server autentica l'utente. La risposta dal server include un cookie di autenticazione.
3. Senza la disconnessione, l'utente visita un sito web dannoso. Questo sito contiene il formato HTML seguente: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Si noti che l'azione del form è inviata al sito vulnerabile, non al sito dannoso. Questa è la parte "cross-site" di CSRF.
4. L'utente fa clic sul pulsante Invia. Il visualizzatore include il cookie di autenticazione con la richiesta.
5. La richiesta viene eseguita nel server con il contesto di autenticazione dell'utente e può eseguire qualsiasi operazione che è consentito un utente autenticato.

Sebbene in questo esempio richiede all'utente di scegliere il pulsante modulo, la pagina è stato possibile analogamente a come eseguire facilmente uno script che invia automaticamente il form. Inoltre, tramite SSL non impedire un attacco di tipo CSRF poiché il sito dannoso può inviare una richiesta "https://".

In genere, gli attacchi CSRF sono possibili contro i siti web che usano i cookie per l'autenticazione, in quanto i browser inviano tutti i cookie pertinenti al sito web di destinazione. Tuttavia, gli attacchi CSRF non sono limitati a sfruttando i cookie. Ad esempio, l'autenticazione di base e classificata sono anche vulnerabili. Dopo un utente effettua l'accesso con autenticazione di base o Digest. il browser invia automaticamente le credenziali fino al termine della sessione.

## <a name="anti-forgery-tokens"></a>Token antifalsificazione

Per aiutare a prevenire gli attacchi CSRF, MVC ASP.NET utilizza i token antifalsificazione, collettivamente indicati come *richiedere token di verifica*.

1. Il client richiede una pagina HTML contenente un modulo.
2. Il server include due token nella risposta. Un token viene inviato come cookie. L'altra viene inserita in un campo nascosto del modulo. I token vengono generati in modo casuale in modo da un avversario non può dedurre i valori.
3. Quando il client invia il form, è necessario inviare entrambi i token al server. Il client invia il token del cookie come cookie e invia il token del form all'interno di dati del form. (Un client browser automaticamente avviene quando l'utente invia il form.)
4. Se una richiesta non include entrambi i token, il server non consente la richiesta.

Di seguito è riportato un esempio di un form HTML con un token del form nascosto:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

I token antifalsificazione funzionano perché la pagina non è possibile leggere i token dell'utente, a causa di criteri di origine stesso. ([i criteri di origine stesso](http://www.w3.org/Security/wiki/Same_Origin_Policy) impedire che i documenti ospitati in due diversi siti di accedere a contenuto di un'altra. Pertanto, nell'esempio precedente, la pagina può inviare richieste all'example.com, ma non è possibile leggere la risposta.)

Per impedire attacchi CSRF, utilizzare i token antifalsificazione con qualsiasi protocollo di autenticazione in cui il browser invia automaticamente le credenziali dopo che l'utente accede. Questo include i protocolli di autenticazione basato su cookie, ad esempio autenticazione basata su form, nonché i protocolli, ad esempio autenticazione di base e Digest.

È necessario richiedere i token antifalsificazione per metodi nonsafe (POST, PUT, DELETE). Inoltre, assicurarsi che provvisoria metodi (GET, HEAD) non dispone di tutti gli effetti collaterali. Inoltre, se si abilita il supporto di domini, ad esempio, JSONP o CORS sicuri anche metodi, ad esempio GET sono potenzialmente vulnerabili ad attacchi CSRF, consentendo all'utente malintenzionato di leggere i dati potrebbero contenere informazioni riservati.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Token antifalsificazione in ASP.NET MVC

Per aggiungere i token antifalsificazione a una pagina Razor, usare il **HtmlHelper.AntiForgeryToken** metodo di supporto:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Questo metodo aggiunge il campo nascosto del modulo e imposta inoltre il token del cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF e AJAX

Il token del form può essere un problema per le richieste AJAX, perché una richiesta AJAX potrebbe inviare i dati JSON, non i dati di form HTML. Una soluzione consiste nell'inviare i token in un'intestazione HTTP personalizzata. Il codice seguente usa la sintassi Razor per generare i token e quindi aggiunge i token a una richiesta AJAX. I token vengono generati nel server chiamando **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Quando si elabora la richiesta, estrarre il token dall'intestazione di richiesta. Chiamare quindi il **AntiForgery.Validate** metodo di convalida dei token. Il **Validate** metodo genera un'eccezione se il token non sono validi.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
