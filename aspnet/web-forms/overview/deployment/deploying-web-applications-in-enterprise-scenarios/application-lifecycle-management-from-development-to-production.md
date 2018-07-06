---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Application Lifecycle Management: Dallo sviluppo alla produzione | Microsoft Docs'
author: jrjlee
description: In questo argomento viene illustrato come una società fittizia gestisce la distribuzione di un'applicazione web ASP.NET tramite gli ambienti di test, staging e produzione come par...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 47af9504bdef294b987cdd23ab1bcefbeadd4681
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808979"
---
<a name="application-lifecycle-management-from-development-to-production"></a>Application Lifecycle Management: Dallo sviluppo alla produzione
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene illustrato come una società fittizia gestisce la distribuzione di un'applicazione web ASP.NET tramite gli ambienti di test, staging e produzione come parte di un processo di sviluppo continuo. In tutta l'argomento, vengono forniti collegamenti a informazioni aggiuntive e procedure dettagliate su come eseguire attività specifiche.
> 
> L'argomento è progettato per fornire una panoramica generale per un [serie di esercitazioni](deploying-web-applications-in-enterprise-scenarios.md) su distribuzione web aziendale. Non occorre preoccuparsi se non si ha familiarità con alcuni dei concetti descritti di seguito&#x2014;le esercitazioni che seguono forniscono informazioni dettagliate su tutte queste attività e tecniche.
> 
> > [!NOTE]
> > I migliori risultati Forthe di semplicità, in questo argomento non vengono trattate aggiornamento database come parte del processo di distribuzione. Tuttavia, rendendo gli aggiornamenti incrementali per le funzionalità dei database è un requisito di molti scenari di distribuzione dell'organizzazione ed è possibile trovare istruzioni su come eseguire questa operazione più avanti in questa serie di esercitazioni. Per altre informazioni, vedere [distribuire progetti di Database](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Panoramica

Il processo di distribuzione illustrato in questa schermata si basa sullo scenario di distribuzione di Fabrikam, Inc. descritto nella [distribuzione Web aziendale: panoramica dello Scenario](enterprise-web-deployment-scenario-overview.md). È consigliabile leggere la panoramica dello scenario prima di esaminare questo argomento. In pratica, lo scenario esamina come un'organizzazione gestisce la distribuzione di un'applicazione web ragionevolmente complesse, il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), attraverso diverse fasi in un tipico ambiente aziendale.

A livello generale, la soluzione Contact Manager passa attraverso queste fasi come parte del processo di distribuzione e di sviluppo:

1. Uno sviluppatore archivia codice in Team Foundation Server (TFS) 2010.
2. TFS compila il codice ed esegue tutti gli unit test associati al progetto team.
3. TFS consente di distribuire la soluzione nell'ambiente di test.
4. Il team di sviluppatori verifica e convalida la soluzione nell'ambiente di test.
5. L'amministratore dell'ambiente gestione temporanea esegue una distribuzione "what if" per l'ambiente di gestione temporanea, per stabilire se la distribuzione si verificheranno eventuali problemi.
6. L'amministratore dell'ambiente gestione temporanea esegue una distribuzione in tempo reale all'ambiente di staging.
7. La soluzione viene sottoposta a test nell'ambiente di staging di accettazione utente.
8. I pacchetti di distribuzione web sono importati manualmente nell'ambiente di produzione.

Queste fasi fanno parte di un ciclo di sviluppo continuo.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

In pratica, il processo è leggermente più complesso, come si vedrà, quando si esamina ogni fase in modo più dettagliato. Fabrikam, Inc. Usa un approccio diverso alla distribuzione per ogni ambiente di destinazione.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Il resto di questo argomento esamina queste fasi principali di questo ciclo di vita di distribuzione:

- **Prerequisiti**: come è necessario configurare l'infrastruttura di server prima di inserire logica di distribuzione in posizione.
- **Sviluppo e distribuzione iniziale**: che cosa è necessario eseguire prima di distribuire la soluzione per la prima volta.
- **Distribuzione per il test**: come creare pacchetti e Distribuisci automaticamente contenuto per un ambiente di test quando uno sviluppatore archivia nuovo codice.
- **Distribuzione di gestione temporanea**: come distribuire specifici delle build di un ambiente di staging ed eseguire "what if" distribuzioni per assicurarsi che una distribuzione non causa alcun problema.
- **Distribuzione nell'ambiente di produzione**: come importare pacchetti web in un ambiente di produzione quando l'infrastruttura di rete impedisce la distribuzione remota.

## <a name="prerequisites"></a>Prerequisiti

La prima attività in qualsiasi scenario di distribuzione è per garantire che l'infrastruttura server soddisfi i requisiti di strumenti di distribuzione e tecniche. In questo caso, Fabrikam, Inc. ha configurato l'infrastruttura server simile al seguente:

- TFS è configurato per includere una raccolta di progetti team, i controller di compilazione e agenti di compilazione. Visualizzare [configurazione di Team Foundation Server per la distribuzione di Web automatizzata](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) per altre informazioni.
- L'ambiente di test è configurato per accettare le distribuzioni remote usando il servizio agente di distribuzione Web (il "agente remoto"), come descritto in [Scenario: configurazione di un ambiente di Test per la distribuzione Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) e [ Configurare un Server Web per la pubblicazione (agente remoto) di distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- Ambiente di gestione temporanea è configurato per accettare le distribuzioni remote usando l'endpoint di gestore distribuzione Web, come descritto in [Scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) e [configurare un Server Web Pubblicazione con distribuzione per il Web (gestore di distribuzione Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- L'ambiente di produzione è configurato per consentire un amministratore importare manualmente i pacchetti di distribuzione web in Internet Information Services (IIS), come descritto in [Scenario: configurazione di un ambiente di produzione per la distribuzione Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) e [configurare un Server Web per la pubblicazione (distribuzione Offline) con distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Attività iniziali di sviluppo e distribuzione

Prima che il team di sviluppo Fabrikam, Inc. è possibile distribuire la soluzione Contact Manager per la prima volta, è necessario eseguire queste attività:

- Creare un nuovo progetto team in TFS.
- Creare i file di progetto di Microsoft Build Engine (MSBuild) che contengono la logica di distribuzione.
- Creare le definizioni di compilazione TFS che attivano i processi di distribuzione.

### <a name="create-a-new-team-project"></a>Creare un nuovo progetto Team

- L'amministratore TFS, Rob Walters, crea un nuovo progetto team per l'applicazione, come descritto in [la creazione di un progetto Team in TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Successivamente, lo sviluppatore responsabile, Matt Hink, crea una soluzione di base. Egli controlla suoi file nel nuovo progetto team in TFS, come descritto in [aggiunta di contenuto al controllo del codice sorgente](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Creare la logica di distribuzione

Matt Hink crea diversi personalizzati file progetto MSBuild, usando l'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt crea:

- Un file di progetto denominato *Publish.proj* che viene eseguito il processo di distribuzione. Questo file contiene le destinazioni di MSBuild di compilano i progetti nella soluzione, creano pacchetti web e distribuire i pacchetti in un ambiente di server di destinazione.
- I file di progetto specifici dell'ambiente denominati *Env-Dev.proj* e *Env-Stage.proj*. Contengono le impostazioni specifiche per l'ambiente di test e l'ambiente di gestione temporanea, rispettivamente, come le stringhe di connessione, gli endpoint di servizio e i dettagli del servizio remoto che riceverà il pacchetto di web. Per indicazioni su come scegliere le impostazioni corrette per gli ambienti di destinazione specifico, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Per eseguire la distribuzione, un utente esegue il *Publish.proj* file utilizzando MSBuild o Team Build e specifica il percorso del file di progetto specifici dell'ambiente pertinente (*Env-Dev.proj* o*Env-Stage.proj*) come argomento della riga di comando. Il *Publish.proj* file quindi Importa il file di progetto specifici dell'ambiente per creare un set completo di istruzioni per ogni ambiente di destinazione di pubblicazione.

> [!NOTE]
> Il modo di che lavorare di questi file di progetto personalizzato è indipendenti dal meccanismo che consente di richiamare MSBuild. Ad esempio, è possibile usare la riga di comando di MSBuild direttamente, come descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). È possibile eseguire i file di progetto da un file di comando, come descritto in [creare ed eseguire un File di comando di distribuzione](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). In alternativa, è possibile eseguire i file di progetto da una definizione di compilazione in TFS, come descritto in [creazione di una definizione di compilazione che la distribuzione supporta](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> In ogni caso il risultato finale è lo stesso&#x2014;MSBuild esegue il file di progetto unito e distribuisce la soluzione nell'ambiente di destinazione. Ciò offre una notevole flessibilità nel modo in cui si attiva il processo di pubblicazione.


Dopo che ha creato i file di progetto personalizzati, Matt li aggiunge a una cartella della soluzione e li archivia nel controllo del codice sorgente.

### <a name="create-build-definitions"></a>Creazione delle definizioni di compilazione

Come un'attività di preparazione finale, Matt e Rob collaborano per creare tre definizioni di compilazione per il nuovo progetto team:

- **DeployToTest**. Ciò consente di compilare la soluzione Contact Manager e la distribuisce nell'ambiente di test ogni volta che si verifica un controllo.
- **DeployToStaging**. Ciò consente di distribuire risorse da una compilazione precedente specificata all'ambiente di staging quando uno sviluppatore mette in coda la compilazione.
- **DeployToStaging-WhatIf**. Questa operazione viene eseguita una distribuzione "what if" per l'ambiente di staging quando uno sviluppatore mette in coda la compilazione.

Le sezioni seguenti forniscono altri dettagli su ognuna di queste definizioni di compilazione.

## <a name="deployment-to-test"></a>Distribuzione di Test

Il team di sviluppo Fabrikam, Inc. gestisce gli ambienti di test per eseguire un'ampia gamma di test di attività, ad esempio verifica e convalida, test di usabilità, test di compatibilità e di test ad hoc o esplorativo del software.

Il team di sviluppo ha creato una definizione di compilazione in TFS denominata **DeployToTest**. Questa definizione di compilazione Usa un trigger di integrazione continua, che significa che il processo di compilazione viene eseguita ogni volta che un membro del team di sviluppo Fabrikam, Inc. consente di eseguire un controllo aggiuntivo. Quando viene attivata una compilazione, la definizione di compilazione sarà:

- Compilare la soluzione ContactManager.sln. Ciò a sua volta compila ogni progetto nella soluzione.
- Eseguire unit test nella struttura di cartelle della soluzione (se la soluzione venga compilata correttamente).
- Eseguire i file di progetto personalizzati che consentono di controllare il processo di distribuzione (se la soluzione viene compilata correttamente e passa tutti gli unit test).

Il risultato finale è che, se la soluzione viene compilata correttamente e passa gli unit test, i pacchetti web ed eventuali altre risorse di distribuzione vengano distribuite nell'ambiente di test.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Come funziona il processo di distribuzione?

Il **DeployToTest** questi argomenti a MSBuild forniture di definizione di compilazione:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


Il **DeployOnBuild = true** e **DeployTarget = pacchetto** proprietà vengono usate quando Team Build Compila i progetti all'interno della soluzione. Quando il progetto è un progetto di applicazione web, queste proprietà indicare a MSBuild per creare un pacchetto di distribuzione web per il progetto. Il **TargetEnvPropsFile** proprietà indica il *Publish.proj* dove trovare il file di progetto specifici dell'ambiente per l'importazione del file.

> [!NOTE]
> Per una procedura dettagliata su come creare una definizione di compilazione simile al seguente, vedere [creazione di una definizione di compilazione che la distribuzione supporta](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Il *Publish.proj* file contiene le destinazioni che compila ogni progetto nella soluzione. Tuttavia, include anche la logica condizionale che ignora gli compilazione destinazioni se si sta tentando di eseguire il file in Team Build. Ciò consente di sfruttare i vantaggi delle funzionalità di compilazione aggiuntivi Team Build offre, come la possibilità di eseguire unit test. Se la compilazione della soluzione o l'unità di test hanno esito negativo, il *Publish.proj* file non verrà eseguito e non verrà distribuita l'applicazione.

La logica condizionale viene eseguita valutando la **BuildingInTeamBuild** proprietà. Si tratta di una proprietà di MSBuild che viene impostata automaticamente su **true** quando si utilizza Team Build per compilare i progetti.

## <a name="deployment-to-staging"></a>Distribuzione di gestione temporanea

Quando una compilazione soddisfa tutti i requisiti del team di sviluppo nell'ambiente di test, il team consiglia di distribuire la stessa build in un ambiente di staging. Gli ambienti di staging sono in genere configurati in modo da corrispondere le caratteristiche dell'ambiente di produzione o "attivi" come strettamente quanto possibile, ad esempio, in termini di specifiche del server, i sistemi operativi e software e la configurazione di rete. Gli ambienti di staging vengono spesso usati per il test di carico, test di accettazione utente e le revisioni interne necessarie più ampie. Le compilazioni vengono distribuite all'ambiente di staging direttamente dal server di compilazione.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Le definizioni di compilazione usate per distribuire la soluzione per l'ambiente di gestione temporanea **DeployToStaging-WhatIf** e **DeployToStaging**, condividere queste caratteristiche:

- Essi non compilare effettivamente alcuna operazione. Quando l'ambiente di gestione temporanea, Rob distribuisce la soluzione, vuole distribuire una build esistente e specifica che è già stata verificata e convalidata nell'ambiente di test. Le definizioni di compilazione sufficiente eseguire i file di progetto personalizzati che consentono di controllare il processo di distribuzione.
- Quando viene attivata una compilazione Rob, Usa i parametri di compilazione per specificare la compilazione che contiene le risorse che Alessandro desidera distribuire dal server di compilazione.
- Le definizioni di compilazione non vengono attivate automaticamente. Rob accoda manualmente una compilazione quando si vuole distribuire la soluzione in ambiente di gestione temporanea.

Questo è il processo generale per una distribuzione in ambiente di gestione temporanea:

1. L'amministratore dell'ambiente gestione temporanea, Rob Walters, Accoda una compilazione usando il **DeployToStaging-WhatIf** definizione di compilazione. Rob Usa i parametri di definizione di compilazione per specificare quali build Alessandro desidera distribuire.
2. Il **DeployToStaging-WhatIf** esecuzioni di definizione di compilazione i file di progetto personalizzato in modalità di "what if". Questo genera file di log come se Rob stava eseguendo una distribuzione in tempo reale, ma in realtà non apporta alcuna modifica nell'ambiente di destinazione.
3. Rob esamina i file di log per conoscere gli effetti della distribuzione nell'ambiente di staging. In particolare, Rob desidera controllare ciò che verranno aggiunti, cosa verrà aggiornata e cosa sarà eliminato.
4. Se Rob è soddisfatta che la distribuzione non apportare modifiche indesiderate alle risorse esistenti o dei dati, egli Accoda una compilazione usando il **DeployToStaging** definizione di compilazione.
5. Il **DeployToStaging** esecuzioni di definizione di compilazione i file di progetto personalizzato. Questi pubblicare le risorse per la distribuzione nel server web primario nell'ambiente di staging.
6. Il controller della Web Farm Framework (WFF) consente di sincronizzare i server web nell'ambiente di staging. Ciò rende l'applicazione disponibile in tutti i server web nella server farm.

### <a name="how-does-the-deployment-process-work"></a>Come funziona il processo di distribuzione?

Il **DeployToStaging** questi argomenti a MSBuild forniture di definizione di compilazione:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


Il **TargetEnvPropsFile** proprietà indica il *Publish.proj* dove trovare il file di progetto specifici dell'ambiente per l'importazione del file. Il **OutputRoot** proprietà sostituisce il valore predefinito e indica la posizione della cartella di compilazione che contiene le risorse da distribuire. Quando Rob mette in coda la compilazione, Usa la **parametri** pressione di tab per fornire un valore aggiornato per il **OutputRoot** proprietà.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Per altre informazioni su come creare una definizione di compilazione simile al seguente, vedere [distribuire un'istanza di Build specifico](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


Il **DeployToStaging-WhatIf** definizione di compilazione contiene la stessa logica di distribuzione come la **DeployToStaging** definizione di compilazione. Tuttavia, include l'argomento aggiuntivo **WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


All'interno di *Publish.proj* file, il **WhatIf** proprietà indica che tutte le risorse di distribuzione devono essere pubblicate in modalità di "what if". In altre parole, i file di log vengono generati come se la distribuzione ha voluto, ma nulla viene modificato effettivamente nell'ambiente di destinazione. Ciò consente di valutare l'impatto di una distribuzione proposta&#x2014;in particolare, ciò che verranno aggiunti, cosa verrà aggiornata e che cosa verrà eliminata&#x2014;prima di apportare effettivamente le modifiche.

> [!NOTE]
> Per altre informazioni su come configurare le distribuzioni di "what if", vedere [esecuzione di una distribuzione "What If"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Dopo aver distribuito l'applicazione per il server web primario nell'ambiente di staging, il WFF verranno automaticamente sincronizzati l'applicazione su tutti i server nella server farm.

> [!NOTE]
> Per altre informazioni sulla configurazione di WFF per sincronizzare i server web, vedere [creare una Server Farm con Web Farm Framework](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Distribuzione nell'ambiente di produzione

Quando una compilazione è stata approvata nell'ambiente di staging, il team di Fabrikam, Inc. può pubblicare l'applicazione nell'ambiente di produzione. L'ambiente di produzione è in cui l'applicazione sarà "attivo" e raggiunge destinato agli utenti finali.

L'ambiente di produzione è in una rete perimetrale con connessione Internet. Ciò è isolata dalla rete interna che contiene il server di compilazione. L'amministratore di ambiente di produzione, Lisa Andrews, manualmente è necessario copiare i pacchetti di distribuzione web dal server di compilazione e importarli in IIS sul server web di produzione primario.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Questo è il processo generale per una distribuzione nell'ambiente di produzione:

1. Il team di sviluppatori informa Lisa che una compilazione è pronta per la distribuzione nell'ambiente di produzione. Il team indicato Lisa della posizione dei pacchetti di distribuzione web all'interno della cartella di rilascio nel server di compilazione.
2. Lisa raccoglie i pacchetti web dal server di compilazione e li copia nel server web primario nell'ambiente di produzione.
3. Davide Usa Gestione IIS per importare e pubblicare i pacchetti web sul server web primario.
4. Il controller WFF consente di sincronizzare i server web nell'ambiente di produzione. Ciò rende l'applicazione disponibile in tutti i server web nella server farm.

### <a name="how-does-the-deployment-process-work"></a>Come funziona il processo di distribuzione?

Gestione IIS include una creazione guidata pacchetto di applicazione di importazione che rende più semplice pubblicare pacchetti web in un sito Web IIS. Per una procedura dettagliata su come eseguire questa procedura, vedere [manualmente l'installazione dei pacchetti Web](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Conclusione

In questo argomento fornita un'illustrazione del ciclo di vita di distribuzione per un'applicazione web su scala aziendale tipico.

In questo argomento fa parte di una serie di esercitazioni che forniscono informazioni aggiuntive su vari aspetti della distribuzione di applicazioni web. In pratica, sono disponibili numerose considerazioni e attività aggiuntive in ogni fase del processo di distribuzione e non è possibile illustrarle tutte in una procedura dettagliata di single. Per altre informazioni, consultare queste esercitazioni:

- [Distribuzione nell'organizzazione Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione offre un'introduzione completa a tecniche di distribuzione web utilizzando MSBuild e lo strumento di distribuzione Web IIS (distribuzione Web).
- [Configurazione degli ambienti Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Questa esercitazione fornisce indicazioni su come configurare gli ambienti di Windows server per supportare diversi scenari di distribuzione.
- [Configurazione di Team Foundation Server per la distribuzione Web automatica](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Questa esercitazione fornisce indicazioni su come integrare la logica di distribuzione nei processi di compilazione TFS.
- [Distribuzione Web aziendale avanzata](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Questa esercitazione fornisce indicazioni su come soddisfare alcune delle problematiche di distribuzione più complessi in un'organizzazione.

> [!div class="step-by-step"]
> [Precedente](enterprise-web-deployment-scenario-overview.md)
