---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor (Visual Basic) | Documenti Microsoft
author: tfitzmac
description: Questa appendice offre una panoramica della programmazione con le pagine Web ASP.NET in Visual Basic, con sintassi Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: aad951a0e4344dbaafbdcc3b3980307a26fa75fc
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor (Visual Basic)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo offre una panoramica della programmazione con ASP.NET Web Pages con sintassi Razor e Visual Basic. ASP.NET è la tecnologia Microsoft per l'esecuzione dinamica di pagine web nei server web.
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


Utilizzano la maggior parte degli esempi di utilizzo di ASP.NET Web Pages con sintassi Razor c#. Ma la sintassi Razor supporta anche Visual Basic. Per programmare una pagina web ASP.NET in Visual Basic, si crea una pagina web con un *. vbhtml* estensione del nome file, quindi aggiungere il codice di Visual Basic. Questo articolo viene fornita una panoramica dell'utilizzo con il linguaggio Visual Basic e la sintassi per creare pagine Web ASP.NET.

> [!NOTE]
> I modelli di sito Web predefinito per Microsoft WebMatrix (**azienda**, **raccolta foto**, e **Starter Site**e così via) sono disponibili nelle versioni di c# e Visual Basic. È possibile installare i modelli di Visual Basic da come pacchetti NuGet. Modelli di siti Web vengono installati nella cartella radice del sito in una cartella denominata *Templates Microsoft*.


## <a name="the-top-8-programming-tips"></a>I suggerimenti di programmazione 8

In questa sezione sono elencati alcuni suggerimenti che è assolutamente necessario conoscere quando si inizia la scrittura di codice server ASP.NET utilizzando la sintassi Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Aggiungere codice a una pagina utilizzando il carattere @

Il `@` carattere venga espressioni inline, a istruzione singola blocchi e blocchi con istruzioni multiple:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Il risultato visualizzato in un browser:

![Razor Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **La codifica HTML**
> 
> Quando si visualizza il contenuto in una pagina utilizzando il `@` carattere, come negli esempi precedenti, ASP.NET codifica in HTML l'output. Ciò consente di sostituire caratteri HTML riservati (ad esempio `<` e `>` e `&`) con i codici di abilitare i caratteri da visualizzare come caratteri in una pagina web anziché essere interpretato come tag HTML o le entità. Senza codifica HTML, l'output dal codice server non vengano visualizzati correttamente e potrebbe esporre una pagina a rischi di sicurezza.
> 
> Se l'obiettivo consiste nel markup HTML che esegue il rendering di tag di markup di output (ad esempio `<p></p>` per un paragrafo o `<em></em>` per evidenziare il testo), vedere la sezione [la combinazione di testo, Markup e codice nei blocchi di codice](#BM_CombiningTextMarkupAndCode) più avanti in questo articolo.
> 
> Altre informazioni sulla codifica HTML [utilizzo di form HTML nei siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Racchiudere i blocchi di codice con il codice... Codice di fine

Un blocco di codice include una o più istruzioni di codice e incluso con le parole chiave `Code` e `End Code`. Inserire l'apertura `Code` parola chiave immediatamente dopo il `@` carattere &#8212; tra di essi non possono essere presenti spazi vuoti.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Il risultato visualizzato in un browser:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. All'interno di un blocco, termine di ogni istruzione del codice con un'interruzione di riga

In un blocco di codice Visual Basic, ogni istruzione termina con un'interruzione di riga. (Più avanti nell'articolo verrà visualizzato un modo per eseguire il wrapping di un'istruzione di codice lunghi in più righe se necessario.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Utilizzare le variabili per archiviare i valori

È possibile archiviare valori in un *variabile*, incluse le stringhe, numeri e date, e così via. Si crea una nuova variabile utilizzando il `Dim` (parola chiave). È possibile inserire i valori delle variabili direttamente in una pagina utilizzando `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Il risultato visualizzato in un browser:

![Razor Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Racchiudere i valori letterali stringa tra virgolette doppie

Oggetto *stringa* è una sequenza di caratteri che vengono considerati come testo. Per specificare una stringa, è racchiuderlo tra virgolette doppie:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Per incorporare le virgolette doppie all'interno di un valore stringa, inserire due caratteri segno di virgolette doppie. Se si desidera il carattere virgolette doppie vengono visualizzati una volta nell'output della pagina, immettere come `""` al stringa e, se si desidera venga visualizzato due volte, immetterne il nome come `""""` all'interno della stringa tra virgolette.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Il risultato visualizzato in un browser:

![Razor Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Codice Visual Basic non fa distinzione tra maiuscole e minuscole

Il linguaggio Visual Basic non è tra maiuscole e minuscole. Parole chiave di programmazione (come `Dim`, `If`, e `True`) e i nomi delle variabili (ad esempio `myString`, o `subTotal`) possono essere scritti in tutti i casi.

Le seguenti righe di codice di assegnare un valore alla variabile `lastname` utilizzando una minuscola nome e quindi restituire il valore della variabile di pagina utilizzando un nome di lettere maiuscole.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Il risultato visualizzato in un browser:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Gran parte del codice implica l'utilizzo di oggetti

Un oggetto rappresenta un elemento che è possibile programmare con &#8212; una pagina di una casella di testo, un file, un'immagine, una richiesta web, un messaggio di posta elettronica, un record del cliente (riga del database), e così via. Gli oggetti hanno proprietà che descrivono le caratteristiche &#8212; dispone di un oggetto di casella di testo una `Text` proprietà, un oggetto di richiesta ha un `Url` proprietà, un messaggio di posta elettronica sono una `From` proprietà e un oggetto customer ha una `FirstName` proprietà. Gli oggetti includono anche metodi che sono il &quot;verbi&quot; possono eseguire. Esempi includono un oggetto di file `Save` (metodo), l'oggetto image `Rotate` (metodo) e un oggetto di posta elettronica `Send` metodo.

Spesso si utilizzerà il `Request` oggetto, che fornisce informazioni quali i valori del form di campi della pagina (caselle di testo, e così via), il tipo di browser ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente, e così via. In questo esempio viene illustrato come accedere alle proprietà del `Request` oggetto e come chiamare il `MapPath` metodo il `Request` oggetto, che fornisce il percorso assoluto della pagina nel server:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Il risultato visualizzato in un browser:

![Razor Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. È possibile scrivere codice che prende decisioni

Una funzionalità chiave di pagine web dinamiche è possibile determinare quali operazioni eseguire in base alle condizioni. È il modo più comune per eseguire questa operazione con il `If` istruzione (e facoltativi `Else` istruzione).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

L'istruzione `If IsPost` è un modo abbreviato di scrittura `If IsPost = True`. Insieme a `If` istruzioni, esistono diversi modi per verificare le condizioni, ripetere i blocchi di codice, e così via, che sono descritte più avanti in questo articolo.

Il risultato visualizzato in un browser (dopo aver fatto clic **Invia**):

![Razor Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET e POST metodi e proprietà istruzione IsPost**
> 
> Il protocollo utilizzato per le pagine web (HTTP) supporta un numero molto limitato di metodi (&quot;verbi&quot;) che vengono utilizzati per effettuare richieste al server. Le due cause più comuni sono GET, viene utilizzato per leggere una pagina, e POST, viene utilizzato per inviare una pagina. In generale, la prima volta che un utente richiede una pagina, la pagina viene richiesta con GET. Se l'utente è un modulo compilato e quindi fa clic su **Invia**, il browser invia una richiesta POST per il server.
> 
> Nella programmazione web, è spesso utile sapere se una pagina viene richiesta come un'operazione GET o POST in modo da sapere come elaborare la pagina. In ASP.NET Web Pages, è possibile utilizzare il `IsPost` proprietà per verificare se una richiesta è un'operazione GET o POST. Se la richiesta è una richiesta POST, il `IsPost` restituirà true e consente di eseguire operazioni come read i valori di caselle di testo in un form. Molti esempi vedrai mostrano come elaborare la pagina in modo diverso a seconda del valore di `IsPost`.


## <a name="a-simple-code-example"></a>Un semplice esempio di codice

In questa procedura viene illustrato come creare una pagina che illustra le tecniche di programmazione di base. Nell'esempio si crea una pagina che consente agli utenti di immettere due numeri interi, quindi li aggiunge e viene visualizzato il risultato.

1. Nell'editor, creare un nuovo file e denominarlo *AddNumbers.vbhtml*.
2. Copiare il codice e markup seguenti nella pagina, sostituendo qualsiasi elemento presente nella pagina.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Ecco alcune operazioni per cui è possibile notare:

    - Il `@` carattere inizia il primo blocco di codice nella pagina e precede il `totalMessage` variabile incorporato nella parte inferiore.
    - Il blocco nella parte superiore della pagina è incluso in `Code...End Code`.
    - Le variabili `total`, `num1`, `num2`, e `totalMessage` archiviare numeri diversi e una stringa.
    - Il valore di stringa letterale assegnato per il `totalMessage` variabile è racchiuso tra virgolette.
    - Poiché il codice Visual Basic viene fatta distinzione tra maiuscole e minuscole, non quando il `totalMessage` variabile viene utilizzata nella parte inferiore della pagina, solo il nome deve corrispondere l'ortografia della dichiarazione di variabile nella parte superiore della pagina. Le maiuscole e minuscole non è rilevante.
    - L'espressione `num1.AsInt()`  +  `num2.AsInt()` viene illustrato l'utilizzo con gli oggetti e metodi. Il `AsInt` metodo su ogni variabile Converte la stringa immessa da un utente a un numero intero (integer) che può essere aggiunto.
    - Il `<form>` tag include un `method="post"` attributo. Specifica che quando l'utente fa clic **Aggiungi**, la pagina verrà inviata al server utilizzando il metodo HTTP POST. Quando l'invio della pagina, il codice `If IsPost` restituisce true e condizionale viene eseguito, Visualizza il risultato dell'aggiunta di numeri di codice.
3. Salvare la pagina ed eseguirlo in un browser. (Assicurarsi che la pagina è selezionata nel **file** dell'area di lavoro prima di eseguirlo.) Immettere due numeri interi, quindi scegliere il **Aggiungi** pulsante.

    ![Razor Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>La sintassi e linguaggio Visual Basic

In precedenza è stato illustrato un esempio su come creare una pagina web ASP.NET e come è possibile aggiungere codice lato server per il markup HTML. Di seguito illustra le nozioni di base dell'utilizzo di Visual Basic per scrivere il codice server ASP.NET utilizzando la sintassi Razor &#8212; , ovvero le regole del linguaggio di programmazione.

Se si è esperti di programmazione (in particolare se si utilizza C, C++, c#, Visual Basic o JavaScript), gran parte delle quali leggere risulteranno familiare. Sarà probabilmente necessario acquisire familiarità con solo come codice WebMatrix viene aggiunto al markup in *. vbhtml* file.

### <a id="BM_CombiningTextMarkupAndCode"></a>  Combinazione di testo, markup e codice nei blocchi di codice

Nei blocchi di codice server, è spesso opportuno all'output di testo e markup alla pagina. Se un blocco di codice server contiene testo che non è codice e che invece deve essere eseguito il rendering è, ASP.NET deve essere in grado di distinguere tale testo dal codice. Esistono diversi modi per eseguire tale operazione.

- Racchiudere il testo in un blocco HTML come `<p></p>` o `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    L'elemento HTML può includere testo, elementi HTML aggiuntivi e le espressioni di codice server. Quando ASP.NET visualizza il tag HTML di apertura (ad esempio, `<p>`), esegue il rendering di tutti gli elementi dell'elemento ed è il proprio contenuto come per il browser (e risolve le espressioni di codice lato server).

- Utilizzare il `@:` operatore o `<text>` elemento. Il `@:` restituisce una singola riga di contenuto che contengono testo normale o tag HTML senza corrispondenza; il `<text>` elemento include più righe di output. Queste opzioni sono utili quando non si desidera eseguire il rendering di un elemento HTML come parte dell'output.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    Nell'esempio seguente viene ripetuto l'esempio precedente, ma utilizza una singola coppia di `<text>` tag per racchiudere il testo per il rendering.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    Nell'esempio seguente, il `<text>` e `</text>` tag racchiudono tre righe, che hanno tutti il testo non indipendente e senza tag HTML (`<br />`), insieme a codice server e i tag HTML corrispondenti. Nuovamente, è impossibile anche far precedere a ogni riga singolarmente con il `@:` operatore; entrambi funzionano modo.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Quando il testo è l'output come illustrato in questa sezione &#8212; utilizzando un elemento HTML, il `@:` operatore o la `<text>` elemento &#8212; ASP.NET non di codifica HTML l'output. (Come indicato in precedenza, ASP.NET codificare l'output di espressioni di codice server e i blocchi di codice server che sono preceduti da `@`, ad eccezione dei casi speciali riportati in questa sezione.)

### <a name="whitespace"></a>Whitespace

Spazi aggiuntivi in un'istruzione (e all'esterno di un valore letterale stringa) non influiscono sull'istruzione:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Istruzioni lungo la suddivisione in più righe

È possibile interrompere un'istruzione di codice lunghi in più righe utilizzando il carattere di sottolineatura `_` (che in Visual Basic viene chiamato il *carattere di continuazione*) dopo ogni riga di codice. Per interrompere un'istruzione sulla riga successiva, aggiungere alla fine della riga di uno spazio e quindi il carattere di continuazione. Continuare l'istruzione nella riga successiva. È possibile eseguire il wrapping di istruzioni in tutte le righe che è necessario migliorare la leggibilità. Le istruzioni seguenti sono uguali:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Tuttavia, non è possibile disporre di una riga all'interno di un valore letterale stringa. Non funziona nell'esempio seguente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Per combinare una stringa lunga che esegue il wrapping di più righe, ad esempio il codice sopra riportato, è necessario utilizzare il *operatore di concatenazione* (`&`), che verrà illustrato più avanti in questo articolo.

### <a name="code-comments"></a>Commenti del codice

Commenti è possibile lasciare le note per se stessi o ad altri utenti. I commenti di sintassi Razor sono precedute dal prefisso `@*` e terminare con `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

All'interno di blocchi di codice è possibile utilizzare i commenti di sintassi Razor, oppure è possibile utilizzare il carattere ordinario Visual Basic commento, ovvero una virgoletta singola (`'`) come prefisso a ogni riga.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Variabili

Una variabile è un oggetto denominato che consente di archiviare i dati. È possibile assegnare variabili di qualsiasi tipo, ma il nome deve iniziare con un carattere alfabetico e non può contenere spazi o caratteri riservati. In Visual Basic, come illustrato in precedenza, non è rilevante nel caso di lettere in un nome di variabile.

### <a name="variables-and-data-types"></a>Le variabili e tipi di dati

Una variabile può avere un tipo di dati specifici, che indica il tipo di dati è archiviato nella variabile. È possibile disporre di variabili di stringa che archiviano i valori stringa (ad esempio &quot;Hello world&quot;), le variabili integer che archiviano i valori numero intero (ad esempio, 3 o 79) e le variabili di date che archiviano i valori di data in una varietà di formati (ad esempio, 12/4/2012 o marzo 2009 ). E sono disponibili molti altri tipi di dati che è possibile utilizzare.

Tuttavia, non devi specificare un tipo per una variabile. Nella maggior parte dei casi ASP.NET può determinare il tipo di base sull'utilizzo di dati nella variabile. (In alcuni casi è necessario specificare un tipo; visualizzato esempi in cui questo è true).

Per dichiarare una variabile senza specificare un tipo, utilizzare `Dim` più il nome di variabile (ad esempio, `Dim myVar`). Per dichiarare una variabile con un tipo, utilizzare `Dim` più il nome della variabile, seguito da `As` e quindi il nome del tipo (ad esempio, `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

L'esempio seguente mostra alcune espressioni inline che utilizzano le variabili in una pagina web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Il risultato visualizzato in un browser:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>La conversione e i tipi di dati di test

Anche se ASP.NET in genere può determinare automaticamente un tipo di dati, in alcuni casi, non può essere Di conseguenza, potrebbe essere necessario aiutare ASP.NET eseguendo una conversione esplicita. Anche se non è necessario convertire i tipi, a volte è utile verificare il tipo di dati se si lavora con.

Il caso più comune è che è necessario convertire una stringa in un altro tipo, ad esempio in un numero intero o una data. Nell'esempio seguente viene illustrato un caso tipico in cui è necessario convertire una stringa in un numero.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Come regola, l'input dell'utente vengono recapitati come stringhe. Anche se è stato richiesto all'utente di immettere un numero e anche se è stato immesso una cifra, quando viene inviato l'input dell'utente e viene letta nel codice, i dati sono in formato stringa. Pertanto, è necessario convertire la stringa a un numero. Nell'esempio, se si tenta di eseguire operazioni aritmetiche su valori senza convertirli, l'errore seguente viene generato, perché ASP.NET non è possibile aggiungere due stringhe:

`Cannot implicitly convert type 'string' to 'int'.`

Per convertire i valori in numeri interi, si chiama il `AsInt` metodo. Se la conversione ha esito positivo, è quindi possibile aggiungere i numeri.

Nella tabella seguente sono elencati alcuni metodi di conversione e di test comuni per le variabili.


0x%2 riga:::::: colonna 0x%2 <strong>(metodo)</strong> ::: colonna-end:::::: colonna::: <strong>descrizione</strong> ::: colonna-end:::::: colonna::: <strong>esempio</strong> ::: colonna-end:::::: fine riga:::
* * *
0x%2 riga:::::: colonna 0x%2 `AsInt(), IsInt()` ::: colonna-end::: 0x%2 colonna::: converte una stringa che rappresenta un numero intero (come &quot;593&quot;) in un intero.
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga:::::: colonna 0x%2 `AsBool(), IsBool()` ::: colonna-end:::::: colonna::: converte una stringa, ad esempio &quot;true&quot; o &quot;false&quot; a un tipo Boolean.
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga:::::: colonna 0x%2 `AsFloat(), IsFloat()` ::: colonna-end::: 0x%2 colonna::: converte una stringa che contiene un valore decimale, ad esempio &quot;1.3&quot; o &quot;7.439&quot; un numero a virgola mobile.
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga:::::: colonna 0x%2 `AsDecimal(), IsDecimal()` ::: colonna-end::: 0x%2 colonna::: converte una stringa che contiene un valore decimale, ad esempio &quot;1.3&quot; o &quot;7.439&quot; in un numero decimale. (In ASP.NET, un numero decimale è più preciso rispetto a un numero a virgola mobile). ::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga::: 0x%2 colonna 0x%2 `AsDateTime(), IsDateTime()` ::: colonna-end:::::: colonna::: converte una stringa che rappresenta un valore data e ora per ASP.NET `DateTime` tipo.
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga::: 0x%2 colonna 0x%2 `ToString()` ::: colonna-end:::::: colonna::: converte qualsiasi altro tipo di dati in una stringa.
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    ::: colonna-end::: 0x%2 riga-end:


## <a name="operators"></a>Operatori

Un operatore è una parola chiave o un carattere che indica il tipo di comando da eseguire in un'espressione di ASP.NET. Visual Basic supporta molti operatori, ma è sufficiente riconoscere alcune per iniziare lo sviluppo di pagine web ASP.NET. Nella tabella seguente sono riepilogati gli operatori più comuni.


0x%2 riga:::::: colonna 0x%2 <strong>operatore</strong> ::: colonna-end:::::: colonna::: <strong>descrizione</strong> ::: colonna-end:::::: colonna::: <strong>esempi</strong> ::: colonna-end:::::: fine riga:::
* * *
0x%2 riga:::::: colonna 0x%2 `+ - * /` ::: colonna-end:::::: colonna 0x%2 matematici (operatori) utilizzati nelle espressioni numeriche.
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga::: 0x%2 colonna 0x%2 `=` ::: colonna-end:::::: colonna::: assegnazione e uguaglianza. A seconda del contesto, assegna il valore sul lato destro di un'istruzione per l'oggetto sul lato sinistro, o controlla i valori per verificarne l'uguaglianza.
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga:::::: colonna 0x%2 `<>` ::: colonna-end:::::: colonna 0x%2 disuguaglianza. Restituisce `True` se i valori non sono uguali.
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga::: 0x%2 colonna 0x%2 `< > <= >=` ::: colonna-end:::::: colonna::: minore, maggiore di, minore o uguale a e maggiore o uguale.
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga:::::: colonna 0x%2 `&` ::: colonna-end:::::: colonna::: concatenazione, viene usata per aggiungere le stringhe.
::: colonna-end::: 0x%2 colonna: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga::: 0x%2 colonna 0x%2 `+= -=` ::: colonna-end:::::: colonna::: gli operatori di incremento e decremento, che di addizione e sottrazione 1 (rispettivamente) da una variabile.
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga:::::: colonna 0x%2 `.` ::: colonna-end:::::: colonna 0x%2 punto. Utilizzato per distinguere gli oggetti e proprietà e metodi.
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga:::::: colonna 0x%2 `()` ::: colonna-end:::::: colonna 0x%2 tra parentesi. Utilizzato per le espressioni di raggruppamento, per passare i parametri ai metodi e accedere ai membri di matrici e raccolte.
::: colonna-end::: 0x%2 colonna: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga:::::: colonna 0x%2 `Not` ::: colonna-end:::::: colonna 0x%2 non. Inverte il valore true in false e viceversa. In genere utilizzato come un modo abbreviato per verificare la presenza di `False` (ovvero, per non `True`).
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    ::: colonna-end::: 0x%2 riga-end:
* * *
0x%2 riga::: 0x%2 colonna 0x%2 `AndAlso OrElse` ::: colonna-end:::::: colonna::: logico e in alternativa, che permettono di collegare le condizioni di.
::: colonna-end::: 0x%2 colonna: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    ::: colonna-end::: 0x%2 riga-end:

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
| Percorso virtuale | */humanresources/CompanyPolicy.htm* |
| Percorso fisico | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

La directory principale virtuale /, come la radice dell'unità c: unità \. (I percorsi delle cartelle virtuali sempre utilizzano le barre). Il percorso virtuale di una cartella non deve avere lo stesso nome della cartella fisica; può trattarsi di un alias. (Nei server di produzione, il percorso virtuale raramente corrisponde a un percorso fisico esatto)

Quando si lavora con i file e cartelle nel codice, in alcuni casi è necessario fare riferimento al percorso fisico e talvolta un percorso virtuale, a seconda di quali oggetti si sta lavorando. ASP.NET fornisce questi strumenti per l'utilizzo di percorsi di file e cartelle nel codice: il `Server.MapPath` (metodo) e `~` operatore e `Href` metodo.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Conversione di percorsi virtuali per fisica: il metodo MapPath

Il `Server.MapPath` metodo converte un percorso virtuale (ad esempio */default.cshtml*) in un percorso fisico assoluto (ad esempio *C:\WebSites\MyWebSiteFolder\default.cshtml*). Utilizzare questo metodo ogni volta che è necessario un percorso fisico completo. Un esempio tipico è quando la lettura o scrittura di un file di testo o un file di immagine nel server web.

In genere non conosce il percorso fisico assoluto del sito nel server del sito di hosting, in modo da questo metodo consente di convertire il percorso presente, il percorso virtuale, nel percorso corrispondente nel server di. Si passa il percorso virtuale per un file o una cartella per il metodo e restituisce il percorso fisico:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>La radice virtuale di riferimento: il ~ (operatore) e il metodo di Href

In un *. cshtml* o *. vbhtml* file, è possibile fare riferimento il percorso radice virtuale utilizzando il `~` operatore. Questo è molto utile poiché pagine è possibile spostarsi in un sito e tutti i collegamenti che ad altre pagine contengono non verrà interrotti. È inoltre utile nel caso in cui si sposta mai il sito Web in un percorso diverso. Ecco alcuni esempi:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Se il sito Web è `http://myserver/myapp`, ecco come ASP.NET tratterà questi percorsi quando viene eseguita la pagina:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Questi percorsi effettivamente non verrà visualizzato come i valori della variabile, ma ASP.NET considererà i percorsi che come se si trattasse).

È possibile utilizzare il `~` operatore nel codice server (vedere sopra) e nel markup, simile al seguente:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

Nel markup, utilizzare il `~` operatore per creare percorsi a risorse quali file di immagine, altre pagine web e file CSS. Quando la pagina viene eseguita, ASP.NET esegue la ricerca tramite la pagina (codice e markup) e risolve tutti i `~` riferimenti al percorso appropriato.

## <a name="conditional-logic-and-loops"></a>Cicli e logica condizionale

Il codice server ASP.NET consente di eseguire attività in base alle condizioni e scrivere il codice che si ripete le istruzioni che del codice, ovvero un numero specifico di volte che viene eseguito un ciclo).

### <a name="testing-conditions"></a>Condizioni di test

Per testare una condizione semplice è usare il `If...Then` istruzione che restituisce `True` o `False` in base a un test specificato:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

Il `If` (parola chiave) viene avviato un blocco. Il test effettivo (condizione) segue il `If` (parola chiave) e restituisce true o false. Il `If` termina con l'istruzione `Then`. Sono racchiusi tra le istruzioni che verranno eseguito se il test è true `If` e `End If`. Un `If` istruzione può includere un `Else` blocco di istruzioni da eseguire se la condizione è false specifica:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Se un `If` istruzione avvia un blocco di codice, non è necessario utilizzare la normale `Code...End Code` istruzioni per includere i blocchi. È sufficiente aggiungere `@` al blocco, e che funzioni correttamente. Questo approccio funziona con `If` nonché altre parole chiave che sono seguite da blocchi di codice, tra cui di programmazione Visual Basic `For`, `For Each`, `Do While`e così via.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

È possibile aggiungere più condizioni utilizzando uno o più `ElseIf` blocchi:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

In questo esempio, se la prima condizione nella `If` blocco non è true, il `ElseIf` condizione è verificata. Se tale condizione viene soddisfatta, le istruzioni di `ElseIf` blocco vengono eseguite. Se nessuna delle condizioni vengono soddisfatte, le istruzioni di `Else` blocco vengono eseguite. È possibile aggiungere un numero qualsiasi di `ElseIf` blocca e quindi chiudere con un `Else` blocco come il &quot;tutto il resto&quot; condizione.

Per testare un numero elevato di condizioni, usare un `Select Case` blocco:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Il valore da verificare è racchiuso tra parentesi (nell'esempio, la variabile del giorno della settimana). Ogni singolo test utilizza un `Case` istruzione contenente un valore. Se il valore di un `Case` istruzione corrisponde al valore di test, il codice che `Case` blocco viene eseguito.

Il risultato delle ultime due blocchi condizionali visualizzati in un browser:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Codice di ciclo

È spesso necessario eseguire le stesse istruzioni ripetutamente. Questo scopo è di ciclo. Ad esempio, si esegue spesso le stesse istruzioni per ogni elemento in una raccolta di dati. Se si conosce esattamente il numero di volte che si desidera eseguire il ciclo, è possibile utilizzare un `For` ciclo. Questo tipo di ciclo è particolarmente utile per il conteggio di o di conteggio verso il basso:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

Il ciclo inizia con il `For` (parola chiave), seguito da tre elementi:

- Immediatamente dopo il `For` l'istruzione, si dichiara una variabile contatore (non è necessario utilizzare `Dim`) e quindi indicare l'intervallo, come in `i = 10 to 20`. Ciò significa che la variabile `i` Avvia conteggio 10 e continuare finché raggiunge 20 (inclusi).
- Tra le `For` e `Next` istruzioni è il contenuto del blocco. Può contenere uno o più istruzioni di codice eseguite a ogni ciclo.
- Il `Next i` istruzione termina il ciclo. Incrementa il contatore e inizia l'iterazione successiva del ciclo.

La riga di codice tra le `For` e `Next` righe contiene il codice eseguito per ogni iterazione del ciclo. Il codice crea un nuovo paragrafo (`<p>` elemento) ogni e aggiunge una riga all'output, visualizzando il valore dei (il contatore). Quando si esegue questa pagina, l'esempio crea 11 righe di visualizzazione dell'output, con il testo in ogni riga che indica il numero dell'elemento.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Se si lavora con una raccolta o una matrice, utilizzare spesso un `For Each` ciclo. Una raccolta è un gruppo di oggetti simili e `For Each` ciclo consente eseguire un'attività su ogni elemento nella raccolta. Questo tipo di ciclo è ideale per le raccolte, perché a differenza di un `For` ciclo, non è necessario incrementare il contatore o impostare un limite. Al contrario, il `For Each` codice ciclo continua semplicemente tramite la raccolta fino al termine.

In questo esempio restituisce gli elementi di `Request.ServerVariables` insieme (che contiene informazioni sul server web). Usa un `For Each` ciclo per visualizzare il nome di ogni elemento creando un nuovo `<li>` elemento in un elenco puntato HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

Il `For Each` parola chiave sia seguita da una variabile che rappresenta un singolo elemento della raccolta (nell'esempio `myItem`), seguito dal `In` (parola chiave), seguito da una raccolta che si desidera scorrere in ciclo. Nel corpo del `For Each` ciclo, è possibile accedere all'elemento corrente utilizzando la variabile dichiarato in precedenza.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Per creare un ciclo più generico, utilizzare il `Do While` istruzione:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Questo ciclo inizia con il `Do While` (parola chiave), seguita da una condizione, seguita dal blocco da ripetere. In genere aumentare di cicli (aggiungere) o decrementare (sottratto) una variabile o un oggetto utilizzato per il conteggio. Nell'esempio di `+=` operatore aggiunge 1 al valore di una variabile ogni volta che il ciclo viene eseguito. (Per decrementare una variabile in un ciclo che conta in senso decrescente, utilizzare l'operatore di decremento `-=`.)

## <a name="objects-and-collections"></a>Oggetti e raccolte

Quasi tutte le funzioni in un sito Web ASP.NET è un oggetto, tra cui la pagina stessa. Questa sezione illustra alcuni oggetti importanti che verranno utilizzate di frequente nel codice.

### <a name="page-objects"></a>Oggetti della pagina

L'oggetto di base in ASP.NET è la pagina. È possibile accedere a proprietà dell'oggetto pagina direttamente senza alcun oggetto originale. Il codice seguente ottiene il percorso di file della pagina, tramite il `Request` oggetto della pagina:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

È possibile utilizzare le proprietà del `Page` oggetto per ottenere una grande quantità di informazioni, ad esempio:

- `Request`. Come già visto, questa è una raccolta di informazioni relative alla richiesta corrente, inclusi il tipo di browser ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente, e così via.
- `Response`. Si tratta di una raccolta di informazioni sulla risposta che verrà inviata al browser al termine dell'esecuzione il codice del server (pagina). Ad esempio, è possibile utilizzare questa proprietà per scrivere informazioni nella risposta.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Oggetti della raccolta (matrici e dizionari)

Una raccolta è un gruppo di oggetti dello stesso tipo, ad esempio una raccolta di `Customer` oggetti da un database. ASP.NET contiene molte raccolte predefinite, ad esempio il `Request.Files` insieme.

Si utilizzerà spesso dati in raccolte. Due tipi di raccolte comuni sono la *matrice* e *dizionario*. Una matrice è utile quando si desidera archiviare una raccolta di elementi simili, ma non si desidera creare una variabile separata per ogni elemento di contenere:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Con le matrici, si dichiara un tipo di dati specifici, ad esempio `String`, `Integer`, o `DateTime`. Per indicare che la variabile può contenere una matrice, il nome della variabile nella dichiarazione è aggiungere le parentesi (ad esempio `Dim myVar() As String`). È possibile accedere agli elementi di una matrice con la loro posizione (indice) o tramite il `For Each` istruzione. Gli indici di matrice sono a base zero &#8212; , ovvero il primo elemento è nella posizione 0, il secondo elemento alla posizione 1 e così via.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

È possibile determinare il numero di elementi in una matrice tramite il recupero dei relativi `Length` proprietà. Per ottenere la posizione di un elemento specifico della matrice (ovvero, la ricerca nella matrice), utilizzare il `Array.IndexOf` metodo. È inoltre possibile eseguire operazioni come reverse il contenuto di una matrice (il `Array.Reverse` (metodo)) o ordinare il contenuto (il `Array.Sort` (metodo)).

L'output del codice di matrice di stringa visualizzato in un browser:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Un dizionario è una raccolta di coppie chiave/valore, in cui fornire la chiave (o nome) per impostare o recuperare il valore corrispondente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Per creare un dizionario, utilizzare il `New` (parola chiave) per indicare che si sta creando un nuovo `Dictionary` oggetto. È possibile assegnare un dizionario a una variabile utilizzando il `Dim` (parola chiave). Indicare i tipi di dati degli elementi del dizionario tramite parentesi ( `( )` ). Alla fine della dichiarazione, è necessario aggiungere un'altra coppia di parentesi, poiché si tratta in realtà un metodo che crea un nuovo dizionario.

Per aggiungere elementi al dizionario, è possibile chiamare il `Add` metodo per la variabile del dizionario (`myScores` in questo caso), quindi specificare una chiave e un valore. In alternativa, è possibile utilizzare parentesi per indicare la chiave ed eseguire una semplice assegnazione, come nell'esempio seguente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Per ottenere un valore dal dizionario, specificare la chiave tra parentesi:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Chiamata di metodi con parametri

Come illustrato in precedenza in questo articolo, gli oggetti che si programma con dispongono di metodi. Ad esempio, un `Database` oggetto potrebbe avere un `Database.Connect` metodo. Molti metodi dispongono anche di uno o più parametri. Oggetto *parametro* è un valore che viene passato a un metodo per abilitare il metodo completare l'attività. Ad esempio, una dichiarazione per esaminare il `Request.MapPath` metodo che accetta tre parametri:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Questo metodo restituisce il percorso fisico sul server che corrisponde a un percorso virtuale specificato. I tre parametri per il metodo sono `virtualPath`, `baseVirtualDir`, e `allowCrossAppMapping`. Si noti che nella dichiarazione, i parametri sono elencati con i tipi di dati dei dati che verrà accettano. Quando si chiama questo metodo, è necessario fornire valori per tutti e tre i parametri.

Quando si utilizza Visual Basic con sintassi Razor, sono disponibili due opzioni per passare parametri a un metodo: *parametri posizionali* o *parametri denominati*. Per chiamare un metodo utilizzando i parametri posizionali, passare i parametri in un ordine fisso specificato nella dichiarazione del metodo. (È necessario in genere conoscere questo ordine leggendo la documentazione relativa al metodo.) È necessario seguire l'ordine e non è possibile omettere i parametri &#8212; se necessario, si passa una stringa vuota (`""`) o null per un parametro posizionale che non si dispone di un valore per.

Nell'esempio seguente si presuppone che si dispone di una cartella denominata *script* nel sito Web. Il codice chiama il `Request.MapPath` metodo e passa i valori per i tre parametri nell'ordine corretto. Viene quindi visualizzato il percorso mappato risulta.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Quando sono presenti numerosi parametri per un metodo, è possibile mantenere il codice più semplice e più leggibili utilizzando parametri denominati. Per chiamare un metodo utilizzando parametri denominati, specificare il nome del parametro seguito da `:=` e quindi fornire il valore. Un vantaggio di parametri denominati è che è possibile aggiungerli in qualsiasi ordine desiderato. (Uno svantaggio è che la chiamata al metodo non compressi).

Nell'esempio seguente chiama il metodo stesso come illustrato in precedenza, ma Usa parametri per fornire i valori denominati:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Come si può notare, i parametri vengono passati in un ordine diverso. Tuttavia, se si esegue l'esempio precedente e in questo esempio, verrà restituiscono lo stesso valore.

## <a name="handling-errors"></a>Gestione degli errori

### <a name="try-catch-statements"></a>Istruzioni Try-Catch

È necessario spesso istruzioni nel codice che potrebbe non riuscire per motivi all'esterno del controllo. Ad esempio:

- Se il codice tenta di aprire, creare, leggere o scrivere un file, potrebbero verificarsi tutti i tipi di errori. Il file desiderato potrebbe non esistere, potrebbe essere stato bloccato, il codice potrebbe non disporre delle autorizzazioni e così via.
- Analogamente, se il codice tenta di aggiornare i record in un database, possono essere presenti problemi relativi alle autorizzazioni, la connessione al database potrebbe essere rimossi, i dati da salvare potrebbero essere non valido e così via.

In termini di programmazione, vengono chiamate queste situazioni *eccezioni*. Se il codice rileva un'eccezione, (genera) genera un messaggio di errore che, nella migliore delle ipotesi, indesiderate agli utenti.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

In situazioni in cui il codice che si verifichino eccezioni e per evitare messaggi di errore di questo tipo, è possibile utilizzare `Try/Catch` istruzioni. Nel `Try` istruzione, eseguire il codice che si sta archiviando. In uno o più `Catch` istruzioni, è possibile cercare specifici errori che potrebbero essersi verificati (tipi specifici di eccezioni). È possibile includere un numero `Catch` istruzioni è necessario cercare gli errori che sta prevedendo.

> [!NOTE]
> È consigliabile evitare di utilizzare il `Response.Redirect` metodo `Try/Catch` istruzioni, poiché potrebbe causare un'eccezione della pagina.


Nell'esempio seguente viene mostrata una pagina che crea un file di testo alla prima richiesta e quindi visualizza un pulsante che consente all'utente di aprire il file. L'esempio è deliberatamente Usa un nome di file non valido in modo che genera un'eccezione. Il codice include `Catch` istruzioni per due possibili eccezioni: `FileNotFoundException`, che si verifica se il nome del file è danneggiato, e `DirectoryNotFoundException`, che si verifica se ASP.NET non è possibile anche trovare la cartella. (È possibile rimuovere il commento nell'esempio, un'istruzione per vedere come viene eseguito quando tutto funziona correttamente.)

Se il codice non gestisce l'eccezione, si vedrà una pagina di errore come schermata precedente. Tuttavia, il `Try/Catch` sezione consente di impedire all'utente di visualizzare questi tipi di errori.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

### <a name="reference-documentation"></a>Documentazione di riferimento

- [ASP.NET 2.0](https://msdn.microsoft.com/library/ee532866.aspx)
- [Linguaggio Visual Basic](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
