---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: 'Introduzione a ASP.NET Web Pages: aggiornamento dei dati di Database | Documenti Microsoft'
author: tfitzmac
description: In questa esercitazione viene illustrato come aggiornare la voce di un database esistente (modifica) quando si utilizzano pagine Web ASP.NET (Razor). Si presuppone di aver completato la serie th...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: e889cd27e2267a08f7b6ea708c92e35edbdd7a1a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898535"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>Introduzione a ASP.NET Web Pages: aggiornamento dei dati di Database
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questa esercitazione viene illustrato come aggiornare la voce di un database esistente (modifica) quando si utilizzano pagine Web ASP.NET (Razor). Si presuppone di aver completato la serie tramite [immissione di dati da utilizzare form utilizzando ASP.NET Web Pages](entering-data.md).
> 
> Illustra quanto segue:
> 
> - Come selezionare un singolo record di `WebGrid` helper.
> - Come leggere un singolo record da un database.
> - Modalità di precaricamento di un form con i valori del record del database.
> - Come aggiornare un record esistente in un database.
> - Come archiviare le informazioni nella pagina senza visualizzarla.
> - Come usare un campo nascosto per archiviare le informazioni.
>   
> 
> Funzionalità/tecnologie descritte:
> 
> - Il `WebGrid` helper.
> - L'istruzione SQL `Update` comando.
> - Metodo `Database.Execute`.
> - Campi nascosti (`<input type="hidden">`).


## <a name="what-youll-build"></a>Cosa Compilerai

Nell'esercitazione precedente, è stato descritto come aggiungere un record a un database. In questo caso, si apprenderà come visualizzare un record per la modifica. Nel *filmati* pagina verrà aggiornata la `WebGrid` helper in modo che venga visualizzato un **modifica** collegamento accanto a ogni filmato:

![WebGrid visualizzare contenente un collegamento di 'Modifica' per ogni film](updating-data/_static/image1.png)

Quando si sceglie il **modifica** collegamento, consente di visualizzare una pagina diversa, in cui le informazioni di film sono già in un formato:

![Modifica pagina filmato che mostra film da modificare](updating-data/_static/image2.png)

È possibile modificare uno dei valori. Quando si inviano le modifiche, il codice nella pagina Aggiorna il database e consente di tornare all'elenco di film.

Questa parte del processo funziona quasi esattamente come il *AddMovie.cshtml* pagina creata nell'esercitazione precedente, pertanto la maggior parte di questa esercitazione risulteranno familiare.

Esistono diversi modi, è possibile implementare un modo per modificare un filmato singoli. L'approccio indicato è stato scelto perché è facile da implementare e facile da comprendere.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Aggiunta di un collegamento di modifica per l'elenco di film

Per iniziare, si aggiornerà il *filmati* pagina in modo che ogni film anche l'elenco contiene un **modifica** collegamento.

Aprire il *Movies.cshtml* file.

Nel corpo della pagina, modificare il `WebGrid` markup aggiungendo una colonna. Di seguito è riportato il codice modificato:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

Questa è la nuova colonna:

[!code-html[Main](updating-data/samples/sample2.html)]

Il punto di questa colonna è di visualizzare un collegamento (`<a>` elemento) con il testo "Modifica". Siamo dopo consiste nel creare un collegamento che è simile al seguente quando si esegue la pagina, con la `id` valore diverso per ogni film:

[!code-css[Main](updating-data/samples/sample3.css)]

Questo collegamento verrà richiamato una pagina denominata *EditMovie*, e passa la stringa di query `?id=7` a tale pagina.

La sintassi per la nuova colonna potrebbe essere un po' complessa, ma solo perché riunisce diversi elementi. Ogni singolo elemento è semplice. Se concentrarsi solo il `<a>` elemento, viene visualizzato questo codice:

[!code-html[Main](updating-data/samples/sample4.html)]

Alcune informazioni generali sul funzionamento della griglia: nella griglia vengono visualizzate le righe, una per ogni record di database, e visualizza le colonne per ogni campo nel record del database. Mentre viene creato ogni riga della griglia, il `item` oggetto contiene i record del database (elemento) per la riga. Questa disposizione consente nel codice per ottenere i dati per la riga. È ciò che viene visualizzato di seguito: l'espressione `item.ID` Ottiene il valore ID dell'elemento del database corrente. È possibile ottenere uno dei valori del database (titolo, genere o anno) allo stesso modo usando `item.Title`, `item.Genre`, o `item.Year`.

L'espressione `"~/EditMovie?id=@item.ID` combina la parte hardcoded dell'URL di destinazione (`~/EditMovie?id=`) con questo ID derivata in modo dinamico. (Si è visto il `~` operatore nell'esercitazione precedente; è un operatore di ASP.NET che rappresenta la radice del sito Web corrente.)

Il risultato è che questa parte del markup nella colonna semplicemente genera un risultato come il seguente codice in fase di esecuzione:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Ovviamente, il valore effettivo del `id` sarà diverso per ogni riga.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Creazione di una visualizzazione personalizzata per una colonna della griglia

Eseguire il backup ora per la colonna della griglia. Le tre colonne in origine era la griglia visualizzata solo i valori dei dati (titolo, genre e l'anno). Questa visualizzazione è specificato, passando il nome della colonna di database &mdash; , ad esempio, `grid.Column("Title")`.

Questa nuova **modifica** colonna di collegamento è diverso. Anziché specificare un nome di colonna, si passa un `format` parametro. Questo parametro consente di definire markup che il `WebGrid` helper verrà eseguito il rendering con il `item` valore per visualizzare i dati della colonna in grassetto o verde o nel formato che si desidera. Ad esempio, se si desidera che il titolo da visualizzare in grassetto, è possibile creare una colonna simile al seguente:

[!code-html[Main](updating-data/samples/sample6.html)]

(I vari `@` caratteri presenti il `format` proprietà contrassegnare la transizione tra markup e un valore di codice.)

Una volta che si conoscono il `format` proprietà, è più facile da comprendere come il nuovo **modifica** viene sommata colonna collegamento:

[!code-html[Main](updating-data/samples/sample7.html)]

La colonna include *solo* di markup che esegue il rendering del collegamento, nonché alcune informazioni (ID) che è stato estratto dal record del database per la riga.

> [!TIP]
> 
> **I parametri denominati e i parametri posizionali per un metodo**
> 
> Numero di volte dopo aver chiamato un metodo e passare i parametri, semplicemente elencati i valori dei parametri separati da virgole. Di seguito sono riportati alcuni esempi:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> È non segnalare il problema quando si è visto prima di tutto il codice, ma in ogni caso, si sta passando i parametri ai metodi in un ordine specifico &mdash; in particolare, l'ordine in cui i parametri sono definiti in tale metodo. Per `db.Execute` e `Validation.RequireFields`, se confuse l'ordine dei valori passati, verrà visualizzato un messaggio di errore quando la pagina viene eseguita, o almeno alcuni risultati imprevisti. Chiaramente, è necessario conoscere l'ordine per passare i parametri in. (In WebMatrix, IntelliSense consente di individuare il nome, tipo e ordine dei parametri di informazioni.)
> 
> In alternativa al passaggio di valori in ordine, è possibile utilizzare *parametri denominati*. (Il passaggio di parametri nell'ordine è noto come utilizzando *parametri posizionali*.) Per i parametri denominati, includere in modo esplicito il nome del parametro quando si passa il relativo valore. Utilizzati parametri denominati già un numero di volte in queste esercitazioni. Ad esempio:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> e
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> I parametri denominati sono utili per un paio di situazioni, soprattutto quando un metodo accetta un numero di parametri. Uno è quando si desidera passare solo uno o due parametri, ma non i valori che si desidera passare tra le posizioni prima nell'elenco di parametri. Anche quando si desidera rendere il codice più leggibile, passando i parametri nell'ordine in cui più appropriato per l'utente.
> 
> Per utilizzare parametri denominati, ovviamente, è necessario conoscere i nomi dei parametri. WebMatrix IntelliSense può *Mostra* per i nomi, ma non è attualmente compilare i campi per l'utente.


## <a name="creating-the-edit-page"></a>Creazione pagina di modifica

Ora è possibile creare il *EditMovie* pagina. Quando gli utenti fanno clic il **modifica** collegamento, essi termine, verrà visualizzata in questa pagina.

Creare una pagina denominata *EditMovie.cshtml* e sostituire quello presente nel file con il markup seguente:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

In questo markup e il codice è simile a quanto è presente nel *AddMovie* pagina. È una piccola differenza nel testo del pulsante Invia. Come con la *AddMovie* pagina, è presente un `Html.ValidationSummary` chiamata che consente di visualizzare se sono presenti errori di convalida. Questa fase ci stiamo escludendo le chiamate a `Validation.Message`, dal momento che gli errori verranno visualizzati nel riepilogo di convalida. Come indicato nell'esercitazione precedente, è possibile utilizzare il riepilogo di convalida e i singoli messaggi di errore in varie combinazioni.

Nuovo notare che il `method` attributo del `<form>` è impostato su `post`. Come con la *AddMovie.cshtml* pagina, questa pagina apporta modifiche al database. Pertanto, questo modulo è necessario eseguire un `POST` operazione. (Per ulteriori informazioni sulla differenza tra `GET` e `POST` operazioni, vedere il [sicurezza verbo HTTP GET e POST](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) barra laterale nell'esercitazione di form HTML.)

Come osservato in un'esercitazione precedente, il `value` vengono impostati gli attributi delle caselle di testo con codice Razor per precaricare li. Questa volta, tuttavia, si usa variabili come `title` e `genre` per l'attività anziché `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Come prima, in questo markup verrà precaricare i valori della casella di testo con i valori di film. Verrà visualizzato in un momento perché è utile usare le variabili di questo momento anziché il `Request` oggetto.

È inoltre disponibile un `<input type="hidden">` elemento in questa pagina. Questo elemento archivia l'ID film senza renderlo visibile nella pagina. L'ID viene inizialmente passato alla pagina, utilizzando un valore di stringa di query (`?id=7` o simili nell'URL). Inserisce il valore ID in un campo nascosto, è possibile assicurarsi che sia disponibile quando il form viene inviato, anche se non è più necessario l'accesso all'URL originale è stata richiamata con la pagina.

A differenza di *AddMovie* pagina, il codice per il *EditMovie* pagina contiene due funzioni distinte. La prima funzione è che quando viene visualizzata la pagina per la prima volta (e *solo* quindi), il codice ottiene l'ID film dalla stringa di query. Il codice quindi utilizza l'ID per leggere il film corrispondente all'esterno del database e visualizzare (Precarica), nelle caselle di testo.

La seconda funzione è che quando l'utente sceglie il **Invia modifiche** pulsante, il codice deve leggere i valori delle caselle di testo e convalidarli. Il codice è inoltre necessario aggiornare l'elemento di database con i nuovi valori. Questa tecnica è analoga all'aggiunta di un record, come specificato nella *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Aggiungere il codice per leggere un singolo filmato

Per eseguire la prima funzione, aggiungere questo codice nella parte superiore della pagina:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

La maggior parte di questo codice si trova all'interno di un blocco che inizia `if(!IsPost)`. Il `!` operatore significa "not", che significa che l'espressione *se la richiesta non è un inoltro post*, ovvero un modo indiretto indicando *se la richiesta è la prima volta che questa pagina è stata eseguita*. Come notato in precedenza, questo codice deve essere eseguito *solo* alla prima esecuzione della pagina. Se si non racchiudere il codice in `if(!IsPost)`, verrà eseguito ogni volta che la pagina viene richiamata, se la prima volta oppure in risposta a un pulsante fare clic su.

Si noti che il codice include un `else` bloccare questo momento. Come indicato quando è stata introdotta `if` blocchi, a volte si desidera eseguire codice alternativo se la condizione che si sta testando non è true. Ovvero in questo caso. Se la condizione supera (ovvero, se l'ID passato alla pagina è ok), leggere una riga dal database. Tuttavia, se non soddisfa la condizione, il `else` blocco viene eseguito e il codice imposta un messaggio di errore.

## <a name="validating-a-value-passed-to-the-page"></a>Convalida di un valore passato alla pagina

Il codice utilizza `Request.QueryString["id"]` per ottenere l'ID che viene passato alla pagina. Il codice garantisce che in realtà è stato passato un valore per l'ID. Se è stato passato alcun valore, il codice imposta un errore di convalida.

Questo codice viene illustrato un modo diverso per convalidare le informazioni. Nell'esercitazione precedente, si è lavorato con il `Validation` helper. La registrazione di campi da convalidare, ASP.NET automaticamente e la convalida ha visualizzati errori tramite `Html.ValidationMessage` e `Html.ValidationSummary`. In questo caso, tuttavia, si effettua non effettivamente la convalida dell'input dell'utente. Al contrario, si effettua la convalida di un valore che è stato passato alla pagina da un' posizione. Il `Validation` helper che non esegue automaticamente.

Pertanto, per verificare il valore manualmente, con `if(!Request.QueryString["ID"].IsEmpty()`). Se si verifica un problema, è possibile visualizzare l'errore utilizzando `Html.ValidationSummary`, come è stato eseguito con il `Validation` helper. A tale scopo, si chiama `Validation.AddFormError` e passarlo a un messaggio da visualizzare. `Validation.AddFormError` è un metodo incorporato che consente di definire messaggi personalizzati che strettamente si ha già familiarità con il sistema di convalida. (Più avanti in questa esercitazione verranno fornite informazioni rendere un po' più affidabile questo processo di convalida.)

Dopo aver apportato che vi sia un ID per il film, il codice legge il database, cercando solo un elemento singolo database. (Si probabilmente notato il modello generale per le operazioni di database: aprire il database, definire un'istruzione SQL ed eseguire l'istruzione.) Questa volta, l'istruzione SQL `Select` istruzione include `WHERE ID = @0`. Poiché l'ID è univoco, può essere restituito un solo record.

La query viene eseguita tramite `db.QuerySingle` (non `db.Query`, come è stato utilizzato per l'elenco di film), e il codice inserisce il risultato nella `row` variabile. Il nome `row` è arbitrario; puoi le variabili di qualsiasi nome desiderato. Le variabili inizializzate nella parte superiore vengono compilate con i dettagli del film in modo che questi valori possono essere visualizzati nelle caselle di testo.

## <a name="testing-the-edit-page-so-far"></a>Test della pagina di modifica (finora)

Se si desidera verificare la pagina, eseguire il *filmati* pagina ora e fare clic su un **modifica** collegamento accanto a qualsiasi film. Verrà visualizzato il *EditMovie* pagina con i dettagli specificati per il film selezionato:

![Modifica pagina filmato che mostra film da modificare](updating-data/_static/image3.png)

Si noti che l'URL della pagina include un output simile `?id=10` (o un altro numero). Finora è stato testato che **modifica** di collegamenti nel *film* pagina lavoro, che la pagina viene letta l'ID dalla stringa di query, e che il database di eseguire una query per ottenere un record singolo filmato sia funzionante.

È possibile modificare le informazioni di film, ma non accade nulla quando si fa clic **Invia modifiche**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Aggiungere il codice per aggiornare il film con le modifiche dell'utente

Nel *EditMovie.cshtml* file, per implementare la seconda funzione (salvataggio), aggiungere il codice seguente all'interno di parentesi graffa di chiusura di `@` blocco. (Se non si è certi esattamente in cui inserire il codice, è possibile esaminare il [completo listato di codice per la pagina Modifica film](#Complete_Page_Listing_for_EditMovie) che viene visualizzato alla fine di questa esercitazione.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Nuovamente, questo markup e il codice è simile al codice in *AddMovie*. Il codice è in un `if(IsPost)` blocco, perché questo codice viene eseguito solo quando l'utente sceglie il **Invia modifiche** pulsante &mdash; , ovvero quando (e solo quando) ha inviato il form. In questo caso, non si usa un test come `if(IsPost && Validation.IsValid())`, vale a dire da non combinare entrambi i test tramite and. In questa pagina, innanzitutto determinare se è presente un invio del modulo (`if(IsPost)`) e registrare solo i campi per la convalida. Quindi è possibile testare i risultati della convalida (`if(Validation.IsValid()`). Il flusso è leggermente diverso da quello di *AddMovie.cshtml* pagina, ma l'effetto è lo stesso.

Per ottenere i valori delle caselle di testo utilizzare `Request.Form["title"]` e codice analogo per gli altri `<input>` elementi. Si noti che questo periodo, il codice di Ottiene l'ID film fuori dal campo nascosto (`<input type="hidden">`). Quando la pagina è stata eseguita la prima volta, il codice ha ricevuto l'ID la stringa di query. Ottenere il valore del campo nascosto per assicurarsi di ottenere l'ID del filmato che originariamente è stato visualizzato, nel caso in cui la stringa di query è stata modificata in qualche modo da allora.

L'importante differenza tra il *AddMovie* codice e il codice è che in questo codice di utilizzare l'istruzione SQL `Update` istruzione anziché il `Insert Into` istruzione. L'esempio seguente mostra la sintassi dell'istruzione SQL `Update` istruzione:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

È possibile specificare le colonne in qualsiasi ordine, e non è necessario aggiornare ogni colonna durante un `Update` operazione. (Non è possibile aggiornare l'ID, in quanto che sarebbe attivo salvare il record come un nuovo record, e che non è consentita per un `Update` operazione.)

> [!NOTE] 
> 
> **Importante** il `Where` clausola con l'ID è molto importante, perché si tratta come database sappia quale database record che si desidera aggiornare. Se si è interrotto il `Where` clausola, il database Aggiorna *ogni* record nel database. Nella maggior parte dei casi, sarebbe un'emergenza.


Nel codice, aggiornare i valori vengono passati all'istruzione SQL utilizzando segnaposto. Per ripetere quanto abbiamo detto prima: per motivi di sicurezza *solo* utilizzare segnaposto per passare valori a un'istruzione SQL.

Dopo il codice Usa `db.Execute` per eseguire il `Update` istruzione, viene reindirizzato alla pagina di elenco, in cui è possibile visualizzare le modifiche.

> [!TIP] 
> 
> **Istruzioni SQL diverso, metodi diversi**
> 
> È possibile notare l'uso di metodi leggermente diversi per eseguire istruzioni SQL diverse. Per eseguire un `Select` query che potrebbero restituisce più record, si utilizza il `Query` metodo. Per eseguire un `Select` che verranno restituiti solo un elemento di database, utilizzare il `QuerySingle` metodo. Per eseguire i comandi che apportano modifiche, ma che non restituiscono gli elementi di database, utilizzare il `Execute` metodo.
> 
> È necessario disporre di diversi metodi in quanto ognuno di essi restituisce risultati diversi, come già specificato nella differenza tra `Query` e `QuerySingle`. (Il `Execute` metodo restituisce un valore anche &mdash; , ovvero il numero di righe di database che sono interessate dal comando &mdash; ma si è stati ignorando che finora.)
> 
> Naturalmente, la `Query` metodo può restituire solo una riga di database. Tuttavia, ASP.NET considera sempre i risultati del `Query` metodo come una raccolta. Anche se il metodo restituisce una sola riga, è necessario estrarre la singola riga dalla raccolta. Pertanto, in situazioni in cui si *conoscere* si otterrà solo una riga, è un po' più comodo usare `QuerySingle`.
> 
> Esistono alcuni altri metodi che eseguono tipi specifici di operazioni di database. È possibile trovare un elenco di metodi di database di [riferimento rapido di ASP.NET Web Pages API](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Effettua la convalida per più ID affidabile

La prima volta che la pagina viene eseguita, si ottiene l'ID film dalla stringa di query in modo da poter accedere ottenere tale film dal database. Verificato che in realtà era presente un valore da passare cercare, che è stato eseguito con questo codice:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Questo codice usato per fare in modo che se un utente ottiene il *EditMovies* pagina senza selezionare un filmato nel *filmati* pagina, la pagina Visualizza un messaggio di errore descrittivo. (In caso contrario, verrà visualizzato un errore che sarebbe probabilmente confondere.)

Tuttavia, la convalida non molto affidabile. La pagina potrebbe essere richiamata anche con questi errori:

- L'ID non è un numero. Ad esempio, la pagina potrebbe essere richiamata con un URL come `http://localhost:nnnnn/EditMovie?id=abc`.
- L'ID è un numero, ma fa riferimento a un filmato che non esiste (ad esempio, `http://localhost:nnnnn/EditMovie?id=100934`).

Se si desidera visualizzare gli errori risultanti da tali URL, eseguire il *filmati* pagina. Selezionare un filmato per modificare e quindi modificare l'URL del *EditMovie* pagina a un URL che contiene un alfabetico ID o l'ID di un filmato inesistente.

Pertanto, deve procedere? La correzione prima consiste nel verificare che non solo è un ID passato alla pagina, ma che l'ID è un numero intero. Modificare il codice per il `!IsPost` test aspetto simile al seguente:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Aver aggiunto una seconda condizione per il `IsEmpty` test, collegate con `&&` (AND logico):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Ricorderà dal [Introduzione alla programmazione di ASP.NET Web Pages](../introducing-razor-syntax-c.md) esercitazione che metodi come `AsBool` un `AsInt` convertire una stringa di caratteri in un altro tipo di dati. Il `IsInt` metodo (e altri, ad esempio `IsBool` e `IsDateTime`) sono simili. Tuttavia, eseguono il test solo se si *possibile* convertire la stringa, senza eseguire effettivamente la conversione. Questo significa fondamentalmente *se il valore di stringa di query può essere convertito in un intero...* .

Potenziale problema sta cercando un filmato che non esiste. Il codice per ottenere un filmato simile al codice:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Se si passa un `movieId` valore per il `QuerySingle` metodo che non corrisponde a un filmato effettivo, non viene restituito alcun risultato e le istruzioni che seguono (ad esempio, `title=row.Title`) provocare errori.

È nuovamente un facile da risolvere. Se il `db.QuerySingle` metodo non restituisce alcun risultato, il `row` variabile sarà null. È possibile controllare se il `row` variabile è null prima di tentare di recuperare i valori da esso. Il codice seguente aggiunge un `if` blocco intorno alle istruzioni che ottengono i valori di timeout di `row` oggetto:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Con questi due test di convalida aggiuntivo, la pagina diventa più valide. Il codice completo per il `!IsPost` ramo avrà ora un aspetto simile al seguente:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Si noterà che ancora una volta che questa operazione è consigliabile utilizzare per un `else` blocco. Se il test non superati, il `else` set di blocchi di messaggi di errore.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Aggiunta di un collegamento per tornare alla pagina di filmati

È un dettaglio finale e utile per aggiungere un collegamento al *filmati* pagina. Nel flusso normale di eventi, gli utenti inizieranno al *filmati* pagina e fare clic su un **modifica** collegamento. Che li include il *EditMovie* pagina, in cui è possibile modificare il filmato e fare clic sul pulsante. Dopo che il codice ha elaborato la modifica, reindirizza il *filmati* pagina.

Tuttavia:

- L'utente potrebbe decidere di non apporta alcuna modifica.
- L'utente potrebbe essere stato ottenuto a questa pagina senza prima avere scelto un **modifica** collegare il *filmati* pagina.

In entrambi i casi, si desidera semplificare tornare all'elenco principale. Si tratta di una facile correzione &mdash; aggiungere il markup seguente subito dopo la chiusura `</form>` tag nel codice:

[!code-html[Main](updating-data/samples/sample19.html)]

Questo codice Usa la stessa sintassi per un `<a>` elemento che si è visto altrove. L'URL include `~` per indicare "radice del sito Web".

## <a name="testing-the-movie-update-process"></a>Test del processo di aggiornamento del film

Ora è possibile eseguire il test. Eseguire il *filmati* pagina e fare clic su **modifica** accanto a un filmato. Quando il *EditMovie* verrà visualizzata la pagina, apportare modifiche per il film e fare clic su **Invia modifiche**. Quando viene visualizzato l'elenco di film, assicurarsi che le modifiche vengono visualizzate.

Per assicurarsi che funzioni correttamente la convalida, fare clic su **modifica** per un altro film. Quando si accede al *EditMovie* pagina, deseleziona il **Genre** campo (o **anno** campo o entrambi) e si tenta di inviare le modifiche. Verrà visualizzato un errore, come previsto:

![Modifica pagina filmato che mostri gli errori di convalida](updating-data/_static/image4.png)

Fare clic su di **tornare all'elenco di film** collegamento per annullare le modifiche e tornare al *filmati* pagina.

## <a name="coming-up-next"></a>Esercitazione seguente

Nella prossima esercitazione, si noterà come eliminare un record di film.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Elenco completo per la pagina di film (aggiornata con i collegamenti di modifica)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Completare l'elenco di pagina per pagina film modifica

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Istruzione SQL UPDATE](http://www.w3schools.com/sql/sql_update.asp) nel sito W3Schools

> [!div class="step-by-step"]
> [Precedente](entering-data.md)
> [Successivo](deleting-data.md)
