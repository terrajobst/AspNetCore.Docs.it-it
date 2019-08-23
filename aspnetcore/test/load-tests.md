---
title: ASP.NET Core test di carico/stress
author: Jeremy-Meng
description: Informazioni sui diversi strumenti e approcci rilevanti per test di carico e test di stress ASP.NET Core app.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 7a9dfc1fedf747ab26daa573b61ed01c31709058
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975237"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="ced75-103">ASP.NET Core test di carico/stress</span><span class="sxs-lookup"><span data-stu-id="ced75-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="ced75-104">Test di carico e test di stress sono importanti per garantire che un'app Web sia efficiente e scalabile.</span><span class="sxs-lookup"><span data-stu-id="ced75-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="ced75-105">I rispettivi obiettivi sono diversi anche se condividono spesso test simili.</span><span class="sxs-lookup"><span data-stu-id="ced75-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="ced75-106">**Test di carico** &ndash; Verificare se l'app è in grado di gestire un carico specificato di utenti per un determinato scenario soddisfacendo comunque l'obiettivo della risposta.</span><span class="sxs-lookup"><span data-stu-id="ced75-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="ced75-107">L'app viene eseguita in condizioni normali.</span><span class="sxs-lookup"><span data-stu-id="ced75-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="ced75-108">**Test di stress** &ndash; Testare la stabilità dell'app quando viene eseguita in condizioni estreme, spesso per un lungo periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="ced75-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="ced75-109">I test inseriscono un carico utente elevato, picchi o aumento graduale del carico, sull'app o limitano le risorse di elaborazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="ced75-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="ced75-110">I test di stress determinano se un'app in stress può recuperare da un errore e restituire normalmente il comportamento previsto.</span><span class="sxs-lookup"><span data-stu-id="ced75-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="ced75-111">In condizioni di stress, l'app non viene eseguita in condizioni normali.</span><span class="sxs-lookup"><span data-stu-id="ced75-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="ced75-112">Visual Studio 2019 è l'ultima versione di Visual Studio con le funzionalità del test di carico.</span><span class="sxs-lookup"><span data-stu-id="ced75-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="ced75-113">Per i clienti che necessitano di strumenti di test di carico in futuro, è consigliabile usare strumenti alternativi, ad esempio Apache JMeter, Akamai CloudTest e BlazeMeter.</span><span class="sxs-lookup"><span data-stu-id="ced75-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="ced75-114">Per ulteriori informazioni, vedere le [Note sulla versione di Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span><span class="sxs-lookup"><span data-stu-id="ced75-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span></span>

<span data-ttu-id="ced75-115">Il servizio di test di carico in Azure DevOps termina in 2020.</span><span class="sxs-lookup"><span data-stu-id="ced75-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="ced75-116">Per altre informazioni, vedere [End of Life del servizio di test di carico basato sul cloud](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="ced75-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="ced75-117">Strumenti di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ced75-117">Visual Studio tools</span></span>

<span data-ttu-id="ced75-118">Visual Studio consente agli utenti di creare, sviluppare ed eseguire il debug di test di carico e prestazioni Web.</span><span class="sxs-lookup"><span data-stu-id="ced75-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="ced75-119">È disponibile un'opzione per la creazione di test mediante la registrazione di azioni in un Web browser.</span><span class="sxs-lookup"><span data-stu-id="ced75-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="ced75-120">Per informazioni su come creare, configurare ed eseguire un progetto di test di carico usando Visual Studio 2017, vedere [Guida introduttiva: Creare un progetto di test di carico](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="ced75-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span>

<span data-ttu-id="ced75-121">I test di carico possono essere configurati per l'esecuzione in locale o in esecuzione nel cloud usando Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="ced75-121">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="ced75-122">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="ced75-122">Azure DevOps</span></span>

<span data-ttu-id="ced75-123">È possibile avviare le esecuzioni dei test di carico usando il servizio [Test Plans di Azure DevOps](/azure/devops/test/load-test/index?view=vsts) .</span><span class="sxs-lookup"><span data-stu-id="ced75-123">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Pagina di destinazione del test di carico di Azure DevOps](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="ced75-125">Il servizio supporta i formati di test seguenti:</span><span class="sxs-lookup"><span data-stu-id="ced75-125">The service supports the following test formats:</span></span>

* <span data-ttu-id="ced75-126">Test Web &ndash; di Visual Studio creato in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ced75-126">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="ced75-127">Il traffico &ndash; http acquisito nell'archivio http viene riprodotto durante i test.</span><span class="sxs-lookup"><span data-stu-id="ced75-127">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="ced75-128">[Basato su URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Consente di specificare gli URL per il test di carico, i tipi di richiesta, le intestazioni e le stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="ced75-128">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="ced75-129">È possibile configurare parametri di impostazione di esecuzione, ad esempio durata, modello di carico e numero di utenti.</span><span class="sxs-lookup"><span data-stu-id="ced75-129">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="ced75-130">[Apache JMeter](https://jmeter.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="ced75-130">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="ced75-131">Portale di Azure</span><span class="sxs-lookup"><span data-stu-id="ced75-131">Azure portal</span></span>

<span data-ttu-id="ced75-132">[Portale di Azure consente la configurazione e l'esecuzione del test di carico delle app Web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) direttamente dalla scheda **prestazioni** del servizio app in portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="ced75-132">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Servizio app Azure in portale di Azure](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="ced75-134">Il test può essere un test manuale con un URL specificato o un file di test Web di Visual Studio, che può testare più URL.</span><span class="sxs-lookup"><span data-stu-id="ced75-134">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Pagina nuovo test delle prestazioni in portale di Azure](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="ced75-136">Alla fine del test, i report generati mostrano le caratteristiche di prestazioni dell'app.</span><span class="sxs-lookup"><span data-stu-id="ced75-136">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="ced75-137">Le statistiche di esempio includono:</span><span class="sxs-lookup"><span data-stu-id="ced75-137">Example statistics include:</span></span>

* <span data-ttu-id="ced75-138">Tempo di risposta medio</span><span class="sxs-lookup"><span data-stu-id="ced75-138">Average response time</span></span>
* <span data-ttu-id="ced75-139">Velocità effettiva massima: richieste al secondo</span><span class="sxs-lookup"><span data-stu-id="ced75-139">Max throughput: requests per second</span></span>
* <span data-ttu-id="ced75-140">Percentuale errori</span><span class="sxs-lookup"><span data-stu-id="ced75-140">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="ced75-141">Strumenti di terze parti</span><span class="sxs-lookup"><span data-stu-id="ced75-141">Third-party tools</span></span>

<span data-ttu-id="ced75-142">L'elenco seguente contiene gli strumenti per le prestazioni Web di terze parti con diversi set di funzionalità:</span><span class="sxs-lookup"><span data-stu-id="ced75-142">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="ced75-143">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="ced75-143">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="ced75-144">ApacheBench (AB)</span><span class="sxs-lookup"><span data-stu-id="ced75-144">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="ced75-145">Gatling</span><span class="sxs-lookup"><span data-stu-id="ced75-145">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="ced75-146">Locusta</span><span class="sxs-lookup"><span data-stu-id="ced75-146">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="ced75-147">Websurge di Wind West</span><span class="sxs-lookup"><span data-stu-id="ced75-147">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="ced75-148">Netling</span><span class="sxs-lookup"><span data-stu-id="ced75-148">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="ced75-149">Vegeta</span><span class="sxs-lookup"><span data-stu-id="ced75-149">Vegeta</span></span>](https://github.com/tsenart/vegeta)
