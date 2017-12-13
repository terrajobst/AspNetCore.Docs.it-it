---
uid: signalr/overview/older-versions/dependency-injection
title: Inserimento di dipendenze in SignalR 1. x | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/15/2013
ms.topic: article
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 6fd155adc9a0aa71b66db7a51062a51fb0c1feca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="b0fb3-102">Inserimento di dipendenze in SignalR 1. x</span><span class="sxs-lookup"><span data-stu-id="b0fb3-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="b0fb3-103">da [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b0fb3-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="b0fb3-104">Inserimento di dipendenze è un modo per rimuovere le dipendenze hardcoded tra gli oggetti, rendendo più semplice per sostituire le dipendenze di un oggetto, uno per i test (tramite oggetti fittizi) o per modificare il comportamento in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="b0fb3-105">In questa esercitazione viene illustrato come eseguire l'inserimento di dipendenze su hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="b0fb3-106">Viene inoltre illustrato come utilizzare i contenitori IoC con SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="b0fb3-107">Un contenitore di inversione di controllo è un framework generale per l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="b0fb3-108">Che cos'è l'inserimento di dipendenze?</span><span class="sxs-lookup"><span data-stu-id="b0fb3-108">What is Dependency Injection?</span></span>

<span data-ttu-id="b0fb3-109">Se si ha già familiarità con l'inserimento di dipendenze, ignorare questa sezione.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="b0fb3-110">*Inserimento di dipendenze* (DI) è un modello in cui gli oggetti non sono responsabili della creazione le proprie dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="b0fb3-111">Di seguito è riportato un esempio semplice di motivare DI.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="b0fb3-112">Si supponga di che avere un oggetto che è necessario registrare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="b0fb3-113">È possibile definire un'interfaccia di registrazione:</span><span class="sxs-lookup"><span data-stu-id="b0fb3-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="b0fb3-114">L'oggetto, è possibile creare un `ILogger` per registrare i messaggi:</span><span class="sxs-lookup"><span data-stu-id="b0fb3-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="b0fb3-115">Questo procedimento funziona, ma non è l'approccio migliore.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-115">This works, but it's not the best design.</span></span> <span data-ttu-id="b0fb3-116">Se si desidera sostituire `FileLogger` con un altro `ILogger` implementazione, sarà necessario modificare `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="b0fb3-117">Presupponendo che molti altri oggetti di uso `FileLogger`, sarà necessario modificare tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="b0fb3-118">O se si decide di creare `FileLogger` un singleton, è inoltre necessario apportare modifiche in tutta l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="b0fb3-119">Un approccio migliore consiste nel "inserire" un `ILogger` nell'oggetto, ad esempio, usando un argomento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="b0fb3-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="b0fb3-120">Dopo l'oggetto non è responsabile della selezione che `ILogger` da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="b0fb3-121">È possibile swich `ILogger` implementazioni senza modificare gli oggetti che dipendono da esso.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="b0fb3-122">Questo modello viene denominato [inserimento costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="b0fb3-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="b0fb3-123">Un altro modello è inserimento di setter, in cui impostare la dipendenza tramite una proprietà o metodo setter.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="b0fb3-124">Inserimento di dipendenze semplice in SignalR</span><span class="sxs-lookup"><span data-stu-id="b0fb3-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="b0fb3-125">Si consideri l'applicazione di Chat dall'esercitazione [Introduzione a SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="b0fb3-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="b0fb3-126">Di seguito è la classe dell'hub da quell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="b0fb3-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="b0fb3-127">Si supponga che si desidera archiviare i messaggi di chat nel server prima di inviarli.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="b0fb3-128">Si potrebbe definire un'interfaccia che astrae questa funzionalità e usare DI per inserire l'interfaccia nella `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="b0fb3-129">Il solo problema è che un'applicazione di SignalR non crea direttamente hub; SignalR verranno creati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="b0fb3-130">Per impostazione predefinita, SignalR presuppone una classe di hub per avere un costruttore senza parametri.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="b0fb3-131">Tuttavia, si può facilmente registrare una funzione per creare istanze di hub e utilizzare questa funzione per eseguire DI.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="b0fb3-132">Registrare la funzione chiamando **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="b0fb3-133">Ora SignalR richiamerà questa funzione anonima ogni volta che è necessario creare un `ChatHub` istanza.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="b0fb3-134">Contenitori IoC</span><span class="sxs-lookup"><span data-stu-id="b0fb3-134">IoC Containers</span></span>

<span data-ttu-id="b0fb3-135">Il codice precedente è appropriato per i casi più semplici.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="b0fb3-136">Ma se è ancora necessario scrivere questo:</span><span class="sxs-lookup"><span data-stu-id="b0fb3-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="b0fb3-137">In un'applicazione complessa con numerose dipendenze, potrebbe essere necessario scrivere una grande quantità di questo codice di "collegamento".</span><span class="sxs-lookup"><span data-stu-id="b0fb3-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="b0fb3-138">Questo codice può essere difficile da gestire, soprattutto se le dipendenze sono annidate.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="b0fb3-139">È inoltre difficile allo unit test.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-139">It is also hard to unit test.</span></span>

<span data-ttu-id="b0fb3-140">Una soluzione consiste nell'utilizzare un contenitore di inversione di controllo.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="b0fb3-141">Un contenitore di inversione di controllo è un componente software che è responsabile della gestione delle dipendenze. Si registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="b0fb3-142">Le relazioni di dipendenza determina automaticamente il contenitore.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="b0fb3-143">Numero di contenitori IoC consentono inoltre di controllare elementi quali la durata degli oggetti e ambito.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="b0fb3-144">"IoC" è l'acronimo di "inversione di controllo", che è un modello generale in cui un framework chiama il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="b0fb3-145">Un contenitore IoC costruisce oggetti, quali "inverte" il normale flusso di controllo.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="b0fb3-146">Utilizzo di contenitori IoC in SignalR</span><span class="sxs-lookup"><span data-stu-id="b0fb3-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="b0fb3-147">L'applicazione di Chat è probabilmente troppo semplice per trarre vantaggio da un contenitore di inversione di controllo.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="b0fb3-148">Al contrario, verrà ora esaminata la [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) esempio.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="b0fb3-149">L'esempio StockTicker definisce due classi principali:</span><span class="sxs-lookup"><span data-stu-id="b0fb3-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="b0fb3-150">`StockTickerHub`: La classe di hub, che gestisce le connessioni client.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="b0fb3-151">`StockTicker`: Un singleton che contiene i prezzi azionari e li aggiorna periodicamente.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="b0fb3-152">`StockTickerHub`contiene un riferimento di `StockTicker` singleton, mentre `StockTicker` contiene un riferimento al **IHubConnectionContext** per il `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="b0fb3-153">Usa questa interfaccia per comunicare con `StockTickerHub` istanze.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="b0fb3-154">(Per ulteriori informazioni, vedere [Server Broadcast con ASP.NET SignalR](index.md).)</span><span class="sxs-lookup"><span data-stu-id="b0fb3-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="b0fb3-155">È possibile usare un contenitore IoC interfoliati un po' queste dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="b0fb3-156">In primo luogo, consente di semplificare il `StockTickerHub` e `StockTicker` classi.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="b0fb3-157">Nel codice seguente, ho impostata come commento le parti che non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="b0fb3-158">Rimuovere il costruttore senza parametri da `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="b0fb3-159">Invece, si utilizzerà sempre DI creare l'hub.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="b0fb3-160">Per StockTicker, rimuovere l'istanza singleton.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="b0fb3-161">In un secondo momento, si userà il contenitore di inversione di controllo per controllare la durata di StockTicker.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="b0fb3-162">Inoltre, rendere il costruttore pubblico.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="b0fb3-163">Successivamente, è possibile effettuare il refactoring di codice tramite la creazione di un'interfaccia per `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="b0fb3-164">Questa interfaccia verrà usato per separare il `StockTickerHub` dalla `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="b0fb3-165">Visual Studio rende questo tipo di refactoring semplice.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="b0fb3-166">Aprire il file StockTicker.cs, fare clic su di `StockTicker` dichiarazione di classe, quindi selezionare **refactoring** ... **Estrai interfaccia**.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="b0fb3-167">Nel **Estrai interfaccia** finestra di dialogo, fare clic su **Seleziona tutto**.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="b0fb3-168">Lasciare le altre impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-168">Leave the other defaults.</span></span> <span data-ttu-id="b0fb3-169">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="b0fb3-170">Visual Studio crea una nuova interfaccia denominata `IStockTicker`e viene modificato anche `StockTicker` da cui derivare `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="b0fb3-171">Aprire il file IStockTicker.cs e modificare l'interfaccia per **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="b0fb3-172">Nel `StockTickerHub` classe, modificare le due istanze di `StockTicker` a `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="b0fb3-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="b0fb3-173">Creazione di un `IStockTicker` interfaccia non è strettamente necessaria, ma si desidera mostrare come SEN consente di ridurre l'accoppiamento tra componenti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="b0fb3-174">Aggiungere la libreria Ninject</span><span class="sxs-lookup"><span data-stu-id="b0fb3-174">Add the Ninject Library</span></span>

<span data-ttu-id="b0fb3-175">Esistono molti contenitori di IoC open source per .NET.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="b0fb3-176">Per questa esercitazione si utilizzerà [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="b0fb3-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="b0fb3-177">(Altre librerie comuni includono [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), e [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="b0fb3-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="b0fb3-178">Usare NuGet Package Manager per installare il [Ninject libreria](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="b0fb3-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="b0fb3-179">In Visual Studio, dal **strumenti** menu selezionare **Gestione pacchetti libreria** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-179">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="b0fb3-180">Nella finestra della Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b0fb3-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="b0fb3-181">Sostituire il Resolver di dipendenza di SignalR</span><span class="sxs-lookup"><span data-stu-id="b0fb3-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="b0fb3-182">Per utilizzare Ninject all'interno di SignalR, creare una classe che deriva da **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="b0fb3-183">Questa classe esegue l'override di **GetService** e **GetServices** metodi di **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="b0fb3-184">SignalR chiama questi metodi per creare vari oggetti in fase di esecuzione, incluse le istanze di hub, nonché diversi servizi utilizzati internamente da SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="b0fb3-185">Il **GetService** metodo crea una singola istanza di un tipo.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="b0fb3-186">Eseguire l'override di questo metodo per chiamare il kernel di Ninject **TryGet** metodo.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="b0fb3-187">Se tale metodo restituisce null, eseguire il fallback per il resolver predefinito.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="b0fb3-188">Il **GetServices** metodo crea una raccolta di oggetti di un tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="b0fb3-189">Eseguire l'override di questo metodo per concatenare i risultati di Ninject e i risultati dal sistema di risoluzione predefinito.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="b0fb3-190">Configurare le associazioni Ninject</span><span class="sxs-lookup"><span data-stu-id="b0fb3-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="b0fb3-191">Ora Ninject verrà utilizzata per dichiarare associazioni del tipo.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="b0fb3-192">Aprire il file RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="b0fb3-193">Nel `RegisterHubs.Start` (metodo), creare il contenitore Ninject, che chiama Ninject il *kernel*.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="b0fb3-194">Creare un'istanza di questo resolver di dipendenza personalizzata:</span><span class="sxs-lookup"><span data-stu-id="b0fb3-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="b0fb3-195">Creare un'associazione per `IStockTicker` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b0fb3-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="b0fb3-196">Questo codice indica che due elementi.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-196">This code is saying two things.</span></span> <span data-ttu-id="b0fb3-197">Primo, ogni volta che l'applicazione richiede un `IStockTicker`, il kernel deve creare un'istanza di `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="b0fb3-198">In secondo luogo, la `StockTicker` classe deve essere creato come un oggetto singleton.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="b0fb3-199">Ninject verrà creata un'istanza dell'oggetto e restituire la stessa istanza per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="b0fb3-200">Creare un'associazione per **IHubConnectionContext** come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="b0fb3-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="b0fb3-201">Questo creatres codice una funzione anonima che restituisce un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="b0fb3-202">Il **WhenInjectedInto** metodo indica Ninject per utilizzare questa funzione solo durante la creazione di `IStockTicker` istanze.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="b0fb3-203">Il motivo è che consente di creare SignalR **IHubConnectionContext** internamente, istanze e non si desidera eseguire l'override come SignalR li crea.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="b0fb3-204">Questa funzione si applica solo ai nostri `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="b0fb3-205">Passare il resolver di dipendenza nel **MapHubs** metodo:</span><span class="sxs-lookup"><span data-stu-id="b0fb3-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="b0fb3-206">Ora SignalR userà il sistema di risoluzione specificato **MapHubs**, anziché il resolver predefinito.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="b0fb3-207">Ecco l'elenco di codice per `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="b0fb3-208">Per eseguire l'applicazione StockTicker in Visual Studio, premere F5.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="b0fb3-209">Nella finestra del browser, passare a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="b0fb3-210">L'applicazione ha esattamente la stessa funzionalità come prima.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="b0fb3-211">(Per una descrizione, vedere [Server Broadcast con ASP.NET SignalR](index.md).) Non è stato modificato il comportamento. appena creati facili da testare, gestire e sviluppare il codice.</span><span class="sxs-lookup"><span data-stu-id="b0fb3-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
