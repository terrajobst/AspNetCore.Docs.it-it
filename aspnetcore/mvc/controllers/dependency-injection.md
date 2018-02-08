---
title: Inserimento di dipendenze in controller
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 118f504311b58258b5a0510477280505135dd2d9
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-into-controllers"></a><span data-ttu-id="790ea-102">Inserimento di dipendenze in controller</span><span class="sxs-lookup"><span data-stu-id="790ea-102">Dependency injection into controllers</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="790ea-103">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="790ea-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="790ea-104">I controller ASP.NET Core MVC devono richiedere le proprie dipendenze in modo esplicito tramite i rispettivi costruttori.</span><span class="sxs-lookup"><span data-stu-id="790ea-104">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="790ea-105">In alcune istanze, singole azioni di controller possono richiedere un servizio che potrebbe non avere senso richiedere a livello di controller.</span><span class="sxs-lookup"><span data-stu-id="790ea-105">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="790ea-106">In questo caso, è anche possibile inserire un servizio come parametro nel metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="790ea-106">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="790ea-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="790ea-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="790ea-108">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="790ea-108">Dependency Injection</span></span>

<span data-ttu-id="790ea-109">L'inserimento di dipendenze è una tecnica che segue il [principio di inversione delle dipendenze](http://deviq.com/dependency-inversion-principle/) e consente di comporre applicazioni con moduli poco accoppiati.</span><span class="sxs-lookup"><span data-stu-id="790ea-109">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="790ea-110">ASP.NET Core dispone del supporto incorporato dell'[inserimento di dipendenze](../../fundamentals/dependency-injection.md), rendendo le applicazioni più facili da testare e gestire.</span><span class="sxs-lookup"><span data-stu-id="790ea-110">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="790ea-111">Inserimento di costruttori</span><span class="sxs-lookup"><span data-stu-id="790ea-111">Constructor Injection</span></span>

<span data-ttu-id="790ea-112">Il supporto incorporato di ASP.NET Core per l'inserimento di dipendenze basato sul costruttore si estende ai controller MVC.</span><span class="sxs-lookup"><span data-stu-id="790ea-112">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="790ea-113">Aggiungendo semplicemente un tipo di servizio al controller come parametro del costruttore, ASP.NET Core tenta di risolvere il tipo corrispondente tramite il proprio contenitore di servizi incorporato.</span><span class="sxs-lookup"><span data-stu-id="790ea-113">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="790ea-114">In genere, ma non sempre, i servizi vengono definiti tramite interfacce.</span><span class="sxs-lookup"><span data-stu-id="790ea-114">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="790ea-115">Se, ad esempio, la logica di business dell'applicazione dipende dall'ora corrente, è possibile inserire un servizio che recuperi l'ora, anziché impostarlo come hardcoded. Ciò consente di superare i test nelle implementazioni che usano un'ora impostata.</span><span class="sxs-lookup"><span data-stu-id="790ea-115">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="790ea-116">L'implementazione di un'interfaccia come questa che usi l'orologio di sistema in fase di runtime è semplice:</span><span class="sxs-lookup"><span data-stu-id="790ea-116">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="790ea-117">A questo punto è possibile usare il servizio nel controller.</span><span class="sxs-lookup"><span data-stu-id="790ea-117">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="790ea-118">In questo caso, è stata aggiunta al metodo `HomeController` `Index` la logica che consente di visualizzare un saluto a seconda dell'ora del giorno.</span><span class="sxs-lookup"><span data-stu-id="790ea-118">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="790ea-119">Se si esegue l'applicazione ora, molto probabilmente si verifica un errore:</span><span class="sxs-lookup"><span data-stu-id="790ea-119">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="790ea-120">Questo errore si verifica se non è stato configurato alcun servizio nel metodo `ConfigureServices` della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="790ea-120">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="790ea-121">Per specificare che le richieste di `IDateTime` devono essere risolte tramite un'istanza di `SystemDateTime`, aggiungere la riga evidenziata nel codice seguente al metodo `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="790ea-121">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="790ea-122">Questo servizio specifico può essere implementato usando una qualsiasi delle diverse opzioni di durata (`Transient`, `Scoped` o `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="790ea-122">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="790ea-123">Per comprendere in che modo ognuna di queste opzioni di ambito influisca sul comportamento del servizio, vedere [Inserimento di dipendenze](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="790ea-123">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="790ea-124">Dopo che il servizio è stato configurato, se si esegue l'applicazione e si passa alla home page viene visualizzato il messaggio basato sull'ora, come previsto:</span><span class="sxs-lookup"><span data-stu-id="790ea-124">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Saluto del server](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="790ea-126">Vedere [Test della logica dei controller](testing.md) per informazioni su come la richiesta esplicita di dipendenze [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) nei controller renda più semplice il test del codice.</span><span class="sxs-lookup"><span data-stu-id="790ea-126">See [Testing Controller Logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="790ea-127">L'inserimento di dipendenze predefinito in ASP.NET Core supporta un unico costruttore per la richiesta di servizi da parte delle classi.</span><span class="sxs-lookup"><span data-stu-id="790ea-127">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="790ea-128">Se ci sono più costruttori, può essere visualizzata l'eccezione seguente:</span><span class="sxs-lookup"><span data-stu-id="790ea-128">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="790ea-129">Come indicato nel messaggio di errore, è possibile correggere il problema mantenendo un unico costruttore.</span><span class="sxs-lookup"><span data-stu-id="790ea-129">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="790ea-130">È anche possibile [sostituire il supporto dell'inserimento di dipendenze predefinito con un'implementazione di terze parti](../../fundamentals/dependency-injection.md#replacing-the-default-services-container). Molte di queste implementazioni supportano più costruttori.</span><span class="sxs-lookup"><span data-stu-id="790ea-130">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="790ea-131">Inserimento di azioni con FromServices</span><span class="sxs-lookup"><span data-stu-id="790ea-131">Action Injection with FromServices</span></span>

<span data-ttu-id="790ea-132">Talvolta un servizio è necessario per un sola azione all'interno del controller.</span><span class="sxs-lookup"><span data-stu-id="790ea-132">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="790ea-133">In questo caso, può avere senso inserire il servizio come parametro nel metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="790ea-133">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="790ea-134">Per eseguire questa operazione, si contrassegna il parametro con l'attributo `[FromServices]`, come illustrato qui:</span><span class="sxs-lookup"><span data-stu-id="790ea-134">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="790ea-135">Accesso alle impostazioni da un controller</span><span class="sxs-lookup"><span data-stu-id="790ea-135">Accessing Settings from a Controller</span></span>

<span data-ttu-id="790ea-136">L'accesso alle impostazioni di un'applicazione o alle impostazioni di configurazione da un controller è un tipo di azione comune.</span><span class="sxs-lookup"><span data-stu-id="790ea-136">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="790ea-137">Questo tipo di accesso deve usare il modello di opzioni descritto in [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="790ea-137">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="790ea-138">In genere non si devono richiedere impostazioni direttamente dal controller tramite l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="790ea-138">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="790ea-139">Un approccio migliore consiste nella richiesta di un'istanza di `IOptions<T>`, dove `T` è la classe di configurazione necessaria.</span><span class="sxs-lookup"><span data-stu-id="790ea-139">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="790ea-140">Per usare il modello di opzioni, è necessario creare una classe che rappresenti le opzioni, ad esempio questa:</span><span class="sxs-lookup"><span data-stu-id="790ea-140">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="790ea-141">È quindi necessario configurare l'applicazione in modo che usi il modello di opzioni e aggiungere la classe di configurazione alla raccolta dei servizi in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="790ea-141">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="790ea-142">Il codice precedente configura l'applicazione in modo che legga le impostazioni da un file in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="790ea-142">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="790ea-143">È anche possibile configurare interamente le impostazioni nel codice, come illustrato nel codice commentato precedente.</span><span class="sxs-lookup"><span data-stu-id="790ea-143">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="790ea-144">Per altre opzioni di configurazione, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="790ea-144">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="790ea-145">Dopo aver specificato un oggetto di configurazione fortemente tipizzato (in questo caso, `SampleWebSettings`) e dopo averlo aggiunto alla raccolta di servizi, è possibile richiederlo da qualsiasi controller o metodo di azione richiedendo un'istanza di `IOptions<T>` (in questo caso, `IOptions<SampleWebSettings>`).</span><span class="sxs-lookup"><span data-stu-id="790ea-145">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="790ea-146">Il codice seguente illustra come richiedere le impostazioni da un controller:</span><span class="sxs-lookup"><span data-stu-id="790ea-146">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="790ea-147">Se si segue il modello di opzioni, è possibile disaccoppiare le impostazioni e la configurazione. Ciò garantisce che il controller segua il principio della [separazione delle competenze](http://deviq.com/separation-of-concerns/), poiché non deve sapere come o dove trovare le informazioni sulle impostazioni.</span><span class="sxs-lookup"><span data-stu-id="790ea-147">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="790ea-148">È anche più facile eseguire lo unit test del controller (vedere [Test della logica dei controller](testing.md)), poiché non ci sono [vincoli statici](http://deviq.com/static-cling/) o creazione diretta di istanze all'interno della classe del controller.</span><span class="sxs-lookup"><span data-stu-id="790ea-148">It also makes the controller easier to unit test [Testing Controller Logic](testing.md), since there's no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
