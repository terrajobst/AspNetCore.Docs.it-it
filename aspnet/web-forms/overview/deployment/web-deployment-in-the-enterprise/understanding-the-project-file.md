---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Informazioni sui File di progetto | Documenti Microsoft
author: jrjlee
description: File di progetto di Microsoft Build Engine (MSBuild) si trovano gli elementi centrali del processo di compilazione e distribuzione. In questo argomento inizia con una panoramica sui concettuale di MSBuild...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 09c3793e9cdddb7c42cf966f2d079245f441540c
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
<a name="understanding-the-project-file"></a>Informazioni sui File di progetto
====================
da [Jason Lee](https://github.com/jrjlee)

[Scarica il PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> File di progetto di Microsoft Build Engine (MSBuild) si trovano gli elementi centrali del processo di compilazione e distribuzione. In questo argomento inizia con una panoramica concettuale di MSBuild e il file di progetto. Vengono descritti i componenti chiave che è essenziale in quando si lavora con i file di progetto e può essere utilizzato con un esempio di come è possibile utilizzare i file di progetto per distribuire le applicazioni reali.
> 
> Illustra quanto segue:
> 
> - MSBuild Usa il file di progetto MSBuild per compilare i progetti.
> - Integrazione MSBuild le tecnologie di distribuzione, ad esempio lo strumento di distribuzione Web di Internet Information Services (IIS) (distribuzione Web).
> - Come comprendere i componenti principali di un file di progetto.
> - Come è possibile utilizzare i file di progetto per compilare e distribuire applicazioni complesse.


## <a name="msbuild-and-the-project-file"></a>Il File di progetto e di MSBuild

Quando si creano e compilare soluzioni in Visual Studio, Visual Studio utilizza MSBuild per compilare ogni progetto nella soluzione. Ogni progetto di Visual Studio include un file di progetto MSBuild, con un'estensione di file che rifletta il tipo di progetto & #x 2014; ad esempio, un progetto c# (con estensione csproj), un progetto di Visual Basic.NET (vbproj) o un progetto di database (con estensione dbproj). Per compilare un progetto, MSBuild deve elaborare il file di progetto associato al progetto. Il file di progetto è un documento XML che contiene tutte le informazioni e istruzioni che necessita di MSBuild per compilare il progetto, ad esempio il contenuto da includere, i requisiti della piattaforma, informazioni sulla versione, server web o impostazioni server di database e attività che devono essere eseguite.

File di progetto MSBuild si basano le [dello schema XML di MSBuild](https://msdn.microsoft.com/library/5dy88c2e.aspx), e di conseguenza il processo di compilazione sia completamente trasparente. Inoltre, non è necessario installare Visual Studio per poter utilizzare il motore MSBuild & #x 2014; il file eseguibile MSBuild.exe fa parte di .NET Framework ed è possibile eseguirlo da un prompt dei comandi. Gli sviluppatori, è possibile elaborare i propri file di progetto MSBuild, tramite lo schema XML di MSBuild, per imporre sofisticato e accurato controllo sulla modalità di compilazione e distribuzione dei progetti. Questi file di progetto personalizzato funzionano nello stesso modo dei file di progetto Visual Studio genera automaticamente.

> [!NOTE]
> È inoltre possibile utilizzare file di progetto MSBuild con il servizio di compilazione Team in Team Foundation Server (TFS). Ad esempio, è possibile utilizzare file di progetto in scenari di integrazione continua (CI) per automatizzare la distribuzione in un ambiente di test quando viene archiviato nuovo codice. Per ulteriori informazioni, vedere [la configurazione di Team Foundation Server per la distribuzione di Web automatizzata](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Convenzioni di denominazione di File di progetto

Quando si creano i file di progetto, è possibile utilizzare qualsiasi estensione di file desiderato. Per semplificare le soluzioni ad altri utenti, è tuttavia consigliabile utilizzare queste convenzioni comuni:

- Quando si crea un file di progetto che compila i progetti, usare l'estensione proj.
- Utilizzare l'estensione targets quando si crea un file di progetto riutilizzabili per importare altri file di progetto. File con estensione targets in genere non vengono compilati nulla autonomamente, semplicemente contengono istruzioni che è possibile importare i file proj.

### <a name="integration-with-deployment-technologies"></a>Integrazione con tecnologie di distribuzione

Se è lavorato con progetti di applicazione web in Visual Studio 2010, come le applicazioni web ASP.NET e applicazioni web ASP.NET MVC, si saprà che tali progetti includono il supporto integrato per l'assemblaggio e distribuzione dell'applicazione web in un ambiente di destinazione. Il **proprietà** pagine per questi progetti includono **pubblicazione/creazione pacchetto Web** e **pubblicazione/creazione pacchetto SQL** schede che è possibile utilizzare per configurare come i componenti del pacchetto di installazione dell'applicazione. Viene illustrato il **pubblicazione/creazione pacchetto Web** scheda:

![](understanding-the-project-file/_static/image1.png)

La tecnologia sottostante alla base di queste funzionalità è nota come Pipeline di pubblicazione sul Web (WPP). WPP consente essenzialmente di MSBuild e [distribuzione Web](https://go.microsoft.com/?linkid=9805122) interagiscono per fornire un processo di compilazione, distribuzione e pacchetto completo per le applicazioni web.

Buone notizie sono che è possibile sfruttare i punti di integrazione che WPP fornisce quando si creano file di progetto personalizzati per i progetti web. È possibile includere istruzioni di distribuzione nel file di progetto, che consente di compilare i progetti, pacchetti di distribuzione web di creare e installare questi pacchetti su server remoti tramite un singolo file del progetto e una singola chiamata a MSBuild. È inoltre possibile chiamare qualsiasi altro file eseguibili come parte del processo di compilazione. Ad esempio, è possibile eseguire lo strumento da riga di comando VSDBCMD.exe per distribuire un database da un file di schema. Nel corso di questo argomento, verrà visualizzato come è possibile sfruttare queste funzionalità per soddisfare i requisiti degli scenari di distribuzione dell'organizzazione.

> [!NOTE]
> Per ulteriori informazioni su come funziona il processo di distribuzione dell'applicazione web, vedere [Panoramica della distribuzione progetto applicazione Web ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>L'anatomia di un File di progetto

Prima di analizzare più dettagliatamente il processo di compilazione, è opportuno acquisire familiarità con la struttura di base di un file di progetto MSBuild qualche secondo. In questa sezione viene fornita una panoramica degli elementi più comuni che si verificano quando esaminare, modificare o creare un file di progetto. In particolare, si apprenderà:

- Come usare *proprietà* per gestire le variabili per il processo di compilazione.
- Come usare *elementi* per identificare gli input per il processo di compilazione, ad esempio file di codice.
- Come usare *destinazioni* e *attività* per fornire istruzioni di esecuzione per MSBuild, utilizzando *proprietà* e *elementi* definita altrove nel file di progetto.

Mostra la relazione tra gli elementi chiave in un file di progetto MSBuild:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>L'elemento di progetto

Il [progetto](https://msdn.microsoft.com/library/bcxfsh87.aspx) elemento è l'elemento radice di ogni file di progetto. Oltre a identificare lo schema XML per il file di progetto, il **progetto** elemento può includere attributi per specificare i punti di ingresso per il processo di compilazione. Ad esempio, nel [soluzione di esempio Contact Manager](the-contact-manager-solution.md), *Publish.proj* file specifica che la compilazione deve essere avviato tramite la chiamata di destinazione denominata **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Proprietà e condizioni

Un file di progetto in genere è necessario fornire un numero elevato di tipi diversi di informazioni per compilare e distribuire i progetti correttamente. Questi tipi di informazioni può includere i nomi dei server, le stringhe di connessione, credenziali, le configurazioni della build, percorsi di file di origine e destinazione e qualsiasi altra informazione che si desidera includere per supportare la personalizzazione. In un file di progetto, le proprietà devono essere definite all'interno di un [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) elemento. Proprietà di MSBuild è costituito da coppie chiave-valore. All'interno di **PropertyGroup** elemento, il nome dell'elemento definisce la chiave della proprietà e il contenuto dell'elemento definisce il valore della proprietà. Ad esempio, è possibile definire le proprietà denominate **ServerName** e **ConnectionString** per archiviare una stringa di connessione e nome del server statico.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Per recuperare un valore della proprietà, utilizzare il formato **$(***PropertyName***) * * *.* Ad esempio, per recuperare il valore della **ServerName** proprietà, digitare:


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Esempi di come e quando utilizzare i valori delle proprietà più avanti in questo argomento verrà visualizzato.


Incorporamento di informazioni come proprietà statiche in un file di progetto non è sempre l'approccio ideale per la gestione del processo di compilazione. In molti scenari, sarà possibile ottenere le informazioni da altre origini o consentire all'utente di specificare le informazioni dal prompt dei comandi. MSBuild consente di specificare qualsiasi valore della proprietà come parametro della riga di comando. Ad esempio, l'utente potrebbe fornire un valore per **ServerName** egli esecuzione MSBuild.exe dalla riga di comando.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> Per ulteriori informazioni sugli argomenti e parametri utilizzabili con MSBuild.exe, vedere [alla riga di comando di MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).


È possibile utilizzare la stessa sintassi di proprietà per ottenere i valori delle variabili di ambiente e le proprietà del progetto predefinito. Un numero elevato di proprietà comunemente utilizzate è definito per l'utente e di utilizzarli nei file di progetto, includendo il nome di parametro in questione. Ad esempio, per recuperare la piattaforma del progetto corrente & #x 2014; ad esempio, **x86** o **AnyCpu**& #x 2014; è possibile includere il **$(Platform)** di riferimento sulle proprietà in file di progetto. Per ulteriori informazioni, vedere [macro per comandi di compilazione e proprietà](https://msdn.microsoft.com/library/c02as0cs.aspx), [proprietà di progetto MSBuild comuni](https://msdn.microsoft.com/library/bb629394.aspx), e [proprietà riservate](https://msdn.microsoft.com/library/ms164309.aspx).

Le proprietà vengono spesso utilizzate in combinazione con *condizioni*. La maggior parte degli elementi di MSBuild supportano il **condizione** attributo, che consente di specificare i criteri su cui MSBuild deve valutare l'elemento. Si consideri, ad esempio, questa definizione di proprietà:


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


Quando MSBuild elabora questa definizione di proprietà, viene verificata per vedere se un **$(OutputRoot)** valore della proprietà è disponibile. Se il valore della proprietà è vuoto & #x 2014; in altre parole, l'utente non è stato specificato un valore per questa proprietà & #x 2014; la condizione restituisce **true** e il valore della proprietà è impostato su **... \Publish\Out**. Se l'utente ha fornito un valore per questa proprietà, la condizione restituisce **false** e non viene utilizzato il valore della proprietà statica.

Per ulteriori informazioni sulle diverse modalità in cui è possibile specificare le condizioni, vedere [condizioni di MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Gli elementi e i gruppi di elementi

Uno dei ruoli del file di progetto importante consiste nel definire l'input per il processo di compilazione. In genere, i dati di input sono file & #x 2014, i file di codice, i file di configurazione, file di comando e altri file che è necessario elaborare o copia come parte del processo di compilazione. Nello schema di progetto MSBuild, i dati di input sono rappresentate da [elemento](https://msdn.microsoft.com/library/ms164283.aspx) elementi. In un file di progetto, gli elementi devono essere definiti all'interno di un [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) elemento. Analogamente **proprietà** elementi, è possibile assegnare un **elemento** elemento tuttavia si desidera. Tuttavia, è necessario specificare un **Include** attributo per identificare il file o un carattere jolly che rappresenta l'elemento.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


Specificando più **elemento** gli elementi con lo stesso nome, si sta creando un elenco di risorse denominato. Un buon metodo per vedere un esempio pratico è dare uno sguardo all'interno di uno dei file di progetto creato da Visual Studio. Ad esempio, il *ContactManager.Mvc.csproj* file nella soluzione di esempio sono inclusi numerosi gruppi di elementi, ciascuno con diverse denominate **elemento** elementi.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


In questo modo, il file di progetto è che indica a MSBuild per costruire gli elenchi di file che devono essere elaborati in modo & #x 2014; stesso il **riferimento** elenco include gli assembly che devono essere presenti per una compilazione corretta, il **Compilare** elenco include file di codice che devono essere compilati e **contenuto** elenco include le risorse che devono essere copiate senza modificate. Verrà esaminato come il processo di compilazione fa riferimento e utilizza questi elementi più avanti in questo argomento.

Possono includere anche elementi [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) gli elementi figlio. Si tratta le coppie chiave-valore definite dall'utente che rappresentano le proprietà specifiche di tale elemento. Ad esempio, molti di **compilare** elementi nel file di progetto includono **DependentUpon** gli elementi figlio.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> Oltre ai metadati di elemento creato dall'utente, tutti gli elementi vengono assegnati diversi metadati comuni durante la creazione. Per altre informazioni, vedere [Metadati noti degli elementi](https://msdn.microsoft.com/library/ms164313.aspx).


È possibile creare **ItemGroup** elementi all'interno del livello di radice **progetto** elemento o all'interno di specifiche **destinazione** elementi. **ItemGroup** gli elementi supportano anche **condizione** gli attributi, che consente di personalizzare l'input per il processo di compilazione in base alle condizioni come la configurazione di progetto o una piattaforma.

### <a name="targets-and-tasks"></a>Destinazioni e attività

Nello schema di MSBuild, un [attività](https://msdn.microsoft.com/library/77f2hx1s.aspx) elemento rappresenta un'istruzione di compilazione singoli (o attività). MSBuild include una vasta gamma di attività predefinite. Ad esempio:

- Il **copia** verranno copiati i file in una nuova posizione.
- Il **Csc** attività richiama il compilatore Visual c#.
- Il **Vbc** attività richiama il compilatore Visual Basic.
- Il **Exec** attività viene eseguito un programma specifico.
- Il **messaggio** attività scrive un messaggio a un logger.

> [!NOTE]
> Per informazioni dettagliate delle attività che sono disponibili all'esterno della casella, vedere [riferimenti delle attività MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Per ulteriori informazioni sulle attività, incluso come creare attività personalizzate, vedere [attività MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).


Attività devono essere sempre contenute in [destinazione](https://msdn.microsoft.com/library/t50z2hka.aspx) elementi. Oggetto **destinazione** elemento è un set di uno o più attività che vengono eseguite in sequenza e un file di progetto può contenere più destinazioni. Quando si desidera eseguire un'attività o un set di attività, si richiama la destinazione che li contiene. Ad esempio, si supponga di avere un file di progetto semplice, che registra un messaggio.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


È possibile richiamare la destinazione dalla riga di comando, utilizzando il **/t** per specificare la destinazione.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


In alternativa, è possibile aggiungere un **DefaultTargets** attributo la **progetto** elemento, per specificare le destinazioni che si desidera richiamare.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


In questo caso, è necessario specificare la destinazione dalla riga di comando. È possibile specificare semplicemente il file di progetto e MSBuild richiamerà il **FullPublish** destinazione per l'utente.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Sia le destinazioni e attività possono includere **condizione** attributi. Di conseguenza, è possibile omettere le singole attività o destinazioni intere se vengono soddisfatte determinate condizioni.

In genere, quando si creano attività utili e destinazioni, è necessario fare riferimento alle proprietà e gli elementi che sono stati definiti altrove nel file di progetto:

- Per utilizzare un valore della proprietà, digitare **$(***PropertyName***)**, dove *PropertyName* è il nome del **proprietà** elemento o il nome del parametro.
- Per usare un elemento, digitare **@(***ItemName***)**, dove *ItemName* è il nome del **elemento** elemento.

> [!NOTE]
> Tenere presente che se si creano più elementi con lo stesso nome, che si sta creando un elenco. Al contrario, se si creano più proprietà con lo stesso nome, l'ultimo valore di proprietà che è fornire sovrascrive qualsiasi proprietà precedente con il nome stesso & #x 2014; una proprietà può contenere solo un singolo valore.


Ad esempio, nel *Publish.proj* file nella soluzione di esempio, esaminiamo il **BuildProjects** destinazione.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


In questo esempio, è possibile osservare i seguenti punti chiave:

- Se il **BuildingInTeamBuild** parametro specificato e ha un valore di **true**, verrà eseguita nessuna delle attività all'interno di questa destinazione.
- La destinazione contiene una singola istanza di [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) attività. Questa attività consente di compilare gli altri progetti MSBuild.
- Il **ProjectsToBuild** elemento viene passato all'attività. Questo elemento può rappresentare un elenco di file progetto o soluzione, tutti definiti da **ProjectsToBuild** elemento gli elementi all'interno di un gruppo di elementi. In questo caso, il **ProjectsToBuild** elemento fa riferimento a un singolo file di soluzione.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- I valori delle proprietà passati il **MSBuild** attività include parametri denominati **OutputRoot** e **configurazione**. Questi vengono impostati se vengono forniti i valori dei parametri o valori di proprietà statica se non lo siano.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

È anche possibile vedere che il **MSBuild** attività richiama una destinazione denominata **compilare**. Questa è una delle diverse destinazioni predefinite che vengono ampiamente utilizzate nel file di progetto di Visual Studio e sono disponibile nei file di progetto personalizzati, ad esempio **compilare**, **Pulisci**, **ricompilare**, e **pubblicare**. Verranno fornite ulteriori informazioni sull'utilizzo di destinazioni e attività per controllare il processo di compilazione e sul **MSBuild** attività, in particolare, più avanti in questo argomento.

> [!NOTE]
> Per ulteriori informazioni sulle destinazioni, vedere [destinazioni di MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Suddivisione dei file di progetto per supportare più ambienti

Si supponga che si desidera essere in grado di distribuire una soluzione in più ambienti, ad esempio un server di prova, piattaforme di gestione temporanea e ambienti di produzione. La configurazione può variare notevolmente tra questi ambienti & #x 2014; non solo in termini di nomi di server, le stringhe di connessione e così via, ma anche potenzialmente in termini di credenziali, le impostazioni di protezione e molti altri fattori. Se è necessario eseguire questa operazione regolarmente, non è realmente vantaggioso modificare più proprietà nel file di progetto ogni volta che si passa all'ambiente di destinazione. Non è una soluzione ideale per richiedere un elenco dei valori delle proprietà da fornire al processo di compilazione continuo.

Fortunatamente, è un'alternativa. MSBuild consente di suddividere la configurazione di compilazione in più file di progetto. Per visualizzarne il funzionamento, nella soluzione di esempio, si noti che sono disponibili due file di progetto personalizzati:

- *Publish.proj*, che contiene le proprietà, elementi e le destinazioni che sono comuni a tutti gli ambienti.
- *Env Dev.proj*, che contiene le proprietà specifiche per un ambiente di sviluppo.

A questo punto si noti che il *Publish.proj* file include un [importazione](https://msdn.microsoft.com/library/92x05xfs.aspx) elemento, immediatamente sotto la parentesi graffa **progetto** tag.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


Il **importare** elemento viene usato per importare il contenuto di un altro file di progetto MSBuild in file di progetto MSBuild corrente. In questo caso, il **TargetEnvPropsFile** parametro fornisce il nome del file del file di progetto che si desidera importare. È possibile fornire un valore per questo parametro quando si esegue MSBuild.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


Il contenuto di due file in modo efficace unisce un singolo file del progetto. Questo approccio, è possibile creare un file di progetto che contiene la configurazione di compilazione universale e più file di progetto supplementari contenente le proprietà specifiche dell'ambiente. Di conseguenza, è sufficiente eseguire un comando con un valore di parametro diversi consente di distribuire la soluzione in un ambiente diverso.

![](understanding-the-project-file/_static/image3.png)

La suddivisione dei file di progetto in questo modo è consigliabile seguire. Consente agli sviluppatori di distribuire in più ambienti eseguendo un comando singolo, evitando la duplicazione delle proprietà di compilazione universale per più file di progetto.

> [!NOTE]
> Per istruzioni su come personalizzare i file di progetto specifici dell'ambiente per ambienti server, vedere [configurazione delle proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Conclusione

In questo argomento viene fornita un'introduzione generale a file di progetto MSBuild e illustrato come creare file di progetto personalizzati per controllare il processo di compilazione. Anche introdotto il concetto di suddividere i file di progetto in istruzioni di compilazione universale e le proprietà di compilazione specifici dell'ambiente, per renderlo più semplice compilare e distribuire progetti in più destinazioni.

L'argomento successivo, [comprendere il processo di compilazione](understanding-the-build-process.md), fornisce informazioni più dettagliate sulla modalità è possibile utilizzare i file di progetto al controllo di compilazione e distribuzione attraverso la distribuzione di una soluzione con un livello di complessità realistico.

## <a name="further-reading"></a>Ulteriori informazioni

Per un'introduzione più dettagliata a WPP e file di progetto, vedere [all'interno di Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi e William Bartholomew, ISBN: 978-0-7356-4524-0.

>[!div class="step-by-step"]
[Precedente](setting-up-the-contact-manager-solution.md)
[Successivo](understanding-the-build-process.md)
