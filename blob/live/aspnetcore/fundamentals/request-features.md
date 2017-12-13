---
title: "Funzionalità di richiesta ASP.NET Core"
author: ardalis
description: Informazioni sui dettagli di implementazione di server web relativi a richieste HTTP e le risposte che sono definite nelle interfacce per ASP.NET Core.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: b689d82d16c6ef55485691b3474a070765c8144b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="8d170-104">Funzionalità di richiesta ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d170-104">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="8d170-105">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8d170-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8d170-106">I dettagli dell'implementazione di server Web relativi alle richieste e alle risposte HTTP vengono definiti nelle interfacce.</span><span class="sxs-lookup"><span data-stu-id="8d170-106">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="8d170-107">Queste interfacce sono usate da implementazioni del server e del middleware per creare e modificare pipeline hosting dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8d170-107">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="8d170-108">Interfacce di funzionalità</span><span class="sxs-lookup"><span data-stu-id="8d170-108">Feature interfaces</span></span>

<span data-ttu-id="8d170-109">ASP.NET Core definisce un numero di interfacce di funzionalità HTTP in `Microsoft.AspNetCore.Http.Features` che sono usati dal server per identificare le caratteristiche supportate.</span><span class="sxs-lookup"><span data-stu-id="8d170-109">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="8d170-110">Le interfacce di funzionalità seguenti di gestiscono le richieste e restituiscono risposte:</span><span class="sxs-lookup"><span data-stu-id="8d170-110">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="8d170-111">`IHttpRequestFeature`Definisce la struttura di una richiesta HTTP, inclusi il protocollo, percorso, la stringa di query, le intestazioni e corpo.</span><span class="sxs-lookup"><span data-stu-id="8d170-111">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="8d170-112">`IHttpResponseFeature`Definisce la struttura di una risposta HTTP, tra cui il codice di stato, intestazioni e corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="8d170-112">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="8d170-113">`IHttpAuthenticationFeature`Definisce il supporto per identificare gli utenti in base a un `ClaimsPrincipal` e specificando un gestore di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="8d170-113">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="8d170-114">`IHttpUpgradeFeature`Definisce il supporto per [aggiornamenti HTTP](https://tools.ietf.org/html/rfc2616.html#section-14.42), che consente al client di specificare ulteriori protocolli si desidera utilizzare se il server si desidera passare protocolli.</span><span class="sxs-lookup"><span data-stu-id="8d170-114">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="8d170-115">`IHttpBufferingFeature`Definisce metodi per la disattivazione del buffer di richieste e/o risposte.</span><span class="sxs-lookup"><span data-stu-id="8d170-115">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="8d170-116">`IHttpConnectionFeature`Definisce le proprietà per le porte e indirizzi locali e remoti.</span><span class="sxs-lookup"><span data-stu-id="8d170-116">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="8d170-117">`IHttpRequestLifetimeFeature`Definisce il supporto per l'interruzione delle connessioni o rilevare se una richiesta è stata terminata in modo anomalo, ad esempio come da una disconnessione del client.</span><span class="sxs-lookup"><span data-stu-id="8d170-117">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="8d170-118">`IHttpSendFileFeature`Definisce un metodo per l'invio di file in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="8d170-118">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="8d170-119">`IHttpWebSocketFeature`Definisce un'API per il supporto di WebSocket.</span><span class="sxs-lookup"><span data-stu-id="8d170-119">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="8d170-120">`IHttpRequestIdentifierFeature`Aggiunge una proprietà che può essere implementata per identificare in modo univoco le richieste.</span><span class="sxs-lookup"><span data-stu-id="8d170-120">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="8d170-121">`ISessionFeature`Definisce `ISessionFactory` e `ISession` astrazioni per supportare le sessioni utente.</span><span class="sxs-lookup"><span data-stu-id="8d170-121">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="8d170-122">`ITlsConnectionFeature`Definisce un'API per il recupero dei certificati client.</span><span class="sxs-lookup"><span data-stu-id="8d170-122">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="8d170-123">`ITlsTokenBindingFeature`Definisce i metodi per l'utilizzo di parametri di associazione dei token TLS.</span><span class="sxs-lookup"><span data-stu-id="8d170-123">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="8d170-124">`ISessionFeature`non è una funzionalità server, ma viene implementato dal `SessionMiddleware` (vedere [lo stato dell'applicazione di gestione](app-state.md)).</span><span class="sxs-lookup"><span data-stu-id="8d170-124">`ISessionFeature` is not a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="8d170-125">Raccolte di funzionalità</span><span class="sxs-lookup"><span data-stu-id="8d170-125">Feature collections</span></span>

<span data-ttu-id="8d170-126">Il `Features` proprietà `HttpContext` fornisce un'interfaccia per ottenere e impostare le funzionalità HTTP disponibili per la richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="8d170-126">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="8d170-127">Poiché l'insieme di funzionalità è modificabile anche all'interno del contesto di una richiesta, middleware consente di modificare la raccolta e aggiungere il supporto per funzionalità aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="8d170-127">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="8d170-128">Funzionalità middleware e richiesta</span><span class="sxs-lookup"><span data-stu-id="8d170-128">Middleware and request features</span></span>

<span data-ttu-id="8d170-129">Anche se i server sono responsabili della creazione dell'insieme di funzionalità, middleware sia possibile aggiungere a questa raccolta e usano funzionalità dalla raccolta.</span><span class="sxs-lookup"><span data-stu-id="8d170-129">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="8d170-130">Ad esempio, il `StaticFileMiddleware` accede il `IHttpSendFileFeature` funzionalità.</span><span class="sxs-lookup"><span data-stu-id="8d170-130">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="8d170-131">Se la funzionalità è presente, utilizzato per inviare il file statico richiesto dal relativo percorso fisico.</span><span class="sxs-lookup"><span data-stu-id="8d170-131">If the feature exists, it is used to send the requested static file from its physical path.</span></span> <span data-ttu-id="8d170-132">In caso contrario, un metodo alternativo più lento viene utilizzato per inviare il file.</span><span class="sxs-lookup"><span data-stu-id="8d170-132">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="8d170-133">Quando è disponibile, il `IHttpSendFileFeature` consente al sistema operativo aprire il file ed eseguire una copia della modalità kernel diretto alla scheda di rete.</span><span class="sxs-lookup"><span data-stu-id="8d170-133">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="8d170-134">Middleware è inoltre possibile aggiungere alla raccolta funzionalità stabilita dal server.</span><span class="sxs-lookup"><span data-stu-id="8d170-134">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="8d170-135">Le funzionalità esistenti possono anche essere sostituite da middleware, consentendo il middleware di incrementare le funzionalità del server.</span><span class="sxs-lookup"><span data-stu-id="8d170-135">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="8d170-136">Funzionalità aggiunte alla raccolta sono disponibili immediatamente a altro middleware o dell'applicazione sottostante in un secondo momento nella pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="8d170-136">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="8d170-137">Combinando le implementazioni personalizzate per il server e i miglioramenti specifico middleware del preciso set di funzionalità che richiede un'applicazione può essere costruito.</span><span class="sxs-lookup"><span data-stu-id="8d170-137">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="8d170-138">In questo modo mancanti funzionalità da aggiungere senza richiedere una modifica nel server e assicura che vengono esposti solo la quantità minima di funzionalità, limitando in tal modo l'attacco area di superficie e migliorare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="8d170-138">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="8d170-139">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="8d170-139">Summary</span></span>

<span data-ttu-id="8d170-140">Le interfacce di funzionalità definire funzionalità specifiche di HTTP che può supportare una determinata richiesta.</span><span class="sxs-lookup"><span data-stu-id="8d170-140">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="8d170-141">Server di definiscono le raccolte delle funzionalità e il set iniziale di funzionalità supportate dal server, ma middleware può essere utilizzato per migliorare le funzionalità.</span><span class="sxs-lookup"><span data-stu-id="8d170-141">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8d170-142">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8d170-142">Additional Resources</span></span>

* [<span data-ttu-id="8d170-143">Server</span><span class="sxs-lookup"><span data-stu-id="8d170-143">Servers</span></span>](servers/index.md)

* [<span data-ttu-id="8d170-144">Middleware</span><span class="sxs-lookup"><span data-stu-id="8d170-144">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="8d170-145">Aprire l'interfaccia Web per .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="8d170-145">Open Web Interface for .NET (OWIN)</span></span>](owin.md)
