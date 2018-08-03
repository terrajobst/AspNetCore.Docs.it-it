---
title: Schema di repository con ASP.NET Core
author: ardalis
description: Informazioni su come implementare lo schema progettuale del repository in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342631"
---
# <a name="repository-pattern-with-aspnet-core"></a><span data-ttu-id="9b3b0-103">Schema di repository con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b3b0-103">Repository pattern with ASP.NET Core</span></span>

<span data-ttu-id="9b3b0-104">Di [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9b3b0-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9b3b0-105">Lo *schema di repository* è uno schema progettuale che consente di isolare l'accesso ai dati dietro astrazioni dell'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-105">The *repository pattern* is a design pattern that isolates data access behind interface abstractions.</span></span> <span data-ttu-id="9b3b0-106">La connessione al database e la modifica degli oggetti di archiviazione dei dati vengono eseguite tramite i metodi forniti dall'implementazione dell'interfaccia.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-106">Connecting to the database and manipulating data storage objects is performed through methods provided by the interface's implementation.</span></span> <span data-ttu-id="9b3b0-107">Di conseguenza, non è necessario chiamare codice per gestire le esigenze del database, ad esempio connessioni, comandi e lettori.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-107">Consequently, there's no need for calling code to deal with database concerns, such as connections, commands, and readers.</span></span>

<span data-ttu-id="9b3b0-108">L'implementazione dello schema di repository con ASP.NET Core offre i vantaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b3b0-108">Implementation of the repository pattern with ASP.NET Core has the following benefits:</span></span>

* <span data-ttu-id="9b3b0-109">L'organizzazione dell'app è meno complessa, senza interdipendenze dirette tra il livello aziendale e il livello di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-109">Organization of the app is less complex with no direct interdependency between the business and data access layers.</span></span>
* <span data-ttu-id="9b3b0-110">È più semplice riutilizzare il codice di accesso del database perché è gestito in modo centralizzato da uno o più repository.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-110">It's easier to reuse database access code because the code is centrally managed by one or more repositories.</span></span>
* <span data-ttu-id="9b3b0-111">È possibile eseguire unit test del dominio aziendale in modo indipendente dal livello del database.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-111">The business domain can be independently unit tested from the database layer.</span></span>

<span data-ttu-id="9b3b0-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9b3b0-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9b3b0-113">L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) usa lo schema del repository per inizializzare e visualizzare un elenco di nomi di personaggi di film.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-113">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) uses the repository pattern to initialize and display a list of movie character names.</span></span> <span data-ttu-id="9b3b0-114">L'app usa [Entity Framework Core](/ef/core/) e una classe `ApplicationDbContext` per la persistenza dei dati, ma l'infrastruttura di database non è evidente nella posizione di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-114">The app uses [Entity Framework Core](/ef/core/) and an `ApplicationDbContext` class for its data persistence, but the database infrastructure isn't apparent where the data is accessed.</span></span> <span data-ttu-id="9b3b0-115">Per gli oggetti di accesso ai dati e di database è prevista l'astrazione dietro un [repository](https://martinfowler.com/eaaCatalog/repository.html).</span><span class="sxs-lookup"><span data-stu-id="9b3b0-115">Data access and database objects are abstracted behind a [repository](https://martinfowler.com/eaaCatalog/repository.html).</span></span>

## <a name="repository-interface"></a><span data-ttu-id="9b3b0-116">Interfaccia del repository</span><span class="sxs-lookup"><span data-stu-id="9b3b0-116">Repository interface</span></span>

<span data-ttu-id="9b3b0-117">Un'interfaccia del repository definisce le proprietà e i metodi per l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-117">A repository interface defines the properties and methods for implementation.</span></span> <span data-ttu-id="9b3b0-118">Nell'app di esempio, l'interfaccia del repository per i dati sui personaggi dei film è `ICharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-118">In the sample app, the repository interface for movie character data is `ICharacterRepository`.</span></span> <span data-ttu-id="9b3b0-119">`ICharacterRepository` definisce i metodi `ListAll` e `Add` necessari per utilizzare le istanze di `Character` nell'app:</span><span class="sxs-lookup"><span data-stu-id="9b3b0-119">`ICharacterRepository` defines the `ListAll` and `Add` methods required to work with `Character` instances in the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="9b3b0-120">L'oggetto `Character` viene definito come:</span><span class="sxs-lookup"><span data-stu-id="9b3b0-120">`Character` is defined as:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a><span data-ttu-id="9b3b0-121">Tipo concreto del repository</span><span class="sxs-lookup"><span data-stu-id="9b3b0-121">Repository concrete type</span></span>

<span data-ttu-id="9b3b0-122">L'interfaccia viene implementata da un tipo concreto.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-122">The interface is implemented by a concrete type.</span></span> <span data-ttu-id="9b3b0-123">Nell'app di esempio, `CharacterRepository` gestisce il contesto del database e implementa i metodi `ListAll` e `Add`:</span><span class="sxs-lookup"><span data-stu-id="9b3b0-123">In the sample app, `CharacterRepository` manages the database context and implements the `ListAll` and `Add` methods:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a><span data-ttu-id="9b3b0-124">Registrare il servizio del repository</span><span class="sxs-lookup"><span data-stu-id="9b3b0-124">Register the repository service</span></span>

<span data-ttu-id="9b3b0-125">Il repository e il contesto del database vengono registrati nel contenitore dei servizi in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-125">The repository and database context are registered with the service container in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9b3b0-126">Nell'app di esempio, `ApplicationDbContext` è configurato con la chiamata al metodo di estensione [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span><span class="sxs-lookup"><span data-stu-id="9b3b0-126">In the sample app, `ApplicationDbContext` is configured with the call to the extension method [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span> <span data-ttu-id="9b3b0-127">`ICharacterRepository` viene registrato come servizio con ambito:</span><span class="sxs-lookup"><span data-stu-id="9b3b0-127">`ICharacterRepository` is registered as a scoped service:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a><span data-ttu-id="9b3b0-128">Inserire un'istanza del repository</span><span class="sxs-lookup"><span data-stu-id="9b3b0-128">Inject an instance of the repository</span></span>

<span data-ttu-id="9b3b0-129">In una classe in cui è richiesto l'accesso al database, un'istanza del repository viene richiesta tramite il costruttore e assegnata a un campo privato per l'uso da parte dei metodi della classe.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-129">In a class where database access is required, an instance of the repository is requested via the constructor and assigned to a private field for use by class methods.</span></span> <span data-ttu-id="9b3b0-130">Nell'app di esempio, `ICharacterRepository` viene usato per:</span><span class="sxs-lookup"><span data-stu-id="9b3b0-130">In the sample app, `ICharacterRepository` is used to:</span></span>

* <span data-ttu-id="9b3b0-131">Popolare il database se non esiste alcun personaggio.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-131">Populate the database if no characters exist.</span></span>
* <span data-ttu-id="9b3b0-132">Ottenere un elenco dei personaggi per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-132">Obtain a list of the characters for display.</span></span>

<span data-ttu-id="9b3b0-133">Si noti come il codice chiamante interagisca solo con l'implementazione dell'interfaccia, `CharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-133">Notice how the calling code only interacts with the interface's implementation, `CharacterRepository`.</span></span> <span data-ttu-id="9b3b0-134">Il codice chiamante non usa `ApplicationDbContext` direttamente:</span><span class="sxs-lookup"><span data-stu-id="9b3b0-134">Calling code doesn't use the `ApplicationDbContext` directly:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a><span data-ttu-id="9b3b0-135">Interfaccia del repository generica</span><span class="sxs-lookup"><span data-stu-id="9b3b0-135">Generic repository interface</span></span>

<span data-ttu-id="9b3b0-136">Questo argomento e la relativa app di esempio illustrano l'implementazione più semplice dello schema del repository, in cui viene creato un repository per ogni oggetto business.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-136">This topic and its sample app demonstrate the simplest implementation of the repository pattern, where one repository is created for each business object.</span></span> <span data-ttu-id="9b3b0-137">Se l'app comprende più di pochi oggetti, un'*interfaccia del repository generica* potrebbe ridurre la quantità di codice necessaria per implementare lo schema del repository.</span><span class="sxs-lookup"><span data-stu-id="9b3b0-137">If the app grows beyond a few objects, a *generic repository interface* may reduce the amount of code required to implement the repository pattern.</span></span> <span data-ttu-id="9b3b0-138">Per altre informazioni, vedere [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/) (DevIQ: Schema del repository: Interfaccia del repository generica).</span><span class="sxs-lookup"><span data-stu-id="9b3b0-138">For more information, see [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b3b0-139">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9b3b0-139">Additional resources</span></span>

* <span data-ttu-id="9b3b0-140">[DevIQ: Repository Pattern](https://deviq.com/repository-pattern/) (DevIQ: Schema del repository)</span><span class="sxs-lookup"><span data-stu-id="9b3b0-140">[DevIQ: Repository Pattern](https://deviq.com/repository-pattern/)</span></span>
* [<span data-ttu-id="9b3b0-141">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="9b3b0-141">Dependency injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="9b3b0-142">Inserimento di dipendenze in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="9b3b0-142">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
* [<span data-ttu-id="9b3b0-143">Inserimento di dipendenze in controller</span><span class="sxs-lookup"><span data-stu-id="9b3b0-143">Dependency injection into controllers</span></span>](xref:mvc/controllers/dependency-injection)
* [<span data-ttu-id="9b3b0-144">Inserimento di dipendenze nei gestori di requisiti</span><span class="sxs-lookup"><span data-stu-id="9b3b0-144">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
* <span data-ttu-id="9b3b0-145">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (Contenitori di inversione del controllo e schema di inserimento delle dipendenze)</span><span class="sxs-lookup"><span data-stu-id="9b3b0-145">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html)</span></span>
