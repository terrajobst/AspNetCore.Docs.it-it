---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: 'Introduzione a ASP.NET Web Pages: visualizzazione dei dati | Microsoft Docs'
author: tfitzmac
description: Questa esercitazione illustra come creare un database in WebMatrix e come visualizzare i dati del database in una pagina quando si usa ASP.NET Web Pages (Razor). Si presuppone di y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 864b9f7826763e307368706116458678abf50d3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837446"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>Introduzione a pagine Web ASP.NET - visualizzazione di dati
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra come creare un database in WebMatrix e come visualizzare i dati del database in una pagina quando si usa ASP.NET Web Pages (Razor). Si presuppone di aver completato la serie attraverso [Introduzione alla programmazione di ASP.NET Web Pages](../introducing-razor-syntax-c.md).
> 
> Che cosa si apprenderà come:
> 
> - Come usare gli strumenti di WebMatrix per creare un database e tabelle di database.
> - Come usare gli strumenti di WebMatrix per aggiungere dati a un database.
> - Come visualizzare i dati dal database in una pagina.
> - Come eseguire i comandi SQL in ASP.NET Web Pages.
> - Come personalizzare il `WebGrid` helper per modificare la visualizzazione dei dati e aggiungere l'ordinamento e paging.
>   
> 
> Le funzionalità/tecnologie illustrate:
> 
> - Strumenti di database di WebMatrix.
> - `WebGrid` metodo di supporto.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

Nell'esercitazione precedente, sono stati introdotti per ASP.NET Web Pages (*cshtml* file), le nozioni di base della sintassi Razor e agli helper. In questa esercitazione, inizierai creazione dell'applicazione web effettiva che si userà per il resto della serie. L'app è un'applicazione di film semplice che consente di visualizzare, aggiungere, modificare ed eliminare le informazioni relative ai filmati.

Al termine di questa esercitazione, sarà possibile visualizzare un elenco di film che è simile a questa pagina:

![Visualizzazione di WebGrid che include i parametri impostati per i nomi delle classi CSS](displaying-data/_static/image1.png)

Tuttavia, per iniziare, è necessario creare un database.

## <a name="a-very-brief-introduction-to-databases"></a>Una breve introduzione al database

Questa esercitazione fornirà solo l'introduzione di Mr ai database. Se hai familiarità con i database, è possibile ignorare questa sezione breve.

Un database contiene uno o più tabelle che contengono informazioni &mdash; tabelle, ad esempio, per i clienti, ordini e i fornitori, o per gli studenti, docenti, classi e dei voti. Strutturalmente, una tabella di database è simile a un foglio di calcolo. Si immagini una rubrica tipico. Per ogni voce della Rubrica (vale a dire, per ogni persona) disporre di varie informazioni quali nome, cognome, indirizzo, indirizzo di posta elettronica e numero di telefono.

![Tabella di database di esempio come griglia semplice](displaying-data/_static/image2.png)

(Le righe vengono dette *record*, e le colonne sono talvolta detti *campi*.)

Per la maggior parte delle tabelle di database, la tabella deve disporre di una colonna che contiene un valore univoco, come un numero cliente, numero di account e così via. Questo valore è noto come la tabella *chiave primaria*, e usarlo per identificare ogni riga nella tabella. Nell'esempio, la colonna ID è la chiave primaria per la Rubrica illustrata nell'esempio precedente.

Gran parte delle operazioni eseguite nelle applicazioni web è costituito da informazioni all'esterno del database e di visualizzarli in una pagina. Verrà inoltre spesso raccogliere informazioni dagli utenti e aggiungerla a un database, o che si desidera modificare i record già presenti nel database. (Si affronterà tutte queste operazioni nel corso di questa serie di esercitazioni.)

Operazioni sul database possono essere molto complessa e possono richiedere conoscenze specializzate. Per questa serie di esercitazioni, tuttavia, è necessario comprendere solo concetti di base, che verranno tutte spiegati mentre Prosegui.

## <a name="creating-a-database"></a>Creazione di un Database

WebMatrix include strumenti che rendono più semplice per creare un database e creare tabelle nel database. (La struttura di un database è detto del database *schema*.) Per questa serie di esercitazioni, si creerà un database che contiene solo una tabella di &mdash; film.

Se non è già fatto, aprire WebMatrix e aprire il sito WebPagesMovies creato nell'esercitazione precedente.

Nel riquadro a sinistra, scegliere il **Database** dell'area di lavoro.

![Nella scheda dell'area di lavoro di Database di WebMatrix](displaying-data/_static/image3.png)

Le modifiche della barra multifunzione per visualizzare le attività relative al database. Nella barra multifunzione, fare clic su **Nuovo Database**.

![Pulsante "Nuovo Database" nella barra multifunzione di WebMatrix](displaying-data/_static/image4.png)

WebMatrix consente di creare un database di SQL Server CE (un *sdf* file) che ha lo stesso nome del sito &mdash; *WebPagesMovies.sdf*. (È non farlo qui, ma è possibile rinominare il file su qualsiasi valore desiderato, purché abbia un' *sdf* estensione.)

## <a name="creating-a-table"></a>Creazione di una tabella

Nella barra multifunzione, fare clic su **nuova tabella**. WebMatrix consente di aprire la finestra di progettazione di tabella in una nuova scheda. (Se l'opzione nuova tabella non è disponibile, assicurarsi che il nuovo database sia selezionato nella visualizzazione albero a sinistra.)

![Pulsante "Nuova tabella" nella barra multifunzione di WebMatrix](displaying-data/_static/image5.png)

Nella casella di testo nella parte superiore (in cui la filigrana afferma "Immettere il nome di tabella"), immettere "Movies".

![Immettere un nome di tabella nella finestra di progettazione di database di WebMatrix](displaying-data/_static/image6.png)

Il riquadro sotto il nome della tabella viene usata per definire singole colonne. Per il *film* tabella in questa esercitazione verrà creata solo alcune colonne: *ID*, *Title*, *Genre*, e *anno*.

Nel **nome** immettere "ID". Immettere un valore qui attiva tutti i controlli per la nuova colonna.

Premere TAB per passare il **tipo di dati** elenco e scegliere **int**. Questo valore specifica che la colonna ID conterrà dati integer (numero).

> [!NOTE]
> Non lo chiamiamo noi eventuali informazioni sono disponibili qui (molto), ma è possibile usare i movimenti della tastiera di Windows standard per spostarsi all'interno di questa griglia. Ad esempio, è possibile spostarsi con tab tra i campi, è semplicemente possibile iniziare a digitare per poter selezionare un elemento in un elenco e così via.


Scheda successiva di **Default Value** casella (vale a dire, lasciare vuoto). Premere TAB per passare il **chiave primaria è** casella di controllo e selezionarla. Questa opzione indica il database che il *ID* colonna conterrà i dati che identifica singole righe. (Ovvero, ogni riga avrà un valore univoco nella colonna ID che è possibile usare per trovare quella riga.)

Scegliere il **Identity** opzione. Questa opzione indica il database che deve essere generato automaticamente il successivo numero sequenziale per ogni nuova riga. (Il **Identity** funzionamento dell'opzione solo se è stata selezionata anche il **chiave primaria è** opzione.)

Fare clic nella riga successiva della griglia, o premere Tab due volte per completare la riga corrente. Entrambi movimento consente di salvare la riga corrente e avvia quello successivo. Si noti che il **il valore predefinito** colonna adesso dice **Null**. (Il valore predefinito è null per il valore predefinito, per così dire.)

Al termine della definizione di nuove **ID** colonna, la finestra di progettazione sarà simile a questa illustrazione:

![Progettazione di database WebMatrix dopo la definizione della colonna ID per la tabella di film](displaying-data/_static/image7.png)

Per creare il prossimo articolo, fare clic nella casella il **nome** colonna. Immettere "Title" per la colonna e quindi selezionare **nvarchar** per il **tipo di dati** valore. La parte "var" del **nvarchar** viene indicato al database che i dati per questa colonna sarà una stringa le cui dimensioni possono variare da un record a altro. (Il prefisso "n" rappresenta "national", che indica che il campo può contenere dati di tipo carattere per qualsiasi alfabeto o la scrittura di sistema, vale a dire, il campo contiene dati Unicode.)

Quando si sceglie **nvarchar**, viene visualizzata un'altra casella in cui è possibile immettere la lunghezza massima consentita per il campo. Immettere 50, in base al presupposto che nessun titolo del film che verranno usate in questa esercitazione saranno più di 50 caratteri.

Skip **il valore predefinito** e deselezionare le **Allow Nulls** opzione. Non si desidera il database per consentire eventuali film da immettere nel database che non hanno un titolo.

Quando è terminato e passare alla riga successiva, la finestra di progettazione è simile a questa illustrazione:

![Progettazione di database WebMatrix dopo la definizione di colonna del titolo per la tabella di film](displaying-data/_static/image8.png)

Ripetere questi passaggi per creare una colonna denominata "Genre", fatta eccezione per la lunghezza, impostarla su solo 30.

Creare un'altra colonna denominata "Year". Per il tipo di dati, scegli **nchar** (non **nvarchar**) e impostare la lunghezza su 4. Per l'anno, si intende usare un numero di 4 cifre, ad esempio "1995" o "2010", in modo che non è necessaria una colonna a dimensione variabile.

Ecco come appare la progettazione completata:

![Progettazione di database WebMatrix dopo che tutti i campi definiti per la tabella di film](displaying-data/_static/image9.png)

Premere Ctrl + S oppure scegliere il **salvare** pulsante sulla barra degli strumenti accesso rapido. Chiudere la finestra di progettazione di database, chiudere la scheda.

## <a name="adding-some-example-data"></a>Aggiungendo alcuni dati di esempio

Più avanti in questa serie di esercitazioni si creerà una pagina in cui è possibile immettere nuovi film in un form. Per il momento, tuttavia, è possibile aggiungere alcuni dati di esempio che è quindi possibile visualizzare in una pagina.

Nel **Database** area di lavoro di WebMatrix, si noti che sono presente un albero che mostra le *sdf* file creato in precedenza. Aprire il nodo per il nuovo *sdf* del file e quindi aprire il **tabelle** nodo.

![Area di lavoro di WebMatrix Database con struttura ad albero aperta alla tabella di film](displaying-data/_static/image10.png)

Fare doppio clic il **film** nodo e quindi scegliere **dati**. WebMatrix consente di aprire una griglia in cui è possibile immettere dati per il *film* tabella:

![Griglia di voce di database in WebMatrix (vuoto)](displaying-data/_static/image11.png)

Scegliere il **titolo** colonna e immettere "Quando Harry soddisfatti Sara". Spostare il **Genre** colonna (è possibile utilizzare il tasto Tab) e immettere "Romantico commedie". Spostare il **anno** colonna e immettere "1989":

![Griglia dei database di movimento in WebMatrix con un record](displaying-data/_static/image12.png)

Premere INVIO e WebMatrix consente di salvare il nuovo film. Si noti che il **ID** colonna è stata compilata.

![Griglia dei database di movimento in WebMatrix con un unico record e l'ID generato automaticamente](displaying-data/_static/image13.png)

Immettere un altro film (ad esempio, "verificato con il vento", "Drammatico", "1939"). La colonna ID viene inserita nuovamente:

![Griglia dei database di movimento in WebMatrix con due record e gli ID generato automaticamente](displaying-data/_static/image14.png)

Immettere un terzo film (ad esempio, "Ghostbusters", "Commedie"). Come esperimento, lasciare il **anno** colonna vuota e quindi premere INVIO. Poiché non è selezionato il **Ammetti** opzione, il database viene visualizzato un errore:

![Se un valore di colonna richiesto viene lasciato vuoto visualizzato errore "Dati non valido"](displaying-data/_static/image15.png)

Fare clic su **OK** per tornare indietro e correggere la voce (l'anno per "Ghostbusters" è 1984) e quindi premere INVIO.

Compilare filmati diversi fino a quando non si dispone di 8 o in modo. (Immettere 8 rende più facile lavorare con paging in un secondo momento. Ma se questo è un numero eccessivo, immettere solo alcune per ora). Non importa i dati effettivi.

![Griglia dei database di movimento in WebMatrix con entrambi i record](displaying-data/_static/image16.png)

Se è stato immesso tutti i film senza errori, i valori ID sono sequenziali. Se si è tentato di salvare un record di film incompleta, i numeri di identificazione non necessariamente sequenziali. In questo caso, che è accettabile. I numeri non hanno alcun significato intrinseco ed è l'unica cosa che è importante che siano univoci nel *film* tabella.

Chiudere la scheda contenente la finestra di progettazione di database.

A questo punto è possibile attivare per visualizzare i dati in una pagina web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Visualizzazione dei dati in una pagina con il supporto WebGrid

Per visualizzare i dati in una pagina, si intende usare il `WebGrid` helper. Questo helper produce una visualizzazione in una griglia o una tabella (righe e colonne). Come si vedrà, si sarà in grado di migliorare la griglia con le funzionalità di formattazione e altri.

Per eseguire la griglia, è possibile scrivere poche righe di codice. Queste poche righe fungerà da un tipo di modello per quasi tutti l'accesso ai dati che effettuate in questa esercitazione.

> [!NOTE]
> Effettivamente disponibili molte opzioni per la visualizzazione dei dati in una pagina. il `WebGrid` helper è solo uno. Abbiamo scelto, per questa esercitazione perché è il modo più semplice per visualizzare i dati e perché è abbastanza flessibile. Nel set successivo dell'esercitazione, verrà illustrato come usare un altro modo "manuale" per lavorare con i dati nella pagina, che offre un controllo più diretto su come visualizzare i dati.


Nel riquadro sinistro di WebMatrix, fare clic sui **file** dell'area di lavoro.

Il nuovo database creato è nel *App\_dati* cartella. Se la cartella non esiste già, WebMatrix creato per il nuovo database. (La cartella potrebbe essere che fosse presente se è stato installato in precedenza gli helper.)

Nella visualizzazione albero, selezionare la radice del sito Web. È necessario selezionare la radice del sito Web; in caso contrario, il nuovo file potrebbe essere aggiunta all'App\_cartella dati.

Nella barra multifunzione, fare clic su **New**. Nel **scegliere un tipo di File** , scegliere **CSHTML**.

Nel **nome** casella, denominare la nuova pagina "Movies.cshtml":

![Finestra di dialogo "Scegli un tipo di File" che mostra la pagina 'Film'](displaying-data/_static/image17.png)

Scegliere il **OK** pulsante. WebMatrix consente di aprire un nuovo file con alcuni elementi in esso scheletro. Prima di tutto si scriverà codice per ottenere i dati dal database. Si aggiungeranno quindi markup alla pagina per visualizzare i dati.

### <a name="writing-the-data-query-code"></a>Scrittura del codice di Query di dati

Nella parte superiore della pagina, tra il `@{` e `}` caratteri, immettere il codice seguente. (Assicurarsi che il codice viene inserito tra l'apertura e la parentesi graffe di chiusura).

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

La prima riga viene aperto il database creato in precedenza, che è sempre il primo passaggio prima di eseguire un elemento con il database. Indicare il `Database.Open` nome del metodo del database da aprire. Si noti che non si includono *sdf* nel nome. Il `Open` metodo presuppone che sta cercando un *sdf* file (vale a dire *WebPagesMovies.sdf*) e che il *sdf* file si trova nel *App\_ Dati* cartella. (In precedenza è stato indicato che il *App\_dati* cartella è riservata; in questo scenario è uno dei posti in cui ASP.NET ipotizza tale nome.)

Quando viene aperto il database, un riferimento a esso viene inserito la variabile denominata `db`. (Che può essere modificato.) Il `db` variabile è come termine, verrà visualizzata l'interazione con il database.

La seconda riga vengono effettivamente recuperati i dati di database usando la `Query` (metodo). Si noti come funziona questo codice: il `db` variabile contiene un riferimento al database aperto e si richiama il `Query` metodo utilizzando il `db` variabile (`db.Query`).

La query stessa è un linguaggio SQL `Select` istruzione. (Per una breve introduzione sulle istruzioni SQL, vedere la spiegazione in un secondo momento). Nell'istruzione `Movies` identifica la tabella alla query. Il `*` carattere specifica che la query deve restituire tutte le colonne dalla tabella. (È possibile inoltre elencare le colonne singolarmente, separati da virgole.)

I risultati della query, se presenti, vengono restituiti e reso disponibili nel `selectedData` variabile. Anche in questo caso, la variabile può essere modificata.

Infine, la terza riga indica ad ASP.NET che si desidera utilizzare un'istanza di `WebGrid` helper. Si crea (*creare un'istanza*) l'oggetto di supporto tramite il `new` (parola chiave) e passare i risultati della query tramite il `selectedData` variabile. Il nuovo `WebGrid` vengono resi disponibili nell'oggetto, insieme ai risultati della query del database, il `grid` variabile. Tale risultato sarà necessario più avanti per visualizzare i dati nella pagina.

In questa fase, il database è stato aperto, ottenuti i dati da e avere preparato la `WebGrid` helper con tali dati. Successivamente consiste nel creare il markup della pagina.

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL è un linguaggio che viene utilizzato nella maggior parte dei database relazionali per la gestione dei dati in un database. Include i comandi che consentono di recuperare i dati e aggiornarlo e che consentono di creare, modificare e gestire i dati nelle tabelle di database. SQL è diversa da un linguaggio di programmazione (ad esempio, c#). Con SQL, viene comunicato al database che si desidera, ed è il processo del database per scoprire come ottenere i dati o eseguire l'attività. Ecco alcuni esempi di alcuni comandi SQL e le operazioni eseguite:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Il primo `Select` istruzione Ottiene tutte le colonne (specificato da `*`) dalle *film* tabella.
> 
> La seconda `Select` istruzione recupera le colonne ID, Name e prezzo dai record nel *prodotto* tabella il cui valore della colonna prezzo è maggiore di 10. Il comando restituisce i risultati in ordine alfabetico in base ai valori della colonna nome. Se nessun record corrispondente ai criteri di prezzo, il comando restituisce un set vuoto.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Questo comando consente di inserire un nuovo record nel *prodotto* tabella, impostando la colonna nome della colonna Description per "Apprezzate dagli inattendibili A", "Croissant" e il prezzo su 1.99.
> 
> Si noti che quando si specifica un valore non numerico, il valore è racchiuso tra virgolette singole (non virgolette doppie, come in c#). Si usano queste virgolette per racchiudere i valori di testo o data, ma non per i numeri.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Questo comando Elimina i record nel *prodotto* tabella la cui colonna di date di scadenza è precedente al 1 gennaio 2008. (Il comando presuppone che il *prodotto* tabella include una colonna, ovviamente.) La data non viene immesso nel formato MM/GG/AAAA, ma deve essere immesso nel formato utilizzato per le impostazioni locali.
> 
> Il `Insert` e `Delete` comandi non restituiscono set di risultati. Al contrario, restituiscono un numero che indica il numero di record sono stato interessato dal comando.
> 
> Per alcune di queste operazioni (ad esempio, inserimento ed eliminazione di record), il processo che sta richiedendo l'operazione deve disporre delle autorizzazioni appropriate nel database. Ecco perché per i database di produzione spesso è necessario specificare un nome utente e password quando ci si connette al database.
> 
> Sono disponibili decine di comandi SQL, ma seguono un modello, ad esempio i comandi riportato di seguito. È possibile usare i comandi SQL per creare tabelle di database, contare il numero di record in una tabella, calcolo dei prezzi ed eseguire molte più operazioni.


### <a name="adding-markup-to-display-the-data"></a>Aggiunta di tag per visualizzare i dati

All'interno di `<head>` elemento, contenuto del set del `<title>` elemento "Movies":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

All'interno di `<body>` elemento della pagina, aggiungere quanto segue:

[!code-html[Main](displaying-data/samples/sample3.html)]

È tutto. Il `grid` variabile è il valore è stato creato durante la creazione di `WebGrid` oggetto nel codice precedente.

Nella visualizzazione albero WebMatrix, fare doppio clic la pagina e selezionare **Avvia nel browser**. Si otterrà un risultato simile a questa pagina:

![Output dell'helper WebGrid predefinito dalla tabella dei film](displaying-data/_static/image18.png)

Fare clic sul collegamento di un'intestazione di colonna per ordinare in base alla colonna. La possibilità di ordinare facendo clic su un'intestazione è una funzionalità integrata nel **WebGrid** helper.

Il `GetHtml` metodo, come suggerisce il nome, genera markup che consente di visualizzare i dati. Per impostazione predefinita, il `GetHtml` metodo genera un HTML `<table>` elemento. (Se si desidera, è possibile verificare il rendering esaminando l'origine della pagina nel browser.)

## <a name="modifying-the-look-of-the-grid"></a>Modifica l'aspetto della griglia

Uso di `WebGrid` helper, ad esempio è stato appena fatto è facile, ma la visualizzazione risultante è normale. Il `WebGrid` helper ha moltissime opzioni che consentono di controllare la modalità di visualizzazione dei dati. Sono presenti troppe per esplorare in questa esercitazione, ma in questa sezione viene fornita un'idea di alcune di queste opzioni. Alcune opzioni aggiuntive verranno trattate nelle esercitazioni successive di questa serie.

### <a name="specifying-individual-columns-to-display"></a>Specifica di singole colonne da visualizzare

Per iniziare, è possibile specificare che si desidera visualizzare solo alcune colonne. Per impostazione predefinita, come si è visto, la griglia Mostra tutte e quattro le colonne dai *film* tabella.

Nel *Movies.cshtml* del file, sostituire il `@grid.GetHtml()` markup appena aggiunto con il codice seguente:

[!code-css[Main](displaying-data/samples/sample4.css)]

Per indicare le colonne da visualizzare l'helper, si include un' `columns` parametro per il `GetHtml` (metodo) e passare in una raccolta di colonne. Nella raccolta, specificare ogni colonna da includere. Si specifica una singola colonna da visualizzare, includendo un `grid.Column` e passare il nome della colonna di dati desiderato. (Queste colonne devono essere incluse nei risultati della query SQL, ovvero il `WebGrid` helper non è possibile visualizzare le colonne che non sono state restituite dalla query.)

Avviare il *Movies.cshtml* pagina nel browser e questa volta che si ottiene una visualizzazione, come quello riportato di seguito (notare che non viene visualizzata alcuna colonna ID):

![Schermata di WebGrid contenente solo le colonne selezionate](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Modificare l'aspetto della griglia

Esistono alcune altre opzioni per la visualizzazione delle colonne, alcuni dei quali sono descritti nelle esercitazioni successive in questo set. Per ora, in questa sezione verrà presentate modi in cui è possibile applicare uno stile della griglia nel suo complesso.

All'interno di `<head>` sezione della pagina, subito prima del tag `</head>` tag, aggiungere il codice seguente `<style>` elemento:

[!code-css[Main](displaying-data/samples/sample5.css)]

Questo codice CSS definisce classi denominate `grid`, `head`e così via. È anche possibile inserire queste definizioni di stile di visualizzazione in un oggetto separato *CSS* file e che crea un collegamento alla pagina. (In effetti, è necessario usare più avanti in questa serie di esercitazioni.) Ma per semplificare le cose per questa esercitazione, sono all'interno della stessa pagina che visualizza i dati.

Ora è possibile ottenere il `WebGrid` helper per usare queste classi di stile. L'helper è un numero di proprietà (ad esempio, `tableStyle`) proprio questo scopo, viene assegnato un nome di classe di stile CSS e il nome della classe è sottoposto a rendering come parte del markup di cui viene eseguito il rendering dall'helper.

Modifica il `grid.GetHtml` markup in modo che l'it ora simile a questo codice:

[!code-css[Main](displaying-data/samples/sample6.css)]

La novità è che è stato aggiunto `tableStyle`, `headerStyle`, e `alternatingRowStyle` i parametri per il `GetHtml` (metodo). Questi parametri sono stati impostati per i nomi degli stili CSS che è stato aggiunto qualche istante fa.

Eseguire la pagina e questa volta è visualizzata una griglia che sembra normale molto meno rispetto a prima:

![Visualizzazione di WebGrid che include i parametri impostati per i nomi delle classi CSS](displaying-data/_static/image20.png)

Per conoscere il `GetHtml` metodo generato, è possibile esaminare l'origine della pagina nel browser. Non verrà esaminato in dettaglio di seguito, ma il punto importante è che si specificano parametri, ad esempio `tableStyle`, si ha causato la griglia generare i tag HTML simile al seguente:

`<table class="grid">`

Il `<table>` tag ha avuto un `class` e viene aggiunto l'attributo che fa riferimento a una delle regole CSS aggiunti in precedenza. Questo codice viene illustrato il modello di base &mdash; parametri diversi per il `GetHtml` "Let" di riferimento al metodo CSS classi che il metodo genera quindi con il markup. Cosa fare con le classi CSS è responsabilità dell'utente.

## <a name="adding-paging"></a>Aggiunta di paging

Come ultima attività per questa esercitazione, si aggiungerà il paging nella griglia. Attualmente non è un problema per visualizzare tutti i film in una sola volta. Ma se è stato aggiunto centinaia di film, la visualizzazione delle pagine potrebbe diventare lunga.

Nel codice della pagina, modificare la riga che crea il `WebGrid` oggetto per il codice seguente:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

È l'unica differenza da prima di aver aggiunto un `rowsPerPage` parametro impostato su 3.

Eseguire la pagina. Nella griglia vengono visualizzate 3 righe a un'ora, più collegamenti di navigazione che consentono di spostarsi tra i film nel database:

![Visualizzazione di WebGrid con paging](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>In arrivo

Nella prossima esercitazione, si apprenderà come usare codice Razor e c# per ottenere l'input utente in un form. Si aggiungerà una casella di ricerca alla pagina Movies in modo che sia possibile trovare per titolo o genere film.

## <a name="complete-listing-for-movies-page"></a>Elenco completo per la pagina Movies

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET usando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Precedente](intro-to-web-pages-programming.md)
> [Successivo](form-basics.md)
