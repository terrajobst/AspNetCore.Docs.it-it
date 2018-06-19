---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: Guida di ASP.NET SignalR hub API - Client .NET (SignalR 1. x) | Documenti Microsoft
author: pfletcher
description: Questo documento viene fornita un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client di .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e gli svantaggi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 9a61bd255a217876aa2fdbeb6389539483b9f013
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28037480"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="6a87e-103">Guida di ASP.NET SignalR hub API - Client .NET (SignalR 1. x)</span><span class="sxs-lookup"><span data-stu-id="6a87e-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>
====================
<span data-ttu-id="6a87e-104">da [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="6a87e-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="6a87e-105">Questo documento viene fornita un'introduzione all'uso dell'API di hub per SignalR versione 2 nel client di .NET, ad esempio Windows Store (WinRT), WPF, Silverlight e applicazioni console.</span><span class="sxs-lookup"><span data-stu-id="6a87e-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="6a87e-106">L'API degli hub SignalR consente di eseguire chiamate di procedure remote (RPC) da un server ai client connessi e dai client al server.</span><span class="sxs-lookup"><span data-stu-id="6a87e-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="6a87e-107">Nel codice server, si definiscono i metodi che possono essere chiamati dai client e si chiamano metodi che eseguono il client.</span><span class="sxs-lookup"><span data-stu-id="6a87e-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="6a87e-108">Nel codice client, si definiscono i metodi che possono essere chiamati dal server e si chiamano metodi eseguiti nel server.</span><span class="sxs-lookup"><span data-stu-id="6a87e-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="6a87e-109">SignalR occuparsene di plumbing client-server.</span><span class="sxs-lookup"><span data-stu-id="6a87e-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="6a87e-110">SignalR offre inoltre un'API di livello inferiore denominata connessioni permanenti.</span><span class="sxs-lookup"><span data-stu-id="6a87e-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="6a87e-111">Per un'introduzione a SignalR, hub e le connessioni persistenti o per un'esercitazione che illustra come compilare un'applicazione SignalR completa, vedere [SignalR - Introduzione](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="6a87e-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="6a87e-112">Panoramica</span><span class="sxs-lookup"><span data-stu-id="6a87e-112">Overview</span></span>

<span data-ttu-id="6a87e-113">Questo documento contiene le seguenti sezioni:</span><span class="sxs-lookup"><span data-stu-id="6a87e-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="6a87e-114">Installazione del client</span><span class="sxs-lookup"><span data-stu-id="6a87e-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="6a87e-115">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="6a87e-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="6a87e-116">Connessioni tra domini da client Silverlight</span><span class="sxs-lookup"><span data-stu-id="6a87e-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="6a87e-117">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="6a87e-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="6a87e-118">Come impostare il numero massimo di connessioni simultanee in client WPF</span><span class="sxs-lookup"><span data-stu-id="6a87e-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="6a87e-119">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="6a87e-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="6a87e-120">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="6a87e-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="6a87e-121">Come specificare le intestazioni HTTP</span><span class="sxs-lookup"><span data-stu-id="6a87e-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="6a87e-122">Come specificare i certificati client</span><span class="sxs-lookup"><span data-stu-id="6a87e-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="6a87e-123">Come creare il proxy dell'Hub</span><span class="sxs-lookup"><span data-stu-id="6a87e-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="6a87e-124">Come definire i metodi nel client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="6a87e-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="6a87e-125">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="6a87e-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="6a87e-126">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="6a87e-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="6a87e-127">Metodi con parametri, specificare gli oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="6a87e-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="6a87e-128">Come rimuovere un gestore</span><span class="sxs-lookup"><span data-stu-id="6a87e-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="6a87e-129">Come chiamare i metodi di server dal client</span><span class="sxs-lookup"><span data-stu-id="6a87e-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="6a87e-130">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="6a87e-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="6a87e-131">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="6a87e-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="6a87e-132">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="6a87e-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="6a87e-133">Esempi di codice di applicazione console per i metodi di client che il server può chiamare WPF e Silverlight</span><span class="sxs-lookup"><span data-stu-id="6a87e-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="6a87e-134">Per un progetto client di esempio .NET, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="6a87e-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="6a87e-135">[Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) su GitHub.com (esempi di app console WinRT, Silverlight,).</span><span class="sxs-lookup"><span data-stu-id="6a87e-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="6a87e-136">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) su GitHub.com (ad esempio WPF).</span><span class="sxs-lookup"><span data-stu-id="6a87e-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="6a87e-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) su GitHub.com (ad esempio app Console).</span><span class="sxs-lookup"><span data-stu-id="6a87e-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="6a87e-138">Per la documentazione su come programmare il server o client JavaScript, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="6a87e-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="6a87e-139">Guida di API degli hub SignalR - Server</span><span class="sxs-lookup"><span data-stu-id="6a87e-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="6a87e-140">Guida di API degli hub SignalR - Client JavaScript</span><span class="sxs-lookup"><span data-stu-id="6a87e-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="6a87e-141">I collegamenti agli argomenti di riferimento API sono alla versione dell'API di .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="6a87e-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="6a87e-142">Se si utilizza .NET 4, vedere [la versione di .NET 4 degli argomenti API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="6a87e-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="6a87e-143">Installazione del client</span><span class="sxs-lookup"><span data-stu-id="6a87e-143">Client setup</span></span>

<span data-ttu-id="6a87e-144">Installare il [italiano](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) pacchetto NuGet (non il [Microsoft. ASPNET](http://nuget.org/packages/microsoft.aspnet.signalr) pacchetto).</span><span class="sxs-lookup"><span data-stu-id="6a87e-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="6a87e-145">Questo pacchetto supporta WinRT, Silverlight, WPF, applicazione console e i client di Windows Phone, per .NET 4 e .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="6a87e-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="6a87e-146">Se la versione di SignalR presenti nel client è diversa dalla versione di cui è installato il server, SignalR è spesso in grado di adattarsi alla differenza.</span><span class="sxs-lookup"><span data-stu-id="6a87e-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="6a87e-147">Ad esempio, quando viene rilasciato SignalR versione 2.0 e si installa che sul server, il server verrà supporta i client che dispongono di 1.1.x installata sia client che dispongono di 2.0 installato.</span><span class="sxs-lookup"><span data-stu-id="6a87e-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="6a87e-148">Se la differenza tra la versione sul server e la versione del client è eccessiva, SignalR genera un `InvalidOperationException` eccezione quando il client tenta di stabilire una connessione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="6a87e-149">Messaggio di errore "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="6a87e-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="6a87e-150">Come stabilire una connessione</span><span class="sxs-lookup"><span data-stu-id="6a87e-150">How to establish a connection</span></span>

<span data-ttu-id="6a87e-151">Prima di stabilire una connessione, è necessario creare un `HubConnection` dell'oggetto e creare un proxy.</span><span class="sxs-lookup"><span data-stu-id="6a87e-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="6a87e-152">Per stabilire la connessione, chiamare il `Start` metodo il `HubConnection` oggetto.</span><span class="sxs-lookup"><span data-stu-id="6a87e-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="6a87e-153">Per i client JavaScript è necessario registrare almeno un gestore dell'evento prima di chiamare il `Start` metodo per stabilire la connessione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="6a87e-154">Questa operazione è necessaria per i client .NET.</span><span class="sxs-lookup"><span data-stu-id="6a87e-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="6a87e-155">Per i client JavaScript, il codice proxy generato automaticamente viene creato proxy per tutti gli hub che esistano nel server e la registrazione di un gestore come indicare quali hub il client intende utilizzare.</span><span class="sxs-lookup"><span data-stu-id="6a87e-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="6a87e-156">Ma per un client .NET è creare proxy Hub manualmente, in modo SignalR si presuppone che verrà utilizzato alcun Hub che si crea un proxy per.</span><span class="sxs-lookup"><span data-stu-id="6a87e-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="6a87e-157">Il codice di esempio viene utilizzato il valore predefinito "/ signalr" URL per la connessione al servizio SignalR.</span><span class="sxs-lookup"><span data-stu-id="6a87e-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="6a87e-158">Per informazioni su come specificare un URL di base diversi, vedere [ASP.NET SignalR hub API Guida - Server - URL /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="6a87e-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="6a87e-159">Il `Start` metodo viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="6a87e-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="6a87e-160">Per assicurarsi che le successive righe di codice non vengono eseguite fino al dopo aver stabilita la connessione, utilizzare `await` in un metodo asincrono ASP.NET 4.5 o `.Wait()` in un metodo sincrono.</span><span class="sxs-lookup"><span data-stu-id="6a87e-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="6a87e-161">Non utilizzare `.Wait()` in un client di WinRT.</span><span class="sxs-lookup"><span data-stu-id="6a87e-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="6a87e-162">La classe `HubConnection` è thread-safe.</span><span class="sxs-lookup"><span data-stu-id="6a87e-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="6a87e-163">Connessioni tra domini da client Silverlight</span><span class="sxs-lookup"><span data-stu-id="6a87e-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="6a87e-164">Per informazioni su come abilitare le connessioni tra domini da client Silverlight, vedere [effettua un servizio disponibile tra confini di domini](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="6a87e-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="6a87e-165">Come configurare la connessione</span><span class="sxs-lookup"><span data-stu-id="6a87e-165">How to configure the connection</span></span>

<span data-ttu-id="6a87e-166">Prima stabilire una connessione, è possibile specificare le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6a87e-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="6a87e-167">Limite di connessioni simultanee.</span><span class="sxs-lookup"><span data-stu-id="6a87e-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="6a87e-168">Parametri di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="6a87e-168">Query string parameters.</span></span>
- <span data-ttu-id="6a87e-169">Il metodo di trasporto.</span><span class="sxs-lookup"><span data-stu-id="6a87e-169">The transport method.</span></span>
- <span data-ttu-id="6a87e-170">Intestazioni HTTP.</span><span class="sxs-lookup"><span data-stu-id="6a87e-170">HTTP headers.</span></span>
- <span data-ttu-id="6a87e-171">Certificati client.</span><span class="sxs-lookup"><span data-stu-id="6a87e-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="6a87e-172">Come impostare il numero massimo di connessioni simultanee in client WPF</span><span class="sxs-lookup"><span data-stu-id="6a87e-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="6a87e-173">Nei client WPF, potrebbe essere necessario aumentare il numero massimo di connessioni simultanee rispetto al valore predefinito di 2.</span><span class="sxs-lookup"><span data-stu-id="6a87e-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="6a87e-174">Il valore consigliato è 10.</span><span class="sxs-lookup"><span data-stu-id="6a87e-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="6a87e-175">Per ulteriori informazioni, vedere [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="6a87e-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="6a87e-176">Come specificare i parametri di stringa di query</span><span class="sxs-lookup"><span data-stu-id="6a87e-176">How to specify query string parameters</span></span>

<span data-ttu-id="6a87e-177">Se si desidera inviare dati al server quando il client si connette, è possibile aggiungere parametri di stringa di query all'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="6a87e-178">Nell'esempio seguente viene illustrato come impostare un parametro di stringa di query nel codice client.</span><span class="sxs-lookup"><span data-stu-id="6a87e-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="6a87e-179">Nell'esempio seguente viene illustrato come leggere un parametro di stringa di query nel codice server.</span><span class="sxs-lookup"><span data-stu-id="6a87e-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="6a87e-180">Come specificare il metodo di trasporto</span><span class="sxs-lookup"><span data-stu-id="6a87e-180">How to specify the transport method</span></span>

<span data-ttu-id="6a87e-181">Come parte del processo di connessione, un client SignalR normalmente negozia con il server per determinare il migliore trasporto supportato dal server e client.</span><span class="sxs-lookup"><span data-stu-id="6a87e-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="6a87e-182">Se si conosce già il trasporto a cui si desidera utilizzare, è possibile ignorare questo processo di negoziazione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="6a87e-183">Per specificare il metodo di trasporto, passare un oggetto di trasporto per il metodo di avvio.</span><span class="sxs-lookup"><span data-stu-id="6a87e-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="6a87e-184">Nell'esempio seguente viene illustrato come specificare il metodo di trasporto nel codice client.</span><span class="sxs-lookup"><span data-stu-id="6a87e-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="6a87e-185">Il [Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) spazio dei nomi include le classi seguenti che è possibile utilizzare per specificare il trasporto.</span><span class="sxs-lookup"><span data-stu-id="6a87e-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="6a87e-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="6a87e-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="6a87e-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="6a87e-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="6a87e-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponibile solo quando i server e client utilizzano .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="6a87e-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="6a87e-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (sceglie automaticamente il migliore trasporto supportato dal client sia sul server.</span><span class="sxs-lookup"><span data-stu-id="6a87e-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="6a87e-190">Questo è il trasporto predefinito.</span><span class="sxs-lookup"><span data-stu-id="6a87e-190">This is the default transport.</span></span> <span data-ttu-id="6a87e-191">Il passaggio di questa funzione è presente per il `Start` metodo ha lo stesso effetto del passaggio non qualcosa.)</span><span class="sxs-lookup"><span data-stu-id="6a87e-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="6a87e-192">Il trasporto ForeverFrame non è incluso nell'elenco perché è utilizzato solo da alcuni browser.</span><span class="sxs-lookup"><span data-stu-id="6a87e-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="6a87e-193">Per informazioni su come controllare il metodo di trasporto nel codice server, vedere [ASP.NET SignalR hub API Guida - Server - come ottenere informazioni sul client dalla proprietà di contesto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="6a87e-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="6a87e-194">Per ulteriori informazioni sui trasporti e di fallback, vedere [Introduzione a SignalR - trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="6a87e-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="6a87e-195">Come specificare le intestazioni HTTP</span><span class="sxs-lookup"><span data-stu-id="6a87e-195">How to specify HTTP headers</span></span>

<span data-ttu-id="6a87e-196">Per impostare le intestazioni HTTP, utilizzare il `Headers` proprietà sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="6a87e-197">Nell'esempio seguente viene illustrato come aggiungere un'intestazione HTTP.</span><span class="sxs-lookup"><span data-stu-id="6a87e-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="6a87e-198">Come specificare i certificati client</span><span class="sxs-lookup"><span data-stu-id="6a87e-198">How to specify client certificates</span></span>

<span data-ttu-id="6a87e-199">Per aggiungere i certificati client, utilizzare il `AddClientCertificate` metodo sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="6a87e-200">Come creare il proxy dell'Hub</span><span class="sxs-lookup"><span data-stu-id="6a87e-200">How to create the Hub proxy</span></span>

<span data-ttu-id="6a87e-201">Per definire i metodi nel client che è possibile chiamare un Hub dal server e per richiamare metodi su un Hub sul server, è possibile creare un proxy per l'Hub chiamando `CreateHubProxy` sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="6a87e-202">La stringa viene passato a `CreateHubProxy` è il nome della classe Hub o il nome specificato per il `HubName` attributo se è stata utilizzata nel server.</span><span class="sxs-lookup"><span data-stu-id="6a87e-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="6a87e-203">Nome non è rilevante.</span><span class="sxs-lookup"><span data-stu-id="6a87e-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="6a87e-204">**Classe hub sul server**</span><span class="sxs-lookup"><span data-stu-id="6a87e-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="6a87e-205">**Creare il proxy client per la classe di Hub**</span><span class="sxs-lookup"><span data-stu-id="6a87e-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="6a87e-206">Se si decorata la classe Hub con un `HubName` attributo, utilizzare tale nome.</span><span class="sxs-lookup"><span data-stu-id="6a87e-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="6a87e-207">**Classe hub sul server**</span><span class="sxs-lookup"><span data-stu-id="6a87e-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="6a87e-208">**Creare il proxy client per la classe di Hub**</span><span class="sxs-lookup"><span data-stu-id="6a87e-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="6a87e-209">L'oggetto proxy è thread-safe.</span><span class="sxs-lookup"><span data-stu-id="6a87e-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="6a87e-210">Infatti, se si chiama `HubConnection.CreateHubProxy` più volte con lo stesso `hubName`, si ottiene lo stesso nella cache `IHubProxy` oggetto.</span><span class="sxs-lookup"><span data-stu-id="6a87e-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="6a87e-211">Come definire i metodi nel client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="6a87e-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="6a87e-212">Per definire un metodo che è possibile chiamare il server, utilizzare il proxy `On` metodo per registrare un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="6a87e-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="6a87e-213">Nome di metodo corrispondente è tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="6a87e-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="6a87e-214">Ad esempio, `Clients.All.UpdateStockPrice` sul server eseguirà `updateStockPrice`, `updatestockprice`, o `UpdateStockPrice` sul client.</span><span class="sxs-lookup"><span data-stu-id="6a87e-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="6a87e-215">Piattaforme client diverse hanno requisiti diversi per la scrittura di codice di metodo per aggiornare l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="6a87e-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="6a87e-216">Gli esempi illustrati sono per i client WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="6a87e-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="6a87e-217">Vengono forniti esempi di applicazione console, WPF e Silverlight in [una sezione separata più avanti in questo argomento](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="6a87e-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="6a87e-218">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="6a87e-218">Methods without parameters</span></span>

<span data-ttu-id="6a87e-219">Se il metodo che si sta gestendo non dispone di parametri, utilizzare il metodo di overload non generico di `On` metodo:</span><span class="sxs-lookup"><span data-stu-id="6a87e-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="6a87e-220">**Chiamata del metodo client senza parametri codice server**</span><span class="sxs-lookup"><span data-stu-id="6a87e-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="6a87e-221">**Codice WinRT Client per il metodo chiamato dal server senza parametri ([WPF e Silverlight esempi più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="6a87e-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="6a87e-222">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="6a87e-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="6a87e-223">Se il metodo per gestire include parametri, specificare i tipi dei parametri come i tipi generici del `On` metodo.</span><span class="sxs-lookup"><span data-stu-id="6a87e-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="6a87e-224">Esistono overload generico del `On` metodo che consente di specificare i parametri fino a 8 (4 in Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="6a87e-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="6a87e-225">Nell'esempio seguente, viene inviato un parametro per il `UpdateStockPrice` metodo.</span><span class="sxs-lookup"><span data-stu-id="6a87e-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="6a87e-226">**Metodo client con un parametro di codice server**</span><span class="sxs-lookup"><span data-stu-id="6a87e-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="6a87e-227">**La classe Stock utilizzata per il parametro**</span><span class="sxs-lookup"><span data-stu-id="6a87e-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="6a87e-228">**Codice WinRT Client per un metodo chiamato dal server con un parametro ([WPF e Silverlight esempi più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="6a87e-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="6a87e-229">Metodi con parametri, specificare gli oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="6a87e-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="6a87e-230">In alternativa, per specificare parametri come tipi generici del `On` (metodo), è possibile specificare i parametri come oggetti dinamici:</span><span class="sxs-lookup"><span data-stu-id="6a87e-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="6a87e-231">**Metodo client con un parametro di codice server**</span><span class="sxs-lookup"><span data-stu-id="6a87e-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="6a87e-232">**La classe Stock utilizzata per il parametro**</span><span class="sxs-lookup"><span data-stu-id="6a87e-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="6a87e-233">**Codice WinRT Client per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro ([WPF e Silverlight esempi più avanti in questo argomento](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="6a87e-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="6a87e-234">Come rimuovere un gestore</span><span class="sxs-lookup"><span data-stu-id="6a87e-234">How to remove a handler</span></span>

<span data-ttu-id="6a87e-235">Per rimuovere un gestore, chiamare il relativo `Dispose` metodo.</span><span class="sxs-lookup"><span data-stu-id="6a87e-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="6a87e-236">**Codice client per un metodo chiamato dal server**</span><span class="sxs-lookup"><span data-stu-id="6a87e-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="6a87e-237">**Codice client per rimuovere il gestore**</span><span class="sxs-lookup"><span data-stu-id="6a87e-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="6a87e-238">Come chiamare i metodi di server dal client</span><span class="sxs-lookup"><span data-stu-id="6a87e-238">How to call server methods from the client</span></span>

<span data-ttu-id="6a87e-239">Per chiamare un metodo nel server, utilizzare il `Invoke` metodo sul proxy di Hub.</span><span class="sxs-lookup"><span data-stu-id="6a87e-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="6a87e-240">Se il metodo server non ha restituito alcun valore, utilizzare il metodo di overload non generico di `Invoke` metodo.</span><span class="sxs-lookup"><span data-stu-id="6a87e-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="6a87e-241">**Codice lato server per un metodo che non restituisce alcun valore**</span><span class="sxs-lookup"><span data-stu-id="6a87e-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="6a87e-242">**Codice client chiama un metodo che non restituisce alcun valore**</span><span class="sxs-lookup"><span data-stu-id="6a87e-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="6a87e-243">Se il metodo server ha un valore restituito, specificare il tipo restituito del tipo generico del `Invoke` metodo.</span><span class="sxs-lookup"><span data-stu-id="6a87e-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="6a87e-244">**Codice lato server per un metodo che restituisce un valore e accetta un parametro di tipo complesso**</span><span class="sxs-lookup"><span data-stu-id="6a87e-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="6a87e-245">**La classe Stock utilizzato per il parametro e valore restituito**</span><span class="sxs-lookup"><span data-stu-id="6a87e-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="6a87e-246">**Codice client chiama un metodo che presenta un valore restituito e accetta un parametro di tipo complesso, in un metodo asincrono ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="6a87e-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="6a87e-247">**Codice client chiama un metodo che presenta un valore restituito e accetta un parametro di tipo complesso, in un metodo sincrono**</span><span class="sxs-lookup"><span data-stu-id="6a87e-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="6a87e-248">Il `Invoke` metodo viene eseguito in modo asincrono e restituisce un `Task` oggetto.</span><span class="sxs-lookup"><span data-stu-id="6a87e-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="6a87e-249">Se non si specifica `await` o `.Wait()`, la riga successiva del codice verrà eseguito prima il metodo che si richiama ha terminato l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="6a87e-250">Come gestire gli eventi di durata connessione</span><span class="sxs-lookup"><span data-stu-id="6a87e-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="6a87e-251">SignalR fornisce la seguente connessione di eventi di durata che è possibile gestire:</span><span class="sxs-lookup"><span data-stu-id="6a87e-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="6a87e-252">`Received`: Generato quando vengono ricevuti dati per la connessione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="6a87e-253">Fornisce i dati ricevuti.</span><span class="sxs-lookup"><span data-stu-id="6a87e-253">Provides the received data.</span></span>
- <span data-ttu-id="6a87e-254">`ConnectionSlow`: Generato quando il client rileva una connessione lenta o l'eliminazione di frequente.</span><span class="sxs-lookup"><span data-stu-id="6a87e-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="6a87e-255">`Reconnecting`: Generato quando il trasporto sottostante inizia la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="6a87e-256">`Reconnected`: Generato quando il trasporto sottostante è stata riconnessa.</span><span class="sxs-lookup"><span data-stu-id="6a87e-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="6a87e-257">`StateChanged`: Generato quando cambia lo stato di connessione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="6a87e-258">Fornisce lo stato precedente e il nuovo stato.</span><span class="sxs-lookup"><span data-stu-id="6a87e-258">Provides the old state and the new state.</span></span> <span data-ttu-id="6a87e-259">Per informazioni sulla connessione vedere i valori dello stato [enumerazione ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="6a87e-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="6a87e-260">`Closed`: Generato quando la connessione è interrotta.</span><span class="sxs-lookup"><span data-stu-id="6a87e-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="6a87e-261">Ad esempio, se si desidera visualizzare i messaggi di avviso per gli errori non irreversibili ma causano problemi di connessione intermittenti, ad esempio di come ciò o frequenti eliminazione della connessione, gestiscono il `ConnectionSlow` evento.</span><span class="sxs-lookup"><span data-stu-id="6a87e-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="6a87e-262">Per ulteriori informazioni, vedere [comprensione e gestione degli eventi di durata connessione in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="6a87e-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="6a87e-263">Come gestire gli errori</span><span class="sxs-lookup"><span data-stu-id="6a87e-263">How to handle errors</span></span>

<span data-ttu-id="6a87e-264">Se si non abilita in modo esplicito i messaggi di errore dettagliato nel server, l'oggetto eccezione che restituisce SignalR dopo un errore contiene informazioni minime sull'errore.</span><span class="sxs-lookup"><span data-stu-id="6a87e-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="6a87e-265">Ad esempio, se una chiamata a `newContosoChatMessage` ha esito negativo, il messaggio di errore nell'oggetto errore contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" invio di messaggi di errore dettagliati per i client nell'ambiente di produzione non è consigliata per motivi di sicurezza, ma se si desidera abilitare per i messaggi di errore dettagliati risoluzione dei problemi, utilizzare il codice seguente nel server.</span><span class="sxs-lookup"><span data-stu-id="6a87e-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="6a87e-266">Per gestire gli errori che genera SignalR, è possibile aggiungere un gestore per il `Error` eventi sull'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="6a87e-267">Per gestire gli errori da chiamate al metodo, inserire il codice in un blocco try-catch.</span><span class="sxs-lookup"><span data-stu-id="6a87e-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="6a87e-268">Come abilitare la registrazione lato client</span><span class="sxs-lookup"><span data-stu-id="6a87e-268">How to enable client-side logging</span></span>

<span data-ttu-id="6a87e-269">Per abilitare la registrazione lato client, impostare il `TraceLevel` e `TraceWriter` le proprietà dell'oggetto connessione.</span><span class="sxs-lookup"><span data-stu-id="6a87e-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="6a87e-270">Esempi di codice di applicazione console per i metodi di client che il server può chiamare WPF e Silverlight</span><span class="sxs-lookup"><span data-stu-id="6a87e-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="6a87e-271">Gli esempi di codice illustrati in precedenza per la definizione di metodi di client che può chiamare il server si applicano ai client di WinRT.</span><span class="sxs-lookup"><span data-stu-id="6a87e-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="6a87e-272">Negli esempi seguenti mostrano l'equivalente del codice per client di applicazioni console WPF e Silverlight.</span><span class="sxs-lookup"><span data-stu-id="6a87e-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="6a87e-273">Metodi senza parametri</span><span class="sxs-lookup"><span data-stu-id="6a87e-273">Methods without parameters</span></span>

<span data-ttu-id="6a87e-274">**Codice client WPF per il metodo chiamato dal server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="6a87e-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="6a87e-275">**Codice client Silverlight per il metodo chiamato dal server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="6a87e-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="6a87e-276">**Codice client dell'applicazione console per il metodo chiamato dal server senza parametri**</span><span class="sxs-lookup"><span data-stu-id="6a87e-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="6a87e-277">Metodi con parametri, che specificano i tipi di parametro</span><span class="sxs-lookup"><span data-stu-id="6a87e-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="6a87e-278">**Codice client WPF per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="6a87e-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="6a87e-279">**Codice client Silverlight per un metodo chiamato dal server con un parametro**</span><span class="sxs-lookup"><span data-stu-id="6a87e-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="6a87e-280">**Codice dell'applicazione client per un metodo chiamato dal server con un parametro di console**</span><span class="sxs-lookup"><span data-stu-id="6a87e-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="6a87e-281">Metodi con parametri, specificare gli oggetti dinamici per i parametri</span><span class="sxs-lookup"><span data-stu-id="6a87e-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="6a87e-282">**Codice client WPF per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="6a87e-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="6a87e-283">**Codice client Silverlight per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="6a87e-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="6a87e-284">**Console di codice dell'applicazione client per un metodo chiamato dal server con un parametro, utilizzando un oggetto dinamico per il parametro**</span><span class="sxs-lookup"><span data-stu-id="6a87e-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
