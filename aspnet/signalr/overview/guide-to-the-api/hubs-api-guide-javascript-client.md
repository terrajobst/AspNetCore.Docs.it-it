---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guida all'API di ASP.NET SignalR Hubs - Client JavaScript | Microsoft Docs
author: pfletcher
description: Questo documento viene fornita un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client JavaScript, ad esempio browser e Windows Store (WinJS) applicat...
ms.author: riande
ms.date: 09/28/2015
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 9edb7fd100a3f4c5331454045ac206d2f7a81961
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912449"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="48582-103">Guida all'API di ASP.NET SignalR Hubs - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="48582-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="48582-104">dal [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="48582-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="48582-105">Questo documento fornisce un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client JavaScript, ad esempio i browser e applicazioni Windows Store (WinJS).</span><span class="sxs-lookup"><span data-stu-id="48582-105">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="48582-106">L'API di hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="48582-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="48582-107">Nel codice server, si definiscono i metodi che possono essere chiamati da parte dei client e si chiamano i metodi eseguiti nel client.</span><span class="sxs-lookup"><span data-stu-id="48582-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="48582-108">Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi che eseguono sul server.</span><span class="sxs-lookup"><span data-stu-id="48582-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="48582-109">SignalR si occupa di tutte le attività client-server.</span><span class="sxs-lookup"><span data-stu-id="48582-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="48582-110">SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="48582-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="48582-111">Per un'introduzione a SignalR hub e connessioni permanenti, vedere [Introduzione a SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="48582-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="48582-112">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="48582-112">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="48582-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="48582-113">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="48582-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="48582-114">.NET 4.5</span></span>
> - <span data-ttu-id="48582-115">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="48582-115">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="48582-116">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="48582-116">Previous versions of this topic</span></span>
>
> <span data-ttu-id="48582-117">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="48582-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="48582-118">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="48582-118">Questions and comments</span></span>
>
> <span data-ttu-id="48582-119">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="48582-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="48582-120">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="48582-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="48582-121">Panoramica</span><span class="sxs-lookup"><span data-stu-id="48582-121">Overview</span></span>

<span data-ttu-id="48582-122">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="48582-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="48582-123">Il proxy generato e che cosa fa per te</span><span class="sxs-lookup"><span data-stu-id="48582-123">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="48582-124">Quando usare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="48582-124">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="48582-125">Programma di installazione client</span><span class="sxs-lookup"><span data-stu-id="48582-125">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="48582-126">Come fanno riferimento al proxy generato in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="48582-126">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="48582-127">Come creare un file fisico per SignalR generato proxy</span><span class="sxs-lookup"><span data-stu-id="48582-127">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="48582-128">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="48582-128">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="48582-129">$. connection.hub è lo stesso oggetto consente di creare tale $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="48582-129">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="48582-130">Esecuzione asincrona del metodo start</span><span class="sxs-lookup"><span data-stu-id="48582-130">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="48582-131">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="48582-131">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="48582-132">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="48582-132">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="48582-133">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="48582-133">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="48582-134">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="48582-134">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="48582-135">Come ottenere un proxy per una classe Hub</span><span class="sxs-lookup"><span data-stu-id="48582-135">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="48582-136">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="48582-136">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="48582-137">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="48582-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="48582-138">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="48582-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="48582-139">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="48582-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="48582-140">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="48582-140">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="48582-141">Per informazioni su come programmare il server o client .NET, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="48582-141">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="48582-142">Guida all'API di SignalR Hubs - Server</span><span class="sxs-lookup"><span data-stu-id="48582-142">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="48582-143">Guida all'API di SignalR Hubs - Client .NET</span><span class="sxs-lookup"><span data-stu-id="48582-143">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="48582-144">Il componente server di SignalR 2 disponibile solo in .NET 4.5 (anche se è disponibile un client .NET per 2 SignalR in .NET 4.0).</span><span class="sxs-lookup"><span data-stu-id="48582-144">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="48582-145">Il proxy generato e che cosa fa per te</span><span class="sxs-lookup"><span data-stu-id="48582-145">The generated proxy and what it does for you</span></span>

<span data-ttu-id="48582-146">È possibile programmare un client JavaScript per comunicare con un servizio di SignalR con o senza un proxy che SignalR genera automaticamente.</span><span class="sxs-lookup"><span data-stu-id="48582-146">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="48582-147">Ciò che esegue il proxy per l'utente è semplificare la sintassi del codice usato per connettersi, metodi di scrittura che chiama il server, e chiamare metodi sul server.</span><span class="sxs-lookup"><span data-stu-id="48582-147">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="48582-148">Quando si scrive codice per chiamare i metodi di server, il proxy generato consente di usare la sintassi che sembra si erano in esecuzione una funzione locale: è possibile scrivere `serverMethod(arg1, arg2)` invece di `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="48582-148">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="48582-149">La sintassi del proxy generato consente inoltre di un errore lato client immediato e comprensibile se si digita un nome di metodo server.</span><span class="sxs-lookup"><span data-stu-id="48582-149">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="48582-150">E se si crea manualmente il file che definisce i proxy, è anche possibile ottenere supporto IntelliSense per la scrittura di codice che chiama i metodi di server.</span><span class="sxs-lookup"><span data-stu-id="48582-150">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="48582-151">Ad esempio, si supponga di che avere la seguente classe dell'Hub sul server:</span><span class="sxs-lookup"><span data-stu-id="48582-151">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="48582-152">Gli esempi di codice seguenti mostrano cosa JavaScript codice simile per richiamare il `NewContosoChatMessage` su server e ricevere le chiamate del metodo di `addContosoChatMessageToPage` metodo dal server.</span><span class="sxs-lookup"><span data-stu-id="48582-152">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="48582-153">**Con il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="48582-153">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="48582-154">**Senza il proxy generato**</span><span class="sxs-lookup"><span data-stu-id="48582-154">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="48582-155">Quando usare il proxy generato</span><span class="sxs-lookup"><span data-stu-id="48582-155">When to use the generated proxy</span></span>

<span data-ttu-id="48582-156">Se si vuole registrare più gestori di eventi per un metodo di client che chiama il server, è possibile usare il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="48582-156">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="48582-157">In caso contrario, è possibile scegliere di usare il proxy generato o non in base alle proprie preferenze di scrittura del codice.</span><span class="sxs-lookup"><span data-stu-id="48582-157">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="48582-158">Se si sceglie di non usarla, non è necessario fare riferimento all'URL "/ degli hub signalr" in un `script` elemento nel codice client.</span><span class="sxs-lookup"><span data-stu-id="48582-158">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="48582-159">Programma di installazione client</span><span class="sxs-lookup"><span data-stu-id="48582-159">Client setup</span></span>

<span data-ttu-id="48582-160">Un client JavaScript vengono richiesti riferimenti ai file SignalR core JavaScript e jQuery.</span><span class="sxs-lookup"><span data-stu-id="48582-160">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="48582-161">La versione di jQuery deve essere 1.6.4 o versioni successive principali, ad esempio 1.7.2, 1.8.2 o 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="48582-161">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="48582-162">Se si decide di usare il proxy generato, è necessario anche un riferimento al proxy di SignalR generati file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48582-162">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="48582-163">Nell'esempio seguente mostra i riferimenti all'aspetto in una pagina HTML che usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="48582-163">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="48582-164">Questi riferimenti devono essere incluso in questo ordine: jQuery, SignalR core in seguito e i proxy di SignalR cognome.</span><span class="sxs-lookup"><span data-stu-id="48582-164">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="48582-165">Come fanno riferimento al proxy generato in modo dinamico</span><span class="sxs-lookup"><span data-stu-id="48582-165">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="48582-166">Nell'esempio precedente, il riferimento al proxy di SignalR generato è al codice JavaScript generato in modo dinamico, non a un file fisico.</span><span class="sxs-lookup"><span data-stu-id="48582-166">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="48582-167">SignalR crea il codice JavaScript per il proxy in tempo reale e li fornisce ai client in risposta all'URL "/ signalr/hub".</span><span class="sxs-lookup"><span data-stu-id="48582-167">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="48582-168">Se è specificato un URL di base diversi per le connessioni SignalR nel server di `MapSignalR` metodo, l'URL del file proxy generato dinamicamente corrisponde all'URL personalizzato con "/ hub" aggiunto a tale.</span><span class="sxs-lookup"><span data-stu-id="48582-168">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="48582-169">Per i client Windows 8 (Windows Store) in JavaScript, usare il file fisico proxy anziché in quello generato in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="48582-169">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="48582-170">Per altre informazioni, vedere [come creare un file fisico per SignalR generato proxy](#manualproxy) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="48582-170">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="48582-171">In un ASP.NET MVC 4 o 5 visualizzazione Razor, usare il carattere tilde per fare riferimento alla radice dell'applicazione nel riferimento file proxy:</span><span class="sxs-lookup"><span data-stu-id="48582-171">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="48582-172">Per altre informazioni sull'uso di SignalR in MVC 5, vedere [Introduzione a SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="48582-172">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="48582-173">In una visualizzazione Razor di ASP.NET MVC 3, usare `Url.Content` relativo al riferimento al file proxy:</span><span class="sxs-lookup"><span data-stu-id="48582-173">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="48582-174">In un'applicazione Web Form ASP.NET, usare `ResolveClientUrl` per il proxy di riferimento di file o la registrazione tramite ScriptManager usando un percorso relativo della radice app (a partire da una tilde):</span><span class="sxs-lookup"><span data-stu-id="48582-174">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="48582-175">Come regola generale, usare lo stesso metodo per specificare l'URL "/ signalr/hub" che è usato per i file CSS o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48582-175">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="48582-176">Se si specifica un URL senza usare una tilde, in alcuni scenari l'applicazione funzionerà correttamente quando si test in Visual Studio con IIS Express ma avrà esito negativo con un errore 404 durante la distribuzione in modalità IIS completa.</span><span class="sxs-lookup"><span data-stu-id="48582-176">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="48582-177">Per altre informazioni, vedere **risoluzione dei riferimenti alle risorse a livello di radice** nelle [i server Web in Visual Studio per progetti Web ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx) nel sito MSDN.</span><span class="sxs-lookup"><span data-stu-id="48582-177">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="48582-178">Quando si esegue un progetto web in Visual Studio 2013 in modalità di debug e se si utilizza Internet Explorer come browser, è possibile visualizzare il file proxy nella **Esplora soluzioni** sotto **documenti Script**, come illustrato di figura seguente.</span><span class="sxs-lookup"><span data-stu-id="48582-178">When you run a web project in Visual Studio 2013 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![File proxy generato di JavaScript in Esplora soluzioni](hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="48582-180">Per visualizzare il contenuto del file, fare doppio clic su **hub**.</span><span class="sxs-lookup"><span data-stu-id="48582-180">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="48582-181">Se non si usa Visual Studio 2012 o 2013 e Internet Explorer, o se non si è in modalità di debug, è anche possibile ottenere il contenuto del file passando all'URL "/ signalR/hub".</span><span class="sxs-lookup"><span data-stu-id="48582-181">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="48582-182">Ad esempio, se il sito è in esecuzione in `http://localhost:56699`, passare a `http://localhost:56699/SignalR/hubs` nel browser.</span><span class="sxs-lookup"><span data-stu-id="48582-182">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="48582-183">Come creare un file fisico per SignalR generato proxy</span><span class="sxs-lookup"><span data-stu-id="48582-183">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="48582-184">Come alternativa per il proxy generato dinamicamente, è possibile creare un file fisico contenente il codice proxy e fare riferimento a tale file.</span><span class="sxs-lookup"><span data-stu-id="48582-184">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="48582-185">È possibile eseguire questa operazione per un controllo sulla memorizzazione nella cache o creazione di bundle comportamento o per ottenere IntelliSense quando si esegue la codifica le chiamate ai metodi di server.</span><span class="sxs-lookup"><span data-stu-id="48582-185">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="48582-186">Per creare un file proxy, seguire i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="48582-186">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="48582-187">Installare il [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="48582-187">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="48582-188">Aprire un prompt dei comandi e passare al *strumenti* cartella che contiene il file SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="48582-188">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="48582-189">La cartella strumenti si trova nella posizione seguente:</span><span class="sxs-lookup"><span data-stu-id="48582-189">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="48582-190">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="48582-190">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="48582-191">Il percorso per il *. dll* è in genere il *bin* cartella nella cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="48582-191">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="48582-192">Questo comando crea un file denominato *server. js* nella stessa cartella *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="48582-192">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="48582-193">Inserire il *server. js* file in una cartella appropriata nel progetto, rinominarlo in modo appropriato per l'applicazione, quindi aggiungere un riferimento a esso al posto del riferimento "/ degli hub signalr".</span><span class="sxs-lookup"><span data-stu-id="48582-193">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="48582-194">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="48582-194">How to establish a connection</span></span>

<span data-ttu-id="48582-195">Prima di stabilire una connessione, è necessario creare un oggetto di connessione, creare un proxy e registrare i gestori eventi per metodi che possono essere chiamati dal server.</span><span class="sxs-lookup"><span data-stu-id="48582-195">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="48582-196">Quando sono configurati i proxy e gestori eventi, è possibile stabilire la connessione tramite una chiamata di `start` (metodo).</span><span class="sxs-lookup"><span data-stu-id="48582-196">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="48582-197">Se si usa il proxy generato, non è necessario creare l'oggetto di connessione nel codice perché il codice proxy generato esegue tutto automaticamente.</span><span class="sxs-lookup"><span data-stu-id="48582-197">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="48582-198">**Stabilire una connessione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-198">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="48582-199">**Stabilire una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-199">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="48582-200">Il codice di esempio Usa il valore predefinito "/ signalr" URL per la connessione al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="48582-200">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="48582-201">Per informazioni su come specificare un URL di base diversi, vedere [Guida all'API di hub di ASP.NET SignalR - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="48582-201">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="48582-202">Per impostazione predefinita, il percorso dell'hub è il server corrente. Se ci si connette a un altro server, specificare l'URL prima di chiamare il `start` metodo, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="48582-202">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="48582-203">In genere si registrano i gestori di eventi prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="48582-203">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="48582-204">Se si vuole registrare alcuni gestori di eventi dopo avere stabilito la connessione, è possibile farlo, ma è necessario registrare almeno una dei gestori di eventi prima di chiamare il `start` (metodo).</span><span class="sxs-lookup"><span data-stu-id="48582-204">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="48582-205">Uno dei motivi è che possono essere presenti molti hub in un'applicazione, ma è preferibile attivare il `OnConnected` evento su ogni Hub se solo si intende utilizzare uno di essi.</span><span class="sxs-lookup"><span data-stu-id="48582-205">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="48582-206">Quando viene stabilita la connessione, la presenza di un metodo client nel proxy dell'Hub è che indica a SignalR per attivare il `OnConnected` evento.</span><span class="sxs-lookup"><span data-stu-id="48582-206">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="48582-207">Se non si registrano i gestori di eventi prima di chiamare il `start` metodo, sarà in grado di richiamare i metodi per l'Hub, ma l'Hub `OnConnected` non chiamare il metodo e non verrà richiamato Nessun metodo client dal server.</span><span class="sxs-lookup"><span data-stu-id="48582-207">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="48582-208">$. connection.hub è lo stesso oggetto consente di creare tale $.hubConnection()</span><span class="sxs-lookup"><span data-stu-id="48582-208">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="48582-209">Come si può notare dagli esempi, quando si usa il proxy generato, `$.connection.hub` fa riferimento all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="48582-209">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="48582-210">Questo è lo stesso oggetto che si ottiene chiamando `$.hubConnection()` quando non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="48582-210">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="48582-211">Il codice proxy generato crea la connessione tramite l'istruzione seguente:</span><span class="sxs-lookup"><span data-stu-id="48582-211">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Creazione di una connessione nel file proxy generato](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="48582-213">Quando si usa il proxy generato, è possibile eseguire qualsiasi operazione con `$.connection.hub` che è possibile eseguire con un oggetto connessione quando non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="48582-213">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="48582-214">Esecuzione asincrona del metodo start</span><span class="sxs-lookup"><span data-stu-id="48582-214">Asynchronous execution of the start method</span></span>

<span data-ttu-id="48582-215">Il `start` metodo viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="48582-215">The `start` method executes asynchronously.</span></span> <span data-ttu-id="48582-216">Restituisce un [oggetto Deferred jQuery](http://api.jquery.com/category/deferred-object/), il che significa che è possibile aggiungere le funzioni di callback da chiamare metodi quali `pipe`, `done`, e `fail`.</span><span class="sxs-lookup"><span data-stu-id="48582-216">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="48582-217">Se si dispone di codice che si desidera eseguire dopo aver stabilita la connessione, ad esempio una chiamata a un metodo server, tale codice inserito in una funzione di callback o chiamarlo da una funzione di callback.</span><span class="sxs-lookup"><span data-stu-id="48582-217">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="48582-218">Il `.done` metodo di callback viene eseguito dopo aver stabilita la connessione e dopo qualsiasi codice che è necessario `OnConnected` metodo del gestore eventi nel server al termine dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="48582-218">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="48582-219">Se si inserisce l'istruzione "Ora connessi" dell'esempio precedente alla riga successiva del codice dopo il `start` chiamata al metodo (non in un `.done` callback), il `console.log` riga verrà eseguita prima che venga stabilita la connessione, come illustrato nell'esempio seguente esempio:</span><span class="sxs-lookup"><span data-stu-id="48582-219">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Modo errato per scrivere codice che viene eseguito una volta stabilita connessione](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="48582-221">Come stabilire una connessione tra domini</span><span class="sxs-lookup"><span data-stu-id="48582-221">How to establish a cross-domain connection</span></span>

<span data-ttu-id="48582-222">In genere se il browser ha caricato una pagina dalla `http://contoso.com`, la connessione SignalR è nello stesso dominio, in `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="48582-222">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="48582-223">Se la pagina dalla `http://contoso.com` stabilisce una connessione a `http://fabrikam.com/signalr`, vale a dire una connessione tra domini.</span><span class="sxs-lookup"><span data-stu-id="48582-223">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="48582-224">Per motivi di sicurezza, le connessioni tra domini sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="48582-224">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="48582-225">In SignalR 1.x, richieste tra domini sono state controllate da un singolo flag EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="48582-225">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="48582-226">Questo flag controllata richieste sia JSONP e la condivisione CORS.</span><span class="sxs-lookup"><span data-stu-id="48582-226">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="48582-227">Per una maggiore flessibilità, supporto di CORS tutti è stata rimossa dal componente server di SignalR (client JavaScript comunque usano CORS in genere se è stato rilevato che il browser supporti), e nuovi middleware OWIN è stato reso disponibile per supportare questi scenari.</span><span class="sxs-lookup"><span data-stu-id="48582-227">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="48582-228">Se JSONP è obbligatorio per il client (per supportare le richieste tra domini in browser meno recenti), dovrà essere abilitata in modo esplicito impostando `EnableJSONP` nella `HubConfiguration` oggetto `true`, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="48582-228">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="48582-229">JSONP è disabilitata per impostazione predefinita, perché è meno sicuro rispetto a CORS.</span><span class="sxs-lookup"><span data-stu-id="48582-229">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="48582-230">**Aggiunta di owin al progetto:** per installare questa libreria, eseguire il comando seguente nella Console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="48582-230">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="48582-231">Questo comando aggiunge il 2.1.0 versione del pacchetto al progetto.</span><span class="sxs-lookup"><span data-stu-id="48582-231">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="48582-232">Chiamare UseCors</span><span class="sxs-lookup"><span data-stu-id="48582-232">Calling UseCors</span></span>

 <span data-ttu-id="48582-233">Il frammento di codice seguente viene illustrato come implementare le connessioni tra domini in SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="48582-233">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="48582-234">**Implementazione di richieste tra domini in SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="48582-234">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="48582-235">Il codice seguente illustra come abilitare JSONP o CORS in un progetto SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="48582-235">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="48582-236">Questo esempio di codice viene utilizzato `Map` e `RunSignalR` invece di `MapSignalR`, in modo che il middleware CORS viene eseguita solo per le richieste di SignalR che richiedono il supporto CORS (invece che per tutto il traffico nel percorso specificato `MapSignalR`.) Mappa può essere usata anche per qualsiasi altro middleware che deve essere eseguita per un prefisso URL specifico, anziché per l'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="48582-236">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="48582-237">Non impostare `jQuery.support.cors` su true nel codice.</span><span class="sxs-lookup"><span data-stu-id="48582-237">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![Non impostare jQuery.support.cors su true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="48582-239">SignalR gestisce l'uso di CORS.</span><span class="sxs-lookup"><span data-stu-id="48582-239">SignalR handles the use of CORS.</span></span> <span data-ttu-id="48582-240">Impostazione `jQuery.support.cors` a true Disabilita JSONP perché causa SignalR per presupporre il browser supporta la condivisione CORS.</span><span class="sxs-lookup"><span data-stu-id="48582-240">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="48582-241">Quando ci si connette a un URL localhost, Internet Explorer 10 non considera una connessione tra domini, in modo che l'applicazione funzionerà in locale con Internet Explorer 10 anche se non è stata abilitata la connessione tra domini nel server.</span><span class="sxs-lookup"><span data-stu-id="48582-241">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="48582-242">Per informazioni sull'uso di connessioni tra domini con Internet Explorer 9, vedere [thread di StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="48582-242">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="48582-243">Per informazioni sull'uso di connessioni tra domini con Chrome, vedere [thread di StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="48582-243">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="48582-244">Il codice di esempio Usa il valore predefinito "/ signalr" URL per la connessione al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="48582-244">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="48582-245">Per informazioni su come specificare un URL di base diversi, vedere [Guida all'API di hub di ASP.NET SignalR - Server - URL /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="48582-245">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="48582-246">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="48582-246">How to configure the connection</span></span>

<span data-ttu-id="48582-247">Prima di aver stabilito una connessione, è possibile specificare parametri della stringa di query o specificare il metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="48582-247">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="48582-248">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="48582-248">How to specify query string parameters</span></span>

<span data-ttu-id="48582-249">Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri della stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="48582-249">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="48582-250">Negli esempi seguenti illustrano come impostare un parametro di stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="48582-250">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="48582-251">**Impostare un valore di stringa di query prima di chiamare il metodo start (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-251">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="48582-252">**Impostare un valore di stringa di query prima di chiamare il metodo start (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-252">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="48582-253">Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.</span><span class="sxs-lookup"><span data-stu-id="48582-253">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="48582-254">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="48582-254">How to specify the transport method</span></span>

<span data-ttu-id="48582-255">Durante il processo di connessione, un client SignalR normalmente negozia con il server per determinare il migliore trasporto supportato dal server e client.</span><span class="sxs-lookup"><span data-stu-id="48582-255">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="48582-256">Se si conosce già il trasporto da usare, è possibile ignorare questo processo di negoziazione specificando il metodo di trasporto quando si chiama il `start` (metodo).</span><span class="sxs-lookup"><span data-stu-id="48582-256">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="48582-257">**Codice client che specifica il metodo di trasporto (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-257">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="48582-258">**Codice client che specifica il metodo di trasporto (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-258">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="48582-259">In alternativa, è possibile specificare più metodi di trasporto nell'ordine in cui si desidera SignalR per provarle in:</span><span class="sxs-lookup"><span data-stu-id="48582-259">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="48582-260">**Codice client che specifica uno schema di fallback di trasporto personalizzato (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-260">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="48582-261">**Codice client che specifica uno schema di fallback di trasporto personalizzato (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-261">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="48582-262">È possibile usare i valori seguenti per specificare il metodo di trasporto:</span><span class="sxs-lookup"><span data-stu-id="48582-262">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="48582-263">"WebSocket"</span><span class="sxs-lookup"><span data-stu-id="48582-263">"webSockets"</span></span>
- <span data-ttu-id="48582-264">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="48582-264">"foreverFrame"</span></span>
- <span data-ttu-id="48582-265">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="48582-265">"serverSentEvents"</span></span>
- <span data-ttu-id="48582-266">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="48582-266">"longPolling"</span></span>

<span data-ttu-id="48582-267">Gli esempi seguenti illustrano come determinare quale metodo di trasporto viene utilizzato da una connessione.</span><span class="sxs-lookup"><span data-stu-id="48582-267">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="48582-268">**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-268">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="48582-269">**Codice client che visualizza il metodo di trasporto utilizzato da una connessione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-269">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="48582-270">Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [- Server - Guida all'API Hubs SignalR di ASP.NET come ottenere informazioni relative al client dalla proprietà di contesto](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="48582-270">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="48582-271">Per altre informazioni sui trasporti e i fallback, vedere [Introduzione a SignalR - trasporti e i fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="48582-271">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="48582-272">Come ottenere un proxy per una classe Hub</span><span class="sxs-lookup"><span data-stu-id="48582-272">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="48582-273">Ogni oggetto di connessione che crea incapsula informazioni su una connessione a un servizio SignalR contenente uno o più classi di Hub.</span><span class="sxs-lookup"><span data-stu-id="48582-273">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="48582-274">Per comunicare con una classe di Hub, usare un oggetto proxy che create dall'utente (se non si usa il proxy generato) o che viene generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="48582-274">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="48582-275">Nel client il nome del proxy è una versione con notazione camel del nome della classe dell'Hub.</span><span class="sxs-lookup"><span data-stu-id="48582-275">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="48582-276">SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48582-276">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="48582-277">**Classe dell'hub sul server**</span><span class="sxs-lookup"><span data-stu-id="48582-277">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="48582-278">**Ottenere un riferimento al proxy client generato per l'Hub**</span><span class="sxs-lookup"><span data-stu-id="48582-278">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="48582-279">**Creare il proxy client per la classe Hub (senza proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-279">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="48582-280">Se si decora la classe di Hub con un `HubName` attributo, usare il nome esatto senza modificare maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="48582-280">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="48582-281">**Classe dell'hub sul server con l'attributo HubName**</span><span class="sxs-lookup"><span data-stu-id="48582-281">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="48582-282">**Ottenere un riferimento al proxy client generato per l'Hub**</span><span class="sxs-lookup"><span data-stu-id="48582-282">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="48582-283">**Creare il proxy client per la classe Hub (senza proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-283">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="48582-284">Come definire i metodi sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="48582-284">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="48582-285">Per definire un metodo che il server è possibile chiamare da un Hub, aggiungere un gestore eventi per il proxy dell'Hub usando il `client` proprietà del proxy generato o chiamata di `on` metodo se non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="48582-285">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="48582-286">I parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="48582-286">The parameters can be complex objects.</span></span>

<span data-ttu-id="48582-287">Aggiungere il gestore dell'evento prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="48582-287">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="48582-288">(Se si desidera aggiungere i gestori eventi dopo la chiamata di `start` metodo, vedere la nota nella [come stabilire una connessione](#establishconnection) in precedenza in questo documento e usare la sintassi illustrata per la definizione di un metodo senza usare il proxy generato.)</span><span class="sxs-lookup"><span data-stu-id="48582-288">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="48582-289">Metodo nome viene applicata alcuna distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="48582-289">Method name matching is case-insensitive.</span></span> <span data-ttu-id="48582-290">Ad esempio, `Clients.All.addContosoChatMessageToPage` sul server eseguirà `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, o `addcontosochatmessagetopage` sul client.</span><span class="sxs-lookup"><span data-stu-id="48582-290">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="48582-291">**Definire un metodo nel client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-291">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="48582-292">**Modo alternativo per definire il metodo nel client (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-292">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="48582-293">**Definire un metodo nel client (senza il proxy generato o durante l'aggiunta dopo aver chiamato il metodo di avvio)**</span><span class="sxs-lookup"><span data-stu-id="48582-293">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="48582-294">**Codice del server che chiama il metodo client**</span><span class="sxs-lookup"><span data-stu-id="48582-294">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="48582-295">Negli esempi seguenti includono un oggetto complesso come un parametro del metodo.</span><span class="sxs-lookup"><span data-stu-id="48582-295">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="48582-296">**Definire un metodo sul client che accetta un oggetto complesso (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-296">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="48582-297">**Definire un metodo sul client che accetta un oggetto complesso (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-297">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="48582-298">**Codice del server che definisce l'oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="48582-298">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="48582-299">**Codice del server che chiama il metodo client utilizzando un oggetto complesso**</span><span class="sxs-lookup"><span data-stu-id="48582-299">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="48582-300">Come chiamare i metodi del server dal client</span><span class="sxs-lookup"><span data-stu-id="48582-300">How to call server methods from the client</span></span>

<span data-ttu-id="48582-301">Per chiamare un metodo server dal client, usare il `server` proprietà del proxy generato o `invoke` metodo sul proxy di Hub, se non si usa il proxy generato.</span><span class="sxs-lookup"><span data-stu-id="48582-301">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="48582-302">Il valore restituito o i parametri possono essere oggetti complessi.</span><span class="sxs-lookup"><span data-stu-id="48582-302">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="48582-303">Passare una versione maiuscole-minuscole camel del nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="48582-303">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="48582-304">SignalR esegue automaticamente questa modifica in modo che il codice JavaScript può essere conforme alle convenzioni di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="48582-304">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="48582-305">Gli esempi seguenti illustrano come chiamare un metodo server che non ha un valore restituito e come chiamare un metodo del server che hanno un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="48582-305">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="48582-306">**Metodo server senza l'attributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="48582-306">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="48582-307">**Codice del server che definisce l'oggetto complesso passato un parametro**</span><span class="sxs-lookup"><span data-stu-id="48582-307">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="48582-308">**Codice client che richiama il metodo del server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-308">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="48582-309">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-309">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="48582-310">Se è decorata del metodo dell'Hub con un `HubMethodName` attributo, usare il nome senza modificare maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="48582-310">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="48582-311">**Metodo server** con un attributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="48582-311">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="48582-312">**Codice client che richiama il metodo del server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-312">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="48582-313">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-313">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="48582-314">Negli esempi precedenti viene illustrato come chiamare un metodo del server che non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="48582-314">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="48582-315">Negli esempi seguenti illustrano come chiamare un metodo server che ha un valore restituito.</span><span class="sxs-lookup"><span data-stu-id="48582-315">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="48582-316">**Codice del server per un metodo che ha un valore restituito**</span><span class="sxs-lookup"><span data-stu-id="48582-316">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="48582-317">**La classe Stock utilizzata per il** valore restituito</span><span class="sxs-lookup"><span data-stu-id="48582-317">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="48582-318">**Codice client che richiama il metodo del server (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-318">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="48582-319">**Codice client che richiama il metodo server (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-319">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="48582-320">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="48582-320">How to handle connection lifetime events</span></span>

<span data-ttu-id="48582-321">SignalR fornisce il collegamento seguente gli eventi di durata che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="48582-321">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="48582-322">`starting`: Generato prima che tutti i dati vengono inviati tramite la connessione.</span><span class="sxs-lookup"><span data-stu-id="48582-322">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="48582-323">`received`: Generato quando vengono ricevuti dati sulla connessione.</span><span class="sxs-lookup"><span data-stu-id="48582-323">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="48582-324">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="48582-324">Provides the received data.</span></span>
- <span data-ttu-id="48582-325">`connectionSlow`: Generato quando il client rileva una connessione lenta o frequentemente eliminazione.</span><span class="sxs-lookup"><span data-stu-id="48582-325">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="48582-326">`reconnecting`: Generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="48582-326">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="48582-327">`reconnected`: Generato quando il trasporto sottostante è stata riconnessa.</span><span class="sxs-lookup"><span data-stu-id="48582-327">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="48582-328">`stateChanged`: Generato quando cambia lo stato della connessione.</span><span class="sxs-lookup"><span data-stu-id="48582-328">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="48582-329">Fornisce lo stato precedente e il nuovo stato (connessione, connesso, riconnessione o Disconnected).</span><span class="sxs-lookup"><span data-stu-id="48582-329">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="48582-330">`disconnected`: Generato quando la connessione è disconnesso.</span><span class="sxs-lookup"><span data-stu-id="48582-330">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="48582-331">Ad esempio, se si desidera visualizzare i messaggi di avviso quando si verificano problemi di connessione che potrebbero causare ritardi notevoli, gestire il `connectionSlow` evento.</span><span class="sxs-lookup"><span data-stu-id="48582-331">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="48582-332">**Gestire l'evento connectionSlow (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-332">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="48582-333">**Gestire l'evento connectionSlow (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-333">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="48582-334">Per altre informazioni, vedere [comprensione e la gestione degli eventi di durata connessione in SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="48582-334">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="48582-335">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="48582-335">How to handle errors</span></span>

<span data-ttu-id="48582-336">Il client SignalR JavaScript fornisce un `error` eventi che è possibile aggiungere un gestore per.</span><span class="sxs-lookup"><span data-stu-id="48582-336">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="48582-337">È anche possibile usare il metodo hanno esito negativo per aggiungere un gestore errori risultanti da una chiamata al metodo server.</span><span class="sxs-lookup"><span data-stu-id="48582-337">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="48582-338">Se si non abilita in modo esplicito i messaggi di errore dettagliato nel server, l'oggetto eccezione SignalR restituito dopo un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="48582-338">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="48582-339">Ad esempio, se una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati per i client nell'ambiente di produzione non è consigliato per motivi di sicurezza, ma se si vuole abilitare i messaggi di errore dettagliato per risoluzione dei problemi, usare il codice seguente nel server.</span><span class="sxs-lookup"><span data-stu-id="48582-339">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="48582-340">Nell'esempio seguente viene illustrato come aggiungere un gestore per l'evento di errore.</span><span class="sxs-lookup"><span data-stu-id="48582-340">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="48582-341">**Aggiungere un gestore degli errori (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-341">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="48582-342">**Aggiungere un gestore degli errori (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-342">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="48582-343">Nell'esempio seguente viene illustrato come gestire un errore da una chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="48582-343">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="48582-344">**Gestire un errore da una chiamata al metodo (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-344">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="48582-345">**Gestire un errore da una chiamata al metodo (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-345">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="48582-346">Se una chiamata al metodo ha esito negativo, il `error` evento viene generato anche in modo che il codice nel `error` gestore del metodo e nel `.fail` metodo callback verrebbe eseguito.</span><span class="sxs-lookup"><span data-stu-id="48582-346">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="48582-347">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="48582-347">How to enable client-side logging</span></span>

<span data-ttu-id="48582-348">Per abilitare la registrazione lato client in una connessione, impostare il `logging` proprietà dell'oggetto di connessione prima di chiamare il `start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="48582-348">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="48582-349">**Abilitare la registrazione (con il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-349">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="48582-350">**Abilitare la registrazione (senza il proxy generato)**</span><span class="sxs-lookup"><span data-stu-id="48582-350">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="48582-351">Per visualizzare i log, aprire Strumenti di sviluppo del browser e passare alla scheda della Console. Per un'esercitazione che illustra le istruzioni dettagliate e schermata schermate che illustrano come eseguire questa operazione, vedere [trasmissione Server con ASP.NET Signalr - abilitare la registrazione](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span><span class="sxs-lookup"><span data-stu-id="48582-351">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span></span>
