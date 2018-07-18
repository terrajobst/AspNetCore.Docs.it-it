---
title: Introduzione a ASP.NET Core SignalR
author: tdykstra
description: Informazioni su come la libreria di ASP.NET Core SignalR semplifica l'aggiunta di funzionalità in tempo reale per le app.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095389"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="e8fda-103">Introduzione a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e8fda-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="e8fda-104">[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="e8fda-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="e8fda-105">Che cos'è SignalR?</span><span class="sxs-lookup"><span data-stu-id="e8fda-105">What is SignalR?</span></span>

<span data-ttu-id="e8fda-106">ASP.NET Core SignalR è una libreria che semplifica l'aggiunta della funzionalità web in tempo reale per le app.</span><span class="sxs-lookup"><span data-stu-id="e8fda-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="e8fda-107">Funzionalità web in tempo reale consente immediatamente il codice lato server push di contenuti ai client.</span><span class="sxs-lookup"><span data-stu-id="e8fda-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="e8fda-108">Buoni candidati per SignalR:</span><span class="sxs-lookup"><span data-stu-id="e8fda-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="e8fda-109">App che richiedono aggiornamenti ad alta frequenza dal server.</span><span class="sxs-lookup"><span data-stu-id="e8fda-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="e8fda-110">Esempi sono di gioco, social network, voto, delle aste, mappe e App GPS.</span><span class="sxs-lookup"><span data-stu-id="e8fda-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="e8fda-111">I dashboard e App di monitoraggio.</span><span class="sxs-lookup"><span data-stu-id="e8fda-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="e8fda-112">Gli esempi includono dashboard aziendali, gli aggiornamenti di vendita immediati, o avvisi di comunicazione.</span><span class="sxs-lookup"><span data-stu-id="e8fda-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="e8fda-113">App di collaborazione.</span><span class="sxs-lookup"><span data-stu-id="e8fda-113">Collaborative apps.</span></span> <span data-ttu-id="e8fda-114">Le app di lavagna e riunione software del team sono esempi di App di collaborazione.</span><span class="sxs-lookup"><span data-stu-id="e8fda-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="e8fda-115">App che richiedono le notifiche.</span><span class="sxs-lookup"><span data-stu-id="e8fda-115">Apps that require notifications.</span></span> <span data-ttu-id="e8fda-116">Social network, messaggio di posta elettronica, chat, giochi, gli avvisi di viaggi e molte altre App usare le notifiche.</span><span class="sxs-lookup"><span data-stu-id="e8fda-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="e8fda-117">SignalR fornisce un'API per la creazione di server a client [remote procedure call (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="e8fda-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="e8fda-118">Le chiamate RPC chiamare le funzioni JavaScript nei client dal codice di .NET Core sul lato server.</span><span class="sxs-lookup"><span data-stu-id="e8fda-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="e8fda-119">SignalR per ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e8fda-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="e8fda-120">Gestisce automaticamente la gestione della connessione.</span><span class="sxs-lookup"><span data-stu-id="e8fda-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="e8fda-121">Abilita la trasmissione di messaggi per tutti i client connessi contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="e8fda-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="e8fda-122">Ad esempio una chat room.</span><span class="sxs-lookup"><span data-stu-id="e8fda-122">For example, a chat room.</span></span>
* <span data-ttu-id="e8fda-123">Abilita l'invio di messaggi a client specifici o gruppi di client.</span><span class="sxs-lookup"><span data-stu-id="e8fda-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="e8fda-124">È open source in [GitHub](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="e8fda-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="e8fda-125">Scalabile.</span><span class="sxs-lookup"><span data-stu-id="e8fda-125">Scalable.</span></span>

<span data-ttu-id="e8fda-126">La connessione tra il client e server è permanente, a differenza di una connessione HTTP.</span><span class="sxs-lookup"><span data-stu-id="e8fda-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="e8fda-127">Trasporti</span><span class="sxs-lookup"><span data-stu-id="e8fda-127">Transports</span></span>

<span data-ttu-id="e8fda-128">Riassunti di SignalR su una serie di tecniche per la compilazione di applicazioni web in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="e8fda-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="e8fda-129">[WebSockets](https://tools.ietf.org/html/rfc7118) è il trasporto ottima, ma è possibile utilizzare altre tecniche quali Server-Sent eventi e di Polling lungo quando questi non sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="e8fda-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="e8fda-130">SignalR verranno automaticamente rilevate e inizializzare il trasporto appropriato in base alle funzionalità supportate nel server e client.</span><span class="sxs-lookup"><span data-stu-id="e8fda-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="e8fda-131">Hub</span><span class="sxs-lookup"><span data-stu-id="e8fda-131">Hubs</span></span>

<span data-ttu-id="e8fda-132">SignalR Usa hub per la comunicazione tra client e server.</span><span class="sxs-lookup"><span data-stu-id="e8fda-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="e8fda-133">Un hub è una pipeline di alto livello che consente il client e server chiamare i metodi a vicenda.</span><span class="sxs-lookup"><span data-stu-id="e8fda-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="e8fda-134">SignalR gestisce l'invio fra computer automaticamente, consentendo ai client di chiamare metodi sul server come facilmente come metodi locali e viceversa.</span><span class="sxs-lookup"><span data-stu-id="e8fda-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="e8fda-135">Hub consentono il passaggio di parametri fortemente tipizzati per metodi, che consente l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="e8fda-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="e8fda-136">SignalR fornisce due protocolli di hub predefinito: un protocollo di testo basati su JSON e un protocollo binario basato sul [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="e8fda-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="e8fda-137">MessagePack crea in genere i messaggi più piccoli rispetto all'uso di JSON.</span><span class="sxs-lookup"><span data-stu-id="e8fda-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="e8fda-138">Devono supportare i browser meno recenti [livello XHR 2](https://caniuse.com/#feat=xhr2) per fornire supporto del protocollo MessagePack.</span><span class="sxs-lookup"><span data-stu-id="e8fda-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="e8fda-139">Hub di chiamare codice lato client mediante l'invio di messaggi mediante il trasporto attivo.</span><span class="sxs-lookup"><span data-stu-id="e8fda-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="e8fda-140">I messaggi contengono il nome e parametri del metodo lato client.</span><span class="sxs-lookup"><span data-stu-id="e8fda-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="e8fda-141">Gli oggetti inviati come parametri di metodo vengono deserializzati tramite il protocollo configurato.</span><span class="sxs-lookup"><span data-stu-id="e8fda-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="e8fda-142">Il client cerca la corrispondenza con il nome a un metodo nel codice lato client.</span><span class="sxs-lookup"><span data-stu-id="e8fda-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="e8fda-143">Quando viene trovata una corrispondenza, viene eseguito il metodo client usando i dati del parametro deserializzato.</span><span class="sxs-lookup"><span data-stu-id="e8fda-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e8fda-144">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e8fda-144">Additional resources</span></span>

* [<span data-ttu-id="e8fda-145">Introduzione a SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8fda-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="e8fda-146">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="e8fda-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="e8fda-147">Hub</span><span class="sxs-lookup"><span data-stu-id="e8fda-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e8fda-148">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="e8fda-148">JavaScript client</span></span>](xref:signalr/javascript-client)
