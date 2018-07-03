---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: Aggiornamento del TableAdapter per l'uso dei join (VB) | Microsoft Docs
author: rick-anderson
description: Quando si lavora con un database è comune per i dati della richiesta che viene distribuiti tra più tabelle. Per recuperare dati da due diverse tabelle è possibile usare una...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 9987d4dab7de4fc19d36625fcebc9d63e21acbe8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377172"
---
<a name="updating-the-tableadapter-to-use-joins-vb"></a>Aggiornamento del TableAdapter per l'uso dei join (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip) o [Scarica il PDF](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> Quando si lavora con un database è comune per i dati della richiesta che viene distribuiti tra più tabelle. Per recuperare dati da due diverse tabelle è possibile usare una sottoquery correlata o un'operazione di JOIN. In questa esercitazione si confronta sottoquery correlate e la sintassi di JOIN prima di esaminare come creare un oggetto TableAdapter che include un JOIN nella query principale.


## <a name="introduction"></a>Introduzione

Con i database relazionali siamo interessati a usare i dati sono spesso sparse tra più tabelle. Ad esempio, quando si visualizzano informazioni sui prodotti è probabile che si voglia elencare ogni categoria di prodotto s corrispondente e nomi s fornitori. Il `Products` tabella ha `CategoryID` e `SupplierID` i valori, ma i nomi di categoria e il fornitore effettivi si trovano nel `Categories` e `Suppliers` tabelle, rispettivamente.

Per recuperare informazioni da un altro, alla tabella correlata, è possibile usare *sottoquery correlate* oppure `JOIN` *s*. Una sottoquery correlata è nidificato `SELECT` query che fa riferimento alle colonne nella query esterna. Ad esempio, nelle [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) usassimo due sottoquery correlate nell'esercitazione il `ProductsTableAdapter` query principale s per restituire i nomi di categoria e il fornitore per ogni prodotto. Oggetto `JOIN` è un costrutto SQL che unisce le righe correlate da due diverse tabelle. È stato usato un `JOIN` nella [Querying Data con il controllo SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md) esercitazione per visualizzare le informazioni di categoria insieme a ogni prodotto.

Il motivo è stata astenuto dall'utilizzo `JOIN` s con gli oggetti TableAdapter è a causa delle limitazioni della procedura guidata s TableAdapter per generare automaticamente corrispondente `INSERT`, `UPDATE`, e `DELETE` istruzioni. In particolare, se contiene la query principale s TableAdapter `JOIN` s, TableAdapter non crea automaticamente le istruzioni SQL ad hoc o stored procedure per la relativa `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà.

In questa esercitazione verrà confrontato brevemente e contrasto sottoquery correlate e `JOIN` s prima di esaminare come creare un oggetto TableAdapter che include `JOIN` s la query principale.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Il confronto e contrasto sottoquery correlate e`JOIN` s

Si tenga presente che il `ProductsTableAdapter` creato nella prima esercitazione nel `Northwind` set di dati Usa sottoquery correlate per riportare in ciascun prodotto s categoria e il fornitore nome corrispondente. Il `ProductsTableAdapter` query principale s è illustrata di seguito.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

I due sottoquery - correlate `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` e `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -sono `SELECT` query che restituiscono un singolo valore per ogni prodotto come una colonna aggiuntiva in esterna `SELECT` elenco colonne istruzione s.

In alternativa, un `JOIN` può essere utilizzato per restituire ogni nome di un fornitore e la categoria di prodotto s. La query seguente restituisce lo stesso output come quello riportato sopra, ma usa `JOIN` s al posto di sottoquery:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

Oggetto `JOIN` unisce i record da una tabella con i record da un'altra tabella in base ad alcuni criteri specificati. Nella query precedente, ad esempio, il `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` indica a SQL Server per unire ogni record di prodotto con la categoria di record la cui proprietà `CategoryID` valore corrisponda al prodotto s `CategoryID` valore. Il risultato unito consente di lavorare con i campi categoria corrispondente per ogni prodotto (ad esempio `CategoryName`).

> [!NOTE]
> `JOIN` s sono comunemente usate durante l'esecuzione di query dei dati dai database relazionali. Se si ha familiarità con le `JOIN` sintassi o senza dover rinfrescarti un bit sul relativo utilizzo, d consiglio il [esercitazione SQL Join](http://www.w3schools.com/sql/sql_join.asp) alla [W3 scuole](http://www.w3schools.com/). Inoltre la pena di leggere sono la [ `JOIN` Fundamentals](https://msdn.microsoft.com/library/ms191517.aspx) e [nozioni fondamentali di sottoquery](https://msdn.microsoft.com/library/ms189575.aspx) sezioni del [documentazione Online di SQL](https://msdn.microsoft.com/library/ms130214.aspx).


Poiché `JOIN` s e sottoquery correlate sia utilizzabile per recuperare i dati correlati da altre tabelle, molti sviluppatori vengono lasciati imbattuto mentalmente e chiedersi quale approccio usare. Tutti i guru di SQL si ve parlata per avrebbe dovuto dirlo di approssimativamente la stessa cosa, che t davvero importanti termini di prestazioni poiché SQL Server produrranno i piani di esecuzione all'incirca identico. I consigli, quindi, consiste nell'usare la tecnica che ha maggiore familiarità con il team. Occorre fare particolare notare che dopo imprimere a questo consiglio questi esperti immediatamente express le preferenze di `JOIN` s sulle sottoquery correlate.

Quando si compila un livello di accesso ai dati utilizzando dataset tipizzati, gli strumenti funzionano meglio quando si utilizza una sottoquery. In particolare, la configurazione guidata TableAdapter s non genererà automaticamente corrispondente `INSERT`, `UPDATE`, e `DELETE` istruzioni se la query principale contiene `JOIN` s, ma genererà automaticamente queste istruzioni quando correlate sottoquery vengono usati.

Per esplorare questa lacuna, creare un DataSet tipizzato temporaneo nel `~/App_Code/DAL` cartella. Durante la configurazione guidata TableAdapter, scegliere di usare le istruzioni SQL ad hoc e immettere le informazioni seguenti `SELECT` query (vedere la figura 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]


[![Immettere una Query principale che contiene i join](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**Figura 1**: immettere una Query principale contenente `JOIN` s ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))


Per impostazione predefinita, verrà automaticamente creato dell'oggetto TableAdapter `INSERT`, `UPDATE`, e `DELETE` istruzioni in base alla query principale. Se si fa clic sul pulsante Avanzate è possibile vedere che questa funzionalità è abilitata. Nonostante questa impostazione, l'oggetto TableAdapter non sarà in grado di creare il `INSERT`, `UPDATE`, e `DELETE` istruzioni perché la query principale include un `JOIN`.


![Immettere una Query principale che contiene i join](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**Figura 2**: immettere una Query principale contenente `JOIN` s


Fare clic su Fine per completare la procedura guidata. A questo punto il set di dati s progettazione includerà un singolo TableAdapter con un oggetto DataTable con colonne per ognuno dei campi restituiti nel `SELECT` elenco colonne query s. Ciò include la `CategoryName` e `SupplierName`, come illustrato nella figura 3.


![L'oggetto DataTable include una colonna per ogni campo restituito nell'elenco delle colonne](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**Figura 3**: DataTable include una colonna per ogni campo restituito nell'elenco delle colonne


Mentre la classe DataTable ha le colonne appropriate, TableAdapter non include i valori per la relativa `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà. Per verificarlo, fare clic sull'oggetto TableAdapter nella finestra di progettazione e quindi passare alla finestra Proprietà. Si vedrà che il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà vengono impostate su (nessuno).


[![La proprietà InsertCommand UpdateCommand e proprietà DeleteCommand sono impostate su (nessuno)](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**Figura 4**: il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà vengono impostate su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))


Per risolvere questo problema, è possibile specificare manualmente le istruzioni SQL e i parametri per il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà tramite la finestra Proprietà. In alternativa, è possibile iniziare configurando query TableAdapter s principale per *non* includere eventuali `JOIN` s. In questo modo il `INSERT`, `UPDATE`, e `DELETE` istruzioni per essere generato automaticamente per noi. Dopo aver completato la procedura guidata, quindi è stato possibile aggiornare manualmente i TableAdapter `SelectCommand` dalla finestra delle proprietà in modo che includa il `JOIN` sintassi.

Sebbene questo approccio funzioni, è molto fragile quando si usa query SQL ad-hoc perché ogni volta che la query principale s TableAdapter è nuovamente configurata tramite la procedura guidata, generata automaticamente `INSERT`, `UPDATE`, e `DELETE` istruzioni vengono ricreate. Pertanto, tutte le personalizzazioni in un secondo momento abbiamo andrebbero perse se pulsante destro del mouse su TableAdapter, ha scelto di configurare il menu di scelta rapida e completare la procedura guidata nuovamente.

Inconvenienti di istanze della classe TableAdapter autogenerato `INSERT`, `UPDATE`, e `DELETE` istruzioni è, Fortunatamente, limitato a istruzioni SQL ad hoc. Se l'oggetto TableAdapter utilizza stored procedure, è possibile personalizzare il `SelectCommand`, `InsertCommand`, `UpdateCommand`, o `DeleteCommand` stored procedure ed eseguire di nuovo la configurazione guidata TableAdapter senza temere che saranno le stored procedure modificato.

Prossimi diversi passaggi, si creerà un oggetto TableAdapter, inizialmente, viene utilizzata una query principale che omette una `JOIN` s in modo che il corrispondente insert, update e delete stored procedure sarà generato automaticamente. Quindi si aggiornerà il `SelectCommand` così che usa un `JOIN` che restituisce le colonne aggiuntive da tabelle correlate. Infine, si verrà creato una classe di livello aziendale per la logica corrispondente e illustrano l'uso del TableAdapter in una pagina web ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Passaggio 1: Creazione dell'oggetto TableAdapter mediante una Query principale semplificata

Per questa esercitazione si aggiungerà un TableAdapter e fortemente tipizzata di DataTable per il `Employees` tabella di `NorthwindWithSprocs` set di dati. Il `Employees` tabella contiene una `ReportsTo` campo specificato il `EmployeeID` della gestione dipendente s. Dipendente, ad esempio, Anne Dodsworth ha un `ReportTo` valore pari a 5, ovvero il `EmployeeID` di Steven Buchanan. Di conseguenza, Anne segnala a Steven, suo responsabile. Insieme a ogni dipendente s reporting `ReportsTo` valore, potrebbe essere inoltre necessario recuperare il nome della loro gestione. Ciò è possibile usare un `JOIN`. Ma tramite un `JOIN` quando si crea inizialmente il TableAdapter preclude la procedura guidata di generazione automatica di inserimento corrispondente, aggiornare ed eliminare le funzionalità. Pertanto, si inizierà creando un oggetto TableAdapter cui query principale non contiene alcun `JOIN` s. Quindi, nel passaggio 2, si aggiornerà la procedura principale query archiviate per recuperare il nome della gestione s tramite un `JOIN`.

Iniziare aprendo il `NorthwindWithSprocs` set di dati nel `~/App_Code/DAL` cartella. Pulsante destro del mouse nella finestra di progettazione, selezionare l'opzione Aggiungi dal menu di scelta rapida e scegliere la voce di menu TableAdapter. Verrà avviata la configurazione guidata TableAdapter. Come viene illustrata nella figura 5, hanno la procedura guidata Crea nuove stored procedure e fare clic su Avanti. Per rivedere la creazione di nuove stored procedure dalla procedura guidata s TableAdapter, consultare il [creazione di nuove Stored procedure per DataSet tipizzata s TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) esercitazione.


[![Selezionare Crea nuove stored procedure. opzione](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**Figura 5**: selezionare la creazione di nuove stored procedure opzione ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))


Usare il comando seguente `SELECT` istruzione per query TableAdapter s principale:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

Poiché questa query non includa alcun `JOIN` s, la configurazione guidata TableAdapter creerà automaticamente stored procedure con i corrispondenti `INSERT`, `UPDATE`, e `DELETE` istruzioni, nonché una stored procedure per l'esecuzione principale query.

Il passaggio seguente consente di denominare le procedure di archiviati TableAdapter. Usare i nomi `Employees_Select`, `Employees_Insert`, `Employees_Update`, e `Employees_Delete`, come illustrato nella figura 6.


[![Denominare le procedure di archiviati TableAdapter](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**Figura 6**: nome Stored procedure del TableAdapter s ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))


Il passaggio finale viene richiesto di denominare i metodi di s TableAdapter. Uso `Fill` e `GetEmployees` come nomi di metodo. Inoltre assicurarsi di lasciare il Crea metodi per inviare aggiornamenti direttamente per la casella di controllo database (GenerateDBDirectMethods) selezionata.


[![Nome del riempimento di metodi TableAdapter s e GetEmployees](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**Figura 7**: assegnare un nome ai metodi di TableAdapter `Fill` e `GetEmployees` ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))


Dopo aver completato la procedura guidata, si consiglia di esaminare le stored procedure nel database. Dovrebbe essere quattro nuovi file: `Employees_Select`, `Employees_Insert`, `Employees_Update`, e `Employees_Delete`. Successivamente, esaminare i `EmployeesDataTable` e `EmployeesTableAdapter` appena creato. L'oggetto DataTable contiene una colonna per ogni campo restituito dalla query principale. Fare clic sull'oggetto TableAdapter e quindi passare alla finestra Proprietà. Si vedrà che il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà siano configurate correttamente per chiamare le stored procedure corrispondente.


[![TableAdapter include Insert, Update e Delete funzionalità](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**Figura 8**: TableAdapter include Insert, Update e Delete funzionalità ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))


Con l'inserimento, aggiornamento ed eliminazione di stored procedure create automaticamente e il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà configurata correttamente, siamo pronti personalizzare il `SelectCommand` s stored procedure per restituire aggiuntive informazioni su ogni dipendente s responsabile. In particolare, è necessario aggiornare il `Employees_Select` stored procedure per usare una `JOIN` e restituisce il gestore s `FirstName` e `LastName` valori. Dopo aver aggiornata la stored procedure, è necessario aggiornare l'oggetto DataTable in modo che includa queste colonne aggiuntive. Che verranno affrontati queste due attività in passaggi 2 e 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Passaggio 2: Personalizzare la Stored Procedure da includere un`JOIN`

Inizio passando a Esplora Server, eseguire il drill-nella cartella Northwind database s Stored procedure e aprendo la `Employees_Select` stored procedure. Se non viene visualizzata questa stored procedure, fare doppio clic sulla cartella Stored procedure e scegliere Aggiorna. Aggiornare la stored procedure in modo che utilizzi un `LEFT JOIN` per restituire la gestione s prima di tutto nome e cognome:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

Dopo aver aggiornato il `SELECT` istruzione, salvare le modifiche selezionando il menu File e scegliendo Salva `Employees_Select`. In alternativa, è possibile fare clic sull'icona Salva nella barra degli strumenti o premere Ctrl + S. Dopo aver salvato le modifiche, fare clic su di `Employees_Select` stored procedure in Esplora Server e scegliere Execute. Verrà eseguito la stored procedure e visualizzare i risultati nella finestra di Output (vedere la figura 9).


[![I risultati di procedure archiviate sono visualizzati nella finestra di Output](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**Figura 9**: la Stored procedure di risultati vengono visualizzate nella finestra di Output ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Passaggio 3: Aggiornare le colonne s DataTable

A questo punto, il `Employees_Select` stored procedure restituisce `ManagerFirstName` e `ManagerLastName` i valori, ma il `EmployeesDataTable` manca queste colonne. Queste colonne mancante possono essere aggiunto alla tabella di dati in uno dei due modi:

- **Manualmente** : fare clic su DataTable in Progettazione DataSet e, nel menu Aggiungi scegliere colonna. È quindi possibile assegnare un nome di colonna e impostare di conseguenza le relative proprietà.
- **Automaticamente** -la configurazione guidata TableAdapter aggiornerà le colonne s DataTable in modo da riflettere i campi restituiti dai `SelectCommand` stored procedure. Quando si usano istruzioni SQL ad hoc, la procedura guidata rimuoverà anche il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà perché il `SelectCommand` contiene ora un `JOIN`. Tuttavia, quando si utilizzano le stored procedure, queste proprietà comando rimangono invariate.

Sono stati esplorati aggiunta manuale di colonne DataTable nelle esercitazioni precedenti, tra cui [Master/dettaglio mediante un elenco puntato di record Master e un controllo DataList di dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) e [caricare file](../working-with-binary-files/uploading-files-vb.md), si riceverà una Esaminiamo questo processo nuovamente in maggiore dettaglio nell'esercitazione successiva. Per questa esercitazione, tuttavia, consentire s utilizzare l'approccio automatico tramite la configurazione guidata TableAdapter.

Per iniziare, facendo clic su di `EmployeesTableAdapter` e selezione di Configura dal menu di scelta rapida. Verrà visualizzata la configurazione guidata TableAdapter, cui sono elencate le stored procedure utilizzate per la selezione, inserimento, aggiornamento ed eliminazione, insieme ai relativi valori restituiti e parametri (se presente). Figura 10 è illustrata la procedura guidata. Qui possiamo vedere che il `Employees_Select` stored procedure restituisce ora il `ManagerFirstName` e `ManagerLastName` campi.


[![La procedura guidata Mostra elenco delle colonne aggiornate per il Employees_Select Stored Procedure](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**Figura 10**: la procedura guidata mostra l'elenco delle colonne aggiornate per il `Employees_Select` Stored Procedure ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))


Completare la procedura guidata, fare clic su Fine. Quando si torna alla finestra di progettazione set di dati, il `EmployeesDataTable` include due colonne aggiuntive: `ManagerFirstName` e `ManagerLastName`.


[![Della scheda Seleziona contiene due nuove colonne](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**Figura 11**: il `EmployeesDataTable` contiene due nuove colonne ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))


Per dimostrare che l'aggiornamento `Employees_Select` stored procedure è attivo e che l'inserimento, aggiornamento ed eliminazione di funzionalità dell'oggetto TableAdapter sono ancora attivi, consentire s di creare una pagina web che consente agli utenti di visualizzare ed eliminare i dipendenti. Prima di creare tale pagina, tuttavia, è necessario innanzitutto creare una nuova classe nel livello di logica di Business per l'uso con i dipendenti dal `NorthwindWithSprocs` set di dati. Nel passaggio 4, si creerà un `EmployeesBLLWithSprocs` classe. Nel passaggio 5, si userà questa classe da una pagina ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Passaggio 4: Implementazione del livello di logica di Business

Creare un nuovo file di classe nel `~/App_Code/BLL` cartella denominata `EmployeesBLLWithSprocs.vb`. Questa classe in grado di simulare la semantica dell'oggetto esistente `EmployeesBLL` (classe), solo questa nuova uno fornisce metodi di un numero minore e si usa la `NorthwindWithSprocs` set di dati (anziché il `Northwind` set di dati). Aggiungere il codice seguente alla classe `EmployeesBLLWithSprocs`.


[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

Il `EmployeesBLLWithSprocs` classe s `Adapter` viene restituita un'istanza del `NorthwindWithSprocs` s set di dati `EmployeesTableAdapter`. Viene utilizzato dalla classe 1!s `GetEmployees` e `DeleteEmployee` metodi. Il `GetEmployees` chiamate al metodo il `EmployeesTableAdapter` s corrispondente `GetEmployees` metodo che richiama la `Employees_Select` stored procedure e popola i risultati in un `EmployeeDataTable`. Il `DeleteEmployee` metodo chiama in modo analogo il `EmployeesTableAdapter` s `Delete` metodo che richiama la `Employees_Delete` stored procedure.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Passaggio 5: Utilizzo dei dati nel livello di presentazione

Con il `EmployeesBLLWithSprocs` classe completa, sono pronti per lavorare con i dati dei dipendenti tramite una pagina ASP.NET. Aprire il `JOINs.aspx` nella pagina la `AdvancedDAL` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativo `ID` proprietà `Employees`. Successivamente, GridView s nello smart tag, eseguire l'associazione della griglia per un nuovo controllo ObjectDataSource denominato `EmployeesDataSource`.

Configurare ObjectDataSource per usare la `EmployeesBLLWithSprocs` classe e, tra le schede SELECT e DELETE, assicurarsi che il `GetEmployees` e `DeleteEmployee` metodi sono selezionati dagli elenchi a discesa. Fare clic su Fine per completare la configurazione di s ObjectDataSource.


[![Configurare ObjectDataSource per usare la classe EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**Figura 12**: configurare ObjectDataSource per usare il `EmployeesBLLWithSprocs` classe ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))


[![È possibile utilizzare ObjectDataSource i metodi di DeleteEmployee e GetEmployees](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**Figura 13**: è possibile utilizzare ObjectDataSource i `GetEmployees` e `DeleteEmployee` metodi ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))


Visual Studio aggiungerà un BoundField di GridView per ciascuna del `EmployeesDataTable` colonne s. Rimuovere tutti questi BoundField, ad eccezione di `Title`, `LastName`, `FirstName`, `ManagerFirstName`, e `ManagerLastName` e rinominare il `HeaderText` le proprietà per le ultime quattro BoundField Last Name, First Name, nome Manager s, e Gestione s Last Name, rispettivamente.

Per consentire agli utenti di eliminare i dipendenti da questa pagina è necessario eseguire due operazioni. In primo luogo, indicare il controllo GridView per fornire funzionalità di eliminazione selezionando l'opzione Abilita eliminazione dal suo smart tag. In secondo luogo, modificare gli oggetti ObjectDataSource `OldValuesParameterFormatString` proprietà dal valore impostato dalla procedura guidata ObjectDataSource (`original_{0}`) il valore predefinito (`{0}`). Dopo aver apportato queste modifiche, il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

Testare la pagina visitando, tramite un browser. Come illustrato nella figura 14, la pagina consente di visualizzare ogni dipendente e il suo nome s manager (presupponendo che dispongono di uno).


[![Il JOIN nel Employees_Select Stored Procedure restituisce il nome della gestione s](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**Figura 14**: il `JOIN` nel `Employees_Select` Stored Procedure restituisce il nome di gestione ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))


Facendo clic sul pulsante Elimina viene avviato il flusso di lavoro l'eliminazione, che culmina nell'esecuzione del `Employees_Delete` stored procedure. Tuttavia, il tentativo `DELETE` istruzione nella stored procedure ha esito negativo a causa di una violazione di vincolo di chiave esterna (vedere Figura 15). In particolare, ciascun dipendente ha uno o più record `Orders` tabella, che causa l'eliminazione esito negativo.


[![L'eliminazione di un dipendente con risultati gli ordini corrispondenti in una violazione di vincolo di chiave esterna](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**Figura 15**: l'eliminazione di un dipendente con risultati gli ordini corrispondenti in una violazione di vincolo di chiave esterna ([fare clic per visualizzare l'immagine con dimensioni normali](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))


Per consentire a un dipendente eliminato è possibile:

- Aggiornare il vincolo di chiave esterna per propagare le eliminazioni,
- Eliminare manualmente i record dal `Orders` tabella per i dipendente (i) si desidera eliminare, o
- Aggiornamento di `Employees_Delete` stored procedure per eliminare innanzitutto i record correlati dal `Orders` tabella prima di eliminare il `Employees` record. È stata illustrata questa tecnica nel [utilizzando Stored procedure esistenti per DataSet tipizzata s TableAdapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) esercitazione.

Lasciare come esercizio per il lettore.

## <a name="summary"></a>Riepilogo

Quando si lavora con i database relazionali, è comune per le query per estrarre i dati da più tabelle correlate. Sottoquery correlate e `JOIN` forniscono due tecniche diverse per l'accesso ai dati da tabelle correlate in una query. Nelle esercitazioni precedenti abbiamo più di frequente utilizzano di sottoquery correlate in quanto il TableAdapter non è possibile generare automaticamente `INSERT`, `UPDATE`, e `DELETE` le istruzioni per le query che implica `JOIN` s. Anche se questi valori possono essere immessi manualmente, quando si usano istruzioni SQL ad-hoc tutte le personalizzazioni verranno sovrascritti quando viene completata la configurazione guidata TableAdapter.

Fortunatamente, gli oggetti TableAdapter creato utilizzando le stored procedure non sono soggetti a inconvenienti stesso come quelle create mediante istruzioni SQL ad hoc. Pertanto, è possibile creare un oggetto TableAdapter cui query principale Usa un `JOIN` quando si utilizzano le stored procedure. In questa esercitazione è stato illustrato come creare questo tipo un TableAdapter. Abbiamo iniziato con un `JOIN`-meno `SELECT` eseguire una query per la query principale s TableAdapter in modo che la corrispondente insert, update e delete stored procedure sarà creato automaticamente. TableAdapter s iniziale completato la configurazione, è aumentata la `SelectCommand` stored procedure per usare una `JOIN` ed eseguito nuovamente la configurazione guidata TableAdapter per aggiornare il `EmployeesDataTable` colonne s.

Eseguire nuovamente la configurazione guidata TableAdapter automaticamente aggiornata il `EmployeesDataTable` colonne in modo da riflettere i campi di dati restituiti dal `Employees_Select` stored procedure di. In alternativa, avremmo potuto aggiungere queste colonne manualmente al DataTable. Verranno analizzati manualmente aggiungendo colonne alla tabella di dati nella prossima esercitazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Geisenow Hilton, David Suru e Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Successivo](adding-additional-datatable-columns-vb.md)
