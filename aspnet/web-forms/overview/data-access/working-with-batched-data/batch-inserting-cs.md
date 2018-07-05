---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
title: Batch di inserimento (c#) | Microsoft Docs
author: rick-anderson
description: Informazioni su come inserire più record di database in un'unica operazione. Il Layer dell'interfaccia utente estende il controllo GridView per consentire all'utente di immettere più di n...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: cf025e08-48fc-4385-b176-8610aa7b5565
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-cs
msc.type: authoredcontent
ms.openlocfilehash: 9979a991935d97ef7c3b2ac62666da318b95d063
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829059"
---
<a name="batch-inserting-c"></a>Batch di inserimento (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_CS.zip) o [Scarica il PDF](batch-inserting-cs/_static/datatutorial66cs1.pdf)

> Informazioni su come inserire più record di database in un'unica operazione. Il Layer dell'interfaccia utente estende il controllo GridView per consentire all'utente di immettere più nuovi record. Nel livello di accesso ai dati è eseguire il wrapping di più operazioni di inserimento all'interno di una transazione per garantire che tutti gli inserimenti di esito positivo o vengano eseguito il rollback di tutti gli inserimenti.


## <a name="introduction"></a>Introduzione

Nel [l'aggiornamento Batch](batch-updating-cs.md) esercitazione è stata esaminata la personalizzazione del controllo GridView per presentare un'interfaccia in cui è stato modificabile più record. L'utente visitando la pagina è stato possibile eseguire una serie di modifiche e quindi, con un singolo clic del pulsante, eseguire un aggiornamento batch. Per situazioni in cui gli utenti in genere aggiornano numero di record in un'unica operazione, tale interfaccia consente di risparmiare moltissime clic e commutazioni di contesto della tastiera-a-del mouse rispetto all'impostazione predefinita le funzionalità di modifica che prima sono state esplorate in ogni riga di [un Panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) esercitazione.

Questo concetto può essere applicato anche quando si aggiungono i record. Si supponga che in questo caso di Northwind Traders è comunemente ricevere le spedizioni di fornitori che contengono un numero di prodotti per una determinata categoria. Ad esempio, potrebbe essere riceviamo una spedizione di sei tè diverso e i prodotti di caffè Traders Tokyo. Se un utente immette le sei prodotti uno alla volta tramite un controllo DetailsView, dovrà scegliere molti degli stessi valori in modo continuativo: sarà necessario scegliere la categoria stessa (bevande), il fornitore stesso (Tokyo Traders), lo stesso non più disponibili (valore False) e le stesse unità sul valore dell'ordine (0). Questa voce dati ripetitivi non solo può richiedere molto tempo, ma è soggetta a errori.

Con qualche operazione è possibile creare un batch di inserimento dell'interfaccia che consente all'utente di scegliere il fornitore e la categoria una sola volta, immettere una serie di nomi di prodotto e prezzo unitario minore e quindi fare clic su un pulsante per aggiungere i nuovi prodotti per il database (vedere la figura 1). Poiché viene aggiunto ciascun prodotto, relativi `ProductName` e `UnitPrice` i campi dati vengono assegnati i valori immessi nelle caselle di testo, mentre relativo `CategoryID` e `SupplierID` valori vengono assegnati i valori dai controlli DropDownList nel fo superiore il form. Il `Discontinued` e `UnitsOnOrder` valori vengono impostati sui valori impostati come hardcoded di `false` e 0, rispettivamente.


[![L'interfaccia di inserimento Batch](batch-inserting-cs/_static/image2.png)](batch-inserting-cs/_static/image1.png)

**Figura 1**: l'interfaccia di inserimento Batch ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image3.png))


In questa esercitazione si creerà una pagina che implementa il batch di inserimento interfaccia illustrata nella figura 1. Come con le due esercitazioni precedenti, si eseguirà il wrapping di operazioni di inserimento all'interno dell'ambito di una transazione per garantire l'atomicità. Introduzione a ti permettono di s.

## <a name="step-1-creating-the-display-interface"></a>Passaggio 1: Creazione dell'interfaccia di visualizzazione

Questa esercitazione sarà costituito da una singola pagina che viene suddiviso in due aree: un'area di visualizzazione e un'area di inserimento. L'interfaccia di visualizzazione, che verrà creato in questo passaggio, vengono indicati i prodotti in un controllo GridView e include un pulsante denominato processo di spedizione del prodotto. Quando si seleziona questo pulsante, l'interfaccia di visualizzazione viene sostituito con l'interfaccia di inserimento, come illustrato nella figura 1. Restituisce l'interfaccia di visualizzazione dopo i prodotti aggiungere dalla spedizione o si fa clic sul pulsanti Annulla. L'interfaccia di inserimento verrà creato nel passaggio 2.

Durante la creazione di una pagina che ha due interfacce, solo uno dei quali è visibile alla volta, ogni interfaccia viene generalmente inserito all'interno di un [controllo pannello Web](http://www.w3schools.com/aspnet/control_panel.asp), che funge da contenitore per altri controlli. Pertanto, la pagina avrà due controlli di pannello uno per ogni interfaccia.

Iniziare aprendo il `BatchInsert.aspx` nella pagina di `BatchData` cartelle e trascinare un pannello dalla casella degli strumenti nella finestra di progettazione (vedere la figura 2). Impostare il pannello s `ID` proprietà `DisplayInterface`. Quando si aggiunge il pannello per la finestra di progettazione, relativi `Height` e `Width` proprietà vengono impostate su 50 px e 125px, rispettivamente. Cancellare le valori delle proprietà dalla finestra Proprietà.


[![Trascinare un pannello dalla casella degli strumenti nella finestra di progettazione](batch-inserting-cs/_static/image5.png)](batch-inserting-cs/_static/image4.png)

**Figura 2**: trascinare un pannello dalla casella degli strumenti nella finestra di progettazione ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image6.png))


Successivamente, trascinare un controllo pulsante e GridView nella casella di gruppo. Impostare il pulsante s `ID` proprietà `ProcessShipment` e il relativo `Text` proprietà processo di spedizione del prodotto. Impostare la s GridView `ID` proprietà `ProductsGrid` e, dal suo smart tag, associarlo a un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per estrarre i dati dal `ProductsBLL` classe s `GetProducts` (metodo). Poiché questo controllo GridView è utilizzato solo per visualizzare i dati, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno). Fare clic su Fine per completare la procedura guidata Configura origine dati.


[![Visualizzare i dati restituiti dal metodo GetProducts ProductsBLL classe s](batch-inserting-cs/_static/image8.png)](batch-inserting-cs/_static/image7.png)

**Figura 3**: visualizzare i dati restituiti dai `ProductsBLL` classe s `GetProducts` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image9.png))


[![Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno)](batch-inserting-cs/_static/image11.png)](batch-inserting-cs/_static/image10.png)

**Figura 4**: impostare l'elenco a discesa sono elencati nell'aggiornamento, inserimento ed eliminare schede su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image12.png))


Dopo aver completato la procedura guidata ObjectDataSource, Visual Studio aggiungerà BoundField e un CampoCasellaDiControllo per i campi di dati del prodotto. Rimuovi tutto tranne le `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, e `Discontinued` campi. È possibile apportare le eventuali personalizzazioni estetiche. Ho deciso di formattare la `UnitPrice` campo come valore di valuta, riordinare i campi e rinominato vari campi `HeaderText` valori. Configurare anche il controllo GridView per includere il paging e ordinamento supporto selezionando le caselle di controllo Attiva Paging e abilitare l'ordinamento nello smart tag s GridView.

Dopo aver aggiunto i controlli di pannello, pulsante, GridView e ObjectDataSource e personalizzare i campi s GridView, il markup dichiarativo pagina s dovrebbe essere simile al seguente:


[!code-aspx[Main](batch-inserting-cs/samples/sample1.aspx)]

Si noti che il markup per il pulsante e GridView visualizzate all'interno di apertura e chiusura `<asp:Panel>` tag. Poiché questi controlli si trovano all'interno di `DisplayInterface` pannello, è possibile nasconderle impostando semplicemente il pannello s `Visible` proprietà `false`. Passaggio 3 illustra a livello di codice modifica il pannello s `Visible` fare clic su proprietà in risposta a un pulsante per visualizzare un'interfaccia, nascondendo l'altro.

Si consiglia di visualizzare lo stato di avanzamento tramite un browser. Come illustrato nella figura 5, verrà visualizzato un pulsante di spedizione del prodotto processo sopra un controllo GridView in cui sono elencati i prodotti dieci alla volta.


[![Il controllo GridView sono elencati i prodotti e offre l'ordinamento e Paging funzionalità](batch-inserting-cs/_static/image14.png)](batch-inserting-cs/_static/image13.png)

**Figura 5**: il controllo GridView sono elencati i prodotti e offre l'ordinamento e la funzionalità di Paging ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Passaggio 2: Creazione dell'interfaccia di inserimento

Con l'interfaccia di visualizzazione completa, sono pronti per creare l'inserimento di interfaccia. Per questa esercitazione, lasciare s creare un'interfaccia di inserimento che richiede un singolo valore di fornitore e la categoria e quindi consente all'utente di immettere fino a cinque nomi di prodotto e i valori di unità di prezzo. Con questa interfaccia, l'utente può aggiungere uno a cinque nuovi prodotti che condividono la stessa categoria e il fornitore, ma i prezzi e i nomi di prodotto univoche.

Inizio trascinando un riquadro dalla casella degli strumenti nella finestra di progettazione, posizionandolo sotto l'oggetto esistente `DisplayInterface` pannello. Impostare il `ID` pannello da appena aggiunta di proprietà di questo `InsertingInterface` e impostare relativo `Visible` proprietà `false`. Si aggiungerà codice che imposta la `InsertingInterface` pannello s `Visible` proprietà `true` nel passaggio 3. Anche cancellare il pannello 1!s `Height` e `Width` i valori delle proprietà.

Successivamente, è necessario creare l'interfaccia di inserimento che è stato illustrato nella figura 1. Questa interfaccia può essere creata tramite una serie di tecniche HTML, ma si userà uno piuttosto semplice: una tabella con sette righe e quattro colonne.

> [!NOTE]
> Durante l'immissione di markup HTML `<table>` elementi, è preferibile utilizzare la vista dell'origine. Anche se Visual Studio dispone di strumenti per l'aggiunta `<table>` elementi tramite la finestra di progettazione, la finestra di progettazione sembra tutto troppo disposto a inserire non richiesto non desiderato per `style` impostazioni nel markup. Dopo aver creato il `<table>` markup, è in genere possibile tornare alla finestra di progettazione per aggiungere i controlli Web e impostare le relative proprietà. Durante la creazione di tabelle con righe e colonne predeterminate preferisco utilizzare il codice HTML statico invece [controllo tabella Web](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) perché i controlli Web posizionati all'interno di un controllo tabella Web sono accessibile solo tramite il `FindControl("controlID")` pattern. , Tuttavia, usare i controlli Web di tabella per le tabelle di dimensioni in modo dinamico (quelli cui righe o colonne basate su alcuni database o i criteri specificati dall'utente), da tabella Web controllo può essere creato a livello di codice.


Immettere il seguente codice all'interno di `<asp:Panel>` tag del `InsertingInterface` pannello:


[!code-html[Main](batch-inserting-cs/samples/sample2.html)]

Ciò `<table>` markup non include tutti i controlli Web ancora, aggiungiamo, quindi quelli momentaneamente. Si noti che ogni `<tr>` elemento contiene una particolare impostazione classe CSS: `BatchInsertHeaderRow` per la riga di intestazione in cui verrà salvato il fornitore e la categoria controlli DropDownList; `BatchInsertFooterRow` per la riga di piè di pagina in cui i prodotti aggiungere da pulsanti Annulla e spedizione passerà; e alterne `BatchInsertRow` e `BatchInsertAlternatingRow` valori per le righe che conterranno il prodotto e unit price controlli casella di testo. Ho creato classi CSS corrispondenti nel `Styles.css` file per dare un aspetto simile a GridView e DetailsView di interfaccia di inserimento controlla si va usato in queste esercitazioni. Queste classi CSS vengono visualizzate sotto.


[!code-css[Main](batch-inserting-cs/samples/sample3.css)]

Con questo tag inseriti, tornare alla visualizzazione progettazione. Ciò `<table>` dovrà essere visualizzato in una tabella di quattro colonne, righe e sette nella finestra di progettazione, come illustrato nella figura 6.


[![L'interfaccia di inserimento è costituito da quattro-di una colonna nella tabella sette righe](batch-inserting-cs/_static/image17.png)](batch-inserting-cs/_static/image16.png)

**Figura 6**: l'interfaccia di inserimento è costituito da quattro-di una colonna nella tabella sette righe ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image18.png))


È nuovamente ora pronti per aggiungere i controlli Web per l'interfaccia di inserimento. Trascinare due controlli DropDownList dalla casella degli strumenti nelle celle della tabella per il fornitore e uno per la categoria appropriate.

Impostare il fornitore s DropDownList `ID` proprietà `Suppliers` e associarlo a un nuovo oggetto ObjectDataSource denominato `SuppliersDataSource`. Configurare il nuovo oggetto ObjectDataSource per recuperare i dati dal `SuppliersBLL` classe s `GetSuppliers` (metodo) e impostare l'aggiornamento scheda elenco a discesa s su (nessuno). Fare clic su Fine per completare la procedura guidata.


[![Configurare ObjectDataSource per usare il metodo GetSuppliers SuppliersBLL classe s](batch-inserting-cs/_static/image20.png)](batch-inserting-cs/_static/image19.png)

**Figura 7**: configurare ObjectDataSource per usare il `SuppliersBLL` classe s `GetSuppliers` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image21.png))


Dispone il `Suppliers` DropDownList visualizzato il `CompanyName` campo dati e utilizzare il `SupplierID` datové pole come relativo `ListItem` valori s.


[![Visualizzare il campo dati CompanyName e utilizzare SupplierID come valore](batch-inserting-cs/_static/image23.png)](batch-inserting-cs/_static/image22.png)

**Figura 8**: visualizzazione di `CompanyName` campo dati e usare `SupplierID` come valore ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image24.png))


Denominare il secondo controllo DropDownList `Categories` e associarlo a un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource`. Configurare il `CategoriesDataSource` ObjectDataSource per usare il `CategoriesBLL` classe s `GetCategories` (metodo), impostare l'elenco a discesa Elenca le schede di aggiornamento ed eliminazione su (nessuno) e fare clic su Fine per completare la procedura guidata. Infine, include la visualizzazione del controllo DropDownList il `CategoryName` campo dati e l'utilizzo di `CategoryID` come valore.

Dopo questi due controlli DropDownList sono stati aggiunti e associato a ObjectDataSource configurato in modo appropriato, la schermata dovrebbe essere simile alla figura 9.


[![La riga di intestazione contiene ora i fornitori e i controlli DropDownList categorie](batch-inserting-cs/_static/image26.png)](batch-inserting-cs/_static/image25.png)

**Figura 9**: l'intestazione di riga contiene ora il `Suppliers` e `Categories` controlli DropDownList ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image27.png))


È ora necessario creare le caselle di testo per raccogliere il nome e il prezzo per ogni nuovo prodotto. Trascinare un controllo casella di testo dalla casella degli strumenti nella finestra di progettazione per ognuna delle righe di nome e il prezzo del cinque prodotto. Impostare il `ID` delle proprietà delle caselle di testo al `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`e così via.

Aggiungere un controllo CompareValidator dopo ognuno del prezzo unitario delle caselle di testo, impostando il `ControlToValidate` proprietà appropriata `ID`. Impostare anche il `Operator` proprietà `GreaterThanEqual`, `ValueToCompare` su 0, e `Type` a `Currency`. Queste impostazioni indicano il controllo CompareValidator per garantire che il prezzo, se inserito, è un valore di valuta validi che è maggiore o uguale a zero. Impostare il `Text` proprietà \*, e `ErrorMessage` al prezzo deve essere maggiore o uguale a zero. Inoltre, per omettere qualsiasi simbolo di valuta.

> [!NOTE]
> L'interfaccia di inserimento non include tutti i controlli RequiredFieldValidator, anche se il `ProductName` campo le `Products` tabella di database non consente `NULL` valori. Questo avviene perché si vuole consentire all'utente di immettere fino a cinque prodotti. Ad esempio, se l'utente per fornire il prodotto nome e il prezzo unitario per le prime tre righe, se si lascia vuoto, le ultime due righe d aggiungiamo tre nuovi prodotti per il sistema. Poiché `ProductName` è obbligatorio, tuttavia, è necessario controllare a livello di codice per garantire che, se un prezzo unitario viene immesso viene fornito un valore di nome prodotto corrispondente. Che verranno affrontati in questo controllo nel passaggio 4.


Durante la convalida dell'input utente s, il controllo CompareValidator segnala i dati non è validi se il valore contiene un simbolo di valuta. Aggiungere un simbolo $ davanti a ogni il prezzo unitario delle caselle di testo come un segnale visivo che chiede all'utente di omettere il simbolo di valuta, quando si immette il prezzo.

Infine, aggiungere un controllo di controllo ValidationSummary all'interno la `InsertingInterface` pannello impostazioni relativo `ShowMessageBox` proprietà `true` e la relativa `ShowSummary` proprietà `false`. Con queste impostazioni, se l'utente immette un valore del prezzo unitario non valido, verrà visualizzato un asterisco accanto i controlli TextBox che causa l'errore e di controllo ValidationSummary visualizzerà una finestra di messaggio sul lato client, che mostra il messaggio di errore che è specificati in precedenza.

A questo punto, la schermata dovrebbe essere simile alla figura 10.


[![L'interfaccia inserimento ora include caselle di testo per i prodotti i nomi e i prezzi](batch-inserting-cs/_static/image29.png)](batch-inserting-cs/_static/image28.png)

**Figura 10**: l'inserimento di interfaccia ora include caselle di testo per i nomi di prodotti e i prezzi ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image30.png))


È necessario aggiungere i prodotti aggiungere dai pulsanti Annulla e spedizione per la riga di piè di pagina successiva. Trascinare due controlli pulsante dalla casella degli strumenti nel piè di pagina dell'interfaccia di inserimento, impostare i pulsanti `ID` delle proprietà per `AddProducts` e `CancelButton` e `Text` proprietà aggiungere prodotti dalla spedizione e Annulla, rispettivamente. Inoltre, impostare il `CancelButton` controllo s `CausesValidation` proprietà `false`.

Infine, è necessario aggiungere un controllo etichetta Web che verrà visualizzati messaggi di stato per le due interfacce. Ad esempio, quando completata, un utente aggiunge una nuova spedizione di prodotti, vogliamo tornare all'interfaccia di visualizzazione e visualizzare un messaggio di conferma. Se, tuttavia, l'utente fornisce un prezzo per un nuovo prodotto, ma lascia disattivato il nome del prodotto, è necessario visualizzare un messaggio di avviso perché la `ProductName` campo è obbligatorio. Poiché è necessario questo messaggio da visualizzare per entrambe le interfacce, inserirlo nella parte superiore della pagina di fuori i pannelli.

Trascinare un controllo etichetta Web dalla casella degli strumenti nella parte superiore della pagina nella finestra di progettazione. Impostare il `ID` proprietà `StatusLabel`, deselezionare le il `Text` proprietà e impostare il `Visible` e `EnableViewState` le proprietà da `false`. Come abbiamo visto nelle esercitazioni precedenti, impostando il `EnableViewState` proprietà `false` consente di modificare i valori delle proprietà etichetta s a livello di codice e chiedere di automaticamente ripristinare i valori predefiniti nei postback successivi. Ciò semplifica il codice per la visualizzazione di un messaggio di stato in risposta a un'azione utente che scompare nel postback successivi. Infine, impostare il `StatusLabel` controllo s `CssClass` proprietà di avviso, ovvero il nome di una classe CSS definita in `Styles.css` che visualizza il testo in un tipo di carattere di grandi dimensioni, corsivo, grassetto e rosso.

Figura 11 mostra la progettazione di Visual Studio dopo l'etichetta è stato aggiunto e configurato.


[![Posizionare il controllo StatusLabel sopra i due controlli Panel](batch-inserting-cs/_static/image32.png)](batch-inserting-cs/_static/image31.png)

**Figura 11**: posizione il `StatusLabel` controllo di sopra di due controlli di pannello ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Passaggio 3: Il passaggio tra la visualizzazione e l'inserimento di interfacce

A questo punto è stata completata il markup per la visualizzazione e inserire le interfacce, ma si ri rimasto con due attività:

- Il passaggio tra la visualizzazione e l'inserimento di interfacce
- Aggiunta di prodotti nella spedizione al database

Attualmente, l'interfaccia di visualizzazione è visibile ma l'interfaccia di inserimento è nascosto. Infatti il `DisplayInterface` pannello s `Visible` è impostata su `true` (valore predefinito), mentre il `InsertingInterface` pannello s `Visible` è impostata su `false`. Per passare tra le due interfacce, è sufficiente s ogni controllo Attiva/disattiva `Visible` valore della proprietà.

Si vuole spostare dall'interfaccia di visualizzazione all'interfaccia di inserimento quando si fa clic sul pulsante processo spedizione del prodotto. Pertanto, creare un gestore eventi per questo pulsante s `Click` evento contenente il codice seguente:


[!code-csharp[Main](batch-inserting-cs/samples/sample4.cs)]

Questo codice consente semplicemente di nascondere il `DisplayInterface` pannello e Mostra il `InsertingInterface` pannello.

Successivamente, creare i gestori eventi per i prodotti aggiungere dai controlli di spedizione e pulsante Annulla nell'interfaccia di inserimento. Quando viene scelto uno di questi pulsanti, è necessario ripristinare l'interfaccia di visualizzazione. Creare `Click` gestori eventi per i controlli pulsante in modo che chiamino `ReturnToDisplayInterface`, un metodo si aggiungeranno momentaneamente. Oltre a nascondere il `InsertingInterface` pannello e che mostra il `DisplayInterface` Pannello di `ReturnToDisplayInterface` metodo deve restituire i controlli Web al rispettivo stato di pre-editing. Ciò comporta l'impostazione di controlli DropDownList `SelectedIndex` delle proprietà su 0 e cancellare il `Text` proprietà dei controlli casella di testo.

> [!NOTE]
> Si consideri cosa potrebbe accadere se si t riportare i controlli al rispettivo stato di pre-modifica prima di tornare all'interfaccia di visualizzazione. Un utente può fare clic sul pulsante di spedizione del prodotto processo, immettere i prodotti dalla spedizione e quindi fare clic su Aggiungi prodotti da spedizione. Ciò Aggiungi i prodotti e restituire l'utente all'interfaccia di visualizzazione. A questo punto l'utente potrebbe voler aggiungere un'altra spedizione. Fare clic sul pulsante di spedizione del prodotto processo che restituiscono l'interfaccia di inserimento, ma il controllo DropDownList su valori nella casella di testo e le selezioni verrebbe comunque popolati con i valori precedenti.


[!code-csharp[Main](batch-inserting-cs/samples/sample5.cs)]

Entrambe `Click` gestori eventi è sufficiente chiamare il `ReturnToDisplayInterface` metodo, anche se è necessario tornare a prodotti aggiungere dalla spedizione `Click` gestore dell'evento nel passaggio 4 e aggiungere il codice per salvare i prodotti. `ReturnToDisplayInterface` viene avviata restituendo il `Suppliers` e `Categories` controlli DropDownList per le prime opzioni. Le due costanti `firstControlID` e `lastControlID` contrassegnare iniziali e finali usati nei nomi di prodotto nome e il prezzo unitario caselle di testo in cui l'inserimento dell'interfaccia e vengono usati entro i limiti di valori di indice di controllo di `for` ciclo che consente di impostare il `Text`eseguire il backup delle proprietà dei controlli casella di testo in una stringa vuota. Infine, i pannelli `Visible` le proprietà vengono reimpostate in modo che l'interfaccia di inserimento è nascosto e l'interfaccia di visualizzazione mostrata.

Si consiglia di testare questa pagina in un browser. Durante la prima visita la pagina verrà visualizzato l'interfaccia di visualizzazione come illustrato nella figura 5. Fare clic sul pulsante di spedizione del prodotto processo. La pagina verrà postback e si noterà ora l'interfaccia di inserimento come illustrato nella figura 12. Facendo clic su entrambi i prodotti aggiungere dai pulsanti di spedizione o Annulla viene nuovamente visualizzata la finestra di interfaccia di visualizzazione.

> [!NOTE]
> Mentre si visualizza l'interfaccia di inserimento, si consiglia di testare il CompareValidators sul prezzo unitario caselle di testo. Verrà visualizzato un avviso quando si fa clic i prodotti aggiungere dal pulsante di spedizione con valori di valuta non è valido o i prezzi con un valore minore di zero oggetto messagebox lato client.


[![L'interfaccia di inserimento viene visualizzato dopo aver fatto clic sul pulsante di spedizione del prodotto processo](batch-inserting-cs/_static/image35.png)](batch-inserting-cs/_static/image34.png)

**Figura 12**: l'interfaccia di inserimento viene visualizzato dopo aver fatto clic sul pulsante di spedizione del prodotto processo ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Passaggio 4: Aggiunta di prodotti

Tutto ciò che rimane per questa esercitazione consiste nel salvare i prodotti per il database nei prodotti aggiungere dal pulsante di spedizione s `Click` gestore dell'evento. Questa operazione può essere eseguita mediante la creazione di un `ProductsDataTable` e l'aggiunta di un `ProductsRow` istanza per ognuno dei nomi di prodotto specificati. Una volta queste `ProductsRow` s sono state aggiunte si effettuerà una chiamata per il `ProductsBLL` classe s `UpdateWithTransaction` metodo passando la `ProductsDataTable`. Si tenga presente che il `UpdateWithTransaction` metodo, che è stato creato nel [wrapping delle modifiche al Database in una transazione](wrapping-database-modifications-within-a-transaction-cs.md) passaggi dell'esercitazione, il `ProductsDataTable` per il `ProductsTableAdapter` s `UpdateWithTransaction` (metodo). Da qui, viene avviata una transazione di ADO.NET e i problemi di TableAdatper un' `INSERT` istruzione per il database per ogni aggiunta `ProductsRow` nella DataTable. Presupponendo che tutti i prodotti vengono aggiunti senza errori, che viene eseguito il commit della transazione, in caso contrario viene eseguito il rollback.

Il codice per i prodotti dal pulsante di spedizione s aggiungere `Click` gestore eventi deve anche eseguire una sorta di controllo degli errori. Poiché non esistono Nessun RequiredFieldValidators utilizzato nell'interfaccia di inserimento, un utente può immettere un prezzo per un prodotto omettendo il relativo nome. Poiché il nome di prodotto s è obbligatorio, se tale condizione espande è necessario avvisare l'utente e non procedere con gli inserimenti. L'intero `Click` codice del gestore eventi seguente:


[!code-csharp[Main](batch-inserting-cs/samples/sample6.cs)]

Il gestore dell'evento viene avviato, verificare che il `Page.IsValid` proprietà restituisce un valore di `true`. Se il valore restituito `false`, quindi che significa che uno o più i CompareValidators segnalano i dati non è validi; in tal caso non si desidera tenta di inserire i prodotti immessi o si otterrà un'eccezione durante il tentativo di assegnare il prezzo unitario immesso dall'utente valore per il `ProductsRow` s `UnitPrice` proprietà.

Successivamente, una nuova `ProductsDataTable` creazione di istanza (`products`). Oggetto `for` ciclo viene utilizzato per scorrere il nome e l'unità di prezzo del prodotto nelle caselle di testo e il `Text` delle proprietà vengono lette in variabili locali `productName` e `unitPrice`. Se l'utente ha immesso un valore per il prezzo unitario ma non per il nome del prodotto corrispondente, il `StatusLabel` consente di visualizzare il messaggio se si specifica un'unità di prezzo è inoltre necessario includere il nome del prodotto e il gestore dell'evento è terminato.

Se è stato fornito un nome di prodotto, una nuova `ProductsRow` istanza viene creata utilizzando la `ProductsDataTable` s `NewProductsRow` (metodo). Questa nuova `ProductsRow` istanza s `ProductName` viene impostata per il prodotto corrente assegnare un nome nella casella di testo durante le `SupplierID` e `CategoryID` le proprietà vengono assegnate al `SelectedValue` le proprietà dei controlli di DropDownList nell'intestazione dell'interfaccia s inserimento. Se l'utente ha immesso un valore per il prezzo del prodotto s, viene assegnato per il `ProductsRow` istanza s `UnitPrice` proprietà; in caso contrario, la proprietà è sinistra non assegnato, e di conseguenza un `NULL` value per `UnitPrice` nel database. Infine, il `Discontinued` e `UnitsOnOrder` delle proprietà vengono assegnate ai valori impostati come hardcoded `false` e 0, rispettivamente.

Dopo che le proprietà sono state assegnate al `ProductsRow` istanza viene aggiunto al `ProductsDataTable`.

Dopo aver completato la `for` ciclo, controlliamo se nessuno dei prodotti è stati aggiunti. L'utente potrebbe, dopotutto, hanno fatto clic su Aggiungi prodotti dalla spedizione prima di entrare in tutti i nomi dei prodotti o i prezzi. Se è presente almeno un prodotto nel `ProductsDataTable`, il `ProductsBLL` classe s `UpdateWithTransaction` viene chiamato il metodo. Successivamente, i dati sono riassociati per il `ProductsGrid` GridView in modo che i prodotti appena aggiunti verranno visualizzato nell'interfaccia di visualizzazione. Il `StatusLabel` viene aggiornato per visualizzare un messaggio di conferma e il `ReturnToDisplayInterface` viene richiamato, nascondendo l'inserimento di interfaccia e che mostra l'interfaccia di visualizzazione.

Se prodotti non sono state immesse, l'interfaccia di inserimento è visualizzato ma il messaggio di che prodotti non sono stati aggiunti. Immettere i nomi di prodotto e prezzo unitario minore nelle caselle di testo viene visualizzato.

S figura 13, 14 e 15 mostrano l'inserimento e visualizzare le interfacce in azione. Nella figura 13, l'utente ha immesso un valore del prezzo unitario senza un nome di prodotto corrispondente. Figura 14 viene illustrata l'interfaccia di visualizzazione dopo tre nuovi prodotti sono stati aggiunti correttamente, sebbene nella figura 15 vengono illustrati due dei prodotti appena aggiunti in GridView (il terzo è nella pagina precedente).


[![Un nome di prodotto è obbligatorio quando immettendo un prezzo unitario](batch-inserting-cs/_static/image38.png)](batch-inserting-cs/_static/image37.png)

**Figura 13**: nome del prodotto A è obbligatorio quando immettendo un prezzo unitario ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image39.png))


[![Sono state aggiunte tre nuove Veggies per il fornitore Mayumi s](batch-inserting-cs/_static/image41.png)](batch-inserting-cs/_static/image40.png)

**Figura 14**: tre nuovi Veggies sono state aggiunte per s Mayumi Supplier ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image42.png))


[![I nuovi prodotti sono reperibili nell'ultima pagina di GridView](batch-inserting-cs/_static/image44.png)](batch-inserting-cs/_static/image43.png)

**Figura 15**: il nuovi prodotti sono disponibili nell'ultima pagina di GridView ([fare clic per visualizzare l'immagine con dimensioni normali](batch-inserting-cs/_static/image45.png))


> [!NOTE]
> Il batch di inserimento per la logica usata in questa esercitazione esegue il wrapping dei comandi di inserimento all'interno dell'ambito della transazione. Per verificarlo, introdurrà intenzionalmente un errore a livello di database. Ad esempio, invece di assegnare il nuovo `ProductsRow` istanza s `CategoryID` sul valore selezionato nel `Categories` DropDownList, assegnare un valore, ad esempio `i * 5`. Di seguito `i` è l'indicizzatore di ciclo e dispone di valori compreso tra 1 e 5. Pertanto, quando l'aggiunta del prodotto prima di inserire due o più prodotti nel batch sarà valido `CategoryID` valore (5), ma i prodotti successivi avranno `CategoryID` i valori che non corrispondono fino a `CategoryID` i valori nel `Categories` tabella. Il risultato finale è che mentre il primo `INSERT` avrà esito positivo, le successive avranno esito negativo con una violazione di vincolo di chiave esterna. Poiché l'inserimento batch è atomico, il primo `INSERT` verrà rollback, restituendo il database sullo stato precedente processo di inserimento batch è iniziato.


## <a name="summary"></a>Riepilogo

Su questa e le due esercitazioni precedenti abbiamo creato le interfacce che consentono l'aggiornamento, eliminazione, e l'inserimento batch di dati, ognuno dei quali usato il supporto delle transazioni viene aggiunto al livello di accesso ai dati nel [di wrapping delle modifiche del Database all'interno di una transazione](wrapping-database-modifications-within-a-transaction-cs.md) esercitazione. Per alcuni scenari, tali interfacce utente per l'elaborazione batch migliorano notevolmente l'efficienza degli utenti finali riducendo il numero di clic, durante i postback e scambi di contesto al mouse di tastiera, pur mantenendo l'integrità dei dati sottostanti.

Questa esercitazione si completa l'esaminare l'uso con i dati in batch. Il set successivo di esercitazioni illustra un'ampia gamma di scenari di livello di accesso ai dati avanzati, incluso l'uso di stored procedure i metodi TableAdapter s, configurazione delle impostazioni di connessione e comando livello DAL, la crittografia di stringhe di connessione e altro ancora!

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Portare i revisori per questa esercitazione sono state Hilton Giesenow e S ren Jacob Lauritsen. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](batch-deleting-cs.md)
> [Successivo](wrapping-database-modifications-within-a-transaction-vb.md)
