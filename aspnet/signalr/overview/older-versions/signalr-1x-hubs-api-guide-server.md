---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: Guida all'API di ASP.NET SignalR Hubs - Server (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Questo documento viene fornita un'introduzione alla programmazione sul lato server dell'API di hub SignalR ASP.NET per SignalR versione 1.1, con demonstratin esempi di codice...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 5597b89688382ab7eb2c2c724baf9a32abee49c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396666"
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="6bcef-103">Guida all'API di ASP.NET SignalR Hubs - Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="6bcef-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="6bcef-104">dal [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6bcef-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="6bcef-105">Questo documento fornisce un'introduzione alla programmazione sul lato server dell'API di hub SignalR ASP.NET per SignalR versione 1.1, con esempi di codice che illustrano le opzioni comuni.</span><span class="sxs-lookup"><span data-stu-id="6bcef-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="6bcef-106">L'API di hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="6bcef-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="6bcef-107">Nel codice server, si definiscono i metodi che possono essere chiamati da parte dei client e si chiamano i metodi eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="6bcef-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="6bcef-108">Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi che eseguono sul server.</span><span class="sxs-lookup"><span data-stu-id="6bcef-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="6bcef-109">SignalR si occupa di tutte le attività client-server.</span><span class="sxs-lookup"><span data-stu-id="6bcef-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="6bcef-110">SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="6bcef-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="6bcef-111">Per un'introduzione a SignalR hub e connessioni permanenti o per un'esercitazione che illustra come compilare un'applicazione di SignalR completa, vedere [SignalR - Guida introduttiva](index.md).</span><span class="sxs-lookup"><span data-stu-id="6bcef-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="6bcef-112">Panoramica</span><span class="sxs-lookup"><span data-stu-id="6bcef-112">Overview</span></span>

<span data-ttu-id="6bcef-113">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="6bcef-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="6bcef-114">Come registrare le route di SignalR e configurare le opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="6bcef-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="6bcef-115">L'URL /signalr</span><span class="sxs-lookup"><span data-stu-id="6bcef-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="6bcef-116">Configurazione delle opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="6bcef-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="6bcef-117">Come creare e usare classi di Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="6bcef-118">Durata dell'oggetto hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="6bcef-119">Notazione camel le maiuscole/minuscole dei nomi dell'Hub nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="6bcef-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="6bcef-120">Più hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="6bcef-121">Come definire i metodi nella classe Hub che i client possono chiamare</span><span class="sxs-lookup"><span data-stu-id="6bcef-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="6bcef-122">Notazione camel le maiuscole/minuscole dei nomi di metodo nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="6bcef-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="6bcef-123">Quando è necessario eseguire in modo asincrono</span><span class="sxs-lookup"><span data-stu-id="6bcef-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="6bcef-124">La definizione di overload</span><span class="sxs-lookup"><span data-stu-id="6bcef-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="6bcef-125">Come chiamare i metodi client dalla classe Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="6bcef-126">Selezionando i client che riceverà la chiamata RPC</span><span class="sxs-lookup"><span data-stu-id="6bcef-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="6bcef-127">Nessuna convalida in fase di compilazione per i nomi di metodo</span><span class="sxs-lookup"><span data-stu-id="6bcef-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="6bcef-128">La corrispondenza dei nomi di metodo tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="6bcef-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="6bcef-129">Esecuzione asincrona</span><span class="sxs-lookup"><span data-stu-id="6bcef-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="6bcef-130">Come gestire l'appartenenza al gruppo dalla classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="6bcef-131">Esecuzione asincrona dei metodi Add e Remove</span><span class="sxs-lookup"><span data-stu-id="6bcef-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="6bcef-132">Persistenza di appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="6bcef-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="6bcef-133">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="6bcef-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="6bcef-134">Come gestire gli eventi di durata connessione nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="6bcef-135">Quando vengono chiamati OnConnected OnDisconnected e OnReconnected</span><span class="sxs-lookup"><span data-stu-id="6bcef-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="6bcef-136">Stato del chiamante non popolato</span><span class="sxs-lookup"><span data-stu-id="6bcef-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="6bcef-137">Come ottenere informazioni relative al client dalla proprietà di contesto</span><span class="sxs-lookup"><span data-stu-id="6bcef-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="6bcef-138">Come passare lo stato tra i client e la classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="6bcef-139">Come gestire gli errori nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="6bcef-140">Come chiamare i metodi di client e gestire i gruppi dall'esterno della classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="6bcef-141">Chiamata dei metodi client</span><span class="sxs-lookup"><span data-stu-id="6bcef-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="6bcef-142">Gestire l'appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="6bcef-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="6bcef-143">Come abilitare la traccia</span><span class="sxs-lookup"><span data-stu-id="6bcef-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="6bcef-144">Come personalizzare la pipeline di hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="6bcef-145">Per informazioni su come i client di programma, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="6bcef-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="6bcef-146">Guida all'API di SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="6bcef-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="6bcef-147">Guida all'API di SignalR Hubs - Client .NET</span><span class="sxs-lookup"><span data-stu-id="6bcef-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="6bcef-148">I collegamenti agli argomenti di riferimento all'API sono alla versione dell'API .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="6bcef-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="6bcef-149">Se si usa .NET 4, vedere [la versione di .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="6bcef-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="6bcef-150">Come registrare le route di SignalR e configurare le opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="6bcef-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="6bcef-151">Per definire le route che i client useranno per connettersi all'hub, chiamare il [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) metodo all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="6bcef-152">`MapHubs` è un' [metodo di estensione](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) per il `System.Web.Routing.RouteCollection` classe.</span><span class="sxs-lookup"><span data-stu-id="6bcef-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="6bcef-153">Nell'esempio seguente viene illustrato come definire la route degli hub SignalR nella *Global. asax* file.</span><span class="sxs-lookup"><span data-stu-id="6bcef-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="6bcef-154">Se si aggiunge una funzionalità SignalR a un'applicazione ASP.NET MVC, assicurarsi che prima di altre route viene aggiunta la route di SignalR.</span><span class="sxs-lookup"><span data-stu-id="6bcef-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="6bcef-155">Per altre informazioni, vedere [esercitazione: Introduzione a SignalR e MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="6bcef-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="6bcef-156">L'URL /signalr</span><span class="sxs-lookup"><span data-stu-id="6bcef-156">The /signalr URL</span></span>

<span data-ttu-id="6bcef-157">Per impostazione predefinita, l'URL di route che i client useranno per connettersi all'hub è "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="6bcef-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="6bcef-158">(Non confondere questo URL con l'URL "hub signalr /", ovvero per il file JavaScript generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6bcef-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="6bcef-159">Per altre informazioni sul proxy generato, vedere [Guida all'API di SignalR Hubs - JavaScript Client - proxy generato e che cosa fa per te](index.md).)</span><span class="sxs-lookup"><span data-stu-id="6bcef-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="6bcef-160">Potrebbero esserci casi straordinari che rendono questo URL di base non è utilizzabile per SignalR; ad esempio, è presente una cartella nel progetto denominato *signalr* e non si desidera modificare il nome.</span><span class="sxs-lookup"><span data-stu-id="6bcef-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="6bcef-161">In tal caso, è possibile modificare l'URL di base, come illustrato negli esempi seguenti (sostituire "/ signalr" nel codice di esempio con il proprio URL desiderato).</span><span class="sxs-lookup"><span data-stu-id="6bcef-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="6bcef-162">**Codice del server che specifica l'URL**</span><span class="sxs-lookup"><span data-stu-id="6bcef-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="6bcef-163">**Codice JavaScript client che specifica l'URL (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="6bcef-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="6bcef-164">**Codice JavaScript client che specifica l'URL (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="6bcef-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="6bcef-165">**Codice client .NET che specifica l'URL**</span><span class="sxs-lookup"><span data-stu-id="6bcef-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="6bcef-166">Configurazione delle opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="6bcef-166">Configuring SignalR Options</span></span>

<span data-ttu-id="6bcef-167">Esegue l'overload del `MapHubs` metodo consentono di specificare un URL personalizzato, un resolver di dipendenza personalizzate e le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6bcef-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="6bcef-168">Abilitare le chiamate tra domini da client browser.</span><span class="sxs-lookup"><span data-stu-id="6bcef-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="6bcef-169">In genere se il browser ha caricato una pagina dalla `http://contoso.com`, la connessione SignalR è nello stesso dominio, in `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="6bcef-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="6bcef-170">Se la pagina dalla `http://contoso.com` stabilisce una connessione a `http://fabrikam.com/signalr`, vale a dire una connessione tra domini.</span><span class="sxs-lookup"><span data-stu-id="6bcef-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="6bcef-171">Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6bcef-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="6bcef-172">Per altre informazioni, vedere [Guida all'API di hub di ASP.NET SignalR - JavaScript Client - come stabilire una connessione cross-domain](index.md).</span><span class="sxs-lookup"><span data-stu-id="6bcef-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="6bcef-173">Abilitare i messaggi di errore dettagliato.</span><span class="sxs-lookup"><span data-stu-id="6bcef-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="6bcef-174">Quando si verificano errori, il comportamento predefinito di SignalR consiste nell'inviare ai client un messaggio di notifica senza informazioni dettagliate su cosa è successo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="6bcef-175">L'invio di informazioni dettagliate sull'errore per i client non è consigliabile nell'ambiente di produzione, perché gli utenti malintenzionati potrebbero essere in grado di usare le informazioni per gli attacchi contro la tua applicazione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="6bcef-176">Per risolvere il problema, è possibile utilizzare questa opzione per abilitare temporaneamente la segnalazione di errori maggiori.</span><span class="sxs-lookup"><span data-stu-id="6bcef-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="6bcef-177">Disabilita la funzionalità file proxy JavaScript generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6bcef-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="6bcef-178">Per impostazione predefinita, viene generato un file JavaScript con i proxy per le classi dell'Hub in risposta all'URL "/ signalr/hub".</span><span class="sxs-lookup"><span data-stu-id="6bcef-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="6bcef-179">Se non si vuole usare i proxy di JavaScript, o se si desidera generare manualmente questo file e fare riferimento a un file fisico nel client, è possibile utilizzare questa opzione per disabilitare la generazione del proxy.</span><span class="sxs-lookup"><span data-stu-id="6bcef-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="6bcef-180">Per altre informazioni, vedere [Guida all'API di SignalR Hubs - JavaScript Client - come creare un file fisico per SignalR generato proxy](index.md).</span><span class="sxs-lookup"><span data-stu-id="6bcef-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="6bcef-181">Nell'esempio seguente viene illustrato come specificare l'URL di connessione SignalR e queste opzioni in una chiamata al `MapHubs` (metodo).</span><span class="sxs-lookup"><span data-stu-id="6bcef-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="6bcef-182">Per specificare un URL personalizzato, sostituire "/ signalr" nell'esempio con l'URL a cui si desidera utilizzare.</span><span class="sxs-lookup"><span data-stu-id="6bcef-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="6bcef-183">Come creare e usare classi di Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-183">How to create and use Hub classes</span></span>

<span data-ttu-id="6bcef-184">Per creare un Hub, creare una classe che deriva da [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="6bcef-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="6bcef-185">Nell'esempio seguente viene illustrata una semplice classe di Hub per un'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="6bcef-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="6bcef-186">In questo esempio, è possibile chiamare un client connesso di `NewContosoChatMessage` (metodo), e in questo caso, i dati ricevuti sono sono trasmesse a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="6bcef-187">Durata dell'oggetto hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-187">Hub object lifetime</span></span>

<span data-ttu-id="6bcef-188">Non si crea un'istanza della classe Hub o chiamarne i metodi dal codice del server. tutto ciò che avviene automaticamente tramite la pipeline degli hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="6bcef-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="6bcef-189">SignalR crea una nuova istanza della classe Hub ogni volta che deve gestire un'operazione dell'Hub, ad esempio quando un client si connette, si disconnette o effettua un chiamata di metodo nel server.</span><span class="sxs-lookup"><span data-stu-id="6bcef-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="6bcef-190">Poiché le istanze della classe Hub sono temporanee, non possono essere utilizzati per mantenere lo stato da una chiamata al metodo a quella successiva.</span><span class="sxs-lookup"><span data-stu-id="6bcef-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="6bcef-191">Ogni volta che il server riceve una chiamata al metodo da un client in una nuova istanza dei processi di classe dell'Hub del messaggio.</span><span class="sxs-lookup"><span data-stu-id="6bcef-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="6bcef-192">Per mantenere lo stato attraverso più connessioni e chiamate al metodo, usare un altro metodo, ad esempio un database o una variabile statica nella classe dell'Hub o un'altra classe non deriva da `Hub`.</span><span class="sxs-lookup"><span data-stu-id="6bcef-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="6bcef-193">Se è rendere persistenti i dati in memoria, usando un metodo, ad esempio una variabile statica della classe dell'Hub, i dati andranno perse quando viene riciclato il dominio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="6bcef-194">Se si desidera inviare messaggi ai client dal codice che viene eseguito all'esterno della classe dell'Hub, non è possibile farlo creando un'istanza della classe dell'Hub, ma è possibile farlo tramite il recupero di un riferimento all'oggetto di contesto SignalR per la classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="6bcef-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="6bcef-195">Per altre informazioni, vedere [come client di chiamare metodi e gestire i gruppi dall'esterno della classe dell'Hub](#callfromoutsidehub) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="6bcef-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="6bcef-196">Notazione camel le maiuscole/minuscole dei nomi dell'Hub nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="6bcef-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="6bcef-197">Per impostazione predefinita, i client JavaScript fare riferimento a un hub usando una versione con notazione camel del nome della classe.</span><span class="sxs-lookup"><span data-stu-id="6bcef-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="6bcef-198">SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6bcef-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="6bcef-199">Nell'esempio precedente potrebbe essere indicata come `contosoChatHub` nel codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6bcef-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="6bcef-200">**Server**</span><span class="sxs-lookup"><span data-stu-id="6bcef-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="6bcef-201">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="6bcef-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="6bcef-202">Se si desidera specificare un nome diverso per i client da usare, aggiungere il `HubName` attributo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="6bcef-203">Quando si usa un `HubName` attributo, non vi è alcuna modifica del nome in notazione camel nel client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6bcef-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="6bcef-204">**Server**</span><span class="sxs-lookup"><span data-stu-id="6bcef-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="6bcef-205">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="6bcef-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="6bcef-206">Più hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-206">Multiple Hubs</span></span>

<span data-ttu-id="6bcef-207">È possibile definire più classi Hub in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="6bcef-208">Quando si esegue questa operazione, la connessione è condivisa ma i gruppi sono separati:</span><span class="sxs-lookup"><span data-stu-id="6bcef-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="6bcef-209">Tutti i client utilizzeranno lo stesso URL per stabilire una connessione di SignalR con il servizio ("/ signalr" o l'URL personalizzato se è stata specificata una), e viene utilizzata per tutti gli hub di connessione definite dal servizio.</span><span class="sxs-lookup"><span data-stu-id="6bcef-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="6bcef-210">Non vi è alcuna differenza nelle prestazioni per più hub rispetto alla definizione di tutte le funzionalità dell'Hub in una singola classe.</span><span class="sxs-lookup"><span data-stu-id="6bcef-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="6bcef-211">Tutti gli hub di ottenere le stesse informazioni di richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6bcef-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="6bcef-212">Poiché tutti gli hub condividono la stessa connessione, l'unica informazione richiesta HTTP che il server ottiene è ciò che viene ricevuta la richiesta HTTP originale che stabilisce la connessione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="6bcef-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="6bcef-213">Se si usa la richiesta di connessione per passare le informazioni dal client al server, specificando una stringa di query, non è possibile fornire le stringhe di query diversi hub diversi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="6bcef-214">Tutti gli hub riceverà le stesse informazioni.</span><span class="sxs-lookup"><span data-stu-id="6bcef-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="6bcef-215">Il file proxy JavaScript generato conterrà i proxy per tutti gli hub in un unico file.</span><span class="sxs-lookup"><span data-stu-id="6bcef-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="6bcef-216">Per informazioni sui proxy JavaScript, vedere [Guida all'API di SignalR Hubs - JavaScript Client - proxy generato e che cosa fa per te](index.md).</span><span class="sxs-lookup"><span data-stu-id="6bcef-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="6bcef-217">I gruppi vengono definiti all'interno di hub.</span><span class="sxs-lookup"><span data-stu-id="6bcef-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="6bcef-218">In SignalR che è possibile definire gruppi per la trasmissione a subset di client connessi con nome.</span><span class="sxs-lookup"><span data-stu-id="6bcef-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="6bcef-219">I gruppi vengono mantenuti separatamente per ogni Hub.</span><span class="sxs-lookup"><span data-stu-id="6bcef-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="6bcef-220">Ad esempio, un gruppo denominato "Administrators" sono un set di client per il `ContosoChatHub` classi e lo stesso nome di gruppo per fare riferimento a un set diverso di client per il `StockTickerHub` classe.</span><span class="sxs-lookup"><span data-stu-id="6bcef-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="6bcef-221">Come definire i metodi nella classe Hub che i client possono chiamare</span><span class="sxs-lookup"><span data-stu-id="6bcef-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="6bcef-222">Per esporre un metodo dell'hub che si desidera essere richiamabile dal client, dichiarare un metodo pubblico, come illustrato negli esempi seguenti.</span><span class="sxs-lookup"><span data-stu-id="6bcef-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="6bcef-223">È possibile specificare un tipo restituito e parametri, compresi i tipi complessi e le matrici, come si farebbe in qualsiasi metodo c#.</span><span class="sxs-lookup"><span data-stu-id="6bcef-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="6bcef-224">Tutti i dati ricevuti nei parametri o restituire al chiamante vengono comunicati tra il client e server mediante JSON e SignalR gestisce automaticamente l'associazione di oggetti complessi e le matrici di oggetti.</span><span class="sxs-lookup"><span data-stu-id="6bcef-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="6bcef-225">Notazione camel le maiuscole/minuscole dei nomi di metodo nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="6bcef-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="6bcef-226">Per impostazione predefinita, i client JavaScript fare riferimento ai metodi dell'Hub usando una versione con notazione camel del nome del metodo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="6bcef-227">SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6bcef-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="6bcef-228">**Server**</span><span class="sxs-lookup"><span data-stu-id="6bcef-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="6bcef-229">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="6bcef-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="6bcef-230">Se si desidera specificare un nome diverso per i client da usare, aggiungere il `HubMethodName` attributo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="6bcef-231">**Server**</span><span class="sxs-lookup"><span data-stu-id="6bcef-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="6bcef-232">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="6bcef-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="6bcef-233">Quando è necessario eseguire in modo asincrono</span><span class="sxs-lookup"><span data-stu-id="6bcef-233">When to execute asynchronously</span></span>

<span data-ttu-id="6bcef-234">Se il metodo verrà essere a esecuzione prolungata o è necessario eseguire le operazioni che verrebbe comportano l'attesa, ad esempio una ricerca nel database o una chiamata al servizio web, rendere asincrono il metodo dell'Hub, restituendo un [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (invece di `void` restituire) o [ Task&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) oggetto (invece di `T` tipo restituito).</span><span class="sxs-lookup"><span data-stu-id="6bcef-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="6bcef-235">Quando si torna una `Task` oggetto dal metodo, SignalR attende la `Task` completamento, e quindi invia il risultato annullato il wrapping al client, pertanto non presenta alcuna differenza nel modo in cui è codice la chiamata al metodo nel client.</span><span class="sxs-lookup"><span data-stu-id="6bcef-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="6bcef-236">Rendere un metodo dell'Hub asincrona consente di evitare blocchi la connessione quando utilizza il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="6bcef-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="6bcef-237">Quando un metodo dell'Hub esegue in modo sincrono e il trasporto WebSocket, le successive chiamate dei metodi dell'hub dallo stesso client vengono bloccate fino al completamento del metodo dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="6bcef-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="6bcef-238">L'esempio seguente illustra il metodo di stesso codificate in modo da eseguire in modo sincrono o asincrono, seguito da codice JavaScript client che funziona per la chiamata a entrambe le versioni.</span><span class="sxs-lookup"><span data-stu-id="6bcef-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="6bcef-239">**Sincrono**</span><span class="sxs-lookup"><span data-stu-id="6bcef-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="6bcef-240">**Asincrono - ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="6bcef-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="6bcef-241">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="6bcef-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="6bcef-242">Per altre informazioni su come usare i metodi asincroni in ASP.NET 4.5, vedere [usando i metodi asincroni in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="6bcef-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="6bcef-243">La definizione di overload</span><span class="sxs-lookup"><span data-stu-id="6bcef-243">Defining Overloads</span></span>

<span data-ttu-id="6bcef-244">Se si desidera definire gli overload per un metodo, il numero di parametri in ogni overload deve essere diverso.</span><span class="sxs-lookup"><span data-stu-id="6bcef-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="6bcef-245">Se un overload si differenzia solo specificando i tipi di parametro diversi, la classe dell'Hub verrà compilate, ma il servizio SignalR genererà un'eccezione in fase di esecuzione quando i client di provano a chiamare uno degli overload.</span><span class="sxs-lookup"><span data-stu-id="6bcef-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="6bcef-246">Come chiamare i metodi client dalla classe Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="6bcef-247">Per chiamare i metodi di client dal server, usare il `Clients` proprietà in un metodo nella classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="6bcef-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="6bcef-248">L'esempio seguente illustra codice lato server che chiama `addNewMessageToPage` sui client connesse tutte le soluzioni e codice client che definisce il metodo in un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6bcef-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="6bcef-249">**Server**</span><span class="sxs-lookup"><span data-stu-id="6bcef-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="6bcef-250">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="6bcef-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="6bcef-251">È possibile ottenere un valore restituito da un metodo client. la sintassi, ad esempio `int x = Clients.All.add(1,1)` non funziona.</span><span class="sxs-lookup"><span data-stu-id="6bcef-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="6bcef-252">È possibile specificare i tipi complessi e matrici per i parametri.</span><span class="sxs-lookup"><span data-stu-id="6bcef-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="6bcef-253">Nell'esempio seguente passa un tipo complesso al client in un parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="6bcef-254">**Codice del server che chiama un metodo client usando un oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="6bcef-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="6bcef-255">**Codice del server che definisce l'oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="6bcef-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="6bcef-256">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="6bcef-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="6bcef-257">Selezionando i client che riceverà la chiamata RPC</span><span class="sxs-lookup"><span data-stu-id="6bcef-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="6bcef-258">La proprietà restituisce ai client un [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) oggetto che fornisce diverse opzioni per specificare che i client riceveranno il RPC:</span><span class="sxs-lookup"><span data-stu-id="6bcef-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="6bcef-259">Tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="6bcef-260">Solo il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="6bcef-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="6bcef-261">Tutti i client, eccetto il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="6bcef-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="6bcef-262">Un client specifico identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="6bcef-263">Questo esempio viene chiamato `addContosoChatMessageToPage` sul client chiamante e ha lo stesso effetto dell'utilizzo `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="6bcef-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="6bcef-264">Tutti i client connessi eccetto il client specificato, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="6bcef-265">Tutti i client in un gruppo specificato connessi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="6bcef-266">Tutti i client connessi in un gruppo specificato eccetto il client specificato, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="6bcef-267">Tutti i client connessi in un gruppo specificato eccetto il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="6bcef-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="6bcef-268">Nessuna convalida in fase di compilazione per i nomi di metodo</span><span class="sxs-lookup"><span data-stu-id="6bcef-268">No compile-time validation for method names</span></span>

<span data-ttu-id="6bcef-269">Il nome del metodo specificato viene interpretato come un oggetto dinamico, ovvero che non IntelliSense o convalida in fase di compilazione appositamente.</span><span class="sxs-lookup"><span data-stu-id="6bcef-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="6bcef-270">L'espressione viene valutata in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="6bcef-271">Quando viene eseguita la chiamata al metodo, SignalR invia il nome del metodo e i valori dei parametri per il client, e se il client dispone di un metodo che corrisponde al nome, la chiamata a metodo e i valori dei parametri vengono passati a esso.</span><span class="sxs-lookup"><span data-stu-id="6bcef-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="6bcef-272">Se non viene trovato alcun metodo corrispondente nel client, viene generato alcun errore.</span><span class="sxs-lookup"><span data-stu-id="6bcef-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="6bcef-273">Per informazioni sul formato dei dati che SignalR trasmette al client dietro le quinte quando si chiama un metodo client, vedere [Introduzione a SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="6bcef-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="6bcef-274">La corrispondenza dei nomi di metodo tra maiuscole e minuscole</span><span class="sxs-lookup"><span data-stu-id="6bcef-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="6bcef-275">Metodo nome viene applicata alcuna distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="6bcef-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="6bcef-276">Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, o `addContosoChatMessageToPage` sul client.</span><span class="sxs-lookup"><span data-stu-id="6bcef-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="6bcef-277">Esecuzione asincrona</span><span class="sxs-lookup"><span data-stu-id="6bcef-277">Asynchronous execution</span></span>

<span data-ttu-id="6bcef-278">Il metodo chiamato viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="6bcef-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="6bcef-279">Qualsiasi codice che segue una chiamata al metodo a un client verrà eseguita immediatamente senza attendere SignalR completare la trasmissione dei dati per i client a meno che non si specifica che le successive righe di codice deve attendere il completamento di metodo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="6bcef-280">Esempi di codice seguenti illustrano come eseguire i due metodi di client in modo sequenziale, uno usando il codice che funziona in .NET 4.5 e uno usando il codice che funziona in .NET 4.</span><span class="sxs-lookup"><span data-stu-id="6bcef-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="6bcef-281">**Esempio di .NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="6bcef-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="6bcef-282">**Esempio 4 di .NET**</span><span class="sxs-lookup"><span data-stu-id="6bcef-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="6bcef-283">Se si usa `await` o `ContinueWith` per attendere fino al completamento di un metodo client prima dell'esecuzione riga di codice successiva, ciò non significa che i client effettivamente riceverà il messaggio prima dell'esecuzione riga di codice successiva.</span><span class="sxs-lookup"><span data-stu-id="6bcef-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="6bcef-284">"Completamento" di una chiamata al metodo client significa solo che ha eseguito tutto il necessario per inviare il messaggio SignalR.</span><span class="sxs-lookup"><span data-stu-id="6bcef-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="6bcef-285">Se è necessario che i client ha ricevuto il messaggio di verifica, è necessario programmare tale meccanismo manualmente.</span><span class="sxs-lookup"><span data-stu-id="6bcef-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="6bcef-286">Ad esempio, è possibile codificare una `MessageReceived` metodo dell'hub e nel `addContosoChatMessageToPage` metodo sul client è possibile chiamare `MessageReceived` al termine dell'operazione è qualsiasi elemento di lavoro da eseguire sul client.</span><span class="sxs-lookup"><span data-stu-id="6bcef-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="6bcef-287">In `MessageReceived` nell'Hub è possibile eseguire le operazioni dipende da client effettiva ricezione e l'elaborazione della chiamata al metodo originale.</span><span class="sxs-lookup"><span data-stu-id="6bcef-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="6bcef-288">Come usare una variabile di stringa come il nome del metodo</span><span class="sxs-lookup"><span data-stu-id="6bcef-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="6bcef-289">Se si vuole richiamare un metodo client utilizzando una variabile di stringa come il nome del metodo, eseguire il cast `Clients.All` (o `Clients.Others`, `Clients.Caller`e così via) per `IClientProxy` e quindi chiamare [Invoke (methodName, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="6bcef-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="6bcef-290">Come gestire l'appartenenza al gruppo dalla classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="6bcef-291">Gruppi in SignalR forniscono un metodo per la trasmissione di messaggi per sottoinsiemi specifici di client connessi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="6bcef-292">Un gruppo può avere qualsiasi numero di client e un client può essere un membro di un numero qualsiasi di gruppi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="6bcef-293">Per gestire l'appartenenza al gruppo, usare il [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) e [rimuovere](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metodi forniti dal `Groups` proprietà della classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="6bcef-293">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="6bcef-294">L'esempio seguente mostra le `Groups.Add` e `Groups.Remove` metodi usati nei metodi dell'Hub che vengono chiamati dal codice client, seguito da codice JavaScript client che li chiama.</span><span class="sxs-lookup"><span data-stu-id="6bcef-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="6bcef-295">**Server**</span><span class="sxs-lookup"><span data-stu-id="6bcef-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="6bcef-296">**Client JavaScript tramite proxy generato**</span><span class="sxs-lookup"><span data-stu-id="6bcef-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="6bcef-297">Non è necessario creare in modo esplicito gruppi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="6bcef-298">In effetti un gruppo viene creato automaticamente la prima volta specificarne il nome in una chiamata a `Groups.Add`, e viene eliminata quando si rimuove l'ultima connessione dall'appartenenza al suo interno.</span><span class="sxs-lookup"><span data-stu-id="6bcef-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="6bcef-299">Non è disponibile alcuna API per ottenere un elenco di appartenenza al gruppo o un elenco di gruppi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="6bcef-300">SignalR invia messaggi al client e i gruppi basati su un [modello pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), e il server non venga mantenuto un elenco di gruppi o appartenenze ai gruppi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="6bcef-301">Ciò consente di ottimizzare la scalabilità, perché ogni volta che si aggiunge un nodo a una web farm, qualsiasi stato che gestisce SignalR deve essere propagata nel nuovo nodo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="6bcef-302">Esecuzione asincrona dei metodi Add e Remove</span><span class="sxs-lookup"><span data-stu-id="6bcef-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="6bcef-303">Il `Groups.Add` e `Groups.Remove` metodi eseguiti in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="6bcef-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="6bcef-304">Se si desidera aggiungere un client a un gruppo e inviare immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il `Groups.Add` metodo termina per prima.</span><span class="sxs-lookup"><span data-stu-id="6bcef-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="6bcef-305">Esempi di codice seguenti illustrano come eseguire questa operazione, una con il codice che funziona in .NET 4.5 e uno con il codice che funziona in .NET 4</span><span class="sxs-lookup"><span data-stu-id="6bcef-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="6bcef-306">**Esempio di .NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="6bcef-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="6bcef-307">**Esempio 4 di .NET**</span><span class="sxs-lookup"><span data-stu-id="6bcef-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="6bcef-308">Persistenza di appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="6bcef-308">Group membership persistence</span></span>

<span data-ttu-id="6bcef-309">SignalR tiene traccia delle connessioni, non per gli utenti, pertanto se si desidera che un utente sia nello stesso gruppo ogni volta che l'utente definisce una connessione, è necessario chiamare `Groups.Add` ogni volta che l'utente definisce una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="6bcef-310">Dopo una perdita temporanea della connettività, in alcuni casi SignalR possibile ripristinare la connessione automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6bcef-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="6bcef-311">In tal caso, SignalR sta ripristinando la stessa connessione, non stabilire una nuova connessione e quindi, vengono ripristinato automaticamente l'appartenenza al gruppo del client.</span><span class="sxs-lookup"><span data-stu-id="6bcef-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="6bcef-312">Questo è possibile anche quando l'interruzione temporanea è il risultato di un riavvio del server o di errore, perché lo stato della connessione per ogni client, incluse le appartenenze di gruppo, viene sottoposto a round trip al client.</span><span class="sxs-lookup"><span data-stu-id="6bcef-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="6bcef-313">Se un server si arresta e viene sostituito da un nuovo server prima del timeout della connessione, un client possa riconnettersi automaticamente al nuovo server e nuovamente la registrazione è un membro dei gruppi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="6bcef-314">Quando una connessione non può essere ripristinata automaticamente dopo una perdita di connettività, o quando si verifica il timeout della connessione o quando il client si disconnette (ad esempio, quando un browser consente di passare a una nuova pagina), le appartenenze di gruppo vengono perse.</span><span class="sxs-lookup"><span data-stu-id="6bcef-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="6bcef-315">Al successivo si connette l'utente sarà una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="6bcef-316">Per mantenere appartenenze quando lo stesso utente stabilisce una nuova connessione, l'applicazione deve rilevare le associazioni tra utenti e gruppi e ripristinare le appartenenze di gruppo ogni volta che un utente stabilisce una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="6bcef-317">Per altre informazioni sulle connessioni e le riconnessioni, vedere [come gestire gli eventi di durata connessione nella classe Hub](#connectionlifetime) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="6bcef-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="6bcef-318">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="6bcef-318">Single-user groups</span></span>

<span data-ttu-id="6bcef-319">Le applicazioni che utilizzano in genere SignalR devono tenere traccia delle associazioni tra utenti e connessioni per sapere quale utente ha inviato un messaggio e che gli utenti dovrebbero ricevere un messaggio.</span><span class="sxs-lookup"><span data-stu-id="6bcef-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="6bcef-320">Gruppi vengono usati in uno dei due modelli di usati comune per questo scopo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="6bcef-321">Gruppi utente singolo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-321">Single-user groups.</span></span>

    <span data-ttu-id="6bcef-322">È possibile specificare il nome utente come nome del gruppo e aggiungere l'ID di connessione corrente al gruppo ogni volta che l'utente si connette o si riconnette.</span><span class="sxs-lookup"><span data-stu-id="6bcef-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="6bcef-323">Per inviare messaggi all'utente di trasmissione al gruppo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="6bcef-324">Uno svantaggio di questo metodo è che il gruppo non offre un modo per verificare se l'utente è online o offline.</span><span class="sxs-lookup"><span data-stu-id="6bcef-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="6bcef-325">Rilevare le associazioni tra i nomi utente e ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="6bcef-326">È possibile archiviare un'associazione tra ogni nome utente e ID di connessione una o più in un dizionario o un database e aggiornare i dati archiviati ogni volta che l'utente si connette o disconnette.</span><span class="sxs-lookup"><span data-stu-id="6bcef-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="6bcef-327">Per inviare messaggi all'utente specificare l'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="6bcef-328">Uno svantaggio di questo metodo è che sono necessari più memoria.</span><span class="sxs-lookup"><span data-stu-id="6bcef-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="6bcef-329">Come gestire gli eventi di durata connessione nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="6bcef-330">Motivi tipici per la gestione degli eventi di durata connessione sono per consentire di controllare se un utente è connesso o meno e di tenere traccia dell'associazione tra i nomi utente e ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="6bcef-331">Per eseguire codice personalizzato quando i client di connettere o disconnettere, eseguire l'override di `OnConnected`, `OnDisconnected`, e `OnReconnected` classe i metodi virtuali dell'Hub, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="6bcef-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="6bcef-332">Quando vengono chiamati OnConnected OnDisconnected e OnReconnected</span><span class="sxs-lookup"><span data-stu-id="6bcef-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="6bcef-333">Ogni volta che un browser consente di passare a una nuova pagina, una nuova connessione deve essere stabilita, ovvero SignalR eseguirà il `OnDisconnected` metodo aggiungendo il `OnConnected` (metodo).</span><span class="sxs-lookup"><span data-stu-id="6bcef-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="6bcef-334">SignalR crea sempre un nuovo ID di connessione quando viene stabilita una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="6bcef-335">Il `OnReconnected` metodo viene chiamato quando si verifica un'interruzione temporanea della connettività che SignalR può automaticamente ripristinare, ad esempio quando un cavo è temporaneamente disconnesso e riconnesso prima del timeout della connessione. Il `OnDisconnected` metodo viene chiamato quando il client viene disconnesso e SignalR non è possibile riconnettersi automaticamente, ad esempio quando un browser passa a una nuova pagina.</span><span class="sxs-lookup"><span data-stu-id="6bcef-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="6bcef-336">Pertanto, è una sequenza di eventi per un determinato client possibili `OnConnected`, `OnReconnected`, `OnDisconnected`; oppure `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="6bcef-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="6bcef-337">Non verrà visualizzata la sequenza `OnConnected`, `OnDisconnected`, `OnReconnected` per una determinata connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="6bcef-338">Il `OnDisconnected` il dominio dell'applicazione ottiene riciclato o metodo non può essere chiamato in alcuni scenari, ad esempio quando un server si arresta.</span><span class="sxs-lookup"><span data-stu-id="6bcef-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="6bcef-339">Quando un altro server è online o il dominio dell'applicazione completato il riciclo, alcuni client potrebbero essere in grado di ristabilire la connessione e vengono attivati i `OnReconnected` evento.</span><span class="sxs-lookup"><span data-stu-id="6bcef-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="6bcef-340">Per altre informazioni, vedere [comprensione e la gestione degli eventi di durata connessione in SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="6bcef-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="6bcef-341">Stato del chiamante non popolato</span><span class="sxs-lookup"><span data-stu-id="6bcef-341">Caller state not populated</span></span>

<span data-ttu-id="6bcef-342">Vengono chiamati i metodi del gestore eventi Durata connessione dal server, il che significa che qualsiasi stato che si inserisce nel `state` oggetti nel client non verranno popolati nel `Caller` proprietà sul server.</span><span class="sxs-lookup"><span data-stu-id="6bcef-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="6bcef-343">Per informazioni sul `state` oggetto e il `Caller` proprietà, vedere [come passare lo stato tra i client e la classe Hub](#passstate) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="6bcef-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="6bcef-344">Come ottenere informazioni relative al client dalla proprietà di contesto</span><span class="sxs-lookup"><span data-stu-id="6bcef-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="6bcef-345">Per ottenere informazioni sul client, usare il `Context` proprietà della classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="6bcef-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="6bcef-346">Il `Context` proprietà restituisce un [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) oggetto che fornisce l'accesso alle informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6bcef-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="6bcef-347">L'ID di connessione del client chiamante.</span><span class="sxs-lookup"><span data-stu-id="6bcef-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="6bcef-348">L'ID di connessione è un GUID assegnato da SignalR (è possibile specificare il valore nel codice).</span><span class="sxs-lookup"><span data-stu-id="6bcef-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="6bcef-349">È un ID di connessione per ogni connessione e la stessa connessione che ID viene utilizzato da tutti gli hub, se si dispone di più hub nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="6bcef-350">Dati dell'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="6bcef-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="6bcef-351">È anche possibile ottenere le intestazioni HTTP dalla `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="6bcef-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="6bcef-352">Il motivo più riferimenti allo stesso concetto è che `Context.Headers` è stato creato prima di tutto la `Context.Request` è stata aggiunta in un secondo momento, proprietà e `Context.Headers` è stata mantenuta per compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="6bcef-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="6bcef-353">Eseguire query sui dati di tipo stringa.</span><span class="sxs-lookup"><span data-stu-id="6bcef-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="6bcef-354">È anche possibile ottenere i dati di stringa di query da `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="6bcef-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="6bcef-355">La stringa di query che si ottiene questa proprietà è quello che è stato usato con la richiesta HTTP che la connessione SignalR stabilita.</span><span class="sxs-lookup"><span data-stu-id="6bcef-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="6bcef-356">È possibile aggiungere parametri della stringa di query nel client tramite la configurazione della connessione, che è un modo pratico per passare i dati relativi ai client dal client al server.</span><span class="sxs-lookup"><span data-stu-id="6bcef-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="6bcef-357">Nell'esempio seguente viene illustrato un modo per aggiungere una stringa di query in un client JavaScript quando si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="6bcef-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="6bcef-358">Per altre informazioni sull'impostazione di parametri della stringa di query, vedere le guide di API per il [JavaScript](index.md) e [.NET](index.md) client.</span><span class="sxs-lookup"><span data-stu-id="6bcef-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="6bcef-359">È possibile trovare il metodo di trasporto utilizzato per la connessione dei dati di stringa di query, con alcuni altri valori utilizzati internamente da SignalR:</span><span class="sxs-lookup"><span data-stu-id="6bcef-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="6bcef-360">Il valore di `transportMethod` sarà "WebSocket", "serverSentEvents", "foreverFrame" o "longPolling".</span><span class="sxs-lookup"><span data-stu-id="6bcef-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="6bcef-361">Si noti che se si archivia il valore di `OnConnected` metodo del gestore eventi, in alcuni scenari è inizialmente potrebbe ottenere un valore di trasporto che non sia il metodo di trasporto negoziata finale per la connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="6bcef-362">In tal caso il metodo genererà un'eccezione e verrà chiamato nuovamente in un secondo momento quando il metodo di trasporto finale viene stabilito.</span><span class="sxs-lookup"><span data-stu-id="6bcef-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="6bcef-363">Cookie.</span><span class="sxs-lookup"><span data-stu-id="6bcef-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="6bcef-364">È anche possibile ottenere i cookie provenienti da `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="6bcef-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="6bcef-365">Informazioni sull'utente.</span><span class="sxs-lookup"><span data-stu-id="6bcef-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="6bcef-366">L'oggetto HttpContext per la richiesta:</span><span class="sxs-lookup"><span data-stu-id="6bcef-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="6bcef-367">Usare questo metodo anziché ottenere `HttpContext.Current` per ottenere il `HttpContext` oggetto per la connessione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="6bcef-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="6bcef-368">Come passare lo stato tra i client e la classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="6bcef-369">Il proxy client fornisce un `state` oggetto in cui è possibile archiviare i dati che si desidera essere trasmessi al server con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="6bcef-370">Nel server è possibile accedere a questi dati nel `Clients.Caller` proprietà nei metodi dell'Hub che vengono chiamati dal client.</span><span class="sxs-lookup"><span data-stu-id="6bcef-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="6bcef-371">Il `Clients.Caller` proprietà non viene popolata per i metodi del gestore eventi Durata connessione `OnConnected`, `OnDisconnected`, e `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="6bcef-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="6bcef-372">La creazione o aggiornamento dei dati nel `state` oggetto e il `Clients.Caller` proprietà funziona in entrambe le direzioni.</span><span class="sxs-lookup"><span data-stu-id="6bcef-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="6bcef-373">È possibile aggiornare i valori nel server e vengono passati al client.</span><span class="sxs-lookup"><span data-stu-id="6bcef-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="6bcef-374">Nell'esempio seguente viene illustrato il codice client JavaScript che archivia lo stato per la trasmissione al server con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="6bcef-375">Nell'esempio seguente mostra il codice equivalente in un client .NET.</span><span class="sxs-lookup"><span data-stu-id="6bcef-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="6bcef-376">Nella classe Hub, è possibile accedere a questi dati nel `Clients.Caller` proprietà.</span><span class="sxs-lookup"><span data-stu-id="6bcef-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="6bcef-377">Nell'esempio seguente viene illustrato il codice che recupera lo stato definito nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="6bcef-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="6bcef-378">Questo meccanismo di persistenza dello stato non è progettato per grandi quantità di dati, poiché tutto ciò che si inserisce nel `state` o `Clients.Caller` proprietà è sottoposto a round trip con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="6bcef-379">È utile per gli elementi più piccoli, ad esempio nomi utente o i contatori.</span><span class="sxs-lookup"><span data-stu-id="6bcef-379">It's useful for smaller items such as user names or counters.</span></span>


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="6bcef-380">Come gestire gli errori nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="6bcef-381">Per gestire gli errori che si verificano nei metodi di classe dell'Hub, usare uno o entrambi i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="6bcef-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="6bcef-382">Eseguire il wrapping del codice del metodo in blocchi try-catch e accedere all'oggetto eccezione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="6bcef-383">A scopo di debug è possibile inviare l'eccezione al client, ma per la sicurezza non sono consigliabile motivi l'invio di informazioni dettagliate per i client nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="6bcef-384">Creare un modulo di hub della pipeline che gestisce il [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) (metodo).</span><span class="sxs-lookup"><span data-stu-id="6bcef-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="6bcef-385">Nell'esempio seguente mostra un modulo di pipeline che registra gli errori, seguiti dal codice in Global. asax che inserisce il modulo nella pipeline di hub.</span><span class="sxs-lookup"><span data-stu-id="6bcef-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="6bcef-386">Per altre informazioni sui moduli di Hub della pipeline, vedere [come personalizzare la pipeline hub](#hubpipeline) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="6bcef-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="6bcef-387">Come abilitare la traccia</span><span class="sxs-lookup"><span data-stu-id="6bcef-387">How to enable tracing</span></span>

<span data-ttu-id="6bcef-388">Per abilitare la traccia sul lato server, aggiungere un elemento System. Diagnostics al file Web. config, come illustrato in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="6bcef-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="6bcef-389">Quando si esegue l'applicazione in Visual Studio, è possibile visualizzare i log nel **Output** finestra.</span><span class="sxs-lookup"><span data-stu-id="6bcef-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="6bcef-390">Come chiamare i metodi di client e gestire i gruppi dall'esterno della classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="6bcef-391">Per chiamare i metodi di client da una classe diversa rispetto a classe Hub, ottenere un riferimento all'oggetto di contesto SignalR per l'Hub e usarlo per chiamare metodi sul client o gestire gruppi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="6bcef-392">L'esempio seguente `StockTicker` classe ottiene l'oggetto di contesto, archiviarli in un'istanza della classe, archivia l'istanza della classe in una proprietà statica e Usa il contesto dall'istanza della classe singleton per chiamare il `updateStockPrice` sul client che si trovano (metodo) connesso a un Hub denominato `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="6bcef-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="6bcef-393">Se è necessario usare i tempi di più rapida in un oggetto di lunga duraturo, ottenere il riferimento a una sola volta e salvare, anziché ottenerlo nuovamente ogni volta.</span><span class="sxs-lookup"><span data-stu-id="6bcef-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="6bcef-394">Recupero del contesto di una volta garantisce che SignalR invia messaggi ai client nella stessa sequenza in cui i metodi dell'Hub sul rendono client chiamate al metodo.</span><span class="sxs-lookup"><span data-stu-id="6bcef-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="6bcef-395">Per un'esercitazione che illustra come usare il contesto di SignalR per un Hub, vedere [trasmissione Server con ASP.NET SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="6bcef-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="6bcef-396">Chiamata dei metodi client</span><span class="sxs-lookup"><span data-stu-id="6bcef-396">Calling client methods</span></span>

<span data-ttu-id="6bcef-397">È possibile specificare che i client riceveranno il RPC, ma sono disponibili meno opzioni rispetto a quando si chiama da una classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="6bcef-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="6bcef-398">Il motivo è che il contesto non è associato a una particolare chiamata da un client, in modo che tutti i metodi che richiedono la conoscenza di ID connessione corrente, ad esempio `Clients.Others`, oppure `Clients.Caller`, o `Clients.OthersInGroup`, non sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="6bcef-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="6bcef-399">Sono disponibili le seguenti opzioni:</span><span class="sxs-lookup"><span data-stu-id="6bcef-399">The following options are available:</span></span>

- <span data-ttu-id="6bcef-400">Tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="6bcef-401">Un client specifico identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="6bcef-402">Tutti i client connessi eccetto il client specificato, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="6bcef-403">Tutti i client in un gruppo specificato connessi.</span><span class="sxs-lookup"><span data-stu-id="6bcef-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="6bcef-404">Client connessi tutti in un gruppo specificato, eccetto i client specificati, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="6bcef-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="6bcef-405">Se si chiama nella classe non-Hub dai metodi nella classe Hub, è possibile passare l'ID di connessione corrente e usarlo con `Clients.Client`, `Clients.AllExcept`, o `Clients.Group` simulare `Clients.Caller`, `Clients.Others`, o `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="6bcef-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="6bcef-406">Nell'esempio seguente, il `MoveShapeHub` classe passa l'ID connessione per il `Broadcaster` classe in modo che le `Broadcaster` classe consente di simulare `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="6bcef-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="6bcef-407">Gestire l'appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="6bcef-407">Managing group membership</span></span>

<span data-ttu-id="6bcef-408">Per gestire i gruppi sono disponibili le opzioni stesso come in una classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="6bcef-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="6bcef-409">Aggiungere un client a un gruppo</span><span class="sxs-lookup"><span data-stu-id="6bcef-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="6bcef-410">Rimuovere un client da un gruppo</span><span class="sxs-lookup"><span data-stu-id="6bcef-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="6bcef-411">Come personalizzare la pipeline di hub</span><span class="sxs-lookup"><span data-stu-id="6bcef-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="6bcef-412">SignalR consente di inserire codice personalizzato nella pipeline dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="6bcef-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="6bcef-413">Nell'esempio seguente mostra un modulo di pipeline Hub personalizzato che registra ogni chiamata al metodo in ingresso ricevuto dal client e in uscita chiamata al metodo richiamato nel client:</span><span class="sxs-lookup"><span data-stu-id="6bcef-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="6bcef-414">Il codice seguente il *Global. asax* file registra il modulo per l'esecuzione della pipeline dell'Hub:</span><span class="sxs-lookup"><span data-stu-id="6bcef-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="6bcef-415">Esistono diversi metodi che è possibile eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="6bcef-415">There are many different methods that you can override.</span></span> <span data-ttu-id="6bcef-416">Per un elenco completo, vedere [HubPipelineModule metodi](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="6bcef-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
