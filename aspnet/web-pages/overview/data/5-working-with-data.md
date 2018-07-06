---
uid: web-pages/overview/data/5-working-with-data
title: Introduzione all'uso di un Database in ASP.NET Web Pages (Razor) siti | Microsoft Docs
author: tfitzmac
description: In questo capitolo descrive come accedere ai dati da un database e visualizzarlo utilizzando ASP.NET Web Pages.
ms.author: aspnetcontent
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 5185769530cf78c301f2ac43b25dba6e77ca75a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808506"
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Introduzione all'uso di un Database in ASP.NET Web Pages (Razor) siti
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive come usare gli strumenti di Microsoft WebMatrix per creare un database in un sito Web ASP.NET Web Pages (Razor) e come creare pagine che consentono di visualizzare, aggiungere, modificare ed eliminare dati.
> 
> **Che cosa si apprenderà come:** 
> 
> - Come creare un database.
> - Come connettersi a un database.
> - Come visualizzare i dati in una pagina web.
> - Come inserire, aggiornare ed eliminare i record del database.
> 
> Queste sono le funzionalità introdotte nell'articolo:
> 
> - Utilizzo di un database Microsoft SQL Server Compact Edition.
> - Utilizzo di query SQL.
> - Classe `Database`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Questa esercitazione si integra inoltre con WebMatrix 3. È possibile usare 3 pagine Web ASP.NET e Visual Studio 2013 (o Visual Studio Express 2013 per il Web); Tuttavia, l'interfaccia utente sarà diversa.


## <a name="introduction-to-databases"></a>Introduzione a database

Si immagini una rubrica tipico. Per ogni voce della Rubrica (vale a dire, per ogni persona) disporre di varie informazioni quali nome, cognome, indirizzo, indirizzo di posta elettronica e numero di telefono.

Un modo comune per i dati immagine simile alla seguente è come una tabella con righe e colonne. In termini di database, ogni riga è noto anche come un record. Ogni colonna (talvolta detto campi) contiene un valore per ogni tipo di dati: nome del primo, ultimo nome e così via.

| **ID** | **firstName** | **Cognome** | **Indirizzo** | **Posta elettronica** | **Telefono** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100 St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Per la maggior parte delle tabelle di database, la tabella deve disporre di una colonna che contiene un identificatore univoco, ad esempio un numero cliente, numero di account e così via. Questo è noto come la tabella *chiave primaria*, e usarlo per identificare ogni riga nella tabella. Nell'esempio, la colonna ID è la chiave primaria per la Rubrica.

Con questa conoscenza di base dei database, si è pronti per imparare a creare un database semplice ed esegua operazioni di aggiunta, modifica ed eliminazione dei dati.

> [!TIP] 
> 
> **Database relazionali**
> 
> È possibile archiviare i dati in molti modi, tra cui fogli di calcolo e file di testo. Per la maggior parte degli usi di business, tuttavia, i dati vengono archiviati in un database relazionale.
> 
> Questo articolo non andare molto approfonditamente nel database. Tuttavia, si potrebbe risultare utile per comprendere un po' riguardo. In un database relazionale, informazioni viene suddiviso logicamente in tabelle separate. Ad esempio, un database per un istituto di istruzione potrebbe contenere tabelle separate per gli studenti e per le offerte di classe. Software (ad esempio SQL Server) supporta avanzato dei comandi di database che consentono in modo dinamico stabiliscono relazioni tra le tabelle. Ad esempio, è possibile utilizzare il database relazionale per stabilire una relazione logica tra gli studenti e classi per creare una pianificazione. L'archiviazione dei dati in tabelle separate riduce la complessità di struttura della tabella e riduce la necessità di mantenere i dati ridondanti nelle tabelle.


## <a name="creating-a-database"></a>Creazione di un Database

Questa procedura illustra come creare un database denominato SmallBakery usando lo strumento di progettazione di Database di SQL Server Compact incluso in WebMatrix. Sebbene sia possibile creare un database usando il codice, è più comune per creare il database e tabelle di database utilizzando uno strumento di progettazione, ad esempio WebMatrix.

1. Avviare WebMatrix e nella pagina avvio rapido fare clic su **sito da modello**.
2. Selezionare **sito vuoto**e il **nome sito** casella immettere "SmallBakery" e quindi fare clic su **OK**. Il sito verrà creato e visualizzato in WebMatrix.
3. Nel riquadro a sinistra, scegliere il **database** dell'area di lavoro.
4. Nella barra multifunzione, fare clic su **Nuovo Database**. Viene creato un database vuoto con lo stesso nome del sito.
5. Nel riquadro sinistro, espandere la **SmallBakery.sdf** nodo e quindi fare clic su **tabelle**.
6. Nella barra multifunzione, fare clic su **nuova tabella**. WebMatrix consente di aprire la finestra di progettazione di tabella.

    ![[immagine]](5-working-with-data/_static/image1.jpg)
7. Fare clic nella **Name** colonna e immettere &quot;Id&quot;.
8. Nel **tipo di dati** colonna, selezionare **int**.
9. Impostare il **è chiave primaria?** e **è identificare?** le opzioni per **Sì**.

    Come suggerisce il nome, **chiave primaria è** viene indicato al database che si tratterà chiave primaria della tabella. **Identità** indica il database da creare automaticamente un numero di ID per ogni nuovo record e per assegnargli il successivo numero sequenziale (a partire da 1).
10. Fare clic nella riga successiva. L'editor viene avviata una nuova definizione di colonna.
11. Per il valore del nome, immettere &quot;nome&quot;.
12. Per la **tipo di dati**, scegliere &quot;nvarchar&quot; e impostare la lunghezza fino a 50. Il *var* fa parte di `nvarchar` viene indicato al database che i dati per questa colonna sarà una stringa le cui dimensioni possono variare da un record a altro. (Il *n* prefisso rappresenta *national*, che indica che il campo può contenere dati di tipo carattere che rappresenta qualsiasi alfabeto o sistema di scrittura &#8212; , ovvero che il campo contiene dati Unicode.)
13. Impostare il **Ammetti** possibilità **No**. Ciò impone che il *nome* colonna non viene lasciata vuota.
14. Usando la stessa procedura, creare una colonna denominata *descrizione*. Impostare **tipo di dati** "nvarchar" e 50 sono riservati per la lunghezza, quindi impostare **Allow Nulls** su false.
15. Creare una colonna denominata *Price*. Impostare **tipo di dati "money"** e impostare **Allow Nulls** su false.
16. Nella casella nella parte superiore di denominare la tabella &quot;prodotto&quot;.

    Al termine, la definizione avrà un aspetto simile al seguente:

    ![[immagine]](5-working-with-data/_static/image2.jpg)
17. Premere Ctrl + S per salvare la tabella.

## <a name="adding-data-to-the-database"></a>Aggiunta di dati al Database

A questo punto è possibile aggiungere alcuni dati di esempio per il database che verranno usate più avanti nell'articolo.

1. Nel riquadro sinistro, espandere la **SmallBakery.sdf** nodo e quindi fare clic su **tabelle**.
2. Fare doppio clic nella tabella Product e quindi fare clic su **dati**.
3. Nel riquadro di modifica, immettere i record seguenti:

    | **Name** | **Descrizione** | **Prezzo** |
    | --- | --- | --- |
    | Pane | Virtuali create con bake aggiornato ogni giorno. | 2.99 |
    | Shortcake strawberry | Eseguita con strawberries organica dal nostro giardino. | 9.99 |
    | Grafico a torta Apple | In secondo luogo solo ai grafici a torta di mom. | 12.99 |
    | Grafico a torta Pecan | Se si desidera che del Queenland, si tratta per l'utente. | 10.99 |
    | Grafico a torta limone | Effettuate con i limoni migliori del mondo. | 11.99 |
    | Cupcakes | I bambini e kid in si apprezzeranno questi. | 7.99 |

    Tenere presente che non è necessario immettere alcun valore per il *Id* colonna. Quando è stato creato il *Id* colonna, si imposta relativo **Identity** proprietà su true, che ne determina automaticamente da compilare.

    Quando hai finito immissione dei dati, Progettazione tabelle avrà un aspetto simile al seguente:

    ![[immagine]](5-working-with-data/_static/image3.jpg)
4. Chiudere la scheda che contiene i dati del database.

## <a name="displaying-data-from-a-database"></a>Visualizzazione dei dati da un Database

Dopo che è disponibile un database con dati in esso, è possibile visualizzare i dati in una pagina web ASP.NET. Per selezionare le righe della tabella per visualizzare, utilizzare un'istruzione SQL, che è un comando che si passa al database.

1. Nel riquadro a sinistra, scegliere il **file** dell'area di lavoro.
2. Nella radice del sito Web, creare una nuova pagina CSHTML denominata *ListProducts.cshtml*.
3. Sostituire il markup esistente con il codice seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    Nel primo blocco di codice, si apre la *SmallBakery.sdf* file (database) creato in precedenza. Il `Database.Open` metodo presuppone che il *sdf* file è di un sito Web *App\_dati* cartella. (Si noti che non è necessario specificare il *sdf* estensione &#8212; in effetti, tal caso, il `Open` (metodo) non funzionerà.)

    > [!NOTE]
    > Il *App\_dati* cartella è una cartella speciale in ASP.NET che viene usato per archiviare i file di dati. Per altre informazioni, vedere [ci si connette a un Database](#SB_ConnectingToADatabase) più avanti in questo articolo.

    Quindi effettuare una richiesta per eseguire query sul database usando il codice SQL seguente `Select` istruzione:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    Nell'istruzione `Product` identifica la tabella alla query. Il `*` carattere specifica che la query deve restituire tutte le colonne dalla tabella. (È possibile inoltre elencare le colonne singolarmente, separati da virgole, se si vogliono visualizzare solo alcune colonne.) Il `Order By` clausola indica come devono essere ordinati i dati di &#8212; in questo caso, per il *Name* colonna. Ciò significa che i dati vengono ordinati in ordine alfabetico in base al valore della *nome* colonna per ogni riga.

    Nel corpo della pagina, il markup crea una tabella HTML che verrà usata per visualizzare i dati. All'interno di `<tbody>` elemento, si utilizza un `foreach` ciclo per ottenere individualmente ogni riga di dati restituito dalla query. Per ogni riga di dati, si crea una riga della tabella HTML (`<tr>` elemento). Quindi, creiamo le celle della tabella HTML (`<td>` elementi) per ogni colonna. Ogni volta che visita del ciclo, la successiva riga disponibile del database è nel `row` variabile (a tale scopo `foreach` istruzione). Per ottenere una singola colonna della riga, è possibile usare `row.Name` o `row.Description` o è il nome della colonna è necessario.
4. Eseguire la pagina in un browser. (Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.) La pagina Visualizza un elenco simile al seguente:

    ![[immagine]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL è un linguaggio che viene utilizzato nella maggior parte dei database relazionali per la gestione dei dati in un database. Include i comandi che consentono di recuperare i dati e aggiornarlo e che consentono di creare, modificare e gestire le tabelle del database. SQL è diverso da quello di un linguaggio di programmazione (simile a quello in uso in WebMatrix) perché con SQL, l'idea è che viene comunicato al database che si desidera, ed è il processo del database per scoprire come ottenere i dati o eseguire l'attività. Ecco alcuni esempi di alcuni comandi SQL e le operazioni eseguite:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Si recupera la *Id*, *Name*, e *prezzo* colonne dai record nel *prodotto* tabella se il valore di *prezzo* è maggiore di 10 e restituisce i risultati in ordine alfabetico in base ai valori del *nome* colonna. Questo comando restituirà un set di risultati che contiene i record che soddisfano i criteri o un set vuoto se nessun record corrispondente.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Inserisce un nuovo record nel *prodotto* tabella, l'impostazione di *nome* colonna &quot;Cornetto&quot;, il *descrizione* colonna &quot; Un apprezzate dagli inattendibili&quot;e il prezzo per 1.99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Questo comando Elimina i record nel *prodotto* tabella la cui colonna di date di scadenza è precedente al 1 gennaio 2008. (Ciò presuppone che il *prodotto* tabella include una colonna, ovviamente.) La data non viene immesso nel formato MM/GG/AAAA, ma deve essere immesso nel formato utilizzato per le impostazioni locali.
> 
> Il `Insert Into` e `Delete` comandi non restituiscono set di risultati. Al contrario, restituiscono un numero che indica il numero di record sono stato interessato dal comando.
> 
> Per alcune di queste operazioni (ad esempio, inserimento ed eliminazione di record), il processo che sta richiedendo l'operazione deve disporre delle autorizzazioni appropriate nel database. Questo è il motivo per i database di produzione è spesso necessario fornire un nome utente e password quando ci si connette al database.
> 
> Sono disponibili decine di comandi SQL, ma seguono un modello simile al seguente. È possibile usare i comandi SQL per creare tabelle di database, contare il numero di record in una tabella, calcolo dei prezzi ed eseguire molte più operazioni.


## <a name="inserting-data-in-a-database"></a>Inserimento di dati in un Database

In questa sezione viene illustrato come creare una pagina che consente agli utenti di aggiungere un nuovo prodotto per il *prodotto* tabella di database. Dopo che viene inserito un nuovo record di prodotto, la pagina viene visualizzata la tabella aggiornata con le *ListProducts.cshtml* pagina creata nella sezione precedente.

La pagina include la convalida per assicurarsi che i dati immessi dall'utente siano validi per il database. Ad esempio, il codice nella pagina assicura che sia stato immesso un valore per tutte le colonne necessarie.

1. Nel sito Web, creare un nuovo file con estensione CSHTML denominato *InsertProducts.cshtml*.
2. Sostituire il markup esistente con il codice seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Il corpo della pagina contiene un modulo HTML con tre caselle di testo che consentono agli utenti di immettere un nome, descrizione e prezzo. Quando gli utenti fanno clic la **inserire** pulsante, il codice nella parte superiore della pagina consente di aprire una connessione per il *SmallBakery.sdf* database. Ottenere quindi i valori che l'utente ha immesso utilizzando il `Request` dell'oggetto e assegnare tali valori a variabili locali.

    Per convalidare che l'utente ha immesso un valore per ogni colonna necessaria, ogni registrazione `<input>` elemento che si desidera convalidare:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    Il `Validation` helper controlla che sia presente un valore in ognuno dei campi che è stato registrato. È possibile verificare se tutti i campi sono stati convalidati controllando `Validation.IsValid()`, che avviene in genere prima di elaborare le informazioni ottenute da parte dell'utente:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (Il `&&` mezzi di operatore AND: questo test viene *se si tratta di invio di un modulo e tutti i campi hanno superato la convalida*.)

    Se tutte le colonne convalidata (Nessuno era vuota), si proseguire e creare un'istruzione SQL per inserire i dati e quindi di eseguirlo come indicato di seguito:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Per i valori da inserire, includere il segnaposto di parametro (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Come misura precauzionale, passare sempre i valori in un'istruzione SQL con parametri, come noterete nell'esempio precedente. Questo offre la possibilità di convalidare i dati dell'utente, oltre aiuta a proteggersi da tentativi di invio di comandi dannosi per il database (talvolta detto attacchi SQL injection).

    Per eseguire la query, è utilizzare questa istruzione, passando le variabili che contengono i valori per sostituire i segnaposto:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Dopo il `Insert Into` esecuzione dell'istruzione, si invia l'utente alla pagina che elenca i prodotti utilizzando questa riga:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Se non è riuscita la convalida, si ignora l'inserimento. È invece necessario un supporto nella pagina in grado di visualizzare i messaggi di errore accumulato (se presente):

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Si noti che il blocco di stile nel markup include una definizione di classe CSS denominata `.validation-summary-errors`. Si tratta del nome della classe CSS utilizzata per impostazione predefinita per il `<div>` elemento che contiene gli errori di convalida. In questo caso, la classe CSS specifica che gli errori di riepilogo di convalida vengono visualizzati in rosso e in grassetto, ma è possibile definire il `.validation-summary-errors` classe per visualizzare qualsiasi formattazione desiderato.

### <a name="testing-the-insert-page"></a>Test della pagina di inserimento

1. Visualizzare la pagina in un browser. La pagina Visualizza un form simile a quello illustrato nella figura seguente.

    ![[immagine]](5-working-with-data/_static/image5.jpg)
2. Immettere i valori per tutte le colonne, ma assicurarsi di lasciare il *prezzo* colonna vuota.
3. Fare clic su **Inserisci**. La pagina Visualizza un messaggio di errore, come illustrato nella figura seguente. (Nessun nuovo record viene creato).

    ![[immagine]](5-working-with-data/_static/image6.jpg)
4. Compilare il modulo completamente e quindi fare clic su **Inserisci**. Questa volta, il *ListProducts.cshtml* pagina viene visualizzata e Mostra il nuovo record.

## <a name="updating-data-in-a-database"></a>L'aggiornamento dei dati in un Database

Dopo l'immissione dei dati in una tabella, si potrebbe essere necessario aggiornarlo. Questa procedura illustra come creare due pagine che sono simili a quelli creati per l'inserimento di dati in precedenza. La prima pagina consente di visualizzare i prodotti e consente agli utenti di selezionarne uno da modificare. La seconda pagina consente agli utenti effettivamente apportare le modifiche e salvarle.

> [!NOTE] 
> 
> **Importante** In siti Web di produzione, è in genere limitare chi è autorizzato per apportare modifiche ai dati. Per informazioni su come configurare l'appartenenza e sui modi per consentire agli utenti di eseguire attività nel sito, vedere [aggiunta di sicurezza e l'appartenenza a un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Nel sito Web, creare un nuovo file con estensione CSHTML denominato *EditProducts.cshtml*.
2. Sostituire il markup esistente nel file con il codice seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    L'unica differenza tra questa pagina e il *ListProducts.cshtml* pagina da versioni precedenti è che la tabella HTML in questa pagina include una colonna aggiuntiva che visualizza un' **modificare** collegamento. Quando si fa clic su questo collegamento, verrà visualizzato il *UpdateProducts.cshtml* pagina (che verrà creata successivamente) in cui è possibile modificare il record selezionato.

    Esaminare il codice che crea il **modifica** collegamento:

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Ciò consente di creare un elemento HTML `<a>` elemento la cui `href` attributo è impostato in modo dinamico. Il `href` attributo specifica la pagina da visualizzare quando l'utente fa clic sul collegamento. Viene anche passata la `Id` valore della riga corrente per il collegamento. Quando la pagina viene eseguita, l'origine della pagina potrebbe contenere collegamenti simili ai seguenti:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Si noti che il `href` attributo è impostato su `UpdateProducts/n`, dove *n* è un numero di prodotto. Quando un utente fa clic su uno di questi collegamenti, l'URL risultante avrà un aspetto simile al seguente:

    `http://localhost:18816/UpdateProducts/6`

    In altre parole, il numero del prodotto per la modifica verrà passato nell'URL.
3. Visualizzare la pagina in un browser. La pagina Visualizza i dati in un formato simile al seguente:

    ![[immagine]](5-working-with-data/_static/image7.jpg)

    Successivamente, si creerà la pagina che consente agli utenti di aggiornare effettivamente i dati. La pagina di aggiornamento include la convalida per convalidare i dati immessi dall'utente. Ad esempio, il codice nella pagina assicura che sia stato immesso un valore per tutte le colonne necessarie.
4. Nel sito Web, creare un nuovo file con estensione CSHTML denominato *UpdateProducts.cshtml*.
5. Sostituire il markup esistente nel file con il codice seguente.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Il corpo della pagina contiene un form HTML in cui viene visualizzato un prodotto e in cui gli utenti possono modificarlo. Per ottenere il prodotto da visualizzare, utilizzare l'istruzione SQL:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Ciò consentirà di selezionare il prodotto il cui ID corrisponde al valore passato il `@0` parametro. (Poiché *Id* è la chiave primaria e pertanto deve essere univoco, record di un solo prodotto mai può essere selezionato in questo modo.) Per ottenere il valore di ID da passare a questa `Select` istruzione, è possibile leggere il valore passato alla pagina come parte dell'URL, utilizzando la sintassi seguente:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Per recuperare effettivamente il record di prodotto, si utilizza il `QuerySingle` metodo, che verrà restituito un solo record:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    La singola riga viene restituita nel `row` variabile. È possibile ottenere dati all'esterno di ogni colonna e assegnarlo alle variabili locali simile al seguente:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    Nel markup per il form, questi valori vengono visualizzati automaticamente in singole caselle di testo tramite codice incorporato simile al seguente:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Tale parte del codice visualizza il record di prodotto da aggiornare. Una volta che il record è stato visualizzato, l'utente può modificare le singole colonne.

    Quando l'utente invia il form facendo il **Update** pulsante, il codice nel `if(IsPost)` bloccare l'esecuzione. Questo modo si ottengono i valori dell'utente dal `Request` archivia i valori nelle variabili di oggetto e verifica che ogni colonna è stata compilata. Se la convalida riesce, il codice crea la seguente istruzione SQL Update:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    In un database SQL `Update` istruzione, si specifica ogni colonna da aggiornare e il valore per impostare l'opzione. In questo codice, i valori vengono specificati mediante i segnaposti dei parametri `@0`, `@1`, `@2`e così via. (Come indicato in precedenza, per sicurezza, è sempre opportuno passare valori a un'istruzione SQL con parametri.)

    Quando si chiama il `db.Execute` metodo, si passano le variabili che contengono i valori nell'ordine corrispondente ai parametri nell'istruzione SQL:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Dopo il `Update` istruzione è stata eseguita, si chiama il metodo seguente per indirizzare l'utente torna alla pagina di modifica:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    L'effetto è che l'utente visualizza un elenco aggiornato dei dati nel database e può modificare un altro prodotto.
6. Salvare la pagina.
7. Eseguire la *EditProducts.cshtml* pagina (non la pagina di aggiornamento) e quindi fare clic su **modificare** selezionare un prodotto da modificare. Il *UpdateProducts.cshtml* pagina viene visualizzata, che mostra il record selezionato.

    ![[immagine]](5-working-with-data/_static/image8.jpg)
8. Apportare una modifica e fare clic su **Update**. L'elenco di prodotti è illustrato anche in questo caso con i dati aggiornati.

## <a name="deleting-data-in-a-database"></a>L'eliminazione dei dati in un Database

Questa sezione illustra come consentire agli utenti di eliminare un prodotto dal *prodotto* tabella di database. L'esempio è costituito da due pagine. Nella prima pagina, gli utenti selezionare un record da eliminare. Il record da eliminare viene quindi visualizzato in una seconda pagina che consente di confermare che si desidera eliminare il record.

> [!NOTE] 
> 
> **Importante** In siti Web di produzione, è in genere limitare chi è autorizzato per apportare modifiche ai dati. Per informazioni su come configurare l'appartenenza e sui modi per autorizzare l'utente di eseguire le attività nel sito, vedere [aggiunta di sicurezza e l'appartenenza a un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Nel sito Web, creare un nuovo file con estensione CSHTML denominato *ListProductsForDelete.cshtml*.
2. Sostituire il markup esistente con il codice seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Questa pagina è simile al *EditProducts.cshtml* pagina illustrato in precedenza. Tuttavia, invece di visualizzare un **Edit** collegamento per ogni prodotto, viene visualizzato un **eliminare** collegamento. Il **eliminare** collegamento viene creato utilizzando il seguente codice incorporato nel markup:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Verrà creato un URL simile alla seguente quando l'utente sceglie il collegamento:

    `http://<server>/DeleteProduct/4`

    L'URL chiama una pagina denominata *DeleteProduct.cshtml* (che verrà creata successivamente) e lo passa l'ID del prodotto da eliminare (in questo esempio, 4).
3. Salvare il file, ma lasciarlo aperto.
4. Creare un altro file CHTML denominato *DeleteProduct.cshtml*. Sostituire il contenuto esistente con il codice seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Questa pagina viene chiamata da *ListProductsForDelete.cshtml* e consente agli utenti di confermare che si desidera eliminare un prodotto. Per elencare il prodotto deve essere eliminato, è ottenere l'ID del prodotto da eliminare dall'URL usando il codice seguente:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    Nella pagina verrà quindi richiesto all'utente di fare clic su un pulsante per eliminare effettivamente i record. Si tratta di un'importante misura di sicurezza: quando si eseguono operazioni sensibili nel sito Web, ad esempio l'aggiornamento o eliminazione dei dati, queste operazioni devono sempre essere eseguite tramite un'operazione POST, non un'operazione GET. Se il sito è configurato in modo che un'operazione di eliminazione può essere eseguita tramite un'operazione GET, chiunque può passare un URL come `http://<server>/DeleteProduct/4` e tutto ciò che desideri eliminare dal database. Aggiungendo la conferma dell'accesso e la codifica della pagina in modo che l'eliminazione può essere eseguita solo utilizzando un POST, si aggiunge una misura di sicurezza per il sito.

    L'operazione di eliminazione viene eseguita usando il codice seguente che conferma prima di tutto che questa è un'operazione post e che l'ID non sia vuoto:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Il codice esegue un'istruzione SQL che elimina il record specificato e quindi reindirizza l'utente torna alla pagina dell'inserzione.
5. Eseguire *ListProductsForDelete.cshtml* in un browser.

    ![[immagine]](5-working-with-data/_static/image9.jpg)
6. Scegliere il **eliminare** collegamento per uno dei prodotti. Il *DeleteProduct.cshtml* viene visualizzata la pagina per confermare che si desidera eliminare il record.
7. Scegliere il **eliminare** pulsante. Il record di prodotto viene eliminato e la pagina viene aggiornata con un elenco di aggiornamenti del prodotto.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>La connessione a un Database
> 
> È possibile connettersi a un database in due modi. Il primo consiste nell'usare la `Database.Open` metodo e per specificare il nome del file di database (meno la *sdf* estensione):
> 
> `var db = Database.Open("SmallBakery");`
> 
> Il `Open` metodo presuppone che il. *sdf* file è il sito Web *App\_dati* cartella. Questa cartella è progettata specificamente per contenere i dati. Ad esempio, dispone delle autorizzazioni appropriate per consentire al sito Web di leggere e scrivere dati e come misura di sicurezza, WebMatrix non consente l'accesso ai file da questa cartella.
> 
> Il secondo modo consiste nell'utilizzare una stringa di connessione. Una stringa di connessione contiene informazioni su come connettersi a un database. Può trattarsi di un percorso di file, o può includere il nome di un database di SQL Server in un server locale o remoto, insieme a un nome utente e password per connettersi a tale server. (Se si mantengono i dati in una versione di SQL Server gestita centralmente, ad esempio nel sito del provider di hosting, sempre usare una stringa di connessione per specificare le informazioni di connessione del database.)
> 
> In WebMatrix, le stringhe di connessione sono in genere archiviate in un file XML denominato *Web. config*. Come suggerisce il nome, è possibile usare una *Web. config* file nella radice del sito Web per archiviare le informazioni di configurazione del sito, inclusi eventuali stringhe di connessione che potrebbe richiedere il sito. Un esempio di una stringa di connessione in un *Web. config* file potrebbe essere simile al seguente:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> Nell'esempio, la stringa di connessione fa riferimento a un database in un'istanza di SQL Server in cui è in esecuzione in un server in un punto (anziché una variabile locale *sdf* file). È necessario sostituire i nomi appropriati per `myServer` e `myDatabase`, specificare i valori di account di accesso di SQL Server per `username` e `password`. (I valori di nome utente e password non sono necessariamente lo stesso come le credenziali di Windows o i valori che il provider di hosting ha assegnato per l'accesso al server. Rivolgersi all'amministratore per i valori esatti che necessari.)
> 
> Il `Database.Open` metodo è flessibile, poiché consente di passare il nome di un database *sdf* file o il nome di una stringa di connessione archiviata nel *Web. config* file. Nell'esempio seguente viene illustrato come connettersi al database usando la stringa di connessione illustrata nell'esempio precedente:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Come accennato, la `Database.Open` metodo consente di passare un nome di database o una stringa di connessione e possibile individuare i cui si desidera utilizzare. Ciò è molto utile quando si distribuisce (pubblica) nel sito Web. È possibile usare un *sdf* del file nei *App\_dati* cartella durante lo sviluppo e test del sito. Quando si sposta il sito in un server di produzione, è possibile usare una stringa di connessione nel *Web. config* file con lo stesso nome del *sdf* file, ma che punta al provider di hosting database &#8212;senza dover modificare il codice.
> 
> Infine, se si desidera lavorare direttamente con una stringa di connessione, è possibile chiamare il `Database.OpenConnectionString` metodo e passare è l'effettiva della stringa di connessione anziché il nome di un oggetto incluso il *Web. config* file. Ciò può risultare utile nelle situazioni in cui per qualche motivo non si ha accesso alla stringa di connessione (o valori in essa contenuti, ad esempio la *sdf* nome file) fino a quando la pagina è in esecuzione. Tuttavia, per la maggior parte degli scenari, è possibile usare `Database.Open` come descritto in questo articolo.


## <a name="additional-resources"></a>Risorse aggiuntive

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [La connessione a un Database SQL Server o MySQL in WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Convalida dell'input utente nelle pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
