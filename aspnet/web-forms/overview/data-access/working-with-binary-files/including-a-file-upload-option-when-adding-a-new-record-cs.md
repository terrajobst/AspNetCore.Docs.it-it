---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: Incluso un File con opzione di caricamento quando si aggiunge un nuovo Record (c#) | Microsoft Docs
author: rick-anderson
description: Questa esercitazione illustra come creare un'interfaccia Web che consente all'utente sia immettere i dati di testo e caricare i file binari. Per illustrare le opzioni disponibili t...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: 5cc1db20a724c8a060e978e2360b977fb16f1e0c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842381"
---
<a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>Inclusione di un'opzione di caricamento di File quando si aggiunge un nuovo Record (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe) o [Scarica il PDF](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> Questa esercitazione illustra come creare un'interfaccia Web che consente all'utente sia immettere i dati di testo e caricare i file binari. Per illustrare le opzioni disponibili per archiviare i dati binari, un file verrà salvato nel database mentre l'altra viene archiviata nel file system.


## <a name="introduction"></a>Introduzione

Nelle due esercitazioni precedenti sono stati presentati le tecniche per l'archiviazione dei dati binari associati al modello di dati applicazione s, preso in esame come usare il controllo FileUpload per inviare file dal client al server web e visto come presentare tali dati binari in un W dei dati controllo EB. È fornire un supporto iniziale per informazioni su come associare i dati caricati con il modello di dati, tuttavia.

In questa esercitazione si creerà una pagina web per aggiungere una nuova categoria. Oltre alle caselle di testo per il nome della categoria s e una descrizione, questa pagina sarà necessario includere due FileUpload controlli per la nuova immagine di categoria s e una per brochure. L'immagine caricata verrà archiviato direttamente nel nuovo record s `Picture` colonna, mentre brochure verrà salvata il `~/Brochures` cartella con il percorso del file salvato nel nuovo record s `BrochurePath` colonna.

Prima di creare la nuova pagina web, è necessario aggiornare l'architettura. Il `CategoriesTableAdapter` query principale s non recupera il `Picture` colonna. Di conseguenza, l'autogenerato `Insert` metodo può avere solo gli input per il `CategoryName`, `Description`, e `BrochurePath` campi. Pertanto, è necessario creare un metodo aggiuntivo in oggetto TableAdapter che richiede a tutti e quattro `Categories` campi. Il `CategoriesBLL` classe nel livello della logica di Business dovrà anche essere aggiornata.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Passaggio 1: Aggiunta di un`InsertWithPicture`metodo per il`CategoriesTableAdapter`

Quando è stata creata la `CategoriesTableAdapter` nuovamente nel [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) esercitazione, è stato configurato per generare automaticamente `INSERT`, `UPDATE`, e `DELETE` istruzioni in base alla query principale. Inoltre, abbiamo indicato da adottare l'approccio DB diretto, che ha creato i metodi TableAdapter `Insert`, `Update`, e `Delete`. Questi metodi eseguire generata automaticamente `INSERT`, `UPDATE`, e `DELETE` istruzioni e, di conseguenza, accettare i parametri di input basati sulle colonne restituite dalla query principale. Nel [caricare file](uploading-files-cs.md) esercitazione abbiamo aumentati il `CategoriesTableAdapter` query principale s utilizzare il `BrochurePath` colonna.

Poiché il `CategoriesTableAdapter` s principali query non fa riferimento il `Picture` colonna, è possibile aggiungere un nuovo record né aggiornare un record esistente con un valore per il `Picture` colonna. Per acquisire queste informazioni, è possibile creare un nuovo metodo nella TableAdapter in cui viene utilizzato in particolare per inserire un record con i dati binari o possiamo personalizzare generata automaticamente `INSERT` istruzione. Il problema con la personalizzazione generata automaticamente `INSERT` istruzione è che si corre il rischio che i nostri personalizzazioni sovrascritte dalla procedura guidata. Ad esempio, si supponga che abbiamo personalizzato la `INSERT` includono l'utilizzo dell'istruzione il `Picture` colonna. In questo modo verrebbe aggiornato i TableAdapter `Insert` metodo per includere un parametro di input aggiuntivo per i dati binari immagine s categoria s. È quindi possibile creare un metodo nel livello di logica di Business per utilizzare questo metodo DAL e richiamare questo metodo BLL attraverso il livello di presentazione e tutto quello che altrimenti funzionerebbero incredibile. Vale a dire, fino al successivo è stato configurato il TableAdapter tramite la configurazione guidata TableAdapter. Non appena completata la procedura guidata, nostro personalizzazioni per il `INSERT` verrebbe sovrascritto istruzione, il `Insert` metodo viene ripristinata la forma precedente e il codice non verrebbe compilata!

> [!NOTE]
> Questo seccatura è un problema quando si utilizzano le stored procedure anziché le istruzioni SQL ad hoc. Un'esercitazione futura illustrerà l'uso di stored procedure al posto di istruzioni SQL ad hoc nel livello di accesso ai dati.


Per evitare questo rischio problema dover, anziché le istruzioni SQL generate automaticamente di personalizzazione consentono s invece creare un nuovo metodo per l'oggetto TableAdapter. Questo metodo, denominato `InsertWithPicture`, accetta i valori per il `CategoryName`, `Description`, `BrochurePath`, e `Picture` colonne ed eseguire un `INSERT` istruzione che archivia tutti i quattro valori in un nuovo record.

Aprire il DataSet tipizzato e, dalla finestra di progettazione, fare clic su di `CategoriesTableAdapter` intestazione s e scegliere Aggiungi Query dal menu di scelta rapida. Verrà avviata la configurazione guidata Query TableAdapter che inizia con richiede la modalità della query TableAdapter di accesso del database. Scegliere Usa istruzioni SQL e fare clic su Avanti. Il passaggio successivo richiede il tipo di query da generare. Poiché abbiamo nuovamente la creazione di una query per aggiungere un nuovo record per il `Categories` tabella scegliere INSERT e fare clic su Avanti.


[![Selezionare l'opzione di inserimento](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**Figura 1**: selezionare l'opzione di inserimento ([fare clic per visualizzare l'immagine con dimensioni normali](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))


È ora necessario specificare il `INSERT` istruzione SQL. La procedura guidata suggerisce automaticamente un `INSERT` istruzione corrisponde alla query principale s TableAdapter. In questo caso, è s un' `INSERT` istruzione che consente di inserire il `CategoryName`, `Description`, e `BrochurePath` valori. L'istruzione Update in modo che il `Picture` colonna è incluso con un `@Picture` parametro, come illustrato di seguito:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

Schermata finale della procedura guidata viene chiesto di specificare un nome al nuovo metodo di TableAdapter. Immettere `InsertWithPicture` e fare clic su Fine.


[![Denominare il nuovo InsertWithPicture TableAdapter (metodo)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**Figura 2**: nome del nuovo metodo TableAdapter `InsertWithPicture` ([fare clic per visualizzare l'immagine con dimensioni normali](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Passaggio 2: Aggiornare il livello di logica di Business

Poiché il livello di presentazione deve solo interfacciarsi con il livello di logica di Business anziché ignorando che consente di passare direttamente a livello di accesso ai dati, è necessario creare un metodo BLL che richiama il metodo DAL appena creato (`InsertWithPicture`). Per questa esercitazione, creare un metodo nella `CategoriesBLL` classe denominata `InsertWithPicture` che accetta come input tre `string` s e una `byte` matrice. Il `string` sono parametri di input per il nome della categoria s, descrizione e percorso del file brochure, mentre il `byte` array sia per il contenuto binario di immagine categoria s. Come illustrato nel codice seguente, questo metodo BLL richiama il metodo DAL corrispondente:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> Assicurarsi che sia stato salvato del DataSet tipizzato prima di aggiungere il `InsertWithPicture` metodo per il livello BLL. Poiché il `CategoriesTableAdapter` codice della classe viene generato automaticamente basata su set di dati tipizzati, se non si t prima di salvare le modifiche al set di dati tipizzato il `Adapter` proprietà ha vinto t a conoscenza di `InsertWithPicture` (metodo).


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Passaggio 3: Elenca le categorie esistenti e i relativi dati binari

In questa esercitazione si creerà una pagina che consente agli utenti finali di aggiungere una nuova categoria al sistema, fornendo un'immagine e brochure per la nuova categoria. Nel [esercitazione precedente](displaying-binary-data-in-the-data-web-controls-cs.md) abbiamo utilizzato un GridView con un TemplateField e ImageField per visualizzare il nome di ogni categoria s, descrizione, immagine e un collegamento per scaricare il brochure. Let s replicare tale funzionalità per questa esercitazione, la creazione di una pagina che elenca tutte le categorie esistenti e consente i nuovi processi da creare.

Iniziare aprendo il `DisplayOrDownload.aspx` dalla pagina di `BinaryData` cartella. Passare alla visualizzazione origine e copiare i GridView e ObjectDataSource s sintassi dichiarativa, averla poi incollata all'interno di `<asp:Content>` elemento `UploadInDetailsView.aspx`. Dimenticare di copiare anche tenere sempre il `GenerateBrochureLink` metodo della classe code-behind di `DisplayOrDownload.aspx` a `UploadInDetailsView.aspx`.


[![Copiare e incollare la sintassi dichiarativa dalla DisplayOrDownload.aspx a UploadInDetailsView.aspx](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**Figura 3**: copiare e incollare la sintassi dichiarativa dalla `DisplayOrDownload.aspx` al `UploadInDetailsView.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))


Dopo aver copiato la sintassi dichiarativa e `GenerateBrochureLink` metodo tramite la `UploadInDetailsView.aspx` pagina, visualizzare la pagina tramite un browser per assicurarsi che tutto ciò che è stato copiato correttamente. Verrà visualizzato un controllo GridView di otto categorie di elenco che include un collegamento per scaricare il brochure, nonché l'immagine di categoria s.


[![Si noterà ora ogni categoria insieme ai relativi dati binari](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**Figura 4**: si dovrebbe ora vedere ogni categoria insieme con i dati binari ([fare clic per visualizzare l'immagine con dimensioni normali](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Passaggio 4: Configurazione di`CategoriesDataSource`all'inserimento di supporto

Il `CategoriesDataSource` ObjectDataSource usato dal `Categories` GridView attualmente non fornisce la possibilità di inserire i dati. Per supportare l'inserimento tramite questo controllo origine dati, è necessario eseguire il mapping relativo `Insert` metodo a un metodo nell'oggetto sottostante, `CategoriesBLL`. In particolare, si desidera eseguire il mapping per il `CategoriesBLL` metodo aggiunto nel passaggio 2, `InsertWithPicture`.

Avviare facendo clic sul collegamento Configura origine dati nello smart tag s ObjectDataSource. La prima schermata mostra l'oggetto origine dati è configurato per funzionare con, `CategoriesBLL`. Lasciare questa impostazione come- e fare clic su Avanti per passare alla schermata di definire i metodi di dati. Passare alla scheda Inserisci e scegli la `InsertWithPicture` metodo dall'elenco a discesa. Fare clic su Fine per completare la procedura guidata.


[![Configurare ObjectDataSource per usare il metodo InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**Figura 5**: configurare ObjectDataSource per usare il `InsertWithPicture` metodo ([fare clic per visualizzare l'immagine con dimensioni normali](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))


> [!NOTE]
> Dopo aver completato la procedura guidata, Visual Studio potrebbe chiedere se si desidera aggiornare i campi e le chiavi, che rigenera i dati Web controlla i campi. Scegliere No, perché se si sceglie Sì sovrascriverà le personalizzazioni qualsiasi campo per avviare l'applicazione.


Dopo aver completato la procedura guidata, ObjectDataSource includerà ora un valore per la relativa `InsertMethod` proprietà nonché `InsertParameters` per le colonne di quattro categoria, come il seguente markup dichiarativo illustra:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Passaggio 5: Creazione dell'interfaccia di inserimento

Come primo trattati nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), il controllo DetailsView fornisce un'interfaccia di inserimento predefinita che può essere utili quando si lavora con un controllo origine dati che supporta l'inserimento. Consentire s aggiungere un controllo DetailsView a questa pagina sopra il controllo GridView che forniranno in modo permanente la relativa interfaccia inserimento, consentendo all'utente di aggiungere rapidamente una nuova categoria. Quando viene aggiunta una nuova categoria in DetailsView, GridView sottostanti verranno automaticamente aggiornare e visualizzare la nuova categoria.

Avvio mediante il trascinamento di un controllo DetailsView dalla casella degli strumenti nella finestra di progettazione sopra il controllo GridView, l'impostazione relativa `ID` proprietà `NewCategory` e cancellare il `Height` e `Width` i valori delle proprietà. DetailsView s nello smart tag, eseguirne l'associazione esistente `CategoriesDataSource` e quindi selezionare la casella di controllo Attiva paging.


[![Associare il CategoriesDataSource DetailsView e Abilita inserimento](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**Figura 6**: associare il controllo DetailsView per la `CategoriesDataSource` e Abilita inserimento ([fare clic per visualizzare l'immagine con dimensioni normali](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))


Per il rendering in modo permanente DetailsView nella relativa interfaccia inserimento, impostare relativi `DefaultMode` proprietà `Insert`.

Si noti che il controllo DetailsView ha cinque BoundField `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, e `BrochurePath` anche se il `CategoryID` BoundField non viene eseguito nell'interfaccia di inserimento perché relativo `InsertVisible` proprietà è impostata su `false`. Questi BoundField esiste perché sono le colonne restituite dal `GetCategories()` metodo, che è ciò che ObjectDataSource richiama per recuperare i dati. Per l'inserimento, tuttavia, non abbiamo t vuole consentire all'utente di specificare un valore per `NumberOfProducts`. Inoltre, è necessario consentire loro di caricare un'immagine per la nuova categoria, nonché di caricare un file PDF per brochure.

Rimuovere il `NumberOfProducts` BoundField dal DetailsView completamente e quindi aggiornare il `HeaderText` delle proprietà delle `CategoryName` e `BrochurePath` BoundField per categoria e Brochure, rispettivamente. Successivamente, convertire le `BrochurePath` BoundField in un TemplateField e aggiungere un nuovo TemplateField dell'immagine, fornendo in questo nuovo TemplateField un `HeaderText` valore dell'immagine. Spostare il `Picture` TemplateField in modo che sia tra il `BrochurePath` TemplateField e CommandField.


![Associare il CategoriesDataSource DetailsView e Abilita inserimento](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**Figura 7**: DetailsView per associare il `CategoriesDataSource` e Abilita inserimento


Se è stato convertito il `BrochurePath` include il TemplateField BoundField in un TemplateField tramite la finestra di dialogo Modifica campi, un' `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate`. Solo il `InsertItemTemplate` è necessario, tuttavia, è possibile rimuovere gli altri due modelli. La sintassi dichiarativa per il controllo DetailsView s a questo punto dovrebbe essere simile al seguente:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Aggiunta di controlli per i campi di immagine e Brochure FileUpload

Attualmente, il `BrochurePath` s TemplateField `InsertItemTemplate` contiene una casella di testo, mentre il `Picture` TemplateField non contiene alcun modello. È necessario aggiornare queste due macchine virtuali TemplateField `InsertItemTemplate` per usare i controlli FileUpload.

DetailsView s smart tag, scegliere l'opzione di modifica modelli e quindi selezionare il `BrochurePath` TemplateField s `InsertItemTemplate` nell'elenco a discesa. Rimuovere la casella di testo e quindi trascinare un controllo FileUpload dalla casella degli strumenti nel modello. Impostare il controllo FileUpload 1!s `ID` a `BrochureUpload`. Analogamente, aggiungere un controllo FileUpload per il `Picture` TemplateField s `InsertItemTemplate`. Impostare questo controllo FileUpload 1!s `ID` a `PictureUpload`.


[![Aggiungere un controllo FileUpload al InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**Figura 8**: aggiungere un controllo FileUpload per il `InsertItemTemplate` ([fare clic per visualizzare l'immagine con dimensioni normali](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))


Dopo aver apportato queste aggiunte, le due sintassi dichiarativa per s TemplateField sarà:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

Quando un utente aggiunge una nuova categoria, è opportuno assicurarsi che le brochure siano del tipo di file corretto. Per brochure, l'utente deve specificare un file PDF. Per l'immagine, è necessario all'utente di caricare un file di immagine, ma è consentita *qualsiasi* solo i file di immagine di un determinato tipo, ad esempio jpg o GIF o file di immagine? Per consentire diversi tipi di file, d è necessario estendere la `Categories` dello schema per includere una colonna che acquisisce il tipo di file in modo che questo tipo può essere inviato al client tramite `Response.ContentType` in `DisplayCategoryPicture.aspx`. Poiché non abbiamo t dispone di una colonna di questo tipo, sarebbe prudente limitare gli utenti a fornire solo un tipo di file di immagine specifico. Il `Categories` immagini esistenti nella tabella s sono mappe di bit, ma il file. jpg sono un formato di file più appropriato per le immagini gestite tramite il web.

Se un utente carica un tipo di file non corretto, è necessario annullare l'inserimento e visualizzare un messaggio che indica il problema. Aggiungere un controllo etichetta Web di sotto di DetailsView. Impostare relativo `ID` proprietà `UploadWarning`, deselezionare le relativo `Text` impostata, il `CssClass` proprietà ad avviso e la `Visible` e `EnableViewState` proprietà da `false`. Il `Warning` CSS classe è definita in `Styles.css` ed esegue il rendering di testo in un tipo di carattere di grandi dimensioni, rosso, in corsivo e grassetto.

> [!NOTE]
> In teoria, il `CategoryName` e `Description` BoundField viene convertita come TemplateFields e le relative interfacce inserimento personalizzati. Il `Description` inserimento dell'interfaccia, ad esempio, sarebbe probabilmente ottimizzandola tramite una casella di testo su più righe. E, poiché il `CategoryName` non accetta un colonna `NULL` valori, RequiredFieldValidator deve essere aggiunto per verificare che l'utente fornisce un valore per il nuovo nome di categoria s. Questi passaggi vengono lasciati come esercizio per il lettore. Fare riferimento a [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) per un approfondimento aumentando le interfacce di modifica dei dati.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Passaggio 6: Salvataggio Brochure caricata nel File System s Server Web

Quando l'utente immette i valori per una nuova categoria e fa clic sul pulsante di inserimento, si verifica un postback ed espande il flusso di lavoro di inserimento. Per prima cosa, la s DetailsView [ `ItemInserting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) viene attivato. Successivamente, gli oggetti ObjectDataSource `Insert()` metodo viene richiamato, offrendo un nuovo record viene aggiunto al `Categories` tabella. Successivamente, la s DetailsView [ `ItemInserted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) viene attivato.

Prima di ObjectDataSource s `Insert()` metodo viene richiamato, è necessario verificare innanzitutto che dall'utente sono stati caricati i tipi di file appropriato e quindi salvare il brochure PDF nel file System s server web. Creare un gestore eventi per s DetailsView `ItemInserting` eventi e aggiungere il codice seguente:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

Il gestore dell'evento inizia facendo la `BrochureUpload` controllo FileUpload dai modelli s DetailsView. Quindi, se una brochure è stata caricata, viene esaminato l'estensione di file caricato s. Se non è l'estensione. PDF, quindi un avviso è visualizzato, l'inserimento è stata annullata e termina l'esecuzione del gestore dell'evento.

> [!NOTE]
> Basandosi sull'estensione s file caricato non è una tecnica più efficiente per garantire che il file caricato non è un documento PDF. L'utente potrebbe comportare un documento PDF valido con estensione `.Brochure`, o potrebbe aver creato un documento non PDF e averlo assegnato un `.pdf` estensione. Il contenuto binario del file s dovrà essere esaminato a livello di codice per più di concludere il tipo di file. Questi approcci completi, tuttavia, sono spesso eccessivamente; verifica l'estensione è sufficiente per la maggior parte degli scenari.


Come descritto nel [caricamento di file](uploading-files-cs.md) dell'esercitazione, è necessario prestare attenzione durante il salvataggio dei file nel file System in modo che il caricamento di un utente s non sovrascrive un altro s. Per questa esercitazione si proverà a usare lo stesso nome del file caricato. Se esiste già un file nei `~/Brochures` directory con lo stesso nome file, tuttavia, è possibile aggiungere un numero alla fine fino a quando non viene trovato un nome univoco. Ad esempio, se l'utente ha caricato un file brochure denominato `Meats.pdf`, ma è già presente un file denominato `Meats.pdf` nel `~/Brochures` cartella, si modificherà il nome del file salvato a `Meats-1.pdf`. Se presente, si proverà a eseguire `Meats-2.pdf`e così via, fino a quando non viene trovato un nome file univoco.

Il codice seguente usa il [ `File.Exists(path)` metodo](https://msdn.microsoft.com/library/system.io.file.exists.aspx) per determinare se esiste già un file con il nome file specificato. In questo caso, continuerà a tentare di nuovi nomi di file per brochure fino a quando non viene trovato alcun conflitto.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

Una volta individuato un nome file valido, il file deve essere salvato nel file system e gli oggetti ObjectDataSource `brochurePath``InsertParameter` valore deve essere aggiornato in modo che il nome del file viene scritto nel database. Come abbiamo visto nella *caricamento di file* esercitazione, il file può essere salvato utilizzando il controllo FileUpload s `SaveAs(path)` (metodo). Per aggiornare gli oggetti ObjectDataSource `brochurePath` parametro, usare il `e.Values` raccolta.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Passaggio 7: Il salvataggio dell'immagine caricata nel database

Per archiviare l'immagine caricata nel nuovo `Categories` record, è necessario assegnare il contenuto binario caricato agli oggetti ObjectDataSource `picture` parametro in s DetailsView `ItemInserting` evento. Prima dell'assegnazione, tuttavia, è necessario assicurarsi innanzitutto che l'immagine caricata è in formato JPG e non un altro tipo di immagine. Come nel passaggio 6, consentire s utilizzare l'estensione di file di immagine caricata s per verificare il relativo tipo.

Mentre il `Categories` table consente `NULL` i valori per il `Picture` colonna, tutte le categorie attualmente dispone di un'immagine. Let s forzare l'utente per fornire un'immagine quando si aggiunge una nuova categoria tramite questa pagina. Il codice seguente controlla per verificare che sia stata caricata un'immagine e che disponga di un'estensione appropriata.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

Questo codice deve essere posizionato *prima di* il codice dal passaggio 6 in modo che se si è verificato un problema con il caricamento di immagini, il gestore dell'evento termina prima che il file brochure viene salvato nel file System.

Supponendo che è stato caricato un file appropriato, assegnare il contenuto binario caricato per il valore di s parametri immagine con la riga di codice seguente:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>L'intero`ItemInserting`gestore dell'evento

Per motivi di completezza, ecco il `ItemInserting` gestore eventi nella loro interezza:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Passaggio 8: Correzione di`DisplayCategoryPicture.aspx`pagina

Let s si consiglia di testare l'interfaccia di inserimento e `ItemInserting` gestore eventi che è stato creato negli ultimi passaggi successivi. Visita il `UploadInDetailsView.aspx` pagina tramite un browser e si tenta di aggiungere una categoria, ma omettere l'immagine o specificare un'immagine non JPG o una brochure non PDF. In uno di questi casi, verrà visualizzato un messaggio di errore e il flusso di lavoro di inserimento annullata.


[![Un messaggio di avviso viene visualizzato se viene caricato un tipo di File non valido](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**Figura 9**: un messaggio di avviso viene visualizzato se viene caricato un tipo di File non valido ([fare clic per visualizzare l'immagine con dimensioni normali](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))


Dopo avere verificato che la pagina richiede un'immagine da caricare e vinti t accetta i file non PDF o non JPG, aggiungere una nuova categoria con un'immagine JPG valida, se si lascia vuoto il campo Brochure. Dopo aver fatto clic sul pulsante Inserisci, verrà postback della pagina e verrà aggiunto un nuovo record per il `Categories` tabella con il contenuto binario di immagine caricata s archiviati direttamente nel database. Il controllo GridView viene aggiornato e Mostra una riga per la categoria appena aggiunta, ma, come illustrato nella figura 10, la nuova immagine s categoria non viene visualizzata correttamente.


[![La nuova categoria s che immagine non viene visualizzata](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**Figura 10**: la nuova categoria s immagine non viene visualizzata ([fare clic per visualizzare l'immagine con dimensioni normali](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))


Il motivo non viene visualizzata la nuova immagine è che il `DisplayCategoryPicture.aspx` pagina che restituisce un'immagine di categoria specificata s è configurato per elaborare le bitmap con un'intestazione OLE. Questa intestazione 78 byte viene rimosso dal `Picture` s binario del contenuto delle colonne prima che vengano inviati al client. Ma il file JPG che è appena caricati per la nuova categoria non dispone di questa intestazione OLE. Pertanto, vengono revocate byte validi, necessari dai dati binari s immagine.

Poiché sono ora disponibili entrambe le bitmap con intestazioni OLE e jpg nel `Categories` tabella, è necessario aggiornare `DisplayCategoryPicture.aspx` in modo che non l'intestazione OLE rimozione per le categorie di otto originale e consente di ignorare questa rimozione ai record di categoria più recenti. Nell'esercitazione successiva esamineremo come aggiornare un'immagine di record s esistente e verrà aggiornato tutte le immagini di categoria precedente in modo che siano file. jpg. Per ora, tuttavia, usare il codice seguente nel `DisplayCategoryPicture.aspx` per rimuovere le intestazioni OLE solo per le otto categorie originale:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

Con questa modifica, l'immagine JPG viene ora eseguito il rendering correttamente in GridView.


[![Le immagini JPG di nuove categorie sono correttamente eseguito il rendering](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**Figura 11**: The immagini JPG di nuove categorie sono correttamente eseguito il rendering ([fare clic per visualizzare l'immagine con dimensioni normali](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Passaggio 9: Eliminare Brochure in caso di un'eccezione

Uno dei problemi di archiviazione dei dati binari nel file system s server web è l'introduzione di uno scollamento tra il modello di dati e i relativi dati binari. Pertanto, ogni volta che un record viene eliminato, i dati binari corrispondenti nel file system devono anche essere rimossi. Ciò può entrare in gioco durante l'inserimento, nonché. Si consideri lo scenario seguente: un utente aggiunge una nuova categoria, specificando un'immagine valida e brochure. Al momento facendo clic sul pulsante Inserisci, si verifica un postback e s DetailsView `ItemInserting` viene generato l'evento, salvataggio brochure nel file System s server web. Successivamente, gli oggetti ObjectDataSource `Insert()` metodo viene richiamato, che chiama il `CategoriesBLL` classe s `InsertWithPicture` metodo, che chiama il `CategoriesTableAdapter` s `InsertWithPicture` (metodo).

Ora, cosa accade se il database è offline o se si verifica un errore nel `INSERT` istruzione SQL? Chiaramente l'inserimento avrà esito negativo, in modo che nessuna nuova riga category verrà aggiunto al database. Ma viene comunque mantenuto il file caricato brochure che si trova nel file system s server web. Questo file deve essere eliminata in caso di un'eccezione durante l'inserimento del flusso di lavoro.

Come illustrato in precedenza nel [BLL - la gestione e le eccezioni di livello in una pagina ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) esercitazione, quando viene generata un'eccezione da entro le profondità dell'architettura viene propagata alle attraverso i vari livelli. A livello di presentazione, è possibile determinare se si è verificata un'eccezione da s DetailsView `ItemInserted` evento. Questo gestore dell'evento fornisce anche i valori di istanze della classe ObjectDataSource `InsertParameters`. Pertanto, è possibile creare un gestore eventi per il `ItemInserted` evento che controlla se si è verificata un'eccezione e, in questo caso, Elimina il file specificato da s ObjectDataSource `brochurePath` parametro:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>Riepilogo

Esistono una serie di passaggi che devono essere eseguiti per fornire un'interfaccia basata sul web per l'aggiunta di record che includono i dati binari. Se vengono archiviati i dati binari direttamente nel database, è probabile, devi aggiornare l'architettura, aggiunta di metodi specifici per gestire il caso in cui viene inseriti i dati binari. Dopo aver aggiornata l'architettura, il passaggio successivo consiste nel creare l'interfaccia di inserimento, che è possibile usare un controllo DetailsView che è stato personalizzato per includere un controllo FileUpload per ogni campo di dati binari. I dati caricati possono essere salvati nel file System s server web o assegnati a un parametro di origine dati in s DetailsView `ItemInserting` gestore dell'evento.

Salvataggio di dati binari nel file System richiede pianificazione più accurata del salvataggio dei dati direttamente nel database. È necessario scegliere uno schema di denominazione per evitare un caricamento utente s sovrascrivendo s un'altra. Inoltre, è necessario effettuare passaggi aggiuntivi per eliminare il file caricato se ha esito negativo dell'inserimento del database.

È ora disponibile la possibilità di aggiungere nuove categorie al sistema con una brochure e immagine, ma è fornire un supporto iniziale per esaminare come aggiornare i dati binari un esistenti categoria s o rimuovere correttamente i dati binari per una categoria eliminato. Nella prossima esercitazione verrà illustrato questi argomenti.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Dave Gardner, Teresa Murphy e Bernadette Leigh. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](displaying-binary-data-in-the-data-web-controls-cs.md)
> [Successivo](updating-and-deleting-existing-binary-data-cs.md)
