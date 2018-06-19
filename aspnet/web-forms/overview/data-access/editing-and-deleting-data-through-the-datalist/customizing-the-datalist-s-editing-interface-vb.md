---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: Personalizzazione DataList di modifica della interfaccia (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione si creerà un'interfaccia di modifica più completa per DataList, che include controlli DropDownList e una casella di controllo.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 7fbdb4687a23604b9f505bbf782d59a8c78e5a02
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30884870"
---
<a name="customizing-the-datalists-editing-interface-vb"></a>Personalizzazione dell'interfaccia di modifica del controllo DataList (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) o [Scarica il PDF](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> In questa esercitazione si creerà un'interfaccia di modifica più completa per DataList, che include controlli DropDownList e una casella di controllo.


## <a name="introduction"></a>Introduzione

Il markup e i controlli Web DataList s `EditItemTemplate` definirne l'interfaccia modificabile. In tutti gli esempi di DataList modificabili è ve finora, esaminare il modificabile interfaccia è stato composto di controlli Web di casella di testo. Nel [esercitazione precedente](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) è migliorata l'esperienza utente in fase di modifica mediante l'aggiunta di controlli di convalida.

Il `EditItemTemplate` possono essere ulteriormente espanse per includere i controlli Web diverso dalla casella di testo, ad esempio controlli DropDownList, RadioButtonList, calendari e così via. Come con caselle di testo, quando si personalizza l'interfaccia di modifica per includere altri controlli Web, utilizzare la procedura seguente:

1. Aggiungere il controllo Web di `EditItemTemplate`.
2. Utilizzare la sintassi di associazione dati per assegnare il valore del campo dati corrispondente alla proprietà appropriata.
3. Nel `UpdateCommand` gestore dell'evento, l'accesso a livello di programmazione Web controllare valore e passarlo al metodo BLL appropriato.

In questa esercitazione si creerà un'interfaccia di modifica più completa per DataList, che include controlli DropDownList e una casella di controllo. In particolare, si creerà un DataList che elenca le informazioni di prodotto e consente la s nome prodotto, fornitore, categoria e lo stato non più supportato da aggiornare (vedere la figura 1).


[![L'interfaccia di modifica include una casella di testo, due controlli DropDownList e una casella di controllo](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Figura 1**: l'interfaccia di modifica include una casella di testo, due controlli DropDownList e una casella di controllo ([fare clic per visualizzare l'immagine ingrandita](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Passaggio 1: Visualizzazione di informazioni sul prodotto

Prima è possibile creare l'interfaccia di DataList s modificabile, è necessario compilare l'interfaccia di sola lettura. Aprire il `CustomizedUI.aspx` pagina dal `EditDeleteDataList` cartella e, dalla finestra di progettazione, aggiungere un controllo DataList alla pagina, l'impostazione relativa `ID` proprietà `Products`. Da DataList s smart tag, creare un nuovo oggetto ObjectDataSource. Nome nuovo ObjectDataSource `ProductsDataSource` e configurarlo per recuperare i dati di `ProductsBLL` classe s `GetProducts` metodo. Come con le esercitazioni di DataList modificabile precedente, verrà aggiornata le informazioni di prodotto modificato s accedendo direttamente al livello di logica di Business. Di conseguenza, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno).


[![Impostare gli elenchi di elenco a discesa di schede DELETE, INSERT e UPDATE su (nessuno)](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**Figura 2**: impostare l'aggiornamento, inserimento ed eliminare elenchi di elenco a discesa schede su (nessuno) ([fare clic per visualizzare l'immagine ingrandita](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


Dopo aver configurato il ObjectDataSource, Visual Studio creerà predefinito `ItemTemplate` per DataList che elenca il nome e il valore per ognuno dei campi di dati restituito. Modificare il `ItemTemplate` in modo che nel modello sono elencati il nome del prodotto in un `<h4>` elemento con il nome di categoria, nome del fornitore, prezzo e lo stato non più supportato. Inoltre, aggiungere un pulsante di modifica, assicurandosi che il relativo `CommandName` proprietà è impostata su Modifica. Il markup dichiarativo per my `ItemTemplate` segue:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Il codice precedente viene disposto il prodotto di informazioni usando un &lt;h4&gt; titolo per il nome del prodotto s e quattro colonne `<table>` per i campi rimanenti. Il `ProductPropertyLabel` e `ProductPropertyValue` classi CSS, definite in `Styles.css`, sono state illustrate nelle esercitazioni precedenti. Figura 3 Mostra stato di avanzamento quando viene visualizzato tramite un browser.


[![Viene visualizzato il nome, fornitore, categoria, lo stato non più disponibile e prezzo di ogni prodotto](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Figura 3**: viene visualizzato il nome, fornitore, categoria, lo stato non più disponibile e prezzo di ogni prodotto ([fare clic per visualizzare l'immagine ingrandita](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Passaggio 2: Aggiungere controlli Web per l'interfaccia di modifica

Il primo passaggio nella creazione di DataList personalizzato interfaccia per la modifica consiste nell'aggiungere i controlli Web necessari per il `EditItemTemplate`. In particolare, è necessario un controllo DropDownList per la categoria, un altro per il fornitore e una casella di controllo per lo stato non più supportato. Poiché il prezzo del prodotto s non è modificabile in questo esempio, è possibile continuare a visualizzare utilizzando un controllo etichetta Web.

Per personalizzare l'interfaccia di modifica, fare clic sul collegamento Modifica modelli dallo smart tag DataList s e scegliere il `EditItemTemplate` opzione dall'elenco a discesa. Aggiungere un controllo DropDownList per il `EditItemTemplate` e impostare il relativo `ID` a `Categories`.


[![Aggiungere un controllo DropDownList per le categorie](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Figura 4**: aggiungere un controllo DropDownList per le categorie ([fare clic per visualizzare l'immagine ingrandita](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


Successivamente, DropDownList s smart tag, selezionare l'opzione Scegli origine dati e creare un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource`. Configurare ObjectDataSource per utilizzare il `CategoriesBLL` classe s `GetCategories()` (metodo) (vedere Figura 5). Successivamente, s DropDownList richiede configurazione guidata origine dati per i campi di dati da utilizzare per ogni `ListItem` s `Text` e `Value` proprietà. La visualizzazione DropDownList il `CategoryName` campo dati e utilizzare il `CategoryID` come valore, come illustrato nella figura 6.


[![Creare un nuovo oggetto ObjectDataSource denominato CategoriesDataSource](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Figura 5**: creare un nuovo denominato ObjectDataSource `CategoriesDataSource` ([fare clic per visualizzare l'immagine ingrandita](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![Configurare la visualizzazione di s DropDownList e il valore di campi](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Figura 6**: configurare le s DropDownList visualizzato e i campi di valore ([fare clic per visualizzare l'immagine ingrandita](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


Ripetere questa serie di passaggi per creare un controllo DropDownList per i fornitori. Impostare il `ID` per questo DropDownList per `Suppliers` e il nome relativo ObjectDataSource `SuppliersDataSource`.

Dopo aver aggiunto i due controlli DropDownList, aggiungere una casella di controllo per lo stato non più supportato e una casella di testo per il nome del prodotto s. Impostare il `ID` s per la casella di controllo e una casella di testo per `Discontinued` e `ProductName`, rispettivamente. Aggiungere un controllo RequiredFieldValidator per assicurarsi che l'utente fornisce un valore per il nome del prodotto s.

Infine, aggiungere i pulsanti Annulla e di aggiornamento. Tenere presente che per questi due pulsanti è fondamentale che i relativi `CommandName` proprietà vengono impostate per aggiornare e annullare, rispettivamente.

È possibile disporre tuttavia si desidera che l'interfaccia di modifica. Ho scelto di utilizzare lo stesso quattro colonne ve `<table>` layout dall'interfaccia di sola lettura, come la seguente sintassi dichiarativa e immagine riportata di seguito viene illustrato:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![Non è più l'interfaccia di modifica cui valido, ad esempio l'interfaccia di sola lettura](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Figura 7**: non è più l'interfaccia di modifica cui valido, ad esempio l'interfaccia di sola lettura ([fare clic per visualizzare l'immagine ingrandita](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Passaggio 3: Creazione EditCommand e gestori eventi CancelCommand

Attualmente non è disponibile alcuna sintassi di Data Binding nel `EditItemTemplate` (tranne che per il `UnitPriceLabel`, che è stato copiato verbatim dal `ItemTemplate`). Verrà aggiunto temporaneamente la sintassi di associazione dati, ma consentono prima s creare i gestori eventi per DataList s `EditCommand` e `CancelCommand` eventi. Tenere presente che la responsabilità del `EditCommand` gestore dell'evento è rendere l'interfaccia di modifica per l'elemento DataList è stato fatto clic con pulsante di modifica, mentre il `CancelCommand` processo s consiste nel ripristinare lo stato pre-modifica DataList.

Creare questi due gestori eventi e richiedere loro di utilizzare il codice seguente:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

Con questi due gestori di eventi sul posto, fare clic sul pulsante Modifica consente di visualizzare l'interfaccia di modifica e facendo clic sul pulsante Annulla restituisce l'elemento modificato per la modalità di sola lettura. Figura 8 Mostra DataList dopo che il pulsante Modifica è stato fatto clic s Chef Anton's Gumbo Mix. Poiché è ve ancora per aggiungere la sintassi di associazione dati per l'interfaccia di modifica, il `ProductName` casella di testo è vuoto, il `Discontinued` deselezionata la casella di controllo e i primi elementi selezionati nella `Categories` e `Suppliers` controlli DropDownList.


[![Scegliere il pulsante di modifica, viene visualizzato l'interfaccia di modifica](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Figura 8**: fare clic sul pulsante Modifica consente di visualizzare l'interfaccia di modifica ([fare clic per visualizzare l'immagine ingrandita](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Passaggio 4: Aggiunta della sintassi di associazione dati per l'interfaccia di modifica

Per visualizzare i valori correnti di s prodotto l'interfaccia di modifica, è necessario utilizzare la sintassi di associazione dati per assegnare i valori dei campi dati per i valori appropriati del controllo Web. La sintassi di associazione dati possono essere applicata tramite la finestra di progettazione, passare alla schermata Modifica modelli e selezionando il collegamento Modifica DataBindings dal Web controlla gli smart tag. In alternativa, è possibile aggiungere la sintassi di associazione dati direttamente al markup dichiarativo.

Assegnare il `ProductName` valore del campo dati il `ProductName` TextBox s `Text` proprietà, il `CategoryID` e `SupplierID` valori al campo dati il `Categories` e `Suppliers` controlli DropDownList `SelectedValue` proprietà e `Discontinued` valore del campo dati di `Discontinued` casella di controllo s `Checked` proprietà. Dopo aver apportato queste modifiche, tramite la finestra di progettazione o direttamente tramite markup dichiarativo, fare clic sul pulsante Modifica per s Chef Anton's Gumbo Mix nuovamente la pagina tramite un browser. Come illustrato nella figura 9, la sintassi di associazione dati ha aggiunto i valori correnti nella casella di testo, controlli DropDownList e casella di controllo.


[![Scegliere il pulsante di modifica, viene visualizzato l'interfaccia di modifica](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Figura 9**: fare clic sul pulsante Modifica consente di visualizzare l'interfaccia di modifica ([fare clic per visualizzare l'immagine ingrandita](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Passaggio 5: Salvare le modifiche di s utente nel gestore eventi UpdateCommand

Quando l'utente modifica un prodotto e fa clic sul pulsante di aggiornamento, si verifica un postback e DataList s `UpdateCommand` viene generato l'evento. Nell'evento gestore, è necessario leggere i valori dai controlli Web nel `EditItemTemplate` e interfaccia con il livello Business LOGIC per aggiornare il prodotto nel database. Come si ve illustrata nelle esercitazioni precedenti, il `ProductID` del prodotto aggiornato è possibile accedere tramite il `DataKeys` insieme. I campi immessi dall'utente sono accessibili facendo riferimento a livello di codice i controlli Web usando `FindControl("controlID")`, come mostrato nel codice seguente:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

Il codice inizia consultando la `Page.IsValid` proprietà per assicurarsi che tutti i controlli di convalida nella pagina siano validi. Se `Page.IsValid` è `True`, il prodotto modificato s `ProductID` valore viene letto dal `DataKeys` raccolta e l'immissione di dati in cui i controlli Web di `EditItemTemplate` a livello di codice a cui fa riferimento. Successivamente, in cui vengono letti i valori di questi controlli Web in variabili che vengono quindi passate appropriata `UpdateProduct` rapporto di overload. Dopo l'aggiornamento dei dati, DataList viene restituito lo stato di pre-modifica.

> [!NOTE]
> Si viene omessa la logica aggiunta nella gestione delle eccezioni di [BLL - gestione e le eccezioni di livello DAL](handling-bll-and-dal-level-exceptions-vb.md) esercitazione per mantenere il codice e in questo esempio con stato attivo. Come esercizio, aggiungere questa funzionalità dopo aver completato questa esercitazione.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Passaggio 6: Gestire CategoryID NULL e valori di ID fornitore

Consente di Northwind `NULL` valori per il `Products` tabella s `CategoryID` e `SupplierID` colonne. Tuttavia, la modifica t interfaccia contenere attualmente `NULL` valori. Se si tenta di modificare un prodotto che ha un `NULL` valore per la relativa `CategoryID` o `SupplierID` colonne, verrà creata un' `ArgumentOutOfRangeException` con un messaggio di errore simile al: *'Categorie' ha un SelectedValue che non è valido perché non è presente nell'elenco di elementi.* Inoltre, s non esiste attualmente non è possibile modificare una categoria di prodotto s o un fornitore valore da un non -`NULL` valore un `NULL` uno.

Per supportare `NULL` valori per la categoria e un fornitore controlli DropDownList, è necessario aggiungere un ulteriore `ListItem`. Si va scelto di usare (nessuno) come il `Text` valore per questo `ListItem`, ma è possibile modificare per qualcos'altro (ad esempio una stringa vuota) se d desiderato. Infine, ricordare di impostare i controlli DropDownList `AppendDataBoundItems` a `True`; se si procede in questo modo, le categorie e fornitori associati a DropDownList sovrascriverà l'oggetto aggiunto in modo statico `ListItem`.

Dopo aver apportato queste modifiche, il markup controlli DropDownList in DataList s `EditItemTemplate` dovrebbe essere simile al seguente:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Statico `ListItem` s può essere aggiunto a un controllo DropDownList tramite la finestra di progettazione o direttamente tramite la sintassi dichiarativa. Quando si aggiunge un elemento DropDownList per rappresentare un database `NULL` valore, accertarsi di aggiungere il `ListItem` tramite la sintassi dichiarativa. Se si utilizza il `ListItem` Editor della raccolta nella finestra di progettazione, la sintassi dichiarativa generata ometterà il `Value` impostazione completamente quando assegnato a una stringa vuota, la creazione di markup dichiarativo, ad esempio: `<asp:ListItem>(None)</asp:ListItem>`. Mentre il risultato potrebbe essere innocuo, la mancante `Value` provoca DropDownList utilizzare il `Text` valore della proprietà al suo posto. Ciò significa che se questo `NULL` `ListItem` è selezionata, il valore (nessuno) verrà tentato da assegnare al campo di dati di prodotto (`CategoryID` o `SupplierID`, in questa esercitazione), che verrà generata un'eccezione. Impostando in modo esplicito `Value=""`, `NULL` verrà assegnato il valore per il prodotto del campo dati quando il `NULL` `ListItem` è selezionata.


Richiedere qualche istante per visualizzare l'avanzamento attraverso un browser. Quando si modifica un prodotto, si noti che il `Categories` e `Suppliers` controlli DropDownList entrambi hanno (nessuno) opzione all'inizio di DropDownList.


[![Le categorie e i controlli DropDownList Suppliers includono (nessuno) opzione](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Figura 10**: il `Categories` e `Suppliers` controlli DropDownList includono un (None) opzione ([fare clic per visualizzare l'immagine ingrandita](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


Per salvare opzione (nessuno) come database `NULL` valore, è necessario restituire il `UpdateCommand` gestore dell'evento. Modifica il `categoryIDValue` e `supplierIDValue` variabili integer ammette valori null e assegnare loro un valore diverso da `Nothing` solo se i dispositivi DropDownList `SelectedValue` non è una stringa vuota:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Con questa modifica, il valore `Nothing` verranno passati di `UpdateProduct` opzione metodo BLL se l'utente selezionato (nessuno) da uno degli elenchi di riepilogo a discesa, che corrisponde a un `NULL` valore del database.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come creare un'interfaccia di modifica DataList più complessa che inclusi tre diversi controlli di Web inpui, una casella di testo, due controlli DropDownList e una casella di controllo con i controlli di convalida. Quando si compila l'interfaccia di modifica, i passaggi sono gli stessi controlli Web in uso: per iniziare, aggiungere i controlli Web per il controllo DataList s `EditItemTemplate`; utilizzare sintassi di associazione dati per assegnare i valori dei campi dati corrispondenti con Web appropriato controllo proprietà. in e il `UpdateCommand` gestore eventi, accedere a livello di codice i controlli Web e le relative proprietà appropriato, passando i relativi valori in BLL.

Quando si crea un'interfaccia di modifica, se si s è composto da solo nelle caselle di testo o una raccolta di controlli Web diversi, assicurarsi di gestire correttamente i database `NULL` valori. Quando si giustificano `NULL` s, è fondamentale non correttamente solo visualizzare esistente `NULL` valore l'interfaccia di modifica, ma anche che offrono un modo per contrassegnare un valore come `NULL`. Per controlli DropDownList in DataList, che in genere significa che l'aggiunta di un valore statico `ListItem` cui `Value` proprietà è impostata esplicitamente su una stringa vuota (`Value=""`) e aggiungendo un bit di codice per il `UpdateCommand` gestore eventi per determinare o meno il `NULL``ListItem` è stato selezionato.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Dennis Patterson e David Suru Randy Schmidt. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
