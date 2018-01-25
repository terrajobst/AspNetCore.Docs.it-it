---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Introduzione a ASP.NET Web Pages - nozioni di base di Form HTML | Documenti Microsoft
author: tfitzmac
description: Questa esercitazione illustra le nozioni di base su come creare un modulo di input e come gestire l'input dell'utente quando si utilizzano pagine Web ASP.NET (Razor). E ora che si...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 68056759b2e80230e5fd2c0f9b2d2a89b549cf37
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>Introduzione a ASP.NET Web Pages - nozioni di base di Form HTML
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra le nozioni di base su come creare un modulo di input e come gestire l'input dell'utente quando si utilizzano pagine Web ASP.NET (Razor). E ora che hai un database, si userà le proprie competenze form per consentire agli utenti di trovare film specifici nel database. Si presuppone di aver completato la serie tramite [introduzione di visualizzazione di dati utilizzando ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Illustra quanto segue:
> 
> - Come creare un form, utilizzando gli elementi HTML standard.
> - In un modulo di input come di lettura dell'utente.
> - Come creare una query SQL che in modo selettivo Ottiene i dati tramite una ricerca termini che l'utente fornisce.
> - Come campi nella pagina "ricordano" l'utente ha immesso.
>   
> 
> Funzionalità/tecnologie descritte:
> 
> - Oggetto `Request`.
> - L'istruzione SQL `Where` clausola.


## <a name="what-youll-build"></a>Cosa Compilerai

Nell'esercitazione precedente, creato un database, aggiungere dati e quindi utilizzato il `WebGrid` helper per visualizzare i dati. In questa esercitazione si aggiungerà una casella di ricerca che consente di trovare film di un genere specifico o il cui titolo contiene qualsiasi parola immesso. (Ad esempio, sarà in grado di trovare tutti i film appartenenti al genere è "Azione" o il cui titolo contiene "Harry" o "Adventure.")

Una volta terminato con questa esercitazione, è necessario una pagina simile alla seguente:

![Pagina di filmati con ricerca Genre e titolo](form-basics/_static/image1.png)

La parte di elenco della pagina è uguale a quello dell'ultima esercitazione &mdash; una griglia. La differenza sarà che nella griglia vengono visualizzate solo i filmati che per effettuare la ricerca.

## <a name="about-html-forms"></a>Informazioni sui moduli HTML

(Se si ha esperienza con la creazione di form HTML e con la differenza tra `GET` e `POST`, è possibile ignorare questa sezione.)

Un form dispone di elementi di input utente &mdash; caselle di testo, pulsanti, pulsanti di opzione, caselle di controllo, elenchi a discesa e così via. Gli utenti di compilare in questi controlli o effettuare selezioni e quindi inviare il modulo facendo clic su un pulsante.

La sintassi di un form HTML di base è illustrata in questo esempio:

[!code-html[Main](form-basics/samples/sample1.html)]

Quando questo codice viene eseguito in una pagina, viene creato un form semplice che è simile a questa illustrazione:

![Base form HTML come visualizzabile nel browser](form-basics/_static/image2.png)

Il `<form>` elemento include gli elementi HTML da inviare. (È un semplice errore apportare per aggiungere elementi alla pagina ma quindi dimenticare di inserirle in un `<form>` elemento. In questo caso, non viene inviato.) Il `method` attributo indica al browser come inviare l'input dell'utente. È impostata su `post` se stai eseguendo un aggiornamento sul server o a `get` se si sta solo recupero dei dati dal server.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **Sicurezza verbo HTTP GET e POST**
> 
> HTTP, il protocollo utilizzato per lo scambio di informazioni, browser e server è molto semplice durante le operazioni di base. I browser usano solo alcuni verbi per effettuare richieste al server. Quando si scrive codice per il web, è utile comprendere tali verbi e utilizzarli come il browser e il server. Senza dubbio i verbi utilizzati più di frequente sono:
> 
> - `GET`. Il browser Usa questo verbo per recuperare un elemento dal server. Ad esempio, quando si digita un URL nel browser, nel browser viene eseguito un `GET` operazione per richiedere la pagina desiderata. Se la pagina include grafica, il browser esegue ulteriori `GET` operazioni per ottenere le immagini. Se il `GET` operazione deve passare informazioni al server, le informazioni vengono passate come parte dell'URL nella stringa di query.
> - `POST`. Il browser invia una `POST` richiesta per poter inviare i dati per essere aggiunti o modificati nel server. Ad esempio, il `POST` verbi viene utilizzato per creare i record in un database o modificare quelli esistenti. La maggior parte dei casi, quando si compila un modulo e fare clic sul pulsante Invia, nel browser viene eseguito un `POST` operazione. In un `POST` operazione, i dati passati al server si trova nel corpo della pagina.
> 
> Un'importante differenza tra questi verbi è che un `GET` operazione non è consentito per apportare modifiche nel server, o per inserirlo in modo leggermente più astratto, un `GET` operazione non produce una modifica nello stato nel server. È possibile eseguire un `GET` operazione nelle stesse risorse di quante volte si desidera, e tali risorse non modificate. (A `GET` operazione viene spesso definita "sicurezza", o per l'utilizzo di un termine tecnico, è *idempotente*.) Al contrario, naturalmente, un `POST` richiesta di modifica un elemento nel server ogni volta che si esegue l'operazione.
> 
> Due esempi aiuteranno a illustrare questa distinzione. Quando si esegue una ricerca usando un motore come Bing o Google, si compila un modulo che include una casella di testo e quindi si sceglie il pulsante di ricerca. Nel browser viene eseguito un `GET` operazione, con il valore immesso nella casella di passato come parte dell'URL. Utilizzando un `GET` operazione per questo tipo di modulo è un problema, perché un'operazione di ricerca non modifica le risorse nel server, vengono recuperati solo informazioni.
> 
> Si consideri ora il processo di ordinamento in linea. Compilare nei dettagli dell'ordine, quindi fare clic sul pulsante Invia. Questa operazione verrà ritentata un `POST` richiesta, perché l'operazione comporterà modifiche nel server, ad esempio un nuovo record di ordine, una modifica di informazioni dell'account e forse molte altre modifiche. A differenza di `GET` operazione, non è possibile ripetere il `POST` richiesta: se è stato eseguito, ogni volta che inviato nuovamente la richiesta, si genera un nuovo ordine nel server. (In casi come questo, siti Web verrà spesso visualizzato un avviso di non fare clic su un pulsante di invio più volte o disabiliterà il pulsante di invio in modo che si non invia nuovamente il modulo accidentalmente.)
> 
> Nel corso di questa esercitazione si userà entrambi un `GET` operazione e un `POST` operazione utilizzare i moduli HTML. Verrà illustrato in ogni caso perché al verbo utilizzato è quello appropriato.
> 
> (Per ulteriori informazioni sui verbi HTTP, vedere il [le definizioni di metodo](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) articolo sul sito di W3C.)


La maggior parte degli elementi di input utente sono in formato HTML `<input>` elementi. Aspetto `<input type="type" name="name">,` in *tipo* indica il tipo di controllo di input utente desiderato. Questi elementi sono più comuni:

- Casella di testo:`<input type="text">`
- Casella di controllo:`<input type="check">`
- Pulsante di opzione:`<input type="radio">`
- Pulsante:`<input type="button">`
- Pulsante di invio:`<input type="submit">`

È inoltre possibile utilizzare il `<textarea>` elemento per creare una casella di testo su più righe e `<select>` elemento per creare un elenco di riepilogo a discesa o scorrevole. (Per ulteriori informazioni su HTML costituiscono gli elementi, vedere [form HTML e Input](http://www.w3schools.com/html/html_forms.asp) nel sito W3Schools.)

Il `name` attributo è molto importante, perché il nome come si otterrà il valore dell'elemento in un secondo momento, come si vedrà a breve.

La parte interessante è, lo sviluppatore della pagina, procedere con l'input dell'utente. Non vi è alcun comportamento predefinito associato a questi elementi. In alternativa, è necessario ottenere i valori che l'utente ha immesso o selezionato ed eseguire un'operazione con essi. Si tratta in questa esercitazione verrà illustrato.

> [!TIP] 
> 
> **HTML5 e moduli di Input**
> 
> Come è noto, HTML è in transizione e la versione più recente (HTML5) include il supporto per la modalità più intuitive agli utenti di immettere le informazioni. Ad esempio, in HTML5, si (lo sviluppatore della pagina) può indicare la pagina che si desidera che l'utente di immettere una data. Il browser può quindi visualizzare automaticamente un calendario, anziché richiedere all'utente di immettere una data manualmente. Tuttavia, HTML5 è nuovo e non è supportato in tutti i browser ancora.
> 
> Le pagine Web ASP.NET supporta HTML5 nella misura in cui il browser dell'utente non di input. Per avere un'idea dei nuovi attributi per il `<input>` elemento HTML5, vedere [HTML &lt;input&gt; attributo type](http://www.w3schools.com/html/html_form_input_types.asp) nel sito W3Schools.


## <a name="creating-the-form"></a>Creazione del form

In WebMatrix nel **file** dell'area di lavoro, aprire il *Movies.cshtml* pagina.

Dopo la chiusura `</h1>` tag e prima dell'apertura `<div>` tag del `grid.GetHtml` chiamare, aggiungere il markup seguente:

[!code-html[Main](form-basics/samples/sample2.html)]

Questo codice crea un modulo che include una casella di testo denominata `searchGenre` e un pulsante di invio. Il testo casella e inviare pulsante sono racchiusi tra parentesi una `<form>` elemento il cui `method` attributo è impostato su `get`. (Tenere presente che se si non posizionare la casella di testo e inviare pulsante all'interno di un `<form>` elemento, non verranno inviate quando si fa clic sul pulsante.) Utilizzare il `GET` verbo qui perché si sta creando un modulo che non apportare modifiche nel server, risulta in una ricerca. (Nell'esercitazione precedente è stata usata una `post` metodo, che è una modalità di invio delle modifiche al server. Si noterà che nella prossima esercitazione nuovamente.)

Eseguire la pagina. Anche se è ancora stato definito alcun comportamento per il form, è possibile visualizzare l'aspetto seguente:

![Pagina di filmati con casella di ricerca per genere](form-basics/_static/image3.png)

Immettere un valore nella casella di testo, ad esempio "Commedia". Quindi fare clic su **ricerca Genre**.

Prendere nota dell'URL della pagina. Quanto è stato impostato il `<form>` dell'elemento `method` attributo `get`, il valore immesso fa ora parte della stringa di query nell'URL, simile al seguente:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Lettura di valori di Form

La pagina contiene già codice che ottiene dati del database e visualizza i risultati in una griglia. È necessario aggiungere il codice che legge il valore della casella di testo, quindi è possibile eseguire una query SQL che include il termine di ricerca.

Poiché il metodo del form è impostata su `get`, è possibile leggere il valore immesso nella casella di testo utilizzando codice simile al seguente:

`var searchTerm = Request.QueryString["searchGenre"];`

Il `Request.QueryString` oggetto (il `QueryString` proprietà del `Request` oggetto) include i valori degli elementi che sono stati inviati come parte di `GET` operazione. Il `Request.QueryString` proprietà contiene un *raccolta* (elenco) dei valori che vengono inviati nel modulo. Per ottenere ogni singolo valore, specificare il nome dell'elemento desiderato. Per tale motivo è necessario avere un `name` attributo la `<input>` elemento (`searchTerm`) che crea la casella di testo. (Per altre informazioni sul `Request` , vedere il [barra laterale](#BKMK_TheRequestObject) in un secondo momento.)

È abbastanza semplice per leggere il valore della casella di testo. Tuttavia, se l'utente non immettere nulla affatto nella casella di testo, ma fare clic su **ricerca** comunque, è possibile ignorare tale clic, poiché non è necessario eseguire la ricerca.

Il codice riportato di seguito è riportato un esempio che illustra come implementare queste condizioni. (Non è necessario aggiungere ancora il codice, che verranno eseguite in un momento).

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Suddivide il test in questo modo:

- Ottenere il valore di `Request.QueryString["searchGenre"]`, ovvero il valore che è stato inserito il `<input>` elemento denominato `searchGenre`.
- Scoprire se è vuoto tramite il `IsEmpty` metodo. Questo metodo è la modalità standard per determinare se un elemento (ad esempio, un elemento di formato) contiene un valore. Ma soprattutto è rilevante solo se è stato *non* vuoto, di conseguenza...
- Aggiungere il `!` operatore in primo piano la `IsEmpty` test. (Il `!` operatore significa NOT logico).

In parole semplici, l'intero `if` condizione si traduce in quanto segue: *se elemento searchGenre del modulo non è vuoto, quindi...*

Questo blocco imposta la fase per la creazione di una query che utilizza il termine di ricerca. È possibile farlo nella sezione successiva.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **L'oggetto richiesta**
> 
> Il `Request` oggetto contiene tutte le informazioni che il browser invia all'applicazione quando viene richiesto o inviata una pagina. Questo oggetto include tutte le informazioni fornite dall'utente, ad esempio i valori di casella di testo o un file da caricare. Include anche tutti i tipi di informazioni aggiuntive, come i cookie, i valori nella stringa di query di URL (se presente), il percorso del file della pagina su cui è in esecuzione, il tipo di browser è in uso, l'elenco delle lingue che vengono impostate nel browser e molto altro ancora.
> 
> Il `Request` oggetto è un *raccolta* (elenco) di valori. Ottenere un singolo valore della raccolta specificandone il nome:
> 
> `var someValue = Request["name"];`
> 
> Il `Request` oggetto effettivamente espone diversi subset. Ad esempio:
> 
> - `Request.Form`Fornisce i valori dagli elementi all'interno di inviato `<form>` elemento se la richiesta è un `POST` richiesta.
> - `Request.QueryString`consente solo i valori presenti nella stringa di query dell'URL. (In un URL come `http://mysite/myapp/page?searchGenre=action&page=2`, `?searchGenre=action&page=2` sezione dell'URL è la stringa di query.)
> - `Request.Cookies`raccolta consente di accedere ai cookie che ha inviato il browser.
> 
> Per ottenere un valore che si è certi del modulo inviato, è possibile utilizzare `Request["name"]`. In alternativa, è possibile utilizzare le versioni più specifiche `Request.Form["name"]` (per `POST` richieste) o `Request.QueryString["name"]` (per `GET` richieste). Naturalmente, *nome* è il nome dell'elemento da ottenere.
> 
> Il nome dell'elemento di cui che si desidera ottenere deve essere univoco all'interno della raccolta in uso. Per questa ragione il `Request` oggetto include i sottoinsiemi come `Request.Form` e `Request.QueryString`. Si supponga che la pagina contiene un elemento form denominato `userName` e *anche* contiene un cookie denominato `userName`. Se si verificano `Request["userName"]`, è ambiguo se si desidera il valore di modulo o il cookie. Tuttavia, se si verificano `Request.Form["userName"]` o `Request.Cookie["userName"]`, sta siano espliciti sul valore da ottenere.
> 
> È buona norma specifici e utilizzare il subset di `Request` che si è interessati, ad esempio `Request.Form` o `Request.QueryString`. Per le pagine semplice che si desidera creare in questa esercitazione, probabilmente non viene effettivamente rende eventuali differenze. Tuttavia, quando si creano pagine più complesse, utilizzando la versione esplicita `Request.Form` o `Request.QueryString` può aiutare a evitare i problemi che possono verificarsi quando la pagina contiene un modulo o più moduli, i cookie, i valori di stringa di query e così via.


## <a name="creating-a-query-by-using-a-search-term"></a>Creazione di una Query con un termine di ricerca

Ora che appreso come ottenere il termine di ricerca immesso dall'utente, è possibile creare una query che lo utilizza. Tenere presente che, per ottenere tutti gli elementi del film all'esterno del database, si sta utilizzando una query SQL che è simile a questa istruzione:

`SELECT * FROM Movies`

Per ottenere solo determinati film, è necessario utilizzare una query che include un `Where` clausola. Questa clausola consente di impostare una condizione in cui viene restituita dalla query. Di seguito è riportato un esempio:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Il formato di base è `WHERE column = value`. È possibile utilizzare diversi operatori oltre appena `=`, ad esempio `>` (maggiore di), `<` (minore di), `<>` (non uguale a), `<=` (minore o uguale a), ecc., a seconda di ciò che sta cercando.

Nel caso in cui si desidera sapere per, le istruzioni SQL non sono sensibili alle maiuscole &mdash; `SELECT` equivale `Select` (o perfino `select`). Tuttavia, gli utenti spesso converte in maiuscolo parole chiave in un'istruzione SQL, ad esempio `SELECT` e `WHERE`, per semplificare la lettura.

### <a name="passing-the-search-term-as-a-parameter"></a>Passando al termine di ricerca come parametro

La ricerca di un genere specifico è abbastanza semplice (`WHERE Genre = 'Action'`), ma si desidera essere in grado di eseguire la ricerca di qualsiasi genere immesso dall'utente. A tale scopo, si crea come query SQL che include un segnaposto per il valore per la ricerca. Sarà simile a questo comando:

`SELECT * FROM Movies WHERE Genre = @0`

Il segnaposto è il `@` caratteri seguiti da zero. Come può immaginare, una query può contenere più segnaposto e verranno denominati `@0`, `@1`, `@2`e così via.

Per impostare l'esecuzione della query e passare effettivamente il valore, utilizzare codice simile al seguente:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Questo codice è simile a ciò che già fatto per visualizzare i dati nella griglia. Le uniche differenze sono:

- La query contiene un segnaposto (`WHERE Genre = @0"`).
- La query viene inserita in una variabile (`selectCommand`); prima, la query è stato passato direttamente il `db.Query` metodo.
- Quando si chiama il `db.Query` metodo, passare la query sia il valore da utilizzare per il segnaposto. (Se la query conteneva più segnaposto, è necessario passare tutti come separare i valori al metodo.)

Se si inseriscono tutti questi elementi insieme, viene visualizzato il codice seguente:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Importante!** Utilizzo di segnaposti (come `@0`) per passare valori a un comando SQL è *estremamente importante* per la sicurezza. Il modo in cui è visualizzato in questo caso, con segnaposto per i dati della variabile, è l'unico modo, è necessario costruire i comandi SQL.
> 
> Mai costruire un'istruzione SQL, combinazione di testo letterale (concatenazione) e i valori che si ottiene da parte dell'utente. Concatenazione di input dell'utente in un'istruzione SQL viene aperto il sito a un *attacco SQL injection* in cui un utente malintenzionato invia i valori alla pagina hack del database. (È possibile leggere informazioni nell'articolo [attacchi SQL Injection](https://msdn.microsoft.com/library/ms161953.aspx) il sito Web MSDN.)


## <a name="updating-the-movies-page-with-search-code"></a>Aggiornare la pagina di filmati con codice di ricerca

Ora è possibile aggiornare il codice di *Movies.cshtml* file. Per iniziare, sostituire il codice nel blocco di codice nella parte superiore della pagina con il seguente codice:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

La differenza è che finora la query nella `selectCommand` variabile, verrà passato al `db.Query` in un secondo momento. Inserire l'istruzione SQL in una variabile consente di modificare l'istruzione, che è per eseguire la ricerca verrà eseguita.

Hai rimosso anche queste due righe, verrà inserita nuovamente nella versione successiva:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Non si desidera eseguire la query ancora (vale a dire chiamare `db.Query`) e non si desidera inizializzare il `WebGrid` helper ancora uno. Dopo aver calcolato deve eseguire l'istruzione SQL verranno eseguite tali operazioni.

Dopo questo blocco riscritto, è possibile aggiungere la nuova logica per la gestione della ricerca. Il codice completato sarà simile al seguente. Aggiornare il codice della pagina in modo che corrisponda in questo esempio:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

La pagina funziona ora simile al seguente. Ogni volta che la pagina viene eseguita, il codice verrà aperto il database e `selectCommand` variabile è impostata per l'istruzione SQL che ottiene tutti i record di `Movies` tabella. Il codice inizializza anche la `searchTerm` variabile.

Tuttavia, se la richiesta corrente include un valore per il `searchGenre` elemento, il codice imposta `selectCommand` per una query diversa, in particolare, in modo che include il `Where` clausola per cercare un genere. Imposta inoltre `searchTerm` per l'elemento è stato passato per la casella di ricerca (che può essere nothing).

Indipendentemente dal fatto che SQL istruzione si trova in `selectCommand`, il codice chiama quindi `db.Query` per eseguire la query, passando qualsiasi è `searchTerm`. Se non c'è niente `searchTerm`, non è importante, poiché in tal caso è presente alcun parametro per passare il valore di `selectCommand` comunque.

Infine, il codice inizializza il `WebGrid` helper utilizzando i risultati della query, esattamente come prima.

È possibile vedere che inserendo l'istruzione SQL e al termine di ricerca in variabili, aggiunti flessibilità al codice. Come verrà illustrato più avanti in questa esercitazione, è possibile utilizzare questo framework di base e continuare ad aggiungere la logica per diversi tipi di ricerca.

## <a name="testing-the-search-by-genre-feature"></a>Verifica la funzionalità di ricerca-per-Genre

In WebMatrix, eseguire il *Movies.cshtml* pagina. Viene visualizzata la pagina con la casella di testo per genere.

Immettere un genere che è immesso per uno dei record di test, quindi fare clic su **ricerca**. Questa volta, verrà visualizzato un elenco di solo i filmati che corrispondono a tale genere:

![Pagina di filmati elenco dopo la ricerca di Comedies' genre'](form-basics/_static/image4.png)

Immettere un genere diverso e ripetere la ricerca. Provare a immettere il genere usando tutte le lettere tutte lettere maiuscole o minuscole in modo che è possibile vedere che la ricerca non fa distinzione tra maiuscole e minuscole.

## <a name="remembering-what-the-user-entered"></a>"Registrazione", l'utente ha immesso

È possibile notare che dopo aver immesso un genere e si fa clic su **ricerca Genre**, si verifica una voce per tale genere. Tuttavia, la casella di testo di ricerca è vuota &mdash; in altre parole, non memorizza sia stato immesso.

È importante comprendere perché questo comportamento si verifica. Quando si invia una pagina, il browser invia una richiesta al server web. Quando ASP.NET riceve la richiesta, crea una nuova istanza della pagina, viene eseguito il codice in essa contenuti e quindi esegue il rendering di pagina nel browser. In effetti, tuttavia, la pagina non sa che stavano solo con una versione precedente di se stesso. Tutti che sa che ha ricevuto una richiesta contenente alcuni dati del modulo in essa contenuti.

Ogni volta che si richiede una pagina &mdash; se per la prima volta o inviandolo &mdash; si riceve una nuova pagina. Il server web non ha in memoria l'ultima richiesta. Neanche ASP.NET e neanche il browser. La connessione sola tra queste istanze separate di pagina è tutti i dati che si trasmettono tra di essi. Se si invia una pagina, ad esempio, la nuova istanza di pagina può ottenere i dati di modulo che è stati inviati dall'istanza precedente. (Un altro modo per passare dati tra le pagine consiste nell'utilizzare i cookie).

È un modo formale per descrivere questa situazione significa che le pagine web sono *senza stato*. I server Web e pagine stesse e gli elementi nella pagina non mantengono le informazioni sullo stato di una pagina precedente. Il web è stato progettato in questo modo perché Gestione dello stato per le singole richieste sarebbe rapidamente esaurire le risorse del server web, che spesso gestire migliaia, forse persino centinaia di migliaia di richieste al secondo.

Quindi la casella di testo è vuota. Dopo che è stata inviata la pagina, ASP.NET creata una nuova istanza della pagina ed eseguito tramite il codice e markup. Non esegue alcuna operazione che il codice che ci hai ASP.NET per inserire un valore nella casella di testo. Pertanto, ASP.NET non esegue alcuna operazione e la casella di testo è stato eseguito il rendering senza un valore in essa contenuti.

È in realtà un modo semplice per risolvere questo problema. Il genere immesso nella casella di testo *è* disponibili nel codice &mdash; in `Request.QueryString["searchGenre"]`.

Aggiornare il markup per la casella di testo in modo che il `value` attributo Ottiene il valore dalla `searchTerm`, come nell'esempio:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

In questa pagina, è possibile avere impostare anche la `value` attributo la `searchTerm` variabile, poiché la variabile contiene inoltre il genere immesso. Ma tramite il `Request` per impostare il `value` attributo come illustrato di seguito è la modalità standard per questa attività. (Supponendo che si desidera eseguire questa operazione anche &mdash; in alcuni casi, si potrebbe voler il rendering della pagina *senza* valori nei campi. Tutto dipende cosa succede con l'app.)

> [!NOTE]
> È possibile "risalire" il valore di una casella di testo che viene utilizzato per le password. Sarebbe un problema di sicurezza per consentire agli utenti di compilare un campo della password tramite il codice.


Eseguire nuovamente la pagina, immettere un genere e fare clic su **ricerca Genre**. Questa fase non solo verranno visualizzati i risultati della ricerca, ma la casella di testo memorizza l'ora dell'ultima immesso:

![Pagina che indicano che la casella di testo è 'memorizzate' la voce precedente](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Cercare qualsiasi parola nel titolo

Ora è possibile cercare qualsiasi genere, ma è anche possibile cercare un titolo. È difficile predisporre un titolo esattamente durante la ricerca, pertanto che è possibile cercare una parola che compare in un punto qualsiasi all'interno di un titolo. A tale scopo, in SQL, utilizzare il `LIKE` operatore e la sintassi è simile alla seguente:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Questo comando Ottiene tutti i film cui titoli contengono "adventure". Quando si utilizza il `LIKE` (operatore), includere il carattere jolly `%` come parte del termine di ricerca. La ricerca `LIKE 'adventure%'` significa "a partire con 'adventure'". (Tecnicamente, significa "La stringa 'adventure' seguito da niente.") Analogamente, il termine di ricerca `LIKE '%adventure'` corrisponde a "qualsiasi seguito dalla stringa 'adventure'", che è possibile ad esempio "finale con 'adventure'".

Al termine di ricerca `LIKE '%adventure%'` pertanto "con"adventure' nel titolo." (Tecnicamente, "qualsiasi elemento nel titolo, seguito da 'adventure', seguito da niente.")

All'interno di `<form>` elemento, aggiungere il markup seguente immediatamente sotto la chiusura `</div>` tag per la ricerca di genere (appena prima della chiusura `</form>` elemento):

[!code-html[Main](form-basics/samples/sample10.html)]

Il codice per gestire questa ricerca è simile al codice per la ricerca del genere, ad eccezione del fatto che è necessario raccogliere il `LIKE` ricerca. All'interno del blocco di codice nella parte superiore della pagina, aggiungere questo `if` blocco subito dopo il `if` blocco per la ricerca di genere:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Questo codice Usa la stessa logica illustrato in precedenza, ad eccezione del fatto che la ricerca viene utilizzato un `LIKE` operatore e inserisce il codice "`%`" prima e dopo il termine di ricerca.

Si noti come è facile aggiungere un'altra ricerca per pagina. Tutto era stato:

- Creare un `if` blocco in cui è stato testato per verificare se la casella di ricerca rilevanti conteneva un valore.
- Impostare il `selectCommand` variabile a una nuova istruzione SQL.
- Impostare il `searchTerm` variabile con il valore per passare alla query.

Di seguito è riportato il blocco di codice completo, che contiene la nuova logica per la ricerca di un titolo:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Di seguito è riportato un riepilogo delle funzionalità di questo codice:

- Le variabili `searchTerm` e `selectCommand` vengono inizializzati nella parte superiore. Che si desidera impostare tali variabili al termine di ricerca appropriato (se presente) e il comando SQL appropriato in base alle operazioni eseguite dall'utente nella pagina. La ricerca del valore predefinito è il caso semplice di recupero di tutti i film dal database.
- I test per `searchGenre` e `searchTitle`, il codice imposta `searchTerm` sul valore desiderato per la ricerca. Questi blocchi di codice imposta inoltre `selectCommand` a un comando SQL appropriato per la ricerca.
- Il `db.Query` metodo viene richiamato una sola volta, mediante il comando SQL è in `selectedCommand` e indipendentemente dal valore `searchTerm`. Se è presente alcun termine di ricerca (nessun genre e word Nessun titolo), il valore di `searchTerm` è una stringa vuota. Tuttavia, che non è rilevante, perché in tal caso la query non richiede un parametro.

## <a name="testing-the-title-search-feature"></a>La funzionalità di ricerca del titolo di test

È ora possibile testare la pagina di ricerca completata. Eseguire *Movies.cshtml*.

Immettere un genere e fare clic su **ricerca Genre**. Nella griglia vengono visualizzate film di questo genere, come prima.

Immettere un titolo e fare clic su **ricerca titolo**. Nella griglia vengono visualizzate filmati che contengono tale parola nel titolo.

![Elenco dopo la ricerca di 'La' nel titolo della pagina di filmati](form-basics/_static/image6.png)

Lasciare entrambe le caselle di testo e fare clic su uno dei pulsanti. La griglia visualizza tutti i film.

## <a name="combining-the-queries"></a>Combinare le query

È possibile notare che è possibile eseguire ricerche basate sono esclusive. È possibile cercare il titolo e il genere nello stesso momento, anche se entrambe le caselle di ricerca contengono i valori in esse contenuti. Ad esempio, è possibile cercare tutti i filmati azione il cui titolo contiene "Adventure". (Come la pagina viene codificata a questo punto, se si immettono valori per il genere e titolo, Microsoft la ricerca di titoli Ottiene la priorità.) Per creare una ricerca che combina le condizioni, è necessario creare una query SQL con la sintassi seguente:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Ed è necessario eseguire la query utilizzando un'istruzione simile al seguente (approssimativamente parlando):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Creazione di logica per consentire le permutazioni di molti criteri di ricerca possibile ottenere un po' coinvolte, come si può vedere. Pertanto, si sarà più possibile qui.

## <a name="coming-up-next"></a>Esercitazione seguente

Nella prossima esercitazione, si creerà una pagina che utilizza un modulo per consentire agli utenti di aggiungere filmati al database.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Completare l'elenco per pagina filmato (aggiornata con ricerca)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Clausola WHERE SQL](http://www.w3schools.com/sql/sql_where.asp) nel sito W3Schools
- [Le definizioni di metodo](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) articolo sul sito W3C

>[!div class="step-by-step"]
[Precedente](displaying-data.md)
[Successivo](entering-data.md)
