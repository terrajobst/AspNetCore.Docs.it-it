---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: 'Introduzione a ASP.NET Web Pages: immissione dei dati di Database tramite moduli | Documenti Microsoft'
author: tfitzmac
description: In questa esercitazione viene illustrato come creare un form di immissione e quindi immettere i dati che ottengono dal form in una tabella di database quando si utilizza ASP.NET Web Pages (...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: bbccf8134e90c19e29efaa5afe1e46e15320c189
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897969"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Introduzione a ASP.NET Web Pages: immissione dei dati di Database tramite moduli
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questa esercitazione viene illustrato come creare un form di immissione e quindi immettere i dati che ottengono dal form in una tabella di database quando si utilizzano pagine Web ASP.NET (Razor). Si presuppone di aver completato la serie tramite [nozioni di base di form HTML in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> Illustra quanto segue:
> 
> - Ulteriori informazioni su come elaborare i form di immissione.
> - Come aggiungere dati (insert) in un database.
> - Come verificare che gli utenti avere immesso un valore obbligatorio in un form (come convalidare l'input dell'utente).
> - Come visualizzare gli errori di convalida.
> - Viene descritto come passare a un'altra pagina dalla pagina corrente.
>   
> 
> Funzionalità/tecnologie descritte:
> 
> - Metodo `Database.Execute`.
> - L'istruzione SQL `Insert Into` istruzione
> - Il `Validation` helper.
> - Metodo `Response.Redirect`.


## <a name="what-youll-build"></a>Cosa Compilerai

Nell'esercitazione precedente che è stato illustrato come creare un database immesso i dati di database modificando il database direttamente in WebMatrix, utilizza il **Database** dell'area di lavoro. Nella maggior parte delle App, che non è un modo pratico per inserire dati nel database, tuttavia. Pertanto in questa esercitazione si creerà un'interfaccia basata sul web che consente di o tutti gli utenti di immettere i dati e salvarlo nel database.

Verrà creata una pagina in cui è possibile immettere nuovi film. La pagina conterrà un modulo di voce che include campi (caselle di testo) in cui è possibile immettere un titolo del film, genre e l'anno. La pagina sarà simile a questa pagina:

!['Aggiungi film' pagina nel browser](entering-data/_static/image1.png)

Le caselle di testo saranno in formato HTML `<input>` elementi che eseguirà la ricerca quali questo markup:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Creazione del Form di base voce

Creare una pagina denominata *AddMovie.cshtml*.

Sostituire quello presente nel file con il markup seguente. Sovrascrivi tutto; a breve si aggiungerà un blocco di codice nella parte superiore.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

In questo esempio viene tipico HTML per la creazione di un form. Usa `<input>` elementi per le caselle di testo e per il pulsante Invia. Le didascalie per le caselle di testo vengono create tramite standard `<label>` elementi. Il `<fieldset>` e `<legend>` elementi inserire una casella nice intorno al form.

Si noti che in questa pagina, il `<form>` elemento Usa `post` come valore per il `method` attributo. Nell'esercitazione precedente, è stato creato un form che utilizzato il `get` metodo. Che è stato corretto, perché anche se i valori per il server di inviare il modulo, la richiesta non apportare alcuna modifica. Tutte le è stato recuperare dati in modi diversi. Tuttavia, in questa pagina è *verrà* apportare modifiche, che si intende aggiungere nuovi record di database. Pertanto, è necessario utilizzare questo modulo la `post` metodo. (Per ulteriori informazioni sulla differenza tra `GET` e `POST` operazioni, vedere il[sicurezza verbo HTTP GET e POST](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) barra laterale nell'esercitazione precedente.)

Si noti che ogni casella di testo ha un `name` elemento (`title`, `genre`, `year`). Come osservato nell'esercitazione precedente, questi nomi sono importanti perché è necessario disporre di tali nomi pertanto è possibile ottenere l'input dell'utente in un secondo momento. È possibile utilizzare qualsiasi nome. È consigliabile utilizzare nomi significativi che consentono di ricordare i dati che si sta lavorando.

Il `value` attributo di ogni `<input>` elemento contiene una combinazione di codice Razor (ad esempio, `Request.Form["title"]`). Dopo aver inviato il modulo è stato descritto una versione di questo accorgimento nell'esercitazione precedente per mantenere il valore immesso nella casella di testo (se presente).

## <a name="getting-the-form-values"></a>Ottenere i valori di Form

Aggiungere quindi codice che elabora il modulo. Nella struttura, verranno eseguite le operazioni seguenti:

1. Controllare se la pagina viene registrata (inviato). Si desidera il codice venga eseguita solo quando gli utenti hanno scelto il pulsante, non quando viene eseguito prima della pagina.
2. Ottenere i valori immessi dall'utente nelle caselle di testo. In questo caso, perché il modulo è utilizzato il `POST` verbo, si otterranno i valori di modulo dal `Request.Form` insieme.
3. Inserire i valori come un nuovo record di *filmati* tabella di database.

Nella parte superiore del file, aggiungere il codice seguente:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

Le prime righe creano variabili (`title`, `genre`, e `year`) per contenere i valori dalle caselle di testo. La riga `if(IsPost)` consente di verificare che siano impostate le variabili *solo* quando gli utenti fanno clic il **aggiungere film** pulsante:, ovvero quando il form è stato inviato.

Come osservato in un'esercitazione precedente, ottenere il valore di una casella di testo utilizzando un'espressione come `Request.Form["name"]`, dove *nome* è il nome del `<input>` elemento.

I nomi delle variabili (`title`, `genre`, e `year`) sono arbitrari. Ad esempio i nomi assegnati a `<input>` elementi, è possibile chiamarli liberamente. (I nomi delle variabili non sono necessario corrispondono agli attributi di nome `<input>` gli elementi del modulo.) Come con, ma il `<input>` elementi, è consigliabile utilizzare nomi di variabili che riflettano i dati in essi contenuti. Quando si scrive codice, nomi coerenti rendono più semplice da ricordare i dati che si sta lavorando.

## <a name="adding-data-to-the-database"></a>Aggiunta di dati al Database

Nel codice blocco appena aggiunta, solo *all'interno di* la parentesi graffa chiusa ( `}` ) del `if` blocco (non solo all'interno del blocco di codice), aggiungere il codice seguente:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Questo esempio è simile al codice utilizzati nell'esercitazione precedente operazione di recupero e visualizzazione dati. La riga che inizia con `db =` apre il database, come prima e la riga successiva definisce nuovamente un'istruzione SQL, come abbiamo visto prima. Tuttavia, questa volta definisce un database SQL `Insert Into` istruzione. L'esempio seguente mostra la sintassi generale del `Insert Into` istruzione:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

In altre parole, specificare la tabella per inserire, quindi elencare le colonne da inserire in e quindi elencati i valori da inserire. (Come accennato prima, SQL non è tra maiuscole e minuscole, ma alcuni utenti di sfruttare appieno le parole chiave per rendere più semplice leggere il comando.)

Le colonne che si sta inserendo in già sono elencate nel comando, ovvero `(Title, Genre, Year)`. La parte interessante modo è possibile ottenere i valori dalle caselle di testo nel `VALUES` fa parte del comando. Anziché i valori effettivi, vedrai `@0`, `@1`, e `@2`, che sono naturalmente segnaposto. Quando si esegue il comando (nel `db.Execute` riga), si passano i valori ottenuti dalle caselle di testo.

**Importante**: Tenere presente che l'unico modo mai includere i dati immessi in linea da un utente in un'istruzione SQL da utilizzare come segnaposto, come riportato seguito (`VALUES(@0, @1, @2)`). Se si concatenano input dell'utente in un'istruzione SQL, si espone a un attacco SQL injection, come illustrato in [nozioni fondamentali sui Form in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251581) (l'esercitazione precedente).

Ancora all'interno di `if` di blocco, aggiungere la riga seguente dopo il `db.Execute` riga:

[!code-css[Main](entering-data/samples/sample4.css)]

Dopo aver inserito il nuovo filmato nel database, questa riga si (reindirizzamenti) passa al *filmati* pagina per visualizzare il film appena immesso. Il `~` operatore significa "radice del sito Web". (Il `~` operatore funziona solo nelle pagine ASP.NET, non in formato HTML, in genere.)

Il blocco di codice completo è simile al seguente:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Test del comando Insert (finora)

Non è ancora finito, ma ora è utile per eseguire il test.

Nella visualizzazione ad albero dei file in WebMatrix, fare doppio clic su di *AddMovie.cshtml* pagina e quindi fare clic su **Avvia nel browser**.

!['Aggiungi film' pagina nel browser](entering-data/_static/image2.png)

(Se si finisce con un'altra pagina nel browser, verificare che l'URL sia `http://localhost:nnnnn/AddMovie`), dove *nnnnn* è il numero di porta in uso.)

Ottenere una pagina di errore? In questo caso, leggere con attenzione e assicurarsi che il codice sarà esattamente ciò che è stato indicato in precedenza.

Immettere un filmato in formato &mdash; , ad esempio, utilizzare "Cittadino Kane", "Serie" e "1941". (O qualsiasi altro.) Quindi fare clic su **aggiungere film**.

Se tutto va bene, si viene reindirizzati al *filmati* pagina. Assicurarsi che sia elencato il nuovo filmato.

![Pagina di filmati che mostra appena aggiunta film](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Convalida dell'Input utente

Tornare al *AddMovie* pagina oppure eseguirla nuovamente. Immettere un altro film, ma questa volta, immettere solo il titolo &mdash; , ad esempio, immettere "Casa" in the ora". Quindi fare clic su **aggiungere film**.

Si viene reindirizzati al *filmati* pagina nuovamente. È possibile trovare il nuovo filmato, ma è incompleto.

![Pagina di filmati con nuovo filmato che manca alcuni valori](entering-data/_static/image4.png)

Quando è stato creato il *filmati* tabella, in modo esplicito pronunciata che nessuno dei campi può essere null. Qui è un modulo di immissione di nuovi film che sta lasciare campi vuoti. Che è un errore.

In questo caso, il database non è stata effettivamente generato (o *generare*) un errore. Non ha fornito un genere o un anno, pertanto il codice il *AddMovie* pagina considerati tali valori cosiddetti *stringhe vuote*. Quando l'istruzione SQL `Insert Into` comando eseguito, i campi genre e l'anno non dispongono di utili dati, ma che non siano null.

Ovviamente, non si desidera consentire agli utenti di immettere informazioni su filmati metà vuoto nel database. La soluzione consiste nel convalidare l'input dell'utente. Inizialmente, la convalida verrà semplicemente assicurarsi che l'utente ha immesso un valore per tutti i campi (vale a dire che nessuna di esse contiene una stringa vuota).

> [!TIP]
> 
> **Stringhe null e vuote**
> 
> Nella programmazione, viene fatta distinzione tra i diversi concetti di "Nessun valore". In generale, un valore è *null* se non è stato impostato o inizializzata in alcun modo. Al contrario, è possibile impostare una variabile che prevede dati di caratteri (stringhe) un *una stringa vuota*. In tal caso, il valore non è null. si è appena stato impostato esplicitamente su una stringa di caratteri la cui lunghezza è pari a zero. Le due istruzioni seguenti mostrano la differenza:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> È un po' più complicato rispetto a quello, ma l'aspetto importante è che `null` rappresenta una sorta di uno stato indeterminato.
> 
> E quindi è importante comprendere esattamente quando un valore è null e quando è semplicemente una stringa vuota. Nel codice per il *AddMovie* , si ottiene i valori delle caselle di testo tramite `Request.Form["title"]` e così via. Quando la pagina esegue la prima volta (prima di fare clic sul pulsante), il valore di `Request.Form["title"]` è null. Tuttavia, quando si invia il form, `Request.Form["title"]` Ottiene il valore della `title` casella di testo. Non è ovvio, ma una casella di testo non è null. che dispone di una stringa vuota in essa contenuti. Pertanto, quando il codice viene eseguito in risposta al pulsante fare clic su, `Request.Form["title"]` contiene una stringa vuota.
> 
> Perché questa distinzione è importante? Quando è stato creato il *filmati* tabella, in modo esplicito pronunciata che nessuno dei campi può essere null. Ma qui è un modulo di immissione di nuovi film che sta lasciare campi vuoti. Il database di protestare durante il tentativo di salvataggio di nuovi film che non dispongono di valori per genere o anno ragionevolmente previsto. Ma che è il punto &mdash; anche se queste caselle di testo viene lasciato vuoto, i valori non null, ma sono stringhe vuote. Di conseguenza, si è in grado di salvare nel database con queste colonne vuote di nuovi film &mdash; , ma non null. valori &mdash;. Pertanto, è necessario assicurarsi che gli utenti non inviare una stringa vuota, che è possibile effettuare la convalida dell'input dell'utente.


### <a name="the-validation-helper"></a>L'Helper di convalida

Pagine Web ASP.NET include un helper &mdash; il `Validation` helper &mdash; che è possibile utilizzare per assicurarsi che gli utenti immettono i dati che soddisfano i requisiti. Il `Validation` helper è uno degli helper che è incorporata in a ASP.NET Web Pages, pertanto non è necessario installarlo come un pacchetto tramite NuGet, il modo in cui è installato il supporto di Gravatar in un'esercitazione precedente.

Per convalidare l'input dell'utente, verranno eseguite le operazioni seguenti:

- Utilizzare codice per specificare che si desidera richiedere i valori nelle caselle di testo nella pagina.
- Il codice inserito un test in modo che le informazioni di film vengono aggiunte al database solo se tutti gli elementi viene convalidato correttamente.
- Aggiungere il codice nel codice per visualizzare i messaggi di errore.

Nel blocco di codice nel *AddMovie* verso l'alto nella parte superiore prima le dichiarazioni di variabili, aggiungere il codice seguente:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Si chiama `Validation.RequireField` una volta per ogni campo (`<input>` elemento) in cui si desidera richiedere una voce. È anche possibile aggiungere un messaggio di errore personalizzato per ogni chiamata, come illustrato di seguito. (È cambiato i messaggi per indicare che è possibile inserire qualsiasi presente).

Se si verifica un problema, si desidera evitare che le nuove informazioni di film venga inserito nel database. Nel `if(IsPost)` di blocco, utilizzare `&&` (AND logico) per aggiungere un'altra condizione che consente di verificare `Validation.IsValid()`. Al termine, l'intero `if(IsPost)` blocco è simile a questo codice:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Se si verifica un errore di convalida con uno dei campi che è registrato con il `Validation` helper, il `Validation.IsValid` metodo restituisce false. E, in tal caso, nessuna parte del codice in tale blocco verrà eseguito in modo non voci film non valido verranno inseriti nel database. E naturalmente si viene reindirizzati a non per il *filmati* pagina.

Il blocco di codice completo, incluso il codice di convalida, sarà simile al seguente:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>La visualizzazione degli errori di convalida

L'ultimo passaggio consiste per visualizzare eventuali messaggi di errore. È possibile visualizzare i singoli messaggi per ogni errore di convalida oppure è possibile visualizzare un riepilogo, o entrambi. Per questa esercitazione, verranno eseguite entrambe in modo che è possibile verificarne il funzionamento.

Accanto a ogni `<input>` elemento che sta effettuando la convalida, chiamata di `Html.ValidationMessage` metodo e passa il nome del `<input>` elemento si effettua la convalida. Inserire il `Html.ValidationMessage` destra metodo in cui si desidera venga visualizzato il messaggio di errore. Quando la pagina viene eseguita, la `Html.ValidationMessage` metodo esegue il rendering un `<span>` elemento entra in cui l'errore di convalida. (Se non si verificano errori, il `<span>` elemento sottoposto a rendering, ma è disponibile alcun testo in essa contenuti.)

Modificare il markup della pagina in modo che includa un `Html.ValidationMessage` metodo per ognuno dei tre `<input>` gli elementi della pagina, come nell'esempio:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Per verificare il funzionamento del riepilogo, aggiungere il markup seguente e codice subito dopo il `<h1>Add a Movie</h1>` elemento nella pagina:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Per impostazione predefinita, il `Html.ValidationSummary` metodo visualizza tutti i messaggi di convalida in un elenco (un `<ul>` elemento all'interno un `<div>` elemento). Come con la `Html.ValidationMessage` metodo, il markup per il riepilogo di convalida viene sempre eseguito il rendering; se non sono presenti errori, nessun elemento di elenco viene visualizzato.

Il riepilogo può essere un modo alternativo per visualizzare i messaggi di convalida anziché tramite il `Html.ValidationMessage` metodo per visualizzare ogni errore specifico del campo. È possibile utilizzare sia un riepilogo e i dettagli. Oppure è possibile utilizzare il `Html.ValidationSummary` metodo per visualizzare un errore generico e quindi utilizzare singoli `Html.ValidationMessage` chiamate per visualizzare i dettagli.

Nella pagina operazione completata sarà simile al seguente:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

È tutto. È ora possibile testare la pagina per l'aggiunta di un filmato ma l'omissione di uno o più dei campi. Quando si esegue l'operazione, vengono visualizzati l'errore seguente:

![Aggiungi pagina film mostrare i messaggi di errore di convalida](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>I messaggi di errore di convalida di stile

Si noterà che sono presenti messaggi di errore, ma non si distinguono molto bene. È un modo semplice per definire lo stile anche se i messaggi di errore.

Per definire lo stile di singoli messaggi di errore visualizzati da `Html.ValidationMessage`, creare una classe di stile CSS denominata `field-validation-error`. Per definire l'aspetto per il riepilogo di convalida, creare una classe di stile CSS denominata `validation-summary-errors`.

Per verificare il funzionamento di questa tecnica, aggiungere un `<style>` elemento all'interno di `<head>` sezione della pagina. Quindi definire le classi di stile denominate `field-validation-error` e `validation-summary-errors` che contengono le regole seguenti:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

In genere è probabile che venga inserito le informazioni di stile in un oggetto distinto *CSS* file, ma per semplicità è possibile inserire nella pagina per il momento. (Più avanti in questo set di esercitazione, si sposterà le regole CSS in una diversa *CSS* file.)

Se si verifica un errore di convalida, il `Html.ValidationMessage` metodo esegue il rendering un `<span>` elemento che include `class="field-validation-error"`. Tramite l'aggiunta di una definizione di stile per tale classe, è possibile configurare il messaggio l'aspetto seguente. Se sono presenti errori, il `ValidationSummary` metodo allo stesso modo, in modo dinamico viene eseguito il rendering dell'attributo `class="validation-summary-errors"`.

Eseguire nuovamente la pagina e tralasciare deliberatamente un paio di campi. Gli errori sono ora più evidenti. (In realtà, si sta overdone, ma che è solo per illustrare ciò che è possibile eseguire).

![Aggiungi pagina filmato che mostri gli errori di convalida che sono stati stile](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Aggiunta di un collegamento alla pagina di filmati

Un passaggio finale consiste nel rendere più pratico ottenere il *AddMovie* pagina dall'elenco di film originale.

Aprire il *filmati* pagina nuovamente. Dopo la chiusura `</div>` tag che segue il `WebGrid` helper, aggiungere il markup seguente:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Come abbiamo visto prima, ASP.NET interpreta il `~` operatore come radice del sito Web. Non è necessario utilizzare il `~` operatore, è possibile utilizzare il markup `<a href="./AddMovie">Add a movie</a>` o in altro modo per definire il percorso che riconosce HTML. Ma la `~` operatore è un buon approccio generale quando si creano collegamenti per pagine Razor, perché rende più flessibile del sito, se si sposta la pagina corrente in una sottocartella, il collegamento verrà ancora inviata al *AddMovie* pagina. (Tenere presente che il `~` operatore funziona solo in *. cshtml* pagine. La riconosce e ASP.NET, ma non è codice HTML standard.)

Al termine, eseguire il *filmati* pagina. Sarà simile a questa pagina:

![Pagina di filmati con collegamento alla pagina 'Aggiungere filmati'](entering-data/_static/image7.png)

Fare clic su di **aggiungere un filmato** collegamento per assicurarsi che passa al *AddMovie* pagina.

## <a name="coming-up-next"></a>Esercitazione seguente

Nella prossima esercitazione, si apprenderà come consentire agli utenti di modificare i dati che si trova già nel database.

## <a name="complete-listing-for-addmovie-page"></a>Elenco completo per la pagina AddMovie

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET con sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Istruzione SQL INSERT INTO](http://www.w3schools.com/sql/sql_insert.asp) sul sito W3Schools
- [Convalida dell'Input utente in ASP.NET Web Pages siti](https://go.microsoft.com/fwlink/?LinkId=253002). Ulteriori informazioni sull'utilizzo di `Validation` helper.

> [!div class="step-by-step"]
> [Precedente](form-basics.md)
> [Successivo](updating-data.md)
