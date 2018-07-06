---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Inserimento di dipendenze di ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Nota: In questo laboratorio pratico presuppone che conoscenze di base dei filtri ASP.NET MVC e ASP.NET MVC 4. Se si utilizzano i filtri ASP.NET MVC 4 prima di, rec...'
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 44c8f2055fb62d589e874683cbf43eed87a8c447
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812342"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="cdc0f-104">Inserimento di dipendenze di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="cdc0f-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="cdc0f-105">da [Camp Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="cdc0f-106">Download Web Camp Kit di formazione</span><span class="sxs-lookup"><span data-stu-id="cdc0f-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="cdc0f-107">Questa pratica si presuppone conoscenze di base **ASP.NET MVC** e **filtri ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="cdc0f-108">Se non è stato utilizzato **filtri ASP.NET MVC 4** in precedenza, è consigliabile esaminare **filtri azione personalizzati di ASP.NET MVC** laboratorio pratico.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="cdc0f-109">Tutto il codice di esempio e frammenti di codice sono inclusi nel Web Camp Kit di formazione, disponibile in [Microsoft-Web/WebCampTrainingKit versioni](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="cdc0f-110">Il progetto specifico per questo lab è disponibile all'indirizzo [inserimento delle dipendenze di ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="cdc0f-111">Nelle **programmazione orientata a oggetti di oggetto** paradigma, tali oggetti interagiscono in un modello per la collaborazione in cui sono presenti i collaboratori e utenti.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="cdc0f-112">Naturalmente, questo modello di comunicazione genera dipendenze tra oggetti e componenti, diventare difficile da gestire quando si aumenta la complessità.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="cdc0f-113">![Classe dipendenze e la complessità del modello](aspnet-mvc-4-dependency-injection/_static/image1.png "classe dipendenze e la complessità del modello")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="cdc0f-114">*Le dipendenze di classe e la complessità del modello*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="cdc0f-115">Probabile che abbiano già sentito parlare di **modello di Factory** e la separazione tra l'interfaccia e l'implementazione tramite servizi, in cui gli oggetti client sono spesso responsabili della posizione del servizio.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="cdc0f-116">Il modello di inserimento delle dipendenze è una particolare implementazione di inversione del controllo.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="cdc0f-117">**Inversione di controllo (IoC)** significa che gli oggetti non creano altri oggetti su cui si basano per svolgere il proprio lavoro.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="cdc0f-118">Al contrario, ricevono gli oggetti necessari da un'origine esterna (ad esempio, un file di configurazione xml).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="cdc0f-119">**Inserimento delle dipendenze** significa che questa viene eseguita senza l'intervento da parte dell'oggetto, in genere da un componente di framework che passa i parametri del costruttore e impostare le proprietà.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="cdc0f-120">Lo schema progettuale di inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="cdc0f-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="cdc0f-121">A livello generale, l'obiettivo di inserimento delle dipendenze è che una classe client (ad esempio *dei giocatori di golf*) richiede un elemento che soddisfa un'interfaccia (ad esempio *IClub*).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="cdc0f-122">Non è rilevante che cos'è il tipo concreto (ad esempio *WoodClub, IronClub, WedgeClub* o *PutterClub*), vuole che qualcun altro che presentavano (ad esempio, una buona *caddy*).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="cdc0f-123">Il Resolver delle dipendenze in ASP.NET MVC in modo da poter registrare la logica di dipendenza in un'altra posizione (ad esempio, un contenitore o un *contenitore di fiori*).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="cdc0f-124">![Diagramma di inserimento delle dipendenze](aspnet-mvc-4-dependency-injection/_static/image2.png "illustrazione di inserimento delle dipendenze")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="cdc0f-125">*Inserimento delle dipendenze - analogia Golf*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="cdc0f-126">Vantaggi dell'uso di modello di inserimento delle dipendenze e inversione di controllo sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="cdc0f-127">Consente di ridurre di accoppiamenti di classi</span><span class="sxs-lookup"><span data-stu-id="cdc0f-127">Reduces class coupling</span></span>
- <span data-ttu-id="cdc0f-128">Aumenta il riutilizzo di codice</span><span class="sxs-lookup"><span data-stu-id="cdc0f-128">Increases code reusing</span></span>
- <span data-ttu-id="cdc0f-129">Migliora la gestibilità del codice</span><span class="sxs-lookup"><span data-stu-id="cdc0f-129">Improves code maintainability</span></span>
- <span data-ttu-id="cdc0f-130">Migliora la verifica delle applicazioni</span><span class="sxs-lookup"><span data-stu-id="cdc0f-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="cdc0f-131">Inserimento di dipendenze in alcuni casi viene confrontato con schema progettuale Factory astratta, ma è presente una leggera differenza tra entrambi gli approcci.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="cdc0f-132">L'inserimento delle dipendenze è un Framework lavoro dietro a risolvere le dipendenze chiamando le factory e i servizi registrati.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="cdc0f-133">Dopo avere appreso il modello di inserimento delle dipendenze, imparerai in tutto questo lab di applicarlo in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="cdc0f-134">Si inizierà con inserimento delle dipendenze nel **controller** per includere un servizio di accesso ai database.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="cdc0f-135">Successivamente, si applicherà l'inserimento delle dipendenze per il **viste** per utilizzare un servizio e visualizzare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="cdc0f-136">Infine, si estenderà l'inserimento delle dipendenze per i filtri di ASP.NET MVC 4, inserimento di un filtro azioni personalizzato nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="cdc0f-137">In questo laboratorio pratico, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="cdc0f-138">Integrazione di ASP.NET MVC 4 con Unity per l'inserimento delle dipendenze tramite pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="cdc0f-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="cdc0f-139">Usare l'inserimento delle dipendenze all'interno di un Controller MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cdc0f-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="cdc0f-140">Usare l'inserimento delle dipendenze all'interno di una visualizzazione ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="cdc0f-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="cdc0f-141">Usare l'inserimento delle dipendenze all'interno di un filtro di azione MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cdc0f-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="cdc0f-142">Questa esercitazione Usa pacchetto NuGet Unity.Mvc3 per la risoluzione delle dipendenze, ma è possibile adattare qualsiasi Framework di inserimento delle dipendenze per lavorare con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="cdc0f-143">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="cdc0f-143">Prerequisites</span></span>

<span data-ttu-id="cdc0f-144">Sono necessari gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="cdc0f-145">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (leggere [appendice A](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="cdc0f-146">Configurazione</span><span class="sxs-lookup"><span data-stu-id="cdc0f-146">Setup</span></span>

<span data-ttu-id="cdc0f-147">**Installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="cdc0f-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="cdc0f-148">Per praticità, gran parte del codice che vengono gestiti insieme questo lab è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="cdc0f-149">Per installare i frammenti di codice eseguiti **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="cdc0f-150">Se non ha familiarità con i frammenti di codice di Visual Studio e si vuole imparare a usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice b: Using Code Snippets](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="cdc0f-151">Esercizi</span><span class="sxs-lookup"><span data-stu-id="cdc0f-151">Exercises</span></span>

<span data-ttu-id="cdc0f-152">In questo laboratorio pratico include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="cdc0f-153">Esercizio 1: Inserimento di un Controller</span><span class="sxs-lookup"><span data-stu-id="cdc0f-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="cdc0f-154">Esercizio 2: Inserimento di una vista</span><span class="sxs-lookup"><span data-stu-id="cdc0f-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="cdc0f-155">Esercizio 3: Inserimento di filtri</span><span class="sxs-lookup"><span data-stu-id="cdc0f-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="cdc0f-156">Ogni esercizio è accompagnato da un **End** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato gli esercizi.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="cdc0f-157">Se ti serve assistenza aggiuntiva esaminando gli esercizi, è possibile usare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="cdc0f-158">Tempo stimato per completare questa esercitazione: **30 minuti**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="cdc0f-159">Esercizio 1: Inserimento di un Controller</span><span class="sxs-lookup"><span data-stu-id="cdc0f-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="cdc0f-160">In questo esercizio, si apprenderà come usare l'inserimento di dipendenze in ASP.NET MVC Controllers grazie all'integrazione con un pacchetto NuGet di Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="cdc0f-161">Per questo motivo, si includerà servizi nei controller MvcMusicStore per separare la logica di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="cdc0f-162">I servizi creerà una nuova dipendenza nel costruttore del controller, che verrà risolto con l'aiuto di inserimento delle dipendenze **Unity**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="cdc0f-163">Questo approccio illustrerà come generare meno applicazioni a cui sono più flessibili e facili da gestire e testare.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="cdc0f-164">Si apprenderà anche come integrare ASP.NET MVC con Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="cdc0f-165">Informazioni sul servizio StoreManager</span><span class="sxs-lookup"><span data-stu-id="cdc0f-165">About StoreManager Service</span></span>

<span data-ttu-id="cdc0f-166">La Store musica MVC fornito nella soluzione begin ora include un servizio che gestisce i dati di Store Controller denominati **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="cdc0f-167">Di seguito si noterà che l'implementazione del servizio Store.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="cdc0f-168">Si noti che tutti i metodi di restituiscono le entità del modello.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="cdc0f-169">**StoreController** dall'inizio soluzione utilizza ora **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="cdc0f-170">Tutti i riferimenti di dati sono stati rimossi da **StoreController**e a questo punto è possibile modificare il provider di accesso ai dati corrente senza modificare qualsiasi metodo che utilizza **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="cdc0f-171">Si noterà che il **StoreController** implementazione presenta una dipendenza con **StoreService** all'interno del costruttore di classe.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="cdc0f-172">La dipendenza presentata in questo esercizio è correlata a **Inversion of Control** (IoC).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="cdc0f-173">Il **StoreController** costruttore di classe riceve un' **IStoreService** parametro di tipo, è essenziale eseguire chiamate al servizio all'interno della classe.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="cdc0f-174">Tuttavia **StoreController** non implementa il costruttore predefinito (senza alcun parametro) che qualsiasi controller è necessario per lavorare con ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="cdc0f-175">Per risolvere la dipendenza, il controller deve essere creato da una factory astratta (una classe che restituisce qualsiasi oggetto del tipo specificato).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="cdc0f-176">Si otterrà un errore quando la classe tenta di creare il StoreController senza inviare l'oggetto di servizio, come non vi è alcun costruttore senza parametri dichiarati.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="cdc0f-177">Attività 1: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="cdc0f-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="cdc0f-178">In questa attività si eseguirà l'applicazione iniziale, che include il servizio nel Controller del Store che separa l'accesso ai dati dalla logica dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="cdc0f-179">Quando si esegue l'applicazione, si riceverà un'eccezione, come il servizio controller non viene passato come parametro per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="cdc0f-180">Aprire il **Begin** soluzione che si trova **inserimento Source\Ex01 Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="cdc0f-181">È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="cdc0f-182">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="cdc0f-183">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="cdc0f-184">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="cdc0f-185">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="cdc0f-186">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="cdc0f-187">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="cdc0f-188">Premere **Ctrl + F5** per eseguire l'applicazione senza eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="cdc0f-189">Verrà visualizzato il messaggio di errore &quot; **alcun costruttore senza parametri per questo oggetto definito**&quot;:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="cdc0f-190">![Errore durante l'esecuzione dell'applicazione ASP.NET MVC iniziano](aspnet-mvc-4-dependency-injection/_static/image3.png "errore durante l'esecuzione dell'applicazione ASP.NET MVC Begin")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="cdc0f-191">*Errore durante l'esecuzione dell'applicazione ASP.NET MVC Begin*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="cdc0f-192">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-192">Close the browser.</span></span>

<span data-ttu-id="cdc0f-193">Nei passaggi seguenti funzionerà sulla soluzione Music Store per inserire la dipendenza da che questo controller è necessaria.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="cdc0f-194">Attività 2 - Unity inclusi nella soluzione MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="cdc0f-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="cdc0f-195">In questa attività includerà **Unity.Mvc3** pacchetto NuGet per la soluzione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="cdc0f-196">Pacchetto Unity.Mvc3 è stata progettata per ASP.NET MVC 3, ma è completamente compatibile con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="cdc0f-197">Unity è ad esempio un contenitore di inserimento delle dipendenze leggera ed estendibile con supporto facoltativo e intercettazione di tipo.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="cdc0f-198">È un contenitore per utilizzo generico per l'uso in qualsiasi tipo di applicazione .NET.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="cdc0f-199">Fornisce tutte le caratteristiche comuni presenti in meccanismi di inserimento delle dipendenze tra cui: creazione di oggetti, l'astrazione dei requisiti, specificando dipendenze in fase di esecuzione e la flessibilità, rinviando la configurazione del componente per il contenitore.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="cdc0f-200">Installare **Unity.Mvc3** pacchetto NuGet nel **MvcMusicStore** progetto.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="cdc0f-201">A tale scopo, aprire il **Console di gestione pacchetti** dalla **View** | **Other Windows**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="cdc0f-202">Eseguire il seguente comando.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-202">Run the following command.</span></span>

    <span data-ttu-id="cdc0f-203">PMC</span><span class="sxs-lookup"><span data-stu-id="cdc0f-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="cdc0f-204">![Installare il pacchetto NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "installare il pacchetto NuGet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="cdc0f-205">*Installare il pacchetto NuGet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="cdc0f-206">Una volta il **Unity.Mvc3** pacchetto viene installato, esplorare i file e cartelle vengono automaticamente aggiunti per semplificare la configurazione di Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="cdc0f-207">![Pacchetto Unity.Mvc3 installata](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 pacchetto installato")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="cdc0f-208">*Pacchetto Unity.Mvc3 installato*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="cdc0f-209">Attività 3: registrazione di Unity in Global.asax.cs applicazione\_Start</span><span class="sxs-lookup"><span data-stu-id="cdc0f-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="cdc0f-210">In questa attività si aggiornerà il **Application\_avviare** metodo che si trova **Global.asax.cs** per chiamare l'inizializzatore del programma di bootstrap di Unity e quindi aggiornare la registrazione di file di programma di avvio automatico il servizio e il Controller che verrà usato per l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="cdc0f-211">A questo punto, si collegherà il programma di avvio che è il file che inizializza il contenitore di Unity e Resolver di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="cdc0f-212">A tale scopo, aprire **Global.asax.cs** e aggiungere il codice evidenziato seguente all'interno di **Application\_avviare** (metodo).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="cdc0f-213">(Code - Snippet *ASP.NET Dependency Injection Lab - Ex01 - inizializzare Unity*)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="cdc0f-214">Aprire **Bootstrapper.cs** file.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="cdc0f-215">Includere gli spazi dei nomi seguenti: **MvcMusicStore.Services** e **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="cdc0f-216">(Code - Snippet *ASP.NET Dependency Injection Lab - Ex01 - programma di avvio aggiunta degli spazi dei nomi*)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="cdc0f-217">Sostituire **BuildUnityContainer** del contenuto con il codice seguente che esegue la registrazione servizio Store e Store Controller metodo.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="cdc0f-218">(Code - Snippet *ASP.NET Dependency Injection Lab - Ex01 - Store Register Controller e il servizio*)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="cdc0f-219">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="cdc0f-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="cdc0f-220">In questa attività si eseguirà l'applicazione per verificare che può ora essere caricato dopo l'inclusione di Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="cdc0f-221">Premere **F5** per eseguire l'applicazione, l'applicazione dovrebbe ora caricare senza visualizzare alcun messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="cdc0f-222">![Esecuzione dell'applicazione con inserimento delle dipendenze](aspnet-mvc-4-dependency-injection/_static/image6.png "applicazione eseguita con inserimento delle dipendenze")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="cdc0f-223">*Applicazione in esecuzione con inserimento delle dipendenze*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="cdc0f-224">Passare a **/Store**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-224">Browse to **/Store**.</span></span> <span data-ttu-id="cdc0f-225">Questa operazione richiamerà **StoreController**, che è ora possibile creare utilizzando **Unity**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="cdc0f-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="cdc0f-227">*MVC Music Store*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="cdc0f-228">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-228">Close the browser.</span></span>

<span data-ttu-id="cdc0f-229">Negli esercizi seguenti si apprenderà come estendere l'ambito di inserimento delle dipendenze per utilizzarla all'interno delle visualizzazioni ASP.NET MVC e i filtri di azione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="cdc0f-230">Esercizio 2: Inserimento di una vista</span><span class="sxs-lookup"><span data-stu-id="cdc0f-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="cdc0f-231">In questo esercizio, si apprenderà come usare l'inserimento di dipendenze in una vista con le nuove funzionalità di ASP.NET MVC 4 per l'integrazione con Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="cdc0f-232">Per farlo, si chiamerà un servizio personalizzato all'interno di Store esplorazione della vista, che viene visualizzato un messaggio e un'immagine riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="cdc0f-233">Si verrà quindi integrare il progetto con Unity e creare un resolver di dipendenza personalizzate per l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="cdc0f-234">Attività 1: creazione di una vista che utilizza un servizio</span><span class="sxs-lookup"><span data-stu-id="cdc0f-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="cdc0f-235">In questa attività si creerà una vista che esegue una chiamata al servizio per generare una nuova dipendenza.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="cdc0f-236">Il servizio presuppone l'uso in un servizio di messaggistica semplice incluso in questa soluzione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="cdc0f-237">Aprire il **Begin** soluzione che si trova nel **inserimento Source\Ex02 View\Begin** cartella.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="cdc0f-238">In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="cdc0f-239">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="cdc0f-240">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="cdc0f-241">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="cdc0f-242">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="cdc0f-243">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="cdc0f-244">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="cdc0f-245">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="cdc0f-246">Per altre informazioni, vedere questo articolo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="cdc0f-247">Includono il **MessageService.cs** e il **IMessageService.cs** classi che si trovano nel **\Assets dell'origine** cartella **/servizi**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="cdc0f-248">A tale scopo, fare doppio clic su **Services** cartella e selezionare **Aggiungi elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="cdc0f-249">Passare al percorso dei file e includerli.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="cdc0f-250">![Aggiunta di Message Service e interfaccia di servizio](aspnet-mvc-4-dependency-injection/_static/image8.png "aggiunta Message Service e l'interfaccia di servizio")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="cdc0f-251">*Aggiunta del servizio di messaggi e l'interfaccia di servizio*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="cdc0f-252">Il **IMessageService** interfaccia definisce due proprietà implementata dalle **messaggio** classe.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="cdc0f-253">Queste proprietà:**messaggi** e **ImageUrl**-memorizzare il messaggio e l'URL dell'immagine da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="cdc0f-254">Creare la cartella **/Pages** cartella radice del progetto e quindi aggiungere la classe esistente **MyBasePage.cs** dalla **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="cdc0f-255">Pagina base che è erediterà dalla presenta la struttura seguente.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="cdc0f-256">![Cartella delle pagine](aspnet-mvc-4-dependency-injection/_static/image9.png "cartella delle pagine")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="cdc0f-257">Aprire **Browse.cshtml** visualizzare dal **/viste/Store** cartella e impostarlo come ereditare **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="cdc0f-258">Nel **esplorare** consente di visualizzare, aggiungere una chiamata a **messaggio** per visualizzare un'immagine e un messaggio recuperato dal servizio.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="cdc0f-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="cdc0f-260">Attività 2 - tra cui un Resolver di dipendenza personalizzata e un attivatore della pagina di visualizzazione personalizzata</span><span class="sxs-lookup"><span data-stu-id="cdc0f-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="cdc0f-261">Nell'attività precedente, inserita una nuova dipendenza all'interno di una vista per eseguire una chiamata al servizio all'interno.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="cdc0f-262">A questo punto, si risolverà tale dipendenza implementando le interfacce di inserimento delle dipendenze di ASP.NET MVC **IViewPageActivator** e **IDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="cdc0f-263">Verranno inclusi nella soluzione un'implementazione di **IDependencyResolver** che gestirà il recupero di servizio con Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="cdc0f-264">Quindi, si includerà un'altra implementazione personalizzata di **IViewPageActivator** interfaccia che consentirà di risolvere la creazione delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="cdc0f-265">Poiché ASP.NET MVC 3, l'implementazione per l'inserimento di dipendenze aveva semplificato le interfacce per registrare i servizi.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="cdc0f-266">**Resolver di dipendenza** e **IViewPageActivator** fanno parte delle funzionalità di ASP.NET MVC 3 per l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="cdc0f-267">**-IDependencyResolver** interfaccia sostituisce il precedente IMvcServiceLocator.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="cdc0f-268">Gli implementatori di resolver di dipendenza devono restituire un'istanza del servizio o una raccolta di servizio.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="cdc0f-269">**-IViewPageActivator** interfaccia fornisce un controllo più accurato sul modo in cui visualizzare le pagine vengono create istanze tramite inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="cdc0f-270">Le classi che implementano **IViewPageActivator** interfaccia può creare istanze di viste mediante le informazioni di contesto.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="cdc0f-271">Creare il /**factory** cartella nella cartella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="cdc0f-272">Includere **CustomViewPageActivator.cs** per la soluzione dall'inizio **/origini/risorse/** al **factory** cartella.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="cdc0f-273">A tale scopo, fare doppio clic il **/Factories** cartella, selezionare **Add | Elemento esistente** e quindi selezionare **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="cdc0f-274">Questa classe implementa il **IViewPageActivator** interface per mantenere il contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="cdc0f-275">**CustomViewPageActivator** ha la responsabilità di gestire la creazione di una vista usando un contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="cdc0f-276">Includere **UnityDependencyResolver.cs** del file da **origini/asset** al **/Factories** cartella.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="cdc0f-277">A tale scopo, fare doppio clic il **/Factories** cartella, selezionare **Add | Elemento esistente** e quindi selezionare **UnityDependencyResolver.cs** file.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="cdc0f-278">**UnityDependencyResolver** classe è un DependencyResolver personalizzato per Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="cdc0f-279">Quando un servizio non viene trovato all'interno del contenitore di Unity, è invocated il resolver di base.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="cdc0f-280">Nell'attività seguente verranno registrate entrambe le implementazioni per consentire al modello di conoscere il percorso dei servizi e le viste.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="cdc0f-281">Attività 3: registrazione per l'inserimento delle dipendenze nel contenitore di Unity</span><span class="sxs-lookup"><span data-stu-id="cdc0f-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="cdc0f-282">In questa attività si inseriranno tutte le operazioni precedenti per far funzionare l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="cdc0f-283">Fino a questo punto la soluzione contiene gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="cdc0f-284">Oggetto **esplorare** vista da cui eredita **MyBaseClass** e utilizza **messaggio**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="cdc0f-285">Una classe intermedia -**MyBaseClass**-con inserimento delle dipendenze dichiarato per l'interfaccia del servizio.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="cdc0f-286">Un servizio - **messaggio** - e la relativa interfaccia **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="cdc0f-287">Un resolver di dipendenza personalizzate per Unity - **UnityDependencyResolver** -che riguarda il recupero del servizio.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="cdc0f-288">Un attivatore della pagina di visualizzazione - **CustomViewPageActivator** -che crea la pagina.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="cdc0f-289">Per inserire **esplorare** visualizzazione, è ora registrerà il resolver di dipendenza personalizzate nel contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="cdc0f-290">Aprire **Bootstrapper.cs** file.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="cdc0f-291">Registrare un'istanza di **messaggio** nel contenitore di Unity per inizializzare il servizio:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="cdc0f-292">(Code - Snippet *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="cdc0f-293">Aggiungere un riferimento a **MvcMusicStore.Factories** dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="cdc0f-294">(Code - Snippet *ASP.NET Dependency Injection Lab - Ex02 - factory Namespace*)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="cdc0f-295">Registrare **CustomViewPageActivator** come un attivatore della pagina di visualizzazione nel contenitore di Unity:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="cdc0f-296">(Code - Snippet *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="cdc0f-297">Sostituire i resolver di dipendenza predefinito di ASP.NET MVC 4 con un'istanza di **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="cdc0f-298">A tale scopo, sostituire **Initialise** metodo contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-298">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="cdc0f-299">(Code - Snippet *Lab - Ex02 - inserimento delle dipendenze ASP.NET Update Resolver di dipendenza*)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="cdc0f-300">ASP.NET MVC fornisce una classe resolver di dipendenza predefinito.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="cdc0f-301">Per utilizzare i resolver di dipendenza personalizzate come quello che è stata creata per unity, il resolver deve essere sostituito.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="cdc0f-302">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="cdc0f-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="cdc0f-303">In questa attività si eseguirà l'applicazione per verificare che il Browser Store Usa il servizio e visualizza l'immagine e il messaggio recuperato:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="cdc0f-304">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="cdc0f-305">Fare clic su **Rock** nel Menu di generi e vedere come il **messaggio** è stata inserita alla visualizzazione e caricato il messaggio di benvenuto e l'immagine.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="cdc0f-306">In questo esempio, si sta attivando per &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="cdc0f-307">![MVC Music Store - vista Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - inserimento della visualizzazione")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="cdc0f-308">*MVC Music Store - inserimento della visualizzazione*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="cdc0f-309">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="cdc0f-310">Esercizio 3: Inserimento filtri azione</span><span class="sxs-lookup"><span data-stu-id="cdc0f-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="cdc0f-311">Nel precedente lab pratici **filtri azione personalizzati** utente abbia familiarità con la personalizzazione di filtri e dell'inserimento.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="cdc0f-312">In questo esercizio, si apprenderà come inserire i filtri con inserimento delle dipendenze mediante il contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="cdc0f-313">A tale scopo, si aggiungerà alla soluzione di Music Store un filtro azioni personalizzato che consente di tenere traccia di attività del sito.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="cdc0f-314">Attività 1: incluso il filtro di rilevamento nella soluzione</span><span class="sxs-lookup"><span data-stu-id="cdc0f-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="cdc0f-315">In questa attività verranno incluse di Music Store un filtro azione personalizzato per gli eventi di traccia.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="cdc0f-316">Come filtro azioni personalizzato concetti siano già considerati nell'ambiente di laboratorio precedente &quot;filtri azione personalizzati&quot;, verrà infatti sufficiente includere la classe di filtro dalla cartella Assets di questa esercitazione e quindi creare un Provider di filtri per Unity:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="cdc0f-317">Aprire il **Begin** soluzione che si trova nel **Source\Ex03 - inserimento azione Filter\Begin** cartella.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="cdc0f-318">In caso contrario, è possibile continuare a usare il **End** soluzione ottenuta completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="cdc0f-319">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="cdc0f-320">A questo scopo, scegliere il **Project** menu e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="cdc0f-321">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="cdc0f-322">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="cdc0f-323">Uno dei vantaggi dell'uso di NuGet è che non è necessario per la spedizione di tutte le librerie nel progetto, ridurre le dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="cdc0f-324">Con gli strumenti avanzati di NuGet, specificando le versioni del pacchetto nel file Packages. config, sarà possibile scaricare tutte le librerie necessarie alla prima che esecuzione del progetto.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="cdc0f-325">Ecco perché è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="cdc0f-326">Per altre informazioni, vedere questo articolo: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="cdc0f-327">Includere **TraceActionFilter.cs** del file da **/origini/asset** al **/Filtra** cartella.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="cdc0f-328">Questo filtro azioni personalizzato esegue l'analisi ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="cdc0f-329">È possibile controllare &quot;ASP.NET MVC 4 locali e i filtri azione dinamici&quot; Lab per altre informazioni di riferimento.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="cdc0f-330">Aggiungere la classe vuota **FilterProvider.cs** al progetto nella cartella   **/filtri.**</span><span class="sxs-lookup"><span data-stu-id="cdc0f-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="cdc0f-331">Aggiungere il **System** e **Microsoft.Practices.Unity** gli spazi dei nomi **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="cdc0f-332">(Code - Snippet *Dependency Injection Lab - Ex03 - filtro del Provider di ASP.NET aggiunta degli spazi dei nomi*)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="cdc0f-333">Rendere la classe ereditare **IFilterProvider** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="cdc0f-334">Aggiungere un **IUnityContainer** proprietà di **FilterProvider** classe e quindi creare un costruttore di classe per assegnare il contenitore.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="cdc0f-335">(Code - Snippet *ASP.NET Dependency Injection Lab - Ex03 - filtro Provider costruttore*)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="cdc0f-336">Il costruttore della classe provider filtro non crea una **nuovo** all'interno dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="cdc0f-337">Il contenitore viene passato come parametro e la dipendenza sia stato risolto da Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="cdc0f-338">Nel **FilterProvider** classe, implementare il metodo **GetFilters** dalla **IFilterProvider** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="cdc0f-339">(Code - Snippet *ASP.NET Dependency Injection Lab - Ex03 - filtro Provider GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="cdc0f-340">Attività 2: registrazione e attivazione del filtro</span><span class="sxs-lookup"><span data-stu-id="cdc0f-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="cdc0f-341">In questa attività si abiliterà il rilevamento di sito.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="cdc0f-342">A tale scopo, è necessario registrare il filtro nella **Bootstrapper.cs BuildUnityContainer** metodo per avviare la registrazione:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="cdc0f-343">Aprire **Web. config** che si trova nella radice del progetto e attivare il rilevamento traccia al gruppo di System. Web.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="cdc0f-344">Aprire **Bootstrapper.cs** alla radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="cdc0f-345">Aggiungere un riferimento per la **MvcMusicStore.Filters** dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="cdc0f-346">(Code - Snippet *ASP.NET Dependency Injection Lab - Ex03 - programma di avvio aggiunta degli spazi dei nomi*)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="cdc0f-347">Selezionare il **BuildUnityContainer** (metodo) e registrare il filtro del contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="cdc0f-348">È necessario registrare il provider di filtri, nonché il filtro delle azioni.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="cdc0f-349">(Code - Snippet *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider e ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="cdc0f-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="cdc0f-350">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="cdc0f-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="cdc0f-351">In questa attività, eseguire l'applicazione e i test verranno che il filtro azioni personalizzato è la traccia dell'attività:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="cdc0f-352">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="cdc0f-353">Fare clic su **Rock** all'interno del Menu di generi.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="cdc0f-354">Se si desidera, è possibile esplorare per altri generi.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="cdc0f-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="cdc0f-356">*Music Store*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-356">*Music Store*</span></span>
3. <span data-ttu-id="cdc0f-357">Passare a **/Trace.axd** per visualizzare la traccia dell'applicazione e quindi fare clic su **Visualizza dettagli**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="cdc0f-358">![Log di traccia dell'applicazione](aspnet-mvc-4-dependency-injection/_static/image12.png "Log di traccia dell'applicazione")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="cdc0f-359">*Log di traccia dell'applicazione*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-359">*Application Trace Log*</span></span>

    <span data-ttu-id="cdc0f-360">![Traccia delle applicazioni - dettagli della richiesta](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - dettagli richiesta")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="cdc0f-361">*Traccia delle applicazioni - dettagli della richiesta*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="cdc0f-362">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-362">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="cdc0f-363">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="cdc0f-363">Summary</span></span>

<span data-ttu-id="cdc0f-364">Completando questa pratica si è appreso come usare l'inserimento delle dipendenze in ASP.NET MVC 4 grazie all'integrazione con un pacchetto NuGet di Unity.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="cdc0f-365">A tale scopo, è stato usato l'inserimento delle dipendenze all'interno di controller, visualizzazioni e filtri dell'azione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="cdc0f-366">Sono stati trattati i concetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="cdc0f-366">The following concepts were covered:</span></span>

- <span data-ttu-id="cdc0f-367">Funzionalità di inserimento delle dipendenze di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="cdc0f-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="cdc0f-368">Integrazione con Unity tramite Unity.Mvc3 Mobileengagement</span><span class="sxs-lookup"><span data-stu-id="cdc0f-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="cdc0f-369">Inserimento delle dipendenze in controller</span><span class="sxs-lookup"><span data-stu-id="cdc0f-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="cdc0f-370">Inserimento delle dipendenze in visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="cdc0f-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="cdc0f-371">Inserimento di dipendenze dei filtri azione</span><span class="sxs-lookup"><span data-stu-id="cdc0f-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="cdc0f-372">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="cdc0f-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="cdc0f-373">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o da un'altra &quot;Express&quot; versione utilizzando il **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="cdc0f-374">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="cdc0f-375">Passare a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-375">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="cdc0f-376">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e cercare il prodotto &quot; <em>Visual Studio Express 2012 per Web con Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="cdc0f-377">Fare clic su **Installa ora**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-377">Click on **Install Now**.</span></span> <span data-ttu-id="cdc0f-378">Se non hai **instalace Webové Platformy** si verrà reindirizzati per scaricarlo e installarlo prima di tutto.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="cdc0f-379">Una volta **instalace Webové Platformy** è aperto, fare clic su **installare** per avviare il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="cdc0f-380">![Installa Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="cdc0f-381">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="cdc0f-382">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="cdc0f-384">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="cdc0f-385">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-385">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="cdc0f-387">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-387">*Installation progress*</span></span>
6. <span data-ttu-id="cdc0f-388">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-388">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="cdc0f-390">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-390">*Installation completed*</span></span>
7. <span data-ttu-id="cdc0f-391">Fare clic su **Exit** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="cdc0f-392">Per aprire Visual Studio Express per Web, fare clic per il **avviare** schermata e iniziare la scrittura &quot; **Visual Studio Express**&quot;, quindi fare clic sul **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Visual Studio Express per il riquadro Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="cdc0f-394">*Visual Studio Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="cdc0f-395">Appendice b: uso dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="cdc0f-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="cdc0f-396">Con i frammenti di codice, hai tutto il codice che necessario a tua disposizione.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="cdc0f-397">Il documento lab indicherà esattamente quando usarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="cdc0f-398">![Uso di frammenti di codice di Visual Studio per inserire codice nel progetto](aspnet-mvc-4-dependency-injection/_static/image19.png "frammenti di codice con Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="cdc0f-399">*Uso di frammenti di codice di Visual Studio per inserire codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="cdc0f-400">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="cdc0f-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="cdc0f-401">Posizionare il cursore in cui si vuole inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="cdc0f-402">Iniziare a digitare il nome del frammento di codice (senza spazi o trattini).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="cdc0f-403">Guarda come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="cdc0f-404">Selezionare il frammento di codice corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="cdc0f-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="cdc0f-405">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="cdc0f-406">![Iniziare a digitare il nome di frammento](aspnet-mvc-4-dependency-injection/_static/image20.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="cdc0f-407">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="cdc0f-408">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-dependency-injection/_static/image21.png "premere Tab per selezionare il frammento di codice evidenziata")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="cdc0f-409">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="cdc0f-410">![Il frammento di codice e premere nuovamente Tab espanderà](aspnet-mvc-4-dependency-injection/_static/image22.png "si espanderà il frammento di codice e premere nuovamente Tab")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="cdc0f-411">*Il frammento di codice e premere nuovamente Tab espanderà*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="cdc0f-412">***Per aggiungere un frammento di codice usando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="cdc0f-413">Pulsante destro del mouse in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="cdc0f-414">Selezionare **Inserisci frammento** aggiungendo **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="cdc0f-415">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="cdc0f-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="cdc0f-416">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento](aspnet-mvc-4-dependency-injection/_static/image23.png "rapida in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="cdc0f-417">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e scegliere Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="cdc0f-418">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-dependency-injection/_static/image24.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa")</span><span class="sxs-lookup"><span data-stu-id="cdc0f-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="cdc0f-419">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di essa*</span><span class="sxs-lookup"><span data-stu-id="cdc0f-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
