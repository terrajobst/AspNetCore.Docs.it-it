---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Esercitazione: Trasmissione server con SignalR 2 | Microsoft Docs'
author: tdykstra
description: Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR 2 per fornire funzionalità di trasmissione di server.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a243c78c7d552f1c82a88c6083871fcd16538618
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837429"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="4cd0d-103">Esercitazione: Server di trasmissione con SignalR 2</span><span class="sxs-lookup"><span data-stu-id="4cd0d-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="4cd0d-104">Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR 2 per fornire funzionalità di trasmissione di server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="4cd0d-105">Trasmissione del server significa che il server inizia le comunicazioni inviate ai client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="4cd0d-106">L'applicazione che verrà creata in questa esercitazione simula le quotazioni di borsa, uno scenario tipico per la funzionalità di trasmissione di server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="4cd0d-107">Periodicamente, il server in modo casuale degli aggiornamenti di quotazioni di titoli e trasmissione gli aggiornamenti a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="4cd0d-108">Nel browser, i numeri e simboli nel **cambiare** e **%** colonne modificate in modo dinamico in risposta a notifiche dal server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="4cd0d-109">Se si apre browser aggiuntivi allo stesso URL, sono visualizzate le stesse modifiche ai dati e gli stessi dati contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Creazione di web](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="4cd0d-111">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4cd0d-112">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="4cd0d-112">Create the project</span></span>
> * <span data-ttu-id="4cd0d-113">Impostare il codice server</span><span class="sxs-lookup"><span data-stu-id="4cd0d-113">Set up the server code</span></span>
> * <span data-ttu-id="4cd0d-114">Esaminare il codice del server</span><span class="sxs-lookup"><span data-stu-id="4cd0d-114">Examine the server code</span></span>
> * <span data-ttu-id="4cd0d-115">Impostare il codice client</span><span class="sxs-lookup"><span data-stu-id="4cd0d-115">Set up the client code</span></span>
> * <span data-ttu-id="4cd0d-116">Esaminare il codice client</span><span class="sxs-lookup"><span data-stu-id="4cd0d-116">Examine the client code</span></span>
> * <span data-ttu-id="4cd0d-117">Testare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="4cd0d-117">Test the application</span></span>
> * <span data-ttu-id="4cd0d-118">Abilitare la registrazione</span><span class="sxs-lookup"><span data-stu-id="4cd0d-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4cd0d-119">Se non si desidera eseguire i passaggi di compilazione dell'applicazione, è possibile installare il pacchetto SignalR.Sample in un nuovo progetto di applicazione Web ASP.NET vuota.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="4cd0d-120">Se si installa il pacchetto NuGet senza eseguire i passaggi descritti in questa esercitazione, è necessario seguire le istruzioni riportate nel *Readme. txt* file.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="4cd0d-121">Per eseguire il pacchetto è necessario aggiungere una OWIN startup classe che chiama il `ConfigureSignalR` metodo nel pacchetto installato.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="4cd0d-122">Si riceverà un errore se non si aggiunge la classe di avvio OWIN.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="4cd0d-123">Vedere le [installare il campione StockTicker](#install-the-stockticker-sample) sezione di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="4cd0d-124">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4cd0d-124">Prerequisites</span></span>

 * <span data-ttu-id="4cd0d-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il **sviluppo ASP.NET e web** carico di lavoro.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="4cd0d-126">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="4cd0d-126">Create the project</span></span>

<span data-ttu-id="4cd0d-127">Questa sezione illustra come usare Visual Studio 2017 per creare un'applicazione Web ASP.NET vuota.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="4cd0d-128">In Visual Studio, creare un'applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Creazione di web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="4cd0d-130">Nel **nuova applicazione Web ASP.NET - SignalR.StockTicker** finestra, lasciare **vuota** selezionato e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="4cd0d-131">Impostare il codice server</span><span class="sxs-lookup"><span data-stu-id="4cd0d-131">Set up the server code</span></span>

<span data-ttu-id="4cd0d-132">In questa sezione è impostare il codice eseguito nel server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="4cd0d-133">Creare la classe Stock</span><span class="sxs-lookup"><span data-stu-id="4cd0d-133">Create the Stock class</span></span>

<span data-ttu-id="4cd0d-134">È necessario creare innanzitutto le *Stock* classe che si userà per archiviare e trasmettere informazioni di riferimento di un modello.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="4cd0d-135">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="4cd0d-136">Denominare la classe *Stock* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="4cd0d-137">Sostituire il codice nel *Stock.cs* file con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="4cd0d-138">Sono le due proprietà che si imposteranno quando si crea stocks `Symbol` (ad esempio, MSFT per Microsoft) e `Price`.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="4cd0d-139">Le altre proprietà dipende da come e quando si imposta `Price`.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="4cd0d-140">La prima volta è impostato `Price`, il valore ottiene propagato a `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="4cd0d-141">Successivamente, quando si imposta `Price`, l'app viene calcolato il `Change` e `PercentChange` i valori delle proprietà in base alla differenza tra `Price` e `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="4cd0d-142">Creare le classi StockTickerHub e StockTicker</span><span class="sxs-lookup"><span data-stu-id="4cd0d-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="4cd0d-143">Si userà l'API di Hub di SignalR per gestire l'interazione di server a client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="4cd0d-144">Oggetto `StockTickerHub` classe che deriva dal `SignalRHub` classe gestirà la ricezione di chiamate al metodo e le connessioni dai client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-144">A `StockTickerHub` class that derives from the `SignalRHub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="4cd0d-145">È anche necessario mantenere i dati azionari ed eseguire un `Timer` oggetto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="4cd0d-146">Il `Timer` oggetto attiverà periodicamente aggiornamenti prezzo indipendenti di connessioni client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="4cd0d-147">È possibile inserire queste funzioni in un `Hub` classe, poiché gli hub sono temporanei.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="4cd0d-148">L'app crea un `Hub` istanza della classe per ogni attività nell'hub, come le connessioni e chiamate dal client al server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="4cd0d-149">In modo che il meccanismo che mantiene i dati azionari, aggiorna i prezzi e trasmette gli aggiornamenti di prezzo deve essere eseguito in una classe separata.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="4cd0d-150">È possibile denominare la classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-150">You'll name the class `StockTicker`.</span></span>

![La trasmissione da StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="4cd0d-152">Si vuole solo un'istanza del `StockTicker` classe per l'esecuzione nel server, quindi è necessario configurare un riferimento da ognuna `StockTickerHub` istanza per il singleton `StockTicker` istanza.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="4cd0d-153">Il `StockTicker` classe deve trasmettere ai client ha dati azionari e attiva gli aggiornamenti, ma `StockTicker` non è un `Hub` classe.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="4cd0d-154">Il `StockTicker` classe deve ottenere un riferimento all'oggetto di contesto di connessione del SignalR Hub.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="4cd0d-155">Quindi possibile usare l'oggetto di contesto di connessione SignalR per trasmettere ai client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="4cd0d-156">Creare StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="4cd0d-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="4cd0d-157">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="4cd0d-158">Nelle **Aggiungi nuovo elemento - SignalR.StockTicker**, selezionare **installati** > **Visual C#**   >  **Web**  >  **SignalR** e quindi selezionare **classe tramite SignalR Hub (v2)**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="4cd0d-159">Denominare la classe *StockTickerHub* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="4cd0d-160">Questo passaggio Crea il *StockTickerHub.cs* file di classe.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="4cd0d-161">Contemporaneamente, aggiunge un set di file di script e i riferimenti agli assembly che supporta SignalR al progetto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="4cd0d-162">Sostituire il codice nel *StockTickerHub.cs* file con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="4cd0d-163">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-163">Save the file.</span></span>

<span data-ttu-id="4cd0d-164">L'app Usa la [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) classe per definire metodi che i client possono chiamare sul server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="4cd0d-165">Si definisce un metodo: `GetAllStocks()`.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="4cd0d-166">Quando un client si connette inizialmente al server, chiama questo metodo per ottenere un elenco di tutti i titoli di borsa con i relativi prezzi correnti.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="4cd0d-167">Il metodo può eseguire in modo sincrono e restituire `IEnumerable<Stock>` perché restituisce i dati dalla memoria.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="4cd0d-168">Se il metodo deve ottenere i dati in questo modo qualcosa che richiederebbe l'intervento di attesa, ad esempio una ricerca nel database o una chiamata al servizio web, si specificherà `Task<IEnumerable<Stock>>` come valore restituito per abilitare l'elaborazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="4cd0d-169">Per altre informazioni, vedere [Guida all'API di hub di ASP.NET SignalR - Server - quando è necessario eseguire in modo asincrono](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="4cd0d-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="4cd0d-170">Il `HubName` attributo specifica la modalità l'app verrà fatto riferimento l'Hub nel codice JavaScript nel client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="4cd0d-171">Il nome predefinito sul client se non si usa questo attributo, è una versione camelCase del nome della classe, che in questo caso sarebbe `stockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="4cd0d-172">Come si vedrà in seguito quando si crea il `StockTicker` (classe), l'app crea un'istanza singleton della classe in statica `Instance` proprietà.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="4cd0d-173">Istanza singleton di `StockTicker` in memoria, indipendentemente dal numero di client di connessione o disconnessione.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="4cd0d-174">Tale istanza è quello che fa il `GetAllStocks()` metodo utilizzato per restituire informazioni sulle azioni corrente.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="4cd0d-175">Creare StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="4cd0d-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="4cd0d-176">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="4cd0d-177">Denominare la classe *StockTicker* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="4cd0d-178">Sostituire il codice nel *StockTicker.cs* file con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="4cd0d-179">Poiché tutti i thread verranno eseguita la stessa istanza del codice StockTicker, la classe StockTicker deve essere thread-safe.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="4cd0d-180">Esaminare il codice del server</span><span class="sxs-lookup"><span data-stu-id="4cd0d-180">Examine the server code</span></span>

<span data-ttu-id="4cd0d-181">Se si esamina il codice del server, aiuterà a comprendere il funzionamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="4cd0d-182">Archiviare l'istanza singleton in un campo statico</span><span class="sxs-lookup"><span data-stu-id="4cd0d-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="4cd0d-183">Il codice inizializza il metodo statico `_instance` campo sottostante il `Instance` proprietà con un'istanza della classe.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="4cd0d-184">Poiché il costruttore è privato, è l'unica istanza della classe in grado di creare l'app.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="4cd0d-185">L'app Usa [inizializzazione differita](/dotnet/framework/performance/lazy-initialization) per il `_instance` campo.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="4cd0d-186">Non è per motivi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-186">It's not for performance reasons.</span></span> <span data-ttu-id="4cd0d-187">È per assicurarsi che la creazione dell'istanza è thread-safe.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="4cd0d-188">Ogni volta che un client si connette al server, una nuova istanza della classe StockTickerHub in esecuzione in un thread separato Ottiene l'istanza singleton di StockTicker dal `StockTicker.Instance` proprietà statica, come illustrato in precedenza nel `StockTickerHub` classe.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="4cd0d-189">Archiviazione dei dati predefinite in un oggetto ConcurrentDictionary</span><span class="sxs-lookup"><span data-stu-id="4cd0d-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="4cd0d-190">Il costruttore inizializza la `_stocks` raccolta con alcuni dati di esempio azionario, e `GetAllStocks` restituisce i titoli di borsa.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="4cd0d-191">Come illustrato in precedenza, questa raccolta di titoli di borsa viene restituita da `StockTickerHub.GetAllStocks`, che è un metodo server il `Hub` classe che i client possono chiamare.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="4cd0d-192">La raccolta di titoli azionari è definita come un [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) tipo per la sicurezza dei thread.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="4cd0d-193">In alternativa, è possibile usare una [dizionario](https://msdn.microsoft.com/library/xfhwa508.aspx) dell'oggetto e il dizionario di blocco in modo esplicito quando si apportano modifiche a esso.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="4cd0d-194">Per questa applicazione di esempio, è bene per archiviare i dati dell'applicazione in memoria e i dati andranno persi quando l'app Elimina il `StockTicker` istanza.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="4cd0d-195">In un'applicazione reale, si procederà con un archivio dati back-end, ad esempio un database.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="4cd0d-196">Aggiornamento periodico dei prezzi delle azioni</span><span class="sxs-lookup"><span data-stu-id="4cd0d-196">Periodically updating stock prices</span></span>

<span data-ttu-id="4cd0d-197">Il costruttore viene avviato un `Timer` oggetto periodicamente chiamare i metodi che aggiornano quotazioni di titoli in modo casuale.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="4cd0d-198">`Timer` le chiamate `UpdateStockPrices`, che passa valore null nel parametro state.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="4cd0d-199">Prima di aggiornare i prezzi, l'app acquisisce un blocco per il `_updateStockPricesLock` oggetto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="4cd0d-200">Il codice controlla se un altro thread sta già aggiornando i prezzi e quindi chiama `TryUpdateStockPrice` su ciascun titolo nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="4cd0d-201">Il `TryUpdateStockPrice` metodo decide se modifica il prezzo azionario e quella da modificare.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="4cd0d-202">Se viene modificato il prezzo delle azioni, l'app chiama `BroadcastStockPrice` per trasmettere la modifica del prezzo azionario a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="4cd0d-203">Il `_updatingStockPrices` flag designata [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) per assicurarsi che sia thread-safe.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="4cd0d-204">In un'applicazione reale, la `TryUpdateStockPrice` metodo chiamare un servizio web per cercare il prezzo.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="4cd0d-205">In questo codice, l'app Usa un generatore di numeri casuali per apportare modifiche in modo casuale.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="4cd0d-206">Recupero del contesto di SignalR in modo che la classe StockTicker può trasmettere ai client</span><span class="sxs-lookup"><span data-stu-id="4cd0d-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="4cd0d-207">Perché le variazioni di prezzo derivano qui nel `StockTicker` dell'oggetto, è l'oggetto che deve chiamare un' `updateStockPrice` metodo su tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="4cd0d-208">In un `Hub` (classe), è disponibile un'API per la chiamata di metodi client, ma `StockTicker` non deriva dalle `Hub` classe e non contiene un riferimento a qualsiasi `Hub` oggetto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="4cd0d-209">Per trasmettere ai client connessi, il `StockTicker` classe deve ottenere l'istanza del contesto SignalR per le `StockTickerHub` classe e usarlo per chiamare metodi sul client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="4cd0d-210">Il codice ottiene un riferimento al contesto di SignalR quando crea l'istanza della classe singleton, che fanno riferimento a passate al costruttore, e viene inserita nel costruttore di `Clients` proprietà.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="4cd0d-211">Esistono due motivi per cui si desidera ottenere il contesto di una sola volta: recupero del contesto è un'attività costosa e messa in funzione dopo che assicura che l'app mantiene l'ordine previsto dei messaggi inviati ai client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="4cd0d-212">Introduzione di `Clients` proprietà di contesto e il relativo inserimento `StockTickerClient` proprietà consente di scrivere codice per chiamare i metodi di client che ha lo stesso aspetto come avviene in un `Hub` classe.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="4cd0d-213">Ad esempio, per trasmettere a tutti i client, è possibile scrivere `Clients.All.updateStockPrice(stock)`.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="4cd0d-214">Il `updateStockPrice` metodo che si chiamano in `BroadcastStockPrice` non esiste già.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="4cd0d-215">Tale servizio verrà aggiunto in un secondo momento quando si scrive codice che viene eseguito sul client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="4cd0d-216">È possibile fare riferimento a `updateStockPrice` qui perché `Clients.All` è dinamico, ovvero l'app verrà valutata l'espressione in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="4cd0d-217">Quando viene eseguita questa chiamata al metodo, SignalR invierà il nome del metodo e il valore del parametro al client, e se il client dispone di un metodo denominato `updateStockPrice`, l'app verrà chiamare il metodo e passare il valore del parametro.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="4cd0d-218">`Clients.All` mezzi di trasmissione a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="4cd0d-219">SignalR offre altre opzioni per specificare quale client o i gruppi di client da inviare.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="4cd0d-220">Per altre informazioni, vedere [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="4cd0d-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="4cd0d-221">Registrare la route di SignalR</span><span class="sxs-lookup"><span data-stu-id="4cd0d-221">Register the SignalR route</span></span>

<span data-ttu-id="4cd0d-222">Il server deve conoscere l'URL da intercettare e indirizzare a SignalR.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="4cd0d-223">A tale scopo, aggiungere una classe di avvio OWIN:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="4cd0d-224">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="4cd0d-225">Nelle **Aggiungi nuovo elemento - SignalR.StockTicker** selezionate **installati** > **Visual C#**   >  **Web** e quindi selezionare **classe di avvio OWIN**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="4cd0d-226">Denominare la classe *avvio* e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="4cd0d-227">Sostituire il codice predefinito nel *Startup.cs* file con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="4cd0d-228">Ora completato la configurazione il codice del server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="4cd0d-229">Nella sezione successiva, è possibile configurare il client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="4cd0d-230">Impostare il codice client</span><span class="sxs-lookup"><span data-stu-id="4cd0d-230">Set up the client code</span></span>

<span data-ttu-id="4cd0d-231">In questa sezione è impostare il codice eseguito sul client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="4cd0d-232">Creare la pagina HTML e file JavaScript</span><span class="sxs-lookup"><span data-stu-id="4cd0d-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="4cd0d-233">La pagina HTML verrà visualizzati i dati e i dati verrà organizzati i file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="4cd0d-234">Creare StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="4cd0d-234">Create StockTicker.html</span></span>

<span data-ttu-id="4cd0d-235">In primo luogo, si aggiungerà il client HTML.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="4cd0d-236">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **pagina HTML**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="4cd0d-237">Denominare il file *StockTicker* e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="4cd0d-238">Sostituire il codice predefinito nel *StockTicker.html* file con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="4cd0d-239">Il codice HTML crea una tabella con cinque colonne, una riga di intestazione e una riga di dati con una singola cella che si estende su tutte le cinque colonne.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="4cd0d-240">La riga dati Mostra "caricamento in corso" momentaneamente all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="4cd0d-241">Il codice JavaScript verrà rimuovere tale riga e aggiungere nelle righe Dell sul posto con dati azionari recuperati dal server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="4cd0d-242">Specificano i tag di script:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-242">The script tags specify:</span></span>

    * <span data-ttu-id="4cd0d-243">Il file di script jQuery.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-243">The jQuery script file.</span></span>

    * <span data-ttu-id="4cd0d-244">Il file di script principale di SignalR.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="4cd0d-245">Il file di script proxy SignalR.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="4cd0d-246">Un file di script StockTicker che verrà creata in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="4cd0d-247">L'app genera in modo dinamico il file di script proxy SignalR.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="4cd0d-248">Specifica l'URL "hub signalr /" e definisce i metodi di proxy per i metodi sulla classe Hub, in questo caso, per `StockTickerHub.GetAllStocks`.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="4cd0d-249">Se si preferisce, è possibile generare manualmente questo file JavaScript usando [SignalR utilità](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="4cd0d-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="4cd0d-250">Non dimenticare di disabilitare la creazione di file dinamici nel `MapHubs` chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="4cd0d-251">Nelle **Esplora soluzioni**, espandere **script**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="4cd0d-252">Librerie di script per jQuery e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="4cd0d-253">Package manager installerà una versione successiva degli script di SignalR.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="4cd0d-254">Aggiornare i riferimenti di script nel blocco di codice in modo che corrispondano alle versioni dei file di script nel progetto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="4cd0d-255">Nelle **Esplora soluzioni**, fare doppio clic su *StockTicker.html*, quindi selezionare **imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="4cd0d-256">Creare StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="4cd0d-256">Create StockTicker.js</span></span>

<span data-ttu-id="4cd0d-257">A questo punto creare il file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="4cd0d-258">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **JavaScript File**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="4cd0d-259">Denominare il file *StockTicker* e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="4cd0d-260">Aggiungere questo codice per il *StockTicker.js* file:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="4cd0d-261">Esaminare il codice client</span><span class="sxs-lookup"><span data-stu-id="4cd0d-261">Examine the client code</span></span>

<span data-ttu-id="4cd0d-262">Se si esamina il codice client, consente di informazioni su come il codice client interagisce con il codice del server per il funzionamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="4cd0d-263">Avvio della connessione</span><span class="sxs-lookup"><span data-stu-id="4cd0d-263">Starting the connection</span></span>

<span data-ttu-id="4cd0d-264">`$.connection` fa riferimento al proxy di SignalR.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="4cd0d-265">Il codice ottiene un riferimento al proxy per il `StockTickerHub` classe e lo inserisce nella `ticker` variabile.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="4cd0d-266">Il nome del proxy è il nome che è stato impostato dal `HubName` attributo:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="4cd0d-267">Dopo aver definito tutte le funzioni e variabili, l'ultima riga di codice nel file Inizializza la connessione SignalR chiamando SignalR `start` (funzione).</span><span class="sxs-lookup"><span data-stu-id="4cd0d-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="4cd0d-268">Il `start` funzione viene eseguita in modo asincrono e restituisce un [jQuery Deferred oggetto](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="4cd0d-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="4cd0d-269">È possibile chiamare la funzione eseguita per specificare la funzione da chiamare dopo il completamento dell'azione asincrona.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="4cd0d-270">Recupero di tutti i titoli di borsa</span><span class="sxs-lookup"><span data-stu-id="4cd0d-270">Getting all the stocks</span></span>

<span data-ttu-id="4cd0d-271">Il `init` chiamate di funzione di `getAllStocks` funzione sul server e utilizza le informazioni che il server restituisce per aggiornare la tabella predefinita.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="4cd0d-272">Si noti che, per impostazione predefinita, è necessario utilizzare la notazione camel nel client anche se il nome del metodo è la convenzione pascal maiuscole/minuscole nel server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="4cd0d-273">La regola di sistema camelCasing si applica solo ai metodi, non oggetti.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="4cd0d-274">Ad esempio, intende `stock.Symbol` e `stock.Price`, non `stock.symbol` o `stock.price`.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="4cd0d-275">Nel `init` metodo, l'app crea codice HTML per una riga della tabella per ogni oggetto dei titoli ricevuto dal server tramite una chiamata `formatStock` alle proprietà del formato delle `stock` dell'oggetto e quindi chiamando `supplant` per sostituire i segnaposto tra il `rowTemplate` variabile con il `stock` i valori delle proprietà dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="4cd0d-276">Il codice HTML risultante viene quindi aggiunto alla tabella dei titolo.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="4cd0d-277">Si chiama `init` passandola come un `callback` funzione che verrà eseguito dopo asincrona `start` al termine della funzione.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="4cd0d-278">Se è stato chiamato `init` come istruzione JavaScript separata dopo avere chiamato `start`, la funzione avrà esito negativo perché viene eseguita immediatamente senza attendere che la funzione di avvio alla fine di stabilire una connessione.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="4cd0d-279">In tal caso, il `init` funzione cercherebbe di chiamare il `getAllStocks` funzionare prima che l'app stabilisce una connessione al server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="4cd0d-280">Recupero di quotazioni di titoli aggiornati</span><span class="sxs-lookup"><span data-stu-id="4cd0d-280">Getting updated stock prices</span></span>

<span data-ttu-id="4cd0d-281">Quando il server viene modificato il prezzo del titolo, chiama il `updateStockPrice` sui client connessi.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="4cd0d-282">L'app aggiunge la funzione per la proprietà client del `stockTicker` proxy per renderlo disponibile per le chiamate dal server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="4cd0d-283">Il `updateStockPrice` un oggetto predefinito ricevuto dal server in una tabella di righe esattamente come in formati di funzione di `init` (funzione).</span><span class="sxs-lookup"><span data-stu-id="4cd0d-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="4cd0d-284">Anziché aggiungere la riga nella tabella, trova riga corrente del titolo nella tabella e la sostituisce tale riga con quello nuovo.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="4cd0d-285">Testare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="4cd0d-285">Test the application</span></span>

<span data-ttu-id="4cd0d-286">È possibile testare l'app per assicurarsi che funzioni.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="4cd0d-287">Tutte le finestre del browser verrà visualizzata la tabella di scorte in tempo reale vengono visualizzati con quotazioni di titoli ripristinarne.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="4cd0d-288">Nella barra degli strumenti, attivare **debug degli Script** e quindi selezionare il pulsante play per eseguire l'app in modalità di Debug.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Screenshot di utente attivando la modalità di debug e selezionare Riproduci.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="4cd0d-290">Una finestra del browser verrà aperta la visualizzazione di **Live tabella Stock**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="4cd0d-291">La tabella azionario Mostra inizialmente la riga "caricamento in corso", quindi, dopo un breve periodo di tempo, l'app Mostra i primi dati azionari, e iniziare a modificare i prezzi delle azioni.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="4cd0d-292">Copiare l'URL dal browser, aprire due altri browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="4cd0d-293">La visualizzazione predefinita iniziale è quello utilizzato per il primo browser e le modifiche vengono eseguite contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="4cd0d-294">Chiudere tutti i browser, aprire un nuovo browser e passare allo stesso URL.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="4cd0d-295">L'oggetto singleton StockTicker continuato ad essere eseguiti nel server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="4cd0d-296">Il **Live tabella Stock** mostra che i titoli di borsa hanno continuato a cambiare.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="4cd0d-297">Non verranno visualizzati nella tabella con zero inizio modificare cifre.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="4cd0d-298">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="4cd0d-299">Abilitare la registrazione</span><span class="sxs-lookup"><span data-stu-id="4cd0d-299">Enable logging</span></span>

<span data-ttu-id="4cd0d-300">SignalR è una funzione di registrazione predefiniti che è possibile abilitare nel client per semplificare la risoluzione dei problemi.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="4cd0d-301">In questa sezione abilitare la registrazione e vedere gli esempi che illustrano come log indicano quale dei seguenti metodi di trasporto utilizza SignalR:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="4cd0d-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supportata da IIS 8 e i browser correnti.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="4cd0d-303">[Gli eventi inviati al server](http://en.wikipedia.org/wiki/Server-sent_events), supportato dai browser diversi da Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="4cd0d-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supportata da Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="4cd0d-305">[AJAX polling prolungato](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supportata da tutti i browser.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="4cd0d-306">Per una connessione specifica, SignalR sceglie il metodo migliore di trasporto che supportano il client sia nel server.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="4cd0d-307">Aprire *StockTicker.js*.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="4cd0d-308">Aggiungere la riga evidenziata di codice per abilitare la registrazione immediatamente prima del codice che inizializza la connessione alla fine del file:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="4cd0d-309">Premere **F5** per eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="4cd0d-310">Finestra Strumenti di sviluppo del browser di aprire e selezionare la Console per visualizzare i log.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="4cd0d-311">Si potrebbe essere necessario aggiornare la pagina per visualizzare i log di negoziare il metodo di trasporto per una nuova connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="4cd0d-312">Se si esegue Internet Explorer 10 in Windows 8 (IIS 8), il metodo di trasporto consiste **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="4cd0d-313">Se si esegue Internet Explorer 10 in Windows 7 (IIS 7.5), il metodo di trasporto consiste **iframe**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="4cd0d-314">Se si eseguono 19 Firefox in Windows 8 (IIS 8), il metodo di trasporto consiste **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="4cd0d-315">In Firefox, installare il componente aggiuntivo Firebug per ottenere una finestra della Console.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="4cd0d-316">Se si eseguono 19 Firefox in Windows 7 (IIS 7.5), il metodo di trasporto consiste **inviate al server** gli eventi.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="4cd0d-317">Installare il campione StockTicker</span><span class="sxs-lookup"><span data-stu-id="4cd0d-317">Install the StockTicker sample</span></span>

<span data-ttu-id="4cd0d-318">Il [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installa l'applicazione StockTicker.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="4cd0d-319">Il pacchetto NuGet include più funzionalità rispetto alla versione semplificata che è stato creato da zero.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="4cd0d-320">In questa sezione dell'esercitazione, si installa il pacchetto NuGet e rivedere le nuove funzionalità e il codice che li implementa.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4cd0d-321">Se si installa il pacchetto senza eseguire i passaggi precedenti in questa esercitazione, è necessario aggiungere una classe di avvio OWIN al progetto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="4cd0d-322">Questo file Readme. txt per il pacchetto NuGet illustra questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="4cd0d-323">Installare il pacchetto SignalR.Sample NuGet</span><span class="sxs-lookup"><span data-stu-id="4cd0d-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="4cd0d-324">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="4cd0d-325">In **Gestione pacchetti NuGet: SignalR.StockTicker**, selezionare **Sfoglia**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="4cd0d-326">Dal **origine pacchetto**, selezionare **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="4cd0d-327">Immettere *SignalR.Sample* nella casella di ricerca e selezionare **Microsoft.AspNet.SignalR.Sample** > **installare**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="4cd0d-328">Nelle **Esplora soluzioni**, espandere il *SignalR.Sample* cartella.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="4cd0d-329">Installazione del pacchetto SignalR.Sample creato nella cartella e il relativo contenuto.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="4cd0d-330">Nel *SignalR.Sample* cartella, fare doppio clic su *StockTicker.html*, quindi selezionare **imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4cd0d-331">Installazione di SignalR.Sample NuGet il pacchetto potrebbe cambiare la versione di jQuery presenti nelle *script* cartella.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="4cd0d-332">Il nuovo *StockTicker.html* che consente di installare il pacchetto nel file la *SignalR.Sample* cartella sarà sincronizzata con la versione di jQuery che installa il pacchetto, ma se si vuole eseguire originale *StockTicker.html* file anche in questo caso, potrebbe essere necessario aggiornare innanzitutto il riferimento di jQuery nel tag di script.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="4cd0d-333">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="4cd0d-333">Run the application</span></span>

 <span data-ttu-id="4cd0d-334">La tabella che si è visto nella prima app aveva funzionalità utili.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="4cd0d-335">L'applicazione di teleborsa complete Mostra le nuove funzionalità: una finestra di scorrimento orizzontale che mostra i dati azionari e azioni che modificano i colori che sono in aumento e rientra.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="4cd0d-336">Premere **F5** per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="4cd0d-337">Quando si esegue l'app per la prima volta, "mercato" è "chiuso" ed è visibile una tabella statica e una finestra del ticker che non viene effettuato uno scorrimento.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="4cd0d-338">Selezionare **Open Market**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-338">Select **Open Market**.</span></span>

    ![Screenshot della finestra di ticker in tempo reale.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="4cd0d-340">Il **Live Stock Ticker** finestra Avvia scorra in orizzontale e il server inizia a trasmettere periodicamente le modifiche di prezzo delle azioni in modo casuale.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="4cd0d-341">Ogni volta che viene modificato un prezzo azionario, l'app aggiornerà di entrambi i **Live tabella Stock** e il **Live Stock Ticker**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="4cd0d-342">Quando la variazione di prezzo del titolo è positiva, l'app Mostra le azioni con uno sfondo verde.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="4cd0d-343">Quando la modifica è negativa, l'app Mostra le azioni con uno sfondo rosso.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="4cd0d-344">Selezionare **chiudere mercato**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="4cd0d-345">La tabella viene aggiornata stop.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-345">The table updates stop.</span></span>

    * <span data-ttu-id="4cd0d-346">Il ticker Arresta lo scorrimento.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="4cd0d-347">Selezionare **reimpostare**.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-347">Select **Reset**.</span></span>

    * <span data-ttu-id="4cd0d-348">Tutti i dati azionari viene reimpostato.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-348">All stock data is reset.</span></span>

    * <span data-ttu-id="4cd0d-349">L'app consente di ripristinare lo stato iniziale prima di variazioni di prezzo avviato.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="4cd0d-350">Copiare l'URL dal browser, aprire due altri browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="4cd0d-351">Si vedranno gli stessi dati aggiornati in modo dinamico al momento stesso in ogni browser.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="4cd0d-352">Quando si seleziona uno dei controlli, tutti i browser rispondono esattamente nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="4cd0d-353">Visualizzazione Stock Ticker in tempo reale</span><span class="sxs-lookup"><span data-stu-id="4cd0d-353">Live Stock Ticker display</span></span>

<span data-ttu-id="4cd0d-354">Il **Live Stock Ticker** visualizzato è un elenco non ordinato in un `<div>` elemento formattato in una singola riga dagli stili CSS.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="4cd0d-355">L'app consente di inizializzare e aggiorna il ticker di nello stesso modo della tabella: sostituendo i segnaposto in una `<li>` stringa di modello e in modo dinamico aggiungendo il `<li>` elementi dal `<ul>` elemento.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="4cd0d-356">L'app include lo scorrimento tramite il componente aggiuntivo jQuery `animate` funzione per variare il margine a sinistra dell'elenco non ordinato all'interno di `<div>`.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="4cd0d-357">SignalR.Sample StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="4cd0d-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="4cd0d-358">Le quotazioni di borsa codice HTML:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="4cd0d-359">SignalR.Sample StockTicker.css</span><span class="sxs-lookup"><span data-stu-id="4cd0d-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="4cd0d-360">Le quotazioni di borsa codice CSS:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="4cd0d-361">SignalR.Sample SignalR.StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="4cd0d-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="4cd0d-362">Scorrere verso il codice jQuery che consente di:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="4cd0d-363">Metodi aggiuntivi nel server in cui il client può chiamare</span><span class="sxs-lookup"><span data-stu-id="4cd0d-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="4cd0d-364">Per aggiungere la flessibilità per l'app, sono disponibili metodi aggiuntivi che può chiamare l'app.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="4cd0d-365">SignalR.Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="4cd0d-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="4cd0d-366">Il `StockTickerHub` classe definisce quattro metodi aggiuntivi che il client può chiamare:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="4cd0d-367">L'app chiama `OpenMarket`, `CloseMarket`, e `Reset` in risposta ai pulsanti nella parte superiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="4cd0d-368">Illustrano il motivo di un client attiva un cambiamento di stato immediatamente propagato a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="4cd0d-369">Ognuno di questi metodi chiama un metodo `StockTicker` classe che comporta la modifica dello stato di immissione sul mercato e quindi trasmette il nuovo stato.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="4cd0d-370">SignalR.Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="4cd0d-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="4cd0d-371">Nel `StockTicker` (classe), l'app mantiene lo stato del mercato con un `MarketState` proprietà che restituisce un `MarketState` valore enum:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="4cd0d-372">Ciascuno dei metodi che modificano lo stato di immissione sul mercato eseguire questa operazione all'interno di un blocco di blocco perché il `StockTicker` classe deve essere thread-safe:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="4cd0d-373">Per assicurarsi che questo codice è thread-safe, il `_marketState` campo sottostante la `MarketState` proprietà designata `volatile`:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="4cd0d-374">Il `BroadcastMarketStateChange` e `BroadcastMarketReset` metodi sono simili al metodo BroadcastStockPrice che hai già visto, ad eccezione del fatto che chiamano metodi diversi definiti nel client:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="4cd0d-375">Funzioni aggiuntive sul client che il server può chiamare</span><span class="sxs-lookup"><span data-stu-id="4cd0d-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="4cd0d-376">Il `updateStockPrice` funzione ora gestisce la tabella e la visualizzazione del ticker e Usa `jQuery.Color` per un attimo i colori rossi e verdi.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="4cd0d-377">Nuove funzioni nel *SignalR.StockTicker.js* abilitare e disabilitare i pulsanti in base allo stato di immissione sul mercato.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="4cd0d-378">Sono anche arrestare o avviare il **Live Stock Ticker** lo scorrimento orizzontale.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="4cd0d-379">Poiché molte funzioni vengono aggiunti al `ticker.client`, l'app Usa la [jQuery estendere funzione](http://api.jquery.com/jQuery.extend/) per aggiungerli.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="4cd0d-380">Programma di installazione client aggiuntivi dopo aver stabilito la connessione</span><span class="sxs-lookup"><span data-stu-id="4cd0d-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="4cd0d-381">Dopo che il client stabilisce la connessione, dispone di alcune operazioni aggiuntive da eseguire:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="4cd0d-382">Verificare se il mercato è aperto o chiuso per chiamare l'appropriato `marketOpened` o `marketClosed` (funzione).</span><span class="sxs-lookup"><span data-stu-id="4cd0d-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="4cd0d-383">Collegare le chiamate al metodo server per i pulsanti.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="4cd0d-384">I metodi di server non sono collegati i pulsanti fino a dopo che l'app stabilisce la connessione.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="4cd0d-385">Si tratta pertanto il codice non è possibile chiamare i metodi di server vengono rese disponibili.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4cd0d-386">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4cd0d-386">Additional resources</span></span>

<span data-ttu-id="4cd0d-387">In questa esercitazione si è appreso come programmare un'applicazione di SignalR che trasmette i messaggi dal server a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="4cd0d-388">A questo punto è possibile trasmettere i messaggi su base periodica e in risposta a notifiche da qualsiasi client.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="4cd0d-389">È possibile usare il concetto di istanza singleton multithread per mantenere lo stato di server in scenari di giochi online multiplayer.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="4cd0d-390">Per un esempio, vedere [gioco ShootR basato su SignalR](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="4cd0d-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="4cd0d-391">Per esercitazioni che illustrano scenari di comunicazione peer-to-peer, vedere [Introduzione a SignalR](introduction-to-signalr.md) e [l'aggiornamento in tempo reale con SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="4cd0d-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="4cd0d-392">Per altre informazioni su SignalR, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="4cd0d-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="4cd0d-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="4cd0d-394">Progetto di SignalR</span><span class="sxs-lookup"><span data-stu-id="4cd0d-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="4cd0d-395">SignalR GitHub ed esempi</span><span class="sxs-lookup"><span data-stu-id="4cd0d-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="4cd0d-396">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="4cd0d-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="4cd0d-397">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="4cd0d-397">Next steps</span></span>

<span data-ttu-id="4cd0d-398">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="4cd0d-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4cd0d-399">Creazione del progetto</span><span class="sxs-lookup"><span data-stu-id="4cd0d-399">Created the project</span></span>
> * <span data-ttu-id="4cd0d-400">Impostare il codice server</span><span class="sxs-lookup"><span data-stu-id="4cd0d-400">Set up the server code</span></span>
> * <span data-ttu-id="4cd0d-401">Esaminare il codice del server</span><span class="sxs-lookup"><span data-stu-id="4cd0d-401">Examined the server code</span></span>
> * <span data-ttu-id="4cd0d-402">Impostare il codice client</span><span class="sxs-lookup"><span data-stu-id="4cd0d-402">Set up the client code</span></span>
> * <span data-ttu-id="4cd0d-403">Esaminare il codice client</span><span class="sxs-lookup"><span data-stu-id="4cd0d-403">Examined the client code</span></span>
> * <span data-ttu-id="4cd0d-404">Testare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="4cd0d-404">Tested the application</span></span>
> * <span data-ttu-id="4cd0d-405">Registrazione abilitata</span><span class="sxs-lookup"><span data-stu-id="4cd0d-405">Enabled logging</span></span>

<span data-ttu-id="4cd0d-406">Passare all'articolo successivo per informazioni su come creare un'applicazione web in tempo reale che usa ASP.NET SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="4cd0d-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="4cd0d-407">Creare app web in tempo reale con SignalR</span><span class="sxs-lookup"><span data-stu-id="4cd0d-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)