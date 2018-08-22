---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: Aggiunta di colonne DataTable aggiuntive (c#) | Microsoft Docs
author: rick-anderson
description: Quando si usa la configurazione guidata TableAdapter per creare un set di dati tipizzato, l'oggetto DataTable corrispondente contiene le colonne restituite dalla query sul database principale. Ma questa posizione...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 059538d3196aaa1fe3a70d9c02565e4e7af36881
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829556"
---
<a name="adding-additional-datatable-columns-c"></a>Aggiunta di colonne DataTable aggiuntive (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) o [Scarica il PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Quando si usa la configurazione guidata TableAdapter per creare un set di dati tipizzato, l'oggetto DataTable corrispondente contiene le colonne restituite dalla query sul database principale. Ma esistono alcuni casi quando l'oggetto DataTable deve includere colonne aggiuntive. In questa esercitazione viene illustrato il motivo per cui le stored procedure sono consigliate al momento del bisogno colonne DataTable aggiuntive.


## <a name="introduction"></a>Introduzione

Quando si aggiunge un oggetto TableAdapter a un DataSet tipizzato, lo schema s DataTable corrispondente è determinato dalla query principale s TableAdapter. Ad esempio, se la query principale restituisce i campi dati *un'*, *B*, e *C*, DataTable sarà presenti tre colonne corrispondente denominate *oggetto*, *B*, e *C*. Oltre alle relative query principale, un oggetto TableAdapter può includere altre query che restituiscono, ad esempio, un subset dei dati in base a un parametro. Ad esempio, oltre al `ProductsTableAdapter` query principale s, che restituisce informazioni su tutti i prodotti, contiene anche metodi quali `GetProductsByCategoryID(categoryID)` e `GetProductByProductID(productID)`, quale restituire le informazioni di prodotto specifico basate su un parametro fornito.

Il modello che lo schema di DataTable s riflettono la query principale s TableAdapter funziona bene se tutti i metodi TableAdapter s restituiscono lo stesso o campi di dati inferiore rispetto a quelle specificate nella query principali. Se un metodo TableAdapter deve restituire i campi dati aggiuntivi, è necessario espandere lo schema di s DataTable di conseguenza. Nel [Master/dettaglio mediante un elenco puntato di record Master e un controllo DataList di dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) esercitazione è stato aggiunto un metodo per il `CategoriesTableAdapter` che ha restituito il `CategoryID`, `CategoryName`, e `Description` campi dati definiti in la query principale più `NumberOfProducts`, un campo di dati aggiuntivi che ha segnalato il numero di prodotti associati a ogni categoria. Abbiamo aggiunto manualmente una nuova colonna per il `CategoriesDataTable` per acquisire il `NumberOfProducts` dal metodo di questo nuovo valore del campo dati.

Come descritto nel [caricamento di file](../working-with-binary-files/uploading-files-cs.md) tutorial, benissimo necessario prestare attenzione con la classe TableAdapter che usano istruzioni SQL ad hoc e dispongono di metodi i cui campi di dati non corrispondono esattamente la query principale. Se si esegue nuovamente la configurazione guidata TableAdapter, aggiornerà tutti i metodi di s TableAdapter in modo che proprio elenco di campi di dati corrisponde alla query principale. Di conseguenza, tutti i metodi con elenchi di colonne personalizzate verranno tornare all'elenco delle colonne query principale s e restituisce i dati previsti. Questo problema non si verifica quando si utilizzano le stored procedure.

In questa esercitazione si esaminerà come estendere lo schema s una DataTable per includere colonne aggiuntive. A causa di inconvenienti dell'oggetto TableAdapter quando si usano istruzioni SQL ad hoc, in questa esercitazione si userà le stored procedure. Fare riferimento al [creazione di nuove Stored procedure per DataSet tipizzata s TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) e [usando Stored procedure esistenti per DataSet tipizzata s TableAdapter](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) esercitazioni per altre informazioni su configurazione di un oggetto TableAdapter per utilizzare le stored procedure.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Passaggio 1: Aggiunta di un`PriceQuartile`colonna per il`ProductsDataTable`

Nel *creazione di nuove Stored procedure per DataSet tipizzata s TableAdapter* esercitazione è stato creato un set di dati tipizzato denominato `NorthwindWithSprocs`. Questo set di dati attualmente contiene due oggetti DataTable: `ProductsDataTable` e `EmployeesDataTable`. Il `ProductsTableAdapter` ha tre metodi seguenti:

- `GetProducts` -la query principale, che restituisce tutti i record di `Products` tabella
- `GetProductsByCategoryID(categoryID)` : restituisce tutti i prodotti con l'oggetto specificato *categoryID*.
- `GetProductByProductID(productID)` -Restituisce il prodotto specifico con l'oggetto specificato *productID*.

La query principale e altri due metodi restituiscono lo stesso set di campi di dati, vale a dire tutte le colonne dai `Products` tabella. Non sono presenti sottoquery correlate o `JOIN` s il pull di dati correlati tratti dal `Categories` o `Suppliers` tabelle. Pertanto, il `ProductsDataTable` include una colonna corrispondente per ogni campo nel `Products` tabella.

Per questa esercitazione, ti permettono di s aggiungere un metodo per la `ProductsTableAdapter` denominato `GetProductsWithPriceQuartile` che restituisce tutti i prodotti. Oltre ai campi di dati del prodotto standard, `GetProductsWithPriceQuartile` includerà anche un `PriceQuartile` campo data che indica in quale quartile cade il prezzo del prodotto s. Ad esempio, i prodotti il cui prezzo più dispendiose % 25 avranno un `PriceQuartile` pari a 1, mentre quelle il cui prezzo è compreso nell'ultimo 25% avrà un valore pari a 4. Prima ci preoccupiamo creazione della stored procedure per restituire tali informazioni, tuttavia, è innanzitutto necessario aggiornare il `ProductsDataTable` per includere una colonna per contenere le `PriceQuartile` risultati quando la `GetProductsWithPriceQuartile` viene usato il metodo.

Aprire il `NorthwindWithSprocs` set di dati e fare clic su di `ProductsDataTable`. Scegliere Aggiungi dal menu di scelta rapida e quindi selezionare colonne.


[![Aggiungere una nuova colonna per il ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Figura 1**: aggiungere una nuova colonna per il `ProductsDataTable` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-additional-datatable-columns-cs/_static/image3.png))


Verrà aggiunto una nuova colonna alla DataTable denominato Column1 typu `System.String`. È necessario aggiornare il nome della colonna s PriceQuartile e relativo tipo su `System.Int32` perché che verrà usato per contenere un numero compreso tra 1 e 4. Selezionare la colonna appena aggiunto nel `ProductsDataTable` e, dalla finestra delle proprietà, impostare il `Name` proprietà PriceQuartile e il `DataType` proprietà `System.Int32`.


[![Impostare le proprietà di tipo di dati e il nome della nuova colonna s](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Figura 2**: impostare la nuova colonna s `Name` e `DataType` delle proprietà ([fare clic per visualizzare l'immagine con dimensioni normali](adding-additional-datatable-columns-cs/_static/image6.png))


Come illustrato nella figura 2, sono disponibili proprietà aggiuntive che è possibile impostare, ad esempio se i valori nella colonna devono essere univoci, se la colonna è una colonna a incremento automatico, o meno database `NULL` sono consentiti valori e così via. Lasciare questi valori impostati sui valori predefiniti.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Passaggio 2: Creazione di`GetProductsWithPriceQuartile`(metodo)

Ora che il `ProductsDataTable` è stata aggiornata per includere il `PriceQuartile` colonna, siamo pronti creare il `GetProductsWithPriceQuartile` (metodo). Avviare facendo clic sull'oggetto TableAdapter e scegliendo Aggiungi Query dal menu di scelta rapida. Verrà visualizzata la configurazione guidata Query TableAdapter, che richiede prima di tutto per quanto riguarda se si desidera utilizzare istruzioni SQL ad hoc o una stored procedure nuova o esistente. Poiché abbiamo desidero ancora privo di una stored procedure che restituisce i dati di quartile prezzo, consentire s consentire TableAdapter creare questa stored procedure per noi. Selezionare l'opzione di creazione nuova stored procedure e fare clic su Avanti.


[![Indicare la procedura guidata TableAdapter per creare la Stored Procedure per noi](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Figura 3**: indicare la procedura guidata TableAdapter per creare la Stored Procedure per Stati Uniti ([fare clic per visualizzare l'immagine con dimensioni normali](adding-additional-datatable-columns-cs/_static/image9.png))


Nella schermata successiva, illustrata nella figura 4, la procedura guidata viene chiesto di specificare il tipo di query da aggiungere. Poiché il `GetProductsWithPriceQuartile` metodo restituirà tutte le colonne e i record dal `Products` di tabella, selezionare il SELECT che restituisce righe opzione e fare clic su Avanti.


[![La Query sarà un'istruzione SELECT che restituisce più righe](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Figura 4**: la nostra Query sarà un `SELECT` istruzione che restituisce più righe ([fare clic per visualizzare l'immagine con dimensioni normali](adding-additional-datatable-columns-cs/_static/image12.png))


Successivamente vengono richieste per il `SELECT` query. Nella procedura guidata, immettere la query seguente:


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

La query precedente Usa SQL Server 2005 s nuove [ `NTILE` funzione](https://msdn.microsoft.com/library/ms175126.aspx) per dividere i risultati in quattro gruppi in cui i gruppi sono determinati dal `UnitPrice` valori ordinati in ordine decrescente.

Sfortunatamente, il generatore delle Query non sa come analizzare il `OVER` (parola chiave) e verrà visualizzato un errore durante l'analisi della query precedente. Pertanto, immettere la query precedente direttamente nella casella di testo della procedura guidata senza usare il generatore delle Query.

> [!NOTE]
> Per altre informazioni su SQL Server 2005 e NTILE s altre funzioni di rango, vedere [restituzione di risultati con classificazione più con Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) e il [sezione funzioni di rango](https://msdn.microsoft.com/library/ms189798.aspx) dal [SQL Documentazione Online di Server 2005](https://msdn.microsoft.com/library/ms189798.aspx).


Dopo aver immesso il `SELECT` query e facendo clic su Avanti, la procedura guidata chiede di specificare un nome per la stored procedure viene creato. Denominare la nuova stored procedure `Products_SelectWithPriceQuartile` e fare clic su Avanti.


[![Nome Products_SelectWithPriceQuartile la Stored Procedure](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Figura 5**: nome della Stored Procedure `Products_SelectWithPriceQuartile` ([fare clic per visualizzare l'immagine con dimensioni normali](adding-additional-datatable-columns-cs/_static/image15.png))


Infine, vengono richieste per denominare metodi dell'oggetto TableAdapter. Lasciare entrambi il riempimento di un oggetto DataTable e restituire un oggetto DataTable caselle di controllo selezionata e il nome dei metodi `FillWithPriceQuartile` e `GetProductsWithPriceQuartile`.


[![I metodi di nomi ai TableAdapter e fare clic su Fine](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Figura 6**: nomi dei metodi s TableAdapter e fare clic su Fine ([fare clic per visualizzare l'immagine con dimensioni normali](adding-additional-datatable-columns-cs/_static/image18.png))


Con la `SELECT` query specificata e la stored procedure e i metodi TableAdapter denominati, fare clic su Fine per completare la procedura guidata. A questo punto è possibile che venga visualizzato un avviso o due dalla procedura guidata che informa che il `OVER` istruzione o costrutto SQL non è supportato. Questi avvisi possono essere ignorati.

Dopo aver completato la procedura guidata, l'oggetto TableAdapter deve includere il `FillWithPriceQuartile` e `GetProductsWithPriceQuartile` metodi e il database deve includere una stored procedure denominata `Products_SelectWithPriceQuartile`. Si consiglia di verificare che l'oggetto TableAdapter contenga effettivamente questo nuovo metodo e che la stored procedure è stato aggiunto correttamente al database. Durante il controllo del database, se non è possibile visualizzare la stored procedure try pulsante destro del mouse sulla cartella Stored procedure e scegliere Aggiorna.


![Verificare che sia stato aggiunto un nuovo metodo all'oggetto TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**Figura 7**: verificare che sia stato aggiunto un nuovo metodo all'oggetto TableAdapter


[![Verificare che il Database contenga il Products_SelectWithPriceQuartile Stored Procedure](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Figura 8**: verificare che il Database contiene il `Products_SelectWithPriceQuartile` Stored Procedure ([fare clic per visualizzare l'immagine con dimensioni normali](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> Uno dei vantaggi dell'utilizzo delle stored procedure anziché le istruzioni SQL ad hoc è che eseguire di nuovo la configurazione guidata TableAdapter non modifica gli elenchi di colonne di stored procedure. Effettuare questa verifica facendo clic sul TableAdapter, scegliendo l'opzione Configura il menu di scelta rapida per avviare la procedura guidata e quindi fare clic su Fine per completare l'operazione. Passare quindi al database e visualizzare il `Products_SelectWithPriceQuartile` stored procedure. Si noti che l'elenco di colonne non è stato modificato. Se avessimo si Usa istruzioni SQL ad hoc, eseguire di nuovo la configurazione guidata TableAdapter verrebbe ripristinato attivo questo elenco di colonne query s in modo da corrispondere l'elenco di colonne query principale, eliminando l'istruzione NTILE dalla query utilizzata dal `GetProductsWithPriceQuartile` (metodo).


Quando il livello di accesso ai dati di s `GetProductsWithPriceQuartile` metodo viene richiamato, viene eseguito il TableAdapter il `Products_SelectWithPriceQuartile` stored procedure e aggiunge una riga per il `ProductsDataTable` per ciascun record restituito. Vengono eseguito il mapping ai campi di dati restituiti dalla stored procedure per la `ProductsDataTable` colonne s. Poiché non esiste un `PriceQuartile` campo di dati restituito dalla stored procedure, il relativo valore viene assegnato per il `ProductsDataTable` s `PriceQuartile` colonna.

Per i metodi TableAdapter con query non restituiscono un `PriceQuartile` campo di dati, il `PriceQuartile` valore colonna s è il valore specificato dal relativo `DefaultValue` proprietà. Come illustrato nella figura 2, questo valore è impostato su `DBNull`, il valore predefinito. Se si preferisce un valore predefinito differente, è sufficiente impostare il `DefaultValue` proprietà conseguenza. Assicurarsi che il `DefaultValue` valore è valido assegnato colonna s `DataType` (ad esempio, `System.Int32` per il `PriceQuartile` colonna).

A questo punto sono stati eseguiti i passaggi necessari per l'aggiunta di una colonna aggiuntiva a un oggetto DataTable. Per verificare che questa colonna aggiuntiva funzioni come previsto, consentire s di creare una pagina ASP.NET che consente di visualizzare ogni s nome prodotto, prezzo e quartile prezzo. Prima di procedere, tuttavia, è innanzitutto necessario aggiornare il livello di logica di Business in modo da includere un metodo che chiama verso il basso per i dispositivi DAL `GetProductsWithPriceQuartile` (metodo). Si aggiorna il livello BLL successivamente, nel passaggio 3 e quindi creare la pagina ASP.NET nel passaggio 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Passaggio 3: Aumentare il livello di logica di Business

Prima che si usa il nuovo `GetProductsWithPriceQuartile` metodo dal livello di presentazione, si dovrebbe innanzitutto aggiungere un metodo corrispondente per il livello BLL. Aprire il `ProductsBLLWithSprocs` file di classe e aggiungere il codice seguente:


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

Come gli altri metodi di recupero dei dati in `ProductsBLLWithSprocs`, il `GetProductsWithPriceQuartile` metodo chiama semplicemente DAL corrispondente s `GetProductsWithPriceQuartile` (metodo) e restituisce i relativi risultati.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Passaggio 4: Visualizzando le informazioni di Quartile prezzo in una pagina Web ASP.NET

Con l'aggiunta di BLL completare sono pronti per creare una pagina ASP.NET che illustra il quartile prezzo per ogni prodotto. Aprire il `AddingColumns.aspx` nella pagina la `AdvancedDAL` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativo `ID` proprietà `Products`. GridView s nello smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per usare la `ProductsBLLWithSprocs` classe s `GetProductsWithPriceQuartile` (metodo). Poiché questa sarà una griglia di sola lettura, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno).


[![Configurare ObjectDataSource per usare la classe ProductsBLLWithSprocs](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Figura 9**: configurare ObjectDataSource per usare il `ProductsBLLWithSprocs` classe ([fare clic per visualizzare l'immagine con dimensioni normali](adding-additional-datatable-columns-cs/_static/image25.png))


[![Recuperare le informazioni sul prodotto dal metodo GetProductsWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Figura 10**: recuperare informazioni sui prodotti dal `GetProductsWithPriceQuartile` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](adding-additional-datatable-columns-cs/_static/image28.png))


Dopo aver completato la procedura guidata Configura origine dati, Visual Studio aggiungerà automaticamente un BoundField o CampoCasellaDiControllo a GridView per ognuno dei campi di dati restituiti dal metodo. Uno di questi campi di dati viene `PriceQuartile`, ovvero la colonna viene aggiunta al `ProductsDataTable` nel passaggio 1.

Modificare i campi s GridView, rimuovendo tutto tranne le `ProductName`, `UnitPrice`, e `PriceQuartile` BoundField. Configurare il `UnitPrice` BoundField per formattare il valore come valuta e il `UnitPrice` e `PriceQuartile` BoundField e center allineato a destra, rispettivamente. Aggiornare infine restanti BoundField `HeaderText` le proprietà del prodotto, prezzo e prezzo Quartile, rispettivamente. Inoltre, controllare la casella di controllo Abilita ordinamento dallo smart tag s GridView.

Dopo queste modifiche, il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

Figura 11 Mostra questa pagina quando visitati tramite un browser. Si noti che, inizialmente, i prodotti vengono ordinati in base i prezzi in ordine decrescente con ogni prodotto assegnato appropriata `PriceQuartile` valore. Ovviamente questi dati possono essere ordinati in base ad altri criteri con il valore della colonna prezzo Quartile ancora che riflette la classificazione prodotto s rispetto al prezzo (vedere la figura 12).


[![I prodotti vengono ordinati in base i prezzi](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Figura 11**: The prodotti vengono ordinati in base i relativi prezzi ([fare clic per visualizzare l'immagine con dimensioni normali](adding-additional-datatable-columns-cs/_static/image31.png))


[![I prodotti vengono ordinati in base i relativi nomi](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Figura 12**: The prodotti vengono ordinati in base i relativi nomi ([fare clic per visualizzare l'immagine con dimensioni normali](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> Con poche righe di codice potremmo forniamo GridView in modo che colorato le righe di prodotti in base i `PriceQuartile` valore. Si potrebbe essere il primo quartile una luce verde, quelli di quartile secondo un giallo chiaro, tali prodotti di colore e così via. Ti invitiamo a si consiglia di aggiungere questa funzionalità. Se è necessario un aggiornamento sulla formattazione di un controllo GridView, consultare il [formattazione basata su dati personalizzati](../custom-formatting/custom-formatting-based-upon-data-cs.md) esercitazione.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Un approccio alternativo: creazione di un altro oggetto TableAdapter

Come abbiamo visto in questa esercitazione, durante l'aggiunta di un metodo a un oggetto TableAdapter che restituisce i campi dati diverso da quelli venga specificata dalla query principale, è possibile aggiungere colonne corrispondenti al DataTable. Questo approccio, tuttavia, funziona bene solo se sono presenti un numero limitato di metodi in TableAdapter che restituiscono campi di dati diversi e se tali campi di dati alternativi non variabile troppo dalla query principale.

Invece di aggiungere colonne alla tabella di dati, è invece possibile aggiungere un altro oggetto TableAdapter al set di dati che contiene i metodi che restituiscono campi di dati diversi dal primo oggetto TableAdapter. Per questa esercitazione, anziché aggiungere il `PriceQuartile` colonna per il `ProductsDataTable` (in cui viene usato solo per il `GetProductsWithPriceQuartile` (metodo)), avremmo potuto aggiungere un TableAdapter aggiuntive per il set di dati denominato `ProductsWithPriceQuartileTableAdapter` che utilizzato il `Products_SelectWithPriceQuartile` archiviati procedure relative query principale. Usano le pagine ASP.NET che devono ottenere informazioni sui prodotti con quartile il prezzo di `ProductsWithPriceQuartileTableAdapter`, mentre quelle che non è stato possibile continuare a usare il `ProductsTableAdapter`.

Aggiungendo un nuovo TableAdapter, oggetti DataTable rimangono untarnished e le relative colonne rispecchiano esattamente ai campi di dati restituiti dai relativi metodi s TableAdapter. Tuttavia, altri oggetti TableAdapter possono introdurre funzionalità e le attività ripetitive. Ad esempio, se tali delle pagine ASP.NET che visualizzata la `PriceQuartile` colonna anche necessari per fornire l'inserimento, aggiornamento ed eliminazione supporto, il `ProductsWithPriceQuartileTableAdapter` deve avere relativo `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà correttamente configurato. Anche se queste proprietà rispecchierebbe il `ProductsTableAdapter` s, questa configurazione viene introdotto un passaggio aggiuntivo. Inoltre, sono ora presenti due modi per aggiornare, eliminare o aggiungere un prodotto del database - tramite il `ProductsTableAdapter` e `ProductsWithPriceQuartileTableAdapter` classi.

Il download per questa esercitazione include un' `ProductsWithPriceQuartileTableAdapter` classe la `NorthwindWithSprocs` set di dati che illustra questo approccio alternativo.

## <a name="summary"></a>Riepilogo

Nella maggior parte degli scenari, tutti i metodi in un oggetto TableAdapter verrà restituito lo stesso set di campi di dati, ma vi sono casi quando un metodo particolare o due potrebbe essere necessario restituire un campo aggiuntivo. Ad esempio, nelle [Master/dettaglio mediante un elenco puntato di record Master e un controllo DataList di dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) esercitazione è stato aggiunto un metodo per il `CategoriesTableAdapter` che, oltre ai campi dati, query principale s ha restituiti un `NumberOfProducts` campo che segnalato il numero di prodotti associati a ogni categoria. In questa esercitazione è stato aggiunta di un metodo nel `ProductsTableAdapter` che ha restituito un `PriceQuartile` campo oltre ai campi di dati della query principale s. Per acquisire dati aggiuntivi campi restituiti dai metodi TableAdapter s che è necessario aggiungere le colonne corrispondenti al DataTable.

Se si prevede di aggiungere manualmente le colonne alla tabella di dati, è consigliabile che l'oggetto TableAdapter utilizzare le stored procedure. Se l'oggetto TableAdapter Usa istruzioni SQL ad hoc, la configurazione guidata TableAdapter è eseguire tutti i metodi negli elenchi dei campi dati ripristino i campi di dati restituiti dalla query principale. Questo problema non si estende alle stored procedure, motivo per cui essi sono consigliati e sono stati usati in questa esercitazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Randy Schmidt, Goor Jacky, Bernadette Leigh e Hilton Giesenow. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](updating-the-tableadapter-to-use-joins-cs.md)
> [Successivo](working-with-computed-columns-cs.md)
