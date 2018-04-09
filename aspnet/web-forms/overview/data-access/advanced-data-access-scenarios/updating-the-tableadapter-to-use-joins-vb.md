---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: L'aggiornamento dell'oggetto TableAdapter per usare join (VB) | Documenti Microsoft
author: rick-anderson
description: Quando si utilizza un database è comune per i dati della richiesta che viene distribuiti tra più tabelle. Per recuperare dati da due diverse tabelle è possibile utilizzare...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 91d700f3de02dc78692e933644e221e2ac8175a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="updating-the-tableadapter-to-use-joins-vb"></a>L'aggiornamento dell'oggetto TableAdapter per usare join (Visual Basic)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip) o [Scarica il PDF](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> Quando si utilizza un database è comune per i dati della richiesta che viene distribuiti tra più tabelle. Per recuperare dati da due tabelle diverse, è possibile utilizzare una sottoquery correlata o un'operazione di JOIN. In questa esercitazione è confrontare sottoquery correlate e la sintassi JOIN prima di esaminare come creare un oggetto TableAdapter che include un JOIN nella query principale.


## <a name="introduction"></a>Introduzione

Con i database relazionali i dati di che si è interessati durante l'utilizzo spesso viene distribuiti tra più tabelle. Ad esempio, quando si visualizzano informazioni sui prodotti è probabilmente da elencare ogni categoria di prodotto s corrispondente e i nomi s fornitore. Il `Products` tabella include `CategoryID` e `SupplierID` valori, ma i nomi di categoria e il fornitore effettivi presenti il `Categories` e `Suppliers` tabelle, rispettivamente.

Per recuperare informazioni da un altro, alla tabella correlata, è possibile utilizzare *sottoquery correlate* o `JOIN` *s*. Una sottoquery correlata è nidificato `SELECT` query che fa riferimento alle colonne nella query esterna. Ad esempio, nel [la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) è stata utilizzata due sottoquery correlate nell'esercitazione di `ProductsTableAdapter` query principale di s per restituire i nomi di categoria e il fornitore per ogni prodotto. Oggetto `JOIN` è un costrutto SQL che unisce le righe correlate da due tabelle diverse. È stato usato un `JOIN` nel [query su dati con il controllo SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md) esercitazione per visualizzare le informazioni di categoria insieme a ogni prodotto.

Il motivo è stata astenuto dall'utilizzo di `JOIN` con gli oggetti TableAdapter è a causa delle limitazioni della procedura guidata s TableAdapter per generare automaticamente corrispondente `INSERT`, `UPDATE`, e `DELETE` istruzioni. In particolare, se la query principale di TableAdapter s contiene qualsiasi `JOIN` s, TableAdapter non crea automaticamente le istruzioni SQL ad hoc o la stored procedure per il relativo `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà.

In questa esercitazione confronteremo brevemente e contrasto sottoquery correlate e `JOIN` s prima di esaminare come creare un oggetto TableAdapter che include `JOIN` s la query principale.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Il confronto e contrasto sottoquery correlate e`JOIN` s

Tenere presente che il `ProductsTableAdapter` creato nella prima esercitazione del `Northwind` set di dati utilizza sottoquery correlate per riportare ogni prodotto s categoria e il fornitore nome corrispondente. Il `ProductsTableAdapter` query principale s è illustrata di seguito.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

I due subquery - `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` e `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -sono `SELECT` query che restituiscono un singolo valore per ogni prodotto come una colonna aggiuntiva in esterna `SELECT` elenco colonne istruzione s.

In alternativa, un `JOIN` può essere utilizzato per restituire ogni nome di un fornitore e la categoria di prodotto s. La query seguente restituisce lo stesso output di quello precedente, ma utilizza `JOIN` s al posto di sottoquery:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

Oggetto `JOIN` unisce i record da una tabella con i record da un'altra tabella in base ad alcuni criteri specificati. Nella query precedente, ad esempio, il `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` indica a SQL Server per unire ogni record di prodotto con la categoria record il cui `CategoryID` valore corrisponda al prodotto s `CategoryID` valore. Risultati sottoposti a merge consentono di operare con i campi categoria corrispondente per ogni prodotto (ad esempio `CategoryName`).

> [!NOTE]
> `JOIN` s comunemente utilizzati per eseguire query sui dati dai database relazionali. Se si ha familiarità con il `JOIN` sintassi o necessità di rinfrescarti la memoria bit sul relativo utilizzo, d si consiglia di [esercitazione Join SQL](http://www.w3schools.com/sql/sql_join.asp) in [scuole W3](http://www.w3schools.com/). Anche la pena di lettura sono i [ `JOIN` nozioni di base](https://msdn.microsoft.com/library/ms191517.aspx) e [principi fondamentali di sottoquery](https://msdn.microsoft.com/library/ms189575.aspx) sezioni del [documentazione Online di SQL](https://msdn.microsoft.com/library/ms130214.aspx).


Poiché `JOIN` s e sottoquery correlate possono essere utilizzate entrambe per recuperare i dati correlati da altre tabelle, molti sviluppatori restano eventuali graffi le intestazioni e chiedere l'approccio da utilizzare. Tutti i controbattono SQL si ve parlata per hanno affermato approssimativamente la stessa operazione, che t è importante inoltro come SQL Server genera i piani di esecuzione quasi identici. I consigli, sono quindi utilizzare la tecnica di utente e il team ha maggiore familiarità con. Opportuno notare che dopo d'imprimere questa raccomandazione questi esperti immediatamente express la preferenza di `JOIN` s sulle sottoquery correlate.

Quando si compila un livello di accesso ai dati utilizzando i dataset tipizzati, gli strumenti funzionano meglio durante l'utilizzo di sottoquery. In particolare, la configurazione guidata TableAdapter s non genererà automaticamente corrispondente `INSERT`, `UPDATE`, e `DELETE` istruzioni se la query principale contiene i `JOIN` s, ma genererà automaticamente queste istruzioni quando correlato sottoquery vengono utilizzati.

Per esplorare questo problema, creare un DataSet tipizzato temporaneo nel `~/App_Code/DAL` cartella. Durante la configurazione guidata TableAdapter, scegliere di utilizzare istruzioni SQL ad hoc e immettere la seguente `SELECT` query (vedere la figura 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]


[![Immettere una Query principale che contiene join](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**Figura 1**: immettere una Query principale contenente `JOIN` s ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))


Per impostazione predefinita, il TableAdapter creerà automaticamente `INSERT`, `UPDATE`, e `DELETE` istruzioni in base alla query principale. Se si fa clic sul pulsante Avanzate è possibile vedere che questa funzionalità è abilitata. Nonostante questa impostazione, l'oggetto TableAdapter non sarà in grado di creare il `INSERT`, `UPDATE`, e `DELETE` istruzioni perché la query principale include un `JOIN`.


![Immettere una Query principale che contiene join](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**Figura 2**: immettere una Query principale che contiene `JOIN` s


Fare clic su Fine per completare la procedura guidata. A questo punto la finestra di progettazione di s set di dati include un solo oggetto TableAdapter con un oggetto DataTable con colonne per ognuno dei campi restituiti nel `SELECT` elenco colonne query s. Ciò include la `CategoryName` e `SupplierName`, come illustrato nella figura 3.


![DataTable include una colonna per ogni campo restituito nell'elenco di colonne](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**Figura 3**: DataTable include una colonna per ogni campo restituito nell'elenco delle colonne


Mentre il DataTable contiene le colonne appropriate, TableAdapter non dispone di valori per il relativo `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà. A tale scopo, fare clic sul TableAdapter nella finestra di progettazione e quindi passare alla finestra Proprietà. Vi si noterà che il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà vengono impostate su (nessuno).


[![La proprietà InsertCommand, UpdateCommand e proprietà DeleteCommand vengono impostate su (nessuno)](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**Figura 4**: il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà vengono impostate su (nessuno) ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))


Per risolvere questo problema, è possibile specificare manualmente le istruzioni SQL e i parametri per il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà mediante la finestra Proprietà. In alternativa, è stato possibile avviare configurando query TableAdapter s principale per *non* includere `JOIN` s. In questo modo il `INSERT`, `UPDATE`, e `DELETE` istruzioni generati automaticamente per noi. Dopo aver completato la procedura guidata, quindi è possibile aggiornare manualmente i TableAdapter `SelectCommand` dalla finestra delle proprietà in modo che includa il `JOIN` sintassi.

Quando viene utilizzato questo approccio, è molto efficace quando riconfigurare la procedura guidata generata automaticamente tramite query SQL ad hoc, perché ogni volta che la query principale di TableAdapter s `INSERT`, `UPDATE`, e `DELETE` vengono ricreate le istruzioni. Ciò significa che tutte le personalizzazioni in un secondo momento, sono stati apportati andrebbero perse se si pulsante destro del mouse sul TableAdapter, scelta Configura dal menu di scelta rapida e completata nuovamente la procedura guidata.

Il vulnerabilità ai TableAdapter generate automaticamente `INSERT`, `UPDATE`, e `DELETE` istruzioni è Fortunatamente, limitato alle istruzioni SQL ad hoc. Se l'oggetto TableAdapter utilizza stored procedure, è possibile personalizzare il `SelectCommand`, `InsertCommand`, `UpdateCommand`, o `DeleteCommand` stored procedure ed eseguire di nuovo la configurazione guidata TableAdapter senza temere che saranno le stored procedure modificato.

Su quella successiva diversi passaggi, si creerà un oggetto TableAdapter che, inizialmente, Usa una query principale che omette qualsiasi `JOIN` s in modo che sia corrispondente insert, update e delete stored procedure verrà generato automaticamente. Quindi si aggiornerà il `SelectCommand` così che utilizza un `JOIN` che restituisce le colonne aggiuntive da tabelle correlate. Infine, si verrà creare una classe di livello di logica di Business corrispondente e dimostrare l'utilizzo dell'oggetto TableAdapter in una pagina web ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Passaggio 1: Creazione del TableAdapter utilizzando una Query principale semplificata

Per questa esercitazione si aggiungerà un oggetto TableAdapter e DataTable fortemente tipizzata per la `Employees` tabella il `NorthwindWithSprocs` set di dati. Il `Employees` tabella contiene un `ReportsTo` campo che specifica il `EmployeeID` della gestione dipendente s. Ad esempio, i dipendenti Anne Dodsworth ha un `ReportTo` valore pari a 5, ovvero il `EmployeeID` di Steven Buchanan. Di conseguenza, Anne segnala a Steven, responsabile. Insieme a ogni dipendente s reporting `ReportsTo` valore, potrebbe essere inoltre necessario recuperare il nome del gestore. Questa può essere eseguita tramite un `JOIN`. Ma utilizzando un `JOIN` quando inizialmente creazione dell'oggetto TableAdapter impedendo la procedura guidata di generazione automatica dell'inserimento corrispondente, aggiornamento ed eliminazione di funzionalità. Pertanto, si inizierà creando un oggetto TableAdapter cui query principale non contiene alcuna `JOIN` s. Quindi, nel passaggio 2, Microsoft aggiornerà la procedura principale query archiviate per recuperare il nome del gestore s tramite un `JOIN`.

Aprire il `NorthwindWithSprocs` set di dati nel `~/App_Code/DAL` cartella. Pulsante destro del mouse nella finestra di progettazione, selezionare l'opzione Aggiungi dal menu di scelta rapida e scegliere la voce di menu TableAdapter. Verrà avviata la configurazione guidata TableAdapter. Come viene illustrata nella figura 5, hanno la procedura guidata Crea nuove stored procedure e fare clic su Avanti. Per un aggiornamento per la creazione di nuove stored procedure dalla configurazione guidata TableAdapter s, consultare il [la creazione di nuove Stored procedure per gli oggetti TableAdapter s DataSet tipizzato](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) esercitazione.


[![Selezionare Crea nuove stored procedure. opzione](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**Figura 5**: selezionare Crea nuove stored procedure opzione ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))


Utilizzare la seguente `SELECT` istruzione per la query principale di TableAdapter s:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

Poiché questa query non include alcun `JOIN` s, la configurazione guidata TableAdapter creerà automaticamente stored procedure con corrispondente `INSERT`, `UPDATE`, e `DELETE` istruzioni, nonché una stored procedure per l'esecuzione principale query.

Il passaggio seguente consente di denominare le procedure archiviati TableAdapter. Utilizzare i nomi `Employees_Select`, `Employees_Insert`, `Employees_Update`, e `Employees_Delete`, come illustrato nella figura 6.


[![Nome del TableAdapter s Stored procedure](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**Figura 6**: nome di Stored procedure del TableAdapter s ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))


Il passaggio finale viene richiesto di denominare metodi s dell'oggetto TableAdapter. Utilizzare `Fill` e `GetEmployees` come i nomi di metodo. È inoltre necessario assicurarsi di lasciare Crea metodi per inviare aggiornamenti direttamente alla casella di controllo database (GenerateDBDirectMethods) selezionata.


[![Nome il riempimento di metodi TableAdapter s e GetEmployees](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**Figura 7**: nome i metodi di TableAdapter `Fill` e `GetEmployees` ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))


Dopo aver completato la procedura guidata, è opportuno esaminare le stored procedure nel database. Dovrebbe essere quattro nuovi: `Employees_Select`, `Employees_Insert`, `Employees_Update`, e `Employees_Delete`. Successivamente, esaminare il `EmployeesDataTable` e `EmployeesTableAdapter` appena creato. DataTable contiene una colonna per ogni campo restituito dalla query principale. Fare clic sul TableAdapter e quindi passare alla finestra Proprietà. Vi si noterà che il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà siano configurate correttamente per chiamare le stored procedure corrispondente.


[![TableAdapter include Insert, Update e Delete funzionalità](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**Figura 8**: TableAdapter include Insert, Update e le funzionalità di eliminazione ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))


Con l'inserimento, aggiornamento ed eliminazione di stored procedure create automaticamente e `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà configurata correttamente, si è pronti per personalizzare il `SelectCommand` s stored procedure per restituire ulteriori informazioni su ogni dipendente s responsabile. In particolare, è necessario aggiornare il `Employees_Select` stored procedure per utilizzare un `JOIN` e restituire il gestore s `FirstName` e `LastName` valori. Dopo aver aggiornata la stored procedure, è necessario aggiornare il DataTable in modo che includa tali colonne aggiuntive. È possibile affrontare queste due attività nei passaggi 2 e 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Passaggio 2: Personalizzare la Stored Procedure per includere un`JOIN`

Iniziare da Esplora Server, il drill-down nella cartella Northwind database s Stored procedure e aprendo la `Employees_Select` stored procedure. Se non viene visualizzata questa stored procedure, fare doppio clic sulla cartella Stored procedure e scegliere Aggiorna. Aggiornare la stored procedure in modo che utilizzi un `LEFT JOIN` per restituire la gestione s innanzitutto e cognome:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

Dopo aver aggiornato il `SELECT` istruzione, salvare le modifiche dal menu File e scegliendo Salva `Employees_Select`. In alternativa, è possibile fare clic sull'icona Salva nella barra degli strumenti o premere Ctrl + S. Dopo aver salvato le modifiche, fare clic su di `Employees_Select` stored procedure in Esplora Server e scegliere Esegui. Verrà eseguito la stored procedure e visualizzare i risultati nella finestra di Output (vedere Figura 9).


[![I risultati di procedure archiviate vengono visualizzati nella finestra di Output](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**Figura 9**: la Stored procedure risultati vengono visualizzati nella finestra di Output ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Passaggio 3: Aggiornare le colonne s DataTable

A questo punto, il `Employees_Select` stored procedure restituisce `ManagerFirstName` e `ManagerLastName` valori, ma la `EmployeesDataTable` manca queste colonne. È possibile aggiungere queste colonne mancante nella DataTable in uno dei due modi:

- **Manualmente** - destro del mouse sul DataTable in Progettazione DataSet e scegliere dal menu Aggiungi colonna. Quindi, è possibile assegnare un nome di colonna e impostarne le proprietà di conseguenza.
- **Automaticamente** -la configurazione guidata TableAdapter aggiornerà le colonne s DataTable per riflettere i campi restituiti dal `SelectCommand` stored procedure. Quando si Usa istruzioni SQL ad hoc, la procedura guidata rimuoverà anche la `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà dopo il `SelectCommand` contiene ora un `JOIN`. Ma quando si utilizzano stored procedure, queste proprietà comando rimangono intatte.

È stata esplorare aggiunta manuale di colonne DataTable in esercitazioni precedenti, tra cui [Master/dettaglio mediante un elenco puntato dei record Master con un controllo DataList dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) e [caricamento di file](../working-with-binary-files/uploading-files-vb.md), e verrà esaminare questo processo nuovamente in modo più dettagliato nell'esercitazione successiva. Per questa esercitazione, tuttavia, consentono s, utilizzare l'approccio automatico tramite la configurazione guidata TableAdapter.

Avviare facendo clic su di `EmployeesTableAdapter` e selezionando Configura dal menu di scelta rapida. Verrà visualizzata la configurazione guidata TableAdapter, che elenca le stored procedure utilizzate per la selezione, inserimento, aggiornamento e l'eliminazione, con i relativi valori restituiti e parametri (se presente). Figura 10 è illustrata la procedura guidata. Qui è possibile osservare che il `Employees_Select` stored procedure restituisce ora il `ManagerFirstName` e `ManagerLastName` campi.


[![La procedura guidata Mostra elenco delle colonne aggiornate per il Employees_Select Stored Procedure](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**Figura 10**: la procedura guidata mostra l'elenco delle colonne aggiornate per il `Employees_Select` Stored Procedure ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))


Completare la procedura guidata, fare clic su Fine. All'uscita alla finestra di progettazione set di dati, il `EmployeesDataTable` include due colonne aggiuntive: `ManagerFirstName` e `ManagerLastName`.


[![Della scheda Seleziona contiene due nuove colonne](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**Figura 11**: il `EmployeesDataTable` contiene due nuove colonne ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))


Per illustrare che l'aggiornamento `Employees_Select` stored procedure è attivo e che l'inserimento, aggiornamento ed eliminazione delle funzionalità dell'oggetto TableAdapter funzionante, consente di creare una pagina web che consente agli utenti di visualizzare ed eliminare i dipendenti s. Prima di creare una pagina, tuttavia, è necessario innanzitutto creare una nuova classe nel livello di logica di Business per l'utilizzo con i dipendenti di `NorthwindWithSprocs` set di dati. Nel passaggio 4, si creerà un `EmployeesBLLWithSprocs` classe. Nel passaggio 5 si utilizzerà questa classe da una pagina ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Passaggio 4: Implementare il livello di logica di Business

Creare un nuovo file di classe nel `~/App_Code/BLL` cartella denominata `EmployeesBLLWithSprocs.vb`. Questa classe riproduce la semantica dell'oggetto esistente `EmployeesBLL` (classe), solo questa nuova uno fornisce un minor numero di metodi e utilizza il `NorthwindWithSprocs` set di dati (anziché il `Northwind` set di dati). Aggiungere il codice seguente alla classe `EmployeesBLLWithSprocs`.


[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

Il `EmployeesBLLWithSprocs` classe s `Adapter` proprietà restituisce un'istanza di `NorthwindWithSprocs` s set di dati `EmployeesTableAdapter`. Viene utilizzato dalla classe s `GetEmployees` e `DeleteEmployee` metodi. Il `GetEmployees` chiamate al metodo di `EmployeesTableAdapter` s corrispondente `GetEmployees` metodo che richiama la `Employees_Select` stored procedure e popola i risultati in un `EmployeeDataTable`. Il `DeleteEmployee` metodo chiama in modo analogo la `EmployeesTableAdapter` s `Delete` metodo che richiama la `Employees_Delete` stored procedure.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Passaggio 5: Utilizzo dei dati nel livello di presentazione

Con la `EmployeesBLLWithSprocs` classe completo, è nuovamente pronto per funzionare con i dati dei dipendenti tramite una pagina ASP.NET. Aprire il `JOINs.aspx` nella pagina di `AdvancedDAL` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativa `ID` proprietà `Employees`. Successivamente, da GridView s smart tag, associare la griglia per un nuovo controllo ObjectDataSource denominato `EmployeesDataSource`.

Configurare ObjectDataSource per utilizzare il `EmployeesBLLWithSprocs` classe e, dalla scheda selezionare ed eliminare, assicurarsi che il `GetEmployees` e `DeleteEmployee` i metodi vengono selezionati dagli elenchi a discesa. Fare clic su Fine per completare la configurazione di ObjectDataSource s.


[![Configurare ObjectDataSource per utilizzare la classe EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**Figura 12**: configurare ObjectDataSource per usare il `EmployeesBLLWithSprocs` classe ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))


[![È possibile utilizzare ObjectDataSource le GetEmployees e i metodi di DeleteEmployee](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**Figura 13**: è possibile utilizzare ObjectDataSource il `GetEmployees` e `DeleteEmployee` metodi ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))


Visual Studio aggiungerà un BoundField a GridView per ognuna del `EmployeesDataTable` colonne s. Rimuovere tutti questi BoundField, ad eccezione di `Title`, `LastName`, `FirstName`, `ManagerFirstName`, e `ManagerLastName` e rinominare il `HeaderText` le proprietà per le ultime quattro BoundField Last Name, nome, nome Manager s, e Gestione s Cognome, rispettivamente.

Per consentire agli utenti di eliminare i dipendenti da questa pagina è necessario eseguire due operazioni. In primo luogo, indicare GridView per fornire funzionalità di eliminazione selezionando l'opzione Abilita eliminazione dal suo smart tag. In secondo luogo, modificare il s ObjectDataSource `OldValuesParameterFormatString` dal valore impostata dalla procedura guidata ObjectDataSource (`original_{0}`) al valore predefinito (`{0}`). Dopo aver apportato queste modifiche, il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

Il test della pagina, visitare il sito tramite un browser. Come mostrato nella figura 14, la pagina consente di visualizzare ogni dipendente e il suo nome s manager (presupponendo che dispongano di uno).


[![Il JOIN nel Employees_Select Stored Procedure restituisce il nome del gestore s](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**Figura 14**: il `JOIN` nel `Employees_Select` Stored Procedure restituisce il nome di gestore ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))


Fare clic sul pulsante Elimina viene avviato il flusso di lavoro l'eliminazione, culmina nell'esecuzione del `Employees_Delete` stored procedure. Tuttavia, il tentativo `DELETE` istruzione nella stored procedure non riesce a causa di una violazione di vincolo di chiave esterna (vedere Figura 15). In particolare, ogni dipendente ha uno o più record `Orders` tabella, causando l'eliminazione di esito negativo.


[![L'eliminazione di un dipendente che ha restituito risultati ordini corrispondente in una violazione di vincolo di chiave esterna](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**Figura 15**: l'eliminazione di un dipendente che ha restituito risultati ordini corrispondente in una violazione di vincolo di chiave esterna ([fare clic per visualizzare l'immagine ingrandita](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))


Per consentire a un dipendente eliminato è stato possibile:

- Aggiornare il vincolo di chiave esterno per eliminazioni a catena
- Eliminare manualmente i record di `Orders` tabella per i dipendenti che si desidera eliminare, o
- Aggiornamento di `Employees_Delete` stored procedure per eliminare i record correlati dal `Orders` tabella prima di eliminare il `Employees` record. Questa tecnica in descritto il [utilizzando Stored procedure esistenti per gli oggetti TableAdapter s DataSet tipizzato](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) esercitazione.

Lasciare questo campo come esercizio per il lettore.

## <a name="summary"></a>Riepilogo

Quando si lavora con i database relazionali, è comune per le query per estrarre i dati da più tabelle correlate. Sottoquery correlate e `JOIN` forniscono due tecniche diverse per l'accesso ai dati provenienti da tabelle correlate in una query. Nelle esercitazioni precedenti sono stati creati più di frequente utilizzano di sottoquery correlate in quanto il TableAdapter non è possibile generare automaticamente `INSERT`, `UPDATE`, e `DELETE` istruzioni per query che interessano `JOIN` s. Questi valori possono essere immessi manualmente, quando si Usa istruzioni SQL ad hoc tutte le personalizzazioni verranno sovrascritte quando viene completata la configurazione guidata TableAdapter.

Fortunatamente, gli oggetti TableAdapter creati utilizzando le stored procedure non subiscono del stessa vulnerabilità come quelli creati utilizzando istruzioni SQL ad hoc. Pertanto, è possibile creare un oggetto TableAdapter cui query principale Usa un `JOIN` quando si utilizzano stored procedure. In questa esercitazione è stato illustrato come creare questo tipo un TableAdapter. È stato avviato utilizzando un `JOIN`-minore `SELECT` query per la query principale di TableAdapter s in modo che il corrispondente insert, update e delete stored procedure sarà creato automaticamente. TableAdapter s iniziale completato la configurazione, è aumentata la `SelectCommand` stored procedure per utilizzare un `JOIN` ed eseguita nuovamente la configurazione guidata TableAdapter per aggiornare il `EmployeesDataTable` colonne s.

Eseguire nuovamente la configurazione guidata TableAdapter automaticamente aggiornata il `EmployeesDataTable` colonne in modo da riflettere i campi di dati restituiti dal `Employees_Select` stored procedure di. In alternativa, avremmo potuto aggiungere queste colonne manualmente nella DataTable. Nella prossima esercitazione verrà illustrato aggiunta manuale di colonne nella DataTable.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Hilton Geisenow e David Suru Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Successivo](adding-additional-datatable-columns-vb.md)
