---
uid: web-api/overview/security/authentication-filters
title: Filtri di autenticazione nell'API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Un filtro di autenticazione è un componente che consente di autenticare una richiesta HTTP. Web API 2 e MVC 5 supportano entrambi i filtri di autenticazione, ma differiscono leggermente...
ms.author: riande
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: a14facad4cbd0f9be1ff7bde2667f61ec8cc2a14
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837776"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>Filtri di autenticazione nell'API Web ASP.NET 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

> Un filtro di autenticazione è un componente che consente di autenticare una richiesta HTTP. Web API 2 e MVC 5 supportano entrambi i filtri di autenticazione, ma differiscono leggermente, soprattutto per le convenzioni di denominazione per l'interfaccia di filtro. In questo argomento vengono descritti i filtri di autenticazione di API Web.


I filtri di autenticazione consentono di impostare uno schema di autenticazione per singoli controller o azioni. In questo modo, l'app può supportare i meccanismi di autenticazione diversi per diverse risorse HTTP.

In questo articolo verrà illustrato dal codice il [l'autenticazione di base](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) esempio sul [ http://aspnet.codeplex.com ](http://aspnet.codeplex.com). L'esempio illustra un filtro di autenticazione che implementa lo schema di autenticazione HTTP di base accesso (RFC 2617). Il filtro viene implementato in una classe denominata `IdentityBasicAuthenticationAttribute`. Non mostrano tutto il codice dell'esempio solo le parti che illustrano come scrivere un filtro di autenticazione.

## <a name="setting-an-authentication-filter"></a>Impostazione di un filtro di autenticazione

Come altri filtri, filtri di autenticazione possono essere applicato per ogni controller, per ogni azione o a livello globale su tutti i controller API Web.

Per applicare un filtro di autenticazione a un controller, decorare la classe controller con l'attributo di filtro. Il codice seguente imposta la `[IdentityBasicAuthentication]` filtro in una classe controller, che consente l'autenticazione di base per tutte le azioni del controller.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Per applicare il filtro a un'azione, decorare l'azione con il filtro. Il codice seguente imposta la `[IdentityBasicAuthentication]` filtrare il controller `Post` (metodo).

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Per applicare il filtro per tutti i controller API Web, aggiungerlo alla **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementazione di un filtro di autenticazione di API Web

Nell'API Web, implementano filtri di autenticazione di [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) interfaccia. Deve anche ereditare **System. Attribute**, in modo da essere applicati come attributi.

Il **IAuthenticationFilter** interfaccia dispone di due metodi:

- **AuthenticateAsync** autentica la richiesta per la convalida delle credenziali nella richiesta, se presente.
- **ChallengeAsync** aggiunge una richiesta di autenticazione nella risposta HTTP, se necessario.

Questi metodi corrispondano per il flusso di autenticazione definito in [RFC 2612](http://tools.ietf.org/html/rfc2616) e [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. Il client invia le credenziali nell'intestazione dell'autorizzazione. Ciò accade solitamente dopo che il client riceve una risposta 401 (non autorizzato) dal server. Tuttavia, un client può inviare le credenziali in tutte le richieste, non solo dopo aver ottenuto un codice 401.
2. Se il server non accetta le credenziali, restituisce una risposta 401 (non autorizzato). La risposta include un'intestazione Www-Authenticate contenente uno o più problemi. Ogni richiesta di verifica specifica uno schema di autenticazione riconosciuto dal server.

Il server può inoltre restituire 401 da una richiesta anonima. In effetti, che è in genere come viene avviato il processo di autenticazione:

1. Il client invia una richiesta anonima.
2. Il server restituisce 401.
3. Il client invia nuovamente la richiesta con le credenziali.

Questo flusso include sia *authentication* e *autorizzazione* passaggi.

- L'autenticazione di dimostrare l'identità del client.
- Autorizzazione determina se il client può accedere a una determinata risorsa.

Nell'API Web, i filtri di autenticazione gestiscono l'autenticazione, ma non di autorizzazione. Autorizzazione deve essere eseguita da un filtro di autorizzazione o all'interno di azione del controller.

Ecco il flusso nella pipeline di Web API 2:

1. Prima di richiamare un'azione, API Web crea un elenco dei filtri di autenticazione per l'azione. Sono inclusi i filtri con ambito globale, ambito del controller e ambito dell'azione.
2. Le chiamate API Web **AuthenticateAsync** su ogni filtro nell'elenco. Ogni filtro è possibile convalidare le credenziali nella richiesta. Se qualsiasi filtro convalida correttamente le credenziali, il filtro consente di creare un **IPrincipal** e lo collega alla richiesta. Un filtro può inoltre generare un errore a questo punto. In questo caso, il resto della pipeline non viene eseguito.
3. Supponendo che non si è verificato alcun errore, la richiesta viene trasmessa attraverso il resto della pipeline.
4. Infine, API Web chiama ogni filtro di autenticazione **ChallengeAsync** (metodo). Filtri di usano questo metodo per aggiungere una sfida per la risposta, se necessario. In genere, ma non sempre, in questo caso vengono eseguiti in risposta a un errore 401.

I diagrammi seguenti mostrano due i casi possibili. Nel primo, il filtro di autenticazione la richiesta viene autenticato correttamente, un filtro di autorizzazione autorizza la richiesta e l'azione del controller restituisce 200 (OK).

![](authentication-filters/_static/image1.png)

Nel secondo esempio, il filtro di autenticazione autentica la richiesta, ma il filtro di autorizzazione restituisce 401 (non autorizzato). In questo caso, non viene richiamata l'azione del controller. Il filtro di autenticazione aggiunge un'intestazione Www-Authenticate nella risposta.

![](authentication-filters/_static/image2.png)

Sono possibili altre combinazioni&mdash;, ad esempio, se l'azione del controller consente le richieste anonime, potrebbe essere un filtro di autenticazione, ma nessuna autorizzazione.

## <a name="implementing-the-authenticateasync-method"></a>L'implementazione del metodo AuthenticateAsync

Il **AuthenticateAsync** metodo tenta di autenticare la richiesta. Ecco la firma del metodo:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

Il **AuthenticateAsync** metodo deve eseguire una delle operazioni seguenti:

1. Nothing (no-op).
2. Creare un **IPrincipal** e impostarla sulla richiesta.
3. Impostare un risultato di errore.

Opzione (1) indica che la richiesta non aveva le credenziali che comprende il filtro. Opzione (2) significa che il filtro viene autenticato correttamente la richiesta. Opzione (3) significa che la richiesta era credenziali non valide (ad esempio, la password errata), che attiva una risposta di errore.

Ecco una descrizione generale per l'implementazione **AuthenticateAsync**.

1. Cercare le credenziali nella richiesta.
2. Se non sono disponibili credenziali, non eseguire alcuna operazione e restituire (no-op).
3. Se sono presenti credenziali, ma il filtro non riconosce lo schema di autenticazione, non eseguire alcuna operazione e restituire (no-op). Un altro filtro nella pipeline può comprendere lo schema.
4. Se sono presenti credenziali che comprende il filtro, provare a eseguire l'autenticazione.
5. Se le credenziali sono danneggiate, restituito 401 impostando `context.ErrorResult`.
6. Se le credenziali sono valide, creare un **IPrincipal** e impostare `context.Principal`.

Nel codice seguente viene illustrato il **AuthenticateAsync** metodo le [l'autenticazione di base](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) esempio. I commenti indicano ogni passaggio. Il codice illustra diversi tipi di errore: intestazione di autorizzazione An senza credenziali, le credenziali in formato non corretto e nome utente/password non valida.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>L'impostazione di un risultato di errore

Se le credenziali sono valide, è necessario impostare il filtro `context.ErrorResult` a un **IHttpActionResult** che crea una risposta di errore. Per altre informazioni sulle **IHttpActionResult**, vedere [i risultati dell'azione nell'API Web 2](../getting-started-with-aspnet-web-api/action-results.md).

L'esempio di autenticazione di base include un `AuthenticationFailureResult` classe adatta a questo scopo.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementazione ChallengeAsync

Lo scopo del **ChallengeAsync** metodo consiste nell'aggiungere problemi di autenticazione nella risposta, se necessario. Ecco la firma del metodo:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Il metodo viene chiamato su ogni filtro di autenticazione nella pipeline delle richieste.

È importante comprendere che **ChallengeAsync** viene chiamato *prima* la risposta HTTP è stata creata ed eventualmente anche prima dell'esecuzione di azione del controller. Quando **ChallengeAsync** viene chiamato `context.Result` contiene un' **IHttpActionResult**, che viene usato in un secondo momento per creare la risposta HTTP. Quando **ChallengeAsync** viene chiamato, non si conosce nulla sulla risposta HTTP ancora. Il **ChallengeAsync** metodo deve sostituire il valore originale della `context.Result` con un nuovo **IHttpActionResult**. Ciò **IHttpActionResult** necessario eseguire il wrapping dell'originale `context.Result`.

![](authentication-filters/_static/image3.png)

Che denominerò originale **IHttpActionResult** le *risultati interno*e il nuovo **IHttpActionResult** il *risultato outer*. Il risultato esterno deve eseguire le operazioni seguenti:

1. Richiamare il risultato interno per creare la risposta HTTP.
2. Esaminare la risposta.
3. Aggiungere una richiesta di autenticazione per la risposta, se necessario.

Nell'esempio seguente è tratto dall'esempio autenticazione di base. Definisce un **IHttpActionResult** per il risultato esterno.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

Il `InnerResult` proprietà contiene interna **IHttpActionResult**. Il `Challenge` proprietà rappresenta un'intestazione Www-Authenticate. Si noti che **ExecuteAsync** chiama innanzitutto `InnerResult.ExecuteAsync` per creare la risposta HTTP e quindi aggiunge la richiesta di verifica se necessario.

Controllare il codice di risposta prima di aggiungere la richiesta di verifica. La maggior parte degli schemi di autenticazione aggiungere solo una richiesta di verifica se la risposta 401, come illustrato di seguito. Tuttavia, alcuni schemi di autenticazione si aggiunge una sfida per una risposta di esito positivo. Ad esempio, vedere [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Dato il `AddChallengeOnUnauthorizedResult` classe, il codice effettivamente presente **ChallengeAsync** è semplice. È solo creare il risultato e collegarlo a `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Nota: L'esempio di autenticazione di base consente di astrarre questa logica un po', inserendolo in un metodo di estensione.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>La combinazione di filtri di autenticazione con l'autenticazione a livello di Host

"L'autenticazione a livello di host" è l'autenticazione eseguita dall'host (ad esempio IIS), prima di framework raggiunge l'API Web di richiesta.

Spesso, è possibile abilitare l'autenticazione a livello di host per il resto dell'applicazione, ma disabilitare la funzionalità di controller API Web. Uno scenario tipico è ad esempio, per abilitare l'autenticazione basata su form a livello di host, ma usano l'autenticazione basata su token per l'API Web.

Per disabilitare l'autenticazione a livello di host all'interno della pipeline di Web API, chiamare `config.SuppressHostPrincipal()` nella configurazione. In questo modo, API Web rimuovere il **IPrincipal** da qualsiasi richiesta in entrata della pipeline API Web. In effetti, si &quot;ONU-autentica&quot; la richiesta.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Risorse aggiuntive

[Filtri di protezione dell'API Web ASP.NET](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
