---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Configurazione di Team Foundation Server per la distribuzione Web | Documenti Microsoft
author: jrjlee
description: "In questa esercitazione verrà illustrato come configurare Team Foundation Server (TFS) 2010 per compilare soluzioni e distribuire il contenuto web in vari ambienti di destinazione. Questo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 72f60841a1381380c0ea6167077420f960180dc7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Configurazione di Team Foundation Server per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questa esercitazione verrà illustrato come configurare Team Foundation Server (TFS) 2010 per compilare soluzioni e distribuire il contenuto web in vari ambienti di destinazione. Sono inclusi gli scenari di integrazione continua (CI), in cui si distribuisce contenuto automaticamente ogni volta che uno sviluppatore apporta una modifica. È inoltre possibile utilizzare trigger manuale scenari, in cui un amministratore può attivare la distribuzione di una compilazione specifica in un ambiente di gestione temporanea dopo la compilazione è stata verificata e convalidata nell'ambiente di test. Negli argomenti di questa esercitazione guiderà l'intero processo di configurazione, tra cui:
> 
> - Come creare un nuovo progetto team in TFS.
> - Come aggiungere contenuto a controllo del codice sorgente.
> - Come configurare un server di compilazione per il supporto degli elementi di configurazione e distribuzione.
> - Come creare una definizione di compilazione che include la logica di distribuzione.
> - Come configurare le autorizzazioni per la distribuzione automatizzata.
> 
> Per una traduzione italiana di queste esercitazioni, visitare [http://www.lucamorelli.it](http://www.lucamorelli.it).


In questa esercitazione si presuppone di aver installato TFS 2010 e creare una raccolta di progetti team come parte del processo di configurazione iniziale. Il [Guida all'installazione di Team Foundation per Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) fornisce indicazioni su queste attività.

## <a name="context"></a>Contesto

Questo fa parte di una serie di esercitazioni in base ai requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) #x 2014; & soluzione per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in cui il processo di compilazione è controllato da due progetti #x 2014; & file contenente una compilare le istruzioni che si applicano a ogni ambiente di destinazione e quella contenente impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="scenario-overview"></a>Panoramica dello scenario

Viene descritto lo scenario di alto livello per queste esercitazioni in [distribuzione Web aziendale: panoramica dello Scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). È consigliabile leggere questo argomento prima di iniziare in questa esercitazione.

## <a name="how-to-use-this-tutorial"></a>Utilizzo dell'esercitazione

Se questa è la prima volta che si sono eseguito le attività descritte in questa esercitazione, o se si desidera seguire gli esempi di utilizzo della soluzione di esempio, è necessario utilizzare gli argomenti dell'esercitazione in ordine. In alternativa, è possibile utilizzare i singoli argomenti come guida per attività specifiche. In questa esercitazione include questi argomenti:

- [Creazione di un progetto Team in TFS](creating-a-team-project-in-tfs.md). Un progetto team è l'unità principale per controllo del codice sorgente e gestione dei processi di compilazione in TFS. È necessario creare un progetto team, prima di poter aggiungere contenuti a controllo del codice sorgente o creare definizioni di compilazione.
- [Aggiunta di contenuto al controllo del codice sorgente](adding-content-to-source-control.md). Dopo aver creato un progetto team, è possibile iniziare ad aggiungere contenuto al controllo del codice sorgente. È necessario aggiungere progetti e soluzioni, insieme a eventuali dipendenze esterne, prima di iniziare a configurare le compilazioni.
- [Configurazione di un TFS Server di compilazione per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md). Se si desidera compilare il contenuto del progetto team, è necessario configurare un server di compilazione. Nella maggior parte dei casi, deve essere in un computer separato dall'installazione di TFS. Per configurare un server di compilazione, è necessario installare e configurare il servizio di compilazione TFS, installare Visual Studio 2010, creare controller di compilazione e agenti di compilazione, installare eventuali prodotti o i componenti del codice necessarie per compilare correttamente e installare il Strumento di distribuzione di Web Internet Information Services (IIS) (distribuzione Web).
- [Creazione di una definizione di compilazione che supporta la distribuzione](creating-a-build-definition-that-supports-deployment.md). Prima di iniziare Accodamento messaggi o attivare le compilazioni in TFS, è necessario creare almeno una configurazione di compilazione per il progetto team. La definizione di compilazione definisce tutti gli aspetti della compilazione, inclusi i cui elementi devono essere inclusi nella compilazione e che cosa dovrebbe attivare la compilazione Team Build in cui deve inviare l'output di compilazione. È possibile configurare una definizione di compilazione per eseguire i file di progetto personalizzati Microsoft Build Engine (MSBuild), che consente di includere logica di distribuzione nelle compilazioni automatizzate.
- [Distribuzione di una compilazione specifica](deploying-a-specific-build.md). In molti scenari, si desidera distribuire una compilazione specifica, anziché la build più recente, in un ambiente di destinazione. In questo caso, è possibile configurare una definizione di compilazione che distribuisce il contenuto da una cartella di ricezione specifico.
- [Configurazione delle autorizzazioni per Team di distribuzione della compilazione](configuring-permissions-for-team-build-deployment.md). Se il servizio di compilazione per distribuire il contenuto come parte di un processo di compilazione automatizzato, è necessario concedere le autorizzazioni diverse per l'account del servizio di compilazione su qualsiasi server web di destinazione e il server di database.

## <a name="key-technologies"></a>Tecnologie principali

In questa esercitazione viene illustrato come utilizzare questi prodotti e tecnologie per supportare la compilazione automatica e distribuzione web:

- Visual Studio Team Foundation Server 2010
- Team Build e MSBuild
- Distribuzione Web

L'esercitazione interessa anche l'utilizzo di ASP.NET MVC 3, IIS 7.5, SQL Server 2008 R2, ASP.NET 4.0 e Windows Server 2008 R2.

## <a name="other-tutorials-in-this-series"></a>Altre esercitazioni di questa serie

Questo fa parte di una serie di cinque esercitazioni su distribuzione web su larga scala. Ecco le altre esercitazioni nella serie:

- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Questo contenuto introduttivo fornisce lo sfondo di scelta rapida per la serie di esercitazioni. Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e procedure dettagliate descritte in tutta la serie rientrano in un processo più ampio di Application Lifecycle Management (ALM).
- [Distribuzione nell'organizzazione Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione viene fornita un'introduzione concettuale a file di progetto MSBuild, la Pipeline di pubblicazione sul Web (WPP), distribuzione Web e altre tecnologie correlate. Spiega come è possibile utilizzare questi strumenti insieme per gestire i processi di distribuzione complessi.
- [Configurazione degli ambienti di Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In questa esercitazione viene descritto come configurare i server di Windows per supportare diversi scenari di distribuzione, inclusa la distribuzione di pacchetto web remoto tramite il servizio agente di distribuzione Web (l'agente remoto) o il gestore di distribuzione Web e la distribuzione del database remoto. Vengono fornite informazioni aggiuntive su come scegliere il metodo di distribuzione appropriata per il proprio ambiente e viene descritto come utilizzare la Web Farm Framework (WFF) per replicare le applicazioni web distribuite tra tutti i server web in una server farm.
- [Distribuzione Web aziendale avanzate](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In questa esercitazione viene illustrato come eseguire diverse attività di distribuzione più avanzate, ad esempio personalizzare le distribuzioni di database per più ambienti, esclusione dalla distribuzione di file e cartelle e l'esecuzione di applicazioni web offline durante il processo di distribuzione .

>[!div class="step-by-step"]
[Successivo](creating-a-team-project-in-tfs.md)
