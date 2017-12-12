---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: Incluso un File di caricare l'opzione quando si aggiunge un nuovo Record (c#) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione viene illustrato come creare un'interfaccia Web che consente all'utente di immettere i dati di testo e caricare file binari. Per illustrare le opzioni disponibili t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: fcb791868e6af9eef1614d039d11ef5232b40af5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>Tra cui un'opzione di caricamento di File quando si aggiunge un nuovo Record (c#)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe) o [Scarica il PDF](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> In questa esercitazione viene illustrato come creare un'interfaccia Web che consente all'utente di immettere i dati di testo e caricare file binari. Per illustrare le opzioni disponibili per archiviare i dati binari, un file verrà salvato nel database mentre l'altra viene archiviata nel file system.


## <a name="introduction"></a>Introduzione

Nelle due esercitazioni precedenti abbiamo esplorato tecniche per l'archiviazione dei dati binari che sono associati al modello di dati applicazione s, esaminato come utilizzare il controllo FileUpload per inviare file dal client al server web e visto come presentare i dati binari in una data W controllo EB. Si sposta ancora da trattare come associare i dati caricati con il modello di dati, tuttavia.

In questa esercitazione si creerà una pagina web per aggiungere una nuova categoria. Oltre alle caselle di testo per il nome della categoria s e la descrizione, questa pagina sarà necessario includere due controlli FileUpload uno per la nuova immagine di categoria s e uno per brochure. L'immagine caricata verrà archiviato direttamente nel nuovo record s `Picture` colonna, mentre la brochure verrà salvata la `~/Brochures` cartella con il percorso al file salvato nel nuovo record s `BrochurePath` colonna.

Prima di creare la nuova pagina web, sarà necessario aggiornare l'architettura. Il `CategoriesTableAdapter` query principale s non recupera il `Picture` colonna. Di conseguenza, generata automaticamente `Insert` dispone solo di input per il `CategoryName`, `Description`, e `BrochurePath` campi. Pertanto, è necessario creare un metodo aggiuntivo in TableAdapter che richiede di tutti e quattro `Categories` campi. La `CategoriesBLL` classe nel livello di logica di Business anche dovrà essere aggiornata.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Passaggio 1: Aggiunta di un`InsertWithPicture`metodo per il`CategoriesTableAdapter`

Quando abbiamo creato il `CategoriesTableAdapter` nuovamente il [la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-cs.md) esercitazione, è configurata in modo da generare automaticamente `INSERT`, `UPDATE`, e `DELETE` istruzioni in base alla query principale. Inoltre, è richiesto da adottare l'approccio diretto DB, che ha creato i metodi TableAdapter `Insert`, `Update`, e `Delete`. Questi metodi eseguire generata automaticamente `INSERT`, `UPDATE`, e `DELETE` istruzioni e, di conseguenza, accettare i parametri di input basati sulle colonne restituite dalla query principale. Nel [caricamento di file](uploading-files-cs.md) esercitazione è aumentate la `CategoriesTableAdapter` per utilizzare query principale s il `BrochurePath` colonna.

Poiché il `CategoriesTableAdapter` non fa riferimento la query principale s il `Picture` colonna, è possibile aggiungere un nuovo record né aggiornare un record esistente con un valore per il `Picture` colonna. Per acquisire queste informazioni, è possibile creare un nuovo metodo nella TableAdapter in cui viene utilizzato in particolare per inserire un record con dati binari oppure ci occupiamo della personalizzazione generata automaticamente `INSERT` istruzione. Il problema con la personalizzazione generata automaticamente `INSERT` istruzione è che si corre il rischio che il nostro personalizzazioni sovrascritte dalla procedura guidata. Ad esempio, si supponga che è stato personalizzato il `INSERT` includono l'utilizzo dell'istruzione il `Picture` colonna. In questo modo verrà aggiornato i TableAdapter `Insert` metodo per includere un parametro di input aggiuntivo per i dati di categoria s s immagine binari. È quindi possibile creare un metodo nel livello di logica di Business di utilizzare questo metodo DAL e richiamare questo metodo BLL attraverso il livello di presentazione e funzioni meravigliosa. Vale a dire, fino al successivo è stato configurato il TableAdapter mediante la configurazione guidata TableAdapter. Non appena completato la procedura guidata, il nostro personalizzazioni il `INSERT` verrebbe sovrascritto istruzione, il `Insert` metodo ripristinata la forma precedente e il codice potrebbe non venire più compilato!

> [!NOTE]
> Questo fastidio è il problema quando si utilizzano stored procedure anziché istruzioni SQL ad hoc. Un'esercitazione in futura verrà illustrato l'utilizzo di stored procedure anziché istruzioni SQL ad hoc nel livello di accesso ai dati.


Per evitare questo rischio cefalea, anziché le istruzioni SQL generate automaticamente di personalizzazione consentono s invece di creare un nuovo metodo per l'oggetto TableAdapter. Questo metodo, denominato `InsertWithPicture`, accetterà i valori per il `CategoryName`, `Description`, `BrochurePath`, e `Picture` colonne ed eseguire un `INSERT` istruzione che archivia tutti i quattro valori in un nuovo record.

Aprire il DataSet tipizzato e, dalla finestra di progettazione, fare clic su di `CategoriesTableAdapter` intestazione s e scegliere Aggiungi Query dal menu di scelta rapida. Verrà avviata la configurazione guidata Query TableAdapter che inizia con richiede la modalità della query TableAdapter deve accedere al database. Scegliere Usa istruzioni SQL e fare clic su Avanti. Il passaggio successivo viene richiesto per il tipo di query da generare. Poiché è nuovamente la creazione di una query per aggiungere un nuovo record per il `Categories` tabella, scegliere l'inserimento e fare clic su Avanti.


[![Selezionare l'opzione di inserimento](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**Figura 1**: selezionare l'opzione di inserimento ([fare clic per visualizzare l'immagine ingrandita](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))


È ora necessario specificare il `INSERT` istruzione SQL. La procedura guidata suggerisce automaticamente un `INSERT` istruzione corrisponde alla query principale s di TableAdapter. In questo caso, è s un `INSERT` istruzione che inserisce il `CategoryName`, `Description`, e `BrochurePath` valori. L'istruzione Update in modo che il `Picture` colonna è inclusa insieme a un `@Picture` parametro, come illustrato di seguito:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

Nella schermata finale della procedura guidata viene chiesto di specificare un nome al nuovo metodo di TableAdapter. Immettere `InsertWithPicture` e fare clic su Fine.


[![Denominare il nuovo InsertWithPicture TableAdapter (metodo)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**Figura 2**: nome del nuovo metodo TableAdapter `InsertWithPicture` ([fare clic per visualizzare l'immagine ingrandita](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Passaggio 2: Aggiornare il livello di logica di Business

Poiché il livello di presentazione deve solo interfacciarsi con il livello di logica di Business anziché ignorando per passare direttamente a livello di accesso ai dati, è necessario creare un metodo BLL che richiama il metodo DAL appena creato (`InsertWithPicture`). Per questa esercitazione, creare un metodo di `CategoriesBLL` classe denominata `InsertWithPicture` che accetta come input tre `string` s e un `byte` matrice. Il `string` sono parametri di input per il nome della categoria s, la descrizione e percorso del file brochure, mentre il `byte` matrice è per il contenuto binario dell'immagine categoria s. Come mostrato nel codice seguente, questo metodo BLL richiama il metodo DAL corrispondente:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> Assicurarsi di aver salvato il set di dati tipizzato prima di aggiungere il `InsertWithPicture` metodo al livello Business Logic. Poiché il `CategoriesTableAdapter` codice della classe viene generato automaticamente in base al set di dati tipizzato, se non si t prima di salvare le modifiche al set di dati tipizzati di `Adapter` proprietà vinto t conoscere il `InsertWithPicture` (metodo).


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Passaggio 3: Elenco di categorie esistenti e i relativi dati binari

In questa esercitazione si creerà una pagina che consente agli utenti finali di aggiungere una nuova categoria per il sistema, fornendo un'immagine e brochure per la nuova categoria. Nel [esercitazione precedente](displaying-binary-data-in-the-data-web-controls-cs.md) è utilizzato un GridView con un TemplateField e ImageField per visualizzare il nome di ogni categoria s, descrizione, immagine e un collegamento per scaricare la brochure. Consente di replicare la funzionalità per questa esercitazione, creazione di una pagina che elenca tutte le categorie esistenti e consente ai nuovi da creare s.

Aprire il `DisplayOrDownload.aspx` pagina dal `BinaryData` cartella. Passare alla visualizzazione origine e copiare GridView e ObjectDataSource s sintassi dichiarativa, incollandoli all'interno di `<asp:Content>` elemento `UploadInDetailsView.aspx`. Inoltre, tenere sempre dimenticare copiare il `GenerateBrochureLink` metodo della classe code-behind di `DisplayOrDownload.aspx` a `UploadInDetailsView.aspx`.


[![Copiare e incollare la sintassi dichiarativa da DisplayOrDownload.aspx a UploadInDetailsView.aspx](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**Figura 3**: copiare e incollare la sintassi dichiarativa da `DisplayOrDownload.aspx` a `UploadInDetailsView.aspx` ([fare clic per visualizzare l'immagine ingrandita](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))


Dopo aver copiato la sintassi dichiarativa e `GenerateBrochureLink` metodo tramite il `UploadInDetailsView.aspx` pagina, visualizzare la pagina tramite un browser per verificare che tutto ciò che è stato copiato correttamente. Dovrebbe essere GridView otto categorie di elenco che include un collegamento per scaricare la brochure, nonché l'immagine di categoria s.


[![Viene visualizzato ogni categoria insieme ai relativi dati binari](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**Figura 4**: È necessario ora vedere ogni categoria insieme con i relativi dati binari ([fare clic per visualizzare l'immagine ingrandita](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Passaggio 4: Configurazione di`CategoriesDataSource`all'inserimento di supporto

Il `CategoriesDataSource` ObjectDataSource viene utilizzato il `Categories` GridView attualmente non fornisce la possibilità di inserire i dati. Per supportare l'inserimento di questo controllo dell'origine dati, è necessario eseguire il mapping relativo `Insert` metodo a un metodo in un oggetto sottostante, `CategoriesBLL`. In particolare, si desidera eseguire il mapping per il `CategoriesBLL` metodo abbiamo aggiunto nel passaggio 2, `InsertWithPicture`.

Avviare facendo clic sul collegamento Configura origine dati da smart tag ObjectDataSource s. La prima schermata mostra l'oggetto origine dati è configurato per funzionare con, `CategoriesBLL`. Lasciare l'impostazione- e fare clic su Avanti per passare alla schermata di definire i metodi di dati. Passare alla scheda Inserisci e selezionare il `InsertWithPicture` metodo dall'elenco a discesa. Fare clic su Fine per completare la procedura guidata.


[![Configurare ObjectDataSource per utilizzare il metodo InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**Figura 5**: ObjectDataSource consente di configurare il `InsertWithPicture` metodo ([fare clic per visualizzare l'immagine ingrandita](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))


> [!NOTE]
> Dopo avere completato la procedura guidata, Visual Studio potrebbe chiedere se si desidera aggiornare i campi e le chiavi, che verrà rigenerata dati Web controlla i campi. Scegliere No, perché se si sceglie Sì, eventuali personalizzazioni campo apportate verranno sovrascritte.


Dopo aver completato la procedura guidata, ObjectDataSource ora includere un valore per il relativo `InsertMethod` proprietà così come `InsertParameters` per le colonne di quattro categoria, come il seguente markup dichiarativo illustra:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Passaggio 5: Creazione dell'interfaccia di inserimento

Come primo analizzate nel [una panoramica di inserimento, aggiornamento ed eliminazione di dati](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), il controllo DetailsView fornisce un'interfaccia di inserimento predefinita che può essere utilizzata quando si lavora con un controllo origine dati che supporta l'inserimento. Consente di aggiungere un controllo DetailsView a questa pagina di sopra di GridView che vengono visualizzati in modo permanente l'interfaccia di inserimento, consentendo all'utente di aggiungere rapidamente una nuova categoria s. Dopo l'aggiunta di una nuova categoria nel controllo DetailsView, GridView sottostanti automaticamente Aggiorna e verrà visualizzato la nuova categoria.

Avvio mediante il trascinamento di un controllo DetailsView dalla casella degli strumenti nella finestra di progettazione sopra GridView, impostare il relativo `ID` proprietà `NewCategory` e cancellare il `Height` e `Width` i valori delle proprietà. Da DetailsView s smart tag, associarlo a esistente `CategoriesDataSource` e quindi selezionare la casella di controllo Abilita inserimento.


[![Associare il controllo DetailsView il CategoriesDataSource e Abilita inserimento](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**Figura 6**: controllo DetailsView per associare il `CategoriesDataSource` e Abilita inserimento ([fare clic per visualizzare l'immagine ingrandita](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))


Per eseguire il rendering in modo permanente di DetailsView nella relativa interfaccia di inserimento, impostare il relativo `DefaultMode` proprietà `Insert`.

Si noti che il controllo DetailsView ha cinque BoundField `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, e `BrochurePath` anche se il `CategoryID` BoundField non viene eseguito nell'interfaccia inserimento perché il relativo `InsertVisible` proprietà è impostata su `false`. Questi BoundField esiste perché sono le colonne restituite dal `GetCategories()` metodo, ovvero cosa ObjectDataSource richiama per recuperare i dati. Per l'inserimento, tuttavia, non abbiamo desidera consentire all'utente di specificare un valore per t `NumberOfProducts`. Inoltre, è necessario consentire loro di caricare un'immagine per la nuova categoria, nonché di caricare un file PDF per brochure.

Rimuovere il `NumberOfProducts` BoundField dal controllo DetailsView completamente e quindi aggiornare il `HeaderText` le proprietà del `CategoryName` e `BrochurePath` BoundField per categoria e Brochure, rispettivamente. Successivamente, convertire il `BrochurePath` BoundField in un TemplateField e aggiungere un nuovo TemplateField per l'immagine di questo nuovo TemplateField fornendo un `HeaderText` valore dell'immagine. Spostare il `Picture` TemplateField in modo che sia compreso tra il `BrochurePath` TemplateField e CommandField.


![Associare il controllo DetailsView il CategoriesDataSource e Abilita inserimento](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**Figura 7**: controllo DetailsView per associare il `CategoriesDataSource` e Abilita inserimento


Se è stato convertito il `BrochurePath` BoundField in un TemplateField tramite la finestra di dialogo Modifica campi, il TemplateField include un `ItemTemplate`, `EditItemTemplate`, e `InsertItemTemplate`. Solo il `InsertItemTemplate` è necessario, tuttavia, quindi è possibile rimuovere i due modelli. La sintassi dichiarativa di DetailsView s a questo punto dovrebbe essere simile al seguente:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Aggiunta di controlli FileUpload per Brochure e i campi di immagine

Attualmente, il `BrochurePath` TemplateField s `InsertItemTemplate` contiene una casella di testo, mentre il `Picture` TemplateField non contiene alcun modello. È necessario aggiornare questi due s TemplateField `InsertItemTemplate` per usare i controlli FileUpload.

Dal controllo DetailsView s smart tag, scegliere l'opzione di modifica modelli, quindi selezionare il `BrochurePath` TemplateField s `InsertItemTemplate` dall'elenco a discesa. Rimuovere la casella di testo e quindi trascinare un controllo FileUpload dalla casella degli strumenti nel modello. Impostare il controllo FileUpload s `ID` a `BrochureUpload`. Analogamente, aggiungere un controllo FileUpload al `Picture` TemplateField s `InsertItemTemplate`. Impostare questo controllo FileUpload s `ID` a `PictureUpload`.


[![Aggiungere un controllo FileUpload il InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**Figura 8**: aggiungere un controllo FileUpload al `InsertItemTemplate` ([fare clic per visualizzare l'immagine ingrandita](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))


Dopo aver apportato queste aggiunte, saranno due sintassi dichiarativa TemplateField s:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

Quando un utente aggiunge una nuova categoria, si desidera assicurarsi che le brochure siano del tipo di file corretti. Per brochure, l'utente deve specificare un file PDF. Per l'immagine, è necessario caricare un file di immagine all'utente, ma è consentito *qualsiasi* solo i file di immagine di un determinato tipo, ad esempio file GIF o jpg o di file di immagine? Per consentire diversi tipi di file, d è necessario estendere il `Categories` dello schema per includere una colonna che acquisisce il tipo di file in modo che questo tipo può essere inviato al client tramite `Response.ContentType` in `DisplayCategoryPicture.aspx`. Poiché non abbiamo t dispone di una colonna di questo tipo, sarebbe opportuno limitare gli utenti a fornire solo un tipo di file di immagine specifico. Il `Categories` immagini esistenti nella tabella s sono mappe di bit, ma jpg sono un formato di file più appropriato per le immagini servita sul web.

Se un utente carica un tipo di file non corretto, è necessario annullare l'inserimento e viene visualizzato un messaggio che indica il problema. Aggiungere un controllo etichetta Web sotto il controllo DetailsView. Impostare relativo `ID` proprietà `UploadWarning`, deselezionare le relativo `Text` impostata, il `CssClass` proprietà avviso e `Visible` e `EnableViewState` proprietà `false`. Il `Warning` classe CSS è definita in `Styles.css` ed esegue il rendering di testo in un tipo di carattere di grandi dimensioni, rosso, corsivo e grassetto.

> [!NOTE]
> In teoria, il `CategoryName` e `Description` BoundField viene convertito in TemplateFields e le relative interfacce inserimento personalizzati. Il `Description` inserimento di interfaccia, ad esempio, sarebbe probabilmente essere adeguato tramite una casella di testo su più righe. E, poiché il `CategoryName` colonna non accetta `NULL` valori, RequiredFieldValidator deve essere aggiunto per verificare che l'utente fornisce un valore per il nuovo nome di categoria s. Questi passaggi vengono lasciati come un esercizio al lettore. Fare riferimento al [personalizzazione dell'interfaccia di modifica dati](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) per approfondimenti aumento le interfacce di modifica dei dati.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Passaggio 6: Salvataggio Brochure caricata nel File System di Web Server s

Quando l'utente immette i valori per una nuova categoria e fa clic sul pulsante Inserisci, espande il flusso di lavoro di inserimento si verifica un postback. Innanzitutto, i dispositivi di DetailsView [ `ItemInserting` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) generato. Successivamente, s ObjectDataSource `Insert()` metodo viene richiamato, dando luogo a un nuovo record, l'aggiunta di `Categories` tabella. Dopo che i dispositivi di DetailsView [ `ItemInserted` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) generato.

Prima di s ObjectDataSource `Insert()` metodo viene richiamato, è necessario innanzitutto assicurarsi che i tipi di file appropriato sono stati caricati dall'utente e quindi salvare la brochure PDF nel file System s server web. Creare un gestore eventi per s DetailsView `ItemInserting` eventi e aggiungere il codice seguente:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

Il gestore dell'evento viene avviato facendo riferimento al `BrochureUpload` controllo FileUpload dai modelli s DetailsView. Quindi, se è stata caricata una brochure, viene esaminato l'estensione di file caricato s. Se non è l'estensione. PDF, quindi un messaggio di avviso è visualizzato, l'istruzione insert è stata annullata e termina l'esecuzione del gestore dell'evento.

> [!NOTE]
> Non basarsi sull'estensione file caricato s è una tecnica più efficiente per garantire che il file caricato sia un documento PDF. L'utente potrebbe avere un documento PDF valido con estensione `.Brochure`, o Impossibile effettuato un documento non PDF e assegnare un `.pdf` estensione. Il contenuto binario del file s dovranno essere esaminati a livello di codice per più di concludere il tipo di file. Questi approcci completo, tuttavia, sono spesso eccessivi; l'estensione di controllo è sufficiente per la maggior parte degli scenari.


Come descritto nel [caricamento di file](uploading-files-cs.md) esercitazione, è necessario prestare attenzione durante il salvataggio dei file nel file System, in modo che il caricamento di un utente s non implica la sovrascrittura s un'altra. Per questa esercitazione si tenterà di utilizzare lo stesso nome del file caricato. Se esiste già un file di `~/Brochures` directory con lo stesso nome file, tuttavia, si sarà accodare un numero alla fine fino a quando non viene trovato un nome univoco. Ad esempio, se l'utente carica un file brochure denominato `Meats.pdf`, ma esiste già un file denominato `Meats.pdf` nel `~/Brochures` cartella, si modificherà il nome del file salvato per `Meats-1.pdf`. Se presenti, email `Meats-2.pdf`e così via, fino a quando non viene trovato un nome file univoco.

Il codice seguente usa il [ `File.Exists(path)` metodo](https://msdn.microsoft.com/en-us/library/system.io.file.exists.aspx) per determinare se esiste già un file con il nome file specificato. In questo caso, continuerà a tentare di nuovi nomi di file per brochure fino a quando non viene trovato alcun conflitto.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

Dopo aver individuato un nome file valido, il file deve essere salvato nel file system e s ObjectDataSource `brochurePath``InsertParameter` valore deve essere aggiornato in modo che il nome del file viene scritto nel database. Come abbiamo visto nuovamente il *caricamento di file* esercitazione, il file può essere salvato utilizzando il controllo FileUpload s `SaveAs(path)` metodo. Per aggiornare i dispositivi ObjectDataSource `brochurePath` parametro, utilizzare il `e.Values` insieme.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Passaggio 7: Il salvataggio dell'immagine caricata nel database

Per archiviare l'immagine caricata nel nuovo `Categories` record, è necessario assegnare il contenuto binario caricato s ObjectDataSource `picture` parametro in s DetailsView `ItemInserting` evento. Prima dell'assegnazione, tuttavia, è necessario innanzitutto assicurarsi che l'immagine caricata è in formato JPG e non un altro tipo di immagine. Come passaggio 6, consentire s di utilizzare l'estensione di file di immagine caricata s per verificare il tipo.

Mentre il `Categories` tabella consente `NULL` valori per il `Picture` colonna, tutte le categorie attualmente dispone di un'immagine. Consente di forzare all'utente di specificare un'immagine quando si aggiunge una nuova categoria tramite questa pagina s. Il codice seguente controlla per verificare che sia stata caricata un'immagine e che abbia un'estensione appropriata.


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

Questo codice deve essere inserito *prima* il codice del passaggio 6 in modo che se si verifica un problema con il caricamento dell'immagine, il gestore dell'evento termina prima brochure viene salvato nel file System.

Supponendo che è stato caricato un file appropriato, assegnare il contenuto binario caricato per il valore di s parametri immagine con la riga di codice seguente:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>L'intero`ItemInserting`gestore dell'evento

Per motivi di completezza, ecco la `ItemInserting` gestore dell'evento nella sua interezza:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Passaggio 8: Correzione di`DisplayCategoryPicture.aspx`pagina

Consentono di s richiedere qualche istante il test dell'interfaccia di inserimento e `ItemInserting` gestore eventi che è stato creato negli ultimi passaggi successivi. Visitare il `UploadInDetailsView.aspx` pagina tramite un browser e il tentativo di aggiungere una categoria, ma si omette l'immagine o specificare un'immagine non JPG o brochure non PDF. In ognuno di questi casi, verrà visualizzato un messaggio di errore e il flusso di lavoro di inserimento annullata.


[![Un messaggio di avviso viene visualizzato se viene caricato un tipo di File non valido](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**Figura 9**: un messaggio di avviso viene visualizzato se viene caricato un tipo di File non valido ([fare clic per visualizzare l'immagine ingrandita](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))


Dopo avere verificato che la pagina richiede un'immagine da caricare e t acquisite accettare i file non PDF o non JPG, aggiungere una nuova categoria con un'immagine JPG valida, se si lascia vuoto il campo Brochure. Dopo aver fatto clic sul pulsante Inserisci, verrà postback della pagina e verrà aggiunto un nuovo record per il `Categories` tabella con il contenuto binario immagine caricata s archiviate direttamente nel database. GridView viene aggiornato e viene visualizzata una riga per la categoria appena aggiunta, ma, come mostrato nella figura 10, la nuova immagine s categoria non viene eseguita correttamente.


[![La nuova categoria s che immagine non viene visualizzata.](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**Figura 10**: la nuova categoria s immagine non viene visualizzata ([fare clic per visualizzare l'immagine ingrandita](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))


Il non motivo viene visualizzata la nuova immagine di `DisplayCategoryPicture.aspx` pagina che restituisce un'immagine di categoria specificata s è configurato per elaborare le bitmap con un'intestazione OLE. Questa intestazione 78 byte viene rimossa dal `Picture` s binario del contenuto delle colonne prima che vengano inviati al client. Ma il file JPG che è appena caricati per la nuova categoria non ha questa intestazione OLE. Pertanto, byte validi, è necessari vengono revocate dai dati binari s immagine.

Poiché sono ora disponibili entrambe le bitmap con intestazioni OLE e jpg nel `Categories` tabella, è necessario aggiornare `DisplayCategoryPicture.aspx` in modo che non l'intestazione OLE rimozione per le categorie di otto originale e consente di ignorare questa rimozione per il record più recente di categoria. Nell'esercitazione successiva verrà esaminato come aggiornare un'immagine di record s esistente e verrà aggiornata tutte le immagini di categoria precedente in modo che siano jpg. Per il momento, tuttavia, il codice seguente in `DisplayCategoryPicture.aspx` per rimuovere le intestazioni OLE solo per quelle categorie di otto originale:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

Con questa modifica, l'immagine JPG viene ora eseguito il rendering correttamente in GridView.


[![Le immagini JPG per le nuove categorie sono correttamente il rendering](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**Figura 11**: il immagini JPG per le nuove categorie sono correttamente il rendering ([fare clic per visualizzare l'immagine ingrandita](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Passaggio 9: Eliminazione Brochure in caso di un'eccezione

Uno dei problemi di archiviazione di dati binari nel file system s server web è l'introduzione di una disconnessione tra il modello di dati e i relativi dati binari. Pertanto, ogni volta che viene eliminato un record, i dati binari corrispondenti nel file system devono inoltre essere rimossi. Questo può tenere conto durante l'inserimento, nonché. Si consideri lo scenario seguente: un utente aggiunge una nuova categoria specifica di un'immagine valida e brochure. Al momento facendo clic sul pulsante Inserisci, si verifica un postback e s DetailsView `ItemInserting` viene generato l'evento, il salvataggio delle brochure nel file System s server web. Successivamente, s ObjectDataSource `Insert()` metodo viene richiamato, che chiama il `CategoriesBLL` classe s `InsertWithPicture` metodo che chiama il `CategoriesTableAdapter` s `InsertWithPicture` metodo.

Ora, cosa accade se il database è offline o se si verifica un errore nel `INSERT` istruzione SQL? Chiaramente l'inserimento avrà esito negativo, pertanto nessuna nuova riga category verrà aggiunto al database. Ma è ancora stato del file caricato brochure posto nel file system s server web. Questo file deve essere eliminata in caso di un'eccezione durante l'inserimento del flusso di lavoro.

Come descritto in precedenza nel [BLL - gestione e le eccezioni DAL livello in una pagina ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) esercitazione, quando viene generata un'eccezione all'interno di efficacia dell'architettura viene passata attraverso i vari livelli. A livello di presentazione, è possibile determinare se un'eccezione da DetailsView s `ItemInserted` evento. Questo gestore fornisce anche i valori di s ObjectDataSource `InsertParameters`. Pertanto, è possibile creare un gestore eventi per il `ItemInserted` evento che verifica se si è verificata un'eccezione e, in tal caso, Elimina il file specificato da ObjectDataSource s `brochurePath` parametro:


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>Riepilogo

Esistono una serie di passaggi che devono essere eseguiti per fornire un'interfaccia basata sul web per l'aggiunta di record che includono dati binari. Se i dati binari vengono archiviati direttamente nel database, probabilità sono che sarà necessario aggiornare l'architettura, aggiunta di metodi specifici per gestire il caso in cui l'inserimento dei dati binari. Una volta l'architettura è stata aggiornata, è necessario creare l'interfaccia di inserimento, che può essere eseguita tramite un controllo DetailsView che è stato personalizzato per includere un controllo FileUpload per ogni campo di dati binari. I dati caricati quindi possono essere salvati nel file System s server web o assegnati a un parametro di origine dati nel controllo DetailsView s `ItemInserting` gestore dell'evento.

Salvataggio di dati binari nel file System richiede una pianificazione più del salvataggio dei dati direttamente nel database. Per evitare la sovrascrittura di un altro s un caricamento di utente s, è necessario scegliere uno schema di denominazione. Inoltre, è necessario eseguire passaggi aggiuntivi per eliminare il file caricato, se l'inserimento del database ha esito negativo.

È ora disponibile la possibilità di aggiungere nuove categorie per il sistema con una brochure e immagine, ma è ve ancora per esaminare come aggiornare i dati binari s categoria esistenti o rimuovere correttamente i dati binari per una categoria eliminata. Nella prossima esercitazione verrà illustrato in questi due argomenti.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Dave Gardner Teresa Murphy e Bernadette Leigh. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](displaying-binary-data-in-the-data-web-controls-cs.md)
[Successivo](updating-and-deleting-existing-binary-data-cs.md)
