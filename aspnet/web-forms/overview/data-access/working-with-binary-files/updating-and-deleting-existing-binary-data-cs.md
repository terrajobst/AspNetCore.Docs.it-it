---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
title: Aggiornamento ed eliminazione di dati binari esistenti (c#) | Documenti Microsoft
author: rick-anderson
description: "Nelle esercitazioni precedenti è stato illustrato come il controllo GridView rende semplice per modificare ed eliminare dati di testo. In questa esercitazione viene illustrato come inoltre rendere il controllo GridView..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 35798f21-1606-434b-83f8-30166906ef49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
msc.type: authoredcontent
ms.openlocfilehash: f2fca1e91720fba0215e12b1a1894a3a31e86b5c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="updating-and-deleting-existing-binary-data-c"></a>Aggiornamento ed eliminazione di dati binari esistenti (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_CS.exe) o [Scarica il PDF](updating-and-deleting-existing-binary-data-cs/_static/datatutorial57cs1.pdf)

> Nelle esercitazioni precedenti è stato illustrato come il controllo GridView rende semplice per modificare ed eliminare dati di testo. In questa esercitazione viene illustrato come il controllo GridView consente inoltre di modificare ed eliminare dati binari, se tali dati binari viene salvati nel database o archiviati nel file system.


## <a name="introduction"></a>Introduzione

Tramite le ultime tre esercitazioni si aggiunta gran parte delle funzionalità per l'utilizzo di dati binari. È stato avviato mediante l'aggiunta di un `BrochurePath` colonna per il `Categories` tabella e aggiornati di conseguenza l'architettura. Abbiamo aggiunto anche il livello di accesso ai dati e il livello di logica di Business metodi per lavorare con la tabella s categorie esistente `Picture` colonna che contiene gli oggetti contenuto binario di un file di immagine. Abbiamo costruito pagine web per presentare i dati binari in un controllo GridView un collegamento di download per brochure, con l'immagine di categoria s mostrata un `<img>` elemento e aver aggiunto un controllo DetailsView per consentire agli utenti di aggiungere una nuova categoria e caricare i dati di brochure e immagine.

L'implementazione è comunque la possibilità di modificare ed eliminare le categorie esistenti, si saranno eseguire in questa esercitazione utilizzando il GridView s incorporato modifica ed eliminazione di funzionalità. Quando si modifica una categoria, l'utente sarà in grado di caricare una nuova immagine o facoltativamente dispongono della categoria di continuare a utilizzare quella esistente. Per brochure, è possibile scegliere di utilizzare brochure esistente, per caricare una nuova brochure o per indicare che la categoria non dispone più di una brochure associata. Let s iniziare!

## <a name="step-1-updating-the-data-access-layer"></a>Passaggio 1: Aggiornare il livello di accesso ai dati

DAL ha generato automaticamente `Insert`, `Update`, e `Delete` metodi, ma questi metodi sono stati generati in base il `CategoriesTableAdapter` query principale s, che non include il `Picture` colonna. Pertanto, il `Insert` e `Update` metodi non includano parametri per specificare i dati binari per l'immagine di categoria s. Come abbiamo fatto il [esercitazione precedente](including-a-file-upload-option-when-adding-a-new-record-cs.md), è necessario creare un nuovo metodo TableAdapter per l'aggiornamento di `Categories` tabella quando si specificano i dati binari.

Aprire il DataSet tipizzato e, dalla finestra di progettazione, fare clic su di `CategoriesTableAdapter` intestazione s e scegliere Aggiungi Query dal menu di scelta rapida di launche la configurazione guidata Query TableAdapter. Questa procedura guidata viene avviata da richiede la modalità della query TableAdapter deve accedere al database. Scegliere Usa istruzioni SQL e fare clic su Avanti. Il passaggio successivo viene richiesto per il tipo di query da generare. Poiché è nuovamente la creazione di una query per aggiungere un nuovo record per il `Categories` tabella, scegliere Aggiorna e fare clic su Avanti.


[![Selezionare l'opzione di aggiornamento](updating-and-deleting-existing-binary-data-cs/_static/image1.gif)](updating-and-deleting-existing-binary-data-cs/_static/image1.png)

**Figura 1**: selezionare l'opzione di aggiornamento ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image2.png))


È ora necessario specificare il `UPDATE` istruzione SQL. La procedura guidata suggerisce automaticamente un `UPDATE` istruzione corrispondente alla query principale s TableAdapter (uno che aggiorna il `CategoryName`, `Description`, e `BrochurePath` valori). Modificare l'istruzione in modo che il `Picture` colonna è inclusa insieme a un `@Picture` parametro, come illustrato di seguito:


[!code-sql[Main](updating-and-deleting-existing-binary-data-cs/samples/sample1.sql)]

Nella schermata finale della procedura guidata viene chiesto di specificare un nome al nuovo metodo di TableAdapter. Immettere `UpdateWithPicture` e fare clic su Fine.


[![Denominare il nuovo UpdateWithPicture TableAdapter (metodo)](updating-and-deleting-existing-binary-data-cs/_static/image2.gif)](updating-and-deleting-existing-binary-data-cs/_static/image3.png)

**Figura 2**: nome del nuovo metodo TableAdapter `UpdateWithPicture` ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image4.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Passaggio 2: Aggiungere i metodi di livello di logica di Business

Oltre all'aggiornamento DAL, è necessario aggiornare il livello Business LOGIC per includere metodi per l'aggiornamento ed eliminazione di una categoria. Questi sono i metodi che verranno richiamati dal livello di presentazione.

Per eliminare una categoria, è possibile utilizzare il `CategoriesTableAdapter` s generato automaticamente `Delete` metodo. Aggiungere il metodo seguente per la `CategoriesBLL` classe:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample2.cs)]

Per questa esercitazione, s consentono di creare due metodi per l'aggiornamento di una categoria, che prevede che i dati immagine binari e richiama il `UpdateWithPicture` metodo è stato appena aggiunto per il `CategoriesTableAdapter` e un altro che accetta solo il `CategoryName`, `Description`e `BrochurePath`valori e Usa `CategoriesTableAdapter` classe generata automaticamente s `Update` istruzione. La logica alla base utilizzando due metodi è che in alcuni casi, un utente potrebbe voler aggiornare l'immagine di s categoria insieme ai relativi altri campi, in cui caso l'utente sarà necessario caricare la nuova immagine. I dati binari immagine caricata s quindi utilizzabili nel `UPDATE` istruzione. In altri casi, l'utente potrebbe essere interessata solo l'aggiornamento, ad esempio, il nome e descrizione. Tuttavia, se il `UPDATE` istruzione richiede che i dati binari per il `Picture` colonna e quindi si d è necessario fornire anche le informazioni. Ciò richiede un ulteriore percorso per il database per ripristinare i dati dell'immagine per il record in fase di modifica. Di conseguenza, è necessario due `UPDATE` metodi. Determinare il livello di logica di Business in base quello da utilizzare se i dati immagine viene forniti quando si aggiorna la categoria.

A tale scopo, aggiungere due metodi per il `CategoriesBLL` classe sia denominato `UpdateCategory`. Il primo deve accettare tre `string` s, un `byte` , matrice e un `int` come input parametri; il secondo, solo tre `string` s e un `int`. Il `string` sono parametri di input per il nome della categoria s, la descrizione e il percorso del file brochure il `byte` matrice è per il contenuto binario categoria s, dell'immagine e `int` identifica il `CategoryID` del record da aggiornare. Si noti che il primo overload richiama il secondo se passato `byte` matrice è `null`:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample3.cs)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Passaggio 3: Copia l'inserimento e la funzionalità di visualizzazione

Nel [esercitazione precedente](including-a-file-upload-option-when-adding-a-new-record-cs.md) creato una pagina denominata `UploadInDetailsView.aspx` che elencate tutte le categorie in un controllo GridView e fornito un controllo DetailsView per aggiungere nuove categorie al sistema. In questa esercitazione verrà esteso per includere il supporto di eliminazione e modifica di GridView. Invece di continuare a lavorare da `UploadInDetailsView.aspx`, s consentono di posizionare invece modifiche questa esercitazione s il `UpdatingAndDeleting.aspx` pagina dalla stessa cartella, `~/BinaryData`. Copiare e incollare il markup dichiarativo e codice da `UploadInDetailsView.aspx` a `UpdatingAndDeleting.aspx`.

Aprire il `UploadInDetailsView.aspx` pagina. Copiare tutta la sintassi dichiarativa all'interno di `<asp:Content>` elemento, come illustrato nella figura 3. Aprire quindi `UpdatingAndDeleting.aspx` e incollare questo markup all'interno di relativo `<asp:Content>` elemento. Analogamente, copiare il codice dal `UploadInDetailsView.aspx` pagina classe code-behind s per `UpdatingAndDeleting.aspx`.


[![Copiare il Markup dichiarativo UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-cs/_static/image3.gif)](updating-and-deleting-existing-binary-data-cs/_static/image5.png)

**Figura 3**: copiare il Markup dichiarativo di `UploadInDetailsView.aspx` ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image6.png))


Dopo aver copiato il markup dichiarativo e codice, visitare `UpdatingAndDeleting.aspx`. Si noterà lo stesso output e avere la stessa esperienza utente come con `UploadInDetailsView.aspx` pagina dall'esercitazione precedente.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Passaggio 4: Aggiunta di eliminazione di supporto alla ObjectDataSource e GridView

Come descritto nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) esercitazione GridView fornisce funzionalità incorporate di eliminazione e queste funzionalità possono essere abilitate al segno di graduazione di una casella di controllo se la griglia s sottostante origine dati supporta l'eliminazione. Attualmente ObjectDataSource GridView è associato a (`CategoriesDataSource`) non supporta l'eliminazione.

Per risolvere questo problema, fare clic sull'opzione Configura origine dati da smart tag s ObjectDataSource per avviare la procedura guidata. La prima schermata mostra che ObjectDataSource sia configurato per funzionare con la `CategoriesBLL` classe. Scegliere "Avanti". Attualmente, solo il ObjectDataSource s `InsertMethod` e `SelectMethod` sono specificate proprietà. Tuttavia, la procedura guidata popolati automaticamente gli elenchi a discesa nelle schede UPDATE e DELETE con la `UpdateCategory` e `DeleteCategory` metodi, rispettivamente. Infatti nel `CategoriesBLL` classe è contrassegnata come questi metodi usando il `DataObjectMethodAttribute` come i metodi predefiniti per l'aggiornamento e l'eliminazione.

Per il momento, impostare l'elenco di riepilogo a discesa aggiornamento scheda s su (nessuno), ma lasciare l'elenco a discesa scheda s DELETE impostato su `DeleteCategory`. È possibile tornare alla procedura guidata nel passaggio 6 per aggiungere il supporto di aggiornamento.


[![Configurare ObjectDataSource per utilizzare il metodo DeleteCategory](updating-and-deleting-existing-binary-data-cs/_static/image4.gif)](updating-and-deleting-existing-binary-data-cs/_static/image7.png)

**Figura 4**: configurare ObjectDataSource per utilizzare il `DeleteCategory` metodo ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image8.png))


> [!NOTE]
> Dopo avere completato la procedura guidata, Visual Studio potrebbe chiedere se si desidera aggiornare i campi e le chiavi, che verrà rigenerata dati Web controlla i campi. Scegliere No, perché se si sceglie Sì, eventuali personalizzazioni campo apportate verranno sovrascritte.


ObjectDataSource ora include un valore per il relativo `DeleteMethod` proprietà, nonché un `DeleteParameter`. Tenere presente che quando si utilizza la procedura guidata per specificare i metodi, Visual Studio imposta s ObjectDataSource `OldValuesParameterFormatString` proprietà `original_{0}`, che può causare problemi con l'aggiornamento ed eliminazione di chiamate al metodo. Pertanto, cancellare completamente questa proprietà o ripristinare il valore predefinito, `{0}`. Se è necessario aggiornare la memoria per questa proprietà ObjectDataSource, vedere il [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) esercitazione.

Dopo aver completato la procedura guidata e la correzione di `OldValuesParameterFormatString`, markup dichiarativo ObjectDataSource s dovrebbe essere simile simile al seguente:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample4.aspx)]

Dopo aver configurato il ObjectDataSource, è possibile aggiungere funzionalità di eliminazione a GridView selezionando la casella di controllo Abilita eliminazione dallo smart tag s di GridView. Verrà aggiunta una CommandField a GridView cui `ShowDeleteButton` è impostata su `true`.


[![Abilitare il supporto per l'eliminazione di GridView](updating-and-deleting-existing-binary-data-cs/_static/image5.gif)](updating-and-deleting-existing-binary-data-cs/_static/image9.png)

**Figura 5**: abilitare il supporto per l'eliminazione in GridView ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image10.png))


È opportuno testare la funzionalità di eliminazione. È presente una chiave esterna tra le `Products` tabella s `CategoryID` e `Categories` tabella s `CategoryID`, pertanto si verificherà un'eccezione di violazione del vincolo di chiave esterna se si tenta di eliminare una delle prime otto categorie. Per testare questa funzionalità out, aggiungere una nuova categoria, fornendo un brochure e l'immagine. La categoria di test, illustrata nella figura 6, include un file di test brochure denominato `Test.pdf` e un'immagine di prova. Figura 7 illustra GridView dopo aver aggiunto la categoria di test.


[![Aggiungere una categoria di Test con un'immagine e Brochure](updating-and-deleting-existing-binary-data-cs/_static/image6.gif)](updating-and-deleting-existing-binary-data-cs/_static/image11.png)

**Figura 6**: aggiungere una categoria di Test con un'immagine e Brochure ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image12.png))


[![Dopo aver inserito la categoria di Test, viene visualizzato nel controllo GridView.](updating-and-deleting-existing-binary-data-cs/_static/image7.gif)](updating-and-deleting-existing-binary-data-cs/_static/image13.png)

**Figura 7**: dopo aver inserito la categoria di Test, viene visualizzato in GridView ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image14.png))


In Visual Studio, aggiornamento di Esplora soluzioni. Viene visualizzato un nuovo file di `~/Brochures` cartella `Test.pdf` (vedere la figura 8).

Successivamente, fare clic sul collegamento Elimina nella riga della categoria di Test, causando il postback della pagina e `CategoriesBLL` classe s `DeleteCategory` metodo. Verranno quindi richiamati i dispositivi DAL `Delete` metodo causando appropriata `DELETE` istruzione da inviare al database. I dati vengano quindi riassociati a GridView e il markup viene inviato al client con la categoria di Test non è più presente.

Mentre il flusso di lavoro di eliminazione è stato rimosso il record di categoria di Test dal `Categories` tabella, non sono state rimosse relativo file brochure dal file system s server web. Aggiornamento di Esplora soluzioni e si noterà che `Test.pdf` restano `~/Brochures` cartella.


![Il File Test.pdf non è stato eliminato dal File System s Server Web](updating-and-deleting-existing-binary-data-cs/_static/image8.gif)

**Figura 8**: il `Test.pdf` File non è stato eliminato dal File System s Server Web


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Passaggio 5: Rimuovere il File di Brochure s categoria eliminata

Uno degli svantaggi di archiviare i dati binari esterni al database è che è necessario eseguire passaggi aggiuntivi per pulire i file quando viene eliminato il record di database associato. GridView e ObjectDataSource forniscono gli eventi che vengono generati prima e dopo aver eseguito il comando di eliminazione. In realtà, è necessario creare gestori eventi per entrambi gli eventi di pre e post-azione. Prima di `Categories` viene eliminato è necessario determinare il percorso del file s PDF, ma non abbiamo t desidera eliminare il file PDF, prima che la categoria viene eliminata nel caso in cui alcune eccezione e la categoria non viene eliminata.

S GridView [ `RowDeleting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) generato durante la prima è stato richiamato il comando di eliminazione ObjectDataSource s, il relativo [ `RowDeleted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) viene generato dopo. Creare gestori per questi due eventi utilizzando il codice seguente:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample5.cs)]

Nel `RowDeleting` gestore dell'evento, il `CategoryID` della riga da eliminare è afferrato da s GridView `DataKeys` raccolta, è possibile accedere in questo gestore eventi tramite il `e.Keys` insieme. Successivamente, il `CategoriesBLL` classe s `GetCategoryByCategoryID(categoryID)` viene richiamato per restituire le informazioni del record da eliminare. Se l'oggetto restituito `CategoriesDataRow` oggetto ha non`NULL``BrochurePath` valore viene archiviato nella variabile di pagina `deletedCategorysPdfPath` in modo che il file può essere eliminato nel `RowDeleted` gestore dell'evento.

> [!NOTE]
> Anziché il recupero il `BrochurePath` i dettagli per il `Categories` record in corso l'eliminazione il `RowDeleting` gestore eventi, è possibile in alternativa aggiunti il `BrochurePath` al s GridView `DataKeyNames` proprietà e il valore di record s accessibile tramite il `e.Keys` insieme. In questo modo sarebbe leggermente aumentare le dimensioni dello stato visualizzazione s GridView, ma potrebbe ridurre la quantità di codice necessario e salvare un andata e ritorno al database.


Dopo il comando delete sottostante s è stato richiamato, ObjectDataSource s GridView `RowDeleted` gestore di evento viene generato. Se si sono verificate eccezioni in l'eliminazione dei dati ed è presente un valore per `deletedCategorysPdfPath`, quindi il file PDF viene eliminato dal file system. Si noti che questo codice aggiuntivo non è necessario pulire i dati binari s categoria associati con la relativa immagine. Tale s perché i dati di immagine vengono archiviati direttamente nel database, eliminando così il `Categories` riga Elimina anche i dati di immagine categoria s.

Dopo aver aggiunto i due gestori, eseguire nuovamente il test case. Quando si elimina la categoria, viene eliminato anche il file PDF associato.

L'aggiornamento di dati binari esistenti s record associato fornisce alcune problematiche interessanti. Aggiunta di funzionalità di aggiornamento le brochure affronta il resto di questa esercitazione. Passaggio 6 analizza le tecniche per l'aggiornamento delle informazioni di brochure durante il passaggio 7 esamina l'aggiornamento dell'immagine.

## <a name="step-6-updating-a-category-s-brochure"></a>Passaggio 6: Aggiornamento Brochure s categoria

Come descritto nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) esercitazione GridView offre supporto modificando a livello di riga predefinito può essere implementato da segni di graduazione di una casella di controllo, se l'origine dati sottostante configurato in modo appropriato. Attualmente, il `CategoriesDataSource` ObjectDataSource non è ancora configurato per includere l'aggiornamento di supporto, che in questo modo consentono s in.

Fare clic sul collegamento Configura origine dati dalla procedura guidata s ObjectDataSource e continuare con il secondo passaggio. Perché il `DataObjectMethodAttribute` utilizzato `CategoriesBLL`, l'elenco di riepilogo a discesa di aggiornamento deve essere popolato automaticamente con il `UpdateCategory` overload che accetta quattro parametri di input (per tutte le colonne, ma `Picture`). Modificarlo in modo che utilizzi l'overload con cinque parametri.


[![Configurare ObjectDataSource per utilizzare il metodo UpdateCategory che include un parametro di immagine](updating-and-deleting-existing-binary-data-cs/_static/image9.gif)](updating-and-deleting-existing-binary-data-cs/_static/image15.png)

**Figura 9**: configurare ObjectDataSource per utilizzare il `UpdateCategory` metodo che include un parametro per `Picture` ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image16.png))


ObjectDataSource ora include un valore per il relativo `UpdateMethod` proprietà nonché corrispondente `UpdateParameter` s. Come indicato nel passaggio 4, Visual Studio imposta s ObjectDataSource `OldValuesParameterFormatString` proprietà `original_{0}` quando si utilizza la configurazione guidata origine dati. Questo causare problemi con l'aggiornamento e di eliminare chiamate al metodo. Pertanto, cancellare completamente questa proprietà o ripristinare il valore predefinito, `{0}`.

Dopo aver completato la procedura guidata e la correzione di `OldValuesParameterFormatString`, markup dichiarativo ObjectDataSource s dovrebbe essere simile al seguente:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample6.aspx)]

Per attivare le funzionalità di modifica predefinite di GridView s, selezionare l'opzione Abilita modifica dallo smart tag s di GridView. Questo verrà impostato il s CommandField `ShowEditButton` proprietà `true`, con conseguente l'aggiunta di un pulsante Modifica (e i pulsanti di aggiornamento e annullamento per la riga in fase di modifica).


[![Configurare il controllo GridView per supportano la modifica](updating-and-deleting-existing-binary-data-cs/_static/image10.gif)](updating-and-deleting-existing-binary-data-cs/_static/image17.png)

**Figura 10**: configurare il controllo GridView per supportano la modifica ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image18.png))


Visitare la pagina tramite un browser e fare clic su uno dei pulsanti Modifica s della riga. Il `CategoryName` e `Description` BoundField vengono visualizzati come caselle di testo. Il `BrochurePath` TemplateField manca un `EditItemTemplate`, pertanto continua a mostrare il `ItemTemplate` un collegamento a brochure. Il `Picture` ImageField esegue il rendering come una casella di testo la cui proprietà `Text` proprietà viene assegnato il valore di s ImageField `DataImageUrlField` valore, in questo caso `CategoryID`.


[![GridView non dispone di un'interfaccia di modifica per BrochurePath](updating-and-deleting-existing-binary-data-cs/_static/image11.gif)](updating-and-deleting-existing-binary-data-cs/_static/image19.png)

**Figura 11**: GridView non dispone di un'interfaccia di modifica per `BrochurePath` ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image20.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Personalizzazione di`BrochurePath`s interfaccia di modifica

È necessario creare un'interfaccia di modifica per il `BrochurePath` TemplateField, uno che consente all'utente a uno:

- Lasciare la brochure categoria s come-è,
- Aggiornare la brochure categoria s caricando una nuova brochure, o
- Rimuovere completamente la brochure categoria s (nel caso che la categoria non dispone più di un brochure associato).

È anche necessario aggiornare il `Picture` ImageField interfaccia di modifica s, ma si otterranno a questo nel passaggio 7.

Il GridView s smart tag, fare clic sul collegamento Modifica modelli e selezionare il `BrochurePath` TemplateField s `EditItemTemplate` dall'elenco a discesa. Aggiungere un controllo RadioButtonList Web a questo modello, impostando il relativo `ID` proprietà `BrochureOptions` e il relativo `AutoPostBack` proprietà `true`. Dalla finestra delle proprietà, fare clic sui puntini di sospensione nel `Items` proprietà, verrà visualizzata la `ListItem` Editor della raccolta. Aggiungere le tre opzioni seguenti con `Value` s 1, 2 e 3, rispettivamente:

- Utilizzare brochure corrente
- Rimuovere brochure corrente
- Caricare nuovo brochure

Impostare il primo `ListItem` s `Selected` proprietà `true`.


![Aggiungere tre elementi ListItem RadioButtonList](updating-and-deleting-existing-binary-data-cs/_static/image12.gif)

**Figura 12**: aggiungere tre `ListItem` s per RadioButtonList


Sotto RadioButtonList, aggiungere un controllo FileUpload denominato `BrochureUpload`. Impostare il relativo `Visible` proprietà `false`.


[![Aggiungere un controllo FileUpload RadioButtonList EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image13.gif)](updating-and-deleting-existing-binary-data-cs/_static/image21.png)

**Figura 13**: aggiungere un RadioButtonList e controllo FileUpload il `EditItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image22.png))


Questo RadioButtonList fornisce tre opzioni per l'utente. L'idea è che il controllo FileUpload verrà visualizzato solo se l'ultima opzione, brochure nuovo caricamento, è selezionata. A tale scopo, creare un gestore eventi per s RadioButtonList `SelectedIndexChanged` eventi e aggiungere il codice seguente:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample7.cs)]

Poiché i controlli RadioButtonList e FileUpload sono all'interno di un modello, è necessario scrivere codice per accedere a questi controlli a livello di codice. Il `SelectedIndexChanged` gestore dell'evento viene passato un riferimento di RadioButtonList nel `sender` parametro di input. Per ottenere il controllo FileUpload, è necessario ottenere l'oggetto padre s RadioButtonList controllo e usare il `FindControl("controlID")` metodo da qui. Dopo aver ottenuto un riferimento a controlli RadioButtonList sia FileUpload, con il caricamento dei file di controllo s `Visible` è impostata su `true` solo se s RadioButtonList `SelectedValue` è uguale a 3, ovvero il `Value` per brochure nuovo caricamento `ListItem`.

Con questo codice, è opportuno testare l'interfaccia di modifica. Fare clic sul pulsante Modifica per una riga. Inizialmente, deve selezionare l'opzione brochure corrente di utilizzo. La modifica dell'indice selezionato provoca un postback. Se la terza opzione è selezionata, viene visualizzato il controllo FileUpload, in caso contrario è nascosta. La figura 14 mostra l'interfaccia di modifica quando innanzitutto clic sul pulsante Modifica; Figura 15 mostra l'interfaccia dopo aver selezionata l'opzione brochure nuovo caricamento.


[![Inizialmente, utilizzare corrente brochure che opzione è selezionata](updating-and-deleting-existing-binary-data-cs/_static/image14.gif)](updating-and-deleting-existing-binary-data-cs/_static/image23.png)

**Nella figura 14**: inizialmente, utilizzare corrente brochure opzione è selezionata ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image24.png))


[![Scelta di caricamento brochure nuova opzione consente di visualizzare il controllo FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image15.gif)](updating-and-deleting-existing-binary-data-cs/_static/image25.png)

**Figura 15**: scelta caricamento brochure nuova opzione consente di visualizzare il controllo FileUpload ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image26.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Salvataggio Brochure File e l'aggiornamento di`BrochurePath`colonna

Quando si fa clic sul pulsante di aggiornamento s GridView, il relativo `RowUpdating` viene generato l'evento. ObjectDataSource viene richiamato il comando di aggiornamento s e quindi s GridView `RowUpdated` viene generato l'evento. Ad esempio con l'eliminazione del flusso di lavoro, è necessario creare gestori eventi per entrambi gli eventi. Nel `RowUpdating` gestore eventi, è necessario determinare l'azione da eseguire in base il `SelectedValue` del `BrochureOptions` RadioButtonList:

- Se il `SelectedValue` è 1, si vuole continuare a usare lo stesso `BrochurePath` impostazione. Pertanto, è necessario impostare il s ObjectDataSource `brochurePath` parametro esistente `BrochurePath` valore di record da aggiornare. S ObjectDataSource `brochurePath` parametro può essere impostato utilizzando `e.NewValues["brochurePath"] = value`.
- Se il `SelectedValue` è 2, allora sarà necessario impostare il record s `BrochurePath` valore `NULL`. Questo può essere soddisfatta impostando s ObjectDataSource `brochurePath` parametro `Nothing`, dando luogo a un database `NULL` utilizzata nel `UPDATE` istruzione. Se è un file brochure esistente che si desidera rimuovere, è necessario eliminare il file esistente. Tuttavia, si vuole inserire solo eseguire questa operazione se al termine dell'aggiornamento senza generare un'eccezione.
- Se il `SelectedValue` è 3, è necessario assicurarsi che l'utente ha caricato un file PDF e quindi salvare il file nel file System e aggiornare il record s `BrochurePath` valore della colonna. Inoltre, se è presente un file brochure esistente che viene sostituito, è necessario eliminare il file precedente. Tuttavia, si vuole inserire solo eseguire questa operazione se al termine dell'aggiornamento senza generare un'eccezione.

I passaggi necessari per completare la quando s RadioButtonList `SelectedValue` è 3 sono praticamente identiche a quelle utilizzate dal controllo DetailsView s `ItemInserting` gestore dell'evento. Questo gestore eventi viene eseguito quando viene aggiunto un nuovo record di categoria dal controllo DetailsView in sono aggiunte le [esercitazione precedente](including-a-file-upload-option-when-adding-a-new-record-cs.md). Pertanto, behooves per effettuare il refactoring di questa funzionalità out in metodi separati. In particolare, spostato la funzionalità comuni in due metodi:

- `ProcessBrochureUpload(FileUpload, out bool)`accetta come input dell'istanza di un controllo FileUpload e un valore booleano di output che specifica se l'operazione di eliminazione o modifica è deve continuare o se deve essere annullata a causa di errori di convalida. Questo metodo restituisce il percorso del file salvato o `null` se è stato salvato alcun file.
- `DeleteRememberedBrochurePath`Elimina il file specificato dal percorso nella variabile di pagina `deletedCategorysPdfPath` se `deletedCategorysPdfPath` non `null`.

Il codice per questi due metodi segue. Si noti la somiglianza tra `ProcessBrochureUpload` e s DetailsView `ItemInserting` gestore dell'evento dall'esercitazione precedente. In questa esercitazione hai aggiornato i gestori di eventi di controllo DetailsView s per l'utilizzo di questi nuovi metodi. Scaricare il codice associato a questa esercitazione per visualizzare le modifiche ai gestori di eventi s DetailsView.


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample8.cs)]

S GridView `RowUpdating` e `RowUpdated` utilizzano gestori di eventi di `ProcessBrochureUpload` e `DeleteRememberedBrochurePath` metodi, come illustrato nel codice seguente:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample9.cs)]

Nota come il `RowUpdating` gestore viene utilizzata una serie di istruzioni condizionali per eseguire l'azione appropriata in base il `BrochureOptions` RadioButtonList s `SelectedValue` valore della proprietà.

Con questo codice, è possibile modificare una categoria e si utilizza catalogo corrente, non utilizzare alcun brochure oppure caricarne uno nuovo. Vado avanti e prova a usarla. Impostare punti di interruzione di `RowUpdating` e `RowUpdated` gestori eventi per farsi un'idea del flusso di lavoro.

## <a name="step-7-uploading-a-new-picture"></a>Passaggio 7: Caricamento di una nuova immagine

Il `Picture` s ImageField modifica esegue il rendering dell'interfaccia come popolato con il valore di una casella di testo relativa `DataImageUrlField` proprietà. Durante la modifica del flusso di lavoro, GridView passa un parametro a ObjectDataSource con il nome del parametro s il valore di s ImageField `DataImageUrlField` proprietà e il parametro s valore il valore immesso nella casella di testo nell'interfaccia di modifica. Questo comportamento è appropriato quando l'immagine viene salvata come file nel file system e `DataImageUrlField` contiene l'URL completo dell'immagine. Con tali circostanze, l'interfaccia di modifica consente di visualizzare l'URL dell'immagine s nella casella di testo, quale l'utente può modificare e sono salvati nel database. Concesse, questa t di interfaccia predefinito consentono all'utente di caricare una nuova immagine, ma permettono di modificare l'URL dell'immagine dal valore corrente a un altro. Per questa esercitazione, tuttavia, il valore predefinito s ImageField modifica interfaccia non è sufficiente perché il `Picture` dati binari vengono archiviati direttamente nel database e `DataImageUrlField` proprietà contiene solo il `CategoryID`.

Per comprendere meglio cosa accade in questa esercitazione, quando un utente modifica una riga con un ImageField, si consideri l'esempio seguente: un utente modifica una riga con `CategoryID` 10, causare il `Picture` ImageField per eseguire il rendering come una casella di testo con il valore 10. Si supponga che l'utente modifica il valore in questa casella di testo su 50 e fa clic sul pulsante di aggiornamento. Si verifica un postback e GridView crea inizialmente un parametro denominato `CategoryID` con il valore 50. Tuttavia, prima di GridView invia questo parametro (e `CategoryName` e `Description` parametri), aggiunge i valori dal `DataKeys` insieme. Pertanto, sovrascrive il `CategoryID` parametro all'oggetto corrente s riga sottostante `CategoryID` valore 10. In breve, s ImageField modifica interfaccia non ha alcun effetto sul Modifica flusso di lavoro per questa esercitazione perché i nomi di istanze della classe ImageField `DataImageUrlField` proprietà e la griglia s `DataKey` valore sono equivalenti.

Mentre il ImageField rende più semplice visualizzare un'immagine in base ai dati di database, non abbiamo t desidera fornire una casella di testo nell'interfaccia di modifica. Invece si desidera offrire un controllo FileUpload che l'utente finale è possibile utilizzare per modificare l'immagine di s categoria. A differenza di `BrochurePath` valore, per queste esercitazioni si ve deciso in modo da richiedere che ciascuna categoria deve avere un'immagine. Pertanto, non abbiamo non è necessario consentire all'utente di indicare che è presente alcuna immagine associata l'utente non può caricare una nuova immagine o lasciare l'immagine corrente come-è.

Per personalizzare l'interfaccia di modifica ImageField s, è necessario convertirlo in un TemplateField. Da GridView s smart tag, fare clic sul collegamento Modifica colonne, selezionare il ImageField e fare clic su Converti il campo in un TemplateField di collegamento.


![Convertire il ImageField in un TemplateField](updating-and-deleting-existing-binary-data-cs/_static/image16.gif)

**Figura 16**: convertire il ImageField in un TemplateField


Conversione di ImageField in un TemplateField in questo modo genera un TemplateField due modelli. Come mostra la sintassi dichiarativa seguente, il `ItemTemplate` contiene un'immagine di Web controllo cui `ImageUrl` proprietà viene assegnata tramite la sintassi di associazione dati in base a s ImageField `DataImageUrlField` e `DataImageUrlFormatString` proprietà. Il `EditItemTemplate` contiene una casella di testo la cui `Text` proprietà è associata al valore specificato per il `DataImageUrlField` proprietà.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample10.aspx)]

È necessario aggiornare il `EditItemTemplate` per utilizzare un controllo FileUpload. Dal controllo GridView smart tag di s fare clic su Modifica modelli di collegamento e quindi selezionare il `Picture` TemplateField s `EditItemTemplate` dall'elenco a discesa. Nel modello verrà visualizzata una casella di testo rimuoverlo. Successivamente, trascinare un controllo FileUpload dalla casella degli strumenti nel modello, l'impostazione relativa `ID` a `PictureUpload`. Inoltre, aggiungere il testo per modificare l'immagine di categoria s, specificare una nuova immagine. Per mantenere la stessa immagine categoria s, lasciare vuoto il campo per il modello.


[![Aggiungere un controllo FileUpload EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image17.gif)](updating-and-deleting-existing-binary-data-cs/_static/image27.png)

**Figura 17**: aggiungere un controllo FileUpload al `EditItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image28.png))


Dopo aver personalizzato l'interfaccia di modifica, visualizzare lo stato di avanzamento in un browser. Quando si visualizza una riga in modalità di sola lettura, l'immagine di s categoria viene visualizzata come prima, ma facendo clic sul pulsante Modifica viene eseguito il rendering della colonna immagine come testo con un controllo FileUpload.


[![L'interfaccia di modifica include un controllo FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image18.gif)](updating-and-deleting-existing-binary-data-cs/_static/image29.png)

**Figura 18**: l'interfaccia di modifica include un controllo FileUpload ([fare clic per visualizzare l'immagine ingrandita](updating-and-deleting-existing-binary-data-cs/_static/image30.png))


Tenere presente che ObjectDataSource sia configurato per chiamare il `CategoriesBLL` classe s `UpdateCategory` metodo che accetta come input i dati binari per l'immagine come un `byte` matrice. Se questa matrice ha un `null` valore, tuttavia, l'alternativa `UpdateCategory` overload viene chiamato, quali problemi di `UPDATE` istruzione SQL che non modifica il `Picture` intatta un'immagine di colonna, lasciando la categoria s corrente. Pertanto, in GridView s `RowUpdating` gestore eventi è necessario fare riferimento a livello di codice il `PictureUpload` FileUpload controllare e determinare se è stato caricato un file. Se uno non è stato caricato, quindi facciamo *non* per specificare un valore per il `picture` parametro. D'altra parte, se è stato caricato un file nel `PictureUpload` controllo FileUpload, si desidera assicurarsi che sia un file JPG. Se è, quindi inviare il contenuto binario a ObjectDataSource mediante il `picture` parametro.

Ad esempio con il codice usato nel passaggio 6, gran parte del codice necessario già esiste in s DetailsView `ItemInserting` gestore dell'evento. Pertanto, si ve sottoposta a refactoring le funzionalità comuni in un nuovo metodo `ValidPictureUpload`e aggiornato il `ItemInserting` gestore eventi per utilizzare questo metodo.

Aggiungere il codice seguente all'inizio di GridView s `RowUpdating` gestore dell'evento. È importante che il codice precedere il codice che consente di salvare il file brochure poiché non abbiamo t s desidera salvare brochure nel file System s server web, se viene caricato un file di immagine non valido.


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample11.cs)]

Il `ValidPictureUpload(FileUpload)` metodo accetta un controllo FileUpload come unico parametro input e controlla l'estensione di file caricato s per garantire che il file caricato in formato JPG, viene chiamato solo se viene caricato un file di immagine. Se è stato caricato alcun file, quindi il parametro di immagine è impostata e pertanto viene utilizzato il valore predefinito di `null`. Se è stata caricata un'immagine e `ValidPictureUpload` restituisce `true`, `picture` parametro assegnato i dati binari dell'immagine caricata; se il metodo restituisce `false`, il flusso di lavoro di aggiornamento viene annullato e terminato il gestore dell'evento.

Il `ValidPictureUpload(FileUpload)` codice del metodo, che è stato effettuato il refactoring da DetailsView s `ItemInserting` gestore eventi segue:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample12.cs)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Passaggio 8: Sostituendo le immagini di categorie originali con jpg

Tenere presente che le immagini di otto categorie originali sono file bitmap eseguito il wrapping in un'intestazione OLE. Ora che è stata aggiunta la possibilità di modificare un'immagine di record s esistente, è opportuno sostituire tali bitmap con jpg. Se si desidera continuare a utilizzare le immagini di categoria corrente, è possibile convertirli in jpg eseguendo i passaggi seguenti:

1. Salvare le immagini bitmap nell'unità disco rigido. Visitare il `UpdatingAndDeleting.aspx` pagina nel browser e per ognuna delle prime otto categorie, l'immagine del pulsante destro del mouse e scegliere di salvare l'immagine.
2. Aprire l'immagine nell'editor di immagini di scelta. Ad esempio, è possibile utilizzare Microsoft Paint.
3. Salvare la mappa di bit come un'immagine JPG.
4. Aggiornare l'immagine di categoria s tramite l'interfaccia di modifica, utilizzando il file JPG.

Dopo la modifica di una categoria e il caricamento dell'immagine JPG, l'immagine non eseguirà il rendering nel browser in quanto il `DisplayCategoryPicture.aspx` pagina è rimuovendo i primi 78 byte dalle immagini delle prime otto categorie. Risolvere il problema, rimuovere il codice che esegue la rimozione di intestazione OLE. Dopo questa operazione, il `DisplayCategoryPicture.aspx``Page_Load` gestore eventi deve avere solo il codice seguente:


[!code-vb[Main](updating-and-deleting-existing-binary-data-cs/samples/sample13.vb)]

> [!NOTE]
> Il `UpdatingAndDeleting.aspx` pagina inserimento e modifica di interfacce potrebbe usare leggermente più complesso. Il `CategoryName` e `Description` BoundField in GridView e DetailsView deve essere convertito in TemplateFields. Poiché `CategoryName` non `NULL` valori, RequiredFieldValidator devono essere aggiunti. E `Description` TextBox probabilmente deve essere convertito in una casella di testo su più righe. Per l'utente lasciare questi ultimi ritocchi un esercizio.


## <a name="summary"></a>Riepilogo

In questa esercitazione viene completata l'aspetto all'utilizzo di dati binari. In questa esercitazione e i tre precedenti, abbiamo visto come binari possono essere archiviati nel file system o direttamente all'interno del database. Un utente fornisce i dati binari al sistema selezionando un file dal disco rigido locale e caricarlo di nuovo al server web, in cui può essere archiviato nel file system o inserito nel database. ASP.NET 2.0 include un controllo FileUpload che rende tale interfaccia semplice come trascinamento della selezione. Tuttavia, come indicato nel [caricamento di file](uploading-files-cs.md) esercitazione, il controllo FileUpload è ideale per il caricamento dei file di dimensioni relativamente ridotte, in teoria non superamento megabyte. È inoltre esplorato come associare i dati caricati con il modello di dati sottostanti, nonché come modificare ed eliminare i dati binari da un record esistente.

Il set successivo di esercitazioni illustra diverse tecniche di memorizzazione nella cache. La memorizzazione nella cache consente di migliorare una s applicazione prestazioni complessive considerando i risultati di operazioni complesse e archiviarli in una posizione in cui è possibile accedere più rapidamente.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Il revisore per questa esercitazione è stata Teresa Murphy. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](including-a-file-upload-option-when-adding-a-new-record-cs.md)
[Successivo](uploading-files-vb.md)
