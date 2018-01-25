---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: Caricamento di file (VB) | Documenti Microsoft
author: rick-anderson
description: Informazioni su come consentire agli utenti di caricare file binari (ad esempio i documenti di Word o PDF) al sito Web in cui possono essere archiviati nel file system del server...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: 69586ade54a40aabb55dd507731a6c2820774c04
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="uploading-files-vb"></a>Il caricamento di file (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe) o [Scarica il PDF](uploading-files-vb/_static/datatutorial54vb1.pdf)

> Informazioni su come consentire agli utenti di caricare file binari (ad esempio i documenti di Word o PDF) al sito Web in cui possono essere archiviati nel file system del server o nel database.


## <a name="introduction"></a>Introduzione

Tutte le esercitazioni abbiamo esaminata finora ve lavorare esclusivamente con i dati di testo. Tuttavia, molte applicazioni dispongono di modelli di dati che acquisiscono testo e dati binari. Un sito di documenti per online potrebbe consentire agli utenti di caricare un'immagine da associare al proprio profilo. Un sito Web di selezione che consenta agli utenti di caricare i curriculum come un documento Microsoft Word o PDF.

Utilizzo di dati binari aggiunge una nuova serie di sfide. È necessario decidere come dati binari vengano archiviati nell'applicazione. L'interfaccia utilizzata per l'inserimento di nuovi record deve essere aggiornata per consentire all'utente di caricare un file dal computer e per visualizzare o fornire un mezzo per il download di un record s i dati binari associati è necessario eseguire passaggi aggiuntivi. In questa esercitazione e i successivi tre si esamineranno come hurdle queste sfide. Al termine di queste esercitazioni si verrà compilata un'applicazione completamente funzionale che associa un'immagine e brochure PDF a ogni categoria. In questa esercitazione particolare viene esaminata diverse tecniche per l'archiviazione dei dati binari ed esplorare come permettere agli utenti di caricare un file dal computer e sono salvati nel file system s server web.

> [!NOTE]
> Dati binari che fa parte di un modello di dati applicazione s sono talvolta detta un [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), l'acronimo di oggetto binario di grandi dimensioni. In queste esercitazioni ho scelto di utilizzare i dati binari di terminologia, sebbene il termine BLOB è sinonimo.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Passaggio 1: Creazione di lavoro con le pagine Web di dati binari

Prima di iniziare a esplorare i problemi associati all'aggiunta del supporto per i dati binari, consentire s attentamente prima di creare le pagine ASP.NET nel progetto sito Web che è necessario per questa esercitazione e i successivi tre. Per iniziare, aggiungere una nuova cartella denominata `BinaryData`. Successivamente, aggiungere le seguenti pagine ASP.NET a quella cartella, assicurandosi di associare ogni pagina con il `Site.master` pagina master:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![Aggiungere le pagine ASP.NET per le esercitazioni relative ai dati binari](uploading-files-vb/_static/image1.gif)

**Figura 1**: aggiungere le pagine ASP.NET per le esercitazioni relative ai dati binari


Come in altre cartelle, `Default.aspx` nel `BinaryData` cartella elencherà le esercitazioni nella relativa sezione. Tenere presente che il `SectionLevelTutorialListing.ascx` controllo utente fornisce questa funzionalità. Pertanto, aggiungere il controllo utente `Default.aspx` trascinandolo da Esplora soluzioni nella pagina di visualizzazione della struttura s.


[![Aggiungere il controllo utente SectionLevelTutorialListing.ascx Default.aspx](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**Figura 2**: aggiungere il `SectionLevelTutorialListing.ascx` controllo utente in `Default.aspx` ([fare clic per visualizzare l'immagine ingrandita](uploading-files-vb/_static/image2.png))


Infine, aggiungere tali pagine come voci per il `Web.sitemap` file. In particolare, aggiungere il markup seguente dopo l'ottimizzazione GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

Dopo aver aggiornato `Web.sitemap`, dedicare alcuni minuti per visualizzare il sito Web esercitazioni tramite un browser. Il menu a sinistra ora include gli elementi per l'utilizzo con le esercitazioni di dati binari.


![Mappa del sito include ora le voci per l'utilizzo con le esercitazioni di dati binari](uploading-files-vb/_static/image3.gif)

**Figura 3**: mappa del sito include ora le voci per l'utilizzo con le esercitazioni di dati binari


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Passaggio 2: Decidere dove archiviare i dati binari

Dati binari che sono associati al modello di dati applicazione s possono essere archiviati in una delle due posizioni: in un sistema file server s web con un riferimento al file archiviato nel database. o direttamente all'interno del database stesso (vedere la figura 4). Ogni approccio presenta un proprio set di vantaggi e svantaggi e merita una discussione più dettagliata.


[![Dati binari possono essere archiviati nel File System o direttamente nel Database](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**Figura 4**: dati binari possono essere archiviati nel File System o direttamente nel Database ([fare clic per visualizzare l'immagine ingrandita](uploading-files-vb/_static/image4.png))


Si supponga di voler estendere il database Northwind per associare un'immagine a ogni prodotto. Un'opzione, è possibile archiviare questi file di immagine nel file system s server web e registrare il percorso di `Products` tabella. Con questo approccio, d aggiungiamo un `ImagePath` colonna per il `Products` tabella di tipo `varchar(200)`, ad esempio. Quando un utente di caricare un'immagine per Chai, tale immagine è archiviata nel file system s server web in `~/Images/Tea.jpg`, dove `~` rappresenta il percorso fisico dell'applicazione s. Ovvero, se il sito web con radice nel percorso fisico `C:\Websites\Northwind\`, `~/Images/Tea.jpg` sarebbe equivalente al `C:\Websites\Northwind\Images\Tea.jpg`. Dopo aver caricato il file di immagine, d è aggiornare il record Chai nel `Products` tabella in modo che il relativo `ImagePath` colonna a cui fa riferimento il percorso della nuova immagine. È possibile usare `~/Images/Tea.jpg` o semplicemente `Tea.jpg` se si è deciso che sarebbero state posizionate tutte le immagini del prodotto in s applicazione `Images` cartella.

I vantaggi principali di archiviare i dati binari nel file system sono:

- **Facilità di implementazione** come vedremo a breve, archiviare e recuperare dati binari archiviati direttamente all'interno del database richiede un po' più codice rispetto a quando l'utilizzo dei dati tramite il file system. Inoltre, affinché un utente di visualizzare o scaricare dati binari devono essere presentati con un URL a tali dati. Se i dati si trovano nel file system s server web, l'URL è semplice. Se i dati vengono archiviati nel database, tuttavia, una pagina web. deve essere creato che recupererà e restituire i dati dal database.
- **Estendere l'accesso ai dati binari** potrebbe essere necessario accessibili ad altri servizi o applicazioni, quelle che non è possibile estrarre i dati dal database i dati binari. Ad esempio, le immagini associate a ogni prodotto inoltre potrebbe essere necessario rendere disponibili agli utenti tramite [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), nel qual caso d da archiviare i dati binari nel file system.
- **Prestazioni** se i dati binari vengono archiviati nel file system, congestione richiesta e di rete tra il server di database e il server web sarà inferiore se i dati binari vengono archiviati direttamente all'interno del database.

Lo svantaggio principale di archiviare i dati binari nel file system è che separa i dati dal database. Se viene eliminato un record dal `Products` della tabella, i file associato nel file system s server web non viene eliminata automaticamente. È necessario scrivere codice aggiuntivo per eliminare il file o nel file system verrà accumularsi di file orfani, inutilizzati. Inoltre, durante il backup del database, è necessario assicurarsi di eseguire il backup di dati binari associati nel file system, nonché. Spostare il database in un altro sito o server comporta simile problematiche.

In alternativa, dati binari possono essere archiviati direttamente in un database di Microsoft SQL Server 2005 mediante la creazione di una colonna di tipo `varbinary`. Come con altri tipi di dati di lunghezza variabile, è possibile specificare una lunghezza massima dei dati binari che possono essere contenuti in questa colonna. Ad esempio, da riservare al massimo 5.000 byte utilizzare `varbinary(5000)`; `varbinary(MAX)` consente per le dimensioni massime di archiviazione, circa 2 GB.

Il vantaggio principale di archiviazione di dati binari direttamente nel database è la stretta integrazione tra i dati binari e il record del database. Questo semplifica notevolmente l'attività di amministrazione di database, ad esempio backup o lo spostamento del database in un server o un sito diverso. Inoltre, un record automaticamente se si elimina i dati binari corrispondenti. Esistono inoltre altre meno evidenti vantaggi offerti dall'archiviazione i dati binari nel database. Vedere [archiviazione binario file direttamente nel Database utilizzando ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) per una discussione più dettagliata.

> [!NOTE]
> In Microsoft SQL Server 2000 e versioni precedenti, il `varbinary` del tipo di dati sono un limite massimo di 8.000 byte. Per archiviare fino a 2 GB di dati binari di [ `image` tipo di dati](https://msdn.microsoft.com/library/ms187993.aspx) deve essere utilizzato. Con l'aggiunta di `MAX` in SQL Server 2005, tuttavia, il `image` tipo di dati è stato deprecato. È s ancora supportata per la compatibilità, ma Microsoft ha annunciato che la `image` il tipo di dati verrà rimossa in una versione futura di SQL Server.


Se si sta usando un modello di dati precedente venga visualizzato il `image` tipo di dati. Il database Northwind s `Categories` tabella include un `Picture` colonna che può essere utilizzato per archiviare i dati binari di un file di immagine per la categoria. Poiché il database Northwind ha radici in Microsoft Access e le versioni precedenti di SQL Server, questa colonna è di tipo `image`.

Per questa esercitazione e i successivi tre, useremo entrambi gli approcci. Il `Categories` tabella dispone già di un `Picture` colonna per l'archiviazione del contenuto binario di un'immagine per la categoria. Verrà aggiunto una colonna aggiuntiva, `BrochurePath`, per archiviare il percorso di un file PDF nel file system che può essere usato per fornire una panoramica di qualità di stampa, elegante della categoria di web server s.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Passaggio 3: Aggiunta di`BrochurePath`colonna per il`Categories`tabella

Attualmente la tabella Categories presenta solo quattro colonne: `CategoryID`, `CategoryName`, `Description`, e `Picture`. Oltre a questi campi, è necessario aggiungere una nuova istanza che farà riferimento a brochure categoria s (se presente). Per aggiungere questa colonna, è possibile tornare a Esplora Server, il drill-down nelle tabelle, fare clic su di `Categories` tabella e scegliere Apri definizione tabella (vedere Figura 5). Se non è possibile visualizzare Esplora Server, viene visualizzata selezionando l'opzione di Esplora Server dal menu Visualizza o premere Ctrl + Alt + S.

Aggiungere un nuovo `varchar(200)` colonna per il `Categories` tabella denominata `BrochurePath` ed `NULL` s e fare clic sull'icona Salva (o premere Ctrl + S).


[![Aggiungere una colonna BrochurePath alla tabella Categories](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**Figura 5**: aggiungere un `BrochurePath` colonna per il `Categories` tabella ([fare clic per visualizzare l'immagine ingrandita](uploading-files-vb/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Passaggio 4: Aggiornamento dell'architettura da usare il`Picture`e`BrochurePath`colonne

Il `CategoriesDataTable` nel livello accesso dati (DAL) sono attualmente disponibili quattro `DataColumn` definiti: `CategoryID`, `CategoryName`, `Description`, e `NumberOfProducts`. Quando è stato progettato inizialmente questa DataTable nel [la creazione di un livello di accesso ai dati](../introduction/creating-a-data-access-layer-vb.md) esercitazione il `CategoriesDataTable` erano disponibili solo le prime tre colonne; il `NumberOfProducts` colonna è stato aggiunto nel [Master-Details mediante un puntato Elenco di record Master con un controllo DataList dettagli](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) esercitazione.

Come descritto in *la creazione di un livello di accesso ai dati*, DataTable nel DataSet tipizzato costituiscono gli oggetti business. Gli oggetti TableAdapter sono responsabili per la comunicazione con il database e agli oggetti business con i risultati della query. Il `CategoriesDataTable` è popolato dal `CategoriesTableAdapter`, che dispone di tre metodi di recupero di dati:

- `GetCategories()`esegue la query principale s TableAdapter e restituisce il `CategoryID`, `CategoryName`, e `Description` campi di tutti i record di `Categories` tabella. La query principale viene utilizzato per la generazione automatica `Insert` e `Update` metodi.
- `GetCategoryByCategoryID(categoryID)`Restituisce il `CategoryID`, `CategoryName`, e `Description` campi della categoria di cui `CategoryID` è uguale a *categoryID*.
- `GetCategoriesAndNumberOfProducts()`-Restituisce il `CategoryID`, `CategoryName`, e `Description` campi per tutti i record di `Categories` tabella. Utilizza inoltre una sottoquery per restituire il numero di prodotti associati a ogni categoria.

Si noti che nessuna di queste query restituiscono il `Categories` tabella s `Picture` o `BrochurePath` colonne; e non il `CategoriesDataTable` fornire `DataColumn` s per questi campi. Per poter funzionare con l'immagine e `BrochurePath` proprietà, è necessario prima aggiungerli al `CategoriesDataTable` e quindi aggiornare la `CategoriesTableAdapter` classe per restituire le colonne.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Aggiunta di`Picture`e`BrochurePath``DataColumn` s

Per iniziare, aggiungere queste due colonne per la `CategoriesDataTable`. Fare clic su di `CategoriesDataTable` intestazione s, selezionare Aggiungi dal menu di scelta rapida e quindi scegliere l'opzione di colonna. Verrà creato un nuovo `DataColumn` nel DataTable denominata `Column1`. Rinominare questa colonna come `Picture`. La finestra Proprietà impostare il `DataColumn` s `DataType` proprietà `System.Byte[]` (non è un'opzione nell'elenco a discesa, è necessario digitarla nella).


[![Creare un'immagine denominata DataColumn il cui tipo di dati è System. Byte [](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**Figura 6**: creare un `DataColumn` denominato `Picture` cui `DataType` è `System.Byte[]` ([fare clic per visualizzare l'immagine ingrandita](uploading-files-vb/_static/image8.png))


Aggiungere un altro `DataColumn` nella DataTable, denominarla `BrochurePath` utilizzando il valore predefinito `DataType` valore (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Restituzione di`Picture`e`BrochurePath`valori dal TableAdapter

Con queste due `DataColumn` aggiunta al `CategoriesDataTable`, è nuovamente pronto per l'aggiornamento di `CategoriesTableAdapter`. È possibile avere entrambi questi valori di colonna restituiti nella query TableAdapter principale, ma in questo modo viene nuovamente i dati binari ogni volta il `GetCategories()` chiamata del metodo. Aggiornare la query TableAdapter principale per riportare invece consentono s `BrochurePath` e creare un metodo di recupero di dati aggiuntivi che restituisce una particolare categoria s `Picture` colonna.

Per aggiornare la query TableAdapter principale, fare clic su di `CategoriesTableAdapter` intestazione s e scegliere l'opzione di configurazione dal menu di scelta rapida. Verrà visualizzata la configurazione guidata adattatore tabella cui è visualizzata in una serie di esercitazioni precedenti ve. Aggiornare la query per riportare il `BrochurePath` e fare clic su Fine.


[![Aggiornare l'elenco di colonne nell'istruzione SELECT per restituire i BrochurePath](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**Figura 7**: aggiornare l'elenco colonne nel `SELECT` istruzione per restituire i `BrochurePath` ([fare clic per visualizzare l'immagine ingrandita](uploading-files-vb/_static/image10.png))


Quando si Usa istruzioni SQL ad hoc per l'oggetto TableAdapter, l'aggiornamento dell'elenco di colonne della query principale aggiorna l'elenco di colonne per tutte le `SELECT` metodi nell'oggetto TableAdapter di query. Ciò significa che il `GetCategoryByCategoryID(categoryID)` metodo è stato aggiornato per restituire il `BrochurePath` colonna, che potrebbe essere è voluto. Tuttavia, anche aggiornato l'elenco colonne nel `GetCategoriesAndNumberOfProducts()` (metodo), rimuovere la sottoquery che restituisce il numero di prodotti per ogni categoria! Pertanto, è necessario aggiornare questo metodo s `SELECT` query. Fare clic su di `GetCategoriesAndNumberOfProducts()` (metodo), scegliere Configura e ripristinare il `SELECT` query al valore originale:


[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

Successivamente, creare un nuovo metodo di TableAdapter che restituisce una particolare categoria s `Picture` valore della colonna. Fare clic su di `CategoriesTableAdapter` intestazione s e scegliere l'opzione Aggiungi Query per avviare la configurazione guidata Query TableAdapter. Il primo passaggio della procedura guidata viene chiesto di specificare che se si desidera eseguire query sui dati utilizzando un'istruzione SQL ad hoc, una nuova stored procedure o una esistente. Selezionare Usa istruzioni SQL e fare clic su Avanti. Poiché si restituisce una riga, scegliere l'istruzione SELECT che restituisce l'opzione di righe del secondo passaggio.


[![Selezionare l'Usa istruzioni SQL opzione](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**Figura 8**: selezionare l'Usa istruzioni SQL opzione ([fare clic per visualizzare l'immagine ingrandita](uploading-files-vb/_static/image12.png))


[![Poiché la Query restituirà un Record dalla tabella Categories, scegliere SELECT che restituisce righe](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**Figura 9**: poiché la Query restituirà un Record dalla tabella Categories, scegliere Seleziona che restituisce righe ([fare clic per visualizzare l'immagine ingrandita](uploading-files-vb/_static/image14.png))


Nel terzo passaggio, immettere la seguente query SQL e fare clic su Avanti:


[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

L'ultimo passaggio consiste nello scegliere il nome per il nuovo metodo. Utilizzare `FillCategoryWithBinaryDataByCategoryID` e `GetCategoryWithBinaryDataByCategoryID` per il riempimento del DataTable e restituire un oggetto DataTable modelli, rispettivamente. Fare clic su Fine per completare la procedura guidata.


[![Scegliere i nomi per i metodi s TableAdapter](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**Figura 10**: scegliere i nomi per i metodi di TableAdapter ([fare clic per visualizzare l'immagine ingrandita](uploading-files-vb/_static/image16.png))


> [!NOTE]
> Dopo aver completato la configurazione guidata Query di tabella Adapter venga visualizzato una finestra di dialogo che informa che il nuovo testo del comando restituisce i dati con uno schema diverso da quello della query principale. In breve, la procedura guidata è notare che la query principale di TableAdapter s `GetCategories()` restituisce uno schema diverso da quello appena creato. Ma questo è ciò che si vuole, pertanto è possibile ignorare questo messaggio.


Inoltre, tenere presente che se si utilizzano istruzioni SQL ad hoc e utilizzare la procedura guidata per modificare query TableAdapter s principale in un momento successivo nel tempo, verrà modificato il `GetCategoryWithBinaryDataByCategoryID` metodo s `SELECT` elenco di colonne istruzione s per includere solo le colonne il query principale (ovvero, verrà rimosso il `Picture` colonna dalla query). È necessario aggiornare manualmente l'elenco di colonne per restituire il `Picture` colonne, analogamente a quanto avveniva con il `GetCategoriesAndNumberOfProducts()` metodo più indietro in questo passaggio.

Dopo aver aggiunto i due `DataColumn` s per il `CategoriesDataTable` e `GetCategoryWithBinaryDataByCategoryID` metodo il `CategoriesTableAdapter`, queste classi in Progettazione DataSet tipizzati dovrebbero essere simile alla schermata illustrata nella figura 11.


![La finestra di progettazione del set di dati include le nuove colonne (metodo)](uploading-files-vb/_static/image11.gif)

**Figura 11**: la finestra di progettazione del set di dati include le nuove colonne (metodo)


## <a name="updating-the-business-logic-layer-bll"></a>Aggiornare il livello di logica di Business (BLL)

Con DAL aggiornato, è comunque per aumentare il livello Business (LOGIC) per includere un metodo per il nuovo `CategoriesTableAdapter` metodo. Aggiungere il metodo seguente per la `CategoriesBLL` classe:


[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Passaggio 5: Caricare un File dal Client al Server Web

Quando si raccolgono dati binari, spesso questi dati viene forniti da un utente finale. Per acquisire queste informazioni, l'utente deve essere in grado di caricare un file dal proprio computer al server web. I dati caricati devono quindi essere integrata con il modello di dati, che potrebbe impedire il salvataggio del file al file system s del server web e aggiunta di un percorso del file nel database o la scrittura del contenuto binario direttamente nel database. In questo passaggio verrà esaminato come consentire all'utente di caricare file dal proprio computer al server. Nella prossima esercitazione si verranno prese in esame l'integrazione del file caricato con modello di dati.

ASP.NET 2.0 s nuovo [controllo Web FileUpload](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) fornisce un meccanismo per gli utenti di inviare un file dal proprio computer al server web. Esegue il rendering del controllo FileUpload come un `<input>` elemento il cui `type` attributo è impostato su file, nei browser visualizzato come una casella di testo con un pulsante Sfoglia. Fare clic sul pulsante Sfoglia viene visualizzata una finestra di dialogo da cui l'utente può selezionare un file. Quando il form viene inviato nuovamente, il contenuto di file selezionato s viene inviato insieme il postback. Sul lato server, informazioni sul file caricato sono accessibile tramite la proprietà controllo FileUpload.

Per illustrare il caricamento di file, aprire il `FileUpload.aspx` nella pagina di `BinaryData` cartella, trascinare un controllo FileUpload dalla casella degli strumenti nella finestra di progettazione e impostare il controllo s `ID` proprietà `UploadTest`. Successivamente, aggiungere un'impostazione di controllo Web Button relativo `ID` e `Text` proprietà `UploadButton` e caricare il File selezionato, rispettivamente. Infine, inserire un controllo etichetta Web sotto il pulsante, cancellare il contenuto relativo `Text` proprietà e impostare il relativo `ID` proprietà `UploadDetails`.


[![Aggiungere un controllo FileUpload alla pagina ASP.NET](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**Figura 12**: aggiungere un controllo FileUpload alla pagina ASP.NET ([fare clic per visualizzare l'immagine ingrandita](uploading-files-vb/_static/image18.png))


Figura 13 Mostra questa pagina quando viene visualizzato tramite un browser. Si noti che facendo clic sul pulsante Sfoglia viene visualizzata una finestra di dialogo di selezione di file, consentendo all'utente di selezionare un file dal computer. Dopo aver selezionato un file, fare clic sul pulsante Carica File selezionato provoca un postback che invia il contenuto binario s file selezionato al server web.


[![L'utente può selezionare un File da caricare dal proprio Computer al Server](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**Figura 13**: l'utente può selezionare un File da caricare dal proprio Computer al Server ([fare clic per visualizzare l'immagine ingrandita](uploading-files-vb/_static/image20.png))


Durante il postback, il file caricato può essere salvato nel file System o i dati binari possono essere gestiti direttamente tramite un flusso. Per questo esempio, consentire s di creare un `~/Brochures` cartella e salvare il file caricato. Per iniziare, aggiungere il `Brochures` cartella per il sito come sottocartella della directory radice. Successivamente, creare un gestore eventi per il `UploadButton` s `Click` eventi e aggiungere il codice seguente:


[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

Il controllo FileUpload fornisce una serie di proprietà per l'utilizzo con i dati caricati. Ad esempio, il [ `HasFile` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) indica se un file è stato caricato dall'utente, mentre il [ `FileBytes` proprietà](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) fornisce l'accesso ai dati binari caricati come una matrice di byte. Il `Click` gestore eventi viene avviato, verificare che sia stato caricato un file. Se è stato caricato un file, l'etichetta Visualizza il nome del file caricato, la dimensione in byte e il relativo tipo di contenuto.

> [!NOTE]
> Per garantire che l'utente carica un file è possibile controllare il `HasFile` proprietà e visualizzare un avviso se si s `False`, oppure è possibile utilizzare il [controllo RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) invece.


FileUpload s `SaveAs(filePath)` Salva il file caricato specificato *filePath*. *filePath* deve essere un *percorso fisico* (`C:\Websites\Brochures\SomeFile.pdf`) anziché un *virtuale* *percorso* (`/Brochures/SomeFile.pdf`). Il [ `Server.MapPath(virtPath)` metodo](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) accetta un percorso virtuale e restituisce il percorso fisico corrispondente. In questo caso, il percorso virtuale è `~/Brochures/fileName`, dove *fileName* è il nome del file caricato. Vedere [utilizzando MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) per ulteriori informazioni su percorsi fisici e virtuali e l'uso `Server.MapPath`.

Dopo aver completato il `Click` gestore eventi, è opportuno testare la pagina in un browser. Fare clic sul pulsante Sfoglia e selezionare un file dal disco rigido e quindi fare clic sul pulsante Carica File selezionato. Il contenuto del file selezionato invierà il postback al server web, che verranno quindi visualizzate le informazioni sul file prima di salvarla per la `~/Brochures` cartella. Dopo aver caricato il file, tornare a Visual Studio e fare clic sul pulsante di aggiornamento in Esplora soluzioni. Si dovrebbe essere visualizzato il file che appena caricato nella cartella ~/Brochures.


[![EvolutionValley.jpg il File è stato caricato per il Server Web](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**Nella figura 14**: il File `EvolutionValley.jpg` è stato caricato per il Server Web ([fare clic per visualizzare l'immagine ingrandita](uploading-files-vb/_static/image22.png))


![EvolutionValley.jpg è stato salvato nella cartella ~/Brochures](uploading-files-vb/_static/image15.gif)

**Figura 15**: `EvolutionValley.jpg` è stato salvato il `~/Brochures` cartella


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Sfumature con il salvataggio di file caricati nel File System

Esistono diverse sfumature che devono essere risolti durante il salvataggio di caricamento dei file nel file System di web server s. Innanzitutto, esiste il problema di sicurezza. Per salvare un file nel file System, il contesto di sicurezza in cui è in esecuzione la pagina ASP.NET deve disporre delle autorizzazioni di scrittura. Il Server Web di sviluppo ASP.NET viene eseguito nel contesto dell'account utente corrente. Se si utilizza s Microsoft Internet Information Services (IIS) come server web, il contesto di sicurezza dipende dalla versione di IIS e la relativa configurazione.

Un'altra richiesta di salvataggio dei file nel file System si basa su denominazione dei file. Attualmente, la pagina Salva tutti i file caricati per la `~/Brochures` directory utilizzando lo stesso nome del file nel computer client s. Se l'utente carica una brochure con il nome `Brochure.pdf`, il file verrà salvato come `~/Brochure/Brochure.pdf`. Ma cosa accade se un intervallo di tempo successivo dell'utente B carica un file di brochure diversi che possono avere lo stesso nome (`Brochure.pdf`)? Con il codice è disponibile a questo punto, utente di un file s verrà sovrascritto con caricamento s utente B.

Esistono diverse tecniche per risolvere i conflitti di nome file. È possibile non consentire il caricamento di un file se esiste già uno con lo stesso nome. Con questo approccio, quando l'utente B tenta di caricare un file denominato `Brochure.pdf`, il sistema non salvare i file e invece visualizzato un messaggio che informa dell'utente B per rinominare il file e riprovare. Un altro approccio consiste nel salvare il file utilizzando un nome file univoco, che può essere un [identificatore univoco globale (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) o il valore dal corrispondente colonne chiave primaria s record del database (supponendo che il caricamento è associato a un riga specifica nel modello di dati). Nella prossima esercitazione verrà illustrato più dettagliatamente le opzioni.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Problemi relativi a grandi quantità di dati binari

Queste esercitazioni si presuppongono che i dati binari acquisiti siano adeguati nella dimensione. Utilizzano grandi quantità di file di dati binari che sono diversi megabyte o maggiori implicazioni che esulano dall'ambito di queste esercitazioni. Ad esempio, per impostazione predefinita ASP.NET rifiuterà caricamenti di più di 4 MB, anche se ciò può essere configurato tramite il [ `<httpRuntime>` elemento](https://msdn.microsoft.com/library/e1f13641.aspx) in `Web.config`. IIS impone delle limitazioni di dimensioni di caricamento file, troppo. Vedere [dimensioni File di caricamento IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) per ulteriori informazioni. Inoltre, il tempo impiegato per caricare file di grandi dimensioni potrebbe superare il valore predefinito 110 secondi che rimarrà in attesa di una richiesta ASP.NET. Sono disponibili anche i problemi di memoria e delle prestazioni che si verificano quando si lavora con file di grandi dimensioni.

Il controllo FileUpload risulta poco pratico per i caricamenti di file di grandi dimensioni. Come il contenuto del file s viene inviato al server, l'utente finale deve attendere pazientemente senza alcuna conferma che il caricamento è in corso. Questo non è così tanto un problema quando si lavora con i file più piccoli che possono essere caricati in pochi secondi, ma possono essere un problema quando si lavora con i file di dimensioni maggiori che potrebbero richiedere minuti da caricare. Sono disponibili i controlli di caricamento che sono più adatti per la gestione di grandi dimensioni caricamenti un'ampia gamma di file di terze parti e molti di questi fornitori forniscono gli indicatori di stato e ActiveX carica i gestori da presentano un'esperienza utente più elegante.

Se l'applicazione deve gestire i file di grandi dimensioni, è necessario esaminare i problemi e trovare soluzioni adatte alle proprie esigenze con attenzione.

## <a name="summary"></a>Riepilogo

Compilazione di un'applicazione che è necessario acquisire i dati binari introduce una serie di difficoltà. In questa esercitazione, è esplorare le prime due: decidere dove archiviare i dati binari e consentendo all'utente di caricare il contenuto binario mediante una pagina web. Tramite le tre esercitazioni, vedremo come associare i dati caricati con un record nel database, nonché come visualizzare i dati binari insieme ai relativi campi di dati di testo.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [Utilizzo di tipi di dati di valori di grandi dimensioni](https://msdn.microsoft.com/library/ms178158.aspx)
- [Guida introduttiva di controllo fileUpload](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [Controllo Server ASP.NET 2.0 FileUpload](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Il lato scuro di caricamenti di File](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Teresa Murphy e Bernadette Leigh. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Precedente](updating-and-deleting-existing-binary-data-cs.md)
[Successivo](displaying-binary-data-in-the-data-web-controls-vb.md)
