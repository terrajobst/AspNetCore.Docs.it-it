---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Introduzione a pagine Web ASP.NET - nozioni di base di Form HTML | Microsoft Docs
author: tfitzmac
description: Questa esercitazione illustra le nozioni di base di come creare un formato di input e come gestire l'input dell'utente quando si usa ASP.NET Web Pages (Razor). E dopo aver...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: f5f7c9813041c443675f4e443822a81c1c508c20
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391991"
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>Introduzione a pagine Web ASP.NET - nozioni fondamentali sui moduli HTML
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra le nozioni di base di come creare un formato di input e come gestire l'input dell'utente quando si usa ASP.NET Web Pages (Razor). E ora che disponibile un database, si userà le tue competenze di form per consentire agli utenti di trovare il film specifici nel database. Si presuppone di aver completato la serie attraverso [Introduzione alla visualizzazione dati con ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Che cosa si apprenderà come:
> 
> - Come creare un form utilizzando gli elementi HTML standard.
> - Come leggere l'utente di input in un form.
> - Come creare una query SQL che in modo selettivo i dati tramite una ricerca termini che l'utente fornisce.
> - Come impostare i campi nella pagina "ricorda" ciò che l'utente ha immesso.
>   
> 
> Le funzionalità/tecnologie illustrate:
> 
> - Oggetto `Request`.
> - Il codice SQL `Where` clausola.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

Nell'esercitazione precedente, è stato creato un database, aggiunto dati a esso e quindi utilizzato il `WebGrid` helper per visualizzare i dati. In questa esercitazione si aggiungerà una casella di ricerca che consente di trovare film di genere specifico o il cui titolo contiene qualsiasi parola immesso. (Ad esempio, sarà in grado di trovare tutti i film appartenenti al genere è "Action" o il cui titolo contiene "Harry" o "Adventure.")

Al termine di questa esercitazione, si avrà una pagina simile alla seguente:

![Pagina di film con ricerca Genre e titolo](form-basics/_static/image1.png)

La parte di elenco della pagina è uguale a quello nell'ultima esercitazione &mdash; una griglia. La differenza sarà che la griglia visualizzerà solo i film che cercato.

## <a name="about-html-forms"></a>Informazioni sui moduli HTML

(Se hai esperienza con la creazione di form HTML e con la differenza tra `GET` e `POST`, è possibile ignorare questa sezione.)

Gli elementi di input utente è dotato di un modulo &mdash; caselle di testo, pulsanti, pulsanti di opzione, caselle di controllo, elenchi a discesa e così via. Gli utenti di compilare in questi controlli o effettuare selezioni e quindi inviare il modulo, fare clic su un pulsante.

In questo esempio è illustrata la sintassi HTML di base di un form:

[!code-html[Main](form-basics/samples/sample1.html)]

Quando questo markup viene eseguito in una pagina, viene creato un semplice form che è simile a questa illustrazione:

![Basic modulo HTML come visualizzabile nel browser](form-basics/_static/image2.png)

Il `<form>` elemento racchiude gli elementi HTML da inviare. (Un semplice errore apportare consiste nell'aggiungere elementi alla pagina quindi dimenticare di inserirli all'interno di un `<form>` elemento. In tal caso, non viene inviato.) Il `method` attributo indica al browser come inviare l'input dell'utente. È impostata su `post` se si sta eseguendo un aggiornamento nel server o a `get` se si sta semplicemente recupero dei dati dal server.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST e la sicurezza verbo HTTP**
> 
> HTTP, il protocollo utilizzato per lo scambio di informazioni, browser e server è straordinariamente semplice durante le operazioni di base. I browser usano soltanto pochi verbi per effettuare richieste al server. Quando si scrive codice per il web, è utile comprendere questi verbi e usarli come il browser e il server. Indubbiamente i verbi usati più comunemente sono:
> 
> - `GET`. Il browser Usa questo verbo per recuperare un elemento dal server. Ad esempio, quando si digita un URL nel browser, nel browser viene eseguito un `GET` operazione per richiedere la pagina desiderata. Se la pagina include elementi grafici, il browser esegue ulteriori `GET` operazioni per ottenere le immagini. Se il `GET` operazione deve passare le informazioni al server, le informazioni vengono passate come parte dell'URL nella stringa di query.
> - `POST`. Il browser invia un `POST` richiesta per poter inviare i dati per essere aggiunti o modificati nel server. Ad esempio, il `POST` verbo viene utilizzato per creare i record in un database o modificare quelli esistenti. La maggior parte dei casi, quando si compila un modulo e fare clic sul pulsante Invia, nel browser viene eseguito un `POST` operazione. In un `POST` operazione, i dati passati al server sono nel corpo della pagina.
> 
> Una differenza importante tra questi verbi è che un `GET` operazione non deve per modificare qualsiasi elemento presente nel server, o inserirlo in modo leggermente più astratto, un `GET` operazione non comporta una modifica nello stato nel server. È possibile eseguire un `GET` operazione nelle stesse risorse di quante volte si desidera, e non modificare tali risorse. (Un `GET` operazione viene spesso definita come "sicure", o per usare un termine tecnico, viene *idempotente*.) Al contrario, naturalmente, un `POST` richiesta viene modificato un elemento nel server ogni volta che si esegue l'operazione.
> 
> Due esempi aiuteranno a illustrare questa distinzione. Quando si esegue una ricerca usando un motore, ad esempio, Google o Bing riempire in un formato che è costituito da una casella di testo e quindi si sceglie il pulsante di ricerca. Nel browser viene eseguito un `GET` operazione con il valore immesso nella casella di passata come parte dell'URL. Usando un `GET` operazione per questo tipo di modulo non è corretto, perché un'operazione di ricerca non modifica le risorse nel server, recupera solo le informazioni.
> 
> Si consideri ora il processo di ordinamento di un elemento in linea. Compilare nei dettagli dell'ordine, quindi fare clic sul pulsante Invia. Questa operazione verrà ritentata un `POST` richiedere, perché l'operazione comporterà modifiche nel server, ad esempio un nuovo record di ordine, una modifica nelle informazioni dell'account e probabilmente molte altre modifiche. A differenza del `GET` operazione, non è possibile ripetere il `POST` richiesta, ovvero se è stato eseguito, ogni volta che inviato di nuovo la richiesta, si genera un nuovo ordine nel server. (In casi come questo, i siti Web spesso avviserà l'utente a non fare clic su un pulsante di invio più volte o disabiliterà il pulsante di invio in modo che non inviare nuovamente il modulo accidentalmente.)
> 
> Nel corso di questa esercitazione si userà entrambi una `GET` operazione e un `POST` operazione utilizzare i moduli HTML. Spiegheremo in ogni caso perché al verbo utilizzato è quello appropriato.
> 
> (Per altre informazioni sui verbi HTTP, vedere la [le definizioni di metodo](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) articolo sul sito di W3C.)


La maggior parte degli elementi di input utente sono in formato HTML `<input>` elementi. Sono simili `<input type="type" name="name">,` in cui *tipo* indica il tipo di controllo di input utente desiderato. Questi elementi sono gli eventi più comuni:

- Casella di testo: `<input type="text">`
- Casella di controllo: `<input type="check">`
- Pulsante di opzione: `<input type="radio">`
- Pulsante: `<input type="button">`
- Pulsante di invio: `<input type="submit">`

È anche possibile usare la `<textarea>` elemento per creare una casella di testo su più righe e di `<select>` elemento per creare un elenco a discesa o un elenco scorrevole. (Per ulteriori informazioni su HTML costituiscono gli elementi, vedere [form HTML e Input](http://www.w3schools.com/html/html_forms.asp) sul sito W3Schools.)

Il `name` attributo è molto importante, perché il nome è come si otterrà il valore dell'elemento in un secondo momento, come si vedrà a breve.

La parte interessante è, lo sviluppatore della pagina, operazioni con l'input dell'utente. Non è disponibile alcun comportamento predefinito associato a questi elementi. In alternativa, è necessario ottenere i valori che l'utente ha immesso o selezionato ed eseguire un'operazione con essi. Questo è ciò che verrà illustrato in questa esercitazione.

> [!TIP] 
> 
> **HTML5 e moduli di Input**
> 
> Come saprete, HTML è in fase di transizione e la versione più recente (HTML5) include il supporto per la modalità più intuitive agli utenti di immettere le informazioni. Ad esempio, in formato HTML5 è (sviluppatore della pagina) può indicare la pagina che si desidera che l'utente di immettere una data. Il browser può quindi visualizzare automaticamente un calendario, anziché richiedere all'utente di immettere manualmente una data. Tuttavia, HTML5 è nuovo e non è supportato in tutti i browser ancora.
> 
> ASP.NET Web Pages supporta HTML5 nella misura in cui il browser dell'utente non di input. Per avere un'idea dei nuovi attributi per il `<input>` elemento di HTML5, vedere [HTML &lt;input&gt; attributo type](http://www.w3schools.com/html/html_form_input_types.asp) sul sito W3Schools.


## <a name="creating-the-form"></a>Creazione del form

In WebMatrix nel **file** dell'area di lavoro, aprire il *Movies.cshtml* pagina.

Dopo il tag di chiusura `</h1>` tag e prima dell'apertura `<div>` tag del `grid.GetHtml` chiamare, aggiungere il markup seguente:

[!code-html[Main](form-basics/samples/sample2.html)]

Questo codice crea un form contenente una casella di testo denominata `searchGenre` e un pulsante di invio. Il testo casella e inviare pulsante sono racchiusi in un `<form>` elemento la cui `method` attributo è impostato su `get`. (Tenere presente che se non inserire la casella di testo e inviare pulsante all'interno di un `<form>` elemento, nulla verrà inviata quando si fa clic sul pulsante.) Si utilizza il `GET` verbo qui perché si sta creando un modulo che non apportare modifiche nel server, semplicemente eseguita una ricerca. (Nell'esercitazione precedente, utilizzato un `post` metodo, ovvero come si invia le modifiche apportate al server. Si noterà che nella prossima esercitazione nuovamente.)

Eseguire la pagina. Anche se è ancora stato definito alcun comportamento per il form, è possibile visualizzare l'aspetto:

![Pagina di film con casella di ricerca per genere](form-basics/_static/image3.png)

Immettere un valore nella casella di testo, ad esempio "Commedie". Quindi fare clic su **Genre ricerca**.

Prendere nota dell'URL della pagina. Quanto è stato impostato il `<form>` dell'elemento `method` dell'attributo `get`, il valore immesso fa ora parte della stringa di query nell'URL, simile al seguente:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Lettura di valori di Form

La pagina contiene già un codice che recupera i dati del database e visualizza i risultati in una griglia. A questo punto è necessario aggiungere il codice che legge il valore della casella di testo in modo che è possibile eseguire una query SQL che include il termine di ricerca.

Poiché il metodo del form è impostata su `get`, è possibile leggere il valore immesso nella casella di testo usando codice simile al seguente:

`var searchTerm = Request.QueryString["searchGenre"];`

Il `Request.QueryString` oggetto (il `QueryString` proprietà del `Request` oggetto) include i valori degli elementi che sono stati inviati come parte di `GET` operazione. Il `Request.QueryString` proprietà contiene un *raccolta* (elenco) dei valori che vengono inviati nei form. Per ottenere ogni singolo valore, specificare il nome dell'elemento desiderato. Ecco perché è necessario avere una `name` attributo la `<input>` elemento (`searchTerm`) che consente di creare la casella di testo. (Per altre informazioni sul `Request` oggetti, vedere la [sidebar](#BKMK_TheRequestObject) in un secondo momento.)

È abbastanza semplice leggere il valore della casella di testo. Tuttavia, se l'utente non immette alcuna stringa affatto nella casella di testo, ma ha fatto clic **ricerca** comunque, è possibile ignorare tale clic, poiché non c'è niente da cercare.

Il codice seguente è un esempio che illustra come implementare queste condizioni. (Non è necessario aggiungere questo codice ancora; è necessario usare più avanti).

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Il test si interrompe in questo modo:

- Ottenere il valore della `Request.QueryString["searchGenre"]`, vale a dire il valore che è stato inserito il `<input>` elemento denominato `searchGenre`.
- Scoprire se è vuoto usando la `IsEmpty` (metodo). Questo metodo è il modo standard per determinare se un elemento (ad esempio, un elemento di modulo) contiene un valore. Tuttavia, importante sapere solo in caso affermativo *non* vuota, quindi...
- Aggiungere il `!` operatore davanti il `IsEmpty` di test. (Il `!` operatore significa NOT logico).

In parole semplici, l'intera `if` condizione si traduce in quanto segue: *se elemento searchGenre del form non è vuoto, quindi...*

Questo blocco Prepara il terreno per la creazione di una query che usa il termine di ricerca. È necessario usare nella sezione successiva.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **L'oggetto richiesta**
> 
> Il `Request` oggetto contiene tutte le informazioni inviate dal browser all'applicazione quando viene richiesto o inviata una pagina. Questo oggetto include tutte le informazioni fornite dall'utente, ad esempio i valori di casella di testo o un file da caricare. Contiene inoltre moltissime informazioni aggiuntive, quali i cookie, i valori nella stringa query dell'URL (se presente), il percorso del file della pagina a cui è in esecuzione, il tipo di browser in cui l'utente sta usando, l'elenco delle lingue che vengono impostate nel browser e altro ancora.
> 
> Il `Request` oggetto è una *raccolta* (elenco) di valori. Ottenere un singolo valore della raccolta specificandone il nome:
> 
> `var someValue = Request["name"];`
> 
> Il `Request` oggetto espone diversi subset. Ad esempio:
> 
> - `Request.Form` offre valori dagli elementi all'interno di inviata `<form>` elemento se la richiesta non è un `POST` richiesta.
> - `Request.QueryString` consente solo i valori presenti nella stringa di query dell'URL. (In un URL come `http://mysite/myapp/page?searchGenre=action&page=2`, il `?searchGenre=action&page=2` sezione dell'URL è la stringa di query.)
> - `Request.Cookies` raccolta consente di accedere ai cookie che è stato inviato dal browser.
> 
> Per ottenere un valore che già conosci è nel formato di invio, è possibile usare `Request["name"]`. In alternativa, è possibile usare le versioni più specifiche `Request.Form["name"]` (per `POST` richieste) o `Request.QueryString["name"]` (per `GET` richieste). Ovviamente *nome* è il nome dell'elemento da ottenere.
> 
> Il nome dell'elemento di cui che si desidera ottenere deve essere univoco all'interno della raccolta in uso. Ecco perché il `Request` oggetto include come subset `Request.Form` e `Request.QueryString`. Si supponga che la pagina contiene un elemento del form denominato `userName` e *inoltre* contiene un cookie denominato `userName`. Se si verificano `Request["userName"]`, è ambiguo se si desidera il valore di form o il cookie. Tuttavia, se si verificano `Request.Form["userName"]` o `Request.Cookie["userName"]`, sta siano espliciti su quale valore da ottenere.
> 
> È consigliabile essere specifici e usare il subset di `Request` che si è interessati, ad esempio `Request.Form` o `Request.QueryString`. Per le pagine semplici che si sta creando questa esercitazione, probabilmente non molto effettua alcuna differenza. Tuttavia, quando si creano pagine più complesse, utilizzando la versione esplicita `Request.Form` o `Request.QueryString` può aiutare a evitare i problemi che possono verificarsi quando la pagina contiene un modulo (o più moduli), i cookie, i valori di stringa di query e così via.


## <a name="creating-a-query-by-using-a-search-term"></a>Creazione di una Query con un termine di ricerca

Ora che sai come ottenere il termine di ricerca immesso dall'utente, è possibile creare una query che lo usa. Tenere presente che per ottenere tutti gli elementi di film all'esterno del database, si usa una query SQL che è simile a questa istruzione:

`SELECT * FROM Movies`

Per ottenere solo determinati film, è necessario usare una query che include un `Where` clausola. Questa clausola consente di impostare una condizione in cui vengono restituite righe dalla query. Di seguito è riportato un esempio:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Il formato di base è `WHERE column = value`. È possibile usare diversi operatori oltre just `=`, ad esempio `>` (maggiore di), `<` (minore di), `<>` (non uguale a), `<=` (minore o uguale a), e così via, a seconda di ciò che sta cercando.

Nel caso ve lo chiediate, le istruzioni SQL non sono tra maiuscole e minuscole &mdash; `SELECT` equivale a `Select` (o addirittura `select`). Tuttavia, le persone spesso converte in maiuscolo parole chiave in un'istruzione SQL, ad esempio `SELECT` e `WHERE`per renderlo più facile da leggere.

### <a name="passing-the-search-term-as-a-parameter"></a>Passando il termine di ricerca come parametro

La ricerca per genere specifico è abbastanza semplice (`WHERE Genre = 'Action'`), ma si desidera essere in grado di eseguire la ricerca di qualsiasi genere immessi dall'utente. A tale scopo, si crea come query SQL che include un segnaposto per il valore da cercare. Sarà simile a questo comando:

`SELECT * FROM Movies WHERE Genre = @0`

Il segnaposto è il `@` caratteri seguiti da zero. Come può immaginare, una query può contenere più segnaposti e verranno denominati `@0`, `@1`, `@2`e così via.

Per impostare l'esecuzione della query e passarlo effettivamente il valore, si usa codice simile al seguente:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Questo codice è simile a ciò che è già stato per visualizzare i dati nella griglia. Le uniche differenze sono:

- La query contiene un segnaposto (`WHERE Genre = @0"`).
- La query viene inserita in una variabile (`selectCommand`); prima, la query è stato passato direttamente al `db.Query` (metodo).
- Quando si chiama il `db.Query` metodo, si passa la query sia il valore da usare per il segnaposto. (Se la query conteneva più segnaposto, è necessario passarli come separare i valori al metodo.)

Se è mettere in pratica tutti questi elementi, viene visualizzato il codice seguente:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Importante**: Utilizzo di segnaposti (come `@0`) per passare valori a un comando SQL viene *estremamente importante* per la sicurezza. Il modo in cui è visualizzato qui, con segnaposto per i dati delle variabili, è l'unico modo, è necessario costruire i comandi SQL.
> 
> Mai comporre un'istruzione SQL tramite la realizzazione di testo letterale (concatenando) e i valori che ottenere dall'utente. Concatenazione di input dell'utente in un'istruzione SQL viene aperto il sito a un *attacco SQL injection* in cui un utente malintenzionato invia i valori a una pagina che hack del database. (Altre informazioni nell'articolo [attacchi SQL Injection](https://msdn.microsoft.com/library/ms161953.aspx) il sito Web MSDN.)


## <a name="updating-the-movies-page-with-search-code"></a>Aggiornare la pagina di film con codice di ricerca

A questo punto è possibile aggiornare il codice nel *Movies.cshtml* file. Per iniziare, sostituire il codice nel blocco di codice nella parte superiore della pagina con il seguente codice:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

La differenza è che è stata inserita la query nel `selectCommand` variabile, che verranno passate alla `db.Query` in un secondo momento. Inserire l'istruzione SQL in una variabile consente di modificare l'istruzione, che è ciò che si esegue per eseguire la ricerca.

È stato anche rimosso queste due righe, verranno riportato indietro in un secondo momento:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Non si desidera eseguire la query ancora (vale a dire, chiamare `db.Query`) e non si vuole inizializzare il `WebGrid` helper ancora uno. Queste operazioni verranno eseguite dopo che è acquisita familiarità con l'istruzione SQL deve essere eseguito.

Dopo questo blocco riscritto, è possibile aggiungere la nuova logica per la gestione di ricerca. Il codice completo avrà un aspetto simile al seguente. Aggiornare il codice della pagina in modo che corrisponda a questo esempio:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

La pagina ora funziona come segue. Ogni volta che viene eseguita la pagina, il codice verrà aperto il database e il `selectCommand` variabile è impostata per l'istruzione SQL che ottiene tutti i record di `Movies` tabella. Il codice inizializza inoltre la `searchTerm` variabile.

Tuttavia, se la richiesta corrente include un valore per il `searchGenre` elemento, il codice imposta `selectCommand` a un'altra query, vale a dire, con uno che include il `Where` clausola per cercare un genere. Imposta inoltre `searchTerm` per qualunque altro elemento passato per la casella di ricerca (che può essere nothing).

Indipendentemente dal fatto che SQL istruzione si trova in `selectCommand`, il codice quindi chiama `db.Query` per eseguire la query, passandogli qualsiasi è in `searchTerm`. Se non c'è nulla `searchTerm`, non è importante, perché in tal caso non è presente nessun parametro da passare il valore da `selectCommand` comunque.

Infine, il codice inizializza il `WebGrid` helper con i risultati della query, come in precedenza.

È possibile notare che, inserendo l'istruzione SQL e al termine di ricerca in variabili, aver aggiunto flessibilità al codice. Come si vedrà più avanti in questa esercitazione, è possibile usare questo framework di base e continuare ad aggiungere logica per diversi tipi di ricerche.

## <a name="testing-the-search-by-genre-feature"></a>La funzionalità di ricerca-da-Genre di testing

In WebMatrix, eseguire la *Movies.cshtml* pagina. Viene visualizzata la pagina con la casella di testo per il genere.

Immettere un genere che è immesso un valore o i record di test, quindi fare clic su **ricerca**. Questa volta è visualizzato un elenco di semplicemente i film che corrispondono a tale genere:

![Pagina Movies listato dopo la ricerca di Comedies' genre'](form-basics/_static/image4.png)

Immettere un genere diverso e ripetere la ricerca. Provare a immettere il genere con tutte le lettere minuscole o tutte maiuscole, in modo che è possibile vedere che la ricerca non fa distinzione tra maiuscole e minuscole.

## <a name="remembering-what-the-user-entered"></a>"Memorizzazione" ciò che l'utente ha immesso

Si è notato che dopo aver immesso un genere e fatto clic **Genre ricerca**, si è visto una voce per tale genere. Tuttavia, la casella di testo di ricerca è vuota &mdash; in altre parole, non è stata ricordare cosa sia stato immesso.

È importante comprendere il motivo per cui questo comportamento si verifica. Quando si invia una pagina, il browser invia una richiesta al server web. Quando ASP.NET riceve la richiesta, crea un'istanza nuova della pagina, viene eseguito il codice in esso e quindi esegue il rendering di pagina nel browser. In effetti, tuttavia, la pagina non sa che si stava semplicemente con una versione precedente di se stesso. Tutto che risulti evidente è che non ha ricevuto una richiesta contenente alcuni dati del modulo in esso.

Ogni volta che si richiede una pagina &mdash; se per la prima volta oppure inviandolo &mdash; si ricevono una nuova pagina. Il server web non ha memoria dell'ultima richiesta. Neanche ASP.NET e neanche il browser. La connessione sola tra queste istanze separate della pagina è tutti i dati che trasmettono tra di essi. Se si invia una pagina, ad esempio, la nuova istanza di pagina possa ottenere i dati del modulo che è stati inviati dall'istanza precedente. (Un altro modo per passare dati tra le pagine prevede l'uso dei cookie).

Un modo formale per descrivere questa situazione è dire che le pagine web siano *senza stato*. Server Web e le pagine stesse e gli elementi nella pagina non mantengono le informazioni sullo stato precedente di una pagina. Il web è stato progettato in questo modo perché Gestione dello stato per le singole richieste sarebbe rapidamente esaurire le risorse del server web, che spesso gestire forse migliaia, persino centinaia di migliaia, di richieste al secondo.

Quindi la casella di testo è vuota. Dopo che è stata inviata la pagina, ASP.NET creata una nuova istanza della pagina ed eseguito tramite il codice e markup. Si è verificato nulla che il codice che ASP.NET per inserire un valore nella casella di testo indicato. Pertanto, ASP.NET non funziona e la casella di testo è stato eseguito il rendering senza un valore in esso.

È effettivamente un modo semplice per risolvere questo problema. Il genere immesso nella casella di testo *viene* disponibili in code &mdash; si trova in `Request.QueryString["searchGenre"]`.

Aggiornare il markup per la casella di testo in modo che il `value` attributo Ottiene il valore dalla `searchTerm`, come in questo esempio:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

In questa pagina, è possibile avere impostare anche il `value` dell'attributo di `searchTerm` variabile, poiché tale variabile contiene inoltre il genere immesso. Ma tramite il `Request` su cui impostare il `value` attributo come illustrato di seguito è il modo standard per questa attività. (Supponendo che si intende addirittura a tale scopo &mdash; in alcuni casi, si potrebbe voler eseguire il rendering della pagina *senza* valori nei campi. Tutto dipende cosa sta succedendo con l'app.)

> [!NOTE]
> Non si "ricorda" il valore di una casella di testo che viene usato per le password. Sarebbe un problema di sicurezza per consentire agli utenti di compilare un campo della password tramite il codice.


Eseguire nuovamente la pagina, immettere un genere e fare clic su **Genre ricerca**. Questa volta non solo vengono visualizzati i risultati della ricerca, ma la casella di testo memorizza l'ora dell'ultimo immesso:

![Pagina che mostra che la casella di testo ha 'memorizzate' la voce precedente](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Cercare qualsiasi parola nel titolo

A questo punto è possibile cercare qualsiasi genere, ma è anche possibile cercare un titolo. È difficile ottenere esattamente a destra un titolo durante la ricerca, ma che è possibile cercare una parola che compare in un punto qualsiasi all'interno di un titolo. A tale scopo, in SQL, si utilizza il `LIKE` operatore e la sintassi analogo al seguente:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Questo comando Ottiene tutti i film cui titoli contengono "adventure". Quando si usa la `LIKE` operatore, si includa il carattere jolly `%` come parte del termine di ricerca. La ricerca `LIKE 'adventure%'` significa "inizia con 'adventure'". (Tecnicamente, significa che "La stringa 'adventure' seguito dal nulla.") Analogamente, il termine di ricerca `LIKE '%adventure'` significa "tutto quello seguito dalla stringa 'adventure'", ovvero un altro modo per dire "finendo con 'adventure'".

Al termine di ricerca `LIKE '%adventure%'` pertanto significa "con"adventure' in un punto qualsiasi nel titolo." (Tecnicamente, "qualsiasi cosa nel titolo, seguito da 'adventure', seguita da qualsiasi elemento".)

All'interno di `<form>` elemento, aggiungere il markup seguente sotto la chiusura `</div>` tag per la ricerca di genere (subito prima del tag `</form>` elemento):

[!code-html[Main](form-basics/samples/sample10.html)]

Il codice per gestire questa ricerca è simile al codice per la ricerca genre, ad eccezione del fatto che sia necessario assemblare la `LIKE` ricerca. All'interno del blocco di codice nella parte superiore della pagina, aggiungere questo `if` blocca subito dopo il `if` blocco per la ricerca di genere:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Questo codice Usa la stessa logica illustrato in precedenza, ad eccezione del fatto che la ricerca Usa un' `LIKE` operatore e inserisce il codice "`%`" prima e dopo il termine di ricerca.

Si noti come era facile aggiungere un'altra ricerca per pagina. È stato sufficiente era:

- Creare un `if` blocco in cui è stato testato per verificare se la casella di ricerca rilevanti conteneva un valore.
- Impostare il `selectCommand` variabile a una nuova istruzione SQL.
- Impostare il `searchTerm` variabili al valore da passare alla query.

Ecco il blocco di codice completo, che contiene la nuova logica per una ricerca di titoli:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Ecco un riepilogo delle operazioni eseguite da questo codice:

- Le variabili `searchTerm` e `selectCommand` vengono inizializzate nella parte superiore. Si intende impostare queste variabili per il termine di ricerca appropriato (se presente) e comando SQL appropriate basate su operazioni eseguite dall'utente nella pagina. La ricerca predefinita è il caso semplice di ottenere tutti i film dal database.
- I test per `searchGenre` e `searchTitle`, il codice imposta `searchTerm` sul valore desiderato per la ricerca. Questi blocchi di codice anche impostare `selectCommand` a un comando SQL appropriato per la ricerca.
- Il `db.Query` metodo viene richiamato una sola volta, usando qualsiasi comando SQL è nel `selectedCommand` e qualsiasi valore è espresso in `searchTerm`. Se non è presente alcun termine di ricerca (nessun genre e nessuna parola titolo), il valore di `searchTerm` è una stringa vuota. Tuttavia, che non è rilevante, perché in tal caso la query non richiede un parametro.

## <a name="testing-the-title-search-feature"></a>La funzionalità di ricerca del titolo di test

È ora possibile testare la pagina di ricerca completata. Eseguire *Movies.cshtml*.

Immettere un genere e fare clic su **Genre ricerca**. Nella griglia vengono visualizzate film di tale genere, come prima.

Immettere una parola di titolo e fare clic su **ricerca titolo**. Nella griglia vengono visualizzate film che contengono tale parola nel titolo.

![Pagina dopo la ricerca di 'La' nel titolo del film](form-basics/_static/image6.png)

Lasciare vuoto entrambe le caselle di testo e fare clic su dei pulsanti. La griglia visualizza tutti i film.

## <a name="combining-the-queries"></a>Combinare le query

È possibile notare che le ricerche è possibile eseguire sono esclusive. È possibile cercare il titolo e il genere nello stesso momento, anche se entrambe le caselle di ricerca dispone di valori in essi contenuti. Ad esempio, è possibile cercare tutti i film di azione il cui titolo contiene "Adventure". (Come la pagina viene codificata a questo punto, se si immettono valori per il genere e titolo, Microsoft la ricerca di titoli Ottiene la precedenza.) Per creare una ricerca che combina le condizioni, è necessario creare una query SQL che ha una sintassi simile alla seguente:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

E sarebbe necessario eseguire la query utilizzando un'istruzione simile al seguente (all'incirca a proposito):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Creazione per la logica per consentire diverse permutazioni di criteri di ricerca può ottenere un po' complesso, come si può notare. Pertanto, occorrerà fermarsi qui.

## <a name="coming-up-next"></a>In arrivo

Nella prossima esercitazione, si creerà una pagina che utilizza una forma per consentire agli utenti di aggiungere film nel database.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Elenco completo per la pagina di film (aggiornata con la ricerca)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET usando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Clausola WHERE SQL](http://www.w3schools.com/sql/sql_where.asp) sul sito W3Schools
- [Le definizioni di metodo](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) articolo sul sito W3C

> [!div class="step-by-step"]
> [Precedente](displaying-data.md)
> [Successivo](entering-data.md)
