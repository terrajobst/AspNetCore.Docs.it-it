---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor (c#) | Documenti Microsoft
author: tfitzmac
description: "In questo capitolo offre una panoramica della programmazione con ASP.NET Web Pages con sintassi Razor. ASP.NET è la tecnologia Microsoft per l'esecuzione di indirizzo pa web dinamico..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: f054d574026ab6444cc59a126ef9dcdc323f7bff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor (c#)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo offre una panoramica della programmazione con ASP.NET Web Pages con sintassi Razor. ASP.NET è la tecnologia Microsoft per l'esecuzione dinamica di pagine web nei server web. Questo articolo è incentrata sulla utilizzando il linguaggio di programmazione c#.
> 
> **Si apprenderà**:
> 
> - Il principale 8 suggerimenti per iniziare a usare ASP.NET Web Pages con sintassi Razor di programmazione di programmazione.
> - Concetti di programmazione di base che è necessario.
> - Il codice server ASP.NET e la sintassi Razor è dedicato.
>   
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


## <a name="the-top-8-programming-tips"></a>I suggerimenti di programmazione 8

In questa sezione sono elencati alcuni suggerimenti che è assolutamente necessario conoscere quando si inizia la scrittura di codice server ASP.NET utilizzando la sintassi Razor.

> [!NOTE]
> La sintassi Razor è basata sul linguaggio di programmazione c#, e che è il linguaggio che viene spesso utilizzato con ASP.NET Web Pages. Tuttavia, la sintassi Razor supporta il linguaggio Visual Basic e qualsiasi elemento che è inoltre possibile eseguire in Visual Basic. Per informazioni dettagliate, vedere l'appendice [linguaggio Visual Basic e la sintassi](https://go.microsoft.com/fwlink/?LinkId=202908).


È possibile trovare altre informazioni sulla maggior parte di queste tecniche di programmazione avanti in questo articolo.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Aggiungere codice a una pagina utilizzando il carattere @

Il `@` carattere venga espressioni inline, blocchi di singola istruzione e blocchi di istruzioni:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Si tratta di queste istruzioni aspetto analogo al seguente quando la pagina viene eseguita in un browser:

![Razor Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **La codifica HTML**
> 
> Quando si visualizza il contenuto in una pagina utilizzando il `@` carattere, come negli esempi precedenti, ASP.NET codifica in HTML l'output. Ciò consente di sostituire caratteri HTML riservati (ad esempio `<` e `>` e `&`) con i codici di abilitare i caratteri da visualizzare come caratteri in una pagina web anziché essere interpretato come tag HTML o le entità. Senza codifica HTML, l'output dal codice server non vengano visualizzati correttamente e potrebbe esporre una pagina a rischi di sicurezza.
> 
> Se l'obiettivo consiste nel markup HTML che esegue il rendering di tag di markup di output (ad esempio `<p></p>` per un paragrafo o `<em></em>` per evidenziare il testo), vedere la sezione [la combinazione di testo, Markup e codice nei blocchi di codice](#BM_CombiningTextMarkupAndCode) più avanti in questo articolo.
> 
> Altre informazioni sulla codifica HTML [utilizzo dei moduli](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Blocchi di codice racchiudere tra parentesi graffe

Oggetto *blocco di codice* include uno o più istruzioni del codice e viene racchiuso tra parentesi graffe.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Il risultato visualizzato in un browser:

![Razor Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. All'interno di un blocco, termine di ogni istruzione del codice con un punto e virgola

All'interno di un blocco di codice, ogni istruzione di codice completo deve terminare con un punto e virgola. Le espressioni inline non terminano con un punto e virgola.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Utilizzare le variabili per archiviare i valori

È possibile archiviare valori in un *variabile*, incluse le stringhe, numeri e date, e così via. Si crea una nuova variabile utilizzando il `var` (parola chiave). È possibile inserire i valori delle variabili direttamente in una pagina utilizzando `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Il risultato visualizzato in un browser:

![Razor Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Racchiudere i valori letterali stringa tra virgolette doppie

Oggetto *stringa* è una sequenza di caratteri che vengono considerati come testo. Per specificare una stringa, è racchiuderlo tra virgolette doppie:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Se la stringa che si desidera visualizzare contiene un carattere barra rovesciata ( `\` ) o virgolette doppie ( `"` ), utilizzare un *stringa letterale verbatim* con prefisso con il `@` operatore. (In c# il \ carattere ha un significato speciale solo se si utilizza un valore letterale di stringa verbatim.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Per incorporare le virgolette doppie, usare un valore letterale di stringa verbatim e ripetere le virgolette:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Di seguito è riportato il risultato dell'utilizzo di questi esempi in una pagina:

![Razor Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Si noti che il `@` carattere viene utilizzato per contrassegnare i valori letterali di stringa verbatim in c# e per contrassegnare il codice in pagine ASP.NET.


### <a name="6-code-is-case-sensitive"></a>6. Codice viene fatta distinzione tra maiuscole e minuscole

In c#, parole chiave (ad esempio `var`, `true`, e `if`) e i nomi delle variabili viene fatta distinzione tra maiuscole e minuscole. Le righe di codice seguente creano due variabili diverse, `lastName` e`LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Se si dichiara una variabile come `var lastName = "Smith";` e se si tenta di fare riferimento a tale variabile nella pagina come `@LastName`, viene generato un errore perché `LastName` non riconosciuto.

> [!NOTE]
> In Visual Basic, parole chiave e le variabili sono *non* tra maiuscole e minuscole.


### <a name="7-much-of-your-coding-involves-objects"></a>7. L'attività di codifica riguarda principalmente la oggetti

Un *oggetto* rappresenta un elemento che è possibile programmare con &#8212; una pagina di una casella di testo, un file, un'immagine, una richiesta web, un messaggio di posta elettronica, un record del cliente (riga), e così via. Gli oggetti hanno proprietà che descrivono le caratteristiche e che è possibile leggere o modificare &#8212; è un oggetto di casella di testo un `Text` proprietà (tra gli altri), un oggetto di richiesta ha un `Url` proprietà, un messaggio di posta elettronica ha un `From` proprietà e un oggetto customer ha una `FirstName` proprietà. Gli oggetti includono anche metodi che sono il &quot;verbi&quot; possono eseguire. Esempi includono un oggetto di file `Save` (metodo), l'oggetto image `Rotate` (metodo) e un oggetto di posta elettronica `Send` metodo.

Spesso si utilizzerà il `Request` dell'oggetto, che fornisce informazioni quali i valori di caselle di testo (campi modulo) nella pagina, il tipo di browser ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente, e così via. Nell'esempio seguente viene illustrato come accedere alle proprietà del `Request` oggetto e come chiamare il `MapPath` metodo il `Request` oggetto, che fornisce il percorso assoluto della pagina nel server:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Il risultato visualizzato in un browser:

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. È possibile scrivere codice che prende decisioni

Una funzionalità chiave di pagine web dinamiche è possibile determinare quali operazioni eseguire in base alle condizioni. È il modo più comune per eseguire questa operazione con il `if` istruzione (e facoltativi `else` istruzione).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

L'istruzione `if(IsPost)` è un modo abbreviato di scrittura `if(IsPost == true)`. Insieme a `if` istruzioni, esistono diversi modi per verificare le condizioni, ripetere i blocchi di codice, e così via, che sono descritte più avanti in questo articolo.

Il risultato visualizzato in un browser (dopo aver fatto clic **Invia**):

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET e POST metodi e proprietà istruzione IsPost
> 
> Il protocollo utilizzato per le pagine web (HTTP) supporta un numero molto limitato di metodi (verbi) che vengono utilizzati per effettuare richieste al server. Le due cause più comuni sono GET, viene utilizzato per leggere una pagina, e POST, viene utilizzato per inviare una pagina. In generale, la prima volta che un utente richiede una pagina, la pagina viene richiesta con GET. Se l'utente è un modulo compilato e quindi fa clic su un pulsante di invio, il browser effettua una richiesta POST al server.
> 
> Nella programmazione web, è spesso utile sapere se una pagina viene richiesta come un'operazione GET o POST in modo da sapere come elaborare la pagina. In ASP.NET Web Pages, è possibile utilizzare il `IsPost` proprietà per verificare se una richiesta è un'operazione GET o POST. Se la richiesta è una richiesta POST, il `IsPost` restituirà true e consente di eseguire operazioni come read i valori di caselle di testo in un form. Molti esempi vedrai mostrano come elaborare la pagina in modo diverso a seconda del valore di `IsPost`.


## <a name="a-simple-code-example"></a>Un semplice esempio di codice

In questa procedura viene illustrato come creare una pagina che illustra le tecniche di programmazione di base. Nell'esempio si crea una pagina che consente agli utenti di immettere due numeri interi, quindi li aggiunge e viene visualizzato il risultato.

1. Nell'editor, creare un nuovo file e denominarlo *AddNumbers.cshtml*.
2. Copiare il codice e markup seguenti nella pagina, sostituendo qualsiasi elemento presente nella pagina.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Ecco alcune operazioni per cui è possibile notare:

    - Il `@` carattere inizia il primo blocco di codice nella pagina e precede il `totalMessage` variabile che è incorporato nella parte inferiore della pagina.
    - Il blocco nella parte superiore della pagina è racchiuso tra parentesi graffe.
    - Nel blocco nella parte superiore, tutte le righe di terminano con un punto e virgola.
    - Le variabili `total`, `num1`, `num2`, e `totalMessage` archiviare numeri diversi e una stringa.
    - Il valore di stringa letterale assegnato per il `totalMessage` variabile è racchiuso tra virgolette.
    - Poiché il codice è tra maiuscole e minuscole, quando il `totalMessage` variabile viene utilizzata nella parte inferiore della pagina, il nome deve corrispondere esattamente alla variabile nella parte superiore.
    - L'espressione `num1.AsInt() + num2.AsInt()` viene illustrato l'utilizzo con gli oggetti e metodi. Il `AsInt` metodo su ogni variabile Converte la stringa immessa dall'utente per un numero (integer) in modo che sia possibile eseguire operazioni aritmetiche su di esso.
    - Il `<form>` tag include un `method="post"` attributo. Specifica che quando l'utente fa clic **Aggiungi**, la pagina verrà inviata al server utilizzando il metodo HTTP POST. Quando viene inviata la pagina, il `if(IsPost)` test restituisce true e condizionale viene eseguito, Visualizza il risultato dell'aggiunta di numeri di codice.
3. Salvare la pagina ed eseguirlo in un browser. (Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.) Immettere due numeri interi, quindi scegliere il **Aggiungi** pulsante. 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Concetti relativi alla programmazione di base

In questo articolo fornisce una panoramica della programmazione web ASP.NET. Non è un esame completo, solo una breve panoramica tramite i concetti di programmazione in genere si utilizzerà. Viene illustrato anche in questo caso, quasi tutto ciò che occorre per iniziare con ASP.NET Web Pages.

Ma prima, poco competenze tecniche di base.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>La sintassi Razor, codice Server e ASP.NET

La sintassi Razor è un semplice programmazione per l'incorporamento di codice basato su server in una pagina web. In una pagina web che utilizza la sintassi Razor, sono disponibili due tipi di contenuto: il codice client contenuto e il server. Il contenuto di client è il contenuto a cui si è abituati nelle pagine web: il markup HTML (elementi), le informazioni di stile, ad esempio CSS, forse alcuni script client, ad esempio, JavaScript e testo normale.

La sintassi Razor consente di aggiungere codice lato server per il contenuto di questo client. Se la pagina è presente codice lato server, il server esegue in primo luogo, il codice prima dell'invio della pagina nel browser. In esecuzione nel server, il codice consente di eseguire attività che possono essere molto più complessa da eseguire usando i client a contenuti da solo, ad esempio l'accesso a database basati su server. In particolare, il codice lato server consentono di creare dinamicamente client contenuto &#8212; può generare il markup HTML o altro contenuto in tempo reale e quindi inviarlo al browser insieme a qualsiasi codice HTML statico che la pagina può contenere. Dal punto di vista del browser, client a contenuti che viene generato dal codice server non è diversa rispetto a qualsiasi altro contenuto di client. Come già visto, il codice server che ha richiesto è piuttosto semplice.

Pagine web ASP.NET che includono la sintassi Razor hanno un'estensione di file speciale (*. cshtml* o *. vbhtml*). Il server riconosce queste estensioni, viene eseguito il codice che è contrassegnato con sintassi Razor e invia quindi la pagina nel browser.

### <a name="where-does-aspnet-fit-in"></a>Possibilità di utilizzo ASP.NET

La sintassi Razor è basata su una tecnologia Microsoft chiamato ASP.NET, che a sua volta è basato su Microsoft .NET Framework. .NET Framework è un framework di programmazione grande e completo da Microsoft per lo sviluppo di qualsiasi tipo di applicazione del computer. ASP.NET è la parte di .NET Framework è progettato specificamente per la creazione di applicazioni web. Gli sviluppatori hanno utilizzato ASP.NET per creare molti dei siti Web più grandi e più elevato traffico nel mondo. (Ogni volta che viene visualizzato l'estensione del nome di file *aspx* come parte dell'URL in un sito, si saprà che il sito sia stato creato mediante ASP.NET.)

La sintassi Razor consente tutte le potenzialità di ASP.NET, ma utilizzando una sintassi semplificata che è più facile apprendere se principianti e che consente di aumentare la produttività se si è esperti. Anche se questa sintassi è semplice da usare, la relazione della famiglia con ASP.NET e .NET Framework significa che quando i siti Web sono più sofisticati, la potenza dei framework più grande disponibile.

![Razor Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Classi e istanze**
> 
> Il codice server ASP.NET utilizza gli oggetti, che a sua volta compilati sul concetto di classi. La classe è la definizione o il modello per un oggetto. Ad esempio, un'applicazione potrebbe contenere un `Customer` classe che definisce le proprietà e metodi necessari per qualsiasi oggetto customer.
> 
> Quando l'applicazione deve utilizzare le informazioni dei clienti, crea un'istanza di (o *crea*) un oggetto customer. Ogni singolo cliente è un'istanza separata del `Customer` classe. Ogni istanza supporta le stesse proprietà e metodi, ma i valori delle proprietà per ogni istanza sono in genere diversi, perché ogni oggetto customer è univoco. Nell'oggetto di un cliente, il `LastName` proprietà potrebbe essere "Smith"; in un altro oggetto customer, la `LastName` proprietà potrebbe essere "Jones".
> 
> Analogamente, qualsiasi singola pagina web nel sito è un `Page` oggetto che rappresenta un'istanza di `Page` classe. È un pulsante nella pagina un `Button` oggetto che rappresenta un'istanza di `Button` classe e così via. Ogni istanza ha caratteristiche, ma tutti sono basate su quello specificato nella definizione di classe dell'oggetto.


## <a name="basic-syntax"></a>Sintassi di base

In precedenza è stato illustrato un esempio su come creare una pagina ASP.NET Web Pages e come è possibile aggiungere codice lato server per il markup HTML. Questo articolo si apprenderà le nozioni di base della scrittura del codice server ASP.NET tramite la sintassi Razor &#8212; vale a dire le regole del linguaggio di programmazione.

Se si è esperti di programmazione (in particolare se si utilizza C, C++, c#, Visual Basic o JavaScript), gran parte delle quali leggere risulteranno familiare. Sarà probabilmente necessario acquisire familiarità con solo il codice lato server viene aggiunto al markup in *. cshtml* file.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Combinazione di testo, Markup e codice nei blocchi di codice

Nei blocchi di codice server, spesso necessario output testo o markup (o entrambe) alla pagina. Se un blocco di codice server contiene testo che non è codice e che invece deve essere eseguito il rendering è, ASP.NET deve essere in grado di distinguere tale testo dal codice. Esistono diversi modi per eseguire tale operazione.

- Racchiudere il testo in un elemento HTML come `<p></p>` o `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    L'elemento HTML può includere testo, elementi HTML aggiuntivi e le espressioni di codice server. Quando ASP.NET visualizza il tag HTML di apertura (ad esempio, `<p>`), viene eseguito il rendering di tutto, incluso l'elemento e il contenuto come si trova nel browser, la risoluzione di espressioni di codice lato server durante la sua esecuzione.
- Utilizzare il `@:` operatore o `<text>` elemento. Il `@:` restituisce una singola riga di contenuto che contengono testo normale o tag HTML senza corrispondenza; il `<text>` elemento include più righe di output. Queste opzioni sono utili quando non si desidera eseguire il rendering di un elemento HTML come parte dell'output.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Se si desidera restituire più righe di testo o i tag HTML non corrispondenti, è possibile far precedere a ogni riga con `@:`, oppure è possibile includere la riga in un `<text>` elemento. Ad esempio il `@:` operatore`<text>` tag sono utilizzate da ASP.NET per identificare il contenuto di testo e mai eseguito il rendering nell'output della pagina.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    Nel primo esempio si ripete l'esempio precedente, ma utilizza una singola coppia di `<text>` tag per racchiudere il testo per il rendering. Nel secondo esempio, il `<text>` e `</text>` tag racchiudono tre righe, che hanno tutti il testo non indipendente e senza tag HTML (`<br />`), insieme a codice server e i tag HTML corrispondenti. Nuovamente, è impossibile anche far precedere a ogni riga singolarmente con il `@:` operatore; entrambi funzionano modo.

    > [!NOTE]
    > Quando si output di testo come illustrato in questa sezione &#8212; utilizzando un elemento HTML, il `@:` operatore o `<text>` elemento &#8212; ASP.NET non codifica HTML l'output. (Come indicato in precedenza, ASP.NET codificare l'output di espressioni di codice server e i blocchi di codice server che sono preceduti da `@`, ad eccezione dei casi speciali riportati in questa sezione.)

### <a name="whitespace"></a>Whitespace

Spazi aggiuntivi in un'istruzione (e all'esterno di un valore letterale stringa) non influiscono sull'istruzione:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Un'interruzione di riga in un'istruzione non ha alcun effetto sull'istruzione ed è possibile eseguire il wrapping di istruzioni per migliorare la leggibilità. Le istruzioni seguenti sono uguali:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Tuttavia, non è possibile disporre di una riga all'interno di un valore letterale stringa. Non funziona nell'esempio seguente:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Per combinare una stringa lunga che esegue il wrapping di più righe nel codice precedente, sono disponibili due opzioni. È possibile utilizzare l'operatore di concatenazione (`+`), che verrà illustrato più avanti in questo articolo. È inoltre possibile utilizzare il `@` carattere per creare una stringa verbatim come valore letterale, come illustrato in precedenza in questo articolo. È possibile interrompere i valori letterali di stringa verbatim su più righe:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Codice (e Markup) commenti

Commenti è possibile lasciare le note per se stessi o ad altri utenti. Consentono inoltre di disabilitare (*commento*) una sezione di codice o markup che non si desidera eseguire, ma desidera mantenere nella pagina per il momento.

È diverso commenti sintassi del codice Razor e per il markup HTML. Come con tutto il codice Razor, commenti Razor vengono elaborati (e quindi rimosse) nel server prima che la pagina venga inviata al browser. Pertanto, la sintassi di commento Razor consente di inserire i commenti nel codice (o anche nel markup) che è possibile vedere quando si modifica il file, ma che gli utenti non viene visualizzata, anche nell'origine della pagina.

Per i commenti ASP.NET Razor, iniziare il commento con `@*` e terminare con `*@`. Il commento non può essere su una o più righe:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Di seguito è riportato un commento all'interno di un blocco di codice:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Qui è lo stesso blocco di codice, con la riga di codice commentato in modo che non verrà eseguito:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

All'interno di un blocco di codice, come alternativa all'utilizzo di sintassi di commento Razor, è possibile utilizzare la sintassi di commenta del linguaggio di programmazione che si utilizza, ad esempio in c#:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

In c#, i commenti a riga singola sono preceduti dal `//` caratteri e i commenti su più righe iniziano con `/*` e terminare con `*/`. (Come per i commenti Razor, commenti c# non viene eseguiti nel browser.)

Per il markup, come è noto, è possibile creare un commento HTML:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Commenti HTML iniziano con `<!--` caratteri e terminano con `-->`. È possibile utilizzare i commenti HTML per racchiudere il testo non solo, ma anche qualsiasi tag HTML che si desidera mantenere nella pagina, ma non si desidera eseguire il rendering. Questo commento HTML nasconderà l'intero contenuto dei tag e il testo che contengono:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

A differenza dei commenti Razor, commenti HTML *sono* eseguito il rendering della pagina e l'utente può visualizzare la visualizzazione dell'origine della pagina.

Razor presenta alcune limitazioni sui blocchi annidati di c#. Per ulteriori informazioni vedere [variabili c# denominata e annidati blocchi genera suddiviso codice](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Variabili

Una variabile è un oggetto denominato che consente di archiviare i dati. È possibile assegnare variabili di qualsiasi tipo, ma il nome deve iniziare con un carattere alfabetico e non può contenere spazi o caratteri riservati.

### <a name="variables-and-data-types"></a>Le variabili e tipi di dati

Una variabile può avere un tipo di dati specifici, che indica il tipo di dati è archiviato nella variabile. È possibile disporre di variabili di stringa che archiviano i valori stringa (ad esempio &quot;Hello world&quot;), le variabili integer che archiviano i valori numero intero (ad esempio, 3 o 79) e le variabili di date che archiviano i valori di data in una varietà di formati (ad esempio, 12/4/2012 o marzo 2009 ). E sono disponibili molti altri tipi di dati che è possibile utilizzare.

Tuttavia, in genere non è necessario specificare un tipo per una variabile. La maggior parte dei casi, ASP.NET può determinare il tipo di base sull'utilizzo di dati nella variabile. (In alcuni casi è necessario specificare un tipo; visualizzato esempi in cui questo è true).

Consente di dichiarare una variabile utilizzando il `var` (parola chiave) (se non si desidera specificare un tipo) o utilizzando il nome del tipo:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

L'esempio seguente mostra alcuni utilizzi tipici di variabili in una pagina web:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Se si combinano gli esempi precedenti in una pagina, viene visualizzato questo visualizzati in un browser:

![Razor Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>La conversione e i tipi di dati di test

Anche se ASP.NET in genere può determinare automaticamente un tipo di dati, in alcuni casi, non può essere Di conseguenza, potrebbe essere necessario aiutare ASP.NET eseguendo una conversione esplicita. Anche se non è necessario convertire i tipi, a volte è utile verificare il tipo di dati se si lavora con.

Il caso più comune è che è necessario convertire una stringa in un altro tipo, ad esempio in un numero intero o una data. Nell'esempio seguente viene illustrato un caso tipico in cui è necessario convertire una stringa in un numero.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Come regola, l'input dell'utente vengono recapitati come stringhe. Anche se hai richiesto agli utenti di immettere un numero e anche se è stato immesso una cifra, quando viene inviato l'input dell'utente e viene letta nel codice, i dati sono in formato stringa. Pertanto, è necessario convertire la stringa a un numero. Nell'esempio, se si tenta di eseguire operazioni aritmetiche su valori senza convertirli, l'errore seguente viene generato, perché ASP.NET non è possibile aggiungere due stringhe:

*Impossibile convertire in modo implicito il tipo 'string' a 'int'.*

Per convertire i valori in numeri interi, si chiama il `AsInt` metodo. Se la conversione ha esito positivo, è quindi possibile aggiungere i numeri.

Nella tabella seguente sono elencati alcuni metodi di conversione e di test comuni per le variabili.

| **Metodo** | **Descrizione** | **Esempio** |
| --- | --- | --- |
| `AsInt(), IsInt()` | Converte una stringa che rappresenta un numero intero (ad esempio "593") in un intero. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)] |
| `AsBool(), IsBool()` | Converte una stringa come &quot;true&quot; o &quot;false&quot; in un tipo Boolean. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)] |
| `AsFloat(), IsFloat()` | Converte una stringa che contiene un valore decimale come &quot;1.3&quot; o &quot;7.439&quot; un numero a virgola mobile. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)] |
| `AsDecimal(), IsDecimal()` | Converte una stringa che contiene un valore decimale come &quot;1.3&quot; o &quot;7.439&quot; in un numero decimale. (In ASP.NET, un numero decimale è più preciso rispetto a un numero a virgola mobile). | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)] |
| `AsDateTime(), IsDateTime()` | Converte una stringa che rappresenta un valore di data e ora per ASP.NET `DateTime` tipo. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)] |
| `ToString()` | Converte qualsiasi altro tipo di dati in una stringa. | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)] |

## <a name="operators"></a>Operatori

Un operatore è una parola chiave o un carattere che indica il tipo di comando da eseguire in un'espressione di ASP.NET. Linguaggio c# (e la sintassi Razor che si basa su di esso) supporta molti operatori, ma è sufficiente riconoscere alcune per iniziare. Nella tabella seguente sono riepilogati gli operatori più comuni.

| **Operator** | **Descrizione** | **Esempi** |
| --- | --- | --- |
| `+` `-` `*` `/` | Matematici (operatori) utilizzati nelle espressioni numeriche. | [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)] |
| `=` | Assegnazione. Assegna il valore sul lato destro di un'istruzione per l'oggetto sul lato sinistro. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)] |
| `==` | Uguaglianza. Restituisce `true` se i valori sono uguali. (Si noti la differenza tra il `=` operatore e `==` operatore.) | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)] |
| `!=` | Disuguaglianza. Restituisce `true` se i valori non sono uguali. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)] |
| `< > <= >=` | Meno-maggiore,-, minore di o uguale e maggiore di o uguale a. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)] |
| `+` | Concatenazione, viene utilizzata per unire più stringhe. ASP.NET riconosce la differenza tra questo operatore e operatore di addizione in base al tipo di dati dell'espressione. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)] |
| `+=` `-=` | Gli operatori di incremento e decremento, quali l'addizione e sottrazione di 1, rispettivamente, da una variabile. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)] |
| `.` | Punto. Utilizzato per distinguere gli oggetti e proprietà e metodi. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)] |
| `()` | Parentesi. Utilizzato per le espressioni di raggruppamento e per passare parametri a metodi. | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)] |
| `[]` | Le parentesi quadre. Utilizzato per accedere ai valori alle raccolte o matrici. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)] |
| `!` | No. Inverte una `true` valore `false` e viceversa. In genere utilizzato come un modo abbreviato per verificare la presenza di `false` (ovvero, per non `true`). | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)] |
| `&&` <code>&#124;&#124;</code> | AND logico e che vengono utilizzati per collegare le condizioni. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)] |

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Utilizzo di File e i percorsi delle cartelle nel codice

Sarà più di frequente con percorsi di file e cartelle nel codice. Ecco un esempio di struttura di cartelle fisico per un sito Web come potrebbe apparire nel computer di sviluppo:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Ecco alcune informazioni essenziali sugli URL e percorsi:

- Un URL inizia con un nome di dominio (`http://www.example.com`) o un nome server (`http://localhost`, `http://mycomputer`).
- Un URL corrisponde a un percorso fisico in un computer host. Ad esempio, `http://myserver` potrebbe corrispondere alla cartella *C:\websites\mywebsite* sul server.
- Un percorso virtuale è a sintassi abbreviata per rappresentare i percorsi nel codice senza la necessità di specificare il percorso completo. Include la parte di un URL che segue il nome di dominio o server. Quando si usano percorsi virtuali, è possibile spostare il codice per un dominio diverso o un server senza la necessità di aggiornare i percorsi.

Di seguito è riportato un esempio che consentono di comprendere le differenze:

| URL completo | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nome del server | *mycompanyserver* |
| Percorso virtuale | */HumanResources/CompanyPolicy.htm* |
| Percorso fisico | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

La directory principale virtuale /, come la radice dell'unità c: unità \. (I percorsi delle cartelle virtuali sempre utilizzano le barre). Il percorso virtuale di una cartella non deve avere lo stesso nome della cartella fisica; può trattarsi di un alias. (Nei server di produzione, il percorso virtuale raramente corrisponde a un percorso fisico esatto)

Quando si lavora con i file e cartelle nel codice, in alcuni casi è necessario fare riferimento al percorso fisico e talvolta un percorso virtuale, a seconda di quali oggetti si sta lavorando. ASP.NET fornisce questi strumenti per l'utilizzo di percorsi di file e cartelle nel codice: il `Server.MapPath` (metodo) e `~` operatore e `Href` metodo.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Conversione di percorsi virtuali per fisica: il metodo MapPath

Il `Server.MapPath` metodo converte un percorso virtuale (ad esempio */default.cshtml*) in un percorso fisico assoluto (ad esempio *C:\WebSites\MyWebSiteFolder\default.cshtml*). Utilizzare questo metodo ogni volta che è necessario un percorso fisico completo. Un esempio tipico è quando la lettura o scrittura di un file di testo o un file di immagine nel server web.

In genere non conosce il percorso fisico assoluto del sito nel server del sito di hosting, in modo da questo metodo consente di convertire il percorso presente, il percorso virtuale, nel percorso corrispondente nel server di. Si passa il percorso virtuale per un file o una cartella per il metodo e restituisce il percorso fisico:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>La radice virtuale di riferimento: il ~ (operatore) e il metodo di Href

In un *. cshtml* o *. vbhtml* file, è possibile fare riferimento il percorso radice virtuale utilizzando il `~` operatore. Questo è molto utile poiché pagine è possibile spostarsi in un sito e tutti i collegamenti che ad altre pagine contengono non verrà interrotti. È inoltre utile nel caso in cui si sposta mai il sito Web in un percorso diverso. Ecco alcuni esempi:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Se il sito Web è `http://myserver/myapp`, ecco come ASP.NET tratterà questi percorsi quando viene eseguita la pagina:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Questi percorsi effettivamente non verrà visualizzato come i valori della variabile, ma ASP.NET considererà i percorsi che come se si trattasse).

È possibile utilizzare il `~` operatore nel codice server (vedere sopra) e nel markup, simile al seguente:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

Nel markup, utilizzare il `~` operatore per creare percorsi a risorse quali file di immagine, altre pagine web e file CSS. Quando la pagina viene eseguita, ASP.NET esegue la ricerca tramite la pagina (codice e markup) e risolve tutti i `~` riferimenti al percorso appropriato.

## <a name="conditional-logic-and-loops"></a>Cicli e logica condizionale

Il codice server ASP.NET consente di eseguire attività in base alle condizioni e scrivere codice che viene ripetuta di istruzioni di un determinato numero di volte (ovvero, codice che esegue un ciclo).

### <a name="testing-conditions"></a>Condizioni di test

Per testare una condizione semplice è usare il `if` istruzione, che restituisce true o false in base a un test specificato:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

Il `if` (parola chiave) viene avviato un blocco. Il test effettivo (condizione) è racchiuso tra parentesi e restituisce true o false. Le istruzioni eseguite se il test è true sono racchiusi tra parentesi graffe. Un `if` istruzione può includere un `else` blocco di istruzioni da eseguire se la condizione è false specifica:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

È possibile aggiungere più condizioni utilizzando un `else if` blocco:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

In questo esempio, se la prima condizione nella se blocco non è true, il `else if` condizione è verificata. Se tale condizione viene soddisfatta, le istruzioni di `else if` blocco vengono eseguite. Se nessuna delle condizioni vengono soddisfatte, le istruzioni di `else` blocco vengono eseguite. È possibile aggiungere un numero qualsiasi di altro se si blocca e quindi chiudere con un `else` blocco come il &quot;tutto il resto&quot; condizione.

Per testare un numero elevato di condizioni, usare un `switch` blocco:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Il valore da verificare è racchiuso tra parentesi (nell'esempio di `weekday` variabile). Ogni singolo test utilizza un `case` istruzione che termina con due punti (:). Se il valore di un `case` istruzione corrisponde al valore di test, viene eseguito il codice in tale blocco case. Chiusura di ogni istruzione case con un `break` istruzione. (Se si dimentica di includere l'interruzione in ogni `case` blocco, il codice dal successivo `case` istruzione verrà eseguita anche.) Oggetto `switch` blocco è spesso un `default` istruzione come ultimo caso per un &quot;tutto il resto&quot; opzione che viene eseguita se viene soddisfatta nessuna degli altri casi.

Il risultato delle ultime due blocchi condizionali visualizzati in un browser:

![Razor Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Codice di ciclo

È spesso necessario eseguire le stesse istruzioni ripetutamente. Questo scopo è di ciclo. Ad esempio, si esegue spesso le stesse istruzioni per ogni elemento in una raccolta di dati. Se si conosce esattamente il numero di volte che si desidera eseguire il ciclo, è possibile utilizzare un `for` ciclo. Questo tipo di ciclo è particolarmente utile per il conteggio di o di conteggio verso il basso:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

Il ciclo inizia con il `for` ogni parola chiave, seguita da tre istruzioni racchiuso tra parentesi, terminato con un punto e virgola.

- All'interno delle parentesi, la prima istruzione (`var i=10;`) crea un contatore e lo inizializza per 10. Non è necessario assegnare un nome di contatore `i` &#8212; è possibile utilizzare qualsiasi variabile. Quando il `for` ciclo viene eseguito, il contatore viene incrementato automaticamente.
- La seconda istruzione (`i < 21;`) imposta la condizione per la distanza con cui si desidera contare. In questo caso, si desidera passare a un massimo di 20 (vale a dire altre operazioni mentre il contatore è minore di 21).
- La terza istruzione (`i++` ) usa un operatore di incremento, che specifica semplicemente che il contatore dovrebbe avere 1 aggiunti ogni volta che il ciclo viene eseguito.

All'interno delle parentesi graffe è il codice che verrà eseguito in ogni iterazione del ciclo. Il codice crea un nuovo paragrafo (`<p>` elemento) ogni e aggiunge una riga all'output, la visualizzazione del valore di `i` (il contatore). Quando si esegue questa pagina, l'esempio crea 11 righe di visualizzazione dell'output, con il testo in ogni riga che indica il numero dell'elemento.

![Razor Img11](introducing-razor-syntax-c/_static/image11.jpg)

Se si lavora con una raccolta o una matrice, utilizzare spesso un `foreach` ciclo. Una raccolta è un gruppo di oggetti simili e `foreach` ciclo consente eseguire un'attività su ogni elemento nella raccolta. Questo tipo di ciclo è ideale per le raccolte, perché a differenza di un `for` ciclo, non è necessario incrementare il contatore o impostare un limite. Al contrario, il `foreach` codice ciclo continua semplicemente tramite la raccolta fino al termine.

Ad esempio, il codice seguente restituisce gli elementi di `Request.ServerVariables` raccolta, ovvero un oggetto che contiene informazioni sul server web. Usa un `foreac` ciclo h per visualizzare il nome di ogni elemento creando un nuovo `<li>` elemento in un elenco puntato HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

Il `foreach` parola chiave sia seguita dalle parentesi in cui si dichiara una variabile che rappresenta un singolo elemento della raccolta (nell'esempio `var item`), seguito dal `in` (parola chiave), seguito da una raccolta che si desidera scorrere in ciclo. Nel corpo del `foreach` ciclo, è possibile accedere all'elemento corrente utilizzando la variabile dichiarato in precedenza.

![Razor Img12](introducing-razor-syntax-c/_static/image12.jpg)

Per creare un ciclo più generico, utilizzare il `while` istruzione:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

A `while` ciclo inizia con il `while` (parola chiave), seguita dalle parentesi, in cui specificare quanto tempo il ciclo continua (in questo caso, per purché `countNum` è inferiore a 50), quindi il blocco di ripetizione. In genere aumentare di cicli (aggiungere) o decrementare (sottratto) una variabile o un oggetto utilizzato per il conteggio. Nell'esempio di `+=` operatore aggiunge 1 a `countNum` ogni volta che il ciclo viene eseguito. (Per decrementare una variabile in un ciclo che conta in senso decrescente, utilizzare l'operatore di decremento `-=`).

## <a name="objects-and-collections"></a>Oggetti e raccolte

Quasi tutte le funzioni in un sito Web ASP.NET è un oggetto, tra cui la pagina stessa. Questa sezione illustra alcuni oggetti importanti che verranno utilizzate di frequente nel codice.

### <a name="page-objects"></a>Oggetti della pagina

L'oggetto di base in ASP.NET è la pagina. È possibile accedere a proprietà dell'oggetto pagina direttamente senza alcun oggetto originale. Il codice seguente ottiene il percorso di file della pagina, tramite il `Request` oggetto della pagina:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Per rendere chiaro che viene fatto riferimento proprietà e metodi dell'oggetto della pagina corrente, è possibile utilizzare la parola chiave `this` per rappresentare l'oggetto della pagina nel codice. Ecco l'esempio di codice precedente, con `this` aggiunto per rappresentare la pagina:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

È possibile utilizzare le proprietà del `Page` oggetto per ottenere una grande quantità di informazioni, ad esempio:

- `Request`. Come già visto, questa è una raccolta di informazioni relative alla richiesta corrente, inclusi il tipo di browser ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente, e così via.
- `Response`. Si tratta di una raccolta di informazioni sulla risposta che verrà inviata al browser al termine dell'esecuzione il codice del server (pagina). Ad esempio, è possibile utilizzare questa proprietà per scrivere informazioni nella risposta. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Oggetti della raccolta (matrici e dizionari)

Oggetto *raccolta* è un gruppo di oggetti dello stesso tipo, ad esempio una raccolta di `Customer` oggetti da un database. ASP.NET contiene molte raccolte predefinite, ad esempio il `Request.Files` insieme.

Si utilizzerà spesso dati in raccolte. Due tipi di raccolte comuni sono la *matrice* e *dizionario*. Una matrice è utile quando si desidera archiviare una raccolta di elementi simili, ma non si desidera creare una variabile separata per ogni elemento di contenere:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Con le matrici, si dichiara un tipo di dati specifici, ad esempio `string`, `int`, o `DateTime`. Per indicare che la variabile può contenere una matrice, aggiungere le parentesi quadre la dichiarazione (ad esempio `string[]` o `int[]`). È possibile accedere agli elementi di una matrice con la loro posizione (indice) o tramite il `foreach` istruzione. Matrice di indici in base zero sono &#8212; vale a dire il primo elemento è nella posizione 0, il secondo elemento alla posizione 1 e così via.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

È possibile determinare il numero di elementi in una matrice tramite il recupero dei relativi `Length` proprietà. Per ottenere la posizione di un elemento specifico della matrice (la ricerca nella matrice), utilizzare il `Array.IndexOf` metodo. È inoltre possibile eseguire operazioni come reverse il contenuto di una matrice (il `Array.Reverse` (metodo)) o ordinare il contenuto (il `Array.Sort` (metodo)).

L'output del codice di matrice di stringa visualizzato in un browser:

![Razor Img13](introducing-razor-syntax-c/_static/image13.jpg)

Un dizionario è una raccolta di coppie chiave/valore, in cui fornire la chiave (o nome) per impostare o recuperare il valore corrispondente:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Per creare un dizionario, utilizzare il `new` (parola chiave) per indicare che si sta creando un nuovo oggetto dizionario. È possibile assegnare un dizionario a una variabile utilizzando il `var` (parola chiave). Indicare i tipi di dati degli elementi nel dizionario di parentesi angolari ( `< >` ). Alla fine della dichiarazione, è necessario aggiungere una coppia di parentesi, poiché si tratta in realtà un metodo che crea un nuovo dizionario.

Per aggiungere elementi al dizionario, è possibile chiamare il `Add` metodo per la variabile del dizionario (`myScores` in questo caso), quindi specificare una chiave e un valore. In alternativa, è possibile utilizzare le parentesi quadre per indicare la chiave ed eseguire una semplice assegnazione, come nell'esempio seguente:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Per ottenere un valore dal dizionario, specificare la chiave tra parentesi quadre:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Chiamata di metodi con parametri

Durante la lettura in precedenza in questo articolo, gli oggetti che si programma con possono disporre di metodi. Ad esempio, un `Database` oggetto potrebbe avere un `Database.Connect` metodo. Molti metodi dispongono anche di uno o più parametri. Oggetto *parametro* è un valore che viene passato a un metodo per abilitare il metodo completare l'attività. Ad esempio, una dichiarazione per esaminare il `Request.MapPath` metodo che accetta tre parametri:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(La riga è stato eseguito il wrapping per renderlo più leggibile. Tenere presente che è possibile inserire interruzioni di riga quasi qualsiasi posizione ad eccezione del fatto che le stringhe all'interno sono racchiusi tra virgolette.)

Questo metodo restituisce il percorso fisico sul server che corrisponde a un percorso virtuale specificato. I tre parametri per il metodo sono `virtualPath`, `baseVirtualDir`, e `allowCrossAppMapping`. Si noti che nella dichiarazione, i parametri sono elencati con i tipi di dati dei dati che verrà accettano. Quando si chiama questo metodo, è necessario fornire valori per tutti e tre i parametri.

La sintassi Razor offre due opzioni per passare parametri a un metodo: *parametri posizionali* e *parametri denominati*. Per chiamare un metodo utilizzando i parametri posizionali, passare i parametri in un ordine fisso specificato nella dichiarazione del metodo. (È necessario in genere conoscere questo ordine leggendo la documentazione relativa al metodo.) È necessario seguire l'ordine e non è possibile omettere i parametri &#8212; Se necessario, si passa una stringa vuota (`""`) o `null` per un parametro posizionale che non si dispone di un valore per.

Nell'esempio seguente si presuppone che si dispone di una cartella denominata *script* nel sito Web. Il codice chiama il `Request.MapPath` metodo e passa i valori per i tre parametri nell'ordine corretto. Viene quindi visualizzato il percorso mappato risulta.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Quando un metodo presenta numerosi parametri, è possibile mantenere il codice più leggibile utilizzando parametri denominati. Per chiamare un metodo utilizzando parametri denominati, specificare il nome del parametro seguito da due punti (:) e il valore. Il vantaggio di parametri denominati è che è possibile passare loro in qualsiasi ordine desiderato. (Uno svantaggio è che la chiamata al metodo non compressi).

Nell'esempio seguente chiama il metodo stesso come illustrato in precedenza, ma Usa parametri per fornire i valori denominati:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Come si può notare, i parametri vengono passati in un ordine diverso. Tuttavia, se si esegue l'esempio precedente e in questo esempio, verrà restituiscono lo stesso valore.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Gestione degli errori

### <a name="try-catch-statements"></a>Istruzioni Try-Catch

È necessario spesso istruzioni nel codice che potrebbe non riuscire per motivi all'esterno del controllo. Ad esempio:

- Se il codice tenta di creare o accedere a un file, potrebbero verificarsi tutti i tipi di errori. Il file desiderato potrebbe non esistere, potrebbe essere stato bloccato, il codice potrebbe non disporre delle autorizzazioni e così via.
- Analogamente, se il codice tenta di aggiornare i record in un database, possono essere presenti problemi relativi alle autorizzazioni, la connessione al database potrebbe essere rimossi, i dati da salvare potrebbero essere non valido e così via.

In termini di programmazione, vengono chiamate queste situazioni *eccezioni*. Se il codice rileva un'eccezione, (genera) genera un messaggio di errore che un indesiderate nel migliore dei casi, per gli utenti:

![Razor Img14](introducing-razor-syntax-c/_static/image14.jpg)

In situazioni in cui il codice che si verifichino eccezioni e per evitare messaggi di errore di questo tipo, è possibile utilizzare `try/catch` istruzioni. Nel `try` istruzione, eseguire il codice che si sta archiviando. In uno o più `catch` istruzioni, è possibile cercare specifici errori che potrebbero essersi verificati (tipi specifici di eccezioni). È possibile includere un numero `catch` istruzioni è necessario cercare gli errori che sono prevedendo.

> [!NOTE]
> È consigliabile evitare di utilizzare il `Response.Redirect` metodo `try/catch` istruzioni, poiché potrebbe causare un'eccezione della pagina.


Nell'esempio seguente viene mostrata una pagina che crea un file di testo alla prima richiesta e quindi visualizza un pulsante che consente all'utente di aprire il file. L'esempio è deliberatamente Usa un nome di file non valido in modo che genera un'eccezione. Il codice include `catch` istruzioni per due possibili eccezioni: `FileNotFoundException`, che si verifica se il nome del file è danneggiato, e `DirectoryNotFoundException`, che si verifica se ASP.NET non è possibile anche trovare la cartella. (È possibile rimuovere il commento nell'esempio, un'istruzione per vedere come viene eseguito quando tutto funziona correttamente.)

Se il codice non gestisce l'eccezione, si vedrà una pagina di errore come schermata precedente. Tuttavia, il `try/catch` sezione consente di impedire all'utente di visualizzare questi tipi di errori.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

**Programmazione con Visual Basic**


[Appendice: linguaggio Visual Basic e sintassi](https://go.microsoft.com/fwlink/?LinkId=202908)


**Documentazione di riferimento**


[ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/ee532866.aspx)

[Linguaggio c#](https://msdn.microsoft.com/en-us/library/kx37x362.aspx)
