---
title: Test di carico/stress di ASP.NET Core
author: Jeremy-Meng
description: Informazioni sulle diverse importanti strumenti e approcci per il test di carico e delle App ASP.NET Core di test di stress.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: test/loadtests
ms.openlocfilehash: 3c21da6c799bc3080a1a16cb62ae4535b8890a1b
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724484"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="4ea30-103">Test di carico/stress di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4ea30-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="4ea30-104">Test di stress e test di carico sono importanti per garantire che un'app web sia efficiente e scalabile.</span><span class="sxs-lookup"><span data-stu-id="4ea30-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="4ea30-105">Gli obiettivi sono diversi, anche se condividono spesso test analoghi.</span><span class="sxs-lookup"><span data-stu-id="4ea30-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="4ea30-106">**Test di carico** &ndash; verificare se l'app può gestire un carico di utenti per un determinato scenario specificato mentre continuando comunque a soddisfare l'obiettivo di risposta.</span><span class="sxs-lookup"><span data-stu-id="4ea30-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="4ea30-107">L'app viene eseguita in condizioni normali.</span><span class="sxs-lookup"><span data-stu-id="4ea30-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="4ea30-108">**Test di stress** &ndash; testare la stabilità delle app durante l'esecuzione in condizioni estreme, spesso per un lungo periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="4ea30-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="4ea30-109">I test inserire carichi utente elevati, picchi o gradualmente crescente carico, nell'app oppure limitano le risorse di calcolo dell'app.</span><span class="sxs-lookup"><span data-stu-id="4ea30-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="4ea30-110">Test di stress determinare se un'app in condizioni di stress può recuperare da un errore e normalmente tornare al comportamento previsto.</span><span class="sxs-lookup"><span data-stu-id="4ea30-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="4ea30-111">In condizioni di stress, l'app non viene eseguita in condizioni normali.</span><span class="sxs-lookup"><span data-stu-id="4ea30-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="4ea30-112">Visual Studio 2019 è l'ultima versione di Visual Studio con le funzionalità di test di carico.</span><span class="sxs-lookup"><span data-stu-id="4ea30-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="4ea30-113">Per i clienti che richiedono in futuro gli strumenti di test di carico, è consigliabile strumenti alternativi, ad esempio Apache JMeter, Akamai CloudTest e BlazeMeter.</span><span class="sxs-lookup"><span data-stu-id="4ea30-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="4ea30-114">Per altre informazioni, vedere la [note sulla versione di Visual Studio 2019](/visualstudio/releases/2019/release-notes#test-tools).</span><span class="sxs-lookup"><span data-stu-id="4ea30-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes#test-tools).</span></span>

<span data-ttu-id="4ea30-115">Il test di carico del servizio in Azure DevOps termina in 2020.</span><span class="sxs-lookup"><span data-stu-id="4ea30-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="4ea30-116">Per altre informazioni, vedere [lato servizio del ciclo di vita di test di carico basati sul Cloud](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="4ea30-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="4ea30-117">Strumenti di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4ea30-117">Visual Studio tools</span></span>

<span data-ttu-id="4ea30-118">Visual Studio consente agli utenti di creare, sviluppare ed eseguire il debug di test di carico e prestazioni web.</span><span class="sxs-lookup"><span data-stu-id="4ea30-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="4ea30-119">Un'opzione è disponibile per creare i test tramite la registrazione delle azioni in un web browser.</span><span class="sxs-lookup"><span data-stu-id="4ea30-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="4ea30-120">Per informazioni su come creare, configurare ed eseguire un test di carico i progetti che usano Visual Studio 2017, vedere [Guida introduttiva: Creare un progetto di test di carico](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="4ea30-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span> <span data-ttu-id="4ea30-121">Per altre informazioni, vedere la [risorse aggiuntive](#additional-resources) sezione.</span><span class="sxs-lookup"><span data-stu-id="4ea30-121">For more information, see the [Additional resources](#additional-resources) section.</span></span>

<span data-ttu-id="4ea30-122">Test di carico possono essere configurati per l'esecuzione in locale o esecuzione nel cloud usando Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="4ea30-122">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="4ea30-123">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="4ea30-123">Azure DevOps</span></span>

<span data-ttu-id="4ea30-124">Esecuzioni test di carico possono essere avviati tramite il [piani di Test di Azure DevOps](/azure/devops/test/load-test/index?view=vsts) servizio.</span><span class="sxs-lookup"><span data-stu-id="4ea30-124">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Azure DevOps il test di carico pagina di destinazione](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="4ea30-126">Il servizio supporta i formati di test seguenti:</span><span class="sxs-lookup"><span data-stu-id="4ea30-126">The service supports the following test formats:</span></span>

* <span data-ttu-id="4ea30-127">Visual Studio &ndash; test Web creati in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ea30-127">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="4ea30-128">Archivio HTTP &ndash; il traffico HTTP acquisito all'interno di archivio viene riprodotto durante i test.</span><span class="sxs-lookup"><span data-stu-id="4ea30-128">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="4ea30-129">[Basato su URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; consente di specificare gli URL per il caricamento dei test, i tipi di richiesta, le intestazioni e le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="4ea30-129">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="4ea30-130">Eseguire l'impostazione dei parametri, ad esempio durata, il modello di carico e il numero di utenti può essere configurato.</span><span class="sxs-lookup"><span data-stu-id="4ea30-130">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="4ea30-131">[Apache JMeter](https://jmeter.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="4ea30-131">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="4ea30-132">portale di Azure</span><span class="sxs-lookup"><span data-stu-id="4ea30-132">Azure portal</span></span>

<span data-ttu-id="4ea30-133">[Portale di Azure consente di configurare ed eseguire il test di carico delle App web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) direttamente dai **prestazioni** scheda del servizio App nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="4ea30-133">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Servizio App di Azure nel portale di Azure](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="4ea30-135">Il test può essere un test manuale con un URL specificato o un file di Test con Visual Studio Web, che è possibile testare più URL.</span><span class="sxs-lookup"><span data-stu-id="4ea30-135">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Nuova pagina di Test delle prestazioni nel portale di Azure](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="4ea30-137">Alla fine del test, i report generati mostrano le caratteristiche delle prestazioni dell'app.</span><span class="sxs-lookup"><span data-stu-id="4ea30-137">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="4ea30-138">Le statistiche di esempio includono:</span><span class="sxs-lookup"><span data-stu-id="4ea30-138">Example statistics include:</span></span>

* <span data-ttu-id="4ea30-139">Tempo medio di risposta</span><span class="sxs-lookup"><span data-stu-id="4ea30-139">Average response time</span></span>
* <span data-ttu-id="4ea30-140">Velocità effettiva massima: le richieste al secondo</span><span class="sxs-lookup"><span data-stu-id="4ea30-140">Max throughput: requests per second</span></span>
* <span data-ttu-id="4ea30-141">Percentuale di errore</span><span class="sxs-lookup"><span data-stu-id="4ea30-141">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="4ea30-142">Strumenti di terze parti</span><span class="sxs-lookup"><span data-stu-id="4ea30-142">Third-party tools</span></span>

<span data-ttu-id="4ea30-143">Nell'elenco seguente contiene gli strumenti delle prestazioni web di terze parti con diversi set di funzionalità:</span><span class="sxs-lookup"><span data-stu-id="4ea30-143">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="4ea30-144">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="4ea30-144">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="4ea30-145">ApacheBench (ab)</span><span class="sxs-lookup"><span data-stu-id="4ea30-145">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="4ea30-146">Gatling</span><span class="sxs-lookup"><span data-stu-id="4ea30-146">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="4ea30-147">Locust</span><span class="sxs-lookup"><span data-stu-id="4ea30-147">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="4ea30-148">WebSurge Wind occidentale</span><span class="sxs-lookup"><span data-stu-id="4ea30-148">West Wind WebSurge</span></span>](http://websurge.west-wind.com/)
* [<span data-ttu-id="4ea30-149">Netling</span><span class="sxs-lookup"><span data-stu-id="4ea30-149">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="4ea30-150">Vegeta</span><span class="sxs-lookup"><span data-stu-id="4ea30-150">Vegeta</span></span>](https://github.com/tsenart/vegeta)

## <a name="additional-resources"></a><span data-ttu-id="4ea30-151">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4ea30-151">Additional resources</span></span>

* [<span data-ttu-id="4ea30-152">Serie di blog di Test di carico</span><span class="sxs-lookup"><span data-stu-id="4ea30-152">Load Test blog series</span></span>](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)
