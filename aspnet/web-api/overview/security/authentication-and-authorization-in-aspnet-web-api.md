---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Autenticazione e autorizzazione in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: Fornisce una panoramica generale di autenticazione e autorizzazione in ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d7cbb9505afb6461ba4c2087d57e9ea0da38ede
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Autenticazione e autorizzazione in ASP.NET Web API
====================
da [Mike Wasson](https://github.com/MikeWasson)

Aver creato un'API web, ma ora si desidera controllare l'accesso a esso. In questa serie di articoli, esamineremo alcune opzioni per la protezione di un'API web da utenti non autorizzati. Verranno illustrate in questa serie sia l'autenticazione e autorizzazione.

- *Autenticazione* è conoscere l'identità dell'utente. Ad esempio, Alice accede con il nome utente e la password e il server utilizza la password per l'autenticazione di Alice.
- *Autorizzazione* deve decidere se un utente è autorizzato a eseguire un'azione. Ad esempio, Alice dispone dell'autorizzazione per ottenere una risorsa senza creare una risorsa.

Il primo articolo nella serie offre una panoramica generale di autenticazione e autorizzazione in ASP.NET Web API. Altri argomenti vengono descritti scenari comuni di autenticazione per l'API Web.

> [!NOTE]
> Grazie a persone esaminato questa serie e forniti suggerimenti utili: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Ge Hongmei, David Matson, Daniel Roth, Tim Teebken.


## <a name="authentication"></a>Autenticazione

API Web si presuppone che l'autenticazione avviene nell'host. Per l'hosting web, l'host è IIS che usa i moduli HTTP per l'autenticazione. È possibile configurare il progetto per utilizzare uno dei moduli di autenticazione integrati in IIS o ASP.NET o scrivere un modulo HTTP personalizzato per eseguire l'autenticazione personalizzata.

Quando l'host autentica l'utente, viene creato un *principale*, ovvero un [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) oggetto che rappresenta il contesto di sicurezza in cui viene eseguito il codice. L'host collega l'entità per il thread corrente impostando **CurrentPrincipal**. Contiene un oggetto associato all'entità **identità** oggetto che contiene informazioni sull'utente. Se l'utente è autenticato, il **Identity.IsAuthenticated** restituisce proprietà **true**. Per le richieste anonime, **IsAuthenticated** restituisce **false**. Per ulteriori informazioni sulle entità, vedere [basata sui ruoli di sicurezza](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Gestori di messaggi HTTP per l'autenticazione

Anziché utilizzare l'host per l'autenticazione, è possibile inserire la logica di autenticazione in un [gestore di messaggi HTTP](../advanced/http-message-handlers.md). In tal caso, il gestore di messaggi esamina la richiesta HTTP e imposta l'entità.

Quando è necessario utilizzare gestori di messaggi per l'autenticazione? Ecco alcuni svantaggi:

- Un modulo HTTP rileva tutte le richieste che passano attraverso la pipeline ASP.NET. Le richieste indirizzate all'API Web vengono visualizzati solo da un gestore di messaggi.
- È possibile impostare gestori di messaggi per ogni route, che consente di applicare uno schema di autenticazione a un ciclo specifico.
- I moduli HTTP sono specifici di IIS. Gestori di messaggi sono indipendente dall'host, pertanto possono essere utilizzati con hosting web e self-hosting.
- I moduli HTTP partecipano registrazione IIS, il controllo e così via.
- I moduli HTTP eseguiti in precedenza nella pipeline. Se si gestisce l'autenticazione in un gestore di messaggi, l'entità non ottenere impostata fino a quando non viene eseguito il gestore. Inoltre, l'entità, vengono ripristinate l'entità precedente quando la risposta lascia il gestore di messaggi.

In genere, se non è necessario il supporto self-hosting, un modulo HTTP è un'opzione migliore. Se è necessario per il supporto self-hosting, prendere in considerazione un gestore di messaggi.

### <a name="setting-the-principal"></a>L'entità di impostazione

Se l'applicazione esegua qualsiasi logica di autenticazione personalizzata, è necessario impostare l'entità in due posizioni:

- **Thread.CurrentPrincipal**. Questa proprietà è la modalità standard di set di entità del thread in .NET.
- **HttpContext.Current.User**. Questa proprietà è specifica di ASP.NET.

Il codice seguente viene illustrato come impostare l'entità:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Per l'hosting web, è necessario impostare l'entità in entrambe le posizioni; in caso contrario il contesto di sicurezza può diventare incoerente. Per il Self-hosting, tuttavia, **HttpContext. Current** è null. Per verificare il codice sia indipendente dall'host, di conseguenza, verificare i valori null prima di assegnare a **HttpContext. Current**, come illustrato.

## <a name="authorization"></a>Autorizzazione

Si verifica l'autorizzazione in un secondo momento nella pipeline, più vicino al controller. Che consente di effettuare scelte più granulare, quando si concede l'accesso alle risorse.

- *Filtri di autorizzazione* eseguire prima l'azione del controller. Se la richiesta non è autorizzata, il filtro restituisce una risposta di errore e non viene richiamata l'azione.
- All'interno di un'azione del controller, è possibile ottenere l'entità corrente dal **ApiController.User** proprietà. Ad esempio, è possibile filtrare un elenco di risorse in base al nome utente, restituisce solo le risorse che appartengono a tale utente.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Utilizzando il [autorizzare] attributo

API Web fornisce un filtro di autorizzazione predefiniti, [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Questo filtro controlla se l'utente è autenticato. In caso contrario, restituisce il codice di stato HTTP 401 (Unauthorized), senza richiamare l'azione.

È possibile applicare il filtro a livello globale, a livello di controller o al livello di singole azioni.

**A livello globale**: per limitare l'accesso per ogni controller API Web, aggiungere il **AuthorizeAttribute** filtro all'elenco di filtri globali:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Controller**: per limitare l'accesso per un controller specifico, aggiungere il filtro come attributo per il controller:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Azione**: per limitare l'accesso per azioni specifiche, aggiungere l'attributo al metodo di azione:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

In alternativa, è possibile limitare il controller di e quindi consentire l'accesso anonimo ad azioni specifiche usando il `[AllowAnonymous]` attributo. Nell'esempio seguente, il `Post` metodo è limitato, ma la `Get` metodo consente l'accesso anonimo.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

Negli esempi precedenti, il filtro consente agli utenti autenticati di accedere ai metodi con restrizioni; solo gli utenti anonimi vengono mantenuti. È inoltre possibile limitare l'accesso a utenti specifici o per gli utenti a ruoli specifici:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> Il **AuthorizeAttribute** filtro per controller API Web si trova nella **System.Web.Http** dello spazio dei nomi. È presente un filtro per i controller MVC simile di **System** spazio dei nomi, che non è compatibile con i controller API Web.


### <a name="custom-authorization-filters"></a>Filtri di autorizzazione personalizzato

Per scrivere un filtro di autorizzazione personalizzato, derivare da uno dei seguenti tipi:

- **AuthorizeAttribute**. Estendere questa classe per eseguire la logica di autorizzazione in base all'utente corrente e i ruoli dell'utente.
- **AuthorizationFilterAttribute**. Estendere questa classe per eseguire la logica di autorizzazione sincrone che non è necessariamente basata su utente corrente o del ruolo.
- **IAuthorizationFilter**. Implementare questa interfaccia per eseguire la logica asincrona autorizzazione; ad esempio, se la logica di autorizzazione effettua chiamate asincrone dei / o o di rete. (Se la logica di autorizzazione è associato alla CPU, è più semplice da cui derivare **AuthorizationFilterAttribute**, perché è necessario scrivere un metodo asincrono.)

Il diagramma seguente mostra la gerarchia di classi per la **AuthorizeAttribute** classe.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorizzazione all'interno di un'azione del Controller

In alcuni casi, è possibile consentire a una richiesta per proseguire, ma modificare il comportamento in base all'entità. Ad esempio, le informazioni che restituiscono potrebbero cambiare a seconda del ruolo dell'utente. All'interno di un metodo del controller, è possibile ottenere il principio corrente dal **ApiController.User** proprietà.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
