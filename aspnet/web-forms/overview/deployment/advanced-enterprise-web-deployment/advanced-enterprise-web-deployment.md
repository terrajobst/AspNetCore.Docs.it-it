---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Distribuzione Web aziendale avanzate | Documenti Microsoft
author: jrjlee
description: In questa esercitazione Mostra come eseguire diverse attività che sono necessari o utili in molti scenari di distribuzione dell'organizzazione. Per un translati italiana...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 892e494b6fde994c4d04952382e4d618d73cad5c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879933"
---
<a name="advanced-enterprise-web-deployment"></a>Distribuzione Web aziendale avanzato
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questa esercitazione Mostra come eseguire diverse attività che sono necessari o utili in molti scenari di distribuzione dell'organizzazione.
> 
> Per una traduzione italiana di queste esercitazioni, visitare [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Questo fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) soluzione&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [comprendere il processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente uno istruzioni che si applicano a ogni ambiente di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifici dell'ambiente di compilazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="scenario-overview"></a>Panoramica dello scenario

Viene descritto lo scenario di alto livello per queste esercitazioni in [distribuzione Web aziendale: panoramica dello Scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). È consigliabile leggere questo argomento prima di iniziare in questa esercitazione.

## <a name="how-to-use-this-tutorial"></a>Utilizzo dell'esercitazione

- Ognuno degli argomenti in questa esercitazione è indipendente e risolve un problema che si verifica in scenari di distribuzione dell'organizzazione o richiesta specifico. Non è necessario eseguire questi argomenti in un ordine particolare. Tuttavia, questa esercitazione vengono illustrate alcune attività avanzate. Di conseguenza, è consigliabile acquisire familiarità con i concetti e tecniche che il [distribuzione Web dell'azienda](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) esercitazione sono trattati in modo da ottenere il massimo vantaggio da questo contenuto.
- In questa esercitazione include questi argomenti:
- [Esecuzione di una distribuzione "Cosa accade se"](performing-a-what-if-deployment.md). In molti scenari, sarà possibile determinare l'impatto di una distribuzione proposta in un ambiente di destinazione o di qualsiasi contenuto esistente prima di eseguire effettivamente le modifiche. In questo argomento viene descritto come eseguire una distribuzione "cosa accade se" per generare file di log e script di aggiornamento del database come se è stato distribuito il contenuto in un ambiente di destinazione, senza apportare modifiche. L'analisi di queste risorse consentono di individuare i potenziali problemi prima di installare una distribuzione in tempo reale.
- [Personalizzazione delle distribuzioni di Database per più ambienti](customizing-database-deployments-for-multiple-environments.md). Quando si distribuisce un progetto di database a più destinazioni, è spesso opportuno personalizzare le proprietà di distribuzione per ogni ambiente di destinazione. Ad esempio, negli ambienti di testing è consigliabile in genere ricreare il database in ogni distribuzione, mentre in ambienti di gestione temporanea o produzione sarebbe molto più probabile che gli aggiornamenti incrementali per conservare i dati. In questo argomento viene descritto come è possibile incorporare tali proprietà viene modificato nella logica di distribuzione tramite la creazione di un file di configurazione (con estensione sqldeployment) specifico dell'ambiente distribuzione per ogni ambiente di destinazione.
- Distribuzione appartenenze al ruolo del Database in ambienti di Test. Quando si ricrea un database in tutte le distribuzioni del&#x2014;, ad esempio, come parte di un'integrazione continua (CI) compilare e distribuire un ambiente di test&#x2014;in genere è necessario configurare le appartenenze ai ruoli di database ogni volta. Ad esempio, in genere è necessario concedere le autorizzazioni per l'identità del pool di applicazioni associato all'applicazione web. In questo argomento viene descritto come è possibile automatizzare questo processo tramite l'aggiunta di uno script SQL di post-distribuzione per la logica di distribuzione.
- [Distribuzione di database di appartenenza per ambienti aziendali](deploying-membership-databases-to-enterprise-environments.md). Database di appartenenza ASP.NET presentano diverse caratteristiche che possono complicare il processo di distribuzione. Ad esempio, una distribuzione solo allo schema verrà lascia il database in uno stato non operativo. Nella maggior parte degli scenari, è preferibile per creare un database di appartenenza direttamente in ogni ambiente di destinazione. Tuttavia, se è necessario distribuire un database di appartenenza, questo argomento descrive alcuni degli approcci che è possibile utilizzare per soddisfare le esigenze di inerente.
- [Esclusione di file e cartelle da distribuzione](excluding-files-and-folders-from-deployment.md). In alcuni scenari, si desidera personalizzare il contenuto del pacchetto di web agli ambienti di destinazione specifico. Potrebbe, ad esempio, si desidera includere le versioni complete di librerie JavaScript quando si distribuisce in un ambiente di test, per supportare il debug sul lato client, ma utilizzare minimizzate versioni delle librerie, quando si distribuisce in un ambiente di gestione temporanea o produzione. In questo argomento viene descritto come è possibile escludere specifici file e cartelle dal processo di creazione del pacchetto.
- [Acquisire le applicazioni Web non in linea con Web distribuire](taking-web-applications-offline-with-web-deploy.md). Quando si distribuiscono soluzioni in un ambiente di produzione o gestione temporanea, è spesso opportuno rendere le applicazioni web in modalità offline per tutta la durata del processo di distribuzione. In questo argomento viene descritto come aggiungere un *App\_offline.htm* file all'applicazione web all'inizio del processo di distribuzione e rimuoverlo al termine. Mentre il *App\_offline.htm* file sul posto, gli utenti a cui passare all'applicazione web vengono automaticamente reindirizzati al *App\_offline.htm* file.
- [Esecuzione di script di Windows PowerShell da MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Molti scenari di distribuzione sono necessarie azioni di post-distribuzione più complesse, quali l'aggiunta di origini di eventi personalizzati nel Registro di sistema o la configurazione della replica tra istanze di SQL Server. Spesso, queste azioni vengono eseguite tramite script di Windows PowerShell. In questo argomento viene descritto come eseguire gli script di Windows PowerShell da un file di progetto di Microsoft Build Engine (MSBuild) come parte del processo di compilazione e distribuzione.
- [Risoluzione dei problemi il processo di creazione del pacchetto](troubleshooting-the-packaging-process.md). La Pipeline di pubblicazione sul Web (WPP) definisce una proprietà di MSBuild denominata **EnablePackageProcessLoggingAndAssert** che è possibile utilizzare per generare informazioni dettagliate sul processo di creazione di pacchetti per i progetti di applicazione web. Questo argomento descrive la proprietà non e come utilizzarla.

## <a name="key-technologies"></a>Tecnologie principali

In questa esercitazione viene illustrato come utilizzare questi prodotti e tecnologie per supportare la compilazione automatica e distribuzione web:

- Visual Studio 2010 and Team Foundation Server (TFS) 2010
- MSBuild e compilazione di Team TFS
- Internet Information Services (IIS) 7.5
- Strumento di distribuzione Web di IIS (distribuzione Web) 2.1
- L'utilità di distribuzione di database VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Altre esercitazioni di questa serie

Questo fa parte di una serie di cinque esercitazioni su distribuzione web su larga scala. Altre esercitazioni nella serie sono:

- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Questo contenuto introduttivo fornisce lo sfondo di scelta rapida per la serie di esercitazioni. Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e procedure dettagliate descritte in tutta la serie rientrano in un processo più ampio di Application Lifecycle Management (ALM).
- [Distribuzione nell'organizzazione Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione viene fornita un'introduzione concettuale a file di progetto MSBuild, WPP, distribuzione Web e altre tecnologie correlate. Spiega come è possibile utilizzare questi strumenti insieme per gestire i processi di distribuzione complessi.
- [Configurazione degli ambienti di Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In questa esercitazione viene descritto come configurare i server di Windows per supportare diversi scenari di distribuzione, inclusa la distribuzione di pacchetto web remoto tramite il servizio agente di distribuzione Web (l'agente remoto) o il gestore di distribuzione Web e la distribuzione del database remoto. Vengono fornite informazioni aggiuntive su come scegliere il metodo di distribuzione appropriata per il proprio ambiente e viene descritto come utilizzare la Web Farm Framework (WFF) per replicare le applicazioni web distribuite tra tutti i server web in una server farm.
- [Configurazione di Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In questa esercitazione viene descritto come configurare TFS per supportare diversi scenari di distribuzione, tra cui la distribuzione automatica come parte di un processo di integrazione continua e attivate manualmente le distribuzioni di compilazioni specifiche.

> [!div class="step-by-step"]
> [avanti](performing-a-what-if-deployment.md)
