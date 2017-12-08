---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Inserimento di dipendenze di ASP.NET MVC 4 | Documenti Microsoft
author: rick-anderson
description: "Nota: Questa pratica si presuppone di conoscenza di base dei filtri ASP.NET MVC e ASP.NET MVC 4. Se si utilizzano i filtri ASP.NET MVC 4 prima di, è consigliato..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: af4967f642ba4615f3392c0c404d2ec62edaaae8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="07874-104">Inserimento di dipendenze di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="07874-104">ASP.NET MVC 4 Dependency Injection</span></span>
====================
<span data-ttu-id="07874-105">da [categorie Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="07874-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="07874-106">Questa pratica presuppone avere conoscenze di base **ASP.NET MVC** e **filtri ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="07874-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="07874-107">Se non è stato utilizzato **filtri ASP.NET MVC 4** in precedenza, è consigliabile esaminare **filtri azione personalizzati di ASP.NET MVC** le esercitazioni pratiche.</span><span class="sxs-lookup"><span data-stu-id="07874-107">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="07874-108">Tutto il codice di esempio e i frammenti di codice sono inclusi nel Web categorie Training Kit, disponibile all'indirizzo [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="07874-108">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<span data-ttu-id="07874-109">In **oggetto orientato alla programmazione** paradigma di interazione tra gli oggetti in un modello di collaborazione in cui sono presenti i collaboratori e i consumer.</span><span class="sxs-lookup"><span data-stu-id="07874-109">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="07874-110">Naturalmente, questo modello di comunicazione genera dipendenze tra oggetti e componenti, diventare difficile da gestire quando si aumenta la complessità.</span><span class="sxs-lookup"><span data-stu-id="07874-110">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="07874-111">![Classe di dipendenze e la complessità del modello](aspnet-mvc-4-dependency-injection/_static/image1.png "classe dipendenze e la complessità del modello")</span><span class="sxs-lookup"><span data-stu-id="07874-111">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="07874-112">*Dipendenze di classe e complessità del modello*</span><span class="sxs-lookup"><span data-stu-id="07874-112">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="07874-113">Probabile che hanno sentito parlare di **modello di Factory** e la separazione tra l'interfaccia e l'implementazione di servizi, in cui gli oggetti client sono spesso responsabili della posizione del servizio.</span><span class="sxs-lookup"><span data-stu-id="07874-113">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="07874-114">Il modello di inserimento di dipendenze è una particolare implementazione di inversione di controllo.</span><span class="sxs-lookup"><span data-stu-id="07874-114">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="07874-115">**Inversione di controllo (IoC)** significa che gli oggetti non creano altri oggetti su cui si basano per svolgere il proprio lavoro.</span><span class="sxs-lookup"><span data-stu-id="07874-115">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="07874-116">Invece, ricevono gli oggetti che devono essere da un'origine esterna (ad esempio, un file di configurazione xml).</span><span class="sxs-lookup"><span data-stu-id="07874-116">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="07874-117">**Dependency Injection (DI)** significa che questo scopo, senza l'intervento dell'oggetto, in genere un componente del framework che passa i parametri del costruttore e impostare le proprietà.</span><span class="sxs-lookup"><span data-stu-id="07874-117">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="07874-118">Il modello di progettazione dipendenza Injection (DI)</span><span class="sxs-lookup"><span data-stu-id="07874-118">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="07874-119">In generale, l'obiettivo di inserimento di dipendenze è che una classe client (ad esempio *dei giocatori di golf*) richiede un elemento che soddisfa un'interfaccia (ad esempio *IClub*).</span><span class="sxs-lookup"><span data-stu-id="07874-119">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="07874-120">Non è rilevante che cos'è il tipo concreto (ad esempio *WoodClub, IronClub, WedgeClub* o *PutterClub*), è richiesto un altro utente per gestire questa (ad esempio, una buona *caddy*).</span><span class="sxs-lookup"><span data-stu-id="07874-120">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="07874-121">Il Resolver di dipendenza in ASP.NET MVC consente di registrare la logica di dipendenza in un altro (ad esempio, un contenitore o un *elenco di associazioni*).</span><span class="sxs-lookup"><span data-stu-id="07874-121">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="07874-122">![Diagramma di inserimento di dipendenze](aspnet-mvc-4-dependency-injection/_static/image2.png "illustrazione di inserimento di dipendenze")</span><span class="sxs-lookup"><span data-stu-id="07874-122">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="07874-123">*Inserimento di dipendenze - analogia Golf*</span><span class="sxs-lookup"><span data-stu-id="07874-123">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="07874-124">I vantaggi dell'utilizzo di modello di inserimento di dipendenze e inversione di controllo sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="07874-124">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="07874-125">Riduce di accoppiamenti di classi</span><span class="sxs-lookup"><span data-stu-id="07874-125">Reduces class coupling</span></span>
- <span data-ttu-id="07874-126">Aumenta il riutilizzo di codice</span><span class="sxs-lookup"><span data-stu-id="07874-126">Increases code reusing</span></span>
- <span data-ttu-id="07874-127">Migliora la gestibilità del codice</span><span class="sxs-lookup"><span data-stu-id="07874-127">Improves code maintainability</span></span>
- <span data-ttu-id="07874-128">Migliora la verifica delle applicazioni</span><span class="sxs-lookup"><span data-stu-id="07874-128">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="07874-129">Inserimento di dipendenze talvolta viene confrontato con il modello di progettazione Factory astratta, ma è presente una leggera differenza tra entrambi gli approcci.</span><span class="sxs-lookup"><span data-stu-id="07874-129">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="07874-130">SEN è un Framework di uso sottostante per risolvere le dipendenze chiamando le factory e i servizi registrati.</span><span class="sxs-lookup"><span data-stu-id="07874-130">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="07874-131">Per comprendere il modello di inserimento di dipendenza, dopo che si imparerà in questa esercitazione per applicarlo in ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="07874-131">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="07874-132">Si inizierà mediante l'inserimento di dipendenza nel **controller** per includere un servizio di accesso ai database.</span><span class="sxs-lookup"><span data-stu-id="07874-132">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="07874-133">Successivamente, si applicherà l'inserimento di dipendenze per il **viste** per utilizzare un servizio e visualizzare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="07874-133">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="07874-134">Infine, sarà possibile estendere il valore DI ASP.NET MVC 4 filtri, inserendo un filtro azione personalizzato nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="07874-134">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="07874-135">In questa pratica, si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="07874-135">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="07874-136">Integrazione di ASP.NET MVC 4 con Unity per l'inserimento di dipendenze mediante pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="07874-136">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="07874-137">Utilizzare l'inserimento di dipendenze all'interno di un Controller MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="07874-137">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="07874-138">Utilizzare l'inserimento di dipendenze all'interno di una visualizzazione ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="07874-138">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="07874-139">Utilizzare l'inserimento di dipendenze all'interno di un filtro azione MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="07874-139">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="07874-140">Questa esercitazione Usa pacchetto NuGet Unity.Mvc3 per la risoluzione delle dipendenze, ma è possibile adattare qualsiasi Framework di inserimento di dipendenza per funzionare con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="07874-140">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="07874-141">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="07874-141">Prerequisites</span></span>

<span data-ttu-id="07874-142">È necessario che gli elementi seguenti per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="07874-142">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="07874-143">[Microsoft Visual Studio Express 2012 per Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superiore (lettura [appendice](#AppendixA) per istruzioni su come installarlo).</span><span class="sxs-lookup"><span data-stu-id="07874-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="07874-144">Configurazione</span><span class="sxs-lookup"><span data-stu-id="07874-144">Setup</span></span>

<span data-ttu-id="07874-145">**L'installazione di frammenti di codice**</span><span class="sxs-lookup"><span data-stu-id="07874-145">**Installing Code Snippets**</span></span>

<span data-ttu-id="07874-146">Per praticità, gran parte del codice che vengono gestiti in questa esercitazione è disponibile come frammenti di codice di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="07874-146">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="07874-147">Per installare i frammenti di codice eseguire **.\Source\Setup\CodeSnippets.vsi** file.</span><span class="sxs-lookup"><span data-stu-id="07874-147">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="07874-148">Se non si ha familiarità con i frammenti di codice di Visual Studio e si desidera per informazioni su come usarle, è possibile fare riferimento all'appendice di questo documento &quot; [appendice b: tramite i frammenti di codice](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="07874-148">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="07874-149">Esercizi</span><span class="sxs-lookup"><span data-stu-id="07874-149">Exercises</span></span>

<span data-ttu-id="07874-150">Questa pratica include gli esercizi seguenti:</span><span class="sxs-lookup"><span data-stu-id="07874-150">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="07874-151">Esercizio 1: Inserimento di un Controller</span><span class="sxs-lookup"><span data-stu-id="07874-151">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="07874-152">Esercizio 2: Inserimento di una vista</span><span class="sxs-lookup"><span data-stu-id="07874-152">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="07874-153">Esercizio 3: Inserimento di filtri</span><span class="sxs-lookup"><span data-stu-id="07874-153">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="07874-154">Ogni esercizio è accompagnato da un **fine** cartella che contiene la soluzione risultante si dovrebbe ottenere dopo aver completato l'esercitazione dall'inizio.</span><span class="sxs-lookup"><span data-stu-id="07874-154">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="07874-155">Se è necessario ulteriore assistenza utilizza gli esercizi, è possibile utilizzare questa soluzione come guida.</span><span class="sxs-lookup"><span data-stu-id="07874-155">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="07874-156">Tempo stimato per completare questa esercitazione: **30 minuti**.</span><span class="sxs-lookup"><span data-stu-id="07874-156">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="07874-157">Esercizio 1: Inserimento di un Controller</span><span class="sxs-lookup"><span data-stu-id="07874-157">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="07874-158">In questo esercizio, si apprenderà come usare Dependency Injection in ASP.NET MVC Controllers integrando Unity utilizzando un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="07874-158">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="07874-159">Per questo motivo, i servizi verranno incluse nel controller di MvcMusicStore per separare la logica di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="07874-159">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="07874-160">I servizi creerà una nuova dipendenza nel costruttore del controller, viene risolto mediante l'inserimento di dipendenza con l'aiuto di **Unity**.</span><span class="sxs-lookup"><span data-stu-id="07874-160">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="07874-161">Questo approccio viene illustrato come generare meno applicazioni a cui sono più flessibili e facili da gestire e testare.</span><span class="sxs-lookup"><span data-stu-id="07874-161">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="07874-162">Inoltre, si apprenderà come integrare ASP.NET MVC con Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-162">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="07874-163">Informazioni sul servizio StoreManager</span><span class="sxs-lookup"><span data-stu-id="07874-163">About StoreManager Service</span></span>

<span data-ttu-id="07874-164">L'archivio di musica MVC fornito nella soluzione begin ora include un servizio che gestisce i dati di archivio Controller denominati **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="07874-164">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="07874-165">Di seguito si noterà che l'implementazione del servizio di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="07874-165">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="07874-166">Si noti che tutti i metodi di restituiscono le entità del modello.</span><span class="sxs-lookup"><span data-stu-id="07874-166">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="07874-167">**StoreController** utilizza ora di inizio soluzione **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="07874-167">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="07874-168">Tutti i riferimenti di dati sono stati rimossi da **StoreController**ed è ora possibile modificare il provider di accesso ai dati corrente senza modificare qualsiasi metodo che utilizza **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="07874-168">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="07874-169">Di seguito che sono il **StoreController** implementazione presenta una dipendenza con **StoreService** all'interno del costruttore di classe.</span><span class="sxs-lookup"><span data-stu-id="07874-169">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="07874-170">La dipendenza presentata in questo esercizio è correlata a **Inversion of Control** (IoC).</span><span class="sxs-lookup"><span data-stu-id="07874-170">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="07874-171">Il **StoreController** costruttore della classe riceve un **IStoreService** parametro di tipo, è essenziale per eseguire chiamate al servizio dall'interno della classe.</span><span class="sxs-lookup"><span data-stu-id="07874-171">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="07874-172">Tuttavia, **StoreController** non implementa il costruttore predefinito (senza alcun parametro) che qualsiasi controller è necessario per l'utilizzo di MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="07874-172">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="07874-173">Per risolvere la dipendenza, il controller deve essere creato da una factory di tipo abstract (una classe che restituisce qualsiasi oggetto del tipo specificato).</span><span class="sxs-lookup"><span data-stu-id="07874-173">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="07874-174">Quando la classe tenta di creare il StoreController senza inviare l'oggetto servizio, perché non esiste alcun costruttore dichiarato, si verificherà un errore.</span><span class="sxs-lookup"><span data-stu-id="07874-174">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="07874-175">Attività 1: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="07874-175">Task 1 - Running the Application</span></span>

<span data-ttu-id="07874-176">In questa attività si eseguirà l'applicazione iniziale, che include il servizio nel Controller dell'archivio che separa l'accesso ai dati dalla logica dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="07874-176">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="07874-177">Quando si esegue l'applicazione, si riceverà un'eccezione, come il servizio controller non viene passato come parametro per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="07874-177">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="07874-178">Aprire il **iniziare** soluzione si trova **inserendo Source\Ex01 Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="07874-178">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

    1. <span data-ttu-id="07874-179">È necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="07874-179">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="07874-180">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="07874-180">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="07874-181">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="07874-181">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="07874-182">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="07874-182">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="07874-183">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="07874-183">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="07874-184">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="07874-184">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="07874-185">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="07874-185">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="07874-186">Premere **Ctrl + F5** per eseguire l'applicazione senza eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="07874-186">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="07874-187">Verrà visualizzato il messaggio di errore &quot; **Nessun costruttore senza parametri definito per questo oggetto**&quot;:</span><span class="sxs-lookup"><span data-stu-id="07874-187">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="07874-188">![Errore durante l'esecuzione dell'applicazione ASP.NET MVC iniziare](aspnet-mvc-4-dependency-injection/_static/image3.png "errore durante l'esecuzione dell'applicazione ASP.NET MVC iniziare")</span><span class="sxs-lookup"><span data-stu-id="07874-188">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="07874-189">*Errore durante l'esecuzione dell'applicazione ASP.NET MVC iniziare*</span><span class="sxs-lookup"><span data-stu-id="07874-189">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="07874-190">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="07874-190">Close the browser.</span></span>

<span data-ttu-id="07874-191">Nei passaggi seguenti verranno usati la soluzione di archiviazione musica per inserire la dipendenza da che questo controller è necessario.</span><span class="sxs-lookup"><span data-stu-id="07874-191">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="07874-192">Attività 2 - Unity inclusi nella soluzione MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="07874-192">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="07874-193">In questa attività si includerà **Unity.Mvc3** pacchetto NuGet per la soluzione.</span><span class="sxs-lookup"><span data-stu-id="07874-193">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="07874-194">Unity.Mvc3 pacchetto è stato progettato per ASP.NET MVC 3, ma è completamente compatibile con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="07874-194">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="07874-195">Unity è un contenitore dell'inserimento di dipendenza leggera ed estendibile con supporto facoltativo per l'istanza e digitare l'intercettazione.</span><span class="sxs-lookup"><span data-stu-id="07874-195">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="07874-196">È un contenitore generico per l'utilizzo in qualsiasi tipo di applicazione .NET.</span><span class="sxs-lookup"><span data-stu-id="07874-196">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="07874-197">Fornisce le funzionalità comuni meccanismi di inserimento di dipendenza tra, vedere: creazione di oggetti, astrazione dei requisiti in base alla specifica delle dipendenze in fase di esecuzione e la flessibilità, rinviando la configurazione del componente al contenitore.</span><span class="sxs-lookup"><span data-stu-id="07874-197">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="07874-198">Installare **Unity.Mvc3** pacchetto NuGet nel **MvcMusicStore** progetto.</span><span class="sxs-lookup"><span data-stu-id="07874-198">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="07874-199">A tale scopo, aprire il **Package Manager Console** da **vista** | **altre finestre**.</span><span class="sxs-lookup"><span data-stu-id="07874-199">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="07874-200">Eseguire il seguente comando.</span><span class="sxs-lookup"><span data-stu-id="07874-200">Run the following command.</span></span>

    <span data-ttu-id="07874-201">PMC</span><span class="sxs-lookup"><span data-stu-id="07874-201">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="07874-202">![Installazione del pacchetto NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "l'installazione del pacchetto NuGet Unity.Mvc3")</span><span class="sxs-lookup"><span data-stu-id="07874-202">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="07874-203">*Installazione del pacchetto NuGet Unity.Mvc3*</span><span class="sxs-lookup"><span data-stu-id="07874-203">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="07874-204">Una volta il **Unity.Mvc3** viene installato, esplorare i file e cartelle vengono aggiunti automaticamente per semplificare la configurazione di Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-204">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="07874-205">![Installato il pacchetto di Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 pacchetto installato")</span><span class="sxs-lookup"><span data-stu-id="07874-205">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="07874-206">*Pacchetto Unity.Mvc3 installato*</span><span class="sxs-lookup"><span data-stu-id="07874-206">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="07874-207">Attività 3 - registrazione Unity nell'applicazione Global.asax.cs\_avviare</span><span class="sxs-lookup"><span data-stu-id="07874-207">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="07874-208">In questa attività si aggiornerà il **applicazione\_avviare** metodo si trova **Global.asax.cs** per chiamare l'inizializzatore di avvio automatico di Unity e quindi aggiornare la registrazione di file di programma di avvio automatico il servizio e il Controller da utilizzare per l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="07874-208">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="07874-209">Verrà associato a questo punto, il programma di avvio che è il file che inizializza il contenitore di Unity e Resolver di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="07874-209">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="07874-210">A tale scopo, aprire **Global.asax.cs** e aggiungere il seguente codice evidenziato all'interno di **applicazione\_avviare** metodo.</span><span class="sxs-lookup"><span data-stu-id="07874-210">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="07874-211">(- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex01 - inizializzare Unity*)</span><span class="sxs-lookup"><span data-stu-id="07874-211">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="07874-212">Aprire **Bootstrapper.cs** file.</span><span class="sxs-lookup"><span data-stu-id="07874-212">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="07874-213">Includere gli spazi dei nomi seguenti: **MvcMusicStore.Services** e **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="07874-213">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="07874-214">(- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex01 - programma di avvio automatico aggiungendo degli spazi dei nomi*)</span><span class="sxs-lookup"><span data-stu-id="07874-214">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="07874-215">Sostituire **BuildUnityContainer** del contenuto con il codice seguente che registra Controller di archiviazione e il servizio di archiviazione di metodo.</span><span class="sxs-lookup"><span data-stu-id="07874-215">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="07874-216">(- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex01 - archivio registro Controller e il servizio*)</span><span class="sxs-lookup"><span data-stu-id="07874-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="07874-217">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="07874-217">Task 4 - Running the Application</span></span>

<span data-ttu-id="07874-218">In questa attività verrà eseguita l'applicazione per verificare che può ora essere caricato dopo l'inclusione di Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-218">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="07874-219">Premere **F5** per eseguire l'applicazione, l'applicazione deve caricare ora senza visualizzare alcun messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="07874-219">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="07874-220">![Applicazione eseguita con l'inserimento di dipendenze](aspnet-mvc-4-dependency-injection/_static/image6.png "applicazione eseguita con l'inserimento di dipendenze")</span><span class="sxs-lookup"><span data-stu-id="07874-220">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="07874-221">*Applicazione in esecuzione con l'inserimento di dipendenze*</span><span class="sxs-lookup"><span data-stu-id="07874-221">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="07874-222">Passare a **e delle archiviazioni**.</span><span class="sxs-lookup"><span data-stu-id="07874-222">Browse to **/Store**.</span></span> <span data-ttu-id="07874-223">Verranno quindi richiamati **StoreController**, che è ora possibile creare utilizzando **Unity**.</span><span class="sxs-lookup"><span data-stu-id="07874-223">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="07874-224">![MVC negozio](aspnet-mvc-4-dependency-injection/_static/image7.png "negozio MVC")</span><span class="sxs-lookup"><span data-stu-id="07874-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="07874-225">*Negozio MVC*</span><span class="sxs-lookup"><span data-stu-id="07874-225">*MVC Music Store*</span></span>
3. <span data-ttu-id="07874-226">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="07874-226">Close the browser.</span></span>

<span data-ttu-id="07874-227">Negli esercizi seguenti si apprenderà come estendere l'ambito di inserimento di dipendenze per poterlo utilizzare all'interno di visualizzazioni MVC ASP.NET e i filtri dell'azione.</span><span class="sxs-lookup"><span data-stu-id="07874-227">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="07874-228">Esercizio 2: Inserimento di una vista</span><span class="sxs-lookup"><span data-stu-id="07874-228">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="07874-229">In questo esercizio, si apprenderà come usare l'inserimento di dipendenze di una vista con le nuove funzionalità di ASP.NET MVC 4 per l'integrazione con Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-229">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="07874-230">A tale scopo, chiamare un servizio personalizzato all'interno di archivio esplorazione della vista, che consente di visualizzare un messaggio e un'immagine riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="07874-230">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="07874-231">Verrà quindi integrare il progetto con Unity e creare un resolver di dipendenza personalizzata per l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="07874-231">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="07874-232">Attività 1 - Creazione di una vista che utilizza un servizio</span><span class="sxs-lookup"><span data-stu-id="07874-232">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="07874-233">In questa attività si creerà una vista che esegue una chiamata al servizio per generare una nuova dipendenza.</span><span class="sxs-lookup"><span data-stu-id="07874-233">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="07874-234">Il servizio consiste in un semplice servizio di messaggistica incluso in questa soluzione.</span><span class="sxs-lookup"><span data-stu-id="07874-234">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="07874-235">Aprire il **iniziare** soluzione si trova nel **inserendo Source\Ex02 View\Begin** cartella.</span><span class="sxs-lookup"><span data-stu-id="07874-235">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="07874-236">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="07874-236">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="07874-237">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="07874-237">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="07874-238">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="07874-238">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="07874-239">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="07874-239">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="07874-240">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="07874-240">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="07874-241">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="07874-241">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="07874-242">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="07874-242">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="07874-243">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="07874-243">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="07874-244">Per ulteriori informazioni, vedere l'articolo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="07874-244">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="07874-245">Includono il **MessageService.cs** e **IMessageService.cs** classi disponibili nel **origine \Assets** cartella **/servizi**.</span><span class="sxs-lookup"><span data-stu-id="07874-245">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="07874-246">A tale scopo, fare doppio clic su **servizi** cartella e selezionare **Aggiungi elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="07874-246">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="07874-247">Passare al percorso dei file e li include.</span><span class="sxs-lookup"><span data-stu-id="07874-247">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="07874-248">![Aggiunta di SMS e l'interfaccia di servizio](aspnet-mvc-4-dependency-injection/_static/image8.png "aggiunta Message Service e interfaccia di servizio")</span><span class="sxs-lookup"><span data-stu-id="07874-248">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="07874-249">*Aggiunta del servizio di messaggi e l'interfaccia di servizio*</span><span class="sxs-lookup"><span data-stu-id="07874-249">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="07874-250">Il **IMessageService** interfaccia definisce due proprietà implementate dal **MessageService** classe.</span><span class="sxs-lookup"><span data-stu-id="07874-250">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="07874-251">Queste proprietà -**messaggio** e **ImageUrl**-memorizzare il messaggio e l'URL dell'immagine da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="07874-251">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="07874-252">Creare la cartella **/pagine** cartella principale del progetto e quindi aggiungere la classe esistente **MyBasePage.cs** da **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="07874-252">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="07874-253">La pagina base che è eredita presenta la struttura seguente.</span><span class="sxs-lookup"><span data-stu-id="07874-253">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="07874-254">![Cartella pagine](aspnet-mvc-4-dependency-injection/_static/image9.png "cartella pagine")</span><span class="sxs-lookup"><span data-stu-id="07874-254">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="07874-255">Aprire **Browse.cshtml** visualizzare da **/viste o all'archivio** cartella e renderlo ereditare **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="07874-255">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="07874-256">Nel **Sfoglia** visualizzare, aggiungere una chiamata a **MessageService** per visualizzare un'immagine e un messaggio recuperato dal servizio.</span><span class="sxs-lookup"><span data-stu-id="07874-256">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
<span data-ttu-id="07874-257">(C#)</span><span class="sxs-lookup"><span data-stu-id="07874-257">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="07874-258">Attività 2 - tra cui un Resolver di dipendenza personalizzata e un attivatore della pagina di visualizzazione personalizzata</span><span class="sxs-lookup"><span data-stu-id="07874-258">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="07874-259">Nell'attività precedente, sono inserite una nuova dipendenza all'interno di una vista per eseguire una chiamata al servizio all'interno.</span><span class="sxs-lookup"><span data-stu-id="07874-259">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="07874-260">A questo punto, si risolverà tale dipendenza implementando le interfacce di ASP.NET MVC Dependency Injection **IViewPageActivator** e **resolver di dipendenza**.</span><span class="sxs-lookup"><span data-stu-id="07874-260">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="07874-261">Verranno incluse nella soluzione un'implementazione di **resolver di dipendenza** che gestirà il recupero del servizio con Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-261">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="07874-262">Quindi, si includerà un'altra implementazione personalizzata di **IViewPageActivator** interfaccia che sia possibile risolvere la creazione delle viste.</span><span class="sxs-lookup"><span data-stu-id="07874-262">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="07874-263">Poiché ASP.NET MVC 3, l'implementazione per l'inserimento di dipendenze ha semplificato le interfacce per registrare i servizi.</span><span class="sxs-lookup"><span data-stu-id="07874-263">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="07874-264">**Resolver di dipendenza** e **IViewPageActivator** fanno parte delle funzionalità di ASP.NET MVC 3 per l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="07874-264">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="07874-265">**-Resolver di dipendenza** interfaccia sostituisce la precedente IMvcServiceLocator.</span><span class="sxs-lookup"><span data-stu-id="07874-265">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="07874-266">Gli implementatori di resolver di dipendenza devono restituire un'istanza del servizio o una raccolta di servizio.</span><span class="sxs-lookup"><span data-stu-id="07874-266">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="07874-267">**-IViewPageActivator** interfaccia fornisce un controllo più accurato su come visualizzare le pagine vengono create istanze tramite l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="07874-267">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="07874-268">Le classi che implementano **IViewPageActivator** interfaccia possa creare istanze di visualizzazione utilizzando le informazioni di contesto.</span><span class="sxs-lookup"><span data-stu-id="07874-268">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="07874-269">Creare il /**factory** cartella nella cartella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="07874-269">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="07874-270">Includere **CustomViewPageActivator.cs** alla soluzione da **/origini asset/** a **factory** cartella.</span><span class="sxs-lookup"><span data-stu-id="07874-270">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="07874-271">A tale scopo, fare doppio clic su di **/Factories** cartella, selezionare **Aggiungi | Elemento esistente** e quindi selezionare **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="07874-271">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="07874-272">Questa classe implementa il **IViewPageActivator** interfaccia per contenere il contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-272">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="07874-273">**CustomViewPageActivator** è responsabile per la gestione della creazione di una vista utilizzando un contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-273">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="07874-274">Includere **UnityDependencyResolver.cs** file **origini/asset** a **/Factories** cartella.</span><span class="sxs-lookup"><span data-stu-id="07874-274">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="07874-275">A tale scopo, fare doppio clic su di **/Factories** cartella, selezionare **Aggiungi | Elemento esistente** e quindi selezionare **UnityDependencyResolver.cs** file.</span><span class="sxs-lookup"><span data-stu-id="07874-275">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="07874-276">**UnityDependencyResolver** classe è un DependencyResolver personalizzato per Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-276">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="07874-277">Quando un servizio non viene trovato all'interno del contenitore di Unity, il sistema di risoluzione di base è invocated.</span><span class="sxs-lookup"><span data-stu-id="07874-277">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="07874-278">Nell'attività seguente verranno registrate entrambe le implementazioni per informare il percorso dei servizi e le viste del modello.</span><span class="sxs-lookup"><span data-stu-id="07874-278">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="07874-279">Attività 3 - registrazione per l'inserimento di dipendenze all'interno del contenitore di Unity</span><span class="sxs-lookup"><span data-stu-id="07874-279">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="07874-280">In questa attività, inserire tutte le operazioni precedenti per far funzionare l'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="07874-280">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="07874-281">Fino a ora la soluzione contiene gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="07874-281">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="07874-282">Oggetto **Sfoglia** visualizzazione che eredita da **MyBaseClass** e utilizza **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="07874-282">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="07874-283">Una classe intermedia -**MyBaseClass**-che ha dichiarato per l'interfaccia del servizio di inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="07874-283">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="07874-284">Un servizio - **MessageService** - e la relativa interfaccia **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="07874-284">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="07874-285">Un resolver di dipendenza personalizzata per Unity - **UnityDependencyResolver** -che gestisce il recupero del servizio.</span><span class="sxs-lookup"><span data-stu-id="07874-285">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="07874-286">Un attivatore della pagina di visualizzazione - **CustomViewPageActivator** -che crea la pagina.</span><span class="sxs-lookup"><span data-stu-id="07874-286">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="07874-287">Per inserire **Sfoglia** visualizzazione, verrà ora registrato il resolver di dipendenza personalizzata nel contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-287">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="07874-288">Aprire **Bootstrapper.cs** file.</span><span class="sxs-lookup"><span data-stu-id="07874-288">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="07874-289">Registrare un'istanza di **MessageService** nel contenitore di Unity per inizializzare il servizio:</span><span class="sxs-lookup"><span data-stu-id="07874-289">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="07874-290">(- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex02 - registro Message Service*)</span><span class="sxs-lookup"><span data-stu-id="07874-290">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="07874-291">Aggiungere un riferimento a **MvcMusicStore.Factories** dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="07874-291">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="07874-292">(- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex02 - factory Namespace*)</span><span class="sxs-lookup"><span data-stu-id="07874-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="07874-293">Registrare **CustomViewPageActivator** come un attivatore della pagina di visualizzazione nel contenitore Unity:</span><span class="sxs-lookup"><span data-stu-id="07874-293">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="07874-294">(- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex02 - registro CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="07874-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="07874-295">Sostituire i resolver di dipendenza predefinito di ASP.NET MVC 4 con un'istanza di **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="07874-295">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="07874-296">A tale scopo, sostituire **Initialise** metodo contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="07874-296">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="07874-297">(- Frammento di codice *del Resolver di dipendenza aggiornamento ASP.NET dipendenza Injection Lab - Ex02 -*)</span><span class="sxs-lookup"><span data-stu-id="07874-297">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="07874-298">ASP.NET MVC fornisce una classe resolver di dipendenza predefinito.</span><span class="sxs-lookup"><span data-stu-id="07874-298">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="07874-299">Per utilizzare i resolver di dipendenza personalizzato come quello a cui che è stata creata per unity, il sistema di risoluzione deve essere sostituito.</span><span class="sxs-lookup"><span data-stu-id="07874-299">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="07874-300">Attività 4: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="07874-300">Task 4 - Running the Application</span></span>

<span data-ttu-id="07874-301">In questa attività verrà eseguita l'applicazione per verificare che il Browser archivio utilizza il servizio e Mostra l'immagine e il recupero del messaggio:</span><span class="sxs-lookup"><span data-stu-id="07874-301">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="07874-302">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="07874-302">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="07874-303">Fare clic su **Rock** nel Menu di generi e vedere come il **MessageService** è stato inserito nella vista e caricato il messaggio di benvenuto e l'immagine.</span><span class="sxs-lookup"><span data-stu-id="07874-303">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="07874-304">In questo esempio, si sta immettendo a &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="07874-304">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="07874-305">![MVC negozio - vista Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC negozio - vista attacco intrusivo nel codice")</span><span class="sxs-lookup"><span data-stu-id="07874-305">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="07874-306">*MVC negozio - vista attacco intrusivo nel codice*</span><span class="sxs-lookup"><span data-stu-id="07874-306">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="07874-307">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="07874-307">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="07874-308">Esercizio 3: Inserimento di filtri azione</span><span class="sxs-lookup"><span data-stu-id="07874-308">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="07874-309">Nell'esercitazione pratica precedente **filtri azione personalizzati** chi ha familiarità con i filtri personalizzazione e l'inserimento.</span><span class="sxs-lookup"><span data-stu-id="07874-309">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="07874-310">In questo esercizio, si apprenderà come inserire i filtri con inserimento di dipendenze mediante il contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-310">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="07874-311">A tale scopo, si aggiungerà alla soluzione di negozio un filtro azione personalizzato che consente di tenere traccia di attività del sito.</span><span class="sxs-lookup"><span data-stu-id="07874-311">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="07874-312">Attività 1 - tra cui il filtro di rilevamento nella soluzione</span><span class="sxs-lookup"><span data-stu-id="07874-312">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="07874-313">In questa attività, verranno incluse nell'archivio di musica un filtro azione personalizzato per gli eventi di traccia.</span><span class="sxs-lookup"><span data-stu-id="07874-313">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="07874-314">Come filtro azione personalizzato concetti siano già considerati nel laboratorio precedente &quot;filtri azione personalizzati&quot;, verrà semplicemente includono la classe di filtro dalla cartella Assets di questa esercitazione e quindi creare un Provider di filtri per Unity:</span><span class="sxs-lookup"><span data-stu-id="07874-314">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="07874-315">Aprire il **iniziare** soluzione si trova nel **Source\Ex03 - inserendo azione Filter\Begin** cartella.</span><span class="sxs-lookup"><span data-stu-id="07874-315">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="07874-316">In caso contrario, è possibile continuare a usare il **fine** soluzione ottenuto completando l'esercizio precedente.</span><span class="sxs-lookup"><span data-stu-id="07874-316">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="07874-317">Se è stato aperto l'oggetto fornito **iniziare** soluzione, è necessario scaricare alcuni pacchetti NuGet mancanti prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="07874-317">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="07874-318">A tale scopo, fare clic su di **progetto** dal menu **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="07874-318">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="07874-319">Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic su **ripristinare** per scaricare i pacchetti mancanti.</span><span class="sxs-lookup"><span data-stu-id="07874-319">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="07874-320">Infine, compilare la soluzione facendo **compilare** | **Compila soluzione**.</span><span class="sxs-lookup"><span data-stu-id="07874-320">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="07874-321">Uno dei vantaggi dell'uso di NuGet è che non è necessario fornire tutte le librerie nel progetto, la riduzione delle dimensioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="07874-321">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="07874-322">Con Power Tools di NuGet, specificando le versioni del pacchetto nel file Packages, sarà in grado di scaricare tutte le librerie richieste la prima volta che si esegue il progetto.</span><span class="sxs-lookup"><span data-stu-id="07874-322">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="07874-323">È per questo motivo è necessario eseguire questi passaggi dopo l'apertura di una soluzione esistente da questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="07874-323">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="07874-324">Per ulteriori informazioni, vedere l'articolo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="07874-324">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="07874-325">Includere **TraceActionFilter.cs** file **origini/asset** a **/filtri** cartella.</span><span class="sxs-lookup"><span data-stu-id="07874-325">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="07874-326">Questo filtro azione personalizzata esegue la traccia di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="07874-326">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="07874-327">È possibile controllare &quot;locale di ASP.NET MVC 4 e filtri azione dinamici&quot; Lab per più riferimento.</span><span class="sxs-lookup"><span data-stu-id="07874-327">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="07874-328">Aggiungere la classe vuota **FilterProvider.cs** al progetto nella cartella   **/filtri.**</span><span class="sxs-lookup"><span data-stu-id="07874-328">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="07874-329">Aggiungere il **System** e **Microsoft.Practices.Unity** spazi dei nomi in **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="07874-329">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="07874-330">(- Frammento di codice *Provider ASP.NET dipendenza Injection Lab - Ex03 - filtro con l'aggiunta di spazi dei nomi*)</span><span class="sxs-lookup"><span data-stu-id="07874-330">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="07874-331">Assicurarsi che la classe erediti dal **IFilterProvider** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="07874-331">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="07874-332">Aggiungere un **IUnityContainer** proprietà il **FilterProvider** classe e quindi creare un costruttore di classe per assegnare il contenitore.</span><span class="sxs-lookup"><span data-stu-id="07874-332">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="07874-333">(- Frammento di codice *il costruttore di Provider filtro ASP.NET dipendenza Injection Lab - Ex03 -*)</span><span class="sxs-lookup"><span data-stu-id="07874-333">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="07874-334">Il costruttore della classe provider filtro non sta creando un **nuovo** all'interno dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="07874-334">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="07874-335">Il contenitore viene passato come parametro e la dipendenza viene risolto da Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-335">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="07874-336">Nel **FilterProvider** classe, implementare il metodo **GetFilters** da **IFilterProvider** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="07874-336">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="07874-337">(- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex03 - filtro Provider GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="07874-337">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="07874-338">Attività 2 - registrazione e attivazione del filtro</span><span class="sxs-lookup"><span data-stu-id="07874-338">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="07874-339">In questa attività verrà abilitata la registrazione del sito.</span><span class="sxs-lookup"><span data-stu-id="07874-339">In this task, you will enable site tracking.</span></span> <span data-ttu-id="07874-340">A tale scopo, è necessario registrare il filtro in **Bootstrapper.cs BuildUnityContainer** metodo per avviare la registrazione:</span><span class="sxs-lookup"><span data-stu-id="07874-340">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="07874-341">Aprire **Web. config** si trova nella radice del progetto e abilitare il rilevamento di traccia al gruppo System. Web.</span><span class="sxs-lookup"><span data-stu-id="07874-341">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="07874-342">Aprire **Bootstrapper.cs** alla radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="07874-342">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="07874-343">Aggiungere un riferimento di **MvcMusicStore.Filters** dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="07874-343">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="07874-344">(- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex03 - programma di avvio automatico aggiungendo degli spazi dei nomi*)</span><span class="sxs-lookup"><span data-stu-id="07874-344">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="07874-345">Selezionare il **BuildUnityContainer** (metodo) e registrare il filtro del contenitore di Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-345">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="07874-346">È necessario registrare il provider di filtri, nonché il filtro di azione.</span><span class="sxs-lookup"><span data-stu-id="07874-346">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="07874-347">(- Frammento di codice *ASP.NET dipendenza Injection Lab - Ex03 - registro FilterProvider e ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="07874-347">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="07874-348">Attività 3: esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="07874-348">Task 3 - Running the Application</span></span>

<span data-ttu-id="07874-349">In questa attività, eseguire l'applicazione e i test verranno che il filtro azione personalizzato è la traccia dell'attività:</span><span class="sxs-lookup"><span data-stu-id="07874-349">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="07874-350">Premere **F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="07874-350">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="07874-351">Fare clic su **Rock** all'interno del Menu generi.</span><span class="sxs-lookup"><span data-stu-id="07874-351">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="07874-352">Se si desidera, è possibile passare alla generi più.</span><span class="sxs-lookup"><span data-stu-id="07874-352">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="07874-353">![Negozio](aspnet-mvc-4-dependency-injection/_static/image11.png "Negozio")</span><span class="sxs-lookup"><span data-stu-id="07874-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="07874-354">*Negozio*</span><span class="sxs-lookup"><span data-stu-id="07874-354">*Music Store*</span></span>
3. <span data-ttu-id="07874-355">Passare a **/Trace.axd** per visualizzare la traccia dell'applicazione di pagina e quindi fare clic su **Visualizza dettagli**.</span><span class="sxs-lookup"><span data-stu-id="07874-355">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="07874-356">![Log di analisi](aspnet-mvc-4-dependency-injection/_static/image12.png "Log di analisi")</span><span class="sxs-lookup"><span data-stu-id="07874-356">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="07874-357">*Log di analisi*</span><span class="sxs-lookup"><span data-stu-id="07874-357">*Application Trace Log*</span></span>

    <span data-ttu-id="07874-358">![Traccia dell'applicazione - dettagli richiesta](aspnet-mvc-4-dependency-injection/_static/image13.png "applicazione Trace - dettagli richiesta")</span><span class="sxs-lookup"><span data-stu-id="07874-358">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="07874-359">*Traccia dell'applicazione - dettagli della richiesta*</span><span class="sxs-lookup"><span data-stu-id="07874-359">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="07874-360">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="07874-360">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="07874-361">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="07874-361">Summary</span></span>

<span data-ttu-id="07874-362">Completando questa pratica si è appreso come utilizzare Dependency Injection in ASP.NET MVC 4 grazie all'integrazione con un pacchetto NuGet di Unity.</span><span class="sxs-lookup"><span data-stu-id="07874-362">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="07874-363">A tal fine, è stato utilizzato l'inserimento di dipendenze all'interno di controller, visualizzazioni e filtri azione.</span><span class="sxs-lookup"><span data-stu-id="07874-363">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="07874-364">Sono stati trattati i concetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="07874-364">The following concepts were covered:</span></span>

- <span data-ttu-id="07874-365">Funzionalità di inserimento di dipendenze di ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="07874-365">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="07874-366">Integrazione con Unity usando pacchetto NuGet Unity.Mvc3</span><span class="sxs-lookup"><span data-stu-id="07874-366">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="07874-367">Inserimento di dipendenze nel controller</span><span class="sxs-lookup"><span data-stu-id="07874-367">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="07874-368">Inserimento di dipendenze nelle viste</span><span class="sxs-lookup"><span data-stu-id="07874-368">Dependency Injection in Views</span></span>
- <span data-ttu-id="07874-369">Inserimento di dipendenze dei filtri azione</span><span class="sxs-lookup"><span data-stu-id="07874-369">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="07874-370">Appendice a: installazione di Visual Studio Express 2012 per Web</span><span class="sxs-lookup"><span data-stu-id="07874-370">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="07874-371">È possibile installare **Microsoft Visual Studio Express 2012 per Web** o un altro &quot;Express&quot; versione utilizzando il  **[installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="07874-371">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="07874-372">Le istruzioni seguenti consentono di eseguire i passaggi necessari per installare *Visual studio Express 2012 per Web* utilizzando *installazione guidata piattaforma Web di Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="07874-372">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="07874-373">Passare a [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="07874-373">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="07874-374">In alternativa, se è già stato installato installazione guidata piattaforma Web, è possibile aprire e ricerca per il prodotto &quot; *Visual Studio Express 2012 per Web con Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="07874-374">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="07874-375">Fare clic su **installa**.</span><span class="sxs-lookup"><span data-stu-id="07874-375">Click on **Install Now**.</span></span> <span data-ttu-id="07874-376">Se non si dispone **installazione guidata piattaforma Web** si verrà reindirizzati per scaricarlo e installarlo prima.</span><span class="sxs-lookup"><span data-stu-id="07874-376">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="07874-377">Una volta **installazione guidata piattaforma Web** è aperto, fare clic su **installare** per avviare l'installazione.</span><span class="sxs-lookup"><span data-stu-id="07874-377">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="07874-378">![Installa Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "installa Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="07874-378">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="07874-379">*Installa Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="07874-379">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="07874-380">Leggere i termini e le licenze di tutti i prodotti e fare clic su **accetto** per continuare.</span><span class="sxs-lookup"><span data-stu-id="07874-380">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Accettare le condizioni di licenza](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="07874-382">*Accettare le condizioni di licenza*</span><span class="sxs-lookup"><span data-stu-id="07874-382">*Accepting the license terms*</span></span>
5. <span data-ttu-id="07874-383">Attendere finché non viene completato il processo di download e l'installazione.</span><span class="sxs-lookup"><span data-stu-id="07874-383">Wait until the downloading and installation process completes.</span></span>

    ![Stato dell'installazione](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="07874-385">*Stato dell'installazione*</span><span class="sxs-lookup"><span data-stu-id="07874-385">*Installation progress*</span></span>
6. <span data-ttu-id="07874-386">Al termine dell'installazione, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="07874-386">When the installation completes, click **Finish**.</span></span>

    ![Installazione completata](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="07874-388">*Installazione completata*</span><span class="sxs-lookup"><span data-stu-id="07874-388">*Installation completed*</span></span>
7. <span data-ttu-id="07874-389">Fare clic su **uscita** per chiudere l'installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="07874-389">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="07874-390">Per aprire Visual Studio Express per Web, passare al **avviare** schermata e iniziare la scrittura &quot; **VS Express**&quot;, quindi fare clic su di **Visual Studio Express per Web** il riquadro.</span><span class="sxs-lookup"><span data-stu-id="07874-390">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express per il riquadro Web](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="07874-392">*VS Express per il riquadro Web*</span><span class="sxs-lookup"><span data-stu-id="07874-392">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="07874-393">Appendice b: utilizzo dei frammenti di codice</span><span class="sxs-lookup"><span data-stu-id="07874-393">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="07874-394">Con i frammenti di codice, si dispone di tutto il codice che necessario a portata di mano.</span><span class="sxs-lookup"><span data-stu-id="07874-394">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="07874-395">Il documento lab consentirà di determinare esattamente quando è possibile utilizzarli, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="07874-395">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="07874-396">![Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto](aspnet-mvc-4-dependency-injection/_static/image19.png "frammenti di codice tramite Visual Studio per inserire codice nel progetto")</span><span class="sxs-lookup"><span data-stu-id="07874-396">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="07874-397">*Utilizzo di frammenti di codice di Visual Studio per inserire il codice nel progetto*</span><span class="sxs-lookup"><span data-stu-id="07874-397">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="07874-398">***Per aggiungere un frammento di codice utilizzando la tastiera (solo c#)***</span><span class="sxs-lookup"><span data-stu-id="07874-398">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="07874-399">Posizionare il cursore dove si desidera inserire il codice.</span><span class="sxs-lookup"><span data-stu-id="07874-399">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="07874-400">Iniziare a digitare il nome del frammento di codice (senza spazi o segni meno).</span><span class="sxs-lookup"><span data-stu-id="07874-400">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="07874-401">Osservare come IntelliSense consente di visualizzare i nomi dei frammenti di codice corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="07874-401">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="07874-402">Selezionare il frammento corretto (o continuare a digitare fino a quando non viene selezionato il nome del frammento intero).</span><span class="sxs-lookup"><span data-stu-id="07874-402">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="07874-403">Premere il tasto Tab due volte per inserire il frammento di codice nella posizione del cursore.</span><span class="sxs-lookup"><span data-stu-id="07874-403">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="07874-404">![Iniziare a digitare il nome del frammento di codice](aspnet-mvc-4-dependency-injection/_static/image20.png "inizia a digitare il nome del frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="07874-404">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="07874-405">*Iniziare a digitare il nome del frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="07874-405">*Start typing the snippet name*</span></span>

<span data-ttu-id="07874-406">![Premere Tab per selezionare il frammento di codice evidenziata](aspnet-mvc-4-dependency-injection/_static/image21.png "premere Tab per selezionare il frammento evidenziato")</span><span class="sxs-lookup"><span data-stu-id="07874-406">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="07874-407">*Premere Tab per selezionare il frammento di codice evidenziata*</span><span class="sxs-lookup"><span data-stu-id="07874-407">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="07874-408">![Premere Tab nuovamente e il frammento di codice verranno espansi](aspnet-mvc-4-dependency-injection/_static/image22.png "espanderà premere Tab nuovamente e il frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="07874-408">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="07874-409">*Premere Tab nuovamente e il frammento di codice verranno espansi*</span><span class="sxs-lookup"><span data-stu-id="07874-409">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="07874-410">***Per aggiungere un frammento di codice utilizzando il mouse (c#, Visual Basic e XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="07874-410">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="07874-411">Fare clic in cui si desidera inserire il frammento di codice.</span><span class="sxs-lookup"><span data-stu-id="07874-411">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="07874-412">Selezionare **Inserisci frammento di codice** seguito da **frammenti di codice**.</span><span class="sxs-lookup"><span data-stu-id="07874-412">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="07874-413">Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso.</span><span class="sxs-lookup"><span data-stu-id="07874-413">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="07874-414">![Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice](aspnet-mvc-4-dependency-injection/_static/image23.png "rapida in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice")</span><span class="sxs-lookup"><span data-stu-id="07874-414">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="07874-415">*Pulsante destro del mouse in cui si desidera inserire il frammento di codice e selezionare Inserisci frammento di codice*</span><span class="sxs-lookup"><span data-stu-id="07874-415">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="07874-416">![Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso](aspnet-mvc-4-dependency-injection/_static/image24.png "selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso")</span><span class="sxs-lookup"><span data-stu-id="07874-416">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="07874-417">*Selezionare il frammento di codice rilevante dall'elenco, facendo clic su di esso*</span><span class="sxs-lookup"><span data-stu-id="07874-417">*Pick the relevant snippet from the list, by clicking on it*</span></span>
