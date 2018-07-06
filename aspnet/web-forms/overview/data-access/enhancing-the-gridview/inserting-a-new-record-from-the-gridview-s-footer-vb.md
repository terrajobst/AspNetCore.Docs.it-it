---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
title: Inserimento di un nuovo Record dal piè di pagina del controllo GridView (VB) | Microsoft Docs
author: rick-anderson
description: Mentre il controllo GridView non fornisce supporto incorporato per l'inserimento di un nuovo record di dati, questa esercitazione illustra come migliorare il controllo GridView per includere un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 528acc48-f20c-4b4e-aa16-4cc02f068ebb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: c228128e551f58aa003af10cf787875d26b1fab7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375179"
---
<a name="inserting-a-new-record-from-the-gridviews-footer-vb"></a>Inserimento di un nuovo Record dal piè di pagina del controllo GridView (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_VB.exe) o [Scarica il PDF](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/datatutorial53vb1.pdf)

> Mentre il controllo GridView non fornisce supporto incorporato per l'inserimento di un nuovo record di dati, questa esercitazione illustra come migliorare il controllo GridView in modo da includere un'interfaccia di inserimento.


## <a name="introduction"></a>Introduzione

Come descritto nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) dell'esercitazione, i controlli GridView, DetailsView e FormView Web includono funzionalità di modifica dei dati incorporati. Se usato con i controlli origine dati dichiarativi, questi tre controlli Web rapidamente e facilmente configurabili per modificare i dati - e negli scenari senza la necessità di scrivere una singola riga di codice. Sfortunatamente, solo i controlli DetailsView e FormView forniscono incorporati di inserimento, modifica ed eliminazione funzionalità. Il controllo GridView offre solo la modifica ed eliminazione di supporto. Con un piccolo gomito grasso, tuttavia, possiamo forniamo GridView per includere un'interfaccia di inserimento.

Aggiunta di funzionalità di inserimento per il controllo GridView, siamo responsabili per decidere come i nuovi record verranno aggiunti, la creazione dell'interfaccia di inserimento e la scrittura del codice per inserire il nuovo record. In questa esercitazione che verrà esaminato l'aggiunta di interfaccia di inserimento nel piè di pagina di GridView s riga (vedere la figura 1). La cella del piè di pagina per ogni colonna include i dati appropriati raccolta elemento dell'interfaccia utente (una casella di testo per il nome del prodotto s, un controllo DropDownList per i fornitori e così via). Collaboriamo anche con una colonna per un'operazione di aggiunta pulsante che, quando si fa clic, viene provocato un postback e inserire un nuovo record nel `Products` tabella usando i valori specificati nella riga del piè di pagina.


[![La riga di piè di pagina offre un'interfaccia per l'aggiunta di nuovi prodotti](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image1.png)

**Figura 1**: la riga di piè di pagina fornisce un'interfaccia per l'aggiunta di nuovi prodotti ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.png))


## <a name="step-1-displaying-product-information-in-a-gridview"></a>Passaggio 1: Visualizzare le informazioni sul prodotto in un controllo GridView

Prima che si sono relative a noi con la creazione dell'interfaccia di inserimento nel piè di pagina s GridView, lasciare che lo stato attivo prima con s sull'aggiunta di un controllo GridView alla pagina che elenca i prodotti nel database. Iniziare aprendo il `InsertThroughFooter.aspx` nella pagina la `EnhancedGridView` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione, l'impostazione s GridView `ID` proprietà `Products`. Usare quindi lo smart tag s di GridView per associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`.


[![Creare un nuovo oggetto ObjectDataSource denominato ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.png)

**Figura 2**: creare un nuovo oggetto ObjectDataSource denominato `ProductsDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.png))


Configurare ObjectDataSource per usare la `ProductsBLL` classe s `GetProducts()` metodo per recuperare le informazioni sul prodotto. Per questa esercitazione, lasciare che lo stato attivo s esclusivamente sull'aggiunta di funzionalità di inserimento e senza doversi preoccupare di modifica ed eliminazione. Pertanto, assicurarsi che l'elenco di riepilogo a discesa nella scheda Inserisci è impostato su `AddProduct()` e che gli elenchi a discesa nelle schede UPDATE e DELETE sono impostati su (nessuno).


[![Eseguire il mapping del metodo AddProduct al metodo ObjectDataSource s Insert)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.png)

**Figura 3**: mappa il `AddProduct` metodo s ObjectDataSource `Insert()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.png))


[![Impostare gli elenchi di riepilogo a discesa UPDATE e DELETE tabulazioni su (nessuno)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.png)

**Figura 4**: impostare l'aggiornamento ed eliminare schede elenco a discesa Elenca su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.png))


Dopo aver completato la procedura guidata Configura origine dati di ObjectDataSource s, Visual Studio aggiungerà automaticamente i campi per il controllo GridView per ognuno dei campi dati corrispondenti. Per il momento, lasciare tutti i campi aggiunti da Visual Studio. Più avanti in questa esercitazione verrà tornare indietro e rimuovere alcuni dei campi i cui valori desidero devono essere specificate quando si aggiunge un nuovo record.

Poiché sono presenti più vicino 80 prodotti nel database, sarà necessario un utente di scorrere tutti gli altri fino alla fine della pagina web per aggiungere un nuovo record. Pertanto, lasciare s attivare il paging rendere l'interfaccia di inserimento più visibile e accessibile. Per attivare il paging, è sufficiente selezionare la casella di controllo Attiva Paging nello smart tag s GridView.

A questo punto, il markup dichiarativo s GridView e ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample1.aspx)]


[![Tutti i campi di dati prodotto vengono visualizzati in un controllo GridView di paging](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.png)

**Per visualizzare la riga di piè di pagina in un controllo GridView è sufficiente impostare relativi **proprietà**.


## <a name="step-2-adding-a-footer-row"></a>Il contenuto della riga del piè di pagina può essere personalizzato per ogni campo convertendo il campo in un TemplateField e aggiungendo l'inserimento dell'interfaccia per il .

Come abbiamo visto in questa esercitazione il  può contenere i pulsanti, caselle di testo, controlli DropDownList, caselle di controllo, i controlli origine dati per il popolamento basato sui dati di controlli Web (ad esempio controlli DropDownList) e i controlli di convalida. Insieme a controlli per la raccolta di input dell'utente s, è necessario un pulsante Aggiungi, LinkButton o ImageButton. Quando si fa clic sul pulsante Aggiungi, s ObjectDataSource `ShowFooter` metodo viene richiamato per iniziare l'inserimento del flusso di lavoro. ObjectDataSource chiamerà quindi il metodo insert configurato (il `ShowFooter` classe s `True` metodo, in questa esercitazione).


[![È necessario copiare i valori da s GridView inserimento dell'interfaccia per gli oggetti ObjectDataSource [ raccolta prima dell'inserimento metodo richiamato.](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.png)

**Questa operazione può essere eseguita facendo riferimento a livello di codice i controlli Web interfaccia inserimento in s ObjectDataSource ** gestore dell'evento.


Questa esercitazione viene completata la sguardo alle tecniche per migliorare l'aspetto GridView. Il set successivo di esercitazioni verrà esaminate come lavorare con dati binari quali immagini, PDF, documenti di Word e così via e i controlli Web dei dati. Il revisore capo di questa esercitazione è stata Bernadette Leigh. Il `FooterStyle` classe è definita in `Styles.css` come indicato di seguito:


[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample2.css)]

> [!NOTE]
> Abbiamo ve esaminate tramite la riga di piè di pagina di GridView s nelle esercitazioni precedenti. Se necessario, fare riferimento al [visualizzando le informazioni di riepilogo nel piè di pagina di GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) esercitazione per un processo di aggiornamento.


Dopo aver impostato la `ShowFooter` proprietà `True`, si consiglia di visualizzare l'output in un browser. Attualmente t riga del piè di pagina contiene testo o controlli Web. Nel passaggio 3 si modificherà il piè di pagina per ogni campo di GridView in modo che includa inserimento interfaccia appropriata.


[![La riga di piè di pagina vuota è visualizzato sopra il Paging di controlli dell'interfaccia](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image13.png)

**Figura 7**: riga di piè di pagina vuota viene visualizzato sopra il Paging di controlli dell'interfaccia ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image14.png))


## <a name="step-3-customizing-the-footer-row"></a>Passaggio 3: Personalizzazione della riga di piè di pagina

Nel [uso di TemplateFields nel controllo GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) esercitazione è stato illustrato come personalizzare notevolmente la visualizzazione di una determinata colonna GridView uso di TemplateFields (in contrapposizione BoundField o CheckBoxFields); nella [ Personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) rimando all'uso di TemplateFields per personalizzare l'interfaccia di modifica in un controllo GridView. Tenere presente che un TemplateField è costituito da diversi modelli che definisce la combinazione di markup, i controlli Web e la sintassi di associazione dati usato per determinati tipi di righe. Il `ItemTemplate`, ad esempio, specifica il modello utilizzato per le righe di sola lettura, mentre il `EditItemTemplate` definisce il modello per la riga modificabile.

Con il `ItemTemplate` e `EditItemTemplate`, il TemplateField include anche un `FooterTemplate` che specifica il contenuto per la riga di piè di pagina. Pertanto, è possibile aggiungere i controlli Web necessari per l'inserimento di interfaccia in ogni il campo s il `FooterTemplate`. Per iniziare, convertire tutti i campi in GridView in TemplateFields. Questa operazione può essere eseguita selezionando il collegamento di modifica colonne in GridView s smart tag, selezionando ciascun campo nell'angolo inferiore sinistro e fare clic su Converti il campo in un collegamento TemplateField.


![Convertire ogni campo in un TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image8.gif)

**Figura 8**: convertire ogni campo in un TemplateField


Facendo clic su Converti il campo in un TemplateField trasforma il tipo di campo corrente in un TemplateField equivalente. Ad esempio, ogni BoundField viene sostituito da un TemplateField con un `ItemTemplate` che contiene un'etichetta che visualizza il campo dati corrispondente e un `EditItemTemplate` che visualizza il campo dati in una casella di testo. Il `ProductName` BoundField è stata convertita in markup TemplateField riportato di seguito:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample3.aspx)]

Allo stesso modo, il `Discontinued` CampoCasellaDiControllo è stata convertita in un TemplateField la cui proprietà `ItemTemplate` e `EditItemTemplate` contengono un controllo casella di controllo Web (con il `ItemTemplate` s casella di controllo disabilitato). Di sola lettura `ProductID` BoundField è stata convertita in un TemplateField con un controllo etichetta in entrambe le `ItemTemplate` e `EditItemTemplate`. In breve, la conversione di un controllo GridView esistente campo in un TemplateField è un modo semplice e rapido per passare per il livello di personalizzazione TemplateField senza perdere le funzionalità esistenti di campo s.

Poiché il controllo GridView è re lavora sul supporto t la modifica, è possibile rimuovere il `EditItemTemplate` da ogni TemplateField, lasciando solo il `ItemTemplate`. Dopo questa operazione, il markup dichiarativo di GridView s dovrebbe essere simile al seguente:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample4.aspx)]

Ora che ogni campo di controllo GridView è stato convertito in un TemplateField, è possibile immettere l'interfaccia appropriata di inserimento in s ogni campo `FooterTemplate`. Alcuni dei campi non avrà un'interfaccia di inserimento (`ProductID`, ad esempio); altri possono variare nei controlli Web consente di raccogliere le informazioni sui nuovi prodotti s.

Per creare l'interfaccia di modifica, scegliere il collegamento di modifica modelli dello smart tag s GridView. Quindi, l'elenco a discesa, selezionare il campo appropriato s `FooterTemplate` e trascinare il controllo appropriato dalla casella degli strumenti nella finestra di progettazione.


[![Aggiungere l'interfaccia di inserimento appropriata a ogni FooterTemplate s campo](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image15.png)

**Figura 9**: aggiungere l'interfaccia di inserimento appropriata a ogni campo s `FooterTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image16.png))


Nell'elenco puntato seguente enumera i campi di GridView, che specifica l'inserimento dell'interfaccia da aggiungere:

- `ProductID` Nessuno.
- `ProductName` aggiungere una casella di testo e impostare relativi `ID` a `NewProductName`. Aggiungere un controllo RequiredFieldValidator anche per assicurarsi che l'utente immette un valore per il nuovo nome del prodotto s.
- `SupplierID` Nessuno.
- `CategoryID` Nessuno.
- `QuantityPerUnit` aggiungere una casella di testo, l'impostazione relativa `ID` a `NewQuantityPerUnit`.
- `UnitPrice` aggiungere una casella di testo denominato `NewUnitPrice` e un controllo CompareValidator che garantisce il valore immesso è maggiore o uguale a zero un valore di valuta.
- `UnitsInStock` usare una casella di testo il cui `ID` è impostata su `NewUnitsInStock`. Includere un controllo CompareValidator che assicura che il valore immesso è un valore intero maggiore o uguale a zero.
- `UnitsOnOrder` usare una casella di testo il cui `ID` è impostata su `NewUnitsOnOrder`. Includere un controllo CompareValidator che assicura che il valore immesso è un valore intero maggiore o uguale a zero.
- `ReorderLevel` usare una casella di testo il cui `ID` è impostata su `NewReorderLevel`. Includere un controllo CompareValidator che assicura che il valore immesso è un valore intero maggiore o uguale a zero.
- `Discontinued` aggiungere una casella di controllo, l'impostazione relativa `ID` a `NewDiscontinued`.
- `CategoryName` aggiungere un controllo DropDownList e impostare relativi `ID` a `NewCategoryID`. Associarlo a un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` e configurarlo per usare il `CategoriesBLL` classe s `GetCategories()` (metodo). Hanno la s DropDownList `ListItem` display s il `CategoryName` dati campo, utilizzando il `CategoryID` campo dati come i relativi valori.
- `SupplierName` aggiungere un controllo DropDownList e impostare relativi `ID` a `NewSupplierID`. Associarlo a un nuovo oggetto ObjectDataSource denominato `SuppliersDataSource` e configurarlo per usare il `SuppliersBLL` classe s `GetSuppliers()` (metodo). Hanno la s DropDownList `ListItem` display s il `CompanyName` dati campo, utilizzando il `SupplierID` campo dati come i relativi valori.

Per ognuno dei controlli di convalida, cancelliamo i `ForeColor` proprietà, in modo che il `FooterStyle` colore di sfondo bianco di classe s CSS verrà usato al posto del valore predefinito di colore rosso. Usare anche il `ErrorMessage` ma impostare la proprietà per una descrizione dettagliata, il `Text` proprietà su un asterisco. Per evitare che il testo del controllo s convalida che causa l'inserimento dell'interfaccia a capo su due righe, impostare il `FooterStyle` s `Wrap` la proprietà su false per ognuna del `FooterTemplate` che usa un controllo di convalida. Infine, aggiungere un controllo di controllo ValidationSummary sotto il controllo GridView e il set relativo `ShowMessageBox` proprietà `True` e il relativo `ShowSummary` proprietà `False`.

Quando si aggiunge un nuovo prodotto, è necessario fornire il `CategoryID` e `SupplierID`. Queste informazioni vengono acquisite tramite i controlli DropDownList nelle celle di un piè di pagina per il `CategoryName` e `SupplierName` campi. Ho scelto di usare questi campi anziché le `CategoryID` e `SupplierID` TemplateFields poiché nella griglia s l'utente righe di dati è probabilmente più interessati a visualizzare i nomi di categoria e il fornitore, anziché i relativi valori ID. Poiché il `CategoryID` e `SupplierID` ora i valori vengono acquisiti nel `CategoryName` e `SupplierName` interfacce inserimento s di campo, è possibile rimuovere il `CategoryID` e `SupplierID` TemplateFields da GridView.

Analogamente, il `ProductID` non viene usato quando si aggiunge un nuovo prodotto, pertanto la `ProductID` TemplateField può anche essere rimossi. Tuttavia, consentire s lasciare il `ProductID` campo nella griglia. Oltre alle caselle di testo, controlli DropDownList, caselle di controllo e i controlli di convalida che costituiscono l'interfaccia di inserimento, sono necessari anche un'operazione di aggiunta pulsante che, quando si fa clic, esegue la logica per aggiungere il nuovo prodotto al database. Nel passaggio 4 verrà incluso un pulsante di aggiunta nell'interfaccia di inserimento nel `ProductID` TemplateField s `FooterTemplate`.

È possibile migliorare l'aspetto dei diversi campi GridView. Potrebbe ad esempio, si desidera formattare il `UnitPrice` Allinea a destra i valori come una valuta, il `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` campi e aggiornamento di `HeaderText` i valori per il TemplateFields.

Dopo aver creato la grande quantità di inserire le interfacce nel `FooterTemplate` s, rimuovendo il `SupplierID`, e `CategoryID` TemplateFields e migliorare l'estetica della griglia tramite formattazione e allineamento di TemplateFields, la s GridView dichiarativa markup dovrebbe essere simile al seguente:


[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample5.aspx)]

Quando viene visualizzato tramite un browser, la riga di piè di pagina di GridView s include ora completato inserimento dell'interfaccia (vedere la figura 10). A questo punto, l'inserimento t di interfaccia include un mezzo per l'utente indicare che s Mary immessi i dati per il nuovo prodotto e si vuole inserire un nuovo record nel database. Inoltre, abbiamo ve ancora a risolvere come i dati immessi nel piè di pagina verranno convertito in un nuovo record nel `Products` database. Nel passaggio 4, esamineremo come includere un pulsante di aggiunta all'interfaccia di inserimento e su come eseguire il codice nel postback quando viene s selezionato. Passaggio 5 viene illustrato come inserire un nuovo record con i dati nel piè di pagina.


[![Il piè di pagina di GridView fornisce un'interfaccia per l'aggiunta di un nuovo Record](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image17.png)

**Figura 10**: il piè di pagina di GridView fornisce un'interfaccia per l'aggiunta di un nuovo Record ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image18.png))


## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Passaggio 4: Incluso un pulsante di aggiunta nell'interfaccia di inserimento

È necessario includere in una posizione un pulsante Aggiungi nell'interfaccia di inserimento, poiché i dispositivi di riga del piè di pagina inserimento dell'interfaccia attualmente non dispone i mezzi per l'utente indicare che sono state completate immettendo le informazioni sui nuovi prodotti s. Ciò può essere inserito in uno dei esistente `FooterTemplate` s o è stato possibile aggiungere una nuova colonna nella griglia per questo scopo. Per questa esercitazione, ti permettono di s posizionare il pulsante Aggiungi nella `ProductID` TemplateField s `FooterTemplate`.

La finestra di progettazione, fare clic sul collegamento Modifica modelli nello smart tag GridView s e quindi scegliere il `ProductID` il campo s `FooterTemplate` nell'elenco a discesa. Aggiungere un controllo pulsante Web (o un LinkButton o ImageButton, se si preferisce) per il modello, impostandone l'ID su `AddProduct`, la relativa `CommandName` a Insert e il relativo `Text` proprietà da aggiungere come illustrato nella figura 11.


[![Posizionare il pulsante Aggiungi in FooterTemplate s ProductID TemplateField](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image19.png)

**Figura 11**: posizionare il pulsante Aggiungi nella `ProductID` s TemplateField `FooterTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image20.png))


Dopo che è già incluso il pulsante Aggiungi, testare la pagina in un browser. Si noti che, quando si fa clic sul pulsante Aggiungi con dati non validi nell'interfaccia di inserimento, viene circuited breve il postback e di controllo ValidationSummary indica i dati non validi (vedere la figura 12). Con i dati corretti immessi, fare clic sul pulsante Aggiungi causa un postback. Nessun record viene aggiunto al database, tuttavia. È necessario scrivere un po' di codice per eseguire effettivamente l'istruzione insert.


[![Il pulsante Aggiungi s Postback è Circuited breve se sono presenti dati non valido nell'interfaccia di inserimento](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image21.png)

**Figura 12**: il pulsante Aggiungi s Postback è Circuited breve se sono presenti dati non valido nell'interfaccia di inserimento ([fare clic per visualizzare l'immagine con dimensioni normali](inserting-a-new-record-from-the-gridview-s-footer-vb/_static/image22.png))


> [!NOTE]
> I controlli di convalida nell'interfaccia di inserimento non sono stati assegnati a un gruppo di convalida. Ciò funziona correttamente, purché l'interfaccia di inserimento è il solo set di controlli di convalida della pagina. Se, tuttavia, esistono altri controlli di convalida della pagina (ad esempio i controlli di convalida nell'interfaccia di modifica della griglia s), i controlli di convalida nell'inserimento dell'interfaccia e aggiungere pulsante s `ValidationGroup` proprietà devono essere assegnate lo stesso valore in modo da associare questi controlli a un gruppo di convalida specifico. Visualizzare [Sezionando i controlli di convalida in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) per altre informazioni sul partizionamento i controlli di convalida e i pulsanti in una pagina in gruppi di convalida.


## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Passaggio 5: Inserimento di un nuovo Record nel`Products`tabella

Quando si usano le funzionalità di modifica predefinite di GridView, il controllo GridView gestisce automaticamente tutte le operazioni necessarie per eseguire l'aggiornamento. In particolare, quando si fa clic sul pulsante Aggiorna copia i valori immessi dall'interfaccia di modifica per i parametri in s ObjectDataSource `UpdateParameters` raccolta e viene attivata disattivato l'aggiornamento richiamando la s ObjectDataSource `Update()` (metodo). Poiché il controllo GridView non fornisce queste funzionalità incorporata per l'inserimento, è necessario implementare il codice che chiama gli oggetti ObjectDataSource `Insert()` (metodo) e vengono copiati i valori dall'inserimento dell'interfaccia per gli oggetti ObjectDataSource `InsertParameters` raccolta .

Questa logica di inserimento deve essere eseguita dopo che è stato fatto clic sul pulsante Aggiungi. Come descritto nel [aggiunta e risposta ai pulsanti in un controllo GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb.md) esercitazione, ogni volta che viene scelto un pulsante, LinkButton o ImageButton in un controllo GridView, s GridView `RowCommand` viene generato l'evento durante il postback. Questo evento viene generato se il pulsante, LinkButton o ImageButton è stato aggiunto in modo esplicito, ad esempio il pulsante Aggiungi nella riga del piè di pagina o se è stato automaticamente aggiunto per il controllo GridView (ad esempio il LinkButton nella parte superiore di ogni colonna quando si abilita l'ordinamento è selezionata, o Controlli LinkButton nell'interfaccia di paging, quando si seleziona Abilita Paging).

Pertanto, per rispondere al clic sul pulsante Aggiungi dell'utente, è necessario creare un gestore eventi per s GridView `RowCommand` evento. Poiché questo evento viene generato ogni volta che *eventuali* si fa clic sul pulsante, LinkButton e ImageButton in GridView, s fondamentali che abbiamo solo procedere con la logica di inserimento se la `CommandName` proprietà passato nel gestore mappe eventi per il `CommandName` valore del pulsante Aggiungi (Insert). Inoltre, abbiamo dovremmo procedere anche solo se i controlli di convalida segnalano dati validi. Per risolvere questo problema, creare un gestore eventi per il `RowCommand` evento con il codice seguente:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample6.vb)]

> [!NOTE]
> Si potrebbe chiedere perché il gestore eventi anche controllo di `Page.IsValid` proprietà. Dopo tutto, t vinti il postback eliminati se i dati non validi viene forniti nell'interfaccia di inserimento? Questa ipotesi è corretta, purché l'utente non ha disabilitato JavaScript o ha eseguito i passaggi per aggirare la logica di convalida lato client. In breve, uno non fare mai affidamento esclusivamente su convalida lato client; un controllo lato server per la validità deve essere sempre eseguito prima di usare i dati.


Nel passaggio 1 è stato creato il `ProductsDataSource` ObjectDataSource in modo che relativo `Insert()` metodo viene eseguito il mapping per il `ProductsBLL` classe s `AddProduct` (metodo). Per inserire il nuovo record nel `Products` tabella, possiamo semplicemente richiamare gli oggetti ObjectDataSource `Insert()` metodo:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample7.vb)]

Ora che il `Insert()` metodo è stato richiamato, tutto ciò che resta consiste nel copiare i valori dall'interfaccia di inserimento per i parametri passati al `ProductsBLL` classe s `AddProduct` (metodo). Come abbiamo visto nella [esaminando gli eventi associati con inserimento, aggiornamento ed eliminazione in corso](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) tutorial, ciò può essere eseguita tramite gli oggetti ObjectDataSource `Inserting` evento. Nel `Inserting` dobbiamo fare riferimento a livello di codice i controlli dall'evento il `Products` piè di pagina di GridView s delle righe e assegnare i relativi valori per il `e.InputParameters` raccolta. Se l'utente la omette un valore, ad esempio lasciando il `ReorderLevel` vuoto nella casella di testo è necessario specificare che il valore inserito nel database deve essere `NULL`. Poiché il `AddProducts` metodo accetta i tipi nullable per i campi del database che ammette valori null, è sufficiente usare un tipo nullable e impostarne il valore su `Nothing` nel caso in cui l'input dell'utente viene omessa.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample8.vb)]

Con il `Inserting` gestore di eventi completato, nuovi record può essere aggiunto al `Products` tabella del database tramite la riga di piè di pagina di GridView s. Proseguo e provare ad aggiungere alcuni nuovi prodotti.

## <a name="enhancing-and-customizing-the-add-operation"></a>Migliorare e personalizzare l'operazione di aggiunta

Attualmente, facendo clic sul pulsante Aggiungi aggiunge un nuovo record alla tabella di database ma non fornisce alcun tipo di feedback visivo che il record è stato aggiunto. In teoria, un'etichetta Web lato client o controllo finestra di avviso potrebbe informare l'utente che loro inserimento è stata completata con esito positivo. Lasciare come esercizio per il lettore.

GridView usato in questa esercitazione non si applica alcun ordinamento per i prodotti elencati, né consente all'utente di ordinare i dati. Di conseguenza, i record vengono ordinati come sono nel database tramite il campo di chiave primaria. Poiché ogni nuovo record ha un `ProductID` valore maggiore di quella più recente, ogni volta che viene aggiunto un nuovo prodotto viene aggiunto alla fine della griglia. Pertanto, è possibile inviare automaticamente l'utente all'ultima pagina di GridView dopo aver aggiunto un nuovo record. Questa operazione può essere eseguita aggiungendo la riga di codice seguente dopo la chiamata a `ProductsDataSource.Insert()` nella `RowCommand` gestore dell'evento per indicare che l'utente deve essere inviato all'ultima pagina dopo l'associazione dati a GridView:


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample9.vb)]

`SendUserToLastPage` variabile booleana a livello di pagina che viene inizialmente assegnata un valore di `False`. In s GridView `DataBound` gestore dell'evento, se `SendUserToLastPage` è false, il `PageIndex` proprietà viene aggiornata per l'invio l'utente all'ultima pagina.


[!code-vb[Main](inserting-a-new-record-from-the-gridview-s-footer-vb/samples/sample10.vb)]

Il motivo il `PageIndex` viene impostata `DataBound` gestore dell'evento (in contrapposizione al `RowCommand` gestore dell'evento), infatti, quando il `RowCommand` gestore dell'evento viene generato è fornire un supporto iniziale per aggiungere il nuovo record per il `Products` tabella di database. Pertanto, nelle `RowCommand` gestore dell'evento dell'ultimo indice di pagina (`PageCount - 1`) rappresenta l'ultimo indice della pagina *prima* è stato aggiunto il nuovo prodotto. Per la maggior parte dei prodotti da aggiungere, l'ultimo indice di pagina è lo stesso dopo aver aggiunto il nuovo prodotto. Mentre quando il risultato prodotto aggiunto in un nuovo indice ultima pagina, se si aggiorna in modo non corretto di `PageIndex` nella `RowCommand` gestore dell'evento, quindi viene visualizzate la seconda all'ultima pagina (l'ultimo indice di pagina prima di aggiungere il nuovo prodotto) anziché la nuova pagina ultimo i indice. Poiché il `DataBound` gestore dell'evento viene generato dopo che è stato aggiunto il nuovo prodotto e i dati riassociati alla griglia, impostando il `PageIndex` presenti proprietà sappiamo si ri ottenere l'ultimo indice di pagina corretti.

Infine, il controllo GridView usato in questa esercitazione è piuttosto ampio a causa del numero di campi che devono essere raccolti per l'aggiunta di un nuovo prodotto. A causa di questa larghezza può essere preferibile un layout verticale s DetailsView. Le s GridView larghezza complessiva potrebbe essere ridotto mediante la raccolta di input un numero inferiore. Forse temiamo t necessità di raccogliere le `UnitsOnOrder`, `UnitsInStock`, e `ReorderLevel` campi quando si aggiunge un nuovo prodotto, nel qual caso è stato possibile rimuovere questi campi da GridView.

Per modificare i dati raccolti, è possibile usare uno dei due approcci:

- Continuare a usare il `AddProduct` metodo che prevede valori per il `UnitsOnOrder`, `UnitsInStock`, e `ReorderLevel` campi. Nel `Inserting` gestore eventi, specificare a livello di codice, da usare per i dati di input che sono state rimosse dall'interfaccia di inserimento valori predefiniti.
- Creare un nuovo overload del `AddProduct` metodo nella `ProductsBLL` classe che non accetta input per il `UnitsOnOrder`, `UnitsInStock`, e `ReorderLevel` campi. Quindi, nella pagina ASP.NET, configurare ObjectDataSource per utilizzare questo overload di nuovo.

Entrambi i casi è possibile usare in modo uniforme. In oltre esercitazioni è stata usata la seconda opzione, la creazione di più overload per il `ProductsBLL` classe s `UpdateProduct` (metodo).

## <a name="summary"></a>Riepilogo

Le funzionalità integrate di inserimento trovate nel controllo DetailsView e FormView non dispone di GridView, ma con un po' di impegno può essere aggiunta un'interfaccia di inserimento alla riga di piè di pagina. Per visualizzare la riga di piè di pagina in un controllo GridView è sufficiente impostare relativi `ShowFooter` proprietà `True`. Il contenuto della riga del piè di pagina può essere personalizzato per ogni campo convertendo il campo in un TemplateField e aggiungendo l'inserimento dell'interfaccia per il `FooterTemplate`. Come abbiamo visto in questa esercitazione il `FooterTemplate` può contenere i pulsanti, caselle di testo, controlli DropDownList, caselle di controllo, i controlli origine dati per il popolamento basato sui dati di controlli Web (ad esempio controlli DropDownList) e i controlli di convalida. Insieme a controlli per la raccolta di input dell'utente s, è necessario un pulsante Aggiungi, LinkButton o ImageButton.

Quando si fa clic sul pulsante Aggiungi, s ObjectDataSource `Insert()` metodo viene richiamato per iniziare l'inserimento del flusso di lavoro. ObjectDataSource chiamerà quindi il metodo insert configurato (il `ProductsBLL` classe s `AddProduct` metodo, in questa esercitazione). È necessario copiare i valori da s GridView inserimento dell'interfaccia per gli oggetti ObjectDataSource `InsertParameters` raccolta prima dell'inserimento metodo richiamato. Questa operazione può essere eseguita facendo riferimento a livello di codice i controlli Web interfaccia inserimento in s ObjectDataSource `Inserting` gestore dell'evento.

Questa esercitazione viene completata la sguardo alle tecniche per migliorare l'aspetto GridView. Il set successivo di esercitazioni verrà esaminate come lavorare con dati binari quali immagini, PDF, documenti di Word e così via e i controlli Web dei dati.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Bernadette Leigh. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](adding-a-gridview-column-of-checkboxes-vb.md)
