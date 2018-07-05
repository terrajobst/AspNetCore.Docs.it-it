---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Inserimento, aggiornamento ed eliminazione dei dati con SqlDataSource (c#) | Microsoft Docs
author: rick-anderson
description: Nelle esercitazioni precedenti è stato illustrato come il controllo ObjectDataSource consentito per l'inserimento, aggiornamento ed eliminazione dei dati. Il controllo SqlDataSource supporta t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: b3037cfdc9a6b27b1f87e0b323b9ae59235cc27c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385810"
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Inserimento, aggiornamento ed eliminazione dei dati con SqlDataSource (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) o [Scarica il PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> Nelle esercitazioni precedenti è stato illustrato come il controllo ObjectDataSource consentito per l'inserimento, aggiornamento ed eliminazione dei dati. Il controllo SqlDataSource supporta le stesse operazioni, ma l'approccio è diverso e in questa esercitazione illustra come configurare SqlDataSource per inserire, aggiornare ed eliminare dati.


## <a name="introduction"></a>Introduzione

Come descritto nella [una panoramica di inserimento, aggiornamento ed eliminazione](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), il controllo GridView fornisce l'aggiornamento predefinite e le funzionalità di eliminazione, anche se i controlli DetailsView e FormView includono l'inserimento di supportano insieme a modifica ed eliminazione di funzionalità. Queste funzionalità di modifica dei dati può essere inserita direttamente in un controllo origine dati senza dovere scrivere codice che devono essere scritti. [Una panoramica di inserimento, aggiornamento ed eliminazione](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) esaminato tramite ObjectDataSource per facilitare l'inserimento, aggiornamento ed eliminazione con i controlli GridView, DetailsView e FormView. In alternativa, SqlDataSource utilizzabile al posto di ObjectDataSource.

Si tenga presente che per supportare l'inserimento, aggiornamento ed eliminazione, con ObjectDataSource è Needed per specificare i metodi di livello di oggetto da richiamare per eseguire l'inserimento, aggiornamento o eliminazione di azione. Con SqlDataSource, è necessario fornire `INSERT`, `UPDATE`, e `DELETE` SQL istruzioni o stored procedure da eseguire. Come sarà illustrato in questa esercitazione, queste istruzioni possono essere create manualmente o possono essere generate automaticamente dalla procedura guidata Configura origine dati s SqlDataSource.

> [!NOTE]
> Poiché abbiamo ve già illustrato l'inserimento, la modifica e l'eliminazione di funzionalità di GridView, DetailsView, FormView controlli e, questa esercitazione è incentrata su come configurare il controllo SqlDataSource per supportare queste operazioni. Se è necessario implementare queste funzionalità all'interno la restituzione di GridView, DetailsView e FormView, alle esercitazioni di modifica, inserimento ed eliminazione di dati, a partire dal [una panoramica di inserimento, aggiornamento ed eliminazione](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Passaggio 1: Specificare`INSERT`,`UPDATE`, e`DELETE`istruzioni

Come abbiamo visualizzato nelle ultime due esercitazioni, per recuperare dati da un controllo SqlDataSource che è necessario fornire un supporto iniziale imposta due proprietà:

1. `ConnectionString`, che consente di specificare quali database per inviare la query, e
2. `SelectCommand`, che consente di specificare l'istruzione SQL ad hoc o il nome della stored procedure da eseguire per restituire i risultati.

Per `SelectCommand` i valori con parametri, il parametro valori vengono specificati tramite la s SqlDataSource `SelectParameters` insieme e possono includere valori hardcoded, valori di origine parametro comuni (i campi stringa di query, le variabili di sessione, i valori di controllo Web, e così via), o possono essere assegnati a livello di codice. Quando il controllo SqlDataSource s `Select()` metodo viene richiamato in modo automatico o a livello di codice da un controllo Web per dati viene stabilita una connessione al database, i valori dei parametri vengono assegnati alla query e il comando viene inviato disattivato per il database. I risultati vengono quindi restituiti sotto forma di set di dati o DataReader, a seconda del valore del controllo s `DataSourceMode` proprietà.

Insieme alla selezione dei dati, il controllo SqlDataSource è utilizzabile per inserire, aggiornare ed eliminare i dati fornendo `INSERT`, `UPDATE`, e `DELETE` istruzioni SQL in modo analogo. È sufficiente assegnare il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` delle proprietà il `INSERT`, `UPDATE`, e `DELETE` istruzioni SQL da eseguire. Se le istruzioni dispongono di parametri (come lo faranno sempre più), includerli nel `InsertParameters`, `UpdateParameters`, e `DeleteParameters` raccolte.

Una volta un' `InsertCommand`, `UpdateCommand`, o `DeleteCommand` valore è stato specificato, l'opzione Attiva paging, abilitare la modifica o eliminazione di abilitare i dati corrispondenti smart tag di controllo s Web sarà rese disponibile. Per illustrare questo concetto, s ti permettono di eseguire un esempio tratto dal `Querying.aspx` creato nella pagina la [Querying Data con il controllo SqlDataSource](querying-data-with-the-sqldatasource-control-cs.md) esercitazione e potenziare che possa includere funzionalità di eliminazione.

Iniziare aprendo il `InsertUpdateDelete.aspx` e `Querying.aspx` pagine dal `SqlDataSource` cartella. Dalla finestra di progettazione nel `Querying.aspx` pagina, selezionare il SqlDataSource e GridView del primo esempio (la `ProductsDataSource` e `GridView1` controlli). Dopo aver selezionato i due controlli, aprire il menu Modifica e scegliere Copia (o semplicemente premere Ctrl + C). Passare quindi alla finestra di progettazione di `InsertUpdateDelete.aspx` e incollare i controlli. Dopo avere spostato i due controlli a `InsertUpdateDelete.aspx`, testare la pagina in un browser. È necessario visualizzare i valori del `ProductID`, `ProductName`, e `UnitPrice` colonne per tutti i record nel `Products` tabella di database.


[![Tutti i prodotti sono elencati, ordinati per ID prodotto](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Figura 1**: tutti i prodotti sono elencati, ordinati in base `ProductID` ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Aggiunta di dispositivi SqlDataSource`DeleteCommand`e`DeleteParameters`proprietà

A questo punto abbiamo un SqlDataSource che restituisce semplicemente tutti i record dal `Products` tabella e un controllo GridView che esegue il rendering di tali dati. Il nostro obiettivo consiste nell'estendere questo esempio per consentire all'utente di eliminare i prodotti tramite il controllo GridView. A tale scopo è necessario specificare valori per il controllo SqlDataSource 1!s `DeleteCommand` e `DeleteParameters` proprietà e quindi configurare il controllo GridView per supportare l'eliminazione.

Il `DeleteCommand` e `DeleteParameters` le proprietà possono essere specificate in diversi modi:

- Tramite la sintassi dichiarativa
- Dalla finestra delle proprietà nella finestra di progettazione
- Dalla specifica di un'istruzione SQL personalizzata o una stored procedure schermata della procedura guidata Configura origine dati
- Tramite il pulsante avanzato nella finestra di specificare le colonne da una tabella della schermata di visualizzazione nella procedura guidata Configura origine dati, che genera effettivamente automaticamente il `DELETE` raccolta di istruzioni e dei parametri SQL utilizzata nella `DeleteCommand` e `DeleteParameters` proprietà

Esamineremo come disporre automaticamente di `DELETE` istruzione creata nel passaggio 2. Per il momento lasciare s usare la finestra delle proprietà nella finestra di progettazione, anche se l'opzione di sintassi dichiarativa o configurazione guidata origine dati funzionerebbero altrettanto bene.

Dalla finestra di progettazione nelle `InsertUpdateDelete.aspx`, fare clic su di `ProductsDataSource` SqlDataSource e quindi visualizzare la finestra Proprietà (dal menu Visualizza, finestra Proprietà scegliere o è sufficiente premere F4). Selezionare la proprietà DeleteQuery, che verrà visualizzata una serie di puntini di sospensione.


![Selezionare la proprietà DeleteQuery dalla finestra delle proprietà](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Figura 2**: selezionare la proprietà DeleteQuery dalla finestra delle proprietà


> [!NOTE]
> T SqlDataSource hanno una proprietà DeleteQuery. Piuttosto, DeleteQuery è una combinazione dei `DeleteCommand` e `DeleteParameters` proprietà e viene elencato solo nella finestra Proprietà quando si visualizza la finestra tramite la finestra di progettazione. Se si sta esaminando la finestra delle proprietà nella visualizzazione origine, si troverà il `DeleteCommand` proprietà invece.


Fare clic sui puntini di sospensione nella proprietà DeleteQuery per visualizzare la finestra di dialogo Editor comandi e parametri casella (vedere la figura 3). Da questa finestra di dialogo è possibile specificare il `DELETE` istruzione SQL e specificare i parametri. Immettere la query seguente il `DELETE` nella casella di testo di comando (sia manualmente o tramite il generatore delle Query, se si preferisce):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Successivamente, fare clic sul pulsante Aggiorna parametri per aggiungere il `@ProductID` parametro all'elenco dei parametri seguenti.


[![Selezionare la proprietà DeleteQuery dalla finestra delle proprietà](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Figura 3**: selezionare la proprietà DeleteQuery dalla finestra delle proprietà ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


Effettuare *non* fornire un valore per questo parametro (lasciare il parametro di origine a None). Una volta che viene aggiunto il supporto di eliminazione a GridView, il controllo GridView fornirà automaticamente questo valore del parametro, utilizzando il valore della relativa `DataKeys` raccolta per la riga è stato fatto clic sul pulsante la cui eliminazione.

> [!NOTE]
> Il nome del parametro utilizzato nel `DELETE` query *deve* essere identico al nome del `DataKeyNames` valore di GridView, DetailsView e FormView. Vale a dire, il parametro nel `DELETE` intenzionalmente denominata istruzione `@ProductID` (anziché, ad esempio, `@ID`), perché il nome di colonna chiave primaria nella tabella Products (e pertanto al valore DataKeyNames in GridView) è `ProductID`.


Se il nome del parametro e `DataKeyNames` corrispondenza di t l valore, il controllo GridView non può assegnare automaticamente il parametro il valore dal `DataKeys` raccolta.

Dopo aver immesso le informazioni relative alla eliminazione nella finestra di dialogo Editor comandi e parametri fare clic su OK e passare alla visualizzazione origine per esaminare il markup dichiarativo risulta:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Si noti l'aggiunta del `DeleteCommand` proprietà, nonché `<DeleteParameters>` sezione e l'oggetto parametro denominato `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Configurazione di GridView per l'eliminazione

Con il `DeleteCommand` proprietà aggiunta, lo smart tag s di GridView contiene ora l'opzione Abilita l'eliminazione. Proseguire e selezionare questa casella di controllo. Come descritto nella [una panoramica di inserimento, aggiornamento ed eliminazione](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), in questo modo, il controllo GridView aggiungere un CommandField con relativo `ShowDeleteButton` impostata su `true`. Come figura 4 mostra, quando si visita la pagina tramite un browser è incluso un pulsante Elimina. L'eliminazione di alcuni prodotti testare questa pagina orizzontale.


[![Ogni riga GridView include ora un pulsante Elimina](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Figura 4**: ogni riga GridView include ora un pulsante Elimina ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


Facendo clic su un pulsante di eliminazione, si verifica un postback, il controllo GridView assegna il `ProductID` il valore di parametro del `DataKeys` il valore di raccolta per la riga è stato fatto clic sul pulsante la cui eliminazione e richiama s SqlDataSource `Delete()` (metodo). Il controllo SqlDataSource, quindi si connette al database ed esegue il `DELETE` istruzione. Il controllo GridView quindi riassocia a SqlDataSource, ricevendo e visualizzare il set corrente di prodotti (che non include più il record appena è stato eliminato).

> [!NOTE]
> Dal momento che utilizza il controllo GridView relativi `DataKeys` raccolta per popolare i parametri con SqlDataSource, lo s fondamentale che s GridView `DataKeyNames` impostata per le colonne che costituiscono la chiave primaria e che s SqlDataSource `SelectCommand` restituisce Queste colonne. Inoltre, è s importante che il parametro di nome in s SqlDataSource `DeleteCommand` è impostata su `@ProductID`. Se il `DataKeyNames` non è impostata o il parametro non è denominato `@ProductsID`, facendo clic sul pulsante Elimina causerà un postback, ma t vinti effettivamente eliminare qualsiasi record.


Figura 5 viene illustrata questa interazione graficamente. Fare riferimento al [esaminare gli eventi associati con inserimento, aggiornamento ed eliminazione](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) esercitazione per informazioni più dettagliate sulla catena di eventi associati a inserimento, aggiornamento ed eliminazione da un controllo Web per dati.


![Facendo clic sul pulsante Elimina in GridView richiama il metodo Delete () s SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Figura 5**: facendo clic sul pulsante Elimina in GridView richiama s SqlDataSource `Delete()` (metodo)


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Passaggio 2: Generare automaticamente il`INSERT`,`UPDATE`, e`DELETE`istruzioni

Come passaggio 1, esaminate `INSERT`, `UPDATE`, e `DELETE` istruzioni SQL possono essere specificate tramite la finestra proprietà o la sintassi dichiarativa per il controllo del codice s. Questo approccio richiede tuttavia che si manualmente scrivono le istruzioni SQL a mano, che può essere monotone e soggetta a errori. Fortunatamente, la procedura guidata Configura origine dati fornisce un'opzione per disporre le `INSERT`, `UPDATE`, e `DELETE` istruzioni generate automaticamente quando si usa lo specificare le colonne da una tabella della schermata di visualizzazione.

Let s esplorare questa opzione di generazione automatica. Aggiungere un controllo DetailsView nella finestra di progettazione nelle `InsertUpdateDelete.aspx` e impostare relativi `ID` proprietà `ManageProducts`. Successivamente, DetailsView s nello smart tag, scegliere di creare una nuova origine dati e creare un SqlDataSource denominato `ManageProductsDataSource`.


[![Creare un nuovo SqlDataSource denominato ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Figura 6**: creare una nuova denominata SqlDataSource `ManageProductsDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


Dalla procedura guidata Configura origine dati, scegliere di usare il `NORTHWINDConnectionString` connection string e fare clic su Avanti. Dalla schermata di istruzione Select, configura lascia specificare le colonne da un pulsante di opzione di tabella o vista selezionato e scegliere il `Products` tabella nell'elenco a discesa. Selezionare il `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colonne nell'elenco della casella di controllo.


[![Usa la tabella Products, restituire il ProductID, ProductName, UnitPrice e le colonne non più supportate](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Figura 7**: tramite il `Products` tabella, per restituire il `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` colonne ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


Per generare automaticamente `INSERT`, `UPDATE`, e `DELETE` le istruzioni in base alla tabella selezionata e le colonne, fare clic sul pulsante Avanzate e controllare la generazione `INSERT`, `UPDATE`, e `DELETE` casella di controllo di istruzioni.


![Selezionare la casella di controllo di istruzioni genera istruzioni INSERT, UPDATE e DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Figura 8**: controlla la generazione `INSERT`, `UPDATE`, e `DELETE` istruzioni casella di controllo


Genera `INSERT`, `UPDATE`, e `DELETE` checkbox istruzioni solo saranno selezionabili se la tabella selezionata ha una chiave primaria e la colonna chiave primaria (o le colonne) vengono inclusi nell'elenco delle colonne restituite. La casella di controllo di concorrenza ottimistica di utilizzo, che diventa selezionabile genera una sola volta `INSERT`, `UPDATE`, e `DELETE` è stata selezionata la casella di controllo di istruzioni, verrà aumentare il `WHERE` clausole nel risultante `UPDATE` e`DELETE` istruzioni per fornire il controllo della concorrenza ottimistica. Per il momento, lasciare questa casella di controllo deselezionata; Nella prossima esercitazione verrà esaminato la concorrenza ottimistica con il controllo SqlDataSource.

Dopo aver verificato la generare `INSERT`, `UPDATE`, e `DELETE` istruzioni casella di controllo, fare clic su OK per tornare alla schermata Configura istruzione Select, quindi fare clic su Avanti e quindi fine, per completare la procedura guidata Configura origine dati. Dopo aver completato la procedura guidata, Visual Studio aggiungerà BoundField di DetailsView per la `ProductID`, `ProductName`, e `UnitPrice` le colonne e un CampoCasellaDiControllo per il `Discontinued` colonna. DetailsView s nello smart tag, selezionare l'opzione attiva Paging in modo che l'utente potrebbe visitare questa pagina può scorrere i prodotti. Anche cancellare i dispositivi di DetailsView `Width` e `Height` proprietà.

Si noti che lo smart tag sono disponibili le opzioni Abilita inserimento Abilita modifica e Abilita eliminazione disponibili. Infatti, SqlDataSource contiene valori per i relativi `InsertCommand`, `UpdateCommand`, e `DeleteCommand`, come illustra la sintassi dichiarativa per il seguente:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

Si noti come il controllo SqlDataSource è stati impostati automaticamente per i valori relativi `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà. Il set di colonne a cui fa riferimento il `InsertCommand` e `UpdateCommand` delle proprietà si basano su quelle nel `SELECT` istruzione. Vale a dire, anziché *ogni* colonna prodotti nel `InsertCommand` e `UpdateCommand`, esistono solo le colonne specificate nel `SelectCommand` (meno `ProductID`, che viene omesso in quanto è s un [ `IDENTITY` colonna](http://www.sqlteam.com/item.asp?ItemID=102), il cui valore non può essere modificato quando modificato e che viene assegnato automaticamente durante l'inserimento). Inoltre, per ogni parametro nel `InsertCommand`, `UpdateCommand`, e `DeleteCommand` sono presenti parametri corrispondenti nella proprietà il `InsertParameters`, `UpdateParameters`, e `DeleteParameters` raccolte.

Per attivare le funzionalità di modifica dei dati di s DetailsView, controllare l'Abilita inserimento Abilita modifica e Abilita eliminazione opzioni nel suo smart tag. Verrà aggiunto un CommandField con relativi `ShowInsertButton`, `ShowEditButton`, e `ShowDeleteButton` impostate su `true`.

Visita la pagina in un browser e notare la modifica, eliminazione e nuovi pulsanti inclusi nel controllo DetailsView. Facendo clic sul pulsante Modifica Trasforma DetailsView in modalità di modifica che consente di visualizzare ogni BoundField cui `ReadOnly` è impostata su `false` (predefinito) come una casella di testo e il CampoCasellaDiControllo come una casella di controllo.


[![S DetailsView predefinito di interfaccia di modifica](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Figura 9**: s DetailsView di interfaccia di modifica predefinito ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


Analogamente, è possibile eliminare il prodotto selezionato o aggiungere un nuovo prodotto al sistema. Poiché il `InsertCommand` istruzione funziona solo con il `ProductName`, `UnitPrice`, e `Discontinued` colonne, le altre colonne dispongono di alcun `NULL` o relativo valore predefinito assegnato dal database al momento dell'inserimento. Come con ObjectDataSource, se il `InsertCommand` non è presente alcuna tabella di database, le colonne che desidero consentono `NULL` don t e s ha un valore predefinito, verificherà un errore SQL durante il tentativo di eseguire il `INSERT` istruzione.

> [!NOTE]
> Le s DetailsView inserimento e modifica le interfacce non dispongono di una sorta di personalizzazione o la convalida. Per aggiungere i controlli di convalida o personalizzare le interfacce, è necessario convertire i BoundField in TemplateFields. Vedere il [aggiunta di controlli di convalida per la modifica e inserimento di interfacce](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) e [personalizzazione dell'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) esercitazioni per altre informazioni.


Inoltre, tenere presente che per l'aggiornamento e l'eliminazione, DetailsView utilizza il prodotto corrente 1!s `DataKey` valore, che è presente solo se il `DataKeyNames` proprietà è configurata. Se la modifica o eliminazione sembra non hanno alcun effetto, assicurarsi che il `DataKeyNames` è impostata.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Limitazioni di generazione automatica di istruzioni SQL

Dopo la generazione `INSERT`, `UPDATE`, e `DELETE` opzione istruzioni è disponibile solo quando il prelievo di colonne da una tabella, per le query più complessa è sarà necessario scriverne uno proprio `INSERT`, `UPDATE`, e `DELETE` istruzioni come abbiamo fatto nel passaggio 1. Comunemente, SQL `SELECT` usano le istruzioni `JOIN` s per riportare online i dati da uno o più tabelle di ricerca per ai fini della visualizzazione (come ad esempio riportare indietro il `Categories` la tabella s `CategoryName` campo quando si visualizzano le informazioni sul prodotto). Allo stesso tempo, potrebbe essere necessario consentire all'utente di modificare, aggiornare o inserire dati nella tabella di base (`Products`, in questo caso).

Mentre il `INSERT`, `UPDATE`, e `DELETE` istruzioni possono essere immessi manualmente, prendere in considerazione il suggerimento di risparmiare tempo seguente. Configurare inizialmente SqlDataSource in modo che nuovamente estrae i dati solo dal `Products` tabella. Utilizzare le colonne di specificare s procedura guidata Configura origine dati da una schermata di tabella o vista in modo che è possibile generare automaticamente il `INSERT`, `UPDATE`, e `DELETE` istruzioni. Quindi, dopo aver completato la procedura guidata, scegliere di configurare il SelectQuery dalla finestra delle proprietà (o, in alternativa, tornare alla procedura guidata Configura origine dati, ma utilizzando un'istruzione SQL personalizzata specifica o opzione della stored procedure). Aggiornare quindi il `SELECT` istruzione includa la `JOIN` sintassi. Questa tecnica offre i vantaggi di risparmiare tempo delle istruzioni SQL generate automaticamente e consente una più personalizzata `SELECT` istruzione.

Un'altra limitazione di generare automaticamente il `INSERT`, `UPDATE`, e `DELETE` istruzioni è che le colonne nel `INSERT` e `UPDATE` istruzioni sono basate sulle colonne restituite dal `SELECT` istruzione. È possibile che sia necessario aggiornare o inserire i campi più o meno, tuttavia. Ad esempio, nell'esempio dal passaggio 2, forse si vuole avere la `UnitPrice` BoundField essere di sola lettura. In tal caso, si t nozioni sono visualizzati di `UpdateCommand`. Oppure si può decidere di impostare il valore di un campo di tabella che non compare nel GridView. Ad esempio, quando si aggiunge un nuovo record talvolta potremmo desiderare di `QuantityPerUnit` valore impostato su TODO.

Se sono necessarie tali personalizzazioni, è necessario renderli manualmente, tramite la finestra Proprietà, di specificare un'istruzione SQL personalizzata o opzione della stored procedure nella procedura guidata o tramite la sintassi dichiarativa.

> [!NOTE]
> Durante l'aggiunta di parametri che non dispongono di campi corrispondenti nei dati di controllo Web, tenere presente che questi valori di parametri dovranno essere assegnati valori in qualche modo. Questi valori possono essere: impostate come hardcoded direttamente nel `InsertCommand` o `UpdateCommand`; possono provenire da un'origine predefinita (la stringa di query, lo stato della sessione, controlli Web in una pagina e così via) o possono essere assegnati a livello di programmazione, come mostrato nell'esercitazione precedente.


## <a name="summary"></a>Riepilogo

Affinché i dati a che controlli Web, per poter usare gli incorporate di inserimento, la modifica e l'eliminazione di funzionalità, è necessario concedere il controllo origine dati a che sono associate tale funzionalità. Per SqlDataSource, ciò significa che `INSERT`, `UPDATE`, e `DELETE` istruzioni SQL devono essere assegnate per il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà. Queste proprietà e le raccolte di parametri corrispondenti, possono essere aggiunti manualmente o generate automaticamente tramite la procedura guidata Configura origine dati. In questa esercitazione abbiamo esaminato entrambe le tecniche.

Sono stati esaminati la concorrenza ottimistica con oggetto ObjectDataSource i [implementazione della concorrenza ottimistica](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) esercitazione. Il controllo SqlDataSource fornisce inoltre supporto della concorrenza ottimistica. Come indicato nel passaggio 2, durante la generazione automatica di `INSERT`, `UPDATE`, e `DELETE` (istruzioni), la procedura guidata offre un'opzione di usare la concorrenza ottimistica. Come vedremo nella prossima esercitazione, la concorrenza ottimistica con SqlDataSource modifica il `WHERE` clausole nel `UPDATE` e `DELETE` istruzioni per assicurarsi che i valori per le altre colonne che t modificato dall'ultima volta i dati visualizzato nella pagina.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [Successivo](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
