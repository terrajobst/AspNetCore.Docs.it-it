---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Batch di aggiornamento (VB) | Documenti Microsoft
author: rick-anderson
description: "Informazioni su come aggiornare più record di database in un'unica operazione. Il Layer dell'interfaccia utente è compilazione di un controllo GridView in cui ogni riga è modificabile. Nei dati..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: bcfdf734de0b4a4aa0a11f35bd6e40d6b97719cf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="batch-updating-vb"></a>Batch di aggiornamento (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) o [Scarica il PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Informazioni su come aggiornare più record di database in un'unica operazione. Il Layer dell'interfaccia utente è compilazione di un controllo GridView in cui ogni riga è modificabile. Nel livello di accesso ai dati si eseguono il wrapping di più operazioni di aggiornamento all'interno di una transazione per garantire che tutti gli aggiornamenti completata o vengono eseguito il rollback di tutti gli aggiornamenti.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](wrapping-database-modifications-within-a-transaction-vb.md) è stato illustrato come estendere il livello di accesso ai dati per aggiungere supporto per le transazioni di database. Le transazioni di database garantiscono che una serie di istruzioni di modifica dei dati verrà considerata come una singola operazione atomica, che assicura che tutte le modifiche avranno esito negativo o tutti avrà esito positivo. Con DAL funzionalità di basso livello in modo, è re pronti per passare alle interfacce di modifica di dati di creazione di batch.

In questa esercitazione verrà creato un controllo GridView in cui ogni riga è modificabile (vedere Figura 1). Poiché ogni riga non viene eseguito il rendering nella relativa interfaccia di modifica, tale s è necessario per una colonna di modifica, aggiornare e annullare i pulsanti. Nella pagina sono invece presenti due pulsanti di aggiornare i prodotti che, quando si fa clic, enumerare le righe di GridView e aggiornare il database.


[![Ogni riga in GridView è modificabile](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Figura 1**: ogni riga in GridView è modificabile ([fare clic per visualizzare l'immagine ingrandita](batch-updating-vb/_static/image2.png))


Let s iniziare!

> [!NOTE]
> Nel [esecuzione di aggiornamenti Batch](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) esercitazione è stato creato un batch Modifica interfaccia utilizza il controllo DataList. In questa esercitazione è diverso dal precedente in cui è utilizzato un GridView e l'aggiornamento batch viene eseguito all'interno dell'ambito di una transazione. Dopo aver completato questa esercitazione consiglia di ripristinare l'esercitazione precedente e aggiornarlo per utilizzare la funzionalità correlate alle transazioni del database aggiunta nell'esercitazione precedente.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Esaminare i passaggi per rendere modificabile tutte le righe di GridView

Come descritto nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) esercitazione GridView offre supporto incorporato per la modifica i dati sottostanti per ogni riga. Internamente, GridView note può essere modificata tramite la riga relativa [ `EditIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Controlla come viene associato il controllo GridView all'origine dati, ogni riga per vedere se l'indice della riga è uguale al valore di `EditIndex`. In questo caso, le interfacce s i campi vengono visualizzati utilizzando la loro modifica tale riga. Per BoundField, l'interfaccia di modifica è una casella di testo la cui `Text` proprietà viene assegnato il valore del campo di dati specificato da BoundField s `DataField` proprietà. Per TemplateFields, il `EditItemTemplate` è usato al posto di `ItemTemplate`.

Tenere presente che il flusso di lavoro di modifica viene avviata quando un utente fa clic su un pulsante Modifica riga s. Ciò provoca un postback, imposta s GridView `EditIndex` proprietà per l'indice di riga si fa clic s e riassociazioni i dati alla griglia. Quando viene selezionato un pulsante Annulla riga s durante il postback di `EditIndex` è impostata su un valore di `-1` prima i dati alla griglia di riassociazione. Poiché le righe di GridView s avviare l'indicizzazione da zero, l'impostazione `EditIndex` a `-1` ha l'effetto di GridView viene visualizzata in modalità di sola lettura.

Il `EditIndex` proprietà funziona anche per la modifica per riga, ma non è progettata per la modifica in blocco. Per rendere modificabile il GridView intero, è necessario eseguire il rendering tramite l'interfaccia di modifica per ogni riga. Il modo più semplice per eseguire questa operazione consiste nel creare in ogni campo modificabile viene implementato come un TemplateField con l'interfaccia di modifica è definito nel `ItemTemplate`.

Su quella successiva diversi passaggi si creerà un GridView completamente modificabile. Nel passaggio 1 si verrà inizia creando l'oggetto GridView con ObjectDataSource e convertire le BoundField e CheckBoxField TemplateFields. Nei passaggi 2 e 3 si sposterà le interfacce di modificare dal TemplateFields `EditItemTemplate` s per loro `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Passaggio 1: Visualizzazione di informazioni sul prodotto

Prima di preoccupazione la creazione di un controllo GridView in cui sono le righe è modificabili, consentire s avviare semplicemente visualizzando le informazioni sul prodotto. Aprire il `BatchUpdate.aspx` nella pagina di `BatchData` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare i dispositivi di GridView `ID` a `ProductsGrid` e dal suo smart tag scegliere da associare a un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per recuperare i dati di `ProductsBLL` classe s `GetProducts` metodo.


[![Configurare ObjectDataSource per utilizzare la classe ProductsBLL](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Figura 2**: configurare ObjectDataSource per utilizzare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine ingrandita](batch-updating-vb/_static/image4.png))


[![Recuperare i dati di prodotto utilizzando il metodo GetProducts](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Figura 3**: recuperare i dati di prodotto usando il `GetProducts` metodo ([fare clic per visualizzare l'immagine ingrandita](batch-updating-vb/_static/image6.png))


Come GridView, le funzioni di modifica ObjectDataSource s sono progettate per funzionare per ogni riga. Per aggiornare un set di record, è necessario scrivere codice per la classe code-behind pagine ASP.NET che invia i dati in batch e le passa al livello Business Logic. Pertanto, impostare gli elenchi a discesa in s ObjectDataSource schede UPDATE, INSERT e DELETE su (nessuno). Fare clic su Fine per completare la procedura guidata.


[![Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare le schede su (nessuno)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Figura 4**: impostare l'elenco a discesa sono elencati nelle schede DELETE, INSERT e UPDATE su (nessuno) ([fare clic per visualizzare l'immagine ingrandita](batch-updating-vb/_static/image8.png))


Dopo aver completato la configurazione guidata origine dati, il markup dichiarativo s ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

Completamento procedura guidata Configura origine dati viene generato anche Visual Studio per creare BoundField e un CheckBoxField per i campi di dati di prodotto in GridView. Per questa esercitazione, lasciare s solo consentire all'utente di visualizzare e modificare il nome del prodotto s, categoria, prezzo e stato non più supportato. Rimuovere tutto tranne il `ProductName`, `CategoryName`, `UnitPrice`, e `Discontinued` campi e rinominare il `HeaderText` le proprietà delle prime tre campi prodotto, categoria e prezzo, rispettivamente. Infine, selezionare le caselle di controllo Abilita Paging e Abilita ordinamento nello smart tag GridView s.

A questo punto GridView ha tre BoundField (`ProductName`, `CategoryName`, e `UnitPrice`) e un CheckBoxField (`Discontinued`). È necessario convertire questi quattro campi TemplateFields e quindi spostare l'interfaccia di modifica da s TemplateField `EditItemTemplate` al relativo `ItemTemplate`.

> [!NOTE]
> Abbiamo esaminato creazione e personalizzazione TemplateFields nel [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) esercitazione. Verranno esaminati i passaggi di conversione di BoundField e CheckBoxField in TemplateFields e definire la loro modifica interfacce loro `ItemTemplate` s, ma se improvvisi o necessario un aggiornamento, tenere sempre esitare per fare riferimento a questa esercitazione precedente.


Il GridView s smart tag, fare clic sul collegamento di modifica colonne per aprire la finestra di dialogo campi. Successivamente, selezionare ogni campo e fare clic su Converti il campo in un TemplateField di collegamento.


![Convertire il BoundField esistente e CheckBoxField TemplateFields](batch-updating-vb/_static/image5.gif)

**Figura 5**: convertire il BoundField esistente e CheckBoxField TemplateFields


Ora che ogni campo è un TemplateField, è nuovamente pronto per lo spostamento di modifica dell'interfaccia dal `EditItemTemplate` s per il `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Passaggio 2: Creazione di`ProductName`,`UnitPrice`, e`Discontinued`modifica interfacce

Creazione di `ProductName`, `UnitPrice`, e `Discontinued` modifica interfacce sono l'argomento di questo passaggio e piuttosto semplice, con ogni interfaccia è già definito in s TemplateField `EditItemTemplate`. Creazione di `CategoryName` modifica interfaccia è un'operazione più complessa poiché è necessario creare un controllo DropDownList le categorie applicabili. Questo `CategoryName` modifica interfaccia è esaminata nel passaggio 3.

Consentire s iniziano con la `ProductName` TemplateField. Fare clic sul collegamento Modifica modelli dal tag smart GridView s ed eseguire il drill-down di `ProductName` TemplateField s `EditItemTemplate`. Selezionare la casella di testo, copiarlo negli Appunti e quindi incollarlo il `ProductName` TemplateField s `ItemTemplate`. Modificare la casella di testo s `ID` proprietà `ProductName`.

Successivamente, aggiungere un controllo RequiredFieldValidator per il `ItemTemplate` per garantire che l'utente fornisce un valore per ogni nome di prodotto s. Impostare il `ControlToValidate` proprietà ProductName, il `ErrorMessage` proprietà per l'utente deve fornire il nome del prodotto. e `Text` proprietà \*. Dopo aver apportato queste aggiunte per la `ItemTemplate`, la schermata dovrebbe essere simile alla figura 6.


[![ProductName TemplateField ora include una casella di testo e un controllo RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Figura 6**: il `ProductName` TemplateField ora include una casella di testo e un RequiredFieldValidator ([fare clic per visualizzare l'immagine ingrandita](batch-updating-vb/_static/image10.png))


Per il `UnitPrice` interfaccia di modifica, iniziare la casella di testo da copiare il `EditItemTemplate` per il `ItemTemplate`. Successivamente, inserire un $ davanti la casella di testo e il set relativo `ID` proprietà UnitPrice e il relativo `Columns` proprietà 8.

Aggiungere anche un CompareValidator per il `UnitPrice` s `ItemTemplate` per garantire che il valore immesso dall'utente è un valore di valuta valido maggiore o uguale a $0,00. Impostare la convalida s `ControlToValidate` proprietà UnitPrice, relativo `ErrorMessage` proprietà per l'utente deve immettere un valore di valuta validi. . Omettere qualsiasi valuta i simboli., il relativo `Text` proprietà \*, le `Type` proprietà `Currency`, le `Operator` proprietà `GreaterThanEqual`e il relativo `ValueToCompare` proprietà su 0.


[![Aggiungere un controllo CompareValidator per garantire il prezzo immesso è un valore di valuta Non negativo](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Figura 7**: aggiungere un controllo CompareValidator per garantire il prezzo immesso è un valore di valuta Non negativo ([fare clic per visualizzare l'immagine ingrandita](batch-updating-vb/_static/image12.png))


Per il `Discontinued` TemplateField è possibile utilizzare la casella di controllo già definito nel `ItemTemplate`. È sufficiente impostare il relativo `ID` in sospeso e il relativo `Enabled` proprietà `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Passaggio 3: Creazione di`CategoryName`la modifica di interfaccia

L'interfaccia di modifica nel `CategoryName` TemplateField s `EditItemTemplate` contiene una casella di testo che visualizza il valore della `CategoryName` campo dati. È necessario sostituire con un controllo DropDownList che elenca le categorie possibili.

> [!NOTE]
> Il [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) esercitazione contiene una descrizione più completa e completa sulla personalizzazione di un modello per includere un DropDownList anziché una casella di testo. Durante questa procedura è completa, in cui vengono presentati fornisce risposte superficiali. Per una descrizione più approfondita di creazione e configurazione di categorie DropDownList, fare riferimento in futuro il [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) esercitazione.


Trascinare un controllo DropDownList dalla casella degli strumenti di `CategoryName` TemplateField s `ItemTemplate`, impostando il relativo `ID` per `Categories`. A questo punto è sarebbe definiscono in genere l'origine dati di controlli DropDownList s tramite il relativo smart tag, la creazione di un nuovo oggetto ObjectDataSource. Tuttavia, verrà aggiunta ObjectDataSource all'interno di `ItemTemplate`, sarà pertanto in un'istanza di ObjectDataSource creata per ogni riga GridView. Invece, consentire s creare ObjectDataSource di fuori di GridView s TemplateFields. Terminare la modifica dei modelli e trascinare un ObjectDataSource dalla casella degli strumenti nella finestra di progettazione sotto il `ProductsDataSource` ObjectDataSource. Nome nuovo ObjectDataSource `CategoriesDataSource` e configurarlo per utilizzare il `CategoriesBLL` classe s `GetCategories` metodo.


[![Configurare ObjectDataSource per utilizzare la classe CategoriesBLL](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Figura 8**: configurare ObjectDataSource per utilizzare il `CategoriesBLL` classe ([fare clic per visualizzare l'immagine ingrandita](batch-updating-vb/_static/image14.png))


[![Recuperare i dati della categoria utilizzando il metodo GetCategories](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Figura 9**: recuperare i dati di categoria utilizzando il `GetCategories` metodo ([fare clic per visualizzare l'immagine ingrandita](batch-updating-vb/_static/image16.png))


Poiché questo ObjectDataSource viene utilizzato solo per recuperare i dati, impostare gli elenchi a discesa nelle schede UPDATE e DELETE su (nessuno). Fare clic su Fine per completare la procedura guidata.


[![Set di elenchi a discesa in di aggiornamento e le schede di eliminazione su (nessuno)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Figura 10**: impostare l'elenco a discesa sono elencati nelle schede di eliminazione e aggiornamento (nessuno) ([fare clic per visualizzare l'immagine ingrandita](batch-updating-vb/_static/image18.png))


Dopo aver completato la procedura guidata, il `CategoriesDataSource` markup dichiarativo s dovrebbe essere simile al seguente:


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

Con il `CategoriesDataSource` creato e configurato, ripristinare il `CategoryName` TemplateField s `ItemTemplate` e, dallo DropDownList s smart tag, fare clic sul collegamento Scegli origine dati. La configurazione guidata origine dati, selezionare il `CategoriesDataSource` opzione dal primo elenco a discesa e scegliere di `CategoryName` utilizzato per la visualizzazione e `CategoryID` come valore.


[![Associare il CategoriesDataSource DropDownList](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Figura 11**: DropDownList per associare il `CategoriesDataSource` ([fare clic per visualizzare l'immagine ingrandita](batch-updating-vb/_static/image20.png))


A questo punto il `Categories` DropDownList Elenca tutte le categorie, ma non è ancora automaticamente selezionare la categoria appropriata per il prodotto associato alla riga GridView. A tale scopo è necessario impostare il `Categories` s DropDownList `SelectedValue` al prodotto s `CategoryID` valore. Fare clic sul collegamento Modifica DataBindings dallo smart tag s DropDownList e associare il `SelectedValue` proprietà con il `CategoryID` campo dati, come illustrato nella figura 12.


![Associare il valore di CategoryID prodotto s alla proprietà SelectedValue s DropDownList](batch-updating-vb/_static/image12.gif)

**Figura 12**: associare il prodotto s `CategoryID` valore al s DropDownList `SelectedValue` proprietà


Un ultimo resta di problema: se il prodotto dispone di un `CategoryID` valore specificato, l'istruzione di associazione dati in `SelectedValue` genererà un'eccezione. Infatti DropDownList contiene solo gli elementi per le categorie e non offre un'opzione per i prodotti con un `NULL` database valore per `CategoryID`. Per risolvere questo problema, impostare il s DropDownList `AppendDataBoundItems` proprietà `True` e aggiungere un nuovo elemento DropDownList, omettendo il `Value` proprietà con la sintassi dichiarativa. Vale a dire, assicurarsi che il `Categories` la sintassi dichiarativa s DropDownList simile al seguente:


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Nota come il `<asp:ListItem Value="">` - selezionare 1 - è relativa `Value` attributo impostato esplicitamente su una stringa vuota. Fare riferimento al [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) esercitazione per una descrizione più dettagliata sul motivo per cui questo elemento DropDownList aggiuntivo è necessaria per gestire il `NULL` case e perché l'assegnazione del `Value` proprietà da una stringa vuota è essenziale.

> [!NOTE]
> È un problema potenziale di prestazioni e scalabilità in questo caso è importante ricordare. Poiché ogni riga dispone di un controllo DropDownList che utilizza il `CategoriesDataSource` come origine dati, il `CategoriesBLL` classe s `GetCategories` metodo verrà chiamato  *n*  visita per pagina, in cui  *n*  è il numero di righe nel controllo GridView. Questi  *n*  le chiamate a `GetCategories` comportare  *n*  query per il database. L'impatto sul database potrebbe essere bilanciato dalla memorizzazione nella cache le categorie restituite in una cache per ogni richiesta o attraverso il livello di memorizzazione nella cache utilizzando una dipendenza o a una scadenza molto basato su breve periodo di tempo di memorizzazione nella cache di SQL. Per ulteriori informazioni sulla richiesta per la memorizzazione nella cache di opzione, vedere [ `HttpContext.Items` un archivio Cache richiesta Per](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Passaggio 4: Completare l'interfaccia di modifica

Abbiamo ve apportate alcune modifiche ai modelli s GridView senza sospensione per visualizzare l'avanzamento. Richiedere qualche istante per visualizzare l'avanzamento attraverso un browser. Come illustrato nella figura 13, ogni riga viene eseguito utilizzando il relativo `ItemTemplate`, che contiene la cella la modifica di interfaccia.


[![Ogni riga GridView è modificabile](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Figura 13**: ogni riga GridView è modificabile ([fare clic per visualizzare l'immagine ingrandita](batch-updating-vb/_static/image22.png))


Esistono alcuni problemi di formattazione secondari che è opportuno prestare attenzione a questo punto di. In primo luogo, si noti che il `UnitPrice` valore contiene quattro cifre decimali. Per risolvere questo problema, ripristinare il `UnitPrice` TemplateField s `ItemTemplate` e, dalla casella di testo s smart tag, fare clic sul collegamento Modifica DataBindings. Successivamente, specificare che il `Text` proprietà deve essere formattata come un numero.


![Formattare la proprietà Text sotto forma di numero](batch-updating-vb/_static/image14.gif)

**Nella figura 14**: formato di `Text` proprietà sotto forma di numero


In secondo luogo, s consentono di allineare al centro la casella di controllo di `Discontinued` colonna (anziché è allineato a sinistra). Fare clic su Modifica colonne dallo smart tag GridView s e selezionare il `Discontinued` TemplateField dall'elenco dei campi nell'angolo inferiore sinistro. Drill-down `ItemStyle` e impostare il `HorizontalAlign` proprietà al centro, come illustrato nella figura 15.


![Allineare al centro la casella di controllo non più supportata](batch-updating-vb/_static/image15.gif)

**Figura 15**: centro di `Discontinued` casella di controllo


Successivamente, aggiungere un controllo ValidationSummary alla pagina e impostare il relativo `ShowMessageBox` proprietà `True` e il relativo `ShowSummary` proprietà `False`. Aggiungere anche il pulsante di controlli Web, che, quando si fa clic, aggiorna le modifiche s utente. In particolare, aggiungere due controlli pulsante Web, uno sopra GridView e di sotto, l'impostazione di entrambi i controlli `Text` proprietà per i prodotti di aggiornamento.

Poiché i dispositivi di GridView modifica interfaccia definita nel relativo TemplateFields `ItemTemplate` s, il `EditItemTemplate` s sono superflue e può essere eliminato.

Dopo effettua quanto sopra indicato modifiche relative alla formattazione, i controlli pulsante di aggiunta e rimozione di inutili `EditItemTemplate` s, la sintassi dichiarativa pagina s dovrebbe essere simile al seguente:


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

Figura 16 Mostra questa pagina quando viene visualizzato tramite un browser dopo essere stati aggiunti i controlli pulsante Web e le modifiche apportate alla formattazione.


[![L'ora di pagina include due pulsanti di prodotti di aggiornamento](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Figura 16**: la pagina ora include due aggiornamenti prodotti pulsanti ([fare clic per visualizzare l'immagine ingrandita](batch-updating-vb/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Passaggio 5: Aggiornamento dei prodotti

Quando un utente visita questa pagina sono verrà apportare le modifiche e quindi fare clic su uno dei due pulsanti i prodotti di aggiornamento. A questo punto è necessario salvare in qualche modo i valori immessi dall'utente per ogni riga in un `ProductsDataTable` istanza, quindi passarlo a un metodo BLL che verrà poi passati che `ProductsDataTable` istanza per i dispositivi DAL `UpdateWithTransaction` metodo. Il `UpdateWithTransaction` metodo, che è stati creati nel [esercitazione precedente](wrapping-database-modifications-within-a-transaction-vb.md), garantisce che il batch di modifiche come operazione atomica.

Creare un metodo denominato `BatchUpdate` in `BatchUpdate.aspx.vb` e aggiungere il codice seguente:


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Questo metodo inizia recuperando tutti i prodotti in un `ProductsDataTable` tramite una chiamata al livello Business LOGIC s `GetProducts` metodo. Enumera quindi il `ProductGrid` GridView s [ `Rows` raccolta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). Il `Rows` raccolta contiene un [ `GridViewRow` istanza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) per ogni riga visualizzata in GridView. Poiché viene mostrato al massimo dieci righe per pagina, i dispositivi di GridView `Rows` raccolta avrà elementi non più di dieci.

Per ogni riga il `ProductID` è afferrato dal `DataKeys` appropriata e raccolta `ProductsRow` sia selezionato il `ProductsDataTable`. I controlli di input TemplateField quattro fanno riferimento a livello di codice e i valori assegnati per il `ProductsRow` proprietà dell'istanza. Dopo ogni GridView sono stati utilizzati i valori di riga o le righe per aggiornare il `ProductsDataTable`, si s passato a s BLL `UpdateWithTransaction` metodo che, come illustrato nell'esercitazione precedente, chiama semplicemente verso il basso in oggetti DAL `UpdateWithTransaction` metodo.

L'algoritmo di aggiornamento batch utilizzato per l'esercitazione Aggiorna ogni riga di `ProductsDataTable` che corrisponde a una riga in GridView, indipendentemente dal fatto che le informazioni sul prodotto s è stati modificati. Mentre tale blind aggiornamenti non in genere un problema di prestazioni, se si stanno controllo modifica alla tabella di database può causare superflue dei record. Nel [esecuzione di aggiornamenti Batch](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) esercitazione, è esplorare un batch di aggiornamento dell'interfaccia con DataList e aggiungere codice che aggiorna solo i record che sono stati effettivamente modificati dall'utente. È possibile utilizzare le tecniche da [esecuzione di aggiornamenti Batch](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) per aggiornare il codice in questa esercitazione, se lo si desidera.

> [!NOTE]
> Quando si associa l'origine dati a GridView mediante smart tag, Visual Studio assegna automaticamente i valori di chiave primaria di origine dati a GridView s `DataKeyNames` proprietà. Se non è stato associato ObjectDataSource a GridView tramite lo smart tag s di GridView come descritto nel passaggio 1, sarà necessario impostare manualmente i dispositivi di GridView `DataKeyNames` ProductID per accedere alle proprietà di `ProductID` valore per ogni riga tramite il `DataKeys` insieme.


Il codice usato nel `BatchUpdate` è simile a quella usata in s BLL `UpdateProduct` metodi, la differenza principale è che nel `UpdateProduct` metodi solo una singola `ProductRow` istanza viene recuperata dall'architettura. Il codice che assegna le proprietà del `ProductRow` quella tra il `UpdateProducts` metodi e il codice all'interno di `For Each` ciclo nella `BatchUpdate`, come è illustrato il modello generale.

Per completare questa esercitazione, è necessario che il `BatchUpdate` metodo richiamato quando si fa clic su uno dei pulsanti i prodotti di aggiornamento. Creare gestori eventi per il `Click` gli eventi di questi due controlli pulsante e aggiungere il codice seguente nei gestori di eventi:


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

Innanzitutto viene eseguita una chiamata a `BatchUpdate`. Successivamente, il [ `ClientScript` proprietà](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) viene utilizzato per inserire JavaScript che consente di visualizzare una finestra di messaggio che legge i prodotti sono stati aggiornati.

Richiedere un minuto, per testare questo codice. Visitare `BatchUpdate.aspx` tramite un browser, modificare un numero di righe e fare clic su uno dei pulsanti i prodotti di aggiornamento. Supponendo che non siano presenti errori di convalida dell'input, verrà visualizzata una finestra di messaggio che legge che i prodotti sono stati aggiornati. Per verificare l'atomicità dell'aggiornamento, è possibile aggiungere un casuale `CHECK` vincolo, ad esempio uno che non consente `UnitPrice` 1234,56 ai valori. Quindi dal `BatchUpdate.aspx`, modificare un numero di record, assicurandosi di impostare uno dei prodotti s `UnitPrice` valore con il valore non consentito (1234,56). Verrà restituito un errore quando scegliendo i prodotti di aggiornamento con le altre modifiche durante tale operazione batch eseguito il rollback in base ai valori originali.

## <a name="an-alternativebatchupdatemethod"></a>In alternativa`BatchUpdate`(metodo)

Il `BatchUpdate` metodo abbiamo esaminato recupera *tutti* dei prodotti da s BLL `GetProducts` (metodo) e quindi aggiorna solo i record che vengono visualizzati in GridView. Questo approccio è ideale se GridView non usa il paging, ma in tal caso, potrebbero esserci centinaia, migliaia o decine di migliaia di prodotti, ma solo dieci righe in GridView. In tal caso, il recupero di tutti i prodotti dal database solo per modificare 10 dei quali è minore di ideale.

Per i tipi di situazioni, è consigliabile utilizzare le seguenti `BatchUpdateAlternate` metodo invece:


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate`inizia creando un nuovo vuoto `ProductsDataTable` denominato `products`. Viene quindi i passaggi da a s GridView `Rows` insieme e per ogni riga Ottiene le informazioni di prodotto specifico con s BLL `GetProductByProductID(productID)` metodo. Recuperato `ProductsRow` istanza ha le proprietà aggiornate in modo identico a `BatchUpdate`, ma dopo aver aggiornato la riga viene importato nel `products` `ProductsDataTable` tramite gli oggetti DataTable [ `ImportRow(DataRow)` metodo](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Dopo il `For Each` ciclo viene completato, `products` contiene uno `ProductsRow` istanza per ogni riga in GridView. Poiché ogni del `ProductsRow` istanze sono state aggiunte per la `products` (anziché aggiornato), se si passa a ciecamente il `UpdateWithTransaction` (metodo) il `ProductsTableAdatper` tenterà di inserire ogni record nel database. In alternativa, è necessario specificare che ognuna di queste righe è stata modificata (non aggiunto).

Questa operazione può essere eseguita aggiungendo un nuovo metodo al livello Business Logic denominato `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, set indicato di seguito, il `RowState` di ogni il `ProductsRow` istanze il `ProductsDataTable` per `Modified` e quindi passa il `ProductsDataTable` per i dispositivi DAL `UpdateWithTransaction` (metodo).


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Riepilogo

GridView fornisce funzionalità di modifica di riga predefinito, ma non offre il supporto per la creazione di interfacce completamente modificabile. Come abbiamo visto in questa esercitazione, queste interfacce sono possibili, ma richiedono un po' di lavoro. Per creare un controllo GridView in cui ogni riga è modificabile, è necessario convertire i campi s GridView in TemplateFields e definire l'interfaccia di modifica all'interno di `ItemTemplate` s. Inoltre, per aggiornare tutti - tipo di controlli Web pulsante deve essere aggiunto alla pagina, separata dal controllo GridView. Questi pulsanti `Click` gestori eventi è necessario enumerare i dispositivi di GridView `Rows` insieme, archiviare le modifiche in un `ProductsDataTable`e passare le informazioni aggiornate al metodo BLL appropriato.

Nella prossima esercitazione vedremo come creare un'interfaccia per l'eliminazione di batch. In particolare, ogni riga GridView verrà include una casella di controllo e invece di aggiornare tutti-tipo di pulsanti, sono i pulsanti Elimina righe selezionate.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Teresa Murphy e David Suru. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](wrapping-database-modifications-within-a-transaction-vb.md)
[Successivo](batch-deleting-vb.md)
