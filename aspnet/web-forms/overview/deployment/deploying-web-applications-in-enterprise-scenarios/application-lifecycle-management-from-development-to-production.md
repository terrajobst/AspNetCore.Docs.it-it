---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Gestione del ciclo di vita delle applicazioni: Dallo sviluppo alla produzione | Documenti Microsoft'
author: jrjlee
description: "In questo argomento viene illustrato come una società fittizia gestisce la distribuzione di un'applicazione web ASP.NET tramite ambienti di test, gestione temporanea e produzione come par..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: f7ffff1c3434ce98c70265e4bf64047fd44252d0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="application-lifecycle-management-from-development-to-production"></a>Gestione del ciclo di vita delle applicazioni: Dallo sviluppo alla produzione
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento viene illustrato come una società fittizia gestisce la distribuzione di un'applicazione web ASP.NET tramite ambienti di test, gestione temporanea e produzione come parte di un processo di sviluppo continuo. All'argomento, vengono forniti collegamenti a ulteriori informazioni e procedure dettagliate su come eseguire attività specifiche.
> 
> L'argomento è progettato per fornire una panoramica di alto livello per un [serie di esercitazioni](deploying-web-applications-in-enterprise-scenarios.md) su distribuzione web dell'organizzazione. Non occorre preoccuparsi se non si ha familiarità con alcuni dei concetti descritti di seguito & #x 2014; le esercitazioni che seguono forniscono informazioni dettagliate su queste attività e tecniche.
> 
> > [!NOTE]
> > I migliori risultati Forthe di semplicità, in questo argomento non vengono trattate aggiornamento database come parte del processo di distribuzione. Tuttavia, eseguire aggiornamenti incrementali per le funzionalità di database è un requisito di molti scenari di distribuzione dell'organizzazione ed è possibile trovare istruzioni su come eseguire questa operazione, più avanti in questa serie di esercitazioni. Per ulteriori informazioni, vedere [progetti di Database di distribuzione](../web-deployment-in-the-enterprise/deploying-database-projects.md).


## <a name="overview"></a>Panoramica

Il processo di distribuzione illustrato di seguito si basa sullo scenario di distribuzione di Fabrikam, Inc. descritto in [distribuzione Web aziendale: panoramica dello Scenario](enterprise-web-deployment-scenario-overview.md). È consigliabile leggere la panoramica dello scenario prima di esaminare questo argomento. In pratica, lo scenario esamina il modo in cui un'organizzazione gestisce la distribuzione di un'applicazione web piuttosto complessi, il [Contact Manager soluzione](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), in varie fasi in un tipico ambiente aziendale.

In generale, la soluzione di gestione di contatto passa attraverso queste fasi come parte del processo di distribuzione e di sviluppo:

1. Uno sviluppatore archivia il codice in Team Foundation Server (TFS) 2010.
2. TFS compila il codice ed esecuzione di unit test associato al progetto team.
3. TFS consente di distribuire la soluzione nell'ambiente di testing.
4. Il team di sviluppo, verifica e la soluzione nell'ambiente di test di convalida.
5. L'amministratore di ambiente di gestione temporanea esegue una distribuzione "cosa accade se" nell'ambiente di gestione temporanea, per stabilire se la distribuzione verrà causa problemi.
6. L'amministratore di ambiente di gestione temporanea esegue una distribuzione in tempo reale all'ambiente di gestione temporanea.
7. La soluzione viene sottoposto a test nell'ambiente di gestione temporanea di accettazione utente.
8. I pacchetti di distribuzione web vengono manualmente importati nell'ambiente di produzione.

Queste fasi fanno parte di un ciclo di sviluppo continuo.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

In pratica, il processo è leggermente più complesso, come si vedrà quando si esamina ogni fase in modo più dettagliato. Fabrikam, Inc. utilizza un approccio diverso per la distribuzione per ogni ambiente di destinazione.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

Il resto di questo argomento vengono esaminate le fasi principali di questo ciclo di vita di distribuzione:

- **Prerequisiti**: come è necessario configurare l'infrastruttura del server prima di inserire la logica di distribuzione sul posto.
- **Sviluppo e distribuzione iniziale**: è necessario eseguire prima di distribuire la soluzione per la prima volta.
- **Distribuzione di test**: come creare un pacchetto e distribuire automaticamente il contenuto in un ambiente di test quando uno sviluppatore archivia nel nuovo codice.
- **Distribuzione di gestione temporanea**: la modalità di distribuzione specifico compila in un ambiente di gestione temporanea e come eseguire "cosa accade se" distribuzioni per garantire che una distribuzione non causa alcun problema.
- **Distribuzione nell'ambiente di produzione**: come importare pacchetti web in un ambiente di produzione quando l'infrastruttura di rete impedisce la distribuzione remota.

## <a name="prerequisites"></a>Prerequisiti

La prima attività in qualsiasi scenario di distribuzione consiste nel verificare che l'infrastruttura del server soddisfi i requisiti delle tecniche e gli strumenti di distribuzione. In questo caso, Fabrikam, Inc. è configurata l'infrastruttura di server simile al seguente:

- TFS è configurato per includere una raccolta di progetti team, i controller di compilazione e agenti di compilazione. Vedere [la configurazione di Team Foundation Server per la distribuzione di Web automatizzata](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) per ulteriori informazioni.
- L'ambiente di test è configurato per accettare le distribuzioni remote tramite il servizio agente di distribuzione Web (il "agente remoto"), come descritto in [Scenario: configurazione di un ambiente di Test per la distribuzione Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) e [ Configurare un Server Web per la pubblicazione (agente remoto) di distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- Ambiente di gestione temporanea è configurato per accettare le distribuzioni remote utilizzando l'endpoint di gestore distribuzione Web, come descritto in [Scenario: configurazione di un ambiente di gestione temporanea per la distribuzione Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) e [configurare un Server Web Pubblicazione di distribuzione Web (gestore distribuzione Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- L'ambiente di produzione è configurato per consentire a un amministratore importare manualmente i pacchetti di distribuzione web in Internet Information Services (IIS), come descritto in [Scenario: configurazione di un ambiente di produzione per la distribuzione Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) e [configurare un Server Web per la pubblicazione (distribuzione Offline) di distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Sviluppo iniziale e distribuzione

Il team di sviluppo di Fabrikam, Inc. è possibile distribuire la soluzione di gestione di contatto per la prima volta, è necessario eseguire le attività seguenti:

- Creare un nuovo progetto team in TFS.
- Creare i file di progetto di Microsoft Build Engine (MSBuild) che contengono la logica di distribuzione.
- Creare le definizioni di compilazione TFS che attivano i processi di distribuzione.

### <a name="create-a-new-team-project"></a>Creare un nuovo progetto Team

- L'amministratore TFS, Rob Walters, crea un nuovo progetto team per l'applicazione, come descritto in [la creazione di un progetto Team in TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Successivamente, lo sviluppatore responsabile, Matt Hink, crea una soluzione di base. Archivia i file nel nuovo progetto team in TFS, come descritto in [aggiunta di contenuto al controllo del codice sorgente](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Creare la logica di distribuzione

Matt Hink crea vari personalizzati file di progetto MSBuild, utilizzando l'approccio di file di progetto split descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt crea:

- Un file di progetto denominato *Publish.proj* che viene eseguito il processo di distribuzione. Questo file contiene le destinazioni di MSBuild di compilano i progetti nella soluzione, i pacchetti web di creare e distribuire i pacchetti in un ambiente server di destinazione.
- File di progetto specifici dell'ambiente denominati *Env Dev.proj* e *Env Stage.proj*. Che contengono le impostazioni specifiche per l'ambiente di test e l'ambiente di gestione temporanea, rispettivamente, quali stringhe di connessione, gli endpoint del servizio e i dettagli del servizio remoto che riceverà il pacchetto di web. Per istruzioni su come scegliere le impostazioni corrette per gli ambienti di destinazione specifico, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Per eseguire la distribuzione, un utente esegue il *Publish.proj* file utilizzando MSBuild o Team Build e specifica il percorso del file di progetto specifici dell'ambiente pertinente (*Env Dev.proj* o *Env Stage.proj*) come argomento della riga di comando. Il *Publish.proj* file quindi Importa il file di progetto specifico dell'ambiente per creare un set completo di istruzioni per ogni ambiente di destinazione di pubblicazione.

> [!NOTE]
> Il funzionamento di questi file di progetto personalizzato è indipendente del meccanismo di che consentono di richiamare MSBuild. Ad esempio, è possibile utilizzare la riga di comando di MSBuild direttamente, come descritto in [informazioni sui File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). È possibile eseguire i file di progetto da un file di comando, come descritto in [creare ed eseguire un File di comando di distribuzione](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). In alternativa, è possibile eseguire i file di progetto da una definizione di compilazione in TFS, come descritto in [la creazione di una definizione di compilazione che la distribuzione supporta](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> In ogni caso, il risultato finale è la stessa #x 2014; & MSBuild esegue il file di progetto sottoposto a merge e distribuisce la soluzione nell'ambiente di destinazione. Ciò consente una notevole flessibilità nella modalità si attiva il processo di pubblicazione.


Una volta che ha creato i file di progetto personalizzati, Matt li aggiunge a una cartella della soluzione e li archivia nel controllo del codice sorgente.

### <a name="create-build-definitions"></a>Creazione delle definizioni di compilazione

Come un'attività di preparazione finale, Matt e Rob contribuiscono a creare tre definizioni di compilazione per il nuovo progetto team:

- **DeployToTest**. Si compila la soluzione di gestione di contatto e lo distribuisce all'ambiente di test ogni volta che un controllo aggiuntivo.
- **DeployToStaging**. Questo consente di distribuire le risorse da una compilazione precedente specificata all'ambiente di gestione temporanea quando uno sviluppatore di code di compilazione.
- **DeployToStaging-WhatIf**. Ciò consente di eseguire una distribuzione "cosa accade se" l'ambiente di gestione temporanea quando uno sviluppatore di code di compilazione.

Le sezioni che seguono forniscono ulteriori dettagli su ognuna di queste definizioni di compilazione.

## <a name="deployment-to-test"></a>Distribuzione di Test

Il team di sviluppo Fabrikam, Inc. gestisce gli ambienti di test per eseguire un'ampia gamma di attività, ad esempio la verifica e convalida, test di usabilità, testare la compatibilità e test ad hoc o esplorativo di test di software.

Il team di sviluppo abbia creato una definizione di compilazione in TFS denominato **DeployToTest**. Questa definizione di compilazione utilizza un trigger di integrazione continua, ovvero che il processo di compilazione viene eseguita ogni volta che un membro del team di sviluppo di Fabrikam, Inc. esegue un controllo aggiuntivo. Quando viene attivata una compilazione, verrà visualizzata la definizione di compilazione:

- Compilare la soluzione ContactManager.sln. Ciò a sua volta compila ogni progetto nella soluzione.
- Eseguire unit test nella struttura della cartella della soluzione (se la soluzione viene compilata correttamente).
- Eseguire i file di progetto personalizzati che consentono di controllare il processo di distribuzione (se la soluzione viene compilata correttamente e passa tutti gli unit test).

Il risultato finale è che, se la soluzione viene compilata correttamente e passa unit test, i pacchetti web e altre risorse di distribuzione vengono distribuite nell'ambiente di testing.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Come funziona il processo di distribuzione?

Il **DeployToTest** questi argomenti MSBuild fornisce definizione di compilazione:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]


Il **DeployOnBuild = true** e **DeployTarget = pacchetto** vengono utilizzate quando Team Build Compila i progetti all'interno della soluzione. Quando il progetto è un progetto di applicazione web, queste proprietà indicano MSBuild per creare un pacchetto di distribuzione web per il progetto. Il **TargetEnvPropsFile** proprietà indica il *Publish.proj* file dove trovare il file di progetto specifici dell'ambiente da importare.

> [!NOTE]
> Per istruzioni dettagliate su come creare una definizione di compilazione come questo, vedere [la creazione di una definizione di compilazione che la distribuzione supporta](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Il *Publish.proj* file contiene le destinazioni che compila ogni progetto nella soluzione. Tuttavia, include inoltre la logica condizionale che ignora questi compilare destinazioni se si sta eseguendo il file in Team Build. Ciò consente di sfruttare le funzionalità di compilazione aggiuntive, ad esempio la possibilità di eseguire unit test, Team Build offerte. Se la compilazione di soluzioni o l'unità verifica ha esito negativo, il *Publish.proj* file non verrà eseguito e l'applicazione non verrà distribuito.

La logica condizionale, è possibile valutare la **BuildingInTeamBuild** proprietà. Questa è una proprietà di MSBuild che viene impostata automaticamente su **true** quando si utilizza Team Build per compilare i progetti.

## <a name="deployment-to-staging"></a>Distribuzione di gestione temporanea

Quando una compilazione soddisfi tutti i requisiti del team di sviluppo nell'ambiente di test, il team opportuno distribuire la stessa build in un ambiente di gestione temporanea. Gli ambienti di gestione temporanea vengono in genere configurati per soddisfare le caratteristiche di produzione o "attivi" ambiente strettamente come possibili, ad esempio, in termini di specifiche del server, software e sistemi operativi e configurazione di rete. Gli ambienti di gestione temporanea vengono spesso utilizzati per il test di carico, test di accettazione utente e revisioni interne più ampie. Le compilazioni vengono distribuite nell'ambiente di gestione temporanea direttamente dal server di compilazione.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Le definizioni di compilazione utilizzate per distribuire la soluzione nell'ambiente di gestione temporanea, **DeployToStaging-WhatIf** e **DeployToStaging**, condividere queste caratteristiche:

- Essi non compilare effettivamente alcun elemento. Quando Rob distribuisce la soluzione nell'ambiente di gestione temporanea, Giorgio desidera distribuire una compilazione esistente e specifica che è già verificata e convalidata nell'ambiente di test. Le definizioni di compilazione è sufficiente eseguire i file di progetto personalizzati che consentono di controllare il processo di distribuzione.
- Quando Rob attiva una compilazione, che utilizza i parametri di compilazione per specificare la compilazione contiene le risorse che Alessandro desidera distribuire dal server di compilazione.
- Le definizioni di compilazione non vengono attivate automaticamente. Rob accoda manualmente una compilazione quando si desidera distribuire la soluzione nell'ambiente di gestione temporanea.

Questo è il processo generale per una distribuzione all'ambiente di gestione temporanea:

1. L'amministratore di ambiente gestione temporanea, Rob Walters, Accoda una compilazione utilizzando il **DeployToStaging-WhatIf** definizione di compilazione. Ezio utilizza i parametri di definizione di compilazione per specificare quali compilazione Alessandro desidera distribuire.
2. Il **DeployToStaging-WhatIf** esecuzioni di definizione di compilazione i file di progetto personalizzati in modalità "cosa accade se". File di log viene generato come se Rob stava eseguendo una distribuzione in tempo reale, ma in realtà non apporta alcuna modifica all'ambiente di destinazione.
3. Rob esamina i file di registro per verificare gli effetti della distribuzione nell'ambiente di gestione temporanea. In particolare, Rob desidera controllare quali verranno aggiunti, cosa verrà aggiornata e cosa verrà eliminata.
4. Se è Rob soddisfatti che la distribuzione non effettua le operazioni di modifica per le risorse esistenti o di dati, quindi Accoda una compilazione utilizzando il **DeployToStaging** definizione di compilazione.
5. Il **DeployToStaging** esecuzioni di definizione di compilazione i file di progetto personalizzati. Questi pubblicare le risorse di distribuzione nel server web primario nell'ambiente di gestione temporanea.
6. Il controller di Web Farm Framework (WFF) consente di sincronizzare il server web nell'ambiente di gestione temporanea. Ciò rende l'applicazione disponibile in tutti i server web nella server farm.

### <a name="how-does-the-deployment-process-work"></a>Come funziona il processo di distribuzione?

Il **DeployToStaging** questi argomenti MSBuild fornisce definizione di compilazione:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]


Il **TargetEnvPropsFile** proprietà indica il *Publish.proj* file dove trovare il file di progetto specifici dell'ambiente da importare. Il **OutputRoot** sostituisce il valore predefinito di proprietà e indica il percorso della cartella di compilazione che contiene le risorse che si desidera distribuire. Quando la compilazione di code di Rob, utilizza il **parametri** scheda per fornire un valore aggiornato per il **OutputRoot** proprietà.

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Per ulteriori informazioni su come creare una definizione di compilazione come questo, vedere [distribuire una compilazione specifica](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).


Il **DeployToStaging-WhatIf** definizione di compilazione contiene la stessa logica di distribuzione come la **DeployToStaging** definizione di compilazione. Tuttavia, include l'argomento aggiuntivo **WhatIf = true**:


[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]


All'interno di *Publish.proj* file, il **WhatIf** proprietà indica che tutte le risorse di distribuzione devono essere pubblicate in modalità "cosa accade se". In altre parole, i file di log vengono generati come se la distribuzione non più avanti, ma non viene effettivamente modificato nell'ambiente di destinazione. Ciò consente di valutare l'impatto di una distribuzione proposto & #x 2014, in particolare, ciò che viene aggiunto, cosa verrà aggiornata e ciò che verrà eliminato & #x 2014; prima di eseguire effettivamente le modifiche.

> [!NOTE]
> Per ulteriori informazioni su come configurare le distribuzioni di "cosa accade se", vedere [con una distribuzione di "Cosa accade se"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).


Dopo aver distribuito l'applicazione al server web primario nell'ambiente di gestione temporanea, il WFF sincronizzerà automaticamente l'applicazione in tutti i server nella server farm.

> [!NOTE]
> Per ulteriori informazioni sulla configurazione di WFF per sincronizzare i server web, vedere [creare una Server Farm con Web Farm Framework](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).


## <a name="deployment-to-production"></a>Distribuzione nell'ambiente di produzione

Quando una compilazione è stata approvata nell'ambiente di gestione temporanea, il team di Fabrikam, Inc. può pubblicare l'applicazione nell'ambiente di produzione. L'ambiente di produzione è in cui l'applicazione passa "attivo" e raggiunge il relativo destinatari degli utenti finali.

L'ambiente di produzione è in una rete perimetrale con connessione Internet. Questo è isolato dalla rete interna che contiene il server di compilazione. L'amministratore di ambiente di produzione, Lisa Andrews, manualmente deve copiare i pacchetti di distribuzione web dal server di compilazione e importarli in IIS sul server web di produzione primario.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Questo è il processo generale per una distribuzione nell'ambiente di produzione:

1. Il team di sviluppatori informa Davide che una compilazione è pronta per la distribuzione nell'ambiente di produzione. Il team informa Davide della posizione dei pacchetti di distribuzione web all'interno della cartella di rilascio nel server di compilazione.
2. Davide raccoglie i pacchetti web dal server di compilazione e li copia nel server web primario nell'ambiente di produzione.
3. Davide Usa Gestione IIS per importare e pubblicare i pacchetti web sul server web primario.
4. Il controller WFF Sincronizza i server web nell'ambiente di produzione. Ciò rende l'applicazione disponibile in tutti i server web nella server farm.

### <a name="how-does-the-deployment-process-work"></a>Come funziona il processo di distribuzione?

Gestione IIS include una creazione guidata pacchetto di applicazione di importazione che rende più semplice pubblicare i pacchetti web in un sito Web IIS. Per una procedura dettagliata su come eseguire questa procedura, vedere [manualmente l'installazione di pacchetti di Web](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Conclusione

In questo argomento viene fornita un'illustrazione del ciclo di vita di distribuzione per un'applicazione web tipico su larga scala.

In questo argomento fa parte di una serie di esercitazioni che forniscono informazioni aggiuntive su vari aspetti della distribuzione di applicazioni web. In pratica, sono disponibili molte altre attività e le considerazioni in ogni fase del processo di distribuzione e non è possibile tutte in una procedura dettagliata di single. Per ulteriori informazioni, consultare queste esercitazioni:

- [Distribuzione nell'organizzazione Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Questa esercitazione viene fornita un'introduzione completa a tecniche di distribuzione web utilizzando MSBuild e lo strumento di distribuzione Web di IIS (distribuzione Web).
- [Configurazione degli ambienti di Server per la distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). In questa esercitazione vengono fornite indicazioni su come configurare gli ambienti di Windows server per supportare diversi scenari di distribuzione.
- [Configurazione di Team Foundation Server per automatizzata Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). In questa esercitazione vengono fornite indicazioni su come integrare i processi di compilazione TFS di logica di distribuzione.
- [Distribuzione Web aziendale avanzate](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). In questa esercitazione vengono fornite indicazioni su come soddisfare alcuni dei problemi di distribuzione più complessi in un'organizzazione.

>[!div class="step-by-step"]
[Precedente](enterprise-web-deployment-scenario-overview.md)
