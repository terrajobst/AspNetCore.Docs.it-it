---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Distribuzione Web aziendale: Panoramica dello Scenario | Microsoft Docs'
author: jrjlee
description: Questa serie di esercitazioni pratiche Usa una soluzione di esempio con un livello di complessità, insieme a uno scenario di distribuzione azienda fittizia, realistico per fornire un parametro ref...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: ec5b62f3991fa256bc8efe7abe9b953d61d1a515
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833968"
---
<a name="enterprise-web-deployment-scenario-overview"></a>Distribuzione Web aziendale: Panoramica dello Scenario
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Questa serie di esercitazioni pratiche Usa una soluzione di esempio con un livello di complessità, insieme a uno scenario di distribuzione azienda fittizia, realistico per fornire un'implementazione di riferimento e per assegnare le attività e procedure dettagliate di contesto comune. In questo argomento viene descritto lo scenario dell'esercitazione e introduce la soluzione di esempio.


## <a name="scenario-description"></a>Descrizione dello scenario

Fabrikam, Inc., una società fittizia, consiste nel creare una soluzione che consente ai team di vendita remoti archiviare e recuperare le informazioni di contatto da un'interfaccia web.

I processi di Application Lifecycle Management (ALM) Fabrikam, Inc. richiedono la soluzione per la distribuzione in tre ambienti del server in varie fasi del processo di sviluppo software:

- Un ambiente di test o "sandbox" per gli sviluppatori.
- Un ambiente di gestione temporanea basato sulla intranet.
- Un ambiente di produzione con connessione Internet.

Ognuno di questi ambienti ha requisiti di protezione e configurazione diversa, e ognuno pone sfide della distribuzione univoco.

### <a name="the-fabrikam-inc-server-infrastructure"></a>Fabrikam, Inc. Infrastruttura server

Si tratta dell'infrastruttura di sviluppo e la distribuzione ad alto livello in Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

La workstation di sviluppo, l'infrastruttura di controllo di origine, l'ambiente di test per sviluppatori e l'ambiente di staging che si trovano tutti nella rete intranet all'interno del dominio Fabrikam.net. In una rete perimetrale (detta anche DMZ DMZ e subnet schermata), che è isolata dalla rete intranet da un firewall si trova l'ambiente di produzione. Si tratta di uno scenario di distribuzione comune: è in genere isolare i server web con connessione Internet dell'infrastruttura server interni tramite l'uso di firewall o server gateway.

In questo esempio:

- Un server di Team Foundation Server (TFS) 2010 con un server di compilazione separata fornisce funzionalità di integrazione continua (CI) e controllo del codice sorgente.
- L'ambiente di test per sviluppatori include un server web Internet Information Services (IIS) 7.5 e un server di database di SQL Server 2008 R2.
- L'ambiente di produzione include più i server web IIS 7.5 sincronizzati da un server di controller di Web Farm Framework (WFF), insieme a un server di database di SQL Server 2008 R2. In pratica, il server di database può usare clustering o il mirroring per migliorare la scalabilità e disponibilità.
- Ambiente di gestione temporanea è progettata per replicare la configurazione dell'ambiente di produzione fedelmente possibile.
- I criteri di isolamento rete e firewall non consentono la distribuzione diretta e automatizzata dalla rete intranet alla rete perimetrale.

La configurazione della ognuno di questi ambienti è descritto più dettagliatamente nella seconda esercitazione [configurazione di ambienti Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Ruoli del team per ALM

Tali utenti siano coinvolti nella creazione, la gestione, compilazione e la pubblicazione della soluzione Contact Manager:

- Matt Hink è uno sviluppatore di applicazioni web presso Fabrikam, Inc. È parte del team ha sviluppato questa soluzione Contact Manager utilizzando Visual Studio 2010. Matt con diritti di amministratore completo per i server nell'ambiente di test per sviluppatori, che gli consente di configurare l'ambiente in base alle sue esigenze. Ha anche accesso utente all'istanza di Visual Studio 2010 TFS in cui si archivia il codice sorgente per la soluzione Contact Manager.
- Rob Walters è un amministratore del server per il team di sviluppo Fabrikam, Inc. Rob ha accesso come amministratore nel server TFS in modo che è possibile configurare tutti gli aspetti di TFS e Team Build. Rob inoltre dispone di accesso amministrativo per il test e i server web di staging e agisce come l'amministratore del database (DBA) per i server di database di test e ambienti di staging. Rob ha configurato Team Build nel server TFS per eseguire queste attività:

    - Compilare ed eseguire unit test sull'applicazione ogni volta che un utente archivia un file da TFS. Si tratta degli elementi di configurazione.
    - Distribuire automaticamente l'applicazione Contact Manager nell'ambiente di test dopo l'applicazione passa gli unit test. Ciò include il database di pubblicazione per il server di test nella distribuzione iniziale ed eventuali aggiornamenti al database dopo la distribuzione iniziale.
    - Distribuire l'applicazione Contact Manager all'ambiente di staging in un unico passaggio processo.
    - Creare un pacchetto Web che un amministratore del server Web e un amministratore di database possono utilizzare per pubblicare l'applicazione nell'ambiente di produzione.
- Lisa Andrews è responsabile della distribuzione di applicazioni per il server di produzione di Fabrikam, Inc. un amministratore del server. Anna ha accesso in lettura alla condivisione in cui il Team di TFS Build archivia il pacchetto di distribuzione web quando viene compilata l'applicazione Contact Manager. Anna ha anche accesso amministrativo ai server web di produzione, in modo che Anna possa distribuire l'applicazione nell'ambiente di produzione. Inoltre, Anna agisce come l'amministratore di database che consente di distribuire i database e gli aggiornamenti del database al server di database nell'ambiente di produzione.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>Soluzione Contact Manager

La soluzione di gestione dei contatti è progettata per consentire agli utenti registrati, ha eseguito l'accesso di aggiungere e modificare le informazioni di contatto tramite un'interfaccia web. La soluzione Contact Manager è costituita da quattro progetti singoli:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager.Mvc**. Si tratta di un progetto di applicazione web ASP.NET MVC 3 che rappresenta il punto di ingresso per la soluzione. Offre alcune funzionalità di applicazione web di base, ad esempio offrendo agli utenti la possibilità di creare e visualizzare i dettagli dei contatti. L'applicazione si basa su un servizio Windows Communication Foundation (WCF) per gestire i contatti e un database di servizi di applicazioni ASP.NET per gestire l'autenticazione e autorizzazione.
- **ContactManager.Database**. Si tratta di un progetto di database di Visual Studio 2010. Il progetto definisce lo schema per un database che archivia i dettagli di contatto.
- **ContactManager.Service**. Si tratta di un progetto di servizio web WCF. L'oggetto espone WCF crea un endpoint che consente ai chiamanti di eseguire, recuperare, aggiornare ed eliminazione (CRUD) nel database di gestione contatti. Il servizio si basa sul database Contact Manager e l'assembly ContactManager.Common.dll.
- **ContactManager.Common**. Si tratta di un progetto di libreria di classi. Il servizio WCF si basa sui tipi definiti in questo assembly.

Nella prima esercitazione di questa serie, viene fornita una revisione completa della soluzione e i requisiti di distribuzione [distribuzione Web aziendale](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Attività di distribuzione

Esistono diverse attività distinte coinvolti nella distribuzione delle applicazioni in ambienti diversi in un'organizzazione di grandi dimensioni. Queste sono le attività principali che coprono le esercitazioni:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Ecco un elenco di ogni passaggio nel processo di distribuzione dal punto di vista degli utenti descritto in precedenza in questo documento:

1. Tutti i membri del team di rivedere la soluzione Contact Manager in Visual Studio 2010, per determinare i problemi e requisiti di distribuzione chiave.
2. Matt Hink può distribuire la soluzione Contact Manager direttamente dalla workstation dello sviluppatore nell'ambiente di test per sviluppatori, per condurre un test iniziale della logica di distribuzione.
3. Matt Hink aggiunge all'applicazione di controllo del codice sorgente in TFS.
4. Rob Walters crea diverse definizioni di compilazione per la soluzione Contact Manager nel Team Build. Una definizione di compilazione Usa integrazione continua per distribuire la soluzione nell'ambiente di test per sviluppatori, ogni volta che un utente archivia nuovo codice. Un'altra definizione di compilazione consente agli utenti di attivare le distribuzioni nell'ambiente di gestione temporanea in base alle esigenze.
5. Ogni volta che un utente archivia nuovo codice, Team Build automaticamente compila i componenti della soluzione, eseguire gli unit test e distribuisce la soluzione per l'ambiente di test per sviluppatori se la compilazione ha avuto esito positivo e unit test superati.
6. Quando un utente viene attivata una distribuzione in ambiente di gestione temporanea, la soluzione è incluso nel pacchetto e distribuita in un unico passaggio processo. Questo processo genera anche un pacchetto per la distribuzione manuale per l'ambiente di produzione.
7. Lisa Andrews distribuisce l'applicazione nell'ambiente di produzione importando manualmente il pacchetto web creato nel passaggio 6.

### <a name="key-deployment-issues"></a>Problemi di distribuzione chiave

La soluzione Contact Manager e lo scenario di Fabrikam, Inc. evidenziare vari comuni e le problematiche che potrebbero verificarsi quando si distribuisce soluzioni su larga scala e complesso. Ad esempio:

- È necessario essere in grado di distribuire progetti in più ambienti, ad esempio per gli sviluppatori o ambienti di test, server di produzione e le piattaforme di gestione temporanea. La soluzione deve essere distribuito con le impostazioni di configurazione diversi per ogni ambiente.
- È necessario distribuire contemporaneamente come parte di un processo di compilazione e distribuzione automatizzato o passo a passo più progetti dipendenti.
- È necessario essere in grado di alla distribuzione di unità da un processo automatizzato. Ad esempio, si desidera utilizzare un processo di integrazione continua per distribuire applicazioni web in un ambiente di staging quando viene archiviato nuovo codice.
- È necessario essere in grado di controllare il processo di distribuzione e impostare le variabili di distribuzione dall'esterno di Visual Studio, come gli sviluppatori sono in genere non hanno le credenziali necessarie per ogni ambiente di destinazione o le impostazioni di configurazione corretto.
- È necessario distribuire i progetti di database basata sullo schema e mantenere i dati esistenti nelle distribuzioni successive.
- È necessario distribuire i database dei membri in base a hoc senza la distribuzione di dati dell'account utente. È necessario anche aggiornare lo schema dei database di appartenenza distribuito senza perdita di dati di account utente esistente.
- È necessario escludere determinati file o cartelle quando si distribuisce contenuto in diversi ambienti di destinazione.

Inoltre, la gestione di distribuzione quando gli aggiornamenti sono incrementali e frequenti genera un'eccezione di alcune problematiche aggiuntive. Ad esempio:

- Ogni volta che uno sviluppatore archivia nuovo codice, si eseguono unit test. Si vuole solo distribuire la soluzione se il codice passa gli unit test.
- Quando si distribuisce un'applicazione web in un ambiente di gestione temporanea o produzione, si desidera reindirizzare gli utenti a un *app\_offline.htm* file per la durata del processo di distribuzione.
- Si desidera registrare le attività di distribuzione. Il processo di distribuzione deve inviare le notifiche di posta elettronica delle distribuzioni di esito positivo o negativo ai destinatari designati.
- Se una distribuzione automatica non riesce, il processo di distribuzione deve ripetere la distribuzione corrente o distribuire il pacchetto web precedente.

> [!div class="step-by-step"]
> [Precedente](deploying-web-applications-in-enterprise-scenarios.md)
> [Successivo](application-lifecycle-management-from-development-to-production.md)
