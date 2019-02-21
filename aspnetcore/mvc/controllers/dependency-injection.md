---
title: Inserimento di dipendenze in controller in ASP.NET Core
author: ardalis
description: Informazioni su come i controller ASP.NET Core MVC richiedono le proprie dipendenze in modo esplicito tramite i rispettivi costruttori, usando l'inserimento delle dipendenze in ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 9d9d0a68927da62fad8df72c868eaf4b8ada440d
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410271"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="c716f-103">Inserimento di dipendenze in controller in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c716f-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="c716f-104">Di [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c716f-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c716f-105">I controller ASP.NET Core MVC devono richiedere le proprie dipendenze in modo esplicito tramite i rispettivi costruttori.</span><span class="sxs-lookup"><span data-stu-id="c716f-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="c716f-106">In alcune istanze, singole azioni di controller possono richiedere un servizio che potrebbe non avere senso richiedere a livello di controller.</span><span class="sxs-lookup"><span data-stu-id="c716f-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="c716f-107">In questo caso, è anche possibile inserire un servizio come parametro nel metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="c716f-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="c716f-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c716f-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="c716f-109">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="c716f-109">Dependency Injection</span></span>

<span data-ttu-id="c716f-110">ASP.NET Core dispone del supporto incorporato dell'[inserimento di dipendenze](../../fundamentals/dependency-injection.md), rendendo le applicazioni più facili da testare e gestire.</span><span class="sxs-lookup"><span data-stu-id="c716f-110">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="c716f-111">Inserimento di costruttori</span><span class="sxs-lookup"><span data-stu-id="c716f-111">Constructor Injection</span></span>

<span data-ttu-id="c716f-112">Il supporto incorporato di ASP.NET Core per l'inserimento di dipendenze basato sul costruttore si estende ai controller MVC.</span><span class="sxs-lookup"><span data-stu-id="c716f-112">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="c716f-113">Aggiungendo semplicemente un tipo di servizio al controller come parametro del costruttore, ASP.NET Core tenta di risolvere il tipo corrispondente tramite il proprio contenitore di servizi incorporato.</span><span class="sxs-lookup"><span data-stu-id="c716f-113">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="c716f-114">In genere, ma non sempre, i servizi vengono definiti tramite interfacce.</span><span class="sxs-lookup"><span data-stu-id="c716f-114">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="c716f-115">Se, ad esempio, la logica di business dell'applicazione dipende dall'ora corrente, è possibile inserire un servizio che recuperi l'ora, anziché impostarlo come hardcoded. Ciò consente di superare i test nelle implementazioni che usano un'ora impostata.</span><span class="sxs-lookup"><span data-stu-id="c716f-115">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="c716f-116">L'implementazione di un'interfaccia come questa che usi l'orologio di sistema in fase di runtime è semplice:</span><span class="sxs-lookup"><span data-stu-id="c716f-116">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="c716f-117">A questo punto è possibile usare il servizio nel controller.</span><span class="sxs-lookup"><span data-stu-id="c716f-117">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="c716f-118">In questo caso, è stata aggiunta al metodo `HomeController` `Index` la logica che consente di visualizzare un saluto a seconda dell'ora del giorno.</span><span class="sxs-lookup"><span data-stu-id="c716f-118">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="c716f-119">Se si esegue l'applicazione ora, molto probabilmente si verifica un errore:</span><span class="sxs-lookup"><span data-stu-id="c716f-119">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="c716f-120">Questo errore si verifica se non è stato configurato alcun servizio nel metodo `ConfigureServices` della classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c716f-120">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="c716f-121">Per specificare che le richieste di `IDateTime` devono essere risolte tramite un'istanza di `SystemDateTime`, aggiungere la riga evidenziata nel codice seguente al metodo `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c716f-121">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="c716f-122">Questo servizio specifico può essere implementato usando una qualsiasi delle diverse opzioni di durata (`Transient`, `Scoped` o `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="c716f-122">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="c716f-123">Per comprendere in che modo ognuna di queste opzioni di ambito influisca sul comportamento del servizio, vedere [Inserimento di dipendenze](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="c716f-123">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="c716f-124">Dopo che il servizio è stato configurato, se si esegue l'applicazione e si passa alla home page viene visualizzato il messaggio basato sull'ora, come previsto:</span><span class="sxs-lookup"><span data-stu-id="c716f-124">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Saluto del server](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="c716f-126">Vedere [Test della logica dei controller](testing.md) per informazioni su come semplificare il test del codice richiedendo in modo esplicito le dipendenze nei controller.</span><span class="sxs-lookup"><span data-stu-id="c716f-126">See [Test controller logic](testing.md) to learn how to make code easier to test by explicitly requesting dependencies in controllers.</span></span>

<span data-ttu-id="c716f-127">L'inserimento di dipendenze predefinito in ASP.NET Core supporta un unico costruttore per la richiesta di servizi da parte delle classi.</span><span class="sxs-lookup"><span data-stu-id="c716f-127">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="c716f-128">Se ci sono più costruttori, può essere visualizzata l'eccezione seguente:</span><span class="sxs-lookup"><span data-stu-id="c716f-128">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="c716f-129">Come indicato nel messaggio di errore, è possibile correggere questo problema usando un unico costruttore.</span><span class="sxs-lookup"><span data-stu-id="c716f-129">As the error message states, you can correct this problem with the use of a single constructor.</span></span> <span data-ttu-id="c716f-130">È anche possibile [sostituire il contenitore di inserimento delle dipendenze predefinito con un'implementazione di terze parti](xref:fundamentals/dependency-injection#default-service-container-replacement). Molte di queste implementazioni supportano più costruttori.</span><span class="sxs-lookup"><span data-stu-id="c716f-130">You can also [replace the default dependency injection container with a third party implementation](xref:fundamentals/dependency-injection#default-service-container-replacement), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="c716f-131">Inserimento di azioni con FromServices</span><span class="sxs-lookup"><span data-stu-id="c716f-131">Action Injection with FromServices</span></span>

<span data-ttu-id="c716f-132">Talvolta un servizio è necessario per un sola azione all'interno del controller.</span><span class="sxs-lookup"><span data-stu-id="c716f-132">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="c716f-133">In questo caso, può avere senso inserire il servizio come parametro nel metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="c716f-133">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="c716f-134">Per eseguire questa operazione, si contrassegna il parametro con l'attributo `[FromServices]`, come illustrato qui:</span><span class="sxs-lookup"><span data-stu-id="c716f-134">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="c716f-135">Accesso alle impostazioni da un controller</span><span class="sxs-lookup"><span data-stu-id="c716f-135">Accessing Settings from a Controller</span></span>

<span data-ttu-id="c716f-136">L'accesso alle impostazioni di un'applicazione o alle impostazioni di configurazione da un controller è un tipo di azione comune.</span><span class="sxs-lookup"><span data-stu-id="c716f-136">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="c716f-137">Questo tipo di accesso deve usare il modello di opzioni descritto in [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="c716f-137">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="c716f-138">In genere non si devono richiedere impostazioni direttamente dal controller tramite l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="c716f-138">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="c716f-139">Un approccio migliore consiste nella richiesta di un'istanza di `IOptions<T>`, dove `T` è la classe di configurazione necessaria.</span><span class="sxs-lookup"><span data-stu-id="c716f-139">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="c716f-140">Per usare il modello di opzioni, è necessario creare una classe che rappresenti le opzioni, ad esempio questa:</span><span class="sxs-lookup"><span data-stu-id="c716f-140">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="c716f-141">È quindi necessario configurare l'applicazione in modo che usi il modello di opzioni e aggiungere la classe di configurazione alla raccolta dei servizi in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c716f-141">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="c716f-142">Il codice precedente configura l'applicazione in modo che legga le impostazioni da un file in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="c716f-142">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="c716f-143">È anche possibile configurare interamente le impostazioni nel codice, come illustrato nel codice commentato precedente.</span><span class="sxs-lookup"><span data-stu-id="c716f-143">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="c716f-144">Per altre opzioni di configurazione, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="c716f-144">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="c716f-145">Dopo aver specificato un oggetto di configurazione fortemente tipizzato (in questo caso, `SampleWebSettings`) e dopo averlo aggiunto alla raccolta di servizi, è possibile richiederlo da qualsiasi controller o metodo di azione richiedendo un'istanza di `IOptions<T>` (in questo caso, `IOptions<SampleWebSettings>`).</span><span class="sxs-lookup"><span data-stu-id="c716f-145">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="c716f-146">Il codice seguente illustra come richiedere le impostazioni da un controller:</span><span class="sxs-lookup"><span data-stu-id="c716f-146">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="c716f-147">Se si segue il modello di opzioni, è possibile disaccoppiare le impostazioni e la configurazione. Ciò garantisce che il controller segua il principio della [separazione delle competenze](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns), poiché non deve sapere come o dove trovare le informazioni sulle impostazioni.</span><span class="sxs-lookup"><span data-stu-id="c716f-147">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="c716f-148">È anche più facile eseguire [unit test](testing.md) del controller perché non è prevista la creazione diretta di istanze delle classi di impostazioni all'interno della classe del controller.</span><span class="sxs-lookup"><span data-stu-id="c716f-148">It also makes the controller easier to [unit test](testing.md), since there's no direct instantiation of settings classes within the controller class.</span></span>
