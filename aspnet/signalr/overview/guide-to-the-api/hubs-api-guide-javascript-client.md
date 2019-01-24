---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guida all'API di ASP.NET SignalR Hubs - Client JavaScript | Microsoft Docs
author: bradygaster
description: Questo documento viene fornita un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client JavaScript, ad esempio browser e Windows Store (WinJS) applicat...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 7689fa5b6a3e81c9f767831423eb3efad72e1ab3
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836597"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="66702-103">Guida all'API di ASP.NET SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="66702-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="66702-104">Questo documento fornisce un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client JavaScript, ad esempio i browser e applicazioni Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="66702-104">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="66702-105">L'API di hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="66702-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="66702-106">Nel codice server, si definiscono i metodi che possono essere chiamati da parte dei client e si chiamano i metodi eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="66702-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="66702-107">Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi che eseguono sul server.</span><span class="sxs-lookup"><span data-stu-id="66702-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="66702-108">SignalR si occupa di tutte le attività client-server.</span><span class="sxs-lookup"><span data-stu-id="66702-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="66702-109">SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="66702-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="66702-110">Per un'introduzione a SignalR hub e connessioni permanenti, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="66702-110">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="66702-111">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="66702-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="66702-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="66702-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="66702-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="66702-113">.NET 4.5</span></span>
> - <span data-ttu-id="66702-114">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="66702-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="66702-115">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="66702-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="66702-116">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="66702-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="66702-117">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="66702-117">Questions and comments</span></span>
>
> <span data-ttu-id="66702-118">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="66702-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="66702-119">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="66702-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="66702-120">Panoramica</span><span class="sxs-lookup"><span data-stu-id="66702-120">Overview</span></span>

<span data-ttu-id="66702-121">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="66702-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="66702-122">Il proxy generato e che cosa fa per te</span><span class="sxs-lookup"><span data-stu-id="66702-122">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="66702-123">Quando usare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="66702-123">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="66702-124">Programma di installazione client</span><span class="sxs-lookup"><span data-stu-id="66702-124">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="66702-125">Come fanno riferimento al proxy generato in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="66702-125">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="66702-126">Come creare un file fisico per SignalR generato proxy</span><span class="sxs-lookup"><span data-stu-id="66702-126">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="66702-127">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="66702-127">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="66702-128">$. connection.hub è lo stesso oggetto consente di creare tale $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="66702-128">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="66702-129">Esecuzione asincrona del metodo start</span><span class="sxs-lookup"><span data-stu-id="66702-129">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="66702-130">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="66702-130">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="66702-131">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="66702-131">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="66702-132">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="66702-132">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="66702-133">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="66702-133">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="66702-134">Come ottenere un proxy per una classe Hub</span><span class="sxs-lookup"><span data-stu-id="66702-134">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="66702-135">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="66702-135">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="66702-136">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="66702-136">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="66702-137">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="66702-137">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="66702-138">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="66702-138">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="66702-139">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="66702-139">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="66702-140">Per informazioni su come programmare il server o client .NET, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="66702-140">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="66702-141">Guida all'API di SignalR Hubs - Server</span><span class="sxs-lookup"><span data-stu-id="66702-141">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="66702-142">Guida all'API di SignalR Hubs - Client .NET</span><span class="sxs-lookup"><span data-stu-id="66702-142">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="66702-143">Il componente server di SignalR 2 disponibile solo in .NET 4.5 (anche se è disponibile un client .NET per 2 SignalR in .NET 4.0).</span><span class="sxs-lookup"><span data-stu-id="66702-143">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="66702-144">Il proxy generato e che cosa fa per te</span><span class="sxs-lookup"><span data-stu-id="66702-144">The generated proxy and what it does for you</span></span>

<span data-ttu-id="66702-145">È possibile programmare un client JavaScript per comunicare con un servizio di SignalR con o senza un proxy che SignalR genera automaticamente.</span><span class="sxs-lookup"><span data-stu-id="66702-145">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="66702-146">Ciò che esegue il proxy per l'utente è semplificare la sintassi del codice usato per connettersi, metodi di scrittura che chiama il server, e chiamare metodi sul server.</span><span class="sxs-lookup"><span data-stu-id="66702-146">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="66702-147">Quando si scrive codice per chiamare i metodi di server, il proxy generato consente di usare la sintassi che sembra si erano in esecuzione una funzione locale: è possibile scrivere `serverMethod(arg1, arg2)` invece di `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="66702-147">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="66702-148">La sintassi del proxy generato consente inoltre di un errore lato client immediato e comprensibile se si digita un nome di metodo server.</span><span class="sxs-lookup"><span data-stu-id="66702-148">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="66702-149">E se si crea manualmente il file che definisce i proxy, è anche possibile ottenere supporto IntelliSense per la scrittura di codice che chiama i metodi di server.</span><span class="sxs-lookup"><span data-stu-id="66702-149">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="66702-150">Ad esempio, si supponga di che avere la seguente classe dell'Hub sul server:</span><span class="sxs-lookup"><span data-stu-id="66702-150">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="66702-151">Gli esempi di codice seguenti mostrano cosa JavaScript codice simile per richiamare il `NewContosoChatMessage` su server e ricevere le chiamate del metodo di `addContosoChatMessageToPage` metodo dal server.</span><span class="sxs-lookup"><span data-stu-id="66702-151">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="66702-152">**Con il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="66702-152">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="66702-153">**Senza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="66702-153">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="66702-154">Quando usare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="66702-154">When to use the generated proxy</span></span>

<span data-ttu-id="66702-155">Se si vuole registrare più gestori di eventi per un metodo di client che chiama il server, è possibile usare il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="66702-155">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="66702-156">In caso contrario, è possibile scegliere di usare il proxy generato o non in base alle proprie preferenze di scrittura del codice.</span><span class="sxs-lookup"><span data-stu-id="66702-156">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="66702-157">Se si sceglie di non usarla, non è necessario fare riferimento all'URL "/ degli hub signalr" in un `script` elemento nel codice client.</span><span class="sxs-lookup"><span data-stu-id="66702-157">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="66702-158">Programma di installazione client</span><span class="sxs-lookup"><span data-stu-id="66702-158">Client setup</span></span>

<span data-ttu-id="66702-159">Un client JavaScript vengono richiesti riferimenti ai file SignalR core JavaScript e jQuery.</span><span class="sxs-lookup"><span data-stu-id="66702-159">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="66702-160">La versione di jQuery deve essere 1.6.4 o versioni successive principali, ad esempio 1.7.2, 1.8.2 o 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="66702-160">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="66702-161">Se si decide di usare il proxy generato, è necessario anche un riferimento al proxy di SignalR generati file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="66702-161">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="66702-162">Nell'esempio seguente mostra i riferimenti all'aspetto in una pagina HTML che usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="66702-162">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="66702-163">Questi riferimenti devono essere incluso in questo ordine: jQuery, SignalR core in seguito e i proxy di SignalR cognome.</span><span class="sxs-lookup"><span data-stu-id="66702-163">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="66702-164">Come fanno riferimento al proxy generato in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="66702-164">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="66702-165">Nell'esempio precedente, il riferimento al proxy di SignalR generato è al codice JavaScript generato in modo dinamico, non a un file fisico.</span><span class="sxs-lookup"><span data-stu-id="66702-165">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="66702-166">SignalR crea il codice JavaScript per il proxy in tempo reale e li fornisce ai client in risposta all'URL "/ signalr/hub".</span><span class="sxs-lookup"><span data-stu-id="66702-166">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="66702-167">Se è specificato un URL di base diversi per le connessioni SignalR nel server di `MapSignalR` metodo, l'URL del file proxy generato dinamicamente corrisponde all'URL personalizzato con "/ hub" aggiunto a tale.</span><span class="sxs-lookup"><span data-stu-id="66702-167">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="66702-168">Per i client Windows 8 (Windows Store) in JavaScript, usare il file fisico proxy anziché in quello generato in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="66702-168">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="66702-169">Per altre informazioni, vedere [come creare un file fisico per SignalR generato proxy](#manualproxy) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="66702-169">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="66702-170">In un ASP.NET MVC 4 o 5 visualizzazione Razor, usare il carattere tilde per fare riferimento alla radice dell'applicazione nel riferimento file proxy:</span><span class="sxs-lookup"><span data-stu-id="66702-170">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="66702-171">Per altre informazioni sull'uso di SignalR in MVC 5, vedere [Introduzione a SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="66702-171">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="66702-172">In una visualizzazione Razor di ASP.NET MVC 3, usare `Url.Content` relativo al riferimento al file proxy:</span><span class="sxs-lookup"><span data-stu-id="66702-172">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="66702-173">In un'applicazione Web Form ASP.NET, usare `ResolveClientUrl` per il proxy di riferimento di file o la registrazione tramite ScriptManager usando un percorso relativo della radice app (a partire da una tilde):</span><span class="sxs-lookup"><span data-stu-id="66702-173">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="66702-174">Come regola generale, usare lo stesso metodo per specificare l'URL "/ signalr/hub" che è usato per i file CSS o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="66702-174">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="66702-175">Se si specifica un URL senza usare una tilde, in alcuni scenari l'applicazione funzionerà correttamente quando si test in Visual Studio con IIS Express ma avrà esito negativo con un errore 404 durante la distribuzione in modalità IIS completa.</span><span class="sxs-lookup"><span data-stu-id="66702-175">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="66702-176">Per altre informazioni, vedere **risoluzione dei riferimenti alle risorse a livello di radice** nelle [i server Web in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) nel sito MSDN.</span><span class="sxs-lookup"><span data-stu-id="66702-176">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="66702-177">Quando si esegue un progetto web in Visual Studio 2017 in modalità di debug e se si utilizza Internet Explorer come browser, è possibile visualizzare il file proxy nella **Esplora soluzioni** sotto **script**.</span><span class="sxs-lookup"><span data-stu-id="66702-177">When you run a web project in Visual Studio 2017 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Scripts**.</span></span>

<span data-ttu-id="66702-178">Per visualizzare il contenuto del file, fare doppio clic su **hub**.</span><span class="sxs-lookup"><span data-stu-id="66702-178">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="66702-179">Se non si usa Visual Studio 2012 o 2013 e Internet Explorer, o se non si è in modalità di debug, è anche possibile ottenere il contenuto del file passando all'URL "/ signalR/hub".</span><span class="sxs-lookup"><span data-stu-id="66702-179">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="66702-180">Ad esempio, se il sito è in esecuzione in `http://localhost:56699`, passare a `http://localhost:56699/SignalR/hubs` nel browser.</span><span class="sxs-lookup"><span data-stu-id="66702-180">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="66702-181">Come creare un file fisico per SignalR generato proxy</span><span class="sxs-lookup"><span data-stu-id="66702-181">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="66702-182">Come alternativa per il proxy generato dinamicamente, è possibile creare un file fisico contenente il codice proxy e fare riferimento a tale file.</span><span class="sxs-lookup"><span data-stu-id="66702-182">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="66702-183">È possibile eseguire questa operazione per un controllo sulla memorizzazione nella cache o creazione di bundle comportamento o per ottenere IntelliSense quando si esegue la codifica le chiamate ai metodi di server.</span><span class="sxs-lookup"><span data-stu-id="66702-183">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="66702-184">Per creare un file proxy, seguire i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="66702-184">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="66702-185">Installare il [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="66702-185">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="66702-186">Aprire un prompt dei comandi e passare al *strumenti* cartella che contiene il file SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="66702-186">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="66702-187">La cartella strumenti si trova nella posizione seguente:</span><span class="sxs-lookup"><span data-stu-id="66702-187">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="66702-188">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="66702-188">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="66702-189">Il percorso per il *. dll* è in genere il *bin* cartella nella cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="66702-189">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="66702-190">Questo comando crea un file denominato *server. js* nella stessa cartella *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="66702-190">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="66702-191">Inserire il *server. js* file in una cartella appropriata nel progetto, rinominarlo in modo appropriato per l'applicazione, quindi aggiungere un riferimento a esso al posto del riferimento "/ degli hub signalr".</span><span class="sxs-lookup"><span data-stu-id="66702-191">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="66702-192">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="66702-192">How to establish a connection</span></span>

<span data-ttu-id="66702-193">Prima di stabilire una connessione, è necessario creare un oggetto di connessione, creare un proxy e registrare i gestori eventi per metodi che possono essere chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="66702-193">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="66702-194">Quando sono configurati i proxy e gestori eventi, è possibile stabilire la connessione tramite una chiamata di `start` (metodo).</span><span class="sxs-lookup"><span data-stu-id="66702-194">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="66702-195">Se si usa il proxy generato, non è necessario creare l'oggetto di connessione nel codice perché il codice proxy generato esegue tutto automaticamente.</span><span class="sxs-lookup"><span data-stu-id="66702-195">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="66702-196">**Stabilire una connessione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-196">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="66702-197">**Stabilire una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-197">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="66702-198">Il codice di esempio Usa il valore predefinito "/ signalr" URL per la connessione al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="66702-198">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="66702-199">Per informazioni su come specificare un URL di base diversi, vedere [Guida all'API di hub di ASP.NET SignalR - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="66702-199">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="66702-200">Per impostazione predefinita, il percorso dell'hub è il server corrente. Se ci si connette a un altro server, specificare l'URL prima di chiamare il `start` metodo, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="66702-200">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="66702-201">In genere si registrano i gestori di eventi prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="66702-201">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="66702-202">Se si vuole registrare alcuni gestori di eventi dopo avere stabilito la connessione, è possibile farlo, ma è necessario registrare almeno una dei gestori di eventi prima di chiamare il `start` (metodo).</span><span class="sxs-lookup"><span data-stu-id="66702-202">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="66702-203">Uno dei motivi è che possono essere presenti molti hub in un'applicazione, ma è preferibile attivare il `OnConnected` evento su ogni Hub se solo si intende utilizzare uno di essi.</span><span class="sxs-lookup"><span data-stu-id="66702-203">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="66702-204">Quando viene stabilita la connessione, la presenza di un metodo client nel proxy dell'Hub è che indica a SignalR per attivare il `OnConnected` evento.</span><span class="sxs-lookup"><span data-stu-id="66702-204">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="66702-205">Se non si registrano i gestori di eventi prima di chiamare il `start` metodo, sarà in grado di richiamare i metodi per l'Hub, ma l'Hub `OnConnected` non chiamare il metodo e non verrà richiamato Nessun metodo client dal server.</span><span class="sxs-lookup"><span data-stu-id="66702-205">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="66702-206">$. connection.hub è lo stesso oggetto consente di creare tale $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="66702-206">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="66702-207">Come si può notare dagli esempi, quando si usa il proxy generato, `$.connection.hub` fa riferimento all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="66702-207">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="66702-208">Questo è lo stesso oggetto che si ottiene chiamando `$.hubConnection()` quando non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="66702-208">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="66702-209">Il codice proxy generato crea la connessione tramite l'istruzione seguente:</span><span class="sxs-lookup"><span data-stu-id="66702-209">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Creazione di una connessione nel file proxy generato](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="66702-211">Quando si usa il proxy generato, è possibile eseguire qualsiasi operazione con `$.connection.hub` che è possibile eseguire con un oggetto connessione quando non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="66702-211">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="66702-212">Esecuzione asincrona del metodo start</span><span class="sxs-lookup"><span data-stu-id="66702-212">Asynchronous execution of the start method</span></span>

<span data-ttu-id="66702-213">Il `start` metodo viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="66702-213">The `start` method executes asynchronously.</span></span> <span data-ttu-id="66702-214">Restituisce un [oggetto Deferred jQuery](http://api.jquery.com/category/deferred-object/), il che significa che è possibile aggiungere le funzioni di callback da chiamare metodi quali `pipe`, `done`, e `fail`.</span><span class="sxs-lookup"><span data-stu-id="66702-214">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="66702-215">Se si dispone di codice che si desidera eseguire dopo aver stabilita la connessione, ad esempio una chiamata a un metodo server, tale codice inserito in una funzione di callback o chiamarlo da una funzione di callback.</span><span class="sxs-lookup"><span data-stu-id="66702-215">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="66702-216">Il `.done` metodo di callback viene eseguito dopo aver stabilita la connessione e dopo qualsiasi codice che è necessario `OnConnected` metodo del gestore eventi nel server al termine dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="66702-216">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="66702-217">Se si inserisce l'istruzione "Ora connessi" dell'esempio precedente alla riga successiva del codice dopo il `start` chiamata al metodo (non in un `.done` callback), il `console.log` riga verrà eseguita prima che venga stabilita la connessione, come illustrato nell'esempio seguente esempio:</span><span class="sxs-lookup"><span data-stu-id="66702-217">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Modo errato per scrivere codice che viene eseguito una volta stabilita connessione](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="66702-219">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="66702-219">How to establish a cross-domain connection</span></span>

<span data-ttu-id="66702-220">In genere se il browser ha caricato una pagina dalla `http://contoso.com`, la connessione SignalR è nello stesso dominio, in `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="66702-220">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="66702-221">Se la pagina dalla `http://contoso.com` stabilisce una connessione a `http://fabrikam.com/signalr`, vale a dire una connessione tra domini.</span><span class="sxs-lookup"><span data-stu-id="66702-221">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="66702-222">Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="66702-222">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="66702-223">In SignalR 1.x, richieste tra domini sono state controllate da un singolo flag EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="66702-223">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="66702-224">Questo flag controllata richieste sia JSONP e la condivisione CORS.</span><span class="sxs-lookup"><span data-stu-id="66702-224">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="66702-225">Per una maggiore flessibilità, supporto di CORS tutti è stata rimossa dal componente server di SignalR (client JavaScript comunque usano CORS in genere se è stato rilevato che il browser supporti), e nuovi middleware OWIN è stato reso disponibile per supportare questi scenari.</span><span class="sxs-lookup"><span data-stu-id="66702-225">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="66702-226">Se JSONP è obbligatorio per il client (per supportare le richieste tra domini in browser meno recenti), dovrà essere abilitata in modo esplicito impostando `EnableJSONP` nella `HubConfiguration` oggetto `true`, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="66702-226">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="66702-227">JSONP è disabilitata per impostazione predefinita, perché è meno sicuro rispetto a CORS.</span><span class="sxs-lookup"><span data-stu-id="66702-227">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="66702-228">**Aggiunta di owin al progetto:** Per installare questa libreria, eseguire il comando seguente nella Console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="66702-228">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="66702-229">Questo comando aggiunge il 2.1.0 versione del pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="66702-229">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="66702-230">Chiamare UseCors</span><span class="sxs-lookup"><span data-stu-id="66702-230">Calling UseCors</span></span>

 <span data-ttu-id="66702-231">Il frammento di codice seguente viene illustrato come implementare le connessioni tra domini in SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="66702-231">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="66702-232">**Implementazione di richieste tra domini in SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="66702-232">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="66702-233">Il codice seguente illustra come abilitare JSONP o CORS in un progetto SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="66702-233">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="66702-234">Questo esempio di codice viene utilizzato `Map` e `RunSignalR` invece di `MapSignalR`, in modo che il middleware CORS viene eseguita solo per le richieste di SignalR che richiedono il supporto CORS (invece che per tutto il traffico nel percorso specificato `MapSignalR`.) Mappa può essere usata anche per qualsiasi altro middleware che deve essere eseguita per un prefisso URL specifico, anziché per l'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="66702-234">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="66702-235">Non impostare `jQuery.support.cors` su true nel codice.</span><span class="sxs-lookup"><span data-stu-id="66702-235">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![Non impostare jQuery.support.cors su true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="66702-237">SignalR gestisce l'uso di CORS.</span><span class="sxs-lookup"><span data-stu-id="66702-237">SignalR handles the use of CORS.</span></span> <span data-ttu-id="66702-238">Impostazione `jQuery.support.cors` a true Disabilita JSONP perché causa SignalR per presupporre il browser supporta la condivisione CORS.</span><span class="sxs-lookup"><span data-stu-id="66702-238">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="66702-239">Quando ci si connette a un URL localhost, Internet Explorer 10 non considera una connessione tra domini, in modo che l'applicazione funzionerà in locale con Internet Explorer 10 anche se non è stata abilitata la connessione tra domini nel server.</span><span class="sxs-lookup"><span data-stu-id="66702-239">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="66702-240">Per informazioni sull'uso di connessioni tra domini con Internet Explorer 9, vedere [thread di StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="66702-240">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="66702-241">Per informazioni sull'uso di connessioni tra domini con Chrome, vedere [thread di StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="66702-241">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="66702-242">Il codice di esempio Usa il valore predefinito "/ signalr" URL per la connessione al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="66702-242">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="66702-243">Per informazioni su come specificare un URL di base diversi, vedere [Guida all'API di hub di ASP.NET SignalR - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="66702-243">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="66702-244">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="66702-244">How to configure the connection</span></span>

<span data-ttu-id="66702-245">Prima di aver stabilito una connessione, è possibile specificare parametri della stringa di query o specificare il metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="66702-245">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="66702-246">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="66702-246">How to specify query string parameters</span></span>

<span data-ttu-id="66702-247">Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri della stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="66702-247">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="66702-248">Negli esempi seguenti illustrano come impostare un parametro di stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="66702-248">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="66702-249">**Impostare un valore di stringa di query prima di chiamare il metodo start (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-249">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="66702-250">**Impostare un valore di stringa di query prima di chiamare il metodo start (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-250">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="66702-251">Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.</span><span class="sxs-lookup"><span data-stu-id="66702-251">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="66702-252">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="66702-252">How to specify the transport method</span></span>

<span data-ttu-id="66702-253">Durante il processo di connessione, un client SignalR normalmente negozia con il server per determinare il migliore trasporto supportato dal server e client.</span><span class="sxs-lookup"><span data-stu-id="66702-253">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="66702-254">Se si conosce già il trasporto da usare, è possibile ignorare questo processo di negoziazione specificando il metodo di trasporto quando si chiama il `start` (metodo).</span><span class="sxs-lookup"><span data-stu-id="66702-254">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="66702-255">**Codice client che specifica il metodo di trasporto (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-255">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="66702-256">**Codice client che specifica il metodo di trasporto (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-256">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="66702-257">In alternativa, è possibile specificare più metodi di trasporto nell'ordine in cui si desidera SignalR per provarle in:</span><span class="sxs-lookup"><span data-stu-id="66702-257">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="66702-258">**Codice client che specifica uno schema di fallback di trasporto personalizzato (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-258">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="66702-259">**Codice client che specifica uno schema di fallback di trasporto personalizzato (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-259">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="66702-260">È possibile usare i valori seguenti per specificare il metodo di trasporto:</span><span class="sxs-lookup"><span data-stu-id="66702-260">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="66702-261">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="66702-261">"webSockets"</span></span>
- <span data-ttu-id="66702-262">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="66702-262">"foreverFrame"</span></span>
- <span data-ttu-id="66702-263">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="66702-263">"serverSentEvents"</span></span>
- <span data-ttu-id="66702-264">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="66702-264">"longPolling"</span></span>

<span data-ttu-id="66702-265">Gli esempi seguenti illustrano come determinare quale metodo di trasporto viene utilizzato da una connessione.</span><span class="sxs-lookup"><span data-stu-id="66702-265">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="66702-266">**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-266">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="66702-267">**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-267">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="66702-268">Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [- Server - Guida all'API Hubs SignalR di ASP.NET come ottenere informazioni relative al client dalla proprietà di contesto](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="66702-268">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="66702-269">Per altre informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR - trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="66702-269">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="66702-270">Come ottenere un proxy per una classe Hub</span><span class="sxs-lookup"><span data-stu-id="66702-270">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="66702-271">Ogni oggetto di connessione che crea incapsula informazioni su una connessione a un servizio SignalR contenente uno o più classi di Hub.</span><span class="sxs-lookup"><span data-stu-id="66702-271">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="66702-272">Per comunicare con una classe di Hub, usare un oggetto proxy che create dall'utente (se non si usa il proxy generato) o che viene generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="66702-272">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="66702-273">Nel client il nome del proxy è una versione con notazione camel del nome della classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="66702-273">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="66702-274">SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="66702-274">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="66702-275">**Classe dell'hub sul server**</span><span class="sxs-lookup"><span data-stu-id="66702-275">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="66702-276">**Ottenere un riferimento al proxy client generato per l'Hub**</span><span class="sxs-lookup"><span data-stu-id="66702-276">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="66702-277">**Creare il proxy client per la classe Hub (senza proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-277">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="66702-278">Se si decora la classe di Hub con un `HubName` attributo, usare il nome esatto senza modificare maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="66702-278">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="66702-279">**Classe dell'hub sul server con l'attributo HubName**</span><span class="sxs-lookup"><span data-stu-id="66702-279">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="66702-280">**Ottenere un riferimento al proxy client generato per l'Hub**</span><span class="sxs-lookup"><span data-stu-id="66702-280">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="66702-281">**Creare il proxy client per la classe Hub (senza proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-281">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="66702-282">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="66702-282">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="66702-283">Per definire un metodo che il server è possibile chiamare da un Hub, aggiungere un gestore eventi per il proxy dell'Hub usando il `client` proprietà del proxy generato o chiamata di `on` metodo se non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="66702-283">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="66702-284">I parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="66702-284">The parameters can be complex objects.</span></span>

<span data-ttu-id="66702-285">Aggiungere il gestore dell'evento prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="66702-285">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="66702-286">(Se si desidera aggiungere i gestori eventi dopo la chiamata di `start` metodo, vedere la nota nella [come stabilire una connessione](#establishconnection) in precedenza in questo documento e usare la sintassi illustrata per la definizione di un metodo senza usare il proxy generato.)</span><span class="sxs-lookup"><span data-stu-id="66702-286">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="66702-287">Metodo nome viene applicata alcuna distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="66702-287">Method name matching is case-insensitive.</span></span> <span data-ttu-id="66702-288">Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, o `addcontosochatmessagetopage` sul client.</span><span class="sxs-lookup"><span data-stu-id="66702-288">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="66702-289">**Definire un metodo nel client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-289">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="66702-290">**Modo alternativo per definire il metodo nel client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-290">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="66702-291">**Definire un metodo nel client (senza il proxy generato o durante l'aggiunta dopo aver chiamato il metodo di avvio)**</span><span class="sxs-lookup"><span data-stu-id="66702-291">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="66702-292">**Codice del server che chiama il metodo client**</span><span class="sxs-lookup"><span data-stu-id="66702-292">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="66702-293">Negli esempi seguenti includono un oggetto complesso come un parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="66702-293">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="66702-294">**Definire un metodo sul client che accetta un oggetto complesso (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-294">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="66702-295">**Definire un metodo sul client che accetta un oggetto complesso (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-295">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="66702-296">**Codice del server che definisce l'oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="66702-296">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="66702-297">**Codice del server che chiama il metodo client utilizzando un oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="66702-297">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="66702-298">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="66702-298">How to call server methods from the client</span></span>

<span data-ttu-id="66702-299">Per chiamare un metodo server dal client, usare il `server` proprietà del proxy generato o `invoke` metodo sul proxy di Hub, se non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="66702-299">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="66702-300">Il valore restituito o i parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="66702-300">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="66702-301">Passare una versione maiuscole-minuscole camel del nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="66702-301">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="66702-302">SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="66702-302">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="66702-303">Gli esempi seguenti illustrano come chiamare un metodo server che non ha un valore restituito e come chiamare un metodo del server che hanno un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="66702-303">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="66702-304">**Metodo server senza l'attributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="66702-304">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="66702-305">**Codice del server che definisce l'oggetto complesso passato un parametro**</span><span class="sxs-lookup"><span data-stu-id="66702-305">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="66702-306">**Codice client che richiama il metodo del server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-306">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="66702-307">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-307">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="66702-308">Se è decorata del metodo dell'Hub con un `HubMethodName` attributo, usare il nome senza modificare maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="66702-308">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="66702-309">**Metodo server** con un attributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="66702-309">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="66702-310">**Codice client che richiama il metodo del server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-310">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="66702-311">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-311">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="66702-312">Negli esempi precedenti viene illustrato come chiamare un metodo del server che non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="66702-312">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="66702-313">Negli esempi seguenti illustrano come chiamare un metodo server che ha un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="66702-313">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="66702-314">**Codice del server per un metodo che ha un valore restituito**</span><span class="sxs-lookup"><span data-stu-id="66702-314">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="66702-315">**La classe Stock utilizzata per il** valore restituito</span><span class="sxs-lookup"><span data-stu-id="66702-315">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="66702-316">**Codice client che richiama il metodo del server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-316">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="66702-317">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-317">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="66702-318">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="66702-318">How to handle connection lifetime events</span></span>

<span data-ttu-id="66702-319">SignalR fornisce il collegamento seguente gli eventi di durata che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="66702-319">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="66702-320">`starting`: Generato prima che tutti i dati vengono inviati tramite la connessione.</span><span class="sxs-lookup"><span data-stu-id="66702-320">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="66702-321">`received`: Generato quando vengono ricevuti tutti i dati nella connessione.</span><span class="sxs-lookup"><span data-stu-id="66702-321">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="66702-322">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="66702-322">Provides the received data.</span></span>
- <span data-ttu-id="66702-323">`connectionSlow`: Generato quando il client rileva una connessione lenta o frequentemente eliminazione.</span><span class="sxs-lookup"><span data-stu-id="66702-323">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="66702-324">`reconnecting`: Generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="66702-324">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="66702-325">`reconnected`: Generato quando il trasporto sottostante è stata riconnessa.</span><span class="sxs-lookup"><span data-stu-id="66702-325">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="66702-326">`stateChanged`: Generato quando cambia lo stato della connessione.</span><span class="sxs-lookup"><span data-stu-id="66702-326">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="66702-327">Fornisce lo stato precedente e il nuovo stato (connessione, connesso, riconnessione o Disconnected).</span><span class="sxs-lookup"><span data-stu-id="66702-327">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="66702-328">`disconnected`: Generato quando la connessione è disconnesso.</span><span class="sxs-lookup"><span data-stu-id="66702-328">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="66702-329">Ad esempio, se si desidera visualizzare i messaggi di avviso quando si verificano problemi di connessione che potrebbero causare ritardi notevoli, gestire il `connectionSlow` evento.</span><span class="sxs-lookup"><span data-stu-id="66702-329">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="66702-330">**Gestire l'evento connectionSlow (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-330">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="66702-331">**Gestire l'evento connectionSlow (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-331">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="66702-332">Per altre informazioni, vedere [comprensione e la gestione degli eventi di durata connessione in SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="66702-332">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="66702-333">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="66702-333">How to handle errors</span></span>

<span data-ttu-id="66702-334">Il client SignalR JavaScript fornisce un `error` eventi che è possibile aggiungere un gestore per.</span><span class="sxs-lookup"><span data-stu-id="66702-334">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="66702-335">È anche possibile usare il metodo hanno esito negativo per aggiungere un gestore errori risultanti da una chiamata al metodo server.</span><span class="sxs-lookup"><span data-stu-id="66702-335">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="66702-336">Se si non abilita in modo esplicito i messaggi di errore dettagliato nel server, l'oggetto eccezione SignalR restituito dopo un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="66702-336">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="66702-337">Ad esempio, se una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati per i client nell'ambiente di produzione non è consigliato per motivi di sicurezza, ma se si vuole abilitare i messaggi di errore dettagliato per risoluzione dei problemi, usare il codice seguente nel server.</span><span class="sxs-lookup"><span data-stu-id="66702-337">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="66702-338">Nell'esempio seguente viene illustrato come aggiungere un gestore per l'evento di errore.</span><span class="sxs-lookup"><span data-stu-id="66702-338">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="66702-339">**Aggiungere un gestore degli errori (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-339">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="66702-340">**Aggiungere un gestore degli errori (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-340">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="66702-341">Nell'esempio seguente viene illustrato come gestire un errore da una chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="66702-341">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="66702-342">**Gestire un errore da una chiamata al metodo (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-342">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="66702-343">**Gestire un errore da una chiamata al metodo (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-343">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="66702-344">Se una chiamata al metodo ha esito negativo, il `error` evento viene generato anche in modo che il codice nel `error` gestore del metodo e nel `.fail` metodo callback verrebbe eseguito.</span><span class="sxs-lookup"><span data-stu-id="66702-344">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="66702-345">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="66702-345">How to enable client-side logging</span></span>

<span data-ttu-id="66702-346">Per abilitare la registrazione lato client in una connessione, impostare il `logging` proprietà dell'oggetto di connessione prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="66702-346">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="66702-347">**Abilitare la registrazione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-347">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="66702-348">**Abilitare la registrazione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="66702-348">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="66702-349">Per visualizzare i log, aprire Strumenti di sviluppo del browser e passare alla scheda della Console. Per un'esercitazione che illustra le istruzioni dettagliate e schermata schermate che illustrano come eseguire questa operazione, vedere [trasmissione Server con ASP.NET Signalr - abilitare la registrazione](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span><span class="sxs-lookup"><span data-stu-id="66702-349">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span></span>
