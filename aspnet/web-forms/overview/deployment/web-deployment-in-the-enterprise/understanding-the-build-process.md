---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Informazioni sul processo di compilazione | Documenti Microsoft
author: jrjlee
description: In questo argomento fornisce una procedura dettagliata di un processo di compilazione e distribuzione su larga scala. L'approccio descritto in questo argomento utilizza Engin di compilazione personalizzata di Microsoft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 3efcefc40dc135ff42f55911036f8b38b5aa13b1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="understanding-the-build-process"></a>Informazioni sul processo di compilazione
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento fornisce una procedura dettagliata di un processo di compilazione e distribuzione su larga scala. L'approccio descritto in questo argomento utilizza file di progetto personalizzati Microsoft Build Engine (MSBuild) per fornire un controllo accurato tutti gli aspetti del processo. All'interno dei file di progetto, le destinazioni di MSBuild personalizzate vengono utilizzate per eseguire l'utilità di distribuzione di database VSDBCMD.exe e utilità di distribuzione come lo strumento di distribuzione Web (MSDeploy.exe) di Internet Information Services (IIS).
> 
> > [!NOTE]
> > L'argomento precedente, [informazioni sui File di progetto](understanding-the-project-file.md), descritti i componenti principali di un file di progetto MSBuild e stato introdotto il concetto di suddivisione dei file di progetto per supportare la distribuzione in più ambienti di destinazione. Se non si ha già familiarità con questi concetti, è necessario rivedere [informazioni sui File di progetto](understanding-the-project-file.md) prima di utilizzare questo argomento.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni utilizza una soluzione di esempio & #x 2014; il [soluzione responsabile contatto](the-contact-manager-solution.md)& #x 2014; per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, Windows realistico Servizio di Communication Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro di queste esercitazioni si basa sul progetto file split approccio descritto in [informazioni sui File di progetto](understanding-the-project-file.md), in cui il processo di compilazione è controllato da due progetti #x 2014; & file contenente una compilare le istruzioni che si applicano a ogni ambiente di destinazione e quella contenente impostazioni specifiche dell'ambiente di compilazione e distribuzione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="build-and-deployment-overview"></a>Cenni preliminari sulla distribuzione e compilazione

Nel [Contact Manager soluzione](the-contact-manager-solution.md), tre file controllano il processo di compilazione e distribuzione:

- Oggetto *file di progetto universale* (*Publish.proj*). Contiene istruzioni di compilazione e distribuzione che non cambiano tra ambienti di destinazione.
- Un *file di progetto specifici dell'ambiente* (*Env Dev.proj*). Contiene le impostazioni di compilazione e distribuzione specifiche per un ambiente di destinazione specifico. Ad esempio, è possibile utilizzare il *Env Dev.proj* file per fornire le impostazioni per un ambiente di sviluppo o test e creare un file alternativo denominato *Env Stage.proj* per fornire le impostazioni per una gestione temporanea ambiente.
- Oggetto *file di comando* (*pubblica Dev.cmd*). Questo file contiene un MSBuild.exe comando che specifica quale progetto file si desidera eseguire. È possibile creare un file di comando per ogni ambiente di destinazione, in cui ogni file contiene un comando di MSBuild.exe che specifica un file di progetto specifici dell'ambiente diverso. In questo modo lo sviluppatore di distribuire in ambienti diversi semplicemente eseguendo il file di comando appropriato.

Nella soluzione di esempio, è possibile trovare questi tre file nella cartella della soluzione di pubblicazione.

![](understanding-the-build-process/_static/image1.png)

Prima di esaminare questi file in maggior dettaglio, esaminiamo un funzionamento il processo generale di compilazione quando si utilizza questo approccio. In generale, il processo di compilazione e distribuzione simile al seguente:

![](understanding-the-build-process/_static/image2.png)

La prima cosa è che i due progetti di file & #x 2014; uno contenente le compilazione universale e istruzioni di distribuzione e uno contenente impostazioni specifiche dell'ambiente & #x 2014; vengono uniti in un singolo file del progetto. MSBuild quindi funziona tramite le istruzioni nel file di progetto. Compila tutti i progetti nella soluzione, tramite il file di progetto per ogni progetto. Viene quindi chiamato per altri strumenti, ad esempio di distribuzione Web (MSDeploy.exe) e l'utilità VSDBCMD per distribuire il contenuto web e i database all'ambiente di destinazione.

Dall'inizio alla fine, il processo di compilazione e distribuzione esegue queste attività:

1. Elimina il contenuto della directory di output, in preparazione per una nuova compilazione.
2. Compila ogni progetto nella soluzione:

    1. Per progetti web & #x 2014; in questo caso, un'applicazione web MVC ASP.NET e WCF web servizio & #x 2014; il processo di compilazione crea un pacchetto di distribuzione web per ogni progetto.
    2. Per i progetti di database, il processo di compilazione crea un manifesto di distribuzione (file con estensione deploymanifest) per ogni progetto.
3. Usa l'utilità VSDBCMD.exe per distribuire ogni progetto di database nella soluzione, utilizzando le proprietà da file di progetto e #x 2014; una stringa di connessione di destinazione e un nome del database & #x 2014; insieme al file con estensione deploymanifest.
4. Usa l'utilità di MSDeploy.exe per la distribuzione di ogni progetto web nella soluzione, utilizzando le proprietà dei file di progetto per controllare il processo di distribuzione.

È possibile utilizzare la soluzione di esempio per questo processo in modo più dettagliato di traccia.

> [!NOTE]
> Per istruzioni su come personalizzare i file di progetto specifici dell'ambiente per ambienti server, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Richiamare un processo di distribuzione e alla Build

Per distribuire la soluzione di gestione di contatto in un ambiente di test per sviluppatori, lo sviluppatore esegue la *pubblica Dev.cmd* file di comando. Viene richiamato MSBuild.exe, specificare *Publish.proj* del file di progetto per l'esecuzione e *Env Dev.proj* come valore di parametro.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> Il **/fl** passare (abbreviazione di **/fileLogger**) registra l'output di compilazione in un file denominato *msbuild.log* nella directory corrente. Per ulteriori informazioni, vedere il [alla riga di comando di MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).


A questo punto, MSBuild viene avviata l'esecuzione, carica il *Publish.proj* file e inizierà a elaborare le istruzioni all'interno di esso. La prima istruzione indica a MSBuild di importare il progetto di file che il **TargetEnvPropsFile** parametro specifica.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


Il **TargetEnvPropsFile** parametro specifica il *Env Dev.proj* file MSBuild unisce il contenuto del *Env Dev.proj* del file nel  *Publish.proj* file.

Gli elementi MSBuild rilevate nel file di progetto sottoposto a merge sono gruppi di proprietà. Proprietà vengono elaborate nell'ordine in cui appaiono nel file. MSBuild crea una coppia chiave-valore per ogni proprietà, fornendo che vengono soddisfatte le condizioni specificate. Proprietà definite in un secondo momento nel file sovrascriveranno tutte le proprietà con lo stesso nome definito in precedenza nel file. Si consideri ad esempio il **OutputRoot** proprietà.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


Quando MSBuild elabora il primo **OutputRoot** elemento, fornendo un parametro denominato in modo analogo non è stato specificato, imposta il valore della **OutputRoot** proprietà **... \Publish\Out**. Quando viene rilevato il secondo **OutputRoot** elemento, se la condizione restituisce **true**, sovrascriverà il valore della **OutputRoot** proprietà con il valore della **OutDir** parametro.

MSBuild rileva l'elemento successivo è un gruppo singolo elemento, che contiene un elemento denominato **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild elabora questa istruzione per la creazione di un elenco di elementi denominato **ProjectsToBuild**. In questo caso, l'elenco di elementi contiene un singolo valore & #x 2014; il percorso e il nome del file della soluzione.

A questo punto, gli elementi rimanenti sono destinazioni. Le destinazioni vengono elaborate in modo diverso le proprietà e gli elementi di & #x 2014, in pratica, le destinazioni non vengono elaborate a meno che vengono esplicitamente specificati dall'utente o richiamati da un altro costrutto all'interno del file di progetto. Tenere presente che l'apertura **progetto** tag include un **DefaultTargets** attributo.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


Ciò indica a MSBuild di richiamare il **FullPublish** destinazione, se non sono destinazioni specificata quando viene richiamata MSBuild.exe. Il **FullPublish** destinazione non contiene alcuna attività; invece semplicemente specifica un elenco di dipendenze.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Questa dipendenza indica a MSBuild che per consentire di eseguire il **FullPublish** destinazione, è necessario richiamare questo elenco di destinazioni nell'ordine indicato:

1. È necessario richiamare il **Pulisci** destinazione.
2. È necessario richiamare il **BuildProjects** destinazione.
3. È necessario richiamare il **GatherPackagesForPublishing** destinazione.
4. È necessario richiamare il **PublishDbPackages** destinazione.
5. È necessario richiamare il **PublishWebPackages** destinazione.

### <a name="the-clean-target"></a>La destinazione Clean

Il **Pulisci** destinazione fondamentalmente consente di eliminare la directory di output e il relativo contenuto, per la preparazione di una nuova compilazione.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Si noti che la destinazione include un **ItemGroup** elemento. Quando si definiscono le proprietà o gli elementi all'interno di un **destinazione** elemento, si creano *dinamica* proprietà ed elementi. In altre parole, le proprietà o gli elementi non vengono elaborati fino a quando non viene eseguita la destinazione. La directory di output non esiste o non potrebbe contenere file fino a quando non viene avviato il processo di compilazione, pertanto non è possibile compilare il  **\_FilesToDelete** elenco come un elemento statico; è necessario attendere fino a quando non è in corso di esecuzione. Di conseguenza, compilare l'elenco come un elemento dinamico all'interno della destinazione.

> [!NOTE]
> In questo caso, poiché il **Pulisci** destinazione è il primo a essere eseguito, non è necessario utilizzare un gruppo di elementi dinamici reale. Tuttavia, è consigliabile utilizzare le proprietà dinamiche e gli elementi in questo tipo di scenario, come è possibile eseguire le destinazioni in modo diverso a un certo punto.  
> Inoltre, è sempre preferibile evitare di dichiarare gli elementi che non verranno mai utilizzati. Se sono presenti elementi che verranno utilizzati solo da una destinazione specifica, è consigliabile inserirli all'interno della destinazione per rimuovere qualsiasi overhead superfluo sul processo di compilazione.


Dinamica degli elementi, il **Pulisci** destinazione è piuttosto semplice e si avvalgono di incorporati **messaggio**, **eliminare**, e **RemoveDir**attività per:

1. Inviare un messaggio per il logger.
2. Compila un elenco di file da eliminare.
3. Eliminare i file.
4. Rimuovere la directory di output.

### <a name="the-buildprojects-target"></a>La destinazione BuildProjects

Il **BuildProjects** destinazione fondamentalmente compila tutti i progetti nella soluzione di esempio.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


La destinazione è stato descritto in maggior dettaglio nell'argomento precedente, [informazioni sui File di progetto](understanding-the-project-file.md), per illustrare come attività e destinazioni fanno riferimento a proprietà e gli elementi. A questo punto, si è interessati soprattutto il **MSBuild** attività. È possibile utilizzare questa attività per compilare più progetti. L'attività non crea una nuova istanza di MSBuild.exe; Usa l'istanza attualmente in esecuzione per ogni progetto di compilazione. I punti chiave di interesse in questo esempio sono le proprietà di distribuzione:

- Il **DeployOnBuild** proprietà indicato a MSBuild per eseguire le istruzioni di distribuzione nelle impostazioni del progetto, una volta completata la compilazione di ogni progetto.
- Il **DeployTarget** proprietà identifica la destinazione che si desidera richiamare dopo la compilazione del progetto. In questo caso, il **pacchetto** in un pacchetto distribuibile web di destinazione si basa l'output del progetto.

> [!NOTE]
> Il **pacchetto** destinazione richiama Web pubblicazione Pipeline (WPP), che offre l'integrazione tra MSBuild e distribuzione Web. Se si desidera esaminare le destinazioni predefinite che fornisce WPP, esaminare il *Microsoft.Web.Publishing.targets* file nella cartella % PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


### <a name="the-gatherpackagesforpublishing-target"></a>La destinazione GatherPackagesForPublishing

Se per analizzare il **GatherPackagesForPublishing** destinazione, si noterà che in realtà non contiene tutte le attività. Al contrario, contiene un gruppo singolo elemento che definisce tre elementi dinamici.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Questi elementi fanno riferimento ai pacchetti di distribuzione che siano stati creati quando il **BuildProjects** destinazione è stata eseguita. Non è stato possibile definire questi elementi in modo statico nel file di progetto, perché i file a cui fanno riferimento gli elementi non esistono fino a quando il **BuildProjects** destinazione viene eseguita. Al contrario, gli elementi devono essere definiti in modo dinamico all'interno di una destinazione che non viene richiamata fino a dopo il **BuildProjects** destinazione viene eseguita.

Gli elementi non vengono utilizzati all'interno di questa destinazione & #x 2014; la destinazione Build semplicemente gli elementi e i metadati associati a ogni valore dell'elemento. Dopo l'elaborazione di questi elementi sono il **PublishPackages** elemento conterrà due valori, il percorso il *ContactManager.Mvc.deploy.cmd* file e il percorso di  *ContactManager.Service.deploy.cmd* file. Distribuzione Web questi file vengono creati come parte del pacchetto per ogni progetto web e sono i file che è necessario richiamare nel server di destinazione per la distribuzione dei pacchetti. Se si apre uno di questi file, si noterà fondamentalmente un comando di MSDeploy.exe con diversi valori di parametro di compilazione specifiche.

Il **DbPublishPackages** elemento conterrà un singolo valore, il percorso di *ContactManager.Database.deploymanifest* file.

> [!NOTE]
> Quando si compila un progetto di database e viene utilizzato lo stesso schema come un file di progetto MSBuild, viene generato un file con estensione deploymanifest. Contiene tutte le informazioni necessarie per distribuire un database, compreso il percorso dello schema del database (dbschema) e i dettagli di qualsiasi script di pre-distribuzione e post-distribuzione. Per ulteriori informazioni, vedere [An Overview of Database Build e la distribuzione](https://msdn.microsoft.com/library/aa833165.aspx).


Si apprenderà ulteriori informazioni su come pacchetti di distribuzione e i manifesti di distribuzione di database vengono creati e utilizzati [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md) e [progetti di Database di distribuzione](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>La destinazione PublishDbPackages

Brevemente generale, il **PublishDbPackages** destinazione richiama l'utilità VSDBCMD per distribuire il **ContactManager** database in un ambiente di destinazione. La configurazione di distribuzione del database prevede un numero elevato di decisioni e sfumature e verranno fornite ulteriori informazioni [progetti di Database di distribuzione](deploying-database-projects.md) e [personalizzazione delle distribuzioni di Database per più ambienti](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). In questo argomento, ci concentreremo su come funziona effettivamente questa destinazione.

In primo luogo, si noti che il tag di apertura include un **output** attributo.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Questo è un esempio di *destinazioni*. Nei file di progetto MSBuild, il batch è una tecnica per l'iterazione delle raccolte. Il valore della **output** attributo, **"% (DbPublishPackages.Identity)"**, fa riferimento al **identità** proprietà dei metadati di **DbPublishPackages**  elenco di elementi. Questa notazione, **Outputs=%***(ItemList.ItemMetadataName)*, viene convertito come:

- Suddividere gli elementi in **DbPublishPackages** in batch di elementi che contengono gli stessi **identità** valore dei metadati.
- Eseguire la destinazione di una volta per ogni batch.

> [!NOTE]
> **Identità** è uno del [i valori predefiniti dei metadati](https://msdn.microsoft.com/library/ms164313.aspx) assegnato a ogni elemento al momento della creazione. Fa riferimento al valore del **Include** attributo la **elemento** elemento & #x 2014; in altre parole, il percorso e il nome dell'elemento.


In questo caso, perché non sono mai più di un elemento con lo stesso percorso e nome file, essenzialmente stiamo lavorando con dimensioni di batch di uno. La destinazione viene eseguita una volta per ogni pacchetto di database.

È possibile visualizzare una notazione simile nel  **\_Cmd** proprietà, che genera un comando VSDBCMD con le opzioni appropriate.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


In questo caso, **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, e **%(DbPublishPackages.FullPath)** fanno tutte riferimento i valori dei metadati del **DbPublishPackages** raccolta di elementi. Il  **\_Cmd** proprietà viene utilizzata per la **Exec** attività che richiama il comando.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


In seguito a questa notazione, il **Exec** attività verrà creata in base alle combinazioni univoche di batch di **DatabaseConnectionString**, **TargetDatabase**e **FullPath** i valori dei metadati e l'attività verrà eseguita una volta per ogni batch. Questo è un esempio di *batch di attività*. Tuttavia, poiché il batch a livello di destinazione è già diviso la raccolta di elementi in batch di singoli elementi, il **Exec** attività verrà eseguita una sola volta per ogni iterazione di destinazione. In altre parole, questa attività richiama l'utilità VSDBCMD una volta per ogni pacchetto di database nella soluzione.

> [!NOTE]
> Per ulteriori informazioni sulla destinazione e l'invio in batch di attività, vedere MSBuild [batch](https://msdn.microsoft.com/library/ms171473.aspx), [metadati dell'elemento nel batch di destinazione](https://msdn.microsoft.com/library/ms228229.aspx), e [metadati dell'elemento nel batch di attività](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>La destinazione PublishWebPackages

A questo punto è stato richiamato il **BuildProjects** destinazione, che genera un pacchetto di distribuzione web per ogni progetto nella soluzione di esempio. Associato a ogni pacchetto è un *deploy* file che contiene i comandi di MSDeploy.exe necessari per distribuire il pacchetto all'ambiente di destinazione, e un *SetParameters* file, che specifica il informazioni necessarie dell'ambiente di destinazione. È stato richiamato anche il **GatherPackagesForPublishing** destinazione, che genera un elemento di raccolta che contiene il *deploy* file si è interessati. In pratica, il **PublishWebPackages** destinazione esegue queste funzioni:

- Modifica il *SetParameters* file per ogni pacchetto per includere i dati corretti per l'ambiente di destinazione, utilizzando il **XmlPoke** attività.
- Richiama il *deploy* file per ogni pacchetto, utilizzando le opzioni appropriate.

Analogamente il **PublishDbPackages** destinazione, il **PublishWebPackages** destinazione utilizza l'invio in batch di destinazione per assicurarsi che la destinazione viene eseguita una volta per ogni pacchetto di web.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


All'interno della destinazione, il **Exec** attività viene utilizzata per eseguire il *deploy* file per ogni pacchetto di web.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Per ulteriori informazioni sulla configurazione della distribuzione di pacchetti web, vedere [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Conclusione

In questo argomento viene fornita una procedura dettagliata della modalità di utilizzo file di progetto suddivisi per controllare il processo di compilazione e distribuzione dall'inizio alla fine per la soluzione di esempio Contact Manager. Questo approccio consente di eseguire complessi, le distribuzioni su larga scala in un singolo passaggio ripetibile, semplicemente eseguendo un file di comando specifici dell'ambiente.

## <a name="further-reading"></a>Ulteriori informazioni

Per un'introduzione più dettagliata a WPP e file di progetto, vedere [all'interno di Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi e William Bartholomew, ISBN: 978-0-7356-4524-0.

>[!div class="step-by-step"]
[Precedente](understanding-the-project-file.md)
[Successivo](building-and-packaging-web-application-projects.md)
