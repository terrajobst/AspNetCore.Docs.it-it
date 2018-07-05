---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
title: Strategie per lo sviluppo di Database e la distribuzione (c#) | Microsoft Docs
author: rick-anderson
description: Quando si distribuisce un'applicazione basata su dati per la prima volta che è possibile copiare il database alla cieca nell'ambiente di sviluppo all'ambiente di produzione. B....
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 3e8b0627-3eb7-488e-807e-067cba7cec05
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
msc.type: authoredcontent
ms.openlocfilehash: 98ba771f6eafad84303279045e5b5c4a167c0678
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371931"
---
<a name="strategies-for-database-development-and-deployment-c"></a>Strategie per lo sviluppo di Database e la distribuzione (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_cs.pdf)

> Quando si distribuisce un'applicazione basata su dati per la prima volta che è possibile copiare il database alla cieca nell'ambiente di sviluppo all'ambiente di produzione. Ma esegue una forzata copia nelle distribuzioni successive sovrascriverà tutti i dati immessi nel database di produzione. Al contrario, la distribuzione di un database comporta applicando le modifiche apportate al database di sviluppo dopo l'ultima distribuzione nel database di produzione. Questa esercitazione esamina questi problemi e offre diverse strategie per agevolare chronicling e applicare le modifiche apportate al database dopo l'ultima distribuzione.


## <a name="introduction"></a>Introduzione

Come illustrato nelle esercitazioni precedenti, la distribuzione di un'applicazione ASP.NET comporta la copia il contenuto pertinente dall'ambiente di sviluppo all'ambiente di produzione. La distribuzione non è un evento singolo, ma piuttosto un elemento che si verifica ogni volta che viene rilasciata una nuova versione del software oppure quando i bug o problemi di sicurezza sono stati identificati e risolti. Quando si copiano le pagine ASP.NET, immagini, file JavaScript e altri tali file nell'ambiente di produzione che non è necessario occuparsi di questi file sono stati modificati dopo l'ultima distribuzione. È possibile copiare il file alla cieca nell'ambiente di produzione, sovrascrivendo il contenuto esistente. Sfortunatamente, questa semplicità non si estende alla distribuzione del database.

Quando si distribuisce un'applicazione basata su dati per la prima volta che è possibile copiare il database alla cieca nell'ambiente di sviluppo all'ambiente di produzione. Ma esegue una forzata copia nelle distribuzioni successive sovrascriverà tutti i dati immessi nel database di produzione. Al contrario, la distribuzione di un database consiste nell'applicare il *modifiche* apportate al database di sviluppo dopo l'ultima distribuzione nel database di produzione. Questa esercitazione esamina questi problemi e offre diverse strategie per agevolare chronicling e applicare le modifiche apportate al database dopo l'ultima distribuzione.

## <a name="the-challenges-of-deploying-a-database"></a>Le sfide della distribuzione di un Database

Prima di aver distribuita un'applicazione basata su dati per la prima volta, è presente un solo database, vale a dire il database nell'ambiente di sviluppo, motivo per cui quando si distribuisce un'applicazione basata su dati per la prima volta alla cieca è possibile copiare il database di ambiente di sviluppo all'ambiente di produzione. Ma dopo aver distribuita l'applicazione sono disponibili due copie del database: uno sviluppo e uno nell'ambiente di produzione.

Tra le distribuzioni possono diventare non sincronizzati i database di sviluppo e produzione. Mentre lo schema del database s rimane invariato, lo schema di sviluppo del database s può cambiare quando vengono aggiunte nuove funzionalità. È possibile aggiungere o rimuovere colonne, tabelle, viste o stored procedure. Potrebbero inoltre essere presenti dati importanti che viene aggiunto al database di sviluppo. Molte applicazioni guidate dai dati includono tabelle di ricerca popolate con i dati a livello di codice specifici dell'applicazione che non sono modificabili dall'utente. Ad esempio, un sito Web delle aste potrebbe avere un elenco di riepilogo a discesa con le opzioni che descrivono la condizione dell'elemento da auctioned: nuovo, come nuova, buona e Fair. Invece di queste opzioni direttamente nell'elenco a discesa a livello di codice è in genere preferibile inserirli in una tabella di database. Se, durante lo sviluppo, viene aggiunta una nuova condizione denominata Poor alla tabella quindi quando si distribuisce l'applicazione lo stesso record dovrà essere aggiunta alla tabella di ricerca nel database di produzione.

In teoria, distribuzione del database comporta la copia del database dallo sviluppo alla produzione. Tuttavia, tenere presente che dopo aver distribuito l'applicazione e ripreso lo sviluppo, il database di produzione viene popolato con dati reali da utenti reali. Pertanto, se si intende semplicemente copiare il database dallo sviluppo alla produzione alla distribuzione successiva si Sovrascrivi il database di produzione e perdere i dati esistenti. Il risultato finale è che il database di distribuzione si riduce per applicare le modifiche apportate al database di sviluppo dopo l'ultima distribuzione.

Poiché la distribuzione di un database comporta l'applicazione delle modifiche di schema e, possibilmente, i dati dopo l'ultima distribuzione, una cronologia delle modifiche deve essere mantenuta (o determinata in fase di distribuzione) in modo che tali modifiche possono essere applicate in produzione. Esistono varie tecniche per la gestione e applicare le modifiche al modello di dati.

### <a name="defining-the-baseline"></a>Definire la linea di base

Per mantenere le modifiche al database dell'applicazione s è necessario avere uno stato inizio, in cui le modifiche vengono applicate a una linea di base. Da una parte lo stato inizio può essere un database vuoto con alcun tabelle, viste o stored procedure. Una linea di base di tali risultati in un log di modifica di grandi dimensioni perché deve includere la creazione di tutte le tabelle di database s, viste e stored procedure insieme a tutte le modifiche apportate dopo la distribuzione iniziale. A altra estremità dello spettro è possibile impostare la linea di base come la versione del database in cui viene inizialmente distribuita nell'ambiente di produzione. Questa scelta comporta un log delle modifiche molto più piccolo perché include solo le modifiche apportate al database dopo la prima distribuzione. Questo è l'approccio che è preferibile. E naturalmente è possibile scegliere un più intermedio dell'approccio strada, che definisce la linea di base come un certo punto tra la creazione iniziale del database e quando il database viene innanzitutto distribuito.

Dopo aver scelto una linea di base si consiglia di generare uno script SQL che può essere eseguito per ricreare la versione di base. Tale script rende possibile ricreare rapidamente la versione di base del database. Questa funzionalità è particolarmente utile nei progetti di grandi dimensioni, cui possono essere presenti più sviluppatori che lavorano nel progetto o altri ambienti, ad esempio test o gestione temporanea, che ogni necessario la propria copia del database.

Sono disponibili un'ampia gamma di strumenti di ìexchange per generare uno script SQL per la versione di base. Da SQL Server Management Studio (SSMS) è possibile fare doppio clic sul database, passare il sottomenu di attività e scegliere l'opzione Genera script. Verrà avviata la procedura guidata dello Script, che è possibile indicare a generare un file che contiene i comandi SQL per creare il database con oggetti di s. Un'altra opzione è la pubblicazione guidata Database, che può generare i comandi SQL non soltanto di creare lo schema del database, ma anche i dati nelle tabelle del database. La pubblicazione guidata Database è stata esaminata in dettaglio nel *distribuzione di un Database* esercitazione. Indipendentemente dal quale strumento usato, alla fine è necessario disporre di un file script che è possibile utilizzare per ricreare la versione di base del database, in caso di necessità si verificano.

## <a name="documenting-the-database-changes-in-prose"></a>Documentare le modifiche del Database nella Prosa

Il modo più semplice per mantenere un registro delle modifiche al modello di dati durante la fase di sviluppo è per registrare le modifiche nella prosa. Ad esempio, se durante lo sviluppo di un'applicazione già distribuita si aggiunge una nuova colonna per il `Employees` tabella, rimuovere una colonna dal `Orders` tabella e aggiungere una nuova tabella (`ProductCategories`), è necessario mantenere un file di testo o un documento Microsoft Word con la cronologia di seguente:

<a id="0.4_table01"></a>


| **Data modifica** | **Dettagli delle modifiche** |
| --- | --- |
| 2009-02-03: | Colonna aggiunta `DepartmentID` (`int`, non NULL) per il `Employees` tabella. Aggiungere un vincolo foreign key dalla `Departments.DepartmentID` a `Employees.DepartmentID`. |
| 2009-02-05: | Colonna rimossa `TotalWeight` dal `Orders` tabella. Associati a dati già acquisiti in `OrderDetails` record. |
| 2009-02-12: | Creazione di `ProductCategories` tabella. Esistono tre colonne: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), e `Active` (`bit`, `NOT NULL`). Aggiunto a un vincolo di chiave primaria `ProductCategoryID`e il valore predefinito 1 per `Active`. |


Esistono una serie di svantaggi di questo approccio. Prima di tutto, non vi è alcuna speranza per l'automazione. In qualsiasi momento queste modifiche devono essere applicati a un database -, ad esempio quando l'applicazione viene distribuita, uno sviluppatore deve implementare manualmente ogni modifica, uno alla volta. Inoltre, se è necessario ricostruire una particolare versione del database dalla linea di base usando il log delle modifiche, tale operazione così richiederà più tempo man mano che aumenta le dimensioni del log. Un altro svantaggio di questo metodo è che la chiarezza e a livello di dettaglio di ogni voce del registro modifiche è lasciato al rappresentante la registrazione della modifica. In un team con più sviluppatori alcuni potrebbe rendere le voci più dettagliate, più leggibile o più precise rispetto ad altri. Inoltre, errori di digitazione e altri errori di immissione dati umane correlati sono possibili.

Il vantaggio principale di come documentare le modifiche del database nella prosa è la semplicità. Non è conoscenza della necessità t la sintassi SQL per la creazione e modifica di oggetti di database. In alternativa, è possibile registrare le modifiche nella prosa e implementarli tramite interfaccia utente grafica di SQL Server Management Studio s.

Gestione del log delle modifiche nella prosa, è vero, non è molto sofisticati e vinti t funzionino bene con determinati progetti, ad esempio quelle che sono di grandi dimensioni nell'ambito, frequenti modifiche al modello di dati o implicano più sviluppatori. Ma ho visto questo approccio funziona piuttosto bene nei progetti di piccole dimensioni, atto che hanno solo modifiche occasionali al modello di dati e dove l'ideali per singoli sviluppatori non ha uno sfondo importante nella sintassi SQL per la creazione e modifica di oggetti di database.

> [!NOTE]
> Sebbene le informazioni nel log delle modifiche, tecnicamente, necessari solo fino alla fase di distribuzione, consigliabile mantenere una cronologia delle modifiche. Ma anziché mantenere un singolo, in continua crescita di file di log delle modifiche, è consigliabile che un file di log di modifica diversi per ogni versione del database. In genere è opportuno alla versione del database ogni volta che viene distribuita. Tramite la gestione di un log di log delle modifiche è possibile, a partire dalla linea di base, ricreare tutte le versioni di database eseguendo gli script di log delle modifiche a partire dalla versione 1 e continuando fino a raggiungere la versione è necessario ricreare.


## <a name="recording-the-sql-change-statements"></a>Registrare le istruzioni di modifica SQL

Lo svantaggio principale di mantenere il log delle modifiche nella prosa è la mancanza di automazione. In teoria, implementazione delle modifiche del database al database di produzione in fase di distribuzione sarebbe sufficiente fare clic su un pulsante per eseguire uno script piuttosto che dover eseguire manualmente un elenco di istruzioni. Tale automazione è possibile tramite la gestione di un log delle modifiche che contiene i comandi SQL consentono di modificare il modello di dati.

La sintassi SQL include un numero di istruzioni per la creazione e modifica di vari oggetti di database. Ad esempio, il [ *istruzione CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx), quando eseguita, crea una nuova tabella con le colonne specificate e i vincoli. Il [ *istruzione ALTER TABLE* ](https://msdn.microsoft.com/library/ms190273.aspx) modifica una tabella esistente, aggiungendo, rimuovendo o modificando le colonne o vincoli. Sono inoltre disponibili istruzioni per creare, modificare ed eliminare indici, viste, funzioni definite dall'utente, stored procedure, trigger e altri oggetti di database.

Ritornando all'esempio precedente precedenti, di immagine che durante lo sviluppo di un'applicazione già distribuita si aggiunge una nuova colonna per il `Employees` tabella, una colonna da rimuovere il `Orders` di tabella e aggiungere una nuova tabella (`ProductCategories`). Azioni di questo tipo comporterebbe un file di log delle modifiche con i comandi SQL seguenti:

[!code-sql[Main](strategies-for-database-development-and-deployment-cs/samples/sample1.sql)]

Eseguire il push di queste modifiche al database di produzione in fase di distribuzione è un'operazione con un clic: aprire SQL Server Management Studio, connettersi al database di produzione, aprire una finestra Nuova Query, incollare il contenuto del log delle modifiche e fare clic su Esegui per eseguire lo script.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Usa uno strumento di confronto per sincronizzare i modelli di dati

La documentazione delle modifiche di database nella prosa è facile, ma l'implementazione delle modifiche richiede agli sviluppatori di ogni modifica apportata nel database di produzione uno alla volta. documentare i comandi SQL di modifica consente di implementare tali modifiche nel database di produzione più semplice e rapido un clic, ma richiede l'apprendimento e all'utilizzo di istruzioni SQL e sintassi per la creazione e modifica di oggetti di database. Strumenti di confronto database sfruttare il meglio di entrambi gli approcci e ignorare il peggiore.

Uno strumento di confronto di database confronta lo schema o i dati di due database e consente di visualizzare un report di riepilogo che mostra le differenze dei database. Quindi, facendo clic su un pulsante, è possibile generare i comandi SQL per la sincronizzazione di una o più oggetti di database. In breve, è possibile usare uno strumento di confronto del database da confrontare lo sviluppo e i database di produzione in fase di distribuzione, generazione di un file che contiene il codice SQL comandi che, quando eseguita, le modifiche verranno applicate per lo schema del database s in modo che l'it identico a quello dello schema del database s di sviluppo.

Sono disponibili numerosi strumenti di confronto del database di terze parti offerte da molti fornitori diversi. È uno di questi esempi [ *SQL Compare*](http://www.red-gate.com/products/SQL_Compare/), da [ *Red Gate Software*](http://www.red-gate.com/). Let s in dettaglio il processo di utilizzo SQL Compare per confrontare e sincronizzare gli schemi di database di sviluppo e produzione.

> [!NOTE]
> Al momento della stesura di questo articolo la versione corrente di SQL Compare era versione 7.1, con l'edizione Standard a partire da 395 dollari un calcolo dei costi. È possibile seguire la procedura, scaricare una versione di valutazione gratuita di 14 giorni.


All'avvio di SQL Compare apre la finestra di dialogo confronto tra progetti, che mostra i progetti SQL Compare salvati. Creare un nuovo progetto. Verrà avviata la procedura guidata configurazione del progetto, che richiede informazioni sul database per il confronto (vedere la figura 1). Immettere le informazioni per i database ambiente di sviluppo e produzione.


[![Confrontare il database di sviluppo e produzione](strategies-for-database-development-and-deployment-cs/_static/image2.jpg)](strategies-for-database-development-and-deployment-cs/_static/image1.jpg)

**Figura 1**: confrontare il database di sviluppo e produzione ([fare clic per visualizzare l'immagine con dimensioni normali](strategies-for-database-development-and-deployment-cs/_static/image3.jpg))


> [!NOTE]
> Se il database ambiente di sviluppo è un file di database di SQL Express Edition nel `App_Data` cartella del sito Web è necessario registrare il database nel server di database SQL Server Express per selezionarlo dalla finestra di dialogo illustrata nella figura 1. Il modo più semplice per farlo è aprire SQL Server Management Studio (SSMS), connettersi al server di database SQL Server Express e collegare il database. Se non è installato nel computer SQL Server Management Studio è possibile scaricare e installare la versione gratuita [ *versione di SQL Server 2008 Management Studio Basic*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Oltre a selezionare i database da confrontare, è possibile specificare anche un'ampia gamma di impostazioni di confronto nella scheda Opzioni. È possibile attivare è "Ignora indice e vincolo nomi." È importante ricordare che nell'esercitazione precedente è stato aggiunto che l'applicazione di servizi gli oggetti di database ai database di sviluppo e produzione. Se è stato usato il `aspnet_regsql.exe` dello strumento per creare questi oggetti nel database di produzione sarà presente che i nomi di vincolo unique e chiave primaria sono diversi tra i database di sviluppo e produzione. Di conseguenza, SQL Compare contrassegnerà tutte le tabelle di servizi di applicazione come diverse. È possibile lasciare i "Ignora indice e vincolo nomi" è deselezionata e sincronizzare i nomi di vincolo o indicare a SQL Compare per ignorare queste differenze.

Dopo aver selezionato i database da confrontare e verificare le opzioni di confronto, fare clic sul pulsante confrontare adesso per iniziare il confronto. Per i secondi diversi successivi e SQL Compare esamina gli schemi dei due database e genera un report delle relative differenze. Ho ve intenzionalmente apportate alcune modifiche al database di sviluppo per mostrare come tali differenze sono indicate nell'interfaccia di confronto SQL. Come illustrato nella figura 2, si va aggiunto un `BirthDate` colonna per il `Authors` tabella, rimuovere il `ISBN` colonna dal `Books` tabella e aggiungere una nuova tabella, `Ratings`, che è progettato per consentire agli utenti che visitano la velocità del sito i libri riviste.

> [!NOTE]
> Le modifiche al modello di dati apportate in questa esercitazione sono stati eseguiti per illustrare l'utilizzo di uno strumento di confronto del database. Queste modifiche nel database si troverà non in esercitazioni future.


[![Confronto SQL sono elencate le differenze tra lo sviluppo e i database di produzione](strategies-for-database-development-and-deployment-cs/_static/image5.jpg)](strategies-for-database-development-and-deployment-cs/_static/image4.jpg)

**Figura 2**: confronto SQL sono elencate le differenze tra sviluppo e i database di produzione ([fare clic per visualizzare l'immagine con dimensioni normali](strategies-for-database-development-and-deployment-cs/_static/image6.jpg))


SQL Compare suddivide gli oggetti di database in gruppi, mostrare rapidamente gli oggetti che esistono in entrambi i database ma sono diversi, gli oggetti esistenti in un database ma non in altra e gli oggetti che sono identici. Come può notare, sono presenti due oggetti presenti in entrambi i database, ma sono diversi: il `Authors` tabella che dispone di una colonna aggiunta, e il `Books` tabella, che ha colpito all'improvviso rimosso. È presente un oggetto che esiste solo nel database di sviluppo, vale a dire l'oggetto appena creato `Ratings` tabella. E sono presenti 117 oggetti sono identici in entrambi i database.

Selezione di un oggetto di database consente di visualizzare la finestra delle differenze di SQL in cui viene illustrato come questi oggetti sono diversi. Nella finestra delle differenze di SQL, nella parte inferiore nella figura 2, vengono evidenziati che il `Authors` tabella nel database di sviluppo ha la `BirthDate` colonna, che non viene trovato nel `Authors` tabella nel database di produzione.

Dopo aver esaminato le differenze e selezionare gli oggetti da sincronizzare, il successivo passaggio consiste nel generare i comandi SQL necessari per aggiornare lo schema del database s in modo che corrisponda il database di sviluppo. Questa operazione viene eseguita tramite la configurazione guidata sincronizzazione. La configurazione guidata sincronizzazione conferma di ciò che gli oggetti per la sincronizzazione e riepiloga l'azione di pianificazione (vedere la figura 3). È possibile sincronizzare i database immediatamente o generare uno script con i comandi SQL che può essere eseguito in base alle esigenze specifiche.


[![Usare la configurazione guidata sincronizzazione per sincronizzare gli schemi di database](strategies-for-database-development-and-deployment-cs/_static/image8.jpg)](strategies-for-database-development-and-deployment-cs/_static/image7.jpg)

**Figura 3**: usare la configurazione guidata sincronizzazione per sincronizzare Your gli schemi di database ([fare clic per visualizzare l'immagine con dimensioni normali](strategies-for-database-development-and-deployment-cs/_static/image9.jpg))


Strumenti di confronto del database, come Red Gate Software s SQL Compare assicurarsi di aver applicato le modifiche allo schema del database di sviluppo nel database di produzione semplice come puntare e fare clic su.

> [!NOTE]
> SQL Compare Confronta e consente di sincronizzare due database *schemi*. Sfortunatamente, non confrontare e sincronizzare i dati all'interno delle tabelle di due database. Red Gate Software offre un prodotto denominato [ *confronto dati SQL* ](http://www.red-gate.com/products/SQL_Data_Compare/) che confronta e sincronizza i dati tra due database, ma è un prodotto distinto da SQL Compare e i costi di un altro 395 dollari.


## <a name="taking-the-application-offline-during-deployment"></a>Portare Offline l'applicazione durante la distribuzione

Come è illustrata in queste esercitazioni, ve distribuzione è un processo che prevede più passaggi: copiare le pagine ASP.NET, pagine master, CSS i file, JavaScript file, immagini e altri contenuti necessari dall'ambiente di sviluppo alla produzione ambiente. copiare il backup delle informazioni di configurazione specifici dell'ambiente di produzione, se necessario; e applicare le modifiche al modello di dati dopo l'ultima distribuzione. A seconda del numero di file e la complessità del database viene modificato, questa procedura può richiedere da alcuni secondi per alcuni minuti per completare. Durante questa finestra è in continuo mutamento, l'applicazione web e gli utenti che visitano il sito potrebbero verificarsi errori o comportamenti imprevisti.

Quando si distribuisce un sito Web è migliore portare l'applicazione web "offline" fino a quando non è stata completata la distribuzione. Portare offline l'applicazione (e portandola eseguire il backup al termine del processo di distribuzione) è sufficiente caricare un file e quindi eliminarlo. A partire da ASP.NET 2.0, la presenza di un file denominato `app_offline.htm` s l'applicazione directory radice viene portata offline l'intero sito Web "." Qualsiasi richiesta inviata a una pagina ASP.NET nel sito in questione viene risposto automaticamente con il contenuto del `app_offline.htm` file. Una volta che tale file viene rimosso, l'applicazione torna online.

Acquisire un'applicazione in modalità offline durante la distribuzione, quindi, è semplice come caricare un `app_offline.htm` file nell'ambiente di produzione s directory prima di iniziare il processo di distribuzione e quindi eliminarlo (o rinominarlo per qualcos'altro) radice distribuzione una sola volta è stata completata. Per altre informazioni su questa tecnica, vedere l'articolo di John Peterson s, accettando un [ *ASP.NET dell'applicazione non in linea*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Riepilogo

La sfida principale nella distribuzione di un'applicazione basata su dati è incentrata sui database di distribuzione. Poiché sono disponibili due versioni del database, uno nell'ambiente di sviluppo e uno nell'ambiente di produzione questi schemi di due database possono diventare non sincronizzati con l'aggiunta di nuove funzionalità in fase di sviluppo. Quali altre, perché il database di produzione come viene popolato con dati reali da utenti reali, è possibile sovrascrivere il database di produzione del database di sviluppo modificate come è possibile quando si distribuiscono i file che costituiscono l'applicazione (le pagine ASP.NET, s i file di immagine e così via). Al contrario, la distribuzione di un database comporta l'uso che implementa lo specifico set di modifiche apportate al database di sviluppo nel database di produzione dopo l'ultima distribuzione.

Questa esercitazione ha illustrato le tre tecniche per la manutenzione e applicazione di un log delle modifiche del database. L'approccio più semplice è per registrare le modifiche nella prosa. Sebbene questa strategia consente di implementare queste modifiche nel database di produzione un processo manuale, non richiede conoscenze dei comandi SQL per la creazione e modifica di oggetti di database. Un approccio più sofisticato e uno che è molto più gradevole nei progetti o i progetti più grandi con più sviluppatori, consiste nel registrare le modifiche come una serie di comandi SQL. Questa opzione notevolmente l'implementazione di queste modifiche al database di destinazione. Il meglio di entrambi gli approcci può essere ottenuto utilizzando uno strumento di confronto del database, ad esempio Red Gate Software s SQL Compare.

Questa esercitazione si conclude incentrato sulla distribuzione di un'applicazione basata su dati. Il set successivo di esercitazioni viene descritto come rispondere agli errori nell'ambiente di produzione. Esamineremo come visualizzare una pagina di errore descrittivo piuttosto anziché la schermata di morte giallo. E vedremo come registrare i dettagli dell'errore s e su come ricevere un avviso quando si verificano tali errori.

Buona programmazione!

> [!div class="step-by-step"]
> [Precedente](configuring-a-website-that-uses-application-services-cs.md)
> [Successivo](displaying-a-custom-error-page-cs.md)
