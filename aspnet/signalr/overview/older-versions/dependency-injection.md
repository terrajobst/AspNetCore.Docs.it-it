---
uid: signalr/overview/older-versions/dependency-injection
title: Inserimento delle dipendenze in SignalR 1.x | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: ff96b4958652dac5109648974770abf3a9b6ae0c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802396"
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="de10c-102">Inserimento delle dipendenze in SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="de10c-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="de10c-103">dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="de10c-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="de10c-104">Inserimento di dipendenze è un modo per rimuovere le dipendenze hardcoded tra gli oggetti, rendendo più semplice per sostituire le dipendenze di un oggetto, uno per i test (tramite oggetti fittizi) o per modificare il comportamento in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="de10c-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="de10c-105">Questa esercitazione illustra come eseguire l'inserimento di dipendenze sugli hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="de10c-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="de10c-106">Viene inoltre illustrato come usare contenitori IoC con SignalR.</span><span class="sxs-lookup"><span data-stu-id="de10c-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="de10c-107">Un contenitore IoC è un framework generale per l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="de10c-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="de10c-108">Che cos'è l'inserimento delle dipendenze?</span><span class="sxs-lookup"><span data-stu-id="de10c-108">What is Dependency Injection?</span></span>

<span data-ttu-id="de10c-109">Se si ha già familiarità con inserimento delle dipendenze, ignorare questa sezione.</span><span class="sxs-lookup"><span data-stu-id="de10c-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="de10c-110">*Inserimento di dipendenze* (inserimento delle dipendenze) è un modello in cui gli oggetti non sono responsabili della creazione le proprie dipendenze.</span><span class="sxs-lookup"><span data-stu-id="de10c-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="de10c-111">Ecco un esempio semplice per motivare l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="de10c-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="de10c-112">Si supponga di che avere un oggetto che è necessario registrare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="de10c-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="de10c-113">È possibile definire un'interfaccia di registrazione:</span><span class="sxs-lookup"><span data-stu-id="de10c-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="de10c-114">L'oggetto, è possibile creare un `ILogger` per registrare i messaggi:</span><span class="sxs-lookup"><span data-stu-id="de10c-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="de10c-115">Questo procedimento funziona, ma non è la migliore struttura.</span><span class="sxs-lookup"><span data-stu-id="de10c-115">This works, but it's not the best design.</span></span> <span data-ttu-id="de10c-116">Se si desidera sostituire `FileLogger` con un'altra `ILogger` implementazione, sarà necessario modificare `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="de10c-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="de10c-117">Presumere che molti degli altri oggetti usare `FileLogger`, sarà necessario modificare tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="de10c-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="de10c-118">O se si decide di apportare `FileLogger` un singleton, è inoltre necessario apportare modifiche in tutta l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="de10c-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="de10c-119">Un approccio migliore consiste nel "inserire" un `ILogger` nell'oggetto, ad esempio, usando un argomento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="de10c-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="de10c-120">A questo punto l'oggetto non è responsabile della selezione che `ILogger` da usare.</span><span class="sxs-lookup"><span data-stu-id="de10c-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="de10c-121">È possibile swich `ILogger` implementazioni senza modificare gli oggetti che dipendono da esso.</span><span class="sxs-lookup"><span data-stu-id="de10c-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="de10c-122">Questo modello viene denominato [inserimento del costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="de10c-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="de10c-123">Un altro modello è l'inserimento di setter, in cui è impostare la dipendenza attraverso un metodo di impostazione o una proprietà.</span><span class="sxs-lookup"><span data-stu-id="de10c-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="de10c-124">Inserimento di dipendenze semplice in SignalR</span><span class="sxs-lookup"><span data-stu-id="de10c-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="de10c-125">Prendere in considerazione l'applicazione di Chat nell'esercitazione [Introduzione a SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="de10c-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="de10c-126">Questa è la classe dell'hub da quell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="de10c-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="de10c-127">Si supponga che si desidera archiviare i messaggi di chat nel server prima di inviarli.</span><span class="sxs-lookup"><span data-stu-id="de10c-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="de10c-128">Si potrebbe definire un'interfaccia che consente di astrarre questa funzionalità e usare l'inserimento delle dipendenze per inserire l'interfaccia nel `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="de10c-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="de10c-129">L'unico problema è che un'applicazione di SignalR non crea direttamente hub; SignalR verranno creati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="de10c-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="de10c-130">Per impostazione predefinita, SignalR si aspetta che una classe di hub per avere un costruttore senza parametri.</span><span class="sxs-lookup"><span data-stu-id="de10c-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="de10c-131">Tuttavia, è possibile facilmente registrare una funzione per creare istanze di hub e usare questa funzione per eseguire l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="de10c-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="de10c-132">Registrare la funzione chiamando **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="de10c-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="de10c-133">A questo punto SignalR richiama questa funzione anonima ogni volta che è necessario crearne un `ChatHub` istanza.</span><span class="sxs-lookup"><span data-stu-id="de10c-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="de10c-134">Contenitori IoC</span><span class="sxs-lookup"><span data-stu-id="de10c-134">IoC Containers</span></span>

<span data-ttu-id="de10c-135">Il codice precedente è appropriato per i casi semplici.</span><span class="sxs-lookup"><span data-stu-id="de10c-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="de10c-136">Ma era ancora necessario scrivere questo codice:</span><span class="sxs-lookup"><span data-stu-id="de10c-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="de10c-137">In un'applicazione complessa con numerose dipendenze, si potrebbe essere necessario scrivere una grande quantità di questo codice di "collegamento".</span><span class="sxs-lookup"><span data-stu-id="de10c-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="de10c-138">Questo codice può essere difficile da gestire, soprattutto se le dipendenze sono annidate.</span><span class="sxs-lookup"><span data-stu-id="de10c-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="de10c-139">È anche difficile lo unit test.</span><span class="sxs-lookup"><span data-stu-id="de10c-139">It is also hard to unit test.</span></span>

<span data-ttu-id="de10c-140">Un'unica soluzione consiste nell'usare un contenitore IoC.</span><span class="sxs-lookup"><span data-stu-id="de10c-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="de10c-141">Un contenitore IoC è un componente software che è responsabile della gestione delle dipendenze. Registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti.</span><span class="sxs-lookup"><span data-stu-id="de10c-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="de10c-142">Il contenitore determina automaticamente le relazioni di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="de10c-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="de10c-143">Numerosi contenitori IoC consentono anche di controllare elementi quali la durata degli oggetti e l'ambito.</span><span class="sxs-lookup"><span data-stu-id="de10c-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="de10c-144">"IoC" è l'acronimo di "inversione di controllo", ovvero un modello generale in cui un framework chiama il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="de10c-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="de10c-145">Un contenitore IoC costruisce oggetti, quali "inverte" il normale flusso di controllo.</span><span class="sxs-lookup"><span data-stu-id="de10c-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="de10c-146">Uso dei contenitori IoC in SignalR</span><span class="sxs-lookup"><span data-stu-id="de10c-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="de10c-147">L'applicazione di Chat è probabilmente troppo semplice per trarre vantaggio da un contenitore IoC.</span><span class="sxs-lookup"><span data-stu-id="de10c-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="de10c-148">Al contrario, verrà ora esaminato il [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) esempio.</span><span class="sxs-lookup"><span data-stu-id="de10c-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="de10c-149">L'esempio StockTicker definisce due classi principali:</span><span class="sxs-lookup"><span data-stu-id="de10c-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="de10c-150">`StockTickerHub`: La classe dell'hub, che gestisce le connessioni client.</span><span class="sxs-lookup"><span data-stu-id="de10c-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="de10c-151">`StockTicker`: Un singleton che contiene quotazioni di titoli e li aggiorna periodicamente.</span><span class="sxs-lookup"><span data-stu-id="de10c-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="de10c-152">`StockTickerHub` contiene un riferimento al `StockTicker` singleton, mentre `StockTicker` contiene un riferimento al **IHubConnectionContext** per il `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="de10c-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="de10c-153">Usa questa interfaccia per comunicare con `StockTickerHub` istanze.</span><span class="sxs-lookup"><span data-stu-id="de10c-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="de10c-154">(Per altre informazioni, vedere [trasmissione Server con ASP.NET SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="de10c-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="de10c-155">È possibile usare un contenitore IoC interfoliati un po' queste dipendenze.</span><span class="sxs-lookup"><span data-stu-id="de10c-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="de10c-156">In primo luogo, è possibile semplificare la `StockTickerHub` e `StockTicker` classi.</span><span class="sxs-lookup"><span data-stu-id="de10c-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="de10c-157">Nel codice seguente, ho ho aggiunto le parti che non è necessario.</span><span class="sxs-lookup"><span data-stu-id="de10c-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="de10c-158">Rimuovere il costruttore senza parametri da `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="de10c-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="de10c-159">In alternativa, sempre si userà l'inserimento delle dipendenze per creare l'hub.</span><span class="sxs-lookup"><span data-stu-id="de10c-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="de10c-160">Per StockTicker, rimuovere l'istanza singleton.</span><span class="sxs-lookup"><span data-stu-id="de10c-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="de10c-161">In un secondo momento, si userà il contenitore IoC per controllare la durata StockTicker.</span><span class="sxs-lookup"><span data-stu-id="de10c-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="de10c-162">Verificare inoltre che il costruttore pubblico.</span><span class="sxs-lookup"><span data-stu-id="de10c-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="de10c-163">Successivamente, è possibile effettuare il refactoring di codice tramite la creazione di un'interfaccia per `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="de10c-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="de10c-164">Si userà questa interfaccia per disaccoppiare le `StockTickerHub` dal `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="de10c-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="de10c-165">Visual Studio rende questo tipo di refactoring semplice.</span><span class="sxs-lookup"><span data-stu-id="de10c-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="de10c-166">Aprire il file StockTicker.cs, fare clic sui `StockTicker` dichiarazione di classe, quindi selezionare **effettuare il refactoring** ... **Estrai interfaccia**.</span><span class="sxs-lookup"><span data-stu-id="de10c-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="de10c-167">Nel **Estrai interfaccia** finestra di dialogo, fare clic su **Seleziona tutto**.</span><span class="sxs-lookup"><span data-stu-id="de10c-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="de10c-168">Lasciare le altre impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="de10c-168">Leave the other defaults.</span></span> <span data-ttu-id="de10c-169">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="de10c-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="de10c-170">Visual Studio crea una nuova interfaccia denominata `IStockTicker`e viene modificato anche `StockTicker` da cui derivare `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="de10c-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="de10c-171">Aprire il file IStockTicker.cs e modificare l'interfaccia **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="de10c-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="de10c-172">Nel `StockTickerHub` classe, modificare le due istanze di `StockTicker` a `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="de10c-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="de10c-173">Creazione di un `IStockTicker` interfaccia non è strettamente necessaria, ma ho preferito dimostrare come l'inserimento delle dipendenze può contribuire a ridurre l'accoppiamento tra componenti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="de10c-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="de10c-174">Aggiungere la libreria Ninject</span><span class="sxs-lookup"><span data-stu-id="de10c-174">Add the Ninject Library</span></span>

<span data-ttu-id="de10c-175">Esistono numerosi contenitori IoC open source per .NET.</span><span class="sxs-lookup"><span data-stu-id="de10c-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="de10c-176">Per questa esercitazione userà [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="de10c-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="de10c-177">(Includono altre librerie molto diffuse [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), e [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="de10c-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="de10c-178">Usare Gestione pacchetti NuGet per installare il [Ninject libreria](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="de10c-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="de10c-179">In Visual Studio dal **strumenti** dal menu **Library Package Manager** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="de10c-179">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="de10c-180">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="de10c-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="de10c-181">Sostituire il Resolver di dipendenza di SignalR</span><span class="sxs-lookup"><span data-stu-id="de10c-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="de10c-182">Per usare Ninject all'interno di SignalR, creare una classe che deriva da **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="de10c-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="de10c-183">Questa classe esegue l'override di **GetService** e **GetServices** metodi **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="de10c-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="de10c-184">SignalR chiama questi metodi per creare vari oggetti in fase di esecuzione, tra cui le istanze di hub, nonché diversi servizi utilizzati internamente da SignalR.</span><span class="sxs-lookup"><span data-stu-id="de10c-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="de10c-185">Il **GetService** metodo crea un'istanza di un tipo singola.</span><span class="sxs-lookup"><span data-stu-id="de10c-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="de10c-186">Eseguire l'override di questo metodo per chiamare il kernel di Ninject **TryGet** (metodo).</span><span class="sxs-lookup"><span data-stu-id="de10c-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="de10c-187">Se tale metodo restituisce null, eseguire il fallback per il resolver predefinito.</span><span class="sxs-lookup"><span data-stu-id="de10c-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="de10c-188">Il **GetServices** metodo crea una raccolta di oggetti di un tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="de10c-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="de10c-189">Eseguire l'override di questo metodo per concatenare i risultati da Ninject con i risultati dal sistema di risoluzione predefinito.</span><span class="sxs-lookup"><span data-stu-id="de10c-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="de10c-190">Configurare i binding Ninject</span><span class="sxs-lookup"><span data-stu-id="de10c-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="de10c-191">A questo punto si userà Ninject per dichiarare associazioni di tipi.</span><span class="sxs-lookup"><span data-stu-id="de10c-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="de10c-192">Aprire il file RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="de10c-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="de10c-193">Nel `RegisterHubs.Start` metodo, creare il contenitore Ninject, che chiama Ninject il *kernel*.</span><span class="sxs-lookup"><span data-stu-id="de10c-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="de10c-194">Creare un'istanza del nostro resolver di dipendenza personalizzata:</span><span class="sxs-lookup"><span data-stu-id="de10c-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="de10c-195">Creare un'associazione per `IStockTicker` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="de10c-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="de10c-196">Questo codice indica che due cose.</span><span class="sxs-lookup"><span data-stu-id="de10c-196">This code is saying two things.</span></span> <span data-ttu-id="de10c-197">Innanzitutto, ogni volta che l'applicazione necessita di un `IStockTicker`, il kernel deve creare un'istanza di `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="de10c-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="de10c-198">Secondo, il `StockTicker` classe deve essere un oggetto creato come un oggetto singleton.</span><span class="sxs-lookup"><span data-stu-id="de10c-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="de10c-199">Ninject verranno creare un'istanza dell'oggetto e restituire la stessa istanza per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="de10c-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="de10c-200">Creare un'associazione per **IHubConnectionContext** come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="de10c-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="de10c-201">Questo codice creatres una funzione anonima che restituisce un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="de10c-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="de10c-202">Il **WhenInjectedInto** metodo indica a Ninject usare questa funzione solo quando si crea `IStockTicker` istanze.</span><span class="sxs-lookup"><span data-stu-id="de10c-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="de10c-203">Il motivo è che consente di creare SignalR **IHubConnectionContext** istanze internamente e non si desidera eseguire l'override come SignalR li crea.</span><span class="sxs-lookup"><span data-stu-id="de10c-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="de10c-204">Questa funzione si applica solo ai nostri `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="de10c-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="de10c-205">Passare il resolver di dipendenza nel **MapHubs** metodo:</span><span class="sxs-lookup"><span data-stu-id="de10c-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="de10c-206">A questo punto SignalR utilizzerà il sistema di risoluzione specificato nel **MapHubs**, anziché il resolver predefinito.</span><span class="sxs-lookup"><span data-stu-id="de10c-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="de10c-207">Di seguito è riportato per il codice completo `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="de10c-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="de10c-208">Per eseguire l'applicazione StockTicker in Visual Studio, premere F5.</span><span class="sxs-lookup"><span data-stu-id="de10c-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="de10c-209">Nella finestra del browser, passare a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="de10c-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="de10c-210">L'applicazione ha esattamente la stessa funzionalità come prima.</span><span class="sxs-lookup"><span data-stu-id="de10c-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="de10c-211">(Per una descrizione, vedere [trasmissione Server con ASP.NET SignalR](index.md).) È ancora stato modificato il comportamento. semplicemente più semplice da testare, gestire e sviluppare il codice.</span><span class="sxs-lookup"><span data-stu-id="de10c-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
