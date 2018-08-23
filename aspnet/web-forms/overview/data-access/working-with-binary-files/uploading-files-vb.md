---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: Caricamento di file (VB) | Microsoft Docs
author: rick-anderson
description: Informazioni su come consentire agli utenti di caricare i file binari (ad esempio i documenti di Word o PDF) sul sito Web in cui possono essere archiviati nel file system del server...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: fdf7b351152d9c64b0ac4b1bc9d558962e704543
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838043"
---
<a name="uploading-files-vb"></a>Caricamento di file (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe) o [Scarica il PDF](uploading-files-vb/_static/datatutorial54vb1.pdf)

> Informazioni su come consentire agli utenti di caricare i file binari (ad esempio i documenti di Word o PDF) sul sito Web in cui possono essere archiviati nel file system del server o nel database.


## <a name="introduction"></a>Introduzione

Tutte le esercitazioni si va esaminato finora hanno lavorato esclusivamente con dati di testo. Tuttavia, molte applicazioni hanno modelli di dati che consentono di acquisire il testo sia i dati binari. Un sito di documenti per online può consentire agli utenti di caricare un'immagine da associare il proprio profilo. Un sito Web di selezione del personale che consenta agli utenti di caricare i curriculum come un documento Microsoft Word o PDF.

Utilizzo dei dati binari aggiunge una nuova serie di sfide. È necessario decidere come i dati binari vengono archiviati nell'applicazione. L'interfaccia utilizzata per l'inserimento di nuovi record deve essere aggiornata per consentire all'utente di caricare un file dal proprio computer e necessario effettuare passaggi aggiuntivi per visualizzare o fornire un mezzo per il download di un record s associati a dati binari. In questa esercitazione e i successivi tre esamineremo qui come ostacolo queste sfide. Al termine di queste esercitazioni è verrà compilata un'applicazione completamente funzionale che associa un'immagine e brochure PDF a ogni categoria. In questa esercitazione particolare verrà esaminare le diverse tecniche per l'archiviazione dei dati binari e Scopri come consentire agli utenti di caricare un file dal proprio computer e sono salvati nel file system s server web.

> [!NOTE]
> Dati binari che fa parte di un modello di dati dell'applicazione s sono talvolta detta un [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), l'acronimo di Binary Large OBject. In queste esercitazioni ho scelto di usare i dati binari di terminologia, sebbene il termine BLOB è sinonimo.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Passaggio 1: Creazione di lavoro con le pagine Web i dati binari

Prima di iniziare a esplorare le sfide associate all'aggiunta del supporto per i dati binari, consentire s prima di tutto si consiglia di creare le pagine ASP.NET nel progetto di sito Web che è necessario per questa esercitazione e i successivi tre. Iniziare aggiungendo una nuova cartella denominata `BinaryData`. Successivamente, aggiungere le seguenti pagine ASP.NET per quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative ai dati binari](uploading-files-vb/_static/image1.gif)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni relative ai dati binari


In altre cartelle, analogo a `Default.aspx` nella `BinaryData` cartella elencherà le esercitazioni nella relativa sezione. Si tenga presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere questo controllo utente da `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente sectionleveltutoriallisting. ascx a default. aspx](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**Figura 2**: aggiungere i `SectionLevelTutorialListing.ascx` controllo utente da `Default.aspx` ([fare clic per visualizzare l'immagine con dimensioni normali](uploading-files-vb/_static/image2.png))


Infine, aggiungere queste pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo l'ottimizzazione GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, si consiglia di visualizzare il sito Web di esercitazioni tramite un browser. Il menu a sinistra ora include elementi per l'uso con le esercitazioni di dati binari.


![Mappa del sito include ora voci per l'uso con le esercitazioni di dati binari](uploading-files-vb/_static/image3.gif)

**Figura 3**: la mappa del sito include ora voci per l'uso con le esercitazioni di dati binari


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Passaggio 2: Decidere dove Store i dati binari

Dati binari che viene associati al modello di dati di s dell'applicazione possono essere archiviati in una delle due posizioni: nel file system s server web con un riferimento al file archiviato nel database. o direttamente all'interno del database stesso (vedere la figura 4). Ogni approccio ha un proprio set di vantaggi e svantaggi e merita una discussione più dettagliata.


[![Dati binari possono essere archiviati nel File System o direttamente nel Database](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**Figura 4**: dati binari possono essere archiviati nel File System o direttamente nel Database ([fare clic per visualizzare l'immagine con dimensioni normali](uploading-files-vb/_static/image4.png))


Si supponga di voler estendere il database Northwind per associare un'immagine a ogni prodotto. Sarebbe un'opzione per archiviare i file di immagine nel file system s server web e registrare il percorso nel `Products` tabella. Con questo approccio, d aggiungiamo un' `ImagePath` colonna per il `Products` tabella di tipo `varchar(200)`, eventualmente. Quando un utente ha caricato una foto Chai, tale immagine è archiviata nel file system s server web nella `~/Images/Tea.jpg`, dove `~` rappresenta il percorso fisico dell'applicazione s. Vale a dire, se il sito web ha origine nel percorso fisico `C:\Websites\Northwind\`, `~/Images/Tea.jpg` corrisponderebbe a `C:\Websites\Northwind\Images\Tea.jpg`. Dopo aver caricato il file di immagine, è il d aggiornare il record Chai compare nel `Products` tabella in modo che relativo `ImagePath` il percorso della nuova immagine di riferimento di colonna. È possibile usare `~/Images/Tea.jpg` o semplicemente `Tea.jpg` se si è deciso che tutte le immagini dei prodotti vengono posizionate nell'applicazione s `Images` cartella.

I vantaggi principali di archiviare i dati binari nel file system sono:

- **Semplicità di implementazione** come vedremo tra breve, archiviare e recuperare dati binari archiviati direttamente all'interno del database richiede un po' più codice rispetto a quando l'utilizzo dei dati tramite il file system. Inoltre, affinché un utente di visualizzare o scaricare i dati binari essere presentati con un URL a tali dati. Se i dati si trovano nel file system s server web, l'URL è semplice. Se i dati vengono archiviati nel database, tuttavia, una pagina web. è necessario creare recuperare e restituire i dati dal database.
- **Estendere l'accesso ai dati binari** potrebbero devono essere accessibili ad altri servizi o applicazioni, quelle che non è possibile estrarre i dati dal database i dati binari. Ad esempio, le immagini associate a ogni prodotto potrebbe essere necessario anche essere disponibili agli utenti tramite [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), nel qual caso d vogliamo archiviare i dati binari nel file system.
- **Prestazioni** se i dati binari vengono archiviati nel file system, la congestione della domanda e la rete tra il server di database e server web sarà minore se i dati binari vengono archiviati direttamente nel database.

Lo svantaggio principale di archiviare i dati binari nel file system è che separa i dati dal database. Se un record viene eliminato dal `Products` della tabella, i file associato nel file system s server web non vengono eliminata automaticamente. È necessario scrivere codice aggiuntivo per eliminare il file o nel file system verrà diventino troppo pieni con i file inutilizzati, orfani. Inoltre, durante il backup del database, è necessario assicurarsi di prendere i backup dei dati binari associati nel file system, nonché. Spostare il database in un altro sito o server può causare problemi simili.

In alternativa, i dati binari possono essere archiviati direttamente in un database Microsoft SQL Server 2005 mediante la creazione di una colonna di tipo `varbinary`. Come con altri tipi di dati di lunghezza variabile, è possibile specificare una lunghezza massima dei dati binari che possono essere contenuti in questa colonna. Ad esempio, da riservare al massimo 5.000 byte usare `varbinary(5000)`; `varbinary(MAX)` consente le dimensioni massime di archiviazione, circa 2 GB.

Il vantaggio principale di archiviare i dati binari direttamente nel database è l'accoppiamento stretto fra i dati binari e il record del database. Questo semplifica notevolmente l'attività di amministrazione di database, come backup o lo spostamento del database a un altro sito o server. Inoltre, un record automaticamente se si elimina i dati binari corrispondenti. Esistono inoltre ulteriori vantaggi meno evidenti di archiviare i dati binari nel database. Visualizzare [archiviazione di binari file direttamente nel Database tramite ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) per una discussione più approfondita.

> [!NOTE]
> In Microsoft SQL Server 2000 e versioni precedenti, il `varbinary` tipo di dati ha un limite massimo di 8.000 byte. Per archiviare fino a 2 GB di dati binari di [ `image` tipo di dati](https://msdn.microsoft.com/library/ms187993.aspx) deve invece usare. Con l'aggiunta di `MAX` in SQL Server 2005, tuttavia, il `image` tipo di dati è stato deprecato. Lo s ancora supportata per la compatibilità, ma Microsoft ha annunciato che la `image` tipo di dati verrà rimosso in una versione futura di SQL Server.


Se si lavora con un modello di dati precedente verifichi il `image` tipo di dati. Il database Northwind s' `Categories` tabella include un `Picture` colonna che può essere utilizzato per archiviare i dati binari di un file di immagine per la categoria. Poiché il database Northwind ha le sue radici nelle versioni precedenti di SQL Server e Microsoft Access, questa colonna è di tipo `image`.

Per questa esercitazione e i successivi tre, useremo entrambi gli approcci. Il `Categories` tabella esiste già un `Picture` colonna per l'archiviazione del contenuto binario di un'immagine per la categoria. Si aggiungerà una colonna aggiuntiva, `BrochurePath`, per archiviare il percorso di un file PDF nel file system che può essere utilizzato per fornire una panoramica della categoria raffinata qualità stampa web server s.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Passaggio 3: Aggiunta di`BrochurePath`colonna il`Categories`tabella

Attualmente la tabella Categories ha solo quattro colonne: `CategoryID`, `CategoryName`, `Description`, e `Picture`. Oltre a questi campi, è necessario aggiungere una nuova istanza che farà riferimento a brochure categoria s (se presente). Per aggiungere questa colonna, è possibile tornare a Esplora Server, il drill-down nelle tabelle, fare clic su di `Categories` di tabella e scegliere Apri definizione tabella (vedere la figura 5). Se non è possibile visualizzare Esplora Server, aprirlo selezionando l'opzione di Esplora Server dal menu Visualizza o premere Ctrl + Alt + S.

Aggiungere un nuovo `varchar(200)` colonna per il `Categories` tabella denominata `BrochurePath` e consente `NULL` s e fare clic sull'icona di salvataggio (o premere Ctrl + S).


[![Aggiungere una colonna BrochurePath con la tabella Categories](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**Figura 5**: aggiungere un' `BrochurePath` colonna il `Categories` tabella ([fare clic per visualizzare l'immagine con dimensioni normali](uploading-files-vb/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Passaggio 4: Aggiornamento dell'architettura per l'uso di`Picture`e`BrochurePath`colonne

Il `CategoriesDataTable` nel Data Access Layer (causa) sono attualmente disponibili quattro `DataColumn` definiti: `CategoryID`, `CategoryName`, `Description`, e `NumberOfProducts`. Quando è stato progettato originariamente questa DataTable nel [creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) esercitazione, il `CategoriesDataTable` avesse solo le prime tre colonne; il `NumberOfProducts` colonna è stata aggiunta nel [Master/dettaglio mediante un puntato Elenco di record Master e un controllo DataList di dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) esercitazione.

Come descritto nella *creazione di un livello di accesso ai dati*, oggetti DataTable del dataset tipizzato costituiscono gli oggetti business. Gli oggetti TableAdapter sono responsabili per la comunicazione con il database e popolare gli oggetti business con i risultati della query. Il `CategoriesDataTable` viene popolato dal `CategoriesTableAdapter`, che ha tre metodi per il recupero dei dati:

- `GetCategories()` esegue la query principale s TableAdapter e restituisce il `CategoryID`, `CategoryName`, e `Description` campi di tutti i record nel `Categories` tabella. La query principale è quello utilizzato da generata automaticamente `Insert` e `Update` metodi.
- `GetCategoryByCategoryID(categoryID)` Restituisce il `CategoryID`, `CategoryName`, e `Description` campi della categoria cui `CategoryID` è uguale a *categoryID*.
- `GetCategoriesAndNumberOfProducts()` -Restituisce il `CategoryID`, `CategoryName`, e `Description` campi per tutti i record di `Categories` tabella. Usa anche una sottoquery per restituire il numero di prodotti associati a ogni categoria.

Si noti che nessuna di queste query restituiscono il `Categories` tabella s `Picture` o `BrochurePath` le colonne; e non il `CategoriesDataTable` forniscono `DataColumn` s per questi campi. Per poter funzionare con l'immagine e `BrochurePath` proprietà, è necessario prima aggiungerli per il `CategoriesDataTable` e quindi aggiornare il `CategoriesTableAdapter` classe per restituire le colonne.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Aggiungere il`Picture`e`BrochurePath``DataColumn` s

Iniziare aggiungendo queste due colonne per il `CategoriesDataTable`. Fare clic su di `CategoriesDataTable` intestazione s, scegliere Aggiungi dal menu di scelta rapida e quindi scegliere l'opzione di colonna. Verrà creata una nuova `DataColumn` DataTable denominato `Column1`. Rinominare la colonna `Picture`. Dalla finestra delle proprietà, impostare il `DataColumn` s `DataType` proprietà `System.Byte[]` (questo non è un'opzione nell'elenco a discesa, è necessario digitarlo in).


[![Creare un'immagine denominata DataColumn il cui tipo di dati è System. byte](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**Figura 6**: creare un `DataColumn` Named `Picture` cui `DataType` viene `System.Byte[]` ([fare clic per visualizzare l'immagine con dimensioni normali](uploading-files-vb/_static/image8.png))


Aggiungere un'altra `DataColumn` al DataTable, denominarlo `BrochurePath` usando il valore predefinito `DataType` valore (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Restituzione di`Picture`e`BrochurePath`valori dall'oggetto TableAdapter

Con queste due `DataColumn` aggiunte per il `CategoriesDataTable`, sono pronti per aggiornare il `CategoriesTableAdapter`. Voleste avere entrambi questi valori di colonna restituiti dalla query TableAdapter principale, ma in questo modo viene nuovamente i dati binari ogni volta che il `GetCategories()` metodo è stato richiamato. Let s aggiornare invece la query TableAdapter principale per reintrodurre `BrochurePath` e creare un metodo di recupero di dati aggiuntivi che restituisce una determinata categoria s `Picture` colonna.

Per aggiornare la query TableAdapter principale, fare clic su di `CategoriesTableAdapter` intestazione s e scegliere l'opzione di configurazione dal menu di scelta rapida. Verrà visualizzata la configurazione guidata adattatore tabella cui è stato visualizzato in una serie di esercitazioni precedenti. Aggiornare la query per riportare online il `BrochurePath` e fare clic su Fine.


[![Aggiornare l'elenco colonne nell'istruzione SELECT per restituire anche BrochurePath](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**Figura 7**: aggiornare l'elenco di colonne nel `SELECT` istruzione restituisce inoltre `BrochurePath` ([fare clic per visualizzare l'immagine con dimensioni normali](uploading-files-vb/_static/image10.png))


Quando si usano istruzioni SQL ad hoc per i TableAdapter, l'aggiornamento dell'elenco di colonne nella query principali Aggiorna l'elenco delle colonne per tutte le `SELECT` metodi nell'oggetto TableAdapter di query. Ciò significa che il `GetCategoryByCategoryID(categoryID)` metodo è stato aggiornato per restituire il `BrochurePath` colonna, che potrebbe essere è destinato. Tuttavia, anche aggiornato l'elenco di colonne nel `GetCategoriesAndNumberOfProducts()` metodo, rimuovere la sottoquery che restituisce il numero di prodotti per ogni categoria. Pertanto, è necessario aggiornare questo metodo s `SELECT` query. Fare clic sui `GetCategoriesAndNumberOfProducts()` metodo, scegliere Configura e ripristinare il `SELECT` query relativo valore originale:


[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

Successivamente, creare un nuovo metodo TableAdapter che restituisce una determinata categoria s `Picture` valore della colonna. Fare clic su di `CategoriesTableAdapter` intestazione s e scegliere l'opzione di Query aggiunta per avviare la configurazione guidata Query TableAdapter. Il primo passaggio della procedura guidata viene chiesto di specificare che se si desidera eseguire query sui dati usando un'istruzione SQL ad hoc, una nuova stored procedure o una esistente. Selezionare Usa istruzioni SQL e fare clic su Avanti. Poiché si restituisce una riga, scegliere il SELECT che restituisce l'opzione righe del secondo passaggio.


[![Le istruzioni Use SQL opzione SELECT](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**Figura 8**: selezionare l'Usa istruzioni SQL opzione ([fare clic per visualizzare l'immagine con dimensioni normali](uploading-files-vb/_static/image12.png))


[![Poiché la Query restituirà un Record dalla tabella Categories, scegli SELECT che restituisce righe](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**Figura 9**: poiché la Query restituirà un Record dalla tabella Categories, scegliere di selezionare quale vengono restituite righe ([fare clic per visualizzare l'immagine con dimensioni normali](uploading-files-vb/_static/image14.png))


Nel terzo passaggio, immettere la query SQL seguente e fare clic su Avanti:


[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

L'ultimo passaggio consiste nello scegliere il nome per il nuovo metodo. Uso `FillCategoryWithBinaryDataByCategoryID` e `GetCategoryWithBinaryDataByCategoryID` per il riempimento un DataTable e restituire un oggetto DataTable modelli, rispettivamente. Fare clic su Fine per completare la procedura guidata.


[![Scegliere i nomi per i metodi di s TableAdapter](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**Figura 10**: scegliere i nomi per i metodi di TableAdapter ([fare clic per visualizzare l'immagine con dimensioni normali](uploading-files-vb/_static/image16.png))


> [!NOTE]
> Dopo aver completato la configurazione guidata Query di tabella Adapter potrebbe apparire una finestra di dialogo che informa che il nuovo testo del comando restituisce i dati con schema diverso da quello della query principale. In breve, la procedura guidata si noti che la query principale s TableAdapter `GetCategories()` restituisce uno schema diverso da quello appena creato. Ma questo è ciò che vogliamo, in modo che è possibile ignorare questo messaggio.


Inoltre, tenere presente che se si usano istruzioni SQL ad hoc e utilizza la procedura guidata per modificare query TableAdapter s principale in un punto successivo nel tempo, verrà modificato il `GetCategoryWithBinaryDataByCategoryID` metodo s `SELECT` elenco colonne istruzione s da includere solo queste colonne dai la query principale (vale a dire, verranno rimossi i `Picture` colonna dalla query). È necessario aggiornare manualmente l'elenco di colonne per restituire il `Picture` colonna, in modo analogo a quanto avveniva con le `GetCategoriesAndNumberOfProducts()` metodo più indietro in questo passaggio.

Dopo aver aggiunto i due `DataColumn` s per il `CategoriesDataTable` e il `GetCategoryWithBinaryDataByCategoryID` metodo per il `CategoriesTableAdapter`, queste classi in Progettazione DataSet tipizzati dovrebbero essere simile alla schermata illustrata nella figura 11.


![La finestra di progettazione set di dati include le nuove colonne e un metodo](uploading-files-vb/_static/image11.gif)

**Figura 11**: la finestra di progettazione set di dati include le nuove colonne e un metodo


## <a name="updating-the-business-logic-layer-bll"></a>Aggiornare il livello di logica di Business (BLL)

Con DAL aggiornato, resta che per aumentare il livello Business (LOGIC) per includere un metodo per il nuovo `CategoriesTableAdapter` (metodo). Aggiungere il metodo seguente per il `CategoriesBLL` classe:


[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Passaggio 5: Caricare un File dal Client al Server Web

Quando si raccolgono i dati binari, spesso questi dati viene forniti da un utente finale. Per acquisire queste informazioni, l'utente deve essere in grado di caricare un file dal proprio computer al server web. I dati caricati devono quindi essere integrata con il modello di dati, che potrebbe impedire il salvataggio del file al sistema file server web e aggiunta di un percorso del file nel database o si scrive il contenuto binario direttamente nel database. In questo passaggio verrà esaminato in che modo consentire all'utente di caricare file dai loro computer al server. Nella prossima esercitazione si sarà concentrare l'attenzione per integrare il file caricato con modello di dati.

ASP.NET 2.0 s nuove [controllo Web FileUpload](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) fornisce un meccanismo per agli utenti di inviare un file dal proprio computer al server web. Esegue il rendering del controllo FileUpload come un `<input>` elemento la cui `type` attributo è impostato su file, che nei browser visualizzato come una casella di testo con un pulsante Sfoglia. Facendo clic sul pulsante Sfoglia viene visualizzata una finestra di dialogo da cui l'utente può selezionare un file. Quando il modulo viene inviato nuovamente, il contenuto di file selezionato s viene inviato insieme al postback. Sul lato server, informazioni sul file caricato sono accessibile tramite le proprietà di s FileUpload controllo.

Per illustrare il caricamento di file, aprire il `FileUpload.aspx` nella pagina la `BinaryData` cartella, trascinare un controllo FileUpload dalla casella degli strumenti nella finestra di progettazione e impostare il controllo s `ID` proprietà `UploadTest`. Successivamente, aggiungere un controllo pulsante Web l'impostazione relativa `ID` e `Text` delle proprietà per `UploadButton` e caricare i File selezionati, rispettivamente. Infine, inserire un controllo etichetta Web sotto il pulsante, cancellare il contenuto relativo `Text` proprietà e set relativo `ID` proprietà `UploadDetails`.


[![Aggiungere un controllo FileUpload nella pagina ASP.NET](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**Figura 12**: aggiungere un controllo FileUpload nella pagina ASP.NET ([fare clic per visualizzare l'immagine con dimensioni normali](uploading-files-vb/_static/image18.png))


Figura 13 Mostra questa pagina quando viene visualizzato tramite un browser. Si noti che facendo clic sul pulsante Sfoglia consente di visualizzare una finestra di dialogo Selezione file, consentendo all'utente di selezionare un file dal proprio computer. Dopo aver selezionato un file, fare clic sul pulsante Carica File selezionato determina un postback che invia il contenuto binario s file selezionato al server web.


[![L'utente può selezionare un File da caricare dal proprio Computer al Server](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**Figura 13**: l'utente può selezionare un File da caricare dal proprio Computer al Server ([fare clic per visualizzare l'immagine con dimensioni normali](uploading-files-vb/_static/image20.png))


Durante il postback, il file caricato può essere salvato nel file System o i dati binari possono essere gestiti direttamente tramite un Stream. Per questo esempio, consentire s di creare un `~/Brochures` cartella e salvare il file caricato. Iniziare aggiungendo il `Brochures` cartella per il sito come sottocartella della directory radice. Successivamente, creare un gestore eventi per il `UploadButton` s `Click` eventi e aggiungere il codice seguente:


[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

Il controllo FileUpload offre un'ampia gamma di proprietà per l'uso con i dati caricati. Ad esempio, il [ `HasFile` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) indica se è stato caricato un file dall'utente, mentre il [ `FileBytes` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) fornisce l'accesso ai dati binari caricati sotto forma di matrice di byte. Il `Click` gestore dell'evento inizia assicurandosi che sia stato caricato un file. Se è stato caricato un file, l'etichetta mostra il nome del file caricato, la dimensione in byte e il relativo tipo di contenuto.

> [!NOTE]
> Per assicurarsi che l'utente carica un file è possibile controllare il `HasFile` proprietà e visualizzare un avviso se si s `False`, oppure è possibile utilizzare il [RequiredFieldValidator (controllo)](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) invece.


Le s FileUpload `SaveAs(filePath)` consente di salvare il file caricato specificato *filePath*. *filePath* deve essere un *percorso fisico* (`C:\Websites\Brochures\SomeFile.pdf`) anziché un *virtuale* *path* (`/Brochures/SomeFile.pdf`). Il [ `Server.MapPath(virtPath)` metodo](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) accetta un percorso virtuale e restituisce il corrispondente percorso fisico. In questo caso, il percorso virtuale fa `~/Brochures/fileName`, dove *fileName* è il nome del file caricato. Visualizzare [tramite server. MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) per altre informazioni su percorsi fisici e virtuali e l'utilizzo di `Server.MapPath`.

Dopo aver completato la `Click` gestore eventi, si consiglia di testare la pagina in un browser. Fare clic sul pulsante Sfoglia e selezionare un file dal disco rigido e quindi fare clic sul pulsante Carica File selezionato. Il contenuto del file selezionato invierà il postback al server web, che verranno quindi visualizzate informazioni sul file prima di salvarla, per il `~/Brochures` cartella. Dopo aver caricato il file, tornare a Visual Studio e fare clic sul pulsante Aggiorna in Esplora soluzioni. Verrà visualizzato il file che appena caricato nella cartella ~/Brochures!


[![È stato caricato il File EvolutionValley.jpg al Server Web](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**Figura 14**: il file ini `EvolutionValley.jpg` è stato caricato nel server Web ([fare clic per visualizzare l'immagine con dimensioni normali](uploading-files-vb/_static/image22.png))


![EvolutionValley.jpg è stato salvato nella cartella ~/Brochures](uploading-files-vb/_static/image15.gif)

**Figura 15**: `EvolutionValley.jpg` è stato salvato il `~/Brochures` cartella


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Sottigliezze al salvataggio dei file caricati nel File System

Esistono diverse sfumature che devono essere affrontati durante il salvataggio di caricamento dei file nel file System s server web. Prima di tutto, qui il problema di sicurezza. Per salvare un file nel file System, il contesto di sicurezza con cui pagina ASP.NET è in esecuzione deve avere autorizzazioni di scrittura. Il Server Web di sviluppo ASP.NET viene eseguito nel contesto dell'account utente corrente. Se si usa s Microsoft Internet Information Services (IIS) come server web, il contesto di sicurezza dipende dalla versione di IIS e la relativa configurazione.

Un'altra sfida di salvataggio dei file nel file System si basa su denominazione dei file. Attualmente, la pagina Salva tutti i file caricati per la `~/Brochures` directory usando lo stesso nome del file nel computer client s. Se l'utente carica una brochure con il nome `Brochure.pdf`, il file verrà salvato come `~/Brochure/Brochure.pdf`. Ma cosa succede se qualche minuto successiva l'utente B carica un file brochure diversa che ha lo stesso nome file (`Brochure.pdf`)? Con il codice è disponibile a questo punto, un file s verrà sovrascritto con caricamento s User B utente.

Esistono diverse tecniche per la risoluzione dei conflitti di nomi di file. È possibile non consentire il caricamento di un file se esiste già uno con lo stesso nome. Con questo approccio, quando l'utente B tenta di caricare un file denominato `Brochure.pdf`, il sistema potrebbe non salva i file e verrà invece visualizzato un messaggio che informa l'utente B per rinominare il file e riprovare. Un altro approccio consiste nel salvare il file usando un nome file univoco, che può essere un' [identificatore univoco globale (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) o il valore dalle corrispondenti colonne chiave primaria s record di database (supponendo che il caricamento è associato un determinata riga nel modello di dati). Nella prossima esercitazione verrà illustrato più dettagliatamente le opzioni.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Problematiche correlate alla con grandi quantità di dati binari

Queste esercitazioni presuppongono che i dati binari acquisiti sono modesti in dimensioni. Uso di grandi quantità di file di dati binari che sono diversi megabyte o superiori introduce nuove sfide che rientrano nell'ambito di queste esercitazioni. Ad esempio, per impostazione predefinita ASP.NET rifiuterà caricamenti di più di 4 MB, anche se ciò può essere configurato tramite il [ `<httpRuntime>` elemento](https://msdn.microsoft.com/library/e1f13641.aspx) in `Web.config`. IIS impone delle limitazioni di dimensione file upload, troppo. Visualizzare [dimensioni File di caricamento IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) per altre informazioni. Inoltre, il tempo impiegato per caricare i file di grandi dimensioni potrebbe superare il valore predefinito 110 secondi in che attesa per una richiesta ASP.NET. Esistono anche problemi di memoria e prestazioni che si verificano quando si lavora con file di grandi dimensioni.

Il controllo FileUpload non è pratico per caricamenti di file di grandi dimensioni. Come il contenuto del file s viene inviato al server, l'utente finale deve attenderà che non richiede una conferma che è in corso il caricamento. Questo non è così tanta un problema quando si lavora con i file più piccoli che possono essere caricati in pochi secondi, ma possono essere un problema quando si lavora con file di dimensioni maggiori che possono richiedere minuti da caricare. Esistono diversi file di terze parti i controlli di caricamento che sono più adatti per la gestione di caricamenti di grandi dimensioni e molti di questi fornitori offrono gli indicatori di stato e ActiveX caricare Gestioni presentano un'esperienza utente più elegante.

Se l'applicazione deve gestire file di grandi dimensioni, è necessario esaminare le sfide attentamente e trovare soluzioni adatte alle proprie esigenze.

## <a name="summary"></a>Riepilogo

Compila un'applicazione che deve acquisire i dati binari introduce una serie di difficoltà. In questa esercitazione vengono esaminate le prime due: decidere dove archiviare i dati binari e consentendo all'utente di caricare i dati binari tramite una pagina web. Failover successive tre esercitazioni, si vedrà come associare i dati caricati con un record nel database, nonché come visualizzare i dati binari insieme ai relativi campi di dati di testo.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per altre informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Utilizzo di tipi di dati di valori di grandi dimensioni](https://msdn.microsoft.com/library/ms178158.aspx)
- [Guide introduttive controllo fileUpload](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [Controllo Server ASP.NET 2.0 FileUpload](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Il lato scuro di caricamenti di File](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette libri e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha collaborato con tecnologie Web di Microsoft dal 1998. Lavora come un consulente, formatore e autore. Il suo ultimo libro si intitola [ *Sams Teach Yourself ASP.NET 2.0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). È possibile contattarlo al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, che è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. I revisori per questa esercitazione sono state Teresa Murphy e Bernadette Leigh. Se si è interessati prossimi articoli MSDN dello? In questo caso, Inviami una riga in corrispondenza [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](updating-and-deleting-existing-binary-data-cs.md)
> [Successivo](displaying-binary-data-in-the-data-web-controls-vb.md)
