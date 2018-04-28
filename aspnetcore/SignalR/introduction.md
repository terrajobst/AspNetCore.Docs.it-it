---
title: Introduzione a ASP.NET Core SignalR
author: rachelappel
description: Informazioni su come la libreria di ASP.NET SignalR Core semplifica l'aggiunta di funzionalità in tempo reale alle app.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: 190dfe9eac95be646b458870ac4ee95f681f45d7
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/28/2018
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="f24b8-103">Introduzione a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="f24b8-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="f24b8-104">[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="f24b8-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>


[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-signalr"></a><span data-ttu-id="f24b8-105">Che cos'è SignalR?</span><span class="sxs-lookup"><span data-stu-id="f24b8-105">What is SignalR?</span></span>

<span data-ttu-id="f24b8-106">Componenti di base di ASP.NET SignalR è una libreria che semplifica l'aggiunta di funzionalità per le app web in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="f24b8-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="f24b8-107">Funzionalità web in tempo reale consente immediatamente il codice lato server per il contenuto push ai client.</span><span class="sxs-lookup"><span data-stu-id="f24b8-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="f24b8-108">Buoni candidati per SignalR:</span><span class="sxs-lookup"><span data-stu-id="f24b8-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="f24b8-109">Applicazioni che richiedono aggiornamenti ad alta frequenza dal server.</span><span class="sxs-lookup"><span data-stu-id="f24b8-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="f24b8-110">Esempi sono giochi, social network, voto, asta, mappe e App GPS.</span><span class="sxs-lookup"><span data-stu-id="f24b8-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="f24b8-111">I dashboard e le applicazioni di monitoraggio.</span><span class="sxs-lookup"><span data-stu-id="f24b8-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="f24b8-112">Gli esempi includono dashboard aziendali, gli aggiornamenti di vendita immediati, o si spostano gli avvisi.</span><span class="sxs-lookup"><span data-stu-id="f24b8-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="f24b8-113">Applicazioni di collaborazione.</span><span class="sxs-lookup"><span data-stu-id="f24b8-113">Collaborative apps.</span></span> <span data-ttu-id="f24b8-114">Le applicazioni di lavagna e riunione software del team sono esempi di applicazioni di collaborazione.</span><span class="sxs-lookup"><span data-stu-id="f24b8-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="f24b8-115">Applicazioni che richiedono le notifiche.</span><span class="sxs-lookup"><span data-stu-id="f24b8-115">Apps that require notifications.</span></span> <span data-ttu-id="f24b8-116">Social network, posta elettronica, chat, giochi, gli avvisi di viaggi e molte altre app di usare le notifiche.</span><span class="sxs-lookup"><span data-stu-id="f24b8-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="f24b8-117">SignalR fornisce un'API per la creazione di server a client [chiamate di procedura remota (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="f24b8-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="f24b8-118">Il RPC chiamare le funzioni JavaScript nei client dal codice .NET Core sul lato server.</span><span class="sxs-lookup"><span data-stu-id="f24b8-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="f24b8-119">SignalR per ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f24b8-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="f24b8-120">Gestisce automaticamente la gestione della connessione.</span><span class="sxs-lookup"><span data-stu-id="f24b8-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="f24b8-121">Consente la trasmissione di messaggi a tutti i client connessi contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="f24b8-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="f24b8-122">Ad esempio una chat room.</span><span class="sxs-lookup"><span data-stu-id="f24b8-122">For example, a chat room.</span></span>
* <span data-ttu-id="f24b8-123">Abilita l'invio di messaggi al client specifici o gruppi di client.</span><span class="sxs-lookup"><span data-stu-id="f24b8-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="f24b8-124">È open source in [GitHub](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="f24b8-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="f24b8-125">Scalabile.</span><span class="sxs-lookup"><span data-stu-id="f24b8-125">Scalable.</span></span>

<span data-ttu-id="f24b8-126">La connessione tra il client e server è persistente, a differenza di una connessione HTTP.</span><span class="sxs-lookup"><span data-stu-id="f24b8-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="f24b8-127">Trasporti</span><span class="sxs-lookup"><span data-stu-id="f24b8-127">Transports</span></span>

<span data-ttu-id="f24b8-128">Estrae SignalR su diverse tecniche per la compilazione di applicazioni web in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="f24b8-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="f24b8-129">[WebSocket](https://tools.ietf.org/html/rfc7118) è il trasporto ottimale, ma altre tecniche quali eventi Server-Sent e di Polling lungo possono essere usati quando quelli non sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="f24b8-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="f24b8-130">SignalR verranno automaticamente rilevate e inizializzare il trasporto appropriato in base alle funzionalità supportate nel server e client.</span><span class="sxs-lookup"><span data-stu-id="f24b8-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="f24b8-131">Hub</span><span class="sxs-lookup"><span data-stu-id="f24b8-131">Hubs</span></span>

<span data-ttu-id="f24b8-132">SignalR utilizza hub per le comunicazioni tra client e server.</span><span class="sxs-lookup"><span data-stu-id="f24b8-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="f24b8-133">Un hub è una pipeline di alto livello che consente il client e server chiamare i metodi di reciproca.</span><span class="sxs-lookup"><span data-stu-id="f24b8-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="f24b8-134">SignalR gestisce la distribuzione tra i limiti della macchina automaticamente, consentendo ai client di chiamare metodi sul server come facilmente come metodi locali e viceversa.</span><span class="sxs-lookup"><span data-stu-id="f24b8-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="f24b8-135">Gli hub di consentono il passaggio di parametri fortemente tipizzati per metodi, che consente l'associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="f24b8-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="f24b8-136">SignalR fornisce due protocolli di hub incorporato: un protocollo di testo in base a JSON e un protocollo binario in base a [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="f24b8-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="f24b8-137">In genere, MessagePack crea messaggi più piccoli rispetto all'utilizzo di JSON.</span><span class="sxs-lookup"><span data-stu-id="f24b8-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="f24b8-138">Deve supportare i browser meno recenti [livello XHR 2](https://caniuse.com/#feat=xhr2) per fornire il supporto del protocollo MessagePack.</span><span class="sxs-lookup"><span data-stu-id="f24b8-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="f24b8-139">Gli hub di chiamare il codice sul lato client mediante l'invio di messaggi mediante il trasporto attivo.</span><span class="sxs-lookup"><span data-stu-id="f24b8-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="f24b8-140">I messaggi contengono il nome e i parametri del metodo sul lato client.</span><span class="sxs-lookup"><span data-stu-id="f24b8-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="f24b8-141">Gli oggetti inviati come parametri di metodo vengono deserializzati mediante il protocollo configurato.</span><span class="sxs-lookup"><span data-stu-id="f24b8-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="f24b8-142">Il client tenta di corrispondere al nome a un metodo nel codice sul lato client.</span><span class="sxs-lookup"><span data-stu-id="f24b8-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="f24b8-143">Quando si verifica una corrispondenza, viene eseguito il metodo di client utilizzando i dati del parametro deserializzato.</span><span class="sxs-lookup"><span data-stu-id="f24b8-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f24b8-144">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f24b8-144">Additional resources</span></span>

* [<span data-ttu-id="f24b8-145">Introduzione a SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f24b8-145">Get started with SignalR for ASP.NET Core</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="f24b8-146">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="f24b8-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="f24b8-147">Hub</span><span class="sxs-lookup"><span data-stu-id="f24b8-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="f24b8-148">Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="f24b8-148">JavaScript client</span></span>](xref:signalr/javascript-client)
