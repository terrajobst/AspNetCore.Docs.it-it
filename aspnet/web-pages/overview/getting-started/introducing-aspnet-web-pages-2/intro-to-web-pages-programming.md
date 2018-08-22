---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: 'Introduzione a ASP.NET Web Pages: nozioni fondamentali di programmazione | Microsoft Docs'
author: tfitzmac
description: 'Questa esercitazione offre una panoramica su come programmare in ASP.NET Web Pages con sintassi Razor. Si apprenderà: la sintassi "Razor" di base usato per richieste pull...'
ms.author: riande
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 120246fab3e71afeef2e2b7c4388f7c294e6b703
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826340"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>Introduzione a pagine Web ASP.NET - nozioni fondamentali di programmazione
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione offre una panoramica su come programmare in ASP.NET Web Pages con sintassi Razor.
> 
> Che cosa si apprenderà come:
> 
> - La sintassi "Razor" base utilizzabili per la programmazione in ASP.NET Web Pages.
> - Alcuni base il linguaggio c#, si userà il linguaggio di programmazione.
> - Alcuni concetti di programmazione fondamentali per le pagine Web.
> - Come installare i pacchetti (componenti che contengono codice predefinito) da usare con il sito.
> - Come usare *helper* per eseguire attività comuni di programmazione.
>   
> 
> Le funzionalità/tecnologie illustrate:
> 
> - NuGet e la gestione dei pacchetti.
> - Il `Gravatar` helper.


Questa esercitazione è principalmente un esercizio di presentare la sintassi di programmazione che si userà per ASP.NET Web Pages. Apprenderanno *sintassi Razor* e linguaggio di programmazione a codice scritto in c#. È stato ottenuto una rapida panoramica di questa sintassi nell'esercitazione precedente; in questa esercitazione illustra la sintassi di più.

Ti assicuriamo che questa esercitazione implica la maggior parte di programmazione che viene visualizzata in un'esercitazione singola e che è l'unico esercitazione che è *solo* sulla programmazione. Le esercitazioni rimanenti in questo set si creerà in effetti pagine in cui eseguire operazioni interessanti.

Verranno inoltre illustrate *helper*. Un helper è un componente, ovvero una parte di backup incluso nel pacchetto di codice, che è possibile aggiungere a una pagina. L'helper esegue operazioni che in caso contrario, potrebbe essere noioso o complessa da svolgere manualmente.

## <a name="creating-a-page-to-play-with-razor"></a>Creazione di una pagina per giocare con Razor

In questa sezione sarà giocarci un po' Razor in modo che è possibile farsi un'idea della sintassi di base.

Se non è già in esecuzione, avviare WebMatrix. Si userà il sito Web è stato creato nell'esercitazione precedente ([Guida introduttiva con le pagine Web](https://go.microsoft.com/fwlink/?LinkId=251578)). Per riaprirlo, fare clic su **siti personali** e scegliere **WebPageMovies**:

![Schermata che mostra le opzioni aprire il sito e siti personali evidenziato iniziale di WebMatrix](intro-to-web-pages-programming/_static/image1.png)

Selezionare il **file** dell'area di lavoro.

Nella barra multifunzione, fare clic su **New** per creare una pagina. Selezionare **CSHTML** e denominare la nuova pagina *TestRazor.cshtml*.

Fare clic su **OK**.

Copiare quanto segue nel file di sostituzione completa che cos'è esiste già.

> [!NOTE]
> Quando si copia codice o markup dagli esempi in una pagina, il rientro e allineamento potrebbero non essere uguale a quello dell'esercitazione. Rientro e l'allineamento non influiscono sul modo in cui viene eseguito il codice, tuttavia.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Esaminando la pagina di esempio

La maggior parte di ciò che viene visualizzato è HTML normale. Tuttavia, nella parte superiore è il blocco di codice:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Si noti quanto segue su questo blocco di codice:

- Il carattere @ indica ad ASP.NET che ciò che segue è codice Razor, non il codice HTML. ASP.NET considera tutti gli elementi dopo il @ carattere come codice fino a quando non viene eseguito nuovamente in HTML. (In questo caso, ecco il &lt;! Tipo di documento&gt; elemento.
- Le parentesi graffe ({e}) racchiudono un blocco di codice Razor se il codice ha più di una riga. Le parentesi graffe indicano ASP.NET inizia e dove finisce il codice per quel blocco.
- Il / / caratteri contrassegna un commento, vale a dire, una parte del codice che non verrà eseguito.
- Ogni istruzione deve terminare con un punto e virgola (;). (Non i commenti, però.)
- È possibile archiviare valori nelle *variabili*, che è stato creato (*dichiarare*) con la parola chiave var Quando si crea una variabile, occorre assegnargli un nome, che può includere lettere, numeri e caratteri di sottolineatura (\_). I nomi delle variabili non possono iniziare con un numero e non è possibile usare il nome di una parola chiave programmazione (ad esempio var).
- Le stringhe di caratteri (ad esempio, "ASP.NET" e "Pagine Web") racchiuderlo tra virgolette. (Devono essere racchiusi tra virgolette doppie). I numeri non sono racchiusi tra virgolette.
- Lo spazio vuoto all'esterno di virgolette non è rilevante. Le interruzioni di riga non sono soprattutto importanti; l'eccezione è che non è possibile dividere una stringa racchiusa tra virgolette in più righe. Rientro e l'allineamento non è rilevante.

Qualcosa che non sia evidente da questo esempio è che tutto il codice tra maiuscole e minuscole. Ciò significa che le somma variabile è una variabile diversa rispetto alle variabili che potrebbe essere denominata la somma o la somma. Analogamente, var è una parola chiave, ma non è Var.

### <a name="objects-and-properties-and-methods"></a>Oggetti e le proprietà e metodi

Non vi è l'espressione DateTime. Now. In altre parole, data/ora è un' *oggetto*. Un oggetto è una cosa che è possibile programmare con, ovvero una pagina di una casella di testo, un file, un'immagine, una richiesta web, un messaggio di posta elettronica, un record del cliente, e così via. Gli oggetti hanno uno o più *proprietà* che descrivono le loro caratteristiche. Un oggetto casella di testo ha una proprietà di testo (tra gli altri), un oggetto richiesta presenta una proprietà Url (e altri), un messaggio di posta elettronica ha una proprietà From e una proprietà a, e così via. Gli oggetti hanno anche *metodi* che si trovano i verbi"" possono eseguire. Si lavorerà con gli oggetti molto.

Come può notare nell'esempio, data/ora è un oggetto che consente di programma date e ore. Include una proprietà denominata a questo punto che restituisce la data e ora correnti.

### <a name="using-code-to-render-markup-in-the-page"></a>Uso del codice per il rendering del markup della pagina

Nel corpo della pagina, tenere presente quanto segue:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Anche in questo caso, il carattere @ indica ad ASP.NET che ciò che segue è il codice, non il codice HTML. Nel markup è possibile aggiungere seguita da un'espressione di codice e ASP.NET forniranno a quel punto il valore di tale diritto di espressione. Nell'esempio riportato @a eseguirà il rendering di qualsiasi elemento è il valore della variabile denominata, @product esegue il rendering di qualsiasi elemento è all'interno del prodotto denominato variabile e così via.

Non è necessario limitarsi a variabili, tuttavia. In alcuni casi, il carattere @ precede un'espressione:

- @(un\*b) esegue il rendering delle informazioni presenti nelle variabili del prodotto una a e b. (Il \* operatore: moltiplicazione.)
- @(tecnologia + "" + prodotto) Visualizza i valori nella tecnologia di variabili e prodotto dopo concatenarle e l'aggiunta di uno spazio tra. L'operatore (+) per la concatenazione di stringhe è quello utilizzato per l'operatore per l'aggiunta di numeri. ASP.NET in genere possibile indicare se si lavora con numeri o stringhe e si esegue le operazioni giuste con l'operatore +.
- @Request.Url esegue il rendering la proprietà Url dell'oggetto richiesta. Oggetto della richiesta contiene informazioni sulla richiesta corrente dal browser e ovviamente la proprietà Url contiene l'URL della richiesta corrente.

L'esempio è anche progettato per mostrare che è possibile lavorare in modi diversi. È possibile eseguire calcoli nel blocco di codice nella parte superiore, inserire i risultati in una variabile e quindi eseguire il rendering la variabile nel markup. Oppure è possibile eseguire calcoli in un diritto di espressione nel markup. L'approccio che scelto dipende dal ciò che si sta eseguendo e, in una certa misura, le proprie esigenze.

### <a name="seeing-the-code-in-action"></a>Visualizzare il codice in azione

Fare doppio clic il nome del file e quindi scegliere **Avvia nel browser**. Viene visualizzata la pagina nel browser con tutti i valori e le espressioni risolvere nella pagina.

![Pagina 'TestRazor' in esecuzione nel browser](intro-to-web-pages-programming/_static/image2.png)

Esaminare l'origine nel browser.

![Origine della pagina 'Test Razor' nel browser](intro-to-web-pages-programming/_static/image3.png)

Come previsto dalla tua esperienza nell'esercitazione precedente, nessuna parte del codice Razor è nella pagina. Siano che visibili solo i valori effettivi visualizzati. Quando si esegue una pagina, si sta effettivamente eseguendo una richiesta al server web incorporato in WebMatrix. Quando viene ricevuta la richiesta, ASP.NET risolve tutti i valori e le espressioni e viene eseguito il rendering i relativi valori nella pagina. Invia quindi la pagina nel browser.

> [!TIP] 
> 
> **Razor e c#**
> 
> Finora abbiamo detto che si lavora con sintassi Razor. È vero, ma non è la storia completa. Il linguaggio di programmazione effettivo in uso viene chiamato *c#*. C# è stato creato da Microsoft oltre un decennio fa ed è diventato uno dei linguaggi di programmazione principali per la creazione di App di Windows. Tutte le regole di cui che si è visto su come assegnare un nome di una variabile e come creare le istruzioni e così via sono effettivamente tutte le regole del linguaggio c#.
> 
> Razor fa riferimento in modo più specifico per il piccolo set di convenzioni per la modalità incorporare il codice in una pagina. Ad esempio, la convenzione di utilizzo di per contrassegnare il codice nella pagina e l'utilizzo di @ {} per incorporare un blocco di codice è l'aspetto di Razor di una pagina. Gli helper sono considerati anche far parte del Razor. Sintassi Razor viene usata in più posti più semplicemente in ASP.NET Web Pages. (Ad esempio, viene utilizzato nelle visualizzazioni di ASP.NET MVC nonché.)
> 
> È importante tenere presente questo perché se si cerca di informazioni sulla programmazione ASP.NET Web Pages, troverai un numero elevato di riferimenti per Razor. Tuttavia, molte di tali riferimenti non sono applicabili a ciò che si ha tale operazione e pertanto potrebbe generare confusione. E infatti, molte delle domande programmazione sono davvero un grosso sviluppo per l'uso di c# o l'utilizzo di ASP.NET. Pertanto, se si esamina in modo specifico per informazioni su Razor, si potrebbero non trovare le risposte che necessarie.


## <a name="adding-some-conditional-logic"></a>Aggiunta di una logica condizionale

Una delle eccezionali funzionalità sull'uso del codice in una pagina è che è possibile modificare cosa accade in base a diverse condizioni. In questa parte dell'esercitazione, è possibile sperimentare con alcuni modi per modificare la visualizzazione nella pagina.

Nell'esempio sarà semplice e un po' complicato, in modo che è possibile concentrarsi sulla logica condizionale. La pagina che si creeranno eseguirà questa operazione:

- Visualizzare un testo diverso nella pagina a seconda se è la prima volta che viene visualizzata la pagina o se si è fatto clic di un pulsante per l'invio della pagina. Che sarà il primo test condizionale.
- Visualizzare il messaggio solo se un determinato valore viene passato nella stringa di query dell'URL (http://...?show=true). Che sarà il secondo test condizionale.

In WebMatrix, creare una pagina e denominarla *TestRazorPart2.cshtml*. (Nella barra multifunzione, fare clic su **New**, scegliere **CSHTML**, denominare il file e quindi fare clic su **OK**.)

Sostituire il contenuto della pagina con il codice seguente:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Il blocco di codice nella parte superiore consente di inizializzare una variabile denominata messaggio con un testo. Nel corpo della pagina, il contenuto della variabile di messaggio viene visualizzato all'interno di un &lt;p&gt; elemento. Il markup contiene anche un &lt;input&gt; elemento per creare un **Submit** pulsante.

Eseguire la pagina per verificarne il funzionamento adesso. Per il momento, è fondamentalmente una pagina statica, anche se si sceglie la **Submit** pulsante.

Tornare a WebMatrix. All'interno del blocco di codice, aggiungere il seguente codice evidenziato *dopo* la riga che inizializza messaggio:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Se il blocco di {}

Appena aggiunto era un blocco if condizione. Nel codice, il se condizione ha una struttura simile al seguente:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

La condizione da testare è racchiuso tra parentesi. Deve essere un valore o un'espressione che restituisce true o false. Se la condizione è true, ASP.NET esegue le istruzioni che si trovano all'interno delle parentesi graffe. (Queste sono le *quindi* fa parte delle *if-then* per la logica.) Se la condizione è false, il blocco di codice viene ignorato.

Ecco alcuni esempi delle condizioni è possibile testare in un'istruzione:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

È possibile testare le variabili in base ai valori o contro espressioni usando un <em>operatore logico</em> oppure <em>operatore di confronto</em>: uguale a (= =), maggiore di (&gt;), minore di (&lt;), maggiore o uguale a (&gt;=) e minore o uguale a (&lt;=). La! = operatore significa che non è uguale a, ad esempio, se (un! = 0) significa <em>se</em> <em>un</em><em>non è uguale a 0</em>.

> [!NOTE]
> Assicurarsi che si notare che l'operatore di confronto per l'uguaglianza a (= =) non è uguale a quella =. I = operatore viene usato solo per assegnare valori (var un = 2). Se si combinano questi operatori di, si ottiene un errore o si verificherà alcuni risultati imprevisti.


Per verificare se un elemento è true, la sintassi completa è if(IsDone == true). Ma è anche possibile usare il if(IsDone) di scelta rapida. Se è presente alcun operatore di confronto, ASP.NET presuppone che si sta testando per true.

Il carattere! operatore di per sé significa che un'operazione con NOT logica. Ad esempio, la condizione if (! Istruzione IsPost) significa *se l'istruzione IsPost non è vero*.

È possibile combinare condizioni con un'operazione con AND logico (&amp; &amp; operator) o OR logico (| | operator). Ad esempio, l'ultima del se le condizioni nei mezzi per gli esempi precedenti *se FileProcessingIsDone non è impostata su true AND displayMessage è impostata su false*.

### <a name="the-else-block"></a>Il blocco else

Un'ultima considerazione da se blocchi: un se blocco può essere seguito da un blocco else. Un blocco else è utile è è necessario eseguire codice diverso quando la condizione è false. Ecco un esempio semplice:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Sono riportati alcuni esempi in esercitazioni successive di questa serie in cui è utile usare un blocco else.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Verificare se la richiesta è un invio (post)

C'è di altro, ma tornando all'esempio, che contiene la condizione if(IsPost) {...}. Istruzione IsPost è effettivamente una proprietà della pagina corrente. La prima volta che viene richiesta la pagina, istruzione IsPost restituisce false. Tuttavia, se si sceglie un pulsante o la pagina invia in caso contrario, ovvero, ovvero di registrarla, istruzione IsPost restituisce true. Quindi, istruzione IsPost consente di determinare se ha a che fare con l'invio di un form. (In termini di verbi HTTP, se la richiesta è un'operazione GET, istruzione IsPost restituisce false. Se la richiesta è un'operazione POST, istruzione IsPost restituisce true). In un'esercitazione successiva si utilizzerà moduli di input, in cui questo test diventa particolarmente utile.

Eseguire la pagina. Poiché questa è la prima volta che ha richiesto la pagina, vedere "Questa è la prima volta che hai richiesto la pagina". Tale stringa è il valore che è stato inizializzato la variabile di messaggio per. È un test if(IsPost), ma che restituisce false al momento, in modo che blocchino il codice all'interno di se non viene eseguito.

Scegliere il **Submit** pulsante. La pagina viene richiesta nuovamente. Come prima, la variabile di messaggio è impostata su "Questa è la prima volta...". Ma questa volta if(IsPost) il test restituisce true, in modo che il codice all'interno di se bloccare l'esecuzione. Il codice modifica il valore della variabile di messaggio su un valore diverso, ossia ciò che viene eseguito il rendering nel markup.

A questo punto aggiungere un blocco if condizione nel markup. Di seguito il &lt;p&gt; elemento che contiene il **Submit** pulsante, aggiungere il markup seguente:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Si sta aggiungendo codice nel markup, è quindi necessario iniziare con @. Non vi è un blocco if test simile a quello aggiunto in precedenza nel blocco di codice. Tra parentesi graffe, tuttavia, si stanno aggiungendo HTML ordinari, almeno, è normale fino ad arrivare a @DateTime.Now. Si tratta di un altro po' di codice Razor, anche in questo caso è necessario aggiungere trovano davanti.

Il concetto è che se è possibile aggiungere condizioni in entrambe il blocco di codice nella parte superiore e nel markup. Se si usa un blocco if condizione nel corpo della pagina, le righe all'interno del blocco può essere codice o markup. In tal caso, e come è true ogni volta che si combinano markup e codice, è necessario utilizzare per renderla deselezionarla per ASP.NET in cui il codice è.

Eseguire la pagina e fare clic su **Submit**. Questa volta non visualizzare solo un messaggio diverso quando si invia ("dopo aver inviato..."), ma viene visualizzato un messaggio nuovo in cui sono elencate la data e ora.

![Nella pagina 'Test Razor 2' in esecuzione nel browser con un timestamp che mostra dopo INVIO](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Verifica del valore di una stringa di query

Ulteriori test. Questa volta, si aggiungerà un blocco if blocco che testa un valore denominato show che possono essere passati nella stringa di query. (Simile al seguente: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) si modificherà la pagina in modo che il messaggio in cui è stato visualizzati ("Questa è la prima volta..." e così via) viene visualizzata solo se il valore di show è true.

Nel basso, ma all'interno, il blocco di codice nella parte superiore della pagina, aggiungere quanto segue:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Di codice completo blocco ora simile al seguente. (Tenere presente che quando si copia il codice in una pagina, il rientro potrebbe apparire diverso. Ma che non influiscono sul modo in cui viene eseguito il codice).

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Il nuovo codice nel blocco consente di inizializzare una variabile denominata showMessage su false. Ed esegue un blocco if test per individuare un valore nella stringa di query. Quando si richiede innanzitutto la pagina, dispone di un URL simile alla seguente:

`http://localhost:43097/TestRazorPart2.cshtml`

Il codice determina se l'URL contiene una variabile denominata show nella stringa di query, ad esempio di questa versione dell'URL:

`http://localhost:43097/TestRazorPart2.cshtml`? Mostra = true

Il test stesso esamina la proprietà di stringa di query dell'oggetto richiesta. Se la stringa di query contiene un elemento denominato show e se tale elemento è impostato su true, se il blocco viene eseguito e imposta la variabile showMessage su true.

È presente un trucco qui, come si può notare. Come è indicato il nome, la stringa di query è una stringa. Tuttavia, è possibile testare solo per true e false se il valore che si sta testando è un valore booleano (true/false). Prima di testare il valore della variabile show nella stringa di query, è necessario convertirlo in un valore booleano. Che avviene con il metodo AsBool, accetta come input una stringa e lo converte in un valore booleano. Chiaramente, se la stringa è "true", il metodo AsBool converte tale valore su true. Se il valore della stringa è diversa, AsBool restituisce false.

> [!TIP] 
> 
> **Tipi di dati e metodi As()**
> 
> Abbiamo solo detto finora quando si crea una variabile, usare la parola chiave var Che non però l'intero brano. Per modificare i valori, aggiungere i numeri, o concatenare le stringhe, o confrontare le date o di test per true o false, c# ha lavorare con una rappresentazione interna appropriata del valore. C# possono *in genere* scoprire quale deve essere tale rappresentazione (vale a dire, cosa *tipo* i dati sono) in base alle operazioni eseguite con i valori. E quindi, tuttavia, non può che. In caso contrario, è necessario supporto indicando in modo esplicito come c# devono rappresentare i dati. Esegue il metodo AsBool: indica a c# che un valore stringa "true" o "false" deve essere considerato come un valore booleano. Esistono metodi simili per rappresentare stringhe in altri tipi, nonché come AsInt (considerata come un numero intero), AsDateTime (considerata come una data/ora), AsFloat (considerata come un numero a virgola mobile) e così via. Quando si usa queste informazioni come metodi (), se c# non può rappresentare il valore della stringa come richiesto, si noterà un errore.


Nel markup della pagina, rimuovere o impostare come commento questo elemento (in questo caso è possibile vederlo commentata):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

A destra in cui è stato rimosso o impostata come commento che il testo, aggiungere quanto segue:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Il se test indica che se la variabile showMessage è true, il rendering di un &lt;p&gt; elemento con il valore della variabile di messaggio.

### <a name="summary-of-your-conditional-logic"></a>Riepilogo della logica condizionale

Nel caso in cui non si è interamente certi di cosa si è appena fatto, ecco un riepilogo.

- La variabile di messaggio viene inizializzata su una stringa predefinita ("Questa è la prima volta...").
- Se la richiesta della pagina è il risultato di un invio (post), il valore del messaggio viene modificato in "dopo aver inviato..."
- La variabile showMessage viene inizializzata su false.
- Se la stringa di query contiene? Mostra = true, la variabile showMessage è impostata su true.
- Nel markup, se è true, showMessage una &lt;p&gt; elemento viene eseguito il rendering che mostra il valore del messaggio. (Se showMessage è false, non viene eseguito il rendering a quel punto nel markup.)
- Nel markup, se la richiesta viene pubblicato un post, un &lt;p&gt; elemento viene eseguito il rendering che visualizza la data e ora.

Eseguire la pagina. Non si verifica alcun messaggio, poiché showMessage è false, in modo che nel markup if(showMessage) test restituisce false.

Fare clic su **Invia**. Noterete che la data e ora, ma ancora alcun messaggio.

Nel browser, passare alla finestra di URL e aggiungere il codice seguente alla fine dell'URL:? Mostra = true e quindi premere INVIO.

!['Test 2 Razor' pagina nel browser che mostra la stringa di query](intro-to-web-pages-programming/_static/image5.png)

La pagina viene visualizzata nuovamente. (Perché è stato modificato l'URL, questa è una nuova richiesta, non un invio). Fare clic su **Submit** nuovamente. Il messaggio viene visualizzato anche in questo caso, perché è la data e ora.

![Pagina 'Test 2 Razor' dopo l'invio quando vi è una stringa di query](intro-to-web-pages-programming/_static/image6.png)

Nell'URL, modificare? Mostra = true per? Mostra = false e premere INVIO. Inviare di nuovo la pagina. La pagina è al modo in cui è stato avviato, ovvero nessun messaggio.

Come indicato in precedenza, la logica di questo esempio è un po' complicato. Tuttavia, se si intende ad avviarsi in molte delle pagine, e richiederà uno o più dei moduli di cui si è visto qui.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Installazione di un Helper (visualizzazione di un'immagine Gravatar)

Alcune attività che spesso gli utenti vogliono eseguire operazioni sulle pagine web richiedono una grande quantità di codice o della Knowledge Base aggiuntiva. Esempi: visualizzazione di un grafico per i dati. Inserire un Facebook "Like" pulsante in una pagina. Invio messaggio di posta elettronica tramite il sito Web; ritaglio o ridimensionamento delle immagini; utilizzo di PayPal per il sito. Per renderlo semplice eseguire questo tipo di attività, ASP.NET Web Pages ti permette di usare *helper*. Gli helper sono componenti da installare per un sito e che consentono di eseguire attività tipiche utilizzando solo poche righe di codice Razor.

ASP.NET Web Pages ha alcuni helper incorporato. Tuttavia, molti helper sono disponibili nei pacchetti forniti tramite Gestione pacchetti NuGet (componenti aggiuntivi). NuGet consente di selezionare un pacchetto da installare e quindi si occupa di tutti i dettagli dell'installazione.

In questa parte dell'esercitazione, verrà installato un helper che consente di visualizzare l'immagine Gravatar ("avatar riconosciuto a livello globale"). Si apprenderà due cose. Uno viene illustrato come trovare e installare un file di supporto. Scoprirai anche come un helper rende più semplice eseguire un'operazione in caso contrario, è necessario per essere eseguite tramite una grande quantità di codice che sarebbe necessario scrivere personalmente.

È possibile registrare il proprio Gravatar presso il sito Web Gravatar alla [ http://www.gravatar.com/ ](http://www.gravatar.com/), ma non è essenziale per creare un account Gravatar per eseguire questa parte dell'esercitazione.

In WebMatrix, fare clic sui **NuGet** pulsante.

![Finestra di dialogo NuGet Gallery in WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Verrà avviato Gestione pacchetti NuGet e visualizza i pacchetti disponibili. (Non tutti i pacchetti sono helper; alcuni aggiungono funzionalità a WebMatrix, alcune sono modelli aggiuntivi e così via.) È possibile ottenere un messaggio di errore sull'incompatibilità di versione. È possibile ignorare questo messaggio di errore facendo **OK** e procedere con questa esercitazione.

![Finestra di dialogo NuGet Gallery in WebMatrix](intro-to-web-pages-programming/_static/image8.png)

Nella casella di ricerca immettere "helper di asp.net". NuGet Mostra i pacchetti che corrispondono ai criteri di ricerca.

![Raccolta NuGet in WebMatrix che mostra i pacchetti](intro-to-web-pages-programming/_static/image9.png)

ASP.NET Web Helpers Library contiene codice per semplificare molte attività comuni, incluso l'uso di immagini Gravatar. Selezionare il **ASP.NET Web Helpers Library** del pacchetto e quindi fare clic su **installare** per avviare il programma di installazione. Selezionare **Sì** quando viene richiesto se si desidera installare il pacchetto e accettare le condizioni per completare l'installazione.

È tutto. NuGet Scarica e installa tutti gli elementi, inclusi tutti i componenti aggiuntivi che potrebbero essere necessari (*dipendenze*).

Se per qualche motivo è necessario disinstallare un helper, il processo è molto simile. Fare clic sul **NuGet** , scegliere il **installato** scheda e selezionare il pacchetto da disinstallare.

## <a name="using-a-helper-in-a-page"></a>Uso di un Helper in una pagina

A questo punto si userà l'helper appena installato. Il processo per l'aggiunta di un helper a una pagina è simile a quello per la maggior parte degli helper.

In WebMatrix, creare una pagina e denominarla *GravatarTest.cshml*. (Si sta creando una pagina speciale per l'helper di test, ma è possibile usare gli helper in qualsiasi pagina nel sito).

All'interno di &lt;corpo&gt; elemento, aggiungere un &lt;div&gt; elemento. All'interno di &lt;div&gt; elemento, digitare quanto segue:

@Gravatar.

Il carattere @ è lo stesso carattere è stato usato per contrassegnare il codice Razor. **Gravatar** è l'oggetto helper che si sta lavorando.

Non appena si digita il punto (.), WebMatrix consente di visualizzare un elenco delle *metodi* (funzioni) che l'helper Gravatar rende disponibile:

![Elenco di helper elenco a discesa IntelliSense Gravatar](intro-to-web-pages-programming/_static/image10.png)

Questa funzionalità è detta *IntelliSense*. Consente di codice, fornendo le scelte appropriato al contesto. IntelliSense funziona con HTML, CSS, codice ASP.NET, JavaScript e altri linguaggi supportati in WebMatrix. Si tratta di un'altra funzionalità che rende più semplice sviluppare le pagine web con WebMatrix.

Premere G sulla tastiera e vedere che IntelliSense consente di trovare il metodo GetHtml. Premere Tab. IntelliSense consente di inserire il metodo selezionato (GetHtml) per l'utente. Digitare una parentesi aperta e notare che la parentesi di chiusura viene aggiunto automaticamente. Digitare l'indirizzo di posta elettronica racchiuso tra virgolette tra due parentesi. Se si ha un account Gravatar, verrà restituito l'immagine del profilo. Se non hai un account Gravatar, viene restituita un'immagine predefinita. Al termine, la riga di aspetto simile al seguente:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

A questo punto è possibile visualizzare la pagina in un browser. Verrà visualizzata l'immagine o l'immagine predefinita, a seconda che si disponga di un account Gravatar.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![immagine predefinita](intro-to-web-pages-programming/_static/image12.png)

Per avere un'idea di ciò che sta eseguendo l'helper per l'utente, visualizzare l'origine della pagina nel browser. Con l'HTML presenti nella pagina, noterete un elemento immagine che include un identificatore. Si tratta di codice che l'helper di cui è stato eseguito il rendering nella pagina in posizioni in cui bisognava @Gravatar.GetHtml. L'helper ha acquisito le informazioni fornite e ha generato il codice che comunica direttamente con Gravatar per accedere di nuovo l'immagine corretta per l'account specificato.

Il metodo GetHtml consente inoltre di personalizzare l'immagine, fornendo gli altri parametri. Il codice seguente viene illustrato come richiedere un'immagine con una larghezza e altezza pari a 40 pixel e usa un'immagine predefinita specificata denominata **wavatar** se l'account specificato non esiste.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Questo codice produce un risultato analogo il seguente risultato (l'immagine predefinita in modo casuale potrà variare).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>In arrivo

Per mantenere questa breve esercitazione, abbiamo dovuto concentrarsi solo alcune nozioni di base. Naturalmente, è disponibile un' *molto* ulteriori Razor e c#. Si apprenderà maggiori informazioni passare attraverso queste esercitazioni. Se vuoi scoprire di più sugli aspetti di programmazione di Razor e c# subito, è possibile leggere un'introduzione più completa qui: [Introduzione alla programmazione Web ASP.NET usando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

L'esercitazione successiva viene presentato con un database. In tale esercitazione, si sarà iniziare a creare l'applicazione di esempio che consente di elencare i film preferiti.

## <a name="complete-listing-for-testrazor-page"></a>Elenco completo per la pagina TestRazor

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Elenco completo per la pagina TestRazorPart2

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Elenco completo per la pagina GravatarTest

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET usando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Helper di Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Precedente](getting-started.md)
> [Successivo](displaying-data.md)
