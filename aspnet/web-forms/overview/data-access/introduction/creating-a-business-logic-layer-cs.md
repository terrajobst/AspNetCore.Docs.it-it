---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: Creazione di un livello di logica di Business (c#) | Documenti Microsoft
author: rick-anderson
description: "In questa esercitazione si vedrà come centralizzare le regole business in un livello Business (LOGIC) che funge da intermediario per lo scambio di dati tra t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: d117456521442972f1bfcfc15a4e8e7253e07e53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-business-logic-layer-c"></a>Creazione di un livello di logica di Business (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe) o [Scarica il PDF](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> In questa esercitazione vedremo come centralizzare le regole business in un livello Business (LOGIC) che funge da intermediario per lo scambio di dati tra il livello di presentazione e DAL tipo.


## <a name="introduction"></a>Introduzione

Il livello accesso dati (DAL) creato nel [prima esercitazione](creating-a-data-access-layer-cs.md) logica separa correttamente i dati tramite la logica di presentazione. Tuttavia, mentre DAL correttamente separa i dettagli di accesso ai dati dal livello di presentazione, non applica tutte le regole di business che possono essere applicate. Ad esempio, per l'applicazione potrebbe voler impedisce il `CategoryID` o `SupplierID` campi del `Products` tabella da modificare quando il `Discontinued` campo è impostato su 1, o applicare le regole di anzianità, potrebbe essere opportuno proibizione delle situazioni in cui un dipendente è gestito da un utente che è stato assunto successivamente. Un altro scenario comune è forse solo utenti di autorizzazione in un particolare ruolo possono eliminare i prodotti o modificare il `UnitPrice` valore.

In questa esercitazione vedremo come centralizzare le regole business in un livello Business (LOGIC) che funge da intermediario per lo scambio di dati tra il livello di presentazione e DAL tipo. In un'applicazione reale, il livello Business LOGIC deve essere implementato come un progetto libreria di classi separato. Tuttavia, per queste esercitazioni verrà implementato il livello Business LOGIC come una serie di classi nel nostro `App_Code` cartella per semplificare la struttura del progetto. La figura 1 illustra le relazioni tra il livello di presentazione, BLL e dell'architettura.


![Il livello Business LOGIC separa il livello di presentazione dal livello di accesso ai dati e impone le regole di Business](creating-a-business-logic-layer-cs/_static/image1.png)

**Figura 1**: il livello Business LOGIC separa il livello di presentazione dal livello di accesso ai dati e impone le regole di Business


## <a name="step-1-creating-the-bll-classes"></a>Passaggio 1: Creazione di classi BLL

Il livello Business LOGIC sarà costituita da quattro classi, uno per ogni TableAdapter in DAL; ognuna di queste classi BLL disporrà di metodi per il recupero, inserimento, aggiornamento ed eliminazione dal rispettivo TableAdapter in DAL, applicando le regole di business appropriata.

Per più correttamente separare le classi correlate DAL e BLL, creare due sottocartelle nella `App_Code` cartella `DAL` e `BLL`. È sufficiente fare clic su di `App_Code` cartella in Esplora soluzioni e scegliere Nuova cartella. Dopo la creazione di queste due cartelle, spostare il DataSet tipizzato creato nella prima esercitazione nel `DAL` sottocartella.

Successivamente, creare i quattro file di classe BLL nel `BLL` sottocartella. A tale scopo, fare clic su di `BLL` sottocartella, scegliere Aggiungi un nuovo elemento e scegliere il modello di classe. Nome le quattro classi `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, e `EmployeesBLL`.


![Aggiungere quattro nuove classi alla cartella App_Code](creating-a-business-logic-layer-cs/_static/image2.png)

**Figura 2**: aggiungere quattro nuove classi per la `App_Code` cartella


Successivamente, aggiungere metodi a ognuna delle classi a capo semplicemente i metodi definiti per gli oggetti TableAdapter nella prima esercitazione. Per il momento, questi metodi solo chiamerà direttamente DAL; verrà restituiamo successive per aggiungere qualsiasi logica di business necessari.

> [!NOTE]
> Se si utilizza Visual Studio Standard Edition o versione successiva (ovvero, si è *non* tramite Visual Web Developer), è facoltativamente possibile progettare le classi in modo visivo mediante il [Progettazione classi](https://msdn.microsoft.com/library/default.asp?url=/library/en-us/dv_vstechart/html/clssdsgnr.asp). Consultare la [Blog progettazione classe](https://blogs.msdn.com/classdesigner/default.aspx) per ulteriori informazioni su questa nuova funzionalità in Visual Studio.


Per il `ProductsBLL` è necessario aggiungere un totale di sette metodi di classe:

- `GetProducts()`Restituisce tutti i prodotti
- `GetProductByProductID(productID)`Restituisce il prodotto con l'ID prodotto specificato
- `GetProductsByCategoryID(categoryID)`Restituisce tutti i prodotti dalla categoria specificata
- `GetProductsBySupplier(supplierID)`Restituisce tutti i prodotti dal fornitore specificato
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)`Inserisce un nuovo prodotto nel database utilizzando i valori passati aggiuntivo. Restituisce il `ProductID` valore del record appena inserito
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)`Aggiorna un prodotto esistente nel database utilizzando i valori passati. Restituisce `true` se è stata aggiornata in modo preciso una riga, `false` in caso contrario
- `DeleteProduct(productID)`Elimina il prodotto specificato dal database

ProductsBLL.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

I metodi che restituiscono semplicemente dati `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, e `GetProductBySuppliersID` sono piuttosto semplici come chiamano semplicemente verso il basso in DAL. Mentre in alcuni scenari potrebbe essere regole business che è necessario implementare questo livello (ad esempio le regole di autorizzazione in base a ruolo a cui appartiene l'utente o l'utente attualmente connesso), è sufficiente lasciare questi metodi come-è. Per questi metodi, quindi, il livello Business LOGIC funge da semplicemente un proxy tramite cui il livello di presentazione accede ai dati sottostanti dal livello di accesso ai dati.

Il `AddProduct` e `UpdateProduct` metodi sia accettano come parametri i valori per i diversi campi di prodotto e aggiungere un nuovo prodotto o aggiornare uno esistente, rispettivamente. Poiché molte del `Product` le colonne della tabella possono accettare `NULL` valori (`CategoryID`, `SupplierID`, e `UnitPrice`, solo per citarne alcune), i parametri di input per `AddProduct` e `UpdateProduct` che eseguono il mapping a tale utilizzo di colonne [tipi nullable](https://msdn.microsoft.com/en-us/library/1t3y8s4s(v=vs.80).aspx). Tipi nullable sono nuovi per .NET 2.0 e fornire una tecnica che indica se un tipo di valore, dovrebbe invece essere `null`. In c# è possibile contrassegnare un tipo di valore come un tipo nullable aggiungendo `?` dopo il tipo (ad esempio `int? x;`). Fare riferimento al [tipi Nullable](https://msdn.microsoft.com/en-us/library/1t3y8s4s.aspx) sezione la [Guida per programmatori c#](https://msdn.microsoft.com/en-us/library/67ef8sbd%28VS.80%29.aspx) per ulteriori informazioni.

Tutti e tre i metodi restituiscono un valore booleano che indica se una riga è stata inserita, aggiornata o eliminata dopo l'operazione non può generare una riga interessata. Ad esempio, se lo sviluppatore della pagina chiama `DeleteProduct` passando un `ProductID` per un prodotto inesistente, la `DELETE` istruzione eseguita nel database non hanno alcun effetto e pertanto il `DeleteProduct` metodo restituirà `false`.

Si noti che quando si aggiunge un nuovo prodotto o l'aggiornamento di uno esistente è eseguire nei valori dei campi del prodotto nuove o modificate come un elenco di valori scalari invece che accetta un `ProductsRow` istanza. Questo approccio è stato scelto perché il `ProductsRow` classe deriva da ADO.NET `DataRow` (classe), che non dispone di un costruttore senza parametri predefinito. Per creare un nuovo `ProductsRow` istanza, è innanzitutto necessario creare un `ProductsDataTable` istanza e quindi richiamare il `NewProductRow()` metodo (che nelle `AddProduct`). Questa lacuna ovaiolo partiti quando è attiva per l'inserimento e aggiornamento di prodotti utilizzando ObjectDataSource. In breve, ObjectDataSource tenterà di creare un'istanza dei parametri di input. Se il metodo BLL prevede un `ProductsRow` istanza ObjectDataSource tenterà di crearne uno, ma non riuscire a causa della mancanza di un costruttore senza parametri predefinito. Per ulteriori informazioni su questo problema, vedere il post di forum ASP.NET due seguenti: [ObjectDataSources l'aggiornamento di DataSet Strongly-Typed](https://forums.asp.net/1098630/ShowPost.aspx), e [problema con ObjectDataSource e set di dati Strongly-Typed](https://forums.asp.net/1048212/ShowPost.aspx).

Successivamente, in entrambi `AddProduct` e `UpdateProduct`, il codice crea un `ProductsRow` istanza e lo popola con i valori passati solo. Quando si assegnano valori alle colonne di dati di un DataRow possono verificarsi vari controlli di convalida a livello di campo. Pertanto, manualmente rimettere i valori passati in un DataRow contribuisce a garantire la validità dei dati passati al metodo BLL. Le classi di DataRow fortemente tipizzato generate da Visual Studio Purtroppo non utilizzano i tipi nullable. Invece, per indicare che una colonna di dati specifico in un DataRow deve corrispondere a un `NULL` database valore è necessario utilizzare il `SetColumnNameNull()` metodo.

In `UpdateProduct` è prima di caricare all'interno del prodotto per l'aggiornamento utilizzando `GetProductByProductID(productID)`. Sebbene ciò possa sembrare un trip non necessari al database, questa ulteriore percorso si rivelerà utile in esercitazioni future che esplorano la concorrenza ottimistica. Concorrenza ottimistica è una tecnica per garantire che due utenti che lavorano contemporaneamente sugli stessi dati non sovrascrivere accidentalmente le modifiche apportate da altri. Cattura l'intero record rende anche più facile creare metodi di aggiornamento in BLL che modificano solo un subset di colonne di DataRow. Quando si Esplora il `SuppliersBLL` vedremo un esempio di classe.

Infine, si noti che il `ProductsBLL` classe dispone di [attributo DataObject](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataobjectattribute.aspx) applicato (il `[System.ComponentModel.DataObject]` sintassi subito prima dell'istruzione di classe nella parte superiore del file) e i metodi [ Gli attributi di DataObjectMethodAttribute](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataobjectmethodattribute.aspx). Il `DataObject` contrassegnato dall'attributo della classe come un oggetto adatto per l'associazione a un [controllo ObjectDataSource](https://msdn.microsoft.com/en-us/library/9a4kyhcx.aspx), mentre il `DataObjectMethodAttribute` indica lo scopo del metodo. Come si vedrà in futuro esercitazioni, ASP.NET 2.0 ObjectDataSource semplifica in modo dichiarativo per accedere ai dati da una classe. Per filtrare l'elenco delle classi possibili da associare alle guidata ObjectDataSource, per impostazione predefinita solo le classi contrassegnate come `DataObjects` vengono visualizzati nell'elenco a discesa della procedura guidata. La `ProductsBLL` classe funziona anche senza questi attributi, ma aggiungendoli rende più facile lavorare con la procedura guidata di ObjectDataSource.

## <a name="adding-the-other-classes"></a>Aggiunta di altre classi

Con la `ProductsBLL` classe completo, è comunque necessario aggiungere le classi per l'utilizzo di categorie, fornitori e dipendenti. Dedicare alcuni minuti per creare le seguenti classi e metodi mediante i concetti dell'esempio precedente:

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

È un metodo non degni di nota il `SuppliersBLL` della classe `UpdateSupplierAddress` metodo. Questo metodo fornisce un'interfaccia per l'aggiornamento solo informazioni di indirizzo del fornitore. Internamente, questo metodo legge il `SupplierDataRow` oggetto per l'oggetto specificato `supplierID` (utilizzando `GetSupplierBySupplierID`), imposta le proprietà relative all'indirizzo e quindi chiama verso il basso il `SupplierDataTable`del `Update` (metodo). Il `UpdateSupplierAddress` metodo indicato di seguito:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

Fare riferimento a download di questo articolo per l'implementazione completa delle classi BLL.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Passaggio 2: Accesso tipizzati mediante le classi BLL

Nella prima esercitazione è stato illustrato negli esempi di utilizzo diretto del DataSet tipizzato a livello di codice, ma con l'aggiunta delle nostre classi BLL, il livello di presentazione dovrebbe funzionare con il livello Business LOGIC invece. Nel `AllProducts.aspx` riportato nella prima esercitazione di `ProductsTableAdapter` è stato utilizzato per associare l'elenco dei prodotti a un controllo GridView, come illustrato nel codice seguente:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

Utilizzare il nuovo livello Business LOGIC le classi, tutti deve essere modificato è la prima riga del codice sostituire semplicemente la `ProductsTableAdapter` dell'oggetto con un `ProductBLL` oggetto:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

Le classi BLL sono accessibili anche in modo dichiarativo (come il set di dati tipizzato) utilizzando ObjectDataSource. Illustrati di seguito si ObjectDataSource più dettagliatamente nelle esercitazioni seguenti.


[![Viene visualizzato l'elenco di prodotti in un controllo GridView.](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**Figura 3**: viene visualizzato l'elenco di prodotti in un controllo GridView ([fare clic per visualizzare l'immagine ingrandita](creating-a-business-logic-layer-cs/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Passaggio 3: Aggiunta della convalida a livello di campo per le classi di DataRow

La convalida a livello di campo sono indicati i controlli che riguardano i valori delle proprietà degli oggetti business durante l'inserimento o aggiornamento. Alcune regole di convalida a livello di campo per i prodotti includono:

- Il `ProductName` campo deve essere di 40 caratteri o lunghezza
- Il `QuantityPerUnit` campo deve essere di lunghezza o 20 caratteri
- Il `ProductID`, `ProductName`, e `Discontinued` i campi sono obbligatori, ma tutti gli altri campi sono facoltativi
- Il `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` campi devono essere maggiore o uguale a zero

Queste regole possono e devono essere espresso a livello di database. Il limite di caratteri di `ProductName` e `QuantityPerUnit` per i tipi di dati di tali colonne vengono acquisiti i campi di `Products` tabella (`nvarchar(40)` e `nvarchar(20)`rispettivamente). Se i campi sono obbligatori e facoltativi espresso se la colonna di tabella di database consente `NULL` s. Quattro [vincoli check](https://msdn.microsoft.com/en-us/library/ms188258.aspx) esiste che assicura che solo i valori maggiori di o uguali a zero possono nel `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, o `ReorderLevel` colonne.

Oltre a attivare queste regole nel database sono inoltre devono essere applicati a livello di set di dati. In effetti, la lunghezza del campo e se un valore è obbligatorio o facoltativo già acquisite per set di ogni oggetto DataTable di DataColumn. Per visualizzare la convalida a livello di campo esistente fornita automaticamente, passare alla finestra di progettazione set di dati, selezionare un campo a un DataTable e quindi passare alla finestra Proprietà. Come illustrato nella figura 4, il `QuantityPerUnit` DataColumn nella `ProductsDataTable` ha una lunghezza massima di 20 caratteri e consentire `NULL` valori. Se si tenta di impostare il `ProductsDataRow`del `QuantityPerUnit` su un valore di stringa più di 20 caratteri un `ArgumentException` verrà generata.


[![DataColumn fornisce la convalida di base a livello di campo](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**Figura 4**: la DataColumn fornisce a livello di campo convalida di base ([fare clic per visualizzare l'immagine ingrandita](creating-a-business-logic-layer-cs/_static/image8.png))


È purtroppo, non è possibile specificare, ad esempio, i controlli dei limiti di `UnitPrice` valore deve essere maggiore o uguale a zero, tramite la finestra Proprietà. Per fornire questo tipo di convalida a livello di campo, è necessario creare un gestore eventi per la DataTable [ColumnChanging](https://msdn.microsoft.com/en-us/library/system.data.datatable.columnchanging%28VS.80%29.aspx) evento. Come accennato nel [esercitazione precedente](creating-a-data-access-layer-cs.md), gli oggetti DataSet, DataTable e DataRow creati dal set di dati tipizzati possono essere estesi tramite l'utilizzo di classi parziali. Utilizzando questa tecnica è possibile creare un `ColumnChanging` gestore eventi per il `ProductsDataTable` classe. Iniziare creando una classe di `App_Code` cartella denominata `ProductsDataTable.ColumnChanging.cs`.


[![Aggiungere una nuova classe nella cartella App_Code](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**Figura 5**: aggiungere una nuova classe per il `App_Code` cartella ([fare clic per visualizzare l'immagine ingrandita](creating-a-business-logic-layer-cs/_static/image11.png))


Successivamente, creare un gestore eventi per il `ColumnChanging` evento che assicura che il `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` i valori di colonna (se non `NULL`) sono maggiori o uguali a zero. Se qualsiasi colonna di questo tipo è compreso nell'intervallo, generare un `ArgumentException`.

ProductsDataTable.ColumnChanging.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Passaggio 4: Aggiunta di regole di Business personalizzate per le classi del BLL

Oltre alla convalida a livello di campo, è possibile che le regole di business personalizzata di alto livello che comprendono diverse entità o concetti non può essere espresso a livello di singola colonna, ad esempio:

- Se un prodotto è stata sospesa, il relativo `UnitPrice` non può essere aggiornata
- Paese di un dipendente di residenza deve corrispondere al paese del gestore di residenza
- Un prodotto non può essere sospesa se è l'unico prodotto fornito dal fornitore

Le classi BLL devono contenere i controlli per verificare la conformità alle regole di business dell'applicazione. Questi controlli possono essere aggiunti direttamente ai metodi a cui si applicano.

Si supponga che il nostro regole di business prevedono che un prodotto potrebbe non essere contrassegnato non più supportato se fosse l'unico prodotto da un fornitore specificato. Vale a dire se prodotto *X* è l'unico prodotto è stati acquistati supplier *Y*, non è stato possibile contrassegnare *X* come non più disponibile; se, tuttavia, fornitore *Y*ci fornito con tre prodotti, *A*, *B*, e *C*, è possibile contrassegnare qualsiasi e tutti questi come non più disponibile. Una regola di business dispari, ma le regole di business e senso sempre allineati!

Per applicare questa regola di business nel `UpdateProducts` metodo iniziare verificando se `Discontinued` è stato impostato su `true` e, se in tal caso, occorre chiamare `GetProductsBySupplierID` per determinare il numero di prodotti è stato acquistato il fornitore del prodotto. Se solo un prodotto viene acquistato presso il fornitore, verrà generata una `ApplicationException`.


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Risponde agli errori di convalida al livello presentazione

Quando si chiama il livello Business LOGIC dal livello di presentazione è possibile decidere se tentare di gestire tutte le eccezioni che potrebbero essere generate o lasciare propagate a ASP.NET (che genererà il `HttpApplication`del `Error` evento). Per gestire un'eccezione quando si lavora con il livello Business LOGIC a livello di codice, è possibile utilizzare un [try … catch](https://msdn.microsoft.com/en-us/library/0yd65esw.aspx) blocco, come illustrato nell'esempio seguente:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

Come possiamo vedere esercitazioni in futuro, gestione delle eccezioni dal BLL quando si utilizza un tipo di dati Web di controllo per l'inserimento, aggiornamento, o l'eliminazione dei dati possono essere gestito direttamente in un gestore eventi anziché dover eseguire il wrapping del codice in `try...catch` blocchi.

## <a name="summary"></a>Riepilogo

Un'applicazione ben strutturata è predisposto livelli distinti, ognuno dei quali incapsula un particolare ruolo. Nella prima esercitazione di questo articolo è stato creato un livello di accesso ai dati utilizzando i dataset tipizzati; in questa esercitazione è compilato un livello di logica di Business come una serie di classi dell'applicazione `App_Code` cartella che chiamano il nostro DAL. Il livello Business LOGIC implementa la logica di livello di campo e a livello aziendale per l'applicazione. Oltre a creare un BLL separato, come è stato fatto in questa esercitazione, è inoltre possibile estendere i metodi degli oggetti di TableAdapter utilizzando classi parziali. Tuttavia, questa tecnica consente di eseguire l'override di metodi esistenti non separare il nostro DAL e nostri BLL rendere più chiara l'approccio in cui che sono state utilizzate in questo articolo.

Con DAL e BLL completo, si è pronti a iniziare il livello di presentazione. Nel [esercitazione successiva](master-pages-and-site-navigation-cs.md) verrà richiedere una deviazione breve gli argomenti di accesso ai dati e definire un layout di pagina coerente per l'utilizzo durante le esercitazioni.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Liz Shulok, Dennis Patterson, Carlos Santos e Hilton Giesenow. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](creating-a-data-access-layer-cs.md)
[Successivo](master-pages-and-site-navigation-cs.md)
