---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Distribuzione nell'organizzazione Web | Microsoft Docs
author: jrjlee
description: Questa esercitazione descrive come soddisfare molte delle sfide che possono incontrare quando si gestisce la distribuzione di applicazioni web su larga scala per svil...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 735cc5ac37e369e6149c526174c3f74a08db9758
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838631"
---
<a name="web-deployment-in-the-enterprise"></a>Distribuzione Web aziendale
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questa esercitazione descrive come soddisfare molte delle sfide che possono incontrare quando si gestisce la distribuzione di applicazioni web su larga scala per ambienti di sviluppo, test, staging e produzione. L'esercitazione include una soluzione di riferimento insieme a una combinazione di contenuto concettuale e orientati alle attività consentono di eseguire varie attività comuni e procedure.
> 
> Per una traduzione italiana di queste esercitazioni, visitare [ http://www.lucamorelli.it ](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Sfide della distribuzione aziendale

Le organizzazioni spesso riscontrare queste sfide durante la ricerca per gestire la distribuzione di soluzioni complesse, su scala aziendale:

- È necessario essere in grado di distribuire progetti in più ambienti, ad esempio per gli sviluppatori o ambienti di test, server di produzione e le piattaforme di gestione temporanea. La soluzione deve essere distribuito con le impostazioni di configurazione diversi per ogni ambiente.
- È necessario distribuire contemporaneamente come parte di un processo di compilazione e distribuzione automatizzato o passo a passo più progetti dipendenti.
- È necessario essere in grado di alla distribuzione di unità da un processo automatizzato. Ad esempio, si desidera utilizzare un processo di integrazione continua (CI) per distribuire applicazioni web in un ambiente di test quando viene archiviato nuovo codice.
- È necessario essere in grado di controllare il processo di distribuzione e impostare le variabili di distribuzione dall'esterno di Visual Studio, come gli sviluppatori sono in genere non hanno le credenziali necessarie per ogni ambiente di destinazione o le impostazioni di configurazione corretto.
- È necessario distribuire i progetti di database basata sullo schema e mantenere i dati esistenti nelle distribuzioni successive.
- È necessario distribuire i database dei membri in base a hoc senza la distribuzione di dati dell'account utente. È necessario anche aggiornare lo schema dei database di appartenenza distribuito senza perdita di dati di account utente esistente.
- È necessario escludere determinati file o cartelle quando si distribuisce contenuto in diversi ambienti di destinazione.

## <a name="overview-of-approach"></a>Panoramica dell'approccio

Questa esercitazione, con le altre esercitazioni di questa serie, Usa questo approccio generale per soddisfare le esigenze descritte in precedenza.

- **Usare i file di progetto personalizzati di Microsoft Build Engine (MSBuild) per controllare il processo di compilazione e distribuzione globale.**
- Ciò consente di compilare e distribuire tutti i progetti nella soluzione come parte di un'unica operazione gestibile tramite script.
- Impostazioni specifiche dell'ambiente vengono configurate utilizzando i file di progetto specifici dell'ambiente semplice. A differenza dell'approccio incentrato sul Visual Studio dell'uso di configurazioni di soluzione e profili di pubblicazione per configurare le distribuzioni per ambienti diversi, questo approccio consente di configurare e gestire il processo di distribuzione da esterno a Visual Studio. Ciò significa che gli sviluppatori non devono passare conoscenza delle stringhe di connessione, gli endpoint di servizio, le credenziali del server e altre variabili di distribuzione per gli ambienti di destinazione.
- I file di progetto personalizzati possono essere richiamati da Team Build come parte di un flusso di lavoro di Team Foundation Server (TFS). Ciò consente di configurare la distribuzione automatizzata per gli scenari di integrazione continua.

**Usare lo strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web) per creare un pacchetto e distribuire progetti di applicazione web.**

- La funzionalità distribuzione Web fornisce un framework che consente di creare un pacchetto e distribuire il contenuto dell'applicazione web a un server web IIS di destinazione, insieme alle dipendenze, le impostazioni di configurazione, le impostazioni di sicurezza e tutti gli altri requisiti.
- È possibile controllare l'intero processo di creazione di pacchetti e distribuzione all'interno dei file di progetto MSBuild personalizzati. È inoltre possibile modificare le impostazioni di configurazione che accompagnano il pacchetto di distribuzione web, come le stringhe di connessione, gli endpoint di servizio e i dettagli di destinazione di IIS.
- La funzionalità distribuzione Web, insieme alle Pipeline di pubblicazione sul Web, offre un numero elevato di punti di estendibilità che consentono di personalizzare le distribuzioni. Ad esempio, è facile da cartelle e file indesiderati esclusi dai pacchetti distribuzione web.

**Utilizzare l'utilità VSDBCMD.exe per distribuire e aggiornare gli schemi di database.**

- VSDBCMD consente di distribuire i database da un file di schema di database (. dbschema), che viene generato quando si compila un progetto di database di Visual Studio. Al contrario, la funzionalità di distribuzione di database inclusa nella distribuzione Web è più adatta alla distribuzione di database esistenti da un'istanza di SQL Server locale.
- A differenza delle funzionalità di Visual Studio per la distribuzione di progetti di database, VSDBCMD ti permette di distribuire gli aggiornamenti differenziali per un database di destinazione esistente. In questo modo è possibile preservare eventuali dati esistenti durante l'aggiornamento dello schema del database.
- È possibile eseguire comandi VSDBCMD all'interno dei file di progetto MSBuild personalizzati.

## <a name="content-map"></a>Mappa del contenuto

Questa esercitazione include argomenti che possono essere suddivise in quattro aree principali.

Questi argomenti viene presentato la soluzione di riferimento&#x2014;soluzione Contact Manager&#x2014;e viene descritto come scaricarlo e configurarlo nel computer locale:

- [Soluzione Contact Manager](the-contact-manager-solution.md)
- [Configurazione della soluzione Contact Manager](setting-up-the-contact-manager-solution.md)

Questi argomenti introdurre i file di progetto MSBuild, viene descritto come creare e usare i file di progetto personalizzati e illustra il processo di distribuzione per la soluzione Contact Manager:

- [Informazioni sul file di progetto](understanding-the-project-file.md)
- [Informazioni sul processo di compilazione](understanding-the-build-process.md)

Questi argomenti descrivono la distribuzione di applicazioni web, tra cui la modalità di compilazione e creazione di pacchetti funzionamento sul processo, come il processo di compilazione si integra con la Pipeline di pubblicazione sul Web, come modificare i parametri di distribuzione e come distribuire i pacchetti web di destinazione ambienti:

- [Compilazione e creazione di pacchetti di progetti di applicazione Web](building-and-packaging-web-application-projects.md)
- [Configurazione dei parametri per la distribuzione di pacchetti Web](configuring-parameters-for-web-package-deployment.md)
- [Distribuzione di pacchetti Web](deploying-web-packages.md)

- [Distribuzione di progetti di Database](deploying-database-projects.md) vengono descritte le tecniche diverse, è possibile usare per distribuire i progetti di database di Visual Studio, insieme ai vantaggi e svantaggi di ogni approccio. [Creazione ed esecuzione di un File di comando di distribuzione](creating-and-running-a-deployment-command-file.md) viene descritto come creare un file di comando semplice che incapsula la logica di distribuzione e consente di distribuire soluzioni complesse come processo a istruzione singola.
- Infine [manualmente l'installazione dei pacchetti Web](manually-installing-web-packages.md) conclude l'esercitazione tramite la visualizzazione è possibile importare pacchetti web in IIS.

## <a name="key-technologies"></a>Tecnologie chiave

Negli argomenti di questa esercitazione usano principalmente queste tecnologie per la gestione di compilazione e distribuzione:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- L'utilità di distribuzione di database VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Altre esercitazioni di questa serie

Ciò fa parte di una serie di cinque esercitazioni su distribuzione web su larga scala. Ecco le altre esercitazioni della serie:

- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Questo contenuto introduttivo fornisce lo sfondo contesto per la serie di esercitazioni. Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e procedure dettagliate descritte in tutta la serie rientrano in un processo più ampio di Application Lifecycle Management (ALM).
- [Configurazione degli ambienti Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Questa esercitazione descrive come configurare i server di Windows per supportare vari scenari di distribuzione, tra cui la distribuzione di pacchetto web remoto utilizzando il servizio agente di distribuzione Web (agente remoto) o il gestore di distribuzione Web e la distribuzione di database remoto. Vengono fornite istruzioni su come scegliere il metodo di distribuzione appropriate per il proprio ambiente e viene descritto come utilizzare la Web Farm Framework (WFF) per eseguire la replica di applicazioni web distribuite tra tutti i server web in una server farm.
- [Configurazione di Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Questa esercitazione descrive come configurare TFS per supportare diversi scenari di distribuzione, tra cui la distribuzione automatizzata come parte di un processo di integrazione continua e attivata manualmente le distribuzioni di compilazioni specifiche.
- [Distribuzione Web aziendale avanzata](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Questa esercitazione descrive come eseguire varie attività di distribuzione più avanzate, come la personalizzazione delle distribuzioni di database per più ambienti, esclusione di file e cartelle dalla distribuzione e portare offline l'applicazioni web durante il processo di distribuzione .

> [!div class="step-by-step"]
> [avanti](the-contact-manager-solution.md)
