---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Gli unit test ASP.NET Web API 2 | Microsoft Docs
author: tfitzmac
description: Questo materiale sussidiario e applicazione illustrano come creare semplici unit test per l'applicazione API Web 2. Questa esercitazione Mostra come includere un proj di unit test...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0a3f9ddde725995359de85c5e0783106f5de02bd
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829315"
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="194ca-104">Unit test ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="194ca-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="194ca-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="194ca-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="194ca-106">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="194ca-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="194ca-107">Questo materiale sussidiario e applicazione illustrano come creare semplici unit test per l'applicazione API Web 2.</span><span class="sxs-lookup"><span data-stu-id="194ca-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="194ca-108">Questa esercitazione illustra come includere un progetto unit test nella soluzione e scrivere i metodi di test che consentono di controllare i valori restituiti da un metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="194ca-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="194ca-109">Questa esercitazione presuppone che si abbia familiarità con i concetti di base dell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="194ca-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="194ca-110">Per un'esercitazione introduttiva, vedere [Introduzione a ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="194ca-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="194ca-111">Gli unit test in questo argomento sono intenzionalmente limitato a scenari di dati semplici.</span><span class="sxs-lookup"><span data-stu-id="194ca-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="194ca-112">Per gli unit test di scenari di dati più avanzati, vedere [comportamento fittizio di Entity Framework quando Unit test ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="194ca-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="194ca-113">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="194ca-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="194ca-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="194ca-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="194ca-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="194ca-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="194ca-116">Contenuto dell'argomento</span><span class="sxs-lookup"><span data-stu-id="194ca-116">In this topic</span></span>

<span data-ttu-id="194ca-117">Di seguito sono elencate le diverse sezioni di questo argomento:</span><span class="sxs-lookup"><span data-stu-id="194ca-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="194ca-118">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="194ca-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="194ca-119">Scaricare il codice</span><span class="sxs-lookup"><span data-stu-id="194ca-119">Download code</span></span>](#download)
- [<span data-ttu-id="194ca-120">Creare applicazioni con progetto unit test</span><span class="sxs-lookup"><span data-stu-id="194ca-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="194ca-121">Aggiungi progetto di unit test, la creazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="194ca-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="194ca-122">Aggiungi progetto di unit test per un'applicazione esistente</span><span class="sxs-lookup"><span data-stu-id="194ca-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="194ca-123">Configurare l'applicazione API Web 2</span><span class="sxs-lookup"><span data-stu-id="194ca-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="194ca-124">Installare i pacchetti NuGet nel progetto di test</span><span class="sxs-lookup"><span data-stu-id="194ca-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="194ca-125">Creare test</span><span class="sxs-lookup"><span data-stu-id="194ca-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="194ca-126">Eseguire i test</span><span class="sxs-lookup"><span data-stu-id="194ca-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="194ca-127">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="194ca-127">Prerequisites</span></span>

<span data-ttu-id="194ca-128">Visual Studio 2017 Community, Professional o Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="194ca-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="194ca-129">Scaricare il codice</span><span class="sxs-lookup"><span data-stu-id="194ca-129">Download code</span></span>

<span data-ttu-id="194ca-130">Scaricare il [progetto completato](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="194ca-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="194ca-131">Il progetto scaricabile include codice degli unit test per questo argomento e per il [comportamento fittizio di Entity Framework quando Unit test ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) argomento.</span><span class="sxs-lookup"><span data-stu-id="194ca-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="194ca-132">Creare applicazioni con progetto unit test</span><span class="sxs-lookup"><span data-stu-id="194ca-132">Create application with unit test project</span></span>

<span data-ttu-id="194ca-133">È possibile creare un progetto unit test durante la creazione dell'applicazione o aggiungere un progetto unit test per un'applicazione esistente.</span><span class="sxs-lookup"><span data-stu-id="194ca-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="194ca-134">Questa esercitazione illustra entrambi i metodi per la creazione di un progetto unit test.</span><span class="sxs-lookup"><span data-stu-id="194ca-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="194ca-135">Per seguire questa esercitazione, è possibile usare entrambi gli approcci.</span><span class="sxs-lookup"><span data-stu-id="194ca-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="194ca-136">Aggiungi progetto di unit test, la creazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="194ca-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="194ca-137">Creare una nuova applicazione Web ASP.NET denominato **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="194ca-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![Crea progetto](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="194ca-139">Nelle finestre del nuovo progetto ASP.NET, selezionare la **vuoto** modello e aggiungere le cartelle e riferimenti di base per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="194ca-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="194ca-140">Selezionare il **aggiungere gli unit test** opzione.</span><span class="sxs-lookup"><span data-stu-id="194ca-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="194ca-141">Progetto di unit test viene automaticamente denominato **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="194ca-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="194ca-142">È possibile mantenere questo nome.</span><span class="sxs-lookup"><span data-stu-id="194ca-142">You can keep this name.</span></span>

![creare progetto unit test](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="194ca-144">Dopo aver creato l'applicazione, si noterà che contiene due progetti.</span><span class="sxs-lookup"><span data-stu-id="194ca-144">After creating the application, you will see it contains two projects.</span></span>

![due progetti](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="194ca-146">Aggiungi progetto di unit test per un'applicazione esistente</span><span class="sxs-lookup"><span data-stu-id="194ca-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="194ca-147">Se non è stato creato il progetto di unit test quando è stata creata l'applicazione, è possibile aggiungerne uno in qualsiasi momento.</span><span class="sxs-lookup"><span data-stu-id="194ca-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="194ca-148">Si supponga, ad esempio, si dispone già di un'applicazione denominata StoreApp e si desidera aggiungere gli unit test.</span><span class="sxs-lookup"><span data-stu-id="194ca-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="194ca-149">Per aggiungere un progetto unit test, la soluzione e scegliere **Add** e **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="194ca-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Aggiungi nuovo progetto alla soluzione](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="194ca-151">Selezionare **Test** nel riquadro a sinistra e scegliere **progetto di Unit Test** per il tipo di progetto.</span><span class="sxs-lookup"><span data-stu-id="194ca-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="194ca-152">Denominare il progetto **StoreApp.Tests**.</span><span class="sxs-lookup"><span data-stu-id="194ca-152">Name the project **StoreApp.Tests**.</span></span>

![Aggiungi progetto di unit test](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="194ca-154">Si noterà il progetto di unit test nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="194ca-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="194ca-155">Nel progetto di unit test, aggiungere un riferimento progetto a progetto originale.</span><span class="sxs-lookup"><span data-stu-id="194ca-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="194ca-156">Configurare l'applicazione API Web 2</span><span class="sxs-lookup"><span data-stu-id="194ca-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="194ca-157">Nel progetto StoreApp, aggiungere un file di classe per il **modelli** cartella denominata **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="194ca-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="194ca-158">Sostituire il contenuto del file con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="194ca-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="194ca-159">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="194ca-159">Build the solution.</span></span>

<span data-ttu-id="194ca-160">Fare clic sulla cartella controller e selezionare **Add** e **nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="194ca-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="194ca-161">Selezionare **Web API 2 Controller - vuoto**.</span><span class="sxs-lookup"><span data-stu-id="194ca-161">Select **Web API 2 Controller - Empty**.</span></span>

![Aggiungere nuovo controller](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="194ca-163">Impostare il nome del controller **SimpleProductController**, fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="194ca-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![specificare controller](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="194ca-165">Sostituire il codice esistente con quello seguente.</span><span class="sxs-lookup"><span data-stu-id="194ca-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="194ca-166">Per semplificare questo esempio, i dati vengono archiviati in un elenco anziché un database.</span><span class="sxs-lookup"><span data-stu-id="194ca-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="194ca-167">L'elenco definito in questa classe rappresenta i dati di produzione.</span><span class="sxs-lookup"><span data-stu-id="194ca-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="194ca-168">Si noti che il controller include un costruttore che accetta come parametro un elenco di oggetti Product.</span><span class="sxs-lookup"><span data-stu-id="194ca-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="194ca-169">Questo costruttore consente di passare dati di test quando gli unit test.</span><span class="sxs-lookup"><span data-stu-id="194ca-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="194ca-170">Il controller include anche due **async** metodi per illustrare gli unit test di metodi asincroni.</span><span class="sxs-lookup"><span data-stu-id="194ca-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="194ca-171">Questi metodi asincroni sono stati implementati chiamando **Task.FromResult** per ridurre al minimo il codice di estraneo, ma in genere i metodi includono operazioni a elevato utilizzo di risorse.</span><span class="sxs-lookup"><span data-stu-id="194ca-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="194ca-172">Il metodo GetProduct restituisce un'istanza di **IHttpActionResult** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="194ca-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="194ca-173">IHttpActionResult è una delle nuove funzionalità di Web API 2 e semplifica lo sviluppo di unit test.</span><span class="sxs-lookup"><span data-stu-id="194ca-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="194ca-174">Classi che implementano l'interfaccia IHttpActionResult si trovano nel [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="194ca-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="194ca-175">Queste classi rappresentano le possibili risposte da una richiesta di azione e corrispondono ai codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="194ca-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="194ca-176">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="194ca-176">Build the solution.</span></span>

<span data-ttu-id="194ca-177">A questo punto si è pronti configurare il progetto di test.</span><span class="sxs-lookup"><span data-stu-id="194ca-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="194ca-178">Installare i pacchetti NuGet nel progetto di test</span><span class="sxs-lookup"><span data-stu-id="194ca-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="194ca-179">Quando si usa il modello vuoto per creare un'applicazione, il progetto di unit test (StoreApp.Tests) non include i pacchetti NuGet installati.</span><span class="sxs-lookup"><span data-stu-id="194ca-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="194ca-180">Altri modelli, ad esempio il modello API Web, includono alcuni pacchetti NuGet nel progetto di unit test.</span><span class="sxs-lookup"><span data-stu-id="194ca-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="194ca-181">Per questa esercitazione, è necessario includere il pacchetto di Microsoft ASP.NET Web API 2 Core per il progetto di test.</span><span class="sxs-lookup"><span data-stu-id="194ca-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="194ca-182">Fare clic sul progetto StoreApp.Tests e selezionare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="194ca-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="194ca-183">È necessario selezionare il progetto StoreApp.Tests per aggiungere i pacchetti al progetto.</span><span class="sxs-lookup"><span data-stu-id="194ca-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![gestire i pacchetti](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="194ca-185">Trovare e installare il pacchetto di Microsoft ASP.NET Web API 2 Core.</span><span class="sxs-lookup"><span data-stu-id="194ca-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![installare il pacchetto principale di api web](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="194ca-187">Chiudere la finestra Gestisci pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="194ca-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="194ca-188">Creare test</span><span class="sxs-lookup"><span data-stu-id="194ca-188">Create tests</span></span>

<span data-ttu-id="194ca-189">Per impostazione predefinita, il progetto di test include un file di test vuoto denominato UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="194ca-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="194ca-190">Questo file indica gli attributi da utilizzare per creare metodi di test.</span><span class="sxs-lookup"><span data-stu-id="194ca-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="194ca-191">Per gli unit test, è possibile usare questo file o creare un file.</span><span class="sxs-lookup"><span data-stu-id="194ca-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="194ca-193">Per questa esercitazione si creerà la propria classe di test.</span><span class="sxs-lookup"><span data-stu-id="194ca-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="194ca-194">È possibile eliminare il file UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="194ca-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="194ca-195">Aggiungere una classe denominata **TestSimpleProductController.cs**e sostituire il codice con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="194ca-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="194ca-196">Esegui test</span><span class="sxs-lookup"><span data-stu-id="194ca-196">Run tests</span></span>

<span data-ttu-id="194ca-197">A questo punto si è pronti eseguire i test.</span><span class="sxs-lookup"><span data-stu-id="194ca-197">You are now ready to run the tests.</span></span> <span data-ttu-id="194ca-198">Tutti il metodo che sono contrassegnati con il **TestMethod** attributo verrà testato.</span><span class="sxs-lookup"><span data-stu-id="194ca-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="194ca-199">Dal **Test** voce di menu, eseguire i test.</span><span class="sxs-lookup"><span data-stu-id="194ca-199">From the **Test** menu item, run the tests.</span></span>

![eseguire test](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="194ca-201">Aprire il **Esplora Test** finestra e osservare i risultati dei test.</span><span class="sxs-lookup"><span data-stu-id="194ca-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![risultati test](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="194ca-203">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="194ca-203">Summary</span></span>

<span data-ttu-id="194ca-204">Questa esercitazione è stata completata.</span><span class="sxs-lookup"><span data-stu-id="194ca-204">You have completed this tutorial.</span></span> <span data-ttu-id="194ca-205">I dati in questa esercitazione è stati semplificati intenzionalmente focalizzare l'attenzione sulle condizioni di testing unità.</span><span class="sxs-lookup"><span data-stu-id="194ca-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="194ca-206">Per gli unit test di scenari di dati più avanzati, vedere [comportamento fittizio di Entity Framework quando Unit test ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="194ca-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
