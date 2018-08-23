---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: 'Introduzione a ASP.NET Web Pages: immissione di dati del Database tramite moduli | Microsoft Docs'
author: tfitzmac
description: Questa esercitazione illustra come creare un modulo di immissione e quindi immettere i dati che si ottengono dal form in una tabella di database quando si usa ASP.NET Web Pages (...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: 453ed888bc219175c5a3a25f857dfa79ab22b608
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838995"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Introduzione a ASP.NET Web Pages: immissione di dati del Database tramite moduli
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra come creare un modulo di immissione e quindi immettere i dati che si ottengono dal form in una tabella di database quando si usa ASP.NET Web Pages (Razor). Si presuppone di aver completato la serie attraverso [nozioni di base di form HTML in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Che cosa si apprenderà come:
> 
> - Informazioni su come elaborare i form di immissione.
> - Come aggiungere dati (inserimento) in un database.
> - Come assicurarsi che gli utenti avere immesso un valore obbligatorio in un form (come convalida dell'input utente).
> - Come visualizzare gli errori di convalida.
> - Viene descritto come passare a un'altra pagina dalla pagina corrente.
>   
> 
> Le funzionalità/tecnologie illustrate:
> 
> - Metodo `Database.Execute`.
> - Il codice SQL `Insert Into` istruzione
> - Il `Validation` helper.
> - Metodo `Response.Redirect`.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

Nell'esercitazione precedente che è stato illustrato come creare un database, immesso i dati del database mediante la modifica del database direttamente in WebMatrix, utilizza il **Database** dell'area di lavoro. Nella maggior parte delle App, che non è un modo pratico per inserire dati nel database, tuttavia. Quindi, in questa esercitazione si creerà un'interfaccia basata sul web che consente a chiunque immettere i dati e salvarlo nel database.

Si creerà una pagina in cui è possibile immettere nuovi film. La pagina conterrà un modulo di immissione contenente campi (caselle di testo) in cui è possibile immettere un titolo del film, il genere e anno. La pagina avrà un aspetto simile a questa pagina:

!['Aggiungi film' pagina nel browser](entering-data/_static/image1.png)

Le caselle di testo saranno in formato HTML `<input>` elementi che avrà un aspetto quali questo markup:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Creazione del Form base voce

Creare una pagina denominata *AddMovie.cshtml*.

Sostituire quello presente nel file con il markup seguente. Sovrascrivere tutti gli elementi; a breve si aggiungerà un blocco di codice nella parte superiore.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Questo esempio viene illustrato tipici HTML per la creazione di un form. Usa `<input>` elementi per le caselle di testo e per il pulsante Invia. Le didascalie per le caselle di testo vengono create tramite standard `<label>` elementi. Il `<fieldset>` e `<legend>` elementi inserire una casella nice intorno al form.

Si noti che in questa pagina, il `<form>` elemento utilizza `post` come valore per il `method` attributo. Nell'esercitazione precedente, è stato creato un modulo che utilizzato il `get` (metodo). Che è stata corretta, perché anche se i valori per il server di inviare il modulo, la richiesta non è stata apportata alcuna modifica. Tutto ha sempre fatto è recuperare dati in modi diversi. Tuttavia, in questa pagina si *verranno* apportare modifiche, ovvero che si intende aggiungere nuovi record di database. Pertanto, è consigliabile usare questo modulo la `post` (metodo). (Per altre informazioni sulla differenza tra `GET` e `POST` operazioni, vedere la[GET, POST e HTTP verbo Safety](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) intestazione laterale nell'esercitazione precedente.)

Si noti che ogni casella di testo ha un `name` elemento (`title`, `genre`, `year`). Come illustrato nell'esercitazione precedente, questi nomi sono importanti perché sono necessarie tali nomi pertanto è possibile ottenere l'input dell'utente in un secondo momento. È possibile usare qualsiasi nome. È utile usare nomi significativi che consentono di ricordare i dati che si sta lavorando.

Il `value` attributo della ognuno `<input>` elemento contiene un frammento di codice Razor (ad esempio, `Request.Form["title"]`). Dopo aver inviato il modulo è stato illustrato una versione di questo espediente nell'esercitazione precedente per mantenere il valore immesso nella casella di testo (se presente).

## <a name="getting-the-form-values"></a>Ottenere i valori del modulo

Aggiungere quindi codice che elabora il modulo. Nella struttura, verranno eseguite le seguenti:

1. Controllare se la pagina viene registrata (è stato inviato). Si desidera il codice venga eseguita solo quando gli utenti hanno scelto il pulsante, non quando viene eseguita prima di tutto la pagina.
2. Ottenere i valori immessi dall'utente nelle caselle di testo. In questo caso, poiché il form Usa il `POST` verbo, si otterranno i valori di modulo dal `Request.Form` raccolta.
3. Inserire i valori come un nuovo record nel *film* tabella di database.

Nella parte superiore del file, aggiungere il codice seguente:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Le prime righe creano variabili (`title`, `genre`, e `year`) per contenere i valori di caselle di testo. La riga `if(IsPost)` assicura che le variabili sono impostate *solo* quando gli utenti fanno clic la **aggiungere film** pulsante, ovvero, ovvero quando il form è stato inviato.

Come illustrato in un'esercitazione precedente, è ottenere il valore di una casella di testo utilizzando un'espressione come `Request.Form["name"]`, dove *name* è il nome del `<input>` elemento.

I nomi delle variabili (`title`, `genre`, e `year`) sono arbitrari. Ad esempio i nomi assegnati a `<input>` elementi, è possibile chiamarli liberamente. (I nomi delle variabili non devono corrispondere gli attributi name degli `<input>` elementi nel form.) Ma come con la `<input>` elementi, è consigliabile usare nomi di variabile che riflettono i dati in essi contenuti. Quando si scrive codice, nomi coerenti rendono più semplice da ricordare i dati che si sta lavorando.

## <a name="adding-data-to-the-database"></a>Aggiunta di dati al Database

Nel codice di blocco è appena aggiunto, solo *all'interno* la parentesi graffa chiusa ( `}` ) del `if` blocco (non solo all'interno del blocco di codice), aggiungere il codice seguente:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Questo esempio è simile al codice usato nell'esercitazione precedente al recupero e la visualizzazione dei dati. La riga che inizia con `db =` si apre il database, come in precedenza e la riga successiva definisce anche in questo caso un'istruzione SQL, come abbiamo visto prima. Tuttavia, questa volta definisce un linguaggio SQL `Insert Into` istruzione. L'esempio seguente illustra la sintassi generale del `Insert Into` istruzione:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

In altre parole, specificare la tabella da inserire nella, quindi elencare le colonne da inserire in e quindi elencare i valori da inserire. (Come illustrato in precedenza, SQL non distinzione maiuscole / minuscole ma alcuni utenti di sfruttare le parole chiave per renderne più semplice leggere il comando).

Le colonne che si sta inserendo in sono già elencate nel comando, ovvero `(Title, Genre, Year)`. La parte interessante modo è possibile ottenere i valori di caselle di testo nel `VALUES` fa parte del comando. Invece i valori effettivi, si vede `@0`, `@1`, e `@2`, che sono ovviamente segnaposto. Quando si esegue il comando (nel `db.Execute` riga), i valori ottenuti di caselle di testo vengono passati.

**Importante**: Tenere presente che l'unico modo mai includere i dati immessi in linea da un utente in un'istruzione SQL deve usare i segnaposto, come illustrato di seguito (`VALUES(@0, @1, @2)`). Se si concatenare l'input dell'utente in un'istruzione SQL, ci si espone a un attacco SQL injection, come illustrato in [nozioni fondamentali sui moduli in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) (esercitazione precedente).

Ancora all'interno di `if` blocco, aggiungere la riga seguente dopo il `db.Execute` riga:

[!code-css[Main](entering-data/samples/sample4.css)]

Dopo l'inserimento di questo nuovo film nel database, questa riga si (reindirizzamento) passa per la *film* pagina in modo da visualizzare sul filmato appena immesso. Il `~` operatore significa "radice del sito Web". (Il `~` operatore funziona solo nelle pagine ASP.NET, non in formato HTML a livello generale.)

Il blocco di codice completo è simile a questo esempio:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Test di comando di inserimento (fino a questo momento)

Non abbiamo ancora finito ancora, ma questo è un buon momento per eseguire il test.

Nella visualizzazione struttura ad albero dei file in WebMatrix, fare doppio clic il *AddMovie.cshtml* e quindi fare clic su **Avvia nel browser**.

!['Aggiungi film' pagina nel browser](entering-data/_static/image2.png)

(Se si finisce con un'altra pagina nel browser, verificare che l'URL sia `http://localhost:nnnnn/AddMovie`), dove *nnnnn* è il numero di porta in uso.)

Si è giunti a una pagina di errore? In questo caso, leggerla con attenzione e assicurarsi che il codice ha un aspetto esattamente ciò che è stato elencato in precedenza.

Immettere un film in formato &mdash; , ad esempio, usare "Kane cittadini", "Drammatico" e "1941". In alternativa, qualsiasi elemento. Quindi fare clic su **film aggiungere**.

Se tutto va bene, si verrà reindirizzati per il *film* pagina. Assicurarsi che sia elencato il nuovo film.

![Pagina Movies che visualizza appena aggiunti film](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Convalida dell'Input utente

Tornare al *AddMovie* pagina oppure eseguirlo nuovamente. Immettere un altro film, ma questa volta, immettere solo il titolo &mdash; , ad esempio, immettere "Casa" in the Rain". Quindi fare clic su **film aggiungere**.

Si verrà reindirizzati per il *film* pagina nuovamente. È possibile trovare questo nuovo film, ma è incompleto.

![Pagina di film con nuovo film che mancano alcuni valori](entering-data/_static/image4.png)

Quando è stato creato il *film* tabella, in modo esplicito ha dichiarato che nessuno dei campi potrebbe essere null. Qui è un modulo di immissione di nuovi film che si sta lasciando vuota campi. Che è un errore.

In questo caso, il database non è stata effettivamente generato (o *throw*) un errore. Senza fornire un genere o un anno, pertanto, il codice nel *AddMovie* pagina considerati questi valori come cosiddette *stringhe vuote*. Quando il codice SQL `Insert Into` comando è stato eseguito, i campi al genere e anno non sono stata utile dei dati in essi contenuti, ma non sono null.

Ovviamente, non si vuole consentire agli utenti di immettere informazioni relative al filmato metà vuoto nel database. La soluzione consiste nel convalidare l'input dell'utente. Inizialmente, la convalida verrà semplicemente assicurarsi che l'utente ha immesso un valore per tutti i campi (vale a dire che nessuna di esse contiene una stringa vuota).

> [!TIP]
> 
> **Stringhe null e vuote**
> 
> Nella programmazione, vi è una differenza tra i diversi concetti di "Nessun valore". In generale, è un valore *null* se è mai stato impostato o inizializzata in alcun modo. Al contrario, è possibile impostare una variabile che prevede dati di tipo carattere (stringhe) un *una stringa vuota*. In tal caso, il valore non è null. si è appena stato impostato esplicitamente su una stringa di caratteri la cui lunghezza è pari a zero. Le due istruzioni seguenti mostrano la differenza:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> È un po' più complicato rispetto a quello, ma il punto importante è che `null` rappresenta una sorta di stato indeterminato.
> 
> E quindi è importante comprendere esattamente quando un valore è null e quando è semplicemente una stringa vuota. Nel codice per il *AddMovie* pagina, per ottenere i valori di caselle di testo utilizzare `Request.Form["title"]` e così via. Quando la pagina prima di tutto viene eseguita (prima di fare clic sul pulsante), il valore di `Request.Form["title"]` è null. Ma quando si invia il modulo `Request.Form["title"]` Ottiene il valore della `title` casella di testo. Non è ovvio, ma una casella di testo vuota non è null. è presente solo una stringa vuota in esso. Pertanto, quando il codice viene eseguito in risposta al pulsante fare clic su, `Request.Form["title"]` contiene una stringa vuota.
> 
> Perché questa distinzione è importante? Quando è stato creato il *film* tabella, in modo esplicito ha dichiarato che nessuno dei campi potrebbe essere null. Ma qui è un modulo di immissione di nuovi film che si sta lasciando vuota campi. Ragionevolmente prevedibile il database da ricevere lamentele da parte quando si è provato a salvare di nuovo film che non dispongono di valori per genere o un anno. Ma questo è il punto &mdash; anche se tali caselle di testo viene lasciato vuoto, i valori non null, ma sono stringhe vuote. Di conseguenza, si è in grado di salvare nuovi film nel database con queste colonne vuote &mdash; , ma non null. valori &mdash;. Pertanto, è necessario assicurarsi che gli utenti non inviano una stringa vuota, a questo scopo, convalidando l'input dell'utente.


### <a name="the-validation-helper"></a>L'Helper di convalida

Pagine Web ASP.NET include un helper &mdash; il `Validation` helper &mdash; che è possibile usare per assicurarsi che gli utenti immettano i dati che soddisfano i requisiti. Il `Validation` helper è uno degli helper che viene compilato a ASP.NET Web Pages, quindi non è necessario installarlo come un pacchetto tramite NuGet, il modo in cui è installato l'helper Gravatar in un'esercitazione precedente.

Per convalidare l'input dell'utente, è possibile eseguire le operazioni seguenti:

- Usare codice per specificare che si desidera richiedere i valori nelle caselle di testo nella pagina.
- Inserisce un test nel codice in modo che le informazioni relative al filmato viene aggiunto al database solo se tutto ciò che viene convalidato correttamente.
- Aggiungere il codice nel markup per visualizzare i messaggi di errore.

Nel blocco di codice nel *AddMovie* pagina, verso l'alto nella parte superiore prima le dichiarazioni di variabili, aggiungere il codice seguente:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Si chiama `Validation.RequireField` una volta per ogni campo (`<input>` elemento) in cui si desidera richiedere una voce. È anche possibile aggiungere un messaggio di errore personalizzato per ogni chiamata, come illustrato di seguito. (Sono diversi per i messaggi per indicare che è possibile inserire qualsiasi presenti).

Se si è verificato un problema, si desidera evitare che le nuove informazioni di film venga inserito nel database. Nel `if(IsPost)` blocco, usare `&&` (AND logico) per aggiungere un'altra condizione che i test `Validation.IsValid()`. Al termine, l'intero `if(IsPost)` blocco è simile a questo codice:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Se si verifica un errore di convalida con uno qualsiasi dei campi che sono state registrate usando il `Validation` helper, il `Validation.IsValid` metodo restituisce false. E in tal caso, nessuna parte del codice in tale blocco viene eseguito, in modo che nessuna voce film non valido verranno inserita nel database. E naturalmente si verrà reindirizzati a non al *film* pagina.

Il blocco di codice completo, incluso il codice di convalida, è ora simile a questo esempio:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>La visualizzazione degli errori di convalida

L'ultimo passaggio consiste nella visualizzazione eventuali messaggi di errore. È possibile visualizzare i singoli messaggi per ogni errore di convalida, o è possibile visualizzare un riepilogo, o entrambi. Per questa esercitazione, verranno eseguite entrambe in modo che è possibile verificarne il funzionamento.

Accanto a ogni `<input>` elemento che si sta eseguendo la convalida, chiamare il `Html.ValidationMessage` metodo e passa il nome del `<input>` elemento si effettua la convalida. Inserire il `Html.ValidationMessage` destra metodo in cui si desidera che il messaggio di errore da visualizzare. Quando si esegue la pagina, il `Html.ValidationMessage` esegue il rendering metodo un `<span>` elemento in cui verrà salvato l'errore di convalida. (Se non si verificano errori, il `<span>` elemento sottoposto a rendering, ma è disponibile alcun testo in essa.)

Modificare il markup della pagina in modo che includa un `Html.ValidationMessage` metodo per ognuno dei tre `<input>` gli elementi della pagina, come in questo esempio:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Per vedere come funziona il riepilogo, anche aggiungere il seguente markup e codice subito dopo il `<h1>Add a Movie</h1>` elemento nella pagina:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Per impostazione predefinita, il `Html.ValidationSummary` metodo visualizza tutti i messaggi di convalida in un elenco (una `<ul>` elemento che si trova all'interno di un `<div>` elemento). Come con la `Html.ValidationMessage` metodo, il markup per il riepilogo di convalida viene sempre eseguito; se non sono presenti errori, nessun elemento di elenco viene visualizzato.

Il riepilogo può essere un modo alternativo per visualizzare i messaggi di convalida anziché tramite la `Html.ValidationMessage` metodo per visualizzare ogni errore specifiche del campo. Oppure è possibile usare sia un riepilogo e i dettagli. In alternativa è possibile usare la `Html.ValidationSummary` metodo per visualizzare un errore generico e quindi usare singoli `Html.ValidationMessage` le chiamate per visualizzare i dettagli.

Nella pagina operazione completata sarà simile all'esempio:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

È tutto. È ora possibile testare la pagina per l'aggiunta di un filmato ma escludendo uno o più dei campi. Quando esegue questa operazione, vengono visualizzati l'errore seguente:

![Aggiungi pagina di film che convalida i messaggi di errore](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>Applicazione di stili i messaggi di errore di convalida

È possibile vedere che sono presenti messaggi di errore, ma non si distinguono in molto bene. È un modo semplice per definire lo stile tramite messaggi di errore.

Per definire lo stile di singoli messaggi di errore che vengono visualizzati dal completamento `Html.ValidationMessage`, creare una classe di stile CSS denominata `field-validation-error`. Per definire l'aspetto per il riepilogo di convalida, creare una classe di stile CSS denominata `validation-summary-errors`.

Per verificare il funzionamento di questa tecnica, aggiungere un `<style>` elemento all'interno di `<head>` sezione della pagina. Quindi definire le classi di stile denominate `field-validation-error` e `validation-summary-errors` che contengono le regole seguenti:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

In genere è probabile che venga inserito le informazioni di stile di visualizzazione in un oggetto separato *CSS* file, ma per semplicità è possibile posizionarli nella pagina per il momento. (Più avanti in questa serie di esercitazioni, si sposterà le regole CSS per un oggetto separato *CSS* file.)

Se è presente un errore di convalida, il `Html.ValidationMessage` metodo esegue il rendering di un `<span>` elemento che include `class="field-validation-error"`. Aggiungendo una definizione di stile per tale classe, è possibile configurare come appare il messaggio. Se sono presenti errori, il `ValidationSummary` metodo allo stesso modo in modo dinamico viene eseguito il rendering dell'attributo `class="validation-summary-errors"`.

Eseguire nuovamente la pagina e deliberatamente tralasciare un paio di campi. Gli errori sono più evidenti. (In effetti, si sta overdone, ma che è importante per mostrare operazioni possibili).

![Aggiungi pagina film che mostri gli errori di convalida che sono stati ora uno stile](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Aggiunta di un collegamento alla pagina Movies

Un passaggio finale consiste nel rendere più pratico ottenere il *AddMovie* pagina nell'elenco di film originale.

Aprire il *film* pagina nuovamente. Dopo il tag di chiusura `</div>` tag che segue il `WebGrid` helper, aggiungere il markup seguente:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Come illustrato prima, ASP.NET interpreta il `~` operatore come radice del sito Web. Non è necessario usare il `~` operatore; è possibile usare il markup `<a href="./AddMovie">Add a movie</a>` o in altro modo per definire il percorso che riconosce HTML. Ma il `~` operatore è un buon approccio generale quando si creano collegamenti per le pagine Razor, perché rende più flessibile il sito, ovvero se si sposta la pagina corrente in una sottocartella, il collegamento verrà comunque inviata al *AddMovie* pagina. (Tenere presente che il `~` operatore funziona solo negli *cshtml* pagine. La riconosce e ASP.NET, ma non è codice HTML standard.)

Al termine, eseguire la *film* pagina. Sarà simile a questa pagina:

![Pagina di film con collegamento alla pagina 'Aggiungere film'](entering-data/_static/image7.png)

Fare clic sui **aggiunta di un filmato** collegamento per assicurarsi che consenta di accedere al *AddMovie* pagina.

## <a name="coming-up-next"></a>In arrivo

Nella prossima esercitazione, verrà illustrato come consentire agli utenti di modificare i dati che si trova già nel database.

## <a name="complete-listing-for-addmovie-page"></a>Elenco completo per la pagina AddMovie

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET usando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Istruzione SQL INSERT INTO](http://www.w3schools.com/sql/sql_insert.asp) sul sito W3Schools
- [Convalida dell'Input utente in ASP.NET Web in siti con pagine](https://go.microsoft.com/fwlink/?LinkId=253002). Altre informazioni sull'utilizzo di `Validation` helper.

> [!div class="step-by-step"]
> [Precedente](form-basics.md)
> [Successivo](updating-data.md)
