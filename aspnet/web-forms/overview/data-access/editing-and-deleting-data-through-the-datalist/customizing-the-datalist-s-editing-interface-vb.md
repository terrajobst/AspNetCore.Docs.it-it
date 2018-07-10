---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: Personalizzazione di DataList di modifica dell'interfaccia (Visual Basic) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione si creerà un'interfaccia di modifica più completa per il controllo DataList, che include controlli DropDownList e una casella di controllo.
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 78001e977a4696e905317eab35604518d059e66d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828757"
---
<a name="customizing-the-datalists-editing-interface-vb"></a>Personalizzazione di interfaccia di modifica di DataList (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) o [Scarica il PDF](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> In questa esercitazione si creerà un'interfaccia di modifica più completa per il controllo DataList, che include controlli DropDownList e una casella di controllo.


## <a name="introduction"></a>Introduzione

Il markup e i controlli Web in DataList s `EditItemTemplate` definirne l'interfaccia modificabile. In tutti gli esempi DataList modificabili è ve fino ad ora esaminato il modificabile interfaccia è stato composto di controlli Web nella casella di testo. Nel [esercitazione precedente](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) è stata migliorata l'esperienza utente in fase di modifica mediante l'aggiunta di controlli di convalida.

Il `EditItemTemplate` possono essere ulteriormente ampliato per includere i controlli Web diversi da casella di testo, ad esempio controlli DropDownList, RadioButtonList, calendari e così via. Come con caselle di testo, quando si personalizza l'interfaccia di modifica per includere altri controlli Web, utilizzare la procedura seguente:

1. Aggiungere il controllo Web per il `EditItemTemplate`.
2. Usare la sintassi di associazione dati per assegnare il valore del campo data corrispondente alla proprietà appropriata.
3. Nel `UpdateCommand` gestore dell'evento, l'accesso a livello di programmazione Web valore controllato e passarlo al metodo BLL appropriato.

In questa esercitazione si creerà un'interfaccia di modifica più completa per il controllo DataList, che include controlli DropDownList e una casella di controllo. In particolare, verrà creato un controllo DataList che elenca le informazioni sul prodotto e consente la s nome prodotto, fornitore, categoria e lo stato non più supportato da aggiornare (vedere la figura 1).


[![L'interfaccia di modifica include un controllo TextBox, due controlli DropDownList e una casella di controllo](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Figura 1**: l'interfaccia di modifica include un controllo TextBox, due controlli DropDownList e una casella di controllo ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Passaggio 1: Visualizzare le informazioni sul prodotto

Prima di poter creare l'interfaccia modificabile s DataList, è innanzitutto necessario compilare l'interfaccia di sola lettura. Iniziare aprendo il `CustomizedUI.aspx` pagina dal `EditDeleteDataList` cartella e, dalla finestra di progettazione, aggiungere un controllo DataList alla pagina, l'impostazione relativa `ID` proprietà `Products`. DataList s nello smart tag, creare un nuovo oggetto ObjectDataSource. Denominare il nuovo oggetto ObjectDataSource `ProductsDataSource` e configurarlo per recuperare i dati di `ProductsBLL` classe s `GetProducts` (metodo). Come con le esercitazioni di DataList modificabile precedente, verrà aggiornato le informazioni di prodotto modificato s accedendo direttamente al livello della logica di Business. Di conseguenza, impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno).


[![Impostare gli elenchi di riepilogo a discesa schede DELETE, INSERT e UPDATE su (nessuno)](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**Figura 2**: impostare l'UPDATE, INSERT e DELETE schede elenco a discesa Elenca su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


Dopo la configurazione di ObjectDataSource, Visual Studio creerà predefinito `ItemTemplate` per il controllo DataList che elenca il nome e il valore per ognuno dei campi di dati restituito. Modificare il `ItemTemplate` in modo che il modello viene elencato il nome di prodotto in un `<h4>` elemento con il nome della categoria, nome del fornitore, prezzo e lo stato non più utilizzato. Inoltre, aggiungere un pulsante di modifica, assicurandosi che relativo `CommandName` è impostata su Modifica. Il markup dichiarativo per la mia `ItemTemplate` segue:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Il markup riportato sopra viene disposto l'informazioni di prodotto usando un &lt;h4&gt; intestazione per il nome del prodotto s e una colonna di quattro `<table>` per i campi rimanenti. Il `ProductPropertyLabel` e `ProductPropertyValue` classi CSS, definite in `Styles.css`, sono stati trattati nelle esercitazioni precedenti. Figura 3 mostra lo stato di avanzamento quando viene visualizzato tramite un browser.


[![Viene visualizzato il nome, fornitore, categoria, lo stato non più utilizzate e prezzo di ogni prodotto](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Figura 3**: viene visualizzato il nome del fornitore, categoria, non più utilizzate e stato prezzo di ogni prodotto ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Passaggio 2: Aggiunta di controlli Web per l'interfaccia di modifica

Il primo passaggio nella creazione di DataList personalizzato interfaccia per la modifica consiste nell'aggiungere i controlli Web necessari per il `EditItemTemplate`. In particolare, è necessario un controllo DropDownList per la categoria, un altro per il fornitore e una casella di controllo per lo stato non più utilizzato. Poiché il prezzo del prodotto s non può essere modificato in questo esempio, è possibile continuare la visualizzazione usando un controllo etichetta Web.

Per personalizzare l'interfaccia di modifica, fare clic sul collegamento di modifica modelli dello smart tag DataList s e scegliere il `EditItemTemplate` opzione nell'elenco a discesa. Aggiungere un controllo DropDownList per il `EditItemTemplate` e impostare relativi `ID` a `Categories`.


[![Aggiungere un controllo DropDownList per le categorie](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Figura 4**: aggiungere un controllo DropDownList per le categorie ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


Successivamente, DropDownList s nello smart tag, selezionare l'opzione Scegli origine dati e creare un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource`. Configurare ObjectDataSource per usare la `CategoriesBLL` classe s `GetCategories()` (metodo) (vedere la figura 5). Successivamente, la s DropDownList chiede di configurazione guidata origine dati per i campi di dati da usare per ognuno `ListItem` s `Text` e `Value` proprietà. La visualizzazione DropDownList il `CategoryName` campo dati e usare il `CategoryID` come valore, come illustrato nella figura 6.


[![Creare un nuovo oggetto ObjectDataSource denominato CategoriesDataSource](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Figura 5**: creare un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![Configurare la visualizzazione di s DropDownList e il valore di campi](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Figura 6**: configurare il controllo DropDownList s visualizzato e i campi dei valori ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


Ripetere questa serie di passaggi per creare un controllo DropDownList per i fornitori. Impostare il `ID` per questo controllo DropDownList per `Suppliers` e assegnare un nome relativo ObjectDataSource `SuppliersDataSource`.

Dopo aver aggiunto i due controlli DropDownList, aggiungere una casella di controllo per lo stato non più utilizzato e una casella di testo per il nome del prodotto s. Impostare il `ID` per la casella di controllo e casella di testo `Discontinued` e `ProductName`, rispettivamente. Aggiungere un RequiredFieldValidator per assicurarsi che l'utente fornisce un valore per il nome del prodotto s.

Infine, aggiungere i pulsanti Annulla e Update. Tenere presente che per questi due pulsanti è fondamentale che loro `CommandName` proprietà vengono impostate per aggiornare e annullare, rispettivamente.

È possibile definire il layout di interfaccia di modifica, tuttavia si desidera. Ho scelto di usare lo stesso quattro colonne ve `<table>` layout dall'interfaccia di sola lettura, come la seguente sintassi dichiarativa e screenshot illustra:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![L'interfaccia di modifica non è più Laid valido, ad esempio l'interfaccia di sola lettura](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Figura 7**: l'interfaccia di modifica è layout orizzontale, ad esempio l'interfaccia di sola lettura ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Passaggio 3: Creazione del EditCommand e gestori eventi CancelCommand

Attualmente non è disponibile alcuna sintassi di associazione dati nel `EditItemTemplate` (tranne che per il `UnitPriceLabel`, che è stato copiato su verbatim dal `ItemTemplate`). Aggiungiamo, quindi la sintassi di associazione dati momentaneamente, ma ti permettono di primo s creare i gestori eventi per s DataList `EditCommand` e `CancelCommand` eventi. Si tenga presente che la responsabilità del `EditCommand` gestore dell'evento è rendere l'interfaccia di modifica per l'elemento DataList è stato fatto clic sul pulsante la cui modifica, mentre il `CancelCommand` processo s consiste nel ripristinare lo stato pre-modifica di DataList.

Creare questi due gestori di eventi e richiedere loro di utilizzare il codice seguente:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

Con questi due gestori di eventi sul posto, facendo clic sul pulsante Modifica consente di visualizzare l'interfaccia di modifica e facendo clic sul pulsante di annullamento restituisce l'elemento modificato per la modalità di sola lettura. Figura 8 Mostra DataList dopo che il pulsante Modifica è stato fatto clic per s Chef Anton's Gumbo Mix. Poiché abbiamo ve ancora per aggiungere una sintassi di associazione dati all'interfaccia di modifica, il `ProductName` nella casella di testo è vuota, il `Discontinued` selezionati dalla casella di controllo è deselezionata e i primi elementi il `Categories` e `Suppliers` controlli DropDownList.


[![Facendo clic su verrà visualizzato il pulsante Modifica l'interfaccia di modifica](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Figura 8**: facendo clic sul pulsante Modifica consente di visualizzare l'interfaccia di modifica ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Passaggio 4: Aggiunta della sintassi di associazione dati all'interfaccia di modifica

Per visualizzare i valori correnti di s prodotto l'interfaccia di modifica, è necessario usare la sintassi di associazione dati per assegnare i valori dei campi dati per i valori del controllo Web appropriati. La sintassi di associazione dati possono essere applicata tramite la finestra di progettazione, passare alla schermata di modifica modelli e selezionando il collegamento di Modifica DataBindings dal Web controlla gli smart tag. In alternativa, è possibile aggiungere la sintassi di associazione dati direttamente al markup dichiarativo.

Assegnare il `ProductName` valore del campo dati il `ProductName` s nella casella di testo `Text` proprietà, il `CategoryID` e `SupplierID` datové pole i valori per i `Categories` e `Suppliers` controlli DropDownList `SelectedValue` proprietà e il `Discontinued` valore del campo dati il `Discontinued` casella di controllo s `Checked` proprietà. Dopo aver apportato queste modifiche, tramite la finestra di progettazione o direttamente tramite markup dichiarativo, visitare di nuovo la pagina tramite un browser e fare clic sul pulsante Modifica per s Chef Anton's Gumbo Mix. Come illustrato nella figura 9, la sintassi di data binding ha aggiunto i valori correnti di casella di testo e controlli DropDownList e casella di controllo.


[![Facendo clic su verrà visualizzato il pulsante Modifica l'interfaccia di modifica](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Figura 9**: facendo clic sul pulsante Modifica consente di visualizzare l'interfaccia di modifica ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Passaggio 5: Salvare le modifiche di s utente nel gestore dell'evento UpdateCommand

Quando l'utente modifica un prodotto e fa clic sul pulsante di aggiornamento, si verifica un postback e DataList s `UpdateCommand` viene generato l'evento. Nell'evento gestore, è necessario leggere i valori dai controlli Web nel `EditItemTemplate` e l'interfaccia con il livello BLL per aggiornare il prodotto nel database. Come abbiamo ve illustrata nelle esercitazioni precedenti, il `ProductID` del prodotto aggiornato è accessibile tramite il `DataKeys` raccolta. I campi immesso dall'utente sono accessibili facendo riferimento a livello di codice i controlli Web usando `FindControl("controlID")`, come illustrato nel codice seguente:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

Il codice inizia consultando il `Page.IsValid` proprietà per assicurarsi che tutti i controlli di convalida nella pagina siano validi. Se `Page.IsValid` viene `True`, il prodotto modificato s `ProductID` valore viene letto dal `DataKeys` raccolta e l'inserimento dei dati in cui i controlli Web il `EditItemTemplate` fanno riferimento a livello di codice. Successivamente, vengono letti i valori di questi controlli Web in variabili che vengono quindi passate in appropriato `UpdateProduct` rapporto di overload. Dopo l'aggiornamento dei dati, DataList viene restituito lo stato di pre-editing.

> [!NOTE]
> Ho ve omesso aggiunta una logica di gestione delle eccezioni di [BLL - la gestione e le eccezioni a livello DAL](handling-bll-and-dal-level-exceptions-vb.md) esercitazione per mantenere il codice e in questo esempio con stato attivo. Come esercizio, aggiungere questa funzionalità dopo aver completato questa esercitazione.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Passaggio 6: Gestire CategoryID NULL e valori di ID fornitore

Consente di Northwind `NULL` i valori per il `Products` tabella s `CategoryID` e `SupplierID` colonne. Tuttavia, la modifica t l interfaccia adattare attualmente `NULL` valori. Se si tenta di modificare un prodotto che ha un `NULL` valore per uno relativo `CategoryID` o `SupplierID` colonne, si otterrà un' `ArgumentOutOfRangeException` con un messaggio di errore simile a: *'Categorie' ha un SelectedValue non è valido perché non esiste nell'elenco di elementi.* Inoltre, 3!s ci attualmente alcuna possibilità di modificare una categoria di prodotto s o un fornitore non valore da un non -`NULL` valore per un `NULL` uno.

Per supportare `NULL` i valori per la categoria e il fornitore controlli DropDownList, dobbiamo aggiungere un ulteriore `ListItem`. Ho ve scelta di usare (nessuno) come il `Text` un valore per questo `ListItem`, ma è possibile modificarlo per qualcos'altro (ad esempio, una stringa vuota) se è il d soddisfacente. Infine, ricordarsi di impostare i controlli DropDownList `AppendDataBoundItems` al `True`; se si dimentica di eseguire questa operazione, le categorie e fornitori associati a DropDownList sovrascrive l'oggetto aggiunto in modo statico `ListItem`.

Dopo aver apportato queste modifiche, il markup controlli DropDownList in DataList s `EditItemTemplate` dovrebbe essere simile al seguente:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Statico `ListItem` s può essere aggiunto a un controllo DropDownList tramite la finestra di progettazione o direttamente tramite la sintassi dichiarativa. Quando si aggiunge un elemento DropDownList per rappresentare un database `NULL` valore, assicurarsi di aggiungere il `ListItem` tramite la sintassi dichiarativa. Se si usa la `ListItem` Editor della raccolta nella finestra di progettazione, la sintassi dichiarativa generata ometterà il `Value` impostazione completamente quando assegnato a una stringa vuota, la creazione di markup dichiarativo simile a quello: `<asp:ListItem>(None)</asp:ListItem>`. Anche se ciò potrebbe avere un aspetto innocuo, parametro mancante `Value` fa sì che il controllo DropDownList usare il `Text` valore della proprietà al suo posto. Ciò significa che se si `NULL` `ListItem` è selezionata, il valore (nessuno) verrà tentato da assegnare al campo dati del prodotto (`CategoryID` o `SupplierID`, in questa esercitazione), che genererà un'eccezione. Impostando in modo esplicito `Value=""`, una `NULL` valore verrà assegnato il prodotto del campo dati quando il `NULL` `ListItem` sia selezionata.


Si consiglia di visualizzare lo stato di avanzamento tramite un browser. Quando si modifica un prodotto, si noti che il `Categories` e `Suppliers` dispone di due controlli DropDownList (nessuno) opzione all'inizio di DropDownList.


[![Le categorie e controlli DropDownList fornitori includono (nessuna) opzione](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Figura 10**: il `Categories` e `Suppliers` controlli DropDownList includono (nessuna) opzione ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


Salvare opzione (nessuno) come un database `NULL` valore, è necessario restituire il `UpdateCommand` gestore dell'evento. Modifica il `categoryIDValue` e `supplierIDValue` variabili integer nullable e assegnarle un valore diverso da `Nothing` solo se s DropDownList `SelectedValue` non è una stringa vuota:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Con questa modifica, un valore pari `Nothing` verranno passati i `UpdateProduct` metodo BLL se l'utente selezionato (nessuno) opzione da uno degli elenchi di riepilogo a discesa, che corrisponde a un `NULL` valore del database.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come creare un'interfaccia di modifica DataList più complessa che include tre diversi controlli di Web inpui una casella di testo, due controlli DropDownList e una casella di controllo con i controlli di convalida. Quando si compila l'interfaccia di modifica, i passaggi sono gli stessi indipendentemente dal fatto di controlli Web in uso: iniziare aggiungendo i controlli Web per il controllo DataList s `EditItemTemplate`; usare la sintassi di associazione dati per assegnare i valori dei campi dati corrispondenti con il Web in modo appropriato controllo proprietà. e, nella `UpdateCommand` gestore eventi, accedere a livello di codice i controlli Web e le relative proprietà appropriato, passando i relativi valori nel livello BLL.

Quando si crea un'interfaccia di modifica, se si s è composto da solo nelle caselle di testo o una raccolta di controlli Web diversi, assicurarsi di gestire correttamente i database `NULL` valori. Quando tenendo conto dei `NULL` s, è fondamentale non solo correttamente visualizzare esistente `NULL` valore l'interfaccia di modifica, ma anche che si offrono un modo per contrassegnare un valore come `NULL`. Per controlli DropDownList in DataList, che in genere significa aggiungere un valore statico `ListItem` la cui `Value` proprietà è impostata esplicitamente su una stringa vuota (`Value=""`) e l'aggiunta di un frammento di codice per il `UpdateCommand` gestore dell'evento per determinare o meno il `NULL``ListItem` è stato selezionato.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Dennis Patterson e David Suru Randy Schmidt. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
