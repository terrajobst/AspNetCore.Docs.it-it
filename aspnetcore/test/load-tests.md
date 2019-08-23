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
# <a name="aspnet-core-loadstress-testing"></a>ASP.NET Core test di carico/stress

Test di carico e test di stress sono importanti per garantire che un'app Web sia efficiente e scalabile. I rispettivi obiettivi sono diversi anche se condividono spesso test simili.

**Test di carico** &ndash; Verificare se l'app è in grado di gestire un carico specificato di utenti per un determinato scenario soddisfacendo comunque l'obiettivo della risposta. L'app viene eseguita in condizioni normali.

**Test di stress** &ndash; Testare la stabilità dell'app quando viene eseguita in condizioni estreme, spesso per un lungo periodo di tempo. I test inseriscono un carico utente elevato, picchi o aumento graduale del carico, sull'app o limitano le risorse di elaborazione dell'app.

I test di stress determinano se un'app in stress può recuperare da un errore e restituire normalmente il comportamento previsto. In condizioni di stress, l'app non viene eseguita in condizioni normali.

Visual Studio 2019 è l'ultima versione di Visual Studio con le funzionalità del test di carico. Per i clienti che necessitano di strumenti di test di carico in futuro, è consigliabile usare strumenti alternativi, ad esempio Apache JMeter, Akamai CloudTest e BlazeMeter. Per ulteriori informazioni, vedere le [Note sulla versione di Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).

Il servizio di test di carico in Azure DevOps termina in 2020. Per altre informazioni, vedere [End of Life del servizio di test di carico basato sul cloud](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Strumenti di Visual Studio

Visual Studio consente agli utenti di creare, sviluppare ed eseguire il debug di test di carico e prestazioni Web. È disponibile un'opzione per la creazione di test mediante la registrazione di azioni in un Web browser.

Per informazioni su come creare, configurare ed eseguire un progetto di test di carico usando Visual Studio 2017, vedere [Guida introduttiva: Creare un progetto di test di carico](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).

I test di carico possono essere configurati per l'esecuzione in locale o in esecuzione nel cloud usando Azure DevOps.

## <a name="azure-devops"></a>Azure DevOps

È possibile avviare le esecuzioni dei test di carico usando il servizio [Test Plans di Azure DevOps](/azure/devops/test/load-test/index?view=vsts) .

![Pagina di destinazione del test di carico di Azure DevOps](./load-tests/_static/azure-devops-load-test.png)

Il servizio supporta i formati di test seguenti:

* Test Web &ndash; di Visual Studio creato in Visual Studio.
* Il traffico &ndash; http acquisito nell'archivio http viene riprodotto durante i test.
* [Basato su URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Consente di specificare gli URL per il test di carico, i tipi di richiesta, le intestazioni e le stringhe di query. È possibile configurare parametri di impostazione di esecuzione, ad esempio durata, modello di carico e numero di utenti.
* [Apache JMeter](https://jmeter.apache.org/).

## <a name="azure-portal"></a>Portale di Azure

[Portale di Azure consente la configurazione e l'esecuzione del test di carico delle app Web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) direttamente dalla scheda **prestazioni** del servizio app in portale di Azure.

![Servizio app Azure in portale di Azure](./load-tests/_static/azure-appservice-perf-test.png)

Il test può essere un test manuale con un URL specificato o un file di test Web di Visual Studio, che può testare più URL.

![Pagina nuovo test delle prestazioni in portale di Azure](./load-tests/_static/azure-appservice-perf-test-config.png)

Alla fine del test, i report generati mostrano le caratteristiche di prestazioni dell'app. Le statistiche di esempio includono:

* Tempo di risposta medio
* Velocità effettiva massima: richieste al secondo
* Percentuale errori

## <a name="third-party-tools"></a>Strumenti di terze parti

L'elenco seguente contiene gli strumenti per le prestazioni Web di terze parti con diversi set di funzionalità:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (AB)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [Locusta](https://locust.io/)
* [Websurge di Wind West](https://websurge.west-wind.com/)
* [Netling](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)
