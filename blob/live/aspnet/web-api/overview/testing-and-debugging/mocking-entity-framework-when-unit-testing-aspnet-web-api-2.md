---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Tali Entity Framework quando gli Unit test ASP.NET Web API 2 | Documenti Microsoft
author: tfitzmac
description: Questa Guida e l'applicazione viene illustrato come creare unit test per l'applicazione Web API 2 che utilizza Entity Framework. Viene illustrato come modificare il...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 2d8a3df94c91d2fac79006916375764c2b90dc85
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="bab6d-104">Tali Entity Framework quando gli Unit test ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="bab6d-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="bab6d-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bab6d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="bab6d-106">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="bab6d-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="bab6d-107">Questa Guida e l'applicazione viene illustrato come creare unit test per l'applicazione Web API 2 che utilizza Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bab6d-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="bab6d-108">Viene illustrato come modificare il controller di scaffolding per abilitare il passaggio di un oggetto di contesto per il testing e come creare gli oggetti di test che funzionano con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bab6d-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
> 
> <span data-ttu-id="bab6d-109">Per un'introduzione agli unit test con ASP.NET Web API, vedere [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bab6d-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
> 
> <span data-ttu-id="bab6d-110">In questa esercitazione si presuppone che si ha familiarità con i concetti di base dell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bab6d-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="bab6d-111">Per un'esercitazione introduttiva, vedere [Introduzione a ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bab6d-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bab6d-112">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="bab6d-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="bab6d-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="bab6d-113">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="bab6d-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="bab6d-114">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="bab6d-115">Contenuto dell'argomento</span><span class="sxs-lookup"><span data-stu-id="bab6d-115">In this topic</span></span>

<span data-ttu-id="bab6d-116">Di seguito sono elencate le diverse sezioni di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="bab6d-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="bab6d-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="bab6d-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="bab6d-118">Scaricare il codice</span><span class="sxs-lookup"><span data-stu-id="bab6d-118">Download code</span></span>](#download)
- [<span data-ttu-id="bab6d-119">Creare l'applicazione con progetto di unit test</span><span class="sxs-lookup"><span data-stu-id="bab6d-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="bab6d-120">Creare la classe modello</span><span class="sxs-lookup"><span data-stu-id="bab6d-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="bab6d-121">Aggiungere il controller</span><span class="sxs-lookup"><span data-stu-id="bab6d-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="bab6d-122">Aggiungere l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="bab6d-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="bab6d-123">Installare i pacchetti NuGet nel progetto di test</span><span class="sxs-lookup"><span data-stu-id="bab6d-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="bab6d-124">Creare il contesto del test</span><span class="sxs-lookup"><span data-stu-id="bab6d-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="bab6d-125">Creazione di test</span><span class="sxs-lookup"><span data-stu-id="bab6d-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="bab6d-126">Eseguire i test</span><span class="sxs-lookup"><span data-stu-id="bab6d-126">Run tests</span></span>](#runtests)

<span data-ttu-id="bab6d-127">Se sono stati già completati i passaggi descritti in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), è possibile passare alla sezione [aggiungere il controller](#controller).</span><span class="sxs-lookup"><span data-stu-id="bab6d-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="bab6d-128">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="bab6d-128">Prerequisites</span></span>

<span data-ttu-id="bab6d-129">Edizione di Visual Studio 2017 Community, Professional o Enterprise</span><span class="sxs-lookup"><span data-stu-id="bab6d-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="bab6d-130">Scaricare il codice</span><span class="sxs-lookup"><span data-stu-id="bab6d-130">Download code</span></span>

<span data-ttu-id="bab6d-131">Scaricare il [progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="bab6d-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="bab6d-132">Il progetto scaricabile include codice unit test per questo argomento e per il [Unit test ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) argomento.</span><span class="sxs-lookup"><span data-stu-id="bab6d-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="bab6d-133">Creare l'applicazione con progetto di unit test</span><span class="sxs-lookup"><span data-stu-id="bab6d-133">Create application with unit test project</span></span>

<span data-ttu-id="bab6d-134">È possibile creare un progetto di unit test quando si crea l'applicazione o aggiungere un progetto di unit test per un'applicazione esistente.</span><span class="sxs-lookup"><span data-stu-id="bab6d-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="bab6d-135">In questa esercitazione viene illustrato come creare un progetto di unit test quando si crea l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bab6d-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="bab6d-136">Creare una nuova applicazione Web ASP.NET denominata **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="bab6d-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="bab6d-137">Nelle finestre del nuovo progetto ASP.NET, selezionare il **vuoto** modello e aggiungere cartelle e i riferimenti di base per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="bab6d-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="bab6d-138">Selezionare il **aggiungere unit test** opzione.</span><span class="sxs-lookup"><span data-stu-id="bab6d-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="bab6d-139">Progetto di unit test viene automaticamente denominato **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="bab6d-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="bab6d-140">È possibile mantenere questo nome.</span><span class="sxs-lookup"><span data-stu-id="bab6d-140">You can keep this name.</span></span>

![creare progetto unit test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="bab6d-142">Dopo aver creato l'applicazione, si vedrà che contiene due progetti: **StoreApp** e **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="bab6d-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="bab6d-143">Creare la classe modello</span><span class="sxs-lookup"><span data-stu-id="bab6d-143">Create the model class</span></span>

<span data-ttu-id="bab6d-144">Nel progetto StoreApp, aggiungere un file di classe per il **modelli** cartella denominata **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="bab6d-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="bab6d-145">Sostituire il contenuto del file con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="bab6d-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="bab6d-146">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="bab6d-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="bab6d-147">Aggiungere il controller</span><span class="sxs-lookup"><span data-stu-id="bab6d-147">Add the controller</span></span>

<span data-ttu-id="bab6d-148">Fare clic sulla cartella controller e selezionare **Aggiungi** e **nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="bab6d-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="bab6d-149">Selezionare i Controller di Web API 2 con azioni, mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bab6d-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Aggiungere nuovo controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="bab6d-151">Impostare i seguenti valori:</span><span class="sxs-lookup"><span data-stu-id="bab6d-151">Set the following values:</span></span>

- <span data-ttu-id="bab6d-152">Nome del controller: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="bab6d-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="bab6d-153">Classe del modello: **prodotto**</span><span class="sxs-lookup"><span data-stu-id="bab6d-153">Model class: **Product**</span></span>
- <span data-ttu-id="bab6d-154">Classe del contesto dati: [selezionare **nuovo contesto dati** pulsante immette i valori illustrati di seguito]</span><span class="sxs-lookup"><span data-stu-id="bab6d-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![specificare controller](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="bab6d-156">Fare clic su **Aggiungi** per creare il controller con il codice generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="bab6d-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="bab6d-157">Il codice include metodi per la creazione, recupero, l'aggiornamento e l'eliminazione di istanze della classe del prodotto.</span><span class="sxs-lookup"><span data-stu-id="bab6d-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="bab6d-158">Nel codice seguente viene illustrato il metodo per aggiungere un prodotto.</span><span class="sxs-lookup"><span data-stu-id="bab6d-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="bab6d-159">Si noti che il metodo restituisce un'istanza di **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="bab6d-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="bab6d-160">IHttpActionResult è una delle nuove funzionalità di Web API 2 e semplifica lo sviluppo di unit test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="bab6d-161">Nella sezione successiva, si personalizzerà il codice generato per facilitare il passaggio di oggetti di test al controller.</span><span class="sxs-lookup"><span data-stu-id="bab6d-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="bab6d-162">Aggiungere l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="bab6d-162">Add dependency injection</span></span>

<span data-ttu-id="bab6d-163">Attualmente, la classe ProductController è hardcoded per utilizzare un'istanza della classe StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="bab6d-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="bab6d-164">Utilizzare un modello di inserimento di dipendenze per modificare l'applicazione e rimuovere tale dipendenza hardcoded.</span><span class="sxs-lookup"><span data-stu-id="bab6d-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="bab6d-165">Interrompendo la dipendenza, è possibile passare un oggetto fittizio durante il test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="bab6d-166">Fare doppio clic su di **modelli** cartella e aggiungere una nuova interfaccia denominata **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="bab6d-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="bab6d-167">Sostituire il codice con il seguente.</span><span class="sxs-lookup"><span data-stu-id="bab6d-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="bab6d-168">Aprire il file StoreAppContext.cs e apportare le seguenti modifiche evidenziate.</span><span class="sxs-lookup"><span data-stu-id="bab6d-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="bab6d-169">Le modifiche importanti da notare sono:</span><span class="sxs-lookup"><span data-stu-id="bab6d-169">The important changes to note are:</span></span>

- <span data-ttu-id="bab6d-170">Classe StoreAppContext implementa interfaccia IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="bab6d-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="bab6d-171">Metodo MarkAsModified implementato</span><span class="sxs-lookup"><span data-stu-id="bab6d-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="bab6d-172">Aprire il file ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="bab6d-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="bab6d-173">Modificare il codice esistente in modo che corrisponda il codice evidenziato.</span><span class="sxs-lookup"><span data-stu-id="bab6d-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="bab6d-174">Queste modifiche, interrompere la dipendenza su StoreAppContext e consentono alle altre classi passare un oggetto diverso per la classe del contesto.</span><span class="sxs-lookup"><span data-stu-id="bab6d-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="bab6d-175">Questa modifica consentirà di passare un contesto di test durante gli unit test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="bab6d-176">È presente una modifica di altre che è necessario apportare ProductController.</span><span class="sxs-lookup"><span data-stu-id="bab6d-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="bab6d-177">Nel **PutProduct** (metodo), sostituire con una chiamata al metodo MarkAsModified è modificata la riga che imposta lo stato dell'entità.</span><span class="sxs-lookup"><span data-stu-id="bab6d-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="bab6d-178">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="bab6d-178">Build the solution.</span></span>

<span data-ttu-id="bab6d-179">A questo punto si è pronti configurare il progetto di test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="bab6d-180">Installare i pacchetti NuGet nel progetto di test</span><span class="sxs-lookup"><span data-stu-id="bab6d-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="bab6d-181">Quando si utilizza il modello vuoto per creare un'applicazione, il progetto di unit test (StoreApp.Tests) non include tutti i pacchetti NuGet installati.</span><span class="sxs-lookup"><span data-stu-id="bab6d-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="bab6d-182">Altri modelli, ad esempio il modello API Web, includono alcuni pacchetti NuGet nel progetto di unit test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="bab6d-183">Per questa esercitazione, è necessario includere il packge Entity Framework e il pacchetto Microsoft ASP.NET Web API 2 Core per il progetto di test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="bab6d-184">Fare clic sul progetto StoreApp.Tests e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="bab6d-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="bab6d-185">È necessario selezionare il progetto StoreApp.Tests per aggiungere i pacchetti al progetto.</span><span class="sxs-lookup"><span data-stu-id="bab6d-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![gestire i pacchetti](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="bab6d-187">Dai pacchetti Online, trovare e installare il pacchetto EntityFramework (versione 6.0 o versione successiva).</span><span class="sxs-lookup"><span data-stu-id="bab6d-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="bab6d-188">Se risulta che il pacchetto EntityFramework sia già installato, potrebbe avere selezionato il progetto StoreApp invece del progetto StoreApp.Tests.</span><span class="sxs-lookup"><span data-stu-id="bab6d-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the the StoreApp.Tests project.</span></span>

![aggiunta di Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="bab6d-190">Trovare e installare il pacchetto Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="bab6d-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![installare il pacchetto di web api core](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="bab6d-192">Chiudere la finestra Gestisci pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="bab6d-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="bab6d-193">Creare il contesto del test</span><span class="sxs-lookup"><span data-stu-id="bab6d-193">Create test context</span></span>

<span data-ttu-id="bab6d-194">Aggiungere una classe denominata **TestDbSet** al progetto di test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="bab6d-195">Questa classe funge da classe base per il set di dati di test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="bab6d-196">Sostituire il codice con il seguente.</span><span class="sxs-lookup"><span data-stu-id="bab6d-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="bab6d-197">Aggiungere una classe denominata **TestProductDbSet** al progetto di test che contiene il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="bab6d-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="bab6d-198">Aggiungere una classe denominata **TestStoreAppContext** e sostituire il codice esistente con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="bab6d-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="bab6d-199">Creare test</span><span class="sxs-lookup"><span data-stu-id="bab6d-199">Create tests</span></span>

<span data-ttu-id="bab6d-200">Per impostazione predefinita, il progetto di test include un file di test a vuoto denominato **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="bab6d-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="bab6d-201">Questo file indica gli attributi da utilizzare per creare metodi di test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="bab6d-202">Per questa esercitazione, è possibile eliminare questo file consente di aggiungere una nuova classe di test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="bab6d-203">Aggiungere una classe denominata **TestProductController** al progetto di test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="bab6d-204">Sostituire il codice con il seguente.</span><span class="sxs-lookup"><span data-stu-id="bab6d-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="bab6d-205">Esegui test</span><span class="sxs-lookup"><span data-stu-id="bab6d-205">Run tests</span></span>

<span data-ttu-id="bab6d-206">A questo punto si è pronti eseguire i test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-206">You are now ready to run the tests.</span></span> <span data-ttu-id="bab6d-207">Tutti il metodo che sono contrassegnati con il **TestMethod** attributo verrà eseguito il test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="bab6d-208">Dal **Test** voce di menu, eseguire i test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-208">From the **Test** menu item, run the tests.</span></span>

![eseguire test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="bab6d-210">Aprire il **Esplora Test** finestra e notare che i risultati dei test.</span><span class="sxs-lookup"><span data-stu-id="bab6d-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![risultati test](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
