---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: Creazione di un livello di logica di Business (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione si vedrà come centralizzare le regole di business in un livello Business (LOGIC) che funge da intermediario per lo scambio di dati tra t...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 1e9fc60d9f6867063c3d4dc4faa8d73a683ec87c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803136"
---
<a name="creating-a-business-logic-layer-c"></a>Creazione di un livello di logica di Business (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe) o [Scarica il PDF](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> In questa esercitazione si noterà come centralizzare le regole di business in un livello Business (LOGIC) che funge da intermediario per lo scambio di dati tra il livello di presentazione e DAL.


## <a name="introduction"></a>Introduzione

La Data Access Layer (DAL) creato nel [prima esercitazione](creating-a-data-access-layer-cs.md) separa correttamente i dati di accesso per la logica dalla logica di presentazione. Tuttavia, mentre il campo DAL separa i dettagli di accesso ai dati dal livello di presentazione, non applica tutte le regole di business che potrebbero essere applicate. Ad esempio, per l'applicazione si potrebbe essere necessario impedire il `CategoryID` o `SupplierID` i campi del `Products` tabella da modificare quando il `Discontinued` campo è impostato su 1, o potrebbe essere necessario applicare le regole anzianità, divieto di situazioni in cui un dipendente è gestito da un utente che è stato assunto dopo la loro. Un altro scenario comune è forse solo agli utenti autorizzazioni in un particolare ruolo possa eliminare i prodotti o modifica il `UnitPrice` valore.

In questa esercitazione si noterà come centralizzare le regole di business in un livello Business (LOGIC) che funge da intermediario per lo scambio di dati tra il livello di presentazione e DAL. In un'applicazione reale, il livello BLL deve essere implementato come un progetto libreria di classi separato. Tuttavia, per queste esercitazioni viene implementato il livello BLL come una serie di classi nel nostro `App_Code` cartella per semplificare la struttura del progetto. Figura 1 illustra le relazioni dell'architettura tra il livello di presentazione, livello BLL e DAL.


![Il livello BLL separa il livello di presentazione dal livello di accesso ai dati e impone le regole di Business](creating-a-business-logic-layer-cs/_static/image1.png)

**Figura 1**: il livello BLL separa il livello di presentazione dal livello di accesso ai dati e impone le regole di Business


## <a name="step-1-creating-the-bll-classes"></a>Passaggio 1: Creare le classi BLL

Il livello BLL sarà composta da quattro classi, uno per ogni oggetto TableAdapter in DAL; ognuna di queste classi BLL avrà i metodi per il recupero, inserimento, aggiornamento ed eliminazione dalla rispettivo TableAdapter in DAL, applicare le regole di business appropriata.

Per più correttamente separare le classi correlate a DAL e BLL, è possibile creare due sottocartelle nella `App_Code` cartella `DAL` e `BLL`. È sufficiente fare clic su di `App_Code` cartella in Esplora soluzioni e scegliere Nuova cartella. Dopo aver creato queste due cartelle, spostare il set di dati tipizzati creati nella prima esercitazione nel `DAL` sottocartella.

Successivamente, creare i quattro file di classe BLL nel `BLL` sottocartella. A tale scopo, fare clic su di `BLL` sottocartella, scegliere Aggiungi un nuovo elemento e scegliere il modello di classe. Denominare le quattro classi `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, e `EmployeesBLL`.


![Aggiungere quattro nuove classi nella cartella App_Code](creating-a-business-logic-layer-cs/_static/image2.png)

**Figura 2**: aggiungere quattro nuove classi per il `App_Code` cartella


Successivamente, è possibile aggiungere metodi a ognuna delle classi eseguire semplicemente il wrapping i metodi definiti per gli oggetti TableAdapter nella prima esercitazione. Per il momento, questi metodi verranno solo chiamare direttamente DAL; è necessario ritornare in seguito per aggiungere una logica di business necessaria.

> [!NOTE]
> Se si usa Visual Studio Standard Edition o versione successiva (vale a dire, si ha *non* utilizzando Visual Web Developer), è facoltativamente possibile progettare le classi in modo visivo mediante il [Progettazione classi](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Vedere il [Blog di progettazione classi](https://blogs.msdn.com/classdesigner/default.aspx) per altre informazioni su questa nuova funzionalità di Visual Studio.


Per il `ProductsBLL` classe dobbiamo aggiungere un totale di sette metodi:

- `GetProducts()` Restituisce tutti i prodotti
- `GetProductByProductID(productID)` Restituisce il prodotto con l'ID prodotto specificato
- `GetProductsByCategoryID(categoryID)` Restituisce tutti i prodotti della categoria specificata
- `GetProductsBySupplier(supplierID)` Restituisce tutti i prodotti dal fornitore specificato
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` Inserisce un nuovo prodotto nel database usando i valori passati aggiuntivo. Restituisce il `ProductID` valore del nuovo record inserito
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` Aggiorna un prodotto esistente nel database usando i valori passati. Restituisce `true` se è stata aggiornata esattamente una riga, `false` in caso contrario,
- `DeleteProduct(productID)` Elimina il prodotto specificato dal database

ProductsBLL.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

I metodi che restituiscono semplicemente i dati `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, e `GetProductBySuppliersID` sono piuttosto semplici come chiamano semplicemente verso il basso nel DAL. In alcuni scenari possono essere anche le regole di business che devono essere implementati in questo livello (ad esempio, le regole di autorizzazione basata su ruolo a cui appartiene l'utente o l'utente attualmente connesso), verranno mantenute semplicemente questi metodi come-è. Per questi metodi, quindi, il livello BLL funge semplicemente un proxy tramite cui il livello di presentazione accede ai dati sottostanti dal livello di accesso ai dati.

Il `AddProduct` e `UpdateProduct` metodi sia accettano come parametri i valori per i vari campi del prodotto e aggiungere un nuovo prodotto o aggiornarne uno esistente, rispettivamente. Poiché molte delle `Product` le colonne della tabella possono accettare `NULL` valori (`CategoryID`, `SupplierID`, e `UnitPrice`, per citarne alcuni), i parametri di input per `AddProduct` e `UpdateProduct` che eseguono il mapping a tale utilizzo di colonne [tipi nullable](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Tipi nullable hanno familiarità con .NET 2.0 e fornire una tecnica per che indica se un tipo di valore deve essere, invece di essere `null`. In c# è possibile contrassegnare un tipo di valore come tipo nullable aggiungendo `?` dopo il tipo (ad esempio `int? x;`). Fare riferimento al [tipi Nullable](https://msdn.microsoft.com/library/1t3y8s4s.aspx) sezione il [Guida per programmatori c#](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx) per altre informazioni.

Tutti e tre i metodi restituiscono un valore booleano che indica se una riga è stata inserita, aggiornata o eliminata dopo l'operazione non può generare una riga interessata. Ad esempio, se lo sviluppatore della pagina chiama `DeleteProduct` passando un `ProductID` per un prodotto inesistente, il `DELETE` istruzione eseguita nel database non hanno alcun effetto e pertanto il `DeleteProduct` metodo restituirà `false`.

Si noti che quando si aggiunge un nuovo prodotto o l'aggiornamento di uno esistente viene preso in valori di campo del prodotto nuovo o modificato un elenco di valori scalari invece di accettare un `ProductsRow` istanza. Questo approccio è stato scelto perché il `ProductsRow` classe deriva da ADO.NET `DataRow` (classe), che non ha un costruttore senza parametri predefinito. Per creare un nuovo `ProductsRow` istanza, è innanzitutto necessario creare un `ProductsDataTable` dell'istanza e richiamare quindi relativo `NewProductRow()` metodo (che eseguiamo `AddProduct`). Questa lacuna ovaiolo la testa quando è attiva per l'inserimento e aggiornamento dei prodotti utilizzando ObjectDataSource. In breve, ObjectDataSource proverà a creare un'istanza dei parametri di input. Se il metodo BLL prevede un `ProductsRow` istanza, ObjectDataSource tenterà di crearne uno, ma non riuscire a causa della mancanza di un costruttore senza parametri predefinito. Per altre informazioni su questo problema, vedere il post di forum su ASP.NET due seguenti: [ObjectDataSource l'aggiornamento con i set di dati Strongly-Typed](https://forums.asp.net/1098630/ShowPost.aspx), e [problema con ObjectDataSource e set di dati Strongly-Typed](https://forums.asp.net/1048212/ShowPost.aspx).

Successivamente, in entrambe `AddProduct` e `UpdateProduct`, il codice crea un `ProductsRow` dell'istanza e lo popola con i valori passati solo. Quando si assegnano valori a colonne di dati di un DataRow possono verificarsi diversi controlli di convalida a livello di campo. Pertanto, manualmente inserire nuovamente i valori passati in un DataRow aiuta a garantire la validità dei dati passati al metodo BLL. Purtroppo le classi di DataRow fortemente tipizzato generate da Visual Studio non usano tipi nullable. Piuttosto, per indicare che una colonna di dati specifico in un DataRow deve corrispondere a un `NULL` database valore è necessario usare il `SetColumnNameNull()` (metodo).

Nelle `UpdateProduct` carichiamo prima di tutto all'interno del prodotto da aggiornare usando `GetProductByProductID(productID)`. Anche se può sembrare un trip non necessario al database, questa ulteriore percorso si rivelerà utile in esercitazioni future che illustrano l'uso della concorrenza ottimistica. La concorrenza ottimistica è una tecnica per garantire che due utenti che lavorano contemporaneamente sugli stessi dati non sovrascrivere accidentalmente modifiche di altri. Cattura l'intero record inoltre rende più semplice creare i metodi di aggiornamento nel livello BLL che modificano solo un subset di colonne di DataRow. Quando si Esplora il `SuppliersBLL` vedremo un esempio di questo tipo di classe.

Infine, si noti che il `ProductsBLL` classe presenta il [attributo DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) applicato (il `[System.ComponentModel.DataObject]` sintassi subito prima dell'istruzione di classe nella parte superiore del file) e i metodi avranno [ Gli attributi DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). Il `DataObject` contrassegnato dall'attributo della classe come un oggetto adatto per l'associazione a un [controllo ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), mentre il `DataObjectMethodAttribute` indica lo scopo del metodo. Come vedremo in futuro le esercitazioni, ObjectDataSource di ASP.NET 2.0 semplifica in modo dichiarativo per accedere ai dati da una classe. Per filtrare l'elenco di possibili classi a cui associarsi nella procedura guidata di ObjectDataSource, per impostazione predefinita, solo le classi contrassegnate come `DataObjects` vengono visualizzati nell'elenco a discesa della procedura guidata. Il `ProductsBLL` classe funziona altrettanto bene senza questi attributi, ma aggiungendoli rende più facile lavorare con nella procedura guidata di ObjectDataSource.

## <a name="adding-the-other-classes"></a>Aggiunta di altre classi

Con il `ProductsBLL` classe completa, è comunque necessario aggiungere le classi per l'utilizzo di categorie, fornitori e dipendenti. Si consiglia di creare le classi e i concetti dell'esempio precedente usando i metodi seguenti:

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

Il metodo la pena notare è il `SuppliersBLL` della classe `UpdateSupplierAddress` (metodo). Questo metodo fornisce un'interfaccia per l'aggiornamento solo informazioni sull'indirizzo del fornitore. Internamente, questo metodo legge il `SupplierDataRow` oggetto per l'oggetto specificato `supplierID` (usando `GetSupplierBySupplierID`), imposta le proprietà correlate agli indirizzi e quindi chiama verso il basso nel `SupplierDataTable`del `Update` (metodo). Il `UpdateSupplierAddress` metodo indicato di seguito:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

Fare riferimento al download di questo articolo per la mia implementazione completa delle classi BLL.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Passaggio 2: L'accesso i dataset tipizzati tramite le classi BLL

Nella prima esercitazione abbiamo visto alcuni esempi di utilizzo diretto del DataSet tipizzato a livello di codice, ma con l'aggiunta delle nostre classi BLL, il livello di presentazione dovrebbe funzionare con il livello BLL invece. Nel `AllProducts.aspx` riportato nella prima esercitazione, il `ProductsTableAdapter` è stata utilizzata per associare l'elenco dei prodotti in un controllo GridView, come illustrato nel codice seguente:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

Usare il livello BLL nuove classi, tutto questo deve essere modificata è la prima riga del codice sostituire semplicemente il `ProductsTableAdapter` dell'oggetto con un `ProductBLL` oggetto:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

Le classi BLL inoltre sono accessibili in modo dichiarativo (come può il set di dati tipizzato) utilizzando ObjectDataSource. L'oggetto ObjectDataSource in maggiore dettaglio verranno descritte le esercitazioni seguenti.


[![Viene visualizzato l'elenco di prodotti in un oggetto GridView](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**Figura 3**: viene visualizzato l'elenco di prodotti in un controllo GridView ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-business-logic-layer-cs/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Passaggio 3: Aggiunta della convalida a livello di campo per le classi di DataRow

La convalida a livello di campo sono indicati i controlli che riguarda i valori delle proprietà degli oggetti business durante l'inserimento o aggiornamento. Alcune regole di convalida a livello di campo per i prodotti includono:

- Il `ProductName` campo deve contenere 40 caratteri o una lunghezza
- Il `QuantityPerUnit` campo deve essere una lunghezza o 20 caratteri
- Il `ProductID`, `ProductName`, e `Discontinued` campi sono obbligatori, ma tutti gli altri campi sono facoltativi
- Il `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` campi devono essere maggiore o uguale a zero

Queste regole possono e devono essere espressi a livello di database. Il limite di caratteri di `ProductName` e `QuantityPerUnit` i campi vengono acquisiti dai tipi di dati di tali colonne nel `Products` tabella (`nvarchar(40)` e `nvarchar(20)`rispettivamente). Se i campi sono obbligatori e facoltativi sono espressi da se la colonna della tabella del database consente `NULL` s. Quattro [vincoli check](https://msdn.microsoft.com/library/ms188258.aspx) esiste che assicura che soli valori maggiori o uguali a zero possono nel `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, o `ReorderLevel` colonne.

Oltre all'implementazione di queste regole a livello di database sono inoltre devono essere applicati a livello di set di dati. In effetti, la lunghezza del campo e se un valore è obbligatorio o facoltativo già vengono acquisiti per set di ogni oggetto DataTable di DataColumn. Per visualizzare la convalida a livello di campo esistente viene fornita automaticamente, passare alla finestra di progettazione set di dati, selezionare un campo da uno degli oggetti DataTable e quindi passare alla finestra Proprietà. Come illustrato nella figura 4, il `QuantityPerUnit` DataColumn nel `ProductsDataTable` ha una lunghezza massima di 20 caratteri e consentire `NULL` valori. Se si tenta di impostare il `ProductsDataRow`del `QuantityPerUnit` proprietà su un valore di stringa più di 20 caratteri un `ArgumentException` verrà generata.


[![La DataColumn fornisce la convalida di base a livello di campo](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**Figura 4**: la DataColumn fornisce a livello di campo convalida di base ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-business-logic-layer-cs/_static/image8.png))


Sfortunatamente, non possiamo specifichiamo verifiche dei limiti, ad esempio il `UnitPrice` valore deve essere maggiore o uguale a zero, nella finestra delle proprietà. Per fornire questo tipo di convalida a livello di campo è necessario creare un gestore eventi per la classe DataTable [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) evento. Come accennato nella [esercitazione precedente](creating-a-data-access-layer-cs.md), gli oggetti DataSet, DataTable e DataRow creati dal set di dati tipizzati possono essere estesi tramite l'uso di classi parziali. Utilizzando questa tecnica è possibile creare un `ColumnChanging` gestore dell'evento per il `ProductsDataTable` classe. Iniziare creando una classe nel `App_Code` cartella denominata `ProductsDataTable.ColumnChanging.cs`.


[![Aggiungere una nuova classe alla cartella App_Code](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**Figura 5**: aggiungere una nuova classe per il `App_Code` cartella ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-business-logic-layer-cs/_static/image11.png))


Successivamente, creare un gestore eventi per il `ColumnChanging` eventi che assicura che il `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` i valori di colonna (in caso contrario `NULL`) sono maggiori o uguali a zero. Se qualsiasi colonna di questo tipo è compreso nell'intervallo valido, viene generato un `ArgumentException`.

ProductsDataTable.ColumnChanging.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Passaggio 4: Aggiunta di regole di Business personalizzata per le classi del livello di regola business

Oltre alla convalida a livello di campo, potrebbero essere presenti le regole di business personalizzata di alto livello che interessano diverse entità o concetti non può essere espresso a livello di colonna singola, ad esempio:

- Se non è più disponibile un prodotto, relativo `UnitPrice` non può essere aggiornata
- Paese del dipendente, un paese di residenza deve corrispondere al paese del responsabile di residenza
- Un prodotto non può essere interrotta se è il solo prodotto fornito dal fornitore

Le classi BLL devono contenere controlli per garantire la conformità alle regole di business dell'applicazione. È possibile aggiungere questi controlli direttamente ai metodi a cui sono applicate.

Si supponga che le regole aziendali prevedono che un prodotto potrebbe non essere contrassegnata come non più utilizzato se fosse l'unico prodotto da un fornitore specificato. Vale a dire, se prodotto *X* era il solo prodotto viene acquistati dal fornitore *Y*, non è stato possibile contrassegnare il *X* poiché interrotta; se, tuttavia, supplier *Y*us fornito con i tre prodotti *un'*, *B*, e *C*, quindi è stato possibile contrassegnare qualsiasi e tutti questi componenti come non più disponibile. Una regola di business strano, ma le regole di business e buon senso sempre non sono allineate.

Per applicare questa regola di business nel `UpdateProducts` metodo, si dovrebbe iniziare verificando se `Discontinued` è stata impostata su `true` e, in caso affermativo, si potrebbe chiamare `GetProductsBySupplierID` per determinare la quantità di prodotti Microsoft acquistati dal fornitore del prodotto. Se solo un unico prodotto viene acquistato presso il fornitore, verrà generata un' `ApplicationException`.


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Risposta agli errori di convalida al livello presentazione

Quando si chiama il livello BLL dal livello di presentazione è possibile decidere se tentare di gestire le eccezioni che potrebbero essere generate o lasciare i propagate ASP.NET (che consente di avviare il `HttpApplication`del `Error` evento). Per gestire un'eccezione quando si lavora con il livello BLL, a livello di codice, è possibile usare una [try... catch](https://msdn.microsoft.com/library/0yd65esw.aspx) blocco, come illustrato nell'esempio seguente:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

Come vedremo in futuro le esercitazioni, la gestione delle eccezioni che viene rilevato da BLL quando si usa una data Web controllo per l'inserimento, aggiornamento, o l'eliminazione dei dati possono essere gestito direttamente in un gestore eventi eliminando la necessità di eseguire il wrapping di codice nel `try...catch` blocchi.

## <a name="summary"></a>Riepilogo

Un'applicazione ben strutturata è distribuita su più livelli distinti, ognuno dei quali incapsula un determinato ruolo. Nella prima esercitazione di questa serie di articoli è stato creato un livello di accesso ai dati utilizzando dataset tipizzati; in questa esercitazione abbiamo creato un livello di logica di Business come una serie di classi nella nostra applicazione `App_Code` cartella che effettuano chiamate verso il basso nel nostro DAL. Il livello BLL implementa la logica a livello di campo e a livello di business per la nostra applicazione. Oltre alla creazione di un livello BLL separato, come abbiamo fatto in questa esercitazione, un'altra opzione consiste nell'estendere i metodi degli oggetti di TableAdapter tramite l'uso di classi parziali. Tuttavia, utilizzando questa tecnica non consente di eseguire l'override di metodi esistenti e separare il livello BLL e DAL nostro rendere più chiara l'approccio che abbiamo impiegato in questo articolo.

Con DAL e BLL completo, siamo pronti per iniziare il livello di presentazione. Nel [prossima esercitazione](master-pages-and-site-navigation-cs.md) verrà accettano una breve deviazione dagli argomenti di accesso dati e definire un layout delle pagine coerente per l'utilizzo durante le esercitazioni.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono Liz Shulok, Dennis Patterson, Carlos Santos e Hilton Giesenow. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](creating-a-data-access-layer-cs.md)
> [Successivo](master-pages-and-site-navigation-cs.md)
