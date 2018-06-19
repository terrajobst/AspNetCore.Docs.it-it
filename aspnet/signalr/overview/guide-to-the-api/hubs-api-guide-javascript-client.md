---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guida di ASP.NET SignalR hub API - Client JavaScript | Documenti Microsoft
author: pfletcher
description: Questo documento viene fornita un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client JavaScript, ad esempio i browser e applicat (WinJS) di Windows Store...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 794ab576d3c6600911f331bab7c335476e45a0c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28035335"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="4ff67-103">Guida di ASP.NET SignalR hub API - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="4ff67-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="4ff67-104">da [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4ff67-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="4ff67-105">Questo documento viene fornita un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client JavaScript, ad esempio i browser e le applicazioni Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="4ff67-105">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="4ff67-106">L'API degli hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="4ff67-107">Nel codice server, si definiscono i metodi che possono essere chiamati dai client e si chiamano metodi che eseguono il client.</span><span class="sxs-lookup"><span data-stu-id="4ff67-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="4ff67-108">Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi eseguiti nel server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="4ff67-109">SignalR occuparsene di plumbing client-server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="4ff67-110">SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="4ff67-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="4ff67-111">Per un'introduzione a SignalR, hub e le connessioni permanenti, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="4ff67-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="4ff67-112">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="4ff67-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="4ff67-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4ff67-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="4ff67-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4ff67-114">.NET 4.5</span></span>
> - <span data-ttu-id="4ff67-115">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="4ff67-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="4ff67-116">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="4ff67-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="4ff67-117">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="4ff67-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="4ff67-118">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="4ff67-118">Questions and comments</span></span>
> 
> <span data-ttu-id="4ff67-119">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="4ff67-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4ff67-120">In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="4ff67-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="4ff67-121">Panoramica</span><span class="sxs-lookup"><span data-stu-id="4ff67-121">Overview</span></span>

<span data-ttu-id="4ff67-122">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="4ff67-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="4ff67-123">Il proxy generato e funzionalità per l'utente</span><span class="sxs-lookup"><span data-stu-id="4ff67-123">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="4ff67-124">Quando utilizzare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="4ff67-124">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="4ff67-125">Installazione del client</span><span class="sxs-lookup"><span data-stu-id="4ff67-125">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="4ff67-126">Come fare riferimento il proxy generato dinamicamente</span><span class="sxs-lookup"><span data-stu-id="4ff67-126">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="4ff67-127">Come creare un file fisico per SignalR generato proxy</span><span class="sxs-lookup"><span data-stu-id="4ff67-127">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="4ff67-128">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="4ff67-128">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="4ff67-129">$. connection.hub è lo stesso oggetto tale $.hubConnection() Crea</span><span class="sxs-lookup"><span data-stu-id="4ff67-129">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="4ff67-130">Esecuzione asincrona del metodo start</span><span class="sxs-lookup"><span data-stu-id="4ff67-130">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="4ff67-131">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="4ff67-131">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="4ff67-132">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="4ff67-132">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="4ff67-133">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="4ff67-133">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="4ff67-134">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="4ff67-134">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="4ff67-135">Come ottenere un proxy per una classe di Hub</span><span class="sxs-lookup"><span data-stu-id="4ff67-135">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="4ff67-136">Come definire i metodi nel client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="4ff67-136">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="4ff67-137">Come chiamare i metodi di server dal client</span><span class="sxs-lookup"><span data-stu-id="4ff67-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="4ff67-138">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="4ff67-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="4ff67-139">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="4ff67-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="4ff67-140">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="4ff67-140">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="4ff67-141">Per la documentazione su come programmare il server o client .NET, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="4ff67-141">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="4ff67-142">Guida di API degli hub SignalR - Server</span><span class="sxs-lookup"><span data-stu-id="4ff67-142">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="4ff67-143">Guida di API degli hub SignalR - Client .NET</span><span class="sxs-lookup"><span data-stu-id="4ff67-143">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="4ff67-144">Il componente server di SignalR 2 disponibile solo in .NET 4.5 (se è disponibile un client .NET per SignalR 2 in .NET 4.0).</span><span class="sxs-lookup"><span data-stu-id="4ff67-144">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="4ff67-145">Il proxy generato e funzionalità per l'utente</span><span class="sxs-lookup"><span data-stu-id="4ff67-145">The generated proxy and what it does for you</span></span>

<span data-ttu-id="4ff67-146">È possibile programmare un client JavaScript per comunicare con un servizio SignalR con o senza un proxy che genera l'errore SignalR per l'utente.</span><span class="sxs-lookup"><span data-stu-id="4ff67-146">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="4ff67-147">Il proxy per l'utente si semplificano la sintassi del codice si utilizza per la connessione, i metodi di scrittura che il server chiama, e chiamare i metodi nel server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-147">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="4ff67-148">Quando si scrive codice per chiamare i metodi di server, il proxy generato consente di utilizzare la sintassi che viene visualizzato come se si erano in esecuzione di una funzione locale: è possibile scrivere `serverMethod(arg1, arg2)` anziché `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="4ff67-148">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="4ff67-149">La sintassi proxy generato consente inoltre di un errore sul lato client immediato e comprensibile se si digita un nome di metodo server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-149">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="4ff67-150">E, se si crea manualmente il file che definisce i proxy, è inoltre possibile ottenere il supporto IntelliSense per la scrittura di codice che chiama i metodi di server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-150">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="4ff67-151">Ad esempio, si supponga la seguente classe Hub sul server:</span><span class="sxs-lookup"><span data-stu-id="4ff67-151">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="4ff67-152">Gli esempi di codice seguente mostrano cosa JavaScript codice simile per richiamare il `NewContosoChatMessage` (metodo) nel server e le chiamate di ricezione il `addContosoChatMessageToPage` metodo dal server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-152">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="4ff67-153">**Con il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="4ff67-153">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="4ff67-154">**Senza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="4ff67-154">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="4ff67-155">Quando utilizzare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="4ff67-155">When to use the generated proxy</span></span>

<span data-ttu-id="4ff67-156">Se si desidera registrare più gestori di eventi per un metodo client che chiama il server, è possibile utilizzare il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="4ff67-156">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="4ff67-157">In caso contrario, è possibile scegliere di utilizzare il proxy generato o non in base alle preferenze di codifica.</span><span class="sxs-lookup"><span data-stu-id="4ff67-157">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="4ff67-158">Se si sceglie di non utilizzarla, non è necessario fare riferimento l'URL "o degli hub signalr" in un `script` elemento nel codice client.</span><span class="sxs-lookup"><span data-stu-id="4ff67-158">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="4ff67-159">Installazione del client</span><span class="sxs-lookup"><span data-stu-id="4ff67-159">Client setup</span></span>

<span data-ttu-id="4ff67-160">Un client JavaScript richiede riferimenti a jQuery e il file di JavaScript SignalR core.</span><span class="sxs-lookup"><span data-stu-id="4ff67-160">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="4ff67-161">La versione di jQuery deve essere 1.6.4 o versioni successive principali, ad esempio 1.7.2, 1.8.2 o 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="4ff67-161">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="4ff67-162">Se si decide di utilizzare il proxy generato, è necessario anche un riferimento al proxy SignalR generati file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4ff67-162">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="4ff67-163">Nell'esempio seguente viene illustrato l'aspetto i riferimenti in una pagina HTML che utilizza il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="4ff67-163">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="4ff67-164">Questi riferimenti devono essere incluso in questo ordine: jQuery, SignalR core in seguito, SignalR proxy e cognome.</span><span class="sxs-lookup"><span data-stu-id="4ff67-164">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="4ff67-165">Come fare riferimento il proxy generato dinamicamente</span><span class="sxs-lookup"><span data-stu-id="4ff67-165">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="4ff67-166">Nell'esempio precedente, il riferimento al proxy generato SignalR è a codice JavaScript generato dinamicamente, non a un file fisico.</span><span class="sxs-lookup"><span data-stu-id="4ff67-166">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="4ff67-167">SignalR crea il codice JavaScript per il proxy in tempo reale e gestisce i client in risposta all'URL di "hub signalr /".</span><span class="sxs-lookup"><span data-stu-id="4ff67-167">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="4ff67-168">Se è specificato un URL di base diversi per le connessioni SignalR nel server il `MapSignalR` (metodo), l'URL del file proxy generato dinamicamente è l'URL personalizzato con "/ hub" aggiunte.</span><span class="sxs-lookup"><span data-stu-id="4ff67-168">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="4ff67-169">Per i client JavaScript (Windows Store) di Windows 8, usare il file fisico del proxy anziché quello generato dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="4ff67-169">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="4ff67-170">Per ulteriori informazioni, vedere [come creare un file fisico per SignalR generato proxy](#manualproxy) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="4ff67-170">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="4ff67-171">In un ASP.NET MVC 4 o 5 visualizzazione Razor, utilizzare la tilde per fare riferimento alla radice dell'applicazione nel riferimento file proxy:</span><span class="sxs-lookup"><span data-stu-id="4ff67-171">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="4ff67-172">Per ulteriori informazioni sull'utilizzo di SignalR in MVC 5, vedere [Guida introduttiva a SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="4ff67-172">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="4ff67-173">In una visualizzazione Razor di ASP.NET MVC 3, utilizzare `Url.Content` per il riferimento al file proxy:</span><span class="sxs-lookup"><span data-stu-id="4ff67-173">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="4ff67-174">In un'applicazione Web Form ASP.NET, utilizzare `ResolveClientUrl` per il proxy di riferimento di file o la registrazione tramite lo ScriptManager usando un percorso relativo della radice app (inizia con una tilde):</span><span class="sxs-lookup"><span data-stu-id="4ff67-174">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="4ff67-175">Come regola generale, usare lo stesso metodo per specificare l'URL di "signalr/hub" usata per i file CSS o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4ff67-175">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="4ff67-176">Se si specifica un URL senza utilizzare una tilde, in alcuni scenari l'applicazione funzionerà correttamente quando si test in Visual Studio utilizzando IIS Express, ma avrà esito negativo con un errore 404, quando si distribuisce in IIS completo.</span><span class="sxs-lookup"><span data-stu-id="4ff67-176">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="4ff67-177">Per ulteriori informazioni, vedere **la risoluzione dei riferimenti alle risorse di livello radice** in [server Web in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) nel sito MSDN.</span><span class="sxs-lookup"><span data-stu-id="4ff67-177">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="4ff67-178">Quando si esegue un progetto web in Visual Studio 2013 in modalità di debug e se si utilizza Internet Explorer come browser, è possibile visualizzare il file proxy in **Esplora** in **documenti Script**, come illustrato di figura seguente.</span><span class="sxs-lookup"><span data-stu-id="4ff67-178">When you run a web project in Visual Studio 2013 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![File proxy generato JavaScript in Esplora soluzioni](hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="4ff67-180">Per visualizzare il contenuto del file, fare doppio clic su **hub**.</span><span class="sxs-lookup"><span data-stu-id="4ff67-180">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="4ff67-181">Se non si utilizza Internet Explorer e Visual Studio 2012 o 2013 o se non si è in modalità di debug, è anche possibile ottenere il contenuto del file passando all'URL di "hub signalR /".</span><span class="sxs-lookup"><span data-stu-id="4ff67-181">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="4ff67-182">Ad esempio, se il sito è in esecuzione in `http://localhost:56699`, passare a `http://localhost:56699/SignalR/hubs` nel browser.</span><span class="sxs-lookup"><span data-stu-id="4ff67-182">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="4ff67-183">Come creare un file fisico per SignalR generato proxy</span><span class="sxs-lookup"><span data-stu-id="4ff67-183">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="4ff67-184">In alternativa al proxy generato dinamicamente, è possibile creare un file fisico con il codice proxy e fare riferimento a tale file.</span><span class="sxs-lookup"><span data-stu-id="4ff67-184">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="4ff67-185">È possibile eseguire tale operazione per la memorizzazione nella cache o aggregazione di comportamento di un controllo o per ottenere IntelliSense quando si esegue la codifica di chiamate ai metodi di server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-185">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="4ff67-186">Per creare un file proxy, eseguire la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="4ff67-186">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="4ff67-187">Installare il [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="4ff67-187">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="4ff67-188">Aprire un prompt dei comandi e passare al *strumenti* cartella che contiene il file SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="4ff67-188">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="4ff67-189">La cartella strumenti si trova nella posizione seguente:</span><span class="sxs-lookup"><span data-stu-id="4ff67-189">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="4ff67-190">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="4ff67-190">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="4ff67-191">Il percorso del *DLL* è in genere il *bin* cartella nella cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="4ff67-191">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="4ff67-192">Questo comando crea un file denominato *server.js* nella stessa cartella *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="4ff67-192">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="4ff67-193">Inserire il *server.js* file in una cartella appropriata nel progetto, rinominarlo in modo appropriato per l'applicazione e aggiungere un riferimento al posto di riferimento "/ degli hub signalr".</span><span class="sxs-lookup"><span data-stu-id="4ff67-193">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="4ff67-194">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="4ff67-194">How to establish a connection</span></span>

<span data-ttu-id="4ff67-195">Prima di stabilire una connessione, è necessario creare un oggetto di connessione, creare un proxy e registra i gestori eventi per metodi che possono essere chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-195">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="4ff67-196">Quando i proxy e gestori eventi vengono impostati, è possibile stabilire la connessione tramite la chiamata di `start` metodo.</span><span class="sxs-lookup"><span data-stu-id="4ff67-196">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="4ff67-197">Se si utilizza il proxy generato, non è necessario creare l'oggetto di connessione nel codice perché il codice proxy generato non viene automaticamente.</span><span class="sxs-lookup"><span data-stu-id="4ff67-197">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="4ff67-198">**Stabilire una connessione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-198">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="4ff67-199">**Stabilire una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-199">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="4ff67-200">Il codice di esempio viene utilizzato il valore predefinito "/ signalr" URL per la connessione al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="4ff67-200">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="4ff67-201">Per informazioni su come specificare un URL di base diversi, vedere [ASP.NET SignalR hub API Guida - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="4ff67-201">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="4ff67-202">Per impostazione predefinita, il percorso di hub è il server corrente. Se ci si connette a un altro server, specificare l'URL prima di chiamare il `start` (metodo), come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="4ff67-202">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="4ff67-203">In genere si registrano i gestori di eventi prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="4ff67-203">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="4ff67-204">Se si desidera registrare gestori di eventi dopo avere stabilito la connessione, è possibile farlo, ma è necessario registrare almeno i gestori di eventi prima di chiamare il `start` metodo.</span><span class="sxs-lookup"><span data-stu-id="4ff67-204">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="4ff67-205">Un motivo è che in un'applicazione possono essere presenti molti hub, ma non si vuole attivare il `OnConnected` evento su ogni Hub, se si intende solo utilizzare per uno di essi.</span><span class="sxs-lookup"><span data-stu-id="4ff67-205">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="4ff67-206">Quando viene stabilita la connessione, la presenza di un metodo client nel proxy dell'Hub è cosa indica SignalR per attivare il `OnConnected` evento.</span><span class="sxs-lookup"><span data-stu-id="4ff67-206">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="4ff67-207">Se non si registrano i gestori di eventi prima di chiamare il `start` (metodo), sarà possibile richiamare metodi su Hub, ma l'Hub `OnConnected` non verrà chiamato e non verrà richiamato alcun metodo client dal server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-207">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="4ff67-208">$. connection.hub è lo stesso oggetto tale $.hubConnection() Crea</span><span class="sxs-lookup"><span data-stu-id="4ff67-208">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="4ff67-209">Come si può notare dagli esempi, quando si usa il proxy generato, `$.connection.hub` fa riferimento all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="4ff67-209">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="4ff67-210">Questo è lo stesso oggetto che si ottiene chiamando `$.hubConnection()` quando non si utilizza il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="4ff67-210">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="4ff67-211">Il codice proxy generato crea la connessione per l'utente eseguendo l'istruzione seguente:</span><span class="sxs-lookup"><span data-stu-id="4ff67-211">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Creazione di una connessione nel file proxy generato](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="4ff67-213">Quando si usa il proxy generato, è possibile eseguire alcuna operazione con `$.connection.hub` che è possibile eseguire con un oggetto di connessione quando non si usi il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="4ff67-213">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="4ff67-214">Esecuzione asincrona del metodo start</span><span class="sxs-lookup"><span data-stu-id="4ff67-214">Asynchronous execution of the start method</span></span>

<span data-ttu-id="4ff67-215">Il `start` metodo viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="4ff67-215">The `start` method executes asynchronously.</span></span> <span data-ttu-id="4ff67-216">Restituisce un [oggetto posticipata jQuery](http://api.jquery.com/category/deferred-object/), il che significa che è possibile aggiungere le funzioni di callback da chiamare metodi quali `pipe`, `done`, e `fail`.</span><span class="sxs-lookup"><span data-stu-id="4ff67-216">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="4ff67-217">Se si dispone di codice che si desidera eseguire dopo aver stabilita la connessione, ad esempio una chiamata a un metodo di server, inserire il codice in una funzione di callback o chiamarlo da una funzione di callback.</span><span class="sxs-lookup"><span data-stu-id="4ff67-217">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="4ff67-218">Il `.done` metodo di callback viene eseguito dopo aver stabilita la connessione e dopo qualsiasi codice che il `OnConnected` metodo del gestore eventi sul server al termine dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4ff67-218">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="4ff67-219">Se si inserisce l'istruzione "Connesso" dell'esempio precedente, come la riga successiva del codice dopo il `start` chiamata al metodo (non in un `.done` callback), il `console.log` riga verrà eseguita prima che venga stabilita la connessione, come illustrato nell'esempio seguente esempio:</span><span class="sxs-lookup"><span data-stu-id="4ff67-219">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Errato per la scrittura di codice che viene eseguito una volta stabilita connessione](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="4ff67-221">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="4ff67-221">How to establish a cross-domain connection</span></span>

<span data-ttu-id="4ff67-222">In genere se il browser viene caricata una pagina da `http://contoso.com`, la connessione SignalR nello stesso dominio, si trova `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="4ff67-222">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="4ff67-223">Se la pagina `http://contoso.com` stabilisce una connessione a `http://fabrikam.com/signalr`, vale a dire una connessione tra domini.</span><span class="sxs-lookup"><span data-stu-id="4ff67-223">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="4ff67-224">Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4ff67-224">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="4ff67-225">In SignalR 1. x, richieste tra domini sono stati controllati da un flag EnableCrossDomain singolo.</span><span class="sxs-lookup"><span data-stu-id="4ff67-225">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="4ff67-226">Questo flag è controllato da JSONP sia CORS richieste.</span><span class="sxs-lookup"><span data-stu-id="4ff67-226">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="4ff67-227">Per una maggiore flessibilità, il supporto CORS tutti è stato rimosso dal componente server di SignalR (i client JavaScript comunque usano CORS in genere se è stato rilevato che il browser supporti), e nuovi middleware OWIN è stato reso disponibile per supportare questi scenari.</span><span class="sxs-lookup"><span data-stu-id="4ff67-227">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="4ff67-228">Se JSONP è obbligatorio per il client (per supportare le richieste tra domini nei browser meno recenti), dovranno essere abilitata in modo esplicito impostando `EnableJSONP` sul `HubConfiguration` oggetto `true`, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="4ff67-228">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="4ff67-229">JSONP è disabilitata per impostazione predefinita, perché è meno sicura rispetto a CORS.</span><span class="sxs-lookup"><span data-stu-id="4ff67-229">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="4ff67-230">**Aggiunta al progetto owin:** per installare questa libreria, eseguire il comando seguente nella Console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="4ff67-230">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="4ff67-231">Questo comando consente di aggiungere il 2.1.0 versione del pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="4ff67-231">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="4ff67-232">La chiamata UseCors</span><span class="sxs-lookup"><span data-stu-id="4ff67-232">Calling UseCors</span></span>

 <span data-ttu-id="4ff67-233">Frammento di codice riportato di seguito viene illustrato come implementare le connessioni tra domini in 2 SignalR.</span><span class="sxs-lookup"><span data-stu-id="4ff67-233">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span> 

<span data-ttu-id="4ff67-234">**Implementazione le richieste tra domini in SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="4ff67-234">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="4ff67-235">Il codice seguente viene illustrato come abilitare CORS o JSONP in un progetto SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="4ff67-235">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="4ff67-236">In questo esempio di codice Usa `Map` e `RunSignalR` anziché `MapSignalR`, in modo che il middleware CORS viene eseguita solo per le richieste di SignalR che richiedono il supporto CORS (anziché per tutto il traffico nel percorso specificato `MapSignalR`.) Mappa utilizzabile anche per qualsiasi altro middleware che deve essere eseguito per un prefisso di URL specifico, anziché per l'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="4ff67-236">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - <span data-ttu-id="4ff67-237">Non impostare `jQuery.support.cors` su true nel codice.</span><span class="sxs-lookup"><span data-stu-id="4ff67-237">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![Non impostare jQuery.support.cors su true](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="4ff67-239">SignalR gestisce l'utilizzo di CORS.</span><span class="sxs-lookup"><span data-stu-id="4ff67-239">SignalR handles the use of CORS.</span></span> <span data-ttu-id="4ff67-240">Impostazione `jQuery.support.cors` a true Disabilita JSONP perché causa SignalR di assumere il browser supporta la condivisione CORS.</span><span class="sxs-lookup"><span data-stu-id="4ff67-240">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="4ff67-241">Quando ci si connette a un URL localhost, Internet Explorer 10 non viene considerata una connessione tra domini, pertanto l'applicazione funzionerà in locale con Internet Explorer 10, anche se non è stata abilitata la connessione tra domini nel server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-241">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="4ff67-242">Per informazioni sull'utilizzo di connessioni tra domini con Internet Explorer 9, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="4ff67-242">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="4ff67-243">Per informazioni sull'utilizzo di connessioni tra domini con Chrome, vedere [questo thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="4ff67-243">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="4ff67-244">Il codice di esempio viene utilizzato il valore predefinito "/ signalr" URL per la connessione al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="4ff67-244">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="4ff67-245">Per informazioni su come specificare un URL di base diversi, vedere [ASP.NET SignalR hub API Guida - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="4ff67-245">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="4ff67-246">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="4ff67-246">How to configure the connection</span></span>

<span data-ttu-id="4ff67-247">Prima di stabilire una connessione, è possibile specificare i parametri di stringa di query o specificare il metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="4ff67-247">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="4ff67-248">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="4ff67-248">How to specify query string parameters</span></span>

<span data-ttu-id="4ff67-249">Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri di stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="4ff67-249">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="4ff67-250">Nell'esempio seguente viene illustrato come impostare un parametro di stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="4ff67-250">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="4ff67-251">**Impostare un valore di stringa di query prima di chiamare il metodo start (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-251">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="4ff67-252">**Impostare un valore di stringa di query prima di chiamare il metodo start (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-252">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="4ff67-253">Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-253">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="4ff67-254">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="4ff67-254">How to specify the transport method</span></span>

<span data-ttu-id="4ff67-255">Come parte del processo di connessione, un client SignalR normalmente negozia con il server per determinare il migliore trasporto supportato dal server e client.</span><span class="sxs-lookup"><span data-stu-id="4ff67-255">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="4ff67-256">Se si conosce già il trasporto a cui si desidera utilizzare, è possibile ignorare questo processo di negoziazione specificando il metodo di trasporto quando si chiama il `start` metodo.</span><span class="sxs-lookup"><span data-stu-id="4ff67-256">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="4ff67-257">**Codice client che specifica il metodo di trasporto (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-257">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="4ff67-258">**Codice client che specifica il metodo di trasporto (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-258">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="4ff67-259">In alternativa, è possibile specificare più metodi di trasporto nell'ordine in cui si desidera SignalR utilizzarli:</span><span class="sxs-lookup"><span data-stu-id="4ff67-259">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="4ff67-260">**Codice client che specifica uno schema di fallback di trasporto personalizzato (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-260">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="4ff67-261">**Codice client che specifica uno schema di trasporto personalizzato fallback (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-261">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="4ff67-262">Per specificare il metodo di trasporto, è possibile utilizzare i valori seguenti:</span><span class="sxs-lookup"><span data-stu-id="4ff67-262">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="4ff67-263">"WebSocket"</span><span class="sxs-lookup"><span data-stu-id="4ff67-263">"webSockets"</span></span>
- <span data-ttu-id="4ff67-264">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="4ff67-264">"foreverFrame"</span></span>
- <span data-ttu-id="4ff67-265">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="4ff67-265">"serverSentEvents"</span></span>
- <span data-ttu-id="4ff67-266">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="4ff67-266">"longPolling"</span></span>

<span data-ttu-id="4ff67-267">Nell'esempio seguente mostrano come determinare quale metodo di trasporto viene utilizzato da una connessione.</span><span class="sxs-lookup"><span data-stu-id="4ff67-267">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="4ff67-268">**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-268">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="4ff67-269">**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-269">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="4ff67-270">Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [ASP.NET SignalR hub API Guida - Server - come ottenere informazioni sul client dalla proprietà di contesto](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="4ff67-270">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="4ff67-271">Per ulteriori informazioni sui trasporti e di fallback, vedere [Introduzione a SignalR - trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="4ff67-271">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="4ff67-272">Come ottenere un proxy per una classe di Hub</span><span class="sxs-lookup"><span data-stu-id="4ff67-272">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="4ff67-273">Ogni oggetto connessione creato incapsula informazioni su una connessione a un servizio SignalR che contiene una o più classi di Hub.</span><span class="sxs-lookup"><span data-stu-id="4ff67-273">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="4ff67-274">Per comunicare con una classe di Hub, utilizzare un oggetto proxy che si crea manualmente (se non si usi il proxy generato) o che viene generata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="4ff67-274">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="4ff67-275">Nel client il nome del proxy è una versione di maiuscole/minuscole camel del nome di classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="4ff67-275">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="4ff67-276">SignalR viene automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4ff67-276">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="4ff67-277">**Classe hub sul server**</span><span class="sxs-lookup"><span data-stu-id="4ff67-277">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="4ff67-278">**Ottenere un riferimento al proxy client generato per l'Hub**</span><span class="sxs-lookup"><span data-stu-id="4ff67-278">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="4ff67-279">**Creare il proxy client per la classe di Hub (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-279">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="4ff67-280">Se si decorata la classe Hub con un `HubName` attributo, utilizzare il nome esatto senza modificare maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="4ff67-280">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="4ff67-281">**Classe hub sul server con l'attributo HubName**</span><span class="sxs-lookup"><span data-stu-id="4ff67-281">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="4ff67-282">**Ottenere un riferimento al proxy client generato per l'Hub**</span><span class="sxs-lookup"><span data-stu-id="4ff67-282">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="4ff67-283">**Creare il proxy client per la classe di Hub (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-283">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="4ff67-284">Come definire i metodi nel client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="4ff67-284">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="4ff67-285">Per definire un metodo che il server è possibile chiamare da un Hub, aggiungere un gestore eventi per il proxy dell'Hub utilizzando il `client` proprietà del proxy generato o chiamata di `on` metodo se non si utilizza il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="4ff67-285">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="4ff67-286">I parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="4ff67-286">The parameters can be complex objects.</span></span>

<span data-ttu-id="4ff67-287">Aggiungere il gestore dell'evento prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="4ff67-287">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="4ff67-288">(Se si desidera aggiungere i gestori eventi dopo la chiamata di `start` metodo, vedere la nota nel [per stabilire una connessione](#establishconnection) più indietro in questo documento e utilizzare la sintassi illustrata per la definizione di un metodo senza utilizzare il proxy generato.)</span><span class="sxs-lookup"><span data-stu-id="4ff67-288">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="4ff67-289">Nome di metodo corrispondente è tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="4ff67-289">Method name matching is case-insensitive.</span></span> <span data-ttu-id="4ff67-290">Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, o `addcontosochatmessagetopage` sul client.</span><span class="sxs-lookup"><span data-stu-id="4ff67-290">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="4ff67-291">**Definire un metodo nel client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-291">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="4ff67-292">**Modo alternativo per definire il metodo nel client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-292">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="4ff67-293">**Definire un metodo nel client (senza il proxy generato, o quando si aggiungono dopo aver chiamato il metodo di avvio)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-293">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="4ff67-294">**Codice che chiama il metodo di client del server**</span><span class="sxs-lookup"><span data-stu-id="4ff67-294">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="4ff67-295">Gli esempi seguenti includono un oggetto complesso come un parametro di metodo.</span><span class="sxs-lookup"><span data-stu-id="4ff67-295">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="4ff67-296">**Definire un metodo nel client che accetta un oggetto complesso (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-296">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="4ff67-297">**Definire un metodo nel client che accetta un oggetto complesso (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-297">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="4ff67-298">**Codice del server che definisce l'oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="4ff67-298">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="4ff67-299">**Codice del server che chiama il metodo di client utilizzando un oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="4ff67-299">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="4ff67-300">Come chiamare i metodi di server dal client</span><span class="sxs-lookup"><span data-stu-id="4ff67-300">How to call server methods from the client</span></span>

<span data-ttu-id="4ff67-301">Per chiamare un metodo server dal client, usare il `server` proprietà del proxy generato o `invoke` metodo sul proxy di Hub, se non si utilizza il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="4ff67-301">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="4ff67-302">Il valore restituito o i parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="4ff67-302">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="4ff67-303">Passare in una versione maiuscole-minuscole camel del nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="4ff67-303">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="4ff67-304">SignalR viene automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4ff67-304">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="4ff67-305">Gli esempi seguenti mostrano come chiamare un metodo del server che non dispone di un valore restituito e come chiamare un metodo del server che hanno un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="4ff67-305">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="4ff67-306">**Metodo server senza l'attributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="4ff67-306">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="4ff67-307">**Codice del server che definisce l'oggetto complesso passato un parametro**</span><span class="sxs-lookup"><span data-stu-id="4ff67-307">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="4ff67-308">**Codice client che richiama il metodo server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-308">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="4ff67-309">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-309">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="4ff67-310">Se è decorata del metodo dell'Hub con un `HubMethodName` attributo, utilizzare il nome senza modificare maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="4ff67-310">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="4ff67-311">**Metodo server** con un attributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="4ff67-311">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="4ff67-312">**Codice client che richiama il metodo server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-312">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="4ff67-313">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-313">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="4ff67-314">Negli esempi precedenti viene illustrano come chiamare un metodo del server che non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="4ff67-314">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="4ff67-315">Nell'esempio seguente viene illustrato come chiamare un metodo del server che ha un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="4ff67-315">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="4ff67-316">**Codice lato server per un metodo che presenta un valore restituito**</span><span class="sxs-lookup"><span data-stu-id="4ff67-316">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="4ff67-317">**La classe Stock utilizzata per il** valore restituito</span><span class="sxs-lookup"><span data-stu-id="4ff67-317">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="4ff67-318">**Codice client che richiama il metodo server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-318">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="4ff67-319">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-319">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="4ff67-320">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="4ff67-320">How to handle connection lifetime events</span></span>

<span data-ttu-id="4ff67-321">SignalR fornisce la seguente connessione di eventi di durata che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="4ff67-321">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="4ff67-322">`starting`: Generato prima che qualsiasi dato viene inviato tramite la connessione.</span><span class="sxs-lookup"><span data-stu-id="4ff67-322">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="4ff67-323">`received`: Generato quando vengono ricevuti dati per la connessione.</span><span class="sxs-lookup"><span data-stu-id="4ff67-323">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="4ff67-324">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="4ff67-324">Provides the received data.</span></span>
- <span data-ttu-id="4ff67-325">`connectionSlow`: Generato quando il client rileva una connessione lenta o l'eliminazione di frequente.</span><span class="sxs-lookup"><span data-stu-id="4ff67-325">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="4ff67-326">`reconnecting`: Generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="4ff67-326">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="4ff67-327">`reconnected`: Generato quando il trasporto sottostante è stata riconnessa.</span><span class="sxs-lookup"><span data-stu-id="4ff67-327">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="4ff67-328">`stateChanged`: Generato quando cambia lo stato di connessione.</span><span class="sxs-lookup"><span data-stu-id="4ff67-328">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="4ff67-329">Fornisce lo stato precedente e il nuovo stato (connessione, connesso, riconnessione in corso o Disconnected).</span><span class="sxs-lookup"><span data-stu-id="4ff67-329">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="4ff67-330">`disconnected`: Generato quando la connessione è interrotta.</span><span class="sxs-lookup"><span data-stu-id="4ff67-330">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="4ff67-331">Ad esempio, se si desidera visualizzare i messaggi di avviso quando si verificano problemi di connessione che potrebbero causare ritardi notevoli, è possibile gestire il `connectionSlow` evento.</span><span class="sxs-lookup"><span data-stu-id="4ff67-331">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="4ff67-332">**Gestire l'evento connectionSlow (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-332">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="4ff67-333">**Gestire l'evento connectionSlow (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-333">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="4ff67-334">Per ulteriori informazioni, vedere [comprensione e gestione degli eventi di durata connessione in SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="4ff67-334">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="4ff67-335">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="4ff67-335">How to handle errors</span></span>

<span data-ttu-id="4ff67-336">Il client SignalR JavaScript fornisce un `error` eventi che è possibile aggiungere un gestore per.</span><span class="sxs-lookup"><span data-stu-id="4ff67-336">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="4ff67-337">È inoltre possibile utilizzare il metodo hanno esito negativo per aggiungere un gestore errori risultanti da una chiamata al metodo server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-337">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="4ff67-338">Se si non abilita in modo esplicito i messaggi di errore dettagliato nel server, l'oggetto eccezione che restituisce SignalR dopo un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="4ff67-338">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="4ff67-339">Ad esempio, se una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati per i client nell'ambiente di produzione non è consigliata per motivi di sicurezza, ma se si desidera abilitare per i messaggi di errore dettagliati risoluzione dei problemi, utilizzare il codice seguente nel server.</span><span class="sxs-lookup"><span data-stu-id="4ff67-339">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="4ff67-340">Nell'esempio seguente viene illustrato come aggiungere un gestore per l'evento di errore.</span><span class="sxs-lookup"><span data-stu-id="4ff67-340">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="4ff67-341">**Aggiungere un gestore degli errori (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-341">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="4ff67-342">**Aggiungere un gestore degli errori (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-342">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="4ff67-343">Nell'esempio seguente viene illustrato come gestire un errore da una chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="4ff67-343">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="4ff67-344">**Gestire un errore da una chiamata al metodo (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-344">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="4ff67-345">**Gestire un errore da una chiamata al metodo (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-345">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="4ff67-346">Se una chiamata al metodo ha esito negativo, il `error` evento viene generato anche, pertanto il codice nel `error` gestore del metodo e nel `.fail` verrebbe eseguito il callback di metodo.</span><span class="sxs-lookup"><span data-stu-id="4ff67-346">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="4ff67-347">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="4ff67-347">How to enable client-side logging</span></span>

<span data-ttu-id="4ff67-348">Per abilitare la registrazione lato client in una connessione, impostare il `logging` proprietà sull'oggetto di connessione prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="4ff67-348">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="4ff67-349">**Abilitare la registrazione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-349">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="4ff67-350">**Abilitare la registrazione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="4ff67-350">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="4ff67-351">Per visualizzare i registri, aprire Strumenti di sviluppo del browser e passare alla scheda della Console. Per un'esercitazione che illustra le istruzioni dettagliate e schermata schermate che illustrano come eseguire questa operazione, vedere [Server Broadcast con ASP.NET Signalr - abilitare la registrazione](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span><span class="sxs-lookup"><span data-stu-id="4ff67-351">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span></span>
