---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
title: Batch di aggiornamento (c#) | Microsoft Docs
author: rick-anderson
description: Informazioni su come aggiornare più record di database in un'unica operazione. Nel Layer dell'interfaccia utente si compila un controllo GridView in cui ogni riga è modificabile. Nei dati...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 4e849bcc-c557-4bc3-937e-f7453ee87265
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
msc.type: authoredcontent
ms.openlocfilehash: c878056273ea821e4dd4481fa1b6f7690f22b285
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830247"
---
<a name="batch-updating-c"></a>Batch di aggiornamento (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare il codice](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_CS.zip) o [Scarica il PDF](batch-updating-cs/_static/datatutorial64cs1.pdf)

> Informazioni su come aggiornare più record di database in un'unica operazione. Nel Layer dell'interfaccia utente si compila un controllo GridView in cui ogni riga è modificabile. Nel livello di accesso ai dati è eseguire il wrapping di più operazioni di aggiornamento all'interno di una transazione per garantire che tutti gli aggiornamenti abbia esito positivo o vengano eseguito il rollback di tutti gli aggiornamenti.


## <a name="introduction"></a>Introduzione

Nel [esercitazione precedente](wrapping-database-modifications-within-a-transaction-cs.md) è stato illustrato come estendere il livello di accesso ai dati per aggiungere il supporto per le transazioni del database. Le transazioni del database garantiscono che una serie di istruzioni di modifica dei dati viene considerata come una singola operazione atomica, che assicura che tutte le modifiche avranno esito negativo o tutti avrà esito positivo. Con questo livello basso funzionalità dall'area di lavoro, è nuovamente pronto per concentrare l'attenzione alla creazione di interfacce di modifica dei dati batch.

In questa esercitazione creeremo un controllo GridView in cui ogni riga è modificabile (vedere la figura 1). Poiché ogni riga non viene eseguito il rendering dell'interfaccia di modifica, qui s è necessario per una colonna di modifica, aggiornare e annullare i pulsanti. Al contrario, sono presenti due pulsanti di aggiornare i prodotti nella pagina che, quando si fa clic, enumerare le righe di GridView e aggiornare il database.


[![Ogni riga nel controllo GridView è modificabile](batch-updating-cs/_static/image1.gif)](batch-updating-cs/_static/image1.png)

**Figura 1**: ogni riga nel controllo GridView è modificabile ([fare clic per visualizzare l'immagine con dimensioni normali](batch-updating-cs/_static/image2.png))


Introduzione a ti permettono di s.

> [!NOTE]
> Nel [esecuzione di aggiornamenti Batch](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) esercitazione è stato creato un batch di modifica dell'interfaccia usando il controllo DataList. Questa esercitazione è diverso dal precedente uno in cui viene utilizzata una GridView e l'aggiornamento batch viene eseguito all'interno dell'ambito di una transazione. Dopo aver completato questa esercitazione consiglia di tornare all'esercitazione precedente e aggiornarla per usare la funzionalità di correlate alle transazioni di database aggiunta nell'esercitazione precedente.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Esaminare i passaggi per rendere modificabile tutte le righe di GridView

Come descritto nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) esercitazione, il controllo GridView offre supporto predefinito per la modifica i dati sottostanti per ogni riga. Internamente, il controllo GridView note quali riga può essere modificata tramite il [ `EditIndex` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Controlla come viene associato il controllo GridView all'origine dati, ogni riga per verificare se l'indice della riga è uguale al valore di `EditIndex`. In questo caso, le interfacce che s i campi vengono visualizzati utilizzando la loro modifica la riga. Per i BoundField, l'interfaccia di modifica è una casella di testo il cui `Text` proprietà viene assegnato il valore del campo dati specificato dalla s BoundField `DataField` proprietà. Per TemplateFields, il `EditItemTemplate` viene usato al posto di `ItemTemplate`.

È importante ricordare che il flusso di lavoro modifica inizia quando un utente fa clic sul pulsante Modifica una riga o le righe. Ciò determina un postback, imposta la s GridView `EditIndex` proprietà per l'indice di riga selezionata s e riassociazioni i dati nella griglia. Quando viene selezionato un pulsante Annulla riga s, durante il postback il `EditIndex` è impostata su un valore di `-1` prima i dati nella griglia la riassociazione. Poiché le righe s GridView avvia l'indicizzazione nella posizione zero, l'impostazione `EditIndex` a `-1` ha l'effetto di visualizzare il controllo GridView in modalità di sola lettura.

Il `EditIndex` proprietà funziona bene per la modifica per riga, ma non è progettato per la modifica in blocco. Per rendere modificabili i GridView intero, è necessario che ogni riga di eseguire il rendering usando l'interfaccia di modifica. Il modo più semplice per eseguire questa operazione consiste nel creare in cui ogni campo modificabile viene implementato come un TemplateField con la relativa interfaccia modifica definito nel `ItemTemplate`.

Prossimi diversi passaggi si creerà un GridView completamente modificabile. Nel passaggio 1 verranno inizia creando l'oggetto GridView con ObjectDataSource e convertire i BoundField e CampoCasellaDiControllo in TemplateFields. Nei passaggi 2 e 3 si passerà le interfacce di modifica da di TemplateFields `EditItemTemplate` s al loro `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Passaggio 1: Visualizzare le informazioni sul prodotto

Prima ci preoccupiamo la creazione di un controllo GridView in cui vengono le righe è modificabili, consentire s per iniziare, è sufficiente visualizzare le informazioni sul prodotto. Aprire il `BatchUpdate.aspx` nella pagina di `BatchData` cartelle e trascinare un controllo GridView dalla casella degli strumenti nella finestra di progettazione. Impostare la s GridView `ID` al `ProductsGrid` e scegliere dal suo smart tag da associare a un nuovo oggetto ObjectDataSource denominato `ProductsDataSource`. Configurare ObjectDataSource per recuperare i dati dal `ProductsBLL` classe s `GetProducts` (metodo).


[![Configurare ObjectDataSource per usare la classe ProductsBLL](batch-updating-cs/_static/image2.gif)](batch-updating-cs/_static/image3.png)

**Figura 2**: configurare ObjectDataSource per usare il `ProductsBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](batch-updating-cs/_static/image4.png))


[![Recuperare i dati del prodotto utilizzando il metodo GetProducts](batch-updating-cs/_static/image3.gif)](batch-updating-cs/_static/image5.png)

**Figura 3**: recuperare i dati di prodotto usando il `GetProducts` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](batch-updating-cs/_static/image6.png))


Come per GridView, le funzionalità di modifica s ObjectDataSource sono progettate per funzionare su una base di ogni riga. Per aggiornare un set di record, è necessario scrivere un po' di codice nella classe code-behind ASP.NET pagina s che invia in batch i dati e lo passa per il livello BLL. Pertanto, impostare gli elenchi a discesa in s ObjectDataSource schede UPDATE, INSERT e DELETE su (nessuno). Fare clic su Fine per completare la procedura guidata.


[![Impostare gli elenchi a discesa nell'aggiornamento, inserimento ed eliminare schede su (nessuno)](batch-updating-cs/_static/image4.gif)](batch-updating-cs/_static/image7.png)

**Figura 4**: impostare l'elenco a discesa sono elencati nell'aggiornamento, inserimento ed eliminare schede su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](batch-updating-cs/_static/image8.png))


Dopo aver completato la procedura guidata Configura origine dati, il markup dichiarativo di ObjectDataSource s dovrebbe essere simile al seguente:


[!code-aspx[Main](batch-updating-cs/samples/sample1.aspx)]

Completamento procedura guidata Configura origine dati provoca anche Visual Studio per creare un CampoCasellaDiControllo per i campi di dati del prodotto e BoundField in GridView. Per questa esercitazione, lasciare s solo consentire all'utente di visualizzare e modificare il s nome prodotto, categoria, prezzo e lo stato non più utilizzato. Rimuovi tutto tranne le `ProductName`, `CategoryName`, `UnitPrice`, e `Discontinued` campi e rinominare il `HeaderText` le proprietà delle prime tre campi per prodotto, categoria e prezzo, rispettivamente. Infine, selezionare le caselle di controllo Attiva Paging e abilitare l'ordinamento nello smart tag s GridView.

A questo punto include anche tre BoundField (`ProductName`, `CategoryName`, e `UnitPrice`) e un CampoCasellaDiControllo (`Discontinued`). È necessario convertire questi quattro campi in TemplateFields e quindi spostare l'interfaccia di modifica da s TemplateField `EditItemTemplate` al relativo `ItemTemplate`.

> [!NOTE]
> È stata esaminata la creazione e personalizzazione di TemplateFields nel [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) esercitazione. Verranno esaminati i passaggi necessari per convertire i BoundField e CampoCasellaDiControllo in TemplateFields e definire la loro modifica interfacce nella loro `ItemTemplate` s, ma se bloccarsi o necessario un ripasso, don t sono riluttanti a fare riferimento a questa esercitazione precedente.


GridView s smart tag, fare clic sul collegamento Modifica colonne per aprire la finestra di dialogo campi. Successivamente, selezionare ogni campo e fare clic su Converti il campo in un collegamento TemplateField.


![Convertire i BoundField esistente e CampoCasellaDiControllo in TemplateField](batch-updating-cs/_static/image5.gif)

**Figura 5**: convertire i BoundField esistente e CampoCasellaDiControllo in TemplateField


Ora che ogni campo è un TemplateField, sono pronti a spostare la modifica dell'interfaccia dal `EditItemTemplate` s per il `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Passaggio 2: Creazione di`ProductName`,`UnitPrice`, e`Discontinued`interfacce di modifica

Creazione di `ProductName`, `UnitPrice`, e `Discontinued` modifica le interfacce sono l'argomento di questo passaggio e sono piuttosto semplici, con ogni interfaccia è già definito in s TemplateField `EditItemTemplate`. Creazione di `CategoryName` modifica interfaccia è un po' più complessa perché è necessario creare un controllo DropDownList le categorie applicabili. Ciò `CategoryName` modifica interfaccia viene affrontata nel passaggio 3.

Let s iniziano con la `ProductName` TemplateField. Fare clic sul collegamento di modifica modelli dello smart tag s GridView e il drill-down per il `ProductName` TemplateField s `EditItemTemplate`. Selezionare la casella di testo, copiarlo negli Appunti e incollarlo il `ProductName` TemplateField s `ItemTemplate`. Modificare gli oggetti nella casella di testo `ID` proprietà `ProductName`.

Successivamente, aggiungere un RequiredFieldValidator al `ItemTemplate` per assicurarsi che l'utente fornisce un valore per ogni nome di prodotto s. Impostare il `ControlToValidate` proprietà ProductName, il `ErrorMessage` proprietà per l'utente deve fornire il nome del prodotto. e il `Text` proprietà \*. Dopo aver apportato queste aggiunte per il `ItemTemplate`, la schermata dovrebbe essere simile alla figura 6.


[![ProductName TemplateField ora include una casella di testo e un RequiredFieldValidator](batch-updating-cs/_static/image6.gif)](batch-updating-cs/_static/image9.png)

**Figura 6**: il `ProductName` TemplateField ora include una casella di testo e un RequiredFieldValidator ([fare clic per visualizzare l'immagine con dimensioni normali](batch-updating-cs/_static/image10.png))


Per il `UnitPrice` interfaccia di modifica, iniziare copiando la casella di testo dal `EditItemTemplate` per il `ItemTemplate`. Successivamente, inserire un simbolo $ davanti la casella di testo e il set relativo `ID` proprietà UnitPrice e la relativa `Columns` proprietà a 8.

Aggiungere anche un controllo CompareValidator per i `UnitPrice` s `ItemTemplate` per garantire che il valore immesso dall'utente è un valore di valuta validi maggiore o uguale a $0,00. Impostare s validator `ControlToValidate` UnitPrice, proprietà relativa `ErrorMessage` proprietà per l'utente deve immettere un valore di valuta validi. . Omettere qualsiasi valuta i simboli., relativo `Text` proprietà \*, la relativa `Type` proprietà `Currency`, la relativa `Operator` proprietà `GreaterThanEqual`e il relativo `ValueToCompare` proprietà su 0.


[![Aggiungere un controllo CompareValidator per assicurarsi di immettere il prezzo è un valore di valuta Non negativo](batch-updating-cs/_static/image7.gif)](batch-updating-cs/_static/image11.png)

**Figura 7**: aggiungere un controllo CompareValidator per assicurarsi di immettere il prezzo è un valore di valuta Non negativo ([fare clic per visualizzare l'immagine con dimensioni normali](batch-updating-cs/_static/image12.png))


Per il `Discontinued` TemplateField è possibile usare la casella di controllo già definito nel `ItemTemplate`. È sufficiente impostare relativi `ID` a Discontinued e la relativa `Enabled` proprietà `true`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Passaggio 3: Creazione di`CategoryName`modifica interfaccia

L'interfaccia di modifica nel `CategoryName` s TemplateField `EditItemTemplate` contiene una casella di testo che visualizza il valore della `CategoryName` campo dati. È necessario sostituire con un controllo DropDownList che elenca le categorie possibili.

> [!NOTE]
> Il [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) esercitazione contiene una discussione più approfondita e completa sulla personalizzazione di un modello per includere un controllo DropDownList invece di una casella di testo. Anche se questi passaggi sono completi, vengono presentati fornisce risposte superficiali. Per un'analisi più approfondita a creare e configurare le categorie DropDownList, vedere la [personalizzazione dell'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) esercitazione.


Trascinare un controllo DropDownList dalla casella degli strumenti di `CategoryName` s TemplateField `ItemTemplate`, l'impostazione relativa `ID` a `Categories`. A questo punto sarebbe in genere definiamo l'origine dati di s controlli DropDownList tramite suo smart tag, la creazione di un nuovo oggetto ObjectDataSource. Tuttavia, verrà aggiunto ObjectDataSource all'interno di `ItemTemplate`, verrà visualizzato un risultato in un'istanza di ObjectDataSource creata per ogni riga GridView. Lasciare invece che s creare ObjectDataSource di fuori di GridView s TemplateFields. Terminare la modifica dei modelli e trascinare un ObjectDataSource dalla casella degli strumenti nella finestra di progettazione sotto le `ProductsDataSource` ObjectDataSource. Denominare il nuovo oggetto ObjectDataSource `CategoriesDataSource` e configurarlo per usare il `CategoriesBLL` classe s `GetCategories` (metodo).


[![Configurare ObjectDataSource per usare la classe CategoriesBLL](batch-updating-cs/_static/image8.gif)](batch-updating-cs/_static/image13.png)

**Figura 8**: configurare ObjectDataSource per usare il `CategoriesBLL` classe ([fare clic per visualizzare l'immagine con dimensioni normali](batch-updating-cs/_static/image14.png))


[![Recuperare i dati della categoria utilizzando il metodo GetCategories](batch-updating-cs/_static/image9.gif)](batch-updating-cs/_static/image15.png)

**Figura 9**: recuperare i dati di categoria usando il `GetCategories` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](batch-updating-cs/_static/image16.png))


Poiché questo ObjectDataSource viene usato semplicemente per recuperare i dati, impostare gli elenchi a discesa nelle schede UPDATE e DELETE su (nessuno). Fare clic su Fine per completare la procedura guidata.


[![Set di elenchi a discesa in di aggiornamento e le schede di eliminazione per (nessuno)](batch-updating-cs/_static/image10.gif)](batch-updating-cs/_static/image17.png)

**Figura 10**: impostare l'elenco a discesa sono elencati nelle schede di eliminazione e aggiornamento su (nessuno) ([fare clic per visualizzare l'immagine con dimensioni normali](batch-updating-cs/_static/image18.png))


Dopo aver completato la procedura guidata, il `CategoriesDataSource` markup dichiarativo s dovrebbe essere simile al seguente:


[!code-aspx[Main](batch-updating-cs/samples/sample2.aspx)]

Con il `CategoriesDataSource` creato e configurato, tornare al `CategoryName` TemplateField s `ItemTemplate` e, DropDownList s nello smart tag, fare clic sul collegamento Scegli origine dati. Nella procedura guidata configurazione dell'origine dati, selezionare la `CategoriesDataSource` l'opzione dal primo elenco a discesa e scegliere di avere `CategoryName` utilizzato per la visualizzazione e `CategoryID` come valore.


[![Associare il CategoriesDataSource DropDownList](batch-updating-cs/_static/image11.gif)](batch-updating-cs/_static/image19.png)

**Figura 11**: associa il controllo DropDownList per il `CategoriesDataSource` ([fare clic per visualizzare l'immagine con dimensioni normali](batch-updating-cs/_static/image20.png))


A questo punto il `Categories` DropDownList Elenca tutte le categorie, ma non verrà ancora automaticamente selezionata la categoria appropriata per il prodotto associato alla riga GridView. A tale scopo è necessario impostare il `Categories` s DropDownList `SelectedValue` al prodotto s `CategoryID` valore. Fare clic sul collegamento Modifica DataBindings dallo smart tag DropDownList s e associare le `SelectedValue` proprietà con il `CategoryID` datové pole come illustrato nella figura 12.


![Associare il valore di CategoryID prodotto s per la proprietà SelectedValue s DropDownList](batch-updating-cs/_static/image12.gif)

**Figura 12**: eseguire l'associazione del prodotto s `CategoryID` valore s DropDownList `SelectedValue` proprietà


Un ultimo rimane di problema: se il prodotto ha un `CategoryID` specificato valore quindi l'istruzione di Data Binding in `SelectedValue` genererà un'eccezione. Ciò avviene perché il controllo DropDownList contiene solo gli elementi per le categorie e non offre un'opzione per i prodotti con un `NULL` database valore `CategoryID`. Per risolvere questo problema, impostare la s DropDownList `AppendDataBoundItems` proprietà `true` e aggiungere un nuovo elemento a DropDownList, omettendo il `Value` proprietà dalla sintassi dichiarativa per il. Vale a dire, assicurarsi che il `Categories` sintassi dichiarativa per il controllo DropDownList s simile al seguente:


[!code-aspx[Main](batch-updating-cs/samples/sample3.aspx)]

Si noti come il `<asp:ListItem Value="">` - selezionare uno, è relativo `Value` attributo impostato in modo esplicito in una stringa vuota. Fare riferimento al [personalizzazione dell'interfaccia di modifica dei dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) esercitazione per una discussione più approfondita sui motivi per cui questo elemento DropDownList aggiuntivo necessari per gestire le `NULL` case e il motivo per cui assegnazione del `Value` proprietà da una stringa vuota è essenziale.

> [!NOTE]
> C'è una potenziale scalabilità e prestazioni problema qui che vale la pena citare. Poiché ogni riga dispone di un controllo DropDownList che usa il `CategoriesDataSource` come origine dati, il `CategoriesBLL` classe s `GetCategories` metodo verrà chiamato *n* volte per ogni pagina visitano, dove *n* è il numero di righe in GridView. Questi *n* le chiamate a `GetCategories` comportare *n* query al database. Questo impatto sul database potrebbe essere diminuito memorizzando nella cache le categorie restituite in una cache per ogni richiesta o tramite il livello di memorizzazione nella cache usando un linguaggio SQL dipendenza o a una scadenza basata molto sul breve periodo di tempo di memorizzazione nella cache. Per altre informazioni sulla richiesta per la memorizzazione nella cache di opzione, vedere [ `HttpContext.Items` Store una Cache Per richiesta](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Passaggio 4: Completare l'interfaccia di modifica

Abbiamo ve apportate alcune modifiche ai modelli s GridView senza la sospensione per visualizzare lo stato di avanzamento. Si consiglia di visualizzare lo stato di avanzamento tramite un browser. Come illustrato nella figura 13, ogni riga viene generato usando relativo `ItemTemplate`, che contiene la s cella interfaccia di modifica.


[![Ogni riga GridView è modificabile](batch-updating-cs/_static/image13.gif)](batch-updating-cs/_static/image21.png)

**Figura 13**: ogni riga GridView è modificabile ([fare clic per visualizzare l'immagine con dimensioni normali](batch-updating-cs/_static/image22.png))


Esistono alcuni problemi di formattazione secondari che dovremmo occupandoci dell'ora. In primo luogo, si noti che il `UnitPrice` valore contiene quattro cifre decimali. Per risolvere questo problema, ripristinare il `UnitPrice` TemplateField s `ItemTemplate` e, nella casella di testo s nello smart tag, fare clic sul collegamento Modifica DataBindings. Successivamente, specificare che il `Text` proprietà deve essere formattata come un numero.


![Formattare la proprietà Text sotto forma di numero](batch-updating-cs/_static/image14.gif)

**Figura 14**: formato di `Text` proprietà sotto forma di numero


In secondo luogo, s consentono di allineare al centro la casella di controllo di `Discontinued` colonna (anziché viene allineato a sinistra). Fare clic su Modifica colonne dello smart tag s GridView e selezionare il `Discontinued` TemplateField dall'elenco dei campi nell'angolo inferiore sinistro. Il drill down `ItemStyle` e impostare il `HorizontalAlign` proprietà Center, come illustrato nella figura 15.


![Allineare al centro la casella di controllo non più supportata](batch-updating-cs/_static/image15.gif)

**Figura 15**: centro di `Discontinued` casella di controllo


Successivamente, aggiungere un controllo di controllo ValidationSummary alla pagina e impostare relativi `ShowMessageBox` proprietà `true` e il relativo `ShowSummary` proprietà `false`. Aggiungere anche il pulsante di controlli Web, che, quando si fa clic, aggiornerà le modifiche s utente. In particolare, aggiungere due controlli pulsante Web, uno sopra il controllo GridView e uno di sotto di esso, l'impostazione entrambi i controlli `Text` proprietà da aggiornamenti dei prodotti.

Poiché la s GridView modifica interfaccia è definita nel relativo TemplateFields `ItemTemplate` s, la `EditItemTemplate` s sono superflue e possono essere eliminati.

Dopo che effettua quanto sopra accennato modifiche relative alla formattazione, i controlli pulsante di aggiunta e rimozione di inutili `EditItemTemplate` s, la sintassi dichiarativa per pagina s dovrebbe essere simile al seguente:


[!code-aspx[Main](batch-updating-cs/samples/sample4.aspx)]

Figura 16 Mostra questa pagina quando viene visualizzato tramite un browser dopo che sono stati aggiunti i controlli pulsante Web e le modifiche apportate alla formattazione.


[![Adesso la pagina include due pulsanti di prodotti di aggiornamento](batch-updating-cs/_static/image16.gif)](batch-updating-cs/_static/image23.png)

**Figura 16**: la pagina ora include due aggiornamenti prodotti pulsanti ([fare clic per visualizzare l'immagine con dimensioni normali](batch-updating-cs/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Passaggio 5: Aggiornare i prodotti

Quando un utente visita questa pagina verranno apportano le modifiche e quindi fare clic su uno dei due pulsanti aggiornamenti dei prodotti. A questo punto è necessario in qualche modo salvare i valori immessi dall'utente per ogni riga in una `ProductsDataTable` dell'istanza e si passa quindi un metodo BLL che verrà quindi passarli `ProductsDataTable` istanza per i dispositivi DAL `UpdateWithTransaction` (metodo). Il `UpdateWithTransaction` metodo, che è stato creato nel [esercitazione precedente](wrapping-database-modifications-within-a-transaction-cs.md), assicura che il batch delle modifiche verrà aggiornato come operazione atomica.

Creare un metodo denominato `BatchUpdate` in `BatchUpdate.aspx.cs` e aggiungere il codice seguente:


[!code-csharp[Main](batch-updating-cs/samples/sample5.cs)]

Questo metodo inizia recuperando tutti i prodotti in una `ProductsDataTable` tramite una chiamata a s BLL `GetProducts` (metodo). Vengono quindi enumerati i `ProductGrid` s GridView [ `Rows` raccolta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). Il `Rows` raccolta contiene un [ `GridViewRow` istanza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) per ogni riga visualizzata nel controllo GridView. Poiché viene mostrata al massimo dieci righe per pagina, la s GridView `Rows` raccolta avrà gli elementi non più di dieci.

Per ogni riga il `ProductID` acquisizione dal `DataKeys` raccolta e appropriato `ProductsRow` viene selezionato dal `ProductsDataTable`. I quattro controlli di input TemplateField fanno riferimento a livello di codice e i relativi valori assegnati al `ProductsRow` s delle proprietà dell'istanza. Dopo ogni GridView sono stati utilizzati i valori di riga o le righe per aggiornare il `ProductsDataTable`, è s passato a s BLL `UpdateWithTransaction` metodo che, come illustrato nell'esercitazione precedente, chiama semplicemente verso il basso in oggetti DAL `UpdateWithTransaction` (metodo).

L'algoritmo di aggiornamento batch utilizzato per questa esercitazione Aggiorna ogni riga nel `ProductsDataTable` che corrisponde a una riga in GridView, indipendentemente dal fatto che le informazioni sul prodotto s è stati modificati. Mentre tale blind Aggiorna t sono in genere un problema di prestazioni, se si ri il controllo viene modificata alla tabella di database può causare ai record superfluo. Nel [esecuzione di aggiornamenti Batch](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) esercitazione abbiamo esplorato un batch di aggiornamento dell'interfaccia con DataList e avere aggiunto il codice che aggiorna solo i record che sono stati effettivamente modificati dall'utente. È possibile usare le tecniche da [esecuzione di aggiornamenti Batch](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) per aggiornare il codice in questa esercitazione, se lo si desidera.

> [!NOTE]
> Quando si associa l'origine dati a GridView mediante relativo smart tag, Visual Studio assegna automaticamente i valori di chiave primaria di origine s dei dati a GridView s `DataKeyNames` proprietà. Se non è stato associato ObjectDataSource di GridView tramite lo smart tag s di GridView come descritto nel passaggio 1, quindi è necessario impostare manualmente la s GridView `DataKeyNames` proprietà ProductID per poter accedere il `ProductID` valore per ogni riga tramite il `DataKeys` raccolta.


Il codice usato nel `BatchUpdate` è simile a quella usata in s BLL `UpdateProduct` metodi, la differenza principale è che nel `UpdateProduct` metodi solo una singola `ProductRow` istanza viene recuperata dall'architettura. Il codice che assegna la proprietà del `ProductRow` è lo stesso tra il `UpdateProducts` metodi e il codice all'interno di `foreach` ciclo nella `BatchUpdate`, poiché è il modello nel suo complesso.

Per completare questa esercitazione, è necessario disporre di `BatchUpdate` metodo richiamato quando si fa clic su uno dei pulsanti di aggiornamenti dei prodotti. Creare gestori eventi per il `Click` gli eventi di questi due controlli pulsante e aggiungere il codice seguente nei gestori eventi:


[!code-csharp[Main](batch-updating-cs/samples/sample6.cs)]

Prima di tutto viene effettuata una chiamata a `BatchUpdate`. Successivamente, il `ClientScript property` consente di inserire codice JavaScript che verrà visualizzata una finestra di messaggio che legge i prodotti sono stati aggiornati.

Richiedere un minuto per testare questo codice. Visitare `BatchUpdate.aspx` tramite un browser, modificare un numero di righe e fare clic su uno dei pulsanti di aggiornamenti dei prodotti. Supponendo che non siano presenti errori di convalida dell'input, verrà visualizzata una finestra di messaggio che legge che i prodotti sono stati aggiornati. Per verificare l'atomicità dell'aggiornamento, è consigliabile aggiungere uno casuale `CHECK` vincolo, ad esempio una che non consente `UnitPrice` 1234,56 ai valori. Quindi dal `BatchUpdate.aspx`, modificare un numero di record, assicurandosi di impostare uno del prodotto s `UnitPrice` al valore non consentito (1234,56). Questo dovrebbe restituire un errore quando facendo clic su aggiornamenti dei prodotti con le altre modifiche durante l'operazione batch eseguito il rollback sui valori originali.

## <a name="an-alternativebatchupdatemethod"></a>Un'alternativa`BatchUpdate`(metodo)

Il `BatchUpdate` metodo viene semplicemente esaminata recupera *tutte* dei prodotti da s BLL `GetProducts` (metodo) e quindi aggiorna solo i record che vengono visualizzati in GridView. Questo approccio è ideale se il controllo GridView non usa il paging, ma in tal caso, potrebbero esserci centinaia, migliaia o decine di migliaia di prodotti, ma solo dieci righe in GridView. In tal caso, ottenere tutti i prodotti dal database solo per modificare 10 dei quali è inferiore a quello ideale.

Per i tipi di situazioni, è consigliabile usare quanto segue `BatchUpdateAlternate` metodo invece:


[!code-csharp[Main](batch-updating-cs/samples/sample7.cs)]

`BatchMethodAlternate` inizia creando una nuova classe vuota `ProductsDataTable` denominato `products`. Viene quindi i passaggi a s GridView `Rows` raccolta e per ogni riga Ottiene le informazioni di prodotto specifico usando il livello BLL s `GetProductByProductID(productID)` (metodo). Recuperato `ProductsRow` istanza ha le relative proprietà aggiornate nello stesso modo come `BatchUpdate`, ma dopo aver aggiornato la riga viene importato nelle `products``ProductsDataTable` tramite gli oggetti DataTable [ `ImportRow(DataRow)` metodo](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Dopo il `foreach` ciclo viene completato, `products` contiene uno `ProductsRow` istanza per ogni riga in GridView. Poiché ogni del `ProductsRow` istanze sono state aggiunte ad il `products` (invece di aggiornato), se si passa alla cieca per il `UpdateWithTransaction` (metodo) il `ProductsTableAdatper` tenterà di inserire ogni record nel database. In alternativa, è necessario specificare che ognuna di queste righe è stata modificata (non aggiunto).

Ciò può essere eseguita aggiungendo un nuovo metodo per il livello BLL denominato `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, set mostrato di seguito, il `RowState` della ognuno del `ProductsRow` istanze nel `ProductsDataTable` a `Modified` e quindi passa il `ProductsDataTable` ai dispositivi DAL `UpdateWithTransaction` (metodo).


[!code-csharp[Main](batch-updating-cs/samples/sample8.cs)]

## <a name="summary"></a>Riepilogo

Il controllo GridView offre funzionalità di modifica predefinito per ogni riga, ma manca il supporto per la creazione di interfacce interamente modificabili. Come abbiamo visto in questa esercitazione, queste interfacce sono possibili, ma richiedono un po' di lavoro. Per creare un controllo GridView in cui ogni riga è modificabile, è necessario convertire i campi s GridView in TemplateFields e definire l'interfaccia di modifica all'interno di `ItemTemplate` s. Inoltre, aggiornare tutti i - tipo di controlli Web pulsante deve essere aggiunto alla pagina, separata da GridView. Questi pulsanti `Click` gestori eventi è necessario enumerare i dispositivi di GridView `Rows` insieme, archiviare le modifiche in un `ProductsDataTable`e passare le informazioni aggiornate relative al metodo BLL appropriato.

Nella prossima esercitazione vedremo come creare un'interfaccia per l'eliminazione di batch. In particolare, ogni riga GridView verrà includono una casella di controllo e invece di aggiornare tutti-tipo di pulsanti, avremo pulsanti Elimina le righe selezionate.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Teresa Murphy e David Suru. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](wrapping-database-modifications-within-a-transaction-cs.md)
> [Successivo](batch-deleting-cs.md)
