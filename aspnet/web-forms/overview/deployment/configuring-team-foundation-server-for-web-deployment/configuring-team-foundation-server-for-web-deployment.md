---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Configurazione di Team Foundation Server per la distribuzione Web | Microsoft Docs
author: jrjlee
description: Questa esercitazione illustrerà come configurare Team Foundation Server (TFS) 2010 per creare soluzioni e distribuire il contenuto web in diversi ambienti di destinazione. In questo...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 0155f8dc4ca05a91ed8921f83aa6fa1b0b59c1a5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829687"
---
<a name="configuring-team-foundation-server-for-web-deployment"></a>Configurazione di Team Foundation Server per la distribuzione Web
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questa esercitazione illustrerà come configurare Team Foundation Server (TFS) 2010 per creare soluzioni e distribuire il contenuto web in diversi ambienti di destinazione. Questo include scenari di integrazione continua (CI), in cui si distribuisce il contenuto automaticamente ogni volta che uno sviluppatore apporta una modifica. È inoltre possibile utilizzare gli scenari di trigger manuale, in cui un amministratore può desidera attivare la distribuzione di una compilazione specifica per un ambiente di staging dopo la compilazione è stata verificata e convalidata nell'ambiente di test. Negli argomenti di questa esercitazione illustrerà l'intero processo di configurazione, tra cui:
> 
> - Come creare un nuovo progetto team in TFS.
> - Come aggiungere contenuto al controllo del codice sorgente.
> - Come configurare un server di compilazione per supportare l'integrazione continua e distribuzione.
> - Come creare una definizione di compilazione che include la logica di distribuzione.
> - Come configurare le autorizzazioni per la distribuzione automatizzata.
> 
> Per una traduzione italiana di queste esercitazioni, visitare [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Questa esercitazione si presuppone di avere installato TFS 2010 e creato una raccolta di progetti team come parte del processo di configurazione iniziale. Il [Guida all'installazione di Team Foundation per Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) fornisce indicazioni complete su queste attività.

## <a name="context"></a>Contesto

Ciò fa parte di una serie di esercitazioni in base ai requisiti di distribuzione aziendale di un'azienda fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) soluzione&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="scenario-overview"></a>Panoramica dello scenario

Lo scenario di alto livello per queste esercitazioni viene descritto [distribuzione Web aziendale: panoramica dello Scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). È consigliabile leggere questo argomento prima di iniziare questa esercitazione.

## <a name="how-to-use-this-tutorial"></a>Utilizzo dell'esercitazione

Se questa è la prima volta le attività descritte in questa esercitazione è stata eseguita, o se si desidera seguire gli esempi di uso della soluzione di esempio, è necessario usare gli argomenti dell'esercitazione in ordine. In alternativa, è possibile utilizzare i singoli argomenti come guida per attività specifiche. Questa esercitazione include questi argomenti:

- [La creazione di un progetto Team in TFS](creating-a-team-project-in-tfs.md). Un progetto team è l'unità principale per controllo del codice sorgente e gestione dei processi di compilazione in TFS. È necessario creare un progetto team prima di poter aggiungere contenuti a controllo del codice sorgente o creare definizioni di compilazione.
- [Aggiunta di contenuto al controllo del codice sorgente](adding-content-to-source-control.md). Dopo aver creato un progetto team, è possibile iniziare ad aggiungere contenuto al controllo del codice sorgente. È necessario aggiungere i progetti e soluzioni, insieme a eventuali dipendenze esterne, prima di iniziare la configurazione di build.
- [Configurazione di un TFS Server di compilazione per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md). Se si desidera compilare il contenuto del progetto team, è necessario configurare un server di compilazione. Nella maggior parte dei casi, deve essere in un computer separato dall'installazione di TFS. Per configurare un server di compilazione, è necessario installare e configurare il servizio di compilazione TFS, installare Visual Studio 2010, creare i controller di compilazione e agenti di compilazione, installare eventuali prodotti o i componenti che il codice necessita per compilazione completata e l'installazione di Strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web).
- [Creazione di una definizione di compilazione che supporta la distribuzione](creating-a-build-definition-that-supports-deployment.md). Prima di iniziare a Accodamento messaggi o attivare le compilazioni in TFS, è necessario creare almeno una definizione di compilazione per il progetto team. La definizione di compilazione definisce tutti gli aspetti della compilazione, inclusi i cui elementi devono essere inclusi nella compilazione, che cosa dovrebbe attivare la compilazione e Team Build in cui deve inviare l'output di compilazione. È possibile configurare una definizione di compilazione per eseguire file di progetto personalizzati Microsoft Build Engine (MSBuild), che consente di includere la logica di distribuzione nelle build automatizzata.
- [Distribuzione di una compilazione specifica](deploying-a-specific-build.md). In molti scenari, è opportuno distribuire una compilazione specifica, anziché la build più recente, in un ambiente di destinazione. In questo caso, è possibile configurare una definizione di compilazione che distribuisce contenuto da una cartella di ricezione specifico.
- [Configurazione delle autorizzazioni per Team di distribuzione della compilazione](configuring-permissions-for-team-build-deployment.md). Se il servizio di compilazione per distribuire il contenuto come parte di un processo di compilazione automatizzata, è necessario concedere varie autorizzazioni all'account del servizio di compilazione su qualsiasi server web di destinazione e i server di database.

## <a name="key-technologies"></a>Tecnologie chiave

Questa esercitazione è incentrata su come usare questi prodotti e tecnologie per supportare la compilazione automatica e distribuzione web:

- Visual Studio Team Foundation Server 2010
- Team Build e MSBuild
- Distribuzione Web

L'esercitazione interessa anche all'uso di Windows Server 2008 R2, IIS 7.5, SQL Server 2008 R2, ASP.NET 4.0 e ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Altre esercitazioni di questa serie

Ciò fa parte di una serie di cinque esercitazioni su distribuzione web su larga scala. Ecco le altre esercitazioni della serie:

- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Questo contenuto introduttivo fornisce lo sfondo contesto per la serie di esercitazioni. Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e procedure dettagliate descritte in tutta la serie rientrano in un processo più ampio di Application Lifecycle Management (ALM).
- [Distribuzione nell'organizzazione Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione offre un'introduzione concettuale a file di progetto MSBuild, la Pipeline di pubblicazione sul Web (WPP), distribuzione Web e altre tecnologie correlate. Viene spiegato come è possibile utilizzare questi strumenti insieme per gestire i processi di distribuzione complesse.
- [Configurazione degli ambienti Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Questa esercitazione descrive come configurare i server di Windows per supportare vari scenari di distribuzione, tra cui la distribuzione di pacchetto web remoto utilizzando il servizio agente di distribuzione Web (agente remoto) o il gestore di distribuzione Web e la distribuzione di database remoto. Vengono fornite istruzioni su come scegliere il metodo di distribuzione appropriate per il proprio ambiente e viene descritto come utilizzare la Web Farm Framework (WFF) per eseguire la replica di applicazioni web distribuite tra tutti i server web in una server farm.
- [Distribuzione Web aziendale avanzata](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Questa esercitazione descrive come eseguire varie attività di distribuzione più avanzate, come la personalizzazione delle distribuzioni di database per più ambienti, esclusione di file e cartelle dalla distribuzione e portare offline l'applicazioni web durante il processo di distribuzione .

> [!div class="step-by-step"]
> [avanti](creating-a-team-project-in-tfs.md)
