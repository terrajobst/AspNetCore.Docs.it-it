---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
title: Creazione di Stored procedure e funzioni definite dall'utente con il codice (VB) gestito | Microsoft Docs
author: rick-anderson
description: Microsoft SQL Server 2005 si integra con .NET Common Language Runtime per consentire agli sviluppatori di creare oggetti di database tramite codice gestito. Questa esercitazione...
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: 8be9a51b-ea6b-46c7-bfa2-476d9b14c24c
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
msc.type: authoredcontent
ms.openlocfilehash: dcbd55185e7d5e3910b4f74f11554bae100057cc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806167"
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-vb"></a>Creazione di Stored procedure e funzioni definite dall'utente con codice gestito (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_VB.zip) o [Scarica il PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/datatutorial75vb1.pdf)

> Microsoft SQL Server 2005 si integra con .NET Common Language Runtime per consentire agli sviluppatori di creare oggetti di database tramite codice gestito. Questa esercitazione viene illustrato come creare le stored procedure gestite e gestite a funzioni definite dall'utente con il codice Visual Basic o c#. Noteremo anche come queste edizioni di Visual Studio consentono di eseguire il debug di tali oggetti di database gestito.


## <a name="introduction"></a>Introduzione

Database, come s Microsoft SQL Server 2005 utilizzano il [Transact-Structured Query Language (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) per inserimento, la modifica e il recupero dei dati. La maggior parte dei sistemi di database includono costrutti per raggruppare una serie di istruzioni SQL che può quindi essere eseguito come un'unità singola e riutilizzabile. Le stored procedure sono un esempio. Un vantaggio è *funzioni definite dall'utente*(UDF), un costrutto che verranno esaminate in maggiore dettaglio nel passaggio 9.

In sostanza, SQL è progettato per l'utilizzo di set di dati. Il `SELECT`, `UPDATE`, e `DELETE` intrinsecamente istruzioni si applicano a tutti i record della tabella corrispondente e sono limitate solo dai loro `WHERE` clausole. Esistono ancora molte funzionalità progettate per l'uso di un record alla volta e per la modifica di dati scalare. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) consentono un set di record possibile eseguirne il ciclo attraverso uno alla volta. Funzioni di manipolazione come stringa `LEFT`, `CHARINDEX`, e `PATINDEX` funzionano con dati scalari. SQL include anche istruzioni del flusso di controllo, ad esempio `IF` e `WHILE`.

Prima di Microsoft SQL Server 2005, le stored procedure e funzioni definite dall'utente potrebbe essere definita solo come una raccolta di istruzioni T-SQL. SQL Server 2005, tuttavia, è stato progettato per fornire l'integrazione con il [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), ovvero il runtime usato da tutti gli assembly .NET. Di conseguenza, le stored procedure e funzioni definite dall'utente in un database di SQL Server 2005 possono essere creati usando codice gestito. Vale a dire, è possibile creare una stored procedure o funzione definita dall'utente come un metodo in una classe Visual Basic. In questo modo le stored procedure e funzioni definite dall'utente per usare la funzionalità in .NET Framework e da proprie classi personalizzate.

In questa esercitazione che verrà esaminato come creare gestito stored procedure e funzioni definite dall'utente e come integrarli nel database Northwind. Introduzione a ti permettono di s.

> [!NOTE]
> Oggetti di database gestito offrono alcuni vantaggi rispetto ai corrispettivi di SQL. Ricchezza del linguaggio e conoscenza e la possibilità di riutilizzare la logica e il codice esistente sono i vantaggi principali. Ma gli oggetti di database gestiti possono risultare meno efficiente quando si lavora con set di dati che non riguardano molto logica procedurale. Per una discussione più approfondita sui vantaggi dell'uso di codice gestito o T-SQL, consultare il [vantaggi dell'utilizzo di codice gestito per creare oggetti di Database](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>Passaggio 1: Spostare il Database Northwind di`App_Data`

Tutte le esercitazioni finora hanno usato un file di database Microsoft SQL Server 2005 Express Edition in s dell'applicazione web `App_Data` cartella. L'inserimento del database in `App_Data` semplificato la distribuzione e l'esecuzione di queste esercitazioni come tutti i file sono stati individuati all'interno di una directory e non necessario alcun passaggio di configurazione aggiuntiva per eseguire il test dell'esercitazione.

Per questa esercitazione, tuttavia, ti permettono di s spostare il database Northwind di `App_Data` e registrarlo in modo esplicito con l'istanza di database di SQL Server 2005 Express Edition. Anche se è possibile eseguire la procedura per questa esercitazione con il database nel `App_Data` cartella apportata una serie di passaggi è molto più semplice registrando in modo esplicito il database con l'istanza di database di SQL Server 2005 Express Edition.

Il download per questa esercitazione ha due file di database - `NORTHWND.MDF` e `NORTHWND_log.LDF` - inserito in una cartella denominata `DataFiles`. Se si segue insieme a un'implementazione personalizzata delle esercitazioni, chiudere Visual Studio e passare il `NORTHWND.MDF` e `NORTHWND_log.LDF` i file dal sito Web di s `App_Data` cartella in una cartella all'esterno del sito Web. Una volta che i file di database sono stati spostati in una cartella diversa è necessario registrare il database Northwind con l'istanza di database di SQL Server 2005 Express Edition. Questa operazione può essere eseguita da SQL Server Management Studio. Se si dispone di un non - Express Edition di SQL Server 2005 installato nel computer quindi probabilmente già installato Management Studio. Se si solo installata nel computer SQL Server 2005 Express Edition, si consiglia di scaricare e installare [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Avviare SQL Server Management Studio. Come illustrato nella figura 1, Management Studio inizia ponendo quali server a cui connettersi. Immettere localhost\SQLExpress per il nome del server, scegliere l'autenticazione di Windows nell'elenco a discesa autenticazione e fare clic su Connetti.


![Connettersi all'istanza del Database appropriato](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image1.png)

**Figura 1**: connettersi all'istanza del Database appropriato


Una volta è già connessi, la finestra di Esplora oggetti elencherà le informazioni relative all'istanza di database di SQL Server 2005 Express Edition, tra cui il database, le informazioni di sicurezza, le opzioni di gestione e così via.

È necessario collegare il database Northwind nel `DataFiles` cartella (o punto in cui si potrebbe essere spostato,) per l'istanza di database di SQL Server 2005 Express Edition. Fare doppio clic sulla cartella database e scegliere l'opzione Connetti dal menu di scelta rapida. Verrà visualizzata la finestra di dialogo Collega database. Fare clic sul pulsante Aggiungi, eseguire il drill down appropriato `NORTHWND.MDF` file e fare clic su OK. A questo punto la schermata dovrebbe essere simile alla figura 2.


[![Connettersi all'istanza del Database appropriato](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image2.png)

**Figura 2**: connettersi all'istanza appropriata del Database ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image4.png))


> [!NOTE]
> Quando ci si connette all'istanza di SQL Server 2005 Express Edition tramite Management Studio nella finestra di dialogo Collega database non consentono di eseguire il drill-nelle directory del profilo utente, ad esempio documenti. Pertanto, assicurarsi di posizionare le `NORTHWND.MDF` e `NORTHWND_log.LDF` file in una directory di profilo non utente.


Fare clic su OK per collegare il database. Verrà chiusa la finestra di dialogo Collega database ed Esplora oggetti a questo punto dovrebbe elencare il database appena collegato. È probabile che Northwind database con un nome simile a `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Rinominare il database a Northwind facendo clic sul database e scegliendo Rinomina.


![Rinominare il Database a Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image5.png)

**Figura 3**: rinominare il Database a Northwind


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Passaggio 2: Creazione di una nuova soluzione e progetto di SQL Server in Visual Studio

Per creare le stored procedure gestite o funzioni definite dall'utente in SQL Server 2005 si scriverà la stored procedure e per la logica definita dall'utente come codice di Visual Basic in una classe. Dopo il codice è stata scritta, è necessario compilare questa classe in un assembly (un `.dll` file), registrare l'assembly con il database di SQL Server e quindi creare una stored procedure o un oggetto funzione definita dall'utente nel database che punta al metodo corrispondente in l'assembly. Questi passaggi possono tutti essere eseguiti manualmente. È possibile creare il codice in qualsiasi testo dell'editor, compilarlo dalla riga di comando usando il compilatore Visual Basic (`vbc.exe`), registrarlo con il database utilizzando il [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx) comando o da Management Studio e aggiungere l'oggetto memorizzato procedura o un oggetto funzione definita dall'utente tramite uno strumento simile. Fortunatamente, le versioni di sistemi Professional e Team di Visual Studio includono un tipo di progetto di SQL Server che consente di automatizzare queste attività. In questa esercitazione si apprenderanno utilizzando il tipo di progetto di SQL Server per creare una stored procedure gestita e funzioni definite dall'utente.

> [!NOTE]
> Se si usa l'edizione Standard di Visual Studio o Visual Web Developer, sarà necessario usare invece l'approccio manuale. Passaggio 13 vengono fornite istruzioni dettagliate per l'esecuzione di questi passaggi manualmente. Consiglia di leggere i passaggi da 2 a 12 prima di leggere passaggio 13 poiché tali passaggi includono importanti istruzioni di configurazione di SQL Server che devono essere applicate indipendentemente dalla versione di Visual Studio in uso.


Iniziare aprendo Visual Studio. Dal menu File, scegliere Nuovo progetto per visualizzare la finestra di dialogo Nuovo progetto (vedere la figura 4). Eseguire il drill down il tipo di progetto di Database e dai modelli elencati a destra, fare clic creare un nuovo progetto di SQL Server. Per assegnare un nome di questo progetto è stato scelto `ManagedDatabaseConstructs` ed è stato inserito all'interno di una soluzione denominata `Tutorial75`.


[![Creare un nuovo progetto SQL Server](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image6.png)

**Figura 4**: creare un nuovo progetto di SQL Server ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image8.png))


Fare clic sul pulsante OK nella finestra di dialogo Nuovo progetto per creare la soluzione e progetto di SQL Server.

Un progetto di SQL Server è associato a un database specifico. Di conseguenza, dopo aver creato il nuovo progetto di SQL Server ci viene immediatamente richiesto di specificare queste informazioni. Figura 5 mostra la finestra di dialogo Nuovo riferimento al Database che è stato compilato in modo che punti al database Northwind che è registrati nell'istanza del database SQL Server 2005 Express Edition nel passaggio 1.


![Associare il progetto di SQL Server con il Database Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image9.png)

**Figura 5**: associare il progetto di SQL Server con il Database Northwind


Per eseguire il debug di stored procedure gestite e funzioni definite dall'utente verrà creato all'interno di questo progetto, è necessario abilitare il supporto per la connessione del debug SQL/CLR. Ogni volta che l'associazione di un progetto di SQL Server con un nuovo database (come abbiamo fatto nella figura 5), Visual Studio viene chiesto di specificare se si vuole abilitare il debug SQL/CLR per la connessione (vedere la figura 6). Fare clic su Sì.


![Abilita debug SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image10.png)

**Figura 6**: abilitare il debug SQL/CLR


A questo punto il nuovo progetto di SQL Server è stato aggiunto alla soluzione. Contiene una cartella denominata `Test Scripts` con un file denominato `Test.sql`, che viene usato per eseguire il debug di oggetti di database gestiti creati nel progetto. Si esaminerà il debug nel passaggio 12.

È ora possibile aggiungere nuove stored procedure gestite e funzioni definite dall'utente a questo progetto, ma prima che facciamo storsimple includono prima applicazione web esistente nella soluzione. Dal menu File selezionare l'opzione Aggiungi e scegliere il sito Web esistente. Passare alla cartella sito Web appropriato e fare clic su OK. Come illustrato nella figura 7, si aggiornerà la soluzione in modo da includere due progetti: il sito Web e il `ManagedDatabaseConstructs` progetto SQL Server.


![Esplora soluzioni include ora due progetti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image11.png)

**Figura 7**: Esplora soluzioni include ora due progetti


Il `NORTHWNDConnectionString` valore in `Web.config` attualmente fa riferimento il `NORTHWND.MDF` del file nel `App_Data` cartella. Poiché è stata rimossa dal database `App_Data` e ha registrato in modo esplicito nell'istanza del database di SQL Server 2005 Express Edition, è necessario aggiornare di conseguenza il `NORTHWNDConnectionString` valore. Aprire il `Web.config` file nel sito Web e modificare il `NORTHWNDConnectionString` valore in modo che legge la stringa di connessione: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Dopo questa modifica, il `<connectionStrings>` sezione `Web.config` dovrebbe essere simile al seguente:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample1.xml)]

> [!NOTE]
> Come descritto nel [esercitazione precedente](debugging-stored-procedures-vb.md), durante il debug di un oggetto di SQL Server da un'applicazione client, ad esempio un sito Web ASP.NET, è necessario disabilitare il pool di connessioni. La stringa di connessione illustrata in precedenza disabilita il pool di connessioni ( `Pooling=false` ). Se non si prevede eseguire il debug di funzioni definite dall'utente e stored procedure gestite dal sito Web ASP.NET, abilitare il pool di connessioni.


## <a name="step-3-creating-a-managed-stored-procedure"></a>Passaggio 3: Creazione di un oggetto gestito Stored Procedure

Per aggiungere una stored procedure gestita al database Northwind è necessario innanzitutto creare la stored procedure come metodo nel progetto di SQL Server. Da Esplora soluzioni, fare clic su di `ManagedDatabaseConstructs` nome del progetto e scegliere di aggiungere un nuovo elemento. Questo visualizzerà la finestra di dialogo Aggiungi nuovo elemento, in cui sono elencati i tipi di oggetti di database gestiti che possono essere aggiunti al progetto. Come illustrato nella figura 8, incluse stored procedure e funzioni definite dall'utente, tra gli altri.

Let s iniziare aggiungendo una stored procedure che restituisce semplicemente tutti i prodotti che sono stati sospesi. Denominare il nuovo file di stored procedure `GetDiscontinuedProducts.vb`.


[![Aggiungere una nuova Stored Procedure denominata GetDiscontinuedProducts.vb](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image12.png)

**Figura 8**: aggiungere una nuova Stored Procedure denominata `GetDiscontinuedProducts.vb` ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image14.png))


Si creerà un nuovo file di classe di Visual Basic con il contenuto seguente:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample2.vb)]

Si noti che la stored procedure viene implementata come un `Shared` metodo all'interno di un `Partial` file di classe denominato `StoredProcedures`. Inoltre, il `GetDiscontinuedProducts` metodo è decorato con il [ `SqlProcedure` attributo](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlprocedureattribute.aspx), che indica che il metodo come stored procedure.

Il codice seguente crea una `SqlCommand` oggetto e imposta relativo `CommandText` a un `SELECT` query che restituisce tutte le colonne dal `Products` tabella per i prodotti i cui `Discontinued` campo è uguale a 1. Quindi esegue il comando e invia i risultati all'applicazione client. Aggiungere questo codice per il `GetDiscontinuedProducts` (metodo).


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample3.vb)]

Tutti gli oggetti di database gestito hanno accesso a un [ `SqlContext` oggetto](https://msdn.microsoft.com/library/ms131108.aspx) che rappresenta il contesto del chiamante. Il `SqlContext` fornisce l'accesso a un [ `SqlPipe` oggetto](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) tramite relativo [ `Pipe` proprietà](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Ciò `SqlPipe` oggetto viene usato per portare le informazioni tra il database di SQL Server e l'applicazione chiamante. Come suggerisce il nome, il [ `ExecuteAndSend` metodo](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) esegue un passato aggiuntivo `SqlCommand` oggetto e invia i risultati nuovamente nell'applicazione client.

> [!NOTE]
> Oggetti di database gestiti sono più adatti per le stored procedure e funzioni definite dall'utente che usano una logica procedurale anziché per la logica basata su set. Logica procedurale comporta l'utilizzo di set di dati in base a una riga per riga o l'utilizzo di dati scalare. Il `GetDiscontinuedProducts` metodo appena creato, tuttavia, non prevede alcuna logica procedurale. Pertanto, sarebbe idealmente essere implementata come una procedura T-SQL archiviate. Viene implementato come una stored procedure gestita per illustrare i passaggi necessari per la creazione e distribuzione gestita stored procedure.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>Passaggio 4: Distribuzione gestita, Stored Procedure

Con questo codice completo, siamo pronti per distribuirla al database Northwind. Distribuzione di un progetto di SQL Server compila il codice in un assembly, registrare l'assembly con il database e crea gli oggetti corrispondenti nel database, collegandole ai metodi appropriati nell'assembly. Il set esatto di attività eseguite dall'opzione di distribuzione in modo più preciso è descritto nel passaggio 13. Fare clic su di `ManagedDatabaseConstructs` nome in Esplora soluzioni del progetto e scegliere l'opzione di distribuzione. Tuttavia, la distribuzione ha esito negativo con l'errore seguente: sintassi errata vicino a 'Esterno'. Si potrebbe essere necessario impostare il livello di compatibilità del database corrente su un valore superiore per abilitare questa funzionalità. Vedere la Guida per la stored procedure `sp_dbcmptlevel`.

Questo messaggio di errore si verifica quando si prova a registrare l'assembly con il database Northwind. Per registrare un assembly con un database di SQL Server 2005, il livello di compatibilità del database s deve essere impostato su 90. Per impostazione predefinita, i nuovi database di SQL Server 2005 hanno un livello di compatibilità 90. Tuttavia, i database creati utilizzando Microsoft SQL Server 2000 hanno un livello di compatibilità predefinito pari a 80. Poiché il database Northwind inizialmente era un database Microsoft SQL Server 2000, il livello di compatibilità è attualmente impostato su 80 e pertanto deve essere aumentato su 90 per registrare gli oggetti di database gestito.

Per aggiornare il livello di compatibilità del database s, aprire una finestra Nuova Query in Management Studio e immettere:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample4.sql)]

Fare clic sull'icona Esegui sulla barra degli strumenti per eseguire la query precedente.


[![Aggiornare il livello di compatibilità del Database Northwind s](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image15.png)

**Figura 9**: aggiornare il Northwind Database s a livello di compatibilità ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image17.png))


Dopo aver aggiornato il livello di compatibilità, ridistribuire il progetto di SQL Server. Questa volta la distribuzione deve essere completata senza errori.

Tornare a SQL Server Management Studio, fare doppio clic sul database Northwind in Esplora oggetti e scegliere Aggiorna. Successivamente, eseguire il drill-nella cartella programmabilità e quindi espandere la cartella degli assembly. Come illustrato nella figura 10, il database Northwind include ora l'assembly generato dal `ManagedDatabaseConstructs` progetto.


![il ManagedDatabaseConstructs Assembly è ora registrato con il Northwind Database](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image18.png)

**Figura 10**: il `ManagedDatabaseConstructs` Assembly è ora registrato con il Northwind Database


Inoltre, espandere la cartella di Stored procedure. Si vedrà una stored procedure denominata `GetDiscontinuedProducts`. Questa stored procedure è stata creata dal processo di distribuzione e punti per il `GetDiscontinuedProducts` metodo nella `ManagedDatabaseConstructs` assembly. Quando la `GetDiscontinuedProducts` stored procedure viene eseguita, a sua volta, esegue il `GetDiscontinuedProducts` (metodo). Poiché si tratta di una stored procedure gestita non può essere modificato tramite Management Studio (di conseguenza l'icona del lucchetto accanto al nome della stored procedure).


![La Stored Procedure GetDiscontinuedProducts è elencata nella cartella Stored procedure](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image19.png)

**Figura 11**: il `GetDiscontinuedProducts` Stored Procedure è elencata nella cartella Stored procedure


È ancora presente un ostacolo è necessario risolvere prima di poterlo chiamare la stored procedure gestita: il database è configurato per impedire l'esecuzione del codice gestito. Verificare questo aprendo una nuova finestra query e l'esecuzione di `GetDiscontinuedProducts` stored procedure. Si riceverà il messaggio di errore seguente: l'esecuzione del codice utente in .NET Framework è disabilitata. Abilitare l'opzione di configurazione di clr abilitato.

Esaminare le informazioni di configurazione database s Northwind, immettere ed eseguire il comando `exec sp_configure` nella finestra di query. Ciò viene illustrato che clr abilitato l'impostazione è attualmente impostato su 0.


[![Clr abilitato impostazione è attualmente impostato su 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image20.png)

**Figura 12**: clr abilitato impostazione è attualmente impostato su 0 ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image22.png))


Si noti che ogni impostazione di configurazione nella figura 12 ha quattro valori elencati con esso: il requisito minimo e i valori massimi e la configurazione e valori esecuzione. Per aggiornare il valore di configurazione per l'impostazione di clr abilitato, eseguire il comando seguente:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample5.sql)]

Se si esegue nuovamente il `exec sp_configure` si noterà che l'istruzione precedente aggiornato il valore di configurazione dell'impostazione di clr abilitato s su 1, ma che il valore di esecuzione è ancora impostato su 0. Per rendere effettiva la modifica di configurazione è necessario eseguire la [ `RECONFIGURE` comando](https://msdn.microsoft.com/library/ms176069.aspx), che imposterà il valore di esecuzione per il valore di configurazione corrente. È sufficiente immettere `RECONFIGURE` nella finestra di query e fare clic sull'icona Esegui sulla barra degli strumenti. Se si esegue `exec sp_configure` a questo punto è consigliabile vedere il valore 1 per la configurazione di impostazione clr abilitato s ed eseguire i valori.

Aver completato la configurazione clr abilitato, siamo pronti per l'esecuzione gestita `GetDiscontinuedProducts` stored procedure. Nella finestra query immettere ed eseguire il comando `exec` `GetDiscontinuedProducts`. Se si richiama la stored procedure, il codice gestito corrispondente nel `GetDiscontinuedProducts` metodo da eseguire. Questo codice genera un `SELECT` query per restituire tutti i prodotti che non sono più disponibili e restituisce i dati all'applicazione chiamante, ovvero SQL Server Management Studio in questa istanza. Management Studio riceve questi risultati e li visualizza nella finestra dei risultati.


[![Il GetDiscontinuedProducts Stored Procedure restituisce tutti i prodotti sospesi](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image23.png)

**Figura 13**: il `GetDiscontinuedProducts` Stored Procedure restituisce prodotti sospesi ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Passaggio 5: Creazione gestito Stored procedure che accettano parametri di Input

Molte delle query e stored procedure è stata creata in queste esercitazioni hanno usato *parametri*. Ad esempio, nelle [creazione di nuove Stored procedure per DataSet tipizzata s TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) esercitazione viene creata una stored procedure denominata `GetProductsByCategoryID` che accettato un parametro di input denominato `@CategoryID`. La stored procedure quindi restituiti tutti i prodotti il cui `CategoryID` campo corrispondente al valore dell'oggetto fornito `@CategoryID` parametro.

Per creare una stored procedure gestita che accetta i parametri di input, è sufficiente specificare tali parametri nella definizione del metodo s. Per illustrare questo concetto, s ti permettono di aggiungere un'altra stored procedure gestita per il `ManagedDatabaseConstructs` progetto denominato `GetProductsWithPriceLessThan`. Questa stored procedure gestita accetta un parametro di input specificando un prezzo e restituirà tutti i prodotti il cui `UnitPrice` campo è minore del valore di s parametri.

Per aggiungere una nuova stored procedure per il progetto, fare clic su di `ManagedDatabaseConstructs` nome del progetto e scegliere di aggiungere una nuova stored procedure. Denominare il file `GetProductsWithPriceLessThan.vb`. Come abbiamo visto nel passaggio 3, si creerà un nuovo file di classe di Visual Basic con un metodo denominato `GetProductsWithPriceLessThan` posizionato all'interno di `Partial` classe `StoredProcedures`.

Aggiorna il `GetProductsWithPriceLessThan` definizione di metodo s in modo che accetti un [ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) parametro di input denominato `price` e scrivere il codice per eseguire e restituire i risultati della query:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample6.vb)]

Il `GetProductsWithPriceLessThan` codice e la definizione di metodo s rispecchia maggiormente la definizione e codice del `GetDiscontinuedProducts` metodo creato nel passaggio 3. Le uniche differenze sono che il `GetProductsWithPriceLessThan` metodo accetta come parametro di input (`price`), il `SqlCommand` s query include un parametro (`@MaxPrice`), e verrà aggiunto un parametro per il `SqlCommand` s `Parameters` raccolta è e assegnato il valore della `price` variabile.

Dopo aver aggiunto questo codice, ridistribuire il progetto di SQL Server. Successivamente, tornare a SQL Server Management Studio e aggiornare la cartella di Stored procedure. Verrà visualizzata una nuova voce, `GetProductsWithPriceLessThan`. Da una finestra query, immettere ed eseguire il comando `exec GetProductsWithPriceLessThan 25`, che verranno elencati tutti i prodotti di minore di $25, come illustrato nella figura 14.


[![Vengono visualizzati i prodotti in $25](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image26.png)

**Figura 14**: vengono visualizzati i prodotti in $25 ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Passaggio 6: Chiamare la Stored Procedure gestita dal livello di accesso ai dati

A questo punto è stata aggiunta la `GetDiscontinuedProducts` e `GetProductsWithPriceLessThan` gestita delle stored procedure di `ManagedDatabaseConstructs` del progetto e avere registrato con il database Northwind di SQL Server. Viene richiamato anche queste stored procedure gestite da SQL Server Management Studio (vedere figure 13 e 14). Affinché l'ASP.NET dell'applicazione per usare questi gestite le stored procedure, tuttavia, è necessario aggiungerli ai livelli di logica Business nell'architettura e l'accesso ai dati. In questo passaggio si aggiungeranno due nuovi metodi per la `ProductsTableAdapter` nella `NorthwindWithSprocs` DataSet tipizzato, è stato inizialmente creato nel [creazione di nuove Stored procedure per DataSet tipizzata s TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) esercitazione. Nel passaggio 7 si aggiungerà metodi corrispondenti per il livello BLL.

Aprire il `NorthwindWithSprocs` DataSet tipizzato in Visual Studio e iniziare aggiungendo un nuovo metodo per il `ProductsTableAdapter` denominata `GetDiscontinuedProducts`. Per aggiungere un nuovo metodo a un oggetto TableAdapter, fare doppio clic sul nome s TableAdapter nella finestra di progettazione e scegliere l'opzione di Query aggiunta dal menu di scelta rapida.

> [!NOTE]
> Dopo la migrazione del database Northwind di `App_Data` cartella all'istanza del database SQL Server 2005 Express Edition, è fondamentale che la stringa di connessione corrispondente nel file Web. config aggiornato per riflettere questa modifica. Nel passaggio 2 è stato illustrato l'aggiornamento di `NORTHWNDConnectionString` valore `Web.config`. Se si dimentica di eseguire questo aggiornamento, quindi noterete il messaggio di errore Impossibile aggiungere query. Impossibile trovare la connessione `NORTHWNDConnectionString` per l'oggetto `Web.config` in una finestra di dialogo quando si tenta di aggiungere un nuovo metodo all'oggetto TableAdapter. Per risolvere questo errore, fare clic su OK e quindi andare al `Web.config` e aggiornare il `NORTHWNDConnectionString` valore come descritto nel passaggio 2. Quindi provare ad aggiungere nuovamente il metodo all'oggetto TableAdapter. Questa volta dovrebbe funzionare senza errori.


Aggiunta di un nuovo metodo avvia la configurazione guidata Query TableAdapter, che è stato usato più volte nelle esercitazioni precedenti. Il primo passaggio richiede l'impostazione di modalità di accesso del database dell'oggetto TableAdapter: tramite un'istruzione SQL ad hoc o una stored procedure nuova o esistente. Poiché abbiamo già creato e registrato il `GetDiscontinuedProducts` stored procedure gestita con il database, scegliere l'Usa esistente archiviato opzione procedure e scegliere "Avanti".


[![Scegliere l'utilizzo di stored procedure opzione esistente](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image29.png)

**Figura 15**: scegliere stored procedure opzione di utilizzo esistente ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image31.png))


Nella schermata successiva richiede per la stored procedure che verrà richiamato dal metodo. Scegliere il `GetDiscontinuedProducts` stored procedure gestita dall'elenco a discesa elenco e scegliere "Avanti".


[![Selezionare il GetDiscontinuedProducts gestito Stored Procedure](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image32.png)

**Figura 16**: selezionare il `GetDiscontinuedProducts` Stored Procedure gestita ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image34.png))


Quindi, ci viene richiesto di specificare se la stored procedure restituisce righe, un singolo valore o nulla. Poiché `GetDiscontinuedProducts` restituisce il set di righe prodotto non più disponibile, scegliere la prima opzione (tabulare) e fare clic su Avanti.


[![Selezionare l'opzione di dati tabulari](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image35.png)

**Figura 17**: selezionare l'opzione di dati tabulare ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image37.png))


Schermata della procedura guidata finale ci consente di specificare i modelli di accesso ai dati usati e i nomi dei metodi risultanti. Lasciare le caselle di controllo selezionata e il nome dei metodi `FillByDiscontinued` e `GetDiscontinuedProducts`. Fare clic su Fine per completare la procedura guidata.


[![Nome FillByDiscontinued i metodi e GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image38.png)

**Figura 18**: nome dei metodi `FillByDiscontinued` e `GetDiscontinuedProducts` ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image40.png))


Ripetere questi passaggi per creare metodi denominati `FillByPriceLessThan` e `GetProductsWithPriceLessThan` nel `ProductsTableAdapter` per il `GetProductsWithPriceLessThan` stored procedure gestita.

Figura 19 mostra una schermata della finestra di progettazione set di dati dopo aver aggiunto i metodi per la `ProductsTableAdapter` per il `GetDiscontinuedProducts` e `GetProductsWithPriceLessThan` gestiti stored procedure.


[![Il ProductsTableAdapter include i nuovi metodi aggiunti in questo passaggio](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image41.png)

**Figura 19**: il `ProductsTableAdapter` include nuovi metodi aggiunti in questo passaggio ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Passaggio 7: Aggiunta di metodi corrispondenti a livello della logica di Business

Ora che abbiamo aggiornato il livello di accesso ai dati in modo da includere i metodi per chiamare la stored procedure gestite aggiunte nei passaggi 4 e 5, è necessario aggiungere metodi corrispondenti al livello della logica di Business. Aggiungere i due metodi seguenti per il `ProductsBLLWithSprocs` classe:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample7.vb)]

Entrambi i metodi è sufficiente chiamare il metodo DAL corrispondente e restituire il `ProductsDataTable` istanza. Il `DataObjectMethodAttribute` markup sopra ogni metodo fa in modo che questi metodi da includere nell'elenco a discesa scegliere nella scheda Seleziona della procedura guidata Configura Zdroj dat s ObjectDataSource.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Passaggio 8: Chiamata gestita, le Stored procedure dal livello di presentazione

Con la logica di Business e i livelli di accesso ai dati ampliata per includere il supporto per la chiamata di `GetDiscontinuedProducts` e `GetProductsWithPriceLessThan` gestite le stored procedure, ora è possibile visualizzare questi archiviati i risultati di procedure tramite una pagina ASP.NET.

Aprire il `ManagedFunctionsAndSprocs.aspx` nella pagina di `AdvancedDAL` cartella e, dalla casella degli strumenti, trascinare un controllo GridView nella finestra di progettazione. Impostare la s GridView `ID` proprietà `DiscontinuedProducts` e, dal suo smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `DiscontinuedProductsDataSource`. Configurare ObjectDataSource per estrarre i dati dal `ProductsBLLWithSprocs` classe s `GetDiscontinuedProducts` (metodo).


[![Configurare ObjectDataSource per usare la classe ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image44.png)

**Figura 20**: configurare ObjectDataSource per usare il `ProductsBLLWithSprocs` classe ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image46.png))


[![Scegliere il metodo GetDiscontinuedProducts nell'elenco a discesa della scheda selezionare](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image47.png)

**Figura 21**: scegliere il `GetDiscontinuedProducts` metodo nell'elenco a discesa della scheda selezionare ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image49.png))


Perché verrà usato in questa griglia per visualizzare solo informazioni di prodotto, impostare gli elenchi a discesa nell'aggiornamento, inserimento, eliminare le schede su (nessuno) e quindi fare clic su Fine.

Dopo aver completato la procedura guidata, Visual Studio aggiungerà automaticamente un BoundField o CampoCasellaDiControllo per ogni campo dati il `ProductsDataTable`. Si consiglia di rimuovere tutti questi campi, ad eccezione di `ProductName` e `Discontinued`, a cui punta il controllo GridView e markup dichiarativo di ObjectDataSource s dovrebbe essere simile al seguente:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample8.aspx)]

Richiedere qualche istante per visualizzare questa pagina tramite un browser. Quando viene visitata la pagina, le chiamate di ObjectDataSource i `ProductsBLLWithSprocs` classe s `GetDiscontinuedProducts` (metodo). Come abbiamo visto nel passaggio 7, questo metodo chiama verso il basso per i dispositivi DAL `ProductsDataTable` classe s `GetDiscontinuedProducts` metodo, che richiama la `GetDiscontinuedProducts` stored procedure. Questa stored procedure è un oggetto gestito stored procedure ed esegue il codice che è stato creato nel passaggio 3, restituendo i prodotti non più utilizzati.

I risultati restituiti dalla stored procedure gestita vengono creati in un `ProductsDataTable` DAL e sono tornati a livello BLL, che quindi li restituisce al livello di presentazione in cui vengono associate a GridView e visualizzati. Come previsto, la griglia sono elencati i prodotti che sono stati sospesi.


[![Sono elencati i prodotti non più disponibile](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image50.png)

**Figura 22**: sono elencati i prodotti di non più disponibili ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image52.png))


Per un'ulteriore procedura consigliata, aggiungere una casella di testo e un altro controllo GridView alla pagina. Visualizzare i prodotti minore di quello immesso nella casella di testo tramite una chiamata di questo controllo GridView i `ProductsBLLWithSprocs` classe s `GetProductsWithPriceLessThan` (metodo).

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Passaggio 9: Creazione e la chiamata a funzioni definite dall'utente di T-SQL

Funzioni definite dall'utente o funzioni definite dall'utente, sono oggetti di database di strettamente che riproducono la semantica delle funzioni nei linguaggi di programmazione. Ad esempio una funzione in Visual Basic, funzioni definite dall'utente possono includere un numero variabile di parametri di input e restituiscono un valore di un determinato tipo. Una funzione definita dall'utente possono restituire entrambi dati scalari - una stringa, un numero intero e così via o dati tabulari. Let s dare un'occhiata veloce entrambi i tipi di funzioni definite dall'utente, a partire da una funzione definita dall'utente che restituisce un tipo di dati scalare.

La funzione definita dall'utente seguente calcola il valore stimato l'inventario per un determinato prodotto. Esegue l'operazione, prendendo in tre parametri di input - i `UnitPrice`, `UnitsInStock`, e `Discontinued` i valori per un determinato prodotto - e restituisce un valore di tipo `money`. Calcola il valore stimato dell'inventario moltiplicando il `UnitPrice` dal `UnitsInStock`. Per gli elementi non più utilizzati, questo valore viene dimezzato.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample9.sql)]

Dopo aver aggiunto questa funzione definita dall'utente nel database, possono essere trovati tramite Management Studio espandere la cartella programmabilità, quindi le funzioni e quindi funzioni con valori scalari. Può essere utilizzato un `SELECT` query come segue:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample10.sql)]

È stato aggiunto il `udf_ComputeInventoryValue` funzione definita dall'utente al database Northwind. Figura 23 è illustrata l'output delle precedenti `SELECT` eseguire una query quando vengono visualizzate tramite Management Studio. Si noti inoltre che la funzione definita dall'utente sia elencato sotto la cartella di funzioni con valori scalari in Esplora oggetti.


[![Viene elencato ogni prodotto s valori di inventario](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image53.png)

**Figura 23**: ogni prodotto s valori di scorte è elencato ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image55.png))


Funzioni definite dall'utente può inoltre restituire dati tabulari. Ad esempio, è possibile creare una funzione definita dall'utente che restituisce i prodotti che appartengono a una determinata categoria:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample11.sql)]

Il `udf_GetProductsByCategoryID` definita dall'utente accetta una `@CategoryID` parametro di input e restituisce i risultati dell'oggetto specificato `SELECT` query. Una volta creato, questa funzione definita dall'utente può fare riferimento il `FROM` (o `JOIN`) della clausola un `SELECT` query. Nell'esempio seguente restituisce il `ProductID`, `ProductName`, e `CategoryID` valori per ognuna delle bevande.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample12.sql)]

È stato aggiunto il `udf_GetProductsByCategoryID` funzione definita dall'utente al database Northwind. Figura 24 Mostra l'output delle precedenti `SELECT` eseguire una query quando vengono visualizzate tramite Management Studio. Funzioni definite dall'utente che restituiscono dati in formato tabulare è reperibile nella cartella di Esplora oggetti s funzioni con valori di tabella.


[![Il ProductID, ProductName e CategoryID elencati per ogni bevande](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image56.png)

**Figura 24**: il `ProductID`, `ProductName`, e `CategoryID` elencati per ogni bevande ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image58.png))


> [!NOTE]
> Per altre informazioni sulla creazione e sull'uso di funzioni definite dall'utente, consultare [Introduzione a funzioni definite dall'utente](http://www.sqlteam.com/item.asp?ItemID=1955). Vedere anche [vantaggi e le funzioni Drawbacks of User-Defined](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>Passaggio 10: Creare una funzione definita dall'utente gestito

Il `udf_ComputeInventoryValue` e `udf_GetProductsByCategoryID` funzioni definite dall'utente creato negli esempi precedenti sono oggetti di database T-SQL. SQL Server 2005 supporta anche la UDF gestita, che può essere aggiunto al `ManagedDatabaseConstructs` progetto esattamente come gestita, le stored procedure dai passaggi 3 e 5. Per questo passaggio, lasciare s implementare il `udf_ComputeInventoryValue` funzione definita dall'utente nel codice gestito.

Per aggiungere un UDF gestito per il `ManagedDatabaseConstructs` del progetto, fare doppio clic sul nome del progetto in Esplora soluzioni e scegliere di aggiungere un nuovo elemento. Selezionare il modello definito dall'utente nella finestra di dialogo Aggiungi nuovo elemento e denominare il nuovo file di funzione definita dall'utente `udf_ComputeInventoryValue_Managed.vb`.


[![Aggiungere una nuova funzione definita dall'utente gestito per il progetto ManagedDatabaseConstructs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image59.png)

**Figura 25**: aggiungere una nuova UDF gestito per il `ManagedDatabaseConstructs` progetto ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image61.png))


Il modello di funzione definita dall'utente crea una `Partial` classe denominata `UserDefinedFunctions` con un metodo il cui nome è lo stesso nome di file s della classe (`udf_ComputeInventoryValue_Managed`, in questo caso). Questo metodo è decorato con il [ `SqlFunction` attributo](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), che contrassegna il metodo come una funzione definita dall'utente gestito.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample13.vb)]

Il `udf_ComputeInventoryValue` metodo attualmente restituisce un [ `SqlString` oggetto](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) e non accetta eventuali parametri di input. È necessario aggiornare la definizione del metodo in modo che accetta tre parametri - di input `UnitPrice`, `UnitsInStock`, e `Discontinued` - e restituisce un `SqlMoney` oggetto. La logica per calcolare il valore di magazzino è identica a quello in T-SQL `udf_ComputeInventoryValue` funzione definita dall'utente.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample14.vb)]

Si noti che i parametri di input del metodo s funzione definita dall'utente sono dei tipi SQL corrispondenti: `SqlMoney` per il `UnitPrice` campo [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) per `UnitsInStock`, e [ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) per `Discontinued`. Questi tipi di dati riflettono i tipi definiti nel `Products` tabella: il `UnitPrice` sloupec JE typu `money`, il `UnitsInStock` colonna di tipo `smallint`e la `Discontinued` colonna di tipo `bit`.

Il codice inizia creando un `SqlMoney` istanza denominata `inventoryValue` che viene assegnato un valore pari a 0. Il `Products` table consente per il database `NULL` i valori di `UnitsInPrice` e `UnitsInStock` colonne. Pertanto, è necessario verificare innanzitutto per vedere se questi valori contengono `NULL` s, come in questo caso tramite i `SqlMoney` oggetto s [ `IsNull` proprietà](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Se entrambi `UnitPrice` e `UnitsInStock` contengono non`NULL` i valori, quindi verrà eseguito il calcolo di `inventoryValue` a essere il prodotto dei due. Se quindi `Discontinued` è true, abbiamo dimezzare il valore.

> [!NOTE]
> Il `SqlMoney` oggetto consente solo di due `SqlMoney` istanze devono essere moltiplicati insieme. Non consente un `SqlMoney` istanza venga moltiplicato per un numero a virgola mobile del valore letterale. Pertanto, per dimezzare `inventoryValue` abbiamo moltiplicarlo per un nuovo `SqlMoney` istanza con il valore 0,5.


## <a name="step-11-deploying-the-managed-udf"></a>Passaggio 11: Distribuzione della UDF gestita

Dopo che è stata creata la funzione definita dall'utente gestiti, siamo pronti per distribuirla al database Northwind. Come abbiamo visto nel passaggio 4, gli oggetti gestiti in un progetto di SQL Server vengono distribuiti facendo clic sul nome del progetto in Esplora soluzioni e scegliendo Distribuisci dal menu di scelta rapida.

Dopo aver distribuito il progetto, tornare a SQL Server Management Studio e aggiornare la cartella di funzioni a valori scalari. È ora verranno visualizzate due voci:

- `dbo.udf_ComputeInventoryValue` -la UDF T-SQL creato nel passaggio 9, e
- `dbo.udf ComputeInventoryValue_Managed` -la UDF gestita creato nel passaggio 10 che è stato appena distribuito.

Per testare questa funzione definita dall'utente gestito, eseguire la query seguente da Management Studio:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample15.sql)]

Questo comando Usa managed `udf ComputeInventoryValue_Managed` definita dall'utente anziché l'istruzione T-SQL `udf_ComputeInventoryValue` funzione definita dall'utente, ma l'output è lo stesso. Vedere Figura 23 per visualizzare una schermata dell'output s funzione definita dall'utente.

## <a name="step-12-debugging-the-managed-database-objects"></a>Passaggio 12: Debug di oggetti di Database gestiti

Nel [debug di Stored procedure](debugging-stored-procedures-vb.md) esercitazione abbiamo discusso le tre opzioni per il debug di SQL Server tramite Visual Studio: debug diretto di Database, il debug dell'applicazione e debug da un progetto di SQL Server. Database gli oggetti non è possibile eseguire il debug tramite debug diretto di Database, ma è possono eseguire il debug gestito da un'applicazione client e direttamente dal progetto di SQL Server. In ordine affinché il debug, tuttavia, il database di SQL Server 2005 deve consentire il debug SQL/CLR. Si tenga presente che quando abbiamo creato prima di tutto la `ManagedDatabaseConstructs` progetto Visual Studio hanno chiesto se si desidera abilitare il debug (vedere la figura 6 nel passaggio 2) SQL/CLR. Questa impostazione può essere modificata facendo clic con il database dalla finestra Esplora Server.


![Assicurarsi che il Database consenta debug SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image62.png)

**Figura 26**: assicurarsi che il Database consenta debug SQL/CLR


Si supponga di voler eseguire il debug di `GetProductsWithPriceLessThan` stored procedure gestita. Iniziamo impostando un punto di interruzione all'interno del codice del `GetProductsWithPriceLessThan` (metodo).


[![Impostare un punto di interruzione nel metodo GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image63.png)

**Figura 27**: impostare un punto di interruzione il `GetProductsWithPriceLessThan` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image65.png))


Let s prima di tutto è esaminiamo il debug di oggetti di database gestiti dal progetto SQL Server. Poiché la nostra soluzione contiene due progetti: il `ManagedDatabaseConstructs` progetto di SQL Server con il nostro sito Web - per eseguire il debug dal progetto di Server SQL, dobbiamo indicare a Visual Studio per avviare il `ManagedDatabaseConstructs` progetto SQL Server quando si avvia il debug. Fare doppio clic su di `ManagedDatabaseConstructs` sul progetto in Esplora soluzioni e scegliere il Set come opzione progetto di avvio dal menu di scelta rapida.

Quando la `ManagedDatabaseConstructs` project viene avviato il debugger esegue le istruzioni SQL nel `Test.sql` file che si trova nel `Test Scripts` cartella. Ad esempio, per testare la `GetProductsWithPriceLessThan` stored procedure, gestita sostituire il `Test.sql` contenuto con l'istruzione seguente, che consente di visualizzare file le `GetProductsWithPriceLessThan` passando stored procedure gestita il `@CategoryID` valore 14.95:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample16.sql)]

Dopo che è già digitato lo script precedente in `Test.sql`, avviare il debug, selezionando il menu di Debug e scegliere Avvia debug oppure premendo F5 o verde icona Riproduci sulla barra degli strumenti. Questo verrà compilare i progetti all'interno della soluzione, distribuire gli oggetti di database gestito per il database Northwind e quindi eseguire il `Test.sql` script. A questo punto, il punto di interruzione verrà raggiunto e posso il `GetProductsWithPriceLessThan` (metodo), esaminare i valori dei parametri di input e così via.


[![Il punto di interruzione nel metodo GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image66.png)

**Figura 28**: il punto di interruzione nel `GetProductsWithPriceLessThan` metodo è stato fatto clic ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image68.png))


Affinché un oggetto di database SQL da sottoporre a debug attraverso un'applicazione client, è fondamentale che il database deve essere configurato per supportare il debug dell'applicazione. Fare doppio clic sul database in Esplora Server e assicurarsi che sia selezionata l'opzione di debug dell'applicazione. Inoltre, è necessario configurare l'applicazione ASP.NET per l'integrazione con il Debugger di SQL e per disabilitare il pool di connessioni. Questi passaggi sono stati illustrati in dettaglio nel passaggio 2 della [debug di Stored procedure](debugging-stored-procedures-vb.md) esercitazione.

Dopo aver configurato il database e l'applicazione ASP.NET, impostare il sito Web ASP.NET come progetto di avvio e avviare il debug. Se si visita una pagina che chiama uno degli oggetti gestiti con un punto di interruzione, l'applicazione verrà interrotta e controllo verrà attivato il debugger, in cui è possibile eseguire il codice come illustrato nella figura 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Passaggio 13: Manualmente la compilazione e distribuzione di oggetti di Database gestiti

Progetti di SQL Server rendono più semplice creare, compilare e distribuire oggetti di database gestiti. Sfortunatamente, i progetti SQL Server sono disponibili solo nelle edizioni Professional e sistemi del Team di Visual Studio. Se si utilizza l'edizione Standard di Visual Studio o Visual Web Developer e si desidera utilizzare oggetti di database gestiti, è necessario creare manualmente e distribuirli. Ciò prevede quattro passaggi:

1. Creare un file che contiene il codice sorgente per l'oggetto di database gestito,
2. Compilare l'oggetto in un assembly,
3. Registrare l'assembly con il database di SQL Server 2005, e
4. Creare un oggetto di database in SQL Server che punta al metodo appropriato nell'assembly.

Per illustrare queste attività, consentire s di creare un nuovo oggetto gestito stored procedure che restituisce i prodotti il cui `UnitPrice` è maggiore del valore specificato. Creare un nuovo file nel computer denominato `GetProductsWithPriceGreaterThan.vb` e immettere il codice seguente nel file (è possibile usare Visual Studio, il blocco note o qualsiasi editor di testo per eseguire questa operazione):


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample17.vb)]

Questo codice è quasi identico a quello del `GetProductsWithPriceLessThan` metodo creato nel passaggio 5. Le uniche differenze sono i nomi di metodo, il `WHERE` clausola e il nome del parametro usato nella query. Nel `GetProductsWithPriceLessThan` metodo, il `WHERE` clausola leggere: `WHERE UnitPrice < @MaxPrice`. In questo caso, nella `GetProductsWithPriceGreaterThan`, usiamo: `WHERE UnitPrice > @MinPrice` .

È ora necessario compilare questa classe in un assembly. Dalla riga di comando, passare alla directory in cui è stato salvato il `GetProductsWithPriceGreaterThan.vb` file e usare il compilatore c# (`csc.exe`) per compilare il file di classe in un assembly:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample18.cmd)]

Se la cartella contenente v `bc.exe` in non incluso nel sistema s `PATH`, sarà necessario fare riferimento completamente il relativo percorso, `%WINDOWS%\Microsoft.NET\Framework\version\`, come illustrato di seguito:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample19.cmd)]


[![Compilare GetProductsWithPriceGreaterThan.vb in un Assembly](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image69.png)

**Figura 29**: compilare `GetProductsWithPriceGreaterThan.vb` in un Assembly ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image71.png))


Il `/t` flag specifica che il file di classe Visual Basic deve essere compilato in una DLL (anziché un file eseguibile). Il `/out` flag specifica il nome dell'assembly risultante.

> [!NOTE]
> Anziché compilare il `GetProductsWithPriceGreaterThan.vb` dalla riga di comando in alternativa, è possibile usare file di classe [Visual Basic Express Edition](https://msdn.microsoft.com/vstudio/express/vb/) o creare un progetto libreria di classi distinto in Visual Studio Standard Edition. S ren Jacob Lauritsen, ha reso disponibile di un progetto di Visual Basic Express Edition con il codice per il `GetProductsWithPriceGreaterThan` stored procedure e i due gestito stored procedure e funzioni definite dall'utente creato nei passaggi 3, 5 e 10. S ren s progetto include anche i comandi T-SQL necessari per aggiungere gli oggetti di database corrispondenti.


Con il codice compilato in un assembly, siamo pronti registrare l'assembly all'interno del database di SQL Server 2005. Questa operazione può essere eseguita tramite T-SQL, usando il comando `CREATE ASSEMBLY`, o tramite SQL Server Management Studio. Menu di scelta s tramite Management Studio.

Management Studio espandere la cartella programmabilità del database Northwind. Uno dei relativa sottocartella è assembly. Per aggiungere manualmente un nuovo Assembly al database, fare clic sulla cartella assembly e scegliere Nuovo Assembly dal menu di scelta rapida. Questo consente di visualizzare il nuovo Assembly dialogo (vedere Figura 30). Fare clic sul pulsante Sfoglia per selezionare il `ManuallyCreatedDBObjects.dll` assembly abbiamo appena compilato e quindi fare clic su OK per aggiungere l'Assembly al database. Non dovrebbe essere visualizzato il `ManuallyCreatedDBObjects.dll` assembly in Esplora oggetti.


[![Aggiungere l'Assembly ManuallyCreatedDBObjects.dll al Database](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image72.png)

**Figura 30**: aggiungere i `ManuallyCreatedDBObjects.dll` Assembly al Database ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image74.png))


![Il ManuallyCreatedDBObjects.dll è elencato in Esplora oggetti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image75.png)

**Figura 31**: il `ManuallyCreatedDBObjects.dll` è elencato in Esplora oggetti


È stata aggiunta l'assembly al database Northwind, è ancora stato associare una stored procedure con il `GetProductsWithPriceGreaterThan` metodo nell'assembly. A tale scopo, aprire una nuova finestra query ed eseguire lo script seguente:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample20.sql)]

Verrà creata una nuova stored procedure nel database Northwind denominato `GetProductsWithPriceGreaterThan` e lo associa il metodo gestito `GetProductsWithPriceGreaterThan` (ovvero nella classe `StoredProcedures`, ovvero nell'assembly `ManuallyCreatedDBObjects`).

Dopo avere eseguito lo script precedente, aggiornare la cartella di Stored procedure in Esplora oggetti. Verrà visualizzata una nuova voce di stored procedure - `GetProductsWithPriceGreaterThan` -che dispone di un'icona di lucchetto accanto a esso. Per testare questa stored procedure, immettere ed eseguire lo script seguente nella finestra di query:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample21.sql)]

Come illustrato nella figura 32, il comando precedente Visualizza informazioni per i prodotti con un `UnitPrice` maggiore di $24.95.


[![Il ManuallyCreatedDBObjects.dll è elencato in Esplora oggetti](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image76.png)

**Figura 32**: il `ManuallyCreatedDBObjects.dll` è elencato in Esplora oggetti ([fare clic per visualizzare l'immagine con dimensioni normali](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image78.png))


## <a name="summary"></a>Riepilogo

Microsoft SQL Server 2005 fornisce l'integrazione con il Common Language Runtime (CLR), che consente agli oggetti di database deve essere creato usando il codice gestito. In precedenza, questi oggetti di database è stato possibile creare solo tramite T-SQL, ma ora è possibile creare questi oggetti usando linguaggi di programmazione quali Visual Basic .NET. In questa esercitazione che abbiamo creato due gestito stored procedure e una funzione definita dall'utente gestito.

S tipo di progetto di SQL Server a Visual Studio facilita la creazione, la compilazione e distribuzione di oggetti di database gestito. Inoltre, offre supporto avanzato per il debug. Tuttavia, i tipi di progetto di SQL Server sono disponibili solo nelle edizioni Professional e sistemi del Team di Visual Studio. Per coloro tramite Visual Web Developer o l'edizione Standard di Visual Studio, la creazione, compilazione e i passaggi di distribuzione deve essere eseguito manualmente, come mostrato nel passaggio 13.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Vantaggi e svantaggi delle funzioni definite dall'utente](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Creazione di oggetti di SQL Server 2005 nel codice gestito](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Creazione di trigger tramite codice gestito in SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Procedura: Creare ed eseguire un CLR di SQL Server Stored Procedure](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Procedura: Creare ed eseguire una funzione definita dall'utente di CLR di SQL Server](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Procedura: Modificare il `Test.sql` Script per l'esecuzione di oggetti di SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Funzioni definite dall'introduzione all'utente](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Codice gestito e SQL Server 2005 (Video)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Riferimento Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Procedura dettagliata: Creazione di una Stored Procedure nel codice gestito](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata S ren Jacob Lauritsen. Oltre a esaminare questo articolo, S ren anche creato il progetto di Visual c# Express Edition incluso in questo articolo s download per la compilazione manualmente gli oggetti di database gestito. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](debugging-stored-procedures-vb.md)
