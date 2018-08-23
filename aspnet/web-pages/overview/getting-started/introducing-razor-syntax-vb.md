---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introduzione alla programmazione Web ASP.NET usando la sintassi Razor (Visual Basic) | Microsoft Docs
author: tfitzmac
description: In questa appendice offre una panoramica della programmazione con pagine Web ASP.NET in Visual Basic usando la sintassi Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: cbec035533c37723afcd5bf4aa0c6e1c83dbae23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832120"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Introduzione alla programmazione Web ASP.NET usando la sintassi Razor (Visual Basic)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo offre una panoramica della programmazione con ASP.NET Web Pages con sintassi Razor e Visual Basic. ASP.NET è la tecnologia Microsoft per l'esecuzione di pagine web dinamiche nei server web.
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


La maggior parte degli esempi dell'uso di ASP.NET Web Pages con sintassi Razor usano c#. Ma la sintassi Razor supporta anche Visual Basic. Per programmare una pagina web ASP.NET in Visual Basic, si crea una pagina web con un *vbhtml* estensione nome file, quindi aggiungere il codice Visual Basic. Questo articolo offre una panoramica dell'utilizzo con la sintassi per creare pagine Web ASP.NET e il linguaggio Visual Basic.

> [!NOTE]
> I modelli di sito Web predefinito di Microsoft WebMatrix (**panificio**, **raccolta foto**, e **Starter Site**e così via) sono disponibili nelle versioni c# e Visual Basic. È possibile installare i modelli di Visual Basic da come pacchetti NuGet. Modelli di siti Web vengono installati nella cartella radice del sito in una cartella denominata *Templates Microsoft*.


## <a name="the-top-8-programming-tips"></a>8 suggerimenti di programmazione principali

In questa sezione sono elencati alcuni suggerimenti che è assolutamente necessario conoscere quando si inizia a scrivere codice server ASP.NET tramite la sintassi Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Aggiungere codice a una pagina utilizzando il carattere @

Il `@` carattere avvia espressioni inline, i blocchi a istruzione singola e blocchi a più istruzioni:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Il risultato visualizzato in un browser:

![Razor Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **La codifica HTML**
> 
> Quando si visualizza il contenuto in una pagina mediante la `@` carattere, come negli esempi precedenti, ASP.NET codifica in HTML di output. Questa impostazione sostituisce i caratteri riservati HTML (ad esempio `<` e `>` e `&`) con i codici che abilitano i caratteri da visualizzare come caratteri in una pagina web anziché essere interpretato come tag HTML o le entità. Senza codifica HTML, l'output dal codice server non vengano visualizzati correttamente e potrebbe esporre una pagina a rischi di sicurezza.
> 
> Se l'obiettivo consiste nel markup HTML che esegue il rendering dei tag come markup di output (ad esempio `<p></p>` per un paragrafo o `<em></em>` per enfatizzare il testo), vedere la sezione [combinazione di testo, Markup e codice nei blocchi di codice](#BM_CombiningTextMarkupAndCode) più avanti in questo articolo.
> 
> Altre informazioni sulla codifica HTML [utilizzo di form HTML nei siti di ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Si racchiudere i blocchi di codice con il codice... Codice di fine

Un blocco di codice include una o più istruzioni di codice e racchiusa tra le parole chiave `Code` e `End Code`. Posizionare l'apertura `Code` parola chiave immediatamente dopo il `@` carattere &#8212; tra di essi non possono essere presenti spazi vuoti.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Il risultato visualizzato in un browser:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. All'interno di un blocco alla fine si ogni istruzione del codice con un'interruzione di riga

In un blocco di codice Visual Basic, ogni istruzione viene terminata con un'interruzione di riga. (Più avanti nell'articolo si noterà un modo per eseguire il wrapping di un'istruzione di codice lunghi in più righe se necessario.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Si usano le variabili per archiviare i valori

È possibile archiviare i valori in una *variabile*, incluse stringhe, numeri e date, e così via. Si crea una nuova variabile usando la `Dim` (parola chiave). È possibile inserire i valori delle variabili direttamente in una pagina mediante `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Il risultato visualizzato in un browser:

![Razor Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Valori letterali stringa racchiuderlo tra virgolette doppie

Oggetto *stringa* è una sequenza di caratteri che vengono considerati come testo. Per specificare una stringa, si racchiuderlo tra virgolette doppie:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Per incorporare le virgolette doppie all'interno di un valore stringa, inserire due caratteri di virgoletta doppia. Se si desidera che il carattere di virgoletta doppia presente una volta nell'output della pagina, immettere il valore `""` all'interno di virgolette di stringhe e se si desidera venga visualizzato due volte, immetterlo come `""""` all'interno della stringa tra virgolette.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Il risultato visualizzato in un browser:

![Razor Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Codice Visual Basic non fa distinzione maiuscole / minuscole

Il linguaggio Visual Basic non distinzione maiuscole / minuscole. Parole chiave di programmazione (ad esempio `Dim`, `If`, e `True`) e i nomi delle variabili (come `myString`, o `subTotal`) possono essere scritti in ogni caso.

Le seguenti righe di codice assegna un valore alla variabile `lastname` usando una minuscola assegnare un nome e quindi restituito il valore della variabile di pagina utilizzando un nome di lettere maiuscole.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Il risultato visualizzato in un browser:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Gran parte della codifica implica l'utilizzo di oggetti

Un oggetto rappresenta una cosa che è possibile programmare con &#8212; una pagina di una casella di testo, un file, un'immagine, una richiesta web, un messaggio di posta elettronica, un record del cliente (riga del database), e così via. Gli oggetti hanno proprietà che descrivono le caratteristiche di &#8212; dispone di un oggetto casella di testo una `Text` proprietà, un oggetto della richiesta ha un `Url` proprietà, un messaggio di posta elettronica ha una `From` proprietà e un oggetto customer ha una `FirstName` proprietà. Oggetti contengono anche i metodi che sono le &quot;verbi&quot; possono eseguire. Un oggetto di file sono esempi `Save` metodo, dell'oggetto image `Rotate` metodo e un oggetto messaggio di posta elettronica `Send` (metodo).

Spesso si userà il `Request` i campi oggetto, che fornisce informazioni quali i valori del form nella pagina (caselle di testo, e così via), il tipo di browser ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via. In questo esempio viene illustrato come accedere alle proprietà del `Request` oggetto e come chiamare il `MapPath` metodo il `Request` oggetto, che fornisce il percorso assoluto della pagina nel server:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Il risultato visualizzato in un browser:

![Razor Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. È possibile scrivere codice che prende decisioni

Una funzionalità chiave di pagine web dinamiche è che è possibile determinare quali operazioni eseguire in base alle condizioni. Il modo più comune per eseguire questa operazione è con il `If` istruzione (e facoltative `Else` istruzione).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

L'istruzione `If IsPost` è un modo abbreviato per la scrittura `If IsPost = True`. Insieme a `If` istruzioni, esistono diversi modi per verificare le condizioni, ripetere i blocchi di codice, e così via, che sono descritte più avanti in questo articolo.

Il risultato visualizzato in un browser (dopo aver fatto clic **Submit**):

![Razor Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET e i metodi POST e la proprietà istruzione IsPost**
> 
> Il protocollo usato per le pagine web (HTTP) supporta un numero molto limitato di metodi (&quot;verbi&quot;) che consentono di effettuare richieste al server. Le due cause più comuni sono GET, che viene usato per leggere una pagina, e POST, che consente di inviare una pagina. In generale, la prima volta che un utente richiede una pagina, la pagina viene richiesta con GET. Se l'utente inserisce in un form e quindi fa clic su **Submit**, il browser invia una richiesta POST al server.
> 
> Nella programmazione web, è spesso utile sapere se una pagina viene richiesta come un'operazione GET o un POST in modo da sapere come elaborare la pagina. In ASP.NET Web Pages, è possibile usare il `IsPost` proprietà per vedere se una richiesta è un'operazione GET o POST. Se la richiesta viene pubblicato un POST, il `IsPost` proprietà restituirà true ed è possibile eseguire operazioni come leggere i valori di caselle di testo in un form. Molti esempi verrà visualizzato mostrano come elaborare la pagina in modo diverso a seconda del valore di `IsPost`.


## <a name="a-simple-code-example"></a>Un semplice esempio di codice

Questa procedura illustra come creare una pagina che illustra le tecniche di programmazione di base. Nell'esempio, si crea una pagina che consente agli utenti di immettere due numeri e quindi li aggiunge e viene visualizzato il risultato.

1. Nell'editor di creare un nuovo file e denominarlo *AddNumbers.vbhtml*.
2. Copiare il codice e il markup seguente alla pagina, sostituendo qualsiasi elemento presente nella pagina.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Ecco alcuni aspetti notare:

    - Il `@` carattere viene avviato il primo blocco di codice nella pagina e precede la `totalMessage` variabile incorporato nella parte inferiore.
    - Il blocco nella parte superiore della pagina è racchiuso tra parentesi `Code...End Code`.
    - Le variabili `total`, `num1`, `num2`, e `totalMessage` archiviare diversi numeri e una stringa.
    - Il valore di stringa letterale assegnato al `totalMessage` variabile è racchiuso tra virgolette doppie.
    - Poiché il codice Visual Basic viene fatta distinzione tra maiuscole e minuscole, non quando il `totalMessage` variabile viene utilizzata nella parte inferiore della pagina, il relativo nome deve solo in modo che corrisponda l'ortografici di dichiarazione di variabile nella parte superiore della pagina. Le maiuscole e minuscole è irrilevante.
    - L'espressione `num1.AsInt()`  +  `num2.AsInt()` viene illustrato come lavorare con gli oggetti e metodi. Il `AsInt` metodo su ogni variabile Converte la stringa immessa dall'utente a un numero intero (integer) che possono essere aggiunti.
    - Il `<form>` tag include un `method="post"` attributo. Specifica che quando l'utente sceglie **Add**, la pagina verrà inviata al server usando il metodo HTTP POST. Quando l'invio della pagina, il codice `If IsPost` restituisce true, il parametro condizionale viene eseguito, visualizzare il risultato della somma i numeri di codice.
3. Salvare la pagina ed eseguirlo in un browser. (Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.) Immettere due numeri interi, quindi scegliere il **Add** pulsante.

    ![Razor Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>La sintassi e linguaggio Visual Basic

In precedenza è stato illustrato un esempio su come creare una pagina web ASP.NET e come è possibile aggiungere il codice lato server per il markup HTML. Qui si apprenderanno i concetti fondamentali dell'utilizzo di Visual Basic per scrivere il codice server ASP.NET tramite la sintassi Razor &#8212; , ovvero le regole del linguaggio di programmazione.

Se è esperti di programmazione (in particolare se è stato usato C, C++, c#, Visual Basic o JavaScript), gran parte delle quali è leggere qui risulteranno familiare. Si sarà probabilmente necessario acquisire familiarità con solo come markup in viene aggiunto codice WebMatrix *vbhtml* file.

### <a id="BM_CombiningTextMarkupAndCode"></a>  Combinazione di testo, markup e codice nei blocchi di codice

Nei blocchi di codice server, è spesso opportuno per l'output di testo e markup alla pagina. Se un blocco di codice server contiene testo che non è codice e che invece deve essere eseguito il rendering è, ASP.NET deve essere in grado di distinguere tale testo dal codice. Esistono diversi modi per eseguire tale operazione.

- Racchiudere il testo in un blocco di HTML, ad esempio `<p></p>` o `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    L'elemento HTML può includere testo, altri elementi HTML ed espressioni di codice lato server. Se ASP.NET rileva il tag HTML di apertura (ad esempio, `<p>`), esegue il rendering di tutti gli elementi dell'elemento e il proprio contenuto come consiste nel browser (e risolve le espressioni di codice lato server).

- Usare la `@:` operatore o `<text>` elemento. Il `@:` restituisce una singola riga di contenuto che contengono testo normale o tag HTML senza corrispondenza; il `<text>` elemento racchiude più righe di output. Queste opzioni sono utili quando non si vuole eseguire il rendering di un elemento HTML come parte dell'output.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    Nell'esempio seguente ripete l'esempio precedente ma usa una singola coppia di `<text>` tag per racchiudere il testo per il rendering.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    Nell'esempio seguente, il `<text>` e `</text>` tag racchiudono tre righe, ognuna delle quali dispone di testo non contenuto e i tag HTML non corrispondenti (`<br />`), insieme a codice lato server e i tag HTML corrispondenti. Anche in questo caso, è possibile anche far precedere a ogni riga singolarmente con il `@:` operatore; entrambi funzionano modo.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Quando il testo è l'output come mostrato in questa sezione &#8212; utilizzando un elemento HTML, il `@:` operatore o il `<text>` elemento &#8212; ASP.NET non di codifica HTML di output. (Come indicato in precedenza, ASP.NET codificare l'output di espressioni di codice server e i blocchi di codice server preceduti da `@`, ad eccezione dei casi speciali elencati in questa sezione.)

### <a name="whitespace"></a>Whitespace

Gli spazi aggiuntivi in un'istruzione (e all'esterno di un valore letterale stringa) non influiscono sull'istruzione:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Suddividere lunghe istruzioni in più righe

È possibile suddividere un'istruzione di codice lunghi in più righe utilizzando il carattere di sottolineatura `_` (che in Visual Basic viene chiamato il *carattere di continuazione*) dopo ogni riga di codice. Per interrompere un'istruzione alla riga successiva, aggiungere uno spazio e quindi il carattere di continuazione alla fine della riga. Continuare l'istruzione nella riga successiva. È possibile eseguire il wrapping di istruzioni in tutte le righe in base alle esigenze migliorare la leggibilità. Le istruzioni seguenti sono gli stessi:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

È, tuttavia, non è possibile eseguire il wrapping di una riga all'interno di un valore letterale stringa. Nell'esempio seguente non funziona:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Per combinare una stringa lunga che esegue il wrapping su più righe, ad esempio il codice sopra riportato, è necessario utilizzare il *operatore di concatenazione* (`&`), si vedrà più avanti in questo articolo.

### <a name="code-comments"></a>Commenti del codice

Commenti è possibile lasciare note per se stessi o ad altri utenti. I commenti di sintassi Razor sono preceduti `@*` e terminare con `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

All'interno di blocchi di codice è possibile usare i commenti di sintassi Razor, o è possibile usare il carattere ordinario Visual Basic commento, ovvero una virgoletta singola (`'`) come prefisso a ogni riga.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Variabili

Una variabile è un oggetto denominato che viene usato per archiviare i dati. È possibile assegnare variabili di qualsiasi oggetto, ma il nome deve iniziare con un carattere alfabetico e non può contenere spazi vuoti o caratteri riservati. In Visual Basic, come illustrato in precedenza, non è rilevante nel caso delle lettere nel nome di una variabile.

### <a name="variables-and-data-types"></a>Le variabili e tipi di dati

Una variabile può avere un tipo di dati specifico, che indica il tipo di dati viene archiviato nella variabile. È possibile avere le variabili di stringa che archiviano i valori stringa (ad esempio &quot;Hello world&quot;), le variabili integer che archiviano valori di numeri interi (ad esempio, 3 o 79) e le variabili di date che archiviano i valori delle date in un'ampia gamma di formati (ad esempio, marzo 2009 o 4/12/2012 ). Ed esistono molti altri tipi di dati che è possibile usare.

Tuttavia, non devi specificare un tipo per una variabile. Nella maggior parte dei casi ASP.NET può determinare il tipo di base sul modo in cui vengono usati i dati nella variabile. (In alcuni casi è necessario specificare un tipo; sono riportati esempi in cui questo è vero).

Per dichiarare una variabile senza specificare un tipo, usare `Dim` più il nome di variabile (ad esempio, `Dim myVar`). Per dichiarare una variabile con un tipo, usare `Dim` più il nome della variabile, seguito da `As` e quindi il nome del tipo (ad esempio, `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

Nell'esempio seguente mostra alcune espressioni inline che usano le variabili in una pagina web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Il risultato visualizzato in un browser:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>La conversione e i tipi di dati di test

Anche se ASP.NET in genere possibile determinare automaticamente un tipo di dati, a volte questo non è possibile. Pertanto, è necessario supporto ASP.NET mediante l'esecuzione di una conversione esplicita. Anche se non è necessario convertire i tipi, a volte è utile eseguire un test per vedere quali tipi di dati si lavora con.

Il caso più comune è che è necessario convertire una stringa in un altro tipo, ad esempio in un numero intero o Data. Nell'esempio seguente viene illustrato un caso tipico in cui è necessario convertire una stringa in un numero.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Di norma, input dell'utente vengono recapitati come stringhe. Anche se è stato richiesto all'utente di immettere un numero e anche se è stato immesso una cifra, quando viene inviato l'input dell'utente e di leggerlo nel codice, i dati sono in formato stringa. Pertanto, è necessario convertire la stringa in un numero. Nell'esempio, se si prova a eseguire operazioni aritmetiche sui valori senza doverle convertire in, l'errore seguente generato, perché ASP.NET non è possibile aggiungere due stringhe:

`Cannot implicitly convert type 'string' to 'int'.`

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
        Converte una stringa che rappresenta un numero intero (ad esempio &quot;593&quot;) in un numero intero.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::


## <a name="operators"></a>Operatori

Un operatore è una parola chiave o un carattere che indica ad ASP.NET che tipo di comando da eseguire in un'espressione. Visual Basic supporta molti operatori, ma è sufficiente riconoscere alcune per iniziare a sviluppare le pagine web ASP.NET. Nella tabella seguente sono riepilogati gli operatori più comuni.


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
        `+ - * /`
    :::column-end:::
    :::column:::
        Operatori matematici di utilizzati nelle espressioni numeriche.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Assegnazione e verificarne l'uguaglianza. A seconda del contesto, assegna il valore sul lato destro di un'istruzione per l'oggetto sul lato sinistro, o controlla i valori per verificarne l'uguaglianza.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Disuguaglianza. Restituisce `True` se i valori non sono uguali.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Minore di, maggiore di, minore o uguale a e maggiore o uguale.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Concatenazione, che consente di unire le stringhe.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        Gli operatori di incremento e decremento, quali addizioni e sottrazioni 1 (rispettivamente) da una variabile.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parentesi. Utilizzato per le espressioni di raggruppamento, per passare parametri a metodi e accedere ai membri di matrici e raccolte.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        No. Inverte il valore true in false e viceversa. In genere utilizzato come un modo abbreviato per testare `False` (vale a dire, per non `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        AND logico e o, che vengono utilizzati per collegare le condizioni.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

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

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>La radice virtuale di riferimento: il ~ operatore e il metodo di Href

In un *. cshtml* oppure *vbhtml* file, è possibile fare riferimento nel percorso radice virtuale usando il `~` operatore. Ciò è molto utile perché pagine è possibile spostarsi in un sito e tutti i collegamenti che ad altre pagine contengono non saranno interrotti. È anche utile nel caso in cui si sposta mai il sito Web in un percorso diverso. Ecco alcuni esempi:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Se il sito Web è `http://myserver/myapp`, ecco come ASP.NET tratterà questi percorsi quando viene eseguita la pagina:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Non viene effettivamente visualizzato questi percorsi come i valori della variabile, ma ASP.NET considererà i percorsi come se questo è ciò che fossero).

È possibile usare il `~` operatore nel codice server (come sopra) sia nel markup, simile al seguente:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

Nel markup, si utilizza il `~` operatore per creare percorsi di risorse, ad esempio i file di immagine, altre pagine web e file CSS. Quando viene eseguita la pagina, ASP.NET cerca tramite la pagina (codice e markup) e risolve tutti i `~` riferimenti al percorso appropriato.

## <a name="conditional-logic-and-loops"></a>I cicli e la logica condizionale

Codice server ASP.NET consente di eseguire attività in base a condizioni e scrivere il codice che si ripete le istruzioni di codice, ovvero un numero specifico di volte che viene eseguito un ciclo).

### <a name="testing-conditions"></a>Le condizioni di test

Per testare una condizione semplice è usare il `If...Then` istruzione che restituisce `True` o `False` basato su un test specificato:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

Il `If` (parola chiave) viene avviato un blocco. Il test effettivo (condizione) segue il `If` (parola chiave) e restituisce true o false. Il `If` termina con l'istruzione `Then`. Le istruzioni che verranno eseguita se il test è true siano racchiusi `If` e `End If`. Un' `If` istruzione può includere un `Else` blocco che consente di specificare istruzioni da eseguire se la condizione è false:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Se un' `If` istruzione avvia un blocco di codice, non è necessario usare la normale `Code...End Code` istruzioni per includere i blocchi. È possibile aggiungere semplicemente `@` al blocco, e il corretto funzionamento. Questo approccio funziona con `If` , nonché altre parole chiave che sono seguite da blocchi di codice, tra cui di programmazione Visual Basic `For`, `For Each`, `Do While`e così via.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

È possibile aggiungere più condizioni usando uno o più `ElseIf` blocchi:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

In questo esempio, se la prima condizione nella `If` blocco non è impostato su true, il `ElseIf` condizione viene confrontata. Se tale condizione viene soddisfatta, le istruzioni di `ElseIf` blocco vengono eseguite. Se nessuna delle condizioni viene soddisfatta, le istruzioni di `Else` blocco vengono eseguite. È possibile aggiungere un numero qualsiasi di `ElseIf` blocca e quindi chiudere con un' `Else` block come le &quot;tutto il resto&quot; condizione.

Per testare un numero elevato di condizioni, usare un `Select Case` blocco:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Valore da testare è racchiuso tra parentesi (nell'esempio, la variabile del giorno della settimana). Ogni singolo test viene utilizzato un `Case` istruzione contenente un valore. Se il valore di una `Case` istruzione corrisponde al valore di test, il codice che `Case` blocco viene eseguito.

Il risultato di ultimi due blocchi condizionali visualizzati in un browser:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Codice di ciclo

È spesso necessario eseguire ripetutamente le stesse istruzioni. Eseguire questa operazione dal ciclo. Ad esempio, si esegue spesso le stesse istruzioni per ogni elemento in una raccolta di dati. Se si conosce esattamente quante volte si desidera eseguire un ciclo, è possibile usare un `For` ciclo. Questo tipo di ciclo è particolarmente utile per il conteggio dei o il conteggio verso il basso:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

Il ciclo inizia con la `For` (parola chiave), seguita da tre elementi:

- Immediatamente dopo il `For` istruzione, si dichiara una variabile contatore (non è necessario usare `Dim`) e quindi indicare l'intervallo, come in `i = 10 to 20`. Ciò significa che la variabile `i` verrà avviare il conteggio da 10 e continuare fino a raggiungere 20 (inclusivo).
- Tra i `For` e `Next` istruzioni è il contenuto del blocco. Può contenere uno o più istruzioni di codice che eseguono con ogni ciclo.
- Il `Next i` istruzione termina il ciclo. Incrementa il contatore e inizia l'iterazione successiva del ciclo.

La riga di codice tra il `For` e `Next` righe contiene il codice eseguito per ogni iterazione del ciclo. Il codice crea un nuovo paragrafo (`<p>` elemento) ogni volta e si aggiunge una riga all'output, visualizzando il valore dei (il contatore). Quando si esegue questa pagina, l'esempio crea 11 righe visualizzando l'output, con il testo in ogni riga che indica il numero dell'elemento.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Se si lavora con una raccolta o una matrice, usano spesso un `For Each` ciclo. Una raccolta è un gruppo di oggetti simili e il `For Each` ciclo consente eseguire un'attività su ogni elemento nella raccolta. Questo tipo di ciclo è utile per le raccolte, perché a differenza di un `For` ciclo, non è necessario incrementare il contatore o impostare un limite. Al contrario, il `For Each` codice del ciclo procede semplicemente tramite la raccolta venga completata.

Gli elementi in questo esempio vengono restituite le `Request.ServerVariables` insieme (che contiene informazioni relative al server web). Usa un' `For Each` per visualizzare il nome di ogni elemento creando un nuovo ciclo `<li>` elemento in un elenco puntato di HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

Il `For Each` parola chiave è seguita da una variabile che rappresenta un singolo elemento della raccolta (nell'esempio `myItem`), seguito dal `In` (parola chiave), la raccolta che si desidera scorrere in ciclo. Nel corpo del `For Each` ciclo, è possibile accedere all'elemento corrente usando la variabile dichiarata in precedenza.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Per creare un ciclo più generico, usare il `Do While` istruzione:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Questo ciclo inizia con la `Do While` (parola chiave), seguita da una condizione, seguita dal blocco di ripetizione. In genere incrementare i cicli (aggiungere) o di decremento (sottrarre) una variabile o un oggetto utilizzato per il conteggio. Nell'esempio di `+=` operatore aggiunge 1 al valore di una variabile ogni volta che viene eseguito il ciclo. (Per decrementano una variabile in un ciclo che conta verso il basso, si utilizzerebbe l'operatore di decremento `-=`.)

## <a name="objects-and-collections"></a>Oggetti e raccolte

Quasi tutti gli elementi in un sito Web ASP.NET è un oggetto, tra cui la pagina web stessa. Questa sezione illustra alcuni oggetti importante che si utilizzerà spesso nel codice.

### <a name="page-objects"></a>Oggetti della pagina

L'oggetto di base in ASP.NET è la pagina. Si può accedere alle proprietà dell'oggetto pagina direttamente, senza alcun oggetto qualificato. Il codice seguente ottiene il percorso di file della pagina, tramite il `Request` oggetto della pagina:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

È possibile usare le proprietà del `Page` oggetto da ottenere molte informazioni, ad esempio:

- `Request`. Come abbiamo già visto, questa è una raccolta di informazioni sulla richiesta corrente, tra cui il tipo di browser ha effettuato la richiesta, l'URL della pagina, l'identità dell'utente e così via.
- `Response`. Si tratta di una raccolta di informazioni sulla risposta che verrà inviata al browser al termine dell'esecuzione il codice del server (pagina). Ad esempio, è possibile usare questa proprietà per scrivere informazioni nella risposta.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Oggetti della raccolta (matrici e dizionari)

Una raccolta è un gruppo di oggetti dello stesso tipo, ad esempio una raccolta di `Customer` oggetti da un database. ASP.NET contiene molte raccolte predefinite, ad esempio il `Request.Files` raccolta.

Sarà spesso lavorano con i dati nelle raccolte. Due tipi di raccolte comuni sono le *matrice* e il *dizionario*. Una matrice è utile quando si vuole archiviare un insieme di elementi simili, ma non si vuole creare una variabile separata per contenere ciascun elemento:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Con le matrici, ad esempio si dichiara un tipo di dati specifico `String`, `Integer`, o `DateTime`. Per indicare che la variabile può contenere una matrice, aggiungere le parentesi per il nome della variabile nella dichiarazione (ad esempio `Dim myVar() As String`). È possibile accedere agli elementi in una matrice usando la loro posizione (indice) o tramite il `For Each` istruzione. Gli indici di matrice sono in base zero in &#8212; , ovvero il primo elemento è nella posizione 0, il secondo elemento alla posizione 1 e così via.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

È possibile determinare il numero di elementi in una matrice tramite il recupero relativo `Length` proprietà. Per ottenere la posizione di un elemento specifico nella matrice (vale a dire, la ricerca nella matrice), usare il `Array.IndexOf` (metodo). È anche possibile eseguire operazioni come reverse il contenuto di una matrice (il `Array.Reverse` metodo) oppure ordinare il contenuto (il `Array.Sort` (metodo)).

L'output del codice di matrice di stringa visualizzato in un browser:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Un dizionario è una raccolta di coppie chiave/valore, in cui è fornire la chiave (o nome) per impostare o recuperare il valore corrispondente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Per creare un dizionario, usare il `New` parola chiave per indicare che si sta creando un nuovo `Dictionary` oggetto. È possibile assegnare un dizionario a una variabile utilizzando la `Dim` (parola chiave). Si indicano i tipi di dati degli elementi nel dizionario usando le parentesi ( `( )` ). Alla fine della dichiarazione, è necessario aggiungere un'altra coppia di parentesi, perché si tratta in realtà un metodo che crea un nuovo dizionario.

Per aggiungere elementi al dizionario, è possibile chiamare il `Add` metodo per la variabile del dizionario (`myScores` in questo caso), quindi specificare una chiave e un valore. In alternativa, è possibile utilizzare le parentesi per indicare la chiave ed eseguire una semplice assegnazione, come nell'esempio seguente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Per ottenere un valore dal dizionario, si specifica la chiave in parentesi:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Chiamata di metodi con parametri

Come illustrato in precedenza in questo articolo, gli oggetti che si programma con dispongono di metodi. Ad esempio, un `Database` oggetto potrebbe avere un `Database.Connect` (metodo). Molti metodi dispongono anche di uno o più parametri. Oggetto *parametro* è un valore che si passa a un metodo per abilitare il metodo completare l'attività. Ad esempio, esaminata una dichiarazione per il `Request.MapPath` metodo che accetta tre parametri:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Questo metodo restituisce il percorso fisico sul server che corrisponde a un percorso virtuale specificato. I tre parametri per il metodo vengono `virtualPath`, `baseVirtualDir`, e `allowCrossAppMapping`. Si noti che nella dichiarazione, i parametri sono elencati con i tipi di dati dei dati che verranno accettano. Quando si chiama questo metodo, è necessario fornire valori per tutti i tre parametri.

Quando si usa Visual Basic con sintassi Razor, sono disponibili due opzioni per passare parametri a un metodo: *parametri posizionali* oppure *parametri denominati*. Per chiamare un metodo usando i parametri posizionali, passare i parametri in un ordine fisso specificato nella dichiarazione del metodo. (È in genere saprebbe quest'ordine leggendo la documentazione relativa al metodo.) È necessario seguire l'ordine e non è possibile ignorare i parametri &#8212; se necessario, si passa una stringa vuota (`""`) o null per un parametro posizionale che non si dispone di un valore per.

Nell'esempio seguente si presuppone una cartella denominata *script* nel tuo sito Web. Il codice chiama il `Request.MapPath` metodo e passa i valori per i tre parametri nell'ordine corretto. Viene quindi visualizzato il percorso risulta mappato.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Quando sono presenti numerosi parametri per un metodo, è possibile mantenere il codice più lineare e più leggibili utilizzando parametri denominati. Per chiamare un metodo utilizzando parametri denominati, specificare il nome del parametro seguito da `:=` e quindi specificare il valore. Un vantaggio dei parametri denominati è che è possibile aggiungerli in qualsiasi ordine desiderato. (Uno svantaggio è che la chiamata al metodo non è compresso).

Nell'esempio seguente chiama il metodo di stesso come illustrato in precedenza, ma utilizza parametri di fornire i valori denominati:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Come può notare, i parametri vengono passati in un ordine diverso. Tuttavia, se si esegue l'esempio precedente e in questo esempio, verrà restituiscono lo stesso valore.

## <a name="handling-errors"></a>Gestione degli errori

### <a name="try-catch-statements"></a>Istruzioni Try-Catch

È necessario spesso istruzioni nel codice che potrebbe non riuscire per motivi di all'esterno del controllo. Ad esempio:

- Se il codice tenta di aprire, creare, leggere o scrivere un file, tutti i tipi di errori potrebbero verificarsi. Il file desiderato potrebbe non esistere, potrebbe essere stato bloccato, il codice potrebbe non disporre delle autorizzazioni e così via.
- Analogamente, se il codice tenta di aggiornare i record in un database, possono essere presenti problemi relativi alle autorizzazioni, la connessione al database potrebbe essere eliminata, i dati da salvare potrebbero essere non valido e così via.

In termini di programmazione, queste situazioni sono denominate *eccezioni*. Se il codice rileva un'eccezione, viene generato (genera) un messaggio di errore nella migliore delle ipotesi, ovvero indesiderate agli utenti.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

In situazioni in cui il codice che si verifichino eccezioni e per evitare messaggi di errore di questo tipo, è possibile usare `Try/Catch` istruzioni. Nel `Try` istruzione, eseguire il codice che si sta archiviando. In uno o più `Catch` (istruzioni), è possibile cercare specifici errori (tipi specifici di eccezioni) che sono stati generati. È possibile includere un numero `Catch` istruzioni è necessario individuare errori che sta previsione.

> [!NOTE]
> È consigliabile evitare di utilizzare il `Response.Redirect` metodo `Try/Catch` (istruzioni), poiché potrebbe causare un'eccezione nella pagina.


Nell'esempio seguente mostra una pagina che consente di creare un file di testo per la prima richiesta e quindi visualizza un pulsante che consente all'utente di aprire il file. L'esempio Usa un nome file errato deliberatamente in modo che lo genererà un'eccezione. Il codice riporta `Catch` istruzioni per due possibili eccezioni: `FileNotFoundException`, che si verifica se il nome del file non è corretto, e `DirectoryNotFoundException`, che si verifica se ASP.NET non è possibile anche trovare la cartella. (È possibile rimuovere il commento nell'esempio, un'istruzione per verificarne l'esecuzione quando tutto funziona correttamente.)

Se il codice non gestisce l'eccezione, si vedrà una pagina di errore, ad esempio lo screenshot precedente. Tuttavia, il `Try/Catch` sezione aiuta a impedire all'utente di visualizzare questi tipi di errori.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

### <a name="reference-documentation"></a>Documentazione di riferimento

- [ASP.NET 2.0](https://msdn.microsoft.com/library/ee532866.aspx)
- [Linguaggio Visual Basic](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
