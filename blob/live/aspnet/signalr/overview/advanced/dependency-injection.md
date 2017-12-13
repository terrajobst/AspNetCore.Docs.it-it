---
uid: signalr/overview/advanced/dependency-injection
title: Inserimento di dipendenze in SignalR | Documenti Microsoft
author: MikeWasson
description: Versioni del software utilizzato in questo argomento di Visual Studio 2013 .NET 4.5 SignalR le versioni precedenti di versione 2 di questo argomento per informazioni sulle versioni precedenti di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="73113-103">Inserimento di dipendenze in SignalR</span><span class="sxs-lookup"><span data-stu-id="73113-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="73113-104">da [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="73113-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="73113-105">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="73113-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="73113-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="73113-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="73113-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="73113-107">.NET 4.5</span></span>
> - <span data-ttu-id="73113-108">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="73113-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="73113-109">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="73113-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="73113-110">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="73113-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="73113-111">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="73113-111">Questions and comments</span></span>
> 
> <span data-ttu-id="73113-112">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="73113-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="73113-113">In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="73113-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="73113-114">Inserimento di dipendenze è un modo per rimuovere le dipendenze hardcoded tra gli oggetti, rendendo più semplice per sostituire le dipendenze di un oggetto, uno per i test (tramite oggetti fittizi) o per modificare il comportamento in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="73113-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="73113-115">In questa esercitazione viene illustrato come eseguire l'inserimento di dipendenze su hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="73113-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="73113-116">Viene inoltre illustrato come utilizzare i contenitori IoC con SignalR.</span><span class="sxs-lookup"><span data-stu-id="73113-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="73113-117">Un contenitore di inversione di controllo è un framework generale per l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="73113-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="73113-118">Che cos'è l'inserimento di dipendenze?</span><span class="sxs-lookup"><span data-stu-id="73113-118">What is Dependency Injection?</span></span>

<span data-ttu-id="73113-119">Se si ha già familiarità con l'inserimento di dipendenze, ignorare questa sezione.</span><span class="sxs-lookup"><span data-stu-id="73113-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="73113-120">*Inserimento di dipendenze* (DI) è un modello in cui gli oggetti non sono responsabili della creazione le proprie dipendenze.</span><span class="sxs-lookup"><span data-stu-id="73113-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="73113-121">Di seguito è riportato un esempio semplice di motivare DI.</span><span class="sxs-lookup"><span data-stu-id="73113-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="73113-122">Si supponga di che avere un oggetto che è necessario registrare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="73113-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="73113-123">È possibile definire un'interfaccia di registrazione:</span><span class="sxs-lookup"><span data-stu-id="73113-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="73113-124">L'oggetto, è possibile creare un `ILogger` per registrare i messaggi:</span><span class="sxs-lookup"><span data-stu-id="73113-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="73113-125">Questo procedimento funziona, ma non è l'approccio migliore.</span><span class="sxs-lookup"><span data-stu-id="73113-125">This works, but it's not the best design.</span></span> <span data-ttu-id="73113-126">Se si desidera sostituire `FileLogger` con un altro `ILogger` implementazione, sarà necessario modificare `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="73113-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="73113-127">Presupponendo che molti altri oggetti di uso `FileLogger`, sarà necessario modificare tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="73113-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="73113-128">O se si decide di creare `FileLogger` un singleton, è inoltre necessario apportare modifiche in tutta l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="73113-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="73113-129">Un approccio migliore consiste nel "inserire" un `ILogger` nell'oggetto, ad esempio, usando un argomento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="73113-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="73113-130">Dopo l'oggetto non è responsabile della selezione che `ILogger` da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="73113-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="73113-131">È possibile swich `ILogger` implementazioni senza modificare gli oggetti che dipendono da esso.</span><span class="sxs-lookup"><span data-stu-id="73113-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="73113-132">Questo modello viene denominato [inserimento costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="73113-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="73113-133">Un altro modello è inserimento di setter, in cui impostare la dipendenza tramite una proprietà o metodo setter.</span><span class="sxs-lookup"><span data-stu-id="73113-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="73113-134">Inserimento di dipendenze semplice in SignalR</span><span class="sxs-lookup"><span data-stu-id="73113-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="73113-135">Si consideri l'applicazione di Chat dall'esercitazione [Introduzione a SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="73113-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="73113-136">Di seguito è la classe dell'hub da quell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="73113-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="73113-137">Si supponga che si desidera archiviare i messaggi di chat nel server prima di inviarli.</span><span class="sxs-lookup"><span data-stu-id="73113-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="73113-138">Si potrebbe definire un'interfaccia che astrae questa funzionalità e usare DI per inserire l'interfaccia nella `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="73113-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="73113-139">Il solo problema è che un'applicazione di SignalR non crea direttamente hub; SignalR verranno creati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="73113-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="73113-140">Per impostazione predefinita, SignalR presuppone una classe di hub per avere un costruttore senza parametri.</span><span class="sxs-lookup"><span data-stu-id="73113-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="73113-141">Tuttavia, si può facilmente registrare una funzione per creare istanze di hub e utilizzare questa funzione per eseguire DI.</span><span class="sxs-lookup"><span data-stu-id="73113-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="73113-142">Registrare la funzione chiamando **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="73113-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="73113-143">Ora SignalR richiamerà questa funzione anonima ogni volta che è necessario creare un `ChatHub` istanza.</span><span class="sxs-lookup"><span data-stu-id="73113-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="73113-144">Contenitori IoC</span><span class="sxs-lookup"><span data-stu-id="73113-144">IoC Containers</span></span>

<span data-ttu-id="73113-145">Il codice precedente è appropriato per i casi più semplici.</span><span class="sxs-lookup"><span data-stu-id="73113-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="73113-146">Ma se è ancora necessario scrivere questo:</span><span class="sxs-lookup"><span data-stu-id="73113-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="73113-147">In un'applicazione complessa con numerose dipendenze, potrebbe essere necessario scrivere una grande quantità di questo codice di "collegamento".</span><span class="sxs-lookup"><span data-stu-id="73113-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="73113-148">Questo codice può essere difficile da gestire, soprattutto se le dipendenze sono annidate.</span><span class="sxs-lookup"><span data-stu-id="73113-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="73113-149">È inoltre difficile allo unit test.</span><span class="sxs-lookup"><span data-stu-id="73113-149">It is also hard to unit test.</span></span>

<span data-ttu-id="73113-150">Una soluzione consiste nell'utilizzare un contenitore di inversione di controllo.</span><span class="sxs-lookup"><span data-stu-id="73113-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="73113-151">Un contenitore di inversione di controllo è un componente software che è responsabile della gestione delle dipendenze. Si registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti.</span><span class="sxs-lookup"><span data-stu-id="73113-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="73113-152">Le relazioni di dipendenza determina automaticamente il contenitore.</span><span class="sxs-lookup"><span data-stu-id="73113-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="73113-153">Numero di contenitori IoC consentono inoltre di controllare elementi quali la durata degli oggetti e ambito.</span><span class="sxs-lookup"><span data-stu-id="73113-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="73113-154">"IoC" è l'acronimo di "inversione di controllo", che è un modello generale in cui un framework chiama il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="73113-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="73113-155">Un contenitore IoC costruisce oggetti, quali "inverte" il normale flusso di controllo.</span><span class="sxs-lookup"><span data-stu-id="73113-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="73113-156">Utilizzo di contenitori IoC in SignalR</span><span class="sxs-lookup"><span data-stu-id="73113-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="73113-157">L'applicazione di Chat è probabilmente troppo semplice per trarre vantaggio da un contenitore di inversione di controllo.</span><span class="sxs-lookup"><span data-stu-id="73113-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="73113-158">Al contrario, verrà ora esaminata la [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) esempio.</span><span class="sxs-lookup"><span data-stu-id="73113-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="73113-159">L'esempio StockTicker definisce due classi principali:</span><span class="sxs-lookup"><span data-stu-id="73113-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="73113-160">`StockTickerHub`: La classe di hub, che gestisce le connessioni client.</span><span class="sxs-lookup"><span data-stu-id="73113-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="73113-161">`StockTicker`: Un singleton che contiene i prezzi azionari e li aggiorna periodicamente.</span><span class="sxs-lookup"><span data-stu-id="73113-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="73113-162">`StockTickerHub`contiene un riferimento di `StockTicker` singleton, mentre `StockTicker` contiene un riferimento al **IHubConnectionContext** per il `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="73113-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="73113-163">Usa questa interfaccia per comunicare con `StockTickerHub` istanze.</span><span class="sxs-lookup"><span data-stu-id="73113-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="73113-164">(Per ulteriori informazioni, vedere [Server Broadcast con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="73113-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="73113-165">È possibile usare un contenitore IoC interfoliati un po' queste dipendenze.</span><span class="sxs-lookup"><span data-stu-id="73113-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="73113-166">In primo luogo, consente di semplificare il `StockTickerHub` e `StockTicker` classi.</span><span class="sxs-lookup"><span data-stu-id="73113-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="73113-167">Nel codice seguente, ho impostata come commento le parti che non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="73113-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="73113-168">Rimuovere il costruttore senza parametri da `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="73113-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="73113-169">Invece, si utilizzerà sempre DI creare l'hub.</span><span class="sxs-lookup"><span data-stu-id="73113-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="73113-170">Per StockTicker, rimuovere l'istanza singleton.</span><span class="sxs-lookup"><span data-stu-id="73113-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="73113-171">In un secondo momento, si userà il contenitore di inversione di controllo per controllare la durata di StockTicker.</span><span class="sxs-lookup"><span data-stu-id="73113-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="73113-172">Inoltre, rendere il costruttore pubblico.</span><span class="sxs-lookup"><span data-stu-id="73113-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="73113-173">Successivamente, è possibile effettuare il refactoring di codice tramite la creazione di un'interfaccia per `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="73113-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="73113-174">Questa interfaccia verrà usato per separare il `StockTickerHub` dalla `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="73113-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="73113-175">Visual Studio rende questo tipo di refactoring semplice.</span><span class="sxs-lookup"><span data-stu-id="73113-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="73113-176">Aprire il file StockTicker.cs, fare clic su di `StockTicker` dichiarazione di classe, quindi selezionare **refactoring** ... **Estrai interfaccia**.</span><span class="sxs-lookup"><span data-stu-id="73113-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="73113-177">Nel **Estrai interfaccia** finestra di dialogo, fare clic su **Seleziona tutto**.</span><span class="sxs-lookup"><span data-stu-id="73113-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="73113-178">Lasciare le altre impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="73113-178">Leave the other defaults.</span></span> <span data-ttu-id="73113-179">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="73113-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="73113-180">Visual Studio crea una nuova interfaccia denominata `IStockTicker`e viene modificato anche `StockTicker` da cui derivare `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="73113-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="73113-181">Aprire il file IStockTicker.cs e modificare l'interfaccia per **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="73113-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="73113-182">Nel `StockTickerHub` classe, modificare le due istanze di `StockTicker` a `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="73113-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="73113-183">Creazione di un `IStockTicker` interfaccia non è strettamente necessaria, ma si desidera mostrare come SEN consente di ridurre l'accoppiamento tra componenti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="73113-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="73113-184">Aggiungere la libreria Ninject</span><span class="sxs-lookup"><span data-stu-id="73113-184">Add the Ninject Library</span></span>

<span data-ttu-id="73113-185">Esistono molti contenitori di IoC open source per .NET.</span><span class="sxs-lookup"><span data-stu-id="73113-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="73113-186">Per questa esercitazione si utilizzerà [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="73113-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="73113-187">(Altre librerie comuni includono [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), e [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="73113-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="73113-188">Usare NuGet Package Manager per installare il [Ninject libreria](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="73113-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="73113-189">In Visual Studio, dal **strumenti** menu selezionare **Gestione pacchetti libreria** | **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="73113-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="73113-190">Nella finestra della Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="73113-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="73113-191">Sostituire il Resolver di dipendenza di SignalR</span><span class="sxs-lookup"><span data-stu-id="73113-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="73113-192">Per utilizzare Ninject all'interno di SignalR, creare una classe che deriva da **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="73113-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="73113-193">Questa classe esegue l'override di **GetService** e **GetServices** metodi di **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="73113-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="73113-194">SignalR chiama questi metodi per creare vari oggetti in fase di esecuzione, incluse le istanze di hub, nonché diversi servizi utilizzati internamente da SignalR.</span><span class="sxs-lookup"><span data-stu-id="73113-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="73113-195">Il **GetService** metodo crea una singola istanza di un tipo.</span><span class="sxs-lookup"><span data-stu-id="73113-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="73113-196">Eseguire l'override di questo metodo per chiamare il kernel di Ninject **TryGet** metodo.</span><span class="sxs-lookup"><span data-stu-id="73113-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="73113-197">Se tale metodo restituisce null, eseguire il fallback per il resolver predefinito.</span><span class="sxs-lookup"><span data-stu-id="73113-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="73113-198">Il **GetServices** metodo crea una raccolta di oggetti di un tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="73113-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="73113-199">Eseguire l'override di questo metodo per concatenare i risultati di Ninject e i risultati dal sistema di risoluzione predefinito.</span><span class="sxs-lookup"><span data-stu-id="73113-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="73113-200">Configurare le associazioni Ninject</span><span class="sxs-lookup"><span data-stu-id="73113-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="73113-201">Ora Ninject verrà utilizzata per dichiarare associazioni del tipo.</span><span class="sxs-lookup"><span data-stu-id="73113-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="73113-202">Aprire classe Startup.cs dell'applicazione (uno creato manualmente in base alle istruzioni pacchetto `readme.txt`, o che è stato creato mediante l'aggiunta di autenticazione per il progetto).</span><span class="sxs-lookup"><span data-stu-id="73113-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="73113-203">Nel `Startup.Configuration` (metodo), creare il contenitore Ninject, che chiama Ninject il *kernel*.</span><span class="sxs-lookup"><span data-stu-id="73113-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="73113-204">Creare un'istanza di questo resolver di dipendenza personalizzata:</span><span class="sxs-lookup"><span data-stu-id="73113-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="73113-205">Creare un'associazione per `IStockTicker` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="73113-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="73113-206">Questo codice indica che due elementi.</span><span class="sxs-lookup"><span data-stu-id="73113-206">This code is saying two things.</span></span> <span data-ttu-id="73113-207">Primo, ogni volta che l'applicazione richiede un `IStockTicker`, il kernel deve creare un'istanza di `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="73113-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="73113-208">In secondo luogo, la `StockTicker` classe deve essere creato come un oggetto singleton.</span><span class="sxs-lookup"><span data-stu-id="73113-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="73113-209">Ninject verrà creata un'istanza dell'oggetto e restituire la stessa istanza per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="73113-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="73113-210">Creare un'associazione per **IHubConnectionContext** come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="73113-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="73113-211">Questo creatres codice una funzione anonima che restituisce un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="73113-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="73113-212">Il **WhenInjectedInto** metodo indica Ninject per utilizzare questa funzione solo durante la creazione di `IStockTicker` istanze.</span><span class="sxs-lookup"><span data-stu-id="73113-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="73113-213">Il motivo è che consente di creare SignalR **IHubConnectionContext** internamente, istanze e non si desidera eseguire l'override come SignalR li crea.</span><span class="sxs-lookup"><span data-stu-id="73113-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="73113-214">Questa funzione si applica solo ai nostri `StockTicker` classe.</span><span class="sxs-lookup"><span data-stu-id="73113-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="73113-215">Passare il resolver di dipendenza nel **MapSignalR** metodo mediante l'aggiunta di una configurazione di hub:</span><span class="sxs-lookup"><span data-stu-id="73113-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="73113-216">Aggiornare il metodo Startup.ConfigureSignalR nella classe di avvio dell'esempio con il nuovo parametro:</span><span class="sxs-lookup"><span data-stu-id="73113-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="73113-217">Ora SignalR userà il sistema di risoluzione specificato **MapSignalR**, anziché il resolver predefinito.</span><span class="sxs-lookup"><span data-stu-id="73113-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="73113-218">Ecco l'elenco di codice per `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="73113-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="73113-219">Per eseguire l'applicazione StockTicker in Visual Studio, premere F5.</span><span class="sxs-lookup"><span data-stu-id="73113-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="73113-220">Nella finestra del browser, passare a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="73113-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="73113-221">L'applicazione ha esattamente la stessa funzionalità come prima.</span><span class="sxs-lookup"><span data-stu-id="73113-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="73113-222">(Per una descrizione, vedere [Server Broadcast con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Non è stato modificato il comportamento. appena creati facili da testare, gestire e sviluppare il codice.</span><span class="sxs-lookup"><span data-stu-id="73113-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
