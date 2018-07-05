---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Informazioni sul processo di compilazione | Microsoft Docs
author: jrjlee
description: In questo argomento fornisce una procedura dettagliata di un processo di compilazione e distribuzione su larga scala. L'approccio descritto in questo argomento Usa Engin compilare Microsoft personalizzato...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 581b7e996bf5aa4c76a6bf3d55a758677c0bf897
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804316"
---
<a name="understanding-the-build-process"></a>Informazioni sul processo di compilazione
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In questo argomento fornisce una procedura dettagliata di un processo di compilazione e distribuzione su larga scala. L'approccio descritto in questo argomento Usa i file di progetto personalizzati di Microsoft Build Engine (MSBuild) per fornire un controllo accurato ogni aspetto del processo. All'interno dei file di progetto, le destinazioni MSBuild personalizzate vengono utilizzate per eseguire l'utilità di distribuzione di database VSDBCMD.exe e utilità di distribuzione, ad esempio lo strumento di distribuzione di Internet Information Services (IIS) Web (MSDeploy.exe).
> 
> > [!NOTE]
> > L'argomento precedente [informazioni sul File di progetto](understanding-the-project-file.md), descritti i componenti chiave di un file di progetto MSBuild e ha introdotto il concetto di dividere i file di progetto per supportare la distribuzione in più ambienti di destinazione. Se non si ha già familiarità con questi concetti, è consigliabile rivedere [informazioni sul File di progetto](understanding-the-project-file.md) prima di affrontare questo argomento.


In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.

Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione. In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.

## <a name="build-and-deployment-overview"></a>Cenni preliminari sulla distribuzione e compilazione

Nel [soluzione Contact Manager](the-contact-manager-solution.md), tre file di controllano il processo di compilazione e distribuzione:

- Oggetto *file di progetto universale* (*Publish.proj*). Contiene le istruzioni di compilazione e distribuzione che non cambiano tra ambienti di destinazione.
- Un' *file di progetto specifici dell'ambiente* (*Env-Dev.proj*). Contiene le impostazioni di compilazione e distribuzione specifiche per un ambiente di destinazione specifico. Ad esempio, è possibile usare la *Env-Dev.proj* file per fornire le impostazioni per un ambiente di sviluppo o test e creare un file alternativo denominato *Env-Stage.proj* per fornire le impostazioni per una gestione temporanea ambiente.
- Oggetto *file di comando* (*Publish-Dev.cmd*). Questo file contiene un MSBuild.exe comando che specifica i file di progetto di cui si desidera eseguire. È possibile creare un file di comando per ogni ambiente di destinazione, in cui ogni file contiene un comando MSBuild.exe che specifica un file di progetto specifici dell'ambiente diverso. In questo modo lo sviluppatore di distribuzione nei diversi ambienti semplicemente eseguendo il file di comando appropriato.

Nella soluzione di esempio, è possibile trovare questi tre file nella cartella della soluzione di pubblicazione.

![](understanding-the-build-process/_static/image1.png)

Prima di esaminare questi file in modo più dettagliato, diamo un'occhiata al funzionamento di processo di compilazione globale quando si usa questo approccio. A livello generale, il processo di compilazione e distribuzione di aspetto simile al seguente:

![](understanding-the-build-process/_static/image2.png)

La prima cosa che accade è che i due file di progetto&#x2014;contenente le istruzioni di compilazione e distribuzione universale e una che contiene impostazioni specifiche dell'ambiente&#x2014;vengono unite in un unico file di progetto. MSBuild quindi funziona tramite le istruzioni nel file di progetto. Viene compilata ogni progetto della soluzione, usando il file di progetto per ogni progetto. Quindi effettua una chiamata a altri strumenti, come distribuzione Web (MSDeploy.exe) e l'utilità VSDBCMD per distribuire il contenuto web e i database nell'ambiente di destinazione.

Dall'inizio alla fine, il processo di compilazione e distribuzione esegue queste attività:

1. Elimina il contenuto della directory di output, in preparazione per una compilazione aggiornata.
2. Compila ogni progetto nella soluzione:

    1. Per i progetti web&#x2014;in questo caso, un'applicazione web ASP.NET MVC e un WCF web service&#x2014;il processo di compilazione crea un pacchetto di distribuzione web per ogni progetto.
    2. Per i progetti di database, il processo di compilazione crea un manifesto della distribuzione (file con estensione deploymanifest) per ogni progetto.
3. Usa l'utilità VSDBCMD.exe per distribuire ogni progetto di database nella soluzione, con varie proprietà dai file di progetto&#x2014;stringa di connessione di destinazione e un nome di database&#x2014;insieme al file con estensione deploymanifest.
4. Usa l'utilità MSDeploy.exe distribuire ciascun progetto web nella soluzione, con varie proprietà dai file di progetto per controllare il processo di distribuzione.

È possibile usare la soluzione di esempio per questo processo in modo più dettagliato di traccia.

> [!NOTE]
> Per indicazioni su come personalizzare i file di progetto specifici dell'ambiente per ambienti server, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Richiamare il processo di distribuzione e compilazione

Per distribuire la soluzione Contact Manager in un ambiente di test per sviluppatori, lo sviluppatore esegue la *Publish-Dev.cmd* file di comando. Questa operazione chiama MSBuild.exe, specificando *Publish.proj* del file di progetto da eseguire e *Env-Dev.proj* come valore di parametro.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> Il **/fl** switch (abbreviazione di **/fileLogger**) registra l'output di compilazione in un file denominato *MSBuild* nella directory corrente. Per altre informazioni, vedere la [riferimenti alla riga di comando di MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).


A questo punto, MSBuild viene avviata l'esecuzione, carica il *Publish.proj* file e inizierà a elaborare le istruzioni in esso contenuti. La prima istruzione indica a MSBuild di importare il progetto di file che il **TargetEnvPropsFile** parametro specifica.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


Il **TargetEnvPropsFile** parametro specifica il *Env-Dev.proj* file, in modo che MSBuild unisce il contenuto del *Env-Dev.proj* del file nei  *Publish.proj* file.

Gli elementi successivi che MSBuild viene rilevato nel file di progetto unite sono gruppi di proprietà. Le proprietà vengono elaborate nell'ordine in cui vengono visualizzati nel file. MSBuild crea una coppia chiave-valore per ogni proprietà, che fornisce che vengono soddisfatte le condizioni specificate. Le proprietà definite in un secondo momento nel file sovrascriveranno tutte le proprietà con lo stesso nome dichiarato in precedenza nel file. Si consideri, ad esempio, il **OutputRoot** proprietà.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


Quando MSBuild elabora i primi **OutputRoot** elemento, fornendo un parametro denominato in modo analogo non è stato specificato, imposta il valore della **OutputRoot** proprietà **... \Publish\Out**. Quando viene rilevato il secondo **OutputRoot** elemento, se la condizione restituisce **true**, sovrascriverà il valore della **OutputRoot** con il valore della proprietà di **OutDir** parametro.

L'elemento successivo che rileva MSBuild è un gruppo singolo elemento, che contiene un elemento denominato **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild elabora questa istruzione creando un elenco di elementi denominato **ProjectsToBuild**. In questo caso, l'elenco di elementi contiene un singolo valore&#x2014;il percorso e il nome del file della soluzione.

A questo punto, gli elementi rimanenti sono destinazioni. Le destinazioni vengono elaborate in modo diverso dalla proprietà e gli elementi&#x2014;in pratica, le destinazioni non vengono elaborate a meno che vengono esplicitamente specificate dall'utente o richiamati da un altro costrutto nel file di progetto. Si tenga presente che l'apertura **Project** tag include un **DefaultTargets** attributo.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


Indica a MSBuild per richiamare il **FullPublish** destinazione, se le destinazioni non sono specificati quando viene richiamato MSBuild.exe. Il **FullPublish** destinazione non contiene tutte le attività; invece specifica semplicemente un elenco di dipendenze.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Questa dipendenza indica a MSBuild che per consentire di eseguire la **FullPublish** destinazione, è necessario richiamare questo elenco di destinazioni nell'ordine indicato:

1. È necessario richiamare il **Pulisci** destinazione.
2. È necessario richiamare il **BuildProjects** destinazione.
3. È necessario richiamare il **GatherPackagesForPublishing** destinazione.
4. È necessario richiamare il **PublishDbPackages** destinazione.
5. È necessario richiamare il **PublishWebPackages** destinazione.

### <a name="the-clean-target"></a>La destinazione pulita

Il **Pulisci** destinazione essenzialmente consente di eliminare la directory di output e il relativo contenuto, come preparazione per una compilazione aggiornata.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Si noti che la destinazione include un' **ItemGroup** elemento. Quando si definisce proprietà o gli elementi all'interno di un **destinazione** elemento, si sta creando *dinamico* proprietà e gli elementi. In altre parole, le proprietà o gli elementi non vengono elaborati fino a quando non viene eseguita la destinazione. La directory di output non esiste o non può contenere tutti i file fino all'inizio del processo di compilazione, pertanto non è possibile compilare il  **\_FilesToDelete** elencato come un elemento statico, è necessario attendere fino a quando non è in corso l'esecuzione. Di conseguenza, si compila l'elenco come un elemento dinamico all'interno della destinazione.

> [!NOTE]
> In questo caso, poiché il **Pulisci** destinazione è il primo a essere eseguito, non è necessario usare un gruppo di elementi dinamico reale. Tuttavia, è consigliabile usare le proprietà dinamiche e gli elementi in questo tipo di scenario, come si potrebbe voler eseguire le destinazioni in un ordine diverso in un determinato momento.  
> Inoltre, è sempre preferibile evitare di dichiarare gli elementi che non verranno mai utilizzati. Se sono presenti elementi che verranno usati solo da una destinazione specifica, è consigliabile inserirli all'interno della destinazione per rimuovere qualsiasi un sovraccarico non necessario nel processo di compilazione.


Dinamica degli elementi, il **Pulisci** target è piuttosto semplice e Usa l'oggetto incorporato **messaggio**, **Elimina**, e **RemoveDir**attività:

1. Inviare un messaggio al logger.
2. Compila un elenco di file da eliminare.
3. Eliminare i file.
4. Rimuovere la directory di output.

### <a name="the-buildprojects-target"></a>La destinazione BuildProjects

Il **BuildProjects** destinazione si basa fondamentalmente tutti i progetti nella soluzione di esempio.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Questa destinazione è stato descritto in modo più dettagliato nell'argomento precedente [informazioni sul File di progetto](understanding-the-project-file.md), per illustrare come attività e destinazioni fanno riferimento a proprietà e gli elementi. A questo punto, si è interessati principalmente il **MSBuild** attività. È possibile usare questa attività per compilare più progetti. L'attività non crea una nuova istanza della MSBuild.exe; Usa l'istanza attualmente in esecuzione per ogni progetto di compilazione. I punti chiave di interesse in questo esempio sono le proprietà di distribuzione:

- Il **DeployOnBuild** proprietà indicato a MSBuild per eseguire le istruzioni di distribuzione nelle impostazioni del progetto una volta completata la compilazione di ogni progetto.
- Il **DeployTarget** identificata dalla proprietà di destinazione che si vuole richiamare dopo la compilazione del progetto. In questo caso, il **pacchetto** destinazione si basa l'output del progetto in un pacchetto distribuibile dal web.

> [!NOTE]
> Il **pacchetto** destinazione richiama Web pubblicazione Pipeline (WPP), che fornisce l'integrazione tra MSBuild e distribuzione Web. Se si desidera esaminare le destinazioni predefinite WPP fornita, rivedere le *Microsoft.Web.Publishing.targets* file nella cartella %\MSBuild\Microsoft\VisualStudio\v10.0\Web % programmi (x86).


### <a name="the-gatherpackagesforpublishing-target"></a>La destinazione GatherPackagesForPublishing

Se è studiare le **GatherPackagesForPublishing** destinazione, si noterà che in realtà non contiene tutte le attività. Al contrario, contiene un gruppo singolo elemento che definisce tre elementi dinamici.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Questi elementi fanno riferimento ai pacchetti di distribuzione che sono stati creati quando la **BuildProjects** destinazione è stata eseguita. È non è stato possibile definire questi elementi in modo statico nel file di progetto, perché non sono presenti i file a cui fanno riferimento gli elementi finché il **BuildProjects** destinazione viene eseguita. Al contrario, gli elementi devono essere definiti in modo dinamico all'interno di una destinazione che non viene richiamata fino a dopo il **BuildProjects** destinazione viene eseguita.

Gli elementi non vengono utilizzati all'interno di questa destinazione&#x2014;questa destinazione Build semplicemente gli elementi e i metadati associati a ogni valore dell'elemento. Dopo che questi elementi vengono elaborati, il **PublishPackages** elemento conterrà due valori, il percorso per il *ContactManager.Mvc.deploy.cmd* file e il percorso il  *ContactManager.Service.deploy.cmd* file. La funzionalità distribuzione Web consente di creare questi file come parte del pacchetto di web per ogni progetto e questi sono i file che è necessario richiamare nel server di destinazione per poter distribuire i pacchetti. Se si apre uno di questi file, viene visualizzato in sostanza un comando MSDeploy.exe con diversi valori di parametro di compilazione specifiche.

Il **DbPublishPackages** elemento conterrà un singolo valore, il percorso per il *ContactManager.Database.deploymanifest* file.

> [!NOTE]
> Quando si compila un progetto di database e Usa lo stesso schema come un file di progetto MSBuild, viene generato un file con estensione deploymanifest. Contiene tutte le informazioni necessarie per distribuire un database, incluso il percorso dello schema del database (. dbschema) e i dettagli di qualsiasi script pre e post-distribuzione. Per altre informazioni, vedere [An Overview of Database Build e distribuzione](https://msdn.microsoft.com/library/aa833165.aspx).


Verranno fornite altre informazioni sul modo in cui i pacchetti di distribuzione e i manifesti della distribuzione del database vengono creati e utilizzati [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md) e [distribuisce i progetti di Database](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>La destinazione PublishDbPackages

Brevemente a proposito, il **PublishDbPackages** destinazione richiama l'utilità VSDBCMD per distribuire il **ContactManager** database in un ambiente di destinazione. Configurazione della distribuzione di database prevede un numero elevato di decisioni e sfumature e si otterranno altre informazioni, vedere [progetti di Database di distribuzione](deploying-database-projects.md) e [personalizzazione le distribuzioni dei Database per più ambienti](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). In questo argomento, ci concentreremo su come funziona effettivamente questa destinazione.

In primo luogo, si noti che il tag di apertura include un' **output** attributo.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Questo è un esempio di *batch di destinazione*. Nei file di progetto MSBuild, l'invio in batch è una tecnica per scorrere le raccolte. Il valore del **output** attributo **"% (DbPublishPackages.Identity)"**, fa riferimento al **identità** proprietà dei metadati il **DbPublishPackages**  elenco di elementi. Questa notazione, **Outputs=%***(ItemList.ItemMetadataName)*, viene convertito come:

- Suddividere gli elementi in **DbPublishPackages** in batch di elementi che contengono gli stessi **identità** valore dei metadati.
- Eseguire la destinazione di una sola volta per ogni batch.

> [!NOTE]
> **Identity** è uno dei [valori predefiniti dei metadati](https://msdn.microsoft.com/library/ms164313.aspx) assegnato a ogni elemento al momento della creazione. Fa riferimento al valore dei **inclusione** attributo il **elemento** elemento&#x2014;in altre parole, il percorso e il nome dell'elemento.


In questo caso, perché non sono mai più di un elemento con lo stesso percorso e nome file, essenzialmente stiamo collaborando con dimensioni di batch di uno. La destinazione viene eseguita una sola volta per ogni pacchetto di database.

È possibile visualizzare una notazione simile nel  **\_Cmd** proprietà, che crea un comando VSDBCMD con le opzioni appropriate.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


In questo caso **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, e **%(DbPublishPackages.FullPath)** fanno tutte riferimento i valori dei metadati del **DbPublishPackages** raccolta di elementi. Il  **\_Cmd** proprietà viene utilizzata per il **Exec** attività che richiama il comando.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


In seguito a questa notazione, il **Exec** attività creerà batch in base a combinazioni univoche delle **DatabaseConnectionString**, **TargetDatabase**e **FullPath** i valori dei metadati e l'attività verrà eseguita una sola volta per ogni batch. Questo è un esempio di *batch di attività*. Tuttavia, poiché l'invio in batch a livello di destinazione è già divisa la raccolta di elementi in batch di singolo elemento, il **Exec** attività verrà eseguita una sola volta per ogni iterazione di destinazione. In altre parole, questa attività richiama l'utilità VSDBCMD una volta per ogni pacchetto di database nella soluzione.

> [!NOTE]
> Per altre informazioni sulla destinazione e l'invio in batch di attività, vedere MSBuild [Batching](https://msdn.microsoft.com/library/ms171473.aspx), [metadati degli elementi in batch di destinazione](https://msdn.microsoft.com/library/ms228229.aspx), e [metadati degli elementi in batch delle attività](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>La destinazione PublishWebPackages

A questo punto è stata richiamata la **BuildProjects** destinazione, che genera un pacchetto di distribuzione web per ogni progetto nella soluzione di esempio. Che accompagna ogni pacchetto è un *deploy. cmd* file che contiene i comandi MSDeploy.exe necessari per distribuire il pacchetto nell'ambiente di destinazione, e una *SetParameters* file, che specifica il informazioni necessarie dell'ambiente di destinazione. È stato richiamato anche il **GatherPackagesForPublishing** destinazione, che genera un elemento di raccolta che contiene il *deploy. cmd* file si è interessati. In pratica, il **PublishWebPackages** destinazione consente di eseguire queste operazioni:

- Consente di modificare la *SetParameters. XML* file per ogni pacchetto a includere i dettagli corretti per l'ambiente di destinazione, usando la **XmlPoke** attività.
- Richiama il *deploy. cmd* file per ogni pacchetto, utilizzando le opzioni appropriate.

Esattamente come le **PublishDbPackages** destinazione, il **PublishWebPackages** destinazione utilizza batch di destinazione per assicurarsi che la destinazione viene eseguita una sola volta per ogni pacchetto di web.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


All'interno della destinazione, il **Exec** attività viene usata per eseguire il *deploy. cmd* file per ogni pacchetto di web.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Per altre informazioni sulla configurazione della distribuzione dei pacchetti web, vedere [compilazione e creazione di pacchetti Web Application Projects](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Conclusione

In questo argomento viene fornita una procedura dettagliata del modo in cui dividere i file di progetto consentono di controllare il processo di compilazione e distribuzione dall'inizio alla fine per la soluzione di esempio Contact Manager. Usando questo approccio consente di eseguire complesso, le distribuzioni su larga scala in un unico passaggio ripetibile, semplicemente eseguendo un file di comando specifici dell'ambiente.

## <a name="further-reading"></a>Ulteriori informazioni

Per un'introduzione più dettagliata per i file di progetto e il sito, vedere [all'interno di Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi, William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Precedente](understanding-the-project-file.md)
> [Successivo](building-and-packaging-web-application-projects.md)
