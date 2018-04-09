---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
title: Inserimento, aggiornamento ed eliminazione di dati con SqlDataSource (VB) | Documenti Microsoft
author: rick-anderson
description: Nelle esercitazioni precedenti è stato descritto come il controllo ObjectDataSource consentito per l'inserimento, aggiornamento ed eliminazione dei dati. Il controllo SqlDataSource supporta t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9673bef3-892c-45ba-a7d8-0da3d6f48ec5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 92d195c3e1e349cd82e0625cf9a6c5a82644b5db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-vb"></a>Inserimento, aggiornamento ed eliminazione di dati con SqlDataSource (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_VB.exe) o [Scarica il PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/datatutorial49vb1.pdf)

> Nelle esercitazioni precedenti è stato descritto come il controllo ObjectDataSource consentito per l'inserimento, aggiornamento ed eliminazione dei dati. Il controllo SqlDataSource supporta le stesse operazioni, ma l'approccio è diversa e in questa esercitazione viene illustrato come configurare SqlDataSource per inserire, aggiornare ed eliminare dati.


## <a name="introduction"></a>Introduzione

Come descritto in [una panoramica di inserimento, aggiornamento ed eliminazione](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), il controllo GridView fornisce aggiornamento predefinite e le funzionalità di eliminazione, mentre i controlli DetailsView e FormView includono l'inserimento di supportano insieme a modifica ed eliminazione di funzionalità. Queste funzionalità di modifica dei dati può essere collegata direttamente in un controllo origine dati senza una riga di codice che devono essere scritti. [Una panoramica di inserimento, aggiornamento ed eliminazione di](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) esaminato tramite ObjectDataSource per facilitare l'inserimento, aggiornamento ed eliminazione con i controlli in GridView, DetailsView e FormView. In alternativa, SqlDataSource può essere utilizzato al posto di ObjectDataSource.

Si tenga presente che per il supporto di inserimento, aggiornamento ed eliminazione, con ObjectDataSource è Needed per specificare i metodi di livello oggetto da richiamare per l'esecuzione dell'inserimento, aggiornamento o eliminazione di azione. Con SqlDataSource, è necessario fornire `INSERT`, `UPDATE`, e `DELETE` SQL (istruzioni o stored procedure) per l'esecuzione. Come si vedrà in questa esercitazione, queste istruzioni possono essere create manualmente o possono essere generate automaticamente dalla procedura guidata Configura origine dati s SqlDataSource.

> [!NOTE]
> Poiché è ve già descritto l'inserimento, modifica ed eliminazione di funzionalità di GridView, DetailsView, FormView controlli e, in questa esercitazione sarà incentrata sulla configurazione del controllo SqlDataSource per supportare queste operazioni. Se è necessario rinfrescarti la memoria sull'implementazione di queste funzionalità in GridView, DetailsView e FormView, restituito per le esercitazioni di modifica, inserimento ed eliminazione di dati, a partire da [una panoramica di inserimento, aggiornamento ed eliminazione di](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Passaggio 1: Specificare`INSERT`,`UPDATE`, e`DELETE`istruzioni

Come si ve visualizzata nelle ultime due esercitazioni, per recuperare dati da un controllo SqlDataSource che è necessario impostare due proprietà:

1. `ConnectionString`, che consente di specificare quali database per inviare la query, e
2. `SelectCommand`, che consente di specificare l'istruzione SQL ad hoc o nome della stored procedure da eseguire per restituire i risultati.

Per `SelectCommand` i valori con parametri, il parametro i valori vengono specificati tramite s SqlDataSource `SelectParameters` insieme e possono includere valori hardcoded, valori dei parametri comuni origine (i campi querystring, variabili di sessione, il valore di controllo Web, e così via), o può essere assegnato a livello di codice. Quando il controllo SqlDataSource s `Select()` metodo viene richiamato in modo automatico o a livello di codice da un controllo Web di dati viene stabilita una connessione al database, vengono assegnati i valori dei parametri alla query e il comando è inviato disattivato per il database. I risultati vengono quindi restituiti sotto forma di set di dati o DataReader, a seconda del valore del controllo s `DataSourceMode` proprietà.

Con la selezione di dati, utilizzare il controllo SqlDataSource per inserire, aggiornare ed eliminare dati fornendo `INSERT`, `UPDATE`, e `DELETE` istruzioni SQL in modo analogo. Assegnare solo il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` le proprietà di `INSERT`, `UPDATE`, e `DELETE` istruzioni SQL da eseguire. Se le istruzioni dispongono di parametri (così come sono sempre più), includerli nel `InsertParameters`, `UpdateParameters`, e `DeleteParameters` raccolte.

Una volta un `InsertCommand`, `UpdateCommand`, o `DeleteCommand` valore è stato specificato, l'opzione Abilita inserimento, abilitare la modifica o eliminazione di abilitare i dati corrispondenti smart tag di controllo s Web diventerà disponibile. Per illustrare questo concetto, s consentono di eseguire un esempio del `Querying.aspx` creato nella pagina di [query su dati con il controllo SqlDataSource](querying-data-with-the-sqldatasource-control-vb.md) esercitazione ed estendere per includere funzionalità di eliminazione.

Aprire il `InsertUpdateDelete.aspx` e `Querying.aspx` le pagine dal `SqlDataSource` cartella. Dalla finestra di progettazione nel `Querying.aspx` pagina, selezionare SqlDataSource e GridView del primo esempio (il `ProductsDataSource` e `GridView1` controlli). Dopo aver selezionato i due controlli, passare al menu Modifica e scegliere Copia oppure preme Ctrl + C. Successivamente, accedere alla finestra di progettazione di `InsertUpdateDelete.aspx` e incollare i controlli. Dopo avere spostato i due controlli a `InsertUpdateDelete.aspx`, testare la pagina in un browser. È necessario visualizzare i valori del `ProductID`, `ProductName`, e `UnitPrice` colonne per tutti i record nel `Products` tabella di database.


[![Tutti i prodotti sono elencati, ordinati in base ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.png)

**Figura 1**: tutti i prodotti sono elencati, ordinati in base `ProductID` ([fare clic per visualizzare l'immagine ingrandita](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Aggiunta di s SqlDataSource`DeleteCommand`e`DeleteParameters`proprietà

A questo punto si dispone di un SqlDataSource che restituisce semplicemente tutti i record di `Products` tabella e un controllo GridView che esegue il rendering di dati. L'obiettivo è estendere questo esempio per consentire all'utente di eliminare i prodotti tramite il controllo GridView. A tale scopo è necessario specificare valori per il controllo SqlDataSource s `DeleteCommand` e `DeleteParameters` proprietà e quindi configurare il controllo GridView per supportare l'eliminazione.

Il `DeleteCommand` e `DeleteParameters` proprietà possono essere specificate in diversi modi:

- Tramite la sintassi dichiarativa
- Dalla finestra delle proprietà nella finestra di progettazione
- Dalla specifica di una stored procedure o un'istruzione SQL personalizzata schermata della procedura guidata Configura origine dati
- Tramite il pulsante Avanzate di specificare le colonne di una tabella della schermata di visualizzazione della procedura guidata Configura origine dati, che verrà generato automaticamente in realtà il `DELETE` raccolta parametro e l'istruzione SQL utilizzata nella `DeleteCommand` e `DeleteParameters` proprietà

Esamineremo come disporre automaticamente di `DELETE` istruzione creato nel passaggio 2. Per il momento, consentire s utilizzare la finestra delle proprietà nella finestra di progettazione, anche se l'opzione di sintassi dichiarativa o la configurazione guidata origine dati funzionerà così.

Dalla finestra di progettazione in `InsertUpdateDelete.aspx`, fare clic su di `ProductsDataSource` SqlDataSource e quindi visualizzare la finestra Proprietà (dal menu Visualizza, scegliere la finestra Proprietà, o semplicemente premere F4). Selezionare la proprietà DeleteQuery, che verrà visualizzata una serie di puntini di sospensione.


![Selezionare la proprietà DeleteQuery dalla finestra delle proprietà](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.gif)

**Figura 2**: selezionare la proprietà DeleteQuery dalla finestra delle proprietà


> [!NOTE]
> T SqlDataSource dispone di una proprietà DeleteQuery. Piuttosto, DeleteQuery è una combinazione del `DeleteCommand` e `DeleteParameters` proprietà e viene elencato solo nella finestra Proprietà quando la visualizzazione della finestra tramite la finestra di progettazione. Se si sta esaminando la finestra delle proprietà nella visualizzazione origine, è possibile trovare il `DeleteCommand` proprietà invece.


Fare clic sui puntini di sospensione nella proprietà DeleteQuery per visualizzare la finestra di dialogo Editor comandi e parametri casella (vedere Figura 3). Questa finestra di dialogo consente di specificare il `DELETE` istruzione SQL e specificare i parametri. Immettere la query seguente nel `DELETE` casella di testo di comando (sia manualmente o utilizzando il generatore di Query, se si preferisce):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample1.sql)]

Successivamente, fare clic sul pulsante Aggiorna parametri per aggiungere il `@ProductID` parametro all'elenco dei parametri seguenti.


[![Selezionare la proprietà DeleteQuery dalla finestra delle proprietà](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.png)

**Figura 3**: selezionare la proprietà DeleteQuery dalla finestra delle proprietà ([fare clic per visualizzare l'immagine ingrandita](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.png))


Eseguire *non* fornire un valore per questo parametro (lasciare il relativo parametro di origine su Nessuno). Una volta che viene aggiunto il supporto di eliminazione a GridView, GridView automaticamente fornirà il valore di questo parametro, utilizzando il valore della relativa `DataKeys` raccolta per la riga è stato fatto clic con pulsante Elimina.

> [!NOTE]
> Il nome del parametro utilizzato nel `DELETE` query *deve* essere lo stesso nome del `DataKeyNames` valore nel controllo GridView, DetailsView o FormView. Vale a dire, il parametro nel `DELETE` istruzione intenzionalmente denominata `@ProductID` (anziché, ad esempio, `@ID`), perché il nome di colonna chiave primaria nella tabella Products (e pertanto il valore DataKeyNames in GridView) è `ProductID`.


Se il nome del parametro e `DataKeyNames` corrispondenza t di valore, GridView non può assegnare automaticamente il parametro il valore di `DataKeys` insieme.

Dopo aver immesso le informazioni relative alla eliminazione nella finestra di dialogo Editor comandi e parametri fare clic su OK e passare alla visualizzazione origine per esaminare il markup dichiarativo risulta:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample2.aspx)]

Si noti l'aggiunta del `DeleteCommand` proprietà, nonché `<DeleteParameters>` sezione e l'oggetto parametro denominato `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Configurazione di GridView per l'eliminazione

Con la `DeleteCommand` proprietà aggiunta, lo smart tag s di GridView ora contiene l'opzione Abilita eliminazione. Vado avanti e selezionare questa casella di controllo. Come descritto in [una panoramica di inserimento, aggiornamento ed eliminazione di](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), in questo modo, il controllo GridView aggiungere un CommandField con relativo `ShowDeleteButton` proprietà impostata su `True`. Come figura 4 mostra, quando si visita la pagina tramite un browser è incluso un pulsante Elimina. L'eliminazione di alcuni prodotti testare questa pagina out.


[![Ogni riga GridView include ora un pulsante Elimina](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.png)

**Figura 4**: ogni riga GridView include ora un pulsante Elimina ([fare clic per visualizzare l'immagine ingrandita](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.png))


Facendo clic sul pulsante Elimina, si verifica un postback, GridView assegna il `ProductID` parametro il valore del `DataKeys` il valore di raccolta per la riga il cui pulsante Elimina è stato fatto clic e richiama il s SqlDataSource `Delete()` metodo. Il controllo SqlDataSource si connette al database ed esegue quindi il `DELETE` istruzione. GridView nuovamente per SqlDataSource, ottenere e visualizzare il set corrente di prodotti (che non include più il record appena è stato eliminato).

> [!NOTE]
> Poiché utilizza GridView relativo `DataKeys` insieme per popolare i parametri SqlDataSource, è essenziale s che s GridView `DataKeyNames` impostata per le colonne che costituiscono la chiave primaria e che s SqlDataSource `SelectCommand` restituisce Queste colonne. Inoltre, è importante che il parametro name in SqlDataSource s s `DeleteCommand` è impostato su `@ProductID`. Se il `DataKeyNames` non è impostata o il parametro non denominato `@ProductsID`, fare clic sul pulsante Delete causerà un postback, ma t acquisite effettivamente elimina qualsiasi record.


Figura 5 viene illustrata questa interazione graficamente. Fare riferimento in futuro il [esaminando gli eventi associati con l'inserimento, aggiornamento ed eliminazione](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) esercitazione per informazioni più dettagliate sulla catena di eventi associati all'inserimento, aggiornamento ed eliminazione da un controllo Web di dati.


![Fare clic sul pulsante Elimina in GridView richiama il metodo Delete () s SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.gif)

**Figura 5**: fare clic sul pulsante Delete in GridView richiama s SqlDataSource `Delete()` (metodo)


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Passaggio 2: Generare automaticamente il`INSERT`,`UPDATE`, e`DELETE`istruzioni

Come passaggio 1 esaminato, `INSERT`, `UPDATE`, e `DELETE` istruzioni SQL possono essere specificate tramite la finestra proprietà o la sintassi dichiarativa controllo s. Questo approccio richiede tuttavia che si manualmente scrivono le istruzioni SQL manualmente, che può essere monotono e soggetta a errori. Fortunatamente, la configurazione guidata origine dati fornisce un'opzione per disporre il `INSERT`, `UPDATE`, e `DELETE` istruzioni generate automaticamente quando si utilizza lo specificare le colonne di una tabella della schermata di visualizzazione.

Consente di esplorare l'opzione di generazione automatica s. Aggiungere un controllo DetailsView nella finestra di progettazione in `InsertUpdateDelete.aspx` e impostare il relativo `ID` proprietà `ManageProducts`. Successivamente, dal controllo DetailsView s smart tag, scegliere di creare una nuova origine dati e creare un SqlDataSource denominato `ManageProductsDataSource`.


[![Creare un nuovo SqlDataSource denominato ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.png)

**Figura 6**: creare un nuovo denominato SqlDataSource `ManageProductsDataSource` ([fare clic per visualizzare l'immagine ingrandita](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.png))


Dalla procedura guidata Configura origine dati, scegliere di utilizzare il `NORTHWINDConnectionString` connessione stringa e fare clic su Avanti. Dalla schermata di istruzione Select, configura lascia specificare le colonne da un pulsante di opzione di tabella o vista selezionato e scegliere il `Products` tabella dall'elenco a discesa. Selezionare il `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colonne nell'elenco della casella di controllo.


[![Utilizza la tabella Products, restituire il ProductID, ProductName, UnitPrice e le colonne non più supportate](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.png)

**Figura 7**: tramite il `Products` tabella, per restituire il `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colonne ([fare clic per visualizzare l'immagine ingrandita](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image10.png))


Per generare automaticamente `INSERT`, `UPDATE`, e `DELETE` istruzioni in base alla tabella selezionata e le colonne, fare clic sul pulsante avanzato e controllare genera `INSERT`, `UPDATE`, e `DELETE` casella di controllo di istruzioni.


![Selezionare la casella di controllo di istruzioni genera istruzioni INSERT, UPDATE e DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.gif)

**Figura 8**: controllare genera `INSERT`, `UPDATE`, e `DELETE` istruzioni casella di controllo


Genera `INSERT`, `UPDATE`, e `DELETE` casella di controllo di istruzioni verrà essere selezionabile solo se la tabella selezionata è una chiave primaria e la colonna chiave primaria (o le colonne) sono inclusi nell'elenco delle colonne restituite. La casella di controllo di concorrenza ottimistica di utilizzo, che diventa selezionabile genera una volta `INSERT`, `UPDATE`, e `DELETE` istruzioni casella di controllo è stato selezionato, verranno potenziare il `WHERE` clausole nella finestra di `UPDATE` e `DELETE` istruzioni per fornire il controllo della concorrenza ottimistica. Per il momento, lasciare questa casella di controllo deselezionata; verranno presi in esame la concorrenza ottimistica con il controllo SqlDataSource nella prossima esercitazione.

Dopo aver controllato genera `INSERT`, `UPDATE`, e `DELETE` istruzioni casella di controllo, fare clic su OK per tornare alla schermata Configura istruzione Select, quindi fare clic su Avanti e quindi Fine per completare la configurazione guidata origine dati. Dopo avere completato la procedura guidata, Visual Studio aggiungerà BoundField al controllo DetailsView per il `ProductID`, `ProductName`, e `UnitPrice` colonne e un CheckBoxField per il `Discontinued` colonna. Da DetailsView s smart tag, selezionare l'opzione Abilita Paging, in modo che l'utente visita questa pagina è possibile eseguire con i prodotti. Anche cancellare i dispositivi di DetailsView `Width` e `Height` proprietà.

Si noti che lo smart tag sono disponibili le opzioni Abilita inserimento, abilitare Modifica e Abilita eliminazione disponibili. Infatti SqlDataSource contiene valori per il relativo `InsertCommand`, `UpdateCommand`, e `DeleteCommand`, come mostra la sintassi dichiarativa seguente:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample3.aspx)]

Si noti come il controllo SqlDataSource è stati impostati automaticamente per i valori relativi `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà. Il set di colonne a cui fa riferimento il `InsertCommand` e `UpdateCommand` le proprietà si basano su quelle del `SELECT` istruzione. Ovvero evitando che *ogni* colonna prodotti di `InsertCommand` e `UpdateCommand`, sono disponibili solo le colonne specificate nel `SelectCommand` (meno `ProductID`, che viene omesso perché è s un [ `IDENTITY` colonna](http://www.sqlteam.com/item.asp?ItemID=102), il cui valore in cui viene modificato e che viene assegnato automaticamente durante l'inserimento). Inoltre, per ogni parametro di `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà corrispondente ai parametri nel `InsertParameters`, `UpdateParameters`, e `DeleteParameters` raccolte.

Per attivare le funzionalità di modifica dei dati di DetailsView s, controllare gli Abilita inserimento, Abilita modifica e l'eliminazione di abilitare le opzioni smart tag. Aggiunge un CommandField con relativo `ShowInsertButton`, `ShowEditButton`, e `ShowDeleteButton` le proprietà impostate su `True`.

Visitare la pagina in un browser e notare la modifica, eliminazione e nuovi pulsanti inclusi nel controllo DetailsView. Fare clic sul pulsante Modifica consente di trasformare DetailsView in modalità di modifica, che visualizza ogni BoundField cui `ReadOnly` è impostata su `False` (predefinito) come una casella di testo e il CheckBoxField come una casella di controllo.


[![S DetailsView predefinito interfaccia di modifica](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image11.png)

**Figura 9**: s DetailsView l'interfaccia di modifica predefinito ([fare clic per visualizzare l'immagine ingrandita](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image12.png))


Analogamente, è possibile eliminare il prodotto selezionato o aggiungere un nuovo prodotto al sistema. Poiché il `InsertCommand` istruzione funziona solo con il `ProductName`, `UnitPrice`, e `Discontinued` colonne, le altre colonne dispongono di `NULL` o al relativo valore predefinito assegnato dal database al momento dell'inserimento. Analogamente a ObjectDataSource, se il `InsertCommand` manca qualsiasi tabella di database consentono di colonne che non è stata ridimensionata `NULL` s e don t hanno un valore predefinito, si verificherà un errore SQL durante il tentativo di eseguire il `INSERT` istruzione.

> [!NOTE]
> I dispositivi di DetailsView inserimento e modifica delle interfacce non dispongono di una sorta di personalizzazione o la convalida. Per aggiungere i controlli di convalida o per personalizzare le interfacce, è necessario convertire il BoundField TemplateFields. Fare riferimento al [aggiunta di controlli di convalida per la modifica e inserimento di interfacce](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) e [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) esercitazioni per altre informazioni.


Inoltre, tenere presente che per l'aggiornamento e l'eliminazione, controllo DetailsView Usa il prodotto corrente s `DataKey` valore, che è presente solo se il `DataKeyNames` proprietà è configurata. Se la modifica o eliminazione non hanno alcun effetto, verificare che il `DataKeyNames` proprietà è impostata.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Limitazioni di generazione automatica di istruzioni SQL

Dopo la generazione `INSERT`, `UPDATE`, e `DELETE` istruzioni opzione è disponibile solo quando selezione colonne da una tabella, per le query più complessa è necessario scrivere la propria `INSERT`, `UPDATE`, e `DELETE` istruzioni come abbiamo visto nel passaggio 1. In genere, SQL `SELECT` utilizzano istruzioni `JOIN` s per ripristinare dati da uno o più tabelle di ricerca per scopi di visualizzazione (come ad esempio riportare nuovamente il `Categories` tabella s `CategoryName` campo quando si visualizzano informazioni sui prodotti). Allo stesso tempo, potrebbe essere opportuno consentire all'utente di modificare, aggiornare o inserire dati nella tabella di base (`Products`, in questo caso).

Mentre il `INSERT`, `UPDATE`, e `DELETE` istruzioni possono essere immesse manualmente, prendere in considerazione il suggerimento di risparmiare tempo seguente. Programma di installazione inizialmente SqlDataSource in modo che nuovamente estrae i dati solo dal `Products` tabella. Utilizzare le colonne di specificare s procedura guidata Configura origine dati da una schermata di tabella o vista in modo che è possibile generare automaticamente il `INSERT`, `UPDATE`, e `DELETE` istruzioni. Quindi, dopo aver completato la procedura guidata, scegliere di configurare il SelectQuery dalla finestra delle proprietà (o, in alternativa, tornare alla configurazione guidata origine dati, ma utilizzando un'istruzione SQL personalizzata specifica o opzione della stored procedure). Aggiornare quindi il `SELECT` istruzione per includere il `JOIN` sintassi. Questa tecnica offre i vantaggi di risparmiare tempo delle istruzioni SQL generate automaticamente e consente più personalizzata `SELECT` istruzione.

Un altro limite la generazione automatica del `INSERT`, `UPDATE`, e `DELETE` istruzioni è che le colonne di `INSERT` e `UPDATE` istruzioni si basano le colonne restituite dal `SELECT` istruzione. È possibile che sia necessario aggiornare o inserire i campi più o meno, tuttavia. Ad esempio, nell'esempio dal passaggio 2, forse si desidera avere il `UnitPrice` BoundField essere di sola lettura. In tal caso, si t rilevanti vengono visualizzati di `UpdateCommand`. O potrebbe essere necessario impostare il valore di un campo di tabella che non compare in GridView. Ad esempio, quando si aggiunge un nuovo record si può decidere di `QuantityPerUnit` valore impostato per attività.

Se tali personalizzazioni sono necessari, è necessario per renderli manualmente, tramite la finestra Proprietà, di specificare un'istruzione SQL personalizzata o opzione della stored procedure nella procedura guidata oppure tramite la sintassi dichiarativa.

> [!NOTE]
> Quando l'aggiunta di parametri che non contengono i campi corrispondenti nei dati di controllo Web, tenere presente che questi valori di parametri dovranno essere assegnati i valori in qualche modo. Questi valori possono essere: hardcoded direttamente nel `InsertCommand` o `UpdateCommand`; possono provenire da un'origine predefinita (la stringa di query, lo stato della sessione, i controlli Web nella pagina e così via); o può essere assegnato a livello di programmazione, come illustrato nell'esercitazione precedente.


## <a name="summary"></a>Riepilogo

Affinché i dati a che controlli Web per utilizzare i relativi incorporate di inserimento, modifica ed eliminazione di funzionalità, il controllo origine dati a che vengono associati deve offrire tale funzionalità. Per SqlDataSource, ciò significa che `INSERT`, `UPDATE`, e `DELETE` istruzioni SQL devono essere assegnate il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà. Queste proprietà e le raccolte di parametri corrispondenti, è possibile aggiungere manualmente o generate automaticamente tramite la configurazione guidata origine dati. In questa esercitazione sono state esaminate entrambe le tecniche.

Sono state esaminate con la concorrenza ottimistica ObjectDataSource nel [implementazione di concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) esercitazione. Il controllo SqlDataSource supporta inoltre la concorrenza ottimistica. Come indicato nel passaggio 2, durante la generazione automatica di `INSERT`, `UPDATE`, e `DELETE` istruzioni, la procedura guidata è disponibile l'opzione Usa concorrenza ottimistica. Come vedremo nella prossima esercitazione, la concorrenza ottimistica con SqlDataSource modifica il `WHERE` clausole di `UPDATE` e `DELETE` istruzioni per assicurarsi che i valori per le altre colonne utili t modificato dall'ultimi i dati visualizzato nella pagina.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](using-parameterized-queries-with-the-sqldatasource-vb.md)
> [Successivo](implementing-optimistic-concurrency-with-the-sqldatasource-vb.md)
