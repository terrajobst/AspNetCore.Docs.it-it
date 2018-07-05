---
uid: web-api/overview/advanced/dependency-injection
title: Inserimento delle dipendenze in ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come inserire le dipendenze nel controller dell'API Web ASP.NET. Versioni del software utilizzate nell'esercitazione di Web API 2 Unity Application Block...
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: a470c778fd5998006a0bf8edb08b62a75d72c48c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802675"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="72ae7-104">Inserimento delle dipendenze in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="72ae7-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="72ae7-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="72ae7-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="72ae7-106">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="72ae7-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="72ae7-107">Questa esercitazione illustra come inserire le dipendenze nel controller dell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="72ae7-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="72ae7-108">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="72ae7-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="72ae7-109">API Web 2</span><span class="sxs-lookup"><span data-stu-id="72ae7-109">Web API 2</span></span>
> - [<span data-ttu-id="72ae7-110">Unity Application Block</span><span class="sxs-lookup"><span data-stu-id="72ae7-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="72ae7-111">Entity Framework 6 (versione 5 funziona anche)</span><span class="sxs-lookup"><span data-stu-id="72ae7-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="72ae7-112">Che cos'è l'inserimento delle dipendenze?</span><span class="sxs-lookup"><span data-stu-id="72ae7-112">What is Dependency Injection?</span></span>

<span data-ttu-id="72ae7-113">Oggetto *dipendenza* è qualsiasi oggetto che richiede un altro oggetto.</span><span class="sxs-lookup"><span data-stu-id="72ae7-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="72ae7-114">Ad esempio, è comune per definire un [repository](http://martinfowler.com/eaaCatalog/repository.html) che gestisce l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="72ae7-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="72ae7-115">Di seguito è illustrato un esempio.</span><span class="sxs-lookup"><span data-stu-id="72ae7-115">Let's illustrate with an example.</span></span> <span data-ttu-id="72ae7-116">In primo luogo, si definirà un modello di dominio:</span><span class="sxs-lookup"><span data-stu-id="72ae7-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="72ae7-117">Di seguito è una classe di repository semplice che archivia gli elementi in un database usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="72ae7-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="72ae7-118">A questo punto è possibile definire un controller API Web che supporta le richieste GET per `Product` entità.</span><span class="sxs-lookup"><span data-stu-id="72ae7-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="72ae7-119">(Spese di spedizione POST e altri metodi per motivi di semplicità.) Di seguito è un primo tentativo:</span><span class="sxs-lookup"><span data-stu-id="72ae7-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="72ae7-120">Si noti che la classe controller dipende `ProductRepository`, e si sta consentendo il controller crea il `ProductRepository` istanza.</span><span class="sxs-lookup"><span data-stu-id="72ae7-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="72ae7-121">Tuttavia, è opportuno evitare di livello di codice in questo modo, la dipendenza per diversi motivi.</span><span class="sxs-lookup"><span data-stu-id="72ae7-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="72ae7-122">Se si desidera sostituire `ProductRepository` con un'implementazione diversa, è anche necessario modificare la classe controller.</span><span class="sxs-lookup"><span data-stu-id="72ae7-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="72ae7-123">Se il `ProductRepository` ha dipendenze, è necessario configurare questi all'interno del controller.</span><span class="sxs-lookup"><span data-stu-id="72ae7-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="72ae7-124">Per un progetto di grandi dimensioni con più controller, il codice di configurazione diventa sparsi tra il progetto.</span><span class="sxs-lookup"><span data-stu-id="72ae7-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="72ae7-125">È difficile allo unit test, perché il controller è hardcoded su query del database.</span><span class="sxs-lookup"><span data-stu-id="72ae7-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="72ae7-126">Per uno unit test, è consigliabile usare un repository fittizi o stub, che non è possibile eseguire con la progettazione di imposta.</span><span class="sxs-lookup"><span data-stu-id="72ae7-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="72ae7-127">Possiamo risolvere questi problemi dal *inserimento* il repository nel controller.</span><span class="sxs-lookup"><span data-stu-id="72ae7-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="72ae7-128">In primo luogo, eseguire il refactoring di `ProductRepository` classe in un'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="72ae7-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="72ae7-129">Fornire quindi il `IProductRepository` come parametro di costruttore:</span><span class="sxs-lookup"><span data-stu-id="72ae7-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="72ae7-130">Questo esempio viene usato [inserimento del costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="72ae7-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="72ae7-131">È anche possibile usare *inserimento di setter*, in cui è necessario impostare la dipendenza attraverso un metodo di impostazione o una proprietà.</span><span class="sxs-lookup"><span data-stu-id="72ae7-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="72ae7-132">Ma Ecco presentarsi un problema, perché l'applicazione non crea direttamente il controller.</span><span class="sxs-lookup"><span data-stu-id="72ae7-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="72ae7-133">API Web crea il controller quando indirizza la richiesta e API Web non riconosce i `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="72ae7-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="72ae7-134">Questo è il resolver di dipendenza di API Web entra in gioco.</span><span class="sxs-lookup"><span data-stu-id="72ae7-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="72ae7-135">Il Resolver di dipendenza di API Web</span><span class="sxs-lookup"><span data-stu-id="72ae7-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="72ae7-136">API Web definisce la **IDependencyResolver** interfaccia per la risoluzione delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="72ae7-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="72ae7-137">Ecco la definizione dell'interfaccia:</span><span class="sxs-lookup"><span data-stu-id="72ae7-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="72ae7-138">Il **IDependencyScope** interfaccia dispone di due metodi:</span><span class="sxs-lookup"><span data-stu-id="72ae7-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="72ae7-139">**GetService** crea un'istanza di un tipo.</span><span class="sxs-lookup"><span data-stu-id="72ae7-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="72ae7-140">**GetServices** crea una raccolta di oggetti di un tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="72ae7-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="72ae7-141">Il **IDependencyResolver** metodo eredita **IDependencyScope** e aggiunge i **BeginScope** (metodo).</span><span class="sxs-lookup"><span data-stu-id="72ae7-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="72ae7-142">Gli ambiti saranno trattati più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="72ae7-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="72ae7-143">Quando l'API Web crea un'istanza del controller, viene prima chiamato **IDependencyResolver.GetService**, passando il tipo di controller.</span><span class="sxs-lookup"><span data-stu-id="72ae7-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="72ae7-144">È possibile utilizzare questo hook di estensibilità per creare il controller, la risoluzione di eventuali dipendenze.</span><span class="sxs-lookup"><span data-stu-id="72ae7-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="72ae7-145">Se **GetService** restituisce null, API Web cerca un costruttore senza parametri nella classe controller.</span><span class="sxs-lookup"><span data-stu-id="72ae7-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="72ae7-146">Risoluzione delle dipendenze con il contenitore di Unity</span><span class="sxs-lookup"><span data-stu-id="72ae7-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="72ae7-147">Anche se è possibile scrivere una completa **IDependencyResolver** implementazione da zero, l'interfaccia è realmente progettato per fungere da ponte tra API Web e i contenitori IoC esistenti.</span><span class="sxs-lookup"><span data-stu-id="72ae7-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="72ae7-148">Un contenitore IoC è un componente software che è responsabile della gestione delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="72ae7-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="72ae7-149">Registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti.</span><span class="sxs-lookup"><span data-stu-id="72ae7-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="72ae7-150">Il contenitore determina automaticamente le relazioni di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="72ae7-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="72ae7-151">Numerosi contenitori IoC consentono anche di controllare elementi quali la durata degli oggetti e l'ambito.</span><span class="sxs-lookup"><span data-stu-id="72ae7-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="72ae7-152">"IoC" è l'acronimo di "inversione di controllo", ovvero un modello generale in cui un framework chiama il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="72ae7-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="72ae7-153">Un contenitore IoC costruisce oggetti, quali "inverte" il normale flusso di controllo.</span><span class="sxs-lookup"><span data-stu-id="72ae7-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="72ae7-154">Per questa esercitazione, useremo [Unity](https://msdn.microsoft.com/library/ff647202.aspx) da Microsoft Patterns &amp; procedure consigliate.</span><span class="sxs-lookup"><span data-stu-id="72ae7-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="72ae7-155">(Includono altre librerie molto diffuse [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), e [StructureMap ](http://docs.structuremap.net/).) È possibile usare Gestione pacchetti NuGet per installare Unity.</span><span class="sxs-lookup"><span data-stu-id="72ae7-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="72ae7-156">Dal **degli strumenti** dal menu di Visual Studio, selezionare **Library Package Manager**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="72ae7-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="72ae7-157">Nella finestra della Console di gestione pacchetti, digitare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="72ae7-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="72ae7-158">Ecco un'implementazione di **IDependencyResolver** che esegue il wrapping di un contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="72ae7-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="72ae7-159">Se il **GetService** metodo non è possibile risolvere un tipo, deve restituire **null**.</span><span class="sxs-lookup"><span data-stu-id="72ae7-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="72ae7-160">Se il **GetServices** (metodo) non è possibile risolvere un tipo, deve restituire un oggetto raccolta vuoto.</span><span class="sxs-lookup"><span data-stu-id="72ae7-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="72ae7-161">Non generare eccezioni per i tipi sconosciuti.</span><span class="sxs-lookup"><span data-stu-id="72ae7-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="72ae7-162">Configurare il Resolver di dipendenza</span><span class="sxs-lookup"><span data-stu-id="72ae7-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="72ae7-163">Impostare il resolver di dipendenza per il **DependencyResolver** proprietà dell'oggetto globale **HttpConfiguration** oggetto.</span><span class="sxs-lookup"><span data-stu-id="72ae7-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="72ae7-164">Il codice seguente registra il `IProductRepository` interfacciarsi con Unity e quindi crea un `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="72ae7-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="72ae7-165">Ambito delle dipendenze e la durata di Controller</span><span class="sxs-lookup"><span data-stu-id="72ae7-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="72ae7-166">I controller vengono creati per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="72ae7-166">Controllers are created per request.</span></span> <span data-ttu-id="72ae7-167">Per la gestione della durata degli oggetti, **IDependencyResolver** Usa il concetto di un *ambito*.</span><span class="sxs-lookup"><span data-stu-id="72ae7-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="72ae7-168">Il resolver di dipendenza associata ai **HttpConfiguration** oggetto ha un ambito globale.</span><span class="sxs-lookup"><span data-stu-id="72ae7-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="72ae7-169">Quando l'API Web crea un controller, viene chiamato **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="72ae7-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="72ae7-170">Questo metodo restituisce un **IDependencyScope** che rappresenta un ambito figlio.</span><span class="sxs-lookup"><span data-stu-id="72ae7-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="72ae7-171">API Web chiamerà **GetService** nell'ambito figlio per creare il controller.</span><span class="sxs-lookup"><span data-stu-id="72ae7-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="72ae7-172">Quando richiesta risulta completata, l'API Web chiama **Dispose** nell'ambito figlio.</span><span class="sxs-lookup"><span data-stu-id="72ae7-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="72ae7-173">Usare la **Dispose** metodo per l'eliminazione delle dipendenze del controller.</span><span class="sxs-lookup"><span data-stu-id="72ae7-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="72ae7-174">Modalità di implementazione **BeginScope** dipende dal contenitore IoC.</span><span class="sxs-lookup"><span data-stu-id="72ae7-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="72ae7-175">Per Unity, ambito corrisponde a un contenitore figlio:</span><span class="sxs-lookup"><span data-stu-id="72ae7-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="72ae7-176">La maggior parte dei contenitori IoC dispongono di equivalenti simile.</span><span class="sxs-lookup"><span data-stu-id="72ae7-176">Most IoC containers have similar equivalents.</span></span>
