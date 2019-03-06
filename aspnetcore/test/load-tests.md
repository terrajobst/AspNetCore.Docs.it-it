---
title: Test di carico/stress di ASP.NET Core
author: Jeremy-Meng
description: Vengono descritti alcuni importanti strumenti e approcci per test di carico e delle App ASP.NET Core di test di stress.
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 587df6e216943d3eeec779df4d0554dd0fc2fda0
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345428"
---
# <a name="load-and-stress-testing-aspnet-core"></a><span data-ttu-id="7062b-103">Caricare e di stress di test ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7062b-103">Load and stress testing ASP.NET Core</span></span>

<span data-ttu-id="7062b-104">Test di stress e test di carico sono importanti per garantire che un'app web sia efficiente e scalabile.</span><span class="sxs-lookup"><span data-stu-id="7062b-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="7062b-105">Gli obiettivi sono diversi anche condividono spesso test analoghi.</span><span class="sxs-lookup"><span data-stu-id="7062b-105">Their goals are different even they often share similar tests.</span></span>

<span data-ttu-id="7062b-106">**Test di carico**: Verifica se l'app può gestire un carico di utenti per un determinato scenario specificato mentre continuando comunque a soddisfare l'obiettivo di risposta.</span><span class="sxs-lookup"><span data-stu-id="7062b-106">**Load tests**: Tests whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="7062b-107">L'app viene eseguita in condizioni normali.</span><span class="sxs-lookup"><span data-stu-id="7062b-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="7062b-108">**Test di stress**: Stabilità di app di test durante l'esecuzione in condizioni estreme e spesso un lungo periodo di tempo:</span><span class="sxs-lookup"><span data-stu-id="7062b-108">**Stress tests**: Tests app stability when running under extreme conditions and often a long period of time:</span></span>

* <span data-ttu-id="7062b-109">Carico elevato di utenti: i picchi o aumentare gradualmente.</span><span class="sxs-lookup"><span data-stu-id="7062b-109">High user load – either spikes or gradually increasing.</span></span>
* <span data-ttu-id="7062b-110">Risorse di elaborazione limitate.</span><span class="sxs-lookup"><span data-stu-id="7062b-110">Limited computing resources.</span></span>  

<span data-ttu-id="7062b-111">In condizioni di stress, può recuperare da un errore dell'app e normalmente tornare al comportamento previsto?</span><span class="sxs-lookup"><span data-stu-id="7062b-111">Under stress, can the app recover from failure and gracefully return to expected behavior?</span></span> <span data-ttu-id="7062b-112">In condizioni di stress, l'app si trova *non* eseguiti in condizioni normali.</span><span class="sxs-lookup"><span data-stu-id="7062b-112">Under stress, the app is *not* run under normal conditions.</span></span>

<span data-ttu-id="7062b-113">Visual Studio 2019 sarà l'ultima versione di Visual Studio con le funzionalità di test di carico.</span><span class="sxs-lookup"><span data-stu-id="7062b-113">Visual Studio 2019 will be the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="7062b-114">Per i clienti che hanno bisogno di strumenti di test di carico, è consigliabile usare strumenti alternativi, ad esempio Apache JMeter, Akamai CloudTest e Blazemeter.</span><span class="sxs-lookup"><span data-stu-id="7062b-114">For customers requiring load testing tools, we recommend using alternate load testing tools such as Apache JMeter, Akamai CloudTest, Blazemeter.</span></span> <span data-ttu-id="7062b-115">Per altre informazioni, vedere la [note sulla versione di anteprima di Visual Studio 2019](/visualstudio/releases/2019/release-notes-preview#test-tools).</span><span class="sxs-lookup"><span data-stu-id="7062b-115">For more information, see the [Visual Studio 2019 Preview Release Notes](/visualstudio/releases/2019/release-notes-preview#test-tools).</span></span>

<span data-ttu-id="7062b-116">Il test di carico del servizio in Azure DevOps termina in 2020.</span><span class="sxs-lookup"><span data-stu-id="7062b-116">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="7062b-117">Per altre informazioni, vedere [lato servizio del ciclo di vita di test di carico basati sul Cloud](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="7062b-117">For more information see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="7062b-118">Strumenti di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7062b-118">Visual Studio Tools</span></span>

<span data-ttu-id="7062b-119">Visual Studio consente agli utenti di creare, sviluppare ed eseguire il debug di test di carico e prestazioni web.</span><span class="sxs-lookup"><span data-stu-id="7062b-119">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="7062b-120">Un'opzione è disponibile per creare i test tramite la registrazione delle azioni nel web browser.</span><span class="sxs-lookup"><span data-stu-id="7062b-120">An option is available to create tests by recording actions in web browser.</span></span>

<span data-ttu-id="7062b-121">[Avvio rapido: Creare un progetto di test di carico](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) viene illustrato come creare, configurare ed eseguire un test di carico i progetti che usano Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="7062b-121">[Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) shows how to create, configure, and run a load test projects using Visual Studio 2017.</span></span>

<span data-ttu-id="7062b-122">Per altre informazioni, vedere [Risorse aggiuntive](#add).</span><span class="sxs-lookup"><span data-stu-id="7062b-122">See [Additional Resources](#add) for more information.</span></span>

<span data-ttu-id="7062b-123">I test di carico possono essere configurati per eseguire in locale o nel cloud usando Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="7062b-123">Load tests can be configured to run in on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="7062b-124">DevOps di Azure</span><span class="sxs-lookup"><span data-stu-id="7062b-124">Azure DevOps</span></span>

<span data-ttu-id="7062b-125">Esecuzioni test di carico possono essere avviati tramite il [piani di Test di Azure DevOps](/azure/devops/test/load-test/index?view=vsts) servizio.</span><span class="sxs-lookup"><span data-stu-id="7062b-125">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="7062b-126">Il servizio supporta i tipi di formato di test seguenti:</span><span class="sxs-lookup"><span data-stu-id="7062b-126">The service supports the following types of test format:</span></span>

- <span data-ttu-id="7062b-127">Test di Visual Studio – test web creati in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7062b-127">Visual Studio test – web test created in Visual Studio.</span></span>
- <span data-ttu-id="7062b-128">Riproduzione di test basato su archivio HTTP: il traffico HTTP acquisito all'interno di archivio viene eseguito durante il test.</span><span class="sxs-lookup"><span data-stu-id="7062b-128">HTTP Archive-based test – captured HTTP traffic inside archive is replayed during testing.</span></span>
- <span data-ttu-id="7062b-129">[Test basato su URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – consente di specificare gli URL per il caricamento dei test, i tipi di richiesta, le intestazioni e le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="7062b-129">[URL-based test](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="7062b-130">Eseguire l'impostazione dei parametri, ad esempio durata, può essere configurato il modello di carico, numero di utenti e così via.</span><span class="sxs-lookup"><span data-stu-id="7062b-130">Run setting parameters such as duration, load pattern, number of users, etc., can be configured.</span></span>
- <span data-ttu-id="7062b-131">[Apache JMeter](https://jmeter.apache.org/) di test.</span><span class="sxs-lookup"><span data-stu-id="7062b-131">[Apache JMeter](https://jmeter.apache.org/) test.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="7062b-132">portale di Azure</span><span class="sxs-lookup"><span data-stu-id="7062b-132">Azure portal</span></span>

<span data-ttu-id="7062b-133">[Portale di Azure consente di configurare ed eseguire il test di carico dell'App Web,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) direttamente dalla scheda delle prestazioni del servizio App nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="7062b-133">[Azure portal allows setting up and running load testing of Web Apps,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the Performance tab of the App Service in Azure portal.</span></span>

![](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="7062b-134">Il test può essere un test manuale con un URL specificato o un file di Test con Visual Studio Web, che è possibile testare più URL.</span><span class="sxs-lookup"><span data-stu-id="7062b-134">The test can be a manual test with a specified URL, or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="7062b-135">Alla fine del test, vengono generati report per mostrare le caratteristiche delle prestazioni dell'app.</span><span class="sxs-lookup"><span data-stu-id="7062b-135">At end of the test, reports are generated to show the performance characteristics of the app.</span></span> <span data-ttu-id="7062b-136">Le statistiche di esempio includono:</span><span class="sxs-lookup"><span data-stu-id="7062b-136">Example statistics include:</span></span>

- <span data-ttu-id="7062b-137">Tempo medio di risposta</span><span class="sxs-lookup"><span data-stu-id="7062b-137">Average response time</span></span>
- <span data-ttu-id="7062b-138">Velocità effettiva massima: le richieste al secondo</span><span class="sxs-lookup"><span data-stu-id="7062b-138">Max throughput: requests per second</span></span>
- <span data-ttu-id="7062b-139">Percentuale di errore</span><span class="sxs-lookup"><span data-stu-id="7062b-139">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="7062b-140">Strumenti di terze parti</span><span class="sxs-lookup"><span data-stu-id="7062b-140">Third-party Tools</span></span>

<span data-ttu-id="7062b-141">Nell'elenco seguente contiene gli strumenti delle prestazioni web di terze parti con diversi set di funzionalità:</span><span class="sxs-lookup"><span data-stu-id="7062b-141">The following list contains third-party web performance tools with various feature sets:</span></span>

- <span data-ttu-id="7062b-142">[Apache JMeter](https://jmeter.apache.org/) : In primo piano suite completa di strumenti di test di carico.</span><span class="sxs-lookup"><span data-stu-id="7062b-142">[Apache JMeter](https://jmeter.apache.org/) : Full featured suite of load testing tools.</span></span> <span data-ttu-id="7062b-143">Associata ai thread: è necessario un thread per ogni utente.</span><span class="sxs-lookup"><span data-stu-id="7062b-143">Thread-bound: need one thread per user.</span></span>
- [<span data-ttu-id="7062b-144">ab - server HTTP Apache benchmarking tool</span><span class="sxs-lookup"><span data-stu-id="7062b-144">ab - Apache HTTP server benchmarking tool</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
- <span data-ttu-id="7062b-145">[Gatling](https://gatling.io/) : Strumento desktop con i masterizzatori di interfaccia utente grafica e test.</span><span class="sxs-lookup"><span data-stu-id="7062b-145">[Gatling](https://gatling.io/) : Desktop tool with a GUI and test recorders.</span></span> <span data-ttu-id="7062b-146">Più efficiente rispetto alla JMeter.</span><span class="sxs-lookup"><span data-stu-id="7062b-146">More performant than JMeter.</span></span>
- <span data-ttu-id="7062b-147">[Locust.IO](https://locust.io/) : Non è limitato dai thread.</span><span class="sxs-lookup"><span data-stu-id="7062b-147">[Locust.io](https://locust.io/) : Not bounded by threads.</span></span>

<a name="add"></a>
## <a name="additional-resources"></a><span data-ttu-id="7062b-148">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7062b-148">Additional Resources</span></span>

<span data-ttu-id="7062b-149">[Serie di blog di Test di carico](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) di Charles Sterling.</span><span class="sxs-lookup"><span data-stu-id="7062b-149">[Load Test blog series](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) by Charles Sterling.</span></span> <span data-ttu-id="7062b-150">Con data di validità, ma la maggior parte degli argomenti sono ancora rilevante.</span><span class="sxs-lookup"><span data-stu-id="7062b-150">Dated but most of the topics are still relevant.</span></span>
