---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Introduzione a pagine Web ASP.NET - aggiornamento di dati del Database | Microsoft Docs
author: tfitzmac
description: Questa esercitazione illustra come aggiornare la voce (modifica) un database esistente quando si usa ASP.NET Web Pages (Razor). Si presuppone di aver completato la serie th...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 9d0da2eed9964e56a01f3c811d6a14c9d1735e10
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823967"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>Introduzione a pagine Web ASP.NET - aggiornamento di dati del Database
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra come aggiornare la voce (modifica) un database esistente quando si usa ASP.NET Web Pages (Razor). Si presuppone di aver completato la serie attraverso [immissione di dati da usando form con ASP.NET Web Pages](entering-data.md).
> 
> Che cosa si apprenderà come:
> 
> - Come selezionare un singolo record nel `WebGrid` helper.
> - Come leggere un singolo record da un database.
> - Come caricare un modulo con i valori del record del database in background.
> - Come aggiornare un record esistente in un database.
> - Come archiviare le informazioni nella pagina senza visualizzarla.
> - Come usare un campo nascosto per archiviare le informazioni.
>   
> 
> Le funzionalità/tecnologie illustrate:
> 
> - Il `WebGrid` helper.
> - Il codice SQL `Update` comando.
> - Metodo `Database.Execute`.
> - Campi nascosti (`<input type="hidden">`).


## <a name="what-youll-build"></a>Scopo dell'esercitazione

Nell'esercitazione precedente, è stato descritto come aggiungere un record a un database. In questo caso, si apprenderà come visualizzare un record per la modifica. Nel *film* pagina, si aggiornerà il `WebGrid` helper, in modo che venga visualizzato un **modifica** collegamento accanto a ogni film:

![WebGrid visualizzare con un collegamento "Modifica" per ogni film](updating-data/_static/image1.png)

Quando si sceglie la **modifica** collegamento, consente a un'altra pagina, in cui le informazioni relative al filmato è già in un form:

![Modifica pagina film che mostra di film da modificare](updating-data/_static/image2.png)

È possibile modificare i valori. Quando si inviano le modifiche, il codice nella pagina Aggiorna il database e consente di tornare all'elenco di film.

Questa parte del processo funziona quasi esattamente come le *AddMovie.cshtml* pagina creata nell'esercitazione precedente, in modo che la maggior parte di questa esercitazione sarà familiare.

Esistono diversi modi, è possibile implementare un metodo per modificare un film singoli. L'approccio illustrato è stato scelto perché è facile da implementare e facile da comprendere.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Aggiunta di un collegamento di modifica per l'elenco di film

Per iniziare, si aggiornerà il *film* pagina in modo che ogni film anche l'elenco contenga un' **modificare** collegamento.

Aprire il *Movies.cshtml* file.

Nel corpo della pagina, modificare il `WebGrid` markup mediante l'aggiunta di una colonna. Ecco il markup modificato:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

La nuova colonna è questa:

[!code-html[Main](updating-data/samples/sample2.html)]

Il punto di questo articolo è mostrare un collegamento (`<a>` elemento) con il testo "Modifica". Siamo dopo consiste nel creare un collegamento che è simile al seguente quando viene eseguita la pagina, con la `id` valore diverso per ogni film:

[!code-css[Main](updating-data/samples/sample3.css)]

Questo collegamento verrà richiamato una pagina denominata *EditMovie*, e lo passerà la stringa di query `?id=7` a tale pagina.

La sintassi per la nuova colonna potrebbe sembrare un po' complessa, ma solo perché scoperte diversi elementi. Ogni singolo elemento è semplice. Se concentrerà sul solo il `<a>` elemento, noterete che questo markup:

[!code-html[Main](updating-data/samples/sample4.html)]

Alcune informazioni di base sul funzionamento della griglia: nella griglia vengono visualizzate righe, una per ogni record di database, e visualizza le colonne per ogni campo nel record del database. Mentre viene creato ogni riga della griglia, il `item` oggetto contiene i record del database (elemento) per quella riga. Questa disposizione consente di ottenere i dati per la riga di codice. Questo è ciò che vedete qui: l'espressione `item.ID` riceve il valore ID della voce di database corrente. È possibile ottenere i valori del database (title, genre o year) allo stesso modo usando `item.Title`, `item.Genre`, o `item.Year`.

L'espressione `"~/EditMovie?id=@item.ID` combina la parte dell'URL di destinazione a livello di codice (`~/EditMovie?id=`) riferendo questo ID in modo dinamico derivato. (Si è visto il `~` operatore nell'esercitazione precedente; è un operatore di ASP.NET che rappresenta la radice del sito Web corrente.)

Il risultato è che questa parte del markup nella colonna semplicemente produce un risultato analogo il markup seguente in fase di esecuzione:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Naturalmente, il valore effettivo del `id` sarà diverso per ogni riga.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Creazione di una visualizzazione personalizzata per una colonna della griglia

A questo punto eseguire il backup per la colonna della griglia. Le tre colonne in origine era la griglia visualizzata solo i valori dei dati (titolo, il genere e anno). Questa visualizzazione è specificato passando il nome della colonna del database &mdash; ad esempio, `grid.Column("Title")`.

Questa nuova **modifica** colonna di collegamento è diverso. Anziché specificare un nome di colonna, si sta passando un `format` parametro. Questo parametro consente di definire markup che il `WebGrid` helper verrà eseguito il rendering con il `item` valore per visualizzare i dati della colonna in grassetto o colore verde o in qualsiasi formato che si desidera. Ad esempio, se si vuole ottenere il titolo da visualizzare in grassetto, è possibile creare una colonna come in questo esempio:

[!code-html[Main](updating-data/samples/sample6.html)]

(I vari `@` caratteri presenti il `format` proprietà contrassegnare la transizione tra i tag e un valore del codice.)

Dopo aver appreso informazioni sul `format` proprietà, è più facile da comprendere come le nuove **modifica** colonna di collegamento viene raccolto:

[!code-html[Main](updating-data/samples/sample7.html)]

La colonna è costituita da *solo* del markup che esegue il rendering di collegamento, inoltre alcune informazioni (ID) che viene estratto il record del database per la riga.

> [!TIP]
> 
> **I parametri denominati e i parametri posizionali per un metodo**
> 
> Numero di volte dopo aver chiamato un metodo e passati parametri a esso, è sufficiente elencati i valori dei parametri separati da virgole. Di seguito sono riportati alcuni esempi:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> È non è importante tenere presente il problema quando si è visto prima di tutto questo codice, ma in ogni caso, si sta passando i parametri ai metodi in un ordine specifico &mdash; vale a dire, l'ordine in cui i parametri sono definiti in tale metodo. Per la `db.Execute` e `Validation.RequireFields`, se combinare l'ordine dei valori passati, si otterrebbe un messaggio di errore quando la pagina viene eseguito, o almeno alcuni risultati imprevisti. Ovviamente, è necessario conoscere l'ordine per passare i parametri in. (In WebMatrix, IntelliSense aiutano individuare il nome, tipo e ordine dei parametri.)
> 
> Come alternativa al passaggio di valori in ordine, è possibile usare *parametri denominati*. (Il passaggio di parametri nell'ordine è nota come utilizzo *parametri posizionali*.) Per i parametri denominati, si include in modo esplicito il nome del parametro quando si passa il relativo valore. Si usa parametri denominati già un numero di volte in queste esercitazioni. Ad esempio:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> e
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> I parametri denominati sono utili per un paio di casi, specialmente quando un metodo accetta numerosi parametri. Uno è quando si desidera passare solo uno o due parametri, ma i valori che si desidera passare non sono tra le posizioni di primo nell'elenco dei parametri. Un'altra situazione è quando si vuole rendere il codice più leggibile, passando i parametri nell'ordine in cui la scelta più sensata all'utente.
> 
> Ovviamente, per usare i parametri denominati, è necessario conoscere i nomi dei parametri. WebMatrix IntelliSense possono *mostrare* si i nomi, ma non è attualmente completarle automaticamente.


## <a name="creating-the-edit-page"></a>Creazione della pagina Edit

Ora è possibile creare il *EditMovie* pagina. Quando gli utenti fanno clic la **modifica** collegamento, si otterrà in questa pagina.

Creare una pagina denominata *EditMovie.cshtml* e sostituire quello presente nel file con il markup seguente:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Questo markup e codice è simile a quello che hai *AddMovie* pagina. È una piccola differenza nel testo per il pulsante di invio. Come con la *AddMovie* pagina, è presente un `Html.ValidationSummary` chiamata che verrà visualizzati gli errori di convalida se presenti. Questa volta abbiamo stiamo escludendo le chiamate a `Validation.Message`, dal momento che gli errori vengono visualizzati nel riepilogo di convalida. Come indicato nell'esercitazione precedente, è possibile usare il riepilogo di convalida e i singoli messaggi di errore in varie combinazioni.

Si noti anche in questo caso che la `method` attributo del `<form>` elemento è impostato su `post`. Come con le *AddMovie.cshtml* pagina, questa pagina apporta le modifiche al database. Pertanto, questo modulo è necessario eseguire un `POST` operazione. (Per altre informazioni sulla differenza tra `GET` e `POST` operazioni, vedere la [GET, POST e HTTP verbo Safety](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) intestazione laterale nell'esercitazione nei form HTML.)

Come si è visto in un'esercitazione precedente, il `value` vengono impostati gli attributi delle caselle di testo con il codice Razor per precaricare li. Stavolta, però, si usa variabili come `title` e `genre` per tale attività anziché `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Come prima, questo markup verrà precaricare i valori della casella di testo con i valori di film. Verrà visualizzato a breve perché è utile usare le variabili, questa volta invece di usare il `Request` oggetto.

È inoltre disponibile un `<input type="hidden">` elemento in questa pagina. Questo elemento archivia l'ID del film senza renderlo visibile nella pagina. L'ID viene passato inizialmente per la pagina in base usando un valore di stringa di query (`?id=7` o simili nell'URL). Inserendo il valore ID in un campo nascosto, è possibile assicurarsi che sia disponibile quando il form viene inviato, anche se non è più necessario l'accesso all'URL originale della pagina è stata richiamata con.

A differenza di *AddMovie* pagina, il codice per il *EditMovie* pagina contiene due funzioni distinte. La prima funzione è che, quando la pagina viene visualizzata per la prima volta (e *solo* quindi), il codice ottiene l'ID del film dalla stringa di query. Il codice quindi utilizza l'ID per leggere il film corrispondente all'esterno del database e visualizzare (Precarica), nelle caselle di testo.

La seconda funzione è che quando l'utente sceglie il **Invia modifiche** pulsante, il codice deve leggere i valori di caselle di testo e la successiva convalida. Il codice ha anche aggiornare l'elemento di database con i nuovi valori. Questa tecnica è analoga all'aggiunta di un record, come illustrato nelle *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Aggiunta di codice per leggere un singolo filmato

Per eseguire la prima funzione, aggiungere questo codice nella parte superiore della pagina:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

La maggior parte di questo codice si trova all'interno di un blocco che inizia `if(!IsPost)`. Il `!` operatore significa "not", in modo che l'espressione significa *se la richiesta non ha un invio post*, che costituisce un metodo indiretto viene espresso indicando *se la richiesta è la prima volta che questa pagina è stata eseguita*. Come indicato in precedenza, questo codice deve essere eseguito *solo* alla prima esecuzione della pagina. Se si non racchiudere il codice in `if(!IsPost)`, viene eseguito ogni volta che la pagina viene richiamata, se la prima volta oppure in risposta a un pulsante fare clic su.

Si noti che il codice include un `else` bloccare questo momento. Come detto in precedenza quando è stato introdotto `if` blocchi, talvolta si desidera eseguire codice alternativo se non viene soddisfatta la condizione che si sta testando. Ovvero in questo caso. Se la condizione ha esito positivo (ovvero, se l'ID passato alla pagina è ok), leggere una riga dal database. Tuttavia, se la condizione non viene superato, il `else` blocco viene eseguito e il codice imposta un messaggio di errore.

## <a name="validating-a-value-passed-to-the-page"></a>La convalida di un valore passato alla pagina

Il codice Usa `Request.QueryString["id"]` per ottenere l'ID che viene passato alla pagina. Il codice consente di verificare che in realtà è stato passato un valore per l'ID. Se è stato passato alcun valore, il codice imposta un errore di convalida.

Questo codice illustra un modo diverso per convalidare le informazioni. Nell'esercitazione precedente, si è lavorato con i `Validation` helper. I campi per la convalida è stata registrata, ASP.NET automaticamente e ha la convalida visualizzati gli errori usando `Html.ValidationMessage` e `Html.ValidationSummary`. In questo caso, tuttavia, si effettua non realmente la convalida dell'input dell'utente. Al contrario, si effettua la convalida di un valore che è stato passato alla pagina da un' posizione. Il `Validation` helper che non esegue automaticamente.

Pertanto, è controllare il valore manualmente, eseguendo il test con `if(!Request.QueryString["ID"].IsEmpty()`). Se si è verificato un problema, è possibile visualizzare l'errore utilizzando `Html.ValidationSummary`, come accadeva con le `Validation` helper. A tale scopo, si chiama `Validation.AddFormError` e passarlo a un messaggio da visualizzare. `Validation.AddFormError` è un metodo incorporato che consente di definire messaggi personalizzati da unire con il sistema di convalida che ha già familiarità. (Più avanti in questa esercitazione parleremo come rendere questo processo di convalida più solido.)

Dopo aver verificato che vi sia un ID per il film, il codice legge il database, cercando solo un elemento singolo database. (Probabilmente si noterà il modello generale per le operazioni di database: aprire il database, definire un'istruzione SQL ed eseguire l'istruzione.) Questa volta, il codice SQL `Select` istruzione include `WHERE ID = @0`. Poiché l'ID è univoco, può essere restituito un solo record.

La query viene eseguita usando `db.QuerySingle` (non `db.Query`, come è stato utilizzato per l'elenco dei filmati), e il codice inserisce il risultato nel `row` variabile. Il nome `row` è arbitrario; puoi le variabili di qualsiasi nome desiderato. Le variabili inizializzate in alto vengono compilate con i dettagli di film in modo che questi valori possono essere visualizzati nelle caselle di testo.

## <a name="testing-the-edit-page-so-far"></a>Test della pagina Edit (fino a questo momento)

Se si desidera testare la pagina, eseguire la *film* ora e fare clic su un **modificare** collegamento accanto a eventuali film. Verrà visualizzato il *EditMovie* compilato pagina con i dettagli per il film selezionato:

![Modifica pagina film che mostra di film da modificare](updating-data/_static/image3.png)

Si noti che l'URL della pagina include un elemento, ad esempio `?id=10` (o un altro numero). Finora hai testato che **Edit** collega nel *film* pagina lavoro, che la pagina viene letta l'ID dalla stringa di query, e che il database di eseguire una query per ottenere un record singolo filmato sia funzionante.

È possibile modificare le informazioni relative al filmato, ma non accade nulla quando si fa clic su **Invia modifiche**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Aggiunta di codice per aggiornare il film con le modifiche dell'utente

Nel *EditMovie.cshtml* , per implementare la seconda funzione (salvataggio delle modifiche), aggiungere il codice seguente all'interno di parentesi graffa di chiusura di `@` blocco. (Se non si conosce esattamente dove inserire il codice, è possibile esaminare i [elenco di codice per la pagina Modifica film](#Complete_Page_Listing_for_EditMovie) che compare alla fine di questa esercitazione.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Anche in questo caso è simile al codice in questo markup e codice *AddMovie*. Il codice è in un `if(IsPost)` bloccare, poiché questo codice viene eseguito solo quando l'utente fa clic il **Invia modifiche** pulsante &mdash; , ovvero quando (e solo quando) ha inviato il form. In questo caso, non si usa un test come `if(IsPost && Validation.IsValid())`, vale a dire, da non combinare entrambi i test tramite and. In questa pagina, prima di tutto determinare se è presente l'invio di un form (`if(IsPost)`) e registrare solo i campi per la convalida. Quindi è possibile testare i risultati della convalida (`if(Validation.IsValid()`). Il flusso è leggermente diverso da quella di *AddMovie.cshtml* pagina, ma l'effetto è lo stesso.

Ottenere i valori di caselle di testo usando `Request.Form["title"]` e un codice simile per gli altri `<input>` elementi. Si noti che questa volta, il codice di ottenere l'ID del film all'esterno del campo nascosto (`<input type="hidden">`). Quando la pagina è stata eseguita la prima volta, il codice segue l'ID dalla stringa di query. Si ottiene il valore del campo nascosto per assicurarsi di ottenere l'ID del film che originariamente è stato visualizzato, nel caso in cui la stringa di query è stata modificata in qualche modo da allora.

La differenza fondamentale tra i *AddMovie* codice e questo codice è che in questo codice è usare il codice SQL `Update` istruzione anziché il `Insert Into` istruzione. L'esempio seguente illustra la sintassi SQL `Update` istruzione:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

È possibile specificare tutte le colonne in qualsiasi ordine, e non necessariamente aggiornare tutte le colonne durante un `Update` operazione. (Non è possibile aggiornare l'ID, in quanto che risparmierebbe in vigore il record come nuovo record, e che non è consentito per un `Update` operazione.)

> [!NOTE] 
> 
> **Importanti** il `Where` clausola con l'ID è molto importante, perché questo è come il database sappia quale database record che si desidera aggiornare. Se è stata interrotta la `Where` clausola, il database aggiornerebbero *ogni* record nel database. Nella maggior parte dei casi, sarebbe una situazione di emergenza.


Nel codice, i valori da aggiornare vengono passati all'istruzione SQL con i segnaposto. Per ripetere quanto abbiamo detto prima: per motivi di sicurezza *solo* usare i segnaposto per passare i valori in un'istruzione SQL.

Dopo che il codice Usa `db.Execute` per eseguire il `Update` istruzione, viene reindirizzato alla pagina di presentazione, in cui è possibile visualizzare le modifiche.

> [!TIP] 
> 
> **Istruzioni SQL differenti, diversi metodi**
> 
> Si potrebbe notare l'uso di metodi leggermente diversi per eseguire diverse istruzioni SQL. Per eseguire una `Select` query che potrebbero restituisce più record, si utilizza il `Query` (metodo). Per eseguire una `Select` query che già conosci restituirà un solo elemento di database, si utilizza il `QuerySingle` (metodo). Per eseguire comandi che apportano modifiche, ma che non restituiscono gli elementi di database, si utilizza il `Execute` (metodo).
> 
> È necessario avere diversi metodi in quanto ognuno di essi restituisce risultati diversi, come già illustrato la differenza tra `Query` e `QuerySingle`. (Il `Execute` metodo effettivamente restituisce un valore anche &mdash; vale a dire, il numero di righe di database che sono stati interessati dal comando &mdash; ma si è stati ignorando che finora.)
> 
> Naturalmente, il `Query` metodo può restituire solo una riga di database. ASP.NET, tuttavia, Considera sempre i risultati del `Query` metodo come una raccolta. Anche se il metodo restituisce una sola riga, è necessario estrarre tale singola riga dalla raccolta. Pertanto, nelle situazioni in cui si *conoscere* verranno restituite solo una riga, è un po' più comodo usare `QuerySingle`.
> 
> Esistono alcuni altri metodi che eseguono tipi specifici di operazioni di database. È possibile trovare un elenco dei metodi di database nel [riferimento rapido di ASP.NET Web Pages API](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Effettua la convalida per l'ID più affidabile

La prima volta che viene eseguita la pagina, si ottiene l'ID del film dalla stringa di query in modo che è possibile passare a ottenere il film dal database. È stata verificata la realtà è stato un valore da passare a cercare, che è avvenuto tramite questo codice:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Questo codice è stato usato per assicurarsi che se un utente per il *EditMovies* pagina senza selezionare prima un film nel *film* pagina, la pagina verrà visualizzato un messaggio di errore descrittivo. (In caso contrario, gli utenti verrebbero visualizzato un errore che sarebbe probabilmente sufficiente confonderli).

Tuttavia, la convalida non è molto efficace. La pagina potrebbe anche essere chiamata con questi errori:

- L'ID non è un numero. Ad esempio, la pagina potrebbe essere richiamata con un URL come `http://localhost:nnnnn/EditMovie?id=abc`.
- L'ID è un numero, ma fa riferimento a un filmato che non esiste (ad esempio, `http://localhost:nnnnn/EditMovie?id=100934`).

Se si è interessati a visualizzare gli errori risultanti da questi URL, eseguire la *film* pagina. Selezionare un film per modificare e quindi modificare l'URL del *EditMovie* pagina a un URL che contiene un carattere alfabetico, ID o l'ID di un film inesistente.

Quindi, cosa fare? La prima correzione consiste nell'assicurarsi che non solo è un ID passato alla pagina, ma che l'ID è un numero intero. Modificare il codice per il `!IsPost` test come illustrato in questo esempio:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

È stato aggiunto una seconda condizione il `IsEmpty` test, collegate con `&&` (AND logico):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Si ricorderà dal [Introduzione alla programmazione di ASP.NET Web Pages](../introducing-razor-syntax-c.md) esercitazione che metodi come `AsBool` un `AsInt` convertire una stringa di caratteri in un altro tipo di dati. Il `IsInt` metodo (e altri, come `IsBool` e `IsDateTime`) sono simili. Tuttavia, eseguono il test solo se si *possibile* convertire la stringa, senza eseguire effettivamente la conversione. Quindi, qui significa fondamentalmente *se il valore di stringa di query può essere convertito in un numero intero...* .

Potenziale problema sta cercando un film che non esiste. Il codice per ottenere un film è simile a questo codice:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Se si passa una `movieId` valore per il `QuerySingle` metodo che non corrisponde a un film effettivo, non viene restituito e le istruzioni che seguono (ad esempio, `title=row.Title`) comportare errori.

Anche in questo caso è facile apportare la correzione. Se il `db.QuerySingle` metodo non restituisce alcun risultato, il `row` variabile sarà null. Sarà pertanto possibile controllare se il `row` variabile è null prima di provare a ottenere i valori da quest'ultimo. Il codice seguente aggiunge un' `if` blocco intorno alle istruzioni che ottengono i valori fuori il `row` oggetto:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Con questi due test di convalida aggiuntivi, la pagina diventa più valide. Il codice completo per il `!IsPost` ramo avrà ora un aspetto simile all'esempio:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Si noterà che ancora una volta che questa attività è un utilizzo corretto per un `else` blocco. Se il test non superati, il `else` blocchi impostato i messaggi di errore.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Aggiunta di un collegamento per tornare alla pagina Movies

Un dettaglio finale e utile consiste nell'aggiungere un collegamento al *film* pagina. Nel flusso ordinario di eventi, gli utenti inizieranno al *film* e fare clic su un **modificare** collegamento. Che viene visualizzato il *EditMovie* pagina, in cui possono modificare il film e fare clic sul pulsante. Dopo che il codice ha elaborato la modifica, reindirizzato al *film* pagina.

Tuttavia:

- L'utente potrebbe decidere di non apportare alcuna modifica.
- L'utente potrebbe essere stato ricevuto da questa pagina senza prima avere scelto un' **Edit** clic sul collegamento nella *film* pagina.

In entrambi i casi, si desidera semplificare per poter tornare all'elenco principale. È facile apportare la correzione &mdash; aggiungere il markup seguente subito dopo la chiusura `</form>` tag nel markup:

[!code-html[Main](updating-data/samples/sample19.html)]

Questo markup Usa la stessa sintassi per un `<a>` elemento che si è visto in un' posizione. L'URL include `~` può indicare "radice del sito Web".

## <a name="testing-the-movie-update-process"></a>Testare il processo di aggiornamento di film

A questo punto è possibile testare. Eseguire la *film* e fare clic su **modificare** accanto a un filmato. Quando la *EditMovie* verrà visualizzata la pagina, apportare modifiche al film e fare clic su **Invia modifiche**. Quando viene visualizzato l'elenco di film, assicurarsi che le modifiche vengono visualizzate.

Per assicurarsi che funzioni correttamente la convalida, fare clic su **modifica** per un altro film. Quando si raggiunge il *EditMovie* pagina, deseleziona il **Genre** campo (o **anno** campo o entrambi) e provare a inviare le modifiche. Verrà visualizzato un errore, come previsto:

![Modifica pagina film che mostri gli errori di convalida](updating-data/_static/image4.png)

Fare clic sui **tornare all'elenco dei filmati** collegamento per ignorare le modifiche e tornare alle *film* pagina.

## <a name="coming-up-next"></a>In arrivo

Nella prossima esercitazione, verrà illustrato come eliminare un record di film.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Elenco completo per la pagina di film (aggiornata con i collegamenti di modifica)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Completare l'elenco di pagina per pagina film di modifica

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET usando la sintassi Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Istruzione SQL UPDATE](http://www.w3schools.com/sql/sql_update.asp) sul sito W3Schools

> [!div class="step-by-step"]
> [Precedente](entering-data.md)
> [Successivo](deleting-data.md)
