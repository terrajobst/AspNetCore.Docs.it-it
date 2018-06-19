---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Distribuzione Web aziendale: Panoramica dello Scenario | Documenti Microsoft'
author: jrjlee
description: Questo set di esercitazioni utilizza una soluzione di esempio con un livello di complessità, insieme a uno scenario di distribuzione dell'organizzazione fittizia, realistico per fornire un riferimento...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/03/2012
ms.topic: article
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 20f6e206d6aa4bebb4936246468f5ada0e213236
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890021"
---
<a name="enterprise-web-deployment-scenario-overview"></a>Distribuzione Web aziendale: Panoramica dello Scenario
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questo set di esercitazioni Usa una soluzione di esempio con un livello di complessità, insieme a uno scenario di distribuzione dell'organizzazione fittizia, realistico per fornire un'implementazione di riferimento e per assegnare le attività e procedure dettagliate di un contesto comune. In questo argomento viene descritto lo scenario dell'esercitazione e introduce la soluzione di esempio.


## <a name="scenario-description"></a>Descrizione dello scenario

Fabrikam, Inc., una società fittizia, consiste nel creare una soluzione che consente di archiviare e recuperare le informazioni di contatto da un'interfaccia web remoto team di vendita.

I processi di Application Lifecycle Management (ALM) Fabrikam, Inc. richiedono la soluzione deve essere distribuito in varie fasi del processo di sviluppo software e tre gli ambienti server:

- Un ambiente di test o "sandbox" developer.
- Un ambiente di gestione temporanea basato sulla intranet.
- Un ambiente di produzione con connessione Internet.

Ciascuno di questi ambienti ha requisiti di sicurezza e di configurazione diverso e ognuno comporta dei problemi di distribuzione univoco.

### <a name="the-fabrikam-inc-server-infrastructure"></a>The Fabrikam, Inc. Infrastruttura di server

Questo è l'infrastruttura di sviluppo e distribuzione ad alto livello Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Le workstation di sviluppo, l'infrastruttura di controllo di origine, l'ambiente di test di sviluppo e l'ambiente di gestione temporanea che si trovano tutti nella rete intranet all'interno del dominio Fabrikam.net. L'ambiente di produzione si trova in una rete perimetrale (detta anche DMZ DMZ e subnet schermata), che è isolata dalla rete intranet da un firewall. Si tratta di uno scenario di distribuzione comune: è in genere isolare i server web con connessione Internet dall'infrastruttura interno del server tramite l'utilizzo di firewall o server gateway.

In questo esempio:

- Un server di Team Foundation Server (TFS) 2010 con un server di compilazione separata fornisce funzionalità di integrazione continua (CI) e di controllo del codice sorgente.
- L'ambiente di test per sviluppatori include un server web Internet Information Services (IIS) 7.5 e un server di database di SQL Server 2008 R2.
- L'ambiente di produzione include più server web di IIS 7.5 sincronizzato da un server di controller di Web Farm Framework (WFF), insieme a un server di database di SQL Server 2008 R2. In pratica, il server di database può utilizzare clustering o il mirroring per migliorare la scalabilità e disponibilità.
- Ambiente di gestione temporanea è progettato per replicare la configurazione dell'ambiente di produzione fedelmente possibile.
- I criteri di isolamento di rete e firewall non consente di limitare diretta, automatizzata distribuzione dalla rete intranet per la rete perimetrale.

La configurazione di ciascuno di questi ambienti è descritta in dettaglio nell'esercitazione secondo [configurazione di ambienti Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Ruoli del team per ALM

Questi utenti coinvolti nella creazione, la gestione, la creazione e pubblicazione della soluzione di gestione di contatto:

- Matt Hink è uno sviluppatore di applicazioni web Fabrikam, Inc. È parte del team che ha sviluppato la soluzione di gestione di contatto tramite Visual Studio 2010. Matt dispone di diritti di amministrazione completi per i server nell'ambiente di test per sviluppatori, che gli consente di configurare l'ambiente in base alle sue esigenze. Ha anche l'accesso all'istanza di Visual Studio 2010 TFS in cui archivia il codice sorgente per la soluzione di gestione di contatto.
- Rob Walters è un amministratore del server per il team di sviluppo di Fabrikam, Inc. Ezio dispone di accesso amministrativo al server TFS in modo che è possibile configurare tutti gli aspetti di TFS e Team Build. Inoltre, Rob dispone di accesso amministrativo per il test e il server web di gestione temporanea e funziona come l'amministratore del database (DBA) per i server di database di test e ambienti di gestione temporanea. Rob configurato Team Build nel server TFS per eseguire queste attività:

    - Compilare ed eseguire unit test sull'applicazione ogni volta che un utente archivia in un file in TFS. Si tratta degli elementi di configurazione.
    - Distribuire l'applicazione Contact Manager nell'ambiente di testing automaticamente dopo l'applicazione passa unit test. Ciò include il database di pubblicazione per il server di prova nella distribuzione iniziale ed eventuali aggiornamenti al database dopo la distribuzione iniziale.
    - Distribuire l'applicazione Contact Manager all'ambiente di gestione temporanea in un singolo passaggio di processo.
    - Creare un pacchetto Web che un amministratore del server Web e un amministratore di database possono utilizzare per pubblicare l'applicazione nell'ambiente di produzione.
- Lisa Andrews sia un amministratore del server responsabile della distribuzione di applicazioni per i server di produzione di Fabrikam, Inc. Utente con accesso in lettura alla condivisione in cui il Team di TFS Build archivia il pacchetto di distribuzione web quando compila l'applicazione Gestione contatti. Stella inoltre dispone di accesso amministrativo ai server web di produzione in modo che l'utente può distribuire l'applicazione nell'ambiente di produzione. Lei funge inoltre, l'amministratore di database che distribuisce i database e gli aggiornamenti del database al server di database nell'ambiente di produzione.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>La soluzione di gestione di contatto

La soluzione Contact Manager è progettata per consentire agli utenti registrati, connesso di aggiungere e modificare le informazioni di contatto tramite un'interfaccia web. La soluzione di gestione di contatto è costituita da quattro progetti singoli:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Si tratta di un progetto di applicazione web ASP.NET MVC3 che rappresenta il punto di ingresso per la soluzione. Offre alcune funzionalità di applicazione web di base, ad esempio fornire agli utenti la possibilità di creare e visualizzare i dettagli di contatto. L'applicazione si basa su un servizio Windows Communication Foundation (WCF) per gestire i contatti e un database di servizi dell'applicazione ASP.NET per gestire l'autenticazione e autorizzazione.
- **ContactManager.Database**. Si tratta di un progetto di database di Visual Studio 2010. Il progetto definisce lo schema per un database che archivia i dettagli di contatto.
- **ContactManager.Service**. Si tratta di un progetto di servizio web WCF. L'oggetto espone WCF crea un endpoint che consente ai chiamanti di eseguire, recuperare l'aggiornamento ed eliminazione (CRUD) sul database di gestione di contatto. Il servizio si basa sul database di gestione di contatto e assembly ContactManager.Common.dll.
- **ContactManager.Common**. Si tratta di un progetto libreria di classi. Il servizio WCF si basa sui tipi definiti in questo assembly.

Viene fornita un'analisi completa della soluzione e i requisiti di distribuzione nella prima esercitazione di questa serie, [distribuzione Web dell'azienda](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Attività di distribuzione

Esistono diverse attività distinte coinvolti nella distribuzione di applicazioni in ambienti diversi in un'organizzazione di grandi dimensioni. Queste sono le attività principali che coprono le esercitazioni:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Di seguito è riportato un elenco di ogni passaggio nel processo di distribuzione dal punto di vista degli utenti descritto in precedenza in questo documento:

1. Tutti i membri del team di rivedere la soluzione di gestione di contatto in Visual Studio 2010 per determinare i problemi e i requisiti principali per la distribuzione.
2. Matt Hink può distribuire la soluzione di gestione contatto direttamente dalla workstation dello sviluppatore nell'ambiente di test per sviluppatori, per condurre un test iniziale della logica di distribuzione.
3. Matt Hink aggiunge all'applicazione di controllo del codice sorgente in TFS.
4. Rob Walters crea diverse definizioni di compilazione per la soluzione di gestione di contatto in Team Build. Una definizione di compilazione Usa CI per distribuire la soluzione nell'ambiente di test per sviluppatori di ogni volta che un utente archivia il nuovo codice. Un'altra definizione di compilazione consente agli utenti le distribuzioni di trigger all'ambiente di gestione temporanea in base alle esigenze.
5. Ogni volta che un utente archivia il nuovo codice, Team Build Compila i componenti della soluzione, eseguire gli unit test e distribuisce automaticamente la soluzione per l'ambiente di test per sviluppatori, se la compilazione ha esito positivo e unit test superati.
6. Quando un utente attiva una distribuzione all'ambiente di gestione temporanea, la soluzione viene fornita e distribuita in un singolo passaggio processo. Questo processo genera anche un pacchetto per la distribuzione manuale nell'ambiente di produzione.
7. Lisa Andrews distribuisce l'applicazione nell'ambiente di produzione per importare manualmente il pacchetto web creato nel passaggio 6.

### <a name="key-deployment-issues"></a>Problemi principali per la distribuzione

La soluzione di gestione di contatto e lo scenario di Fabrikam, Inc. evidenziare diversi i problemi comuni e i problemi che possono verificare quando si distribuisce soluzioni su larga scala e complesso. Ad esempio:

- È necessario essere in grado di distribuire progetti in più ambienti, ad esempio per sviluppatori o ambienti di test di gestione temporanea, piattaforme e i server di produzione. La soluzione deve essere distribuito con le impostazioni di configurazione diverse per ogni ambiente.
- È necessario distribuire come parte di un processo di compilazione e distribuzione singolo passaggio o automatizzato contemporaneamente più progetti dipendenti.
- È necessario essere in grado di distribuzione di unità da un processo automatizzato. Ad esempio, si desidera utilizzare un processo di elemento di configurazione per distribuire applicazioni web in un ambiente di gestione temporanea quando viene archiviato nuovo codice.
- È necessario essere in grado di controllare il processo di distribuzione e impostare le variabili di distribuzione dall'esterno di Visual Studio, come gli sviluppatori sono in genere non hanno le impostazioni di configurazione corretto o le credenziali necessarie per ogni ambiente di destinazione.
- È necessario distribuire progetti di database basati sullo schema e mantenere i dati esistenti nelle distribuzioni successive.
- È necessario distribuire il database di appartenenza in base ad hoc senza distribuire i dati dell'account utente. È inoltre potrebbe essere necessario aggiornare lo schema dei database di appartenenza distribuito senza perdere i dati dell'account utente esistente.
- È necessario escludere determinati file o cartelle quando si distribuisce contenuto in diversi ambienti di destinazione.

Inoltre, la gestione della distribuzione quando gli aggiornamenti sono frequenti e incrementale genera un'eccezione di alcuni problemi aggiuntivi. Ad esempio:

- Eseguire gli unit test ogni volta che uno sviluppatore archivia nel nuovo codice. Si desidera distribuire la soluzione, se il codice passa gli unit test.
- Quando si distribuisce un'applicazione web in un ambiente di gestione temporanea o produzione, si desidera reindirizzare gli utenti a un *app\_offline.htm* file per la durata del processo di distribuzione.
- Si desidera registrare le attività di distribuzione. Il processo di distribuzione deve inviare notifiche tramite posta elettronica delle distribuzioni di esito positivo o negativo di destinatari specificati.
- Se una distribuzione automatica non riesce, il processo di distribuzione deve ripetere la distribuzione corrente o distribuire il pacchetto web precedente.

> [!div class="step-by-step"]
> [Precedente](deploying-web-applications-in-enterprise-scenarios.md)
> [Successivo](application-lifecycle-management-from-development-to-production.md)
