---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: Personalizzazione dell'interfaccia di modifica dei dati (c#) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà esaminato in che modo personalizzare l'interfaccia di un controllo GridView modificabile, sostituendo la casella di testo standard e consente di controllare la casella di controllo con alternati...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: f7004192edd636f4660f3184c3e725a6bfda865c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827112"
---
<a name="customizing-the-data-modification-interface-c"></a>Personalizzazione dell'interfaccia di modifica dei dati (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe) o [Scarica il PDF](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> In questa esercitazione verrà esaminato in che modo personalizzare l'interfaccia di un controllo GridView modificabile, sostituendo la casella di testo standard e controlli casella di controllo con i controlli Web di input alternativi.


## <a name="introduction"></a>Introduzione

I BoundField e CheckBoxFields utilizzato dai controlli GridView e DetailsView semplificano il processo di modifica dei dati a causa delle loro capacità di eseguire il rendering delle interfacce di sola lettura, modificabile e inseribile. Queste interfacce possono eseguire il rendering senza la necessità per l'aggiunta di qualsiasi codice o markup dichiarativo aggiuntive. Tuttavia, i BoundField e di CampoCasellaDiControllo interfacce non dispongono della possibilità di personalizzazione spesso necessarie per scenari reali. Per personalizzare l'interfaccia modificabile o inseribile in un controllo GridView o controllo DetailsView è necessario usare invece un TemplateField.

Nel [esercitazione precedente](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) è stato illustrato come personalizzare le interfacce di modifica dei dati mediante l'aggiunta di controlli Web di convalida. In questa esercitazione verrà esaminato come personalizzare i dati effettivi raccolta controlli Web, sostituendo i BoundField e casella di testo standard della CampoCasellaDiControllo e i controlli casella di controllo con i controlli Web di input alternativi. In particolare, verrà compilata una GridView modificabile che consente a un prodotto nome, categoria, fornitore e lo stato non più supportato da aggiornare. Quando si modifica una determinata riga, i campi categoria e il fornitore eseguirà il rendering come controlli DropDownList, che contiene il set di categorie disponibili e fornitori, tra cui scegliere. Inoltre, sostituiremo predefinito dell'elemento CheckBoxField casella di controllo con un controllo RadioButtonList che sono disponibili due opzioni: "Attivo" e "Sospeso".


[![Interfaccia di modifica del controllo GridView include controlli DropDownList e pulsanti di opzione](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**Figura 1**: modifica di controlli DropDownList include interfaccia e pulsanti di opzione di GridView ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Passaggio 1: Creazione appropriato`UpdateProduct`Overload

In questa esercitazione verrà compilato un GridView modificabile che consenta la modifica del nome di un prodotto, categoria, fornitore e lo stato sospeso. Pertanto, è necessario un `UpdateProduct` overload che accetta cinque parametri di input di questi valori quattro prodotti più il `ProductID`. Come nel nostro overload precedente, questo uno verrà:

1. Recuperare le informazioni sul prodotto dal database per l'oggetto specificato `ProductID`,
2. Aggiorna il `ProductName`, `CategoryID`, `SupplierID`, e `Discontinued` campi, e
3. Inviare la richiesta di aggiornamento al livello dal tramite dell'oggetto TableAdapter `Update()` (metodo).

Per brevità, per questo specifico overload è stata omessa la verifica della regola business che garantisce un prodotto viene contrassegnato come non più disponibile non è il solo prodotto offerto dal relativo fornitore. È possibile aggiungerlo in se si preferisce, o, in teoria, il refactoring out la logica a un metodo separato.

Il codice seguente illustra le nuove `UpdateProduct` rapporto di overload nel `ProductsBLL` classe:


[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>Passaggio 2: Creazione GridView modificabile

Con la `UpdateProduct` overload aggiunto, siamo pronti per creare il controllo GridView modificabile. Aprire il `CustomizedUI.aspx` nella pagina di `EditInsertDelete` cartella e aggiungere un controllo GridView nella finestra di progettazione. Successivamente, creare un nuovo oggetto ObjectDataSource dallo smart tag del controllo GridView. Configurare ObjectDataSource per recuperare informazioni sui prodotti tramite il `ProductBLL` della classe `GetProducts()` metodo e per aggiornare dati di prodotto usando il `UpdateProduct` overload appena creato. WSSSITE INSERT e DELETE, select (nessuno) dagli elenchi a discesa.


[![Configurare ObjectDataSource per usare l'Overload UpdateProduct appena creato](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**Figura 2**: configurare ObjectDataSource per usare il `UpdateProduct` Overload appena creato ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image6.png))


Come abbiamo visto durante le esercitazioni di modifica dei dati, la sintassi dichiarativa per l'oggetto ObjectDataSource creato da Visual Studio assegna la `OldValuesParameterFormatString` proprietà `original_{0}`. Ciò, naturalmente, non funzionerà con il livello di logica di Business poiché i metodi non prevedono originale `ProductID` valore da passare. Pertanto, come illustrato nelle esercitazioni precedenti, si consiglia di rimuovere questa assegnazione di proprietà dalla sintassi dichiarativa o, al contrario, valore di questa proprietà viene impostata su `{0}`.

Dopo questa modifica, markup dichiarativo di ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

Si noti che il `OldValuesParameterFormatString` proprietà è stata rimossa e che non vi è una `Parameter` nel `UpdateParameters` raccolta per ognuno dei parametri di input previsti dal nostro `UpdateProduct` rapporto di overload.

Mentre l'oggetto ObjectDataSource è configurato per aggiornare solo un subset di valori del prodotto, il controllo GridView è attualmente visualizzata *tutti* dei campi del prodotto. Si consiglia di modificare il controllo GridView in modo che:

- Include solo le `ProductName`, `SupplierName`, `CategoryName` BoundField e `Discontinued` CampoCasellaDiControllo
- Il `CategoryName` e `SupplierName` i campi da visualizzare prima (a sinistra del) il `Discontinued` CampoCasellaDiControllo
- Il `CategoryName` e `SupplierName` BoundField `HeaderText` è impostata su "Category" e "Fornitore", rispettivamente
- È abilitato il supporto di modifica (selezionare la casella di controllo Abilita modifica nello smart tag del controllo GridView)

Dopo tali modifiche, la finestra di progettazione avrà un aspetto simile alla figura 3, con sintassi dichiarativa di GridView illustrata di seguito.


[![Rimuovere i campi non necessari dal GridView](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**Figura 3**: rimuovere i campi non necessari dal GridView ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

Comportamento di sola lettura del controllo GridView a questo punto è stato completato. Quando si visualizzano i dati, ogni prodotto viene eseguito il rendering come una riga GridView, che mostra il nome del prodotto, categoria, fornitore e lo stato sospeso.


[![Interfaccia di sola lettura del controllo GridView è completa](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**Figura 4**: l'interfaccia di GridView di sola lettura è completa ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image12.png))


> [!NOTE]
> Come descritto nella [una panoramica di inserimento, aggiornamento ed eliminazione di dati esercitazione](an-overview-of-inserting-updating-and-deleting-data-cs.md), è estremamente importante che il controllo GridView lo stato di visualizzazione s essere abilitato (comportamento predefinito). Se si imposta la s GridView `EnableViewState` proprietà `false`, si corre il rischio di consentire agli utenti simultanei involontariamente l'eliminazione o modifica di record. Visualizzare [avviso: problema di concorrenza con ASP.NET 2.0 GridViews/DetailsView/FormViews tale supporto di modifica e/o l'eliminazione e il cui stato di visualizzazione è disabilitato](http://scottonwriting.net/sowblog/posts/10054.aspx) per altre informazioni.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Passaggio 3: Uso di un controllo DropDownList per la categoria e il fornitore interfacce di modifica

Si tenga presente che il `ProductsRow` oggetto contiene `CategoryID`, `CategoryName`, `SupplierID`, e `SupplierName` proprietà che specificano i valori di ID chiave esterna effettivi nel `Products` tabella e i corrispondenti database `Name` i valori di `Categories` e `Suppliers` tabelle. Il `ProductRow`del `CategoryID` e `SupplierID` possono essere entrambe essere letti e scritti, mentre il `CategoryName` e `SupplierName` proprietà sono di sola lettura.

A causa lo stato di sola lettura del `CategoryName` e `SupplierName` le proprietà, i BoundField corrispondente è stata loro `ReadOnly` proprietà impostata su `true`, impedendo che questi valori da modificare quando viene modificata una riga. Mentre è possibile impostare il `ReadOnly` proprietà `false`, il rendering le `CategoryName` e `SupplierName` BoundField come caselle di testo durante la modifica, questo approccio comporterà un'eccezione quando l'utente tenta di aggiornare il prodotto poiché non esiste alcun `UpdateProduct` overload che accetta `CategoryName` e `SupplierName` input. In realtà, non si desidera creare un overload di questo tipo per due motivi:

- Il `Products` la tabella non contiene `SupplierName` oppure `CategoryName` campi, ma `SupplierID` e `CategoryID`. Pertanto, il nostro metodo deve essere passato a questi particolari valori di ID, non i valori delle relative tabelle di ricerca.
- Richiedere all'utente di digitare il nome del fornitore o categoria è inferiore a quello ideale, poiché richiede all'utente di conoscere le categorie disponibili e fornitori e i grafie corrette.

I campi categoria e fornitore devono visualizzare la categoria e i nomi dei fornitori in modalità di sola lettura (come avviene, a questo punto) e un elenco di riepilogo a discesa di opzioni applicabili quando viene modificato. Usa un elenco di riepilogo a discesa, l'utente finale può visualizzare rapidamente quali categorie e i suoi fornitori sono disponibili per la selezione tra e possibile più facilmente la selezione.

Per fornire questo comportamento, è necessario convertire il `SupplierName` e `CategoryName` BoundField in TemplateFields cui `ItemTemplate` emette il `SupplierName` e `CategoryName` valori e il cui `EditItemTemplate` Usa un controllo DropDownList per l'elenco di categorie disponibili e fornitori.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Aggiungere il`Categories`e`Suppliers`controlli DropDownList

Iniziare convertendo il `SupplierName` e `CategoryName` BoundField in TemplateFields da: facendo clic sul collegamento Modifica colonne dallo smart tag del controllo GridView; selezionando i BoundField dall'elenco in basso a sinistra; e scegliendo la "Converti il campo in un Collegamento TemplateField". Il processo di conversione creerà un TemplateField sia con un `ItemTemplate` e un `EditItemTemplate`, come illustrato nella sintassi dichiarativa riportato di seguito:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

Poiché i BoundField è stato contrassegnato come di sola lettura, sia la `ItemTemplate` e `EditItemTemplate` contengono un'etichetta Web controllo la cui `Text` proprietà è associata al campo dati applicabile (`CategoryName`, nella sintassi precedente). È necessario modificare il `EditItemTemplate`, sostituendo il controllo etichetta Web con un controllo DropDownList.

Come illustrato nelle esercitazioni precedenti, il modello può essere modificato tramite la finestra di progettazione o direttamente dalla sintassi dichiarativa. Per la modifica tramite la finestra di progettazione, fare clic sul collegamento di modifica modelli dallo smart tag del controllo GridView e scegliere di utilizzare il campo di categoria `EditItemTemplate`. Rimuovere il controllo Web Label e sostituirlo con un controllo DropDownList, impostando la proprietà ID del controllo DropDownList `Categories`.


[![Rimuovere la casella di testo e aggiungere un controllo DropDownList per EditItemTemplate](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**Figura 5**: rimuovere la casella di testo e aggiungere un controllo DropDownList per il `EditItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image15.png))


È quindi necessario popolare il controllo DropDownList con le categorie disponibili. Fare clic sul collegamento dallo smart tag del controllo DropDownList Scegli origine dati e scegliere di creare un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource`.


[![Creare un nuovo controllo ObjectDataSource denominato CategoriesDataSource](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**Figura 6**: creare un nuovo controllo ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image18.png))


Per restituire tutte le categorie di ObjectDataSource, eseguirne l'associazione per il `CategoriesBLL` della classe `GetCategories()` (metodo).


[![Associare ObjectDataSource a GetCategories() metodo del CategoriesBLL](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**Figura 7**: associare ObjectDataSource per il `CategoriesBLL`del `GetCategories()` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image21.png))


Infine, configurare le impostazioni del controllo DropDownList in modo che il `CategoryName` campo viene visualizzato in ogni controllo DropDownList `ListItem` con il `CategoryID` utilizzato come valore del campo.


[![Avere il campo CategoryName visualizzato e il CategoryID utilizzato come valore](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**Figura 8**: sono il `CategoryName` campo visualizzato e il `CategoryID` utilizzato come valore ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image24.png))


Dopo apportare modifiche markup dichiarativo per la `EditItemTemplate` nella `CategoryName` TemplateField includerà un controllo DropDownList sia ObjectDataSource:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> DropDownList nel `EditItemTemplate` deve avere il proprio stato abilitato. Sintassi di associazione dati a breve verrà aggiunto alla sintassi dichiarativa di DropDownList e, ad esempio i comandi di Data Binding `Eval()` e `Bind()` può apparire solo nei controlli il cui stato di visualizzazione è abilitata.


Ripetere questi passaggi per aggiungere un controllo DropDownList denominato `Suppliers` per il `SupplierName` del TemplateField `EditItemTemplate`. Questo passaggio comporterà l'aggiunta di un controllo DropDownList per il `EditItemTemplate` e la creazione di un altro oggetto ObjectDataSource. Il `Suppliers` ObjectDataSource del controllo DropDownList, tuttavia, deve essere configurato per richiamare il `SuppliersBLL` della classe `GetSuppliers()` (metodo). Configurare anche il `Suppliers` DropDownList per visualizzare il `CompanyName` campo e usare il `SupplierID` campo come valore per relativo `ListItem` s.

Dopo l'aggiunta di controlli DropDownList ai due `EditItemTemplate` s, caricare la pagina in un browser e fare clic sul pulsante Modifica per il prodotto Seasoning Cajun di Chef Anton. Come illustrato nella figura 9, le colonne di categoria e il fornitore del prodotto vengono visualizzate come elenchi a discesa contenente le categorie e i fornitori, tra cui scegliere. Si noti tuttavia che il *primo* in entrambi gli elenchi di riepilogo a discesa sono selezionati per impostazione predefinita (Beverages per la categoria) e liquidi originali come fornitore, anche se Seasoning Cajun Chef Anton è per un condimento fornita da New Orleans Cajun Coinvolgente.


[![Per impostazione predefinita viene selezionato il primo elemento in elenchi dell'elenco a discesa](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**Figura 9**: l'elenco a discesa sono elencati il primo elemento è selezionato per impostazione predefinita ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image27.png))


Inoltre, se si fa clic su Aggiorna, si noterà che il prodotto `CategoryID` e `SupplierID` impostate sui valori `NULL`. Entrambi questi non desiderato i comportamenti causati perché i controlli DropDownList nel `EditItemTemplate` s non sono associati a tutti i campi dati dai dati sottostanti del prodotto.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Associazione di controlli DropDownList per il`CategoryID`e`SupplierID`campi dati

Per disporre di categoria e il fornitore del prodotto modificato elenchi a discesa impostare i valori appropriati e per avere questi valori con rinviati al livello BLL `UpdateProduct` metodo facendo clic sull'aggiornamento, è necessario associare i controlli DropDownList `SelectedValue` le proprietà per il `CategoryID` e `SupplierID` campi di dati con Data Binding bidirezionale. Per eseguire questa operazione con il `Categories` DropDownList, è possibile aggiungere `SelectedValue='<%# Bind("CategoryID") %>'` direttamente per la sintassi dichiarativa.

In alternativa, è possibile impostare databindings di DropDownList modificare il modello tramite la finestra di progettazione e scegliendo il collegamento di Modifica DataBindings dallo smart tag del controllo DropDownList. Successivamente, indicare che il `SelectedValue` proprietà deve essere associata ai `CategoryID` campo tramite associazione dati bidirezionale (vedere la figura 10). Ripetere il processo dichiarativo o della finestra di progettazione da associare il `SupplierID` campo dati e il `Suppliers` DropDownList.


[![Associare il CategoryID proprietà SelectedValue del DropDownList tramite associazione dati bidirezionale](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**Figura 10**: associa il `CategoryID` per il controllo DropDownList `SelectedValue` Data Binding bidirezionale di utilizzo di proprietà ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image30.png))


Dopo avere applicate le associazioni per il `SelectedValue` alle proprietà dei due controlli DropDownList, le colonne di categoria e il fornitore del prodotto modificato utilizzerà per impostazione predefinita i valori del prodotto corrente. Facendo clic sull'aggiornamento, il `CategoryID` e `SupplierID` i valori dell'elemento dell'elenco di riepilogo a discesa selezionato verranno passati al `UpdateProduct` (metodo). Figura 11 mostra l'esercitazione dopo l'aggiunta di istruzioni di associazione dati; Si noti come gli elementi dell'elenco di riepilogo a discesa selezionato per Chef Anton Cajun Seasoning siano correttamente per condimento e New Orleans Cajun coinvolgente.


[![Categoria corrente e i valori di fornitore del prodotto modificati vengono selezionati per impostazione predefinita](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**Figura 11**: la modifica e del prodotto categoria corrente Supplier valori sono selezionati per impostazione predefinita ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image33.png))


## <a name="handlingnullvalues"></a>La gestione`NULL`valori

Il `CategoryID` e `SupplierID` colonne il `Products` tabella può essere `NULL`, ma i controlli DropDownList nel `EditItemTemplate` storsimple non includono una voce di elenco per rappresentare un `NULL` valore. Questo comportamento ha due conseguenze:

- Utente non è possibile usare l'interfaccia per modificare categoria o fornitore di un prodotto da un non -`NULL` valore per un `NULL` uno
- Se un prodotto ha un `NULL` `CategoryID` o `SupplierID`, facendo clic sul pulsante Modifica genererà un'eccezione. Infatti il `NULL` valore restituito da `CategoryID` (o `SupplierID`) nel `Bind()` istruzione non esegue il mapping a un valore in DropDownList (DropDownList genera un'eccezione quando relativo `SelectedValue` proprietà è impostata su un valore *non* nella propria raccolta di elementi dell'elenco).

Per supportare `NULL` `CategoryID` e `SupplierID` i valori, è necessario aggiungere un altro `ListItem` per ogni controllo DropDownList per rappresentare il `NULL` valore. Nel [Master/Dettagli filtro con un controllo DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) esercitazione, è stato illustrato come aggiungere un ulteriore `ListItem` per un'associazione a dati DropDownList, quale coinvolta impostando il controllo DropDownList `AppendDataBoundItems` proprietà `true` e aggiunta manuale di aggiuntivo `ListItem`. In tale esercitazione precedente, tuttavia, abbiamo aggiunto un `ListItem` con un `Value` di `-1`. Tuttavia, la logica di associazione dati in ASP.NET verrà convertita automaticamente una stringa vuota per un `NULL` valore e a-viceversa. Di conseguenza, per questa esercitazione è necessario il `ListItem`del `Value` sia una stringa vuota.

Avvio tramite l'impostazione due controlli DropDownList `AppendDataBoundItems` proprietà `true`. Successivamente, aggiungere il `NULL` `ListItem` aggiungendo il codice seguente `<asp:ListItem>` elemento per ogni controllo DropDownList in modo che risulti markup dichiarativo, ad esempio:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

Ho scelto di utilizzare "(None)" come valore di testo per l'oggetto `ListItem`, ma è possibile modificare in modo che sia anche una stringa vuota se si desidera.

> [!NOTE]
> Come abbiamo visto nel *Master/Dettagli filtro con un controllo DropDownList* esercitazione `ListItem` s può essere aggiunto a un controllo DropDownList tramite la finestra di progettazione facendo clic del controllo DropDownList `Items` proprietà nella finestra Proprietà (che verrà visualizzato il `ListItem` Editor della raccolta). Tuttavia, assicurarsi di aggiungere il `NULL` `ListItem` per questa esercitazione tramite la sintassi dichiarativa. Se si usa la `ListItem` Editor della raccolta, la sintassi dichiarativa generata ometterà il `Value` impostazione completamente quando assegnato a una stringa vuota, la creazione di markup dichiarativo simile a quello: `<asp:ListItem>(None)</asp:ListItem>`. Mentre questo potrebbe avere un aspetto innocuo, il valore mancante fa sì che il controllo DropDownList usare il `Text` valore della proprietà al suo posto. Ciò significa che se si `NULL` `ListItem` è selezionata, il valore "(None)" verrà tentato da assegnare al `CategoryID`, verrà visualizzato un risultato di un'eccezione. Impostando in modo esplicito `Value=""`, una `NULL` verrà assegnato al valore `CategoryID` quando la `NULL` `ListItem` sia selezionata.


Ripetere questi passaggi per il controllo DropDownList Suppliers.

Con questo aggiuntiva `ListItem`, ora è possibile assegnare l'interfaccia di modifica `NULL` valori a un prodotto `CategoryID` e `SupplierID` campi, come illustrato nella figura 12.


[![(Nessuno) scegliere di assegnare un valore NULL per la categoria o al fornitore del prodotto](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**Figura 12**: (None) scegliere di assegnare un `NULL` valore per la categoria o al fornitore del prodotto ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Passaggio 4: Uso di pulsanti di opzione per lo stato non più supportato

Attualmente i prodotti `Discontinued` campo dati viene espresso mediante un CampoCasellaDiControllo, che esegue il rendering di una casella di controllo disabilitata per le righe di sola lettura e una casella di controllo abilitato per la riga da modificare. Anche se questa interfaccia utente è spesso adatta, è possibile personalizzarlo se necessario, utilizzando un TemplateField. Per questa esercitazione, è possibile modificare il CampoCasellaDiControllo in un TemplateField che usa un controllo RadioButtonList con due opzioni "Attive" e "Non più disponibile" da cui l'utente può specificare il prodotto `Discontinued` valore.

Iniziare convertendo il `Discontinued` CampoCasellaDiControllo in un TemplateField, che creerà un TemplateField con un' `ItemTemplate` e `EditItemTemplate`. Entrambi i modelli includono una casella di controllo con relativo `Checked` proprietà associata ai `Discontinued` datové pole, l'unica differenza tra i due è che il `ItemTemplate`della casella di controllo `Enabled` viene impostata su `false`.

Sostituire la casella di controllo sia nel `ItemTemplate` e `EditItemTemplate` con un controllo RadioButtonList, l'impostazione entrambi RadioButtonList `ID` delle proprietà per `DiscontinuedChoice`. Successivamente, indicare il RadioButtonList deve ciascuna delle quali contiene due pulsanti di opzione, uno con etichettato "attivo" con il valore "False" e uno con l'etichetta "Sospeso" con valore "True". A tale scopo è possibile immettere il `<asp:ListItem>` elementi in direttamente tramite la sintassi dichiarativa o l'uso di `ListItem` Editor della raccolta dalla finestra di progettazione. Figura 13 Mostra il `ListItem` Editor della raccolta dopo che le due opzioni di un pulsante di opzione sono state specificate.


[![Aggiungere](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**Figura 13**: aggiungere "Attivo" e "Sospeso" Opzioni RadioButtonList ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image39.png))


Poiché RadioButtonList nel `ItemTemplate` non deve essere modificabile, impostare relativi `Enabled` proprietà `false`, lasciando il `Enabled` proprietà `true` (predefinito) per RadioButtonList nel `EditItemTemplate`. Ciò renderà i pulsanti di opzione nella riga non modificata in sola lettura, ma consente all'utente di modificare i valori di pulsante di opzione per la riga modificata.

È comunque necessario assegnare dei controlli RadioButtonList `SelectedValue` delle proprietà in modo che sia selezionato il pulsante di opzione appropriato in base al momento del prodotto `Discontinued` campo dati. Come con i controlli DropDownList esaminato in precedenza in questa esercitazione, questa sintassi di associazione dati può essere aggiunto direttamente nel markup dichiarativo o tramite il collegamento di Modifica DataBindings di smart tag degli RadioButtonList.

Dopo aver aggiunto i due RadioButtonList e configurarli, il `Discontinued` markup dichiarativo di TemplateField simile a:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

Con queste modifiche, il `Discontinued` colonna è stata trasformata in un elenco di caselle di controllo per un elenco di coppie di pulsante di opzione (vedere la figura 14). Quando si modifica un prodotto, si seleziona il pulsante di opzione appropriato e lo stato di non più utilizzate del prodotto può essere aggiornato selezionando il pulsante di opzione e fare clic su Aggiorna.


[![Le caselle di controllo non più supportate sono state sostituite dalle coppie di pulsante di opzione](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**Figura 14**: il non più disponibili le caselle di controllo sono state sostituite dalle coppie di pulsante di opzione ([fare clic per visualizzare l'immagine con dimensioni normali](customizing-the-data-modification-interface-cs/_static/image42.png))


> [!NOTE]
> Poiché il `Discontinued` colonna il `Products` database non può contenere `NULL` valori, non è necessario preoccuparsi di acquisizione `NULL` informazioni nell'interfaccia. Se, tuttavia `Discontinued` colonna può contenere `NULL` valori si potrebbe voler aggiungere un terzo pulsante all'elenco con relativo `Value` impostato su una stringa vuota (`Value=""`), proprio come con la categoria e il fornitore controlli DropDownList.


## <a name="summary"></a>Riepilogo

Mentre i BoundField e CampoCasellaDiControllo automaticamente il rendering di sola lettura, modifica e inserimento di interfacce, hanno la possibilità di personalizzazione. Spesso, tuttavia, è necessario personalizzare il processo di modifica o l'inserimento di interfaccia, ad esempio aggiungendo i controlli di convalida (come mostrato nell'esercitazione precedente) o personalizzando l'interfaccia utente di raccolta dati (come mostrato in questa esercitazione). Personalizzazione dell'interfaccia con un TemplateField possono essere ricondotte nei passaggi seguenti:

1. Aggiungere un TemplateField o convertire un BoundField esistenti o CampoCasellaDiControllo in un TemplateField
2. Potenziare l'interfaccia in base alle esigenze
3. Associare i campi dati appropriata ai controlli Web appena aggiunti tramite associazione dati bidirezionale

Oltre a usare i controlli Web ASP.NET incorporati, è anche possibile personalizzare i modelli di un TemplateField con i controlli server personalizzati, compilato e controlli utente.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
> [Successivo](implementing-optimistic-concurrency-cs.md)
