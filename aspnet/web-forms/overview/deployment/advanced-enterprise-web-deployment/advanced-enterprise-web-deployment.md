---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Distribuzione Web aziendale avanzata | Microsoft Docs
author: jrjlee
description: Questa esercitazione illustrerà come eseguire diverse attività che sono necessari o utili in molti scenari di distribuzione enterprise. Per una lingua italiana translati...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: adadd921a96b5643eb54cd6f6384ef881ca6ba50
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835077"
---
<a name="advanced-enterprise-web-deployment"></a>Distribuzione Web aziendale avanzata
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questa esercitazione illustrerà come eseguire diverse attività che sono necessari o utili in molti scenari di distribuzione enterprise.
> 
> Per una traduzione italiana di queste esercitazioni, visitare [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


Ciò fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) soluzione&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="scenario-overview"></a>Panoramica dello scenario

Lo scenario di alto livello per queste esercitazioni viene descritto [distribuzione Web aziendale: panoramica dello Scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). È consigliabile leggere questo argomento prima di iniziare questa esercitazione.

## <a name="how-to-use-this-tutorial"></a>Utilizzo dell'esercitazione

- Ognuno degli argomenti in questa esercitazione è indipendente e risolve una sfida particolare o un problema che si verifica negli scenari di distribuzione enterprise. Non è necessario esaminare questi argomenti in un ordine particolare. Tuttavia, questa esercitazione illustra alcune attività avanzate. Di conseguenza, è consigliabile acquisire familiarità con i concetti e tecniche che il [distribuzione Web aziendale](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) dell'esercitazione per ottenere il massimo vantaggio da questo contenuto.
- Questa esercitazione include questi argomenti:
- [Esecuzione di una distribuzione "What If"](performing-a-what-if-deployment.md). In molti scenari, è opportuno determinare l'impatto di una distribuzione proposta in un ambiente di destinazione o qualsiasi contenuto esistente prima di apportare effettivamente le modifiche. Questo argomento descrive come eseguire una distribuzione "what if" per generare file di log e gli script di aggiornamento del database come se è stato distribuito il contenuto in un ambiente di destinazione, senza apportare effettivamente alcune modifiche. L'analisi di queste risorse possono aiutare a individuare potenziali problemi prima che una distribuzione in tempo reale.
- [Personalizzazione delle distribuzioni di Database per più ambienti](customizing-database-deployments-for-multiple-environments.md). Quando si distribuisce un progetto di database a più destinazioni, è spesso opportuno personalizzare le proprietà di distribuzione per ogni ambiente di destinazione. Ad esempio, in ambienti di test è consigliabile in genere ricreare il database in ogni distribuzione, mentre negli ambienti di gestione temporanea o produzione sarebbe stato molto più probabile che gli aggiornamenti incrementali per conservare i dati. Questo argomento descrive come è possibile incorporare queste modifiche delle proprietà per la logica di distribuzione tramite la creazione di un file di configurazione (sqldeployment) di distribuzione specifici dell'ambiente per ogni ambiente di destinazione.
- Distribuzione di appartenenze al ruolo del Database in ambienti di Test. Quando si ricrea un database in ogni distribuzione&#x2014;, ad esempio, come parte di un'integrazione continua (CI) compilare e distribuire in un ambiente di test&#x2014;in genere è necessario configurare le appartenenze ai ruoli di database ogni volta. Ad esempio, in genere è necessario concedere le autorizzazioni per l'identità del pool di applicazioni associato all'applicazione web. Questo argomento descrive come è possibile automatizzare questo processo tramite l'aggiunta di uno script di post-distribuzione SQL per la logica di distribuzione.
- [Distribuzione di database di appartenenza negli ambienti aziendali](deploying-membership-databases-to-enterprise-environments.md). I database di appartenenza ASP.NET hanno caratteristiche diverse che possono complicare il processo di distribuzione. Ad esempio, una distribuzione con solo schema verrà lascia il database in uno stato non operativo. Nella maggior parte degli scenari, è preferibile per creare un database di appartenenza direttamente in ogni ambiente di destinazione. Tuttavia, se è necessario distribuire un database di appartenenza, in questo argomento descrive alcuni approcci che utilizzabili per soddisfare le sfide.
- [Esclusione file e cartelle dalla distribuzione](excluding-files-and-folders-from-deployment.md). In alcuni scenari, è opportuno personalizzare il contenuto del pacchetto di web agli ambienti di destinazione specifico. Ad esempio, voler includere le versioni complete di librerie JavaScript quando si distribuisce in un ambiente di test, per supportare il debug sul lato client, ma usare minimizzate versioni delle librerie quando si distribuisce in un ambiente di gestione temporanea o produzione. Questo argomento descrive come è possibile escludere specifici file e cartelle dal processo di creazione del pacchetto.
- [Impostazione delle applicazioni Web non in linea con Web distribuisce](taking-web-applications-offline-with-web-deploy.md). Quando si distribuiscono soluzioni in un ambiente di gestione temporanea o produzione, è spesso opportuno rendere le applicazioni web in modalità offline per la durata del processo di distribuzione. In questo argomento viene descritto come aggiungere un *App\_offline.htm* all'applicazione web all'inizio del processo di distribuzione di file e rimuoverlo al termine. Mentre il *App\_offline.htm* del file sono presenti, tutti gli utenti che esplorano per l'applicazione web vengono automaticamente reindirizzati al *App\_offline.htm* file.
- [Esecuzione di script di PowerShell di Windows da MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Molti scenari di distribuzione richiedono azioni di post-distribuzione più complesse, quali l'aggiunta di origini evento personalizzate nel Registro di sistema o la configurazione della replica tra istanze di SQL Server. Queste azioni vengono spesso eseguite mediante gli script di Windows PowerShell. Questo argomento descrive come eseguire gli script di Windows PowerShell da un file di progetto di Microsoft Build Engine (MSBuild) come parte del processo di compilazione e distribuzione.
- [Risoluzione dei problemi il processo di Packaging](troubleshooting-the-packaging-process.md). La Pipeline di pubblicazione sul Web (WPP) definisce una proprietà di MSBuild denominata **EnablePackageProcessLoggingAndAssert** che è possibile usare per generare informazioni dettagliate sul processo di creazione di pacchetti per i progetti applicazione web. Questo argomento descrive cosa la proprietà e come usarlo.

## <a name="key-technologies"></a>Tecnologie chiave

Questa esercitazione è incentrata su come usare questi prodotti e tecnologie per supportare la compilazione automatica e distribuzione web:

- Visual Studio 2010 e Team Foundation Server (TFS) 2010
- MSBuild e TFS Team Build
- Internet Information Services (IIS) 7.5
- Strumento di distribuzione Web IIS (distribuzione Web) 2.1
- L'utilità di distribuzione di database VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Altre esercitazioni di questa serie

Ciò fa parte di una serie di cinque esercitazioni su distribuzione web su larga scala. Si tratta di altre esercitazioni della serie:

- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Questo contenuto introduttivo fornisce lo sfondo contesto per la serie di esercitazioni. Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e procedure dettagliate descritte in tutta la serie rientrano in un processo più ampio di Application Lifecycle Management (ALM).
- [Distribuzione nell'organizzazione Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione offre un'introduzione concettuale a file di progetto MSBuild, WPP, distribuzione Web e altre tecnologie correlate. Viene spiegato come è possibile utilizzare questi strumenti insieme per gestire i processi di distribuzione complesse.
- [Configurazione degli ambienti Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Questa esercitazione descrive come configurare i server di Windows per supportare vari scenari di distribuzione, tra cui la distribuzione di pacchetto web remoto utilizzando il servizio agente di distribuzione Web (agente remoto) o il gestore di distribuzione Web e la distribuzione di database remoto. Vengono fornite istruzioni su come scegliere il metodo di distribuzione appropriate per il proprio ambiente e viene descritto come utilizzare la Web Farm Framework (WFF) per eseguire la replica di applicazioni web distribuite tra tutti i server web in una server farm.
- [Configurazione di Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Questa esercitazione descrive come configurare TFS per supportare diversi scenari di distribuzione, tra cui la distribuzione automatizzata come parte di un processo di integrazione continua e attivata manualmente le distribuzioni di compilazioni specifiche.

> [!div class="step-by-step"]
> [avanti](performing-a-what-if-deployment.md)
