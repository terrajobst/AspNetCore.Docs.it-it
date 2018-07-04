---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Con esistenti delle Stored procedure per i TableAdapter del set di dati tipizzato (VB) | Microsoft Docs
author: rick-anderson
description: Nell'esercitazione precedente abbiamo appreso come usare la configurazione guidata TableAdapter per generare nuove stored procedure. In questa esercitazione si apprenderà come stesso TableAdapter...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: a478b52387a6bbf726872978a0ad4a83c22c9911
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382896"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Con esistenti delle Stored procedure per i TableAdapter del set di dati tipizzato (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) o [Scarica il PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> Nell'esercitazione precedente abbiamo appreso come usare la configurazione guidata TableAdapter per generare nuove stored procedure. In questa esercitazione contiene informazioni come la stessa configurazione guidata TableAdapter possono usare le stored procedure esistente. È anche informazioni su come aggiungere manualmente nuove stored procedure al database.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) abbiamo visto come DataSet tipizzata s TableAdapter è stato possibile configurare per l'uso di istruzioni SQL anziché ad-hoc i dati di accesso delle stored procedure. In particolare, abbiamo esaminato come impostare la configurazione guidata TableAdapter per creare automaticamente queste stored procedure. Durante il trasferimento di un'applicazione legacy in ASP.NET 2.0 o durante la creazione di un sito Web di ASP.NET 2.0 intorno a un modello di dati esistente, è probabile che il database contiene già le stored procedure che è necessario. In alternativa, è preferibile creare le stored procedure manualmente o tramite uno strumento diverso dalla configurazione guidata TableAdapter che genera automaticamente la stored procedure.

In questa esercitazione si esaminerà come configurare il TableAdapter per l'utilizzo di stored procedure esistenti. Poiché il database Northwind ha solo un piccolo set di stored procedure incorporate, si esaminerà anche i passaggi necessari per aggiungere manualmente nuove stored procedure nel database tramite l'ambiente di Visual Studio. Introduzione a ti permettono di s.

> [!NOTE]
> Nel [di wrapping delle modifiche al Database in una transazione](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) esercitazione metodi è stato aggiunto all'oggetto TableAdapter per supportare le transazioni (`BeginTransaction`, `CommitTransaction`e così via). In alternativa, le transazioni possono essere gestite completamente all'interno di una stored procedure, che non richiede modifiche al codice di livello di accesso ai dati. In questa esercitazione illustra i comandi T-SQL utilizzati per eseguire istruzioni di s una stored procedure all'interno dell'ambito di una transazione.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Passaggio 1: Aggiunta di Stored procedure al Database Northwind

Visual Studio è facile aggiungere nuove stored procedure a un database. S ti permettono di aggiungere una nuova stored procedure al database Northwind che restituisce tutte le colonne dai `Products` tabella per coloro che hanno un particolare `CategoryID` valore. Dalla finestra Esplora Server, espandere il database Northwind in modo che le relative cartelle - diagrammi di Database, tabelle, viste e così via - vengono visualizzate. Come illustrato nell'esercitazione precedente, la cartella di Stored procedure contiene il database s stored procedure esistenti. Per aggiungere una nuova stored procedure, è sufficiente fare doppio clic su della cartella Stored Procedures e scegliere l'opzione Aggiungi nuova Stored Procedure dal menu di scelta rapida.


[![Fare clic sulla cartella la Stored procedure e aggiungere una nuova Stored Procedure](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figura 1**: fare doppio clic nella cartella Stored procedure e aggiungere una nuova Stored Procedure ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


Come illustrato nella figura 1, se si seleziona l'opzione Aggiungi nuova Stored Procedure apre una finestra di script in Visual Studio con la struttura dello script SQL necessari per creare la stored procedure. È il processo per approfondire questo script ed eseguirlo, a questo punto la stored procedure verrà aggiunto al database.

Immettere lo script seguente:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Questo script, quando eseguita, verrà aggiunto una nuova stored procedure nel database Northwind denominato `Products_SelectByCategoryID`. Questa stored procedure accetta un parametro di input (`@CategoryID`, di tipo `int`) e restituisce tutti i campi per i prodotti con un corrispondente `CategoryID` valore.

Per eseguire questa operazione `CREATE PROCEDURE` script e aggiungere stored procedure nel database, fare clic sull'icona Salva nella barra degli strumenti o premere Ctrl + S. Al termine dell'operazione, aggiornare la cartella Stored procedure che mostra l'oggetto appena creato di stored procedure. Inoltre, lo script nella finestra cambierà sottigliezza dal `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` al `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Aggiunge una nuova stored procedure nel database, mentre `ALTER PROCEDURE` ne aggiorna uno esistente. Poiché l'inizio dello script è stato modificato per `ALTER PROCEDURE`, modificare le stored procedure di input parametri o istruzioni SQL e facendo clic sull'icona Salva aggiornerà la stored procedure con queste modifiche.

Figura 2 mostra Visual Studio dopo la `Products_SelectByCategoryID` stored procedure è stata salvata.


[![La Stored Procedure Products_SelectByCategoryID è stato aggiunto al Database](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Figura 2**: la Stored Procedure `Products_SelectByCategoryID` è stato aggiunto al Database ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Passaggio 2: Configurazione del TableAdapter per l'uso di una Stored Procedure esistente

Ora che il `Products_SelectByCategoryID` stored procedure è stato aggiunto al database, è possibile configurare il livello di accesso ai dati per utilizzare questa stored procedure quando uno dei relativi metodi viene richiamato. In particolare, si aggiungerà un `GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->` metodo per il `ProductsTableAdapter` nel `NorthwindWithSprocs` DataSet tipizzato che chiama il `Products_SelectByCategoryID` stored procedure appena creato.

Iniziare aprendo il `NorthwindWithSprocs` set di dati. Fare clic su di `ProductsTableAdapter` e scegliere Aggiungi Query per avviare la configurazione guidata Query TableAdapter. Nel [esercitazione precedente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) Abbiamo optato per creare una nuova stored procedure per noi TableAdapter. Per questa esercitazione, tuttavia, si vuole associare il nuovo metodo TableAdapter esistente `Products_SelectByCategoryID` stored procedure. Pertanto, scegliere l'opzione della stored procedure esistente usare innanzitutto la procedura guidata s e quindi fare clic su Avanti.


[![Scegliere l'utilizzo di stored procedure opzione esistente](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Figura 3**: scegliere stored procedure opzione di utilizzo esistente ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


La schermata seguente fornisce che un elenco di riepilogo a discesa popolato con il database s stored procedure. Selezione di una stored procedure sono elencati i parametri di input a sinistra e i campi di dati restituiti (se presenti) a destra. Scegliere il `Products_SelectByCategoryID` stored procedure dall'elenco e fare clic su Avanti.


[![Selezionare il Products_SelectByCategoryID Stored Procedure](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Figura 4**: selezionare il `Products_SelectByCategoryID` Stored Procedure ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


Nella schermata successiva viene chiesto di specificare il tipo di dati viene restituito dalla stored procedure e la risposta di seguito determina il tipo restituito dal metodo s TableAdapter. Ad esempio, se si indicano che vengono restituiti dati tabulari, il metodo restituirà un `ProductsDataTable` istanza popolata con i record restituiti dalla stored procedure. Al contrario, se viene indicato che questa stored procedure restituisce un singolo valore dell'oggetto TableAdapter verrà restituito un `Object` che viene assegnato il valore nella prima colonna del primo record restituito dalla stored procedure.

Poiché il `Products_SelectByCategoryID` stored procedure restituisce tutti i prodotti che appartengono a una determinata categoria, scegliere la prima risposta - TSD - e fare clic su Avanti.


[![Indicare che la Stored Procedure restituisce dati tabulari](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Figura 5**: indica che la Stored Procedure restituisce dati tabellari ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


Questo punto, è per indicare quali modelli di metodo da usare seguita dai nomi per questi metodi. Lasciare il riempimento di entrambi un DataTable e restituire le opzioni di una DataTable verificata, ma i metodi da rinominare `FillByCategoryID` e `GetProductsByCategoryID`. Quindi fare clic su Avanti per esaminare un riepilogo delle attività che verrà eseguita la procedura guidata. Se tutto sembra corretto, fare clic su Fine.


[![Nome FillByCategoryID i metodi e GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figura 6**: nome dei metodi `FillByCategoryID` e `GetProductsByCategoryID` ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> I metodi TableAdapter appena creato `FillByCategoryID` e `GetProductsByCategoryID`, prevede un parametro di input di tipo `Integer`. Questo valore di parametro di input viene passato alla stored procedure tramite relativo `@CategoryID` parametro. Se si modifica il `Products_SelectByCategory` parametri stored procedure s, è necessario aggiornare anche i parametri per questi metodi TableAdapter. Come descritto nel [esercitazione precedente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), questa operazione può essere eseguita in uno dei due modi: manualmente aggiungendo o rimuovendo i parametri dalla raccolta di parametri o eseguendo nuovamente la configurazione guidata TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Passaggio 3: Aggiunta di un`GetProductsByCategoryID(categoryID)`per il livello BLL (metodo)

Con la `GetProductsByCategoryID` metodo DAL completo, il passaggio successivo consiste nel fornire l'accesso a questo metodo nel livello di logica di Business. Aprire il `ProductsBLLWithSprocs` file di classe e aggiungere il metodo seguente:


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Questo metodo BLL restituisce semplicemente il `ProductsDataTable` restituiti dai `ProductsTableAdapter` s `GetProductsByCategoryID` (metodo). Il `DataObjectMethodAttribute` attributo fornisce metadati utilizzati dalla procedura guidata Configura origine dati s ObjectDataSource. In particolare, questo metodo verrà visualizzato nell'elenco a discesa scheda Seleziona s.

## <a name="step-4-displaying-products-by-category"></a>Passaggio 4: Visualizzazione dei prodotti per categoria

Per eseguire il test appena aggiunta `Products_SelectByCategoryID` stored procedure e i metodi DAL e BLL corrispondenti, consentire s di creare una pagina ASP.NET che contiene un controllo DropDownList e un controllo GridView. DropDownList elencherà tutte le categorie nel database durante il controllo GridView verrà visualizzati i prodotti appartenenti alla categoria selezionata.

> [!NOTE]
> Abbiamo interfacce master/dettaglio ve create utilizzando controlli DropDownList nelle esercitazioni precedenti. Per un'analisi più approfondita che implementa questo rapporto master/dettagli, vedere la [Master/Dettagli filtro con un controllo DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) esercitazione.


Aprire il `ExistingSprocs.aspx` nella pagina di `AdvancedDAL` cartelle e trascinare un controllo DropDownList dalla casella degli strumenti nella finestra di progettazione. Impostare la s DropDownList `ID` proprietà `Categories` e il relativo `AutoPostBack` proprietà `True`. Successivamente, dal suo smart tag, associare il controllo DropDownList per un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource`. Configurare ObjectDataSource in modo che recupera i dati dal `CategoriesBLL` classe s `GetCategories` (metodo). Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno).


[![Recuperare i dati dal metodo GetCategories CategoriesBLL classe s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figura 7**: recupera i dati dal `CategoriesBLL` classe s `GetCategories` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Figura 8**: impostare l'elenco a discesa sono elencati nell'aggiornamento, inserimento ed eliminare schede su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


Dopo aver completato la procedura guidata ObjectDataSource, configurare il controllo DropDownList per visualizzare il `CategoryName` campo dati e usare il `CategoryID` campo come le `Value` per ogni `ListItem`.

A questo punto, il markup dichiarativo s DropDownList e ObjectDataSource deve simile al seguente:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Successivamente, trascinare un controllo GridView nella finestra di progettazione, posizionandolo sotto il controllo DropDownList. Impostare la s GridView `ID` al `ProductsByCategory` e, dal suo smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsByCategoryDataSource`. Configurare il `ProductsByCategoryDataSource` ObjectDataSource per usare il `ProductsBLLWithSprocs` (classe), averlo recuperare i relativi dati tramite la `GetProductsByCategoryID(categoryID)` (metodo). Perché questo GridView verranno usati solo per visualizzare i dati, impostare gli elenchi a discesa nell'aggiornamento, inserimento, eliminare le schede su (nessuno) e fare clic su Avanti.


[![Configurare ObjectDataSource per usare la classe ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Figura 9**: configurare ObjectDataSource per usare il `ProductsBLLWithSprocs` classe ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![Recuperare i dati dal metodo GetProductsByCategoryID(categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Figura 10**: recupera i dati dal `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


Il metodo scelto nella scheda Seleziona prevede un parametro, in modo che il passaggio finale della procedura guidata richiede per l'origine di s parametri. Impostare l'elenco di riepilogo a discesa Origine parametro al controllo e scegliere il `Categories` controllo dall'elenco a discesa ControlID. Fare clic su Fine per completare la procedura guidata.


[![Utilizzare il controllo DropDownList categorie come origine il parametro CategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figura 11**: usare il `Categories` DropDownList come origine della `categoryID` parametro ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


Dopo aver completato la procedura guidata ObjectDataSource, Visual Studio aggiungerà BoundField e un CampoCasellaDiControllo per ognuno dei campi di dati del prodotto. È possibile personalizzare questi campi nel modo desiderato.

Visitare la pagina tramite un browser. Quando si visita la pagina che sia selezionata la categoria Beverages e i prodotti corrispondenti elencati nella griglia. Modifica dell'elenco di riepilogo a discesa a una categoria alternativa, come nella figura 12 mostra, causa un postback e ricarica la griglia con i prodotti della categoria appena selezionata.


[![Vengono visualizzati i prodotti nella categoria di produrre](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figura 12**: vengono visualizzati di prodotti nella categoria di produrre ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Passaggio 5: Ritorno a capo le istruzioni s una Stored Procedure all'interno dell'ambito di una transazione

Nel [di wrapping delle modifiche al Database in una transazione](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) esercitazione viene illustrato le tecniche per l'esecuzione di una serie di istruzioni di modifica di database all'interno dell'ambito di una transazione. Tenere presente che le modifiche eseguite sia in quello di una transazione tutti esito positivo o negativo tutti, garantire atomicità. Tecniche per l'uso di transazioni includono:

- Utilizzando le classi di `System.Transactions` dello spazio dei nomi,
- Con il livello di accesso ai dati usare classi ADO.NET come `SqlTransaction`, e
- Aggiunta di comandi di transazione T-SQL direttamente all'interno di stored procedure

Il *di wrapping delle modifiche al Database in una transazione* esercitazione ha usato le classi ADO.NET nel DAL. Nella parte restante di questa esercitazione viene esaminato come gestire una transazione usando i comandi T-SQL all'interno di una stored procedure.

I tre comandi SQL chiave per l'avvio manuale, eseguire il commit e rollback di una transazione vengono `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, e `ROLLBACK TRANSACTION`, rispettivamente. Ad esempio con l'approccio ADO.NET, durante l'utilizzo di transazioni all'interno di una stored procedure è necessario applicare il modello seguente:

1. Indicare l'inizio di una transazione.
2. Eseguire le istruzioni SQL che costituiscono la transazione.
3. Se si verifica un errore in una delle istruzioni dal passaggio 2, eseguire il rollback della transazione.
4. Se tutte le istruzioni del passaggio 2 viene completata senza errori, eseguire il commit della transazione.

Questo modello può essere implementato nella sintassi T-SQL usando il modello seguente:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Il modello inizia definendo un `TRY...CATCH` block, un costrutto di nuovo a SQL Server 2005. Ad esempio con `Try...Catch` blocchi in Visual Basic, il codice SQL `TRY...CATCH` blocco esegue le istruzioni nel `TRY` blocco. Se qualsiasi istruzione genera un errore, il controllo viene trasferito a immediatamente il `CATCH` blocco.

Se non sono presenti errori di esecuzione delle istruzioni SQL che composizione della transazione, il `COMMIT TRANSACTION` istruzione esegue il commit delle modifiche e completa la transazione. Se, tuttavia, una delle istruzioni provoca un errore, il `ROLLBACK TRANSACTION` nella `CATCH` blocco restituisce il database allo stato precedente l'inizio della transazione. La stored procedure genera anche un errore usando il [comando RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), determinando in tal modo un `SqlException` a essere generati nell'applicazione.

> [!NOTE]
> Poiché il `TRY...CATCH` blocco è una novità di SQL Server 2005, il modello precedente non funzionerà se si usano versioni precedenti di Microsoft SQL Server. Se non si usa SQL Server 2005, consultare [gestione delle transazioni in Stored procedure di SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) per un modello che funzionerà con altre versioni di SQL Server.


Let s esaminato un esempio concreto. Esiste un vincolo di chiave esterno tra le `Categories` e `Products` tabelle, vale a dire che ogni `CategoryID` campo il `Products` tabella devono essere associati a un `CategoryID` valore nel `Categories` tabella. Qualsiasi azione che viola questo vincolo, ad esempio tenta di eliminare una categoria a cui è associati i prodotti, comporta una violazione di vincolo di chiave esterna. Per verificarlo, visitare di nuovo l'esempio di aggiornamento ed eliminazione di dati binari esistenti nell'uso di sezione di dati binari (`~/BinaryData/UpdatingAndDeleting.aspx`). Questa pagina elenca ogni categoria nel sistema unitamente a pulsanti di modifica ed eliminazione (vedere la figura 13), ma se si tenta di eliminare una categoria a cui è associati i prodotti - ad esempio bibite - l'eliminazione ha esito negativo a causa di una violazione di vincolo di chiave esterna (vedere la figura 14).


[![Ogni categoria viene visualizzata in un controllo GridView con i pulsanti Elimina e modifica](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Figura 13**: ogni categoria viene visualizzata in un controllo GridView con i pulsanti Elimina e modifica ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![Non è possibile eliminare una categoria con prodotti esistenti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Figura 14**: non è possibile eliminare una categoria con prodotti esistenti ([fare clic per visualizzare l'immagine con dimensioni normali](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


Poniamo, tuttavia, che si desidera consentire categorie deve essere eliminato indipendentemente dal fatto che vengono prodotti associati. Deve essere eliminata una categoria con i prodotti, si supponga che si desidera eliminare anche i relativi prodotti esistenti (anche se un'altra opzione possibile è sufficiente impostare i propri prodotti `CategoryID` valori `NULL`). Questa funzionalità potrebbe essere implementata tramite le regole cascade del vincolo di chiave esterna. In alternativa, è possibile creare una stored procedure che accetta un `@CategoryID` parametro di input e, quando richiamata, in modo esplicito consente di eliminare tutti i prodotti associati e quindi la categoria specificata.

Il primo tentativo di una stored procedure potrebbe essere simile al seguente:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Durante questa operazione eliminerà definitivamente i prodotti associati e categoria, lo scopo non in quello di una transazione. Si supponga che vi sia un vincolo di chiave esterna nel `Categories` che non consente l'eliminazione di un determinato `@CategoryID` valore. Il problema è che in questo caso tutti i prodotti verranno eliminati prima di tentare di eliminare la categoria. Il risultato finale è che per una categoria, questa stored procedure potrebbe rimuovere tutti i relativi mentre la categoria è rimasta dal momento che ancora dispone di record correlati in un'altra tabella.

Se la stored procedure sono stata incapsulata all'interno dell'ambito di una transazione, tuttavia, le operazioni di eliminazione il `Products` tabella verrebbe eseguito il rollback in caso di un'operazione di eliminazione non riuscita `Categories`. Il seguente script di stored procedure Usa una transazione per garantire l'atomicità tra i due `DELETE` istruzioni:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Si consiglia di aggiungere il `Categories_Delete` stored procedure per il database Northwind. Fare riferimento al passaggio 1 per istruzioni sull'aggiunta di stored procedure a un database.

## <a name="step-6-updating-thecategoriestableadapter"></a>Passaggio 6: L'aggiornamento di`CategoriesTableAdapter`

Mentre è stato aggiunto il `Categories_Delete` stored procedure nel database, DAL è attualmente configurata per utilizzare istruzioni SQL ad hoc per eseguire l'eliminazione. È necessario aggiornare il `CategoriesTableAdapter` e fare in modo di usare il `Categories_Delete` stored procedure di invece.

> [!NOTE]
> In precedenza in questa esercitazione si lavora con i `NorthwindWithSprocs` set di dati. Ma tale set di dati contiene solo una singola entità, `ProductsDataTable`, ed è necessario usare le categorie. Pertanto, per il resto di questa esercitazione quando parla di m Data Access Layer ricerca per categorie che fa riferimento al `Northwind` set di dati, quello che abbiamo creato prima di tutto nel [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) esercitazione.


Aprire il Northwind DataSet, selezionare il `CategoriesTableAdapter`e passare alla finestra Proprietà. Gli elenchi di finestra delle proprietà di `InsertCommand`, `UpdateCommand`, `DeleteCommand`, e `SelectCommand` utilizzata dal TableAdapter, nonché informazioni relative a nome e connessione. Espandere il `DeleteCommand` proprietà per visualizzarne i dettagli. Come illustrato nella figura 15, il `DeleteCommand` s `ComamndType` è impostata su testo, che indica a inviare il testo `CommandText` proprietà come query SQL ad hoc.


![Selezionare il CategoriesTableAdapter nella finestra di progettazione per visualizzarne le proprietà nella finestra proprietà](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Figura 15**: selezionare il `CategoriesTableAdapter` nella finestra di progettazione per visualizzarne le proprietà nella finestra proprietà


Per modificare queste impostazioni, selezionare il testo (DeleteCommand) nella finestra proprietà e scegliere (nuova) dall'elenco a discesa. Ciò consente di correggere le impostazioni per il `CommandText`, `CommandType`, e `Parameters` proprietà. Successivamente, impostare il `CommandType` proprietà `StoredProcedure` e quindi digitare il nome di stored procedure per il `CommandText` (`dbo.Categories_Delete`). Se è assicurarsi di immettere le proprietà nell'ordine seguente: innanzitutto le `CommandType` e quindi il `CommandText` -Visual Studio verrà popolato automaticamente la raccolta di parametri. Se non si immette queste proprietà in questo ordine, è necessario aggiungere manualmente i parametri tramite l'Editor raccolta di parametri. In entrambi i casi, è consigliabile fare clic sui puntini di sospensione nella proprietà di parametri per visualizzare Editor raccolta di parametri per verificare che siano state apportate le modifiche alle impostazioni di parametro corretti (vedere Figura 16) s. Se non è possibile visualizzare i parametri nella finestra di dialogo, aggiungere il `@CategoryID` parametro manualmente (non è necessario aggiungere il `@RETURN_VALUE` parametro).


![Assicurarsi che le impostazioni di parametri siano corretti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figura 16**: assicurarsi che le impostazioni di parametri siano corretti


Dopo aver aggiornato DAL, l'eliminazione di una categoria verrà eliminare tutti i relativi prodotti associati automaticamente e farlo in quello di una transazione. Per verificarlo, fare clic sul pulsante Elimina per una delle categorie di tornare alla pagina di aggiornamento ed eliminazione di dati binari esistenti. Con un solo clic del mouse, la categoria e tutti i relativi prodotti associati verranno eliminati.

> [!NOTE]
> Prima di testare il `Categories_Delete` stored procedure, che verrà eliminato un numero di prodotti insieme alla categoria selezionata, potrebbe essere opportuno eseguire una copia di backup del database. Se si usa la `NORTHWND.MDF` del database nel `App_Data`, è sufficiente chiudere Visual Studio e copiare i file MDF e LDF in `App_Data` a un'altra cartella. Dopo aver testato la funzionalità, è possibile ripristinare il database, chiudere Visual Studio e sostituendo corrente MDF e LDF file `App_Data` con le copie di backup.


## <a name="summary"></a>Riepilogo

Durante la configurazione guidata TableAdapter s genererà automaticamente le stored procedure per noi, vi sono casi quando si potrebbe essere già tali stored procedure vengono creati o desidera crearle manualmente o con altri strumenti invece. Per consentire questi scenari, l'oggetto TableAdapter può essere configurato anche per puntare a una stored procedure esistente. In questa esercitazione è stato illustrato come aggiungere manualmente le stored procedure a un database tramite l'ambiente di Visual Studio e su come collegare i metodi di s TableAdapter a queste stored procedure. Abbiamo anche esaminato il modello di script usato per avviare, eseguire il commit e rollback delle transazioni all'interno di una stored procedure e comandi T-SQL.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Hilton Geisenow ren S Lauritsen Jacob e Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Successivo](updating-the-tableadapter-to-use-joins-vb.md)
