---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: Aggiunta di colonne aggiuntive DataTable (c#) | Documenti Microsoft
author: rick-anderson
description: Quando si utilizza la configurazione guidata TableAdapter per creare un DataSet tipizzato, DataTable corrispondente contiene le colonne restituite dalla query sul database principale. Ma non esiste...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 0b1fe8d2e376065aed8d94b1267910bd1f7e5bd0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-additional-datatable-columns-c"></a>Aggiunta di colonne aggiuntive DataTable (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) o [Scarica il PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Quando si utilizza la configurazione guidata TableAdapter per creare un DataSet tipizzato, DataTable corrispondente contiene le colonne restituite dalla query sul database principale. Ma esistono casi quando DataTable deve includere colonne aggiuntive. In questa esercitazione viene illustrato perché le stored procedure sono consigliate quando abbiamo bisogno di altre colonne DataTable.


## <a name="introduction"></a>Introduzione

Quando si aggiunge un oggetto TableAdapter a un DataSet tipizzato, lo schema s DataTable corrispondente è determinato dalla query principale s TableAdapter. Ad esempio, se la query principale restituisce i campi dati *A*, *B*, e *C*, DataTable disporrà di tre colonne corrispondente denominate *A*, *B*, e *C*. Oltre alle query principale, un oggetto TableAdapter può includere ulteriori query che restituiscono, ad esempio, un subset dei dati in base a un parametro. Ad esempio, oltre al `ProductsTableAdapter` query principale s, che restituisce informazioni su tutti i prodotti, contiene anche metodi, ad esempio `GetProductsByCategoryID(categoryID)` e `GetProductByProductID(productID)`, che restituisce informazioni di prodotto specifico in base a un parametro specificato.

Il modello che lo schema di DataTable s corrisponde alla query principale di TableAdapter s funziona anche se tutti i metodi TableAdapter s restituiscono lo stesso o campi di dati inferiore rispetto a quelle specificate nella query principale. Se un metodo di TableAdapter deve restituire i campi dati aggiuntivi, è necessario espandere lo schema di DataTable s conseguenza. Nel [Master/dettaglio mediante un elenco puntato dei record Master con un controllo DataList dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) esercitazione è stato aggiunto un metodo per il `CategoriesTableAdapter` che ha restituito il `CategoryID`, `CategoryName`, e `Description` i campi dati definiti la query principale più `NumberOfProducts`, un campo di dati aggiuntivi che ha segnalato il numero di prodotti associati a ogni categoria. Abbiamo aggiunto manualmente una nuova colonna per il `CategoriesDataTable` per acquisire il `NumberOfProducts` campo dati valore di questo nuovo metodo.

Come descritto nel [caricamento di file](../working-with-binary-files/uploading-files-cs.md) tutorial, grande attenzione degli oggetti TableAdapter Usa istruzioni SQL ad hoc che dispongono di metodi i cui campi dati non corrispondono esattamente la query principale. Se la configurazione guidata TableAdapter viene rieseguita, verranno aggiornati tutti i metodi TableAdapter s in modo che i relativi elenco di campi di dati corrisponde alla query principale. Di conseguenza, tutti i metodi con elenchi di colonne personalizzato verranno tornare all'elenco delle colonne della query principale s e non restituisce i dati previsti. Questo problema non si verifica quando si utilizzano stored procedure.

In questa esercitazione verranno esaminati come estendere lo schema di s una DataTable per includere colonne aggiuntive. A causa della vulnerabilità del TableAdapter quando si Usa istruzioni SQL ad hoc, in questa esercitazione si utilizzerà le stored procedure. Fare riferimento al [la creazione di nuove Stored procedure per gli oggetti TableAdapter s DataSet tipizzato](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) e [utilizzando Stored procedure esistenti per gli oggetti TableAdapter s DataSet tipizzato](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) esercitazioni per ulteriori informazioni su configurazione di un oggetto TableAdapter per l'utilizzo di stored procedure.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Passaggio 1: Aggiunta di un`PriceQuartile`colonna per il`ProductsDataTable`

Nel *la creazione di nuove Stored procedure per gli oggetti TableAdapter s DataSet tipizzato* esercitazione è stato creato un set di dati tipizzato denominato `NorthwindWithSprocs`. Attualmente, questo set di dati contiene due DataTable: `ProductsDataTable` e `EmployeesDataTable`. Il `ProductsTableAdapter` ha tre metodi seguenti:

- `GetProducts`-la query principale, che restituisce tutti i record di `Products` tabella
- `GetProductsByCategoryID(categoryID)`-Restituisce tutti i prodotti con l'oggetto specificato *categoryID*.
- `GetProductByProductID(productID)`-Restituisce il prodotto specifico con l'oggetto specificato *productID*.

La query principale e altri due metodi restituiscono lo stesso set di campi di dati, vale a dire tutte le colonne dal `Products` tabella. Sono non disponibili le sottoquery correlate o `JOIN` s recupera i dati correlati di `Categories` o `Suppliers` tabelle. Pertanto, il `ProductsDataTable` include una colonna corrispondente per ciascun campo il `Products` tabella.

Per questa esercitazione, s consentono di aggiungere un metodo per il `ProductsTableAdapter` denominato `GetProductsWithPriceQuartile` che restituisce tutti i prodotti. Oltre ai campi dati del prodotto standard, `GetProductsWithPriceQuartile` includerà anche un `PriceQuartile` campo dei dati che indica in quale quartile rientra il prezzo del prodotto s. Ad esempio, include i prodotti il cui prezzo presenti il maggior numero di risorse % 25 un `PriceQuartile` valore pari a 1, mentre quelli il cui prezzo è compreso nella parte inferiore 25% avrà un valore pari a 4. Prima di preoccupazione creazione della stored procedure per restituire queste informazioni, tuttavia, è necessario aggiornare il `ProductsDataTable` per includere una colonna per contenere il `PriceQuartile` risultati quando la `GetProductsWithPriceQuartile` viene usato il metodo.

Aprire il `NorthwindWithSprocs` set di dati e fare clic su di `ProductsDataTable`. Scegliere Aggiungi dal menu di scelta rapida e scegliere la colonna.


[![Aggiungere una nuova colonna per il ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Figura 1**: aggiungere una nuova colonna per il `ProductsDataTable` ([fare clic per visualizzare l'immagine ingrandita](adding-additional-datatable-columns-cs/_static/image3.png))


Verrà aggiunta una nuova colonna alla tabella di dati denominato Column1 di tipo `System.String`. È necessario aggiornare il nome della colonna s PriceQuartile e il relativo tipo di `System.Int32` dal momento che verrà utilizzato per contenere un numero compreso tra 1 e 4. Selezionare la colonna appena aggiunta il `ProductsDataTable` e, dalla finestra delle proprietà, impostare il `Name` proprietà PriceQuartile e `DataType` proprietà `System.Int32`.


[![Impostare le proprietà di tipo di dati e il nome della nuova colonna s](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Figura 2**: impostare la nuova colonna di s `Name` e `DataType` proprietà ([fare clic per visualizzare l'immagine ingrandita](adding-additional-datatable-columns-cs/_static/image6.png))


Come illustrato nella figura 2, sono disponibili proprietà aggiuntive che è possibile impostare, ad esempio se i valori nella colonna devono essere univoci, se la colonna è una colonna a incremento automatico, o meno database `NULL` sono consentiti valori e così via. Lasciare i valori impostati sui valori predefiniti.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Passaggio 2: Creazione di`GetProductsWithPriceQuartile`(metodo)

Ora che il `ProductsDataTable` è stato aggiornato per includere il `PriceQuartile` colonna, si è pronti a creare il `GetProductsWithPriceQuartile` metodo. Avviare facendo clic sul TableAdapter e scegliendo Aggiungi Query dal menu di scelta rapida. Verrà visualizzata la configurazione guidata Query TableAdapter, che richiede ci come se si desidera utilizzare istruzioni SQL ad hoc o una stored procedure nuova o esistente. Poiché è non è stata ridimensionata ancora di una stored procedure che restituisce i dati di quartile prezzo, permettere s consentire TableAdapter creare questa stored procedure per noi. Selezionare l'opzione di creare nuove stored procedure e fare clic su Avanti.


[![Indicare la configurazione guidata TableAdapter per creare la Stored Procedure per ci](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Figura 3**: indicare la configurazione guidata TableAdapter per creare la Stored Procedure per Stati Uniti ([fare clic per visualizzare l'immagine ingrandita](adding-additional-datatable-columns-cs/_static/image9.png))


Nella schermata successiva, illustrata nella figura 4, la procedura guidata viene chiesto di specificare il tipo di query da aggiungere. Poiché il `GetProductsWithPriceQuartile` metodo restituirà tutte le colonne e i record di `Products` tabella, selezionare l'istruzione SELECT che restituisce righe opzione e fare clic su Avanti.


[![La Query sarà un'istruzione SELECT che restituisce più righe](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Figura 4**: la nostra Query sarà un `SELECT` istruzione che restituisce più righe ([fare clic per visualizzare l'immagine ingrandita](adding-additional-datatable-columns-cs/_static/image12.png))


Successivamente vengono richieste per il `SELECT` query. Nella procedura guidata, immettere la query seguente:


[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

La query precedente utilizza SQL Server 2005 s nuovo [ `NTILE` funzione](https://msdn.microsoft.com/en-us/library/ms175126.aspx) per dividere i risultati in quattro gruppi in cui i gruppi sono determinati dal `UnitPrice` valori ordinati in ordine decrescente.

Sfortunatamente, il generatore di Query non è in grado di analizzare il `OVER` (parola chiave) e verrà visualizzato un errore durante l'analisi di query sopra indicata. Pertanto, è possibile immettere la query sopra indicata direttamente nella casella di testo della procedura guidata senza utilizzare il generatore delle Query.

> [!NOTE]
> Per ulteriori informazioni su NTILE e SQL Server 2005 s altre funzioni di rango, vedere [restituzione di risultati classificati con Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) e [sezione funzioni di rango](https://msdn.microsoft.com/en-us/library/ms189798.aspx) dal [SQL Documentazione Online di Server 2005](https://msdn.microsoft.com/en-us/library/ms189798.aspx).


Dopo aver immesso il `SELECT` query e fare clic su Avanti, la procedura guidata chiede di specificare un nome per la stored procedure verrà creato. Denominare la nuova stored procedure `Products_SelectWithPriceQuartile` e fare clic su Avanti.


[![Nome Products_SelectWithPriceQuartile la Stored Procedure](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Figura 5**: nome della Stored Procedure `Products_SelectWithPriceQuartile` ([fare clic per visualizzare l'immagine ingrandita](adding-additional-datatable-columns-cs/_static/image15.png))


Infine, vengono richieste per denominare metodi dell'oggetto TableAdapter. Lasciare entrambi il riempimento di un oggetto DataTable e restituire un DataTable caselle di controllo selezionata e il nome dei metodi `FillWithPriceQuartile` e `GetProductsWithPriceQuartile`.


[![Metodi di nomi ai TableAdapter e fare clic su Fine.](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Figura 6**: nomi dei metodi s TableAdapter e fare clic su Fine ([fare clic per visualizzare l'immagine ingrandita](adding-additional-datatable-columns-cs/_static/image18.png))


Con il `SELECT` query specificata e la stored procedure e i metodi TableAdapter denominati, fare clic su Fine per completare la procedura guidata. A questo punto è possibile che venga visualizzato un avviso o due dalla procedura guidata che informa che il `OVER` costrutto SQL o istruzione non è supportata. È possono ignorare questi avvisi.

Dopo aver completato la procedura guidata, è necessario includere il TableAdapter di `FillWithPriceQuartile` e `GetProductsWithPriceQuartile` metodi e il database deve includere una stored procedure denominata `Products_SelectWithPriceQuartile`. È opportuno verificare che l'oggetto TableAdapter effettivamente contenga questo nuovo metodo e che la stored procedure sia stato aggiunto correttamente al database. Quando il controllo del database, se non viene visualizzata la stored procedure try facendo clic sulla cartella Stored procedure e scegliere Aggiorna.


![Verificare che sia stato aggiunto un nuovo metodo all'oggetto TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**Figura 7**: verificare che sia stato aggiunto un nuovo metodo all'oggetto TableAdapter


[![Verificare che il Database contenga il Products_SelectWithPriceQuartile Stored Procedure](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Figura 8**: verificare che il Database contiene il `Products_SelectWithPriceQuartile` Stored Procedure ([fare clic per visualizzare l'immagine ingrandita](adding-additional-datatable-columns-cs/_static/image22.png))


> [!NOTE]
> Uno dei vantaggi dell'utilizzo di stored procedure anziché istruzioni SQL ad hoc è che a rieseguire la configurazione guidata TableAdapter non modificare gli elenchi di colonne della stored procedure. Verificare questa condizione facendo clic sul TableAdapter, scegliendo l'opzione di configurazione dal menu contestuale per avviare la procedura guidata e quindi fare clic su Fine per completare l'operazione. Successivamente, accedere al database e visualizzare il `Products_SelectWithPriceQuartile` stored procedure. Si noti che l'elenco di colonne non è stato modificato. Avessimo è stata utilizza istruzioni SQL ad hoc, eseguire nuovamente la configurazione guidata TableAdapter verrà ripristinata questo elenco di colonne per corrispondere alle colonne elencate query principale, eliminando così l'istruzione NTILE dalla query utilizzata da query s il `GetProductsWithPriceQuartile` metodo.


Quando il livello di accesso ai dati di s `GetProductsWithPriceQuartile` metodo viene richiamato, viene eseguito il TableAdapter il `Products_SelectWithPriceQuartile` stored procedure e aggiunge una riga per il `ProductsDataTable` per ciascun record restituito. Mapping dei campi di dati restituiti dalla stored procedure per la `ProductsDataTable` colonne s. Poiché non esiste un `PriceQuartile` campo dei dati restituiti dalla stored procedure, viene assegnato il valore di `ProductsDataTable` s `PriceQuartile` colonna.

Per i metodi TableAdapter le cui query non restituiscono un `PriceQuartile` campo di dati, il `PriceQuartile` valore della colonna s è il valore specificato dal relativo `DefaultValue` proprietà. Come illustrato nella figura 2, questo valore è impostato su `DBNull`, il valore predefinito. Se si preferisce un valore predefinito diverso, impostare semplicemente il `DefaultValue` proprietà conseguenza. Assicurarsi che il `DefaultValue` valore è valido dato colonna s `DataType` (ad esempio, `System.Int32` per il `PriceQuartile` colonna).

A questo punto sono stati eseguiti i passaggi necessari per l'aggiunta di una colonna aggiuntiva a un oggetto DataTable. Per verificare che questa colonna aggiuntiva funzioni come previsto, consentire s di creare una pagina ASP.NET che consente di visualizzare ogni nome s del prodotto, prezzo e quartile prezzo. Prima di procedere è tuttavia, è innanzitutto necessario aggiornare il livello di logica di Business per includere un metodo che chiama verso il basso per i dispositivi DAL `GetProductsWithPriceQuartile` metodo. Si aggiorna il livello Business LOGIC successivamente, nel passaggio 3 e quindi creare la pagina ASP.NET nel passaggio 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Passaggio 3: Aumentare il livello di logica di Business

Prima viene usato il nuovo `GetProductsWithPriceQuartile` metodo dal livello di presentazione, è innanzitutto necessario aggiungere un metodo corrispondente al livello Business Logic. Aprire il `ProductsBLLWithSprocs` file di classe e aggiungere il codice seguente:


[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

Come gli altri metodi di recupero di dati in `ProductsBLLWithSprocs`, `GetProductsWithPriceQuartile` metodo chiama semplicemente DAL corrispondente s `GetProductsWithPriceQuartile` (metodo) e restituisce i risultati.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Passaggio 4: Visualizzare le informazioni di Quartile prezzo in una pagina Web ASP.NET

Con l'aggiunta di BLL completare è nuovamente pronto per creare una pagina ASP.NET che mostra il quartile prezzo per ogni prodotto. Aprire il `AddingColumns.aspx` nella pagina di `AdvancedDAL` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione relativa `ID` proprietà `Products`. Il GridView s smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per utilizzare il `ProductsBLLWithSprocs` classe s `GetProductsWithPriceQuartile` metodo. Poiché questa sarà una griglia di sola lettura, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno).


[![Configurare ObjectDataSource per utilizzare la classe ProductsBLLWithSprocs](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Figura 9**: configurare ObjectDataSource per utilizzare il `ProductsBLLWithSprocs` classe ([fare clic per visualizzare l'immagine ingrandita](adding-additional-datatable-columns-cs/_static/image25.png))


[![Recuperare informazioni sul prodotto dal metodo GetProductsWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Figura 10**: recuperare informazioni sui prodotti dal `GetProductsWithPriceQuartile` metodo ([fare clic per visualizzare l'immagine ingrandita](adding-additional-datatable-columns-cs/_static/image28.png))


Dopo aver completato la configurazione guidata origine dati, Visual Studio aggiungerà automaticamente un BoundField o CheckBoxField a GridView per ognuno dei campi di dati restituiti dal metodo. Uno di questi campi di dati è `PriceQuartile`, ovvero la colonna è stato aggiunto al `ProductsDataTable` nel passaggio 1.

Modificare i campi s GridView, la rimozione di tutto tranne il `ProductName`, `UnitPrice`, e `PriceQuartile` BoundField. Configurare il `UnitPrice` BoundField per formattare il valore come valuta e avere la `UnitPrice` e `PriceQuartile` BoundField e center allineato a destra, rispettivamente. Infine, aggiornare restanti BoundField `HeaderText` le proprietà per prodotto, prezzo e prezzo Quartile, rispettivamente. Inoltre, controllare la casella di controllo Abilita ordinamento dallo smart tag GridView s.

Dopo queste modifiche, il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

Figura 11 Mostra questa pagina quando visitato tramite un browser. Si noti che, inizialmente, i prodotti vengono ordinati in base i prezzi in ordine decrescente con ogni prodotto assegnato un appropriato `PriceQuartile` valore. Naturalmente questi dati possono essere ordinati per altri criteri con il valore della colonna prezzo Quartile ancora che riflette la valutazione del prodotto s rispetto al prezzo (vedere Figura 12).


[![I prodotti vengono ordinati i prezzi](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Figura 11**: il prodotti vengono ordinati in base i prezzi ([fare clic per visualizzare l'immagine ingrandita](adding-additional-datatable-columns-cs/_static/image31.png))


[![I prodotti vengono ordinati in base ai nomi](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Figura 12**: il prodotti vengono ordinati in base ai nomi ([fare clic per visualizzare l'immagine ingrandita](adding-additional-datatable-columns-cs/_static/image34.png))


> [!NOTE]
> Con poche righe di codice è stato possibile aumentare in modo che colore le righe di prodotto in base GridView loro `PriceQuartile` valore. Si potrebbe essere il primo quartile una luce verde, quelli il secondo quartile un giallo chiaro, tali prodotti di colore e così via. Ti suggeriamo di dedicare alcuni minuti per aggiungere questa funzionalità. Se è necessario un aggiornamento sulla formattazione di un controllo GridView, consultare il [formattazione basato su dati personalizzati](../custom-formatting/custom-formatting-based-upon-data-cs.md) esercitazione.


## <a name="an-alternative-approach---creating-another-tableadapter"></a>Un approccio alternativo - creazione di un altro oggetto TableAdapter

Come abbiamo visto in questa esercitazione, aggiunta a un oggetto TableAdapter che restituisce i campi di dati diversi da quelli di un metodo dichiarato dalla query principale, è possibile aggiungere colonne corrispondenti nella DataTable. Tale approccio, tuttavia, funziona bene solo se sono presenti un numero ridotto di metodi TableAdapter che restituiscono campi di dati diversi e i campi di dati alternativi non variano a seconda troppo dalla query principale.

Invece di aggiungere colonne alla tabella di dati, è possibile aggiungere invece un altro oggetto TableAdapter al set di dati che contiene i metodi che restituiscono campi di dati diversi dal primo TableAdapter. Per questa esercitazione, anziché aggiungere il `PriceQuartile` colonna il `ProductsDataTable` (in cui viene utilizzato solo per il `GetProductsWithPriceQuartile` (metodo)), un TableAdapter aggiuntive avremmo potuto aggiungere al set di dati denominato `ProductsWithPriceQuartileTableAdapter` utilizzati il `Products_SelectWithPriceQuartile` archiviati procedure relative query principale. Utilizzano le pagine ASP.NET che è necessario per ottenere informazioni sui prodotti con quartile il prezzo di `ProductsWithPriceQuartileTableAdapter`, mentre quelli che non è possibile continuare a utilizzare il `ProductsTableAdapter`.

Aggiungendo un nuovo TableAdapter, DataTable rimangono untarnished e le relative colonne mirror con precisione i campi di dati restituiti da metodi TableAdapter s. Tuttavia, altri oggetti TableAdapter possono introdurre funzionalità e le attività ripetitive. Ad esempio, se tali pagine ASP.NET che visualizzata il `PriceQuartile` colonna anche necessari per fornire l'inserimento, aggiornamento ed eliminazione, il supporto di `ProductsWithPriceQuartileTableAdapter` dovranno disporre relativo `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà correttamente configurato. Anche se queste proprietà sarebbero eseguire il mirroring di `ProductsTableAdapter` s, questa configurazione introduce un passaggio aggiuntivo. Inoltre, sono disponibili ora due modi per aggiornare, eliminare o aggiungere un prodotto al database - tramite il `ProductsTableAdapter` e `ProductsWithPriceQuartileTableAdapter` classi.

Il download per questa esercitazione include un `ProductsWithPriceQuartileTableAdapter` classe il `NorthwindWithSprocs` set di dati che illustra questo approccio alternativo.

## <a name="summary"></a>Riepilogo

Nella maggior parte degli scenari, tutti i metodi in un oggetto TableAdapter verrà restituito lo stesso set di campi di dati, ma sono disponibili quando per restituire un campo aggiuntivo potrebbe essere necessario un metodo particolare o due volte. Ad esempio, nel [Master/dettaglio mediante un elenco puntato dei record Master con un controllo DataList dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) esercitazione è stato aggiunto un metodo per il `CategoriesTableAdapter` che, oltre ai campi di dati, query principale s restituiti un `NumberOfProducts` campo segnalato il numero di prodotti associati a ogni categoria. In questa esercitazione è stato esaminato l'aggiunta di un metodo di `ProductsTableAdapter` che ha restituito un `PriceQuartile` campo oltre ai campi di dati della query principale s. Per acquisire dati aggiuntivi campi restituiti dai metodi TableAdapter s che è necessario aggiungere le colonne corrispondenti nella DataTable.

Se si prevede di aggiungere manualmente le colonne nella DataTable, è consigliabile che l'oggetto TableAdapter utilizzare stored procedure. Se l'oggetto TableAdapter utilizza istruzioni SQL ad hoc, ogni volta che la configurazione guidata TableAdapter viene eseguita tutti i metodi negli elenchi dei campi dati ripristino i campi di dati restituiti dalla query principale. Questo problema non si estende a stored procedure, motivo per cui essi sono consigliati e utilizzati in questa esercitazione.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Randy Schmidt, Goor Luisa, Bernadette Leigh e Hilton Giesenow. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](updating-the-tableadapter-to-use-joins-cs.md)
[Successivo](working-with-computed-columns-cs.md)
