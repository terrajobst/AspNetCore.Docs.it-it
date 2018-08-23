---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Introduzione alla programmazione Web ASP.NET usando la sintassi Razor (c#) | Microsoft Docs
author: tfitzmac
description: In questo capitolo offre una panoramica della programmazione con ASP.NET Web Pages con sintassi Razor. ASP.NET è una tecnologia Microsoft per l'esecuzione di indirizzo pa web dinamiche...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 347e5ddbc02866887d3f422ecc291e5e3dfacaaf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827512"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Introduzione alla programmazione Web ASP.NET usando la sintassi Razor (c#)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo offre una panoramica della programmazione con ASP.NET Web Pages con sintassi Razor. ASP.NET è la tecnologia Microsoft per l'esecuzione di pagine web dinamiche nei server web. Questo articoli è incentrata sulla usando il linguaggio di programmazione c#.
> 
> **Si apprenderà**:
> 
> - Il principale 8 suggerimenti per l'introduzione a ASP.NET Web Pages con sintassi Razor di programmazione di programmazione.
> - Concetti di programmazione di base che è necessario.
> - Il codice server ASP.NET e la sintassi Razor è dedicato.
>   
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


## <a name="the-top-8-programming-tips"></a>8 suggerimenti di programmazione principali

In questa sezione sono elencati alcuni suggerimenti che è assolutamente necessario conoscere quando si inizia a scrivere codice server ASP.NET tramite la sintassi Razor.

> [!NOTE]
> La sintassi Razor è basata sul linguaggio di programmazione c#, e che è il linguaggio che viene usato spesso con ASP.NET Web Pages. Tuttavia, la sintassi Razor supporta inoltre il linguaggio Visual Basic e tutto ciò che è anche possibile eseguire in Visual Basic. Per informazioni dettagliate, vedere l'appendice [linguaggio Visual Basic e la sintassi](https://go.microsoft.com/fwlink/?LinkId=202908).


È possibile trovare altre informazioni sulla maggior parte di queste tecniche di programmazione più avanti nell'articolo.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Aggiungere codice a una pagina utilizzando il carattere @

Il `@` carattere avvia espressioni inline, blocchi di istruzioni singolo e blocchi a più istruzioni:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Si tratta di queste istruzioni come appaiono quando la pagina viene eseguita in un browser:

![Razor Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **La codifica HTML**
> 
> Quando si visualizza il contenuto in una pagina mediante la `@` carattere, come negli esempi precedenti, ASP.NET codifica in HTML di output. Questa impostazione sostituisce i caratteri riservati HTML (ad esempio `<` e `>` e `&`) con i codici che abilitano i caratteri da visualizzare come caratteri in una pagina web anziché essere interpretato come tag HTML o le entità. Senza codifica HTML, l'output dal codice server non vengano visualizzati correttamente e potrebbe esporre una pagina a rischi di sicurezza.
> 
> Se l'obiettivo consiste nel markup HTML che esegue il rendering dei tag come markup di output (ad esempio `<p></p>` per un paragrafo o `<em></em>` per enfatizzare il testo), vedere la sezione [combinazione di testo, Markup e codice nei blocchi di codice](#BM_CombiningTextMarkupAndCode) più avanti in questo articolo.
> 
> Altre informazioni sulla codifica HTML [utilizzo di moduli](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Blocchi di codice racchiudere tra parentesi graffe

Oggetto *blocco di codice* include uno o più istruzioni di codice e viene racchiuso tra parentesi graffe.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Il risultato visualizzato in un browser:

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. All'interno di un blocco alla fine si ogni istruzione del codice con un punto e virgola

All'interno di un blocco di codice, ogni istruzione di codice completo deve terminare con un punto e virgola. Espressioni inline non terminano con un punto e virgola.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Si usano le variabili per archiviare i valori

È possibile archiviare i valori in una *variabile*, incluse stringhe, numeri e date, e così via. Si crea una nuova variabile usando la `var` (parola chiave). È possibile inserire i valori delle variabili direttamente in una pagina mediante `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Il risultato visualizzato in un browser:

![Razor Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Valori letterali stringa racchiuderlo tra virgolette doppie

Oggetto *stringa* è una sequenza di caratteri che vengono considerati come testo. Per specificare una stringa, si racchiuderlo tra virgolette doppie:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Se la stringa che si desidera visualizzare contiene un carattere barra rovesciata ( `\` ) o le virgolette doppie ( `"` ), utilizzare un *valore letterale stringa verbatim* che viene anteposto con il `@` operatore. (In c#, la \ carattere ha un significato speciale solo se si utilizza un valore letterale stringa verbatim.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Per incorporare le virgolette doppie, usare un valore letterale stringa verbatim e ripetere le virgolette:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Ecco il risultato dell'uso di entrambi gli esempi in una pagina:

![Razor Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Si noti che la `@` carattere viene utilizzato per contrassegnare i valori letterali stringa verbatim in c# e per contrassegnare il codice nelle pagine ASP.NET.


### <a name="6-code-is-case-sensitive"></a>6. Codice viene fatta distinzione tra maiuscole e minuscole

In c#, le parole chiave (ad esempio `var`, `true`, e `if`) e i nomi delle variabili sono tra maiuscole e minuscole. Le righe di codice seguente creano due variabili diverse, `lastName` e `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Se si dichiara una variabile come `var lastName = "Smith";` e, se si tenta di fare riferimento a tale variabile nella stessa `@LastName`, viene generato un errore perché `LastName` non riconosciuto.

> [!NOTE]
> In Visual Basic, parole chiave e le variabili siano *non* distinzione maiuscole / minuscole.


### <a name="7-much-of-your-coding-involves-objects"></a>7. Gran parte del codice sono necessari oggetti

Un' *oggetti* rappresenta un elemento che è possibile programmare con &#8212; una pagina di una casella di testo, un file, un'immagine, una richiesta web, un messaggio di posta elettronica, un record del cliente (riga del database), e così via. Gli oggetti hanno proprietà che descrivono le caratteristiche e che è possibile leggere o modificare &#8212; dispone di un oggetto casella di testo una `Text` dispone di un oggetto della richiesta di proprietà (tra gli altri), un `Url` proprietà, un messaggio di posta elettronica ha una `From` proprietà, e un oggetto customer ha una `FirstName` proprietà. Oggetti contengono anche i metodi che sono le &quot;verbi&quot; possono eseguire. Un oggetto di file sono esempi `Save` metodo, dell'oggetto image `Rotate` metodo e un oggetto messaggio di posta elettronica `Send` (metodo).

Spesso si userà il `Request` dell'oggetto, che mostra informazioni quali i valori di caselle di testo (i campi del modulo) nella pagina, il tipo di browser ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via. Nell'esempio seguente viene illustrato come accedere alle proprietà del `Request` oggetto e come chiamare il `MapPath` metodo il `Request` oggetto, che fornisce il percorso assoluto della pagina nel server:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Il risultato visualizzato in un browser:

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. È possibile scrivere codice che prende decisioni

Una funzionalità chiave di pagine web dinamiche è che è possibile determinare quali operazioni eseguire in base alle condizioni. Il modo più comune per eseguire questa operazione è con il `if` istruzione (e facoltative `else` istruzione).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

L'istruzione `if(IsPost)` è un modo abbreviato per la scrittura `if(IsPost == true)`. Insieme a `if` istruzioni, esistono diversi modi per verificare le condizioni, ripetere i blocchi di codice, e così via, che sono descritte più avanti in questo articolo.

Il risultato visualizzato in un browser (dopo aver fatto clic **Submit**):

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET e i metodi POST e la proprietà istruzione IsPost
> 
> Il protocollo usato per le pagine web (HTTP) supporta un numero molto limitato di metodi (verbi) che consentono di effettuare richieste al server. Le due cause più comuni sono GET, che viene usato per leggere una pagina, e POST, che consente di inviare una pagina. In generale, la prima volta che un utente richiede una pagina, la pagina viene richiesta con GET. Se l'utente inserisce un form e quindi fa clic su un pulsante di invio, il browser invia una richiesta POST al server.
> 
> Nella programmazione web, è spesso utile sapere se una pagina viene richiesta come un'operazione GET o un POST in modo da sapere come elaborare la pagina. In ASP.NET Web Pages, è possibile usare il `IsPost` proprietà per vedere se una richiesta è un'operazione GET o POST. Se la richiesta viene pubblicato un POST, il `IsPost` proprietà restituirà true ed è possibile eseguire operazioni come leggere i valori di caselle di testo in un form. Molti esempi verrà visualizzato mostrano come elaborare la pagina in modo diverso a seconda del valore di `IsPost`.


## <a name="a-simple-code-example"></a>Un semplice esempio di codice

Questa procedura illustra come creare una pagina che illustra le tecniche di programmazione di base. Nell'esempio, si crea una pagina che consente agli utenti di immettere due numeri e quindi li aggiunge e viene visualizzato il risultato.

1. Nell'editor di creare un nuovo file e denominarlo *AddNumbers.cshtml*.
2. Copiare il codice e il markup seguente alla pagina, sostituendo qualsiasi elemento presente nella pagina.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Ecco alcuni aspetti notare:

    - Il `@` carattere viene avviato il primo blocco di codice nella pagina e precede la `totalMessage` variabile che viene incorporato nella parte inferiore della pagina.
    - Il blocco nella parte superiore della pagina è racchiuso tra parentesi graffe.
    - Nel blocco nella parte superiore, tutte le righe terminano con un punto e virgola.
    - Le variabili `total`, `num1`, `num2`, e `totalMessage` archiviare diversi numeri e una stringa.
    - Il valore di stringa letterale assegnato al `totalMessage` variabile è racchiuso tra virgolette doppie.
    - Poiché il codice è tra maiuscole e minuscole, quando il `totalMessage` variabile viene utilizzata nella parte inferiore della pagina, il relativo nome deve corrispondere esattamente alla variabile nella parte superiore.
    - L'espressione `num1.AsInt() + num2.AsInt()` viene illustrato come lavorare con gli oggetti e metodi. Il `AsInt` metodo su ogni variabile Converte la stringa immessa dall'utente a un numero (integer) in modo che sia possibile eseguire operazioni aritmetiche su di esso.
    - Il `<form>` tag include un `method="post"` attributo. Specifica che quando l'utente sceglie **Add**, la pagina verrà inviata al server usando il metodo HTTP POST. Quando viene inviata la pagina, il `if(IsPost)` test restituisce true e il parametro condizionale viene eseguito, visualizzare il risultato della somma i numeri di codice.
3. Salvare la pagina ed eseguirlo in un browser. (Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.) Immettere due numeri interi, quindi scegliere il **Add** pulsante. 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Concetti di programmazione di base

Questo articolo offre una panoramica della programmazione web ASP.NET. Non è un esame esaustivo, solo una presentazione rapida tramite i concetti di programmazione che si userà più spesso. Anche in questo caso, viene descritto come quasi tutto ciò che occorre per iniziare a usare le pagine Web con ASP.NET.

Ma innanzitutto, una breve introduzione tecnica.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>La sintassi Razor, il codice lato Server e ASP.NET

Sintassi Razor è una semplice sintassi di programmazione per l'incorporamento di codice basato su server in una pagina web. In una pagina web che usa la sintassi Razor, sono disponibili due tipi di contenuto: client del contenuto e codice lato server. Contenuto client sia le caratteristiche di cui si è abituati nelle pagine web: il markup HTML (elementi), le informazioni di stile, ad esempio CSS, forse alcuni script client, ad esempio JavaScript e testo normale.

Sintassi Razor consente di aggiungere il codice lato server per il contenuto di client. Se è presente codice server nella pagina, il server esegue prima di tutto che il codice prima di inviare la pagina nel browser. Tramite l'esecuzione nel server, il codice può eseguire attività che può essere molto più complessa da eseguire con i client a contenuti da solo, ad esempio l'accesso a database basati su server. Ancora più importante, il codice lato server può creare in modo dinamico contenuto client &#8212; può generare il markup HTML o altri contenuti in tempo reale e quindi inviarlo al browser e qualsiasi codice HTML statico che la pagina potrebbe contenere. Dal punto di vista del browser, client a contenuti che viene generato dal codice server non è diverso rispetto a qualsiasi altro contenuto client. Come abbiamo già visto, il codice lato server che ha richiesto è piuttosto semplice.

Pagine web ASP.NET che includono la sintassi Razor hanno un'estensione di file speciale (*. cshtml* oppure *vbhtml*). Il server riconosce queste estensioni, viene eseguito il codice che è contrassegnato con sintassi Razor e invia quindi la pagina nel browser.

### <a name="where-does-aspnet-fit-in"></a>Dove è ASP.NET adatta?

Sintassi Razor è basata su una tecnologia di Microsoft chiamata ASP.NET, che a sua volta è basato su Microsoft .NET Framework. .NET Framework è un framework di programmazione di big data, completa di Microsoft per lo sviluppo di praticamente qualsiasi tipo di applicazione per computer. ASP.NET è la parte di .NET Framework che è stata appositamente progettata per la creazione di applicazioni web. Gli sviluppatori hanno usato ASP.NET per creare molti dei siti Web più grandi e più elevato traffico nel mondo. (Qualsiasi ora noterete che l'estensione del nome file *aspx* come parte dell'URL in un sito, si saprà che il sito sia stato creato utilizzando ASP.NET.)

La sintassi Razor ti offre tutta la potenza di ASP.NET, ma usando una sintassi semplificata che è più facile apprendere se sei un principiante e che rende più produttiva se sei un esperto. Anche se questa sintassi è semplice da usare, la relazione della famiglia con ASP.NET e .NET Framework significa che, man mano che diventano più sofisticati i siti Web, è necessario la potenza dei framework più grande disponibile all'utente.

![Razor Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Classi e istanze**
> 
> Codice server ASP.NET utilizza oggetti, che a sua volta sono basati sul concetto di classi. La classe è la definizione o il modello per un oggetto. Ad esempio, un'applicazione potrebbe contenere un `Customer` classe che definisce le proprietà e metodi necessari per qualsiasi oggetto cliente.
> 
> Quando l'applicazione deve utilizzare informazioni reali dei clienti, crea un'istanza di (o *crea un'istanza*) un oggetto customer. Ogni singolo cliente è un'istanza separata del `Customer` classe. Ogni istanza supporta le stesse proprietà e metodi, ma i valori delle proprietà per ogni istanza sono in genere diversi, perché ogni oggetto customer è univoco. Nell'oggetto di un cliente, il `LastName` proprietà potrebbe essere "Smith"; in un altro oggetto customer, la `LastName` proprietà potrebbe essere "Jones".
> 
> In modo analogo, qualsiasi singola pagina web nel sito è un `Page` oggetto che rappresenta un'istanza di `Page` classe. Un pulsante nella pagina è una `Button` oggetto che rappresenta un'istanza del `Button` classe e così via. Ogni istanza ha caratteristiche, ma tutti si basano su quanto specificato nella definizione di classe dell'oggetto.


## <a name="basic-syntax"></a>Sintassi di base

In precedenza è stato illustrato un esempio su come creare una pagina ASP.NET Web Pages e come è possibile aggiungere il codice lato server per il markup HTML. Qui si apprenderanno i concetti fondamentali di scrittura di codice server ASP.NET tramite la sintassi Razor &#8212; , ovvero le regole del linguaggio di programmazione.

Se è esperti di programmazione (in particolare se è stato usato C, C++, c#, Visual Basic o JavaScript), gran parte delle quali è leggere qui risulteranno familiare. Si sarà probabilmente necessario acquisire familiarità con solo come codice lato server viene aggiunta al markup nel *cshtml* file.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Combinazione di testo, Markup e codice nei blocchi di codice

Nei blocchi di codice server, spesso necessario output testo o markup (o entrambi) alla pagina. Se un blocco di codice server contiene testo che non è codice e che invece deve essere eseguito il rendering è, ASP.NET deve essere in grado di distinguere tale testo dal codice. Esistono diversi modi per eseguire tale operazione.

- Racchiudere il testo in un elemento HTML, ad esempio `<p></p>` o `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    L'elemento HTML può includere testo, altri elementi HTML ed espressioni di codice lato server. Se ASP.NET rileva il tag HTML di apertura (ad esempio, `<p>`), viene eseguito il rendering di tutti gli elementi, compreso l'elemento e il contenuto come si trova nel browser, la risoluzione di espressioni di codice lato server durante la sua esecuzione.
- Usare la `@:` operatore o `<text>` elemento. Il `@:` restituisce una singola riga di contenuto che contengono testo normale o tag HTML senza corrispondenza; il `<text>` elemento racchiude più righe di output. Queste opzioni sono utili quando non si vuole eseguire il rendering di un elemento HTML come parte dell'output.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Se si desidera restituire più righe di testo o i tag HTML non corrispondenti, è possibile far precedere a ogni riga con `@:`, oppure è possibile includere la riga in un `<text>` elemento. Ad esempio la `@:` operatore`<text>` i tag vengono usati da ASP.NET per identificare il contenuto di testo e mai eseguito il rendering nell'output della pagina.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    Nel primo esempio viene ripetuta nell'esempio precedente ma usa una singola coppia di `<text>` tag per racchiudere il testo per il rendering. Nel secondo esempio, il `<text>` e `</text>` tag racchiudono tre righe, ognuna delle quali dispone di testo non contenuto e i tag HTML non corrispondenti (`<br />`), insieme a codice lato server e i tag HTML corrispondenti. Anche in questo caso, è possibile anche far precedere a ogni riga singolarmente con il `@:` operatore; entrambi funzionano modo.

    > [!NOTE]
    > Quando il testo è l'output come mostrato in questa sezione &#8212; utilizzando un elemento HTML, il `@:` operatore o il `<text>` elemento &#8212; ASP.NET non di codifica HTML di output. (Come indicato in precedenza, ASP.NET codificare l'output di espressioni di codice server e i blocchi di codice server preceduti da `@`, ad eccezione dei casi speciali elencati in questa sezione.)

### <a name="whitespace"></a>Whitespace

Gli spazi aggiuntivi in un'istruzione (e all'esterno di un valore letterale stringa) non influiscono sull'istruzione:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Un'interruzione di riga in un'istruzione non ha alcun effetto sull'istruzione e si possono eseguire il wrapping di istruzioni per migliorare la leggibilità. Le istruzioni seguenti sono gli stessi:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

È, tuttavia, non è possibile eseguire il wrapping di una riga all'interno di un valore letterale stringa. Nell'esempio seguente non funziona:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Per combinare una stringa lunga che esegue il wrapping su più righe, ad esempio il codice riportato sopra, sono disponibili due opzioni. È possibile usare l'operatore di concatenazione (`+`), si vedrà più avanti in questo articolo. È anche possibile usare il `@` carattere per creare una stringa verbatim letterale, come illustrato in precedenza in questo articolo. È possibile interrompere i valori letterali stringa verbatim su più righe:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Codice (e il Markup) commenti

Commenti è possibile lasciare note per se stessi o ad altri utenti. Consentono inoltre di disabilitare (*commento*) una sezione di codice o markup non intendessero gestire ma si desidera mantenere nella pagina per il momento.

È diversa impostazione come commento la sintassi per il codice Razor e per il markup HTML. Come con tutto il codice Razor, i commenti Razor vengono elaborati (e quindi rimosse) nel server prima che la pagina viene inviata al browser. Pertanto, la sintassi di commento Razor consente di inserire commenti nel codice (o anche nel markup) che è possibile visualizzare quando si modifica il file, ma che gli utenti non vedono, anche in dell'origine della pagina.

Per i commenti Razor di ASP.NET, iniziare il commento con `@*` e terminare con `*@`. Il commento non può essere su una o più righe:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Ecco un commento all'interno di un blocco di codice:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Qui è lo stesso blocco di codice, con la riga di codice commentato in modo che non verrà eseguito:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

All'interno di un blocco di codice, come alternativa all'uso di sintassi di commento Razor, è possibile usare la sintassi del linguaggio di programmazione che si utilizza, ad esempio c# commenti:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

In c#, i commenti a riga singola preceduti dal `//` caratteri e commenti su più righe iniziano con `/*` e terminare con `*/`. (Come per i commenti Razor, c# commenti non viene eseguiti nel browser.)

Per il markup, come è probabilmente noto, è possibile creare un commento HTML:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

HTML i commenti iniziano con `<!--` caratteri e terminano con `-->`. È possibile utilizzare i commenti HTML per racchiudere il testo non solo, ma anche eventuali markup HTML che può essere utile nella pagina, ma non si vuole eseguire il rendering. Questo commento HTML verrà nascondere l'intero contenuto del tag e il testo che contengono:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

A differenza dei commenti Razor, commenti HTML *sono* rendering sulla pagina e l'utente possa vederli visualizzando il sorgente della pagina.

Razor presenta alcune limitazioni sui blocchi annidati di c#. Per altre informazioni vedere [variabili c# denominato e annidati blocchi genera interrotto codice](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Variabili

Una variabile è un oggetto denominato che viene usato per archiviare i dati. È possibile assegnare variabili di qualsiasi oggetto, ma il nome deve iniziare con un carattere alfabetico e non può contenere spazi vuoti o caratteri riservati.

### <a name="variables-and-data-types"></a>Le variabili e tipi di dati

Una variabile può avere un tipo di dati specifico, che indica il tipo di dati viene archiviato nella variabile. È possibile avere le variabili di stringa che archiviano i valori stringa (ad esempio &quot;Hello world&quot;), le variabili integer che archiviano valori di numeri interi (ad esempio, 3 o 79) e le variabili di date che archiviano i valori delle date in un'ampia gamma di formati (ad esempio, marzo 2009 o 4/12/2012 ). Ed esistono molti altri tipi di dati che è possibile usare.

Tuttavia, in genere non è necessario specificare un tipo per una variabile. La maggior parte dei casi, ASP.NET può determinare il tipo di base sul modo in cui vengono usati i dati nella variabile. (In alcuni casi è necessario specificare un tipo; sono riportati esempi in cui questo è vero).

Si dichiara una variabile utilizzando la `var` (parola chiave) (se non si vuole specificare un tipo) o utilizzando il nome del tipo:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

Nell'esempio seguente mostra alcuni usi tipici delle variabili in una pagina web:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Se si combinano gli esempi precedenti in una pagina, vedere questi dati vengono visualizzati in un browser:

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>La conversione e i tipi di dati di test

Anche se ASP.NET in genere possibile determinare automaticamente un tipo di dati, a volte questo non è possibile. Pertanto, è necessario supporto ASP.NET mediante l'esecuzione di una conversione esplicita. Anche se non è necessario convertire i tipi, a volte è utile eseguire un test per vedere quali tipi di dati si lavora con.

Il caso più comune è che è necessario convertire una stringa in un altro tipo, ad esempio in un numero intero o Data. Nell'esempio seguente viene illustrato un caso tipico in cui è necessario convertire una stringa in un numero.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Di norma, input dell'utente vengono recapitati come stringhe. Anche se è stato richiesto agli utenti di immettere un numero e anche se è stato immesso una cifra, quando viene inviato l'input dell'utente e di leggerlo nel codice, i dati sono in formato stringa. Pertanto, è necessario convertire la stringa in un numero. Nell'esempio, se si prova a eseguire operazioni aritmetiche sui valori senza doverle convertire in, l'errore seguente generato, perché ASP.NET non è possibile aggiungere due stringhe:

*Impossibile convertire implicitamente il tipo 'string' a 'int'.*

Per convertire i valori interi, si chiama il `AsInt` (metodo). Se la conversione ha esito positivo, è quindi possibile aggiungere i numeri.

La tabella seguente elenca alcuni metodi di conversione e di test comuni per le variabili.

:::row:::
    :::column:::
        <strong>Metodo</strong>
    :::column-end:::
    :::column:::
        <strong>Descrizione</strong>
    :::column-end:::
    :::column:::
        <strong>Esempio</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Converte una stringa che rappresenta un numero intero (ad esempio, "593") in un numero intero.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        Converte una stringa like &quot;true&quot; oppure &quot;false&quot; a un tipo Boolean.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        Converte una stringa che contiene un valore decimale, ad esempio &quot;1.3&quot; oppure &quot;7.439&quot; su un numero a virgola mobile.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Converte una stringa che contiene un valore decimale, ad esempio &quot;1.3&quot; oppure &quot;7.439&quot; in un numero decimale. (In ASP.NET, un numero decimale è più preciso rispetto a un numero a virgola mobile). :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        Converte una stringa che rappresenta un valore di data e ora in ASP.NET `DateTime` tipo.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        Converte qualsiasi altro tipo di dati in una stringa.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Operatori

Un operatore è una parola chiave o un carattere che indica ad ASP.NET che tipo di comando da eseguire in un'espressione. Il linguaggio c# (e la sintassi Razor che si basa su di esso) supporta molti operatori, ma è sufficiente riconoscere alcune per iniziare. Nella tabella seguente sono riepilogati gli operatori più comuni.


:::row:::
    :::column:::
        <strong>Operator</strong>
    :::column-end:::
    :::column:::
        <strong>Descrizione</strong>
    :::column-end:::
    :::column:::
        <strong>Esempi</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
        Operatori matematici di utilizzati nelle espressioni numeriche.
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Assegnazione. Assegna il valore sul lato destro di un'istruzione per l'oggetto sul lato sinistro.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
        Uguaglianza. Restituisce `true` se i valori sono uguali. (Si noti che la distinzione tra i `=` operatore e il `==` operator.) :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
        Disuguaglianza. Restituisce `true` se i valori non sono uguali.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Meno-di, maggiore-di, minore di-than-or-equal e maggiore-than-or-equal.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
        Concatenazione, che consente di unire le stringhe. ASP.NET riconosce la differenza tra questo operatore e operatore dell'addizione in base al tipo di dati dell'espressione.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
        Gli operatori di incremento e decremento, quali addizioni e sottrazioni 1 (rispettivamente) da una variabile.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        Punto. Utilizzato per distinguere gli oggetti e le relative proprietà e metodi.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parentesi. Utilizzato per le espressioni di raggruppamento e passare i parametri a metodi.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
        Le parentesi quadre. Utilizzato per l'accesso ai valori nelle matrici o raccolte.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
        No. Inverte una `true` valore `false` e viceversa. In genere utilizzato come un modo abbreviato per testare `false` (vale a dire, per non `true`).
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&&` <code>&#124;&#124;</code>
    :::column-end:::
    :::column:::
        AND logico e o, che vengono utilizzati per collegare le condizioni.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Utilizzo di File e i percorsi delle cartelle nel codice

Sarà spesso lavorano con i percorsi di file e cartelle nel codice. Ecco un esempio di struttura di cartelle fisico per un sito Web come potrebbe apparire nel computer di sviluppo:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Ecco alcuni dettagli essenziali sui percorsi e gli URL:

- Un URL inizia con un nome di dominio (`http://www.example.com`) o un nome di server (`http://localhost`, `http://mycomputer`).
- Un URL corrisponde a un percorso fisico in un computer host. Ad esempio, `http://myserver` possono corrispondere alla cartella *C:\websites\mywebsite* nel server.
- Un percorso virtuale è a sintassi abbreviata per rappresentare i percorsi nel codice senza la necessità di specificare il percorso completo. Include la parte di un URL che segue il nome di dominio o server. Quando si usano i percorsi virtuali, è possibile spostare il codice a un dominio diverso o un server senza la necessità di aggiornare i percorsi.

Ecco un esempio che consentono di comprendere le differenze:

| URL completo | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nome del server | *mycompanyserver* |
| Percorso virtuale | */humanresources/CompanyPolicy.htm* |
| Percorso fisico | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

È la radice virtuale /, esattamente come la radice dell'unità c: è unità \. (I percorsi delle cartelle virtuali sempre usano le barre). Il percorso virtuale di una cartella non deve necessariamente avere lo stesso nome della cartella fisica; può trattarsi di un alias. (Nei server di produzione, il percorso virtuale raramente corrisponde a un percorso fisico esatto)

Quando si lavora con i file e cartelle nel codice, in alcuni casi è necessario fare riferimento al percorso fisico e in alcuni casi un percorso virtuale, a seconda di quali oggetti si lavora con. ASP.NET fornisce questi strumenti per l'uso di percorsi di file e cartelle nel codice: il `Server.MapPath` metodo e il `~` operatore e `Href` (metodo).

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>La conversione dei percorsi virtuali a fisici: il metodo server. MapPath

Il `Server.MapPath` metodo converte un percorso virtuale (ad esempio */default.cshtml*) in un percorso fisico assoluto (ad esempio *C:\WebSites\MyWebSiteFolder\default.cshtml*). Utilizzare questo metodo ogni volta che è necessario un percorso fisico completo. Un esempio tipico è quando si esegue la lettura o la scrittura di un file di testo o file di immagine nel server web.

In genere non conosce il percorso fisico assoluto del sito nel server del sito di hosting, in modo che questo metodo consente di convertire il percorso si conosce, ovvero il percorso virtuale, ovvero per il percorso corrispondente nel server per l'utente. Si passa il percorso virtuale in un file o cartella in cui il metodo e restituisce il percorso fisico:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>La radice virtuale di riferimento: il ~ operatore e il metodo di Href

In un *. cshtml* oppure *vbhtml* file, è possibile fare riferimento nel percorso radice virtuale usando il `~` operatore. Ciò è molto utile perché pagine è possibile spostarsi in un sito e tutti i collegamenti che ad altre pagine contengono non saranno interrotti. È anche utile nel caso in cui si sposta mai il sito Web in un percorso diverso. Ecco alcuni esempi:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Se il sito Web è `http://myserver/myapp`, ecco come ASP.NET tratterà questi percorsi quando viene eseguita la pagina:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Non viene effettivamente visualizzato questi percorsi come i valori della variabile, ma ASP.NET considererà i percorsi come se questo è ciò che fossero).

È possibile usare il `~` operatore nel codice server (come sopra) sia nel markup, simile al seguente:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

Nel markup, si utilizza il `~` operatore per creare percorsi di risorse, ad esempio i file di immagine, altre pagine web e file CSS. Quando viene eseguita la pagina, ASP.NET cerca tramite la pagina (codice e markup) e risolve tutti i `~` riferimenti al percorso appropriato.

## <a name="conditional-logic-and-loops"></a>I cicli e la logica condizionale

Codice server ASP.NET consente di eseguire attività in base alle condizioni e scrivere codice che si ripete istruzioni un numero specifico di volte in cui (vale a dire, codice che esegue un ciclo).

### <a name="testing-conditions"></a>Le condizioni di test

Per testare una condizione semplice è usare il `if` istruzione che restituisce true o false in base a un test è specificare:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

Il `if` (parola chiave) viene avviato un blocco. Il test effettivo (condizione) è racchiuso tra parentesi e restituisce true o false. Le istruzioni eseguite se il test è true sono racchiusi tra parentesi graffe. Un' `if` istruzione può includere un `else` blocco che consente di specificare istruzioni da eseguire se la condizione è false:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

È possibile aggiungere più condizioni usando un `else if` blocco:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

In questo esempio, se la prima condizione nella se non è impostato su true, blocca il `else if` condizione viene confrontata. Se tale condizione viene soddisfatta, le istruzioni di `else if` blocco vengono eseguite. Se nessuna delle condizioni viene soddisfatta, le istruzioni di `else` blocco vengono eseguite. È possibile aggiungere un numero qualsiasi di se invece si blocca e quindi chiudere con un `else` bloccare come il &quot;tutto il resto&quot; condizione.

Per testare un numero elevato di condizioni, usare un `switch` blocco:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Valore da testare è racchiuso tra parentesi (nell'esempio di `weekday` variabile). Ogni singolo test viene utilizzato un `case` istruzione che termina con due punti (:). Se il valore di un `case` istruzione corrisponde al valore di test, viene eseguito il codice in tale blocco case. Si chiude ogni istruzione case con un `break` istruzione. (Se si dimentica di includere l'interruzione in ognuno `case` blocca, il codice dal successivo `case` istruzione eseguirà inoltre.) Oggetto `switch` blocco ha spesso un `default` istruzione come ultimo caso per un &quot;tutto il resto&quot; opzione che viene eseguito se viene soddisfatta nessuna di altri casi.

Il risultato di ultimi due blocchi condizionali visualizzati in un browser:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Codice di ciclo

È spesso necessario eseguire ripetutamente le stesse istruzioni. Eseguire questa operazione dal ciclo. Ad esempio, si esegue spesso le stesse istruzioni per ogni elemento in una raccolta di dati. Se si conosce esattamente quante volte si desidera eseguire un ciclo, è possibile usare un `for` ciclo. Questo tipo di ciclo è particolarmente utile per il conteggio dei o il conteggio verso il basso:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

Il ciclo inizia con la `for` (parola chiave), seguita da tre istruzioni racchiuso tra parentesi, ognuna terminato con un punto e virgola.

- All'interno delle parentesi, la prima istruzione (`var i=10;`) viene creato un contatore e lo inizializza per 10. Non è necessario assegnare un nome del contatore `i` &#8212; è possibile utilizzare qualsiasi variabile. Quando il `for` ciclo viene eseguito, il contatore viene incrementato automaticamente.
- La seconda istruzione (`i < 21;`) imposta la condizione per fino a quando si desidera contare. In questo caso, si desidera che consente di passare a un massimo di 20 (vale a dire, continua a usare anche se il contatore è minore di 21).
- La terza istruzione (`i++` ) usa un operatore di incremento, che specifica semplicemente che il contatore dovrebbe avere 1 aggiunto ad esso ogni volta che viene eseguito il ciclo.

All'interno delle parentesi graffe è il codice che verrà eseguito in ogni iterazione del ciclo. Il codice crea un nuovo paragrafo (`<p>` elemento) ogni volta e si aggiunge una riga all'output, la visualizzazione del valore di `i` (il contatore). Quando si esegue questa pagina, l'esempio crea 11 righe visualizzando l'output, con il testo in ogni riga che indica il numero dell'elemento.

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

Se si lavora con una raccolta o una matrice, usano spesso un `foreach` ciclo. Una raccolta è un gruppo di oggetti simili e il `foreach` ciclo consente eseguire un'attività su ogni elemento nella raccolta. Questo tipo di ciclo è utile per le raccolte, perché a differenza di un `for` ciclo, non è necessario incrementare il contatore o impostare un limite. Al contrario, il `foreach` codice del ciclo procede semplicemente tramite la raccolta venga completata.

Ad esempio, il codice seguente restituisce gli elementi di `Request.ServerVariables` raccolta, che è un oggetto che contiene informazioni relative al server web. Usa un' `foreac` ciclo h per visualizzare il nome di ogni elemento creando un nuovo `<li>` elemento in un elenco puntato di HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

Il `foreach` parola chiave è seguita dalle parentesi in cui si dichiara una variabile che rappresenta un singolo elemento della raccolta (nell'esempio `var item`), seguito dal `in` (parola chiave), la raccolta che si desidera scorrere in ciclo. Nel corpo del `foreach` ciclo, è possibile accedere all'elemento corrente usando la variabile dichiarata in precedenza.

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Per creare un ciclo più generico, usare il `while` istruzione:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

Oggetto `while` ciclo inizia con il `while` (parola chiave), seguita dalle parentesi, in cui si specifica quanto tempo il ciclo continua (qui, come, purché `countNum` è minore di 50), quindi il blocco da ripetere. In genere incrementare i cicli (aggiungere) o di decremento (sottrarre) una variabile o un oggetto utilizzato per il conteggio. Nell'esempio, il `+=` operatore aggiunge 1 a `countNum` ogni volta che viene eseguito il ciclo. (Per decrementano una variabile in un ciclo che conta verso il basso, si utilizzerebbe l'operatore di decremento `-=`).

## <a name="objects-and-collections"></a>Oggetti e raccolte

Quasi tutti gli elementi in un sito Web ASP.NET è un oggetto, tra cui la pagina web stessa. Questa sezione illustra alcuni oggetti importante che si utilizzerà spesso nel codice.

### <a name="page-objects"></a>Oggetti della pagina

L'oggetto di base in ASP.NET è la pagina. Si può accedere alle proprietà dell'oggetto pagina direttamente, senza alcun oggetto qualificato. Il codice seguente ottiene il percorso di file della pagina, tramite il `Request` oggetto della pagina:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Per rendere chiaro che si sta che fanno riferimento a proprietà e metodi sull'oggetto pagina corrente, è anche possibile usare la parola chiave `this` per rappresentare l'oggetto pagina nel codice. Ecco un esempio di codice precedente, con `this` aggiunto per rappresentare la pagina:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

È possibile usare le proprietà del `Page` oggetto da ottenere molte informazioni, ad esempio:

- `Request`. Come abbiamo già visto, questa è una raccolta di informazioni sulla richiesta corrente, tra cui il tipo di browser ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via.
- `Response`. Si tratta di una raccolta di informazioni sulla risposta che verrà inviata al browser al termine dell'esecuzione il codice del server (pagina). Ad esempio, è possibile usare questa proprietà per scrivere informazioni nella risposta. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Oggetti della raccolta (matrici e dizionari)

Oggetto *collection* è un gruppo di oggetti dello stesso tipo, ad esempio una raccolta di `Customer` oggetti da un database. ASP.NET contiene molte raccolte predefinite, ad esempio il `Request.Files` raccolta.

Sarà spesso lavorano con i dati nelle raccolte. Due tipi di raccolte comuni sono le *matrice* e il *dizionario*. Una matrice è utile quando si vuole archiviare un insieme di elementi simili, ma non si vuole creare una variabile separata per contenere ciascun elemento:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Con le matrici, ad esempio si dichiara un tipo di dati specifico `string`, `int`, o `DateTime`. Per indicare che la variabile può contenere una matrice, è aggiungere le parentesi alla dichiarazione (ad esempio `string[]` o `int[]`). È possibile accedere agli elementi in una matrice usando la loro posizione (indice) o tramite il `foreach` istruzione. Gli indici di matrice sono in base zero in &#8212; , ovvero il primo elemento è nella posizione 0, il secondo elemento alla posizione 1 e così via.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

È possibile determinare il numero di elementi in una matrice tramite il recupero relativo `Length` proprietà. Per ottenere la posizione di un elemento specifico nella matrice (la ricerca nella matrice), usare il `Array.IndexOf` (metodo). È anche possibile eseguire operazioni come reverse il contenuto di una matrice (il `Array.Reverse` metodo) oppure ordinare il contenuto (il `Array.Sort` (metodo)).

L'output del codice di matrice di stringa visualizzato in un browser:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Un dizionario è una raccolta di coppie chiave/valore, in cui è fornire la chiave (o nome) per impostare o recuperare il valore corrispondente:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Per creare un dizionario, usare il `new` (parola chiave) per indicare che si sta creando un nuovo oggetto dizionario. È possibile assegnare un dizionario a una variabile utilizzando la `var` (parola chiave). Si indicano i tipi di dati degli elementi nel dizionario tramite parentesi angolari ( `< >` ). Alla fine della dichiarazione, è necessario aggiungere una coppia di parentesi, perché si tratta in realtà un metodo che crea un nuovo dizionario.

Per aggiungere elementi al dizionario, è possibile chiamare il `Add` metodo per la variabile del dizionario (`myScores` in questo caso), quindi specificare una chiave e un valore. In alternativa, è possibile utilizzare le parentesi quadre indicano la chiave ed eseguire una semplice assegnazione, come nell'esempio seguente:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Per ottenere un valore dal dizionario, si specifica la chiave tra parentesi quadre:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Chiamata di metodi con parametri

Come leggere più indietro in questo articolo, gli oggetti che si programma con possono presentare metodi. Ad esempio, un `Database` oggetto potrebbe avere un `Database.Connect` (metodo). Molti metodi dispongono anche di uno o più parametri. Oggetto *parametro* è un valore che si passa a un metodo per abilitare il metodo completare l'attività. Ad esempio, esaminata una dichiarazione per il `Request.MapPath` metodo che accetta tre parametri:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(La riga è stato eseguito il wrapping per renderlo più leggibile. Tenere presente che è possibile inserire interruzioni di riga quasi in qualsiasi luogo ad eccezione del fatto all'interno di stringhe che sono racchiusi tra virgolette doppie.)

Questo metodo restituisce il percorso fisico sul server che corrisponde a un percorso virtuale specificato. I tre parametri per il metodo vengono `virtualPath`, `baseVirtualDir`, e `allowCrossAppMapping`. Si noti che nella dichiarazione, i parametri sono elencati con i tipi di dati dei dati che verranno accettano. Quando si chiama questo metodo, è necessario fornire valori per tutti i tre parametri.

La sintassi Razor offre due opzioni per il passaggio di parametri a un metodo: *parametri posizionali* e *parametri denominati*. Per chiamare un metodo usando i parametri posizionali, passare i parametri in un ordine fisso specificato nella dichiarazione del metodo. (È in genere saprebbe quest'ordine leggendo la documentazione relativa al metodo.) È necessario seguire l'ordine e non è possibile ignorare i parametri &#8212; se necessario, si passa una stringa vuota (`""`) o `null` per un parametro posizionale che non si dispone di un valore per.

Nell'esempio seguente si presuppone una cartella denominata *script* nel tuo sito Web. Il codice chiama il `Request.MapPath` metodo e passa i valori per i tre parametri nell'ordine corretto. Viene quindi visualizzato il percorso risulta mappato.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Quando un metodo ha molti parametri, è possibile mantenere il codice più leggibile utilizzando parametri denominati. Per chiamare un metodo utilizzando parametri denominati, si specifica il nome del parametro seguito da due punti (:) e quindi il valore. Il vantaggio di parametri denominati è che è possibile passarli in qualsiasi ordine desiderato. (Uno svantaggio è che la chiamata al metodo non è compresso).

Nell'esempio seguente chiama il metodo di stesso come illustrato in precedenza, ma utilizza parametri di fornire i valori denominati:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Come può notare, i parametri vengono passati in un ordine diverso. Tuttavia, se si esegue l'esempio precedente e in questo esempio, verrà restituiscono lo stesso valore.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Gestione degli errori

### <a name="try-catch-statements"></a>Istruzioni Try-Catch

È necessario spesso istruzioni nel codice che potrebbe non riuscire per motivi di all'esterno del controllo. Ad esempio:

- Se il codice tenta di creare o accedere a un file, tutti i tipi di errori potrebbero verificarsi. Il file desiderato potrebbe non esistere, potrebbe essere stato bloccato, il codice potrebbe non disporre delle autorizzazioni e così via.
- Analogamente, se il codice tenta di aggiornare i record in un database, possono essere presenti problemi relativi alle autorizzazioni, la connessione al database potrebbe essere eliminata, i dati da salvare potrebbero essere non valido e così via.

In termini di programmazione, queste situazioni sono denominate *eccezioni*. Se il codice rileva un'eccezione, viene generato (genera) un messaggio di errore di, nella migliore delle ipotesi, indesiderate agli utenti:

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

In situazioni in cui il codice che si verifichino eccezioni e per evitare messaggi di errore di questo tipo, è possibile usare `try/catch` istruzioni. Nel `try` istruzione, eseguire il codice che si sta archiviando. In uno o più `catch` (istruzioni), è possibile cercare specifici errori (tipi specifici di eccezioni) che sono stati generati. È possibile includere un numero `catch` istruzioni è necessario individuare errori che si prevede di usare.

> [!NOTE]
> È consigliabile evitare di utilizzare il `Response.Redirect` metodo `try/catch` (istruzioni), poiché potrebbe causare un'eccezione nella pagina.


Nell'esempio seguente mostra una pagina che consente di creare un file di testo per la prima richiesta e quindi visualizza un pulsante che consente all'utente di aprire il file. L'esempio Usa un nome file errato deliberatamente in modo che lo genererà un'eccezione. Il codice riporta `catch` istruzioni per due possibili eccezioni: `FileNotFoundException`, che si verifica se il nome del file non è corretto, e `DirectoryNotFoundException`, che si verifica se ASP.NET non è possibile anche trovare la cartella. (È possibile rimuovere il commento nell'esempio, un'istruzione per verificarne l'esecuzione quando tutto funziona correttamente.)

Se il codice non gestisce l'eccezione, si vedrà una pagina di errore, ad esempio lo screenshot precedente. Tuttavia, il `try/catch` sezione aiuta a impedire all'utente di visualizzare questi tipi di errori.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

**Programmazione con Visual Basic**


[Appendice: linguaggio Visual Basic e sintassi](https://go.microsoft.com/fwlink/?LinkId=202908)


**Documentazione di riferimento**


[ASP.NET 2.0](https://msdn.microsoft.com/library/ee532866.aspx)

[Linguaggio c#](https://msdn.microsoft.com/library/kx37x362.aspx)
