---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Informazioni sul File di progetto | Microsoft Docs
author: jrjlee
description: I file di progetto di Microsoft Build Engine (MSBuild) si trovano il fulcro del processo di compilazione e distribuzione. Questo argomento inizia con una panoramica concettuale di MSBuild...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 7e117459f5953be7bac53267700dfb9f69802aec
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836843"
---
<a name="understanding-the-project-file"></a>Informazioni sul file di progetto.
====================
da [Jason Lee](https://github.com/jrjlee)

[Scaricare PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> I file di progetto di Microsoft Build Engine (MSBuild) si trovano il fulcro del processo di compilazione e distribuzione. Questo argomento inizia con una panoramica concettuale di MSBuild e il file di progetto. Vengono descritti i componenti chiave, che è possibile riscontrare quando si lavora con i file di progetto e può essere utilizzato con un esempio di come è possibile utilizzare i file di progetto per distribuire le applicazioni reali.
> 
> Che cosa si apprenderà come:
> 
> - Come MSBuild Usa i file di progetto MSBuild per compilare i progetti.
> - Modo in cui MSBuild si integra con le tecnologie di distribuzione, come lo strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web).
> - Come comprendere i componenti chiave di un file di progetto.
> - Come è possibile utilizzare i file di progetto per compilare e distribuire applicazioni complesse.


## <a name="msbuild-and-the-project-file"></a>MSBuild e il File di progetto

Quando si creano e creare soluzioni in Visual Studio, Visual Studio Usa MSBuild per compilare ogni progetto nella soluzione. Ogni progetto di Visual Studio include un file di progetto MSBuild, con un'estensione di file che riflette il tipo di progetto&#x2014;, ad esempio, un progetto c# (con estensione csproj), un progetto di Visual Basic.NET (con estensione vbproj) o un progetto di database (con estensione dbproj). Per compilare un progetto, MSBuild deve elaborare il file di progetto associato al progetto. Il file di progetto è un documento XML che contiene tutte le informazioni e istruzioni che MSBuild è necessario per compilare il progetto, ad esempio il contenuto da includere, i requisiti di piattaforma, le informazioni di controllo delle versioni, server web o impostazioni server di database e il attività che devono essere eseguite.

I file di progetto MSBuild si basano le [dello schema XML di MSBuild](https://msdn.microsoft.com/library/5dy88c2e.aspx), e di conseguenza il processo di compilazione è completamente aperto e trasparente. Inoltre, non è necessario installare Visual Studio per poter utilizzare il motore MSBuild&#x2014;dell'eseguibile MSBuild.exe fa parte di .NET Framework ed è possibile eseguirlo da un prompt dei comandi. Gli sviluppatori, è possibile creare i proprio file di progetto MSBuild, usando lo schema XML di MSBuild, per imporre controllo con granularità fine e sofisticata come compilare e distribuire i progetti. Questi file di progetto personalizzati funzionano in esattamente allo stesso modo dei file di progetto Visual Studio genera automaticamente.

> [!NOTE]
> È anche possibile usare i file di progetto MSBuild con il servizio di compilazione Team in Team Foundation Server (TFS). Ad esempio, è possibile utilizzare i file di progetto in scenari di integrazione continua (CI) per automatizzare la distribuzione in un ambiente di test quando viene archiviato nuovo codice. Per altre informazioni, vedere [configurazione di Team Foundation Server per la distribuzione di Web automatizzata](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Convenzioni di denominazione di File di progetto

Quando si creano i proprio file di progetto, è possibile usare qualsiasi estensione di file desiderato. Tuttavia, per semplificare le soluzioni ad altri utenti, è consigliabile usare tali convenzioni comuni:

- Quando si crea un file di progetto che compila i progetti, usare l'estensione proj.
- Usare l'estensione con estensione targets quando si crea un file di progetto riutilizzabili per importare in altri file di progetto. File con estensione targets in genere non vengono compilati alcuna operazione se stessi, semplicemente contengono istruzioni che è possibile importare i file con estensione proj.

### <a name="integration-with-deployment-technologies"></a>Integrazione con tecnologie di distribuzione

Se è lavorato con i progetti applicazione web in Visual Studio 2010, come le applicazioni web ASP.NET e applicazioni web ASP.NET MVC, si saprà che questi progetti includono supporto incorporato per l'assemblaggio e distribuzione dell'applicazione web in un ambiente di destinazione. Il **delle proprietà** pagine per questi progetti includono **pubblicazione/creazione pacchetto Web** e **pubblicazione/creazione pacchetto SQL** schede che è possibile usare per configurare come i componenti del pacchetto di installazione dell'applicazione. Viene illustrato il **pubblicazione/creazione pacchetto Web** scheda:

![](understanding-the-project-file/_static/image1.png)

La tecnologia su queste funzionalità è noto come Pipeline di pubblicazione sul Web (WPP). WPP offre essenzialmente MSBuild e [distribuzione Web](https://go.microsoft.com/?linkid=9805122) interagiscono per fornire un processo di compilazione, distribuzione e pacchetto completo per le applicazioni web.

La buona notizia è che è possibile sfruttare i vantaggi dei punti di integrazione che WPP fornisce quando si creano file di progetto personalizzati per i progetti web. È possibile includere istruzioni per la distribuzione nel file di progetto, che consente di compilare i progetti, creare pacchetti di distribuzione web e installare questi pacchetti su server remoti tramite un singolo file di progetto e una singola chiamata a MSBuild. È inoltre possibile chiamare qualsiasi altro file eseguibili come parte del processo di compilazione. Ad esempio, è possibile eseguire lo strumento da riga di comando VSDBCMD.exe per distribuire un database da un file di schema. Nel corso di questo argomento, si noterà come è possibile sfruttare queste funzionalità per soddisfare i requisiti degli scenari di utilizzo dell'organizzazione.

> [!NOTE]
> Per altre informazioni su come funziona il processo di distribuzione dell'applicazione web, vedere [Panoramica della distribuzione progetto applicazione Web ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>Anatomia di un File di progetto

Prima di esaminare il processo di compilazione in modo più dettagliato, vale la pena richiede alcuni minuti per acquisire familiarità con la struttura di base di un file di progetto MSBuild. In questa sezione viene fornita una panoramica degli elementi più comuni che possono verificare in caso esaminare, modificare o creare un file di progetto. In particolare, si apprenderà come:

- Come usare *proprietà* per gestire le variabili per il processo di compilazione.
- Come usare *elementi* per identificare l'input al processo di compilazione, ad esempio i file di codice.
- Come usare *destinazioni* e *attività* ricevano le istruzioni di esecuzione a MSBuild, tramite *proprietà* e *elementi* definito altrove nel file di progetto.

Ciò viene illustrata la relazione tra gli elementi chiave in un file di progetto MSBuild:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>L'elemento di progetto

Il [progetto](https://msdn.microsoft.com/library/bcxfsh87.aspx) elemento è l'elemento radice di ogni file di progetto. Oltre a identificare il XML schema per il file di progetto, il **progetto** elemento può includere attributi per specificare i punti di ingresso per il processo di compilazione. Ad esempio, nelle [soluzione di esempio Contact Manager](the-contact-manager-solution.md), il *Publish.proj* file specifica che la compilazione deve essere avviato tramite la chiamata di destinazione denominata **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Le proprietà e le condizioni

In genere un file di progetto deve fornire un numero elevato di tipi diversi di informazioni per compilare e distribuire i progetti correttamente. Questi tipi di informazioni può includere i nomi dei server, le stringhe di connessione, le credenziali, le configurazioni della build, percorsi dei file di origine e di destinazione e qualsiasi altra informazione da includere per supportare la personalizzazione. In un file di progetto, è necessario definire le proprietà all'interno di un [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) elemento. Proprietà di MSBuild sono costituiti da coppie chiave-valore. All'interno di **PropertyGroup** elemento, il nome dell'elemento definisce la chiave di proprietà e il contenuto dell'elemento definisce il valore della proprietà. Ad esempio, è possibile definire le proprietà denominate **ServerName** e **ConnectionString** per archiviare una stringa di connessione e nome del server statico.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Per recuperare un valore della proprietà, si utilizza il formato <strong>$(</strong><em>PropertyName</em><strong>)</strong><em>.</em> Ad esempio, per recuperare il valore della <strong>ServerName</strong> proprietà, digitare:


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Si noterà esempi su come e quando usare i valori delle proprietà più avanti in questo argomento.


Incorporare le informazioni come proprietà statiche in un file di progetto non è sempre l'approccio ideale per la gestione del processo di compilazione. In molti scenari, vorrai ottenere le informazioni provenienti da altre origini oppure permettere all'utente di specificare le informazioni dal prompt dei comandi. MSBuild consente di specificare qualsiasi valore della proprietà come un parametro della riga di comando. Ad esempio, l'utente è stato possibile fornire un valore per **ServerName** quando direste esegue MSBuild.exe dalla riga di comando.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> Per altre informazioni sugli argomenti e parametri utilizzabili MSBuild.exe, vedere [riferimenti alla riga di comando di MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).


È possibile usare la stessa sintassi di proprietà per ottenere i valori delle variabili di ambiente e le proprietà del progetto incorporati. Un numero elevato di proprietà usate di frequente è definito per l'utente e di utilizzarli nei file di progetto, includendo il nome di parametro in questione. Ad esempio, per recuperare la piattaforma del progetto corrente&#x2014;, ad esempio, **x86** oppure **AnyCpu**&#x2014;è possibile includere il **$ (Platform)** riferimento alla proprietà in file di progetto. Per altre informazioni, vedere [macro per comandi di compilazione e la proprietà](https://msdn.microsoft.com/library/c02as0cs.aspx), [proprietà di progetto MSBuild comuni](https://msdn.microsoft.com/library/bb629394.aspx), e [le proprietà riservate](https://msdn.microsoft.com/library/ms164309.aspx).

Le proprietà vengono spesso usate in combinazione con *condizioni*. La maggior parte degli elementi di MSBuild supportano la **condizione** attributo, che consente di specificare i criteri su cui MSBuild deve valutare l'elemento. Ad esempio, prendere in considerazione questa definizione delle proprietà:


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


Quando MSBuild elabora questa definizione delle proprietà, viene verificata per vedere se un **$(OutputRoot)** valore della proprietà è disponibile. Se è vuoto il valore della proprietà&#x2014;in altre parole, l'utente non ha fornito un valore per questa proprietà&#x2014;la condizione viene valutata **true** e il valore della proprietà è impostato su **... \Publish\Out**. Se l'utente ha fornito un valore per questa proprietà, la condizione viene valutata **false** e non viene usato il valore della proprietà statica.

Per altre informazioni sulle diverse modalità in cui è possibile specificare le condizioni, vedere [condizioni di MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Gli elementi e gruppi di elementi

Uno dei ruoli del file di progetto importanti consiste nel definire gli input per il processo di compilazione. I dati di input sono in genere, i file&#x2014;codice file, i file di configurazione, file di comando e qualsiasi altro file che è necessario elaborare o copia come parte del processo di compilazione. Nello schema di progetto MSBuild, i dati di input sono rappresentati da [elemento](https://msdn.microsoft.com/library/ms164283.aspx) elementi. In un file di progetto, è necessario definire gli elementi all'interno di un' [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) elemento. Analogamente **proprietà** gli elementi, è possibile assegnare un nome un **elemento** elemento tuttavia si desidera. Tuttavia, è necessario specificare un **inclusione** attributo per identificare il file o carattere jolly che rappresenta l'elemento.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


Specificando multiplo **elemento** elementi con lo stesso nome, in modo efficace si sta creando un elenco di risorse denominato. Un buon metodo per vedere un esempio pratico è dare uno sguardo all'interno di uno dei file di progetto creato da Visual Studio. Ad esempio, il *ContactManager.Mvc.csproj* file nella soluzione di esempio sono inclusi numerosi gruppi di elementi, ognuno con diverse denominate **elemento** elementi.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


In questo modo, il file di progetto è che indicano a MSBuild per costruire gli elenchi di file che devono essere elaborati nello stesso modo di&#x2014;il **riferimento** elenco include gli assembly che devono essere presenti per una compilazione corretta, il  **Compilare** elenco include i file di codice che devono essere compilati, e il **contenuto** elenco include le risorse che devono essere copiate senza modificate. Esamineremo come il processo di compilazione fa riferimento e utilizza questi elementi più avanti in questo argomento.

Possono includere anche elementi Item [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) gli elementi figlio. Si tratta di coppie chiave-valore definite dall'utente ed essenzialmente rappresentano le proprietà specifiche per quell'elemento. Ad esempio, molti il **compilare** gli elementi nel file di progetto includono **DependentUpon** gli elementi figlio.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> Oltre ai metadati degli elementi creati dall'utente, tutti gli elementi vengono assegnati diversi metadati comuni al momento della creazione. Per altre informazioni, vedere [Metadati noti degli elementi](https://msdn.microsoft.com/library/ms164313.aspx).


È possibile creare **ItemGroup** gli elementi all'interno del livello di radice **Project** elemento o all'interno di specifiche **destinazione** elementi. **Elemento ItemGroup** supportano anche gli elementi **condizione** attributi, che consente di personalizzare l'input al processo di compilazione in base alle condizioni, ad esempio la configurazione di progetto o la piattaforma.

### <a name="targets-and-tasks"></a>Destinazioni e attività

Nello schema di MSBuild, un [attività](https://msdn.microsoft.com/library/77f2hx1s.aspx) elemento rappresenta un'istruzione singola build (o attività). MSBuild include una vasta gamma di attività predefinite. Ad esempio:

- Il **copia** attività Copia file in una nuova posizione.
- Il **Csc** attività richiama il compilatore Visual c#.
- Il **Vbc** attività richiama il compilatore Visual Basic.
- Il **Exec** attività esegue un programma specifico.
- Il **messaggio** attività scrive un messaggio a un logger.

> [!NOTE]
> Per informazioni dettagliate delle attività che sono disponibili per impostazione predefinita, vedere [riferimenti delle attività MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Per altre informazioni sulle attività, incluse quelle sulla creazione di attività personalizzate, vedere [attività MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).


Attività devono essere contenute sempre all'interno [destinazione](https://msdn.microsoft.com/library/t50z2hka.aspx) elementi. Oggetto **destinazione** elemento è un set di uno o più attività che vengono eseguite in sequenza e un file di progetto può contenere più destinazioni. Quando si desidera eseguire un'attività o un set di attività, si richiama la destinazione che li contiene. Si supponga, ad esempio, che si dispone di un file di progetto semplice che registri un messaggio.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


È possibile richiamare la destinazione dalla riga di comando, usando il **/t** per specificare la destinazione.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


In alternativa, è possibile aggiungere un **DefaultTargets** attributo il **progetto** elemento, per specificare le destinazioni che si vuole richiamare.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


In questo caso, non devi specificare la destinazione dalla riga di comando. È possibile specificare semplicemente il file di progetto e MSBuild richiamerà il **FullPublish** destinazione per l'utente.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Attività e destinazioni può includere **condizione** attributi. Di conseguenza, è possibile omettere le singole attività o destinazioni intere se vengono soddisfatte determinate condizioni.

In generale, quando si creano utili attività e destinazioni, è necessario fare riferimento alle proprietà e gli elementi che sono stati definiti in un' posizione nel file di progetto:

- Per utilizzare un valore della proprietà, digitare <strong>$(</strong><em>PropertyName</em><strong>)</strong>, dove <em>PropertyName</em> è il nome del <strong>proprietà</strong> elemento o il nome del parametro.
- Per usare un elemento, digitare <strong>@(</strong><em>ItemName</em><strong>)</strong>, dove <em>ItemName</em> è il nome del <strong>elemento</strong> elemento.

> [!NOTE]
> Tenere presente che se si creano più elementi con lo stesso nome, si sta creando un elenco. Al contrario, se si creano più proprietà con lo stesso nome, l'ultimo valore della proprietà è fornire sovrascrive qualsiasi proprietà precedente con lo stesso nome&#x2014;una proprietà può contenere solo un singolo valore.


Ad esempio, nelle *Publish.proj* file nella soluzione di esempio, esaminiamo il **BuildProjects** destinazione.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


In questo esempio, è possibile osservare questi punti importanti:

- Se il **BuildingInTeamBuild** parametro è specificato e ha il valore **true**, nessuna attività all'interno di questa destinazione verrà eseguita.
- La destinazione contiene una singola istanza di [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) attività. Questa attività consente di compilare gli altri progetti MSBuild.
- Il **ProjectsToBuild** elemento viene passato all'attività. Questo elemento è stato possibile rappresentano un elenco dei file progetto o una soluzione, tutto definiti da **ProjectsToBuild** elementi all'interno di un gruppo di elementi item. In questo caso, il **ProjectsToBuild** elemento fa riferimento a un singolo file di soluzione.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- I valori delle proprietà passati per il **MSBuild** attività include parametri denominati **OutputRoot** e **configurazione**. Questi vengono impostati se vengono forniti i valori dei parametri o valori di proprietà statica se non sono.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

È anche possibile vedere che il **MSBuild** attività richiama una destinazione denominata **compilazione**. Questa è una delle diverse destinazioni predefinite che vengono ampiamente usate nel file di progetto di Visual Studio e sono disponibile nei file di progetto personalizzati, ad esempio **compilare**, **Pulisci**, **ricompilare**, e **pubblicare**. Verranno fornite altre informazioni sull'uso di destinazioni e attività per controllare il processo di compilazione e sui **MSBuild** di attività, in particolare, più avanti in questo argomento.

> [!NOTE]
> Per altre informazioni sulle destinazioni, vedere [destinazioni di MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Suddivisione dei file di progetto per supportare più ambienti

Si supponga che si desidera essere in grado di distribuire una soluzione in più ambienti, ad esempio un server di prova, piattaforme di gestione temporanea e gli ambienti di produzione. La configurazione può variare notevolmente tra questi ambienti&#x2014;non solo in termini di nomi di server, le stringhe di connessione e così via, ma anche potenzialmente in termini di credenziali, le impostazioni di sicurezza e molti altri fattori. Se è necessario eseguire questa operazione regolarmente, non è molto più agevole modificare più proprietà nel file di progetto ogni volta che si passa all'ambiente di destinazione. Non è una soluzione ideale per richiedere un elenco di valori di proprietà devono essere fornite al processo di compilazione infinito.

Fortunatamente è un'alternativa. MSBuild consente di dividere la configurazione della build su più file di progetto. Per visualizzarne il funzionamento, nella soluzione di esempio, si noti che sono presenti due file di progetto personalizzati:

- *Publish.proj*, che contiene gli elementi, proprietà e destinazioni che sono comuni a tutti gli ambienti.
- *Env-Dev.proj*, che contiene le proprietà specifiche per un ambiente di sviluppo.

A questo punto si noti che il *Publish.proj* file include un [importazione](https://msdn.microsoft.com/library/92x05xfs.aspx) elemento, immediatamente sotto la parentesi graffa **progetto** tag.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


Il **importare** elemento viene usato per importare il contenuto di un altro file di progetto MSBuild nel file di progetto MSBuild corrente. In questo caso, il **TargetEnvPropsFile** parametro fornisce il nome del file del file di progetto da importare. Quando si esegue MSBuild, è possibile fornire un valore per questo parametro.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


Ciò in modo efficace unisce il contenuto dei due file in un unico file di progetto. Con questo approccio, è possibile creare un file di progetto che contiene la configurazione della build universal e più file di progetto complementare contenente le proprietà specifiche dell'ambiente. Di conseguenza, è sufficiente eseguire un comando con un valore di parametro diversi ti permette di distribuire la soluzione a un altro ambiente.

![](understanding-the-project-file/_static/image3.png)

La suddivisione dei file di progetto in questo modo è una buona norma da seguire. Consente agli sviluppatori di distribuire in più ambienti mediante l'esecuzione di un singolo comando, evitando la duplicazione di proprietà di compilazione universali su più file di progetto.

> [!NOTE]
> Per indicazioni su come personalizzare i file di progetto specifici dell'ambiente per ambienti server, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Conclusione

In questo argomento viene fornita un'introduzione generale ai file di progetto MSBuild e illustrato come creare i proprio file di progetto personalizzati per controllare il processo di compilazione. È stato anche introdotto il concetto di suddividere i file di progetto in compilazione universal istruzioni e le proprietà di compilazione specifici dell'ambiente, per renderlo semplice compilare e distribuire i progetti a più destinazioni.

Argomento successivo [informazioni sul processo di compilazione](understanding-the-build-process.md), fornisce informazioni più dettagliate su come usare i file di progetto al controllo di compilazione e distribuzione attraverso la distribuzione di una soluzione con un livello di complessità realistico.

## <a name="further-reading"></a>Ulteriori informazioni

Per un'introduzione più dettagliata per i file di progetto e il sito, vedere [all'interno di Microsoft Build Engine: uso di MSBuild e Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi, William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Precedente](setting-up-the-contact-manager-solution.md)
> [Successivo](understanding-the-build-process.md)
