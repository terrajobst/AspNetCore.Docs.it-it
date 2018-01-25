---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Usando Stored procedure per gli oggetti TableAdapter del DataSet tipizzato (VB) | Documenti Microsoft
author: rick-anderson
description: "Nell'esercitazione precedente è stato descritto come utilizzare la configurazione guidata TableAdapter per generare nuove stored procedure. In questa esercitazione viene illustrato come stesso TableAdapter..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d1be6c30cda5a06087516210a77f48b6a3fe45b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Usando Stored procedure per gli oggetti TableAdapter del DataSet tipizzato (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) o [Scarica il PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> Nell'esercitazione precedente è stato descritto come utilizzare la configurazione guidata TableAdapter per generare nuove stored procedure. In questa esercitazione è illustrato l'utilizzo di stored procedure esistenti la configurazione guidata TableAdapter stesso. È inoltre illustrato come aggiungere manualmente le nuove stored procedure per il database.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) è stato illustrato come è stato possibile configurare TableAdapter s DataSet tipizzato per l'utilizzo di stored procedure in istruzioni SQL dati anziché ad hoc di accesso. In particolare, abbiamo esaminato come disporre la configurazione guidata TableAdapter di creare automaticamente le stored procedure. Durante il porting di un'applicazione legacy a ASP.NET 2.0 o durante la compilazione di un sito Web di ASP.NET 2.0 intorno a un modello di dati esistente, probabilità sono che il database contiene già le stored procedure che è necessario. In alternativa, è preferibile creare le stored procedure manualmente o tramite uno strumento diverso dalla configurazione guidata TableAdapter che genera automaticamente le stored procedure.

In questa esercitazione verranno esaminati come configurare il TableAdapter per l'utilizzo di stored procedure esistenti. Poiché il database Northwind ha solo un piccolo set di stored procedure incorporate, verranno inoltre esaminati i passaggi necessari per aggiungere manualmente le nuove stored procedure nel database tramite l'ambiente di Visual Studio. Let s iniziare!

> [!NOTE]
> Nel [di wrapping delle modifiche del Database all'interno di una transazione](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) esercitazione metodi è stato aggiunto all'oggetto TableAdapter per supportare le transazioni (`BeginTransaction`, `CommitTransaction`e così via). In alternativa, è possibile gestire le transazioni completamente all'interno di una stored procedure, che non richiede modifiche al codice a livello di accesso ai dati. In questa esercitazione si esamineranno i comandi T-SQL utilizzati per eseguire istruzioni di s una stored procedure all'interno dell'ambito di una transazione.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Passaggio 1: Aggiunta di Stored procedure nel database Northwind

Visual Studio è facile aggiungere nuove stored procedure a un database. S consentono di aggiungere una nuova stored procedure al database Northwind che restituisce tutte le colonne di `Products` tabella per quelli che dispongono di un particolare `CategoryID` valore. Dalla finestra di Esplora Server, espandere il database Northwind in modo che le cartelle - diagrammi di Database, tabelle, viste e così via, vengono visualizzate. Come illustrato nell'esercitazione precedente, la cartella di Stored procedure contiene il database s stored procedure esistenti. Per aggiungere una nuova stored procedure, fare clic sulla cartella la Stored procedure e scegliere l'opzione Aggiungi nuova Stored Procedure dal menu di scelta rapida.


[![Fare clic sulla cartella la Stored procedure e aggiungere una nuova Stored Procedure](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figura 1**: fare clic sulla cartella Stored procedure e aggiungere una nuova Stored Procedure ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


Come illustrato nella figura 1, si seleziona l'opzione Aggiungi nuova Stored Procedure Visualizza una finestra di script in Visual Studio con la struttura dello script SQL necessari per creare la stored procedure. È il processo per approfondire questo script ed eseguirlo, a quel punto verrà aggiunto al database della stored procedure.

Immettere lo script seguente:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Questo script, quando eseguita, verrà aggiunta automaticamente una nuova stored procedure del database Northwind denominato `Products_SelectByCategoryID`. Questa stored procedure accetta un singolo parametro di input (`@CategoryID`, di tipo `int`) e restituisce tutti i campi per i prodotti con un corrispondente `CategoryID` valore.

Per eseguire questa applicazione `CREATE PROCEDURE` script e aggiungere la stored procedure nel database, fare clic sull'icona Salva nella barra degli strumenti o premere Ctrl + S. Al termine dell'operazione, aggiornare la cartella Stored procedure che mostra l'oggetto appena creato di stored procedure. Inoltre, lo script nella finestra viene modificata abbastanza particolare da `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` a `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE`Aggiunge una nuova stored procedure nel database, mentre `ALTER PROCEDURE` aggiorna uno esistente. Poiché l'inizio dello script è stato modificato in `ALTER PROCEDURE`, modificare le stored procedure immettere parametri o istruzioni SQL e facendo clic sull'icona Salva aggiornerà la stored procedure con queste modifiche.

La figura 2 mostra Visual Studio dopo la `Products_SelectByCategoryID` stored procedure è stata salvata.


[![Products_SelectByCategoryID la Stored Procedure è stato aggiunto al Database](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Figura 2**: la Stored Procedure `Products_SelectByCategoryID` è stato aggiunto al Database ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Passaggio 2: Configurazione del TableAdapter per l'utilizzo di una Stored Procedure esistente

Ora che il `Products_SelectByCategoryID` stored procedure è stato aggiunto al database, è possibile configurare il livello di accesso ai dati per utilizzare questa stored procedure quando viene richiamato uno dei relativi metodi. In particolare, verrà aggiunto un `GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->` metodo il `ProductsTableAdapter` nel `NorthwindWithSprocs` DataSet tipizzato che chiama il `Products_SelectByCategoryID` stored procedure che abbiamo appena creato.

Aprire il `NorthwindWithSprocs` set di dati. Fare clic su di `ProductsTableAdapter` e scegliere Aggiungi Query per avviare la configurazione guidata Query TableAdapter. Nel [esercitazione precedente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) è optato per creare una nuova stored procedure per noi TableAdapter. Per questa esercitazione, tuttavia, è necessario collegare il nuovo metodo TableAdapter esistente `Products_SelectByCategoryID` stored procedure. Pertanto, scegliere l'opzione Usa stored procedure esistenti tra il primo passaggio s di procedura guidata e quindi fare clic su Avanti.


[![Scegliere l'utilizzo di stored procedure opzione esistente](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Figura 3**: scegliere esistenti utilizzare stored procedure opzione ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


La schermata seguente fornisce che un elenco a discesa popolato con il database s stored procedure. Selezione di una stored procedure sono elencati i parametri di input a sinistra e i campi di dati restituiti (se presente) a destra. Scegliere il `Products_SelectByCategoryID` stored procedure dall'elenco e fare clic su Avanti.


[![Selezionare il Products_SelectByCategoryID Stored Procedure](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Figura 4**: selezionare il `Products_SelectByCategoryID` Stored Procedure ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


Nella schermata successiva viene chiesto di specificare il tipo di dati viene restituito dalla stored procedure e la risposta qui determina il tipo restituito dal metodo s TableAdapter. Ad esempio, se si indicano che i dati in formato tabulare viene restituiti, il metodo restituirà un `ProductsDataTable` istanza popolata con i record restituiti dalla stored procedure. Al contrario, se si indica che questa stored procedure restituisce un valore singolo TableAdapter verrà restituito un `Object` che viene assegnato il valore nella prima colonna del primo record restituito dalla stored procedure.

Poiché il `Products_SelectByCategoryID` stored procedure restituisce tutti i prodotti che appartengono a una particolare categoria, scegliere la prima risposta - dati tabulari - e fare clic su Avanti.


[![Indicare che la Stored Procedure restituisce dati in formato tabulare](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Figura 5**: indicare che la Stored Procedure restituisce dati tabulari ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


È comunque per indicare quali modelli di metodo da utilizzare seguita dai nomi per questi metodi. Lasciare il riempimento di entrambi, DataTable e restituire un oggetto DataTable di opzioni è selezionata, ma rinominare i metodi per `FillByCategoryID` e `GetProductsByCategoryID`. Fare clic su Avanti per esaminare un riepilogo delle attività che verrà eseguita la procedura guidata. Se tutto sembra corretto, fare clic su Fine.


[![Nome FillByCategoryID i metodi e GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figura 6**: i metodi di nome `FillByCategoryID` e `GetProductsByCategoryID` ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> I metodi TableAdapter appena creato, `FillByCategoryID` e `GetProductsByCategoryID`, prevede un parametro di input di tipo `Integer`. Il valore di parametro di input viene passato alla stored procedure tramite il relativo `@CategoryID` parametro. Se si modifica il `Products_SelectByCategory` parametri di stored procedure s, è necessario aggiornare anche i parametri per i metodi TableAdapter. Come descritto nel [esercitazione precedente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), questa operazione può essere eseguita in uno dei due modi: manualmente aggiungendo o rimuovendo i parametri dalla raccolta di parametri o rieseguendo la configurazione guidata TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Passaggio 3: Aggiunta di un`GetProductsByCategoryID(categoryID)`al livello Business Logic (metodo)

Con la `GetProductsByCategoryID` DAL completo, il passaggio successivo è per fornire l'accesso a questo metodo nel livello di logica di Business. Aprire il `ProductsBLLWithSprocs` file di classe e aggiungere il metodo seguente:


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Questo metodo BLL restituisce semplicemente il `ProductsDataTable` restituito dal `ProductsTableAdapter` s `GetProductsByCategoryID` metodo. Il `DataObjectMethodAttribute` attributo fornisce metadati utilizzati dalla procedura guidata ObjectDataSource s Configura origine dati. In particolare, questo metodo verrà visualizzato nell'elenco scegliere scheda s elenco a discesa.

## <a name="step-4-displaying-products-by-category"></a>Passaggio 4: Visualizzare i prodotti per categoria

Per eseguire il test appena aggiunta `Products_SelectByCategoryID` stored procedure e i metodi DAL e BLL corrispondenti, consente di creare una pagina ASP.NET che contiene un controllo DropDownList e un controllo GridView s. DropDownList elencherà tutte le categorie nel database durante il controllo GridView visualizzerà i prodotti appartenenti alla categoria selezionata.

> [!NOTE]
> È interfacce master-details ve create utilizzando controlli DropDownList nelle esercitazioni precedenti. Per una descrizione più approfondita di tale una relazione master/dettaglio di implementazione, vedere il [Master-Details filtro con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) esercitazione.


Aprire il `ExistingSprocs.aspx` nella pagina di `AdvancedDAL` cartelle e trascinare un controllo DropDownList dalla casella degli strumenti nella finestra di progettazione. Impostare il s DropDownList `ID` proprietà `Categories` e il relativo `AutoPostBack` proprietà `True`. Successivamente, smart tag, di associare DropDownList a un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource`. Configurare ObjectDataSource in modo che recuperi i dati di `CategoriesBLL` classe s `GetCategories` metodo. Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno).


[![Recuperare i dati dal metodo GetCategories CategoriesBLL classe s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figura 7**: recuperare dati dal `CategoriesBLL` classe s `GetCategories` metodo ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Figura 8**: impostare l'elenco a discesa sono elencati nelle schede DELETE, INSERT e UPDATE su (nessuno) ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


Dopo aver completato la procedura guidata ObjectDataSource, configurare DropDownList per visualizzare il `CategoryName` campo dei dati e di utilizzare il `CategoryID` campo come il `Value` per ogni `ListItem`.

A questo punto, il markup dichiarativo s DropDownList e ObjectDataSource deve simile al seguente:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Successivamente, trascinare un controllo GridView nella finestra di progettazione, posizionarlo sotto DropDownList. Impostare i dispositivi di GridView `ID` a `ProductsByCategory` e dal suo smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsByCategoryDataSource`. Configurare il `ProductsByCategoryDataSource` ObjectDataSource per utilizzare il `ProductsBLLWithSprocs` (classe), avendolo recuperare relativi dati tramite il `GetProductsByCategoryID(categoryID)` metodo. Poiché questo GridView verrà utilizzato solo per visualizzare i dati, impostare gli elenchi a discesa nell'aggiornamento, inserimento, eliminare le tabulazioni su (nessuno) e fare clic su Avanti.


[![Configurare ObjectDataSource per utilizzare la classe ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Figura 9**: configurare ObjectDataSource per utilizzare il `ProductsBLLWithSprocs` classe ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![Recuperare i dati dal metodo GetProductsByCategoryID(categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Figura 10**: recuperare dati dal `GetProductsByCategoryID(categoryID)` metodo ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


Il metodo selezionato nella scheda Seleziona prevede un parametro, pertanto il passaggio finale della procedura guidata richiede Microsoft per l'origine di s parametri. Impostare l'elenco di riepilogo a discesa Origine parametro al controllo e scegliere il `Categories` controllo dall'elenco a discesa ControlID. Fare clic su Fine per completare la procedura guidata.


[![Utilizzare DropDownList categorie come origine di categoryID parametro](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figura 11**: utilizzo di `Categories` DropDownList come origine del `categoryID` parametro ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


Dopo avere completato la procedura guidata ObjectDataSource, Visual Studio aggiungerà BoundField e un CheckBoxField per ognuno dei campi dati di prodotto. È possibile personalizzare questi campi in base alle esigenze.

Visitare la pagina tramite un browser. Quando si visita la pagina che è selezionata la categoria delle bevande e i prodotti corrispondenti elencati nella griglia. Modifica dell'elenco di riepilogo a discesa a una categoria alternativa, come nella figura 12 illustra, provoca un postback e ricarica la griglia con i prodotti della categoria selezionata.


[![Vengono visualizzati i prodotti nella categoria produrre](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figura 12**: vengono visualizzati il prodotti nella categoria produrre ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Passaggio 5: Disposizione testo per le istruzioni s una Stored Procedure all'interno dell'ambito di una transazione

Nel [di wrapping delle modifiche del Database all'interno di una transazione](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) esercitazione sono illustrate le tecniche per eseguire una serie di istruzioni di modifica dei database all'interno dell'ambito di una transazione. Tenere presente che le modifiche eseguite nel generico di una transazione, ovvero tutti esito positivo o negativo tutti, garantire l'atomicità. Tecniche per l'utilizzo di transazioni includono:

- Utilizzando le classi di `System.Transactions` dello spazio dei nomi,
- Con il livello di accesso ai dati di utilizzare le classi di ADO.NET come `SqlTransaction`, e
- Aggiungere i comandi di transazione T-SQL direttamente all'interno di stored procedure

Il *di wrapping delle modifiche del Database all'interno di una transazione* esercitazione utilizzate le classi ADO.NET in DAL. Il resto di questa esercitazione viene illustrato come gestire una transazione utilizzando i comandi T-SQL all'interno di una stored procedure.

I tre comandi SQL chiavi per l'avvio manuale, eseguire il commit e rollback di una transazione sono `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, e `ROLLBACK TRANSACTION`, rispettivamente. Come con l'approccio ADO.NET, quando si utilizzano transazioni all'interno di una stored procedure è necessario applicare il modello seguente:

1. Indicare l'inizio di una transazione.
2. Eseguire le istruzioni SQL che costituiscono la transazione.
3. Se si verifica un errore in una delle istruzioni nel passaggio 2, il rollback della transazione.
4. Se tutte le istruzioni nel passaggio 2 viene completata senza errori, eseguire il commit della transazione.

Questo modello può essere implementato nella sintassi T-SQL utilizzando il modello seguente:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

Il modello di avvio definendo un `TRY...CATCH` blocco, un costrutto di nuovo a SQL Server 2005. Come con `Try...Catch` blocchi in Visual Basic, l'istruzione SQL `TRY...CATCH` blocco esegue le istruzioni nel `TRY` blocco. Se qualsiasi istruzione genera un errore, il controllo viene trasferito a immediatamente il `CATCH` blocco.

Se non sono presenti errori di esecuzione delle istruzioni SQL che composizione della transazione, il `COMMIT TRANSACTION` istruzione esegue il commit delle modifiche e completa la transazione. Se, tuttavia, una delle istruzioni genera un errore, il `ROLLBACK TRANSACTION` nel `CATCH` blocco restituisce il database allo stato precedente l'inizio della transazione. La stored procedure genera anche un errore usando il [comando RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), causando un `SqlException` per essere generati nell'applicazione.

> [!NOTE]
> Poiché il `TRY...CATCH` blocco è una novità di SQL Server 2005, il modello riportato sopra non funziona se si utilizzano versioni precedenti di Microsoft SQL Server. Se non si utilizza SQL Server 2005, consultare [gestione delle transazioni in Stored procedure di SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) per un modello che funzionerà con altre versioni di SQL Server.


Consente di esaminare un esempio concreto s. Esiste un vincolo di chiave esterno tra la `Categories` e `Products` tabelle, vale a dire che ogni `CategoryID` campo il `Products` tabella deve essere associato a un `CategoryID` valore il `Categories` tabella. Qualsiasi operazione che violi il vincolo, ad esempio il tentativo di eliminare una categoria di cui è associati i prodotti, comporta una violazione di vincolo di chiave esterna. Per effettuare questa verifica, rivedere l'esempio di aggiornamento ed eliminazione di dati binari esistenti nella sezione Utilizzo di dati binari (`~/BinaryData/UpdatingAndDeleting.aspx`). Questa pagina elenca ogni categoria nel sistema insieme ai pulsanti Modifica e l'eliminazione (vedere Figura 13), ma se si tenta di eliminare una categoria di prodotti, ad esempio bevande - associata l'eliminazione ha esito negativo a causa di una violazione di vincolo di chiave esterna (vedere Figura 14).


[![Ogni categoria viene visualizzata in un controllo GridView con modifica e l'eliminazione di pulsanti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Figura 13**: ogni categoria viene visualizzata in un controllo GridView con modifica e i pulsanti Elimina ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![Non è possibile eliminare una categoria di prodotti esistenti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Nella figura 14**: non è possibile eliminare una categoria di prodotti esistenti ([fare clic per visualizzare l'immagine ingrandita](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


Si supponga, tuttavia, che si desidera consentire le categorie da eliminare indipendentemente dal fatto che essi prodotti associati. Eliminare una categoria di prodotti, si supponga che si desidera eliminare anche i relativi prodotti esistenti (anche se un'altra opzione, è possibile impostare semplicemente i propri prodotti `CategoryID` valori `NULL`). Questa funzionalità può essere implementata tramite le regole cascade del vincolo di chiave esterna. In alternativa, è possibile creare una stored procedure che accetta un `@CategoryID` parametro di input e, quando richiamata, Elimina in modo esplicito tutti i prodotti associati e quindi la categoria specificata.

Il primo tentativo di una stored procedure potrebbe essere simile al seguente:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Mentre la categoria di prodotti associati verranno eliminati definitivamente, non esegue questa operazione in generico di una transazione. Si supponga che vi sia un vincolo di chiave esterno nel `Categories` che potrebbe impedire l'eliminazione di un determinato `@CategoryID` valore. Il problema è che in questo caso tutti i prodotti verranno eliminati prima di tentare di eliminare la categoria. Il risultato è che per una categoria, questa stored procedure potrebbe rimuovere tutti i prodotti mentre la categoria è rimasta poiché ancora dispone di record correlati in un'altra tabella.

Se la stored procedure sono state mandato a capo all'interno dell'ambito di una transazione, tuttavia, le operazioni di eliminazione per il `Products` tabella sarebbe possibile eseguire il rollback in caso di un'operazione di eliminazione non riuscita `Categories`. Lo script di stored procedure seguente utilizza una transazione per garantire l'atomicità tra i due `DELETE` istruzioni:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

È opportuno aggiungere il `Categories_Delete` stored procedure per il database Northwind. Fare riferimento al passaggio 1 per istruzioni sull'aggiunta di stored procedure a un database.

## <a name="step-6-updating-thecategoriestableadapter"></a>Passaggio 6: Aggiornamento di`CategoriesTableAdapter`

Mentre si va aggiunto il `Categories_Delete` stored procedure DAL database, è attualmente configurata per utilizzare istruzioni SQL ad hoc per eseguire l'eliminazione. È necessario aggiornare il `CategoriesTableAdapter` e indicare l'utilizzo di `Categories_Delete` stored procedure di invece.

> [!NOTE]
> In precedenza in questa esercitazione si lavora con la `NorthwindWithSprocs` set di dati. Ma tale set di dati include solo una singola entità, `ProductsDataTable`, è necessario utilizzare le categorie. Pertanto, per il resto di questa esercitazione, quando parla di riferimento a m Data Access Layer I il `Northwind` set di dati, quello creato nel primo il [la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) esercitazione.


Aprire il Northwind DataSet, selezionare il `CategoriesTableAdapter`e passare alla finestra Proprietà. Gli elenchi di finestra delle proprietà di `InsertCommand`, `UpdateCommand`, `DeleteCommand`, e `SelectCommand` utilizzata dal TableAdapter, nonché informazioni relative a nome e connessione. Espandere il `DeleteCommand` proprietà per visualizzarne i dettagli. Come illustrato nella figura 15, il `DeleteCommand` s `ComamndType` è impostata su testo, che indica di inviare il testo `CommandText` proprietà come query SQL ad hoc.


![Selezionare il CategoriesTableAdapter nella finestra di progettazione per visualizzarne le proprietà nella finestra proprietà](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Figura 15**: selezionare il `CategoriesTableAdapter` nella finestra di progettazione per visualizzarne le proprietà nella finestra proprietà


Per modificare queste impostazioni, selezionare il testo (DeleteCommand) nella finestra proprietà e scegliere (nuova) dall'elenco a discesa. Ciò consente di correggere le impostazioni per il `CommandText`, `CommandType`, e `Parameters` proprietà. Successivamente, impostare il `CommandType` proprietà `StoredProcedure` e quindi digitare il nome di stored procedure per la `CommandText` (`dbo.Categories_Delete`). Se è assicurarsi di immettere le proprietà nell'ordine seguente: prima di `CommandType` e quindi la `CommandText` -Visual Studio popola automaticamente la raccolta di parametri. Se non si immette queste proprietà in questo ordine, è necessario aggiungere manualmente i parametri tramite l'Editor della raccolta Parameters. In entrambi i casi, è consigliabile fare clic sui puntini di sospensione nella proprietà di parametri per visualizzare Editor raccolta di parametri per verificare che sono state apportate le modifiche alle impostazioni di parametro corretti (vedere Figura 16) s. Se non è possibile visualizzare tutti i parametri nella finestra di dialogo, aggiungere il `@CategoryID` parametro manualmente (non è necessario aggiungere il `@RETURN_VALUE` parametro).


![Verificare che le impostazioni di parametri siano corretti](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figura 16**: verificare che le impostazioni di parametri siano corretti


Una volta DAL è stato aggiornato, l'eliminazione di una categoria verrà eliminare tutti i prodotti associati automaticamente e sensi generico di una transazione. Per verificarlo, tornare alla pagina di aggiornamento ed eliminazione di dati binari esistente e fare clic sul pulsante di eliminazione per una delle categorie. Con un solo clic del mouse, la categoria e tutti i prodotti associati verranno eliminati.

> [!NOTE]
> Prima di testare il `Categories_Delete` stored procedure, che verrà eliminato il numero di prodotti insieme alla categoria selezionata, potrebbe essere opportuno creare una copia di backup del database. Se si utilizza il `NORTHWND.MDF` database `App_Data`, semplicemente chiudere Visual Studio e copiare i file MDF e LDF in `App_Data` in un'altra cartella. Dopo avere testato le funzionalità, è possibile ripristinare il database, chiudere Visual Studio e sostituendo corrente MDF e LDF file `App_Data` con le copie di backup.


## <a name="summary"></a>Riepilogo

Durante la configurazione guidata TableAdapter s genererà automaticamente le stored procedure per noi, vi sono casi quando si potrebbe essere già dispongono di tali stored procedure create o desidera crearli manualmente oppure con altri strumenti invece. Per gestire tali scenari, TableAdapter può anche essere configurato per puntare a una stored procedure esistente. In questa esercitazione è stato esaminato come aggiungere manualmente le stored procedure a un database tramite l'ambiente di Visual Studio e come associare i metodi s TableAdapter per queste stored procedure. È inoltre esaminato il modello di script utilizzato per avviare, eseguire il commit e rollback delle transazioni all'interno di una stored procedure e i comandi T-SQL.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Geisenow Hilton S ren Lauritsen Bonaldi e Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
[Successivo](updating-the-tableadapter-to-use-joins-vb.md)
