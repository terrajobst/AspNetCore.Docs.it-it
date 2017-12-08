---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Integrazione continua e il recapito continuo (creazione di applicazioni con Azure Cloud del mondo reale) | Documenti Microsoft
author: MikeWasson
description: "Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 0af5f7e841bb43fa41fa0daa4ad8d59ee0596404
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Integrazione continua e il recapito continuo (creazione di applicazioni Cloud reale in Azure)
====================
da [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [E-book di Download](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [primo capitolo](introduction.md).


I primi due consigliato di modelli di processo di sviluppo sono stati [automatizzare tutti gli elementi](automate-everything.md) e [controllo del codice sorgente](source-control.md), e li combina il terzo modello di processo. Integrazione continua (CI) significa che ogni volta che uno sviluppatore archivia nel codice per il repository di origine, viene attivata automaticamente una compilazione. Il recapito continuo (CD) accetta questo passaggio ulteriormente: dopo una compilazione e unit test automatizzati hanno esito positivo, distribuire automaticamente l'applicazione in un ambiente in cui è possibile eseguire test approfonditi più.

Il cloud consente di ridurre al minimo i costi di manutenzione di un ambiente di test perché si paga solo per le risorse di ambiente come in uso. Il processo CD possibile configurare l'ambiente di test quando è necessario, è possibile portare offline l'ambiente al termine test.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Flusso di lavoro di integrazione continua e la distribuzione continua

In genere, si consiglia di effettuare la distribuzione continua per lo sviluppo e di ambienti di gestione temporanea. La maggior parte dei team, anche con Microsoft, richiedono un processo di revisione e approvazione manuale per la distribuzione di produzione. Per un ambiente di produzione è consigliabile per assicurarsi che la distribuzione avviene quando gli utenti principali del team di sviluppo sono disponibili per il supporto o durante i periodi di scarso traffico. Ma non esiste nulla a che consentono di automatizzare completamente gli ambienti di sviluppo e test in modo che uno sviluppatore ha a che fare archivia una modifica e un ambiente è configurato per i test di accettazione.

Nel seguente diagramma da [un Microsoft Patterns and Practices e-book sul recapito continuo](http://aka.ms/ReleasePipeline) viene illustrato un tipico flusso di lavoro. Clic sull'immagine per visualizzarla dimensioni completa nel relativo contesto originale.

[![Flusso di lavoro di distribuzione continua](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/en-us/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Come il cloud consente economiche CI e CD

Automazione di questi processi in Azure è semplice. Perché si sta eseguendo tutti gli elementi nel cloud, non è necessario acquistare o gestire server per le compilazioni o ambienti di test. E non è necessario attendere sia possibile eseguire test su un server. Con ogni compilazione è, è possibile creare rapidamente un ambiente di test in Azure utilizzando lo script di automazione, i test di accettazione di esecuzione o più test approfonditi su di essa e quindi al termine semplicemente chiudere. E, se si esegue solo il server per 2 ore o di 8 ore o di un giorno, la quantità di denaro che è necessario pagare per è minima, poiché si sta solo a pagamento per il tempo che un computer sia in esecuzione. Ad esempio, l'ambiente necessario per risolvere il problema che applicazione fondamentalmente costa circa 1 cento oraria se si passa un livello del livello gratuito. Nel corso di un mese, se si esegue solo l'ambiente di un'ora alla volta, nell'ambiente di test sarebbe probabilmente costo minore di un macchiato acquistati Starbucks.

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

Visual Studio Team Services fornisce una serie di funzionalità per facilitare lo sviluppo di applicazioni dalla pianificazione della distribuzione.

- Supporta Git (distribuito) e controllo del codice sorgente TFVC (centralizzata).
- Offre un servizio di compilazione elastica, ovvero in modo dinamico crea server di compilazione quando sono necessari e li ricava verso il basso quando sono pronti. È possibile avviare automaticamente una compilazione quando qualcuno archivia le modifiche al codice sorgente e non è necessario disporre di allocare e pagare i propri server di compilazione che si trovano della maggior parte del tempo di inattività. Il servizio di compilazione è disponibile, purché si non supera un determinato numero di compilazioni. Se si prevede di eseguire un numero elevato di compilazioni, è possibile pagare un piccolo extra server di compilazione riservato.
- Supporta la distribuzione continua in Azure.
- Supporta il test di carico automatizzato. Test di carico è fondamentale per un'app cloud, ma è spesso trascurato fino a quando non è troppo tardi. Test di carico simula un uso massiccio di un'app da migliaia di utenti, consentendo di individuare i colli di bottiglia e migliorare la velocità effettiva, prima di rilasciare l'app nell'ambiente di produzione.
- Supporta la collaborazione nella chat team, che facilita la comunicazione in tempo reale e la collaborazione per piccoli team agile.
- Supporta la gestione dei progetti agile.


Per ulteriori informazioni sull'integrazione continua e le funzionalità di recapito di Visual Studio Team Services, vedere [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

Se si sta cercando un controllo di gestione dei progetti, la collaborazione tra team e soluzione di controllo di origine, di chiavi in mano out VSTS. Il servizio è gratuito per gli utenti fino a 5 ed è possibile iscriversi a in [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

## <a name="summary"></a>Riepilogo

I modelli di sviluppo prima di tre cloud stati su come implementare un processo di sviluppo ripetibili, affidabile e stimabili con durata del ciclo di bassa. Nel [capitolo successivo](web-development-best-practices.md) verranno esaminati i modelli di architettura e codifica.

## <a name="resources"></a>Risorse

Per ulteriori informazioni, vedere [distribuire un'app web in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-deploy/).

Vedere anche le risorse seguenti:

- [La creazione di una Pipeline di rilascio con Team Foundation Server 2012](http://aka.ms/ReleasePipeline). Laboratori pratici E-book e codice di esempio da Microsoft Patterns and Practices, viene fornita un'introduzione approfondita per il recapito continuo. Viene illustrato l'utilizzo di Visual Studio Lab Management e Release Management per Visual Studio.
- [Indicazioni e strumenti di ALM Rangers DevOps](https://aka.ms/vsarsolutions/). ALM Rangers introdotta la soluzione complementare di esempio Workbench di DevOps e istruzioni pratiche in collaborazione con i modelli &amp; libro consigliate *la creazione di una Pipeline di rilascio con TFS 2012*, come un ottimo modo per iniziare concetti di DevOps &amp; Release Management per TFS 2012 e per avviare il servizio. Le indicazioni fornite di seguito viene illustrato come compilare una sola volta e di distribuzione in più ambienti.
- [Test per il recapito continuo con Visual Studio 2012](https://msdn.microsoft.com/en-us/library/jj159345.aspx). E-book Microsoft Patterns and Practices, verrà illustrato come integrare i test automatizzati con il recapito continuo.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Codice sorgente per uno strumento progettato per acquisire una compilazione da TFS (basato su un'etichetta), compilare, pacchetto, consentire ad altri utenti nel ruolo DevOps per configurare gli aspetti specifici di esso e inserirlo in Azure. Lo strumento registra il processo di distribuzione per consentire operazioni "rollback" a una versione precedentemente distribuita. Lo strumento non presenta dipendenze esterne e può funzionare autonomo usando le API di TFS e Azure SDK.
- [Il recapito continuo: Software affidabile rilascia tramite l'automazione della distribuzione, compilazione e Test](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Rubrica tramite Jez modesto.
- [Rilasciarlo! Progettare e distribuire il Software di ambiente di produzione](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Rubrica tramite Michael T. Nygard.

>[!div class="step-by-step"]
[Precedente](source-control.md)
[Successivo](web-development-best-practices.md)
