---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: Batch di inserimento (VB) | Documenti Microsoft
author: rick-anderson
description: "Informazioni su come inserire più record di database in un'unica operazione. Il Layer dell'interfaccia utente estende GridView per consentire all'utente di immettere più di n..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: e1e77dde4602350b18508bf5d71dbcd953f8961c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="batch-inserting-vb"></a>Batch di inserimento (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip) o [Scarica il PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> Informazioni su come inserire più record di database in un'unica operazione. Il Layer dell'interfaccia utente estende GridView per consentire all'utente di immettere più nuovi record. Nel livello di accesso ai dati si eseguono il wrapping di più operazioni di inserimento all'interno di una transazione per garantire che tutti gli inserimenti completata o viene eseguito il rollback di tutte le operazioni di inserimento.


## <a name="introduction"></a>Introduzione

Nel [l'aggiornamento in Batch](batch-updating-vb.md) esercitazione è stato esaminato personalizzazione del controllo GridView per presentare un'interfaccia in cui più record sono modificabili. L'utente visita la pagina potrebbe eseguire una serie di modifiche e quindi, con un singolo clic del pulsante, eseguire un aggiornamento batch. Per situazioni in cui gli utenti in genere aggiornano il numero di record in un'unica operazione, tale interfaccia consente di risparmiare innumerevoli clic e commutazioni di tastiera per mouse rispetto all'impostazione predefinita le funzionalità di modifica che sono state illustrate innanzitutto in per ogni riga di [un Panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) esercitazione.

Questo concetto può inoltre essere applicato durante l'aggiunta di record. Si supponga che in questo caso Northwind Traders comunemente riceviamo spedizioni fornitori che contengono un numero di prodotti per una categoria specifica. Ad esempio, potrebbe essere riceviamo una spedizione di sei diversi tè e prodotti Tokyo Traders. Se un utente immette i sei prodotti uno alla volta tramite un controllo DetailsView, sarà necessario scegliere molti degli stessi valori in modo continuativo: sarà necessario scegliere la stessa categoria (bibite), il fornitore stesso (Tokyo Traders), lo stesso sospeso (valore False) e le stesse unità nel valore dell'ordine (0). Questa voce di dati ripetitivi non solo molto tempo, ma è soggetta a errori.

Con un minimo sforzo è possibile creare un batch di inserimento di interfaccia che consente all'utente di scegliere il fornitore e la categoria una volta, immettere una serie di nomi di prodotto e prezzo unitario minore e quindi fare clic su un pulsante per aggiungere i nuovi prodotti nel database (vedere la figura 1). Poiché viene aggiunto ogni prodotto, il relativo `ProductName` e `UnitPrice` campi dati vengono assegnati i valori immessi nelle caselle di testo, mentre il `CategoryID` e `SupplierID` valori vengono assegnati i valori da controlli di DropDownList nel fo superiore, il modulo. Il `Discontinued` e `UnitsOnOrder` valori vengono impostati i valori hardcoded di `False` e 0, rispettivamente.


[![L'interfaccia di inserimento Batch](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**Figura 1**: l'interfaccia di inserimento Batch ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image3.png))


In questa esercitazione si creerà una pagina che implementa il batch inserimento interfaccia illustrato nella figura 1. Come con le esercitazioni di due precedenti, si eseguirà il wrapping gli inserimenti all'interno dell'ambito di una transazione per garantire l'atomicità. Let s iniziare!

## <a name="step-1-creating-the-display-interface"></a>Passaggio 1: Creazione dell'interfaccia di visualizzazione

In questa esercitazione sarà costituito da una singola pagina divisa in due aree: un'area di visualizzazione e un'area di inserimento. L'interfaccia di visualizzazione, che verrà creata in questo passaggio, Mostra i prodotti in un controllo GridView e include un pulsante denominato processo di spedizione del prodotto. Quando viene fatto clic su questo pulsante, l'interfaccia di visualizzazione viene sostituito con l'interfaccia di inserimento, come illustrato nella figura 1. L'interfaccia di visualizzazione restituisce dopo Aggiungi prodotti da spedizione o vengono selezionati i pulsanti Annulla. L'interfaccia di inserimento verrà creata nel passaggio 2.

Quando la creazione di una pagina che ha due interfacce, solo uno dei quali è visibile alla volta, ogni interfaccia in genere viene inserito all'interno di un [controllo Web Panel](http://www.w3schools.com/aspnet/control_panel.asp), che funge da contenitore per altri controlli. La pagina sarà pertanto presenti due controlli pannello uno per ogni interfaccia.

Aprire il `BatchInsert.aspx` nella pagina di `BatchData` cartelle e trascinare un pannello dalla casella degli strumenti nella finestra di progettazione (vedere la figura 2). Impostare il pannello s `ID` proprietà `DisplayInterface`. Quando si aggiunge il pannello alla finestra di progettazione, il relativo `Height` e `Width` proprietà vengono impostate su 50px e 125px, rispettivamente. Cancellare i valori delle proprietà dalla finestra delle proprietà.


[![Trascinare un pannello dalla casella degli strumenti nella finestra di progettazione](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**Figura 2**: trascinare un pannello dalla casella degli strumenti nella finestra di progettazione ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image6.png))


Successivamente, trascinare un controllo pulsante e GridView nel pannello. Impostare il pulsante s `ID` proprietà `ProcessShipment` e il relativo `Text` proprietà processo di spedizione del prodotto. Impostare i dispositivi di GridView `ID` proprietà `ProductsGrid` e dal suo smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per estrarre i dati di `ProductsBLL` classe s `GetProducts` metodo. Poiché questo GridView viene utilizzato solo per visualizzare i dati, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno). Fare clic su Fine per completare la configurazione guidata origine dati.


[![Visualizzare i dati restituiti dal metodo GetProducts ProductsBLL classe s](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**Figura 3**: visualizzare i dati restituiti dal `ProductsBLL` classe s `GetProducts` metodo ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image9.png))


[![Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno)](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**Figura 4**: impostare l'elenco a discesa sono elencati nelle schede DELETE, INSERT e UPDATE su (nessuno) ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image12.png))


Dopo aver completato la procedura guidata ObjectDataSource, Visual Studio aggiungerà BoundField e un CheckBoxField per i campi di dati di prodotto. Rimuovere tutto tranne il `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, e `Discontinued` campi. È possibile apportare le personalizzazioni estetiche. Ho deciso di formattare il `UnitPrice` campo come valore di valuta, riordinare i campi e molti dei campi rinominati `HeaderText` valori. Configurare inoltre GridView per includere il paging e supporto per l'ordinamento selezionando le caselle di controllo Abilita Paging e Abilita ordinamento nello smart tag GridView s.

Dopo aver aggiunto i controlli pannello, pulsante, GridView e ObjectDataSource e personalizzare i campi s GridView, markup dichiarativo pagina s dovrebbe essere simile al seguente:


[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

Si noti che il markup per il pulsante e GridView visualizzati all'interno di apertura e chiusura `<asp:Panel>` tag. Poiché questi controlli sono all'interno di `DisplayInterface` pannello, è possibile nasconderle impostando semplicemente il pannello s `Visible` proprietà `False`. Passaggio 3 esamina la modifica a livello di codice il pannello s `Visible` fare clic su proprietà in risposta a un pulsante per visualizzare un'interfaccia e l'altro nasconderla.

Richiedere qualche istante per visualizzare l'avanzamento attraverso un browser. Come illustrato nella figura 5, si verrà visualizzato un pulsante di spedizione del prodotto processo di sopra di un controllo GridView in cui sono elencati i prodotti dieci alla volta.


[![GridView sono elencati i prodotti e offre l'ordinamento e Paging funzionalità](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**Figura 5**: GridView sono elencati i prodotti e offre l'ordinamento e Paging funzionalità ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Passaggio 2: Creazione dell'interfaccia di inserimento

Con l'interfaccia di visualizzazione completa, è nuovamente pronto per creare l'inserimento di interfaccia. Per questa esercitazione, lasciare s creare un'interfaccia di inserimento che richiede un singolo valore di categoria e fornitore e quindi consente all'utente di immettere fino a cinque nomi di prodotto e i valori di unità di prezzo. Con questa interfaccia, l'utente può aggiungere un massimo di cinque nuovi prodotti che condividono la stessa categoria e il fornitore, ma di prezzi e i nomi di prodotto univoco.

Avvio mediante il trascinamento di un pannello dalla casella degli strumenti nella finestra di progettazione, posizionarlo sotto esistente `DisplayInterface` pannello. Impostare il `ID` Pannello di aggiunta di proprietà di questo `InsertingInterface` e impostare il relativo `Visible` proprietà `False`. Verrà aggiunto codice che imposta il `InsertingInterface` pannello s `Visible` proprietà `True` nel passaggio 3. Cancellare anche il pannello s `Height` e `Width` i valori delle proprietà.

Successivamente, è necessario creare l'interfaccia di inserimento che è stato illustrato nella figura 1. Questa interfaccia può essere creata tramite varie tecniche HTML, ma verrà utilizzata una semplice: una tabella di sette righe e quattro colonne.

> [!NOTE]
> Quando si immettono markup per HTML `<table>` elementi, è preferibile utilizzare la vista di origine. Mentre in Visual Studio sono strumenti per l'aggiunta di `<table>` elementi tramite la finestra di progettazione, sembra tutti troppo disposto a inserire la finestra di progettazione non richiesto non desiderato per `style` impostazioni nel markup. Dopo aver creato il `<table>` markup, in genere visualizzare nella finestra di progettazione per aggiungere i controlli Web e impostare le relative proprietà. Durante la creazione di tabelle con colonne predeterminate e righe preferisce utilizzare HTML statico invece di [controllo Web Table](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.table.aspx) perché tutti i controlli Web inseriti all'interno di un controllo tabella Web è possibile accedere solo tramite il `FindControl("controlID")` modello. , Tuttavia, usare i controlli Web di tabella per le tabelle di dimensioni in modo dinamico (quelli il cui righe o colonne si basano su alcuni database o i criteri specificati dall'utente), dal Web tabella controllo può essere costruito a livello di codice.


Immettere il seguente codice all'interno di `<asp:Panel>` tag del `InsertingInterface` pannello:


[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

Questo `<table>` markup non include tutti i controlli Web ancora, aggiungeremo quelli temporaneamente. Si noti che ogni `<tr>` elemento contiene una particolare impostazione classe CSS: `BatchInsertHeaderRow` per la riga di intestazione in cui verrà salvato il fornitore e la categoria controlli DropDownList; `BatchInsertFooterRow` per la riga di piè di pagina in cui verrà salvato; Aggiungi prodotti di spedizione e pulsanti Annulla e alternati `BatchInsertRow` e `BatchInsertAlternatingRow` i valori per le righe contenenti il prodotto e l'unità di prezzo controlli casella di testo. Ho creato classi CSS corrispondenti di `Styles.css` file per fornire l'interfaccia di inserimento di un aspetto simile a GridView e DetailsView controlla si va utilizzata in queste esercitazioni. Queste classi CSS è illustrate di seguito.


[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

Con questo markup immesso, tornare alla visualizzazione progettazione. Questo `<table>` dovrà essere visualizzato in una tabella di sette righe e quattro colonne nella finestra di progettazione, come illustrato nella figura 6.


[![L'interfaccia di inserimento è costituito da quattro-di una colonna tabella sette righe](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**Figura 6**: l'interfaccia di inserimento è costituito da quattro-di una colonna tabella sette righe ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image18.png))


È re ora pronti per aggiungere i controlli Web per l'interfaccia di inserimento. Trascinare due controlli DropDownList dalla casella degli strumenti nelle celle della tabella per il fornitore e uno per la categoria appropriate.

Impostare il fornitore DropDownList s `ID` proprietà `Suppliers` e associarlo a un nuovo oggetto ObjectDataSource denominato `SuppliersDataSource`. Configura il nuovo ObjectDataSource per recuperare i dati di `SuppliersBLL` classe s `GetSuppliers` metodo e impostare l'aggiornamento scheda elenco a discesa s su (nessuno). Fare clic su Fine per completare la procedura guidata.


[![Configurare ObjectDataSource per utilizzare il metodo di classe SuppliersBLL s GetSuppliers](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**Figura 7**: configurare ObjectDataSource per utilizzare il `SuppliersBLL` classe s `GetSuppliers` metodo ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image21.png))


Dispone il `Suppliers` DropDownList visualizzazione il `CompanyName` campo dati e utilizzare il `SupplierID` campo dati come relativo `ListItem` valori s.


[![Visualizzare il campo dei dati di CompanyName e utilizzare SupplierID come valore](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**Figura 8**: visualizzazione di `CompanyName` campo dati e utilizzare `SupplierID` come valore ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image24.png))


Nome DropDownList secondo `Categories` e associarlo a un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource`. Configurare il `CategoriesDataSource` ObjectDataSource per utilizzare il `CategoriesBLL` classe s `GetCategories` (metodo); impostare l'elenco a discesa sono elencati in schede UPDATE e DELETE su (nessuno) e fare clic su Fine per completare la procedura guidata. Infine, avere la visualizzazione DropDownList il `CategoryName` campo dati e utilizzare il `CategoryID` come valore.

Dopo questi due controlli DropDownList aggiunto e associato a ObjectDataSources configurato in modo appropriato, la schermata dovrebbe essere simile alla figura 9.


[![La riga di intestazione contiene ora i fornitori e i controlli DropDownList categorie](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**Figura 9**: l'intestazione di riga contiene ora il `Suppliers` e `Categories` controlli DropDownList ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image27.png))


È ora necessario creare caselle di testo per raccogliere il nome e il prezzo per ogni nuovo prodotto. Trascinare un controllo casella di testo dalla casella degli strumenti nella finestra di progettazione per ognuna delle righe di nome e il prezzo del cinque prodotto. Impostare il `ID` le proprietà delle caselle di testo per `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`e così via.

Aggiungere un controllo CompareValidator dopo ogni del prezzo unitario nelle caselle di testo, l'impostazione di `ControlToValidate` proprietà appropriati `ID`. Impostare inoltre il `Operator` proprietà `GreaterThanEqual`, `ValueToCompare` su 0, e `Type` per `Currency`. Queste impostazioni indicano CompareValidator per assicurarsi che il prezzo, se specificato, è un valore di valuta validi che è maggiore o uguale a zero. Impostare il `Text` proprietà \*, e `ErrorMessage` al prezzo deve essere maggiore o uguale a zero. Inoltre, per omettere i simboli di valuta.

> [!NOTE]
> L'inserimento di interfaccia non include tutti i controlli RequiredFieldValidator, anche se il `ProductName` campo il `Products` non consente la tabella di database `NULL` valori. In questo modo si desidera consentire all'utente di immettere fino a cinque prodotti. Ad esempio, se l'utente per fornire il prezzo del prodotto, nome e l'unità per le prime tre righe, lasciando vuota, le ultime due righe d è sufficiente aggiungere tre nuovi prodotti al sistema. Poiché `ProductName` è obbligatorio, tuttavia, è necessario controllare a livello di codice per assicurarsi che se un prezzo unitario viene immesso viene fornito un valore di nome di prodotto corrispondente. È possibile affrontare questo controllo nel passaggio 4.


Quando si convalida l'input dell'utente s, il controllo CompareValidator report dati non valido se il valore contiene un simbolo di valuta. Aggiungere un $ all'inizio di ogni del prezzo unitario nelle caselle di testo come un segnale visivo che indica all'utente di omettere il simbolo di valuta, quando si immette il prezzo.

Infine, aggiungere un controllo ValidationSummary all'interno di `InsertingInterface` pannello impostazioni relativo `ShowMessageBox` proprietà `True` e il relativo `ShowSummary` proprietà `False`. Con queste impostazioni, se l'utente immette un valore del prezzo unitario non valido, verrà visualizzato un asterisco accanto i controlli casella di testo contenente l'errore e il controllo ValidationSummary visualizzerà una finestra di messaggio sul lato client che verrà visualizzato il messaggio di errore che è specificati in precedenza.

A questo punto, la schermata dovrebbe essere simile alla figura 10.


[![L'interfaccia inserimento ora include caselle di testo per i prodotti di nomi e i prezzi](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**Figura 10**: l'inserimento di interfaccia ora include caselle di testo per i nomi di prodotti e i prezzi ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image30.png))


Successivamente è necessario aggiungere Aggiungi prodotti dai pulsanti Annulla e di spedizione per la riga di piè di pagina. Trascinare due controlli pulsante dalla casella degli strumenti nel piè di pagina dell'interfaccia, inserire i pulsanti di impostazione `ID` proprietà `AddProducts` e `CancelButton` e `Text` proprietà per aggiungere i prodotti di spedizione e Annulla, rispettivamente. Inoltre, impostare il `CancelButton` controllo s `CausesValidation` proprietà `false`.

Infine, è necessario aggiungere un controllo etichetta Web che consente di visualizzare i messaggi di stato per le due interfacce. Ad esempio, quando un utente aggiunge correttamente una nuova spedizione dei prodotti, si desidera ripristinare l'interfaccia di visualizzazione e visualizzare un messaggio di conferma. Se, tuttavia, l'utente fornisce un prezzo per un nuovo prodotto, ma lascia disattivare il nome del prodotto, è necessario visualizzare un messaggio di avviso dopo il `ProductName` campo è obbligatorio. Poiché è necessario il messaggio da visualizzare per entrambe le interfacce, è possibile inserirlo nella parte superiore della pagina di fuori di pannelli.

Trascinare un controllo etichetta Web dalla casella degli strumenti nella parte superiore della pagina nella finestra di progettazione. Impostare il `ID` proprietà `StatusLabel`, deselezionare le il `Text` proprietà e impostare il `Visible` e `EnableViewState` proprietà `False`. Come abbiamo visto nelle esercitazioni precedenti, l'impostazione di `EnableViewState` proprietà `False` consente di modificare i valori di proprietà etichetta s a livello di codice e farli automaticamente ripristinare i valori predefiniti con il postback successivi. Questa operazione semplifica il codice per la visualizzazione di un messaggio di stato in risposta a un'operazione dell'utente che non viene più visualizzata sul postback successivi. Infine, impostare il `StatusLabel` controllo s `CssClass` proprietà avviso, ovvero il nome di una classe CSS definita in `Styles.css` che visualizza il testo in caratteri grandi, corsivo, grassetto e rosso.

Figura 11 mostra la progettazione di Visual Studio dopo l'etichetta è stato aggiunta e configurata.


[![Posizionare il controllo StatusLabel sopra i due controlli pannello](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**Figura 11**: sul posto di `StatusLabel` controllo di sopra di due controlli pannello ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Passaggio 3: Il passaggio tra la visualizzazione e l'inserimento di interfacce

A questo punto è stato completato il markup per la visualizzazione e l'inserimento di interfacce, ma è re rimasti con due attività:

- Il passaggio tra la visualizzazione e l'inserimento di interfacce
- Aggiunta di prodotti nella spedizione al database

Attualmente, l'interfaccia di visualizzazione è visibile, ma l'interfaccia di inserimento è nascosta. In questo modo il `DisplayInterface` pannello s `Visible` è impostata su `True` (valore predefinito), mentre il `InsertingInterface` pannello s `Visible` è impostata su `False`. Per passare tra le due interfacce, è sufficiente passare ogni controllo s `Visible` valore della proprietà.

È necessario passare dall'interfaccia di visualizzazione per l'interfaccia di inserimento quando viene scelto il pulsante di spedizione del prodotto processo. Pertanto, creare un gestore eventi per questo pulsante s `Click` evento che contiene il codice seguente:


[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

Questo codice nasconde semplicemente il `DisplayInterface` pannello e Mostra il `InsertingInterface` pannello.

Successivamente, è possibile creare gestori eventi per i prodotti aggiungere dai controlli di spedizione e pulsante Annulla nell'interfaccia di inserimento. Quando viene scelto uno di questi pulsanti, è necessario ripristinare l'interfaccia di visualizzazione. Creare `Click` gestori eventi per i controlli pulsante in modo che chiamano `ReturnToDisplayInterface`, un metodo che verrà aggiunta solo temporaneamente. Oltre a nascondere la `InsertingInterface` pannello e visualizzare il `DisplayInterface` Pannello di `ReturnToDisplayInterface` metodo deve restituire i controlli Web allo stato di pre-modifica. Ciò comporta l'impostazione di controlli DropDownList `SelectedIndex` proprietà su 0 e cancellare il `Text` proprietà dei controlli casella di testo.

> [!NOTE]
> Si consideri cosa potrebbe accadere se si non riportare i controlli pre-modifica lo stato prima di restituire l'interfaccia di visualizzazione. Un utente può fare clic sul pulsante di spedizione del prodotto processo, immettere i prodotti dalla spedizione e quindi fare clic su Aggiungi prodotti da spedizione. Questo aggiungere i prodotti e verrà restituito all'utente l'interfaccia di visualizzazione. A questo punto l'utente potrebbe voler aggiungere un'altra spedizione. Fare clic sul pulsante di spedizione del prodotto processo che restituiscono l'interfaccia inserimento ma DropDownList su valori di casella di testo e le selezioni verrebbero ancora popolati con i valori precedenti.


[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

Entrambi `Click` gestori eventi è sufficiente chiamare il `ReturnToDisplayInterface` (metodo), anche se sarà restituiamo ai prodotti aggiungere da spedizione `Click` gestore dell'evento nel passaggio 4 e aggiungere il codice per salvare i prodotti. `ReturnToDisplayInterface`viene avviata restituendo il `Suppliers` e `Categories` controlli DropDownList per le prime opzioni. Le due costanti `firstControlID` e `lastControlID` contrassegnare l'inizio e fine utilizzati nei nomi di prodotto nome e il prezzo unitario nelle caselle di testo in cui l'inserimento di interfaccia e vengono utilizzati i limiti di valori di indice di controllo di `For` ciclo che imposta il `Text`proprietà dei controlli casella di testo nuovo in una stringa vuota. Infine, i pannelli `Visible` le proprietà vengono reimpostate in modo che l'interfaccia di inserimento è nascosta e l'interfaccia di visualizzazione illustrato.

È opportuno testare questa pagina in un browser. Durante la prima visita la pagina verrà visualizzato l'interfaccia di visualizzazione come è stato illustrato nella figura 5. Fare clic sul pulsante elabora spedizione del prodotto. La pagina verrà postback e dovrebbe essere l'interfaccia inserimento come illustrato nella figura 12. Fare clic su entrambi i prodotti aggiungere pulsanti di spedizione o annullare nuovamente l'interfaccia di visualizzazione.

> [!NOTE]
> Durante la visualizzazione dell'interfaccia di inserimento, è opportuno testare il CompareValidators sul prezzo unitario nelle caselle di testo. Verrà visualizzata una finestra di messaggio sul lato client di avviso quando si fa clic Aggiungi prodotti dal pulsante di spedizione con valori di valuta non valido o prezzi con un valore minore di zero.


[![L'interfaccia di inserimento viene visualizzato dopo aver fatto clic sul pulsante di spedizione del prodotto di processo](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**Figura 12**: l'interfaccia di inserimento viene visualizzato dopo aver fatto clic sul pulsante di spedizione del prodotto di processo ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Passaggio 4: Aggiunta di prodotti

All rimanente per questa esercitazione è possibile salvare i prodotti nel database in Aggiungi prodotti dal pulsante spedizione s `Click` gestore dell'evento. Questa operazione può essere eseguita mediante la creazione di un `ProductsDataTable` e l'aggiunta di un `ProductsRow` istanza per ciascuno dei nomi di prodotto specificati. Una volta questi `ProductsRow` sia stata aggiunta si effettua una chiamata al `ProductsBLL` classe s `UpdateWithTransaction` metodo passando la `ProductsDataTable`. Tenere presente che il `UpdateWithTransaction` metodo, che è stato creato nel [di wrapping delle modifiche del Database all'interno di una transazione](wrapping-database-modifications-within-a-transaction-vb.md) tutorial, passa il `ProductsDataTable` per il `ProductsTableAdapter` s `UpdateWithTransaction` metodo. Da qui, viene avviata una transazione di ADO.NET e i problemi di TableAdatper un `INSERT` istruzione per il database per ogni aggiunta `ProductsRow` nel DataTable. Presupponendo che tutti i prodotti vengono aggiunti senza errori, che viene eseguito il commit della transazione, in caso contrario viene eseguito il rollback.

Il codice per i prodotti aggiungere dal pulsante spedizione s `Click` gestore dell'evento deve anche eseguire operazioni di controllo degli errori. Poiché non esistono alcun RequiredFieldValidators utilizzato nell'interfaccia di inserimento, un utente può immettere un prezzo per un prodotto omettendo il relativo nome. Poiché il nome del prodotto s è richiesto, se tale condizione espande è necessario avvisare l'utente e non procedere con le operazioni di inserimento. L'intero `Click` codice del gestore eventi segue:


[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

Il gestore dell'evento inizia assicurando che il `Page.IsValid` proprietà restituisce un valore di `True`. Se restituisce `False`, quindi che significa che uno o più di CompareValidators segnalano i dati non validi; in tal caso non si desidera tenta di inserire i prodotti immessi o si otterrà un'eccezione durante il tentativo di assegnare il prezzo unitario immesso dall'utente valore di `ProductsRow` s `UnitPrice` proprietà.

Successivamente, un nuovo `ProductsDataTable` viene creata l'istanza (`products`). Oggetto `For` ciclo viene utilizzato per scorrere il nome e l'unità di prezzo del prodotto nelle caselle di testo e `Text` proprietà vengono lette in variabili locali `productName` e `unitPrice`. Se l'utente ha immesso un valore per il prezzo unitario ma non per il nome del prodotto corrispondente, il `StatusLabel` consente di visualizzare il messaggio se si specifica un'unità di prezzo è inoltre necessario includere il nome del prodotto e il gestore dell'evento è stato terminato.

Se è stato specificato un nome di prodotto, un nuovo `ProductsRow` istanza viene creata utilizzando il `ProductsDataTable` s `NewProductsRow` metodo. Questa nuova `ProductsRow` istanza s `ProductName` proprietà è impostata per il prodotto corrente Nome casella di testo durante il `SupplierID` e `CategoryID` vengono assegnate le proprietà di `SelectedValue` le proprietà di controlli di DropDownList nell'intestazione di interfaccia s inserimento. Se viene immesso un valore per il prezzo del prodotto s, viene assegnato al `ProductsRow` istanza s `UnitPrice` proprietà; in caso contrario, la proprietà è a sinistra non assegnato, provocando un `NULL` valore per `UnitPrice` nel database. Infine, il `Discontinued` e `UnitsOnOrder` proprietà assegnate ai valori hardcoded `False` e 0, rispettivamente.

Dopo che le proprietà sono state assegnate al `ProductsRow` aggiunto all'istanza di `ProductsDataTable`.

Dopo aver completato il `For` ciclo, controlliamo se sono stati aggiunti tutti i prodotti. L'utente può, dopo tutto, hai fatto clic Aggiungi prodotti da spedizione prima di immettere i nomi dei prodotti o i prezzi. Se è presente almeno un prodotto di `ProductsDataTable`, `ProductsBLL` classe s `UpdateWithTransaction` metodo viene chiamato. Successivamente, i dati vengano riassociati al `ProductsGrid` GridView in modo che i prodotti appena aggiunti verranno visualizzato nell'interfaccia di visualizzazione. Il `StatusLabel` viene aggiornato per visualizzare un messaggio di conferma e `ReturnToDisplayInterface` viene richiamato, nascondere l'interfaccia di inserimento e che mostra l'interfaccia di visualizzazione.

Se non viene immesso alcun prodotti, l'interfaccia di inserimento è visualizzato ma il messaggio di che prodotti non sono stati aggiunti. Immettere i nomi di prodotto e prezzo unitario minore nelle caselle di testo viene visualizzato.

S figura 13, 14 e 15 Visualizza l'inserimento e visualizza le interfacce in azione. Nella figura 13, l'utente ha immesso un valore di prezzo unitario senza un nome di prodotto corrispondente. Nella figura 14 mostra l'interfaccia di visualizzazione dopo tre nuovi prodotti sono stati aggiunti correttamente, mentre nella figura 15 vengono illustrati due dei prodotti appena aggiunti in GridView (il terzo è nella pagina precedente).


[![Nome di un prodotto è richiesto quando immettendo un prezzo unitario](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**Figura 13**: un nome di prodotto è richiesto quando immettendo un prezzo unitario ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image39.png))


[![Sono stati aggiunti tre Veggies nuovi per il fornitore Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**Nella figura 14**: tre nuovi Veggies sono stati aggiunti per s Mayumi fornitore ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image42.png))


[![I nuovi prodotti disponibili nell'ultima pagina del controllo GridView.](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**Figura 15**: il nuovo prodotti sono disponibili nell'ultima pagina di GridView ([fare clic per visualizzare l'immagine ingrandita](batch-inserting-vb/_static/image45.png))


> [!NOTE]
> Il batch di inserimento di logica utilizzata in questa esercitazione esegue il wrapping le operazioni di inserimento all'interno dell'ambito della transazione. Per verificarlo, introdurre intenzionalmente un errore a livello di database. Ad esempio, invece di assegnare il nuovo `ProductsRow` istanza s `CategoryID` proprietà al valore selezionato nel `Categories` DropDownList, assegnare un valore come `i * 5`. Qui `i` è l'indicizzatore di ciclo e con valori compresi tra 1 e 5. Pertanto, quando aggiunta del prodotto prima di inserire due o più prodotti in batch avrà un valore valido `CategoryID` valore (5), ma i prodotti successive avranno `CategoryID` valori che non corrispondono a un massimo di `CategoryID` i valori di `Categories` tabella. L'effetto finale è che, mentre il primo `INSERT` avrà esito positivo, le successive avranno esito negativo con una violazione di vincolo di chiave esterna. Poiché l'inserimento batch è atomico, il primo `INSERT` sarà possibile eseguire il rollback, restituendo il database allo stato prima che il batch inserimento processo iniziato.


## <a name="summary"></a>Riepilogo

Su questa e le esercitazioni precedenti due interfacce che consentono l'aggiornamento, eliminazione, abbiamo creato e l'inserimento di batch di dati, ognuno dei quali usato il supporto delle transazioni è stato aggiunto a livello di accesso ai dati nel [di wrapping delle modifiche del Database all'interno di una transazione](wrapping-database-modifications-within-a-transaction-vb.md) esercitazione. Per alcuni scenari, tali interfacce utente per l'elaborazione batch migliorare notevolmente l'efficienza degli utenti finali riducendo il numero di clic, i postback e commutazioni di contesto di tastiera per mouse, pur mantenendo l'integrità dei dati sottostanti.

In questa esercitazione completa esaminare l'utilizzo dei dati in batch. Il set successivo di esercitazioni viene esaminata un'ampia gamma di scenari avanzati di livello di accesso ai dati, incluso l'utilizzo di stored procedure nei metodi TableAdapter s, configurazione delle impostazioni del livello di connessione e comando DAL, la crittografia delle stringhe di connessione e altro ancora!

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Portare i revisori per questa esercitazione sono stati Hilton Giesenow e S ren Jacob Lauritsen. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](batch-deleting-vb.md)
