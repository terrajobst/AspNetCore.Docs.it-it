---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: Guida di ASP.NET SignalR hub API - Server (SignalR 1. x) | Documenti Microsoft
author: pfletcher
description: Questo documento viene fornita un'introduzione alla programmazione l'API di ASP.NET SignalR hub sul lato server per SignalR versione 1.1, con demonstratin esempi di codice...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: e594dd1ea4ae027cf0b82574fc5a3eb061b1f2e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="24d27-103">Guida di ASP.NET SignalR hub API - Server (SignalR 1. x)</span><span class="sxs-lookup"><span data-stu-id="24d27-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="24d27-104">da [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="24d27-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="24d27-105">Questo documento viene fornita un'introduzione alla programmazione l'API di ASP.NET SignalR hub sul lato server per SignalR versione 1.1, con esempi di codice che illustrano le opzioni comuni.</span><span class="sxs-lookup"><span data-stu-id="24d27-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="24d27-106">L'API degli hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="24d27-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="24d27-107">Nel codice server, si definiscono i metodi che possono essere chiamati dai client e si chiamano metodi che eseguono il client.</span><span class="sxs-lookup"><span data-stu-id="24d27-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="24d27-108">Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi eseguiti nel server.</span><span class="sxs-lookup"><span data-stu-id="24d27-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="24d27-109">SignalR occuparsene di plumbing client-server.</span><span class="sxs-lookup"><span data-stu-id="24d27-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="24d27-110">SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="24d27-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="24d27-111">Per un'introduzione a SignalR, hub e le connessioni persistenti o per un'esercitazione che illustra come compilare un'applicazione SignalR completa, vedere [SignalR - Introduzione](index.md).</span><span class="sxs-lookup"><span data-stu-id="24d27-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="24d27-112">Panoramica</span><span class="sxs-lookup"><span data-stu-id="24d27-112">Overview</span></span>

<span data-ttu-id="24d27-113">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="24d27-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="24d27-114">Come registrare le route SignalR e configurare le opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="24d27-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="24d27-115">L'URL /signalr</span><span class="sxs-lookup"><span data-stu-id="24d27-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="24d27-116">Configurazione delle opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="24d27-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="24d27-117">Come creare e utilizzare le classi Hub</span><span class="sxs-lookup"><span data-stu-id="24d27-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="24d27-118">Durata dell'oggetto hub</span><span class="sxs-lookup"><span data-stu-id="24d27-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="24d27-119">Iniziali maiuscole e minuscole dei nomi di Hub nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="24d27-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="24d27-120">Hub di più</span><span class="sxs-lookup"><span data-stu-id="24d27-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="24d27-121">Come definire i metodi nella classe Hub che i client possono chiamare</span><span class="sxs-lookup"><span data-stu-id="24d27-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="24d27-122">Iniziali maiuscole e minuscole dei nomi di metodo nei client JavaScript</span><span class="sxs-lookup"><span data-stu-id="24d27-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="24d27-123">Quando è necessario eseguire in modo asincrono</span><span class="sxs-lookup"><span data-stu-id="24d27-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="24d27-124">Definizione di overload</span><span class="sxs-lookup"><span data-stu-id="24d27-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="24d27-125">Come chiamare i metodi della classe Hub client</span><span class="sxs-lookup"><span data-stu-id="24d27-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="24d27-126">Selezionare i client che riceverà il RPC</span><span class="sxs-lookup"><span data-stu-id="24d27-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="24d27-127">Nessuna convalida in fase di compilazione per i nomi di metodo</span><span class="sxs-lookup"><span data-stu-id="24d27-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="24d27-128">Corrispondenza dei nomi tra maiuscole e minuscole (metodo)</span><span class="sxs-lookup"><span data-stu-id="24d27-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="24d27-129">Esecuzione asincrona</span><span class="sxs-lookup"><span data-stu-id="24d27-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="24d27-130">Come gestire l'appartenenza al gruppo dalla classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="24d27-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="24d27-131">Esecuzione asincrona dei metodi Add e Remove</span><span class="sxs-lookup"><span data-stu-id="24d27-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="24d27-132">Persistenza l'appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="24d27-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="24d27-133">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="24d27-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="24d27-134">Come gestire gli eventi di durata connessione nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="24d27-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="24d27-135">Quando vengono chiamati OnConnected OnDisconnected e OnReconnected</span><span class="sxs-lookup"><span data-stu-id="24d27-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="24d27-136">Stato del chiamante non popolato</span><span class="sxs-lookup"><span data-stu-id="24d27-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="24d27-137">Come ottenere informazioni sul client dalla proprietà di contesto</span><span class="sxs-lookup"><span data-stu-id="24d27-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="24d27-138">Come passare lo stato tra client e la classe di Hub</span><span class="sxs-lookup"><span data-stu-id="24d27-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="24d27-139">Come gestire gli errori nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="24d27-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="24d27-140">Come chiamare metodi di client e gestire gruppi appartenenti all'esterno della classe di Hub</span><span class="sxs-lookup"><span data-stu-id="24d27-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="24d27-141">Chiamata di metodi di client</span><span class="sxs-lookup"><span data-stu-id="24d27-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="24d27-142">L'appartenenza al gruppo di gestione</span><span class="sxs-lookup"><span data-stu-id="24d27-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="24d27-143">Come attivare la traccia</span><span class="sxs-lookup"><span data-stu-id="24d27-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="24d27-144">Come personalizzare la pipeline di hub</span><span class="sxs-lookup"><span data-stu-id="24d27-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="24d27-145">Per la documentazione su come i client di programma, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="24d27-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="24d27-146">Guida di API degli hub SignalR - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="24d27-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="24d27-147">Guida di API degli hub SignalR - Client .NET</span><span class="sxs-lookup"><span data-stu-id="24d27-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="24d27-148">I collegamenti agli argomenti di riferimento API sono alla versione dell'API di .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="24d27-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="24d27-149">Se si utilizza .NET 4, vedere [la versione di .NET 4 degli argomenti API](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="24d27-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="24d27-150">Come registrare le route SignalR e configurare le opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="24d27-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="24d27-151">Per definire la route che i client utilizzeranno per connettersi all'Hub di, chiamare il [MapHubs](https://msdn.microsoft.com/en-us/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) metodo all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="24d27-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/en-us/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="24d27-152">`MapHubs`è un [metodo di estensione](https://msdn.microsoft.com/en-us/library/vstudio/bb383977.aspx) per la `System.Web.Routing.RouteCollection` classe.</span><span class="sxs-lookup"><span data-stu-id="24d27-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/en-us/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="24d27-153">Nell'esempio seguente viene illustrato come definire le route degli hub SignalR nella *Global. asax* file.</span><span class="sxs-lookup"><span data-stu-id="24d27-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="24d27-154">Se si aggiunge una funzionalità di SignalR a un'applicazione ASP.NET MVC, assicurarsi che la route SignalR viene aggiunto prima di altre route.</span><span class="sxs-lookup"><span data-stu-id="24d27-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="24d27-155">Per ulteriori informazioni, vedere [esercitazione: Introduzione a SignalR e MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="24d27-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="24d27-156">L'URL /signalr</span><span class="sxs-lookup"><span data-stu-id="24d27-156">The /signalr URL</span></span>

<span data-ttu-id="24d27-157">Per impostazione predefinita, l'URL di route che i client utilizzeranno per connettersi all'Hub di è "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="24d27-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="24d27-158">(Non confondere questo URL con l'URL "hub signalr /", ovvero per il file JavaScript generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="24d27-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="24d27-159">Per ulteriori informazioni su proxy generato, vedere [Guida API per gli hub SignalR - JavaScript Client, il proxy generato ed esegue automaticamente](index.md).)</span><span class="sxs-lookup"><span data-stu-id="24d27-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="24d27-160">Potrebbero esserci circostanze straordinarie che rendono l'URL di base non è utilizzabile per SignalR; ad esempio, si dispone di una cartella nel progetto denominato *signalr* e non si desidera modificare il nome.</span><span class="sxs-lookup"><span data-stu-id="24d27-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="24d27-161">In tal caso, è possibile modificare l'URL di base, come illustrato negli esempi riportati di seguito (sostituire "/ signalr" nel codice di esempio con l'URL desiderato).</span><span class="sxs-lookup"><span data-stu-id="24d27-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="24d27-162">**Codice che specifica l'URL del server**</span><span class="sxs-lookup"><span data-stu-id="24d27-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="24d27-163">**Codice client JavaScript che specifica l'URL (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="24d27-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="24d27-164">**Codice client JavaScript che specifica l'URL (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="24d27-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="24d27-165">**Codice client .NET che specifica l'URL**</span><span class="sxs-lookup"><span data-stu-id="24d27-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="24d27-166">Configurazione delle opzioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="24d27-166">Configuring SignalR Options</span></span>

<span data-ttu-id="24d27-167">Esegue l'overload di `MapHubs` metodo consentono di specificare un URL personalizzato, un resolver di dipendenza personalizzata e le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="24d27-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="24d27-168">Abilitare le chiamate tra domini dai client del browser.</span><span class="sxs-lookup"><span data-stu-id="24d27-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="24d27-169">In genere se il browser viene caricata una pagina da `http://contoso.com`, la connessione SignalR nello stesso dominio, si trova `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="24d27-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="24d27-170">Se la pagina `http://contoso.com` stabilisce una connessione a `http://fabrikam.com/signalr`, vale a dire una connessione tra domini.</span><span class="sxs-lookup"><span data-stu-id="24d27-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="24d27-171">Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="24d27-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="24d27-172">Per ulteriori informazioni, vedere [ASP.NET SignalR hub API Guida - JavaScript Client - come stabilire una connessione tra domini](index.md).</span><span class="sxs-lookup"><span data-stu-id="24d27-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="24d27-173">Abilitare i messaggi di errore dettagliati.</span><span class="sxs-lookup"><span data-stu-id="24d27-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="24d27-174">Quando si verificano errori, il comportamento predefinito di SignalR è per inviare ai client un messaggio di notifica senza informazioni dettagliate su cosa è successo.</span><span class="sxs-lookup"><span data-stu-id="24d27-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="24d27-175">L'invio di informazioni dettagliate sull'errore a client non è consigliato in produzione, perché gli utenti malintenzionati potrebbero essere in grado di utilizzare le informazioni per gli attacchi contro l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="24d27-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="24d27-176">Per risolvere il problema, è possibile utilizzare questa opzione per abilitare temporaneamente la segnalazione di errori più descrittivi.</span><span class="sxs-lookup"><span data-stu-id="24d27-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="24d27-177">Disabilitare i file di proxy JavaScript generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="24d27-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="24d27-178">Per impostazione predefinita, viene generato un file di JavaScript con proxy per le classi Hub in risposta all'URL di "hub signalr /".</span><span class="sxs-lookup"><span data-stu-id="24d27-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="24d27-179">Se non si desidera utilizzare il proxy JavaScript o se si desidera generare manualmente questo file e fare riferimento a un file fisico nel client, è possibile utilizzare questa opzione per disabilitare la generazione di proxy.</span><span class="sxs-lookup"><span data-stu-id="24d27-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="24d27-180">Per ulteriori informazioni, vedere [Guida di API degli hub SignalR - JavaScript Client, come creare un file fisico per SignalR generato proxy](index.md).</span><span class="sxs-lookup"><span data-stu-id="24d27-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="24d27-181">Nell'esempio seguente viene illustrato come specificare l'URL di connessione SignalR e queste opzioni in una chiamata al `MapHubs` metodo.</span><span class="sxs-lookup"><span data-stu-id="24d27-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="24d27-182">Per specificare un URL personalizzato, sostituire "/ signalr" nell'esempio con l'URL a cui si desidera utilizzare.</span><span class="sxs-lookup"><span data-stu-id="24d27-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="24d27-183">Come creare e utilizzare le classi Hub</span><span class="sxs-lookup"><span data-stu-id="24d27-183">How to create and use Hub classes</span></span>

<span data-ttu-id="24d27-184">Per creare un Hub, creare una classe che deriva da [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="24d27-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="24d27-185">Nell'esempio seguente viene illustrata una classe semplice di Hub per un'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="24d27-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="24d27-186">In questo esempio, è possibile chiamare un client connesso il `NewContosoChatMessage` (metodo), e in questo caso, i dati ricevuti trasmissione a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="24d27-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="24d27-187">Durata dell'oggetto hub</span><span class="sxs-lookup"><span data-stu-id="24d27-187">Hub object lifetime</span></span>

<span data-ttu-id="24d27-188">Non creare un'istanza della classe Hub o chiamare i metodi dal codice personalizzato sul server. tutto ciò che viene eseguita automaticamente dalla pipeline di hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="24d27-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="24d27-189">SignalR crea una nuova istanza della classe Hub ogni volta che è necessario per gestire un'operazione di Hub, ad esempio quando un client si connette, si disconnette o effettua un chiamata di metodo per il server.</span><span class="sxs-lookup"><span data-stu-id="24d27-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="24d27-190">Poiché le istanze della classe Hub sono temporanee, non possono essere utilizzati per mantenere lo stato da una chiamata al metodo successivo.</span><span class="sxs-lookup"><span data-stu-id="24d27-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="24d27-191">Ogni volta che il server riceve una chiamata al metodo da un client in una nuova istanza dei processi di classe Hub il messaggio.</span><span class="sxs-lookup"><span data-stu-id="24d27-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="24d27-192">Per mantenere lo stato attraverso più connessioni e chiamate al metodo, utilizzare un altro metodo, ad esempio un database o una variabile statica nella classe dell'Hub o un'altra classe che deriva da `Hub`.</span><span class="sxs-lookup"><span data-stu-id="24d27-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="24d27-193">Se si utilizzano dati in memoria, usando un metodo, ad esempio una variabile statica della classe di Hub, i dati andranno persi quando viene riciclato il dominio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="24d27-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="24d27-194">Se si desidera inviare messaggi ai client dal codice eseguito all'esterno della classe di Hub, è possibile eseguire questa operazione creando un'istanza della classe Hub, ma è possibile farlo tramite il recupero di un riferimento all'oggetto di contesto SignalR per la classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="24d27-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="24d27-195">Per ulteriori informazioni, vedere [come chiamare metodi di client e gestire gruppi appartenenti all'esterno della classe Hub](#callfromoutsidehub) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="24d27-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="24d27-196">Iniziali maiuscole e minuscole dei nomi di Hub nel client JavaScript</span><span class="sxs-lookup"><span data-stu-id="24d27-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="24d27-197">Per impostazione predefinita, i client JavaScript per fare riferimento a un hub utilizzando una versione di maiuscole/minuscole camel del nome della classe.</span><span class="sxs-lookup"><span data-stu-id="24d27-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="24d27-198">SignalR viene automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="24d27-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="24d27-199">Nell'esempio precedente potrebbe essere indicata come `contosoChatHub` nel codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="24d27-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="24d27-200">**Server**</span><span class="sxs-lookup"><span data-stu-id="24d27-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="24d27-201">**Client JavaScript mediante il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="24d27-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="24d27-202">Se si desidera specificare un nome diverso per i client da utilizzare, aggiungere il `HubName` attributo.</span><span class="sxs-lookup"><span data-stu-id="24d27-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="24d27-203">Quando si utilizza un `HubName` attributo, non è presente alcuna modifica nome maiuscole-minuscole camel nei client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="24d27-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="24d27-204">**Server**</span><span class="sxs-lookup"><span data-stu-id="24d27-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="24d27-205">**Client JavaScript mediante il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="24d27-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="24d27-206">Hub di più</span><span class="sxs-lookup"><span data-stu-id="24d27-206">Multiple Hubs</span></span>

<span data-ttu-id="24d27-207">In un'applicazione, è possibile definire più classi di Hub.</span><span class="sxs-lookup"><span data-stu-id="24d27-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="24d27-208">Quando a tale scopo, la connessione è condivisa, ma i gruppi sono separati:</span><span class="sxs-lookup"><span data-stu-id="24d27-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="24d27-209">Tutti i client utilizzeranno lo stesso URL per stabilire una connessione SignalR con il servizio ("/ signalr" o l'URL personalizzato se è stata specificata una), e viene utilizzata per tutti gli hub di connessione definite dal servizio.</span><span class="sxs-lookup"><span data-stu-id="24d27-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="24d27-210">Non vi è alcuna differenza nelle prestazioni per gli hub di più rispetto alla definizione di tutte le funzionalità di Hub in una singola classe.</span><span class="sxs-lookup"><span data-stu-id="24d27-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="24d27-211">Tutti gli hub di ottenere le stesse informazioni di richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="24d27-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="24d27-212">Poiché tutti gli hub condividono la stessa connessione, l'unica informazione richiesta HTTP che ottiene il server è ciò che viene fornito nella richiesta HTTP originale che stabilisce la connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="24d27-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="24d27-213">Se si utilizza la richiesta di connessione per passare le informazioni dal client al server, specificando una stringa di query, è possibile fornire le stringhe di query diversi agli hub diverso.</span><span class="sxs-lookup"><span data-stu-id="24d27-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="24d27-214">Tutti gli hub riceverà le stesse informazioni.</span><span class="sxs-lookup"><span data-stu-id="24d27-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="24d27-215">Il file proxy JavaScript generato conterrà proxy per tutti gli hub in un file.</span><span class="sxs-lookup"><span data-stu-id="24d27-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="24d27-216">Per informazioni sul proxy di JavaScript, vedere [Guida API per gli hub SignalR - JavaScript Client, il proxy generato ed esegue automaticamente](index.md).</span><span class="sxs-lookup"><span data-stu-id="24d27-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="24d27-217">I gruppi sono definiti all'interno di hub.</span><span class="sxs-lookup"><span data-stu-id="24d27-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="24d27-218">In SignalR che è possibile definire gruppi per la trasmissione di sottoinsiemi di client connessi denominati.</span><span class="sxs-lookup"><span data-stu-id="24d27-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="24d27-219">I gruppi vengono gestiti separatamente per ogni Hub.</span><span class="sxs-lookup"><span data-stu-id="24d27-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="24d27-220">Ad esempio, un gruppo denominato "Administrators" include un set di client per il `ContosoChatHub` classe e lo stesso nome di gruppo, fare riferimento a un set diverso di client per la `StockTickerHub` classe.</span><span class="sxs-lookup"><span data-stu-id="24d27-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="24d27-221">Come definire i metodi nella classe Hub che i client possono chiamare</span><span class="sxs-lookup"><span data-stu-id="24d27-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="24d27-222">Per esporre un metodo dell'hub che si desidera essere chiamato dal client, è possibile dichiarare un metodo pubblico, come illustrato negli esempi seguenti.</span><span class="sxs-lookup"><span data-stu-id="24d27-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="24d27-223">È possibile specificare un tipo restituito e parametri, inclusi i tipi complessi e matrici, esattamente come si farebbe con qualsiasi metodo c#.</span><span class="sxs-lookup"><span data-stu-id="24d27-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="24d27-224">Tutti i dati ricevuti nei parametri o restituire al chiamante viene comunicati tra il client e il server usando JSON e SignalR gestisce automaticamente l'associazione di oggetti complessi e matrici di oggetti.</span><span class="sxs-lookup"><span data-stu-id="24d27-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="24d27-225">Iniziali maiuscole e minuscole dei nomi di metodo nei client JavaScript</span><span class="sxs-lookup"><span data-stu-id="24d27-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="24d27-226">Per impostazione predefinita, i client JavaScript per fare riferimento ai metodi di Hub utilizzando una versione di maiuscole/minuscole camel del nome del metodo.</span><span class="sxs-lookup"><span data-stu-id="24d27-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="24d27-227">SignalR viene automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="24d27-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="24d27-228">**Server**</span><span class="sxs-lookup"><span data-stu-id="24d27-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="24d27-229">**Client JavaScript mediante il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="24d27-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="24d27-230">Se si desidera specificare un nome diverso per i client da utilizzare, aggiungere il `HubMethodName` attributo.</span><span class="sxs-lookup"><span data-stu-id="24d27-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="24d27-231">**Server**</span><span class="sxs-lookup"><span data-stu-id="24d27-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="24d27-232">**Client JavaScript mediante il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="24d27-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="24d27-233">Quando è necessario eseguire in modo asincrono</span><span class="sxs-lookup"><span data-stu-id="24d27-233">When to execute asynchronously</span></span>

<span data-ttu-id="24d27-234">Se verrà essere a esecuzione prolungata o deve utilizzare il metodo che verrebbe implicano in attesa, ad esempio una ricerca nel database o una chiamata al servizio web, il metodo dell'Hub asincrona restituendo un [attività](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) (invece di `void` restituire) o [ Attività&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/dd321424.aspx) oggetto (invece di `T` tipo restituito).</span><span class="sxs-lookup"><span data-stu-id="24d27-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/en-us/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="24d27-235">Quando viene restituito un `Task` oggetto dal metodo, SignalR attende la `Task` per completare, e quindi invia il risultato annullato il wrapping al client, pertanto non c'è alcuna differenza nella modalità in cui la chiamata al metodo nel client codice.</span><span class="sxs-lookup"><span data-stu-id="24d27-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="24d27-236">Effettua un metodo dell'Hub asincrona si evita di bloccare la connessione quando utilizza il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="24d27-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="24d27-237">Quando un metodo dell'Hub esegue in modo sincrono e il trasporto WebSocket, le successive chiamate dei metodi dell'hub dallo stesso client vengono bloccate fino al completamento del metodo dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="24d27-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="24d27-238">L'esempio seguente viene illustrato lo stesso metodo codificate in modo da eseguire in modo sincrono o in modo asincrono, seguito dal codice client JavaScript che funziona per la chiamata a entrambe le versioni.</span><span class="sxs-lookup"><span data-stu-id="24d27-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="24d27-239">**Sincrono**</span><span class="sxs-lookup"><span data-stu-id="24d27-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="24d27-240">**Asincrona - ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="24d27-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="24d27-241">**Client JavaScript mediante il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="24d27-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="24d27-242">Per ulteriori informazioni su come usare i metodi asincroni in ASP.NET 4.5, vedere [utilizzando metodi asincroni in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="24d27-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="24d27-243">Definizione di overload</span><span class="sxs-lookup"><span data-stu-id="24d27-243">Defining Overloads</span></span>

<span data-ttu-id="24d27-244">Se si desidera definire gli overload per un metodo, il numero di parametri in ogni overload deve essere diverso.</span><span class="sxs-lookup"><span data-stu-id="24d27-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="24d27-245">Se un overload si differenzia solo specificando i tipi di parametri diversi, la classe Hub verrà compilati ma il servizio SignalR genererà un'eccezione in fase di esecuzione quando i client tentano di chiamare uno degli overload.</span><span class="sxs-lookup"><span data-stu-id="24d27-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="24d27-246">Come chiamare i metodi della classe Hub client</span><span class="sxs-lookup"><span data-stu-id="24d27-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="24d27-247">Per chiamare i metodi client dal server, utilizzare il `Clients` proprietà in un metodo nella classe di Hub.</span><span class="sxs-lookup"><span data-stu-id="24d27-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="24d27-248">L'esempio seguente mostra il codice che chiama server `addNewMessageToPage` su tutti i client e il codice client che definisce il metodo in un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="24d27-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="24d27-249">**Server**</span><span class="sxs-lookup"><span data-stu-id="24d27-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="24d27-250">**Client JavaScript mediante il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="24d27-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="24d27-251">È possibile ottenere un valore restituito da un metodo client; la sintassi `int x = Clients.All.add(1,1)` non funziona.</span><span class="sxs-lookup"><span data-stu-id="24d27-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="24d27-252">È possibile specificare i tipi complessi e matrici per i parametri.</span><span class="sxs-lookup"><span data-stu-id="24d27-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="24d27-253">Nell'esempio seguente passa un tipo complesso al client in un parametro di metodo.</span><span class="sxs-lookup"><span data-stu-id="24d27-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="24d27-254">**Codice del server che chiama un metodo client utilizzando un oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="24d27-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="24d27-255">**Codice del server che definisce l'oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="24d27-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="24d27-256">**Client JavaScript mediante il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="24d27-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="24d27-257">Selezionare i client che riceverà il RPC</span><span class="sxs-lookup"><span data-stu-id="24d27-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="24d27-258">La proprietà restituisce ai client un [HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) oggetto che fornisce diverse opzioni per specificare che i client riceveranno il RPC:</span><span class="sxs-lookup"><span data-stu-id="24d27-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="24d27-259">Tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="24d27-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="24d27-260">Solo il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="24d27-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="24d27-261">Tutti i client, eccetto il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="24d27-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="24d27-262">Un client specifico identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="24d27-263">Questo esempio viene chiamato `addContosoChatMessageToPage` sul client chiamante e ha lo stesso effetto dell'utilizzo `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="24d27-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="24d27-264">Tutti i client connessi eccetto il client specificati, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="24d27-265">Tutti i client in un gruppo specificato.</span><span class="sxs-lookup"><span data-stu-id="24d27-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="24d27-266">Tutti i client connessi in un gruppo specificato eccetto il client specificati, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="24d27-267">Tutti i client connessi in un gruppo specificato eccetto il client chiamante.</span><span class="sxs-lookup"><span data-stu-id="24d27-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="24d27-268">Nessuna convalida in fase di compilazione per i nomi di metodo</span><span class="sxs-lookup"><span data-stu-id="24d27-268">No compile-time validation for method names</span></span>

<span data-ttu-id="24d27-269">Il nome del metodo specificato viene interpretato come un oggetto dinamico, ovvero che nessuna IntelliSense o la relativa convalida in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="24d27-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="24d27-270">L'espressione viene valutata in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="24d27-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="24d27-271">Quando viene eseguita la chiamata al metodo, SignalR invia il nome del metodo e i valori dei parametri per il client e se il client dispone di un metodo che corrisponde al nome, la chiamata a metodo e i valori dei parametri vengono passati al metodo.</span><span class="sxs-lookup"><span data-stu-id="24d27-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="24d27-272">Se viene trovato alcun metodo di corrispondenza nel client, viene generato alcun errore.</span><span class="sxs-lookup"><span data-stu-id="24d27-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="24d27-273">Per informazioni sul formato dei dati SignalR trasmette al client in background quando si chiama un metodo client, vedere [Introduzione a SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="24d27-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="24d27-274">Corrispondenza dei nomi tra maiuscole e minuscole (metodo)</span><span class="sxs-lookup"><span data-stu-id="24d27-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="24d27-275">Nome di metodo corrispondente è tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="24d27-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="24d27-276">Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, o `addContosoChatMessageToPage` sul client.</span><span class="sxs-lookup"><span data-stu-id="24d27-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="24d27-277">Esecuzione asincrona</span><span class="sxs-lookup"><span data-stu-id="24d27-277">Asynchronous execution</span></span>

<span data-ttu-id="24d27-278">Il metodo chiamato viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="24d27-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="24d27-279">Qualsiasi codice che viene fornito dopo una chiamata al metodo a un client verrà eseguita immediatamente senza attendere SignalR completare la trasmissione dei dati per i client a meno che non si specifica che le successive righe di codice deve attendere il completamento di metodo.</span><span class="sxs-lookup"><span data-stu-id="24d27-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="24d27-280">Gli esempi di codice seguente viene illustrato come eseguire i due metodi di client in modo sequenziale, uno usando il codice che funziona in .NET 4.5 e uno usando il codice che funziona in .NET 4.</span><span class="sxs-lookup"><span data-stu-id="24d27-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="24d27-281">**Esempio di .NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="24d27-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="24d27-282">**Esempio 4 di .NET**</span><span class="sxs-lookup"><span data-stu-id="24d27-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="24d27-283">Se si utilizza `await` o `ContinueWith` per l'attesa fino al completamento di un metodo client prima che la riga successiva del codice viene eseguito, questo non significa che i client effettivamente riceverà il messaggio prima dell'esecuzione successiva riga di codice.</span><span class="sxs-lookup"><span data-stu-id="24d27-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="24d27-284">"Completamento" di una chiamata al metodo client significa solo che abbia eseguito tutto il necessario per inviare il messaggio SignalR.</span><span class="sxs-lookup"><span data-stu-id="24d27-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="24d27-285">Se è necessario che i client ha ricevuto il messaggio di verifica, è necessario programmare manualmente tale meccanismo.</span><span class="sxs-lookup"><span data-stu-id="24d27-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="24d27-286">Ad esempio, è possibile codificare una `MessageReceived` metodo dell'hub e nel `addContosoChatMessageToPage` metodo sul client è possibile chiamare `MessageReceived` dopo aver eseguito l'elemento di lavoro è necessario eseguire sul client.</span><span class="sxs-lookup"><span data-stu-id="24d27-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="24d27-287">In `MessageReceived` nell'Hub è possibile eseguire le operazioni dipende dalla ricezione client effettivo e l'elaborazione della chiamata al metodo originale.</span><span class="sxs-lookup"><span data-stu-id="24d27-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="24d27-288">Come utilizzare una variabile di stringa come il nome del metodo</span><span class="sxs-lookup"><span data-stu-id="24d27-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="24d27-289">Se si desidera richiamare un metodo client utilizzando una variabile di stringa come il nome del metodo, eseguire il cast `Clients.All` (o `Clients.Others`, `Clients.Caller`e così via) a `IClientProxy` e quindi chiamare [Invoke (NomeMetodo, args...) ](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="24d27-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="24d27-290">Come gestire l'appartenenza al gruppo dalla classe dell'Hub</span><span class="sxs-lookup"><span data-stu-id="24d27-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="24d27-291">Gruppi di SignalR forniscono un metodo per la trasmissione di messaggi a un subset specificato di client connessi.</span><span class="sxs-lookup"><span data-stu-id="24d27-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="24d27-292">Un gruppo può avere qualsiasi numero di client e un client può essere un membro di un numero qualsiasi di gruppi.</span><span class="sxs-lookup"><span data-stu-id="24d27-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="24d27-293">Per gestire l'appartenenza al gruppo, utilizzare il [Aggiungi](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) e [rimuovere](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metodi forniti dal `Groups` proprietà della classe di Hub.</span><span class="sxs-lookup"><span data-stu-id="24d27-293">To manage group membership, use the [Add](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="24d27-294">Nell'esempio seguente il `Groups.Add` e `Groups.Remove` metodi usati nei metodi dell'Hub che vengono chiamati dal codice client, seguito dal codice client JavaScript che li chiama.</span><span class="sxs-lookup"><span data-stu-id="24d27-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="24d27-295">**Server**</span><span class="sxs-lookup"><span data-stu-id="24d27-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="24d27-296">**Client JavaScript mediante il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="24d27-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="24d27-297">Non è necessario creare in modo esplicito gruppi.</span><span class="sxs-lookup"><span data-stu-id="24d27-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="24d27-298">In effetti un gruppo viene creato automaticamente la prima volta, specificare il nome in una chiamata a `Groups.Add`, e viene eliminata quando si rimuove l'ultima connessione dall'appartenenza in essa contenuti.</span><span class="sxs-lookup"><span data-stu-id="24d27-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="24d27-299">Non vi è alcuna API per ottenere un elenco di appartenenze di gruppo oppure un elenco di gruppi.</span><span class="sxs-lookup"><span data-stu-id="24d27-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="24d27-300">SignalR invia messaggi al client e i gruppi in base a un [modello pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), e il server non gestisce gli elenchi di gruppi o appartenenza al gruppo.</span><span class="sxs-lookup"><span data-stu-id="24d27-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="24d27-301">Ciò consente di ottimizzare la scalabilità, poiché ogni volta che si aggiunge un nodo a una web farm, qualsiasi stato che gestisce SignalR deve essere propagata nel nuovo nodo.</span><span class="sxs-lookup"><span data-stu-id="24d27-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="24d27-302">Esecuzione asincrona dei metodi Add e Remove</span><span class="sxs-lookup"><span data-stu-id="24d27-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="24d27-303">Il `Groups.Add` e `Groups.Remove` metodi eseguire in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="24d27-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="24d27-304">Se si desidera aggiungere un client a un gruppo e invia immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il `Groups.Add` metodo termina per prima.</span><span class="sxs-lookup"><span data-stu-id="24d27-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="24d27-305">Gli esempi di codice seguente viene illustrato come eseguire questa operazione, utilizzando il codice funzioni in .NET 4.5 e uno usando il codice funzioni in .NET 4</span><span class="sxs-lookup"><span data-stu-id="24d27-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="24d27-306">**Esempio di .NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="24d27-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="24d27-307">**Esempio 4 di .NET**</span><span class="sxs-lookup"><span data-stu-id="24d27-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="24d27-308">Persistenza l'appartenenza al gruppo</span><span class="sxs-lookup"><span data-stu-id="24d27-308">Group membership persistence</span></span>

<span data-ttu-id="24d27-309">SignalR tiene traccia delle connessioni, non gli utenti, pertanto, se si desidera che un utente a essere nello stesso gruppo ogni volta che l'utente stabilisce una connessione, è necessario chiamare `Groups.Add` ogni volta che l'utente definisce una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="24d27-310">Dopo una perdita temporanea della connettività, talvolta SignalR possibile ripristinare la connessione automaticamente.</span><span class="sxs-lookup"><span data-stu-id="24d27-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="24d27-311">In tal caso, SignalR in corso il ripristino la stessa connessione, senza definire una nuova connessione e quindi ripristinato automaticamente l'appartenenza al gruppo del client.</span><span class="sxs-lookup"><span data-stu-id="24d27-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="24d27-312">Questo vale anche quando l'interruzione temporanea è il risultato di un errore, o il riavvio del server poiché lo stato di connessione per ogni client, inclusa l'appartenenza al gruppo, è sottoposto a round trip al client.</span><span class="sxs-lookup"><span data-stu-id="24d27-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="24d27-313">Se un server si arresta e viene sostituito da un nuovo server prima del timeout della connessione, un client possa riconnettersi automaticamente al nuovo server e registrare di nuovo in è un membro dei gruppi.</span><span class="sxs-lookup"><span data-stu-id="24d27-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="24d27-314">Quando una connessione non può essere ripristinata automaticamente dopo una perdita di connettività, o quando il timeout della connessione o quando il client si disconnette (ad esempio, quando un browser passa a una nuova pagina), l'appartenenza al gruppo viene persi.</span><span class="sxs-lookup"><span data-stu-id="24d27-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="24d27-315">Alla successiva si connette l'utente sarà una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="24d27-316">Per gestire l'appartenenza ai gruppi quando lo stesso utente stabilisce una nuova connessione, l'applicazione deve rilevare le associazioni tra utenti e gruppi e il ripristino di appartenenza al gruppo ogni volta che un utente stabilisce una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="24d27-317">Per ulteriori informazioni sulle connessioni e riconnessione, vedere [come gestire gli eventi di durata connessione nella classe Hub](#connectionlifetime) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="24d27-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="24d27-318">Gruppi utente singolo</span><span class="sxs-lookup"><span data-stu-id="24d27-318">Single-user groups</span></span>

<span data-ttu-id="24d27-319">Le applicazioni che utilizzano in genere SignalR sono necessario tenere traccia delle associazioni tra utenti e le connessioni per sapere quale utente ha inviato un messaggio e che gli utenti dovrebbero ricevere un messaggio.</span><span class="sxs-lookup"><span data-stu-id="24d27-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="24d27-320">Gruppi vengono usati in uno dei due modelli di usati comune per questo scopo.</span><span class="sxs-lookup"><span data-stu-id="24d27-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="24d27-321">Gruppi utente singolo.</span><span class="sxs-lookup"><span data-stu-id="24d27-321">Single-user groups.</span></span>

    <span data-ttu-id="24d27-322">È possibile specificare il nome utente come il nome del gruppo e aggiungere l'ID di connessione corrente al gruppo ogni volta che l'utente si connette o si riconnette.</span><span class="sxs-lookup"><span data-stu-id="24d27-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="24d27-323">Per inviare messaggi all'utente di inviare il gruppo.</span><span class="sxs-lookup"><span data-stu-id="24d27-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="24d27-324">Uno svantaggio di questo metodo è che il gruppo non offre un modo per sapere se l'utente è online o offline.</span><span class="sxs-lookup"><span data-stu-id="24d27-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="24d27-325">Rilevare le associazioni tra i nomi utente e ID connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="24d27-326">È possibile archiviare un'associazione tra ogni nome utente e la connessione di uno o più ID in un database o un dizionario e aggiornare i dati archiviati ogni volta che l'utente si connette o disconnette.</span><span class="sxs-lookup"><span data-stu-id="24d27-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="24d27-327">Per inviare messaggi all'utente specificare l'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="24d27-328">Uno svantaggio di questo metodo è che occorre una maggiore quantità di memoria.</span><span class="sxs-lookup"><span data-stu-id="24d27-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="24d27-329">Come gestire gli eventi di durata connessione nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="24d27-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="24d27-330">Cause comuni di gestione degli eventi di durata connessione sono di tenere traccia se un utente è connesso o meno e di tenere traccia dell'associazione tra i nomi utente e ID connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="24d27-331">Per eseguire codice quando i client di connettono o disconnessione, eseguire l'override di `OnConnected`, `OnDisconnected`, e `OnReconnected` metodi virtuali dell'Hub di classe, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="24d27-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="24d27-332">Quando vengono chiamati OnConnected OnDisconnected e OnReconnected</span><span class="sxs-lookup"><span data-stu-id="24d27-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="24d27-333">Ogni volta che una si accede a una nuova pagina, una nuova connessione deve essere elaborato, ovvero SignalR eseguirà il `OnDisconnected` metodo aggiungendo il `OnConnected` (metodo).</span><span class="sxs-lookup"><span data-stu-id="24d27-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="24d27-334">SignalR crea sempre un nuovo ID di connessione quando viene stabilita una nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="24d27-335">Il `OnReconnected` metodo viene chiamato quando si verifica un'interruzione temporanea della connettività che SignalR è possibile recuperare automaticamente da, ad esempio quando un cavo è temporaneamente disconnesso e riconnesso prima del timeout della connessione. Il `OnDisconnected` metodo viene chiamato quando il client è disconnesso e SignalR non è possibile riconnettersi automaticamente, ad esempio quando un si accede a una nuova pagina.</span><span class="sxs-lookup"><span data-stu-id="24d27-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="24d27-336">Pertanto, una possibile sequenza di eventi per un determinato client è `OnConnected`, `OnReconnected`, `OnDisconnected`; o `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="24d27-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="24d27-337">Non verrà visualizzata la sequenza `OnConnected`, `OnDisconnected`, `OnReconnected` per una determinata connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="24d27-338">Il `OnDisconnected` metodo non può essere chiamato in alcuni scenari, ad esempio quando si arresta un server o il dominio dell'applicazione ottiene riciclato.</span><span class="sxs-lookup"><span data-stu-id="24d27-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="24d27-339">Quando un altro server nella riga o il dominio dell'applicazione viene completato il riciclo, alcuni client potrebbero essere in grado di riconnettersi e generare il `OnReconnected` evento.</span><span class="sxs-lookup"><span data-stu-id="24d27-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="24d27-340">Per ulteriori informazioni, vedere [comprensione e gestione degli eventi di durata connessione in SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="24d27-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="24d27-341">Stato del chiamante non popolato</span><span class="sxs-lookup"><span data-stu-id="24d27-341">Caller state not populated</span></span>

<span data-ttu-id="24d27-342">Vengono chiamati i metodi del gestore eventi Durata connessione dal server, il che significa che qualsiasi stato che si inserisce nel `state` oggetto nel client non viene popolata nel `Caller` proprietà sul server.</span><span class="sxs-lookup"><span data-stu-id="24d27-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="24d27-343">Per informazioni sul `state` oggetto e `Caller` proprietà, vedere [come passare lo stato tra client e la classe Hub](#passstate) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="24d27-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="24d27-344">Come ottenere informazioni sul client dalla proprietà di contesto</span><span class="sxs-lookup"><span data-stu-id="24d27-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="24d27-345">Per ottenere informazioni sul client, utilizzare il `Context` proprietà della classe di Hub.</span><span class="sxs-lookup"><span data-stu-id="24d27-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="24d27-346">Il `Context` proprietà restituisce un [HubCallerContext](https://msdn.microsoft.com/en-us/library/jj890883(v=vs.111).aspx) oggetto che fornisce l'accesso alle informazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="24d27-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/en-us/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="24d27-347">L'ID di connessione del client chiamante.</span><span class="sxs-lookup"><span data-stu-id="24d27-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="24d27-348">L'ID di connessione è un GUID assegnato da SignalR (è possibile specificare il valore nel codice).</span><span class="sxs-lookup"><span data-stu-id="24d27-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="24d27-349">È un ID di connessione per ogni connessione e la stessa connessione che ID viene utilizzato da tutti gli hub, se si dispone di più hub nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="24d27-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="24d27-350">Dati dell'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="24d27-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="24d27-351">È inoltre possibile ottenere le intestazioni HTTP dalla `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="24d27-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="24d27-352">Il motivo più riferimenti allo stesso elemento è che `Context.Headers` è stato creato prima di tutto, il `Context.Request` proprietà è stata aggiunta in un secondo momento, e `Context.Headers` è stata mantenuta per compatibilità con le versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="24d27-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="24d27-353">Eseguire query sui dati di stringa.</span><span class="sxs-lookup"><span data-stu-id="24d27-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="24d27-354">È inoltre possibile ottenere dati di stringa di query da `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="24d27-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="24d27-355">La stringa di query che si ottiene in questa proprietà è quella utilizzata con la richiesta HTTP che stabilito la connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="24d27-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="24d27-356">È possibile aggiungere parametri di stringa di query nel client mediante la configurazione della connessione, che è un modo pratico per passare i dati sul client dal client al server.</span><span class="sxs-lookup"><span data-stu-id="24d27-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="24d27-357">Nell'esempio seguente viene illustrato un modo per aggiungere una stringa di query in un client JavaScript, quando si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="24d27-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="24d27-358">Per ulteriori informazioni sull'impostazione di parametri di stringa di query, vedere le guide di API per la [JavaScript](index.md) e [.NET](index.md) client.</span><span class="sxs-lookup"><span data-stu-id="24d27-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="24d27-359">È possibile trovare il metodo di trasporto utilizzato per la connessione dei dati di stringa di query, con alcuni altri valori utilizzati internamente da SignalR:</span><span class="sxs-lookup"><span data-stu-id="24d27-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="24d27-360">Il valore di `transportMethod` sarà "WebSocket", "serverSentEvents", "foreverFrame" o "longPolling".</span><span class="sxs-lookup"><span data-stu-id="24d27-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="24d27-361">Si noti che se si archivia il valore di `OnConnected` metodo del gestore eventi, in alcuni scenari è inizialmente potrebbe ricevere un valore di trasporto che non è il metodo di trasporto negoziata finale per la connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="24d27-362">In tal caso, il metodo genererà un'eccezione e verrà chiamato nuovamente in un secondo momento quando il metodo di trasporto finale viene stabilito.</span><span class="sxs-lookup"><span data-stu-id="24d27-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="24d27-363">Cookie.</span><span class="sxs-lookup"><span data-stu-id="24d27-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="24d27-364">È inoltre possibile ottenere i cookie dal `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="24d27-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="24d27-365">Informazioni utente.</span><span class="sxs-lookup"><span data-stu-id="24d27-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="24d27-366">L'oggetto HttpContext per la richiesta:</span><span class="sxs-lookup"><span data-stu-id="24d27-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="24d27-367">Utilizzare questo metodo anziché `HttpContext.Current` per ottenere il `HttpContext` oggetto per la connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="24d27-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="24d27-368">Come passare lo stato tra client e la classe di Hub</span><span class="sxs-lookup"><span data-stu-id="24d27-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="24d27-369">Il proxy client fornisce un `state` oggetto in cui è possibile archiviare i dati che si desidera essere trasmessi al server con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="24d27-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="24d27-370">Nel server è possibile accedere a questi dati nel `Clients.Caller` proprietà nei metodi dell'Hub che vengono chiamati dal client.</span><span class="sxs-lookup"><span data-stu-id="24d27-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="24d27-371">Il `Clients.Caller` proprietà non viene popolata per i metodi del gestore eventi Durata connessione `OnConnected`, `OnDisconnected`, e `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="24d27-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="24d27-372">La creazione o aggiornamento dei dati nel `state` oggetto e `Clients.Caller` proprietà viene utilizzata in entrambe le direzioni.</span><span class="sxs-lookup"><span data-stu-id="24d27-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="24d27-373">È possibile aggiornare i valori nel server e vengono passati al client.</span><span class="sxs-lookup"><span data-stu-id="24d27-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="24d27-374">Nell'esempio seguente viene illustrato il codice client JavaScript che archivia lo stato per la trasmissione al server con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="24d27-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="24d27-375">Nell'esempio seguente viene illustrato l'equivalente del codice in un client .NET.</span><span class="sxs-lookup"><span data-stu-id="24d27-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="24d27-376">Nella classe Hub, è possibile accedere a questi dati nel `Clients.Caller` proprietà.</span><span class="sxs-lookup"><span data-stu-id="24d27-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="24d27-377">Nell'esempio seguente viene illustrato il codice che recupera lo stato in cui all'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="24d27-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="24d27-378">Questo meccanismo di persistenza dello stato non è destinato grandi quantità di dati, perché tutto ciò che si inserisce nella `state` o `Clients.Caller` proprietà è sottoposto a round trip con ogni chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="24d27-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="24d27-379">È utile per gli elementi più piccoli, ad esempio nomi utente o i contatori.</span><span class="sxs-lookup"><span data-stu-id="24d27-379">It's useful for smaller items such as user names or counters.</span></span>


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="24d27-380">Come gestire gli errori nella classe Hub</span><span class="sxs-lookup"><span data-stu-id="24d27-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="24d27-381">Per gestire gli errori che si verificano nei metodi di classe di Hub, utilizzare uno o entrambi i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="24d27-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="24d27-382">Eseguire il wrapping del codice del metodo in blocchi try-catch e accedere all'oggetto eccezione.</span><span class="sxs-lookup"><span data-stu-id="24d27-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="24d27-383">Ai fini del debug è possibile inviare l'eccezione al client, ma per la sicurezza non sono consigliabile motivi l'invio di informazioni dettagliate per i client nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="24d27-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="24d27-384">Creare un modulo di pipeline hub che gestisce il [OnIncomingError](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) metodo.</span><span class="sxs-lookup"><span data-stu-id="24d27-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="24d27-385">Nell'esempio seguente viene illustrato un modulo di pipeline che registra gli errori, seguiti dal codice in Global. asax che inserisce il modulo nella pipeline di hub.</span><span class="sxs-lookup"><span data-stu-id="24d27-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="24d27-386">Per ulteriori informazioni sui moduli della pipeline di Hub, vedere [come personalizzare la pipeline di hub](#hubpipeline) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="24d27-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="24d27-387">Come attivare la traccia</span><span class="sxs-lookup"><span data-stu-id="24d27-387">How to enable tracing</span></span>

<span data-ttu-id="24d27-388">Per abilitare la traccia sul lato server, aggiungere un elemento System. Diagnostics nel file Web. config, come illustrato in questo esempio:</span><span class="sxs-lookup"><span data-stu-id="24d27-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="24d27-389">Quando si esegue l'applicazione in Visual Studio, è possibile visualizzare i log di **Output** finestra.</span><span class="sxs-lookup"><span data-stu-id="24d27-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="24d27-390">Come chiamare metodi di client e gestire gruppi appartenenti all'esterno della classe di Hub</span><span class="sxs-lookup"><span data-stu-id="24d27-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="24d27-391">Per chiamare metodi di client da una classe diversa rispetto a classe Hub, ottenere un riferimento all'oggetto di contesto SignalR per l'Hub e utilizzarlo per chiamare metodi sul client o gestire i gruppi.</span><span class="sxs-lookup"><span data-stu-id="24d27-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="24d27-392">L'esempio seguente `StockTicker` classe ottiene l'oggetto di contesto, viene memorizzato in un'istanza della classe, archivia l'istanza della classe in una proprietà statica e Usa il contesto dell'istanza di classe singleton per chiamare il `updateStockPrice` nei client che sono (metodo) connesso a un Hub denominato `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="24d27-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="24d27-393">Se è necessario utilizzare i contesto più-volte in un oggetto di lunga durato, ottenere il riferimento a una sola volta e salvare anziché essere nuovamente ogni volta.</span><span class="sxs-lookup"><span data-stu-id="24d27-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="24d27-394">Ottenere il contesto di una volta assicura che SignalR invia messaggi ai client nella stessa sequenza in cui i metodi dell'Hub rendere client chiamate al metodo.</span><span class="sxs-lookup"><span data-stu-id="24d27-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="24d27-395">Per un'esercitazione che illustra come utilizzare il contesto di SignalR per un Hub, vedere [Server Broadcast con ASP.NET SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="24d27-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="24d27-396">Chiamata di metodi di client</span><span class="sxs-lookup"><span data-stu-id="24d27-396">Calling client methods</span></span>

<span data-ttu-id="24d27-397">È possibile specificare che i client riceveranno il RPC, ma sono disponibili meno opzioni rispetto a quando si chiama da una classe di Hub.</span><span class="sxs-lookup"><span data-stu-id="24d27-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="24d27-398">Il motivo è che il contesto non è associato a una determinata chiamata da un client, pertanto tutti i metodi che richiedono la conoscenza dell'ID di connessione corrente, ad esempio `Clients.Others`, o `Clients.Caller`, o `Clients.OthersInGroup`, non sono disponibili.</span><span class="sxs-lookup"><span data-stu-id="24d27-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="24d27-399">Sono disponibili le seguenti opzioni:</span><span class="sxs-lookup"><span data-stu-id="24d27-399">The following options are available:</span></span>

- <span data-ttu-id="24d27-400">Tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="24d27-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="24d27-401">Un client specifico identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="24d27-402">Tutti i client connessi eccetto il client specificati, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="24d27-403">Tutti i client in un gruppo specificato.</span><span class="sxs-lookup"><span data-stu-id="24d27-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="24d27-404">Tutti i client in un gruppo specificato, eccetto il client specificati, identificato dall'ID di connessione.</span><span class="sxs-lookup"><span data-stu-id="24d27-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="24d27-405">Se si chiama la classe non Hub dai metodi nella classe di Hub, è possibile passare l'ID di connessione corrente e utilizzarla con `Clients.Client`, `Clients.AllExcept`, o `Clients.Group` per simulare `Clients.Caller`, `Clients.Others`, o `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="24d27-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="24d27-406">Nell'esempio seguente, il `MoveShapeHub` classe passa l'ID di connessione per il `Broadcaster` classe in modo che il `Broadcaster` possibile simulare la classe `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="24d27-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="24d27-407">L'appartenenza al gruppo di gestione</span><span class="sxs-lookup"><span data-stu-id="24d27-407">Managing group membership</span></span>

<span data-ttu-id="24d27-408">Per gestire i gruppi sono disponibili le opzioni stesso come in una classe di Hub.</span><span class="sxs-lookup"><span data-stu-id="24d27-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="24d27-409">Aggiungere un client a un gruppo</span><span class="sxs-lookup"><span data-stu-id="24d27-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="24d27-410">Rimuovere un client da un gruppo</span><span class="sxs-lookup"><span data-stu-id="24d27-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="24d27-411">Come personalizzare la pipeline di hub</span><span class="sxs-lookup"><span data-stu-id="24d27-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="24d27-412">SignalR consente di inserire codice personalizzato nella pipeline dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="24d27-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="24d27-413">Nell'esempio seguente viene illustrato un modulo di pipeline Hub personalizzato che registra ogni chiamata al metodo in ingresso ricevuto dal client e in uscita chiamata al metodo richiamato nel client:</span><span class="sxs-lookup"><span data-stu-id="24d27-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="24d27-414">Nell'esempio di codice il *Global. asax* file registra il modulo per l'esecuzione della pipeline di Hub:</span><span class="sxs-lookup"><span data-stu-id="24d27-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="24d27-415">Esistono diversi metodi che è possibile eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="24d27-415">There are many different methods that you can override.</span></span> <span data-ttu-id="24d27-416">Per un elenco completo, vedere [HubPipelineModule metodi](https://msdn.microsoft.com/en-us/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="24d27-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/en-us/library/jj918633(v=vs.111).aspx).</span></span>
