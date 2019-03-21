---
title: Test di carico/stress di ASP.NET Core
author: Jeremy-Meng
description: Vengono descritti alcuni importanti strumenti e approcci per test di carico e delle App ASP.NET Core di test di stress.
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 39632af2c92dac548c03e24d35a5e8a03e00890d
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209833"
---
# <a name="load-and-stress-testing-aspnet-core"></a>Caricare e di stress di test ASP.NET Core

Test di stress e test di carico sono importanti per garantire che un'app web sia efficiente e scalabile. Gli obiettivi sono diversi anche condividono spesso test analoghi.

**Test di carico**: Verifica se l'app può gestire un carico di utenti per un determinato scenario specificato mentre continuando comunque a soddisfare l'obiettivo di risposta. L'app viene eseguita in condizioni normali.

**Test di stress**: Stabilità di app di test durante l'esecuzione in condizioni estreme e spesso un lungo periodo di tempo:

* Carico elevato di utenti: i picchi o aumentare gradualmente.
* Risorse di elaborazione limitate.

In condizioni di stress, può recuperare da un errore dell'app e normalmente tornare al comportamento previsto? In condizioni di stress, l'app si trova *non* eseguiti in condizioni normali.

Visual Studio 2019 sarà l'ultima versione di Visual Studio con le funzionalità di test di carico. Per i clienti che hanno bisogno di strumenti di test di carico, è consigliabile usare strumenti alternativi, ad esempio Apache JMeter, Akamai CloudTest e Blazemeter. Per altre informazioni, vedere la [note sulla versione di anteprima di Visual Studio 2019](/visualstudio/releases/2019/release-notes-preview#test-tools).

Il test di carico del servizio in Azure DevOps termina in 2020. Per altre informazioni, vedere [lato servizio del ciclo di vita di test di carico basati sul Cloud](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Strumenti di Visual Studio

Visual Studio consente agli utenti di creare, sviluppare ed eseguire il debug di test di carico e prestazioni web. Un'opzione è disponibile per creare i test tramite la registrazione delle azioni nel web browser.

[Avvio rapido: Creare un progetto di test di carico](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) viene illustrato come creare, configurare ed eseguire un test di carico i progetti che usano Visual Studio 2017.

Per altre informazioni, vedere [Risorse aggiuntive](#add).

I test di carico possono essere configurati per eseguire in locale o nel cloud usando Azure DevOps.

## <a name="azure-devops"></a>DevOps di Azure

Esecuzioni test di carico possono essere avviati tramite il [piani di Test di Azure DevOps](/azure/devops/test/load-test/index?view=vsts) servizio.

![Azure DevOps il test di carico pagina di destinazione](./load-tests/_static/azure-devops-load-test.png)

Il servizio supporta i tipi di formato di test seguenti:

* Test di Visual Studio – test web creati in Visual Studio.
* Riproduzione di test basato su archivio HTTP: il traffico HTTP acquisito all'interno di archivio viene eseguito durante il test.
* [Test basato su URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – consente di specificare gli URL per il caricamento dei test, i tipi di richiesta, le intestazioni e le stringhe di query. Eseguire l'impostazione dei parametri, ad esempio durata, può essere configurato il modello di carico, numero di utenti e così via.
* [Apache JMeter](https://jmeter.apache.org/) di test.

## <a name="azure-portal"></a>portale di Azure

[Portale di Azure consente di configurare ed eseguire il test di carico dell'App Web,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) direttamente dalla scheda delle prestazioni del servizio App nel portale di Azure.

![Servizio App di Azure nel portale di Azure](./load-tests/_static/azure-appservice-perf-test.png)

Il test può essere un test manuale con un URL specificato o un file di Test con Visual Studio Web, che è possibile testare più URL.

![Nuova pagina di Test delle prestazioni nel portale di Azure](./load-tests/_static/azure-appservice-perf-test-config.png)

Alla fine del test, vengono generati report per mostrare le caratteristiche delle prestazioni dell'app. Le statistiche di esempio includono:

* Tempo medio di risposta
* Velocità effettiva massima: le richieste al secondo
* Percentuale di errore

## <a name="third-party-tools"></a>Strumenti di terze parti

Nell'elenco seguente contiene gli strumenti delle prestazioni web di terze parti con diversi set di funzionalità:

* [Apache JMeter](https://jmeter.apache.org/) : In primo piano suite completa di strumenti di test di carico. Associata ai thread: è necessario un thread per ogni utente.
* [ab - server HTTP Apache benchmarking tool](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/) : Strumento desktop con i masterizzatori di interfaccia utente grafica e test. Più efficiente rispetto alla JMeter.
* [Locust.IO](https://locust.io/) : Non è limitato dai thread.

<a name="add"></a>

## <a name="additional-resources"></a>Risorse aggiuntive

[Serie di blog di Test di carico](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) di Charles Sterling. Con data di validità, ma la maggior parte degli argomenti sono ancora rilevante.
