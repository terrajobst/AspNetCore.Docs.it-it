---
uid: web-api/overview/security/authentication-filters
title: Filtri di autenticazione in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: "Un filtro di autenticazione è un componente che consente di autenticare una richiesta HTTP. API Web 2 e MVC 5 supportano entrambi i filtri di autenticazione, ma differiscono leggermente..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 16e451f52799625983368bc938091eff47019b52
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>Filtri di autenticazione in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Un filtro di autenticazione è un componente che consente di autenticare una richiesta HTTP. API Web 2 e MVC 5 supportano entrambi i filtri di autenticazione, ma differiscono leggermente, principalmente in convenzioni di denominazione per l'interfaccia di filtro. Questo argomento descrive i filtri di autenticazione API Web.


I filtri di autenticazione consentono di impostare uno schema di autenticazione per i singoli controller o azioni. In questo modo, l'app può supportare i meccanismi di autenticazione diversi per diverse risorse HTTP.

In questo articolo verrà illustrato codice il [l'autenticazione di base](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) sample in [http://aspnet.codeplex.com](http://aspnet.codeplex.com). Nell'esempio viene illustrato un filtro di autenticazione che implementa lo schema di autenticazione di accesso di base HTTP (RFC 2617). Il filtro viene implementato in una classe denominata `IdentityBasicAuthenticationAttribute`. Non tutto il codice dell'esempio mostra solo le parti che illustrano come scrivere un filtro di autenticazione.

## <a name="setting-an-authentication-filter"></a>L'impostazione di un filtro di autenticazione

Come altri filtri, i filtri di autenticazione possono essere applicato per ogni controller, per ogni azione o globalmente a tutti i controller API Web.

Per applicare un filtro di autenticazione a un controller, contrassegnare la classe controller con l'attributo di filtro. Il codice seguente imposta il `[IdentityBasicAuthentication]` filtro su una classe controller, che consente l'autenticazione di base per tutte le azioni del controller.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Per applicare il filtro a un'azione, decorare l'azione con il filtro. Il codice seguente imposta il `[IdentityBasicAuthentication]` filtrare il controller `Post` metodo.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Per applicare il filtro a tutti i controller API Web, aggiungerlo alla **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementazione di un filtro di autenticazione Web API

Nell'API Web, implementano filtri di autenticazione il [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interfaccia. Deve inoltre ereditare **Attribute**, in modo da applicare come attributi.

Il **iauthenticationfilter e** interfaccia dispone di due metodi:

- **AuthenticateAsync** autentica la richiesta per la convalida delle credenziali nella richiesta, se presente.
- **ChallengeAsync** aggiunge una richiesta di autenticazione alla risposta HTTP, se necessario.

Questi metodi corrispondono al flusso di autenticazione definito in [2612 RFC](http://tools.ietf.org/html/rfc2616) e [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. Il client invia le credenziali nell'intestazione di autorizzazione. Ciò accade solitamente dopo che il client riceve una risposta 401 (non autorizzato) dal server. Tuttavia, un client può inviare le credenziali con qualsiasi richiesta, non solo dopo il recupero di 401.
2. Se il server non accetta le credenziali, viene restituito una risposta 401 (non autorizzato). La risposta include un'intestazione Www-Authenticate che contiene uno o più problemi. Ogni richiesta di verifica specifica uno schema di autenticazione riconosciuto dal server.

Il server può inoltre restituire 401 da una richiesta anonima. In realtà, che è in genere la modalità in cui viene avviato il processo di autenticazione:

1. Il client invia una richiesta anonima.
2. Il server restituisce 401.
3. Il client invia nuovamente la richiesta con le credenziali.

Questo flusso include sia *autenticazione* e *autorizzazione* passaggi.

- L'autenticazione di verifica l'identità del client.
- Autorizzazione determina se il client può accedere a una particolare risorsa.

Nell'API Web, i filtri di autenticazione gestiscono l'autenticazione, ma non l'autorizzazione. Autorizzazione deve essere eseguita da un filtro di autorizzazione o all'interno di azione del controller.

Ecco il flusso nella pipeline di Web API 2:

1. Prima di richiamare un'azione, l'API Web crea un elenco di filtri di autenticazione per l'azione. Questo include i filtri con ambito globale, ambito controller e ambito dell'azione.
2. Le chiamate all'API Web **AuthenticateAsync** ogni filtro nell'elenco. Ogni filtro è possibile convalidare le credenziali nella richiesta. Se qualsiasi filtro viene correttamente convalidato le credenziali, il filtro viene creato un **IPrincipal** e lo associa alla richiesta. Un filtro può inoltre generare un errore a questo punto. In questo caso, il resto della pipeline non viene eseguito.
3. Se che non si verificano errori, la richiesta passa attraverso il resto della pipeline.
4. Infine, l'API Web chiama ogni filtro di autenticazione **ChallengeAsync** metodo. Filtri di utilizzano questo metodo per aggiungere una richiesta alla risposta, se necessario. In genere (ma non sempre) che potrebbe verificarsi in risposta a un errore 401.

I diagrammi seguenti mostrano due casi possibili. Nel primo, il filtro di autenticazione autentica correttamente la richiesta, un filtro di autorizzazione autorizza la richiesta e l'azione del controller restituisce 200 (OK).

![](authentication-filters/_static/image1.png)

Nel secondo esempio, il filtro di autenticazione autentica la richiesta, ma il filtro di autorizzazione restituisce 401 (non autorizzato). In questo caso, l'azione del controller non viene richiamato. Il filtro di autenticazione aggiunge un'intestazione Www-Authenticate nella risposta.

![](authentication-filters/_static/image2.png)

Sono possibili altre combinazioni di&mdash;, ad esempio, se l'azione del controller consente le richieste anonime, potrebbe essere un filtro di autenticazione, ma nessuna autorizzazione.

## <a name="implementing-the-authenticateasync-method"></a>Implementazione del metodo AuthenticateAsync

Il **AuthenticateAsync** metodo tenta di autenticare la richiesta. Di seguito è riportata la firma del metodo:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

Il **AuthenticateAsync** metodo deve eseguire una delle operazioni seguenti:

1. Nulla (nessuna operazione).
2. Creare un **IPrincipal** e impostata per la richiesta.
3. Impostare come risultato un errore.

Opzione (1) indica che la richiesta non aveva le credenziali che riconosce il filtro. Opzione (2) indica il filtro viene autenticato correttamente la richiesta. Opzione (3) prevede la richiesta era credenziali non valide (ad esempio, la password errata), che genera una risposta di errore.

Ecco un quadro generale per l'implementazione **AuthenticateAsync**.

1. Cercare le credenziali nella richiesta.
2. Se non esistono credenziali, non eseguire alcuna operazione e tornare (nessuna operazione).
3. Se sono presenti credenziali, ma il filtro non riconosce lo schema di autenticazione, non eseguire alcuna operazione e tornare (nessuna operazione). Un altro filtro nella pipeline può comprendere lo schema.
4. Se sono presenti credenziali che riconosce il filtro, prova per l'autenticazione.
5. Se le credenziali sono danneggiate, restituire 401 impostando `context.ErrorResult`.
6. Se le credenziali siano valide, creare un **IPrincipal** e impostare `context.Principal`.

Nel codice seguente viene illustrato il **AuthenticateAsync** metodo il [l'autenticazione di base](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) esempio. Ogni passaggio indicano i commenti. Il codice illustra i diversi tipi di errore: intestazione di un'autorizzazione con nessun credenziali, le credenziali in formato non corretto e nome utente e password non valida.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Come risultato un errore di impostazione

Se le credenziali sono valide, è necessario impostare il filtro `context.ErrorResult` per un **IHttpActionResult** che crea una risposta di errore. Per ulteriori informazioni su **IHttpActionResult**, vedere [risultati dell'azione in Web API 2](../getting-started-with-aspnet-web-api/action-results.md).

Nell'esempio di autenticazione di base è incluso un `AuthenticationFailureResult` classe adatta a questo scopo.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementazione ChallengeAsync

Lo scopo del **ChallengeAsync** metodo consiste nell'aggiungere richieste di autenticazione nella risposta, se necessario. Di seguito è riportata la firma del metodo:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Il metodo viene chiamato su ogni filtro nella pipeline delle richieste di autenticazione.

È importante comprendere che **ChallengeAsync** viene chiamato *prima* la risposta HTTP è stata creata ed eventualmente anche prima dell'esecuzione di azione del controller. Quando **ChallengeAsync** viene chiamato, `context.Result` contiene un **IHttpActionResult**, utilizzato in seguito per creare la risposta HTTP. Quando **ChallengeAsync** viene chiamato, non si conosce nulla sulla risposta HTTP ancora. Il **ChallengeAsync** metodo deve sostituire il valore originale di `context.Result` con un nuovo **IHttpActionResult**. Questo **IHttpActionResult** deve eseguire il wrapping originale `context.Result`.

![](authentication-filters/_static/image3.png)

Chiamerò originale **IHttpActionResult** il *risultati interno*e il nuovo **IHttpActionResult** il *risultato outer*. Il risultato esterno deve eseguire le operazioni seguenti:

1. Richiamare il risultato interno per creare la risposta HTTP.
2. Esaminare la risposta.
3. Aggiungere una richiesta di autenticazione nella risposta, se necessario.

L'esempio seguente è tratto dall'esempio di autenticazione di base. Definisce un **IHttpActionResult** per il risultato esterno.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

Il `InnerResult` proprietà contiene interna **IHttpActionResult**. Il `Challenge` proprietà rappresenta un'intestazione Www-Authenticate. Si noti che **ExecuteAsync** chiama innanzitutto `InnerResult.ExecuteAsync` per creare la risposta HTTP e quindi aggiunge la richiesta, se necessario.

Controllare il codice di risposta prima di aggiungere la richiesta di verifica. La maggior parte degli schemi di autenticazione aggiungere solo un problema se la risposta 401, come illustrato di seguito. Tuttavia, alcuni schemi di autenticazione aggiungere una richiesta a una risposta con esito positivo. Ad esempio, vedere [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Dato il `AddChallengeOnUnauthorizedResult` classe, il codice effettivo in **ChallengeAsync** è semplice. Appena creata il risultato e associarlo al `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Nota: L'esempio di autenticazione di base astrae questa logica, inserendolo in un metodo di estensione.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Combinazione di filtri di autenticazione con l'autenticazione a livello di Host

"Autenticazione a livello di host" è l'autenticazione eseguita dall'host (ad esempio IIS), prima di framework raggiunge l'API Web di richiesta.

Spesso, si desidera abilitare l'autenticazione a livello di host per il resto dell'applicazione, ma è disabilitata per i controller API Web. Ad esempio, uno scenario tipico è per abilitare l'autenticazione basata su form a livello di host, ma usa l'autenticazione basata su token per l'API Web.

Per disabilitare l'autenticazione a livello di host all'interno della pipeline di Web API, chiamare `config.SuppressHostPrincipal()` nella configurazione. In questo modo, l'API Web rimuovere il **IPrincipal** da qualsiasi richiesta in entrata la pipeline di Web API. È in effetti, &quot;annullare-autentica&quot; la richiesta.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Risorse aggiuntive

[Filtri di sicurezza di ASP.NET Web API](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
