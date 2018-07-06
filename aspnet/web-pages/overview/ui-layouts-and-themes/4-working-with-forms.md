---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Utilizzo di moduli HTML in ASP.NET Web Pages (Razor) Sites | Microsoft Docs
author: tfitzmac
description: Un modulo è una sezione di un documento HTML si inseriranno i controlli dell'input dell'utente, ad esempio le caselle di testo, caselle di controllo, pulsanti di opzione e gli elenchi a discesa. È necessario utilizzare moduli eriore a...
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 4658e80d63c9c3665f197071f3f3d1420362e4a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820285"
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Uso dei moduli HTML in ASP.NET Web Pages (Razor) Sites
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive come elaborare un form HTML (con le caselle di testo e pulsanti) quando si lavora in un sito Web ASP.NET Web Pages (Razor).
> 
> **Che cosa si apprenderà come:** 
> 
> - Come creare un form HTML.
> - Come leggere l'input dell'utente dal form.
> - Come convalidare l'input dell'utente.
> - Come ripristinare i valori del form dopo l'invio della pagina.
> 
> Questi sono i concetti introdotti nell'articolo di programmazione ASP.NET:
> 
> - Oggetto `Request`.
> - Convalida dell'input.
> - La codifica HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.


## <a name="creating-a-simple-html-form"></a>Creazione di un Form HTML semplice

1. Creare un nuovo sito Web.
2. Nella cartella radice, creare una pagina web denominata *Form.cshtml* e immettere il markup seguente:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Avvio della pagina nel browser. (In WebMatrix, nelle **file** dell'area di lavoro, il pulsante destro e quindi selezionare **Avvia nel browser**.) Un modulo semplice con tre campi di input e un **Submit** pulsante viene visualizzato.

    ![Screenshot di un form tre caselle di testo.](4-working-with-forms/_static/image1.jpg)

    A questo punto, se si sceglie la **Submit** pulsante, non accade nulla. Per rendere il modulo è utile, è necessario aggiungere il codice che verrà eseguito nel server.

## <a name="reading-user-input-from-the-form"></a>Input dell'utente durante la lettura dal Form

Per elaborare il form, si aggiunge codice che legge i valori dei campi inviato e non esegue un'operazione con essi. Questa procedura viene illustrato come leggere i campi e visualizzare l'input dell'utente della pagina. (In un'applicazione di produzione in genere eseguire operazioni più interessanti con input utente. È possibile farlo nell'articolo sull'uso di database.)

1. In cima il *Form.cshtml* file, immettere il codice seguente:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Quando l'utente richiede innanzitutto la pagina, viene visualizzato solo il modulo vuoto. L'utente (che sarà è) compila il modulo e quindi fa clic su **Submit**. (POST) viene inviata l'input dell'utente al server. Per impostazione predefinita, la richiesta viene inviata alla stessa pagina (vale a dire *Form.cshtml*).

    Quando si invia la pagina in questo momento, i valori immessi vengono visualizzati sopra la forma:

    ![Screenshot che mostra i valori che immessi sono visualizzati nella pagina.](4-working-with-forms/_static/image2.jpg)

    Esaminare il codice per la pagina. Utilizzare innanzitutto la `IsPost` metodo per determinare se la pagina viene registrata &#8212; , ovvero se è stato selezionato un il **Invia** pulsante. Se si tratta di un post, `IsPost` restituisce true. Questo è il modo standard nelle pagine Web ASP.NET per determinare se si lavora con una richiesta iniziale (una richiesta GET) o un postback (una richiesta POST). (Per ulteriori informazioni su GET e POST, vedere l'intestazione laterale "HTTP GET e POST e l'istruzione IsPost Property" nella [Introduzione a ASP.NET Web Pages di programmazione utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Successivamente, si otterranno i valori che l'utente compilata dalla `Request.Form` oggetto e inserirli in variabili per un uso successivo. Il `Request.Form` oggetto contiene tutti i valori che sono stati inviati con la pagina, ognuna identificata da una chiave. La chiave è l'equivalente di `name` attributo del campo del form che si desidera leggere. Ad esempio, per leggere il `companyname` campo (casella di testo), si utilizza `Request.Form["companyname"]`.

    I valori del form vengono archiviati nel `Request.Form` oggetto sotto forma di stringhe. Pertanto, quando si lavora con un valore come numero o una data o un altro tipo, è necessario convertirlo da stringa al tipo. Nell'esempio, il `AsInt` metodo del `Request.Form` viene usata per convertire il valore del campo, che contiene un conteggio dipendenti, i dipendenti in un numero intero.
2. Avvio della pagina nel browser, compilare i campi del form e fare clic su **Submit**. La pagina Visualizza i valori immessi.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>HTML codifica per l'aspetto e sicurezza
> 
> HTML ha usi speciali per i caratteri, ad esempio `<`, `>`, e `&`. Se sono presenti questi caratteri speciali in cui si non sta previsto, può danneggiare la l'aspetto e le funzionalità della pagina web. Ad esempio, il browser interpreta il `<` di caratteri (a meno che non è seguito da uno spazio) come inizio di un elemento HTML, ad esempio `<b>` o `<input ...>`. Se il browser non riconosce l'elemento, ignora semplicemente la stringa che inizia con `<` nuovamente finché non raggiunge un elemento che esso venga riconosciuto. Ovviamente, ciò può comportare alcune strano per il rendering della pagina.
> 
> La codifica HTML sostituisce questi caratteri riservati con un codice che i browser interpretano come nel simbolo corretto. Ad esempio, il `<` viene sostituito con `&lt;` e il `>` viene sostituito con `&gt;`. Il browser esegue il rendering di queste stringhe di sostituzione come i caratteri che si desidera visualizzare.
> 
> È consigliabile usare ogni volta che si visualizza stringhe codificate in formato HTML (input) che è stato creato da un utente. In caso contrario, un utente può provare a ottenere la pagina web per eseguire uno script dannoso o eseguire altre operazioni che compromette la protezione del sito oppure non è previsto. (Ciò è particolarmente importante se si accettano input dell'utente, archiviarlo altre fonti e quindi visualizzarli in un secondo momento &#8212; , ad esempio, come un commento di blog, verifica utente o qualcosa del genere.)
> 
> Per consentire di evitare questi problemi, ASP.NET Web Pages automaticamente codifica in HTML qualsiasi testo di contenuto che si dal codice di output. Ad esempio, quando si visualizza il contenuto di una variabile o espressione tramite codice come `@MyVar`, ASP.NET Web Pages codifica automaticamente l'output.


## <a name="validating-user-input"></a>Convalida dell'Input utente

Gli utenti commettono errori. Chiedere loro di inserire in un campo e ne dimenticassero, o chiedere loro di immettere il numero di dipendenti e digitano invece un nome. Per assicurarsi che un modulo è stato compilato correttamente prima elaborarla, convalidare l'input dell'utente.

Questa procedura viene illustrato come convalidare tutti i campi modulo tre per assicurarsi che l'utente non ha lasciarli vuoti. È anche verificare che il valore del conteggio dipendenti è un numero. Se sono presenti errori, si verrà visualizzato un errore messaggio che indica all'utente i valori che non supera la convalida.

1. Nel *Form.cshtml* file, sostituire il primo blocco di codice con il codice seguente: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Per convalidare l'input dell'utente, si utilizza il `Validation` helper. È possibile registrare i campi obbligatori chiamata `Validation.RequireField`. È possibile registrare altri tipi di convalida chiamata `Validation.Add` e specificando il campo da convalidare e il tipo di convalida da eseguire.

    Quando si esegue la pagina, ASP.NET esegue tutte le convalide per l'utente. È possibile controllare i risultati chiamando `Validation.IsValid`, che restituisce true se tutti gli elementi passati e false se qualsiasi campo non è riuscita la convalida. In genere, si chiama `Validation.IsValid` prima di eseguire qualsiasi elaborazione input dell'utente.
2. Aggiorna il `<body>` elemento mediante l'aggiunta di tre chiamate al `Html.ValidationMessage` metodo, simile al seguente:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Per visualizzare i messaggi di errore di convalida, è possibile chiamare codice Html.`ValidationMessage` e passa il nome del campo che si desidera che il messaggio per.
3. Eseguire la pagina. Lasciare vuoti i campi e fare clic su **Submit**. Vengono visualizzati messaggi di errore.

    ![Screenshot che mostra i messaggi di errore visualizzati se l'input dell'utente non soddisfa la convalida.](4-working-with-forms/_static/image3.jpg)
4. Aggiungere una stringa (ad esempio, "ABC") per il **Employee Count** campo e fare clic su **Submit** nuovamente. Questa volta che viene visualizzato un errore che indica che la stringa non è nel formato corretto, vale a dire, un numero intero.

    ![Screenshot che mostra i messaggi di errore visualizzati se gli utenti immettere una stringa per il campo di dipendenti.](4-working-with-forms/_static/image4.jpg)

ASP.NET Web Pages sono disponibili più opzioni per la convalida dell'input utente, inclusa la possibilità di eseguire automaticamente la convalida usando lo script client, in modo che gli utenti di ottenere un feedback immediato nel browser. Vedere le [risorse aggiuntive](#Additional_Resources) sezione più avanti per ulteriori informazioni.

## <a name="restoring-form-values-after-postbacks"></a>Ripristinare i valori del Form dopo il postback

Quando il test della pagina nella sezione precedente, si potrebbe notare che se si verifica un errore di convalida, tutti gli elementi che immessi (non solo i dati non validi) è stato ancora attivo ed era necessario immettere nuovamente i valori per tutti i campi. Nell'esempio viene illustrato un punto importante: quando si invia una pagina, elaborarlo e quindi di nuovo il rendering della pagina, la pagina viene ricreata da zero. Come si è visto, ciò significa che tutti i valori presenti nella pagina quando al momento dell'invio vengono persi.

È possibile risolvere il problema con facilità, tuttavia. È possibile utilizzare i valori che sono stati inviati (nelle `Request.Form` dell'oggetto, pertanto è possibile inserire tali valori in campi modulo quando viene eseguito il rendering della pagina.

1. Nel *Form.cshtml* del file, sostituire il `value` gli attributi del `<input>` elementi, usando il `value` attributo.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    Il `value` attributo del `<input>` elementi è stata impostata per leggere in modo dinamico il valore del campo del `Request.Form` oggetto. La prima volta che viene richiesta la pagina, i valori di `Request.Form` oggetto sono tutte vuote. Ciò è corretto, perché in questo modo il modulo è vuoto.
2. Avvio della pagina nel browser, compilare i campi modulo o lasciarli vuoti e fare clic su **Submit**. Viene visualizzata una pagina che mostra i valori presentati.

    ![forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [1.001 modi per ottenere l'Input dagli utenti Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [Utilizzo di form e l'elaborazione dell'Input dell'utente](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Convalida dell'input utente nelle pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Uso di AutoComplete nei form HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
