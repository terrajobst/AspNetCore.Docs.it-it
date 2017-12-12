---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Distribuzione nell'organizzazione Web | Documenti Microsoft
author: jrjlee
description: In questa esercitazione viene descritto come soddisfano molti dei problemi che si verificano quando si gestisce la distribuzione di applicazioni web su larga scala di sviluppo per sistemi operativi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: 6210d01f65bcadf8ae4209e372d5aac68861bd7a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="web-deployment-in-the-enterprise"></a>Distribuzione Web dell'organizzazione
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questa esercitazione viene descritto come soddisfano molti dei problemi che si verificano quando si gestisce la distribuzione di applicazioni web su larga scala per ambienti di sviluppo, test, gestione temporanea e produzione. L'esercitazione include una soluzione di riferimento con una combinazione di contenuto concettuale e orientata alle attività per eseguire varie attività comuni e procedure.
> 
> Per una traduzione italiana di queste esercitazioni, visitare [http://www.lucamorelli.it](http://www.lucamorelli.it).


## <a name="enterprise-deployment-challenges"></a>Problemi legati alla distribuzione dell'organizzazione

Le organizzazioni spesso utilizzato questi problemi durante la ricerca per la distribuzione delle soluzioni su larga scala complessi, di gestire:

- È necessario essere in grado di distribuire progetti in più ambienti, ad esempio per sviluppatori o ambienti di test di gestione temporanea, piattaforme e i server di produzione. La soluzione deve essere distribuito con le impostazioni di configurazione diverse per ogni ambiente.
- È necessario distribuire come parte di un processo di compilazione e distribuzione singolo passaggio o automatizzato contemporaneamente più progetti dipendenti.
- È necessario essere in grado di distribuzione di unità da un processo automatizzato. Ad esempio, si desidera utilizzare un processo di integrazione continua (CI) per distribuire applicazioni web in un ambiente di test quando viene archiviato nuovo codice.
- È necessario essere in grado di controllare il processo di distribuzione e impostare le variabili di distribuzione dall'esterno di Visual Studio, come gli sviluppatori sono in genere non hanno le impostazioni di configurazione corretto o le credenziali necessarie per ogni ambiente di destinazione.
- È necessario distribuire progetti di database basati sullo schema e mantenere i dati esistenti nelle distribuzioni successive.
- È necessario distribuire il database di appartenenza in base ad hoc senza distribuire i dati dell'account utente. È inoltre potrebbe essere necessario aggiornare lo schema dei database di appartenenza distribuito senza perdere i dati dell'account utente esistente.
- È necessario escludere determinati file o cartelle quando si distribuisce contenuto in diversi ambienti di destinazione.

## <a name="overview-of-approach"></a>Panoramica dell'approccio

In questa esercitazione, con le altre esercitazioni di questa serie, viene utilizzato questo approccio di alto livello per soddisfare le esigenze descritte in precedenza.

- **Utilizzare file di progetto di Microsoft Build Engine (MSBuild) personalizzati per controllare il processo di compilazione e distribuzione generale.**
- Consente di compilare e distribuire ogni progetto nella soluzione come parte di un'operazione singola, configurabile tramite script.
- Impostazioni specifiche dell'ambiente vengono configurate utilizzando file di progetto specifici dell'ambiente semplice. A differenza di Visual Studio l'approccio incentrato dell'utilizzo di configurazioni di soluzione e i profili di pubblicazione per configurare le distribuzioni per ambienti diversi, questo approccio consente di configurare e gestire il processo di distribuzione dall'esterno di Visual Studio. Ciò significa che gli sviluppatori non devono passare conoscenza delle stringhe di connessione, gli endpoint del servizio, le credenziali del server e altre variabili di distribuzione per gli ambienti di destinazione.
- I file di progetto personalizzati possono essere richiamati da Team Build come parte di un flusso di lavoro di Team Foundation Server (TFS). Consente di configurare la distribuzione automatica per gli scenari di elemento di configurazione.

**Utilizzare lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web) per creare un pacchetto e distribuire progetti di applicazione web.**

- Distribuzione Web fornisce un framework che consente di creare un pacchetto e distribuire il contenuto dell'applicazione web a un server web IIS di destinazione, insieme alle dipendenze, le impostazioni di configurazione, le impostazioni di sicurezza e dagli altri requisiti.
- È possibile controllare l'intero processo di assemblaggio e distribuzione all'interno dei file di progetto MSBuild personalizzati. È inoltre possibile modificare le impostazioni di configurazione che accompagnano il pacchetto di distribuzione web, ad esempio le stringhe di connessione, gli endpoint del servizio e i dettagli di destinazione di IIS.
- Distribuzione Web, con la Pipeline di pubblicazione sul Web, offre un numero elevato di punti di estendibilità che consentono di personalizzare le distribuzioni. Ad esempio, è facile da escludere file e cartelle dai pacchetti distribuzione web.

**Utilizzare l'utilità VSDBCMD.exe per distribuire e aggiornare gli schemi di database.**

- VSDBCMD consente di distribuire i database da un file di schema di database (dbschema), che viene generato quando si compila un progetto di database di Visual Studio. Al contrario, la funzionalità di distribuzione di database inclusa nella distribuzione Web è più adatta per la distribuzione di database esistenti da un'istanza di SQL Server locale.
- A differenza delle funzionalità di Visual Studio per la distribuzione di progetti di database, VSDBCMD consente di distribuire gli aggiornamenti differenziali in un database di destinazione esistente. Ciò consente di mantenere i dati esistenti durante l'aggiornamento dello schema del database.
- È possibile eseguire comandi VSDBCMD all'interno dei file di progetto MSBuild personalizzati.

## <a name="content-map"></a>Mappa del contenuto

In questa esercitazione è inclusi argomenti che possono essere suddivise in quattro aree principali.

Questi argomenti la soluzione di riferimento & #x 2014; la soluzione di gestione di contatto & #x 2014; e viene descritto come scaricarlo e configurarlo nel computer locale:

- [La soluzione di gestione di contatto](the-contact-manager-solution.md)
- [Impostazione della soluzione di gestione di contatto](setting-up-the-contact-manager-solution.md)

Questi argomenti introdurre i file di progetto MSBuild, viene descritto come è possibile creare e utilizzare i file di progetto personalizzato e scorrere il processo di distribuzione per la soluzione di gestione di contatto:

- [Informazioni sui File di progetto](understanding-the-project-file.md)
- [Informazioni sul processo di compilazione](understanding-the-build-process.md)

Questi argomenti descrivono la distribuzione di applicazioni web, tra cui la creazione di pacchetti e alla build funzionamento sul processo, come il processo di compilazione si integra con la Pipeline di pubblicazione sul Web, come modificare i parametri di distribuzione e come distribuire i pacchetti web di destinazione ambienti:

- [Compilazione e l'assemblaggio di progetti di applicazione Web](building-and-packaging-web-application-projects.md)
- [Configurazione dei parametri per la distribuzione di pacchetto Web](configuring-parameters-for-web-package-deployment.md)
- [Distribuzione di pacchetti Web](deploying-web-packages.md)

- [Distribuzione di progetti di Database](deploying-database-projects.md) descrive le diverse tecniche che è possibile utilizzare per distribuire i progetti di database di Visual Studio, con i vantaggi e svantaggi di ogni approccio. [Creazione ed esecuzione di un File di comando di distribuzione](creating-and-running-a-deployment-command-file.md) viene descritto come creare un semplice file di comando che incapsula la logica di distribuzione e consente di distribuire soluzioni complesse come processo un singolo passaggio.
- Infine, [manualmente l'installazione di pacchetti di Web](manually-installing-web-packages.md) si conclude l'esercitazione con la visualizzazione è possibile importare pacchetti web in IIS.

## <a name="key-technologies"></a>Tecnologie principali

Negli argomenti di questa esercitazione principalmente usare queste tecnologie per la gestione di compilazione e distribuzione:

- Visual Studio 2010
- MSBuild
- IIS 7.5
- Web Deploy 2.0
- L'utilità di distribuzione di database VSDBCMD.exe

## <a name="other-tutorials-in-this-series"></a>Altre esercitazioni di questa serie

Questo fa parte di una serie di cinque esercitazioni su distribuzione web su larga scala. Ecco le altre esercitazioni nella serie:

- [Distribuzione di applicazioni Web in scenari aziendali](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Questo contenuto introduttivo fornisce lo sfondo di scelta rapida per la serie di esercitazioni. Viene descritto lo scenario dell'esercitazione e viene illustrato come le attività e procedure dettagliate descritte in tutta la serie rientrano in un processo più ampio di Application Lifecycle Management (ALM).
- [Configurazione degli ambienti di Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In questa esercitazione viene descritto come configurare i server di Windows per supportare diversi scenari di distribuzione, inclusa la distribuzione di pacchetto web remoto tramite il servizio agente di distribuzione Web (l'agente remoto) o il gestore di distribuzione Web e la distribuzione del database remoto. Vengono fornite informazioni aggiuntive su come scegliere il metodo di distribuzione appropriata per il proprio ambiente e viene descritto come utilizzare la Web Farm Framework (WFF) per replicare le applicazioni web distribuite tra tutti i server web in una server farm.
- [Configurazione di Team Foundation Server per la distribuzione Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In questa esercitazione viene descritto come configurare TFS per supportare diversi scenari di distribuzione, tra cui la distribuzione automatica come parte di un processo di integrazione continua e attivate manualmente le distribuzioni di compilazioni specifiche.
- [Distribuzione Web aziendale avanzate](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In questa esercitazione viene illustrato come eseguire diverse attività di distribuzione più avanzate, ad esempio personalizzare le distribuzioni di database per più ambienti, esclusione dalla distribuzione di file e cartelle e l'esecuzione di applicazioni web offline durante il processo di distribuzione .

>[!div class="step-by-step"]
[Successivo](the-contact-manager-solution.md)
