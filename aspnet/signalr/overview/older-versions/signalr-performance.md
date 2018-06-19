---
uid: signalr/overview/older-versions/signalr-performance
title: Prestazioni di SignalR (SignalR 1. x) | Documenti Microsoft
author: pfletcher
description: Prestazioni di SignalR
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bad742af28d6c36bb1b66207c2ba09d140332449
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28037152"
---
<a name="signalr-performance-signalr-1x"></a><span data-ttu-id="02881-103">Prestazioni di SignalR (SignalR 1. x)</span><span class="sxs-lookup"><span data-stu-id="02881-103">SignalR Performance (SignalR 1.x)</span></span>
====================
<span data-ttu-id="02881-104">da [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="02881-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="02881-105">In questo argomento viene descritto come progettare per misurare e migliorare le prestazioni in un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="02881-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>


<span data-ttu-id="02881-106">Per una presentazione recente sulle prestazioni di SignalR e scalabilità, vedere [scala Web in tempo reale con ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="02881-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="02881-107">Di seguito sono elencate le diverse sezioni di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="02881-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="02881-108">Considerazioni sulla progettazione</span><span class="sxs-lookup"><span data-stu-id="02881-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="02881-109">Ottimizzazione delle prestazioni del server di SignalR</span><span class="sxs-lookup"><span data-stu-id="02881-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="02881-110">Risoluzione dei problemi di prestazioni</span><span class="sxs-lookup"><span data-stu-id="02881-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="02881-111">Utilizzando i contatori delle prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="02881-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="02881-112">Con altri contatori di prestazioni</span><span class="sxs-lookup"><span data-stu-id="02881-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="02881-113">Altre risorse</span><span class="sxs-lookup"><span data-stu-id="02881-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="02881-114">Considerazioni sulla progettazione</span><span class="sxs-lookup"><span data-stu-id="02881-114">Design considerations</span></span>

<span data-ttu-id="02881-115">In questa sezione descrive i modelli che possono essere implementate durante la progettazione di un'applicazione di SignalR, per garantire che le prestazioni non viene risentono generare traffico di rete non necessarie.</span><span class="sxs-lookup"><span data-stu-id="02881-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="02881-116">La limitazione della frequenza di messaggio</span><span class="sxs-lookup"><span data-stu-id="02881-116">Throttling message frequency</span></span>

<span data-ttu-id="02881-117">Anche in un'applicazione che invia messaggi a una frequenza elevata (ad esempio un'applicazione di giochi in tempo reale), la maggior parte delle applicazioni non necessario inviare più messaggi al secondo.</span><span class="sxs-lookup"><span data-stu-id="02881-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="02881-118">Per ridurre la quantità di traffico che genera ciascun client, è possibile implementare un ciclo di messaggi che code e invia i messaggi non più di frequente un corrispettivo fisso (ovvero, fino a un determinato numero di messaggi verranno inviati al secondo, se sono presenti messaggi in quel momento in terval invio).</span><span class="sxs-lookup"><span data-stu-id="02881-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="02881-119">Per un'applicazione di esempio che limita i messaggi per un determinato tasso (da client e server), vedere [in tempo reale ad alta frequenza con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="02881-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="02881-120">Riduzione delle dimensioni di messaggio</span><span class="sxs-lookup"><span data-stu-id="02881-120">Reducing message size</span></span>

<span data-ttu-id="02881-121">È possibile ridurre le dimensioni di un messaggio SignalR riducendo le dimensioni degli oggetti serializzati.</span><span class="sxs-lookup"><span data-stu-id="02881-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="02881-122">Nel codice server, se si invia un oggetto che contiene le proprietà che non devono essere trasmessi, evitare tali proprietà venga serializzato utilizzando il `JsonIgnore` attributo.</span><span class="sxs-lookup"><span data-stu-id="02881-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="02881-123">I nomi delle proprietà vengono archiviati anche nel messaggio. i nomi delle proprietà possono essere abbreviati utilizzando il `JsonProperty` attributo.</span><span class="sxs-lookup"><span data-stu-id="02881-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="02881-124">Esempio di codice seguente viene illustrato come escludere una proprietà vengano inviate al client e su come ridurre i nomi delle proprietà:</span><span class="sxs-lookup"><span data-stu-id="02881-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="02881-125">**Codice server .NET che illustra l'attributo JsonIgnore per escludere i dati vengano inviati al client e l'attributo JsonProperty per ridurre la dimensione del messaggio**</span><span class="sxs-lookup"><span data-stu-id="02881-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="02881-126">Per mantenere la leggibilità / manutenibilità nel codice client, i nomi di proprietà abbreviato possono essere rimappato a umane descrittivo nomi dopo aver ricevuto il messaggio.</span><span class="sxs-lookup"><span data-stu-id="02881-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="02881-127">Esempio di codice riportato di seguito viene illustrato uno dei modi di modifica del mapping di nomi abbreviati per più lunghi, definendo un contratto di messaggio (mapping) e l'utilizzo di `reMap` funzione per applicare il contratto per la classe messaggio ottimizzata:</span><span class="sxs-lookup"><span data-stu-id="02881-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="02881-128">**Codice JavaScript sul lato client che esegue la abbreviato i nomi delle proprietà di nomi leggibili**</span><span class="sxs-lookup"><span data-stu-id="02881-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="02881-129">Nomi possono essere abbreviati in messaggi dal client al server, anche con lo stesso metodo.</span><span class="sxs-lookup"><span data-stu-id="02881-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="02881-130">Ridurre il footprint di memoria (ovvero, la quantità di memoria utilizzata per il messaggio) del messaggio di oggetto può inoltre migliorare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="02881-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="02881-131">Ad esempio, se l'intervallo completo di un `int` non è necessaria una `short` o `byte` è invece possibile utilizzare.</span><span class="sxs-lookup"><span data-stu-id="02881-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="02881-132">Poiché i messaggi vengono archiviati nel bus di messaggi nella memoria del server, riducendo le dimensioni dei messaggi può anche risolvere i problemi di memoria server.</span><span class="sxs-lookup"><span data-stu-id="02881-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="02881-133">Ottimizzazione delle prestazioni del server di SignalR</span><span class="sxs-lookup"><span data-stu-id="02881-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="02881-134">Le impostazioni di configurazione seguente consente di ottimizzare il server per migliorare le prestazioni in un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="02881-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="02881-135">Per informazioni generali su come migliorare le prestazioni in un'applicazione ASP.NET, vedere [miglioramento delle prestazioni di ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="02881-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="02881-136">**Impostazioni di configurazione di SignalR**</span><span class="sxs-lookup"><span data-stu-id="02881-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="02881-137">**DefaultMessageBufferSize**: per impostazione predefinita, SignalR mantiene 1000 messaggi in memoria per l'hub per ogni connessione.</span><span class="sxs-lookup"><span data-stu-id="02881-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="02881-138">Se vengono utilizzati i messaggi di grandi dimensioni, questo può creare problemi di memoria che possono essere ridotta mediante la riduzione di questo valore.</span><span class="sxs-lookup"><span data-stu-id="02881-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="02881-139">Questa impostazione può essere definita `Application_Start` gestore dell'evento in un'applicazione ASP.NET o nel `Configuration` metodo di una classe di avvio OWIN in un'applicazione indipendente.</span><span class="sxs-lookup"><span data-stu-id="02881-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="02881-140">L'esempio seguente viene illustrato come ridurre questo valore per ridurre il footprint di memoria dell'applicazione per ridurre la quantità di memoria del server utilizzata:</span><span class="sxs-lookup"><span data-stu-id="02881-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="02881-141">**Codice server .NET in Global. asax per la relativa riduzione delle dimensioni predefinite del buffer del messaggio**</span><span class="sxs-lookup"><span data-stu-id="02881-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="02881-142">**Impostazioni di configurazione di IIS**</span><span class="sxs-lookup"><span data-stu-id="02881-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="02881-143">**Numero massimo di richieste simultanee per ogni applicazione**: aumentando il numero di simultanee IIS richieste aumenterà risorse server disponibili per gestire le richieste.</span><span class="sxs-lookup"><span data-stu-id="02881-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="02881-144">Il valore predefinito è 5000; Per aumentare questa impostazione, eseguire i comandi seguenti in un prompt dei comandi con privilegi elevati:</span><span class="sxs-lookup"><span data-stu-id="02881-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="02881-145">**Impostazioni di configurazione ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="02881-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="02881-146">In questa sezione include le impostazioni di configurazione che possono essere impostate nella `aspnet.config` file.</span><span class="sxs-lookup"><span data-stu-id="02881-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="02881-147">Questo file si trova in uno dei due percorsi, a seconda della piattaforma:</span><span class="sxs-lookup"><span data-stu-id="02881-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="02881-148">Impostazioni di ASP.NET che possono migliorare le prestazioni di SignalR includono quanto segue:</span><span class="sxs-lookup"><span data-stu-id="02881-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="02881-149">**Numero massimo di richieste simultaneo per CPU**: aumentare questa impostazione può ridurre i colli di bottiglia delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="02881-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="02881-150">Per aumentare questa impostazione, aggiungere la seguente impostazione di configurazione per il `aspnet.config` file:</span><span class="sxs-lookup"><span data-stu-id="02881-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="02881-151">**Limite coda richieste**: quando il numero totale di connessioni supera le `maxConcurrentRequestsPerCPU` impostazione, ASP.NET inizierà la limitazione delle richieste effettuate utilizzando una coda.</span><span class="sxs-lookup"><span data-stu-id="02881-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="02881-152">Per aumentare le dimensioni della coda, è possibile aumentare la `requestQueueLimit` impostazione.</span><span class="sxs-lookup"><span data-stu-id="02881-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="02881-153">A tale scopo, aggiungere la seguente impostazione di configurazione per il `processModel` nodo `config/machine.config` (anziché `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="02881-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="02881-154">Risoluzione dei problemi di prestazioni</span><span class="sxs-lookup"><span data-stu-id="02881-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="02881-155">In questa sezione viene descritto come individuare eventuali colli di bottiglia nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="02881-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="02881-156">Verifica che sia in uso WebSocket</span><span class="sxs-lookup"><span data-stu-id="02881-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="02881-157">SignalR è possibile utilizzare una varietà di trasporti per la comunicazione tra client e server, WebSocket offre un vantaggio significativo delle prestazioni, mentre deve essere utilizzato se supporta client e server.</span><span class="sxs-lookup"><span data-stu-id="02881-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="02881-158">Per determinare se il client e il server soddisfa i requisiti per WebSocket, vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="02881-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="02881-159">Per determinare il trasporto utilizzato nell'applicazione, è possibile utilizzare gli strumenti di sviluppo di browser e i registri per il trasporto è stata utilizzata per la connessione.</span><span class="sxs-lookup"><span data-stu-id="02881-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="02881-160">Per informazioni sull'uso degli strumenti di sviluppo del browser in Internet Explorer e Chrome, vedere [trasporti e fallback](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="02881-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="02881-161">Utilizzando i contatori delle prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="02881-161">Using SignalR performance counters</span></span>

<span data-ttu-id="02881-162">In questa sezione viene descritto come abilitare e usare i contatori delle prestazioni di SignalR, vedere il `Microsoft.AspNet.SignalR.Utils` pacchetto.</span><span class="sxs-lookup"><span data-stu-id="02881-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="02881-163">L'installazione signalr.exe</span><span class="sxs-lookup"><span data-stu-id="02881-163">Installing signalr.exe</span></span>

<span data-ttu-id="02881-164">Contatori di prestazioni possono essere aggiunti al server tramite l'utilità SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="02881-164">Peformance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="02881-165">Per installare questa utilità, seguire questi passaggi:</span><span class="sxs-lookup"><span data-stu-id="02881-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="02881-166">Nell'applicazione Visual Studio, selezionare **strumenti**, **Gestione pacchetti libreria**, **Gestisci pacchetti NuGet per la soluzione...**</span><span class="sxs-lookup"><span data-stu-id="02881-166">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="02881-167">Cercare **signalr.utils**e scegliere Installa.</span><span class="sxs-lookup"><span data-stu-id="02881-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="02881-168">Accettare il contratto di licenza per installare il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="02881-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="02881-169">Verrà installato SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="02881-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="02881-170">L'installazione di contatori delle prestazioni con SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="02881-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="02881-171">Per installare i contatori delle prestazioni di SignalR, eseguire SignalR.exe in un prompt dei comandi con privilegi elevati con il parametro seguente:</span><span class="sxs-lookup"><span data-stu-id="02881-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="02881-172">Per rimuovere i contatori delle prestazioni di SignalR, eseguire SignalR.exe in un prompt dei comandi con privilegi elevati con il parametro seguente:</span><span class="sxs-lookup"><span data-stu-id="02881-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="02881-173">Contatori delle prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="02881-173">SignalR Performance counters</span></span>

<span data-ttu-id="02881-174">Il pacchetto di utilità installa i contatori delle prestazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="02881-174">The utilites package installs the following performance counters.</span></span> <span data-ttu-id="02881-175">I contatori "Total" misurano il numero di eventi dopo l'ultimo pool di applicazioni o server riavviare.</span><span class="sxs-lookup"><span data-stu-id="02881-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="02881-176">**Metriche di connessione**</span><span class="sxs-lookup"><span data-stu-id="02881-176">**Connection metrics**</span></span>

<span data-ttu-id="02881-177">Le metriche seguenti misurano gli eventi di durata connessione che si verificano.</span><span class="sxs-lookup"><span data-stu-id="02881-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="02881-178">Per ulteriori informazioni, vedere [comprensione e gestione degli eventi di durata connessione](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="02881-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="02881-179">**Connessioni**</span><span class="sxs-lookup"><span data-stu-id="02881-179">**Connections Connected**</span></span>
- <span data-ttu-id="02881-180">**Connessioni riconnesse**</span><span class="sxs-lookup"><span data-stu-id="02881-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="02881-181">**Connessioni Disonnected**</span><span class="sxs-lookup"><span data-stu-id="02881-181">**Connections Disonnected**</span></span>
- <span data-ttu-id="02881-182">**Connessioni correnti**</span><span class="sxs-lookup"><span data-stu-id="02881-182">**Connections Current**</span></span>

<span data-ttu-id="02881-183">**Metrica messaggio**</span><span class="sxs-lookup"><span data-stu-id="02881-183">**Message metrics**</span></span>

<span data-ttu-id="02881-184">Le metriche seguenti misurano il traffico di messaggi generato da SignalR.</span><span class="sxs-lookup"><span data-stu-id="02881-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="02881-185">**Totale messaggi ricevuti di connessione**</span><span class="sxs-lookup"><span data-stu-id="02881-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="02881-186">**Totale messaggi connessione inviati**</span><span class="sxs-lookup"><span data-stu-id="02881-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="02881-187">**Connessione messaggi ricevuti/Sec**</span><span class="sxs-lookup"><span data-stu-id="02881-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="02881-188">**Connessione messaggi inviati al secondo**</span><span class="sxs-lookup"><span data-stu-id="02881-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="02881-189">**Metriche del bus messaggi**</span><span class="sxs-lookup"><span data-stu-id="02881-189">**Message bus metrics**</span></span>

<span data-ttu-id="02881-190">Le metriche seguenti misurano il traffico tramite il bus di messaggi SignalR interno, la coda in cui vengono inseriti tutti i messaggi SignalR in ingresso e in uscita.</span><span class="sxs-lookup"><span data-stu-id="02881-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="02881-191">Un messaggio è **pubblicato** quando viene inviato o trasmettere.</span><span class="sxs-lookup"><span data-stu-id="02881-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="02881-192">Oggetto **sottoscrittore** in questo contesto è una sottoscrizione nel bus di messaggi, questo deve essere uguale al numero di client più il server stesso.</span><span class="sxs-lookup"><span data-stu-id="02881-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="02881-193">Un **lavoro allocato** è un componente che invia i dati per le connessioni attive; un **lavoro occupato** è quello che sta inviando attivamente un messaggio.</span><span class="sxs-lookup"><span data-stu-id="02881-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="02881-194">**Bus di messaggi ricevuti totale dei messaggi**</span><span class="sxs-lookup"><span data-stu-id="02881-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="02881-195">**Bus di messaggi messaggi ricevuti/Sec**</span><span class="sxs-lookup"><span data-stu-id="02881-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="02881-196">**Bus di messaggi pubblicati totale**</span><span class="sxs-lookup"><span data-stu-id="02881-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="02881-197">**Bus di messaggi messaggi pubblicati/Sec**</span><span class="sxs-lookup"><span data-stu-id="02881-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="02881-198">**Messaggio Bus sottoscrizione corrente**</span><span class="sxs-lookup"><span data-stu-id="02881-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="02881-199">**Totale di sottoscrittori Bus di messaggi**</span><span class="sxs-lookup"><span data-stu-id="02881-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="02881-200">**I sottoscrittori di Bus di messaggi al secondo**</span><span class="sxs-lookup"><span data-stu-id="02881-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="02881-201">**Bus di messaggi allocati worker**</span><span class="sxs-lookup"><span data-stu-id="02881-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="02881-202">**Processi di lavoro occupati Bus messaggio**</span><span class="sxs-lookup"><span data-stu-id="02881-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="02881-203">**Messaggio Bus argomenti corrente**</span><span class="sxs-lookup"><span data-stu-id="02881-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="02881-204">**Metrica di errore**</span><span class="sxs-lookup"><span data-stu-id="02881-204">**Error metrics**</span></span>

<span data-ttu-id="02881-205">Le metriche seguenti misurano gli errori generati dal traffico di messaggi SignalR.</span><span class="sxs-lookup"><span data-stu-id="02881-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="02881-206">**Risoluzione di hub** si verificano errori quando un hub o un metodo dell'hub non può essere risolto.</span><span class="sxs-lookup"><span data-stu-id="02881-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="02881-207">**Chiamata dell'hub** gli errori sono le eccezioni generate durante la chiamata di un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="02881-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="02881-208">**Trasporto** gli errori di connessione generata durante una richiesta o risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="02881-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="02881-209">**Gli errori: Totale di tutti**</span><span class="sxs-lookup"><span data-stu-id="02881-209">**Errors: All Total**</span></span>
- <span data-ttu-id="02881-210">**Errori: All/Sec**</span><span class="sxs-lookup"><span data-stu-id="02881-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="02881-211">**Errori: Totale risoluzione di Hub**</span><span class="sxs-lookup"><span data-stu-id="02881-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="02881-212">**Errori: Risoluzione Hub/Sec**</span><span class="sxs-lookup"><span data-stu-id="02881-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="02881-213">**Errori: Totale chiamata di Hub**</span><span class="sxs-lookup"><span data-stu-id="02881-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="02881-214">**Errori: Chiamata di Hub/Sec**</span><span class="sxs-lookup"><span data-stu-id="02881-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="02881-215">**Errori: Totale messaggi trasporto**</span><span class="sxs-lookup"><span data-stu-id="02881-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="02881-216">**: Errori di trasporto/Sec**</span><span class="sxs-lookup"><span data-stu-id="02881-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="02881-217">**Metriche di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="02881-217">**Scaleout metrics**</span></span>

<span data-ttu-id="02881-218">Le metriche seguenti misurano il traffico e gli errori generati dal provider di scalabilità orizzontale.</span><span class="sxs-lookup"><span data-stu-id="02881-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="02881-219">Oggetto **flusso** in questo contesto è un'unità di scala utilizzata dal provider di scalabilità orizzontale; si tratta di una tabella se viene utilizzato SQL Server, un argomento se viene utilizzato il Bus di servizio e una sottoscrizione se viene usato Redis.</span><span class="sxs-lookup"><span data-stu-id="02881-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="02881-220">Per impostazione predefinita, viene utilizzato un solo flusso, ma questo valore può essere aumentato mediante configurazione sul Server SQL e Bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="02881-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="02881-221">Oggetto **Buffering** flusso è entrato in uno stato di errore; quando il flusso è in stato di errore, tutti i messaggi inviati al backplane avrà esito negativo immediatamente fino a quando il flusso non è in errore.</span><span class="sxs-lookup"><span data-stu-id="02881-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="02881-222">Il **lunghezza coda di invio** è il numero di messaggi che sono state registrate ma non ancora inviati.</span><span class="sxs-lookup"><span data-stu-id="02881-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="02881-223">**Bus di messaggi di scalabilità orizzontale dei messaggi ricevuti/Sec**</span><span class="sxs-lookup"><span data-stu-id="02881-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="02881-224">**Totale di flussi di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="02881-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="02881-225">**Apertura di flussi di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="02881-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="02881-226">**Flussi di scalabilità orizzontale memorizzazione nel buffer**</span><span class="sxs-lookup"><span data-stu-id="02881-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="02881-227">**Totale errori di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="02881-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="02881-228">**Errori di scalabilità orizzontale al secondo**</span><span class="sxs-lookup"><span data-stu-id="02881-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="02881-229">**Lunghezza coda di trasmissione di scalabilità orizzontale**</span><span class="sxs-lookup"><span data-stu-id="02881-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="02881-230">Per ulteriori informazioni su ciò che misurano le questi contatori, vedere [SignalR Scaleout con Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="02881-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="02881-231">Con altri contatori di prestazioni</span><span class="sxs-lookup"><span data-stu-id="02881-231">Using other performance counters</span></span>

<span data-ttu-id="02881-232">Contatori delle prestazioni seguenti possono essere utili per monitorare le prestazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="02881-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="02881-233">**Memoria**</span><span class="sxs-lookup"><span data-stu-id="02881-233">**Memory**</span></span>

- <span data-ttu-id="02881-234">Memoria CLR .NET n. di byte in tutti gli heap (per w3wp)</span><span class="sxs-lookup"><span data-stu-id="02881-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="02881-235">**ASP.NET 2.0**</span><span class="sxs-lookup"><span data-stu-id="02881-235">**ASP.NET**</span></span>

- <span data-ttu-id="02881-236">Net\richieste correnti</span><span class="sxs-lookup"><span data-stu-id="02881-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="02881-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="02881-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="02881-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="02881-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="02881-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="02881-239">**CPU**</span></span>

- <span data-ttu-id="02881-240">Tempo del processore Information\Processor</span><span class="sxs-lookup"><span data-stu-id="02881-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="02881-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="02881-241">**TCP/IP**</span></span>

- <span data-ttu-id="02881-242">TCPv6 le connessioni stabilite</span><span class="sxs-lookup"><span data-stu-id="02881-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="02881-243">TCPv4 le connessioni stabilite</span><span class="sxs-lookup"><span data-stu-id="02881-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="02881-244">**Servizio Web**</span><span class="sxs-lookup"><span data-stu-id="02881-244">**Web Service**</span></span>

- <span data-ttu-id="02881-245">Web\connessioni</span><span class="sxs-lookup"><span data-stu-id="02881-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="02881-246">Connessioni Service\Maximum Web</span><span class="sxs-lookup"><span data-stu-id="02881-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="02881-247">**Threading**</span><span class="sxs-lookup"><span data-stu-id="02881-247">**Threading**</span></span>

- <span data-ttu-id="02881-248">LocksAndThreads CLR .NET\# di thread logici attuali</span><span class="sxs-lookup"><span data-stu-id="02881-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="02881-249">Thread di .NET CLR LocksAnd\# thread fisici attuali</span><span class="sxs-lookup"><span data-stu-id="02881-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="02881-250">Altre risorse</span><span class="sxs-lookup"><span data-stu-id="02881-250">Other Resources</span></span>

<span data-ttu-id="02881-251">Per ulteriori informazioni sul monitoraggio e ottimizzazione delle prestazioni di ASP.NET, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="02881-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="02881-252">[Cenni preliminari sulle prestazioni di ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="02881-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="02881-253">ASP.NET Thread Usage on IIS 6.0, IIS 7.5 e IIS 7.0</span><span class="sxs-lookup"><span data-stu-id="02881-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="02881-254">&lt;applicationPool&gt; elemento (impostazioni Web)</span><span class="sxs-lookup"><span data-stu-id="02881-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
