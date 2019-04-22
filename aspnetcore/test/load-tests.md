---
title: Test di carico/stress di ASP.NET Core
author: Jeremy-Meng
description: Informazioni sulle diverse importanti strumenti e approcci per il test di carico e delle App ASP.NET Core di test di stress.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: test/loadtests
ms.openlocfilehash: 0a8449ea2c9df0f2ac93058f03af0a1a2aa66508
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068183"
---
# <a name="aspnet-core-loadstress-testing"></a>Test di carico/stress di ASP.NET Core

Test di stress e test di carico sono importanti per garantire che un'app web sia efficiente e scalabile. Gli obiettivi sono diversi, anche se condividono spesso test analoghi.

**Test di carico** &ndash; verificare se l'app può gestire un carico di utenti per un determinato scenario specificato mentre continuando comunque a soddisfare l'obiettivo di risposta. L'app viene eseguita in condizioni normali.

**Test di stress** &ndash; testare la stabilità delle app durante l'esecuzione in condizioni estreme, spesso per un lungo periodo di tempo. I test inserire carichi utente elevati, picchi o gradualmente crescente carico, nell'app oppure limitano le risorse di calcolo dell'app.

Test di stress determinare se un'app in condizioni di stress può recuperare da un errore e normalmente tornare al comportamento previsto. In condizioni di stress, l'app non viene eseguita in condizioni normali.

Visual Studio 2019 è l'ultima versione di Visual Studio con le funzionalità di test di carico. Per i clienti che richiedono in futuro gli strumenti di test di carico, è consigliabile strumenti alternativi, ad esempio Apache JMeter, Akamai CloudTest e BlazeMeter. Per altre informazioni, vedere la [note sulla versione di Visual Studio 2019](/visualstudio/releases/2019/release-notes#test-tools).

Il test di carico del servizio in Azure DevOps termina in 2020. Per altre informazioni, vedere [lato servizio del ciclo di vita di test di carico basati sul Cloud](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Strumenti di Visual Studio

Visual Studio consente agli utenti di creare, sviluppare ed eseguire il debug di test di carico e prestazioni web. Un'opzione è disponibile per creare i test tramite la registrazione delle azioni in un web browser.

Per informazioni su come creare, configurare ed eseguire un test di carico i progetti che usano Visual Studio 2017, vedere [Guida introduttiva: Creare un progetto di test di carico](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017). Per altre informazioni, vedere la [risorse aggiuntive](#additional-resources) sezione.

Test di carico possono essere configurati per l'esecuzione in locale o esecuzione nel cloud usando Azure DevOps.

## <a name="azure-devops"></a>Azure DevOps

Esecuzioni test di carico possono essere avviati tramite il [piani di Test di Azure DevOps](/azure/devops/test/load-test/index?view=vsts) servizio.

![Azure DevOps il test di carico pagina di destinazione](./load-tests/_static/azure-devops-load-test.png)

Il servizio supporta i formati di test seguenti:

* Visual Studio &ndash; test Web creati in Visual Studio.
* Archivio HTTP &ndash; il traffico HTTP acquisito all'interno di archivio viene riprodotto durante i test.
* [Basato su URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; consente di specificare gli URL per il caricamento dei test, i tipi di richiesta, le intestazioni e le stringhe di query. Eseguire l'impostazione dei parametri, ad esempio durata, il modello di carico e il numero di utenti può essere configurato.
* [Apache JMeter](https://jmeter.apache.org/).

## <a name="azure-portal"></a>portale di Azure

[Portale di Azure consente di configurare ed eseguire il test di carico delle App web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) direttamente dai **prestazioni** scheda del servizio App nel portale di Azure.

![Servizio App di Azure nel portale di Azure](./load-tests/_static/azure-appservice-perf-test.png)

Il test può essere un test manuale con un URL specificato o un file di Test con Visual Studio Web, che è possibile testare più URL.

![Nuova pagina di Test delle prestazioni nel portale di Azure](./load-tests/_static/azure-appservice-perf-test-config.png)

Alla fine del test, i report generati mostrano le caratteristiche delle prestazioni dell'app. Le statistiche di esempio includono:

* Tempo medio di risposta
* Velocità effettiva massima: le richieste al secondo
* Percentuale di errore

## <a name="third-party-tools"></a>Strumenti di terze parti

Nell'elenco seguente contiene gli strumenti delle prestazioni web di terze parti con diversi set di funzionalità:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (ab)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [Locust](https://locust.io/)
* [WebSurge Wind occidentale](http://websurge.west-wind.com/)
* [Netling](https://github.com/hallatore/Netling)

## <a name="additional-resources"></a>Risorse aggiuntive

* [Serie di blog di Test di carico](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)
