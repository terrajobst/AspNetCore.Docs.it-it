---
uid: web-api/overview/advanced/dependency-injection
title: Dependency Injection in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: In questa esercitazione viene illustrato come inserire le dipendenze nel controller di ASP.NET Web API. Versioni del software utilizzate nell'esercitazione di Web API 2 Unity Application Block...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036515"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="d8e3c-104">Dependency Injection in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="d8e3c-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="d8e3c-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d8e3c-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d8e3c-106">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="d8e3c-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="d8e3c-107">In questa esercitazione viene illustrato come inserire le dipendenze nel controller di ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d8e3c-108">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="d8e3c-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d8e3c-109">Web API 2</span><span class="sxs-lookup"><span data-stu-id="d8e3c-109">Web API 2</span></span>
> - [<span data-ttu-id="d8e3c-110">Blocco applicazione per Unity</span><span class="sxs-lookup"><span data-stu-id="d8e3c-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="d8e3c-111">Entity Framework 6 (versione 5 funziona anche)</span><span class="sxs-lookup"><span data-stu-id="d8e3c-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="d8e3c-112">Che cos'è l'inserimento di dipendenze?</span><span class="sxs-lookup"><span data-stu-id="d8e3c-112">What is Dependency Injection?</span></span>

<span data-ttu-id="d8e3c-113">Oggetto *dipendenza* è qualsiasi oggetto che richiede un altro oggetto.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="d8e3c-114">Ad esempio, è comune per definire un [repository](http://martinfowler.com/eaaCatalog/repository.html) che gestisce l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="d8e3c-115">Di seguito è illustrato un esempio.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-115">Let's illustrate with an example.</span></span> <span data-ttu-id="d8e3c-116">In primo luogo, definiamo un modello di dominio:</span><span class="sxs-lookup"><span data-stu-id="d8e3c-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="d8e3c-117">Di seguito è una classe semplice di repository che contiene gli elementi in un database, mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="d8e3c-118">Ora è pertanto possibile definire un controller API Web che supporta le richieste GET per `Product` entità.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="d8e3c-119">(Spese di spedizione POST e altri metodi per motivi di semplicità.) Di seguito è riportato un primo tentativo:</span><span class="sxs-lookup"><span data-stu-id="d8e3c-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="d8e3c-120">Si noti che la classe controller dipende `ProductRepository`, e ci stiamo consentendo il controller di creare il `ProductRepository` istanza.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="d8e3c-121">Tuttavia, è una buona idea a livello di codice in questo modo, la dipendenza per diversi motivi.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="d8e3c-122">Se si desidera sostituire `ProductRepository` con un'implementazione diversa, è inoltre necessario modificare la classe controller.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="d8e3c-123">Se il `ProductRepository` dispone di dipendenze, è necessario configurare questi all'interno del controller.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="d8e3c-124">Per un progetto di grandi dimensioni con più controller, il codice di configurazione diventa distribuito il progetto.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="d8e3c-125">È difficile allo unit test, perché il controller è hardcoded alla query del database.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="d8e3c-126">Per uno unit test, è consigliabile utilizzare un repository non è possibile con la progettazione imposta fittizi o stub.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="d8e3c-127">È possibile risolvere questi problemi da *inserendo* repository nel controller.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="d8e3c-128">In primo luogo, eseguire il refactoring di `ProductRepository` classe in un'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="d8e3c-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="d8e3c-129">Fornire quindi il `IProductRepository` come parametro di costruttore:</span><span class="sxs-lookup"><span data-stu-id="d8e3c-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="d8e3c-130">Questo esempio viene utilizzato [inserimento costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="d8e3c-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="d8e3c-131">È inoltre possibile utilizzare *inserimento di setter*, in cui è necessario impostare la dipendenza tramite una proprietà o metodo setter.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="d8e3c-132">Ma ora si verifica un problema, perché l'applicazione non è possibile creare il controller direttamente.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="d8e3c-133">API Web crea il controller quando si indirizza la richiesta e l'API Web non riconosce i `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="d8e3c-134">Si tratta di dove è il resolver di dipendenza di Web API disponibile in.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="d8e3c-135">Il Resolver di dipendenza di API Web</span><span class="sxs-lookup"><span data-stu-id="d8e3c-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="d8e3c-136">API Web definisce il **resolver di dipendenza** interfaccia per la risoluzione delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="d8e3c-137">Di seguito è riportata la definizione dell'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="d8e3c-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="d8e3c-138">Il **IDependencyScope** interfaccia dispone di due metodi:</span><span class="sxs-lookup"><span data-stu-id="d8e3c-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="d8e3c-139">**GetService** crea un'istanza di un tipo.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="d8e3c-140">**GetServices** crea una raccolta di oggetti di un tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="d8e3c-141">Il **resolver di dipendenza** metodo eredita **IDependencyScope** e aggiunge il **BeginScope** metodo.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="d8e3c-142">Gli ambiti verrà trattato più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="d8e3c-143">Quando l'API Web crea un'istanza del controller, chiama innanzitutto **IDependencyResolver.GetService**, passando il tipo di controller.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="d8e3c-144">È possibile utilizzare questo hook di estensibilità per creare il controller, di risoluzione di eventuali dipendenze.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="d8e3c-145">Se **GetService** restituisce null, API Web cerca un costruttore senza parametri nella classe controller.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="d8e3c-146">Risoluzione delle dipendenze con il contenitore di Unity</span><span class="sxs-lookup"><span data-stu-id="d8e3c-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="d8e3c-147">Sebbene sia possibile scrivere un completo **resolver di dipendenza** implementazione partendo da zero, l'interfaccia è destinato funga da ponte tra API Web e i contenitori IoC esistenti.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="d8e3c-148">Un contenitore di inversione di controllo è un componente software che è responsabile della gestione delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="d8e3c-149">Si registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="d8e3c-150">Le relazioni di dipendenza determina automaticamente il contenitore.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="d8e3c-151">Numero di contenitori IoC consentono inoltre di controllare elementi quali la durata degli oggetti e ambito.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="d8e3c-152">"IoC" è l'acronimo di "inversione di controllo", che è un modello generale in cui un framework chiama il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="d8e3c-153">Un contenitore IoC costruisce oggetti, quali "inverte" il normale flusso di controllo.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="d8e3c-154">Per questa esercitazione si userà [Unity](https://msdn.microsoft.com/library/ff647202.aspx) da Microsoft Patterns &amp; consigliate.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="d8e3c-155">(Altre librerie comuni includono [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), e [StructureMap ](http://docs.structuremap.net/).) Per installare Unity, è possibile utilizzare Gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="d8e3c-156">Dal **strumenti** menu in Visual Studio, selezionare **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d8e3c-157">Nella finestra della Console di gestione pacchetti, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d8e3c-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="d8e3c-158">Di seguito è un'implementazione di **resolver di dipendenza** che esegue il wrapping di un contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="d8e3c-159">Se il **GetService** (metodo) non è possibile risolvere un tipo, deve restituire **null**.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="d8e3c-160">Se il **GetServices** metodo non è possibile risolvere un tipo, deve restituire un oggetto raccolta vuoto.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="d8e3c-161">Non generano eccezioni per i tipi sconosciuti.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="d8e3c-162">Configurare il Resolver di dipendenza</span><span class="sxs-lookup"><span data-stu-id="d8e3c-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="d8e3c-163">Impostare il resolver di dipendenza per il **DependencyResolver** proprietà dell'oggetto globale **HttpConfiguration** oggetto.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="d8e3c-164">Nell'esempio di codice registri il `IProductRepository` interfacciarsi con Unity e crea quindi un `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="d8e3c-165">Ambito di dipendenza e la durata di Controller</span><span class="sxs-lookup"><span data-stu-id="d8e3c-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="d8e3c-166">I controller vengono creati per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-166">Controllers are created per request.</span></span> <span data-ttu-id="d8e3c-167">Per la gestione della durata degli oggetti, **resolver di dipendenza** viene utilizzato il concetto di un *ambito*.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="d8e3c-168">Il resolver di dipendenza è collegata la **HttpConfiguration** oggetto ha un ambito globale.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="d8e3c-169">Quando l'API Web viene creato un controller, chiama **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="d8e3c-170">Questo metodo restituisce un **IDependencyScope** che rappresenta un ambito figlio.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="d8e3c-171">API Web chiama quindi **GetService** nell'ambito figlio per creare il controller.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="d8e3c-172">Una volta completata, richiesta di chiamate all'API Web **Dispose** nell'ambito figlio.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="d8e3c-173">Utilizzare il **Dispose** metodo per eliminare le dipendenze del controller.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="d8e3c-174">Modalità di implementazione di **BeginScope** varia a seconda del contenitore di inversione di controllo.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="d8e3c-175">Per Unity, ambito corrisponde a un contenitore figlio:</span><span class="sxs-lookup"><span data-stu-id="d8e3c-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="d8e3c-176">La maggior parte dei contenitori IoC dispongono di equivalenti simili.</span><span class="sxs-lookup"><span data-stu-id="d8e3c-176">Most IoC containers have similar equivalents.</span></span>
