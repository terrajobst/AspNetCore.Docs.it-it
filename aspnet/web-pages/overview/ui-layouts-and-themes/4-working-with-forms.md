---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Utilizzo di form HTML nei siti ASP.NET Web Pages (Razor) | Documenti Microsoft
author: tfitzmac
description: Un modulo è una sezione di un documento HTML in cui inserire i controlli di input dell'utente, ad esempio elenchi a discesa, caselle di controllo, pulsanti di opzione e caselle di testo. È necessario utilizzare moduli CO...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8579c444fd19d1a366349cc09f9f768de23055f8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Utilizzo di form HTML in siti Web di ASP.NET di pagine (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo articolo viene descritto come elaborare un form HTML (con i pulsanti e caselle di testo) quando si lavora in un sito Web ASP.NET Web Pages (Razor).
> 
> **Illustra quanto segue:** 
> 
> - Come creare un form HTML.
> - Come leggere l'input dell'utente dal modulo.
> - Come convalidare l'input dell'utente.
> - Come ripristinare i valori del form, dopo l'invio della pagina.
> 
> Programmazione di concetti introdotti nell'articolo ASP.NET sono:
> 
> - Oggetto `Request`.
> - Convalida dell'input.
> - La codifica HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


## <a name="creating-a-simple-html-form"></a>Creazione di un Form HTML semplice

1. Creare un nuovo sito Web.
2. Nella cartella radice, creazione di una pagina web denominata *Form.cshtml* e immettere il markup seguente:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Avviare la pagina nel browser. (In WebMatrix, nel **file** area di lavoro, il pulsante destro e quindi selezionare **Avvia nel browser**.) Un modulo semplice con tre campi di input e un **Invia** pulsante viene visualizzato.

    ![Schermata di un form con tre caselle di testo.](4-working-with-forms/_static/image1.jpg)

    A questo punto, se si sceglie il **Invia** pulsante, non accade nulla. Per rendere il modulo è utile, è necessario aggiungere il codice che verrà eseguito nel server.

## <a name="reading-user-input-from-the-form"></a>Leggere l'Input dell'utente dal modulo

Per elaborare il form, Aggiungi il codice che legge i valori dei campi inviati ed esegue un'operazione con essi. In questa procedura viene illustrato come leggere i campi e visualizzare l'input dell'utente della pagina. (In un'applicazione di produzione, generalmente si procede più interessanti con l'input dell'utente. L'utente eseguirà che nell'articolo sull'utilizzo di database.)

1. Nella parte superiore del *Form.cshtml* file, immettere il codice seguente:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Quando l'utente richiede innanzitutto la pagina, viene visualizzato solo il form vuoto. L'utente (che sarà si) compila il modulo e quindi fa clic su **Invia**. In questo modo inviata (POST) all'input dell'utente al server. Per impostazione predefinita, la richiesta viene inviata alla stessa pagina (vale a dire, *Form.cshtml*).

    Quando si invia la pagina questa volta, i valori immessi vengono visualizzati sopra la forma:

    ![Schermata che mostra i valori immessi visualizzati nella pagina.](4-working-with-forms/_static/image2.jpg)

    Esaminare il codice per la pagina. Utilizzare innanzitutto la `IsPost` metodo per determinare se viene inviata la pagina &#8212; vale a dire, se un utente ha selezionato il **Invia** pulsante. Se questa è una richiesta post, `IsPost` restituisce true. Questo è il modo standard nelle pagine Web ASP.NET per determinare se si lavora con una richiesta iniziale (una richiesta GET) o un postback (una richiesta POST). (Per ulteriori informazioni su GET e POST, vedere "HTTP GET e POST e l'istruzione IsPost Property" barra laterale in [Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Successivamente, si otterranno i valori che viene compilato dal `Request.Form` oggetto e inserirli in variabili per un momento successivo. Il `Request.Form` oggetto contiene tutti i valori che sono stati inviati con la pagina, ognuna identificata da una chiave. La chiave è l'equivalente di `name` attributo del campo del form che si desidera leggere. Ad esempio, per leggere il `companyname` campo (casella di testo), utilizzare `Request.Form["companyname"]`.

    I valori del form vengono archiviati nel `Request.Form` oggetti come stringhe. Pertanto, quando si lavora con un valore come numero o una data o un altro tipo, è necessario convertirlo da stringa al tipo. Nell'esempio di `AsInt` metodo il `Request.Form` viene usata per convertire il valore del campo, che contiene un conteggio di dipendenti, i dipendenti in un intero.
2. Avviare la pagina nel browser, compilare i campi del form e fare clic su **Invia**. La pagina Visualizza i valori immessi.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>La codifica per la sicurezza e aspetto HTML
> 
> HTML è speciale viene utilizzato per i caratteri, ad esempio `<`, `>`, e `&`. Se sono presenti questi caratteri speciali in cui non si previste, essi possono compromettere l'aspetto e le funzionalità della pagina web. Ad esempio, il browser interpreta il `<` caratteri (a meno che è seguito da uno spazio) come carattere iniziale di un elemento HTML, ad esempio `<b>` o `<input ...>`. Se il browser non riconosce l'elemento, ignora semplicemente la stringa che inizia con `<` nuovamente finché raggiunge un elemento che esso venga riconosciuto. Ovviamente, questo può comportare alcuni strano per il rendering della pagina.
> 
> La codifica HTML sostituisce questi caratteri riservati con un codice di browser di interpretare come simbolo corretto. Ad esempio, il `<` viene sostituito con `&lt;` e `>` viene sostituito con `&gt;`. Il browser esegue il rendering di queste stringhe di sostituzione come i caratteri che si desidera visualizzare.
> 
> È consigliabile utilizzare ogni volta che si visualizzare stringhe codificate in formato HTML (input) che è stato creato da un utente. In caso contrario, un utente può provare a ottenere la pagina web per eseguire uno script dannoso o altre operazioni che compromette la sicurezza del sito o che non si desidera. (Ciò è particolarmente importante se si accettano input dell'utente, archiviarlo identificabile e quindi visualizzarla in un secondo momento &#8212; , ad esempio, come un commento di blog, revisione utente o molto simile a quello.)
> 
> Per evitare questi problemi, ASP.NET Web Pages automaticamente codifica in HTML qualsiasi testo contenuto che si output dal codice. Ad esempio, quando si visualizza il contenuto di una variabile o un'espressione che utilizza, ad esempio di codice `@MyVar`, ASP.NET Web Pages codifica automaticamente l'output.


## <a name="validating-user-input"></a>Convalida dell'Input utente

Gli utenti commettono errori. Chiedere di riempimento in un campo e ne dimenticassero, o si chiedere loro di immettere il numero di dipendenti e quindi digitare un nome invece. Per assicurarsi che un modulo è stato compilato correttamente prima elaborarlo, convalidare l'input dell'utente.

Questa procedura viene illustrato come convalidare tutti i campi di tre form per assicurarsi che l'utente non lascia il campo vuoto. È inoltre possibile verificare che il valore del conteggio dipendente è un numero. Se sono presenti errori, si verrà visualizzato un errore messaggio che indica all'utente i valori non superare la convalida.

1. Nel *Form.cshtml* file, sostituire il primo blocco di codice con il codice seguente: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Per convalidare l'input dell'utente, utilizzare il `Validation` helper. Registrare i campi obbligatori chiamando `Validation.RequireField`. Registrare altri tipi di convalida chiamando `Validation.Add` e specificando il campo da convalidare e il tipo di convalida da eseguire.

    Quando la pagina viene eseguita, ASP.NET esegue tutti la convalida per l'utente. È possibile controllare i risultati chiamando `Validation.IsValid`, che restituisce true se tutti gli elementi passati e false se qualsiasi campo non è riuscita la convalida. In genere, la chiamata `Validation.IsValid` prima di eseguire qualsiasi elaborazione in input dell'utente.
2. Aggiornamento di `<body>` elemento tramite l'aggiunta di tre chiamate al `Html.ValidationMessage` metodo, simile al seguente:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Per visualizzare i messaggi di errore di convalida, è possibile chiamare Html.`ValidationMessage` e passa il nome del campo che si desidera che il messaggio per.
3. Eseguire la pagina. Lasciare vuoti i campi e fare clic su **Invia**. Vengono visualizzati messaggi di errore.

    ![Schermata che mostra i messaggi di errore visualizzati se l'input dell'utente non supera la convalida.](4-working-with-forms/_static/image3.jpg)
4. Aggiungere una stringa (ad esempio, "ABC") per il **Employee Count** campo e fare clic su **Invia** nuovamente. Questa volta che viene visualizzato un errore che indica che la stringa non è nel formato corretto, vale a dire, un numero intero.

    ![Schermata che mostra i messaggi di errore visualizzati se gli utenti immettere una stringa per il campo di dipendenti.](4-working-with-forms/_static/image4.jpg)

Le pagine Web ASP.NET fornisce più opzioni per la convalida dell'input utente, inclusa la possibilità di eseguire automaticamente la convalida utilizzando lo script client, in modo che gli utenti di ottenere un feedback immediato nel browser. Vedere il [risorse aggiuntive](#Additional_Resources) sezione in un secondo momento per ulteriori informazioni.

## <a name="restoring-form-values-after-postbacks"></a>Ripristinare i valori del Form dopo il postback

Quando il test della pagina nella sezione precedente, è possibile notare che se si verifica un errore di convalida, immesse (non solo i dati non validi) è stato eliminato ed era necessario immettere nuovamente i valori per tutti i campi. Nell'esempio viene illustrato un punto importante: quando si invia una pagina, elaborarlo e quindi di eseguire il rendering della pagina, la pagina è stato nuovamente creata da zero. Come si è visto, ciò significa che tutti i valori presenti nella pagina quando è stato inviato andranno persi.

È possibile risolvere il problema facilmente, tuttavia. Si ha accesso ai valori che sono stati inviati (nel `Request.Form` dell'oggetto, pertanto è possibile inserire tali valori nei campi modulo quando viene eseguito il rendering della pagina.

1. Nel *Form.cshtml* file, sostituire il `value` gli attributi del `<input>` elementi utilizzando il `value` attributo.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    Il `value` attributo del `<input>` elementi è stata impostata in modo dinamico, leggere il valore del campo da di `Request.Form` oggetto. La prima volta che viene richiesta la pagina, i valori di `Request.Form` oggetto siano vuoti. L'operazione è corretta, perché in questo modo il modulo è vuoto.
2. Avviare la pagina nel browser, compilare i campi modulo o lasciare il campo vuoto e fare clic su **Invia**. Viene visualizzata una pagina che mostra i valori inviati.

    ![forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [1.001 modi per ottenere l'Input dagli utenti Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [Utilizzo di form e l'elaborazione dell'Input dell'utente](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Convalida dell'input utente nelle pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Utilizzo di completamento automatico nei form HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
