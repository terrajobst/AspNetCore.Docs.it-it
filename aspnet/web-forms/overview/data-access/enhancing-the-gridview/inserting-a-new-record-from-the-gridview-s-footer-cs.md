---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
title: "Inserimento di un nuovo Record dal piè di pagina del controllo GridView (c#) | Documenti Microsoft"
author: rick-anderson
description: Mentre il controllo GridView non fornisce supporto incorporato per l'inserimento di un nuovo record di dati, in questa esercitazione viene illustrato come aumentare GridView per includere un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 49545652-98af-46ba-9dbc-9ab529805d9b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: b0208b4df0194abaf37f7f9ac66c9ce24c35d721
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="inserting-a-new-record-from-the-gridviews-footer-c"></a>Inserimento di un nuovo Record dal piè di pagina del controllo GridView (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_CS.exe) o [Scarica il PDF](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/datatutorial53cs1.pdf)

> Mentre il controllo GridView non fornisce supporto incorporato per l'inserimento di un nuovo record di dati, in questa esercitazione viene illustrato come aumentare GridView per includere un'interfaccia di inserimento.


## <a name="introduction"></a>Introduzione

Come descritto nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) dell'esercitazione, i controlli GridView, DetailsView e FormView Web includono funzionalità di modifica dei dati incorporati. Se utilizzato con i controlli origine dati dichiarativa, questi tre controlli Web rapidamente e facilmente configurabile per modificare i dati e in scenari senza la necessità di scrivere una singola riga di codice. Purtroppo, solo i controlli di DetailsView e FormView forniscono predefiniti di inserimento, modifica ed eliminazione di funzionalità. GridView offre solo la modifica ed eliminazione di supporto. Con una piccola gomito grasso, tuttavia, possiamo forniamo GridView per includere un'interfaccia di inserimento.

Aggiunta di funzionalità di inserimento a GridView, ci sono responsabili si decide di nuovi record verranno aggiunti, la creazione dell'interfaccia di inserimento e la scrittura del codice per inserire il nuovo record. In questa esercitazione che verranno esaminati aggiunta dell'interfaccia di inserimento per il piè di pagina di GridView s riga (vedere la figura 1). La cella del piè di pagina per ogni colonna include i dati appropriati raccolta elemento dell'interfaccia utente (una casella di testo per il nome del prodotto s, un DropDownList per il fornitore e così via). È anche necessario una colonna per un componente pulsante che, quando si fa clic, causerà un postback e inserire un nuovo record nel `Products` tabella utilizzando i valori specificati nella riga di piè di pagina.


[![La riga di piè di pagina fornisce un'interfaccia per l'aggiunta di nuovi prodotti](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.png)

**Figura 1**: la riga di piè di pagina fornisce un'interfaccia per l'aggiunta di nuovi prodotti ([fare clic per visualizzare l'immagine ingrandita](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Passaggio 1: Visualizzazione di informazioni sui prodotti in un controllo GridView.

È riguardano effettuata con la creazione dell'interfaccia di inserimento nel piè di pagina s GridView, prima di consentire lo stato attivo prima di s sull'aggiunta di un controllo GridView alla pagina che elenca i prodotti nel database. Aprire il `InsertThroughFooter.aspx` nella pagina di `EnhancedGridView` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione di s GridView `ID` proprietà `Products`. Successivamente, utilizzare lo smart tag s di GridView per associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`.


[![Creare un nuovo oggetto ObjectDataSource denominato ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.png)

**Figura 2**: creare un nuovo ObjectDataSource denominato `ProductsDataSource` ([fare clic per visualizzare l'immagine ingrandita](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.png))


Configurare ObjectDataSource per utilizzare il `ProductsBLL` classe s `GetProducts()` metodo per recuperare informazioni sul prodotto. Per questa esercitazione, lasciare che lo stato attivo s esclusivamente sull'aggiunta di funzionalità di inserimento e non preoccuparsi di modifica ed eliminazione. Pertanto, assicurarsi che l'elenco di riepilogo a discesa nella scheda Inserisci è impostato su `AddProduct()` e che gli elenchi a discesa nelle schede UPDATE e DELETE sono impostati su (nessuno).


[![Eseguire il mapping del metodo AddProduct al metodo ObjectDataSource s Insert)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.png)

**Figura 3**: mappa di `AddProduct` metodo s ObjectDataSource `Insert()` metodo ([fare clic per visualizzare l'immagine ingrandita](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.png))


[![Impostare gli elenchi di riepilogo a discesa UPDATE e DELETE schede su (nessuno)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.png)

**Figura 4**: impostare l'aggiornamento ed eliminazione Elenca elenco a discesa di schede (nessuno) ([fare clic per visualizzare l'immagine ingrandita](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.png))


Dopo aver completato la procedura guidata Configura origine dati di ObjectDataSource s, Visual Studio aggiungerà automaticamente i campi a GridView per ognuno dei campi dati corrispondenti. Per il momento, lasciare tutti i campi aggiunti da Visual Studio. Più avanti in questa esercitazione verrà tornare e rimuovere alcuni dei campi i cui valori non è stata ridimensionata deve essere specificato quando si aggiunge un nuovo record.

Poiché sono presenti vicino a 80 prodotti nel database, un utente deve scorrere completamente fino alla fine della pagina web per aggiungere un nuovo record. Pertanto, consentire s Abilita il paging rendere l'interfaccia di inserimento più visibile e accessibile. Per attivare il paging, è sufficiente selezionare la casella di controllo Abilita Paging lo smart tag s di GridView.

A questo punto, il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample1.aspx)]


[![Tutti i campi dati di prodotto vengono visualizzati in un controllo GridView di paging](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.png)

**Figura 5**: tutti i campi dati di prodotto vengono visualizzati in un controllo GridView di paging ([fare clic per visualizzare l'immagine ingrandita](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.png))


## <a name="step-2-adding-a-footer-row"></a>Passaggio 2: Aggiunta di una riga di piè di pagina

Insieme all'intestazione e le righe di dati, il controllo GridView include una riga di piè di pagina. Le righe di intestazione e piè di pagina vengono visualizzate in base ai valori di GridView s [ `ShowHeader` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showheader.aspx) e [ `ShowFooter` ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.gridview.showfooter.aspx) proprietà. Per visualizzare la riga di piè di pagina, impostare semplicemente il `ShowFooter` proprietà `true`. Come illustrato nella figura 6, l'impostazione di `ShowFooter` proprietà `true` aggiunta alla griglia una riga di piè di pagina.


[![Per visualizzare la riga di piè di pagina, impostare ShowFooter su True](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.png)

**Figura 6**: per visualizzare la riga di piè di pagina, impostare `ShowFooter` a `True` ([fare clic per visualizzare l'immagine ingrandita](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.png))


Si noti che la riga di piè di pagina ha un colore di sfondo di colore rosso scuro. Ciò è dovuto al tema DataWebControls è creato e applicato a tutte le pagine nel [la visualizzazione di dati con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) esercitazione. In particolare, il `GridView.skin` file configura il `FooterStyle` proprietà tali che utilizza il `FooterStyle` classe CSS. Il `FooterStyle` classe è definita in `Styles.css` come indicato di seguito:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample2.css)]

> [!NOTE]
> Abbiamo ve esaminate con la riga di piè di pagina s GridView nelle esercitazioni precedenti. Se necessario, fare riferimento al [la visualizzazione di riepilogo delle informazioni nel piè di pagina del controllo GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) esercitazione per un aggiornamento.


Dopo aver impostato la `ShowFooter` proprietà `true`, dedicare alcuni minuti per visualizzare l'output in un browser. Attualmente t riga del piè di pagina contiene testo o i controlli Web. Nel passaggio 3 il piè di pagina per ogni campo di GridView verrà modificata in modo che includa l'interfaccia appropriata di inserimento.


[![La riga di piè di pagina vuota è visualizzato sopra il Paging controlli dell'interfaccia](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image13.png)

**Figura 7**: riga di piè di pagina vuota è visualizzato sopra il Paging controlli dell'interfaccia ([fare clic per visualizzare l'immagine ingrandita](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Passaggio 3: Personalizzazione della riga di piè di pagina

Nel [TemplateFields utilizzo nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) esercitazione è stato illustrato come personalizzare notevolmente la visualizzazione di una determinata colonna GridView con TemplateFields (in contrapposizione BoundField o CheckBoxFields), in [ Personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) abbiamo esaminato tramite TemplateFields per personalizzare l'interfaccia di modifica in un controllo GridView. Tenere presente che un TemplateField è costituito da un numero di modelli che definisce la combinazione di markup, i controlli Web e la sintassi di associazione dati utilizzata per alcuni tipi di righe. Il `ItemTemplate`, ad esempio, specifica il modello utilizzato per le righe di sola lettura, mentre il `EditItemTemplate` definisce il modello per la riga modificabile.

Insieme al `ItemTemplate` e `EditItemTemplate`, il TemplateField include anche un `FooterTemplate` che specifica il contenuto per la riga di piè di pagina. Pertanto, è possibile aggiungere i controlli Web necessari per ogni campo s inserimento interfaccia nel `FooterTemplate`. Per avviare, convertire tutti i campi in GridView TemplateFields. Questa operazione può essere eseguita facendo clic sul collegamento Modifica colonne nel controllo GridView s smart tag, la selezione di ogni campo nell'angolo inferiore sinistro e fare clic su Converti il campo in un collegamento TemplateField.


![Converte ogni campo in un TemplateField](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.gif)

**Figura 8**: converte ogni campo in un TemplateField


Fare clic su Converti il campo in un TemplateField attiva il tipo di campo corrente in un TemplateField equivalente. Ad esempio, ogni BoundField viene sostituito da un TemplateField con un `ItemTemplate` che contiene un'etichetta che consente di visualizzare il campo dati corrispondente e un `EditItemTemplate` che consente di visualizzare il campo dei dati in una casella di testo. Il `ProductName` BoundField è stata convertita nel markup TemplateField seguenti:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample3.aspx)]

Allo stesso modo, il `Discontinued` CheckBoxField è stata convertita in un TemplateField cui `ItemTemplate` e `EditItemTemplate` contiene un controllo casella di controllo Web (con il `ItemTemplate` s casella di controllo disabilitato). La proprietà di sola lettura `ProductID` BoundField è stata convertita in un TemplateField con un controllo etichetta in entrambe le `ItemTemplate` e `EditItemTemplate`. In breve, la conversione di un GridView esistente in un TemplateField è un modo semplice e rapido per attivare il più personalizzabile TemplateField senza perdere tutte le funzionalità esistenti campo s.

Poiché GridView è re utilizzo t supportano la modifica, è possibile rimuovere il `EditItemTemplate` da ogni TemplateField, lasciando solo il `ItemTemplate`. Dopo questa operazione, il markup dichiarativo di GridView s dovrebbe essere simile al seguente:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample4.aspx)]

Ora che ogni campo di GridView è stato convertito in un TemplateField, è possibile immettere l'interfaccia di inserimento appropriata in ogni campo s `FooterTemplate`. Alcuni campi non disporrà di un'interfaccia di inserimento (`ProductID`, ad esempio); altri utenti nei controlli Web utilizzati per raccogliere le informazioni sui nuovi prodotti s possono variare.

Per creare l'interfaccia di modifica, scegliere il collegamento di modifica modelli dal tag smart s GridView. Quindi, nell'elenco a discesa selezionare il campo s `FooterTemplate` e trascinare il controllo appropriato dalla casella degli strumenti nella finestra di progettazione.


[![Aggiungere l'interfaccia di inserimento appropriata per ogni FooterTemplate s campo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image15.png)

**Figura 9**: aggiungere l'interfaccia di inserimento appropriata per ogni campo s `FooterTemplate` ([fare clic per visualizzare l'immagine ingrandita](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image16.png))


Il seguente elenco puntato enumera i campi di GridView, che specifica l'interfaccia di inserimento per aggiungere:

- `ProductID`Nessuno.
- `ProductName`aggiungere una casella di testo e impostare il relativo `ID` a `NewProductName`. Aggiungere un controllo RequiredFieldValidator anche per assicurarsi che l'utente immette un valore per il nuovo nome di prodotto s.
- `SupplierID`Nessuno.
- `CategoryID`Nessuno.
- `QuantityPerUnit`aggiungere una casella di testo, l'impostazione relativa `ID` a `NewQuantityPerUnit`.
- `UnitPrice`aggiungere una casella di testo denominato `NewUnitPrice` e un controllo CompareValidator che garantisce il valore immesso è maggiore o uguale a zero un valore di valuta.
- `UnitsInStock`utilizzare una casella di testo la cui `ID` è impostato su `NewUnitsInStock`. Includere un CompareValidator che assicura che il valore immesso è un valore intero maggiore o uguale a zero.
- `UnitsOnOrder`utilizzare una casella di testo la cui `ID` è impostato su `NewUnitsOnOrder`. Includere un CompareValidator che assicura che il valore immesso è un valore intero maggiore o uguale a zero.
- `ReorderLevel`utilizzare una casella di testo la cui `ID` è impostato su `NewReorderLevel`. Includere un CompareValidator che assicura che il valore immesso è un valore intero maggiore o uguale a zero.
- `Discontinued`aggiungere una casella di controllo, l'impostazione relativa `ID` a `NewDiscontinued`.
- `CategoryName`aggiungere un controllo DropDownList e impostare il relativo `ID` a `NewCategoryID`. Associarlo a un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` e configurarlo per utilizzare il `CategoriesBLL` classe s `GetCategories()` metodo. Hanno s DropDownList `ListItem` visualizzazione s il `CategoryName` dati campo utilizzando il `CategoryID` dei valori di campo dei dati.
- `SupplierName`aggiungere un controllo DropDownList e impostare il relativo `ID` a `NewSupplierID`. Associarlo a un nuovo oggetto ObjectDataSource denominato `SuppliersDataSource` e configurarlo per utilizzare il `SuppliersBLL` classe s `GetSuppliers()` metodo. Hanno s DropDownList `ListItem` visualizzazione s il `CompanyName` dati campo utilizzando il `SupplierID` dei valori di campo dei dati.

Per ognuno dei controlli di convalida, cancellare il `ForeColor` proprietà in modo che il `FooterStyle` colore di sfondo bianco classe s CSS verrà utilizzato invece il valore predefinito di colore rosso. Utilizzare inoltre il `ErrorMessage` ma impostare la proprietà per una descrizione dettagliata, il `Text` proprietà su un asterisco. Per impedire che il testo del controllo s convalida che causa l'inserimento interfaccia va a capo in due righe, impostare il `FooterStyle` s `Wrap` la proprietà su false per ogni il `FooterTemplate` che usa un controllo di convalida. Infine, aggiungere un controllo ValidationSummary sotto GridView e set relativo `ShowMessageBox` proprietà `true` e il relativo `ShowSummary` proprietà `false`.

Quando si aggiunge un nuovo prodotto, è necessario fornire il `CategoryID` e `SupplierID`. Queste informazioni vengono acquisite tramite i controlli DropDownList nelle celle del piè di pagina per il `CategoryName` e `SupplierName` campi. Si è scelto di usare questi campi anziché le `CategoryID` e `SupplierID` TemplateFields poiché nella griglia s utente righe di dati è probabilmente più interessati a visualizzare i nomi di categoria e il fornitore anziché i relativi valori di ID. Poiché il `CategoryID` e `SupplierID` valori ora vengono acquisiti nel `CategoryName` e `SupplierName` interfacce inserimento s di campo, è possibile rimuovere il `CategoryID` e `SupplierID` TemplateFields dal controllo GridView.

Analogamente, il `ProductID` non viene utilizzato quando si aggiunge un nuovo prodotto, pertanto la `ProductID` TemplateField possono essere rimossi anche. S consente, tuttavia, lasciare il `ProductID` campo nella griglia. Oltre alle caselle di testo, controlli DropDownList, caselle di controllo e i controlli di convalida che costituiscono l'interfaccia di inserimento, sarà necessario anche un componente pulsante che, quando si fa clic, esegue la logica per aggiungere il nuovo prodotto per il database. Nel passaggio 4 verrà incluso un pulsante nell'interfaccia di inserimento in Aggiungi il `ProductID` TemplateField s `FooterTemplate`.

È possibile migliorare l'aspetto dei diversi campi di GridView. Potrebbe ad esempio, si desidera formattare il `UnitPrice` Allinea a destra i valori come valuta, il `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` campi e aggiornare il `HeaderText` i valori per il TemplateFields.

Dopo aver creato la gamma di inserimento delle interfacce nel `FooterTemplate` s, la rimozione di `SupplierID`, e `CategoryID` TemplateFields e migliorando estetica della griglia mediante la formattazione e l'allineamento di TemplateFields, di s GridView dichiarativa markup dovrebbe essere simile al seguente:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample5.aspx)]

Quando viene visualizzato tramite un browser, la riga di piè di pagina s GridView include ora completato inserimento dell'interfaccia (vedere la figura 10). A questo punto, t interfaccia inserimento includono un mezzo per l'utente indicare che s Mary immesso i dati per il nuovo prodotto e per inserire un nuovo record nel database. Inoltre, è ve ancora indirizzare come i dati immessi nel piè di pagina verranno convertito in un nuovo record di `Products` database. Nel passaggio 4 viene illustrato come includere un pulsante Aggiungi all'interfaccia di inserimento e su come eseguire codice su postback quando si fa clic su s. Passaggio 5 di seguito viene illustrato come inserire un nuovo record utilizzando i dati dal piè di pagina.


[![Il piè di pagina di GridView fornisce un'interfaccia per l'aggiunta di un nuovo Record](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image17.png)

**Figura 10**: piè di pagina GridView fornisce un'interfaccia per l'aggiunta di un nuovo Record ([fare clic per visualizzare l'immagine ingrandita](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Passaggio 4: Incluso un pulsante Aggiungi nell'interfaccia di inserimento

È necessario includere in un punto un pulsante Aggiungi nell'interfaccia di inserimento, poiché i dispositivi di riga di piè di pagina inserimento dell'interfaccia attualmente non dispone i mezzi per l'utente indicare che sono state completate immettere le nuove informazioni di prodotto s. Questo può essere inserito in una delle esistente `FooterTemplate` s o è stato possibile aggiungere una nuova colonna nella griglia a questo scopo. Per questa esercitazione, s consentono di posizionare il pulsante Aggiungi nel `ProductID` TemplateField s `FooterTemplate`.

Dalla finestra di progettazione, fare clic sul collegamento Modifica modelli nello smart tag GridView s e quindi scegliere il `ProductID` il campo s `FooterTemplate` dall'elenco a discesa. Aggiungere un controllo pulsante Web (o un LinkButton o ImageButton, se si preferisce) per il modello, impostandone l'ID su `AddProduct`, l'oggetto `CommandName` per inserire e il relativo `Text` proprietà da aggiungere come illustrato nella figura 11.


[![Posizionare il pulsante Aggiungi in FooterTemplate s ProductID TemplateField](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image19.png)

**Figura 11**: posizionare il pulsante Aggiungi nel `ProductID` TemplateField s `FooterTemplate` ([fare clic per visualizzare l'immagine ingrandita](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image20.png))


Una volta che è già incluso il pulsante Aggiungi, testare la pagina in un browser. Si noti che quando si fa clic sul pulsante Aggiungi con dati non valido nell'interfaccia di inserimento, il postback è circuited breve e il controllo ValidationSummary indica che i dati non validi (vedere Figura 12). Con i dati immessi appropriati, fare clic sul pulsante Aggiungi provoca un postback. Nessun record viene aggiunto al database, tuttavia. È necessario scrivere codice per eseguire effettivamente l'inserimento.


[![Il pulsante Aggiungi s Postback è Circuited breve se sono presenti dati non valido nell'interfaccia di inserimento](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image21.png)

**Figura 12**: il pulsante Aggiungi s Postback è Circuited breve se sono presenti dati non valido nell'interfaccia di inserimento ([fare clic per visualizzare l'immagine ingrandita](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image22.png))


> [!NOTE]
> I controlli di convalida nell'interfaccia di inserimento non sono stati assegnati a un gruppo di convalida. Funziona correttamente, purché l'interfaccia di inserimento è il solo set di controlli di convalida della pagina. Se, tuttavia, vi sono altri controlli di convalida della pagina (ad esempio i controlli di convalida nell'interfaccia di modifica della griglia s), i controlli di convalida nell'inserimento di interfaccia e aggiungere i pulsante s `ValidationGroup` proprietà devono essere assegnate lo stesso valore in modo da associare questi controlli a un gruppo di convalida specifico. Vedere [Sezionando i controlli di convalida in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) per ulteriori informazioni sul partizionamento i controlli di convalida e i pulsanti in una pagina in gruppi di convalida.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Passaggio 5: Inserimento di un nuovo Record nel`Products`tabella

Quando si usano le funzionalità di modifica predefinite di GridView, GridView gestisce automaticamente tutto il lavoro necessario per eseguire l'aggiornamento. In particolare, quando si fa clic sul pulsante Aggiorna copia i valori immessi dall'interfaccia di modifica per i parametri in s ObjectDataSource `UpdateParameters` raccolta e viene attivato, disattivato l'aggiornamento richiamando s ObjectDataSource `Update()` metodo. Poiché il controllo GridView non fornisce tale funzionalità incorporate per l'inserimento, è necessario implementare il codice che chiama il s ObjectDataSource `Insert()` (metodo) e copia i valori dall'inserimento di interfaccia al s ObjectDataSource `InsertParameters` raccolta .

Questa logica di inserimento deve essere eseguita una volta scelto il pulsante Aggiungi. Come descritto nel [aggiunta e la risposta a un controllo GridView i pulsanti](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs.md) esercitazione, ogni volta che viene scelto un pulsante, LinkButton o ImageButton in GridView, s GridView `RowCommand` evento viene generato durante il postback. Questo evento viene generato se il pulsante, LinkButton o ImageButton è stata aggiunta in modo esplicito, ad esempio il pulsante Aggiungi nella riga di piè di pagina o se è stato aggiunto automaticamente da GridView (ad esempio il LinkButton nella parte superiore di ogni colonna quando si abilita ordinamento è selezionata, o LinkButton nell'interfaccia di paging quando si seleziona Abilita Paging).

Pertanto, per rispondere all'utente facendo clic sul pulsante Aggiungi, è necessario creare un gestore eventi per i dispositivi di GridView `RowCommand` evento. Poiché questo evento viene generato ogni volta che *qualsiasi* si fa clic sul pulsante, LinkButton o ImageButton in GridView, si s fondamentali che abbiamo continuare solo con la logica di inserimento se la `CommandName` proprietà passato i mapping di gestore evento per il `CommandName` valore del pulsante Aggiungi (Insert). Inoltre, si dovrebbe continuare anche solo se i controlli di convalida dati validi del report. A tale scopo, creare un gestore eventi per il `RowCommand` evento con il codice seguente:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample6.cs)]

> [!NOTE]
> Si potrebbe chiedere perché il gestore dell'evento esserci verifica la `Page.IsValid` proprietà. Dopo tutto, t acquisite il postback essere eliminata se vengono forniti dati non validi nell'interfaccia di inserimento? Questa ipotesi sia corretta, purché l'utente non ha disabilitato JavaScript o abbia intrapreso azioni per aggirare il sistema la logica di convalida sul lato client. In breve, è possibile mai fare affidamento esclusivamente su convalida lato client; un controllo sul lato server per la validità deve sempre essere eseguito prima di utilizzare i dati.


Nel passaggio 1 è stato creato il `ProductsDataSource` ObjectDataSource in modo che il relativo `Insert()` metodo viene mappato al `ProductsBLL` classe s `AddProduct` (metodo). Per inserire il nuovo record nel `Products` tabella, possiamo semplicemente richiamare il s ObjectDataSource `Insert()` metodo:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample7.cs)]

Ora che il `Insert()` è stato richiamato metodo, tutto ciò che è comunque necessario copiare i valori dall'interfaccia di inserimento per i parametri passati al `ProductsBLL` classe s `AddProduct` metodo. Come abbiamo visto nuovamente il [esaminando gli eventi associati con l'inserimento, aggiornamento ed eliminazione](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) dell'esercitazione, ciò può essere eseguita tramite il s ObjectDataSource `Inserting` evento. Nel `Inserting` evento è necessario fare riferimento a livello di programmazione di controlli dal `Products` piè di pagina di GridView s riga e assegnare i valori per il `e.InputParameters` insieme. Se si omette un valore, ad esempio se si lascia il `ReorderLevel` casella di testo vuoto è necessario specificare che il valore inserito nel database deve essere `NULL`. Poiché il `AddProducts` metodo accetta i tipi nullable per i campi di database nullable, semplicemente utilizzare un tipo nullable e impostarne il valore su `null` nel caso in cui l'input dell'utente viene omesso.


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample8.cs)]

Con il `Inserting` gestore eventi completato, aggiungere nuovi record per il `Products` tabella di database tramite la riga di piè di pagina s GridView. Proseguo e provare ad aggiungere diversi nuovi prodotti.

## <a name="enhancing-and-customizing-the-add-operation"></a>Migliorare e personalizzare l'operazione di aggiunta

Attualmente, fare clic sul pulsante Aggiungi consente di aggiungere un nuovo record alla tabella di database ma non fornisce alcun feedback visivo che il record è stato aggiunto. In teoria, un Web etichetta controllo o sul lato client avviso sarebbe informerà l'utente che i relativi insert è stata completata con esito positivo. Lasciare questo campo come esercizio per il lettore.

GridView utilizzato in questa esercitazione non è valida qualsiasi criterio di ordinamento per i prodotti elencati, né consente all'utente di ordinare i dati. Di conseguenza, i record vengono ordinati come se fossero del database per il campo di chiave primaria. Poiché ogni nuovo record contiene un `ProductID` valore maggiore di quella più recente, ogni volta che viene aggiunto un nuovo prodotto aggiunto alla fine della griglia. Pertanto, vuoi inviare automaticamente l'utente all'ultima pagina di GridView dopo l'aggiunta di un nuovo record. Questa operazione può essere eseguita aggiungendo la riga di codice seguente dopo la chiamata a `ProductsDataSource.Insert()` nel `RowCommand` gestore dell'evento per indicare che l'utente deve essere inviato all'ultima pagina dopo l'associazione dati a GridView:


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample9.cs)]

`SendUserToLastPage`è una variabile Boolean a livello di pagina che inizialmente è assegnata un valore di `false`. In GridView s `DataBound` gestore dell'evento, se `SendUserToLastPage` è false, il `PageIndex` proprietà viene aggiornata per inviare all'utente per l'ultima pagina.


[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample10.cs)]

Il motivo il `PageIndex` proprietà viene impostata nel `DataBound` gestore dell'evento (in contrapposizione al `RowCommand` gestore dell'evento) perché quando il `RowCommand` gestore dell'evento viene generato è ve ancora per aggiungere il nuovo record per il `Products` tabella di database. Pertanto, nel `RowCommand` gestore dell'evento l'ultimo indice della pagina (`PageCount - 1`) rappresenta l'ultimo indice della pagina *prima* è stato aggiunto il nuovo prodotto. Per la maggior parte dei prodotti da aggiungere, l'ultimo indice della pagina è lo stesso dopo aver aggiunto il nuovo prodotto. Ma quando il prodotto aggiunto comporta un nuovo indice ultima pagina, se si aggiorna in modo non corretto di `PageIndex` nel `RowCommand` gestore eventi, si verrà visualizzate la seconda all'ultima pagina (l'ultimo indice della pagina prima di aggiungere il nuovo prodotto) anziché l'ultima pagina nuova i indice. Poiché il `DataBound` gestore dell'evento viene generato dopo che è stato aggiunto il nuovo prodotto e i dati riassociati alla griglia, impostando il `PageIndex` sono proprietà sappiamo è re ottenere l'ultimo indice di pagina corretti.

Infine, GridView utilizzato in questa esercitazione è molto ampia a causa del numero di campi che devono essere raccolti per l'aggiunta di un nuovo prodotto. A causa di questa larghezza, un layout verticale di DetailsView s potrebbe essere preferito. S GridView larghezza potrebbe essere ridotto mediante la raccolta di input di un numero inferiore. Probabilmente non abbiamo non è necessario raccogliere il `UnitsOnOrder`, `UnitsInStock`, e `ReorderLevel` campi quando si aggiunge un nuovo prodotto, nel qual caso è stato possibile rimuovere questi campi dal controllo GridView.

Per modificare i dati raccolti, è possibile utilizzare uno dei due approcci:

- Continuare a utilizzare il `AddProduct` metodo che prevede valori per il `UnitsOnOrder`, `UnitsInStock`, e `ReorderLevel` campi. Nel `Inserting` gestore dell'evento, fornire hardcoded, predefiniti i valori da utilizzare per i dati di input che sono state rimosse dall'interfaccia di inserimento.
- Creare un nuovo overload del `AddProduct` metodo il `ProductsBLL` classe che non accetta input per il `UnitsOnOrder`, `UnitsInStock`, e `ReorderLevel` campi. Quindi, nella pagina ASP.NET, configurare ObjectDataSource per utilizzare questo overload di nuovo.

Entrambe le opzioni funzionerà come altrettanto bene. In oltre esercitazioni è stata utilizzata la seconda opzione, la creazione di più overload per il `ProductsBLL` classe s `UpdateProduct` metodo.

## <a name="summary"></a>Riepilogo

Le funzionalità incorporate di inserimento trovate nel controllo DetailsView e FormView non dispone di GridView, ma con un minimo sforzo è possibile aggiungere un'interfaccia di inserimento alla riga di piè di pagina. Per visualizzare semplicemente la riga di piè di pagina in GridView, impostare il relativo `ShowFooter` proprietà `true`. Il contenuto della riga del piè di pagina può essere personalizzato per ogni campo convertendo il campo in un TemplateField e aggiungendo l'inserimento di interfaccia di `FooterTemplate`. Come illustrato in questa esercitazione, il `FooterTemplate` può contenere i pulsanti, caselle di testo, controlli DropDownList, caselle di controllo, i controlli origine dati per il popolamento basati sui dati di controlli Web (ad esempio controlli DropDownList) e i controlli di convalida. Insieme a controlli per raccogliere l'input dell'utente s, è necessario un pulsante Aggiungi, LinkButton o ImageButton.

Quando si fa clic sul pulsante Aggiungi, s ObjectDataSource `Insert()` metodo viene chiamato per avviare il flusso di lavoro di inserimento. ObjectDataSource chiamerà quindi il metodo insert configurato (il `ProductsBLL` classe s `AddProduct` (metodo), in questa esercitazione). È necessario copiare i valori da s GridView inserimento interfaccia al s ObjectDataSource `InsertParameters` raccolta prima il metodo insert richiamato. Questa operazione può essere eseguita facendo riferimento a livello di codice i controlli Web inserimento dell'interfaccia in s ObjectDataSource `Inserting` gestore dell'evento.

Questa esercitazione viene completato il nostro esaminerà tecniche per migliorare l'aspetto di GridView s. Il set successivo di esercitazioni esaminerà come lavorare con dati binari, ad esempio immagini, documenti di Word, PDF e così via e i controlli Web di dati.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Bernadette Leigh. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](adding-a-gridview-column-of-checkboxes-cs.md)
[Successivo](adding-a-gridview-column-of-radio-buttons-vb.md)
