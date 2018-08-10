---
title: DevOps con ASP.NET Core e Azure
author: CamSoper
description: Questa guida include informazioni complete sulla creazione di una pipeline DevOps per un'app ASP.NET Core ospitata in Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722538"
---
# <a name="devops-with-aspnet-core-and-azure"></a>DevOps con ASP.NET Core e Azure

Benvenuti alla guida del ciclo di sviluppo di Azure per .NET. Questa guida presenta i concetti di base della creazione di un ciclo di sviluppo basato su Azure usando processi e strumenti di .NET. Dopo aver completato questa guida, è possibile sfruttare i vantaggi di una toolchain DevOps completa.

## <a name="who-this-guide-is-for"></a>A chi è destinata questa guida

Questa guida è destinata a sviluppatori ASP.NET esperti (livello 200-300). Non è necessario conoscere Azure, che viene illustrato in questa introduzione. La guida può essere utile anche per i tecnici di DevOps che si occupano di operazioni più che di sviluppo.

Questa guida è destinata agli sviluppatori per Windows. Tuttavia .NET Core dispone di supporto completo per Linux e macOS. Per adattare la guida a Linux/macOS, vedere i callout che indicano le differenze per Linux/macOS.

## <a name="what-this-guide-doesnt-cover"></a>Argomenti non trattati dalla guida

Questa guida è incentrata su un'esperienza di distribuzione continua end-to-end per gli sviluppatori .NET. Non è una guida completa per tutti gli aspetti di Azure e non illustra nel dettaglio le API .NET per i servizi di Azure. Il contenuto riguarda l'integrazione continua, la distribuzione, il monitoraggio e il debug. Verso la fine della guida vengono inclusi suggerimenti per le fasi successive. I suggerimenti includono servizi della piattaforma Azure utili per gli sviluppatori ASP.NET.

## <a name="whats-in-this-guide"></a>Contenuto della Guida

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[Strumenti e download](xref:azure/devops/tools-and-downloads)

Informazioni su come ottenere gli strumenti usati nella guida.

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[Distribuire nel servizio app](xref:azure/devops/deploy-to-app-service)

Informazioni sui diversi metodi per distribuire un'app ASP.NET Core nel servizio App di Azure.

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[Integrazione e distribuzione continua](xref:azure/devops/cicd)

Compilazione di una soluzione end-to-end di integrazione e distribuzione continua per l'app ASP.NET Core con GitHub, VSTS e Azure.

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[Monitoraggio e debug](xref:azure/devops/monitor)

Informazioni sull'uso degli strumenti di Azure per eseguire il monitoraggio, risolvere i problemi e ottimizzare l'applicazione.

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[Passaggi successivi](xref:azure/devops/next-steps)

Altri percorsi di apprendimento per gli sviluppatori ASP.NET Core che si avvicinano ad Azure.

## <a name="acknowledgments"></a>Ringraziamenti

Grazie a tutti gli utenti della community .NET che hanno contribuito a questa guida con suggerimenti utili. Un ringraziamento speciale ai seguenti membri della community, che hanno contribuito alla revisione finale di questo materiale:

* [Sam Wronski](https://www.youtube.com/c/worldofzerodevelopment)
* [Jeffrey Palermo](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a>Conclusione

Questa guida illustra la creazione di un ciclo di sviluppo a integrazione continua basato su ASP.NET Core e sul servizio app di Azure.

## <a name="additional-reading"></a>Altre informazioni

* [Che cos'è il cloud computing?](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [Esempi di Cloud Computing](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [Che cos'è IaaS?](https://azure.microsoft.com/overview/what-is-iaas/)
* [Che cos'è PaaS?](https://azure.microsoft.com/overview/what-is-paas/)
