---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guida all'API di ASP.NET SignalR Hubs - Server (c#) | Microsoft Docs
author: bradygaster
description: Questo documento viene fornita un'introduzione alla programmazione sul lato server dell'API di hub SignalR ASP.NET per SignalR versione 2, con esempi di codice...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: a28663c8d5c679f85e863e7d0b4523a6f4dd4a1f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837455"
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="c8f62-103">Guida all'API di ASP.NET SignalR Hubs - Server (c#)</span><span class="sxs-lookup"><span data-stu-id="c8f62-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>
====================
<span data-ttu-id="c8f62-104">dal [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c8f62-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="c8f62-105">Questo documento fornisce un'introduzione alla programmazione sul lato server dell'API di hub SignalR ASP.NET per SignalR versione 2, con esempi di codice che illustrano le opzioni comuni.</span><span class="sxs-lookup"><span data-stu-id="c8f62-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="c8f62-106">L'API di hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="c8f62-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="c8f62-107">Nel codice server, si definiscono i metodi che possono essere chiamati da parte dei client e si chiamano i metodi eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="c8f62-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="c8f62-108">Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi che eseguono sul server.</span><span class="sxs-lookup"><span data-stu-id="c8f62-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="c8f62-109">SignalR si occupa di tutte le attività client-server.</span><span class="sxs-lookup"><span data-stu-id="c8f62-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="c8f62-110">SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="c8f62-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="c8f62-111">Per un'introduzione a SignalR hub e connessioni permanenti, vedere [Introduzione a SignalR 2](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="c8f62-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="c8f62-112">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="c8f62-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="c8f62-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c8f62-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="c8f62-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c8f62-114">.NET 4.5</span></span>
> - <span data-ttu-id="c8f62-115">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="c8f62-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="c8f62-116">Versioni di argomento</span><span class="sxs-lookup"><span data-stu-id="c8f62-116">Topic versions</span></span>
> 
> <span data-ttu-id="c8f62-117">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="c8f62-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="c8f62-118">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="c8f62-118">Questions and comments</span></span>
> 
> <span data-ttu-id="c8f62-119">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="c8f62-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="c8f62-120">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="c8f62-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="c8f62-121">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c8f62-121">Overview</span></span>

<span data-ttu-id="c8f62-122">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="c8f62-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="c8f62-123">Come registrare il middleware di SignalR</span><span class="sxs-lookup"><span data-stu-id="c8f62-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="c8f62-124">L'URL /signalr</span><span class="sxs-lookup"><span data-stu-id="c8f62-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="c8f62-125">Configurazione delle opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="c8f62-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="c8f62-126">Come creare e usare classi di Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="c8f62-127">Durata dell'oggetto hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="c8f62-128">Notazione camel le maiuscole/minuscole dei nomi dell'Hub nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c8f62-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="c8f62-129">Più hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="c8f62-130">Hub fortemente tipizzati</span><span class="sxs-lookup"><span data-stu-id="c8f62-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="c8f62-131">Come definire i metodi nella classe Hub che i client possono chiamare</span><span class="sxs-lookup"><span data-stu-id="c8f62-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="c8f62-132">Notazione camel le maiuscole/minuscole dei nomi di metodo nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c8f62-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="c8f62-133">Quando è necessario eseguire in modo asincrono</span><span class="sxs-lookup"><span data-stu-id="c8f62-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="c8f62-134">La definizione di overload</span><span class="sxs-lookup"><span data-stu-id="c8f62-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="c8f62-135">Creazione di report stato di avanzamento di chiamate del metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="c8f62-136">Come chiamare i metodi client dalla classe Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="c8f62-137">Selezionando i client che riceverà la chiamata RPC</span><span class="sxs-lookup"><span data-stu-id="c8f62-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="c8f62-138">Nessuna convalida in fase di compilazione per i nomi di metodo</span><span class="sxs-lookup"><span data-stu-id="c8f62-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="c8f62-139">La corrispondenza dei nomi di metodo tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="c8f62-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="c8f62-140">Esecuzione asincrona</span><span class="sxs-lookup"><span data-stu-id="c8f62-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="c8f62-141">Come gestire l'appartenenza al gruppo dalla classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="c8f62-142">Esecuzione asincrona dei metodi Add e Remove</span><span class="sxs-lookup"><span data-stu-id="c8f62-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="c8f62-143">Persistenza di appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="c8f62-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="c8f62-144">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="c8f62-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="c8f62-145">Come gestire gli eventi di durata connessione nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="c8f62-146">Quando vengono chiamati OnConnected OnDisconnected e OnReconnected</span><span class="sxs-lookup"><span data-stu-id="c8f62-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="c8f62-147">Stato del chiamante non popolato</span><span class="sxs-lookup"><span data-stu-id="c8f62-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="c8f62-148">Come ottenere informazioni relative al client dalla proprietà di contesto</span><span class="sxs-lookup"><span data-stu-id="c8f62-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="c8f62-149">Come passare lo stato tra i client e la classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="c8f62-150">Come gestire gli errori nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="c8f62-151">Come chiamare i metodi di client e gestire i gruppi dall'esterno della classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="c8f62-152">Chiamata dei metodi client</span><span class="sxs-lookup"><span data-stu-id="c8f62-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="c8f62-153">Gestire l'appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="c8f62-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="c8f62-154">Come abilitare la traccia</span><span class="sxs-lookup"><span data-stu-id="c8f62-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="c8f62-155">Come personalizzare la pipeline di hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="c8f62-156">Per informazioni su come i client di programma, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8f62-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="c8f62-157">Guida all'API di SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c8f62-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="c8f62-158">Guida all'API di SignalR Hubs - Client .NET</span><span class="sxs-lookup"><span data-stu-id="c8f62-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="c8f62-159">Sono disponibili in .NET 4.5 solo i componenti server per SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="c8f62-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="c8f62-160">Server che eseguono .NET 4.0 è necessario usare v1.x SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8f62-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="c8f62-161">Come registrare il middleware di SignalR</span><span class="sxs-lookup"><span data-stu-id="c8f62-161">How to register SignalR middleware</span></span>

<span data-ttu-id="c8f62-162">Per definire le route che i client useranno per connettersi all'hub, chiamare il `MapSignalR` metodo all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="c8f62-163">`MapSignalR` è un' [metodo di estensione](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) per il `OwinExtensions` classe.</span><span class="sxs-lookup"><span data-stu-id="c8f62-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="c8f62-164">Nell'esempio seguente viene illustrato come definire le route degli hub SignalR tramite una classe di avvio OWIN.</span><span class="sxs-lookup"><span data-stu-id="c8f62-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="c8f62-165">Se si aggiunge una funzionalità SignalR a un'applicazione ASP.NET MVC, assicurarsi che prima di altre route viene aggiunta la route di SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8f62-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="c8f62-166">Per altre informazioni, vedere [Esercitazione: Introduzione a SignalR 2 e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="c8f62-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="c8f62-167">L'URL /signalr</span><span class="sxs-lookup"><span data-stu-id="c8f62-167">The /signalr URL</span></span>

<span data-ttu-id="c8f62-168">Per impostazione predefinita, l'URL di route che i client useranno per connettersi all'hub è "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="c8f62-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="c8f62-169">(Non confondere questo URL con l'URL "hub signalr /", ovvero per il file JavaScript generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c8f62-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="c8f62-170">Per altre informazioni sul proxy generato, vedere [Guida all'API di SignalR Hubs - JavaScript Client - proxy generato e che cosa fa per te](hubs-api-guide-javascript-client.md#genproxy).)</span><span class="sxs-lookup"><span data-stu-id="c8f62-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="c8f62-171">Potrebbero esserci casi straordinari che rendono questo URL di base non è utilizzabile per SignalR; ad esempio, è presente una cartella nel progetto denominato *signalr* e non si desidera modificare il nome.</span><span class="sxs-lookup"><span data-stu-id="c8f62-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="c8f62-172">In tal caso, è possibile modificare l'URL di base, come illustrato negli esempi seguenti (sostituire "/ signalr" nel codice di esempio con il proprio URL desiderato).</span><span class="sxs-lookup"><span data-stu-id="c8f62-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="c8f62-173">**Codice del server che specifica l'URL**</span><span class="sxs-lookup"><span data-stu-id="c8f62-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="c8f62-174">**Codice JavaScript client che specifica l'URL (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c8f62-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="c8f62-175">**Codice JavaScript client che specifica l'URL (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="c8f62-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="c8f62-176">**Codice client .NET che specifica l'URL**</span><span class="sxs-lookup"><span data-stu-id="c8f62-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="c8f62-177">Configurazione delle opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="c8f62-177">Configuring SignalR Options</span></span>

<span data-ttu-id="c8f62-178">Esegue l'overload del `MapSignalR` metodo consentono di specificare un URL personalizzato, un resolver di dipendenza personalizzate e le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8f62-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="c8f62-179">Abilitare le chiamate tra domini con JSONP o CORS da browser client.</span><span class="sxs-lookup"><span data-stu-id="c8f62-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="c8f62-180">In genere se il browser ha caricato una pagina dalla `http://contoso.com`, la connessione SignalR è nello stesso dominio, in `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="c8f62-181">Se la pagina dalla `http://contoso.com` stabilisce una connessione a `http://fabrikam.com/signalr`, vale a dire una connessione tra domini.</span><span class="sxs-lookup"><span data-stu-id="c8f62-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="c8f62-182">Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c8f62-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="c8f62-183">Per altre informazioni, vedere [Guida all'API di hub di ASP.NET SignalR - JavaScript Client - come stabilire una connessione cross-domain](hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="c8f62-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="c8f62-184">Abilitare i messaggi di errore dettagliato.</span><span class="sxs-lookup"><span data-stu-id="c8f62-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="c8f62-185">Quando si verificano errori, il comportamento predefinito di SignalR consiste nell'inviare ai client un messaggio di notifica senza informazioni dettagliate su cosa è successo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="c8f62-186">L'invio di informazioni dettagliate sull'errore per i client non è consigliabile nell'ambiente di produzione, perché gli utenti malintenzionati potrebbero essere in grado di usare le informazioni per gli attacchi contro la tua applicazione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="c8f62-187">Per risolvere il problema, è possibile utilizzare questa opzione per abilitare temporaneamente la segnalazione di errori maggiori.</span><span class="sxs-lookup"><span data-stu-id="c8f62-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="c8f62-188">Disabilita la funzionalità file proxy JavaScript generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c8f62-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="c8f62-189">Per impostazione predefinita, viene generato un file JavaScript con i proxy per le classi dell'Hub in risposta all'URL "/ signalr/hub".</span><span class="sxs-lookup"><span data-stu-id="c8f62-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="c8f62-190">Se non si vuole usare i proxy di JavaScript, o se si desidera generare manualmente questo file e fare riferimento a un file fisico nel client, è possibile utilizzare questa opzione per disabilitare la generazione del proxy.</span><span class="sxs-lookup"><span data-stu-id="c8f62-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="c8f62-191">Per altre informazioni, vedere [Guida all'API di SignalR Hubs - JavaScript Client - come creare un file fisico per SignalR generato proxy](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="c8f62-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="c8f62-192">Nell'esempio seguente viene illustrato come specificare l'URL di connessione SignalR e queste opzioni in una chiamata al `MapSignalR` (metodo).</span><span class="sxs-lookup"><span data-stu-id="c8f62-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="c8f62-193">Per specificare un URL personalizzato, sostituire "/ signalr" nell'esempio con l'URL a cui si desidera utilizzare.</span><span class="sxs-lookup"><span data-stu-id="c8f62-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="c8f62-194">Come creare e usare classi di Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-194">How to create and use Hub classes</span></span>

<span data-ttu-id="c8f62-195">Per creare un Hub, creare una classe che deriva da [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="c8f62-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="c8f62-196">Nell'esempio seguente viene illustrata una semplice classe di Hub per un'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="c8f62-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="c8f62-197">In questo esempio, è possibile chiamare un client connesso di `NewContosoChatMessage` (metodo), e in questo caso, i dati ricevuti sono sono trasmesse a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="c8f62-198">Durata dell'oggetto hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-198">Hub object lifetime</span></span>

<span data-ttu-id="c8f62-199">Non si crea un'istanza della classe Hub o chiamarne i metodi dal codice del server. tutto ciò che avviene automaticamente tramite la pipeline degli hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8f62-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="c8f62-200">SignalR crea una nuova istanza della classe Hub ogni volta che deve gestire un'operazione dell'Hub, ad esempio quando un client si connette, si disconnette o effettua un chiamata di metodo nel server.</span><span class="sxs-lookup"><span data-stu-id="c8f62-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="c8f62-201">Poiché le istanze della classe Hub sono temporanee, non possono essere utilizzati per mantenere lo stato da una chiamata al metodo a quella successiva.</span><span class="sxs-lookup"><span data-stu-id="c8f62-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="c8f62-202">Ogni volta che il server riceve una chiamata al metodo da un client in una nuova istanza dei processi di classe dell'Hub del messaggio.</span><span class="sxs-lookup"><span data-stu-id="c8f62-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="c8f62-203">Per mantenere lo stato attraverso più connessioni e chiamate al metodo, usare un altro metodo, ad esempio un database o una variabile statica nella classe dell'Hub o un'altra classe non deriva da `Hub`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="c8f62-204">Se è rendere persistenti i dati in memoria, usando un metodo, ad esempio una variabile statica della classe dell'Hub, i dati andranno perse quando viene riciclato il dominio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="c8f62-205">Se si desidera inviare messaggi ai client dal codice che viene eseguito all'esterno della classe dell'Hub, non è possibile farlo creando un'istanza della classe dell'Hub, ma è possibile farlo tramite il recupero di un riferimento all'oggetto di contesto SignalR per la classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="c8f62-206">Per altre informazioni, vedere [come client di chiamare metodi e gestire i gruppi dall'esterno della classe dell'Hub](#callfromoutsidehub) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="c8f62-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="c8f62-207">Notazione camel le maiuscole/minuscole dei nomi dell'Hub nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c8f62-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="c8f62-208">Per impostazione predefinita, i client JavaScript fare riferimento a un hub usando una versione con notazione camel del nome della classe.</span><span class="sxs-lookup"><span data-stu-id="c8f62-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="c8f62-209">SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c8f62-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="c8f62-210">Nell'esempio precedente potrebbe essere indicata come `contosoChatHub` nel codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c8f62-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="c8f62-211">**Server**</span><span class="sxs-lookup"><span data-stu-id="c8f62-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="c8f62-212">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="c8f62-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="c8f62-213">Se si desidera specificare un nome diverso per i client da usare, aggiungere il `HubName` attributo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="c8f62-214">Quando si usa un `HubName` attributo, non vi è alcuna modifica del nome in notazione camel nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c8f62-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="c8f62-215">**Server**</span><span class="sxs-lookup"><span data-stu-id="c8f62-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="c8f62-216">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="c8f62-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="c8f62-217">Più hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-217">Multiple Hubs</span></span>

<span data-ttu-id="c8f62-218">È possibile definire più classi Hub in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="c8f62-219">Quando si esegue questa operazione, la connessione è condivisa ma i gruppi sono separati:</span><span class="sxs-lookup"><span data-stu-id="c8f62-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="c8f62-220">Tutti i client utilizzeranno lo stesso URL per stabilire una connessione di SignalR con il servizio ("/ signalr" o l'URL personalizzato se è stata specificata una), e viene utilizzata per tutti gli hub di connessione definite dal servizio.</span><span class="sxs-lookup"><span data-stu-id="c8f62-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="c8f62-221">Non vi è alcuna differenza nelle prestazioni per più hub rispetto alla definizione di tutte le funzionalità dell'Hub in una singola classe.</span><span class="sxs-lookup"><span data-stu-id="c8f62-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="c8f62-222">Tutti gli hub di ottenere le stesse informazioni di richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c8f62-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="c8f62-223">Poiché tutti gli hub condividono la stessa connessione, l'unica informazione richiesta HTTP che il server ottiene è ciò che viene ricevuta la richiesta HTTP originale che stabilisce la connessione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8f62-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="c8f62-224">Se si usa la richiesta di connessione per passare le informazioni dal client al server, specificando una stringa di query, non è possibile fornire le stringhe di query diversi hub diversi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="c8f62-225">Tutti gli hub riceverà le stesse informazioni.</span><span class="sxs-lookup"><span data-stu-id="c8f62-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="c8f62-226">Il file proxy JavaScript generato conterrà i proxy per tutti gli hub in un unico file.</span><span class="sxs-lookup"><span data-stu-id="c8f62-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="c8f62-227">Per informazioni sui proxy JavaScript, vedere [Guida all'API di SignalR Hubs - JavaScript Client - proxy generato e che cosa fa per te](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="c8f62-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="c8f62-228">I gruppi vengono definiti all'interno di hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="c8f62-229">In SignalR che è possibile definire gruppi per la trasmissione a subset di client connessi con nome.</span><span class="sxs-lookup"><span data-stu-id="c8f62-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="c8f62-230">I gruppi vengono mantenuti separatamente per ogni Hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="c8f62-231">Ad esempio, un gruppo denominato "Administrators" sono un set di client per il `ContosoChatHub` classi e lo stesso nome di gruppo per fare riferimento a un set diverso di client per il `StockTickerHub` classe.</span><span class="sxs-lookup"><span data-stu-id="c8f62-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="c8f62-232">Hub fortemente tipizzati</span><span class="sxs-lookup"><span data-stu-id="c8f62-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="c8f62-233">Per definire un'interfaccia per i metodi dell'hub che il client può essere riferimento e Abilita Intellisense sui metodi dell'hub, derivare hub indicato nel `Hub<T>` (introdotta in SignalR 2.1) anziché `Hub`:</span><span class="sxs-lookup"><span data-stu-id="c8f62-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="c8f62-234">Come definire i metodi nella classe Hub che i client possono chiamare</span><span class="sxs-lookup"><span data-stu-id="c8f62-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="c8f62-235">Per esporre un metodo dell'hub che si desidera essere richiamabile dal client, dichiarare un metodo pubblico, come illustrato negli esempi seguenti.</span><span class="sxs-lookup"><span data-stu-id="c8f62-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="c8f62-236">È possibile specificare un tipo restituito e parametri, compresi i tipi complessi e le matrici, come si farebbe in qualsiasi metodo c#.</span><span class="sxs-lookup"><span data-stu-id="c8f62-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="c8f62-237">Tutti i dati ricevuti nei parametri o restituire al chiamante vengono comunicati tra il client e server mediante JSON e SignalR gestisce automaticamente l'associazione di oggetti complessi e le matrici di oggetti.</span><span class="sxs-lookup"><span data-stu-id="c8f62-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="c8f62-238">Notazione camel le maiuscole/minuscole dei nomi di metodo nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c8f62-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="c8f62-239">Per impostazione predefinita, i client JavaScript fare riferimento ai metodi dell'Hub usando una versione con notazione camel del nome del metodo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="c8f62-240">SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c8f62-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="c8f62-241">**Server**</span><span class="sxs-lookup"><span data-stu-id="c8f62-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="c8f62-242">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="c8f62-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="c8f62-243">Se si desidera specificare un nome diverso per i client da usare, aggiungere il `HubMethodName` attributo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="c8f62-244">**Server**</span><span class="sxs-lookup"><span data-stu-id="c8f62-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="c8f62-245">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="c8f62-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="c8f62-246">Quando è necessario eseguire in modo asincrono</span><span class="sxs-lookup"><span data-stu-id="c8f62-246">When to execute asynchronously</span></span>

<span data-ttu-id="c8f62-247">Se il metodo verrà essere a esecuzione prolungata o è necessario eseguire le operazioni che verrebbe comportano l'attesa, ad esempio una ricerca nel database o una chiamata al servizio web, rendere asincrono il metodo dell'Hub, restituendo un [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (invece di `void` restituire) o [ Task&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) oggetto (invece di `T` tipo restituito).</span><span class="sxs-lookup"><span data-stu-id="c8f62-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="c8f62-248">Quando si torna una `Task` oggetto dal metodo, SignalR attende la `Task` completamento, e quindi invia il risultato annullato il wrapping al client, pertanto non presenta alcuna differenza nel modo in cui è codice la chiamata al metodo nel client.</span><span class="sxs-lookup"><span data-stu-id="c8f62-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="c8f62-249">Rendere un metodo dell'Hub asincrona consente di evitare blocchi la connessione quando utilizza il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="c8f62-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="c8f62-250">Quando un metodo dell'Hub esegue in modo sincrono e il trasporto WebSocket, le successive chiamate dei metodi dell'hub dallo stesso client vengono bloccate fino al completamento del metodo dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="c8f62-251">L'esempio seguente illustra il metodo di stesso codificate in modo da eseguire in modo sincrono o asincrono, seguito da codice JavaScript client che funziona per la chiamata a entrambe le versioni.</span><span class="sxs-lookup"><span data-stu-id="c8f62-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="c8f62-252">**Sincrono**</span><span class="sxs-lookup"><span data-stu-id="c8f62-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="c8f62-253">**Asincrono**</span><span class="sxs-lookup"><span data-stu-id="c8f62-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="c8f62-254">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="c8f62-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="c8f62-255">Per altre informazioni su come usare i metodi asincroni in ASP.NET 4.5, vedere [usando i metodi asincroni in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="c8f62-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="c8f62-256">La definizione di overload</span><span class="sxs-lookup"><span data-stu-id="c8f62-256">Defining Overloads</span></span>

<span data-ttu-id="c8f62-257">Se si desidera definire gli overload per un metodo, il numero di parametri in ogni overload deve essere diverso.</span><span class="sxs-lookup"><span data-stu-id="c8f62-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="c8f62-258">Se un overload si differenzia solo specificando i tipi di parametro diversi, la classe dell'Hub verrà compilate, ma il servizio SignalR genererà un'eccezione in fase di esecuzione quando i client di provano a chiamare uno degli overload.</span><span class="sxs-lookup"><span data-stu-id="c8f62-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="c8f62-259">Creazione di report stato di avanzamento di chiamate del metodo dell'hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="c8f62-260">2.1 SignalR aggiunge il supporto per la [modello di report di stato](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introdotta in .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="c8f62-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="c8f62-261">Per implementare i report di stato di avanzamento, definire un `IProgress<T>` parametro per il metodo dell'hub che il client può accedere a:</span><span class="sxs-lookup"><span data-stu-id="c8f62-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="c8f62-262">Quando si scrive un metodo di server con esecuzione prolungata, è importante usare un modello di programmazione asincrona, ad esempio Async / Await anziché bloccare il thread dell'hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="c8f62-263">Come chiamare i metodi client dalla classe Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="c8f62-264">Per chiamare i metodi di client dal server, usare il `Clients` proprietà in un metodo nella classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="c8f62-265">L'esempio seguente illustra codice lato server che chiama `addNewMessageToPage` sui client connesse tutte le soluzioni e codice client che definisce il metodo in un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c8f62-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="c8f62-266">**Server**</span><span class="sxs-lookup"><span data-stu-id="c8f62-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="c8f62-267">Richiama un metodo client è un'operazione asincrona e restituisce un `Task`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-267">Invoking a client method is an asynchronous operation and returns a `Task`.</span></span> <span data-ttu-id="c8f62-268">Usare `await`:</span><span class="sxs-lookup"><span data-stu-id="c8f62-268">Use `await`:</span></span>

* <span data-ttu-id="c8f62-269">Per verificare che il messaggio viene inviato senza errori.</span><span class="sxs-lookup"><span data-stu-id="c8f62-269">To ensure the message is sent without error.</span></span> 
* <span data-ttu-id="c8f62-270">Per abilitare l'intercettazione e gestione degli errori in un blocco try-catch.</span><span class="sxs-lookup"><span data-stu-id="c8f62-270">To enable catching and handling errors in a try-catch block.</span></span>

<span data-ttu-id="c8f62-271">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="c8f62-271">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="c8f62-272">È possibile ottenere un valore restituito da un metodo client. la sintassi, ad esempio `int x = Clients.All.add(1,1)` non funziona.</span><span class="sxs-lookup"><span data-stu-id="c8f62-272">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="c8f62-273">È possibile specificare i tipi complessi e matrici per i parametri.</span><span class="sxs-lookup"><span data-stu-id="c8f62-273">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="c8f62-274">Nell'esempio seguente passa un tipo complesso al client in un parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-274">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="c8f62-275">**Codice del server che chiama un metodo client usando un oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="c8f62-275">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="c8f62-276">**Codice del server che definisce l'oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="c8f62-276">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="c8f62-277">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="c8f62-277">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="c8f62-278">Selezionando i client che riceverà la chiamata RPC</span><span class="sxs-lookup"><span data-stu-id="c8f62-278">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="c8f62-279">La proprietà restituisce ai client un [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) oggetto che fornisce diverse opzioni per specificare che i client riceveranno il RPC:</span><span class="sxs-lookup"><span data-stu-id="c8f62-279">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="c8f62-280">Tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-280">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="c8f62-281">Solo il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="c8f62-281">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="c8f62-282">Tutti i client, eccetto il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="c8f62-282">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="c8f62-283">Un client specifico identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-283">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="c8f62-284">Questo esempio viene chiamato `addContosoChatMessageToPage` sul client chiamante e ha lo stesso effetto dell'utilizzo `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-284">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="c8f62-285">Tutti i client connessi eccetto il client specificato, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-285">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="c8f62-286">Tutti i client in un gruppo specificato connessi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-286">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="c8f62-287">Tutti i client connessi in un gruppo specificato eccetto il client specificato, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-287">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="c8f62-288">Tutti i client connessi in un gruppo specificato eccetto il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="c8f62-288">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="c8f62-289">Un utente specifico, identificato dall'ID utente.</span><span class="sxs-lookup"><span data-stu-id="c8f62-289">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="c8f62-290">Per impostazione predefinita, si tratta `IPrincipal.Identity.Name`, ma può essere modificato dal [la registrazione di un'implementazione di IUserIdProvider con l'host globale](mapping-users-to-connections.md#IUserIdProvider).</span><span class="sxs-lookup"><span data-stu-id="c8f62-290">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="c8f62-291">Tutti i client e i gruppi in un elenco di ID connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-291">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="c8f62-292">Un elenco di gruppi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-292">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="c8f62-293">Un utente in base al nome.</span><span class="sxs-lookup"><span data-stu-id="c8f62-293">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="c8f62-294">Un elenco di nomi utente (introdotta in SignalR 2.1).</span><span class="sxs-lookup"><span data-stu-id="c8f62-294">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="c8f62-295">Nessuna convalida in fase di compilazione per i nomi di metodo</span><span class="sxs-lookup"><span data-stu-id="c8f62-295">No compile-time validation for method names</span></span>

<span data-ttu-id="c8f62-296">Il nome del metodo specificato viene interpretato come un oggetto dinamico, ovvero che non IntelliSense o convalida in fase di compilazione appositamente.</span><span class="sxs-lookup"><span data-stu-id="c8f62-296">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="c8f62-297">L'espressione viene valutata in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-297">The expression is evaluated at run time.</span></span> <span data-ttu-id="c8f62-298">Quando viene eseguita la chiamata al metodo, SignalR invia il nome del metodo e i valori dei parametri per il client, e se il client dispone di un metodo che corrisponde al nome, la chiamata a metodo e i valori dei parametri vengono passati a esso.</span><span class="sxs-lookup"><span data-stu-id="c8f62-298">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="c8f62-299">Se non viene trovato alcun metodo corrispondente nel client, viene generato alcun errore.</span><span class="sxs-lookup"><span data-stu-id="c8f62-299">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="c8f62-300">Per informazioni sul formato dei dati che SignalR trasmette al client dietro le quinte quando si chiama un metodo client, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="c8f62-300">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="c8f62-301">La corrispondenza dei nomi di metodo tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="c8f62-301">Case-insensitive method name matching</span></span>

<span data-ttu-id="c8f62-302">Metodo nome viene applicata alcuna distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="c8f62-302">Method name matching is case-insensitive.</span></span> <span data-ttu-id="c8f62-303">Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, o `addContosoChatMessageToPage` sul client.</span><span class="sxs-lookup"><span data-stu-id="c8f62-303">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="c8f62-304">Esecuzione asincrona</span><span class="sxs-lookup"><span data-stu-id="c8f62-304">Asynchronous execution</span></span>

<span data-ttu-id="c8f62-305">Il metodo chiamato viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="c8f62-305">The method that you call executes asynchronously.</span></span> <span data-ttu-id="c8f62-306">Qualsiasi codice che segue una chiamata al metodo a un client verrà eseguita immediatamente senza attendere SignalR completare la trasmissione dei dati per i client a meno che non si specifica che le successive righe di codice deve attendere il completamento di metodo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-306">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="c8f62-307">Esempio di codice seguente viene illustrato come eseguire i due metodi di client in modo sequenziale.</span><span class="sxs-lookup"><span data-stu-id="c8f62-307">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="c8f62-308">**Si attende (.NET 4.5)**</span><span class="sxs-lookup"><span data-stu-id="c8f62-308">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="c8f62-309">Se si usa `await` per attendere fino al completamento di un metodo client prima dell'esecuzione riga di codice successiva, ciò non significa che i client effettivamente riceverà il messaggio prima dell'esecuzione riga di codice successiva.</span><span class="sxs-lookup"><span data-stu-id="c8f62-309">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="c8f62-310">"Completamento" di una chiamata al metodo client significa solo che ha eseguito tutto il necessario per inviare il messaggio SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8f62-310">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="c8f62-311">Se è necessario che i client ha ricevuto il messaggio di verifica, è necessario programmare tale meccanismo manualmente.</span><span class="sxs-lookup"><span data-stu-id="c8f62-311">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="c8f62-312">Ad esempio, è possibile codificare una `MessageReceived` metodo dell'hub e nel `addContosoChatMessageToPage` metodo sul client è possibile chiamare `MessageReceived` al termine dell'operazione è qualsiasi elemento di lavoro da eseguire sul client.</span><span class="sxs-lookup"><span data-stu-id="c8f62-312">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="c8f62-313">In `MessageReceived` nell'Hub è possibile eseguire le operazioni dipende da client effettiva ricezione e l'elaborazione della chiamata al metodo originale.</span><span class="sxs-lookup"><span data-stu-id="c8f62-313">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="c8f62-314">Come usare una variabile di stringa come il nome del metodo</span><span class="sxs-lookup"><span data-stu-id="c8f62-314">How to use a string variable as the method name</span></span>

<span data-ttu-id="c8f62-315">Se si vuole richiamare un metodo client utilizzando una variabile di stringa come il nome del metodo, eseguire il cast `Clients.All` (o `Clients.Others`, `Clients.Caller`e così via) per `IClientProxy` e quindi chiamare [Invoke (methodName, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="c8f62-315">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="c8f62-316">Come gestire l'appartenenza al gruppo dalla classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-316">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="c8f62-317">Gruppi in SignalR forniscono un metodo per la trasmissione di messaggi per sottoinsiemi specifici di client connessi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-317">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="c8f62-318">Un gruppo può avere qualsiasi numero di client e un client può essere un membro di un numero qualsiasi di gruppi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-318">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="c8f62-319">Per gestire l'appartenenza al gruppo, usare il [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) e [rimuovere](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metodi forniti dal `Groups` proprietà della classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-319">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="c8f62-320">L'esempio seguente mostra le `Groups.Add` e `Groups.Remove` metodi usati nei metodi dell'Hub che vengono chiamati dal codice client, seguito da codice JavaScript client che li chiama.</span><span class="sxs-lookup"><span data-stu-id="c8f62-320">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="c8f62-321">**Server**</span><span class="sxs-lookup"><span data-stu-id="c8f62-321">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="c8f62-322">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="c8f62-322">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="c8f62-323">Non è necessario creare in modo esplicito gruppi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-323">You don't have to explicitly create groups.</span></span> <span data-ttu-id="c8f62-324">In effetti un gruppo viene creato automaticamente la prima volta specificarne il nome in una chiamata a `Groups.Add`, e viene eliminata quando si rimuove l'ultima connessione dall'appartenenza al suo interno.</span><span class="sxs-lookup"><span data-stu-id="c8f62-324">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="c8f62-325">Non è disponibile alcuna API per ottenere un elenco di appartenenza al gruppo o un elenco di gruppi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-325">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="c8f62-326">SignalR invia messaggi al client e i gruppi basati su un [modello pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), e il server non venga mantenuto un elenco di gruppi o appartenenze ai gruppi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-326">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="c8f62-327">Ciò consente di ottimizzare la scalabilità, perché ogni volta che si aggiunge un nodo a una web farm, qualsiasi stato che gestisce SignalR deve essere propagata nel nuovo nodo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-327">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="c8f62-328">Esecuzione asincrona dei metodi Add e Remove</span><span class="sxs-lookup"><span data-stu-id="c8f62-328">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="c8f62-329">Il `Groups.Add` e `Groups.Remove` metodi eseguiti in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="c8f62-329">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="c8f62-330">Se si desidera aggiungere un client a un gruppo e inviare immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il `Groups.Add` metodo termina per prima.</span><span class="sxs-lookup"><span data-stu-id="c8f62-330">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="c8f62-331">Esempio di codice seguente viene illustrato come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-331">The following code example shows how to do that.</span></span>

<span data-ttu-id="c8f62-332">**Aggiunta di un client a un gruppo e quindi il client di messaggistica**</span><span class="sxs-lookup"><span data-stu-id="c8f62-332">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="c8f62-333">Persistenza di appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="c8f62-333">Group membership persistence</span></span>

<span data-ttu-id="c8f62-334">SignalR tiene traccia delle connessioni, non per gli utenti, pertanto se si desidera che un utente sia nello stesso gruppo ogni volta che l'utente definisce una connessione, è necessario chiamare `Groups.Add` ogni volta che l'utente definisce una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-334">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="c8f62-335">Dopo una perdita temporanea della connettività, in alcuni casi SignalR possibile ripristinare la connessione automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c8f62-335">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="c8f62-336">In tal caso, SignalR sta ripristinando la stessa connessione, non stabilire una nuova connessione e quindi, vengono ripristinato automaticamente l'appartenenza al gruppo del client.</span><span class="sxs-lookup"><span data-stu-id="c8f62-336">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="c8f62-337">Questo è possibile anche quando l'interruzione temporanea è il risultato di un riavvio del server o di errore, perché lo stato della connessione per ogni client, incluse le appartenenze di gruppo, viene sottoposto a round trip al client.</span><span class="sxs-lookup"><span data-stu-id="c8f62-337">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="c8f62-338">Se un server si arresta e viene sostituito da un nuovo server prima del timeout della connessione, un client possa riconnettersi automaticamente al nuovo server e nuovamente la registrazione è un membro dei gruppi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-338">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="c8f62-339">Quando una connessione non può essere ripristinata automaticamente dopo una perdita di connettività, o quando si verifica il timeout della connessione o quando il client si disconnette (ad esempio, quando un browser consente di passare a una nuova pagina), le appartenenze di gruppo vengono perse.</span><span class="sxs-lookup"><span data-stu-id="c8f62-339">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="c8f62-340">Al successivo si connette l'utente sarà una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-340">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="c8f62-341">Per mantenere appartenenze quando lo stesso utente stabilisce una nuova connessione, l'applicazione deve rilevare le associazioni tra utenti e gruppi e ripristinare le appartenenze di gruppo ogni volta che un utente stabilisce una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-341">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="c8f62-342">Per altre informazioni sulle connessioni e le riconnessioni, vedere [come gestire gli eventi di durata connessione nella classe Hub](#connectionlifetime) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="c8f62-342">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="c8f62-343">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="c8f62-343">Single-user groups</span></span>

<span data-ttu-id="c8f62-344">Le applicazioni che utilizzano in genere SignalR devono tenere traccia delle associazioni tra utenti e connessioni per sapere quale utente ha inviato un messaggio e che gli utenti dovrebbero ricevere un messaggio.</span><span class="sxs-lookup"><span data-stu-id="c8f62-344">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="c8f62-345">Gruppi vengono usati in uno dei due modelli di usati comune per questo scopo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-345">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="c8f62-346">Gruppi utente singolo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-346">Single-user groups.</span></span>

    <span data-ttu-id="c8f62-347">È possibile specificare il nome utente come nome del gruppo e aggiungere l'ID di connessione corrente al gruppo ogni volta che l'utente si connette o si riconnette.</span><span class="sxs-lookup"><span data-stu-id="c8f62-347">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="c8f62-348">Per inviare messaggi all'utente di trasmissione al gruppo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-348">To send messages to the user you send to the group.</span></span> <span data-ttu-id="c8f62-349">Uno svantaggio di questo metodo è che il gruppo non offre un modo per verificare se l'utente è online o offline.</span><span class="sxs-lookup"><span data-stu-id="c8f62-349">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="c8f62-350">Rilevare le associazioni tra i nomi utente e ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-350">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="c8f62-351">È possibile archiviare un'associazione tra ogni nome utente e ID di connessione una o più in un dizionario o un database e aggiornare i dati archiviati ogni volta che l'utente si connette o disconnette.</span><span class="sxs-lookup"><span data-stu-id="c8f62-351">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="c8f62-352">Per inviare messaggi all'utente specificare l'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-352">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="c8f62-353">Uno svantaggio di questo metodo è che sono necessari più memoria.</span><span class="sxs-lookup"><span data-stu-id="c8f62-353">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="c8f62-354">Come gestire gli eventi di durata connessione nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-354">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="c8f62-355">Motivi tipici per la gestione degli eventi di durata connessione sono per consentire di controllare se un utente è connesso o meno e di tenere traccia dell'associazione tra i nomi utente e ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-355">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="c8f62-356">Per eseguire codice personalizzato quando i client di connettere o disconnettere, eseguire l'override di `OnConnected`, `OnDisconnected`, e `OnReconnected` classe i metodi virtuali dell'Hub, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="c8f62-356">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="c8f62-357">Quando vengono chiamati OnConnected OnDisconnected e OnReconnected</span><span class="sxs-lookup"><span data-stu-id="c8f62-357">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="c8f62-358">Ogni volta che un browser consente di passare a una nuova pagina, una nuova connessione deve essere stabilita, ovvero SignalR eseguirà il `OnDisconnected` metodo aggiungendo il `OnConnected` (metodo).</span><span class="sxs-lookup"><span data-stu-id="c8f62-358">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="c8f62-359">SignalR crea sempre un nuovo ID di connessione quando viene stabilita una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-359">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="c8f62-360">Il `OnReconnected` metodo viene chiamato quando si verifica un'interruzione temporanea della connettività che SignalR può automaticamente ripristinare, ad esempio quando un cavo è temporaneamente disconnesso e riconnesso prima del timeout della connessione. Il `OnDisconnected` metodo viene chiamato quando il client viene disconnesso e SignalR non è possibile riconnettersi automaticamente, ad esempio quando un browser passa a una nuova pagina.</span><span class="sxs-lookup"><span data-stu-id="c8f62-360">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="c8f62-361">Pertanto, è una sequenza di eventi per un determinato client possibili `OnConnected`, `OnReconnected`, `OnDisconnected`; oppure `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-361">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="c8f62-362">Non verrà visualizzata la sequenza `OnConnected`, `OnDisconnected`, `OnReconnected` per una determinata connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-362">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="c8f62-363">Il `OnDisconnected` il dominio dell'applicazione ottiene riciclato o metodo non può essere chiamato in alcuni scenari, ad esempio quando un server si arresta.</span><span class="sxs-lookup"><span data-stu-id="c8f62-363">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="c8f62-364">Quando un altro server è online o il dominio dell'applicazione completato il riciclo, alcuni client potrebbero essere in grado di ristabilire la connessione e vengono attivati i `OnReconnected` evento.</span><span class="sxs-lookup"><span data-stu-id="c8f62-364">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="c8f62-365">Per altre informazioni, vedere [comprensione e la gestione degli eventi di durata connessione in SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="c8f62-365">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="c8f62-366">Stato del chiamante non popolato</span><span class="sxs-lookup"><span data-stu-id="c8f62-366">Caller state not populated</span></span>

<span data-ttu-id="c8f62-367">Vengono chiamati i metodi del gestore eventi Durata connessione dal server, il che significa che qualsiasi stato che si inserisce nel `state` oggetti nel client non verranno popolati nel `Caller` proprietà sul server.</span><span class="sxs-lookup"><span data-stu-id="c8f62-367">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="c8f62-368">Per informazioni sul `state` oggetto e il `Caller` proprietà, vedere [come passare lo stato tra i client e la classe Hub](#passstate) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="c8f62-368">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="c8f62-369">Come ottenere informazioni relative al client dalla proprietà di contesto</span><span class="sxs-lookup"><span data-stu-id="c8f62-369">How to get information about the client from the Context property</span></span>

<span data-ttu-id="c8f62-370">Per ottenere informazioni sul client, usare il `Context` proprietà della classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-370">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="c8f62-371">Il `Context` proprietà restituisce un [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) oggetto che fornisce l'accesso alle informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8f62-371">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="c8f62-372">L'ID di connessione del client chiamante.</span><span class="sxs-lookup"><span data-stu-id="c8f62-372">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="c8f62-373">L'ID di connessione è un GUID assegnato da SignalR (è possibile specificare il valore nel codice).</span><span class="sxs-lookup"><span data-stu-id="c8f62-373">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="c8f62-374">È un ID di connessione per ogni connessione e la stessa connessione che ID viene utilizzato da tutti gli hub, se si dispone di più hub nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-374">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="c8f62-375">Dati dell'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="c8f62-375">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="c8f62-376">È anche possibile ottenere le intestazioni HTTP dalla `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-376">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="c8f62-377">Il motivo più riferimenti allo stesso concetto è che `Context.Headers` è stato creato prima di tutto la `Context.Request` è stata aggiunta in un secondo momento, proprietà e `Context.Headers` è stata mantenuta per compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="c8f62-377">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="c8f62-378">Eseguire query sui dati di tipo stringa.</span><span class="sxs-lookup"><span data-stu-id="c8f62-378">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="c8f62-379">È anche possibile ottenere i dati di stringa di query da `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-379">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="c8f62-380">La stringa di query che si ottiene questa proprietà è quello che è stato usato con la richiesta HTTP che la connessione SignalR stabilita.</span><span class="sxs-lookup"><span data-stu-id="c8f62-380">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="c8f62-381">È possibile aggiungere parametri della stringa di query nel client tramite la configurazione della connessione, che è un modo pratico per passare i dati relativi ai client dal client al server.</span><span class="sxs-lookup"><span data-stu-id="c8f62-381">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="c8f62-382">Nell'esempio seguente viene illustrato un modo per aggiungere una stringa di query in un client JavaScript quando si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="c8f62-382">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="c8f62-383">Per altre informazioni sull'impostazione di parametri della stringa di query, vedere le guide di API per il [JavaScript](hubs-api-guide-javascript-client.md) e [.NET](hubs-api-guide-net-client.md) client.</span><span class="sxs-lookup"><span data-stu-id="c8f62-383">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="c8f62-384">È possibile trovare il metodo di trasporto utilizzato per la connessione dei dati di stringa di query, con alcuni altri valori utilizzati internamente da SignalR:</span><span class="sxs-lookup"><span data-stu-id="c8f62-384">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="c8f62-385">Il valore di `transportMethod` sarà "WebSocket", "serverSentEvents", "foreverFrame" o "longPolling".</span><span class="sxs-lookup"><span data-stu-id="c8f62-385">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="c8f62-386">Si noti che se si archivia il valore di `OnConnected` metodo del gestore eventi, in alcuni scenari è inizialmente potrebbe ottenere un valore di trasporto che non sia il metodo di trasporto negoziata finale per la connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-386">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="c8f62-387">In tal caso il metodo genererà un'eccezione e verrà chiamato nuovamente in un secondo momento quando il metodo di trasporto finale viene stabilito.</span><span class="sxs-lookup"><span data-stu-id="c8f62-387">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="c8f62-388">Cookie.</span><span class="sxs-lookup"><span data-stu-id="c8f62-388">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="c8f62-389">È anche possibile ottenere i cookie provenienti da `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-389">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="c8f62-390">Informazioni sull'utente.</span><span class="sxs-lookup"><span data-stu-id="c8f62-390">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="c8f62-391">L'oggetto HttpContext per la richiesta:</span><span class="sxs-lookup"><span data-stu-id="c8f62-391">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="c8f62-392">Usare questo metodo anziché ottenere `HttpContext.Current` per ottenere il `HttpContext` oggetto per la connessione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="c8f62-392">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="c8f62-393">Come passare lo stato tra i client e la classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-393">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="c8f62-394">Il proxy client fornisce un `state` oggetto in cui è possibile archiviare i dati che si desidera essere trasmessi al server con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-394">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="c8f62-395">Nel server è possibile accedere a questi dati nel `Clients.Caller` proprietà nei metodi dell'Hub che vengono chiamati dal client.</span><span class="sxs-lookup"><span data-stu-id="c8f62-395">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="c8f62-396">Il `Clients.Caller` proprietà non viene popolata per i metodi del gestore eventi Durata connessione `OnConnected`, `OnDisconnected`, e `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-396">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="c8f62-397">La creazione o aggiornamento dei dati nel `state` oggetto e il `Clients.Caller` proprietà funziona in entrambe le direzioni.</span><span class="sxs-lookup"><span data-stu-id="c8f62-397">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="c8f62-398">È possibile aggiornare i valori nel server e vengono passati al client.</span><span class="sxs-lookup"><span data-stu-id="c8f62-398">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="c8f62-399">Nell'esempio seguente viene illustrato il codice client JavaScript che archivia lo stato per la trasmissione al server con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-399">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="c8f62-400">Nell'esempio seguente mostra il codice equivalente in un client .NET.</span><span class="sxs-lookup"><span data-stu-id="c8f62-400">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="c8f62-401">Nella classe Hub, è possibile accedere a questi dati nel `Clients.Caller` proprietà.</span><span class="sxs-lookup"><span data-stu-id="c8f62-401">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="c8f62-402">Nell'esempio seguente viene illustrato il codice che recupera lo stato definito nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="c8f62-402">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="c8f62-403">Questo meccanismo di persistenza dello stato non è progettato per grandi quantità di dati, poiché tutto ciò che si inserisce nel `state` o `Clients.Caller` proprietà è sottoposto a round trip con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-403">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="c8f62-404">È utile per gli elementi più piccoli, ad esempio nomi utente o i contatori.</span><span class="sxs-lookup"><span data-stu-id="c8f62-404">It's useful for smaller items such as user names or counters.</span></span>


<span data-ttu-id="c8f62-405">In Visual Basic.NET o in un hub fortemente tipizzato, l'oggetto di stato del chiamante non sono accessibili attraverso `Clients.Caller`; in alternativa, usare `Clients.CallerState` (introdotta in SignalR 2.1):</span><span class="sxs-lookup"><span data-stu-id="c8f62-405">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="c8f62-406">**Utilizzo CallerState in c#**</span><span class="sxs-lookup"><span data-stu-id="c8f62-406">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="c8f62-407">**Utilizzo CallerState in Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="c8f62-407">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="c8f62-408">Come gestire gli errori nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-408">How to handle errors in the Hub class</span></span>

<span data-ttu-id="c8f62-409">Per gestire gli errori che si verificano nei metodi di classe dell'Hub, prima di tutto assicurarsi "osservare" tutte le eccezioni dalle operazioni asincrone (ad esempio, richiamando metodi di client) tramite `await`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-409">To handle errors that occur in your Hub class methods, first ensure you "observe" any exceptions from async operations (such as invoking client methods) by using `await`.</span></span> <span data-ttu-id="c8f62-410">Usare quindi uno o più dei metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8f62-410">Then use one or more of the following methods:</span></span>

- <span data-ttu-id="c8f62-411">Eseguire il wrapping del codice del metodo in blocchi try-catch e accedere all'oggetto eccezione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-411">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="c8f62-412">A scopo di debug è possibile inviare l'eccezione al client, ma per la sicurezza non sono consigliabile motivi l'invio di informazioni dettagliate per i client nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-412">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="c8f62-413">Creare un modulo di hub della pipeline che gestisce il [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) (metodo).</span><span class="sxs-lookup"><span data-stu-id="c8f62-413">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="c8f62-414">Nell'esempio seguente mostra un modulo di pipeline che registra gli errori, seguiti dal codice nel file Startup.cs che inserisce il modulo nella pipeline di hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-414">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="c8f62-415">Usare il `HubException` classe (introdotta in SignalR 2).</span><span class="sxs-lookup"><span data-stu-id="c8f62-415">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="c8f62-416">Questo errore può essere generato da qualsiasi chiamata dell'hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-416">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="c8f62-417">Il `HubError` costruttore accetta una stringa di messaggio e un oggetto per l'archiviazione dei dati aggiuntivi errore.</span><span class="sxs-lookup"><span data-stu-id="c8f62-417">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="c8f62-418">SignalR verrà auto-serializzare l'eccezione e inviarlo al client, in cui utilizzarlo per rifiutare o interrompere la chiamata al metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-418">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="c8f62-419">Gli esempi di codice seguenti illustrano come generare un `HubException` durante una chiamata dell'Hub e su come gestire l'eccezione nel client .NET e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c8f62-419">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="c8f62-420">**Codice del server per illustrare la classe HubException**</span><span class="sxs-lookup"><span data-stu-id="c8f62-420">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="c8f62-421">**Codice JavaScript client dimostrazione della risposta alla generazione di un HubException in un hub**</span><span class="sxs-lookup"><span data-stu-id="c8f62-421">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="c8f62-422">**Codice client .NET dimostrazione della risposta alla generazione di un HubException in un hub**</span><span class="sxs-lookup"><span data-stu-id="c8f62-422">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="c8f62-423">Per altre informazioni sui moduli di Hub della pipeline, vedere [come personalizzare la pipeline hub](#hubpipeline) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="c8f62-423">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="c8f62-424">Come abilitare la traccia</span><span class="sxs-lookup"><span data-stu-id="c8f62-424">How to enable tracing</span></span>

<span data-ttu-id="c8f62-425">Per abilitare la traccia sul lato server, aggiungere un elemento System. Diagnostics al file Web. config, come illustrato in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="c8f62-425">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="c8f62-426">Quando si esegue l'applicazione in Visual Studio, è possibile visualizzare i log nel **Output** finestra.</span><span class="sxs-lookup"><span data-stu-id="c8f62-426">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="c8f62-427">Come chiamare i metodi di client e gestire i gruppi dall'esterno della classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-427">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="c8f62-428">Per chiamare i metodi di client da una classe diversa rispetto a classe Hub, ottenere un riferimento all'oggetto di contesto SignalR per l'Hub e usarlo per chiamare metodi sul client o gestire gruppi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-428">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="c8f62-429">L'esempio seguente `StockTicker` classe ottiene l'oggetto di contesto, archiviarli in un'istanza della classe, archivia l'istanza della classe in una proprietà statica e Usa il contesto dall'istanza della classe singleton per chiamare il `updateStockPrice` sul client che si trovano (metodo) connesso a un Hub denominato `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-429">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="c8f62-430">Se è necessario usare i tempi di più rapida in un oggetto di lunga duraturo, ottenere il riferimento a una sola volta e salvare, anziché ottenerlo nuovamente ogni volta.</span><span class="sxs-lookup"><span data-stu-id="c8f62-430">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="c8f62-431">Recupero del contesto di una volta garantisce che SignalR invia messaggi ai client nella stessa sequenza in cui i metodi dell'Hub sul rendono client chiamate al metodo.</span><span class="sxs-lookup"><span data-stu-id="c8f62-431">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="c8f62-432">Per un'esercitazione che illustra come usare il contesto di SignalR per un Hub, vedere [trasmissione Server con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="c8f62-432">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="c8f62-433">Chiamata dei metodi client</span><span class="sxs-lookup"><span data-stu-id="c8f62-433">Calling client methods</span></span>

<span data-ttu-id="c8f62-434">È possibile specificare che i client riceveranno il RPC, ma sono disponibili meno opzioni rispetto a quando si chiama da una classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-434">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="c8f62-435">Il motivo è che il contesto non è associato a una particolare chiamata da un client, in modo che tutti i metodi che richiedono la conoscenza di ID connessione corrente, ad esempio `Clients.Others`, oppure `Clients.Caller`, o `Clients.OthersInGroup`, non sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="c8f62-435">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="c8f62-436">Sono disponibili le seguenti opzioni:</span><span class="sxs-lookup"><span data-stu-id="c8f62-436">The following options are available:</span></span>

- <span data-ttu-id="c8f62-437">Tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-437">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="c8f62-438">Un client specifico identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-438">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="c8f62-439">Tutti i client connessi eccetto il client specificato, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-439">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="c8f62-440">Tutti i client in un gruppo specificato connessi.</span><span class="sxs-lookup"><span data-stu-id="c8f62-440">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="c8f62-441">Client connessi tutti in un gruppo specificato, eccetto i client specificati, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="c8f62-441">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="c8f62-442">Se si chiama nella classe non-Hub dai metodi nella classe Hub, è possibile passare l'ID di connessione corrente e usarlo con `Clients.Client`, `Clients.AllExcept`, o `Clients.Group` simulare `Clients.Caller`, `Clients.Others`, o `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-442">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="c8f62-443">Nell'esempio seguente, il `MoveShapeHub` classe passa l'ID connessione per il `Broadcaster` classe in modo che le `Broadcaster` classe consente di simulare `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="c8f62-443">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="c8f62-444">Gestire l'appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="c8f62-444">Managing group membership</span></span>

<span data-ttu-id="c8f62-445">Per gestire i gruppi sono disponibili le opzioni stesso come in una classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-445">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="c8f62-446">Aggiungere un client a un gruppo</span><span class="sxs-lookup"><span data-stu-id="c8f62-446">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="c8f62-447">Rimuovere un client da un gruppo</span><span class="sxs-lookup"><span data-stu-id="c8f62-447">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="c8f62-448">Come personalizzare la pipeline di hub</span><span class="sxs-lookup"><span data-stu-id="c8f62-448">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="c8f62-449">SignalR consente di inserire codice personalizzato nella pipeline dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="c8f62-449">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="c8f62-450">Nell'esempio seguente mostra un modulo di pipeline Hub personalizzato che registra ogni chiamata al metodo in ingresso ricevuto dal client e in uscita chiamata al metodo richiamato nel client:</span><span class="sxs-lookup"><span data-stu-id="c8f62-450">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="c8f62-451">Il codice seguente il *Startup.cs* file registra il modulo per l'esecuzione della pipeline dell'Hub:</span><span class="sxs-lookup"><span data-stu-id="c8f62-451">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="c8f62-452">Esistono diversi metodi che è possibile eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="c8f62-452">There are many different methods that you can override.</span></span> <span data-ttu-id="c8f62-453">Per un elenco completo, vedere [HubPipelineModule metodi](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="c8f62-453">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
