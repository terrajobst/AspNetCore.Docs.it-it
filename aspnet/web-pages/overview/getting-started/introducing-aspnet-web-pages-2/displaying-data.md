---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Introduzione a ASP.NET Web Pages - visualizzazione di dati | Documenti Microsoft
author: tfitzmac
description: In questa esercitazione viene illustrato come creare un database in WebMatrix e come visualizzare i dati del database in una pagina quando si utilizzano pagine Web ASP.NET (Razor). Si presuppone di y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 6c66e5fb0a1a49da411286e19c7954f83055c3fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>Introduzione a ASP.NET Web Pages - visualizzazione di dati
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questa esercitazione viene illustrato come creare un database in WebMatrix e come visualizzare i dati del database in una pagina quando si utilizzano pagine Web ASP.NET (Razor). Si presuppone di aver completato la serie tramite [Introduzione alla programmazione di pagine Web ASP.NET](../introducing-razor-syntax-c.md).
> 
> Illustra quanto segue:
> 
> - Come utilizzare gli strumenti di WebMatrix per creare un database e tabelle di database.
> - Come utilizzare gli strumenti di WebMatrix per aggiungere dati a un database.
> - Come visualizzare i dati dal database in una pagina.
> - Come eseguire comandi SQL in ASP.NET Web Pages.
> - Come personalizzare il `WebGrid` helper per modificare la visualizzazione dei dati e aggiungere l'ordinamento e paging.
>   
> 
> Funzionalità/tecnologie descritte:
> 
> - Strumenti di database di WebMatrix.
> - `WebGrid` helper.


## <a name="what-youll-build"></a>Cosa Compilerai

Nell'esercitazione precedente, sono stati introdotti per ASP.NET Web Pages (*. cshtml* file), per le nozioni di base della sintassi Razor e per gli helper. In questa esercitazione, innanzitutto creare l'applicazione web che verrà utilizzato per il resto della serie. L'applicazione è un'applicazione semplice filmato che consente di visualizzare, aggiungere, modificare ed eliminare le informazioni sui film.

Una volta terminato con questa esercitazione, sarà in grado di visualizzare un elenco di film simile a questa pagina:

![Visualizzazione WebGrid che include i parametri impostati per i nomi delle classi CSS](displaying-data/_static/image1.png)

Tuttavia, per iniziare, è necessario creare un database.

## <a name="a-very-brief-introduction-to-databases"></a>Una breve introduzione ai database

In questa esercitazione verrà fornite solo briefest Introduzione ai database. Se si ha esperienza di database, è possibile ignorare questa sezione breve.

Un database contiene uno o più tabelle che contengono informazioni &mdash; tabelle, ad esempio, per i clienti, ordini e fornitori o per studenti, docenti, classi e livelli. Livello di struttura, una tabella di database è un foglio di lavoro. Si supponga una tipico di Rubrica. Per ogni voce della Rubrica (ovvero, per ogni persona) si dispone di diverse parti di informazioni quali nome, cognome, indirizzo, indirizzo di posta elettronica e numero di telefono.

![Tabella di database di esempio come griglia semplice](displaying-data/_static/image2.png)

(Righe vengono dette *record*, e le colonne sono dette *campi*.)

Per la maggior parte delle tabelle di database, la tabella deve disporre di una colonna che contiene un valore univoco, ad esempio un numero cliente, il numero di conto e così via. Questo valore è noto come la tabella *chiave primaria*, e usarlo per identificare ogni riga nella tabella. Nell'esempio, la colonna ID è la chiave primaria per la Rubrica illustrata nell'esempio precedente.

Gran parte delle operazioni eseguite nelle applicazioni web è costituito da informazioni dal database di lettura e visualizzarli in una pagina. È inoltre spesso sarà raccogliere informazioni dagli utenti e aggiungerlo a un database o che si desidera modificare i record che sono già presenti nel database. (Tratteremo tutte queste operazioni nel corso di questa esercitazione set.)

Operazioni di database possono essere particolarmente complesso e possono richiedere conoscenze specializzate. Per set di questa esercitazione, tuttavia, è necessario comprendere solo concetti di base, che verranno tutti spiegati mentre Prosegui.

## <a name="creating-a-database"></a>Creazione di un Database

WebMatrix include strumenti che semplificano la creazione di un database e per creare tabelle nel database. (La struttura di un database viene definita il database *schema*.) Per set di questa esercitazione, si creerà un database che contiene una sola tabella &mdash; film.

Se non è già fatto, aprire WebMatrix e aprire il sito WebPagesMovies creato nell'esercitazione precedente.

Nel riquadro a sinistra, fare clic su di **Database** dell'area di lavoro.

![Scheda di WebMatrix Database dell'area di lavoro](displaying-data/_static/image3.png)

La barra multifunzione cambia per mostrare le attività correlate ai database. Nella barra multifunzione, fare clic su **Nuovo Database**.

![Pulsante "Nuovo Database" nella barra multifunzione di WebMatrix](displaying-data/_static/image4.png)

WebMatrix consente di creare un database di SQL Server CE (un *sdf* file) che ha lo stesso nome del sito &mdash; *WebPagesMovies.sdf*. (Si non esegue questa operazione qui, ma è possibile rinominare il file su qualsiasi elemento che si desidera, purché abbia un *sdf* estensione.)

## <a name="creating-a-table"></a>Creazione di una tabella

Nella barra multifunzione, fare clic su **nuova tabella**. WebMatrix consente di aprire Progettazione tabelle in una nuova scheda. (Se l'opzione nuova tabella non è disponibile, assicurarsi che il nuovo database sia selezionato nella visualizzazione albero a sinistra.)

!['Nella tabella nuovo' pulsante nella barra multifunzione di WebMatrix](displaying-data/_static/image5.png)

Nella casella di testo nella parte superiore (in cui il limite è indicato "Immettere il nome di tabella"), immettere "Filmati".

![Immettere un nome di tabella nella finestra di progettazione di database di WebMatrix](displaying-data/_static/image6.png)

Riquadro di sotto il nome della tabella viene usata per definire singole colonne. Per il *filmati* tabella in questa esercitazione verrà creato solo alcune colonne: *ID*, *titolo*, *Genre*, e *anno*.

Nel **nome** , immettere "ID". Immettere un valore attiva tutti i controlli per la nuova colonna.

Premere TAB per passare il **tipo di dati** elenco e scegliere **int**. Questo valore specifica che la colonna ID conterrà dati integer (numero).

> [!NOTE]
> È non chiamarlo un dettaglio di seguito (molto), ma è possibile utilizzare i movimenti di tastiera Windows standard per spostarsi all'interno di questa griglia. Ad esempio, può spostarsi tra i campi, è solo possibile iniziare a digitare per selezionare un elemento in un elenco e così via.


Scheda oltre il **Default Value** casella (ossia, lasciare vuoto il campo). Premere TAB per passare il **chiave primaria è** casella di controllo e selezionarlo. Questa opzione indica il database di *ID* colonna conterrà i dati che identifica singole righe. (Ovvero, ogni riga avrà un valore univoco nella colonna ID che è possibile utilizzare per trovare tale riga.)

Scegliere il **identità** opzione. Questa opzione indica il database che dovrà generare automaticamente il successivo numero sequenziale per ogni nuova riga. (Il **identità** opzione funziona solo se è selezionata anche il **chiave primaria è** opzione.)

Fare clic sulla riga successiva della griglia, o premere il tasto Tab due volte per terminare la riga corrente. L'azione consente di salvare la riga corrente e avvia quello successivo. Si noti che il **Default Value** colonna viene visualizzato ora **Null**. (Il valore predefinito è null per il valore predefinito, pertanto per dire.)

Al termine della definizione di nuovo **ID** colonna, la finestra di progettazione sarà simile a questa illustrazione:

![Progettazione database WebMatrix dopo la definizione della colonna di ID per la tabella di filmati](displaying-data/_static/image7.png)

Per creare la colonna successiva, fare clic nella casella di **nome** colonna. Immettere "Title" per la colonna e quindi selezionare **nvarchar** per il **tipo di dati** valore. La parte "var" **nvarchar** indica il database che i dati per questa colonna sarà una stringa il cui dimensioni possono variare da un record a altro. (Il prefisso "n" rappresenta "national", che indica che il campo può contenere qualsiasi carattere alfabetico o sistema di scrittura dati di tipo carattere, vale a dire, il campo contiene i dati Unicode.)

Quando si sceglie **nvarchar**, un'altra casella viene visualizzata in cui è possibile immettere la lunghezza massima per il campo. Immettere 50, presupponendo che nessun titolo del film che verranno utilizzate in questa esercitazione sarà più di 50 caratteri.

Ignora **Default Value** e deselezionare il **Ammetti** opzione. Per evitare che il database per consentire qualsiasi filmati essere immessi nel database che non dispongono di un titolo.

Quando è terminato e passare alla riga successiva, la finestra di progettazione simile a questa illustrazione:

![Progettazione database WebMatrix dopo la definizione di colonna del titolo per la tabella di filmati](displaying-data/_static/image8.png)

Ripetere questi passaggi per creare una colonna denominata "Genre", ad eccezione di lunghezza, impostarla su appena 30.

Creare un'altra colonna denominata "Year". Per il tipo di dati, scegliere **nchar** (non **nvarchar**) e impostare la lunghezza su 4. Per l'anno, che si intende utilizzare un numero di 4 cifre come "1995" o "2010", in modo che non sia richiesto una colonna a lunghezza variabile.

Ecco come appare la progettazione terminata:

![Progettazione di database WebMatrix dopo tutti i campi definiti per la tabella di filmati](displaying-data/_static/image9.png)

Premere Ctrl + S oppure scegliere di **salvare** sulla barra degli strumenti accesso rapido. Chiudere la finestra di progettazione di database, chiudere la scheda.

## <a name="adding-some-example-data"></a>Aggiunta di alcuni dati di esempio

Più avanti in questa serie di esercitazioni si creerà una pagina in cui è possibile immettere nuovi film in un form. Per il momento, tuttavia, è possibile aggiungere alcuni dati di esempio che è quindi possibile visualizzare in una pagina.

Nel **Database** area di lavoro di WebMatrix, si noti che sono una struttura ad albero che mostra il *sdf* file creato in precedenza. Aprire il nodo per il nuovo *sdf* file e quindi aprire il **tabelle** nodo.

![Area di lavoro di WebMatrix Database con struttura ad albero aprire alla tabella di filmati](displaying-data/_static/image10.png)

Fare doppio clic su di **filmati** nodo e quindi scegliere **dati**. WebMatrix verrà visualizzata una griglia in cui è possibile immettere dati per il *filmati* tabella:

![Griglia di voce di database in WebMatrix (vuoto)](displaying-data/_static/image11.png)

Fare clic su di **titolo** colonna e immettere "Quando Harry soddisfatti Sally". Spostare il **Genre** colonna (è possibile utilizzare il tasto Tab) e immettere "Romantico Commedia". Spostare il **anno** colonna e immettere "1989":

![Griglia di voce di database in WebMatrix con un record](displaying-data/_static/image12.png)

Premere INVIO e WebMatrix Salva il nuovo film. Si noti che il **ID** colonna è stata compilata.

![Griglia di voce di database in WebMatrix con un record e l'ID generato automaticamente](displaying-data/_static/image13.png)

Immettere un altro filmato (ad esempio, "effettuato con il vento", "Serie", "1939"). La colonna ID viene inserita nuovamente:

![Griglia di voce di database in WebMatrix con due record e l'ID generato automaticamente](displaying-data/_static/image14.png)

Immettere un terzo filmato (ad esempio, "Ghostbusters", "Commedia"). Per un esperimento, lasciare il **anno** colonna vuota e quindi premere INVIO. Poiché è stata deselezionata l'opzione di **Ammetti** opzione, il database viene visualizzato un errore:

!['Data non valida' errore visualizzato se un valore di colonna richiesta è vuoto.](displaying-data/_static/image15.png)

Fare clic su **OK** per tornare indietro e correggere la voce (l'anno per "Ghostbusters" è 1984) e quindi premere INVIO.

Compilare filmati diversi fino a quando non si dispone di 8 o in modo. (Immettere 8 rende più facile lavorare con paging in un secondo momento. Ma se è un numero eccessivo, immettere solo alcune per ora). I dati effettivi non ha importanza.

![Griglia di voce di database in WebMatrix con entrambi i record](displaying-data/_static/image16.png)

Se è stato immesso tutti i film senza errori, i valori di ID sono sequenziali. Se si tenta di salvare un record incompleti film, i numeri di ID potrebbero non essere sequenziali. In questo caso, che è accettabile. L'unico elemento importante è che questi sistemi univoci in e i numeri non hanno alcun significato intrinseco di *filmati* tabella.

Chiudere la scheda che contiene la finestra di progettazione di database.

Ora è possibile attivare la visualizzazione di dati in una pagina web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Visualizzazione dei dati in una pagina con il supporto WebGrid

Per visualizzare i dati in una pagina, che si intende utilizzare il `WebGrid` helper. Questo supporto produce una visualizzazione in una griglia o una tabella (righe e colonne). Come si vedrà, sarà in grado di definire la griglia con formattazione e altre funzionalità.

Per eseguire la griglia, sarà necessario scrivere poche righe di codice. Alcune di queste righe fungerà da un tipo di modello per la maggior parte dell'accesso ai dati di questa esercitazione.

> [!NOTE]
> In realtà offrono diverse opzioni per la visualizzazione dei dati in una pagina. il `WebGrid` helper è solo una. Abbiamo scelto, per questa esercitazione perché è il modo più semplice per visualizzare i dati e, in quanto è abbastanza flessibile. Nel set di esercitazione successivo, si noterà come utilizzare un altro modo "manual" per funzionare con i dati nella pagina, che fornisce un controllo più diretto su come visualizzare i dati.


Nel riquadro a sinistra in WebMatrix, fare clic su di **file** dell'area di lavoro.

Il nuovo database creato è il *App\_dati* cartella. Se la cartella non esiste già, WebMatrix creato per il nuovo database. (La cartella potrebbe esistito se è stato installato in precedenza helper.)

Nella visualizzazione albero, selezionare la radice del sito Web. È necessario selezionare la radice del sito Web; in caso contrario, il nuovo file potrebbe essere aggiunti all'App\_cartella dati.

Nella barra multifunzione, fare clic su **New**. Nel **scegliere un tipo di File** scegliere **CSHTML**.

Nel **nome** casella, denominare la nuova pagina "Movies.cshtml":

![La finestra di dialogo ', scegliere un tipo di File' che illustra la pagina 'Film'](displaying-data/_static/image17.png)

Fare clic su di **OK** pulsante. WebMatrix consente di aprire un nuovo file con alcuni elementi della struttura in essa contenuti. Innanzitutto verrà scritto il codice per passare a ottenere i dati dal database. Si aggiungeranno quindi markup alla pagina per visualizzare i dati.

### <a name="writing-the-data-query-code"></a>Scrittura del codice di Query di dati

Nella parte superiore della pagina, tra il `@{` e `}` caratteri, immettere il codice seguente. (Assicurarsi immettere questo codice tra parentesi graffe di chiusura di apertura e).

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

La prima riga consente di aprire il database creato in precedenza, che è sempre il primo passaggio prima di procedere con il database. Indicare il `Database.Open` nome del metodo del database da aprire. Si noti che non si includono *sdf* nel nome. Il `Open` metodo presuppone che sta cercando un *sdf* file (vale a dire *WebPagesMovies.sdf*) e che il *sdf* file è nel *App\_ Dati* cartella. (In precedenza è stato indicato che il *App\_dati* cartella è riservata, questo scenario è una delle posizioni in cui ASP.NET ipotizza tale nome.)

Quando il database è aperto, un riferimento a esso viene inserito nella variabile denominata `db`. (Che può essere modificato.) Il `db` variabile è il termine, verrà visualizzata l'interazione con il database.

La seconda riga effettivamente recupera i dati del database utilizzando il `Query` metodo. Si noti il funzionamento di questo codice: il `db` variabile contiene un riferimento al database aperto e si richiama il `Query` metodo tramite il `db` variabile (`db.Query`).

La query stessa è un database SQL `Select` istruzione. (Per una breve introduzione su SQL, vedere la spiegazione in un secondo momento). Nell'istruzione, `Movies` identifica la tabella alla query. Il `*` carattere specifica che la query deve restituire tutte le colonne dalla tabella. (È stato possibile anche elenco colonne singolarmente, separati da virgole.)

I risultati della query, se presenti, vengono restituiti e reso disponibili nel `selectedData` variabile. Nuovamente, la variabile può essere modificata.

Infine, la terza riga indica ad ASP.NET che si desidera utilizzare un'istanza di `WebGrid` helper. Create (*creare un'istanza di*) l'oggetto helper utilizzando il `new` (parola chiave) e passare i risultati della query tramite il `selectedData` variabile. Il nuovo `WebGrid` oggetto, insieme ai risultati della query sul database, vengono resi disponibili nel `grid` variabile. È necessario che in un momento per visualizzare i dati nella pagina.

In questa fase, il database è stato aperto, è stato utilizzato i dati desiderati e preparato il `WebGrid` helper con tali dati. Successivamente consiste nel creare il markup della pagina.

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL è un linguaggio che viene utilizzato nella maggior parte dei database relazionali per la gestione dei dati in un database. Include i comandi che consentono di recuperare i dati e l'aggiornamento e che consentono di creare, modificare e gestire i dati nelle tabelle di database. SQL è diverso da un linguaggio di programmazione (ad esempio c#). Con SQL, viene comunicato al database desiderato, ed è il processo del database per scoprire come ottenere i dati o eseguire l'attività. Di seguito sono riportati esempi di alcuni comandi SQL e le operazioni eseguite:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Il primo `Select` istruzione Ottiene tutte le colonne (specificato da `*`) dal *filmati* tabella.
> 
> Il secondo `Select` istruzione recupera le colonne di tipo ID, nome e prezzo dal record di *prodotto* tabella il cui valore della colonna prezzo è maggiore di 10. Il comando restituisce i risultati in ordine alfabetico in base ai valori della colonna nome. Se nessun record corrispondente ai criteri di prezzo, il comando restituisce un set vuoto.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Questo comando consente di inserire un nuovo record nel *prodotto* tabella, impostare la colonna nome per il prezzo, la colonna Descrizione "Piacere affidabile A" e "Croissant" 1.99.
> 
> Si noti che quando si specifica un valore non numerico, il valore è racchiuso tra virgolette singole (non virgolette doppie, come in c#). Utilizzare le virgolette racchiudono i valori di testo o data, ma non per i numeri.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Questo comando Elimina i record di *prodotto* tabella la cui colonna Data di scadenza è precedente al 1 gennaio 2008. (Il comando si presuppone che il *prodotto* tabella include una colonna, ovviamente.) La data non viene immesso nel formato MM/GG/AAAA, ma deve essere immesso nel formato utilizzato per le impostazioni locali.
> 
> Il `Insert` e `Delete` comandi non restituiscono set di risultati. Invece, restituiscono un numero che indica il numero di record è interessato dal comando.
> 
> Per alcune di queste operazioni (ad esempio, inserimento ed eliminazione di record), il processo che richiede l'operazione deve disporre delle autorizzazioni appropriate nel database. Per tale motivo per il database di produzione è spesso necessario fornire un nome utente e una password quando ci si connette al database.
> 
> Esistono numerosi comandi SQL, ma seguono un modello, ad esempio i comandi visualizzata qui. È possibile utilizzare i comandi SQL per creare tabelle di database, contare il numero di record in una tabella, calcolare prezzi e molte altre operazioni.


### <a name="adding-markup-to-display-the-data"></a>Aggiunta di tag per visualizzare i dati

All'interno di `<head>` set contenuto dell'elemento il `<title>` elemento "Filmati":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

All'interno di `<body>` elemento della pagina, aggiungere quanto segue:

[!code-html[Main](displaying-data/samples/sample3.html)]

È tutto. Il `grid` variabile è il valore è stato creato al momento della creazione di `WebGrid` oggetto nel codice precedente.

Nella visualizzazione albero, WebMatrix destro della pagina e selezionare **Avvia nel browser**. Verrà visualizzato qualcosa di simile a questa pagina:

![Output di supporto WebGrid predefinito dalla tabella di filmati](displaying-data/_static/image18.png)

Fare clic su un collegamento di intestazione di colonna per ordinare in base alla colonna. La possibilità di ordinare facendo clic su un'intestazione è una funzionalità è incorporata il **WebGrid** helper.

Il `GetHtml` (metodo), come suggerisce il nome, genera markup che consente di visualizzare i dati. Per impostazione predefinita, il `GetHtml` metodo genera un elemento HTML `<table>` elemento. (Se si desidera, è possibile verificare il rendering esaminando l'origine della pagina nel browser.)

## <a name="modifying-the-look-of-the-grid"></a>Modifica l'aspetto della griglia

Utilizzo di `WebGrid` helper come fatto in precedenza solo è semplice, ma la visualizzazione risultante è normale. Il `WebGrid` helper sono disponibili numerose opzioni che consentono di controllare la modalità di visualizzazione dei dati. Vi sono troppi per esplorare in questa esercitazione, ma in questa sezione verrà farsi un'idea di alcune di queste opzioni. Alcune opzioni aggiuntive verranno descritta nelle esercitazioni successive di questa serie.

### <a name="specifying-individual-columns-to-display"></a>Specifica di singole colonne da visualizzare

Per iniziare, è possibile specificare che si desidera visualizzare solo alcune colonne. Per impostazione predefinita, come si è visto, nella griglia vengono visualizzati tutti e quattro colonne di *filmati* tabella.

Nel *Movies.cshtml* file, sostituire il `@grid.GetHtml()` markup appena aggiunto con quanto segue:

[!code-css[Main](displaying-data/samples/sample4.css)]

Per indicare l'helper le colonne da visualizzare, includere un `columns` parametro per il `GetHtml` (metodo) e passare in una raccolta di colonne. Specificare nella raccolta, ogni colonna da includere. Specificare una singola colonna per visualizzare includendo un `grid.Column` , quindi passare il nome della colonna di dati desiderato. (Queste colonne devono essere incluse nei risultati della query SQL, ovvero il `WebGrid` helper non è possibile visualizzare le colonne che non sono state restituite dalla query.)

Avviare il *Movies.cshtml* pagina nel browser e questa volta si una visualizzazione simile a quello riportato di seguito (si noti che non esiste una colonna ID viene visualizzata):

![Schermata di WebGrid contenente solo le colonne selezionate](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Modificare l'aspetto della griglia

Esistono alcune altre opzioni per la visualizzazione delle colonne, alcuni dei quali verrà esaminato nelle esercitazioni successive in questo set. Per il momento in questa sezione verrà presentate modi in cui è possibile applicare uno stile della griglia nel suo complesso.

All'interno di `<head>` sezione della pagina, appena prima della chiusura `</head>` tag, aggiungere le seguenti `<style>` elemento:

[!code-css[Main](displaying-data/samples/sample5.css)]

Questo codice CSS definisce classi denominate `grid`, `head`e così via. È anche possibile inserire queste definizioni di stile in un apposito *CSS* file e che crea un collegamento alla pagina. (In realtà, si eseguirà che più avanti in questa esercitazione set.) Ma per semplificare le cose per questa esercitazione, all'interno della stessa pagina che visualizza i dati.

Ora è possibile ottenere il `WebGrid` helper usare queste classi di stile. L'helper è un numero di proprietà (ad esempio, `tableStyle`) per solo a questo scopo, viene assegnato un nome di classe di stile CSS e il nome della classe viene visualizzato come parte del codice che viene eseguito il rendering per il supporto.

Modifica il `grid.GetHtml` markup in modo che è ora simile questo codice:

[!code-css[Main](displaying-data/samples/sample6.css)]

Novità in questo campo è di avere aggiunto `tableStyle`, `headerStyle`, e `alternatingRowStyle` parametri per il `GetHtml` metodo. Questi parametri sono stati impostati per i nomi degli stili CSS che è stato aggiunto un istante fa.

Eseguire la pagina e questa volta, viene visualizzata una griglia che esegue la ricerca semplice molto meno rispetto a prima:

![Visualizzazione WebGrid che include i parametri impostati per i nomi delle classi CSS](displaying-data/_static/image20.png)

Per vedere quali il `GetHtml` metodo generato, è possibile esaminare l'origine della pagina nel browser. Non verrà esaminato in dettaglio di seguito, ma l'aspetto importante è che, specificando i parametri come `tableStyle`, si ha causato la griglia generare i tag HTML simile al seguente:

`<table class="grid">`

Il `<table>` tag è stato un `class` e viene aggiunto l'attributo che fa riferimento a una delle regole CSS aggiunta in precedenza. Questo codice viene illustrato il modello di base &mdash; diversi parametri per il `GetHtml` consentono di riferimento al metodo CSS classi che il metodo genera quindi con il markup. Operazioni con le classi CSS è responsabilità dell'utente.

## <a name="adding-paging"></a>Aggiunta di paging

Come ultima attività per questa esercitazione, si aggiungerà il paging alla griglia. Ora non è alcun problema per visualizzare tutti i filmati in una sola volta. Ma se è stato aggiunto centinaia di film, visualizzazione della pagina otterrebbe lungo.

Nel codice della pagina, modificare la riga che crea il `WebGrid` oggetto per il codice seguente:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

L'unica differenza da prima è che è stato aggiunto un `rowsPerPage` parametro che è impostato su 3.

Eseguire la pagina. Nella griglia vengono visualizzate 3 righe in un tempo più collegamenti di navigazione che consente di scorrere i film nel database le pagine:

![Visualizzazione WebGrid con paging](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Esercitazione seguente

Nella prossima esercitazione, si apprenderà come usare codice Razor e c# per ottenere l'input dell'utente in un form. Si aggiungerà una casella di ricerca per la pagina di filmati in modo che è possibile trovare filmati per titolo o genere.

## <a name="complete-listing-for-movies-page"></a>Elenco completo per la pagina di filmati

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET con sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Precedente](intro-to-web-pages-programming.md)
> [Successivo](form-basics.md)
