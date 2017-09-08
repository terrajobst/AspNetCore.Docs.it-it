---
title: Inserimento di dipendenze controller
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bc8b4ba3-e9ba-48fd-b1eb-cd48ff6bc7a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 371fb0f721797e4d8f7a26858ae0a709cb5cd39e
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="dependency-injection-into-controllers"></a><span data-ttu-id="51bdf-103">Inserimento di dipendenze controller</span><span class="sxs-lookup"><span data-stu-id="51bdf-103">Dependency injection into controllers</span></span>

<a name=dependency-injection-controllers></a>

<span data-ttu-id="51bdf-104">Da [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="51bdf-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="51bdf-105">Controller MVC ASP.NET Core deve richiedere le relative dipendenze in modo esplicito tramite i relativi costruttori.</span><span class="sxs-lookup"><span data-stu-id="51bdf-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="51bdf-106">In alcuni casi, le azioni del controller singoli potrebbero richiedere un servizio e non può essere utile per richiedere a livello di controller.</span><span class="sxs-lookup"><span data-stu-id="51bdf-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="51bdf-107">In questo caso, è possibile scegliere di inserire un servizio come parametro al metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="51bdf-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

[<span data-ttu-id="51bdf-108">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="51bdf-108">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample)

## <a name="dependency-injection"></a><span data-ttu-id="51bdf-109">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="51bdf-109">Dependency Injection</span></span>

<span data-ttu-id="51bdf-110">Inserimento di dipendenze è una tecnica che segue il [principio di inversione dipendenza](http://deviq.com/dependency-inversion-principle), consentendo di applicazioni deve essere composta da moduli regime di controllo.</span><span class="sxs-lookup"><span data-stu-id="51bdf-110">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="51bdf-111">ASP.NET di base include il supporto incorporato per [inserimento di dipendenze](../../fundamentals/dependency-injection.md), rendendo più facile da testare e gestire le applicazioni.</span><span class="sxs-lookup"><span data-stu-id="51bdf-111">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="51bdf-112">Inserimento del costruttore</span><span class="sxs-lookup"><span data-stu-id="51bdf-112">Constructor Injection</span></span>

<span data-ttu-id="51bdf-113">Supporto incorporato dell'ASP.NET Core inserimento di dipendenza basato sul costruttore si estende al controller MVC.</span><span class="sxs-lookup"><span data-stu-id="51bdf-113">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="51bdf-114">Aggiungendo semplicemente un tipo di servizio al controller come parametro costruttore, ASP.NET Core tenterà di risolvere il tipo in questione utilizzando incorporato nel contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="51bdf-114">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="51bdf-115">I servizi sono in genere, ma non sempre, definiti utilizzando le interfacce.</span><span class="sxs-lookup"><span data-stu-id="51bdf-115">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="51bdf-116">Ad esempio, se l'applicazione dispone di logica di business che dipende dal tempo corrente, è possibile inserire un servizio che recupera il tempo (anziché a livello di codice), quali i test passare le implementazioni che usano un determinato periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="51bdf-116">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

<span data-ttu-id="51bdf-117">[!code-csharp[Principale](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]</span><span class="sxs-lookup"><span data-stu-id="51bdf-117">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]</span></span>


<span data-ttu-id="51bdf-118">Implementazione di un'interfaccia come questo in modo che utilizzi l'orologio di sistema in fase di esecuzione è piuttosto semplice:</span><span class="sxs-lookup"><span data-stu-id="51bdf-118">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

<span data-ttu-id="51bdf-119">[!code-csharp[Principale](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]</span><span class="sxs-lookup"><span data-stu-id="51bdf-119">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]</span></span>


<span data-ttu-id="51bdf-120">A questo punto, è possibile usare il servizio in questo controller.</span><span class="sxs-lookup"><span data-stu-id="51bdf-120">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="51bdf-121">In questo caso, è stata aggiunta logica per il `HomeController` `Index` metodo per visualizzare un saluto all'utente in base all'ora del giorno.</span><span class="sxs-lookup"><span data-stu-id="51bdf-121">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

<span data-ttu-id="51bdf-122">[!code-csharp[Principale](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]</span><span class="sxs-lookup"><span data-stu-id="51bdf-122">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]</span></span>

<span data-ttu-id="51bdf-123">Se si esegue ora l'applicazione, si verificherà probabilmente un errore:</span><span class="sxs-lookup"><span data-stu-id="51bdf-123">If we run the application now, we will most likely encounter an error:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="51bdf-124">Questo errore si verifica quando non è stato configurato un servizio nel `ConfigureServices` metodo nel nostro `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="51bdf-124">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="51bdf-125">Per specificare che le richieste di `IDateTime` deve essere risolto utilizzando un'istanza di `SystemDateTime`, aggiungere la riga evidenziata nell'elenco seguente per il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="51bdf-125">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

<span data-ttu-id="51bdf-126">[!code-csharp[Principale](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]</span><span class="sxs-lookup"><span data-stu-id="51bdf-126">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]</span></span>

> [!NOTE]
> <span data-ttu-id="51bdf-127">Questo particolare servizio può essere implementato usando una qualsiasi delle diverse opzioni di durata diversa (`Transient`, `Scoped`, o `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="51bdf-127">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="51bdf-128">Vedere [Dependency Injection](../../fundamentals/dependency-injection.md) comprendere ognuna di queste opzioni di ambito verrà influenza il comportamento del servizio.</span><span class="sxs-lookup"><span data-stu-id="51bdf-128">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="51bdf-129">Una volta configurato il servizio, che esegue l'applicazione e passare alla home page dovrebbe essere visualizzato il messaggio basato sul tempo come previsto:</span><span class="sxs-lookup"><span data-stu-id="51bdf-129">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Messaggio introduttivo di server](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="51bdf-131">Vedere [logica del Controller di test](testing.md) per informazioni su come richiedere in modo esplicito le dipendenze [http://deviq.com/explicit-dependencies-principle](http://deviq.com/explicit-dependencies-principle) nei controller di codice risulta più facile da testare.</span><span class="sxs-lookup"><span data-stu-id="51bdf-131">See [Testing Controller Logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle](http://deviq.com/explicit-dependencies-principle) in controllers makes code easier to test.</span></span>

<span data-ttu-id="51bdf-132">Inserimento di dipendenze predefinito del ASP.NET Core supporta solo un singolo costruttore per le classi di richiesta di servizi.</span><span class="sxs-lookup"><span data-stu-id="51bdf-132">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="51bdf-133">Se si dispone di più di un costruttore, si potrebbe ottenere un'eccezione indicante:</span><span class="sxs-lookup"><span data-stu-id="51bdf-133">If you have more than one constructor, you may get an exception stating:</span></span>

<!-- literal_block {"ids": [], "xml:space": "preserve"} -->

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="51bdf-134">Come indicato nel messaggio di errore, è possibile correggere il problema con solo un singolo costruttore.</span><span class="sxs-lookup"><span data-stu-id="51bdf-134">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="51bdf-135">È anche possibile [sostituire il supporto di inserimento di dipendenza predefinito con un'implementazione di terze parti](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), molti dei quali supporta più costruttori.</span><span class="sxs-lookup"><span data-stu-id="51bdf-135">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="51bdf-136">Azione di inserimento con FromServices</span><span class="sxs-lookup"><span data-stu-id="51bdf-136">Action Injection with FromServices</span></span>

<span data-ttu-id="51bdf-137">In alcuni casi non è necessario un servizio per più di un'azione all'interno del controller.</span><span class="sxs-lookup"><span data-stu-id="51bdf-137">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="51bdf-138">In questo caso, può avere senso per inserire il servizio come parametro al metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="51bdf-138">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="51bdf-139">Questa operazione viene eseguita contrassegnando il parametro con l'attributo `[FromServices]` come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="51bdf-139">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

<span data-ttu-id="51bdf-140">[!code-csharp[Principale](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]</span><span class="sxs-lookup"><span data-stu-id="51bdf-140">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]</span></span>

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="51bdf-141">Accesso alle impostazioni da un Controller</span><span class="sxs-lookup"><span data-stu-id="51bdf-141">Accessing Settings from a Controller</span></span>

<span data-ttu-id="51bdf-142">Accesso alle impostazioni dell'applicazione o di configurazione all'interno di un controller è un modello comune.</span><span class="sxs-lookup"><span data-stu-id="51bdf-142">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="51bdf-143">Questo tipo di accesso deve utilizzare il modello di opzioni descritto [configurazione](../../fundamentals/configuration.md).</span><span class="sxs-lookup"><span data-stu-id="51bdf-143">This access should use the Options pattern described in [configuration](../../fundamentals/configuration.md).</span></span> <span data-ttu-id="51bdf-144">È in genere consigliabile non richiedere impostazioni direttamente dal controller di mediante l'inserimento di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="51bdf-144">You generally should not request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="51bdf-145">Un approccio migliore consiste nella richiesta un `IOptions<T>` istanza, in cui `T` è la classe di configurazione è necessario.</span><span class="sxs-lookup"><span data-stu-id="51bdf-145">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="51bdf-146">Per utilizzare il modello di opzioni, è necessario creare una classe che rappresenta le opzioni, ad esempio questo:</span><span class="sxs-lookup"><span data-stu-id="51bdf-146">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

<span data-ttu-id="51bdf-147">[!code-csharp[Principale](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]</span><span class="sxs-lookup"><span data-stu-id="51bdf-147">[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]</span></span>

<span data-ttu-id="51bdf-148">È quindi necessario configurare l'applicazione per utilizzare il modello di opzioni e aggiungere la classe di configurazione per la raccolta di servizi in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="51bdf-148">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

<span data-ttu-id="51bdf-149">[!code-csharp[Principale](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]</span><span class="sxs-lookup"><span data-stu-id="51bdf-149">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]</span></span>

> [!NOTE]
> <span data-ttu-id="51bdf-150">Nell'elenco precedente, è in fase di configurazione dell'applicazione per leggere le impostazioni da un file in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="51bdf-150">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="51bdf-151">È anche possibile configurare le impostazioni interamente nel codice, come illustrato nel codice commentato precedente.</span><span class="sxs-lookup"><span data-stu-id="51bdf-151">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="51bdf-152">Vedere [configurazione](../../fundamentals/configuration.md) per ulteriori opzioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="51bdf-152">See [Configuration](../../fundamentals/configuration.md) for further configuration options.</span></span>

<span data-ttu-id="51bdf-153">Dopo aver specificato un oggetto fortemente tipizzato configurazione (in questo caso, `SampleWebSettings`) e aggiungerlo alla raccolta di servizi, è possibile richiedere qualsiasi metodo di azione o Controller richiedendo un'istanza di `IOptions<T>` (in questo caso, `IOptions<SampleWebSettings>`) .</span><span class="sxs-lookup"><span data-stu-id="51bdf-153">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="51bdf-154">Il codice seguente viene illustrato come uno richiede le impostazioni da un controller:</span><span class="sxs-lookup"><span data-stu-id="51bdf-154">The following code shows how one would request the settings from a controller:</span></span>

<span data-ttu-id="51bdf-155">[!code-csharp[Principale](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]</span><span class="sxs-lookup"><span data-stu-id="51bdf-155">[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]</span></span>

<span data-ttu-id="51bdf-156">Modello di opzioni consente le impostazioni e configurazione per essere separato rispetto a altro e assicura il controller è seguito [separazione delle problematiche](http://deviq.com/separation-of-concerns/), poiché non è necessario sapere come o dove trovare le impostazioni informazioni.</span><span class="sxs-lookup"><span data-stu-id="51bdf-156">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="51bdf-157">Inoltre semplifica il controller di test unità [logica del Controller di test](testing.md), poiché non esiste alcun [adesive statico](http://deviq.com/static-cling/) o diretta creazione di istanze di classi di impostazioni all'interno della classe controller.</span><span class="sxs-lookup"><span data-stu-id="51bdf-157">It also makes the controller easier to unit test [Testing Controller Logic](testing.md), since there is no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
