---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Integrazione continua e recapito continuo (creazione di App Cloud funzionanti con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 7a92a68ce8bbeec604a22e082975d33f9f3377c1
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340069"
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Integrazione continua e recapito continuo (creazione di App Cloud funzionanti con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).


I primi due consigliate sono state modelli di processo di sviluppo [automatizzare tutto](automate-everything.md) e [controllo del codice sorgente](source-control.md), e li combina il terzo modello di processo. Integrazione continua (CI) significa che, ogni volta che uno sviluppatore archivia il codice al repository di origine, viene automaticamente attivata una compilazione. Recapito continuo (CD) accetta ulteriormente questo passaggio: dopo una compilazione e unit test automatizzati hanno esito positivo, si distribuisce automaticamente l'applicazione in un ambiente in cui è possibile eseguire test approfonditi più.

Il cloud ti permette di ridurre al minimo i costi di manutenzione di un ambiente di test perché si paga solo per le risorse di ambiente, purché si usa li. Il processo CD può configurare l'ambiente di test quando ti serve, e può arrestare l'ambiente al termine test.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Flusso di lavoro di integrazione continua e recapito continuo

In genere è consigliabile eseguire la distribuzione continua per lo sviluppo e ambienti di staging. La maggior parte dei team, anche in Microsoft, richiedono un processo di revisione e approvazione manuale per la distribuzione di produzione. Per un ambiente di produzione è possibile assicurarsi che la distribuzione avviene quando persone chiave nel team di sviluppo sono disponibili per il supporto o durante i periodi di scarso traffico. Ma non esiste nulla a che non consentono di automatizzare completamente gli ambienti di sviluppo e test in modo che uno sviluppatore deve eseguire operazioni solo archiviare una modifica e un ambiente è configurato per i test di accettazione.

Nel seguente diagramma da [un Microsoft Patterns and Practices e-book sul recapito continuo](http://aka.ms/ReleasePipeline) illustra un tipico flusso di lavoro. Fare clic sull'immagine per visualizzarla dimensione completa nel contesto originale.

[![Flusso di lavoro di recapito continuo](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Come il cloud consente conveniente integrazione continua e recapito Continuo

Automazione di tali processi in Azure è facile. Poiché tutto ciò che viene eseguito nel cloud, non è necessario acquistare o gestire server per le compilazioni o ambienti di test. E non è necessario attendere che sia possibile eseguire test su un server. Con ogni compilazione è eseguire, è possibile creare rapidamente un ambiente di test in Azure usando lo script di automazione, i test di accettazione esecuzione o più test approfonditi su di esso e quindi dopo aver appena ridurne le dimensioni. E se si esegue solo tale server per 2 ore o 8 ore al giorno, la quantità di denaro che è necessario pagarlo è minima, poiché sta pagando solo per l'ora in cui è in esecuzione una macchina. Ad esempio, l'ambiente necessari per la correzione che dell'applicazione in pratica costa circa 1 centesimi all'ora se si passa un livello di dal livello gratuito. Nel corso di un mese, se è stato eseguito solo l'ambiente di un'ora alla volta, l'ambiente di test sarebbe probabilmente costare meno di un latte che acquista Starbucks.

## <a name="azure-devops-services"></a>Servizi di Azure DevOps 

Servizi di Azure DevOps offre numerose funzionalità che consentono lo sviluppo di applicazioni dalla pianificazione alla distribuzione.

- Supporta Git (distribuite) e controllo del codice sorgente TFVC (centralizzato).
- Offre un servizio di compilazione elastica, ovvero dinamico crea server di compilazione quando sono necessari e li ricava verso il basso quando sono pronti. È possibile avviare automaticamente una compilazione quando qualcuno Archivia modifiche al codice sorgente e non è necessario disporre di allocare e pagare per i propri server di compilazione che si trovano di gran parte del tempo di inattività. Il servizio di compilazione è gratuito fino a quando non superare un certo numero di compilazioni. Se si prevede di eseguire un volume elevato di compilazioni, è possibile pagare un piccolo aggiuntivi per i server di compilazione riservato.
- Supporta il recapito continuo in Azure.
- Supporta il test di carico automatizzato. Il test di carico è fondamentale per un'app cloud ma è spesso sottovalutato fino a quando non è troppo tardi. Il test di carico simula un uso massiccio di un'app da migliaia di utenti, consentendo di trovare i colli di bottiglia e migliorare la velocità effettiva, prima di rilasciare l'app nell'ambiente di produzione.
- Supporta la collaborazione nella chat team, che facilita la comunicazione in tempo reale e la collaborazione per piccoli team agile.
- Supporta la gestione dei progetti agile.


Per altre informazioni sulla funzionalità per la distribuzione dei servizi di Azure DevOps e integrazione continua, vedere [la documentazione di Azure DevOps](/azure/devops/index).

Se si sta cercando un controllo di chiavi in mano gestione dei progetti, la collaborazione tra team e soluzione di controllo di origine, i servizi di Azure DevOps. Acceder [servizi di Azure DevOps](https://dev.azure.com/).

## <a name="summary"></a>Riepilogo

I modelli di sviluppo tre cloud prima sono stati su come implementare un processo di sviluppo ripetibile, affidabili e prevedibili con durata del ciclo bassa. Nel [capitolo successivo](web-development-best-practices.md) si inizia a esaminare i modelli di architettura e scrittura di codice.

## <a name="resources"></a>Risorse

Per altre informazioni, vedere [distribuire un'app web nel servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Vedere anche le risorse seguenti:

- [Creazione di una Pipeline di rilascio con Team Foundation Server 2012](http://aka.ms/ReleasePipeline). Laboratori pratici E-book e codice di esempio da Microsoft Patterns and Practices, fornisce un'introduzione approfondita a recapito continuo. Illustra l'uso di Visual Studio Lab Management e Release Management per Visual Studio.
- [Indicazioni e strumenti di ALM Rangers DevOps](https://aka.ms/vsarsolutions/). ALM Rangers ha introdotto la soluzione complementare di esempio DevOps Workbench e istruzioni pratiche in collaborazione con i modelli &amp; libro di procedure consigliate *creazione di una Pipeline di rilascio a TFS 2012*, come un ottimo modo per iniziare apprendimento dei concetti della metodologia DevOps &amp; Release Management per TFS 2012 e per provarla prima del. Le indicazioni fornite di seguito viene illustrato come compilare una sola volta e distribuire in più ambienti.
- [Test per il recapito continuo con Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). E-book Microsoft Patterns and Practices, spiega come integrare i test automatizzati con il recapito continuo.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Codice sorgente per uno strumento progettato per acquisire una compilazione da TFS (basato su un'etichetta), compilarla, crearne il pacchetto, consentire ad altri utenti nel ruolo DevOps configurare alcuni aspetti specifici di esso ed eseguirne il push in Azure. Lo strumento registra il processo di distribuzione per consentire operazioni di "rollback" a una versione distribuita in precedenza. Lo strumento non presenta dipendenze esterne e può funzionare autonomo usando le API di TFS e il SDK di Azure.
- [Recapito continuo: Software affidabile rilascia tramite compilazione, Test e automazione della distribuzione](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Libro di Jez Humble.
- [Rilasciarlo. Progettare e distribuire il Software di produzione](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Libro di Michael T. Nygard.

> [!div class="step-by-step"]
> [Precedente](source-control.md)
> [Successivo](web-development-best-practices.md)
