---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Opzioni di archiviazione di dati (creazione di App Cloud funzionanti con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione Real World di App Cloud con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure consigliate che egli può...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 16388bf00c291ee21eec7bc72a9c01c33cec9371
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820334"
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Opzioni di archiviazione di dati (creazione di App Cloud funzionanti con Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download risolverlo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **creazione Real World di App Cloud con Azure** eBook si basa su una presentazione sviluppata da Scott Guthrie. Viene illustrato 13 modelli e procedure consigliate che consentono di avere esito positivo lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [capitolo prima](introduction.md).


La maggior parte delle persone vengono utilizzati per i database relazionali e tendono a sottovalutare altre opzioni di archiviazione dei dati quando si progetta un'applicazione cloud. Il risultato può essere prestazioni non ottimali, le spese elevate o peggio, poiché [NoSQL](http://en.wikipedia.org/wiki/NoSQL) database (non relazionali) possono gestire alcune attività in modo più efficiente rispetto ai database relazionali. Quando i clienti ci chiedono per poter risolvere un problema di archiviazione di dati critici, è spesso perché hanno un database relazionale in cui una delle opzioni NoSQL avrebbe avuto meglio. In questi casi il cliente sarebbe stato meglio se si avesse implementato la soluzione NoSQL prima di distribuire l'app nell'ambiente di produzione.

D'altra parte, sarebbe anche erroneamente presumere che un database NoSQL può effettuare tutte le operazioni anche o abbastanza bene. Non vi è alcun singolo gestione dei dati ottimale per tutte le attività di archiviazione dei dati. soluzioni di gestione di dati diversi sono ottimizzate per le diverse attività. La maggior parte delle App cloud reali un'ampia gamma di requisiti di archiviazione dei dati e spesso vengono servite migliore da una combinazione di più soluzioni di archiviazione di dati.

Lo scopo di questo capitolo è per offrirti un senso più ampio delle opzioni di archiviazione di dati disponibili per un'app cloud e alcune indicazioni di base su come scegliere quelle più adatte allo scenario. È consigliabile tenere presenti le opzioni disponibili e considerare i punti di forza e punti deboli prima di sviluppare un'applicazione. Modificando le opzioni di archiviazione di dati in un'app di produzione può essere estremamente difficile, ad esempio la necessità di modificare un motore jet mentre il piano è in corso.

## <a name="data-storage-options-on-azure"></a>Opzioni di archiviazione di dati in Azure

Il cloud rende relativamente semplice usare un'ampia gamma di relazionale e gli archivi dati NoSQL. Ecco alcune delle piattaforme di archiviazione dei dati che è possibile usare in Azure.

![](data-storage-options/_static/image1.png)

La tabella mostra quattro tipi di database NoSQL:

- [I database di chiave/valore](https://msdn.microsoft.com/library/dn313285.aspx#sec7) archiviare un singolo oggetto serializzato per ogni valore di chiave. Sono utili per l'archiviazione di grandi volumi di dati in cui si vuole ottenere un solo elemento per un determinato valore chiave e non è necessario per eseguire query in base alle altre proprietà dell'elemento.

    [Archiviazione Blob di Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) è un database di chiave/valore che le funzioni, ad esempio archiviazione di file nel cloud, con valori di chiave che corrispondono ai nomi dei file e cartelle. Si recupera un file per la cartella e il nome di file, non eseguendo una ricerca per i valori nel contenuto del file.

    [Archiviazione tabelle di Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) è anche un database di chiave/valore. Ogni valore viene chiamato un *entity* (simile a una riga, identificata da una chiave di partizione e chiave di riga) e contiene più *proprietà* (simile alle colonne, ma non tutte le entità in una tabella devono condividere lo stesso colonne). L'esecuzione di query su colonne diverse dalla chiave è estremamente inefficiente e deve essere evitata. Ad esempio, è possibile archiviare i dati del profilo utente, con un'unica partizione, l'archiviazione delle informazioni relative a un singolo utente. È possibile archiviare i dati, ad esempio nome utente, hash della password, la data di nascita e così via, in proprietà separate di un'entità o nell'entità separate nella stessa partizione. Ma è preferibile non eseguire una query per tutti gli utenti con un determinato intervallo di date di nascita e non è possibile eseguire una query di join tra la tabella dei profili e un'altra tabella. Archiviazione tabelle è più scalabile e meno costoso rispetto a un database relazionale, ma non di abilitare le query complesse o join.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) sono database chiave/valore in cui i valori siano *documenti*. "Documento" di seguito non viene usato nel senso di un documento di Word o Excel, ma si intende una raccolta di campi denominati e i valori, ognuno dei quali può essere un documento secondario. In una tabella di cronologia di ordine, ad esempio, potrebbe essere un documento di ordine di numero di ordine, data dell'ordine e i campi del cliente; e il campo cliente potrebbe avere campi nome e indirizzo. Il database di codifica dei dati in un formato, ad esempio XML, YAML, JSON o BSON; oppure è possibile usare testo normale. Una funzionalità che consente di impostare il database di documenti oltre alla chiave-valore è la possibilità di eseguire query su campi non chiave e definire indici secondari per rendere più efficiente l'esecuzione di query. Questa possibilità rende un database di documenti più adatto per le applicazioni che devono recuperare i dati in base ai criteri più complessi rispetto al valore della chiave del documento. Ad esempio, in un database di documenti di ordine di vendita della cronologia è Impossibile eseguire una query in diversi campi quali ID prodotto, ID cliente, nome del cliente e così via. [MongoDB](http://www.mongodb.org/) è un database di documenti più diffusi.
- [Database a colonne](https://msdn.microsoft.com/library/dn313285.aspx#sec9) sono gli archivi dati che ti consentono di archiviazione di strutture dei dati in raccolte di colonne correlate, dette famiglie di colonne chiave/valore. Ad esempio, un database census potrebbe avere un gruppo di colonne per nome di una persona (in primo luogo, secondo nome, cognome), un gruppo per l'indirizzo del contatto e un gruppo per le informazioni sul profilo (tua data di nascita, sesso e così via.). Il database può quindi archiviare ogni famiglia di colonne in una partizione separata, mantenendo tutti i dati di una persona correlati alla stessa chiave. È quindi possibile leggere tutte le informazioni del profilo senza la necessità di leggere con attenzione tutte informazioni nome e l'indirizzo. [Cassandra](http://cassandra.apache.org/) è un diffuso database a colonne.
- [Grafico i database](https://msdn.microsoft.com/library/dn313285.aspx#sec10) archiviare informazioni come una raccolta di oggetti e relazioni. Lo scopo di un database a grafo è abilitare un'applicazione da eseguire in modo efficiente le query che passano attraverso la rete di oggetti e le relazioni tra di essi. Ad esempio, gli oggetti possono essere impiegati in un database di gestione delle risorse umane e potrebbe essere consigliabile per facilitare le query, ad esempio "trovare tutti i dipendenti che lavorano direttamente o indirettamente a Scott." [Neo4j](http://www.neo4j.org/) è un diffuso database a grafi.

Rispetto ai database relazionali, le opzioni NoSQL offrono maggiore scalabilità ed efficienza nei costi per l'archiviazione e analisi dei dati non strutturati. Lo svantaggio è che non offrono il queryability avanzate e le funzionalità di integrità dei dati affidabili dei database relazionali. NoSQL funzionerebbe bene per i dati di log IIS, che prevede l'elevato volume senza che sia necessario per le query join. Database NoSQL non funzionerebbe bene per il settore bancario transazioni, che richiede l'integrità dei dati assoluta e implica molte relazioni con altri dati relativi agli account.

È inoltre disponibile una categoria più recente della piattaforma di database denominata [NewSQL](http://en.wikipedia.org/wiki/NewSQL) che combina la scalabilità di un database NoSQL con la queryability e l'integrità transazionale di un database relazionale. I database NewSQL sono progettati per l'archiviazione distribuita e l'elaborazione di query, che spesso è difficile da implementare nei database "OldSQL". [NuoDB](http://www.nuodb.com/) è riportato un esempio di un database NewSQL che può essere utilizzato in Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop e MapReduce

I volumi elevati di dati che è possibile archiviare nei database NoSQL possono essere difficili da analizzare in modo efficiente in modo tempestivo. Per eseguire operazioni che è possibile usare un framework, ad esempio [Hadoop](http://hadoop.apache.org/) che implementa [MapReduce](http://en.wikipedia.org/wiki/MapReduce) funzionalità. Essenzialmente quali un processo MapReduce è la seguente:

- Limite di dimensioni dei dati che devono essere elaborati selezionando i dati di archivio solo i dati effettivamente necessari per l'analisi. Ad esempio, si desidera conoscere la composizione dell'utente di base per anno di nascita, quindi si seleziona solo gli anni di nascita dall'archivio dati di profilo utente.
- Suddividere i dati in parti e inviarli a computer diversi per l'elaborazione. Il computer viene calcolato il numero di persone con date 1950 1959, il computer B esegue 1960-1969, e così via. Il gruppo di computer viene chiamato un *cluster Hadoop*.
- Inserire i risultati di ogni parte tenerle insieme al termine dell'elaborazione per le parti. Ora disponibile un elenco relativamente breve di quante persone per ogni anno di nascita e il compito di calcolare le percentuali in questo elenco globale è gestibile.

In Azure, [HDInsight](https://azure.microsoft.com/services/hdinsight/) consente di elaborare, analizzare e Ottieni nuovi approfondimenti dai big data con la potenza di Hadoop. Ad esempio, è possibile usarlo per analizzare i log del server web:

- Abilitare la registrazione del server web all'account di archiviazione. Si configura Azure per scrivere i log del servizio Blob per ogni richiesta HTTP all'applicazione. Il servizio Blob è fondamentalmente l'archiviazione file di cloud e si integra perfettamente con HDInsight. 

    ![Log nell'archiviazione BLOB](data-storage-options/_static/image2.png)
- Quando l'applicazione diventa il traffico, i log IIS del server web vengono scritti nell'archiviazione Blob. 

    ![Log del server Web](data-storage-options/_static/image3.png)
- Nel portale, fare clic su **New** - **Data Services** - **HDInsight** - **creazione rapida**, e specificare il nome di un cluster HDInsight, dimensioni del cluster (numero di nodi dati del cluster HDInsight) e un nome utente e password per il cluster HDInsight. 

    ![HDInsight](data-storage-options/_static/image4.png)

È ora possibile configurare i processi MapReduce per analizzare i log e ottenere risposte a domande come:

- Le ore del giorno mia app passare il traffico maggiori o minori?
- In quali paesi è il mio traffico proveniente da?
- Che cos'è il reddito medio vicinato di aree di da che My il traffico proviene. (È un set di dati pubblico che offre reddito vicinato tramite l'indirizzo IP, e si può corrispondere a quello per indirizzo IP nel log del server web).
- Come reddito vicinato è correlato a pagine specifiche o prodotti nel sito?

È quindi possibile usare le risposte a domande come queste per gli annunci di destinazione basati sulla probabilità che un cliente sarebbe esserti utili o con maggiore probabili acquisteranno un prodotto specifico.

Come spiegato nel [automatizzare tutto capitolo](automate-everything.md), è possono automatizzare la maggior parte delle funzioni che è possibile eseguire nel portale e che include la configurazione e l'esecuzione dei processi di analisi di HDInsight. Uno script di HDInsight tipico potrebbe contenere i passaggi seguenti:

- Effettuare il provisioning di un cluster HDInsight e collegarla all'account di archiviazione per l'input archiviazione Blob.
- Caricare il processo MapReduce di file eseguibili (file con estensione jar o .exe) nel cluster HDInsight.
- Inviare un MapReduce che archivia i dati di output in archiviazione Blob.
- Attendere il completamento del processo.
- Eliminare il cluster HDInsight.
- Accedere all'output da un archivio Blob.

Eseguendo uno script che fa tutto questo, ridurre al minimo la quantità di tempo che viene eseguito il provisioning di cluster HDInsight, riducendo al minimo i costi.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Piattaforma distribuita come servizio (PaaS) e infrastruttura come servizio (IaaS)

Le opzioni di archiviazione di dati elencate in precedenza includono Platform-as-a-Service (PaaS) e soluzioni Infrastructure-as-a-Service (IaaS). PaaS significa che è gestire l'infrastruttura hardware e software e si usa solo il servizio. Database SQL è una funzionalità PaaS di Azure. È chiedere per i database e dietro le quinte Azure Configura e configuri le macchine virtuali e configura i database su di essi. Si ha accesso diretto alle macchine virtuali e non per gestirli. IaaS significa che è configurato, configurare e gestire macchine virtuali che eseguono nella nostra infrastruttura centro dati e si inserirvi qualsiasi elemento su di essi. Offriamo una raccolta di immagini di macchine Virtuali preconfigurate per le configurazioni di VM comuni. Ad esempio, è possibile installare immagini di macchine Virtuali preconfigurate per Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Oracle Database e così via.

Le soluzioni dati PaaS offerte da Azure includono:

- Database SQL di Azure (precedentemente noto come SQL Azure). Un database relazionale cloud basato su SQL Server.
- Archiviazione tabelle di Azure. Un chiave/valore database NoSQL.
- Archiviazione Blob di Azure. Archiviazione di file nel cloud.

Per IaaS, è possibile eseguire alcuna operazione che è possibile caricare una macchina virtuale, ad esempio:

- Database relazionali come SQL Server, Oracle, MySQL, SQL Compact, SQLite o Postgres.
- Archivi dati chiave/valore, ad esempio con Memcache, Redis, Cassandra e Riak.
- Gli archivi dati di colonna, ad esempio HBase.
- Database di documenti, ad esempio MongoDB e RavenDB, CouchDB.
- Database di grafi, Neo4j come.

![Opzioni di archiviazione di dati in Azure](data-storage-options/_static/image5.png)

L'opzione IaaS ti offre opzioni di archiviazione quasi illimitato di dati e molti di essi sono particolarmente facile da utilizzare poiché è possibile creare macchine virtuali usando immagini preconfigurate. Nel portale di gestione, ad esempio, passare a **macchine virtuali**, fare clic sui **immagini** scheda, quindi scegliere **Sfoglia VM Depot**.

![Sfoglia VM Depot](data-storage-options/_static/image6.png)

Viene quindi visualizzato un elenco dei [centinaia di immagini di macchina virtuale preconfigurate](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), ed è possibile creare una macchina virtuale da un'immagine con un sistema di gestione di database preinstallato, ad esempio MongoDB, Neo4J, Redis, Cassandra o CouchDB:

![MongoDB in VM Depot](data-storage-options/_static/image7.png)

Azure rende facile da usare, come possibili opzioni di archiviazione dati IaaS, ma le offerte PaaS offrono molti vantaggi che li rendono più economico e pratico per molti scenari:

- Non è necessario creare macchine virtuali, è sufficiente utilizzare il portale o uno script per configurare un archivio dati. Se si desidera che un archivio dati di 200 TB, è possibile semplicemente fare clic su un pulsante o eseguire un comando e in pochi secondi è pronto per l'utilizzo.
- Non è necessario gestire o applicare patch le macchine virtuali usate dal servizio. Microsoft esegue automaticamente automaticamente.-non è necessario preoccuparsi della configurazione di infrastruttura per la disponibilità elevata o di ridimensionamento; Microsoft gestisce tutto questo automaticamente.
- Non è necessario acquistare licenze; costi delle licenze sono inclusi nelle tariffe del servizio.
- Si paga solo per le risorse usate.

Opzioni di archiviazione dati PaaS in Azure includono le offerte dai provider di terze parti. Ad esempio, è possibile scegliere il [componente aggiuntivo MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) da Store di Azure per effettuare il provisioning di un database MongoDB come servizio.

## <a name="choosing-a-data-storage-option"></a>Scegliendo un'opzione di archiviazione dei dati

Non un approccio è adatto a tutti gli scenari. Se tutti gli utenti è indicato che questa tecnologia è questa la risposta, la prima cosa da porre è "Qual è la domanda?", poiché soluzioni diverse sono ottimizzate per scopi diversi. Esistono vantaggi definiti per il modello relazionale; Ecco perché è stato rilasciata così a lungo. Ma ci sono anche verso il basso-facce di SQL che può essere indirizzata con una soluzione NoSQL.

Spesso ciò che vediamo funzionano in modo ottimale è un approccio composizionale, in cui si usa SQL e NoSQL in un'unica soluzione. Anche quando persone sostengono che si adottano NoSQL, se si analizza cosa sta facendo è spesso find che stanno usando Framework diversi NoSQL: utilizzano [CouchDB](http://wiki.apache.org/couchdb/Introduction), e [Redis](http://redis.io/)e [ Riak](http://basho.com/riak/) per scopi diversi. Persino Facebook, che fa ampio uso di database NoSQL, Usa diversi Framework NoSQL per le diverse parti del servizio. La possibilità di combinare e abbinare gli approcci di archiviazione di dati è uno degli aspetti che è interessante nel cloud, perché è facile da usare più soluzioni di data e di integrarle in una singola app.

Di seguito sono riportate alcune domande da considerare quando si sceglie un approccio:

| Semantica dei dati | -Che cos'è il core dati e archiviazione DAS semantica (si stanno archiviando i dati relazionali o non strutturati)? I dati non strutturati, ad esempio i file multimediali si adatta meglio nell'archiviazione blob. una raccolta di dati correlati, ad esempio prodotti, inventari, fornitori, gli ordini dei clienti e così via, si adatta meglio in un database relazionale. |
| --- | --- |
| Supporto delle query | -Come è facile per eseguire query sui dati? -I tipi di domande che possono essere efficiente frequenti? Gli archivi chiave/valore dei dati sono molto efficaci per ottenere una singola riga ha un valore di chiave, ma non in modo ottimale per le query complesse. Per un utente del profilo data store in cui si ottengono sempre i dati per un particolare utente, un archivio dati chiave/valore poteva funzionare bene; per un catalogo di prodotti in cui si desiderano raggruppamenti differenti in base agli attributi di vari prodotti potrebbe funzionare meglio un database relazionale. I database NoSQL è possono archiviare volumi elevati di dati in modo efficiente, ma è necessario strutturare il database intorno a come eseguire query sui dati dell'app e ciò rende più difficile da eseguire query ad hoc. Con un database relazionale, è possibile compilare qualsiasi tipo di query. |
| Proiezione funzionale | -È possibile domande, aggregazioni e così via, essere eseguita sul lato server? Se esegue SELECT COUNT (\*) da una tabella in SQL, verrà in modo molto efficiente eseguire tutte le operazioni sul server e restituire il numero che si sta cercando. Se desidera che lo stesso calcolo da un archivio dati NoSQL che non supporta l'aggregazione, questo è una "unbounded query inefficiente" e probabilmente raggiungerà il timeout. Anche se la query ha esito positivo deve recuperare tutti i dati dal server al client e contare le righe nel client. -Quali linguaggi o i tipi di espressioni possono essere utilizzati? Con un database relazionale è possibile usare SQL. Con alcuni database NoSQL, ad esempio archiviazione tabelle di Azure, utilizzerò [OData](http://www.odata.org/), e posso filtrare sulla chiave primaria e ottenere le proiezioni (selezionare un subset dei campi disponibili). |
| Facilità di scalabilità | -Modalità spesso e come quantità che verrà dati necessario per la scalabilità? -La piattaforma di implementa in modo nativo scalabilità? -Come è facile aggiungere o rimuovere capacità (dimensioni e velocità effettiva)? Le tabelle e database relazionali non sono partizionati automaticamente per fare in modo scalabile, in modo da risultare difficili da ridimensionare più di alcune limitazioni. Gli archivi dati NoSQL, ad esempio archiviazione tabelle di Azure di partizione intrinsecamente tutti gli elementi e quasi illimitato all'aggiunta di partizioni. È possibile passare rapidamente archiviazione tabelle fino a 200 terabyte, ma le dimensioni massime del database per Database SQL di Azure sono 500 GB. È possibile ridimensionare i dati relazionali suddividendolo in più database, ma l'impostazione di un'applicazione per supportare questo modello implica una grande quantità di lavoro di programmazione. |
| Strumentazione e la gestibilità | -La semplicità è la piattaforma per instrumentare, monitorare e gestire? Si dovrà restare informati sull'integrità e prestazioni dell'archivio dati, pertanto è necessario conoscere in anticipo quali metriche una piattaforma di fornisce gratuitamente e ciò che è necessario sviluppate in modo indipendente. |
| Operazioni | -La semplicità è la piattaforma per distribuire ed eseguire in Azure? PaaS? Modello IaaS? Linux? Archiviazione tabelle e Database SQL sono facili da configurare in Azure. Piattaforme che non sono soluzioni PaaS di Azure predefinite risultare più complessa. |
| Supporto dell'API | -È disponibile un'API che rende più semplice lavorare con la piattaforma? Per il servizio tabelle di Azure è disponibile un SDK con un'API .NET che supporta il modello di programmazione asincrono .NET 4.5. Se si sta scrivendo un'app .NET, sarà molto più semplice scrivere e testare il codice per il servizio tabelle di Azure rispetto a un'altra chiave/valore colonna store piattaforma dati che dispone di alcuna API o in una minore completa. |
| Coerenza di dati e l'integrità transazionale | -È fondamentale che la piattaforma supporta le transazioni per garantire la coerenza dei dati? Per tenere traccia dei messaggi di posta elettronica in blocco inviate, prestazioni e costi di archiviazione dati bassa potrebbe essere più importante del supporto automatico per le transazioni o l'integrità referenziale nella piattaforma dati, rendendo il servizio tabelle di Azure la scelta ideale. Per tenere traccia di conto bancario bilancia o gli ordini di acquisto una piattaforma di database relazionale che offre ottime garanzie transazionali sarebbe una scelta migliore. |
| Continuità aziendale | -Semplice sono backup, ripristino e ripristino di emergenza? Prima o poi i dati di produzione verranno danneggiati e sarà necessario una funzione di annullamento. Database relazionali hanno spesso più funzionalità di ripristino con granularità fine, ad esempio la possibilità di ripristinare in un punto nel tempo. Comprendere quali funzionalità di ripristino sono disponibili in ogni piattaforma che si sta considerando il è un fattore importante da prendere in considerazione. |
| Costi | -Se più di una piattaforma può supportare il carico di lavoro dei dati, le differenze nella costo? Ad esempio, se si usa ASP.NET Identity, è possibile archiviare i dati del profilo utente nel servizio tabelle di Azure o Database SQL di Azure. Se non sono necessarie le numerose query su strutture di Database SQL, è possibile scegliere in parte le tabelle di Azure perché ha un costo molto inferiore per un determinato periodo di archiviazione. |

Che cos'è in genere consigliabile conoscere le risposte alle domande in ognuna di queste categorie prima di scegliere le soluzioni di archiviazione dei dati.

Inoltre, il carico di lavoro potrebbe avere requisiti specifici che alcune piattaforme possono supportare prestazioni migliori rispetto ad altri utenti. Ad esempio:

- Richiede l'applicazione controlla le funzionalità?
- Quali sono i requisiti di durabilità dei dati, ovvero si richiedono funzionalità di archiviazione o ripulitura automatica?
- Si hanno esigenze di sicurezza specializzati? Ad esempio, i dati includono informazioni personali (informazioni personali), ma è necessario essere in grado di verificare che le informazioni personali vengano escluso dai risultati della query.
- Se si dispone di alcuni dati che non possono essere archiviati nel cloud per motivi legali o tecnologici, potrebbe essere una piattaforma di archiviazione dei dati cloud che facilita l'integrazione con l'archiviazione locale.

## <a name="demo--using-sql-database-in-azure"></a>Demo: uso di Database SQL in Azure

L'app Fix It Usa un database relazionale per archiviare le attività. Lo script di Windows PowerShell di creazione ambiente illustrato nella [automatizzare tutto capitolo](automate-everything.md) crea due istanze del Database SQL. È possibile visualizzare questi elementi nel portale facendo il **database SQL** scheda.

![Database SQL nel portale](data-storage-options/_static/image8.png)

È anche facile creare i database tramite il portale.

Fare clic su **New - Data Services** -- **Database SQL** -- **creazione rapida**, immettere un nome di database, scegliere un server nel proprio account è già presente o crearne uno nuovo e fare clic su **Create Database SQL**.

![Nuovo database SQL](data-storage-options/_static/image9.png)

Attendere alcuni secondi e si dispone di un database in Azure pronta da usare.

![Nuovo Database SQL creato](data-storage-options/_static/image10.png)

In modo da Azure in pochi secondi, ciò che potrebbe richiedere è un giorno o settimana o più tempo per eseguire nell'ambiente locale. E poiché è semplicemente possibile creare automaticamente i database in uno script o utilizzando un'API di gestione, è la scalabilità orizzontale dinamica distribuendo i dati tra più database < o:p >, purché l'applicazione è stata programmata per cui. < /o : p >

Questo è un esempio del modello Platform-as-a-Service. Non è necessario gestire i server, è eseguire questa operazione. Non è necessario preoccuparsi di backup, useremo. È in esecuzione in un'elevata disponibilità, i dati nel database vengono replicati in tre server automaticamente. Se si interrompe una macchina, viene eseguito il failover e non si perde alcun dato. Il server viene riparato regolarmente, non occorre preoccuparsi che.

Fare clic su un pulsante e ottenere la stringa di connessione esatte necessarie e può iniziare immediatamente a usare il nuovo database.

![Stringhe di connessione](data-storage-options/_static/image11.png)

Il Dashboard Mostra la cronologia delle connessioni e la quantità di spazio di archiviazione usato.

![Dashboard del Database SQL](data-storage-options/_static/image12.png)

È possibile gestire i database nel portale o con gli strumenti di SQL Server che già conosci, tra cui SQL Server Management Studio (SSMS) e gli strumenti di Visual Studio, Esplora Server e SQL Server oggetto Explorer (SSOX).

![SSOX](data-storage-options/_static/image13.png)

Un altro aspetto interessante è il modello di determinazione prezzi. È possibile iniziare lo sviluppo con un database gratuito da 20 MB e un database di produzione inizia in corrispondenza di circa 5 dollari al mese. Paghi solo la quantità di dati che effettivamente archiviati nel database, non la capacità massima. Non devi acquistare una licenza.

Database SQL è semplice applicare la scalabilità. Per l'app Fix It, il database che verrà creata nello script di automazione prevede un limite di 1 GB. Se si desidera che per la scalabilità fino a 150 GB, è possibile solo passare al portale e modificare tale impostazione o eseguire un comando API REST e in pochi secondi si dispone di un database di 150 GB che è possibile distribuire i dati in.

![Le dimensioni e le edizioni di Database SQL](data-storage-options/_static/image14.png)

Che è la potenza del cloud per realizzare infrastrutture facilmente e rapidamente e iniziare a usare immediatamente.

L'app Fix It usa due database SQL, uno per l'appartenenza (autenticazione e autorizzazione) e uno per i dati e questo è tutto ciò che devi fare per effettuare il provisioning e scalabilità. Illustrato in precedenza come effettuare il provisioning di database tramite gli script di Windows PowerShell e a questo punto si è visto anche come è facile da eseguire nel portale.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework e accesso diretto al database mediante ADO.NET

L'app Fix It accede a questi database tramite Entity Framework, ORM (object relational mapper) consigliata da Microsoft per le applicazioni .NET. Un ORM è uno strumento straordinario che facilita la produttività degli sviluppatori, ma la produttività comporta tuttavia influire negativamente sulle prestazioni in alcuni scenari. In un'app per cloud reali non si apporteranno una scelta tra l'uso di Entity Framework o tramite ADO.NET direttamente, si userà entrambi. La maggior parte dei casi quando si scrive codice che funziona con il database, ottenere le massime prestazioni non è critico ed è possibile sfruttare la scrittura di codice semplificato e i test che si ottiene con Entity Framework. In situazioni in cui il sovraccarico EF causerebbe inaccettabile delle prestazioni, è possibile scrivere ed eseguire query usando ADO.NET, idealmente chiamando le stored procedure.

Indipendentemente dal metodo usato per accedere al database, si vuole ridurre al minimo la "frammentarietà" quanto più possibile. In altre parole, se è possibile ottenere tutti i dati che necessari in uno più grande set di risultati query invece di decine o centinaia di quelli più piccoli, che è in genere preferibile. Ad esempio, se è necessario per gli studenti di elenco e i corsi che sono stati registrati in, è in genere migliore ottenere tutti i dati nella query di un join anziché recupero degli studenti in una query e l'esecuzione di query separate per i corsi di ogni studente.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Database SQL ed Entity Framework nell'app Fix It

Nell'app Fix It il `FixItContext` classe che deriva da Entity Framework `DbContext` classe, identifica il database e specifica le tabelle nel database. Il contesto specifica un set di entità (tabella) per le attività e il codice passa al contesto il nome di stringa di connessione. Questo nome fa riferimento a una stringa di connessione definita nel file Web. config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

La stringa di connessione nel *Web. config* file è denominato appdb (qui che punta al database di sviluppo locale):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Entity Framework crea una *FixItTasks* tabella basata sulle proprietà incluse nel `FixItTask` classe di entità. Si tratta di una classe POCO (Plain Old CLR Object) semplice, il che significa non erediti da o è presente alcuna dipendenza su Entity Framework. Ma Entity Framework in grado di creare una tabella basata su di esso ed eseguire le operazioni CRUD (create-read-update-delete) con esso.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabella FixItTasks](data-storage-options/_static/image15.png)

L'app Fix It include un'interfaccia del repository utilizzata per operazioni CRUD di lavorare con l'archivio dati.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Si noti che i metodi di repository sono tutti asincroni, in modo che ogni accesso ai dati può essere eseguita in un modo completamente asincrono.

L'implementazione del repository chiama i metodi asincroni di Entity Framework per lavorare con i dati, tra cui query LINQ oltre a quella di inserimento, Aggiorna ed Elimina le operazioni. Di seguito è riportato un esempio del codice per la ricerca di un'attività Fix It.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Si noterà che è anche disponibile alcuni temporizzazione e codice di registrazione errore in questo caso, vedremo che successivamente nel [capitolo di monitoraggio e telemetria](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Scelta di Database SQL (PaaS) e SQL Server in una VM (IaaS) in Azure

Un aspetto interessante su SQL Server e Database SQL di Azure è che il modello di programmazione principali per entrambi è identico. È possibile usare la maggior parte delle competenze stesso in entrambi gli ambienti. È anche possibile usare un database di SQL Server in fase di sviluppo e un'istanza di Database SQL nel cloud, ovvero come impostare l'app Fix It.

In alternativa, è possibile eseguire la stessa di SQL Server nel cloud che si esegue in locale tramite l'installazione nelle macchine virtuali IaaS. Per alcune applicazioni legacy che esegue SQL Server in una macchina virtuale potrebbe essere una soluzione migliore. Poiché un database di SQL Server viene eseguito su una macchina virtuale dedicata, dispone di più risorse disponibili per il processo rispetto a un database di Database SQL che viene eseguito in un server condiviso. Ovvero un database di SQL Server può essere più grande e comunque prestazioni ottimali. In generale, minori le dimensioni del database e tabella, migliore il caso d'uso funziona per Database SQL (PaaS).

Di seguito sono riportate alcune linee guida su come scegliere tra i due modelli.

| Database SQL di Azure (PaaS) | SQL Server in una macchina virtuale (IaaS) |
| --- | --- |
| **I professionisti** -non è necessario creare o gestire le macchine virtuali, aggiornare o applicare patch a sistema operativo o SQL. Azure esegue automaticamente. -Incorporate disponibilità elevata, con un contratto di servizio a livello di database. -A basso costo totale di proprietà (TCO) poiché si paga solo ciò che usi (non è necessaria alcuna licenza). -Adatti per la gestione di un numero elevato di database di dimensioni ridotte (&lt;= 500 GB ciascuno). -Facile da creare in modo dinamico nuovi database per abilitare la scalabilità orizzontale. | ***I professionisti*** - funzionalità compatibile con on-premises SQL Server. -È possibile implementare SQL Server [disponibilità elevata tramite AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) in oltre 2 macchine virtuali, con contratto di servizio a livello di macchina virtuale. -È avere controllo completo sulla modalità di gestione SQL. -È possibile riutilizzare le licenze SQL già proprietari oppure pagare con l'ora di uno. -Adatti per la gestione di un numero inferiore ma di dimensioni maggiori (1 TB +) i database. |
| **Svantaggi** -alcune delle funzionalità gap rispetto a un'istanza locale di SQL Server (non dispongono di [integrazione con CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [supporto della compressione](https://technet.microsoft.com/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx)e così via)-limite delle dimensioni di Database di 500 GB. | ***Svantaggi*** : aggiornamenti/patch (sistema operativo e SQL) sono responsabilità del cliente - creazione e gestione di database sono responsabilità del cliente - disco IOPS (operazioni di input/output al secondo) limitato a circa 8000 (tramite le unità 16 data). |

Se si desidera usare SQL Server in una macchina virtuale, è possibile usare la propria licenza di SQL Server oppure è possibile pagare per una base oraria. Nel portale o tramite l'API REST, ad esempio, è possibile creare una nuova macchina virtuale usando un'immagine di SQL Server.

![Creare VM con SQL Server](data-storage-options/_static/image16.png)

![Elenco di immagini di VM di SQL Server](data-storage-options/_static/image17.png)

Quando si crea una VM con un'immagine di SQL Server, ripartizione il costo della licenza di SQL Server base oraria in base all'utilizzo della macchina virtuale. Se si dispone di un progetto che tratta solo la funzionalità per l'esecuzione per un paio di mesi, è più conveniente di addebito con l'ora. Se si ritiene che il progetto sta per ultimo per anni, è più conveniente acquistare la licenza il modo in cui che si farebbe normalmente.

## <a name="summary"></a>Riepilogo

Il cloud computing rende utile combinare e approcci di archiviazione di dati di corrispondenza al meglio le esigenze dell'applicazione. Se si sta creando una nuova applicazione, considerare attentamente le risposte alle domande elencate di seguito per usare gli approcci che continueranno a funzionare anche quando l'applicazione aumenta. Il [capitolo successivo](data-partitioning-strategies.md) illustra alcune strategie di partizionamento che è possibile usare per combinare più approcci di archiviazione di dati.

## <a name="resources"></a>Risorse

Per altre informazioni, vedere le seguenti risorse.

Scegliere una piattaforma di database:

- [Accesso ai dati per le soluzioni altamente scalabili: con SQL, NoSQL e persistenza poliglotta](http://aka.ms/dag-doc). E-book Microsoft Patterns and Practices che esamina in dettaglio i diversi tipi di dati archivia disponibili per le applicazioni cloud.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/ff898430.aspx). Vedere Introduzione alla coerenza dei dati, la replica dei dati e indicazioni di sincronizzazione, il modello di tabella dell'indice, il modello di vista materializzata.
- [BASE: Alternativa Acid](http://queue.acm.org/detail.cfm?id=1394128). L'articolo sui compromessi tra coerenza dei dati e scalabilità.
- [Sette database di sette settimane: una Guida per i database moderni e il movimento "NoSQL"](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Libro di Eric Redmond e Jim R. Wilson. Fortemente consigliato per l'introduzione di autonomamente la gamma di piattaforme di archiviazione di dati attualmente disponibili.

Scelta tra SQL Server e Database SQL:

- [Anteprima di Premium per linee guida del Database SQL](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Introduzione a Database SQL Premium e indicazioni su quando scegliere tramite le edizioni di SQL Database Web e Business.
- [Linee guida e limitazioni (Database SQL di Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Pagina del portale che include collegamenti alla documentazione sulle limitazioni del Database SQL, tra cui quella che si concentra sulle funzionalità di SQL Server Database SQL non supporta.
- [SQL Server in macchine virtuali di Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Pagina del portale che include collegamenti alla documentazione sull'esecuzione di SQL Server in Azure.
- [Scott Guthrie spiega database SQL di Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). 6 minuti video di introduzione al Database SQL di Scott Guthrie.
- [I modelli di applicazione e strategie di sviluppo per SQL Server in macchine virtuali di Azure](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Uso di Entity Framework e il Database SQL in un'app Web ASP.NET

- [Introduzione a EF 6 con MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Serie di esercitazioni nove parti che illustra la creazione di un'app MVC che usa Entity Framework e distribuisce il database di Azure e Database SQL.
- [Distribuzione Web ASP.NET tramite Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Dodici parti serie di esercitazioni che va inserito nel modo più approfondito su come distribuire un database tramite Entity Framework Code First.
- [Distribuire un'app ASP.NET MVC 5 sicura con appartenenza, OAuth e Database SQL in un sito Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Esercitazione dettagliata che illustra come creare un'app web che utilizza l'autenticazione, le tabelle dell'applicazione sono archiviate nel database delle appartenenze, modifica dello schema del database e l'app viene distribuita in Azure.
- [Mappa del contenuto ASP.NET Data Access](https://go.microsoft.com/fwlink/p/?LinkId=282414). Collegamenti alle risorse per l'uso di Entity Framework e il Database SQL.

Uso di MongoDB in Azure:

- [MongoLab - MongoDB in Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Pagina del portale per la documentazione sull'esecuzione di MongoDB in Azure.
- [Creare un sito web di Azure che si connette a MongoDB in esecuzione su una macchina virtuale in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Esercitazione dettagliata che illustra come usare un database MongoDB in un'applicazione web ASP.NET.

HDInsight (Hadoop in Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Documentazione di HDInsight nel portale di [Azure](https://azure.microsoft.com/) sito Web.
- [Hadoop e HDInsight: Big Data in Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Articolo di MSDN Magazine Bruno Terkaly e Ricardo Villalobos, Introduzione a Hadoop in Azure.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Modello MapReduce vedere.

> [!div class="step-by-step"]
> [Precedente](single-sign-on.md)
> [Successivo](data-partitioning-strategies.md)
