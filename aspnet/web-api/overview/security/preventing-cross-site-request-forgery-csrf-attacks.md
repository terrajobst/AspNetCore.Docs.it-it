---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Prevenzione degli attacchi di richiesta intersito falsa (CSRF) nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Viene descritto come implementare misure anti-CSRF in ASP.NET Web API e l'attacco di richiesta intersito falsa (CSRF).
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: cd7d978190d28a028285746781a380d9bb5f91d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833429"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>Prevenzione degli attacchi di richiesta intersito falsa (CSRF) nell'API Web ASP.NET
====================
da [Mike Wasson](https://github.com/MikeWasson)

Cross-Site richiesta intersito falsa (CSRF) è un attacco in cui un sito dannoso invia una richiesta a un sito vulnerabile in cui l'utente è connesso

Di seguito è riportato un esempio di un attacco CSRF:

1. Un utente accede a `www.example.com` tramite autenticazione basata su form.
2. Il server autentica l'utente. La risposta dal server include un cookie di autenticazione.
3. Senza la disconnessione, l'utente visita un sito web dannoso. Questo sito dannoso contiene il form HTML seguente: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Si noti che l'azione del form è inviata al sito vulnerabile, non al sito dannoso. Questa è la parte "intersito" di CSRF.
4. L'utente fa clic sul pulsante Invia. Il visualizzatore include il cookie di autenticazione con la richiesta.
5. La richiesta viene eseguita nel server con il contesto di autenticazione dell'utente e può eseguire qualsiasi operazione che un utente autenticato può eseguire.

Anche se in questo esempio richiede all'utente di fare clic sul pulsante del form, la pagina dannosa potrebbe eseguite facilmente uno script che invia il form automaticamente. Inoltre, con SSL non impedire un attacco CSRF, poiché il sito dannoso può inviare una richiesta di "https://".

In genere, gli attacchi CSRF sono possibili contro i siti web che usano i cookie per l'autenticazione, perché i browser inviano tutti i cookie pertinenti al sito web di destinazione. Attacchi CSRF, tuttavia, non sono limitati a sfruttare i cookie. Ad esempio, l'autenticazione di base e classificata sono anche vulnerabili. Dopo un utente accede con l'autenticazione di base o Digest. il browser invia automaticamente le credenziali fino a quando non termina la sessione.

## <a name="anti-forgery-tokens"></a>Token antifalsificazione

Per prevenire attacchi CSRF, ASP.NET MVC Usa i token antifalsificazione, chiamati anche *richiedere i token di verifica*.

1. Il client richiede una pagina HTML che contiene un modulo.
2. Il server include due token nella risposta. Un token viene inviato come cookie. L'altra viene inserita nel campo del form nascosto. I token vengono generati in modo casuale, in modo che un utente malintenzionato non può dedurre i valori.
3. Quando il client invia il form, è necessario inviare entrambi i token al server. Il client invia il token del cookie come cookie e invia il token del form all'interno di dati del form. (Un client browser esegue automaticamente questa quando l'utente invia il form.)
4. Se una richiesta non include entrambi i token, il server non consente la richiesta.

Ecco un esempio di un form HTML con un token del form nascosto:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

I token antifalsificazione funzionano perché la pagina non è in grado di leggere i token dell'utente, a causa dei criteri di corrispondenza dell'origine. ([i criteri di corrispondenza dell'origine](http://www.w3.org/Security/wiki/Same_Origin_Policy) impedire che i documenti ospitati in due siti diversi l'accesso al contenuto di altro. Nell'esempio precedente, la pagina dannosa può inviare richieste all'example.com, ma che non è possibile leggere la risposta.)

Per impedire attacchi CSRF, usare i token antifalsificazione con qualsiasi protocollo di autenticazione in cui il browser invia automaticamente le credenziali dopo l'accesso dell'utente. Questo include i protocolli di autenticazione basata su cookie, ad esempio autenticazione basata su form, nonché i protocolli, ad esempio autenticazione di base e Digest.

È consigliabile richiedere i token antifalsificazione per alcun metodo nonsafe (POST, PUT, DELETE). Inoltre, assicurarsi che non sicuri metodi (GET, HEAD) dispone tutti gli effetti collaterali. Inoltre, se si abilita il supporto di domini, ad esempio, JSONP o CORS sicuri anche metodi come GET sono potenzialmente vulnerabili agli attacchi CSRF, consentendo all'utente malintenzionato di leggere i dati potenzialmente sensibili.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Token antifalsificazione in ASP.NET MVC

Per aggiungere i token antifalsificazione in una pagina Razor, usare il **HtmlHelper.AntiForgeryToken** metodo helper:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Questo metodo aggiunge il campo del form nascosto e imposta anche il token del cookie.

## <a name="anti-csrf-and-ajax"></a>AJAX e Intersito

Il token del form può rappresentare un problema per le richieste AJAX, perché una richiesta AJAX potrebbe inviare dati JSON, non i dati di form HTML. Una soluzione consiste nell'inviare i token in un'intestazione HTTP personalizzata. Il codice seguente usa la sintassi Razor per generare i token e quindi li aggiunge a una richiesta AJAX. I token vengono generati nel server chiamando **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Quando si elabora la richiesta, estrarre i token dall'intestazione della richiesta. Chiamare quindi il **Antiforgery** metodo per convalidare i token. Il **Validate** metodo genera un'eccezione se il token non sono validi.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
