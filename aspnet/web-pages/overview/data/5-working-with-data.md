---
uid: web-pages/overview/data/5-working-with-data
title: Introduzione all'uso di un Database in ASP.NET Web Pages siti (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo capitolo viene illustrato come accedere ai dati da un database e visualizzarlo in ASP.NET Web Pages.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 563074cf3e60717c2e6c336a2c282b4203f73b8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898908"
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Introduzione all'uso di un Database in ASP.NET Web Pages siti (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene descritto come utilizzare gli strumenti di Microsoft WebMatrix per creare un database in un sito Web ASP.NET Web Pages (Razor) e come creare pagine che consentono di visualizzare, aggiungere, modificare ed eliminare dati.
> 
> **Illustra quanto segue:** 
> 
> - Come creare un database.
> - Come connettersi a un database.
> - Come visualizzare i dati in una pagina web.
> - Come inserire, aggiornare ed eliminare i record del database.
> 
> Queste sono le funzionalità introdotte in questo articolo:
> 
> - Utilizzo di un database di Microsoft SQL Server Compact Edition.
> - Utilizzo di query SQL.
> - Classe `Database`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> In questa esercitazione si integra inoltre con 3 di WebMatrix. È possibile utilizzare ASP.NET Web Pages 3 e Visual Studio 2013 (o Visual Studio Express 2013 per Web) Tuttavia, l'interfaccia utente sarà diversa.


## <a name="introduction-to-databases"></a>Introduzione ai database

Si supponga una tipico di Rubrica. Per ogni voce della Rubrica (ovvero, per ogni persona) si dispone di diverse parti di informazioni quali nome, cognome, indirizzo, indirizzo di posta elettronica e numero di telefono.

Un modo comune di dati di immagine simile al seguente è una tabella con righe e colonne. In termini di database, ogni riga è noto anche come un record. Ogni colonna (talvolta detto campi) contiene un valore per ogni tipo di dati: nome del primo, ultimo nome e così via.

| **ID** | **FirstName** | **LastName** | **Indirizzo** | **Posta elettronica** | **Telefono** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Per la maggior parte delle tabelle di database, la tabella deve disporre di una colonna che contiene un identificatore univoco, ad esempio un numero cliente, numero di conto, e così via. Questo è noto come la tabella *chiave primaria*, e usarlo per identificare ogni riga nella tabella. Nell'esempio, la colonna ID è la chiave primaria per la Rubrica.

Con questa conoscenza di base dei database, si è pronti per imparare a creare un semplice database ed eseguire operazioni, ad esempio aggiunta, modifica ed eliminazione di dati.

> [!TIP] 
> 
> **Database relazionali**
> 
> È possibile archiviare i dati in molti modi, ad esempio fogli di calcolo e file di testo. Tuttavia, per la maggior parte degli usi aziendali, dati vengono archiviati in un database relazionale.
> 
> In questo articolo non passare molto eccessivamente i database. Tuttavia, può risultare utile per comprendere un po' su di essi. In un database relazionale, informazioni viene suddiviso logicamente in tabelle separate. Ad esempio, un database per un istituto di istruzione potrebbe contenere tabelle separate per studenti e per le offerte di classe. I software (ad esempio SQL Server) supporta potente i comandi di database che consentono di dinamicamente stabiliscono relazioni tra le tabelle. Ad esempio, è possibile utilizzare il database relazionale per stabilire una relazione logica tra studenti e le classi per creare una pianificazione. L'archiviazione dei dati in tabelle separate riduce la complessità della struttura di tabella e riduce la necessità di mantenere i dati ridondanti nelle tabelle.


## <a name="creating-a-database"></a>Creazione di un Database

Questa procedura viene illustrato come creare un database denominato SmallBakery utilizzando lo strumento di progettazione di database di SQL Server Compact Edition è incluso in WebMatrix. Sebbene sia possibile creare un database tramite codice, è più comune per creare il database e tabelle di database utilizzando uno strumento di progettazione come WebMatrix.

1. Avviare WebMatrix e nella pagina avvio rapido, fare clic su **del sito da un modello**.
2. Selezionare **sito vuoto**e il **nome sito** casella immettere "SmallBakery" e quindi fare clic su **OK**. Il sito verrà creato e visualizzato in WebMatrix.
3. Nel riquadro a sinistra, fare clic su di **database** dell'area di lavoro.
4. Nella barra multifunzione, fare clic su **Nuovo Database**. Viene creato un database vuoto con lo stesso nome del sito.
5. Nel riquadro sinistro, espandere il **SmallBakery.sdf** nodo e quindi fare clic su **tabelle**.
6. Nella barra multifunzione, fare clic su **nuova tabella**. WebMatrix consente di aprire Progettazione tabelle.

    ![[immagine]](5-working-with-data/_static/image1.jpg)
7. Fare clic il **nome** colonna e immettere &quot;Id&quot;.
8. Nel **tipo di dati** colonna, selezionare **int**.
9. Impostare il **è chiave primaria?** e **è identificare?** opzioni su **Sì**.

    Come suggerisce il nome, **chiave primaria è** indica il database che si tratterà di chiave primaria della tabella. **Identità** indica il database da creare automaticamente un numero di ID per ogni nuovo record e per assegnare il successivo numero sequenziale (a partire da 1).
10. Fare clic nella riga successiva. L'editor avvia una nuova definizione di colonna.
11. Per il valore del nome, immettere &quot;nome&quot;.
12. Per **tipo di dati**, scegliere &quot;nvarchar&quot; e impostare la lunghezza su 50. Il *var* parte `nvarchar` indica il database che i dati per questa colonna sarà una stringa il cui dimensioni possono variare da un record a altro. (Il *n* prefisso rappresenta *national*, che indica che il campo può contenere dati di tipo carattere che rappresenta qualsiasi carattere alfabetico o sistema di scrittura &#8212; vale a dire che il campo contiene i dati Unicode.)
13. Impostare il **Ammetti** opzione **n**. Impone che il *nome* colonna non è vuoto.
14. Utilizzando lo stesso processo, creare una colonna denominata *descrizione*. Impostare **tipo di dati** "nvarchar" e 50 per la lunghezza, quindi impostare **Ammetti** su false.
15. Creare una colonna denominata *prezzo*. Impostare **tipo di dati "money"** e impostare **Ammetti** su false.
16. Nella casella superiore, denominare la tabella &quot;prodotto&quot;.

    Al termine, la definizione avrà un aspetto simile al seguente:

    ![[immagine]](5-working-with-data/_static/image2.jpg)
17. Premere Ctrl + S per salvare la tabella.

## <a name="adding-data-to-the-database"></a>Aggiunta di dati al Database

È ora possibile aggiungere alcuni dati di esempio al database che verranno utilizzate in un secondo momento nell'articolo.

1. Nel riquadro sinistro, espandere il **SmallBakery.sdf** nodo e quindi fare clic su **tabelle**.
2. Il pulsante destro della tabella Product e quindi fare clic su **dati**.
3. Nel riquadro di modifica, immettere i record seguenti:

    | **Name** | **Descrizione** | **Prezzo** |
    | --- | --- | --- |
    | Pane | Salvati sono aggiornate ogni giorno. | 2.99 |
    | Shortcake alla | Eseguita con fragole organiche dal nostro garden. | 9.99 |
    | Grafico a torta Apple | Secondo solo ai grafici a torta di mom. | 12.99 |
    | Grafico a torta Pecan | Se si desidera del Queenland, questo viene automaticamente. | 10.99 |
    | Grafico a torta al limone | Effettuata con il migliori limoni nel mondo. | 11.99 |
    | Tortine | I bambini e kid è apprezzeranno questi. | 7.99 |

    Tenere presente che non è necessario immettere un valore per il *Id* colonna. Quando è stato creato il *Id* colonna, impostare il relativo **identità** proprietà su true, in modo che essere compilato automaticamente.

    Terminata l'immissione dei dati, Progettazione tabelle avrà un aspetto simile al seguente:

    ![[immagine]](5-working-with-data/_static/image3.jpg)
4. Chiudere la scheda che contiene i dati del database.

## <a name="displaying-data-from-a-database"></a>Visualizzazione dei dati da un Database

Dopo aver ottenuto un database con i dati in essa contenuti, è possibile visualizzare i dati in una pagina web ASP.NET. Per selezionare le righe della tabella per visualizzare, utilizzare un'istruzione SQL, ovvero un comando che viene passato al database.

1. Nel riquadro a sinistra, fare clic su di **file** dell'area di lavoro.
2. Nella radice del sito Web, creare una nuova pagina CSHTML denominata *ListProducts.cshtml*.
3. Sostituire il codice esistente con il seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    Nel primo blocco di codice, si apre il *SmallBakery.sdf* file (database) creato in precedenza. Il `Database.Open` metodo presuppone che il *sdf* file è di un sito Web *App\_dati* cartella. (Si noti che non è necessario specificare il *sdf* estensione &#8212; in effetti, in caso contrario, il `Open` (metodo) non funzionerà.)

    > [!NOTE]
    > Il *App\_dati* cartella è una cartella speciale di ASP.NET che viene utilizzato per archiviare i file di dati. Per ulteriori informazioni, vedere [connessione a un Database](#SB_ConnectingToADatabase) più avanti in questo articolo.

    È quindi effettuare una richiesta per eseguire query sul database utilizzando l'istruzione SQL seguente `Select` istruzione:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    Nell'istruzione, `Product` identifica la tabella alla query. Il `*` carattere specifica che la query deve restituire tutte le colonne dalla tabella. (È stato possibile anche elenco colonne singolarmente, separati da virgole, se si desidera visualizzare solo alcune colonne.) Il `Order By` clausola indica la modalità di ordinamento dei dati &#8212; in questo caso, per il *nome* colonna. Ciò significa che i dati sono ordinati in ordine alfabetico in base al valore del *nome* colonna per ogni riga.

    Nel corpo della pagina, il codice crea una tabella HTML che verrà utilizzata per visualizzare i dati. All'interno di `<tbody>` elemento, utilizzare un `foreach` ciclo per ottenere individualmente ogni riga di dati restituito dalla query. Per ogni riga di dati, si crea una riga della tabella HTML (`<tr>` elemento). Quindi creare le celle di tabella HTML (`<td>` elementi) per ogni colonna. Ogni volta che si visita il ciclo, la successiva riga disponibile dal database è nel `row` variabile (a tale scopo `foreach` istruzione). Per ottenere una singola colonna della riga, è possibile utilizzare `row.Name` o `row.Description` o il nome di qualsiasi è della colonna.
4. Eseguire la pagina in un browser. (Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.) La pagina Visualizza un elenco simile al seguente:

    ![[immagine]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL è un linguaggio che viene utilizzato nella maggior parte dei database relazionali per la gestione dei dati in un database. Include i comandi che consentono di recuperare i dati e l'aggiornamento e che consentono di creare, modificare e gestire le tabelle di database. SQL è diverso da un linguaggio di programmazione (simile a quello in uso in WebMatrix). con SQL, l'idea è che viene comunicato al database desiderato, ed è il processo del database per scoprire come ottenere i dati o eseguire l'attività. Di seguito sono riportati esempi di alcuni comandi SQL e le operazioni eseguite:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Si recupera il *Id*, *nome*, e *prezzo* colonne dai record di *prodotto* tabella se il valore di *prezzo* è maggiore di 10 e restituisce i risultati in ordine alfabetico in base ai valori del *nome* colonna. Questo comando restituisce un set di risultati che contiene i record che soddisfano i criteri, o un set vuoto se nessun record corrispondente.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Inserisce un nuovo record nel *prodotto* tabella, l'impostazione di *nome* colonna &quot;"croissant"&quot;, *descrizione* colonna &quot; Un piacere affidabile&quot;e il prezzo 1.99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Questo comando Elimina i record di *prodotto* tabella la cui colonna Data di scadenza è precedente al 1 gennaio 2008. (Si presuppone che il *prodotto* tabella include una colonna, ovviamente.) La data non viene immesso nel formato MM/GG/AAAA, ma deve essere immesso nel formato utilizzato per le impostazioni locali.
> 
> Il `Insert Into` e `Delete` comandi non restituiscono set di risultati. Invece, restituiscono un numero che indica il numero di record è interessato dal comando.
> 
> Per alcune di queste operazioni (ad esempio, inserimento ed eliminazione di record), il processo che richiede l'operazione deve disporre delle autorizzazioni appropriate nel database. Questo è il motivo per il database di produzione è spesso necessario fornire un nome utente e una password quando ci si connette al database.
> 
> Esistono numerosi comandi SQL, ma seguono un modello simile al seguente. È possibile utilizzare i comandi SQL per creare tabelle di database, contare il numero di record in una tabella, calcolare prezzi e molte altre operazioni.


## <a name="inserting-data-in-a-database"></a>Inserimento di dati in un Database

In questa sezione viene illustrato come creare una pagina che consente agli utenti di aggiungere un nuovo prodotto di *prodotto* tabella di database. Dopo l'inserimento di un nuovo record di prodotto, la pagina Visualizza la tabella aggiornata utilizzando il *ListProducts.cshtml* pagina creata nella sezione precedente.

La pagina include la convalida per assicurarsi che i dati immessi dall'utente siano validi per il database. Ad esempio, il codice nella pagina assicura che sia stato immesso un valore per tutte le colonne necessarie.

1. Nel sito Web, creare un nuovo file CSHTML denominato *InsertProducts.cshtml*.
2. Sostituire il codice esistente con il seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Il corpo della pagina contiene un modulo HTML con tre caselle di testo che consentono agli utenti di immettere un nome, descrizione e prezzo. Quando gli utenti fanno clic il **inserire** pulsante, il codice nella parte superiore della pagina consente di aprire una connessione per il *SmallBakery.sdf* database. È quindi ottenere i valori che l'utente è inviata tramite il `Request` dell'oggetto e assegnare i valori alle variabili locali.

    Per verificare che l'utente ha immesso un valore per ogni colonna richiesta, ogni registrare `<input>` elemento che si desidera convalidare:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    Il `Validation` helper controlla che sia presente un valore in ognuno dei campi che è registrato. È possibile verificare se tutti i campi ha superato la convalida controllando `Validation.IsValid()`, che è in genere eseguire prima di elaborare le informazioni ottenute da parte dell'utente:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (Il `&&` significa operatore AND, questo test è *se questa è l'invio di un form e tutti i campi sono stati convalidati*.)

    Se tutte le colonne convalidato (Nessuno era vuoto), si procede e creare un'istruzione SQL per inserire i dati e quindi eseguirlo come indicato di seguito:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Per i valori da inserire, includere i segnaposto dei parametri (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Come misura di sicurezza, passare sempre i valori per un'istruzione SQL con parametri, come può vedere nell'esempio precedente. Questo offre la possibilità per convalidare i dati dell'utente, oltre consente di proteggere contro i tentativi di invio di comandi dannosi per il database (talvolta detto attacchi SQL injection).

    Per eseguire la query, si utilizza questa istruzione, passando le variabili che contengono i valori da sostituire i segnaposto:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Dopo il `Insert Into` è stata eseguita un'istruzione, si indirizza l'utente alla pagina che elenca i prodotti utilizzando questa riga:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Se la convalida non è riuscito, ignorando l'inserimento. Un helper è invece presente nella pagina che è possibile visualizzare i messaggi di errore accumulato (se presente):

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Si noti che il blocco di stile nel codice include una definizione di classe CSS denominata `.validation-summary-errors`. Si tratta del nome della classe CSS che viene utilizzato per impostazione predefinita per il `<div>` elemento che contiene gli errori di convalida. In questo caso, la classe CSS specifica che gli errori di riepilogo di convalida vengono visualizzati in rosso e in grassetto, ma è possibile definire la `.validation-summary-errors` classe per visualizzare qualsiasi formattazione desiderati.

### <a name="testing-the-insert-page"></a>Test della pagina di inserimento

1. Visualizzare la pagina in un browser. Nella pagina viene visualizzato un form che è simile a quella illustrata nella figura seguente.

    ![[immagine]](5-working-with-data/_static/image5.jpg)
2. Immettere i valori per tutte le colonne, ma assicurarsi di lasciare il *prezzo* colonna vuota.
3. Fare clic su **Inserisci**. La pagina Visualizza un messaggio di errore, come illustrato nella figura seguente. (Nessun nuovo record creato).

    ![[immagine]](5-working-with-data/_static/image6.jpg)
4. Compilare il modulo completamente e quindi fare clic su **inserire**. Questa volta il *ListProducts.cshtml* pagina viene visualizzata e viene mostrato il nuovo record.

## <a name="updating-data-in-a-database"></a>Aggiornamento dei dati in un Database

Dopo che sono stati immessi dati in una tabella, potrebbe essere necessario aggiornarlo. In questa procedura viene illustrato come creare due pagine che sono simili a quelli creati per versioni precedenti l'inserimento di dati. La prima pagina Visualizza i prodotti e consente agli utenti di selezionarne uno da modificare. La seconda pagina consente agli utenti effettivamente apportare le modifiche e salvarle.

> [!NOTE] 
> 
> **Importante** In un sito Web di produzione, è in genere limitare chi è consentito per apportare modifiche ai dati. Per informazioni su come configurare l'appartenenza e sui modi per consentire agli utenti di eseguire attività nel sito, vedere [sicurezza aggiunta e l'appartenenza a un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Nel sito Web, creare un nuovo file CSHTML denominato *EditProducts.cshtml*.
2. Sostituire il codice esistente nel file con le operazioni seguenti:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    L'unica differenza tra questa pagina e *ListProducts.cshtml* pagina da versioni precedenti è che la tabella HTML in questa pagina include una colonna aggiuntiva che visualizza un **modifica** collegamento. Quando si fa clic su questo collegamento, necessario per il *UpdateProducts.cshtml* pagina (che si creerà quindi) in cui è possibile modificare il record selezionato.

    Esaminare il codice che crea il **modifica** collegamento:

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Crea un elemento HTML `<a>` elemento il cui `href` attributo è impostato in modo dinamico. Il `href` attributo specifica la pagina da visualizzare quando l'utente fa clic sul collegamento. Inoltre, viene passato il `Id` valore della riga corrente per il collegamento. Quando la pagina viene eseguita, l'origine della pagina potrebbe contenere collegamenti simili:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Si noti che il `href` attributo è impostato su `UpdateProducts/n`, dove *n* è un numero di prodotto. Quando un utente fa clic su uno di questi collegamenti, l'URL risulta avrà un aspetto simile al seguente:

    `http://localhost:18816/UpdateProducts/6`

    In altre parole, il numero del prodotto per la modifica verrà passato nell'URL.
3. Visualizzare la pagina in un browser. La pagina Visualizza i dati in un formato simile al seguente:

    ![[immagine]](5-working-with-data/_static/image7.jpg)

    Successivamente, si creerà la pagina che consente agli utenti di aggiornare effettivamente i dati. La pagina di aggiornamento include la convalida per convalidare i dati immessi dall'utente. Ad esempio, il codice nella pagina assicura che sia stato immesso un valore per tutte le colonne necessarie.
4. Nel sito Web, creare un nuovo file CSHTML denominato *UpdateProducts.cshtml*.
5. Sostituire il codice esistente nel file con il codice seguente.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Il corpo della pagina contiene un form HTML in cui viene visualizzato un prodotto e in cui gli utenti possono modificare. Per ottenere il prodotto per visualizzare, utilizzare l'istruzione SQL seguente:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Ciò consentirà di selezionare il prodotto il cui ID corrisponde al valore che viene passato il `@0` parametro. (Poiché *Id* è la chiave primaria e pertanto deve essere univoco, il record di un solo prodotto mai selezionabile in questo modo.) Per ottenere il valore di ID da passare a questo `Select` istruzione, è possibile leggere il valore passato alla pagina come parte dell'URL, utilizzando la sintassi seguente:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Per recuperare effettivamente il record di prodotto, si utilizza il `QuerySingle` metodo, che verrà restituito un solo record:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    La singola riga viene restituita nel `row` variabile. È possibile ottenere dati all'esterno di ogni colonna e assegnarla a variabili locali simile al seguente:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    Nel markup per il form, questi valori vengono visualizzati automaticamente in singole caselle di testo usando un codice incorporato simile al seguente:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    La parte del codice consente di visualizzare il record di prodotto da aggiornare. Una volta che il record è stato visualizzato, l'utente può modificare le singole colonne.

    Quando l'utente invia il form, fare clic sul **aggiornamento** pulsante, il codice di `if(IsPost)` blocco viene eseguito. Ottiene i valori dell'utente dal `Request` archivia i valori nelle variabili di oggetto e verifica che ogni colonna è stato compilato. Se la convalida viene eseguita, il codice crea l'istruzione SQL Update seguente:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    In un database SQL `Update` istruzione, specificare ogni colonna da aggiornare e il valore per impostare l'opzione. In questo codice, i valori vengono specificati mediante i segnaposti dei parametri `@0`, `@1`, `@2`e così via. (Come indicato in precedenza, per la sicurezza, è sempre opportuno passare valori a un'istruzione SQL con parametri.)

    Quando si chiama il `db.Execute` (metodo), con cui si passano le variabili che contengono i valori nell'ordine in cui corrisponde ai parametri nell'istruzione SQL:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Dopo il `Update` è stata eseguita l'istruzione, chiamare il metodo seguente per reindirizzare l'utente alla pagina di modifica:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    L'effetto è che l'utente visualizza un elenco aggiornato dei dati nel database e possa modificare un altro prodotto.
6. Salvare la pagina.
7. Eseguire il *EditProducts.cshtml* pagina (non la pagina di aggiornamento) e quindi fare clic su **modifica** per selezionare un prodotto da modificare. Il *UpdateProducts.cshtml* verrà visualizzata la pagina, che mostra il record selezionato.

    ![[immagine]](5-working-with-data/_static/image8.jpg)
8. Apportare una modifica e fare clic su **aggiornamento**. L'elenco dei prodotti viene nuovamente visualizzato con i dati aggiornati.

## <a name="deleting-data-in-a-database"></a>L'eliminazione dei dati in un Database

In questa sezione viene illustrato come consentire agli utenti di eliminare un prodotto dal *prodotto* tabella di database. L'esempio è costituito da due pagine. Nella prima pagina, gli utenti selezionare un record da eliminare. Il record da eliminare viene quindi visualizzato in un'altra pagina che consente di confermare che si desidera eliminare il record.

> [!NOTE] 
> 
> **Importante** In un sito Web di produzione, è in genere limitare chi è consentito per apportare modifiche ai dati. Per informazioni su come configurare l'appartenenza e sulle modalità di autorizzazione utente per eseguire attività nel sito, vedere [sicurezza aggiunta e l'appartenenza a un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Nel sito Web, creare un nuovo file CSHTML denominato *ListProductsForDelete.cshtml*.
2. Sostituire il codice esistente con il seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Questa pagina è simile al *EditProducts.cshtml* pagina precedenti. Tuttavia, anziché visualizzare un' **modifica** collegamento per ogni prodotto, viene visualizzato un **eliminare** collegamento. Il **eliminare** viene creato utilizzando il seguente codice incorporato nel markup di collegamento:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Consente di creare un URL simile al seguente quando si fa clic sul collegamento:

    `http://<server>/DeleteProduct/4`

    L'URL chiama una pagina denominata *DeleteProduct.cshtml* (che si creerà quindi) e lo passa l'ID del prodotto da eliminare (in questo esempio, 4).
3. Salvare il file, ma lasciarlo aperto.
4. Creare un altro file CHTML denominato *DeleteProduct.cshtml*. Sostituire il contenuto esistente con il seguente:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Questa pagina viene chiamata da *ListProductsForDelete.cshtml* e consente agli utenti di confermare che si desidera eliminare un prodotto. Per elencare il prodotto da eliminare, è visualizzato l'ID del prodotto da eliminare dall'URL tramite il codice seguente:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    Nella pagina verrà quindi richiesto all'utente di fare clic su un pulsante per eliminare effettivamente il record. Si tratta di una misura di sicurezza importante: quando si eseguono operazioni nel sito Web quali l'aggiornamento o eliminazione di dati, queste operazioni devono sempre essere eseguite tramite un'operazione di POST, non un'operazione GET. Se il sito è configurato in modo che un'operazione di eliminazione può essere eseguita mediante un'operazione GET, chiunque può passare un URL come `http://<server>/DeleteProduct/4` ed eliminare tutte le applicazioni desiderate dal database. Aggiungendo il messaggio di conferma e codificare la pagina in modo che l'eliminazione può essere eseguita solo utilizzando un POST, aggiungere una misura di sicurezza del sito.

    L'operazione di eliminazione effettiva viene eseguita utilizzando il codice seguente, che verifica innanzitutto che si tratta di un'operazione post e che l'ID non è vuoto:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Il codice esegue un'istruzione SQL che elimina il record specificato e che quindi reindirizza l'utente torna alla pagina di elenco.
5. Eseguire *ListProductsForDelete.cshtml* in un browser.

    ![[immagine]](5-working-with-data/_static/image9.jpg)
6. Fare clic su di **eliminare** collegamento per uno dei prodotti. Il *DeleteProduct.cshtml* viene visualizzata la pagina per confermare che si desidera eliminare il record.
7. Fare clic su di **eliminare** pulsante. Viene eliminato il record di prodotto e la pagina viene aggiornata con un elenco di aggiornamenti del prodotto.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Connessione a un Database
> 
> È possibile connettersi a un database in due modi. Il primo consiste nell'usare il `Database.Open` (metodo) e per specificare il nome del file di database (meno di *sdf* estensione):
> 
> `var db = Database.Open("SmallBakery");`
> 
> Il `Open` metodo presuppone che il. *sdf* file è il sito Web *App\_dati* cartella. Questa cartella è progettata specificamente per contenere i dati. Ad esempio, disponga delle autorizzazioni appropriate per consentire il sito Web leggere e scrivere dati e come misura di sicurezza, WebMatrix non consente l'accesso ai file da questa cartella.
> 
> Il secondo modo consiste nell'utilizzare una stringa di connessione. Una stringa di connessione contiene informazioni su come connettersi a un database. Può trattarsi di un percorso di file o può includere il nome di un database di SQL Server in un server locale o remoto, insieme a un nome utente e una password per connettersi al server. (Se si mantengono i dati in una versione di SQL Server gestita centralmente, ad esempio sul sito del provider di hosting, è sempre utilizzare una stringa di connessione per specificare le informazioni di connessione di database.)
> 
> In WebMatrix, le stringhe di connessione sono in genere archiviate in un file XML denominato *Web. config*. Come suggerisce il nome, è possibile utilizzare un *Web. config* file nella radice del sito Web per archiviare le informazioni di configurazione del sito, inclusi eventuali stringhe di connessione che può richiedere il sito. Un esempio di una stringa di connessione in un *Web. config* file potrebbe essere simile al seguente:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> Nell'esempio, la stringa di connessione punta a un database in un'istanza di SQL Server in esecuzione in un server in un punto (anziché una variabile locale *sdf* file). È necessario sostituire i nomi appropriati per `myServer` e `myDatabase`e specificare i valori di account di accesso di SQL Server per `username` e `password`. (I valori di nome utente e password non sono necessariamente lo stesso come le credenziali di Windows o i valori che il provider di hosting ha assegnato per l'accesso ai server. Rivolgersi all'amministratore per i valori esatti che necessari.)
> 
> Il `Database.Open` metodo è flessibile, in quanto consente di passare il nome di un database *sdf* file o il nome di una stringa di connessione che viene archiviato nel *Web. config* file. Nell'esempio seguente viene illustrato come connettersi al database utilizzando la stringa di connessione come illustrata nell'esempio precedente:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Come già indicato, il `Database.Open` metodo consente di passare un nome di database o una stringa di connessione e calcolerà da usare. È molto utile quando si distribuisce (pubblica) nel sito Web. È possibile utilizzare un *sdf* file nel *App\_dati* cartella quando si esegue lo sviluppo e il test del sito. Quando si sposta il sito in un server di produzione, è possibile utilizzare una stringa di connessione nel *Web. config* file con lo stesso nome del *sdf* file ma che punta al provider di hosting database &#8212;tutto senza dover modificare il codice.
> 
> Infine, se si desidera lavorare direttamente con una stringa di connessione, è possibile chiamare il `Database.OpenConnectionString` (metodo) e l'effettiva connessione stringa anziché solo il nome di uno nel passaggio di *Web. config* file. Questo potrebbe essere utile nelle situazioni in cui per qualche motivo non si ha accesso alla stringa di connessione (o i valori in essa contenuti, ad esempio il *sdf* nome file) fino a quando la pagina è in esecuzione. Per la maggior parte degli scenari, tuttavia, è possibile utilizzare `Database.Open` come descritto in questo articolo.


## <a name="additional-resources"></a>Risorse aggiuntive

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Connessione a un Database SQL Server o MySQL in WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Convalida dell'input utente nelle pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
