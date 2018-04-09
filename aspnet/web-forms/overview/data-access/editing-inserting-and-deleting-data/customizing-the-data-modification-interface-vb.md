---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: Personalizzazione dell'interfaccia di modifica di dati (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione viene illustrato come personalizzare l'interfaccia di un controllo GridView modificabile, sostituendo la casella di testo standard e controlla la casella di controllo con alternati...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 7d334a86fcf2fbd1069628527c6e89f3ab655dd5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="customizing-the-data-modification-interface-vb"></a>Personalizzazione dell'interfaccia di modifica di dati (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) o [Scarica il PDF](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> In questa esercitazione viene illustrato come personalizzare l'interfaccia di un controllo GridView modificabile, sostituendo la casella di testo standard e controlla la casella di controllo con i controlli Web di input alternativi.


## <a name="introduction"></a>Introduzione

Il BoundField e CheckBoxFields usato dai controlli GridView e DetailsView semplificano il processo di modifica dei dati a causa delle loro capacità di eseguire il rendering di sola lettura, modificabile e inseribile interfacce. Queste interfacce possono essere visualizzate senza la necessità di aggiungere eventuali ulteriori markup dichiarativo o codice. Tuttavia, interfacce di BoundField e del CheckBoxField mancano il possibilità spesso necessarie in scenari reali. Per personalizzare l'interfaccia modificabile o inseribile in un controllo GridView o DetailsView è necessario usare invece un TemplateField.

Nel [esercitazione precedente](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) è stato illustrato come personalizzare le interfacce di modifica dei dati mediante l'aggiunta di controlli Web di convalida. In questa esercitazione viene illustrato come personalizzare i dati effettivi raccolta controlli Web, sostituendo il BoundField e casella di testo standard della CheckBoxField e i controlli casella di controllo con i controlli Web di input alternativi. In particolare, verranno creati un GridView modificabile che consente a un prodotto nome, categoria, fornitore e lo stato non più supportato da aggiornare. Quando si modifica una determinata riga, i campi categoria e fornitore vengono visualizzati come controlli DropDownList, contenente il set di categorie e fornitori, tra cui scegliere. Inoltre, sostituiremo predefinito dell'elemento CheckBoxField casella di controllo con un controllo RadioButtonList che sono disponibili due opzioni: "Attivo" e "Sospeso".


[![Interfaccia di modifica del controllo GridView include controlli DropDownList e pulsanti di opzione](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Figura 1**: modificare controlli include DropDownList interfaccia e pulsanti di opzione di GridView ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Passaggio 1: Creazione appropriata`UpdateProduct`Overload

In questa esercitazione verrà compilato un GridView modificabile che consenta la modifica del nome del prodotto, categoria, fornitore e lo stato sospeso. Pertanto, è necessario un `UpdateProduct` overload che accetta cinque parametri di input valori sono i seguenti quattro prodotto più il `ProductID`. Come nel nostro overload precedente, questa operazione una comporta:

1. Recuperare le informazioni sul prodotto dal database per l'oggetto specificato `ProductID`,
2. Aggiornamento di `ProductName`, `CategoryID`, `SupplierID`, e `Discontinued` campi, e
3. Inviare la richiesta di aggiornamento DAL tramite dell'oggetto TableAdapter `Update()` metodo.

Per ragioni di brevità, per questo particolare overload è stata omessa la verifica della regola business che garantisce un prodotto viene contrassegnato come non più disponibile non è l'unico prodotto offerto dal relativo fornitore. È possibile aggiungerla in se si preferisce, o, in teoria, effettuarne il refactoring out la logica per un metodo separato.

Il codice seguente viene illustrata la nuova `UpdateProduct` overload nella `ProductsBLL` classe:


[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Passaggio 2: Creazione di GridView modificabile

Con il `UpdateProduct` aggiunto overload, si è pronti per creare il nostro GridView modificabile. Aprire il `CustomizedUI.aspx` nella pagina di `EditInsertDelete` cartella e aggiungere un controllo GridView nella finestra di progettazione. Successivamente, creare un nuovo oggetto ObjectDataSource smart tag del controllo GridView. Configurare ObjectDataSource per recuperare informazioni sul prodotto tramite il `ProductBLL` della classe `GetProducts()` (metodo) e aggiornare i dati di prodotto usando il `UpdateProduct` overload appena creato. Dalla scheda INSERT e DELETE, selezionare (nessuno) dagli elenchi a discesa.


[![Configurare ObjectDataSource per utilizzare l'Overload UpdateProduct appena creato](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**Figura 2**: configurare ObjectDataSource per usare il `UpdateProduct` Overload appena creato ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image6.png))


Come abbiamo visto durante le esercitazioni di modifica di dati, la sintassi dichiarativa per ObjectDataSource creata da Visual Studio assegna il `OldValuesParameterFormatString` proprietà `original_{0}`. Ciò, naturalmente, non funzionerà con il livello di logica di Business poiché i metodi non si prevedono l'originale `ProductID` valore deve essere passato. Pertanto, come che abbiamo realizzato nelle esercitazioni precedenti, è opportuno rimuovere questa assegnazione di proprietà con la sintassi dichiarativa o, invece, impostare il valore di questa proprietà su `{0}`.

Dopo questa modifica, markup dichiarativo di ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Si noti che il `OldValuesParameterFormatString` proprietà è stata rimossa e che sia un `Parameter` nel `UpdateParameters` raccolta per ognuno dei parametri di input previsti dal nostro `UpdateProduct` rapporto di overload.

Mentre ObjectDataSource è configurato per aggiornare solo un sottoinsieme dei valori di prodotto, è attualmente visualizzata GridView *tutti* dei campi del prodotto. È opportuno modificare GridView in modo che:

- Include solo il `ProductName`, `SupplierName`, `CategoryName` BoundField e `Discontinued` CheckBoxField
- Il `CategoryName` e `SupplierName` campi da visualizzare prima (a sinistra del) il `Discontinued` CheckBoxField
- Il `CategoryName` e `SupplierName` BoundField `HeaderText` proprietà è impostata rispettivamente su "Category" e "Fornitore",
- È abilitato il supporto di modifica (selezionare la casella di controllo Abilita modifica smart tag del controllo GridView)

Dopo aver apportato queste modifiche, la finestra di progettazione sarà simile alla figura 3, con la sintassi dichiarativa di GridView illustrata di seguito.


[![Rimuovere i campi non necessari dal controllo GridView.](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Figura 3**: rimuovere i campi non necessari dal controllo GridView ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

A questo punto, il comportamento di sola lettura di GridView è stato completato. Quando si visualizzano i dati, ogni prodotto viene eseguito il rendering come una riga in GridView, che mostra il nome del prodotto, categoria, fornitore e lo stato sospeso.


[![Interfaccia di sola lettura di GridView è completato.](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Figura 4**: interfaccia del GridView di sola lettura è completa ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image12.png))


> [!NOTE]
> Come descritto in [esercitazione una panoramica di inserimento, aggiornamento ed eliminazione di dati](an-overview-of-inserting-updating-and-deleting-data-cs.md), è estremamente importante che lo stato di visualizzazione s essere GridView abilitato (comportamento predefinito). Se si imposta la s GridView `EnableViewState` proprietà `false`, si corre il rischio di consentire agli utenti simultanei accidentalmente l'eliminazione o la modifica di record. Vedere [avviso: problema di concorrenza con ASP.NET 2.0 GridView/DetailsView/FormViews che supportano la modifica e/o l'eliminazione e il cui stato di visualizzazione è disabilitato](http://scottonwriting.net/sowblog/posts/10054.aspx) per ulteriori informazioni.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Passaggio 3: Utilizzo di un DropDownList per la categoria e un fornitore, la modifica delle interfacce

Tenere presente che il `ProductsRow` oggetto contiene `CategoryID`, `CategoryName`, `SupplierID`, e `SupplierName` le proprietà che forniscono i valori di ID effettivi di chiave esterna nel `Products` tabella e i corrispondenti database `Name` i valori di `Categories` e `Suppliers` tabelle. Il `ProductRow`del `CategoryID` e `SupplierID` possono essere entrambi essere letti e scritti, mentre il `CategoryName` e `SupplierName` proprietà sono di sola lettura.

A causa dello stato di sola lettura del `CategoryName` e `SupplierName` proprietà, il BoundField corrispondente è stato loro `ReadOnly` proprietà impostata su `True`, impedendo che questi valori viene modificata quando viene modificata una riga. Mentre è possibile impostare il `ReadOnly` proprietà `False`, il rendering di `CategoryName` e `SupplierName` BoundField come caselle di testo durante la modifica, tale approccio genererà un'eccezione quando l'utente tenta di aggiornare il prodotto poiché non esiste alcun `UpdateProduct` overload che accetta `CategoryName` e `SupplierName` input. Infatti, non si desidera creare un overload di questo tipo per due motivi:

- Il `Products` la tabella non contiene `SupplierName` o `CategoryName` campi, ma `SupplierID` e `CategoryID`. Pertanto, il metodo di deve essere passato a questi valori di ID specifici, non i valori delle relative tabelle di ricerca.
- Richiedere all'utente di digitare il nome del fornitore o categoria è minore di ideale, poiché richiede all'utente di conoscere le categorie disponibili e i fornitori e i relativi suggerite.

I campi categoria e fornitore devono visualizzare la categoria e i nomi dei fornitori in modalità di sola lettura (come avviene ora) e un elenco di riepilogo a discesa di opzioni applicabili quando viene modificato. Utilizza un elenco a discesa, l'utente finale è possibile visualizzare rapidamente quali categorie e i fornitori sono disponibili per la scelta tra e più facilmente effettuabili loro selezione.

Per fornire questo comportamento, è necessario convertire il `SupplierName` e `CategoryName` BoundField in TemplateFields cui `ItemTemplate` genera il `SupplierName` e `CategoryName` valori e il cui `EditItemTemplate` Usa un controllo DropDownList all'elenco di le categorie disponibili e fornitori.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Aggiunta di`Categories`e`Suppliers`controlli DropDownList

Avviare convertendo il `SupplierName` e `CategoryName` BoundField in TemplateFields da: facendo clic sul collegamento Modifica colonne smart tag del controllo GridView; selezionando nell'elenco in basso a sinistra; il BoundField e facendo clic sul "Converti il campo in un Collegamento TemplateField". Il processo di conversione creerà un TemplateField con entrambi un `ItemTemplate` e `EditItemTemplate`, come illustrato nella sintassi dichiarativa seguente:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Poiché il BoundField è stato contrassegnato come di sola lettura, sia il `ItemTemplate` e `EditItemTemplate` contengono un'etichetta di Web controllo cui `Text` proprietà è associata al campo dati applicabili (`CategoryName`, nella sintassi precedente). È necessario modificare il `EditItemTemplate`, sostituendo il controllo etichetta Web con un controllo DropDownList.

Come abbiamo visto nelle esercitazioni precedenti, il modello può essere modificato tramite la finestra di progettazione o direttamente la sintassi dichiarativa. Per la modifica tramite la finestra di progettazione, fare clic sul collegamento Modifica modelli smart tag del controllo GridView e scegliere di utilizzare il campo categoria `EditItemTemplate`. Rimuovere il controllo Web Label e sostituirlo con un controllo DropDownList, impostare proprietà ID del DropDownList `Categories`.


[![Rimuovere il TextBox e aggiungere un controllo DropDownList EditItemTemplate](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Figura 5**: rimuovere il TextBox e aggiungere un controllo DropDownList per il `EditItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image15.png))


Successivamente è necessario popolare DropDownList con le categorie disponibili. Fare clic sul collegamento smart tag del DropDownList Scegli origine dati e scegliere di creare un nuovo oggetto ObjectDataSource denominato `CategoriesDataSource`.


[![Creare un nuovo controllo ObjectDataSource denominato CategoriesDataSource](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Figura 6**: creare un nuovo controllo ObjectDataSource denominato `CategoriesDataSource` ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image18.png))


Per restituire tutte le categorie ObjectDataSource, associarlo al `CategoriesBLL` della classe `GetCategories()` metodo.


[![Associare ObjectDataSource al metodo GetCategories() del CategoriesBLL](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Figura 7**: associare ObjectDataSource per la `CategoriesBLL`del `GetCategories()` metodo ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image21.png))


Infine, configurare le impostazioni del DropDownList in modo che il `CategoryName` campo viene visualizzato in ogni DropDownList `ListItem` con il `CategoryID` utilizzato come valore del campo.


[![Dispone il campo CategoryName visualizzato e il CategoryID utilizzato come il valore](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Figura 8**: sono il `CategoryName` campo visualizzato e il `CategoryID` utilizzato come valore ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image24.png))


Dopo avere apportare queste modifiche markup dichiarativo per la `EditItemTemplate` nel `CategoryName` TemplateField includerà un DropDownList sia ObjectDataSource:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> DropDownList nel `EditItemTemplate` deve avere il proprio stato abilitato. Non appena si aggiungeranno sintassi di associazione dati per la sintassi dichiarativa del DropDownList e comandi di associazione dati come `Eval()` e `Bind()` può trovarsi solo nei controlli il cui stato di visualizzazione è abilitata.


Ripetere questi passaggi per aggiungere un controllo DropDownList denominato `Suppliers` per il `SupplierName` del TemplateField `EditItemTemplate`. Questo comporta l'aggiunta di un controllo DropDownList per il `EditItemTemplate` e la creazione di un altro ObjectDataSource. Il `Suppliers` ObjectDataSource del DropDownList, tuttavia, deve essere configurato per richiamare il `SuppliersBLL` della classe `GetSuppliers()` metodo. Configurare inoltre il `Suppliers` DropDownList per visualizzare il `CompanyName` campo e utilizzare il `SupplierID` campo come valore per il relativo `ListItem` s.

Dopo l'aggiunta di controlli DropDownList ai due `EditItemTemplate` s, caricare la pagina in un browser e fare clic sul pulsante Modifica per il prodotto Cajun Seasoning di Chef Anton. Come illustrato nella figura 9, le colonne di categoria e il fornitore del prodotto vengono visualizzate come elenchi a discesa contenente le categorie e i fornitori, tra cui scegliere. Si noti tuttavia che il *prima* in entrambi gli elenchi a discesa sono selezionati per impostazione predefinita (bevande per la categoria) e liquide esotiche come il fornitore, anche se Chef Anton Cajun Seasoning è per un condimento fornito da New Orleans Cajun Delights.


[![Per impostazione predefinita viene selezionato il primo elemento in elenchi di elenco a discesa](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Figura 9**: per impostazione predefinita viene selezionato il primo elemento in elenchi di elenco a discesa ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image27.png))


Inoltre, se si sceglie di aggiornamento, si noterà che il prodotto `CategoryID` e `SupplierID` valori vengono impostati su `NULL`. Entrambi questi indesiderati comportamenti causati perché i controlli DropDownList nel `EditItemTemplate` s non sono associati a tutti i campi dati dai dati sottostanti del prodotto.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Associazione di controlli DropDownList per il`CategoryID`e`SupplierID`campi dati

Per disporre di categoria e il fornitore del prodotto modificato elenchi a discesa impostare i valori appropriati e di disporre di questi valori inviati il livello Business LOGIC `UpdateProduct` metodo facendo clic sull'aggiornamento, è necessario associare i controlli DropDownList `SelectedValue` proprietà per il `CategoryID` e `SupplierID` campi di dati utilizzando l'associazione dati bidirezionale. Per eseguire questa operazione con il `Categories` DropDownList, è possibile aggiungere `SelectedValue='<%# Bind("CategoryID") %>'` direttamente per la sintassi dichiarativa.

In alternativa, è possibile impostare i databindings del DropDownList la modifica del modello tramite la finestra di progettazione e scegliendo il collegamento Modifica DataBindings smart tag del DropDownList. Successivamente, indicano che il `SelectedValue` proprietà deve essere associata al `CategoryID` campo con associazione a dati bidirezionale (vedere la figura 10). Ripetere il processo della finestra di progettazione o dichiarativo per associare il `SupplierID` campo dati e il `Suppliers` DropDownList.


[![Associare il CategoryID alla proprietà SelectedValue di DropDownList utilizzando l'associazione dati bidirezionale](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Figura 10**: associare il `CategoryID` per il controllo DropDownList `SelectedValue` Data Binding bidirezionale di utilizzo di proprietà ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image30.png))


Dopo che sono stati applicati i binding per il `SelectedValue` delle proprietà di una di due controlli DropDownList, le colonne di categoria e il fornitore del prodotto modificato utilizzerà i valori del prodotto corrente. Facendo clic sull'aggiornamento, il `CategoryID` e `SupplierID` valori di elemento di elenco a discesa selezionato verranno passati il `UpdateProduct` metodo. Figura 11 mostra l'esercitazione dopo le istruzioni di associazione dati sono stati aggiunti; Si noti come gli elementi di elenco a discesa selezionato per Chef Anton Cajun Seasoning vengono correttamente per condimento e Bolzano Cajun Delights.


[![Categoria corrente del prodotto modificato e fornitore valori selezionati per impostazione predefinita](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Figura 11**: la modifica del prodotto categoria corrente e fornitore valori selezionati per impostazione predefinita ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image33.png))


## <a name="handlingnullvalues"></a>Gestione`NULL`valori

Il `CategoryID` e `SupplierID` colonne il `Products` tabella può essere `NULL`, ma i controlli DropDownList nel `EditItemTemplate` s non include un elemento di elenco per rappresentare un `NULL` valore. Questo comportamento ha due conseguenze:

- Utente non può utilizzare l'interfaccia per modificare fornitore o categoria di un prodotto da un non -`NULL` valore un `NULL` uno
- Se un prodotto è un `NULL` `CategoryID` o `SupplierID`, fare clic sul pulsante Modifica verrà generata un'eccezione. Infatti la `NULL` valore restituito da `CategoryID` (o `SupplierID`) nel `Bind()` istruzione non è mappato a un valore in DropDownList (DropDownList genera un'eccezione quando il relativo `SelectedValue` proprietà è impostata su un valore *non* nella propria raccolta di elementi dell'elenco).

Per supportare `NULL` `CategoryID` e `SupplierID` valori, è necessario aggiungere un altro `ListItem` per ogni DropDownList per rappresentare il `NULL` valore. Nel [Master-Details filtro con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) dell'esercitazione, è stato illustrato come aggiungere un ulteriore `ListItem` a un'associazione a dati DropDownList, quale coinvolti impostando il DropDownList `AppendDataBoundItems` proprietà `True` e aggiunta manuale di aggiuntiva `ListItem`. In tale esercitazione precedente, tuttavia, è aggiunto un `ListItem` con un `Value` di `-1`. Tuttavia, la logica di associazione dati in ASP.NET, verrà convertiti automaticamente in una stringa vuota per un `NULL` valore e a-viceversa. Di conseguenza, per questa esercitazione è necessario il `ListItem`del `Value` per essere una stringa vuota.

Inizio impostando entrambi controlli DropDownList `AppendDataBoundItems` proprietà `True`. Successivamente, aggiungere il `NULL` `ListItem` aggiungendo quanto segue `<asp:ListItem>` come elemento per ogni DropDownList in modo che il markup dichiarativo:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

Ho scelto di utilizzare "(None)" come valore di testo per questo `ListItem`, ma è possibile modificare in modo che sia anche una stringa vuota se si desidera.

> [!NOTE]
> Come illustrato nel *Master-Details filtro con un DropDownList* esercitazione `ListItem` s è possibile aggiungere a un controllo DropDownList tramite la finestra di progettazione facendo clic su di DropDownList `Items` proprietà nella finestra Proprietà (che verrà visualizzato il `ListItem` Editor della raccolta). Tuttavia, assicurarsi di aggiungere il `NULL` `ListItem` per questa esercitazione tramite la sintassi dichiarativa. Se si utilizza il `ListItem` Editor della raccolta, la sintassi dichiarativa generata ometterà il `Value` impostazione completamente quando assegnato a una stringa vuota, la creazione di markup dichiarativo, ad esempio: `<asp:ListItem>(None)</asp:ListItem>`. Anche se potrebbero risultare innocuo, il valore mancante parte DropDownList utilizzare il `Text` valore della proprietà al suo posto. Ciò significa che se questo `NULL` `ListItem` è selezionata, il valore "(None)" verrà tentato da assegnare al `CategoryID`, che verrà generata un'eccezione. L'impostazione esplicita `Value=""`, un `NULL` valore verrà assegnato alla `CategoryID` quando il `NULL` `ListItem` è selezionata.


Ripetere questi passaggi per DropDownList fornitori.

Con questo aggiuntive `ListItem`, l'interfaccia di modifica a questo punto è possibile assegnare `NULL` valori a un prodotto `CategoryID` e `SupplierID` campi, come illustrato nella figura 12.


[![(None) scegliere di assegnare un valore NULL per categoria o un fornitore del prodotto](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Figura 12**: scegliere (nessuno) per assegnare un `NULL` valore per la categoria di un prodotto o fornitore ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Passaggio 4: Utilizzo di pulsanti di opzione per lo stato non più supportato

Attualmente i prodotti `Discontinued` campo dati viene espresso mediante un CheckBoxField, che esegue il rendering di una casella di controllo disabilitata per le righe di sola lettura e una casella di controllo abilitato per la riga da modificare. Anche se questa interfaccia utente è spesso adatta, è possibile personalizzarlo se necessario, utilizzando un TemplateField. Per questa esercitazione, è necessario modificare il CheckBoxField in un TemplateField che utilizza un controllo RadioButtonList con due opzioni "Attive" e "Sospeso" da cui l'utente può specificare il prodotto `Discontinued` valore.

Avviare convertendo il `Discontinued` CheckBoxField in un TemplateField, che creerà un TemplateField con un `ItemTemplate` e `EditItemTemplate`. Entrambi i modelli includono una casella di controllo con relativo `Checked` proprietà associata al `Discontinued` campo dati, l'unica differenza tra i due in cui il `ItemTemplate`della casella di controllo `Enabled` è impostata su `False`.

Sostituire la casella di controllo in entrambe la `ItemTemplate` e `EditItemTemplate` con un controllo RadioButtonList, l'impostazione entrambi RadioButtonList `ID` proprietà `DiscontinuedChoice`. Successivamente, indicare il RadioButtonList deve ciascuna delle quali contiene due pulsanti di opzione, uno con etichettato "attivo" con valore "False" e uno con l'etichetta "Sospeso" con un valore "True". A tale scopo è possibile immettere il `<asp:ListItem>` elementi direttamente tramite la sintassi dichiarativa o l'uso di `ListItem` Editor della raccolta dalla finestra di progettazione. Figura 13 illustra il `ListItem` Editor della raccolta dopo i due opzioni pulsante di opzione sono state specificate.


[![Aggiungere opzioni attive e non più supportate a RadioButtonList](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Figura 13**: aggiungere Active e non più disponibili le opzioni per RadioButtonList ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image39.png))


Poiché RadioButtonList nel `ItemTemplate` non deve essere modificabile, impostare relativo `Enabled` proprietà `False`, lasciando il `Enabled` proprietà `True` (predefinito) per RadioButtonList nel `EditItemTemplate`. Renderà i pulsanti di opzione nella riga non modificata come di sola lettura, ma consente all'utente di modificare i valori di RadioButton per la riga modificata.

È necessario assegnare dei controlli RadioButtonList `SelectedValue` proprietà in modo che sia selezionato il pulsante di opzione appropriato in base al momento del prodotto `Discontinued` campo dati. Come con i controlli DropDownList esaminato in precedenza in questa esercitazione, questa sintassi di associazione dati può essere aggiunta direttamente nel markup dichiarativo o tramite il collegamento Modifica DataBindings in smart tag degli RadioButtonList.

Dopo l'aggiunta di due RadioButtonList e configurarli, il `Discontinued` markup dichiarativo del TemplateField dovrebbe essere simile:


[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Con queste modifiche, il `Discontinued` colonna è stata trasformata da un elenco di caselle di controllo per un elenco di coppie di pulsante di opzione (vedere Figura 14). Quando si modifica un prodotto, viene selezionato il pulsante di opzione appropriato e lo stato di funzionalità del prodotto può essere aggiornato selezionando il pulsante di opzione e fare clic su Aggiorna.


[![Le caselle di controllo non più supportate sono state sostituite dalle coppie di pulsante di opzione](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Figura 14**: il non più disponibili le caselle di controllo sono state sostituite dalle coppie di pulsante di opzione ([fare clic per visualizzare l'immagine ingrandita](customizing-the-data-modification-interface-vb/_static/image42.png))


> [!NOTE]
> Poiché il `Discontinued` colonna il `Products` database non può contenere `NULL` valori, non occorre preoccuparsi di acquisizione `NULL` informazioni nell'interfaccia. Se, tuttavia, `Discontinued` colonna potrebbe contenere `NULL` valori che si desidera aggiungere un terzo RadioButton all'elenco con il relativo `Value` impostato su una stringa vuota (`Value=""`), proprio come con la categoria e un fornitore controlli DropDownList.


## <a name="summary"></a>Riepilogo

Mentre il BoundField e CheckBoxField automaticamente il rendering di sola lettura, modifica e inserimento di interfacce, dispongono del possibilità per la personalizzazione. Spesso, tuttavia, sarà necessario personalizzare il processo di modifica o l'inserimento di interfaccia, ad esempio aggiungendo i controlli di convalida (come illustrato nell'esercitazione precedente) o personalizzando l'interfaccia utente di raccolta dati (come illustrato in questa esercitazione). Personalizzazione dell'interfaccia con un TemplateField possono essere sommata nei passaggi seguenti:

1. Aggiungere un TemplateField o convertire un CheckBoxField BoundField esistente in un TemplateField
2. Aumentare l'interfaccia in base alle esigenze
3. Associare i campi di dati appropriato per i controlli Web appena aggiunti mediante associazione dati bidirezionale

Oltre a utilizzare i controlli Web ASP.NET predefiniti, è anche possibile personalizzare i modelli di un TemplateField con server personalizzato, compilato e controlli utente.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Precedente](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [Successivo](implementing-optimistic-concurrency-vb.md)
