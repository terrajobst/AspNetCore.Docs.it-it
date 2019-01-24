---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: SignalR 1.x Guida all'API Hubs - Client JavaScript | Microsoft Docs
author: bradygaster
description: Questo documento viene fornita un'introduzione all'uso dell'API di hub per SignalR versione 1.1 nel client JavaScript, ad esempio browser e Windows Store (WinJS) UT...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: eb40648ca06adcceaa613ba86abfcf7459369c7e
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836753"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="fd0db-103">SignalR 1.x Guida all'API Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="fd0db-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="fd0db-104">dal [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="fd0db-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="fd0db-105">Questo documento fornisce un'introduzione all'uso dell'API di hub per SignalR versione 1.1 nel client JavaScript, ad esempio browser e applicazioni Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="fd0db-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="fd0db-106">L'API di hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="fd0db-107">Nel codice server, si definiscono i metodi che possono essere chiamati da parte dei client e si chiamano i metodi eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="fd0db-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="fd0db-108">Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi che eseguono sul server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="fd0db-109">SignalR si occupa di tutte le attività client-server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="fd0db-110">SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="fd0db-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="fd0db-111">Per un'introduzione a SignalR hub e connessioni permanenti o per un'esercitazione che illustra come compilare un'applicazione di SignalR completa, vedere [SignalR - Guida introduttiva](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="fd0db-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="fd0db-112">Panoramica</span><span class="sxs-lookup"><span data-stu-id="fd0db-112">Overview</span></span>

<span data-ttu-id="fd0db-113">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="fd0db-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="fd0db-114">Il proxy generato e che cosa fa per te</span><span class="sxs-lookup"><span data-stu-id="fd0db-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="fd0db-115">Quando usare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="fd0db-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="fd0db-116">Programma di installazione client</span><span class="sxs-lookup"><span data-stu-id="fd0db-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="fd0db-117">Come fanno riferimento al proxy generato in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="fd0db-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="fd0db-118">Come creare un file fisico per SignalR generato proxy</span><span class="sxs-lookup"><span data-stu-id="fd0db-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="fd0db-119">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="fd0db-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="fd0db-120">$. connection.hub è lo stesso oggetto consente di creare tale $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="fd0db-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="fd0db-121">Esecuzione asincrona del metodo start</span><span class="sxs-lookup"><span data-stu-id="fd0db-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="fd0db-122">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="fd0db-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="fd0db-123">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="fd0db-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="fd0db-124">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="fd0db-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="fd0db-125">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="fd0db-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="fd0db-126">Come ottenere un proxy per una classe Hub</span><span class="sxs-lookup"><span data-stu-id="fd0db-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="fd0db-127">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="fd0db-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="fd0db-128">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="fd0db-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="fd0db-129">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="fd0db-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="fd0db-130">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="fd0db-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="fd0db-131">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="fd0db-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="fd0db-132">Per informazioni su come programmare il server o client .NET, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="fd0db-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="fd0db-133">Guida all'API di SignalR Hubs - Server</span><span class="sxs-lookup"><span data-stu-id="fd0db-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="fd0db-134">Guida all'API di SignalR Hubs - Client .NET</span><span class="sxs-lookup"><span data-stu-id="fd0db-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="fd0db-135">I collegamenti agli argomenti di riferimento all'API sono alla versione dell'API .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="fd0db-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="fd0db-136">Se si usa .NET 4, vedere [la versione di .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="fd0db-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="fd0db-137">Il proxy generato e che cosa fa per te</span><span class="sxs-lookup"><span data-stu-id="fd0db-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="fd0db-138">È possibile programmare un client JavaScript per comunicare con un servizio di SignalR con o senza un proxy che SignalR genera automaticamente.</span><span class="sxs-lookup"><span data-stu-id="fd0db-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="fd0db-139">Ciò che esegue il proxy per l'utente è semplificare la sintassi del codice usato per connettersi, metodi di scrittura che chiama il server, e chiamare metodi sul server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="fd0db-140">Quando si scrive codice per chiamare i metodi di server, il proxy generato consente di usare la sintassi che sembra si erano in esecuzione una funzione locale: è possibile scrivere `serverMethod(arg1, arg2)` invece di `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="fd0db-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="fd0db-141">La sintassi del proxy generato consente inoltre di un errore lato client immediato e comprensibile se si digita un nome di metodo server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="fd0db-142">E se si crea manualmente il file che definisce i proxy, è anche possibile ottenere supporto IntelliSense per la scrittura di codice che chiama i metodi di server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="fd0db-143">Ad esempio, si supponga di che avere la seguente classe dell'Hub sul server:</span><span class="sxs-lookup"><span data-stu-id="fd0db-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="fd0db-144">Gli esempi di codice seguenti mostrano cosa JavaScript codice simile per richiamare il `NewContosoChatMessage` su server e ricevere le chiamate del metodo di `addContosoChatMessageToPage` metodo dal server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="fd0db-145">**Con il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="fd0db-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="fd0db-146">**Senza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="fd0db-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="fd0db-147">Quando usare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="fd0db-147">When to use the generated proxy</span></span>

<span data-ttu-id="fd0db-148">Se si vuole registrare più gestori di eventi per un metodo di client che chiama il server, è possibile usare il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="fd0db-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="fd0db-149">In caso contrario, è possibile scegliere di usare il proxy generato o non in base alle proprie preferenze di scrittura del codice.</span><span class="sxs-lookup"><span data-stu-id="fd0db-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="fd0db-150">Se si sceglie di non usarla, non è necessario fare riferimento all'URL "/ degli hub signalr" in un `script` elemento nel codice client.</span><span class="sxs-lookup"><span data-stu-id="fd0db-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="fd0db-151">Programma di installazione client</span><span class="sxs-lookup"><span data-stu-id="fd0db-151">Client setup</span></span>

<span data-ttu-id="fd0db-152">Un client JavaScript vengono richiesti riferimenti ai file SignalR core JavaScript e jQuery.</span><span class="sxs-lookup"><span data-stu-id="fd0db-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="fd0db-153">La versione di jQuery deve essere 1.6.4 o versioni successive principali, ad esempio 1.7.2, 1.8.2 o 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="fd0db-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="fd0db-154">Se si decide di usare il proxy generato, è necessario anche un riferimento al proxy di SignalR generati file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd0db-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="fd0db-155">Nell'esempio seguente mostra i riferimenti all'aspetto in una pagina HTML che usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="fd0db-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="fd0db-156">Questi riferimenti devono essere incluso in questo ordine: jQuery, SignalR core in seguito e i proxy di SignalR cognome.</span><span class="sxs-lookup"><span data-stu-id="fd0db-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="fd0db-157">Come fanno riferimento al proxy generato in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="fd0db-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="fd0db-158">Nell'esempio precedente, il riferimento al proxy di SignalR generato è al codice JavaScript generato in modo dinamico, non a un file fisico.</span><span class="sxs-lookup"><span data-stu-id="fd0db-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="fd0db-159">SignalR crea il codice JavaScript per il proxy in tempo reale e li fornisce ai client in risposta all'URL "/ signalr/hub".</span><span class="sxs-lookup"><span data-stu-id="fd0db-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="fd0db-160">Se è specificato un URL di base diversi per le connessioni SignalR nel server di `MapHubs` metodo, l'URL del file proxy generato dinamicamente corrisponde all'URL personalizzato con "/ hub" aggiunto a tale.</span><span class="sxs-lookup"><span data-stu-id="fd0db-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="fd0db-161">Per i client Windows 8 (Windows Store) in JavaScript, usare il file fisico proxy anziché in quello generato in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="fd0db-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="fd0db-162">Per altre informazioni, vedere [come creare un file fisico per SignalR generato proxy](#manualproxy) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="fd0db-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="fd0db-163">In una visualizzazione Razor di ASP.NET MVC 4, usare il carattere tilde per fare riferimento alla radice dell'applicazione nel riferimento file proxy:</span><span class="sxs-lookup"><span data-stu-id="fd0db-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="fd0db-164">Per altre informazioni sull'uso di SignalR in MVC 4, vedere [Introduzione a SignalR e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="fd0db-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="fd0db-165">In una visualizzazione Razor di ASP.NET MVC 3, usare `Url.Content` relativo al riferimento al file proxy:</span><span class="sxs-lookup"><span data-stu-id="fd0db-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="fd0db-166">In un'applicazione Web Form ASP.NET, usare `ResolveClientUrl` per il proxy di riferimento di file o la registrazione tramite ScriptManager usando un percorso relativo della radice app (a partire da una tilde):</span><span class="sxs-lookup"><span data-stu-id="fd0db-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="fd0db-167">Come regola generale, usare lo stesso metodo per specificare l'URL "/ signalr/hub" che è usato per i file CSS o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd0db-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="fd0db-168">Se si specifica un URL senza usare una tilde, in alcuni scenari l'applicazione funzionerà correttamente quando si test in Visual Studio con IIS Express ma avrà esito negativo con un errore 404 durante la distribuzione in modalità IIS completa.</span><span class="sxs-lookup"><span data-stu-id="fd0db-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="fd0db-169">Per altre informazioni, vedere **risoluzione dei riferimenti alle risorse a livello di radice** nelle [i server Web in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) nel sito MSDN.</span><span class="sxs-lookup"><span data-stu-id="fd0db-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="fd0db-170">Quando si esegue un progetto web in Visual Studio 2012 in modalità di debug e se si utilizza Internet Explorer come browser, è possibile visualizzare il file proxy nella **Esplora soluzioni** sotto **documenti Script**, come illustrato di figura seguente.</span><span class="sxs-lookup"><span data-stu-id="fd0db-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![File proxy generato di JavaScript in Esplora soluzioni](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="fd0db-172">Per visualizzare il contenuto del file, fare doppio clic su **hub**.</span><span class="sxs-lookup"><span data-stu-id="fd0db-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="fd0db-173">Se non si usa Visual Studio 2012 e Internet Explorer, o se non si è in modalità di debug, è anche possibile ottenere il contenuto del file passando all'URL "/ signalR/hub".</span><span class="sxs-lookup"><span data-stu-id="fd0db-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="fd0db-174">Ad esempio, se il sito è in esecuzione in `http://localhost:56699`, passare a `http://localhost:56699/SignalR/hubs` nel browser.</span><span class="sxs-lookup"><span data-stu-id="fd0db-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="fd0db-175">Come creare un file fisico per SignalR generato proxy</span><span class="sxs-lookup"><span data-stu-id="fd0db-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="fd0db-176">Come alternativa per il proxy generato dinamicamente, è possibile creare un file fisico contenente il codice proxy e fare riferimento a tale file.</span><span class="sxs-lookup"><span data-stu-id="fd0db-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="fd0db-177">È possibile eseguire questa operazione per un controllo sulla memorizzazione nella cache o creazione di bundle comportamento o per ottenere IntelliSense quando si esegue la codifica le chiamate ai metodi di server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="fd0db-178">Per creare un file proxy, seguire i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="fd0db-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="fd0db-179">Installare il [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="fd0db-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="fd0db-180">Aprire un prompt dei comandi e passare al *strumenti* cartella che contiene il file SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="fd0db-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="fd0db-181">La cartella strumenti si trova nella posizione seguente:</span><span class="sxs-lookup"><span data-stu-id="fd0db-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="fd0db-182">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="fd0db-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="fd0db-183">Il percorso per il *. dll* è in genere il *bin* cartella nella cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="fd0db-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="fd0db-184">Questo comando crea un file denominato *server. js* nella stessa cartella *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="fd0db-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="fd0db-185">Inserire il *server. js* file in una cartella appropriata nel progetto, rinominarlo in modo appropriato per l'applicazione, quindi aggiungere un riferimento a esso al posto del riferimento "/ degli hub signalr".</span><span class="sxs-lookup"><span data-stu-id="fd0db-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="fd0db-186">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="fd0db-186">How to establish a connection</span></span>

<span data-ttu-id="fd0db-187">Prima di stabilire una connessione, è necessario creare un oggetto di connessione, creare un proxy e registrare i gestori eventi per metodi che possono essere chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="fd0db-188">Quando sono configurati i proxy e gestori eventi, è possibile stabilire la connessione tramite una chiamata di `start` (metodo).</span><span class="sxs-lookup"><span data-stu-id="fd0db-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="fd0db-189">Se si usa il proxy generato, non è necessario creare l'oggetto di connessione nel codice perché il codice proxy generato esegue tutto automaticamente.</span><span class="sxs-lookup"><span data-stu-id="fd0db-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="fd0db-190">**Stabilire una connessione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="fd0db-191">**Stabilire una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="fd0db-192">Il codice di esempio Usa il valore predefinito "/ signalr" URL per la connessione al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="fd0db-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="fd0db-193">Per informazioni su come specificare un URL di base diversi, vedere [Guida all'API di hub di ASP.NET SignalR - Server - URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="fd0db-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="fd0db-194">In genere si registrano i gestori di eventi prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="fd0db-195">Se si vuole registrare alcuni gestori di eventi dopo avere stabilito la connessione, è possibile farlo, ma è necessario registrare almeno una dei gestori di eventi prima di chiamare il `start` (metodo).</span><span class="sxs-lookup"><span data-stu-id="fd0db-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="fd0db-196">Uno dei motivi è che possono essere presenti molti hub in un'applicazione, ma è preferibile attivare il `OnConnected` evento su ogni Hub se solo si intende utilizzare uno di essi.</span><span class="sxs-lookup"><span data-stu-id="fd0db-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="fd0db-197">Quando viene stabilita la connessione, la presenza di un metodo client nel proxy dell'Hub è che indica a SignalR per attivare il `OnConnected` evento.</span><span class="sxs-lookup"><span data-stu-id="fd0db-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="fd0db-198">Se non si registrano i gestori di eventi prima di chiamare il `start` metodo, sarà in grado di richiamare i metodi per l'Hub, ma l'Hub `OnConnected` non chiamare il metodo e non verrà richiamato Nessun metodo client dal server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="fd0db-199">$. connection.hub è lo stesso oggetto consente di creare tale $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="fd0db-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="fd0db-200">Come si può notare dagli esempi, quando si usa il proxy generato, `$.connection.hub` fa riferimento all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="fd0db-201">Questo è lo stesso oggetto che si ottiene chiamando `$.hubConnection()` quando non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="fd0db-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="fd0db-202">Il codice proxy generato crea la connessione tramite l'istruzione seguente:</span><span class="sxs-lookup"><span data-stu-id="fd0db-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Creazione di una connessione nel file proxy generato](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="fd0db-204">Quando si usa il proxy generato, è possibile eseguire qualsiasi operazione con `$.connection.hub` che è possibile eseguire con un oggetto connessione quando non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="fd0db-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="fd0db-205">Esecuzione asincrona del metodo start</span><span class="sxs-lookup"><span data-stu-id="fd0db-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="fd0db-206">Il `start` metodo viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="fd0db-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="fd0db-207">Restituisce un [oggetto Deferred jQuery](http://api.jquery.com/category/deferred-object/), il che significa che è possibile aggiungere le funzioni di callback da chiamare metodi quali `pipe`, `done`, e `fail`.</span><span class="sxs-lookup"><span data-stu-id="fd0db-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="fd0db-208">Se si dispone di codice che si desidera eseguire dopo aver stabilita la connessione, ad esempio una chiamata a un metodo server, tale codice inserito in una funzione di callback o chiamarlo da una funzione di callback.</span><span class="sxs-lookup"><span data-stu-id="fd0db-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="fd0db-209">Il `.done` metodo di callback viene eseguito dopo aver stabilita la connessione e dopo qualsiasi codice che è necessario `OnConnected` metodo del gestore eventi nel server al termine dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="fd0db-210">Se si inserisce l'istruzione "Ora connessi" dell'esempio precedente alla riga successiva del codice dopo il `start` chiamata al metodo (non in un `.done` callback), il `console.log` riga verrà eseguita prima che venga stabilita la connessione, come illustrato nell'esempio seguente esempio:</span><span class="sxs-lookup"><span data-stu-id="fd0db-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Modo errato per scrivere codice che viene eseguito una volta stabilita connessione](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="fd0db-212">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="fd0db-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="fd0db-213">In genere se il browser ha caricato una pagina dalla `http://contoso.com`, la connessione SignalR è nello stesso dominio, in `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="fd0db-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="fd0db-214">Se la pagina dalla `http://contoso.com` stabilisce una connessione a `http://fabrikam.com/signalr`, vale a dire una connessione tra domini.</span><span class="sxs-lookup"><span data-stu-id="fd0db-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="fd0db-215">Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="fd0db-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="fd0db-216">Per stabilire una connessione tra domini, assicurarsi che le connessioni tra domini sono abilitate nel server e specificare l'URL di connessione quando si crea l'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="fd0db-217">SignalR userà la tecnologia appropriata per le connessioni tra domini, ad esempio [JSONP](http://en.wikipedia.org/wiki/JSONP) oppure [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="fd0db-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="fd0db-218">Nel server, abilitare le connessioni tra domini selezionando questa opzione quando si chiama il `MapHubs` (metodo).</span><span class="sxs-lookup"><span data-stu-id="fd0db-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="fd0db-219">Sul client, specificare l'URL quando si crea l'oggetto di connessione (senza il proxy generato) o prima di chiamare il metodo start (con il proxy generato).</span><span class="sxs-lookup"><span data-stu-id="fd0db-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="fd0db-220">**Codice client che specifica una connessione cross-domain (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="fd0db-221">**Codice client che specifica una connessione cross-domain (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="fd0db-222">Quando si usa la `$.hubConnection` costruttore, non è necessario includere `signalr` nell'URL quanto viene aggiunto automaticamente (a meno che non si specifica `useDefaultUrl` come `false`).</span><span class="sxs-lookup"><span data-stu-id="fd0db-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="fd0db-223">È possibile creare più connessioni a endpoint diversi.</span><span class="sxs-lookup"><span data-stu-id="fd0db-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="fd0db-224">Non impostare `jQuery.support.cors` su true nel codice.</span><span class="sxs-lookup"><span data-stu-id="fd0db-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![Non impostare jQuery.support.cors su true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="fd0db-226">SignalR gestisce l'uso di JSONP o CORS.</span><span class="sxs-lookup"><span data-stu-id="fd0db-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="fd0db-227">Impostazione `jQuery.support.cors` a true Disabilita JSONP perché causa SignalR per presupporre il browser supporta la condivisione CORS.</span><span class="sxs-lookup"><span data-stu-id="fd0db-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="fd0db-228">Quando ci si connette a un URL localhost, Internet Explorer 10 non considera una connessione tra domini, in modo che l'applicazione funzionerà in locale con Internet Explorer 10 anche se non è stata abilitata la connessione tra domini nel server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="fd0db-229">Per informazioni sull'uso di connessioni tra domini con Internet Explorer 9, vedere [thread di StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="fd0db-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="fd0db-230">Per informazioni sull'uso di connessioni tra domini con Chrome, vedere [thread di StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="fd0db-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="fd0db-231">Il codice di esempio Usa il valore predefinito "/ signalr" URL per la connessione al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="fd0db-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="fd0db-232">Per informazioni su come specificare un URL di base diversi, vedere [Guida all'API di hub di ASP.NET SignalR - Server - URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="fd0db-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="fd0db-233">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="fd0db-233">How to configure the connection</span></span>

<span data-ttu-id="fd0db-234">Prima di aver stabilito una connessione, è possibile specificare parametri della stringa di query o specificare il metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="fd0db-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="fd0db-235">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="fd0db-235">How to specify query string parameters</span></span>

<span data-ttu-id="fd0db-236">Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri della stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="fd0db-237">Negli esempi seguenti illustrano come impostare un parametro di stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="fd0db-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="fd0db-238">**Impostare un valore di stringa di query prima di chiamare il metodo start (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="fd0db-239">**Impostare un valore di stringa di query prima di chiamare il metodo start (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="fd0db-240">Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="fd0db-241">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="fd0db-241">How to specify the transport method</span></span>

<span data-ttu-id="fd0db-242">Durante il processo di connessione, un client SignalR normalmente negozia con il server per determinare il migliore trasporto supportato dal server e client.</span><span class="sxs-lookup"><span data-stu-id="fd0db-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="fd0db-243">Se si conosce già il trasporto da usare, è possibile ignorare questo processo di negoziazione specificando il metodo di trasporto quando si chiama il `start` (metodo).</span><span class="sxs-lookup"><span data-stu-id="fd0db-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="fd0db-244">**Codice client che specifica il metodo di trasporto (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="fd0db-245">**Codice client che specifica il metodo di trasporto (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="fd0db-246">In alternativa, è possibile specificare più metodi di trasporto nell'ordine in cui si desidera SignalR per provarle in:</span><span class="sxs-lookup"><span data-stu-id="fd0db-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="fd0db-247">**Codice client che specifica uno schema di fallback di trasporto personalizzato (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="fd0db-248">**Codice client che specifica uno schema di fallback di trasporto personalizzato (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="fd0db-249">È possibile usare i valori seguenti per specificare il metodo di trasporto:</span><span class="sxs-lookup"><span data-stu-id="fd0db-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="fd0db-250">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="fd0db-250">"webSockets"</span></span>
- <span data-ttu-id="fd0db-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="fd0db-251">"foreverFrame"</span></span>
- <span data-ttu-id="fd0db-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="fd0db-252">"serverSentEvents"</span></span>
- <span data-ttu-id="fd0db-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="fd0db-253">"longPolling"</span></span>

<span data-ttu-id="fd0db-254">Gli esempi seguenti illustrano come determinare quale metodo di trasporto viene utilizzato da una connessione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="fd0db-255">**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="fd0db-256">**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="fd0db-257">Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [- Server - Guida all'API Hubs SignalR di ASP.NET come ottenere informazioni relative al client dalla proprietà di contesto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="fd0db-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="fd0db-258">Per altre informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR - trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="fd0db-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="fd0db-259">Come ottenere un proxy per una classe Hub</span><span class="sxs-lookup"><span data-stu-id="fd0db-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="fd0db-260">Ogni oggetto di connessione che crea incapsula informazioni su una connessione a un servizio SignalR contenente uno o più classi di Hub.</span><span class="sxs-lookup"><span data-stu-id="fd0db-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="fd0db-261">Per comunicare con una classe di Hub, usare un oggetto proxy che create dall'utente (se non si usa il proxy generato) o che viene generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="fd0db-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="fd0db-262">Nel client il nome del proxy è una versione con notazione camel del nome della classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="fd0db-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="fd0db-263">SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd0db-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="fd0db-264">**Classe dell'hub sul server**</span><span class="sxs-lookup"><span data-stu-id="fd0db-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="fd0db-265">**Ottenere un riferimento al proxy client generato per l'Hub**</span><span class="sxs-lookup"><span data-stu-id="fd0db-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="fd0db-266">**Creare il proxy client per la classe Hub (senza proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="fd0db-267">Se si decora la classe di Hub con un `HubName` attributo, usare il nome esatto senza modificare maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="fd0db-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="fd0db-268">**Classe dell'hub sul server con l'attributo HubName**</span><span class="sxs-lookup"><span data-stu-id="fd0db-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="fd0db-269">**Ottenere un riferimento al proxy client generato per l'Hub**</span><span class="sxs-lookup"><span data-stu-id="fd0db-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="fd0db-270">**Creare il proxy client per la classe Hub (senza proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="fd0db-271">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="fd0db-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="fd0db-272">Per definire un metodo che il server è possibile chiamare da un Hub, aggiungere un gestore eventi per il proxy dell'Hub usando il `client` proprietà del proxy generato o chiamata di `on` metodo se non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="fd0db-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="fd0db-273">I parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="fd0db-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="fd0db-274">Aggiungere il gestore dell'evento prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="fd0db-275">(Se si desidera aggiungere i gestori eventi dopo la chiamata di `start` metodo, vedere la nota nella [come stabilire una connessione](#establishconnection) in precedenza in questo documento e usare la sintassi illustrata per la definizione di un metodo senza usare il proxy generato.)</span><span class="sxs-lookup"><span data-stu-id="fd0db-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="fd0db-276">Metodo nome viene applicata alcuna distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="fd0db-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="fd0db-277">Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, o `addcontosochatmessagetopage` sul client.</span><span class="sxs-lookup"><span data-stu-id="fd0db-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="fd0db-278">**Definire un metodo nel client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="fd0db-279">**Modo alternativo per definire il metodo nel client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="fd0db-280">**Definire un metodo nel client (senza il proxy generato o durante l'aggiunta dopo aver chiamato il metodo di avvio)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="fd0db-281">**Codice del server che chiama il metodo client**</span><span class="sxs-lookup"><span data-stu-id="fd0db-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="fd0db-282">Negli esempi seguenti includono un oggetto complesso come un parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="fd0db-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="fd0db-283">**Definire un metodo sul client che accetta un oggetto complesso (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="fd0db-284">**Definire un metodo sul client che accetta un oggetto complesso (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="fd0db-285">**Codice del server che definisce l'oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="fd0db-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="fd0db-286">**Codice del server che chiama il metodo client utilizzando un oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="fd0db-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="fd0db-287">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="fd0db-287">How to call server methods from the client</span></span>

<span data-ttu-id="fd0db-288">Per chiamare un metodo server dal client, usare il `server` proprietà del proxy generato o `invoke` metodo sul proxy di Hub, se non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="fd0db-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="fd0db-289">Il valore restituito o i parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="fd0db-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="fd0db-290">Passare una versione maiuscole-minuscole camel del nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="fd0db-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="fd0db-291">SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fd0db-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="fd0db-292">Gli esempi seguenti illustrano come chiamare un metodo server che non ha un valore restituito e come chiamare un metodo del server che hanno un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="fd0db-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="fd0db-293">**Metodo server senza l'attributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="fd0db-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="fd0db-294">**Codice del server che definisce l'oggetto complesso passato un parametro**</span><span class="sxs-lookup"><span data-stu-id="fd0db-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="fd0db-295">**Codice client che richiama il metodo del server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="fd0db-296">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="fd0db-297">Se è decorata del metodo dell'Hub con un `HubMethodName` attributo, usare il nome senza modificare maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="fd0db-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="fd0db-298">**Metodo server** con un attributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="fd0db-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="fd0db-299">**Codice client che richiama il metodo del server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="fd0db-300">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="fd0db-301">Negli esempi precedenti viene illustrato come chiamare un metodo del server che non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="fd0db-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="fd0db-302">Negli esempi seguenti illustrano come chiamare un metodo server che ha un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="fd0db-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="fd0db-303">**Codice del server per un metodo che ha un valore restituito**</span><span class="sxs-lookup"><span data-stu-id="fd0db-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="fd0db-304">**La classe Stock utilizzata per il** valore restituito</span><span class="sxs-lookup"><span data-stu-id="fd0db-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="fd0db-305">**Codice client che richiama il metodo del server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="fd0db-306">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="fd0db-307">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="fd0db-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="fd0db-308">SignalR fornisce il collegamento seguente gli eventi di durata che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="fd0db-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="fd0db-309">`starting`: Generato prima che tutti i dati vengono inviati tramite la connessione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="fd0db-310">`received`: Generato quando vengono ricevuti tutti i dati nella connessione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="fd0db-311">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="fd0db-311">Provides the received data.</span></span>
- <span data-ttu-id="fd0db-312">`connectionSlow`: Generato quando il client rileva una connessione lenta o frequentemente eliminazione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="fd0db-313">`reconnecting`: Generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="fd0db-314">`reconnected`: Generato quando il trasporto sottostante è stata riconnessa.</span><span class="sxs-lookup"><span data-stu-id="fd0db-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="fd0db-315">`stateChanged`: Generato quando cambia lo stato della connessione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="fd0db-316">Fornisce lo stato precedente e il nuovo stato (connessione, connesso, riconnessione o Disconnected).</span><span class="sxs-lookup"><span data-stu-id="fd0db-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="fd0db-317">`disconnected`: Generato quando la connessione è disconnesso.</span><span class="sxs-lookup"><span data-stu-id="fd0db-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="fd0db-318">Ad esempio, se si desidera visualizzare i messaggi di avviso quando si verificano problemi di connessione che potrebbero causare ritardi notevoli, gestire il `connectionSlow` evento.</span><span class="sxs-lookup"><span data-stu-id="fd0db-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="fd0db-319">**Gestire l'evento connectionSlow (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="fd0db-320">**Gestire l'evento connectionSlow (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="fd0db-321">Per altre informazioni, vedere [comprensione e la gestione degli eventi di durata connessione in SignalR](index.md).</span><span class="sxs-lookup"><span data-stu-id="fd0db-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="fd0db-322">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="fd0db-322">How to handle errors</span></span>

<span data-ttu-id="fd0db-323">Il client SignalR JavaScript fornisce un `error` eventi che è possibile aggiungere un gestore per.</span><span class="sxs-lookup"><span data-stu-id="fd0db-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="fd0db-324">È anche possibile usare il metodo hanno esito negativo per aggiungere un gestore errori risultanti da una chiamata al metodo server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="fd0db-325">Se si non abilita in modo esplicito i messaggi di errore dettagliato nel server, l'oggetto eccezione SignalR restituito dopo un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="fd0db-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="fd0db-326">Ad esempio, se una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati per i client nell'ambiente di produzione non è consigliato per motivi di sicurezza, ma se si vuole abilitare i messaggi di errore dettagliato per risoluzione dei problemi, usare il codice seguente nel server.</span><span class="sxs-lookup"><span data-stu-id="fd0db-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="fd0db-327">Nell'esempio seguente viene illustrato come aggiungere un gestore per l'evento di errore.</span><span class="sxs-lookup"><span data-stu-id="fd0db-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="fd0db-328">**Aggiungere un gestore degli errori (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="fd0db-329">**Aggiungere un gestore degli errori (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="fd0db-330">Nell'esempio seguente viene illustrato come gestire un errore da una chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="fd0db-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="fd0db-331">**Gestire un errore da una chiamata al metodo (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="fd0db-332">**Gestire un errore da una chiamata al metodo (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="fd0db-333">Se una chiamata al metodo ha esito negativo, il `error` evento viene generato anche in modo che il codice nel `error` gestore del metodo e nel `.fail` metodo callback verrebbe eseguito.</span><span class="sxs-lookup"><span data-stu-id="fd0db-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="fd0db-334">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="fd0db-334">How to enable client-side logging</span></span>

<span data-ttu-id="fd0db-335">Per abilitare la registrazione lato client in una connessione, impostare il `logging` proprietà dell'oggetto di connessione prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="fd0db-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="fd0db-336">**Abilitare la registrazione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="fd0db-337">**Abilitare la registrazione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="fd0db-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="fd0db-338">Per visualizzare i log, aprire Strumenti di sviluppo del browser e passare alla scheda della Console. Per un'esercitazione che illustra le istruzioni dettagliate e schermata schermate che illustrano come eseguire questa operazione, vedere [trasmissione Server con ASP.NET Signalr - abilitare la registrazione](index.md).</span><span class="sxs-lookup"><span data-stu-id="fd0db-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
