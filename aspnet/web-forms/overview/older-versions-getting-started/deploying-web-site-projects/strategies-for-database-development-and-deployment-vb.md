---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
title: Strategie per lo sviluppo del Database e la distribuzione (VB) | Documenti Microsoft
author: rick-anderson
description: "Quando si distribuisce un'applicazione basati sui dati per la prima volta che è possibile copiare automaticamente il database nell'ambiente di sviluppo all'ambiente di produzione. B..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 07b8905d-78ac-4252-97fb-8675b3fb0bbf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
msc.type: authoredcontent
ms.openlocfilehash: 877056dc74e0b5a64d6e0f11d63ed9f642b0a2cd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="strategies-for-database-development-and-deployment-vb"></a>Strategie per lo sviluppo del Database e la distribuzione (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica il PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_vb.pdf)

> Quando si distribuisce un'applicazione basati sui dati per la prima volta che è possibile copiare automaticamente il database nell'ambiente di sviluppo all'ambiente di produzione. Ma eseguendo una forzata copia nelle distribuzioni successive verrà sovrascritte tutti i dati immessi nel database di produzione. Al contrario, la distribuzione di un database comporta l'applicazione delle modifiche apportate nel database di sviluppo dopo l'ultima distribuzione nel database di produzione. In questa esercitazione vengono esaminati questi problemi e offre diverse strategie per agevolare chronicling e applicando le modifiche apportate al database dopo l'ultima distribuzione.


## <a name="introduction"></a>Introduzione

Come illustrato nelle esercitazioni precedenti, la distribuzione di un'applicazione ASP.NET comporta la copia il contenuto pertinente dall'ambiente di sviluppo all'ambiente di produzione. Distribuzione non è un evento singolo, ma piuttosto un elemento che si verifica ogni volta che viene rilasciata una nuova versione del software o quando i bug o problemi di sicurezza identificati e risolti. Quando si copiano le pagine ASP.NET, immagini, file JavaScript e altri tali file nell'ambiente di produzione che non è necessario preoccuparsi di come questi file sono stati modificati dopo l'ultima distribuzione. È possibile copiare il file ciecamente nell'ambiente di produzione, sovrascrivendo il contenuto esistente. Purtroppo questa semplicità non si estende per la distribuzione del database.

Quando si distribuisce un'applicazione basati sui dati per la prima volta che è possibile copiare automaticamente il database nell'ambiente di sviluppo all'ambiente di produzione. Ma eseguendo una forzata copia nelle distribuzioni successive verrà sovrascritte tutti i dati immessi nel database di produzione. Al contrario, la distribuzione di un database comporta l'applicazione di *modifiche* apportate nel database di sviluppo dopo l'ultima distribuzione nel database di produzione. In questa esercitazione vengono esaminati questi problemi e offre diverse strategie per agevolare chronicling e applicando le modifiche apportate al database dopo l'ultima distribuzione.

## <a name="the-challenges-of-deploying-a-database"></a>I problemi di distribuzione di un Database

Prima di un'applicazione basata sui dati è stata distribuita per la prima volta, è presente un solo database, ovvero il database nell'ambiente di sviluppo, perché quando si distribuisce un'applicazione basati sui dati per la prima volta che è possibile a copiare il database nel ambiente di sviluppo all'ambiente di produzione. Ma una volta distribuito l'applicazione sono presenti due copie del database: uno sviluppo e uno nell'ambiente di produzione.

Tra le distribuzioni, i database di sviluppo e produzione possono risultare non sincronizzati. Mentre lo schema di produzione del database s rimane invariato, lo schema di sviluppo del database s può cambiare dal momento che vengono aggiunte nuove funzionalità. È possibile aggiungere o rimuovere colonne, tabelle, viste o stored procedure. È anche possibile dati importanti che viene aggiunto al database di sviluppo. Molte applicazioni guidate dai dati includono tabelle di ricerca popolate con i dati a livello di codice specifici dell'applicazione che non sono modificabili dall'utente. Ad esempio, un sito Web di vendita all'asta potrebbe avere un elenco di riepilogo a discesa con opzioni che descrivono lo stato dell'elemento da auctioned: nuovo, come nuovo, valida e discreto. Anziché hardcoded queste opzioni direttamente nell'elenco a discesa in genere è preferibile vengono inseriti in una tabella di database. Se, durante lo sviluppo, viene aggiunta una nuova condizione denominata insufficiente per la tabella quindi quando si distribuisce l'applicazione lo stesso record deve essere aggiunto alla tabella di ricerca nel database di produzione.

Idealmente, la distribuzione del database comporta la copia del database dallo sviluppo alla produzione. Tuttavia, tenere presente che dopo aver distribuito l'applicazione e ripresa di sviluppo, il database di produzione viene popolato con dati reali da utenti reali. Pertanto, se si dovesse semplicemente copiare il database dallo sviluppo alla produzione in fase di distribuzione il successiva è Sovrascrivi il database di produzione e perdere i dati esistenti. Il risultato è che il database di distribuzione si riduce per applicare le modifiche apportate nel database di sviluppo dopo l'ultima distribuzione.

Poiché la distribuzione di un database comporta l'applicazione delle modifiche di schema e, possibilmente, i dati dopo l'ultima distribuzione, una cronologia delle modifiche deve essere mantenuta (o determinata in fase di distribuzione) in modo che tali modifiche possono essere applicate in produzione. Esistono diverse tecniche per la gestione e l'applicazione delle modifiche al modello di dati.

### <a name="defining-the-baseline"></a>Definire la linea di base

Per mantenere le modifiche al database dell'applicazione s è necessario disporre di uno stato inizio, in cui le modifiche vengono applicate a una linea di base. A un'estremità lo stato inizio potrebbe essere un database vuoto senza tabelle, viste o stored procedure. Una linea di base di tali risultati in un log di modifica di grandi dimensioni perché deve includere la creazione di tutti i database s tabelle, viste e le stored procedure insieme a tutte le modifiche apportate dopo la distribuzione iniziale. A altra estremità dello spettro è possibile impostare la linea di base come la versione del database che viene inizialmente distribuita nell'ambiente di produzione. Questa scelta comporta un registro delle modifiche molto più piccolo, poiché include solo le modifiche apportate al database dopo la prima distribuzione. Si tratta dell'approccio che come preferito. E naturalmente è possibile scegliere un ulteriori intermedio dell'approccio strada, definire la linea di base come un punto compreso tra la creazione iniziale del database e quando prima della distribuzione del database.

Dopo aver scelto una linea di base si consiglia di generare uno script SQL che può essere eseguito per ricreare la versione di base. Lo script consente di ricreare rapidamente la versione di base del database. Questa funzionalità è particolarmente utile nei progetti di grandi dimensioni, in cui potrebbero essere presenti più sviluppatori che utilizzano il progetto o di altri ambienti, ad esempio test o gestione temporanea, che ogni necessario la propria copia del database.

Esistono una serie di strumenti a disposizione per generare uno script SQL per la versione di base. Da SQL Server Management Studio (SSMS) è possibile fare doppio clic sul database, passare al sottomenu attività e scegliere l'opzione Genera script. Verrà avviata la procedura guidata di Script, che è possibile indicare di generare un file che contiene i comandi SQL per creare il database oggetti s. Un'altra opzione è la pubblicazione guidata Database, che può generare comandi SQL non solo creare lo schema del database, ma anche i dati nelle tabelle del database. La pubblicazione guidata Database è stata esaminata in dettaglio nel *distribuzione di un Database* esercitazione. Indipendentemente dal quale strumento usato, alla fine è necessario disporre di un file script che è possibile utilizzare per ricreare la versione di base del database, in caso di necessità si verificano.

## <a name="documenting-the-database-changes-in-prose"></a>Documentare le modifiche del Database nella Prosa

Il modo più semplice per mantenere un registro delle modifiche al modello di dati durante la fase di sviluppo è per registrare le modifiche nella prosa. Ad esempio, se durante lo sviluppo di un'applicazione già distribuita si aggiunge una nuova colonna per il `Employees` tabella, una colonna da rimuovere il `Orders` tabella e aggiungere una nuova tabella (`ProductCategories`), è necessario gestire un file di testo o di un documento Microsoft Word con la cronologia di seguente:

<a id="0.8_table01"></a>


| **Data di modifica** | **Modificare i dettagli** |
| --- | --- |
| 2009-02-03: | Colonna aggiunta `DepartmentID` (`int`, non NULL) per il `Employees` tabella. Aggiunta di un vincolo di chiave esterna da `Departments.DepartmentID` a `Employees.DepartmentID`. |
| 2009-02-05: | Colonna rimossa `TotalWeight` dal `Orders` tabella. Dati già acquisiti associata `OrderDetails` record. |
| 2009-02-12: | Creazione di `ProductCategories` tabella. Sono presenti tre colonne: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), e `Active` (`bit`, `NOT NULL`). Aggiunta di un vincolo di chiave primaria per `ProductCategoryID`e il valore predefinito 1 per `Active`. |


Esistono numerosi svantaggi di questo approccio. Per iniziare, non vi è alcuna possibilità per l'automazione. In qualsiasi momento tali modifiche devono essere applicato a un database, ad esempio se l'applicazione viene distribuita, uno sviluppatore deve implementare manualmente ogni modifica, uno alla volta. Inoltre, se è necessario ricostruire una particolare versione del database dalla linea di base utilizzando il log di modifica, tale così richiederà più tempo man mano che aumenta le dimensioni del log. Un altro svantaggio di questo metodo è che la chiarezza e il livello di dettaglio di ogni voce di log di modifica viene lasciato all'utente di registrazione della modifica. In un team con più sviluppatori alcuni potrebbe creare voci più dettagliate, più leggibile o più precise rispetto ad altri. Inoltre, sono possibili errori di digitazione e di altri errori di immissione dati umane correlati.

Il vantaggio principale di documentare le modifiche del database nella prosa è la semplicità. Non è conoscenza della necessità t con la sintassi SQL per la creazione e modifica di oggetti di database. In alternativa, è possibile registrare le modifiche nella prosa e distribuirle tramite l'interfaccia utente grafica di SQL Server Management Studio s.

Mantenere il registro delle modifiche nella prosa, è vero, non è molto più sofisticate e acquisite t funzionanti con alcuni progetti, ad esempio quelle che sono di grandi dimensioni nell'ambito, frequenti modifiche al modello di dati o comportano più sviluppatori. Ma ho visto questo approccio funziona bene in progetti di piccole dimensioni, atto che dispongono solo modifiche occasionali al modello di dati e in cui lo sviluppatore solo non dispone di uno sfondo sicuro nella sintassi SQL per la creazione e modifica di oggetti di database.

> [!NOTE]
> Mentre le informazioni nel registro delle modifiche sono tecnicamente, necessario solo fino alla distribuzione di esecuzione, consiglia di mantenere una cronologia delle modifiche. Ma piuttosto che di gestione di un singolo file di log di modifica, in continua crescita considerare la configurazione di un file di log di modifica diversi per ogni versione del database. In genere è consigliabile versione del database ogni volta che viene distribuita. Tramite la gestione di un log di log di modifica è possibile, a partire dalla linea di base, ricreare qualsiasi versione del database eseguendo gli script di log di modifica a partire dalla versione 1 e continuare fino a raggiungere la versione sarà necessario ricrearli.


## <a name="recording-the-sql-change-statements"></a>Registrare le istruzioni di modifica SQL

Lo svantaggio principale di mantenere il registro delle modifiche nella prosa è la mancanza di automazione. In teoria, l'implementazione di modifiche del database al database di produzione in fase di distribuzione sarebbe sufficiente fare clic su un pulsante per eseguire uno script, anziché dover eseguire manualmente un elenco di istruzioni. Questo tipo di automazione è possibile tramite la gestione di un registro delle modifiche che contiene i comandi SQL utilizzati per modificare il modello di dati.

La sintassi SQL include un numero di istruzioni per la creazione e modifica di diversi oggetti di database. Ad esempio, il [ *istruzione CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx), quando eseguita, crea una nuova tabella con le colonne specificate e i vincoli. Il [ *istruzione ALTER TABLE* ](https://msdn.microsoft.com/library/ms190273.aspx) modifica una tabella esistente, aggiungendo, rimuovendo o modificando le colonne o vincoli. Sono inoltre disponibili istruzioni per creare, modificare ed eliminare gli indici, viste, funzioni definite dall'utente, stored procedure, trigger e altri oggetti di database.

Durante lo sviluppo di un'applicazione già distribuita si aggiunge una nuova colonna da restituire riprendendo l'esempio precedente, immagine che la `Employees` tabella, una colonna da rimuovere il `Orders` tabella e aggiungere una nuova tabella (`ProductCategories`). Tali azioni creerebbe un file di log di modifica con i comandi SQL seguenti:

[!code-sql[Main](strategies-for-database-development-and-deployment-vb/samples/sample1.sql)]

Inserimento di queste modifiche al database di produzione in fase di distribuzione è un'operazione di un solo clic: aprire SQL Server Management Studio, connettersi al database di produzione, aprire una finestra Nuova Query, incollare il contenuto del log di modifica e fare clic su Esegui per eseguire lo script.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Utilizzo di uno strumento di confronto per sincronizzare i modelli di dati

Documentare le modifiche del database nella prosa è semplice, ma l'implementazione delle modifiche richiede allo sviluppatore di ogni modifica apportata nel database di produzione, uno alla volta. documentare i comandi SQL di modifica consente di implementare le modifiche nel database di produzione come facilmente e rapidamente con un pulsante, ma richiede l'apprendimento e all'utilizzo di istruzioni SQL e la sintassi per la creazione e modifica di oggetti di database. Strumenti di confronto database prendere le migliori da entrambi gli approcci e ignorare il peggiore.

Uno strumento di confronto di database confronta lo schema o i dati di due database e consente di visualizzare un report di riepilogo che mostra le differenze dei database. Facendo clic su un pulsante, quindi, è possibile generare i comandi SQL per la sincronizzazione di uno o più oggetti di database. In breve, è possibile utilizzare uno strumento di confronto del database per confrontare lo sviluppo e i database di produzione fase di distribuzione, generazione di un file che contiene l'istruzione SQL i comandi che, quando eseguita, le modifiche verranno applicate allo schema del database s di produzione in modo che venga riflette lo schema del database s sviluppo.

Sono disponibili numerosi strumenti di confronto del database di terze parti offerti da numerosi fornitori diversi. Un esempio è [ *SQL Compare*](http://www.red-gate.com/products/SQL_Compare/), da [ *Red Gate Software*](http://www.red-gate.com/). Consente di scorrere il processo di utilizzo SQL Compare per confrontare e sincronizzare gli schemi di database di sviluppo e produzione s.

> [!NOTE]
> Al momento della redazione del presente documento la versione corrente di confronto SQL è stato versione 7.1, con l'edizione Standard costi 395 dollari. È possibile seguire la procedura per scaricare una versione di valutazione gratuita di 14 giorni.


Avvio di SQL Compare verrà visualizzata la finestra di dialogo confronto progetti, che mostra i progetti SQL Compare salvati. Creare un nuovo progetto. Verrà avviata la procedura guidata configurazione di progetto, che richiede informazioni sui database da confrontare (vedere la figura 1). Immettere le informazioni per i database di ambiente di sviluppo e produzione.


[![Confrontare il database di sviluppo e produzione](strategies-for-database-development-and-deployment-vb/_static/image2.jpg)](strategies-for-database-development-and-deployment-vb/_static/image1.jpg)

**Figura 1**: confrontare il database di sviluppo e produzione ([fare clic per visualizzare l'immagine ingrandita](strategies-for-database-development-and-deployment-vb/_static/image3.jpg))


> [!NOTE]
> Se il database di ambiente di sviluppo è un file di database di SQL Express Edition nel `App_Data` cartella del sito Web è necessario registrare il database nel server di database di SQL Server Express per selezionarlo nella finestra di dialogo illustrata nella figura 1. Il modo più semplice a questo scopo è aprire SQL Server Management Studio (SSMS), connettersi al server di database di SQL Server Express e collegare il database. Se non è installato nel computer con SQL Server Management Studio è possibile scaricare e installare gratuitamente [ *versione SQL Server 2008 Management Studio Basic*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Oltre a selezionare i database da confrontare, è possibile specificare anche un'ampia gamma di impostazioni di confronto nella scheda Opzioni. È possibile attivare è "Ignora indice e vincolo nomi." Tenere presente che nell'esercitazione precedente è stato aggiunto che l'applicazione Servizi gli oggetti di database per i database di sviluppo e produzione. Se è stata utilizzata la `aspnet_regsql.exe` strumento per creare questi oggetti nel database di produzione, quindi si noterà che la chiave primaria e i nomi di vincolo unique differiscono tra i database di sviluppo e produzione. Di conseguenza, il confronto di SQL contrassegnerà tutte le tabelle di servizi di applicazione come diverso. È possibile lasciare i "Ignora indice e vincolo nomi" non è selezionata e sincronizzare i nomi di vincolo o indicare SQL Compare per ignorare le differenze.

Dopo aver selezionato i database da confrontare e verificare le opzioni di confronto, fare clic sul pulsante confrontare ora per iniziare il confronto. Nel successivi alcuni secondi, SQL Compare esamina gli schemi dei due database e genera un report delle differenze. Si sposta intenzionalmente apportate alcune modifiche nel database di sviluppo per mostrare come tali differenze vengono indicate nell'interfaccia di confronto SQL. Come illustrato nella figura 2, si viene aggiunto un `BirthDate` colonna il `Authors` tabella, rimuovere il `ISBN` colonna dal `Books` tabella e aggiungere una nuova tabella, `Ratings`, che è progettato per consentire agli utenti che visitano la velocità del sito i libri di revisionati.

> [!NOTE]
> Le modifiche apportate in questa esercitazione del modello di dati sono state eseguite per illustrare l'utilizzo di uno strumento di confronto del database. Queste modifiche nel database si troverà non in esercitazioni future.


[![Confronto SQL sono elencate le differenze tra lo sviluppo e il database di produzione](strategies-for-database-development-and-deployment-vb/_static/image5.jpg)](strategies-for-database-development-and-deployment-vb/_static/image4.jpg)

**Figura 2**: confronto SQL sono elencate le differenze tra il database di sviluppo e produzione ([fare clic per visualizzare l'immagine ingrandita](strategies-for-database-development-and-deployment-vb/_static/image6.jpg))


Confrontare SQL suddivide gli oggetti di database in gruppi, rapidamente che mostra gli oggetti che esistono in entrambi i database, ma sono diverso, gli oggetti esistenti in un database ma non in altro e gli oggetti sono identici. Come si può notare, sono presenti due oggetti che esistono in entrambi i database, ma sono diverse: la `Authors` tabella, che include una colonna aggiunta, e `Books` tabella, che ha rimosso. Non è presente un oggetto che esiste solo nel database di sviluppo, vale a dire l'oggetto appena creato `Ratings` tabella. E sono presenti 117 oggetti sono identici in entrambi i database.

Selezione di un oggetto di database visualizza la finestra differenze SQL, che mostra le differenziano di questi oggetti. Nella finestra di differenze SQL, nella parte inferiore nella figura 2, vengono evidenziati che il `Authors` tabella nel database di sviluppo è il `BirthDate` colonna, non viene trovato nel `Authors` tabella nel database di produzione.

Dopo aver esaminato le differenze e selezionare gli oggetti da sincronizzare, il passaggio successivo consiste nel generare comandi SQL necessari per aggiornare lo schema di produzione del database s corrisponda al database di sviluppo. Questa operazione viene eseguita tramite la procedura guidata della sincronizzazione. La configurazione guidata sincronizzazione conferma quali oggetti per la sincronizzazione e riepiloga l'azione pianificare (vedere la figura 3). È possibile sincronizzare i database immediatamente o generare uno script con i comandi SQL che può essere eseguita in base alle esigenze specifiche.


[![Utilizzare la procedura guidata della sincronizzazione per sincronizzare gli schemi di database](strategies-for-database-development-and-deployment-vb/_static/image8.jpg)](strategies-for-database-development-and-deployment-vb/_static/image7.jpg)

**Figura 3**: utilizzare la procedura guidata della sincronizzazione per sincronizzare il database schemi ([fare clic per visualizzare l'immagine ingrandita](strategies-for-database-development-and-deployment-vb/_static/image9.jpg))


Strumenti di confronto del database, ad esempio Red Gate Software s SQL Compare rendere l'applicazione delle modifiche allo schema del database di sviluppo nel database di produzione semplice come e fare clic su.

> [!NOTE]
> SQL Compare Confronta e consente di sincronizzare due database *schemi*. Purtroppo, non confrontare e sincronizzare i dati all'interno delle tabelle di due database. Red Gate Software offre un prodotto denominato [ *confronto dati SQL* ](http://www.red-gate.com/products/SQL_Data_Compare/) che confronta e sincronizzazione dei dati tra due database, ma è un prodotto distinto da SQL Compare e costa $395 un'altra.


## <a name="taking-the-application-offline-during-deployment"></a>Utilizzo dell'applicazione durante la distribuzione

Come si sposta in queste esercitazioni, distribuzione è un processo che prevede più passaggi: copiare le pagine ASP.NET, pagine master, CSS file, JavaScript file, immagini e altri contenuti necessari dall'ambiente di sviluppo alla produzione ambiente. copia di backup delle informazioni di configurazione specifici dell'ambiente di produzione, se necessaria. e applica le modifiche al modello di dati dopo l'ultima distribuzione. In base al numero di file e della complessità delle modifiche apportate al database, questi passaggi possono richiedere da alcuni secondi a diversi minuti per completare. Durante questo periodo l'applicazione web è in continuo mutamento, e gli utenti che visitano il sito può essere soggetta a errori o comportamenti imprevisti.

Quando si distribuisce un sito Web è migliore rendere l'applicazione web "offline" fino al termine della distribuzione. Portare l'applicazione offline (e attivare la modalità di backup al termine del processo di distribuzione) è sufficiente caricare un file e quindi eliminarlo. A partire da ASP.NET 2.0, la presenza di un file denominato `app_offline.htm` in s applicazione directory radice accetta l'intero sito Web "offline". Qualsiasi richiesta per una pagina ASP.NET in tale sito viene automaticamente risposto con il contenuto del `app_offline.htm` file. Dopo la rimozione di file, l'applicazione torna online.

Esecuzione di un'applicazione durante la distribuzione, quindi, è semplice come caricare un `app_offline.htm` file nell'ambiente di produzione s directory prima di iniziare il processo di distribuzione e quindi eliminarlo (o rinominare quello) radice distribuzione una volta è stata completata. Per ulteriori informazioni su questa tecnica, fare riferimento all'articolo s John Peterson, richiede un [ *applicazione ASP.NET non in linea*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Riepilogo

La sfida principale nella distribuzione di un'applicazione basata sui dati è incentrata sui database di distribuzione. Poiché esistono due versioni del database, uno nell'ambiente di sviluppo e uno nell'ambiente di produzione questi schemi di due database possono risultare non sincronizzati con l'aggiunta di nuove funzionalità in fase di sviluppo. Novità più, perché il database di produzione come popolati con dati reali da utenti reali, come quando si distribuisce i file che costituiscono l'applicazione (le pagine ASP.NET, non è possibile sovrascrivere i database di produzione del database di sviluppo modificate file di immagine e così via). Distribuzione di un database, invece, comporta che implementa lo specifico set di modifiche apportate al database di sviluppo del database di produzione dopo l'ultima distribuzione.

In questa esercitazione è esaminato e tre le tecniche per la gestione e l'applicazione di un log delle modifiche del database. L'approccio più semplice è per registrare le modifiche nella prosa. Durante questa strategia consente l'implementazione di queste modifiche nel database di produzione di un processo manuale, non richiede conoscenze di comandi SQL per la creazione e modifica di oggetti di database. Un approccio più sofisticato e uno che è molto più appetibile in progetti o progetti di grandi dimensioni con più sviluppatori, consiste nel registrare le modifiche come una serie di comandi SQL. Questo hastens notevolmente l'implementazione di queste modifiche al database di destinazione. Il meglio di entrambi gli approcci può essere ottenuto utilizzando uno strumento di confronto del database, ad esempio Red Gate Software s SQL Compare.

Questa esercitazione si conclude l'attenzione sulla distribuzione di un'applicazione basati sui dati. Il set successivo di esercitazioni viene descritto come rispondere agli errori nell'ambiente di produzione. Verrà esaminato come visualizzare una pagina di errore descrittivo piuttosto anziché la schermata di morte giallo. E vedremo come registrare i dettagli dell'errore s e come ricevere un avviso quando si verificano tali errori.

Buona programmazione!

>[!div class="step-by-step"]
[Precedente](configuring-a-website-that-uses-application-services-vb.md)
[Successivo](displaying-a-custom-error-page-vb.md)
