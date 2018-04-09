---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Opzioni di archiviazione di dati (creazione di applicazioni con Azure Cloud del mondo reale) | Documenti Microsoft
author: MikeWasson
description: Le App per Cloud mondo reale compilazione con e-book Azure si basa su una presentazione sviluppata da Scott Guthrie. Viene spiegato 13 modelli e procedure che è possibile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: d638dca331cb24c340a4471e5964a00b75bb608a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Opzioni di archiviazione di dati (creazione di applicazioni Cloud reale in Azure)
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download correggerlo progetto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [scaricare E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Il **predefiniti reale World Cloud App con Azure** e-book è basato su una presentazione sviluppata da Scott Guthrie. Vengono descritte le 13 modelli e procedure consigliate che consentono di avere esito negativo con lo sviluppo di App web per il cloud. Per informazioni sull'e-book, vedere [primo capitolo](introduction.md).


La maggior parte delle persone vengono utilizzati per i database relazionali e tendono a sottovalutare altre opzioni di archiviazione di dati quando si progetta un'applicazione cloud. Può essere il risultato delle prestazioni non ottimali, spese elevate o peggiorando, perché [NoSQL](http://en.wikipedia.org/wiki/NoSQL) database (non relazionali) possono gestire alcune attività in modo più efficiente rispetto ai database relazionali. Quando i clienti chiedere per risolvere un problema di archiviazione di dati critici, è spesso perché contengono un database relazionale in cui una delle opzioni di database NoSQL avrebbe avuto prestazioni migliori. In questi casi il cliente sarebbe stato preferibile se si avesse implementato la soluzione NoSQL prima di distribuire l'app nell'ambiente di produzione.

D'altra parte, sarebbe inoltre un errore di supporre che un database NoSQL possa eseguire tutte le operazioni anche o abbastanza bene. Nessun criterio di gestione dati migliore singola per tutte le attività di archiviazione di dati. soluzioni di gestione di dati diversi vengono ottimizzate per le diverse attività. La maggior parte delle App cloud del mondo reale un'ampia gamma di requisiti di archiviazione dati e spesso vengono gestite migliore da una combinazione di più soluzioni di archiviazione di dati.

Lo scopo di questo capitolo è per avere un'idea più ampia delle opzioni di archiviazione di dati disponibili per un'applicazione cloud e alcune linee guida di base su come scegliere quelli adatti al proprio scenario. È consigliabile tenere in considerazione le opzioni disponibili e valutare i vantaggi e svantaggi prima di sviluppare un'applicazione. Modificando le opzioni di archiviazione di dati in un'app di produzione può essere estremamente difficile, ad esempio la necessità di modificare un motore jet, mentre il piano è in corso.

## <a name="data-storage-options-on-azure"></a>Opzioni di archiviazione di dati in Azure

Il cloud rende relativamente facile da utilizzare un'ampia gamma di relazionale e gli archivi dati NoSQL. Ecco alcune delle piattaforme di archiviazione di dati che è possibile utilizzare in Azure.

![](data-storage-options/_static/image1.png)

La tabella contiene quattro tipi di database NoSQL:

- [I database di chiave/valore](https://msdn.microsoft.com/library/dn313285.aspx#sec7) archiviare un singolo oggetto serializzato per ogni valore di chiave. Sono infatti utili per l'archiviazione di grandi volumi di dati in cui si desidera ottenere un elemento per un determinato valore di chiave e non è necessario per eseguire query in base ad altre proprietà dell'elemento.

    [Archiviazione Blob di Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) è un database di chiave/valore che funziona come l'archivio di file nel cloud, con valori di chiave che corrispondono ai nomi dei file e cartelle. Recuperare un file per la cartella e il nome di file e non per la ricerca di valori nel contenuto del file.

    [Archiviazione tabelle di Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) è anche un database di chiave/valore. Ogni valore viene chiamato un *entità* (simile a una riga, identificata da una chiave di partizione e chiave di riga) e contiene più *proprietà* (simile alle colonne, ma non tutte le entità in una tabella devono condividere lo stesso colonne). Esecuzione di query su colonne diverse dalle chiavi di è poco efficiente ed è opportuno evitarle. Ad esempio, è possibile archiviare i dati del profilo utente, con un'unica partizione, l'archiviazione delle informazioni relative a un singolo utente. È possibile memorizzare i dati, ad esempio nome utente, hash della password, la data di nascita e così via, in proprietà separate di un'entità o entità separate nella stessa partizione. Ma è preferibile eseguire una query per tutti gli utenti con un determinato intervallo di date di nascita e non è possibile eseguire una query di join tra la tabella dei profili e un'altra tabella. Archiviazione tabelle è più scalabile e meno costoso di un database relazionale, ma non consente le query più complesse o join.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) sono database chiave/valore in cui i valori sono *documenti*. "Documento" qui non viene utilizzato il concetto di un documento di Word o Excel, ma si intende una raccolta di campi denominati e i valori, ognuna delle quali potrebbe essere un documento secondario. In una tabella di cronologia di ordine, ad esempio, potrebbe essere un documento di ordine di numero di ordine, data dell'ordine e i campi del cliente; e il campo cliente potrebbe avere campi nome e indirizzo. Il database codifica i dati di campo in un formato, ad esempio BSON; YAML, JSON o XML è possibile utilizzare testo normale. Una funzionalità che imposta il database di documento oltre ai database di chiave/valore è la possibilità di eseguire una query su campi non chiave e definire gli indici secondari per rendere più efficiente l'esecuzione di query. Questa possibilità rende un database di documenti più adatte per le applicazioni necessarie per recuperare i dati in base ai criteri più complessi rispetto al valore della chiave del documento. Ad esempio, in un database di documento di ordine di vendita della cronologia è Impossibile eseguire la query in diversi campi, ad esempio ID prodotto, ID cliente, il nome del cliente e così via. [MongoDB](http://www.mongodb.org/) è un database di documenti più diffusi.
- [I database della famiglia di colonna](https://msdn.microsoft.com/library/dn313285.aspx#sec9) sono gli archivi di dati che consentono di archiviazione dei dati di struttura in raccolte di colonne correlate chiamate famiglie di colonna chiave/valore. Ad esempio, un database census potrebbe disporre di un gruppo di colonne per nome di una persona (primo, secondo nome, cognome), un gruppo per l'indirizzo dell'utente e un gruppo per le informazioni sul profilo dell'utente (tua data di nascita, sesso, ecc.). Il database è quindi possibile archiviare ogni famiglia di colonna in una partizione separata, mantenendo tutti i dati per una persona correlato per la stessa chiave. È quindi possibile leggere tutte le informazioni senza dover leggere le informazioni nome e l'indirizzo. [Cassandra](http://cassandra.apache.org/) è un database della famiglia di colonna più diffuso.
- [Grafico database](https://msdn.microsoft.com/library/dn313285.aspx#sec10) archiviare informazioni come una raccolta di oggetti e relazioni. Lo scopo di un database di grafico è per abilitare un'applicazione da eseguire in modo efficiente le query che passano attraverso la rete di oggetti e le relazioni tra di essi. Ad esempio, gli oggetti siano dipendenti in un database delle risorse umane e potrebbe essere necessario per facilitare la query, ad esempio "trovare tutti i dipendenti che lavorano direttamente o indirettamente a Scott." [Neo4j](http://www.neo4j.org/) è un database grafico più diffusi.

Rispetto ai database relazionali, le opzioni di database NoSQL offrono maggiore scalabilità e la redditività dei costi per l'archiviazione e analisi dei dati non strutturati. Il compromesso è che non forniscono le funzionalità di integrità dei dati affidabili dei database relazionali queryability RTF. NoSQL funzionerà anche per i dati di log IIS, che comporta la presenza di volumi elevati senza che sia necessario per le query di join. NoSQL non funziona bene per servizi bancari di transazioni, che richiede l'integrità dei dati assoluto e include molte relazioni con altri dati relativi agli account.

È inoltre disponibile una categoria più recente della piattaforma di database denominata [NewSQL](http://en.wikipedia.org/wiki/NewSQL) che combina la scalabilità di un database NoSQL queryability e l'integrità transazionale di un database relazionale. Database NewSQL sono progettati per l'archiviazione distribuita e l'elaborazione delle query, è spesso difficile da implementare nei database "OldSQL". [NuoDB](http://www.nuodb.com/) è riportato un esempio di un database NewSQL che può essere usato in Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop e MapReduce

I volumi elevati di dati che è possibile archiviare nel database NoSQL potrebbero essere difficili da analizzare in modo efficiente in modo tempestivo. A tale che è possibile utilizzare un framework, ad esempio [Hadoop](http://hadoop.apache.org/) che implementa l'interfaccia [MapReduce](http://en.wikipedia.org/wiki/MapReduce) funzionalità. Quali un processo MapReduce vengono essenzialmente è la seguente:

- Le dimensioni dei dati che deve essere elaborato dalla selezione di dati archiviare solo i dati effettivamente necessari per analizzare il limite. Ad esempio, si desidera conoscere la struttura dell'utente di base per anno di nascita, in modo da selezionare solo gli anni di nascita fuori l'archivio dati di profilo utente.
- Suddividere i dati in parti e li invia a diversi computer per l'elaborazione. Il computer viene calcolato il numero di persone con date 1950 1959, 1969-1960, a computer B e così via. Il gruppo di computer viene chiamato un *cluster Hadoop*.
- Inserire i risultati di ogni parte riunirli dopo l'elaborazione per le parti viene eseguita. È ora un elenco relativamente breve di quante persone per ogni anno di nascita e l'attività di calcolo delle percentuali in questo elenco globale è gestibile.

In Azure, [HDInsight](https://azure.microsoft.com/services/hdinsight/) consente di elaborare, analizzare e acquisire nuovi approfondimenti big data utilizzando le potenzialità di Hadoop. Ad esempio, è possibile utilizzare per analizzare i registri del server web:

- Abilitare la registrazione di server web per l'account di archiviazione. Azure verrà impostata per scrivere i log per il servizio Blob per tutte le richieste HTTP per l'applicazione. Il servizio Blob è fondamentalmente la memorizzazione dei file cloud e si integra perfettamente con HDInsight. 

    ![Log nell'archiviazione BLOB](data-storage-options/_static/image2.png)
- Quando l'applicazione diventa il traffico, nell'archiviazione Blob vengono scritti i log IIS del server web. 

    ![Registri del server Web](data-storage-options/_static/image3.png)
- Nel portale, fare clic su **New** - **Data Services** - **HDInsight** - **creazione rapida**, e specificare un nome del cluster HDInsight, dimensione del cluster (numero di nodi di dati del cluster HDInsight) e un nome utente e una password per il cluster HDInsight. 

    ![HDInsight](data-storage-options/_static/image4.png)

È ora possibile configurare i processi MapReduce per analizzare i log e ottenere risposte alle domande, ad esempio:

- Ora del giorno mia app Ottiene il traffico maggiori o minori?
- In quali paesi è il traffico proveniente da?
- Che cos'è il reddito medio risorse delle aree del da che traffico proviene. (È un set di dati pubblico che consente di reddito risorse indirizzo IP, e si può corrispondere a quello con indirizzo IP nei registri del server web).
- Come reddito di risorse sono correlate a pagine specifiche o prodotti nel sito?

È quindi possibile utilizzare le risposte alle domande per gli annunci di destinazione in base la probabilità che un cliente sarebbe utile o probabilmente acquisteranno un prodotto specifico.

Come spiegato nel [automatizzare tutti gli elementi capitolo](automate-everything.md), è possono automatizzare la maggior parte delle funzioni che è possibile eseguire nel portale e che include la configurazione e l'esecuzione dei processi di analisi di HDInsight. Un tipico script HDInsight potrebbe contenere i seguenti passaggi:

- Eseguire il provisioning di un cluster HDInsight e collegarlo all'account di archiviazione per l'input di archiviazione Blob.
- Caricare il processo MapReduce di file eseguibili (file JAR o .exe) al cluster HDInsight.
- Inviare un MapReduce che archivia i dati di output nell'archiviazione Blob.
- Attendere il completamento del processo.
- Eliminare il cluster HDInsight.
- Accedere all'output dall'archiviazione Blob.

Eseguendo uno script che esegue tutte, ridurre al minimo la quantità di tempo in cui viene eseguito il provisioning del cluster HDInsight, riducendo al minimo i costi.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Piattaforma distribuita come servizio (PaaS) e dell'infrastruttura come servizio (IaaS)

Le opzioni di archiviazione di dati elencate in precedenza includono Platform-as-a-Service (PaaS) sia come soluzioni Infrastructure-as-a-Service (IaaS). PaaS significa che si gestisce l'infrastruttura hardware e software ed è sufficiente utilizzare il servizio. Database SQL è una funzionalità PaaS di Azure. Chiedere per i database e in background Azure consente di impostare e consente di configurare le macchine virtuali e consente di impostare i database su di essi. Non possibile accedere direttamente alle macchine virtuali e non è necessario gestirli. IaaS significa che è impostato, configurare e gestire macchine virtuali in esecuzione nell'infrastruttura del data center e inserire desiderato su di essi. È fornire una raccolta di immagini di macchina virtuale preconfigurate per configurazioni di macchina virtuale. Ad esempio, è possibile installare preconfigurate immagini di macchina virtuale per Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Database Oracle e così via.

Soluzioni di data PaaS offerte Azure includono:

- Database SQL di Azure (precedentemente noto come SQL Azure). Un database relazionale di cloud basato su SQL Server.
- Archiviazione tabelle di Azure. Una chiave/valore database NoSQL.
- Archiviazione Blob di Azure. L'archiviazione dei file nel cloud.

Per IaaS, è possibile eseguire alcuna operazione che su una macchina virtuale, è possibile caricare, ad esempio:

- Database relazionali, ad esempio SQL Server, Oracle, MySQL, SQL Compact, SQLite o Postgres.
- Archivi dati chiave/valore, ad esempio con Memcache, Redis, Cassandra e Riak.
- Archivi dati di colonna, ad esempio HBase.
- I database, ad esempio MongoDB, RavenDB e CouchDB del documento.
- Database di grafico, ad esempio Neo4j.

![Opzioni di archiviazione di dati in Azure](data-storage-options/_static/image5.png)

L'opzione IaaS offre opzioni di archiviazione di dati quasi illimitate e molte di esse sono particolarmente facile da usare, poiché è possibile creare macchine virtuali con un'immagine preconfigurata. Nel portale di gestione, ad esempio, passare a **macchine virtuali**, fare clic su di **immagini** scheda e fare clic su **Sfoglia VM Depot**.

![Sfogliare VM Depot](data-storage-options/_static/image6.png)

Verrà quindi visualizzato un elenco di [centinaia di immagini di macchina virtuale preconfigurate](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx), ed è possibile creare una macchina virtuale da un'immagine con un sistema di gestione di database preinstallato, ad esempio MongoDB, Neo4J, Redis, Cassandra o CouchDB:

![In VM Depot MongoDB](data-storage-options/_static/image7.png)

Azure rende facile da usare, come possibili opzioni di archiviazione dati IaaS, ma le offerte PaaS offrono molti vantaggi che li rendono più economica e conveniente per molti scenari:

- Non è necessario creare macchine virtuali, è sufficiente utilizzare il portale o uno script per impostare un archivio dati. Se si desidera un archivio dati di 200 TB, è possibile semplicemente fare clic su un pulsante o eseguire un comando, e in secondi è pronto per l'utilizzo.
- Non è necessario gestire o patch per le macchine virtuali utilizzate dal servizio. Microsoft esegue automaticamente automaticamente.-non è necessario preoccuparsi di configurazione dell'infrastruttura per la disponibilità elevata o ridimensionamento; Microsoft gestisce tutto è.
- Non è necessario acquistare licenze; spese del servizio sono inclusi i costi delle licenze.
- Si paga solo per ciò che si utilizza.

Opzioni di archiviazione dati PaaS in Azure includono le offerte dai provider di terze parti. Ad esempio, è possibile scegliere di [componente aggiuntivo MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) dall'archivio di Azure per eseguire il provisioning di un database di MongoDB come servizio.

## <a name="choosing-a-data-storage-option"></a>Scegliendo un'opzione di archiviazione di dati

Non è adatta a tutti gli scenari. Se tutti gli utenti che questa tecnologia è la risposta, la prima operazione da richiedere è "Qual è la domanda?", perché diverse soluzioni sono ottimizzate per scopi diversi. Esistono vantaggi definiti per il modello relazionale. per tale motivo è stato incluso per a condizione. Ma esistono anche verso il basso-lati to SQL che possono essere gestiti con una soluzione di database NoSQL.

Spesso quello che possiamo osservare migliore è un approccio di composizione, in cui si utilizza SQL e NoSQL in un'unica soluzione. Anche quando gli utenti ad esempio si decide di NoSQL, se si analizza cosa sta facendo è spesso trova che stanno usando Framework diversi NoSQL: utilizzano [CouchDB](http://wiki.apache.org/couchdb/Introduction), e [Redis](http://redis.io/)e [ Riak](http://basho.com/riak/) per scopi diversi. Anche Facebook, che utilizza ampiamente NoSQL, utilizza Framework NoSQL diversi per parti diverse del servizio. La possibilità di combinare e associare gli approcci di archiviazione di dati è una delle operazioni che nice sul cloud, poiché è facile da usare più soluzioni di data e integrarli in una singola app.

Di seguito sono riportate alcune domande da considerare quando si sceglie un approccio:

| Semantica dei dati | -Qual è la base dati e archiviazione accesso ai dati semantica (si archiviano dati relazionali o non strutturati)? Dati non strutturati, ad esempio i file multimediali più appropriato nell'archiviazione blob. una raccolta di dati correlati, ad esempio prodotti, inventari, fornitori, gli ordini dei clienti, ecc., più appropriato in un database relazionale. |
| --- | --- |
| Supporto di query | -La semplicità è possibile eseguire query sui dati? -I tipi di domande che possono essere efficiente richiesto? Archivi dati chiave/valore sono molto efficaci per ottenere una singola riga ha un valore di chiave, ma non è pertanto appropriata per le query più complesse. Per un archivio di dati della profilo utente in cui si desidera ottenere sempre i dati per un particolare utente, un archivio dati di chiave/valore poteva funzionare. per un catalogo di prodotti in cui si desiderano raggruppamenti differenti in base a vari attributi del prodotto potrebbe funzionare meglio un database relazionale. Nessun database SQL è possono archiviare grandi volumi di dati in modo efficiente, ma sono per definire la struttura del database intorno come app eseguire query sui dati e più difficili le query ad hoc da eseguire. Con un database relazionale, è possibile creare qualsiasi tipo di query. |
| Proiezione funzionale | -Possibile domande, aggregazioni e così via, essere eseguita sul lato server? Se eseguo selezionare Conteggio (\*) da una tabella in SQL, si verrà eseguono tutte le operazioni nel server in modo molto efficiente e restituisce il numero che si sta cercando. Se lo stesso calcolo da un archivio dati NoSQL che non supporta l'aggregazione, questa è una query non efficiente"unbounded" e verrà probabilmente timeout. Anche se la query ha esito positivo, è necessario recuperare tutti i dati dal server al client e di contare le righe nel client. -Le lingue o tipi di espressioni possono essere usati? Con un database relazionale è possibile utilizzare SQL. Con alcuni database NoSQL, ad esempio l'archiviazione tabelle di Azure, useranno [OData](http://www.odata.org/), ed è sufficiente filtrare sulla chiave primaria e ottenere le proiezioni (selezionare un sottoinsieme dei campi disponibili). |
| Facilità di scalabilità | -La frequenza e il quantità dei dati devono aumentare? -La piattaforma implementa in modo nativo scalare in orizzontale? -La semplicità è per aggiungere o rimuovere capacità (velocità effettiva e dimensioni)? Tabelle e database relazionali non sono partizionate automaticamente in modo da renderli scalabile, in modo da renderli difficili da scalare oltre determinate limitazioni. Gli archivi dati NoSQL come archiviazione tabelle di Azure partizione intrinsecamente tutti gli elementi e non è quasi alcun limite per l'aggiunta di partizioni. È possibile scalare immediatamente archiviazione tabella fino a 200 TB, ma le dimensioni massime del database per Database SQL di Azure sono 500 gigabyte. È possibile scalare i dati relazionali suddividendolo in più database, ma l'impostazione di un'applicazione per supportare tale modello implica molto lavoro di programmazione. |
| Strumentazione e la gestibilità | -La semplicità è la piattaforma per instrumentare, monitorare e gestire? È necessario essere informati sull'integrità e prestazioni dell'archivio dati, pertanto è necessario conoscere in anticipo quali metriche una piattaforma offre gratuitamente, e ciò che è necessario sviluppare manualmente. |
| Operazioni | -La semplicità è la piattaforma per distribuire ed eseguire in Azure? PaaS? IaaS? Linux? Archiviazione tabelle e Database SQL sono facili da configurare in Azure. Piattaforme che non sono incorporate soluzioni PaaS per Azure risultare più impegnativo. |
| Supporto API | -È disponibile un'API che rende più facile da usare con la piattaforma? Per il servizio tabelle di Azure è disponibile un SDK con un'API .NET che supporta il modello di programmazione asincrona .NET 4.5. Se si sta scrivendo un'applicazione .NET, e deve essere molto più semplice scrivere e testare il codice per il servizio tabelle di Azure rispetto a un'altra chiave/valore colonna dati archivio piattaforma che dispone di alcuna API o una minore completa. |
| Coerenza di dati e l'integrità transazionale | -È che la piattaforma supporta le transazioni per garantire la coerenza dei dati critico? Per tenere traccia dei messaggi di posta elettronica in blocco inviati, prestazioni e costi di archiviazione dati bassa potrebbe essere più importante del supporto automatico per le transazioni o l'integrità referenziale della piattaforma di dati, il servizio tabelle di Azure una scelta ottimale. Per tenere traccia di conto bancario saldi o gli ordini di acquisto una piattaforma di database relazionale che fornisce ottime garanzie transazionale sarebbe una scelta migliore. |
| Continuità aziendale | -La semplicità sono backup, ripristino e ripristino di emergenza? Prima o poi verranno corrotti dati di produzione e sarà necessario una funzione di annullamento. Database relazionali hanno spesso altre funzionalità di ripristino con granularità fine, ad esempio la possibilità di ripristinare in un punto nel tempo. Informazioni sulle funzionalità di ripristino sono disponibili in ogni piattaforma che si sta considerando il è un fattore importante da considerare. |
| Costi | -Se più di una piattaforma può supportare il carico di lavoro di dati, come essi confrontare costo? Ad esempio, se si utilizza ASP.NET Identity, è possibile archiviare i dati del profilo utente nel Database SQL Azure o del servizio tabelle di Azure. Se non è necessario il ricco di funzionalità del Database SQL di una query, è possibile scegliere in parte le tabelle di Azure perché comportano anche costi molto meno, per una determinata quantità di spazio di archiviazione. |

Ciò che è in genere consigliabile conoscere le risposte alle domande in ognuna di queste categorie, prima di scegliere le soluzioni di archiviazione di dati.

Inoltre, il carico di lavoro potrebbe avere requisiti specifici in grado di supportare meglio di altri alcune piattaforme. Ad esempio:

- Funzionalità di controllo richiedono l'applicazione?
- Quali sono i requisiti di durabilità dei dati, è necessario funzionalità di archiviazione o ripulitura automatica?
- Si dispone di sicurezza specifiche esigenze? Ad esempio, i dati includono informazioni personali (informazioni personali), ma è necessario essere in grado di verificare che informazioni personali viene escluso dai risultati della query.
- Se si dispone di alcuni dati non possono essere memorizzati nel cloud per motivi legali o tecnologici, potrebbe essere una piattaforma di cloud di archiviazione di dati che facilita l'integrazione con l'archiviazione locale.

## <a name="demo--using-sql-database-in-azure"></a>Demo: utilizzo del Database SQL in Azure

L'app Correggi utilizza un database relazionale per archiviare le attività. Lo script di Windows PowerShell di creazione ambiente nel [automatizzare tutti gli elementi capitolo](automate-everything.md) vengono create due istanze di Database SQL. Che è possibile visualizzare nel portale, fare clic su di **database SQL** scheda.

![Database SQL nel portale](data-storage-options/_static/image8.png)

Risulta inoltre più semplice creare un database tramite il portale.

Fare clic su **New - Data Services** -- **Database SQL** -- **creazione rapida**, immettere un nome di database, scegliere un server è già presente nel tuo account o crearne uno nuovo e fare clic su **crea Database di SQL**.

![Nuovo database SQL](data-storage-options/_static/image9.png)

Attendere alcuni secondi e si dispone di un database in Azure è pronto per l'utilizzo.

![Nuovo Database SQL creato](data-storage-options/_static/image10.png)

In modo da Azure in pochi secondi ciò che potrebbe richiedere è un giorno o settimana o più tempo per eseguire nell'ambiente locale. E poiché è facilmente possibile creare automaticamente database in uno script o utilizzando un'API di gestione, è possibile in modo dinamico scalare orizzontalmente distribuendo i dati tra più database o: < p >, a condizione che l'applicazione è stato programmato per che. < /o : p >

Questo è un esempio del modello di piattaforma come servizio. Non è necessario gestire i server, si procede. Non è necessario preoccuparsi di backup, si procede. È in esecuzione in high availability-i dati nel database vengono replicati in tre server automaticamente. Se si interrompe una macchina, viene automaticamente il failover e non si perde alcun dato. Non è necessario preoccuparsi che il server viene riparato regolarmente.

Fare clic su un pulsante e ottenere la stringa di connessione esatto, è necessario e possibile iniziare immediatamente a usare il nuovo database.

![Stringhe di connessione](data-storage-options/_static/image11.png)

Il Dashboard Mostra la cronologia delle connessioni e quantità di spazio di archiviazione utilizzato.

![Dashboard del Database SQL](data-storage-options/_static/image12.png)

È possibile gestire i database nel portale o tramite gli strumenti di SQL Server si ha già familiarità con, tra cui SQL Server Management Studio (SSMS) e gli strumenti di Visual Studio Esplora sillaba (SQL Server oggetto SSOX) e a Esplora Server.

![SSOX](data-storage-options/_static/image13.png)

Un altro aspetto interessante è il modello di determinazione dei prezzi. È possibile avviare lo sviluppo con un database gratuito di 20 MB e un database di produzione inizia in corrispondenza di circa 5 al mese. Si paga solo per la quantità di dati che effettivamente archiviati nel database, non la capacità massima. Non è necessario acquistare una licenza.

Database SQL è facili da scalare. Per l'app Correggi, il database che è creare nello script di automazione è limitato a 1 GB. Se si desidera modificarlo in scala fino a 150 GB, è possibile solo passare al portale e modificare questa impostazione o eseguire un comando di API REST, e in secondi è che è possibile distribuire dati in un database di 150 GB.

![Le dimensioni e le edizioni di Database SQL](data-storage-options/_static/image14.png)

Che è la potenza del cloud per spiega come configurare infrastruttura facilmente e rapidamente e iniziare a utilizzare immediatamente.

Correggi l'applicazione utilizza due database SQL, uno per l'appartenenza (autenticazione e autorizzazione) e uno per i dati, e questo è sufficiente per effettuare il provisioning e ridimensionarlo. Illustrato in precedenza come eseguire il provisioning di database tramite script di Windows PowerShell e dopo avere visto anche quanto sia facile eseguire nel portale.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework e accesso diretto al database mediante ADO.NET

L'app Correggi accede a questi database tramite Entity Framework, Microsoft consiglia di ORM (mapping relazionale a oggetti) per le applicazioni .NET. ORM è un ottimo strumento che facilita la produttività degli sviluppatori, ma viene fornito per la produttività a scapito della riduzione delle prestazioni in alcuni scenari. In un'app cloud del mondo reale si non effettua una scelta tra l'uso di Entity Framework o tramite ADO.NET direttamente - userai entrambi. La maggior parte dei casi, quando si scrive codice che funziona con il database, ottenere le massime prestazioni non è critico ed è possibile sfruttare la codifica semplificata e si ottiene con Entity Framework di testing. In situazioni in cui il sovraccarico di EF causerebbe inaccettabile delle prestazioni, è possibile scrivere ed eseguire le query tramite ADO.NET, idealmente chiamando le stored procedure.

Qualsiasi metodo da utilizzare per accedere al database, a cui si desidera ridurre al minimo la "frammentarietà" quanto possibile. In altre parole, se è possibile ottenere tutti i dati che necessari in uno più grande set di risultati query anziché decine o centinaia di quelli più piccoli, che è in genere preferibile. Ad esempio, se è necessario per studenti di elenco e i corsi che sono stati registrati in, è preferibile ottenere tutti i dati nella query di un join anziché recupero studenti in una query e l'esecuzione di query separate per ogni corsi.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Database SQL e Entity Framework in app Correggi

Nell'app Correggi il `FixItContext` classe che deriva da Entity Framework `DbContext` classe, identifica il database e specifica le tabelle nel database. Il contesto specifica un set di entità (tabella) per le attività e il codice passa al contesto del nome stringa di connessione. Questo nome fa riferimento a una stringa di connessione definita nel file Web. config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

La stringa di connessione nel *Web. config* file è denominato appdb (qui che punta al database di sviluppo locale):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Entity Framework consente di creare un *FixItTasks* tabella basata sulle proprietà incluse nel `FixItTask` classe di entità. Si tratta di una classe semplice POCO (Plain oggetto CLR precedente), ovvero non ereditare da o è presente alcuna dipendenza su Entity Framework. Ma Entity Framework sia in grado di creare una tabella basata su di esso e di eseguire le operazioni CRUD (create-lettura-aggiornamento-eliminazione) con esso.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabella FixItTasks](data-storage-options/_static/image15.png)

L'app Correggi include un'interfaccia del repository utilizzata per operazioni CRUD per lavorare con l'archivio dati.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Si noti che i metodi di repository siano tutti async, accesso tutti i dati può essere eseguito in un modo completamente asincrono.

L'implementazione repository chiama i metodi asincroni Entity Framework per funzionare con i dati, tra cui le query LINQ anche come inserimento, aggiornamento ed eliminazione di operazioni. Di seguito è riportato un esempio del codice per la ricerca di un'attività di correzione.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Si noterà che è inoltre disponibile un intervallo e il codice di registrazione errore qui, esamineremo che successivamente nel [capitolo di monitoraggio e telemetria](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Scelta di Database SQL (PaaS) rispetto a SQL Server in una macchina virtuale (IaaS) in Azure

Un aspetto interessante su SQL Server e Database SQL di Azure è che il modello di programmazione di componenti di base per entrambi gli elementi è identico. È possibile utilizzare la maggior parte delle stesse risorse in entrambi gli ambienti. È anche possibile utilizzare un database di SQL Server in fase di sviluppo e di un'istanza del Database SQL nel cloud, ovvero come impostare l'app correggere.

In alternativa, è possibile eseguire lo stesso SQL Server nel cloud che si esegue locale per l'installazione nelle macchine virtuali IaaS. Per alcune applicazioni legacy, una soluzione migliore potrebbe essere in esecuzione SQL Server in una macchina virtuale. Poiché un database di SQL Server viene eseguito in una macchina virtuale dedicata, dispone di più risorse disponibili per il processo rispetto a un database di SQL che viene eseguita in un server condiviso. Pertanto, un database di SQL Server può essere più grande e ancora sono risultate ottimali. In generale, minore sarà le dimensioni di database e tabella, una migliore il caso d'uso funziona per Database SQL (PaaS).

Di seguito sono riportate alcune linee guida su come scegliere tra i due modelli.

| Database SQL di Azure (PaaS) | SQL Server in una macchina virtuale (IaaS) |
| --- | --- |
| **I professionisti** -non è necessario creare o gestire macchine virtuali, aggiornare o applicare la patch OS o SQL. Azure esegue automaticamente. -La disponibilità elevata incorporata, con un contratto di servizio a livello di database. -A basso costo totale di proprietà (TCO) poiché si paga solo per ciò che si usa (non è necessaria alcuna licenza). -Ottimale per la gestione di un numero elevato di database più piccoli (&lt;= 500 GB). -Facili da creare in modo dinamico i nuovi database per abilitare la scalabilità orizzontale. | ***I professionisti*** - funzionalità-compatibile con SQL Server locale. -È possibile implementare SQL Server [la disponibilità elevata tramite AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) in 2 + macchine virtuali, con contratto di servizio a livello di macchina virtuale. -Si dispone di controllo completo sulla modalità di gestione di SQL. -È possibile riutilizzare le licenze SQL già proprietari oppure paga per ora per uno. -Ottimale per la gestione di un numero inferiore ma di dimensioni maggiori (1 TB) i database. |
| **Svantaggi** -alcuni scenari funzionali gap rispetto al Server SQL locale (non dispongono di [integrazione con CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [supporto della compressione](https://technet.microsoft.com/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx)e così via)-limite delle dimensioni di 500 GB. | ***Svantaggi*** : gli aggiornamenti o patch (sistema operativo e SQL) sono le responsabilità del programmatore - creazione e la gestione di database sono le responsabilità del programmatore - IOPS disco (operazioni di input/output al secondo) limitato a circa 8000 (tramite le unità 16 dati). |

Se si desidera utilizzare SQL Server in una macchina virtuale, è possibile utilizzare la propria licenza di SQL Server oppure è possibile pagare una base oraria. Nel portale o tramite l'API REST, ad esempio, è possibile creare una nuova macchina virtuale usando un'immagine di SQL Server.

![Creare una macchina virtuale con SQL Server](data-storage-options/_static/image16.png)

![Elenco di immagini di macchina virtuale SQL Server](data-storage-options/_static/image17.png)

Quando si crea una macchina virtuale con un'immagine di SQL Server, è il Pro-tasso di i costi di licenza di SQL Server per l'ora in base all'utilizzo della macchina virtuale. Se si dispone di un progetto che si limiterà l'esecuzione per un paio di mesi, è più conveniente per pagare per ora. Se si ritiene che il progetto sta per ultimo per anni, è più conveniente acquistare la licenza il modo in cui che normalmente.

## <a name="summary"></a>Riepilogo

Il cloud computing rende utile combinare e gli approcci di archiviazione dati di corrispondenza al meglio le esigenze dell'applicazione. Se si sta creando una nuova applicazione, considerare con attenzione le domande riportate qui per selezionare gli approcci che continuano a funzionare anche quando si espande l'applicazione. Il [capitolo successivo](data-partitioning-strategies.md) illustrerà alcune strategie di partizionamento che consente di combinare più approcci di archiviazione di dati.

## <a name="resources"></a>Risorse

Per altre informazioni, vedere le seguenti risorse.

Scelta di una piattaforma di database:

- [L'accesso ai dati per le soluzioni altamente scalabile: utilizzo di SQL, NoSQL e persistenza Polyglot](http://aka.ms/dag-doc). Consente di archiviare E-book Microsoft Patterns and Practices che entra in modo approfondito i diversi tipi di dati disponibili per le applicazioni cloud.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/ff898430.aspx). Vedere Nozioni di base della coerenza dei dati, la replica dei dati e informazioni aggiuntive di sincronizzazione, il modello di tabella dell'indice, il modello di vista materializzata.
- [BASE: Alternativa Acid](http://queue.acm.org/detail.cfm?id=1394128). L'articolo sui compromessi tra la coerenza dei dati e la scalabilità.
- [Sette database nelle settimane di sette: una Guida per i database moderni e lo spostamento di database NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Libro da Eric Redmond e Jim R. Wilson. Consigliata per l'introduzione di manualmente l'intervallo di piattaforme di archiviazione di dati attualmente disponibili.

Scelta tra SQL Server e Database SQL:

- [Anteprima della versione Premium per materiale sussidiario per Database SQL](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Introduzione a Database SQL Premium e fornite istruzioni su come scegliere tramite il Database SQL Web e Business Edition.
- [Linee guida e limitazioni (Database SQL di Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Pagina del portale che si collega alla documentazione sulle limitazioni del Database SQL, tra cui quella che si concentra sulle funzionalità di SQL Server Database SQL non supporta.
- [SQL Server in macchine virtuali di Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Pagina del portale che si collega alla documentazione sull'esecuzione di SQL Server in Azure.
- [Scott Guthrie spiega i database SQL di Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). 6 minuti video di introduzione al Database SQL da Scott Guthrie.
- [I modelli di applicazione e strategie di sviluppo per SQL Server in macchine virtuali di Azure](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Utilizzo di Entity Framework e il Database SQL in un'app Web ASP.NET

- [Introduzione a Entity Framework 6 con MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Serie di esercitazioni di nove parti che illustra la creazione di un'applicazione MVC che usa Entity Framework e distribuisce il database in Azure e Database SQL.
- [Distribuzione di Web ASP.NET utilizzando Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Parte di dodici serie di esercitazioni che va inserito in maniera più approfondita su come distribuire un database tramite Entity Framework Code First.
- [Distribuire un'app protetta ASP.NET MVC 5 con appartenenza, OAuth e il Database SQL a un sito Web di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). Esercitazione dettagliata che illustra la creazione di un'app web che utilizza l'autenticazione, le tabelle di applicazione sono archiviate nel database delle appartenenze, modifica dello schema del database e distribuisce l'app in Azure.
- [Mappa del contenuto di accesso ai dati ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282414). Collegamenti a risorse per l'utilizzo di Entity Framework e il Database SQL.

Uso di MongoDB in Azure:

- [MongoLab - MongoDB in Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Pagina del portale per la documentazione sull'esecuzione di MongoDB in Azure.
- [Creare un sito web di Azure che si connette a MongoDB in esecuzione in una macchina virtuale in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Esercitazione dettagliata che illustra come utilizzare un database di MongoDB in un'applicazione web ASP.NET.

HDInsight (Hadoop in Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Portale di documentazione di HDInsight sul [Azure](https://azure.microsoft.com/) sito Web.
- [Hadoop e HDInsight: i dati di grandi dimensioni in Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Articolo di MSDN Magazine Bruno Terkaly e Villalobos Ricardo, Introduzione a Hadoop in Azure.
- [Microsoft Patterns and Practices - informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere MapReduce modello.

> [!div class="step-by-step"]
> [Precedente](single-sign-on.md)
> [Successivo](data-partitioning-strategies.md)
