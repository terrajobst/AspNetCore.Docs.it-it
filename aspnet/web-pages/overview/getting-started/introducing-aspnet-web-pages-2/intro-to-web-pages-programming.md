---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: 'Introduzione a ASP.NET Web Pages: programmazione di concetti di base | Documenti Microsoft'
author: tfitzmac
description: 'In questa esercitazione fornisce una panoramica di come programma in ASP.NET Web Pages con sintassi Razor. Si apprenderà: la sintassi "Razor" di base utilizzato per pr...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/17/2015
ms.topic: article
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 60115dd06a27bf856427953de29e993194afb991
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/07/2018
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>Introduzione a ASP.NET Web Pages - nozioni di base sulla programmazione
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questa esercitazione fornisce una panoramica di come programma in ASP.NET Web Pages con sintassi Razor.
> 
> Illustra quanto segue:
> 
> - La sintassi "Razor" base utilizzate per la programmazione in ASP.NET Web Pages.
> - Alcuni c# di base, ovvero il linguaggio di programmazione da usare.
> - Alcuni concetti di programmazione fondamentali per le pagine Web.
> - Come installare i pacchetti (componenti che contengono codice predefiniti) da utilizzare con il sito.
> - Come usare *helper* per eseguire attività comuni di programmazione.
>   
> 
> Funzionalità/tecnologie descritte:
> 
> - NuGet e la gestione dei pacchetti.
> - Il `Gravatar` helper.


In questa esercitazione è principalmente un esercizio per una presentazione introduttiva è la sintassi di programmazione che si userà per ASP.NET Web Pages. Si apprenderà sulle *sintassi Razor* e linguaggio di programmazione codice scritto in c#. Una panoramica di questa sintassi ottenuti nell'esercitazione precedente; in questa esercitazione verrà illustrato la sintassi più.

Ti assicuriamo che questa esercitazione prevede più di programmazione che verrà visualizzato in un'esercitazione singola e che tratta dell'esercitazione solo è *solo* sulla programmazione. Nelle esercitazioni rimanenti in questo set, si creeranno effettivamente pagine in cui eseguire operazioni interessanti.

Apprenderà anche *helper*. Un helper è un componente, ovvero una parte di backup inclusi nel pacchetto di codice, che è possibile aggiungere a una pagina. L'helper esegue operazioni che in caso contrario potrebbe essere difficile o complessa da svolgere manualmente.

## <a name="creating-a-page-to-play-with-razor"></a>Creazione di una pagina per la riproduzione con Razor

In questa sezione si proveranno un bit con Razor in modo è possibile farsi un'idea della sintassi di base.

Se non è già in esecuzione, avviare WebMatrix. Si utilizzerà il sito Web è stato creato nell'esercitazione precedente ([recupero avviato con le pagine Web](https://go.microsoft.com/fwlink/?LinkId=251578)). Per riaprirla, fare clic su **siti personali** e scegliere **WebPageMovies**:

![Schermata iniziale di WebMatrix che illustra le opzioni Apri sito e siti personali evidenziato](intro-to-web-pages-programming/_static/image1.png)

Selezionare il **file** dell'area di lavoro.

Nella barra multifunzione, fare clic su **New** per creare una pagina. Selezionare **CSHTML** e denominare la nuova pagina *TestRazor.cshtml*.

Fare clic su **OK**.

Copiare quanto segue nel file di sostituzione completa cosa esiste già.

> [!NOTE]
> Quando si copia markup o codice degli esempi in una pagina, il rientro e allineamento potrebbero non essere uguale a quello dell'esercitazione. Rientro e allineamento non influiscono sulla modalità di esecuzione del codice, tuttavia.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Esaminare la pagina di esempio

La maggior parte di ciò che viene visualizzato è HTML normale. Tuttavia, nella parte superiore è il blocco di codice:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Si noti quanto segue su questo blocco di codice:

- Il carattere @ indica ad ASP.NET che quello che segue è codice Razor, non HTML. ASP.NET considererà tutti gli elementi dopo il @ carattere come codice finché non viene eseguito nuovamente in HTML. (In questo caso, che il &lt;! DOCTYPE&gt; elemento.
- Le parentesi graffe ({e}), racchiudere un blocco di codice Razor se il codice ha più di una riga. Le parentesi graffe indicano ASP.NET in cui il codice per tale blocco inizia e termina.
- Il / / caratteri contrassegnare un commento, vale a dire, una parte del codice non verrà eseguite.
- Ogni istruzione deve terminare con un punto e virgola (;). (Non i commenti, tuttavia.)
- È possibile archiviare valori in *variabili*, che creano (*dichiarare*) con la funzione VAR (parola chiave). Quando si crea una variabile, assegnargli un nome, che può includere lettere, numeri e caratteri di sottolineatura (\_). I nomi delle variabili non può iniziare con un numero e non è possibile utilizzare il nome di una parola chiave di programmazione (ad esempio var).
- Stringhe di caratteri (ad esempio, "ASP.NET" e "Pagine") racchiudere tra virgolette. (Devono essere le virgolette doppie). I numeri non sono inclusi tra virgolette.
- Gli spazi vuoti all'esterno di virgolette non ha importanza. Non importante principalmente le interruzioni di riga. l'eccezione è che non è possibile dividere una stringa tra virgolette in più righe. Rientro e allineamento non è rilevante.

Un elemento che non sia evidente in questo esempio è che tutto il codice tra maiuscole e minuscole. Ciò significa che il somma di variabile è una variabile diversa rispetto alle variabili che potrebbe essere denominata la somma o la somma. Analogamente, var è una parola chiave, ma non è Var.

### <a name="objects-and-properties-and-methods"></a>Gli oggetti, proprietà e metodi

Non vi è l'espressione DateTime. Now. In altre parole, DateTime è un *oggetto*. Un oggetto è un elemento che è possibile programmare con, ovvero una pagina di una casella di testo, un file, un'immagine, una richiesta web, un messaggio di posta elettronica, un record del cliente, e così via. Gli oggetti dispongono di uno o più *proprietà* che descrivono le caratteristiche. Oggetto di una casella di testo ha una proprietà di testo (tra gli altri), un oggetto di richiesta ha una proprietà Url (e altri), un messaggio di posta elettronica è una proprietà From e una proprietà a, e così via. Gli oggetti hanno anche *metodi* che sono "verbi" possono eseguire. Verrà utilizzato con gli oggetti molto.

Come si può notare nell'esempio, DateTime è un oggetto che consente di programma date e ore. Include una proprietà denominata ora che restituisce la data e ora correnti.

### <a name="using-code-to-render-markup-in-the-page"></a>Utilizzo di codice per il rendering del markup nella pagina

Nel corpo della pagina, tenere presente quanto segue:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Nuovamente, il carattere @ indica ad ASP.NET che quello che segue è il codice, non HTML. Nel codice è possibile aggiungere seguita da un'espressione di codice e ASP.NET restituirà il valore di tale diritto espressione a quel punto. Nell'esempio @a verrà eseguito il rendering l'elemento è il valore della variabile denominata, @product esegue il rendering di qualunque sia il prodotto denominato variabile e così via.

Non si è limitato alle variabili, tuttavia. In alcuni casi, il carattere @ precede un'espressione:

- @(un\*b) esegue il rendering del prodotto delle informazioni presenti nelle variabili di un a e b. (Il \* operatore: moltiplicazione.)
- @(tecnologia + "" + prodotto) Visualizza i valori in una tecnologia di variabili e il prodotto dopo concatenarle e aggiunta di uno spazio tra. L'operatore (+) per la concatenazione di stringhe è lo stesso come operatore per l'aggiunta di numeri. ASP.NET in genere è possibile indicare se si sta lavorando con numeri o stringhe e consente di eseguire l'operazione di destra con l'operatore +.
- @Request.Url esegue il rendering la proprietà Url dell'oggetto della richiesta. L'oggetto richiesta contiene informazioni sulla richiesta corrente dal browser e naturalmente la proprietà Url contiene l'URL della richiesta corrente.

L'esempio è inoltre progettato per mostrare che è possibile eseguire il lavoro in modi diversi. È possibile eseguire calcoli nel blocco di codice nella parte superiore, inserire i risultati in una variabile e quindi eseguire il rendering la variabile nel markup. Oppure è possibile eseguire calcoli in un diritto di espressione nel markup. L'approccio adottato dipende dalle operazioni eseguite e, in alcuni casi, delle proprie preferenze.

### <a name="seeing-the-code-in-action"></a>Visualizzare i codici di azione

Il nome del file e quindi **Avvia nel browser**. Viene visualizzata la pagina nel browser con tutti i valori e le espressioni risolte nella pagina.

![Pagina 'TestRazor' in esecuzione nel browser](intro-to-web-pages-programming/_static/image2.png)

Esaminare l'origine nel browser.

!['Test Razor' origine della pagina nel browser](intro-to-web-pages-programming/_static/image3.png)

Come si prevede che l'esperienza nell'esercitazione precedente, nessuna parte del codice Razor è nella pagina. Siano che visibili solo i valori di visualizzazione effettivo. Quando si esegue una pagina, si apportano effettivamente una richiesta al server web che viene compilato in WebMatrix. Quando viene ricevuta la richiesta, ASP.NET risolve tutti i valori e le espressioni e viene eseguito il rendering di valori nella pagina. Invia quindi la pagina nel browser.

> [!TIP] 
> 
> **Razor e c#**
> 
> Fino a ora abbiamo abbiamo detto che si lavora con sintassi Razor. Che è true, ma non la storia completa. Il linguaggio di programmazione effettivo in uso viene chiamato *c#*. In c# è stato creato da Microsoft oltre un decennio fa ed è diventato uno dei linguaggi di programmazione primari per la creazione di App di Windows. Tutte le regole di cui che si è visto su come assegnare un nome di una variabile e come creare le istruzioni e così via sono effettivamente tutte le regole del linguaggio c#.
> 
> Razor fa riferimento in modo più specifico per il piccolo set di convenzioni per come incorporare il codice in una pagina. Ad esempio, la convenzione di usando per contrassegnare il codice della pagina e @ {} per incorporare un blocco di codice è l'aspetto Razor di una pagina. Gli helper sono considerati come parte del Razor. Sintassi Razor è utilizzata in altre posizioni di solo in ASP.NET Web Pages. (Ad esempio, utilizzato nelle visualizzazioni di ASP.NET MVC nonché.)
> 
> È importante tenere presente questo perché se si cerca di informazioni sulla programmazione ASP.NET Web Pages, è disponibile un numero elevato di riferimenti a Razor. Tuttavia, molte di tali riferimenti non si applicano a quanto è associata a tale operazione e pertanto potrebbe risultare poco chiaro. E, in realtà, molte delle domande programmazione effettivamente devono essere relative a lavorare con c# o l'utilizzo con ASP.NET. Pertanto, se si esamina in modo specifico per informazioni su Razor, potrebbe non trovare le risposte che necessarie.


## <a name="adding-some-conditional-logic"></a>Aggiunta di logica condizionale

Una delle eccezionali funzionalità sull'utilizzo di codice in una pagina è che è possibile modificare cosa accade in base a diverse condizioni. In questa parte dell'esercitazione, sarà sperimentare alcuni modi per modificare la visualizzazione della pagina.

Nell'esempio sarà semplice e un certo studiato appositamente in modo che è possibile concentrarsi sulla logica condizionale. La pagina che si creeranno eseguirà questo:

- Visualizzare il testo diverso nella pagina, a seconda se è la prima volta che viene visualizzata la pagina o se si è fatto clic di un pulsante per l'invio della pagina. Che sarà il primo test condizionale.
- Il messaggio viene visualizzato solo se un determinato valore viene passato nella stringa di query dell'URL (http://...?show=true). Che sarà il secondo test condizionale.

In WebMatrix, creare una pagina e il nome *TestRazorPart2.cshtml*. (Nella barra multifunzione, fare clic su **New**, scegliere **CSHTML**, denominare il file e quindi fare clic su **OK**.)

Sostituire il contenuto di tale pagina con le operazioni seguenti:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Il blocco di codice nella parte superiore consente di inizializzare una variabile denominata messaggio con del testo. Nel corpo della pagina, il contenuto della variabile di messaggio viene visualizzato all'interno di un &lt;p&gt; elemento. Il codice contiene inoltre un &lt;input&gt; elemento per creare un **Invia** pulsante.

Eseguire la pagina per vedere come funziona ora. Per il momento, si tratta in sostanza una pagina statica, anche se si sceglie il **Invia** pulsante.

Tornare a WebMatrix. All'interno del blocco di codice, aggiungere il seguente codice evidenziato *dopo* la riga che inizializza messaggio:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Se il blocco di {}

Appena aggiunto è stato un se condizione. Nel codice, il se condizione dispone di una struttura simile al seguente:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

La condizione da testare è racchiuso tra parentesi. Deve essere un valore o un'espressione che restituisce true o false. Se la condizione è true, ASP.NET esegue le istruzioni che si trovano all'interno delle parentesi graffe. (Sono il *quindi* in parte il *if-then* logica.) Se la condizione è false, il blocco di codice viene ignorato.

Ecco alcuni esempi di condizioni, è possibile verificare in un'istruzione:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

È possibile testare le variabili in base a valori o espressioni utilizzando un <em>operatore logico</em> o <em>operatore di confronto</em>: uguale a (= =), maggiore di (&gt;), minore di (&lt;), maggiore o uguale a (&gt;=) e minore o uguale a (&lt;=). Il! = operatore significa non è uguale a, ad esempio, se (un! = 0) significa <em>se</em> <em>un</em><em>non è uguale a 0</em>.

> [!NOTE]
> Verificare che si nota che l'operatore di confronto uguale a (= =) non è identico a quello =. I = operatore viene usato solo per assegnare valori (var un = 2). Se si combinano questi operatori di, si ottiene un errore o si otterrà alcuni risultati imprevisti.


Per verificare se un elemento è true, la sintassi completa è if(IsDone == true). Ma è anche possibile usare il if(IsDone) di scelta rapida. Se è presente nessun operatore di confronto, ASP.NET si presuppone che si sta testando per true.

Il carattere! operatore di per sé significa NOT logico. Ad esempio, la condizione if (! Istruzione IsPost) significa *se l'istruzione IsPost non è vero*.

È possibile combinare condizioni con un operatore logico AND (&amp; &amp; operator) o OR logico (| | (operatore)). Ad esempio, l'ultima del se le condizioni nel precedente si esempi *se FileProcessingIsDone non è impostata su true e displayMessage è impostata su false*.

### <a name="the-else-block"></a>Il blocco else

L'ultimo elemento sul se blocchi: un se il blocco può essere seguito da un blocco else. Un blocco else è utile è necessario eseguire codice diverso quando la condizione è false. Di seguito è riportato un esempio:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Nelle esercitazioni successive di questa serie in cui l'utilizzo di un blocco else è utile, si noterà alcuni esempi.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Verificare se la richiesta è un invio (post)

Altre, ma tornando all'esempio, che ha il if(IsPost) condizione {…}. Istruzione IsPost è effettivamente una proprietà della pagina corrente. La prima volta che viene richiesta la pagina, istruzione IsPost restituisce false. Tuttavia, se si sceglie un pulsante o in caso contrario inviare la pagina, vale a dire, registrando: istruzione IsPost restituisce true. Pertanto, istruzione IsPost consente di determinare se si vuole eseguire l'invio di un form. (In termini di verbi HTTP, se la richiesta è un'operazione GET, istruzione IsPost restituisce false. Se la richiesta è un'operazione POST, istruzione IsPost restituisce true.) In un'esercitazione successiva si utilizzerà moduli di input, in cui questo test condizione risulta particolarmente utile.

Eseguire la pagina. Poiché questa è la prima volta è richiesta la pagina, vedere "È la prima volta che hai richiesto la pagina". Tale stringa è il valore che è stato inizializzato per la variabile del messaggio. Si tratta di un test di if(IsPost), ma che restituisce false al momento, in modo da bloccare il codice all'interno di se non viene eseguito.

Fare clic su di **Invia** pulsante. La pagina viene richiesta nuovamente. Come prima, la variabile di messaggio è impostata su "È la prima volta...". Ma questa volta if(IsPost) il test restituisce true, il codice all'interno di se bloccare l'esecuzione. Il codice modifica il valore della variabile di messaggio a un valore diverso, viene eseguito il rendering nel markup.

A questo punto aggiungere un se condizione nel markup. Di sotto di &lt;p&gt; elemento che contiene il **Invia** pulsante, aggiungere il markup seguente:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Si sta aggiungendo codice all'interno del markup, pertanto è necessario iniziare con @. Non vi è un se test simile a quello aggiunto in precedenza nel blocco di codice. All'interno delle parentesi, tuttavia, si aggiungono HTML ordinario, almeno, è normale fino ad arrivare a @DateTime.Now. Si tratta di un altro poco di codice Razor, dunque è necessario aggiungere trovano davanti.

Il punto è che se è possibile aggiungere condizioni sia il codice blocca nella parte superiore e nel markup. Se si utilizza un se condizione nel corpo della pagina, le righe all'interno del blocco può essere markup o codice. In tal caso, e come è true ogni volta che si combinano markup e codice, è necessario utilizzare per rendere più chiare di ASP.NET in cui il codice è.

Eseguire la pagina e fare clic su **Invia**. Questa volta non visualizzare solo un messaggio diverso quando si invia ("dopo avere inviato..."), ma viene visualizzato un nuovo messaggio che indica la data e ora.

![Pagina 'Test Razor 2' in esecuzione nel browser con un timestamp di visualizzazione dopo l'invio](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Verifica del valore di una stringa di query

Un test di altre. Questa volta, si aggiungerà un se blocco che consente di verificare un valore denominato show che possono essere passati nella stringa di query. (Come segue: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) si modificherà la pagina in modo che il messaggio è stato visualizzati ("This is la prima volta..." e così via) viene visualizzata solo se il valore di show è true.

Nella parte inferiore (ma interno), il blocco di codice nella parte superiore della pagina, aggiungere quanto segue:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Il codice completo blocco ora simile a nell'esempio seguente. (Tenere presente che quando si copia il codice in una pagina, il rientro potrebbe essere diverso. Ma che non influisce sul modo in cui viene eseguito il codice).

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Il nuovo codice nel blocco consente di inizializzare una variabile denominata showMessage su false. Esegue quindi un se test per cercare un valore nella stringa di query. Quando si richiede innanzitutto la pagina, dispone di un URL simile alla seguente:

`http://localhost:43097/TestRazorPart2.cshtml`

Il codice determina se l'URL contiene una variabile denominata Mostra nella stringa di query, ad esempio di questa versione dell'URL:

`http://localhost:43097/TestRazorPart2.cshtml`? Mostra = true

Il test di esamina la proprietà di stringa di query dell'oggetto della richiesta. Se la stringa di query contiene un'elemento denominata presentazione e se tale elemento è impostato su true, se blocco viene eseguito e imposta la variabile showMessage su true.

È uno stratagemma in questo caso, è possibile visualizzare. Come indicato il nome, la stringa di query è una stringa. Tuttavia, è possibile testare solo per true e false se il valore di cui che si sta testando è valore booleano (true/false). Prima di testare il valore della variabile Mostra nella stringa di query, è necessario convertirlo in un valore booleano. Che è quello che il metodo AsBool: accetta una stringa come input e lo converte in un valore booleano. Chiaramente, se la stringa è "true", il metodo AsBool converte il valore su true. Se il valore della stringa è diversa, AsBool restituisce false.

> [!TIP] 
> 
> **Tipi di dati e i metodi As()**
> 
> È stato detto solo fino a questo punto quando si crea una variabile, utilizzare la funzione VAR (parola chiave). Che non però l'intera sequenza. Per modificare i valori, per aggiungere i numeri, o concatenare stringhe, o confrontare le date o di test per true o false, c# deve utilizzare a una rappresentazione interna appropriata del valore. C# può *in genere* scoprire quale deve essere tale rappresentazione (vale a dire, cosa *tipo* i dati sono) in base alle operazioni eseguite con i valori. E quindi, tuttavia, non può che. In caso contrario, è necessario aiutare indicando come c# devono rappresentare i dati in modo esplicito. Esegue il metodo AsBool, indica in c# che un valore stringa "true" o "false" deve essere considerato come un valore booleano. Sono disponibili metodi simili per rappresentare le stringhe come altri tipi, nonché come AsInt (considera come un numero intero), AsDateTime (considera come una data/ora), AsFloat (considera come numero a virgola mobile) e così via. Quando si utilizza queste informazioni come metodi (), c# non può rappresentare il valore di stringa come richiesto, verrà visualizzato un errore.


Nel markup della pagina, rimuovere o impostare come commento l'elemento (qui viene visualizzato come commento):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Destra in cui è stato rimosso o impostate come commento il testo, aggiungere quanto segue:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Il se test indica che se la variabile showMessage è true, il rendering di un &lt;p&gt; elemento con il valore della variabile di messaggio.

### <a name="summary-of-your-conditional-logic"></a>Riepilogo della logica condizionale

Nel caso in cui non si è certi interamente di le modifiche appena apportate, di seguito è riportato un riepilogo.

- La variabile di messaggio viene inizializzata su una stringa predefinita ("This is la prima volta...").
- Se la richiesta della pagina è il risultato dell'inoltro (post), il valore del messaggio viene modificato in "dopo avere inviato..."
- La variabile showMessage viene inizializzata su false.
- Se la stringa di query contiene? Mostra = true, la variabile showMessage è impostata su true.
- Nel codice, se è true, showMessage un &lt;p&gt; elemento viene eseguito il rendering che mostra il valore del messaggio. (Se showMessage è false, non viene eseguito il rendering a questo punto nel codice.)
- Nel codice, se la richiesta è una richiesta post, un &lt;p&gt; elemento viene eseguito il rendering che visualizza la data e ora.

Eseguire la pagina. Nessun messaggio, infatti showMessage è false, nel markup if(showMessage) test restituisce false.

Fare clic su **Invia**. Viene visualizzato, la data e ora, ma ancora alcun messaggio.

Nel browser, passare alla casella URL e aggiungere quanto segue alla fine dell'URL:? Mostra = true e quindi premere INVIO.

![Pagina 'Test Razor 2' nel browser con una stringa di query](intro-to-web-pages-programming/_static/image5.png)

La pagina viene visualizzata nuovamente. (Perché l'URL è stata modificata, questa è una nuova richiesta, non un invio). Fare clic su **Invia** nuovamente. Il messaggio viene visualizzato di nuovo come data e ora.

![Pagina 'Test 2 Razor' dopo l'invio quando vi è una stringa di query](intro-to-web-pages-programming/_static/image6.png)

Nell'URL, modificare? Mostra = true per? Mostra = false e premere INVIO. Inviare di nuovo la pagina. La pagina è di nuovo l'avvio, ovvero nessun messaggio.

Come notato in precedenza, la logica di questo esempio è un complicato. Tuttavia, se viene visualizzata con molte delle pagine e richiederà uno o più moduli di cui è stato illustrato di seguito.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>L'installazione di un Helper (visualizzazione di un'immagine Gravatar)

Alcune attività che gli utenti desiderano spesso nelle pagine web richiedono una grande quantità di codice o richiedono la conoscenza aggiuntivo. Esempi: visualizzazione di un grafico per i dati. Inserisce un Facebook "pulsante mi piace" in una pagina. Invio messaggio di posta elettronica dal sito Web; il ritaglio e ridimensionamento di immagini. utilizzo di PayPal per il sito. Per consentire un facile eseguire questi tipi di elementi, le pagine Web ASP.NET consente di utilizzare *helper*. Gli helper sono componenti installabili per un sito e che consentono di eseguire attività comuni con poche righe di codice Razor.

Le pagine Web ASP.NET dispone di alcuni helper compilato. Tuttavia, molte funzioni di supporto sono disponibili nei pacchetti che sono forniti usando Gestione pacchetti NuGet (componenti aggiuntivi). NuGet consente di selezionare un pacchetto da installare e quindi si occupa di tutti i dettagli dell'installazione.

In questa parte dell'esercitazione, verrà installato un helper che consente di visualizzare l'immagine Gravatar ("avatar riconosciuto a livello globale"). Si apprenderà due operazioni. Uno viene illustrato come trovare e installare un file di supporto. Si apprenderà inoltre come un helper semplifica eseguire un'operazione in caso contrario è necessario eseguire con una grande quantità di codice che è necessario scrivere manualmente.

È possibile registrare la propria Gravatar nel sito Web Gravatar al [ http://www.gravatar.com/ ](http://www.gravatar.com/), ma non è essenziale per creare un account Gravatar per eseguire questa parte dell'esercitazione.

In WebMatrix, fare clic su di **NuGet** pulsante.

![Nella finestra di dialogo Raccolta NuGet WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Verrà avviato Gestione pacchetti NuGet e visualizza i pacchetti disponibili. (Non tutti i pacchetti sono helper; alcuni aggiungere funzionalità a WebMatrix, alcuni modelli aggiuntivi e così via.) È possibile ottenere un messaggio di errore relativo a un'incompatibilità di versione. È possibile ignorare questo messaggio di errore, fare clic su **OK** e procedere con questa esercitazione.

![Nella finestra di dialogo Raccolta NuGet WebMatrix](intro-to-web-pages-programming/_static/image8.png)

Nella casella di ricerca, immettere "helper di asp.net". NuGet Mostra i pacchetti che corrispondono ai criteri di ricerca.

![Raccolta NuGet in WebMatrix che mostra i pacchetti](intro-to-web-pages-programming/_static/image9.png)

ASP.NET Web Helpers Library contiene il codice per semplificare molte attività comuni, inclusi l'utilizzo di immagini Gravatar. Selezionare il **ASP.NET Web Helpers Library** pacchetto e quindi fare clic su **installare** per avviare il programma di installazione. Selezionare **Sì** quando viene richiesto se si desidera installare il pacchetto e accettare le condizioni per completare l'installazione.

È tutto. NuGet Scarica e installa tutti gli elementi, inclusi tutti i componenti aggiuntivi che potrebbero essere necessari (*dipendenze*).

Se per qualche motivo, è necessario disinstallare un helper, il processo è molto simile. Fare clic su di **NuGet** fare clic su di **installato** scheda e selezionare il pacchetto che si desidera disinstallare.

## <a name="using-a-helper-in-a-page"></a>Utilizzo di un Helper in una pagina

Ora si userà l'helper appena installata. Il processo per l'aggiunta di un helper a una pagina è simile per la maggior parte delle funzioni di supporto.

In WebMatrix, creare una pagina e il nome *GravatarTest.cshml*. (Si sta creando una pagina speciale per l'helper di test, ma è possibile utilizzare l'helper in qualsiasi pagina del sito).

All'interno di &lt;corpo&gt; elemento, aggiungere un &lt;div&gt; elemento. All'interno di &lt;div&gt; elemento, digitare quanto segue:

@Gravatar.

Il carattere @ è lo stesso carattere che hai usato per contrassegnare il codice Razor. **Gravatar** è l'oggetto helper che si sta lavorando.

Non appena si digita il punto (.), WebMatrix consente di visualizzare un elenco di *metodi* (funzioni), che rende disponibile il supporto di Gravatar:

![Elenco di helper elenco a discesa IntelliSense Gravatar](intro-to-web-pages-programming/_static/image10.png)

Questa funzionalità è nota come *IntelliSense*. È utile che il codice, fornendo scelte appropriato al contesto. IntelliSense funziona con HTML, CSS, codice ASP.NET, JavaScript e altri linguaggi supportati in WebMatrix. Si tratta di un'altra funzionalità che semplifica lo sviluppo di pagine web in WebMatrix.

Premere G sulla tastiera e vedere IntelliSense consente di trovare il metodo GetHtml. Premere Tab. IntelliSense consente di inserire il metodo selezionato (GetHtml) per l'utente. Digitare una parentesi aperta e notare che la parentesi di chiusura viene aggiunto automaticamente. Digitare l'indirizzo di posta elettronica tra virgolette tra due parentesi. Se si dispone di un account Gravatar, verrà restituito l'immagine del profilo. Se non si dispone di un account Gravatar, un'immagine predefinita. Al termine, la riga è simile al seguente:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Ora è possibile visualizzare la pagina in un browser. Verrà visualizzata l'immagine o l'immagine predefinita, a seconda se si dispone di un account Gravatar.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![immagine predefinita](intro-to-web-pages-programming/_static/image12.png)

Per ottenere un'idea di cosa sta effettuando l'helper per l'utente, visualizzare l'origine della pagina nel browser. Insieme a HTML presenti nella pagina, vedrai un elemento immagine che include un identificatore. Si tratta di codice che il supporto di cui è stato eseguito il rendering nella pagina nella posizione in cui era @Gravatar.GetHtml. Il supporto ha acquisito le informazioni fornite e ha generato il codice che comunica direttamente con Gravatar per ottenere nuovamente l'immagine corretta per l'account specificato.

Il metodo GetHtml consente inoltre di personalizzare l'immagine fornendo altri parametri. Il codice seguente viene illustrato come richiedere un'immagine con una larghezza e l'altezza di 40 pixel e che usa un'immagine predefinita specificata denominata **wavatar** se l'account specificato non esiste.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Questo codice produce un risultato simile il risultato seguente (l'immagine predefinita in modo casuale potrà variare).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Esercitazione seguente

Per mantenere questa breve esercitazione, era necessario concentrarsi solo alcune nozioni di base. Ovviamente, è presente un *molti* ulteriori Razor e c#. Si apprenderà come esaminare le esercitazioni. Se si è interessati a ottenere ulteriori informazioni su aspetti della programmazione relativi Razor e c# ora, è possibile leggere qui l'introduzione approfondita: [Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

L'esercitazione successiva presentato l'utilizzo di un database. In tale esercitazione innanzitutto creare l'applicazione di esempio che consente di elencare i film preferiti.

## <a name="complete-listing-for-testrazor-page"></a>Elenco completo per la pagina TestRazor

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Elenco completo per la pagina TestRazorPart2

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Elenco completo per la pagina GravatarTest

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET con sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Helper di Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Precedente](getting-started.md)
> [Successivo](displaying-data.md)
