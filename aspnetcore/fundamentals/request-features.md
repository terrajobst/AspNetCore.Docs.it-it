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
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/01/2017
---
# <a name="request-features-in-aspnet-core"></a>Funzionalità di richiesta ASP.NET Core

Da [Steve Smith](https://ardalis.com/)

Dettagli di implementazione di server Web relativi alle richieste HTTP e le risposte sono definite nelle interfacce. Queste interfacce sono usate da implementazioni del server e del middleware per creare e modificare pipeline hosting dell'applicazione.

## <a name="feature-interfaces"></a>Interfacce di funzionalità

ASP.NET Core definisce un numero di interfacce di funzionalità HTTP in `Microsoft.AspNetCore.Http.Features` che sono usati dal server per identificare le caratteristiche supportate. Le interfacce di funzionalità seguenti di gestiscono le richieste e restituiscono risposte:

`IHttpRequestFeature`Definisce la struttura di una richiesta HTTP, inclusi il protocollo, percorso, la stringa di query, le intestazioni e corpo.

`IHttpResponseFeature`Definisce la struttura di una risposta HTTP, tra cui il codice di stato, intestazioni e corpo della risposta.

`IHttpAuthenticationFeature`Definisce il supporto per identificare gli utenti in base a un `ClaimsPrincipal` e specificando un gestore di autenticazione.

`IHttpUpgradeFeature`Definisce il supporto per [aggiornamenti HTTP](https://tools.ietf.org/html/rfc2616.html#section-14.42), che consente al client di specificare ulteriori protocolli si desidera utilizzare se il server si desidera passare protocolli.

`IHttpBufferingFeature`Definisce metodi per la disattivazione del buffer di richieste e/o risposte.

`IHttpConnectionFeature`Definisce le proprietà per le porte e indirizzi locali e remoti.

`IHttpRequestLifetimeFeature`Definisce il supporto per l'interruzione delle connessioni o rilevare se una richiesta è stata terminata in modo anomalo, ad esempio come da una disconnessione del client.

`IHttpSendFileFeature`Definisce un metodo per l'invio di file in modo asincrono.

`IHttpWebSocketFeature`Definisce un'API per il supporto di WebSocket.

`IHttpRequestIdentifierFeature`Aggiunge una proprietà che può essere implementata per identificare in modo univoco le richieste.

`ISessionFeature`Definisce `ISessionFactory` e `ISession` astrazioni per supportare le sessioni utente.

`ITlsConnectionFeature`Definisce un'API per il recupero dei certificati client.

`ITlsTokenBindingFeature`Definisce i metodi per l'utilizzo di parametri di associazione dei token TLS.

> [!NOTE]
> `ISessionFeature`non è una funzionalità server, ma viene implementato dal `SessionMiddleware` (vedere [lo stato dell'applicazione di gestione](app-state.md)).

## <a name="feature-collections"></a>Raccolte di funzionalità

Il `Features` proprietà `HttpContext` fornisce un'interfaccia per ottenere e impostare le funzionalità HTTP disponibili per la richiesta corrente. Poiché l'insieme di funzionalità è modificabile anche all'interno del contesto di una richiesta, middleware consente di modificare la raccolta e aggiungere il supporto per funzionalità aggiuntive.

## <a name="middleware-and-request-features"></a>Funzionalità middleware e richiesta

Anche se i server sono responsabili della creazione dell'insieme di funzionalità, middleware sia possibile aggiungere a questa raccolta e usano funzionalità dalla raccolta. Ad esempio, il `StaticFileMiddleware` accede il `IHttpSendFileFeature` funzionalità. Se la funzionalità è presente, utilizzato per inviare il file statico richiesto dal relativo percorso fisico. In caso contrario, un metodo alternativo più lento viene utilizzato per inviare il file. Quando è disponibile, il `IHttpSendFileFeature` consente al sistema operativo aprire il file ed eseguire una copia della modalità kernel diretto alla scheda di rete.

Middleware è inoltre possibile aggiungere alla raccolta funzionalità stabilita dal server. Le funzionalità esistenti possono anche essere sostituite da middleware, consentendo il middleware di incrementare le funzionalità del server. Funzionalità aggiunte alla raccolta sono disponibili immediatamente a altro middleware o dell'applicazione sottostante in un secondo momento nella pipeline delle richieste.

Combinando le implementazioni personalizzate per il server e i miglioramenti specifico middleware del preciso set di funzionalità che richiede un'applicazione può essere costruito. In questo modo mancanti funzionalità da aggiungere senza richiedere una modifica nel server e assicura che vengono esposti solo la quantità minima di funzionalità, limitando in tal modo l'attacco area di superficie e migliorare le prestazioni.

## <a name="summary"></a>Riepilogo

Le interfacce di funzionalità definire funzionalità specifiche di HTTP che può supportare una determinata richiesta. Server di definiscono le raccolte delle funzionalità e il set iniziale di funzionalità supportate dal server, ma middleware può essere utilizzato per migliorare le funzionalità.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Server](servers/index.md)

* [Middleware](middleware.md)

* [Aprire l'interfaccia Web per .NET (OWIN)](owin.md)
