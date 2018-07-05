---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: Aggiornamento ed eliminazione di dati binari esistenti (VB) | Microsoft Docs
author: rick-anderson
description: Nelle esercitazioni precedenti abbiamo visto come il controllo GridView rende più semplice modificare ed eliminare i dati di testo. In questa esercitazione viene illustrato come il controllo GridView inoltre rendere...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: eac5d4da253e3553b50a22fc48fa710a99ee030b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386640"
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>Aggiornamento ed eliminazione di dati binari esistenti (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) o [Scarica il PDF](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> Nelle esercitazioni precedenti abbiamo visto come il controllo GridView rende più semplice modificare ed eliminare i dati di testo. In questa esercitazione viene illustrato come il controllo GridView inoltre rende possibile modificare ed eliminare i dati binari, se tali dati binari viene salvati nel database o archiviati nel file system.


## <a name="introduction"></a>Introduzione

Failover di tre esercitazioni precedenti è stato aggiunto un bel di funzionalità per l'utilizzo di dati binari. È stato avviato mediante l'aggiunta di un `BrochurePath` colonna il `Categories` di tabella e aggiornati di conseguenza l'architettura. Sono stati aggiunti anche i metodi di livello di accesso ai dati e Business Logic Layer per lavorare con la tabella s categorie esistenti `Picture` colonna che contiene gli oggetti contenuto binario di un file di immagine. È stato creato le pagine web per presentare i dati binari in un controllo GridView a un collegamento di download per brochure, immagine categoria s mostrata un `<img>` elemento e aver aggiunto un controllo DetailsView per consentire agli utenti di aggiungere una nuova categoria e caricare i dati di brochure e immagine.

Per essere implementata resta che la possibilità di modificare ed eliminare le categorie esistenti, che verrà eseguito in questa esercitazione usando i GridView s incorporati di modifica ed eliminazione di funzionalità. Quando si modifica una categoria, l'utente sarà in grado di caricare una nuova immagine o facoltativamente dispongono della categoria di continuare a usare quello esistente. Per brochure, è possibile scegliere di usare brochure esistente, per caricare una nuovo brochure o per indicare che la categoria non ha più una brochure associata. Introduzione a ti permettono di s.

## <a name="step-1-updating-the-data-access-layer"></a>Passaggio 1: Aggiornare il livello di accesso ai dati

DAL ha generato automaticamente `Insert`, `Update`, e `Delete` metodi, ma questi metodi sono stati generati in base il `CategoriesTableAdapter` query principale s, che non include il `Picture` colonna. Pertanto, il `Insert` e `Update` metodi non includono i parametri per specificare i dati binari per l'immagine di categoria s. Come abbiamo fatto il [esercitazione precedente](including-a-file-upload-option-when-adding-a-new-record-vb.md), è necessario creare un nuovo metodo TableAdapter per l'aggiornamento di `Categories` tabella quando si specificano i dati binari.

Aprire il DataSet tipizzato e, dalla finestra di progettazione, fare clic su di `CategoriesTableAdapter` intestazione s e scegliere Aggiungi Query dal menu di scelta rapida per launche la configurazione guidata Query TableAdapter. Questa procedura guidata viene avviata da richiede la modalità della query TableAdapter di accesso del database. Scegliere Usa istruzioni SQL e fare clic su Avanti. Il passaggio successivo richiede il tipo di query da generare. Poiché abbiamo nuovamente la creazione di una query per aggiungere un nuovo record per il `Categories` di tabella, scegliere l'aggiornamento e fare clic su Avanti.


[![Selezionare l'opzione di aggiornamento](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Figura 1**: selezionare l'opzione di aggiornamento ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


È ora necessario specificare il `UPDATE` istruzione SQL. La procedura guidata suggerisce automaticamente un `UPDATE` istruzione corrisponde alla query principale s TableAdapter (uno che aggiorna il `CategoryName`, `Description`, e `BrochurePath` valori). Modificare l'istruzione in modo che il `Picture` colonna è incluso con un `@Picture` parametro, come illustrato di seguito:


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

Schermata finale della procedura guidata viene chiesto di specificare un nome al nuovo metodo di TableAdapter. Immettere `UpdateWithPicture` e fare clic su Fine.


[![Denominare il nuovo UpdateWithPicture TableAdapter (metodo)](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Figura 2**: nome del nuovo metodo TableAdapter `UpdateWithPicture` ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Passaggio 2: Aggiungere i metodi di livello per la logica di Business

Oltre all'aggiornamento DAL, è necessario aggiornare il livello BLL in modo da includere metodi per l'aggiornamento ed eliminazione di una categoria. Questi sono i metodi che verranno richiamati dal livello di presentazione.

Per eliminare una categoria, è possibile usare la `CategoriesTableAdapter` s autogenerato `Delete` (metodo). Aggiungere il metodo seguente per il `CategoriesBLL` classe:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

Per questa esercitazione, s ti permettono di creare due metodi per l'aggiornamento di una categoria - che prevede che i dati immagine binari e richiama il `UpdateWithPicture` metodo appena aggiunto per il `CategoriesTableAdapter` e un altro che accetta solo la `CategoryName`, `Description`e `BrochurePath`i valori e viene utilizzato `CategoriesTableAdapter` classe s autogenerato `Update` istruzione. La logica alla base usando due metodi è che in alcuni casi, un utente potrebbe essere necessario aggiornare l'immagine di s categoria insieme ai relativi altri campi, in cui i casi l'utente saranno necessario caricare la nuova immagine. I dati binari dell'immagine caricata s sono quindi utilizzabile nel `UPDATE` istruzione. In altri casi, l'utente potrebbe essere interessata solo l'aggiornamento, ad esempio, il nome e una descrizione. Ma se il `UPDATE` istruzione prevede che i dati binari per il `Picture` anche colonna, quindi abbiamo d necessario fornire tali informazioni. Ciò richiede un ulteriore percorso per il database per riportare i dati dell'immagine per il record da modificare. Pertanto, intendiamo due `UPDATE` metodi. Il livello di logica di Business è determineranno quello da usare è basata sul fatto che i dati immagine viene forniti durante l'aggiornamento della categoria.

A tale scopo, aggiungere due metodi per la `CategoriesBLL` classe, entrambi denominati `UpdateCategory`. Il primo deve accettare tre `String` s, una `Byte` array e un `Integer` come input parametri; il secondo, solo tre `String` s e un `Integer`. Il `String` sono parametri di input per il nome della categoria s, descrizione e percorso del file brochure, il `Byte` array sia per il contenuto binario di immagine, la categoria s e il `Integer` identifica il `CategoryID` del record da aggiornare. Si noti che il primo overload richiama il secondo se passato `Byte` matrice è `Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Passaggio 3: Copiare tramite l'inserimento e la funzionalità di visualizzazione

Nel [esercitazione precedente](including-a-file-upload-option-when-adding-a-new-record-vb.md) abbiamo creato una pagina denominata `UploadInDetailsView.aspx` che elencate tutte le categorie in un controllo GridView e fornito un DetailsView per aggiungere nuove categorie al sistema. In questa esercitazione si estenderà GridView per includere la modifica ed eliminazione di supporto. Invece di continuare a lavorare da `UploadInDetailsView.aspx`, ti permettono di s invece inserire questo cambia esercitazione s nel `UpdatingAndDeleting.aspx` pagina dalla stessa cartella, `~/BinaryData`. Copiare e incollare il markup dichiarativo e codice dal `UploadInDetailsView.aspx` a `UpdatingAndDeleting.aspx`.

Iniziare aprendo il `UploadInDetailsView.aspx` pagina. Copiare tutta la sintassi dichiarativa all'interno di `<asp:Content>` elemento, come illustrato nella figura 3. Successivamente, aprire `UpdatingAndDeleting.aspx` e incollare questo markup all'interno di relativo `<asp:Content>` elemento. Analogamente, copiare il codice dal `UploadInDetailsView.aspx` pagina classe code-behind s a `UpdatingAndDeleting.aspx`.


[![Copiare il codice dichiarativo da UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Figura 3**: copiare dal Markup dichiarativo `UploadInDetailsView.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


Dopo aver copiato il markup dichiarativo e codice, visitare `UpdatingAndDeleting.aspx`. Dovrebbe essere lo stesso output e avere la stessa esperienza utente come con `UploadInDetailsView.aspx` pagina dall'esercitazione precedente.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Passaggio 4: Aggiunta di eliminazione di supporto per l'oggetto ObjectDataSource e GridView

Come illustrato nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) esercitazione, il controllo GridView offre funzionalità di eliminazione incorporate e queste funzionalità possono essere abilitate al segno di graduazione di una casella di controllo se s griglia sottostante origine dati supporta l'eliminazione. Attualmente ObjectDataSource di GridView è associato a (`CategoriesDataSource`) non supporta l'eliminazione.

Per risolvere questo problema, fare clic sull'opzione Configura origine dati nello smart tag s ObjectDataSource per avviare la procedura guidata. La prima schermata mostra che ObjectDataSource è configurato per funzionare con il `CategoriesBLL` classe. Scegliere "Avanti". Attualmente, solo l'oggetto ObjectDataSource s `InsertMethod` e `SelectMethod` sono specificate proprietà. Tuttavia, la procedura guidata popolati automaticamente gli elenchi a discesa nelle schede UPDATE e DELETE con la `UpdateCategory` e `DeleteCategory` metodi, rispettivamente. Infatti nel `CategoriesBLL` classe è contrassegnata come questi metodi usando la `DataObjectMethodAttribute` come i metodi predefiniti per l'aggiornamento e l'eliminazione.

Per ora, impostare l'elenco di riepilogo aggiornamento scheda s su (nessuno), ma lasciare l'elenco a discesa scheda s eliminazione impostato su `DeleteCategory`. È necessario tornare a questa procedura guidata nel passaggio 6 per aggiungere il supporto ad aggiornamento.


[![Configurare ObjectDataSource per usare il metodo DeleteCategory](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Figura 4**: configurare ObjectDataSource per usare il `DeleteCategory` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> Dopo aver completato la procedura guidata, Visual Studio potrebbe chiedere se si desidera aggiornare i campi e le chiavi, che rigenera i dati Web controlla i campi. Scegliere No, perché se si sceglie Sì sovrascriverà le personalizzazioni qualsiasi campo per avviare l'applicazione.


ObjectDataSource include ora un valore per la relativa `DeleteMethod` proprietà, nonché un `DeleteParameter`. È importante ricordare che quando si usa la procedura guidata per specificare i metodi, Visual Studio imposta la s ObjectDataSource `OldValuesParameterFormatString` proprietà `original_{0}`, che può causare problemi con l'aggiornamento ed eliminazione chiamate al metodo. Pertanto, cancellare completamente questa proprietà o ripristinare le impostazioni per l'impostazione predefinita, `{0}`. Se è necessario aggiornare la memoria su questa proprietà di ObjectDataSource, vedere la [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) esercitazione.

Dopo aver completato la procedura guidata e correggere il `OldValuesParameterFormatString`, markup dichiarativo s ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

Dopo la configurazione di ObjectDataSource, aggiungere funzionalità di eliminazione a GridView selezionando la casella di controllo Abilita eliminazione nello smart tag s GridView. Verrà aggiunto un CommandField a GridView cui `ShowDeleteButton` è impostata su `True`.


[![Abilitare il supporto per l'eliminazione in GridView](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Figura 5**: abilitare il supporto per l'eliminazione in GridView ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


Si consiglia di testare la funzionalità di eliminazione. È presente una chiave esterna tra le `Products` tabella s `CategoryID` e il `Categories` la tabella s `CategoryID`, pertanto si otterrà un'eccezione di violazione di vincolo di chiave esterna se si tenta di eliminare una delle prime otto categorie. Per testare questa funzionalità orizzontale, aggiungere una nuova categoria, fornendo brochure sia un'immagine. La categoria di test, illustrata nella figura 6, include un file brochure di test denominato `Test.pdf` e un'immagine di test. Figura 7 mostra il controllo GridView dopo aver aggiunto la categoria di test.


[![Aggiungere una categoria di Test con un'immagine e Brochure](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Figura 6**: aggiungere una categoria di Test con un'immagine e Brochure ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![Dopo aver inserito la categoria di Test, viene visualizzato nel controllo GridView.](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Figura 7**: dopo aver inserito la categoria di Test, viene visualizzato in GridView ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


In Visual Studio, aggiornare Esplora soluzioni. Si noterà ora un nuovo file nei `~/Brochures` cartella `Test.pdf` (vedere la figura 8).

Successivamente, fare clic sul collegamento Elimina nella riga relativa alla categoria di Test, causando il postback della pagina e il `CategoriesBLL` classe s `DeleteCategory` metodo da attivare. Questa operazione richiamerà la s DAL `Delete` metodo causando appropriato `DELETE` istruzione da inviare al database. I dati vengano quindi riassociati a GridView e il markup viene inviato al client con la categoria di Test non è più presente.

Mentre il flusso di lavoro di eliminazione è stato rimosso il record di categoria di Test dal `Categories` tabella, non è stato rimosso il relativo file brochure dal file system s server web. Aggiornare Esplora soluzioni e si noterà che `Test.pdf` restano `~/Brochures` cartella.


![Il File Test.pdf non è stato eliminato dal File System s Server Web](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Figura 8**: il `Test.pdf` File non è stato eliminato dal File System s Server Web


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Passaggio 5: Rimozione del File di Brochure s categoria eliminato

Uno degli svantaggi dell'archiviazione di dati binari esterni al database è che è necessario effettuare passaggi aggiuntivi per pulire questi file quando viene eliminato il record del database associato. Il controllo GridView e ObjectDataSource fornire eventi che vengono generati prima e dopo aver eseguito il comando delete. È effettivamente necessario creare i gestori eventi per entrambi gli eventi di pre-elaborazione e post-azioni. Prima di `Categories` record viene eliminato è necessario determinare il percorso del file s PDF, ma non vogliamo t desidera eliminare il file PDF, prima che la categoria viene eliminata nel caso in cui è presente un'eccezione e la categoria non viene eliminata.

S GridView [ `RowDeleting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) viene attivato prima di aver chiamato il comando di eliminazione ObjectDataSource s, mentre le [ `RowDeleted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) viene attivato dopo. Creare i gestori eventi per questi due eventi usando il codice seguente:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

Nel `RowDeleting` gestore dell'evento, il `CategoryID` della riga in corso l'eliminazione viene catturato da s GridView `DataKeys` raccolta, che è accessibile in questo gestore dell'evento tramite la `e.Keys` raccolta. Successivamente, il `CategoriesBLL` classe s `GetCategoryByCategoryID(categoryID)` viene richiamata per restituire le informazioni del record da eliminare. Se l'oggetto restituito `CategoriesDataRow` oggetto dispone di un non -`NULL``BrochurePath` valore viene archiviato nella variabile di pagina `deletedCategorysPdfPath` in modo che i file può essere eliminato nel `RowDeleted` gestore dell'evento.

> [!NOTE]
> Invece di recuperare il la `BrochurePath` illustra in dettaglio per il `Categories` registrare in corso l'eliminazione nel `RowDeleting` gestore eventi, avremmo potuto in alternativa aggiungere il `BrochurePath` al s GridView `DataKeyNames` proprietà e accessibile il valore di record s tramite il `e.Keys` raccolta. In questo modo sarebbe leggermente aumentare le dimensioni dello stato visualizzazione s GridView, ma potrebbe ridurre la quantità di codice necessario e salvare una corsa nel database.


Dopo aver ObjectDataSource è stato richiamato il comando di eliminazione sottostante s, la s GridView `RowDeleted` attivazione gestore dell'evento. Se sono presenti eccezioni in grado di eliminare i dati ed è presente un valore per `deletedCategorysPdfPath`, quindi il file PDF viene eliminato dal file system. Si noti che questo codice aggiuntivo non è necessaria per pulire i dati binari s categoria associati con la relativa immagine. Che s poiché i dati dell'immagine vengono archiviati direttamente nel database, pertanto, l'eliminazione di `Categories` riga Elimina anche i dati di immagine che la categoria di s.

Dopo aver aggiunto i due gestori di eventi, eseguire di nuovo questo test case. Quando si elimina la categoria, viene eliminato anche il file PDF associato.

L'aggiornamento di un dati binari s record associato esistente fornisce alcune problematiche interessanti. Nella parte restante di questa esercitazione approfondisce l'aggiunta di funzionalità di aggiornamento brochure le immagini. Passaggio 6 analizza le tecniche per l'aggiornamento di informazioni brochure mentre il Step 7 esamina l'aggiornamento dell'immagine.

## <a name="step-6-updating-a-category-s-brochure"></a>Passaggio 6: Aggiornare una Brochure s categoria

Come illustrato nell'esercitazione di una panoramica di inserimento, aggiornamento ed eliminazione di dati, il controllo GridView offre supporto modifica predefinito a livello di riga che può essere implementato dal segno di graduazione di una casella di controllo se l'origine dati sottostante è configurato in modo appropriato. Attualmente, il `CategoriesDataSource` ObjectDataSource non è ancora configurato per includere l'aggiornamento del supporto, che in questo modo consentono s che in.

Fare clic sul collegamento Configura origine dati dalla procedura guidata ObjectDataSource s e procedere al secondo passaggio. Perché il `DataObjectMethodAttribute` usato in `CategoriesBLL`, l'elenco di riepilogo a discesa di aggiornamento deve essere popolato automaticamente con il `UpdateCategory` overload che accetta quattro parametri di input (per tutte le colonne ma `Picture`). Modificare questa impostazione in modo che utilizzi l'overload con cinque parametri.


[![Configurare ObjectDataSource per usare il metodo UpdateCategory che includa un parametro per immagine](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Figura 9**: configurare ObjectDataSource per usare il `UpdateCategory` metodo che includa un parametro per `Picture` ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


ObjectDataSource include ora un valore per la relativa `UpdateMethod` proprietà, nonché corrispondente `UpdateParameter` s. Come indicato nel passaggio 4, Visual Studio imposta la s ObjectDataSource `OldValuesParameterFormatString` proprietà `original_{0}` quando si usa la procedura guidata Configura origine dati. Verrà causare problemi con l'aggiornamento e le chiamate ai metodi di eliminazione. Pertanto, cancellare completamente questa proprietà o ripristinare le impostazioni per l'impostazione predefinita, `{0}`.

Dopo aver completato la procedura guidata e correggere il `OldValuesParameterFormatString`, markup dichiarativo s ObjectDataSource dovrebbe essere simile al seguente:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

Per abilitare le funzionalità di modifica predefinite di GridView s, selezionare l'opzione Abilita modifica dallo smart tag s GridView. Questa operazione verrà impostata la s CommandField `ShowEditButton` proprietà `True`, ottenendo l'aggiunta di un pulsante Modifica (e i pulsanti di aggiornamento e annullamento per la riga da modificare).


[![Configurare il controllo GridView alla modifica del supporto](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Figura 10**: configurare il controllo GridView alla modifica del supporto ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


Visita la pagina tramite un browser e fare clic su uno dei pulsanti di modifica s della riga. Il `CategoryName` e `Description` BoundField vengono visualizzati come caselle di testo. Il `BrochurePath` TemplateField manca un' `EditItemTemplate`, quindi continua a mostrare relativo `ItemTemplate` un collegamento a brochure. Il `Picture` ImageField esegue il rendering come una casella di testo la cui proprietà `Text` proprietà viene assegnato il valore di istanze della classe ImageField `DataImageUrlField` valore, in questo caso `CategoryID`.


[![Il controllo GridView non ha un'interfaccia di modifica per BrochurePath](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Figura 11**: il controllo GridView non dispone di un'interfaccia di modifica `BrochurePath` ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Personalizzazione di`BrochurePath`s interfaccia di modifica

È necessario creare un'interfaccia di modifica per il `BrochurePath` TemplateField, che consente all'utente uno:

- Lasciare la brochure categoria s come-è,
- Aggiornare la brochure categoria s caricando una brochure di nuovo, o
- Rimuovere completamente la brochure categoria s (nel caso che la categoria non ha più una brochure associato).

È anche necessario aggiornare il `Picture` interfaccia per la modifica di ImageField s, ma si otterrà a questa nel passaggio 7.

GridView s nello smart tag, fare clic sul collegamento di modifica modelli e selezionare il `BrochurePath` TemplateField s `EditItemTemplate` nell'elenco a discesa. Aggiungere un controllo RadioButtonList Web a questo modello, l'impostazione relativa `ID` proprietà `BrochureOptions` e il relativo `AutoPostBack` proprietà `True`. Dalla finestra delle proprietà, fare clic sui puntini di sospensione il `Items` proprietà, che attiverà il `ListItem` Editor della raccolta. Aggiungere le tre opzioni seguenti con `Value` s 1, 2 e 3, rispettivamente:

- Usare brochure corrente
- Rimuovere brochure corrente
- Carica nuovo brochure

Imposta i primi `ListItem` s `Selected` proprietà `True`.


![Aggiungere tre ListItems RadioButtonList](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Figura 12**: aggiungere tre `ListItem` s per RadioButtonList


Di sotto di RadioButtonList, aggiungere un controllo FileUpload denominato `BrochureUpload`. Impostare relativi `Visible` proprietà `False`.


[![Aggiungere un RadioButtonList e controllo FileUpload EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Figura 13**: aggiungere un RadioButtonList e FileUpload controllo per il `EditItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


Questo RadioButtonList fornisce le tre opzioni per l'utente. L'idea è che il controllo FileUpload verrà visualizzato solo se l'ultima opzione, brochure nuovo caricamento, è selezionato. A tale scopo, creare un gestore eventi per s RadioButtonList `SelectedIndexChanged` eventi e aggiungere il codice seguente:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

Poiché i controlli RadioButtonList e FileUpload sono all'interno di un modello, è necessario scrivere un po' di codice per questi controlli di accesso a livello di codice. Il `SelectedIndexChanged` gestore dell'evento viene passato un riferimento di RadioButtonList nel `sender` parametro di input. Per ottenere il controllo FileUpload, è necessario ottenere l'elemento padre s RadioButtonList controllo e usare il `FindControl("controlID")` metodo da tale posizione. Dopo aver ottenuto un riferimento a controlli RadioButtonList sia FileUpload, il FileUpload controllare 1!s `Visible` è impostata su `True` solo se s RadioButtonList `SelectedValue` è uguale a 3, ovvero il `Value` per brochure di nuovo il caricamento `ListItem`.

Con questo codice, si consiglia di testare l'interfaccia di modifica. Fare clic sul pulsante Modifica per una riga. Inizialmente, l'opzione brochure corrente Usa debba essere selezionato. Modifica l'indice selezionato determina un postback. Se la terza opzione è selezionata, viene visualizzato il controllo FileUpload, in caso contrario è nascosto. Figura 14 viene illustrata l'interfaccia di modifica quando il pulsante di modifica prima di tutto è selezionato; Figura 15 viene illustrata l'interfaccia dopo aver selezionata l'opzione brochure nuovo caricamento.


[![Inizialmente, uso corrente brochure che opzione è selezionata](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Figura 14**: inizialmente, uso corrente brochure opzione è selezionata ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![Scelta di caricamento nuovo brochure opzione Visualizza il controllo FileUpload](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Figura 15**: scelta brochure di nuovo il caricamento opzione Visualizza il controllo FileUpload ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Brochure di salvataggio File, aggiornare il`BrochurePath`colonna

Quando si fa clic sul pulsante di aggiornamento s GridView, relativo `RowUpdating` viene generato l'evento. ObjectDataSource viene richiamato il comando di aggiornamento s e quindi s GridView `RowUpdated` viene generato l'evento. Ad esempio con l'eliminazione del flusso di lavoro, è necessario creare gestori eventi per entrambi questi eventi. Nel `RowUpdating` gestore eventi, è necessario determinare l'azione da eseguire in base il `SelectedValue` del `BrochureOptions` RadioButtonList:

- Se il `SelectedValue` è 1, si vuole continuare a usare lo stesso `BrochurePath` impostazione. Pertanto, è necessario impostare la s ObjectDataSource `brochurePath` parametro per l'oggetto esistente `BrochurePath` valore del record da aggiornare. Gli oggetti ObjectDataSource `brochurePath` parametro può essere impostato utilizzando `e.NewValues["brochurePath"] = value`.
- Se il `SelectedValue` è 2, allora sarà necessario impostare il record s `BrochurePath` valore `NULL`. Questa operazione può essere eseguita impostando s ObjectDataSource `brochurePath` parametro per `Nothing`, che comporta un database `NULL` usate nei `UPDATE` istruzione. Se è presente un file brochure esistente che viene rimosso, è necessario eliminare il file esistente. Tuttavia, si vuole solo eseguire questa operazione se il completamento dell'aggiornamento senza generare un'eccezione.
- Se il `SelectedValue` è 3, è necessario assicurarsi che l'utente ha caricato un file con estensione PDF e quindi salvarla nel file System e aggiornare il record s `BrochurePath` valore della colonna. Inoltre, se è presente un file brochure esistente che viene sostituito, è necessario eliminare il file precedente. Tuttavia, si vuole solo eseguire questa operazione se il completamento dell'aggiornamento senza generare un'eccezione.

I passaggi necessari per essere completata quando s RadioButtonList `SelectedValue` è 3 sono praticamente identici a quelli utilizzati da s DetailsView `ItemInserting` gestore dell'evento. Questo gestore eventi viene eseguito quando viene aggiunto un nuovo record di categoria dal controllo DetailsView è stato aggiunto nel [esercitazione precedente](including-a-file-upload-option-when-adding-a-new-record-vb.md). Pertanto, behooves per effettuare il refactoring di questa funzionalità out in metodi separati. In particolare, è stata spostata la funzionalità comuni in due metodi:

- `ProcessBrochureUpload(FileUpload, out bool)` accetta come input un'istanza del controllo FileUpload e un valore di output booleano che specifica se l'operazione di eliminazione o modifica procedere o se deve essere annullata a causa di errori di convalida. Questo metodo restituisce il percorso del file salvato o `null` se è stato salvato alcun file.
- `DeleteRememberedBrochurePath` Elimina il file specificato dal percorso nella variabile di pagina `deletedCategorysPdfPath` se `deletedCategorysPdfPath` non è `null`.

Il codice per questi due metodi segue. Notare la somiglianza tra `ProcessBrochureUpload` e il controllo DetailsView s `ItemInserting` gestore dell'evento dall'esercitazione precedente. In questa esercitazione hai aggiornato i gestori di eventi s DetailsView per usare questi nuovi metodi. Scaricare il codice associato a questa esercitazione per vedere le modifiche ai gestori eventi s DetailsView.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

Le s GridView `RowUpdating` e `RowUpdated` gestori eventi usano la `ProcessBrochureUpload` e `DeleteRememberedBrochurePath` metodi, come illustrato nel codice seguente:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Si noti come il `RowUpdating` gestore dell'evento utilizza una serie di istruzioni condizionali per eseguire l'azione appropriata in base il `BrochureOptions` RadioButtonList s `SelectedValue` valore della proprietà.

Con questo codice, è possibile modificare una categoria e averlo usare relativo brochure corrente, non usare Nessun brochure o caricarne uno nuovo. Proseguire e provarlo subito. Impostare punti di interruzione `RowUpdating` e `RowUpdated` gestori eventi per farsi un'idea del flusso di lavoro.

## <a name="step-7-uploading-a-new-picture"></a>Passaggio 7: Caricare una nuova immagine

Il `Picture` ImageField s esegue il rendering dell'interfaccia di modifica come una casella di testo popolato con il valore dal relativo `DataImageUrlField` proprietà. Durante la modifica del flusso di lavoro, il controllo GridView passa un parametro a ObjectDataSource con il nome del parametro s il valore di istanze della classe ImageField `DataImageUrlField` proprietà e il parametro s valore il valore immesso nella casella di testo nell'interfaccia di modifica. Questo comportamento è appropriato quando l'immagine viene salvata come file nel file system e `DataImageUrlField` contiene l'URL completo dell'immagine. Con tali circostanze, l'interfaccia di modifica consente di visualizzare l'URL dell'immagine s nella casella di testo, quale l'utente può modificare e sono salvati nel database. Concesso, t interfaccia predefinito consentono all'utente di caricare una nuova immagine, ma permettono di modificare l'URL dell'immagine dal valore corrente in un altro. Per questa esercitazione, tuttavia, il valore predefinito s ImageField modifica interfaccia non è sufficiente perché il `Picture` vengono archiviati i dati binari direttamente nel database e il `DataImageUrlField` proprietà contiene solo il `CategoryID`.

Per comprendere meglio cosa accade nel corso di questa esercitazione, quando un utente modifica una riga con un ImageField, si consideri l'esempio seguente: un utente modifica una riga con `CategoryID` 10, causando la `Picture` ImageField per eseguire il rendering come una casella di testo con il valore 10. Si supponga che l'utente modifica il valore nella casella di testo su 50 e fa clic sul pulsante di aggiornamento. Si verifica un postback e GridView crea inizialmente un parametro denominato `CategoryID` con il valore 50. Tuttavia, prima che il controllo GridView invia questo parametro (e il `CategoryName` e `Description` parametri), aggiunge i valori dal `DataKeys` raccolta. Pertanto, questa sovrascriverà la `CategoryID` parametro con il sottostante oggetto riga s corrente `CategoryID` valore, 10. In breve, la s ImageField modifica interfaccia non ha alcun effetto sul flusso di lavoro modifica per questa esercitazione perché i nomi di istanze della classe ImageField `DataImageUrlField` proprietà e la griglia s `DataKey` valore sono equivalenti.

Mentre ImageField rende più semplice visualizzare un'immagine basata sui dati del database, non abbiamo t desidera fornire una casella di testo nell'interfaccia di modifica. Piuttosto, si vuole offrire un controllo FileUpload che l'utente finale può usare per cambiare l'immagine di s categoria. A differenza di `BrochurePath` valore, per queste esercitazioni è ve deciso in modo da richiedere che ciascuna categoria deve avere un'immagine. Pertanto, non abbiamo t necessità di consentire all'utente di indicare che è presente alcuna immagine associata l'utente non può caricare una nuova immagine o lasciare l'immagine corrente come-è.

Per personalizzare l'interfaccia di modifica s ImageField, è necessario convertirlo in un TemplateField. GridView s nello smart tag, fare clic sul collegamento Modifica colonne e fare clic su Converti il campo in un collegamento TemplateField selezionare ImageField.


![Convertire ImageField in un TemplateField](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Figura 16**: convertire ImageField in un TemplateField


La conversione di ImageField in un TemplateField in questo modo viene generato un TemplateField con due modelli. Come la seguente sintassi dichiarativa, la `ItemTemplate` contiene un'immagine Web controllo la cui `ImageUrl` proprietà viene assegnato usando la sintassi di associazione dati basata su dispositivi ImageField `DataImageUrlField` e `DataImageUrlFormatString` proprietà. Il `EditItemTemplate` contiene una casella di testo la cui `Text` è associata la proprietà sul valore specificato per il `DataImageUrlField` proprietà.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

È necessario aggiornare il `EditItemTemplate` usare un controllo FileUpload. Da GridView tag smart s fare clic su Modifica modelli collegarsi e quindi selezionare il `Picture` TemplateField s `EditItemTemplate` nell'elenco a discesa. Il modello dovrebbe essere una casella di testo rimuovere questo. Successivamente, trascinare un controllo FileUpload dalla casella degli strumenti nel modello, l'impostazione relativa `ID` a `PictureUpload`. Aggiungere anche il testo per modificare l'immagine di categoria s, specificare una nuova immagine. Per mantenere lo stesso dell'immagine di categoria s, lasciare vuoto il campo al modello, nonché.


[![Aggiungere un controllo FileUpload a EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Figura 17**: aggiungere un controllo FileUpload per il `EditItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


Dopo la personalizzazione dell'interfaccia di modifica, visualizzare lo stato di avanzamento in un browser. Quando si visualizza una riga in modalità di sola lettura, viene visualizzata l'immagine di s categoria come prima, ma facendo clic sul pulsante Modifica viene eseguito il rendering della colonna immagine come testo con un controllo FileUpload.


[![L'interfaccia di modifica include un controllo FileUpload](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Figura 18**: l'interfaccia di modifica include un controllo FileUpload ([fare clic per visualizzare l'immagine con dimensioni normali](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


È importante ricordare che ObjectDataSource è configurato per richiamare il `CategoriesBLL` classe s `UpdateCategory` metodo che accetta come input i dati binari per l'immagine come un `Byte` matrice. Se questa matrice è `Nothing`, tuttavia, l'alternativa `UpdateCategory` overload viene chiamato, quali problemi il `UPDATE` istruzione SQL che non modifica il `Picture` intatta dell'immagine di colonna, lasciando la categoria s corrente. Pertanto, in s GridView `RowUpdating` gestore dell'evento è necessario fare riferimento a livello di codice il `PictureUpload` FileUpload controllano e determinare se è stato caricato un file. Se uno non è stato caricato, allora lo facciamo *non* per specificare un valore per il `picture` parametro. D'altra parte, se è stato caricato un file di `PictureUpload` controllo FileUpload, è opportuno assicurarsi che sia un file JPG. Se si tratta, quindi è possibile inviare il contenuto binario a ObjectDataSource mediante le `picture` parametro.

Come con il codice usato nel passaggio 6, gran parte del codice necessario già esiste in s DetailsView `ItemInserting` gestore dell'evento. Pertanto, ho va effettuato il refactoring della funzionalità comune in un nuovo metodo, `ValidPictureUpload`e aggiornato il `ItemInserting` gestore eventi per utilizzare questo metodo.

Aggiungere il codice seguente all'inizio di istanze della classe GridView `RowUpdating` gestore dell'evento. È importante che questo codice vengono prima il codice che consente di salvare il file brochure poiché non abbiamo t s desidera salvare brochure nel file System s server web se è caricato un file di immagine non valido.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

Il `ValidPictureUpload(FileUpload)` metodo accetta un controllo FileUpload come unico parametro input e controlla l'estensione di file caricato s per assicurarsi che il file caricato non è in formato JPG; viene chiamato solo se viene caricato un file di immagine. Se è caricato alcun file, quindi non è impostata, il parametro di immagine e pertanto utilizza il valore predefinito di `Nothing`. Se è stata caricata un'immagine e `ValidPictureUpload` restituisce `True`, il `picture` parametro assegnato i dati binari dell'immagine caricata; se il metodo restituisce `False`, il flusso di lavoro di aggiornamento viene annullato e terminato il gestore dell'evento.

Il `ValidPictureUpload(FileUpload)` codice del metodo, che è stata sottoposta a refactoring da s DetailsView `ItemInserting` gestore eventi seguente:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Passaggio 8: Sostituendo le immagini originali categorie con file. jpg

Tenere presente che le immagini di otto categorie originali sono i file bitmap sottoposta a wrapping in un'intestazione OLE. Ora che è stata aggiunta la possibilità di modificare un'immagine di record s esistente, si consiglia di sostituire tali bitmap con file. jpg. Se si desidera continuare a usare le immagini di categoria corrente, è possibile convertirli in file. jpg attenendosi alla procedura seguente:

1. Salvare le immagini bitmap nell'unità disco rigido. Visita il `UpdatingAndDeleting.aspx` pagina nel browser e per ognuna delle prime otto categorie, fare doppio clic sull'immagine e scegliere di salvare l'immagine.
2. Aprire l'immagine nell'editor di immagini di scelta. È possibile usare Microsoft Paint, ad esempio.
3. Salvare la mappa di bit come un'immagine JPG.
4. Aggiornare l'immagine di categoria s tramite l'interfaccia di modifica, usando il file JPG.

Dopo la modifica di una categoria e caricare l'immagine JPG, l'immagine non eseguirà il rendering nel browser in quanto il `DisplayCategoryPicture.aspx` pagina è la rimozione dei primi 78 byte dalle immagini delle prime otto categorie. Risolvere il problema rimuovendo il codice che esegue la rimozione di intestazione OLE. Dopo questa operazione, il `DisplayCategoryPicture.aspx``Page_Load` gestore eventi deve avere solo il codice seguente:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> Il `UpdatingAndDeleting.aspx` pagina inserimento e la modifica di interfacce potrebbe usare più consistenti. Il `CategoryName` e `Description` BoundField in GridView e DetailsView deve essere convertito in TemplateFields. Poiché `CategoryName` nepovoluje hodnotu `NULL` valori, RequiredFieldValidator deve essere aggiunto. E `Description` casella probabilmente deve essere convertito in una casella di testo su più righe. Posso lasciare questi ultimi ritocchi come esercizio per l'utente.


## <a name="summary"></a>Riepilogo

Questa esercitazione si completa l'esaminare l'uso con dati binari. In questa esercitazione e i tre precedenti, abbiamo visto i dati come binari possono essere archiviati nel file system o direttamente all'interno del database. Un utente fornisce i dati binari al sistema selezionando un file dal disco rigido locale e caricarlo per il server web, in cui può essere archiviato nel file system o inserito nel database. ASP.NET 2.0 include un controllo FileUpload che rende tale interfaccia semplice come trascinamento della selezione. Tuttavia, come indicato nel [caricare file](uploading-files-vb.md) è solo di esercitazione, il controllo FileUpload particolarmente adatto per i caricamenti di file relativamente piccola, idealmente non supera un megabyte. Abbiamo anche analizzato come associare i dati caricati con il modello di dati sottostanti, nonché come modificare ed eliminare i dati binari dal record esistenti.

Il set successivo di esercitazioni illustra diverse tecniche di memorizzazione nella cache. La memorizzazione nella cache fornisce un mezzo per migliorare una s applicazione prestazioni complessive prendendo i risultati di operazioni complesse e archiviandoli in un percorso che è possibile accedere più rapidamente.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore capo di questa esercitazione è stata Teresa Murphy. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](including-a-file-upload-option-when-adding-a-new-record-vb.md)
