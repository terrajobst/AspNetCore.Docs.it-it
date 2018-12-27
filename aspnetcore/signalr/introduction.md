---
title: Introduzione a ASP.NET Core SignalR
author: tdykstra
description: Informazioni su come la libreria di ASP.NET Core SignalR semplifica l'aggiunta di funzionalità in tempo reale per le app.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: da18837c690d2182589db5f486ae74e537ade931
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425146"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="cb3e7-103">Introduzione a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="cb3e7-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="cb3e7-104">Che cos'è SignalR?</span><span class="sxs-lookup"><span data-stu-id="cb3e7-104">What is SignalR?</span></span>

<span data-ttu-id="cb3e7-105">ASP.NET Core SignalR è una libreria open source che semplifica l'aggiunta della funzionalità web in tempo reale per le app.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="cb3e7-106">Funzionalità web in tempo reale consente immediatamente il codice lato server push di contenuti ai client.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="cb3e7-107">Buoni candidati per SignalR:</span><span class="sxs-lookup"><span data-stu-id="cb3e7-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="cb3e7-108">App che richiedono aggiornamenti ad alta frequenza dal server.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="cb3e7-109">Esempi sono di gioco, social network, voto, delle aste, mappe e App GPS.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="cb3e7-110">I dashboard e App di monitoraggio.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="cb3e7-111">Gli esempi includono dashboard aziendali, gli aggiornamenti di vendita immediati, o avvisi di comunicazione.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="cb3e7-112">App di collaborazione.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-112">Collaborative apps.</span></span> <span data-ttu-id="cb3e7-113">Le app di lavagna e riunione software del team sono esempi di App di collaborazione.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="cb3e7-114">App che richiedono le notifiche.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-114">Apps that require notifications.</span></span> <span data-ttu-id="cb3e7-115">Social network, messaggio di posta elettronica, chat, giochi, gli avvisi di viaggi e molte altre App usare le notifiche.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="cb3e7-116">SignalR fornisce un'API per la creazione di server a client [remote procedure call (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="cb3e7-116">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="cb3e7-117">Le chiamate RPC chiamare le funzioni JavaScript nei client dal codice di .NET Core sul lato server.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="cb3e7-118">Ecco alcune funzionalità di SignalR per ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="cb3e7-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="cb3e7-119">Gestisce automaticamente la gestione della connessione.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="cb3e7-120">Invia messaggi a tutti i client connessi contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="cb3e7-121">Ad esempio una chat room.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-121">For example, a chat room.</span></span>
* <span data-ttu-id="cb3e7-122">Invia messaggi al client specifici o gruppi di client.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="cb3e7-123">Scalabilità sufficiente a gestire il crescente traffico.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="cb3e7-124">L'origine è ospitato in un [SignalR repository in GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span><span class="sxs-lookup"><span data-stu-id="cb3e7-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="cb3e7-125">Trasporti</span><span class="sxs-lookup"><span data-stu-id="cb3e7-125">Transports</span></span>

<span data-ttu-id="cb3e7-126">SignalR supporta diverse tecniche per la gestione delle comunicazioni in tempo reale:</span><span class="sxs-lookup"><span data-stu-id="cb3e7-126">SignalR supports several techniques for handling real-time communications:</span></span>

* [<span data-ttu-id="cb3e7-127">Oggetti WebSocket</span><span class="sxs-lookup"><span data-stu-id="cb3e7-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="cb3e7-128">Eventi inviati al server</span><span class="sxs-lookup"><span data-stu-id="cb3e7-128">Server-Sent Events</span></span>
* <span data-ttu-id="cb3e7-129">Polling di lunga durata</span><span class="sxs-lookup"><span data-stu-id="cb3e7-129">Long Polling</span></span>

<span data-ttu-id="cb3e7-130">SignalR sceglie automaticamente il migliore metodo di trasporto compreso le funzionalità del server e client.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-130">SignalR automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="cb3e7-131">Hub</span><span class="sxs-lookup"><span data-stu-id="cb3e7-131">Hubs</span></span>

<span data-ttu-id="cb3e7-132">Usa SignalR *hub* per la comunicazione tra client e server.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-132">SignalR uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="cb3e7-133">Un hub è una pipeline di alto livello che consente a un client e server chiamare i metodi a vicenda.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> <span data-ttu-id="cb3e7-134">SignalR gestisce l'invio fra computer automaticamente, consentendo ai client di chiamare metodi sul server e viceversa.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="cb3e7-135">È possibile passare parametri fortemente tipizzati per metodi, che consente l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="cb3e7-136">SignalR fornisce due protocolli di hub predefinito: un protocollo di testo basati su JSON e un protocollo binario basato sul [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="cb3e7-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="cb3e7-137">MessagePack crea in genere i messaggi più piccoli rispetto a JSON.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="cb3e7-138">Devono supportare i browser meno recenti [livello XHR 2](https://caniuse.com/#feat=xhr2) per fornire supporto del protocollo MessagePack.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="cb3e7-139">Hub di chiamare codice lato client mediante l'invio di messaggi che contengono il nome e parametri del metodo lato client.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="cb3e7-140">Gli oggetti inviati come parametri di metodo vengono deserializzati tramite il protocollo configurato.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="cb3e7-141">Il client cerca la corrispondenza con il nome a un metodo nel codice lato client.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="cb3e7-142">Quando il client trova una corrispondenza, chiama il metodo e passa i dati del parametro deserializzato.</span><span class="sxs-lookup"><span data-stu-id="cb3e7-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cb3e7-143">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="cb3e7-143">Additional resources</span></span>

* [<span data-ttu-id="cb3e7-144">Introduzione a SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cb3e7-144">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="cb3e7-145">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="cb3e7-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="cb3e7-146">Hub</span><span class="sxs-lookup"><span data-stu-id="cb3e7-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="cb3e7-147">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="cb3e7-147">JavaScript client</span></span>](xref:signalr/javascript-client)
