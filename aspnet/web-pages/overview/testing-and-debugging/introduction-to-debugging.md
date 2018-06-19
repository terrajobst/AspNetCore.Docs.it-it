---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Introduzione al debug ASP.NET Web Pages siti (Razor) | Documenti Microsoft
author: tfitzmac
description: Il debug è il processo di individuazione e correzione degli errori nelle pagine di codice. Questo capitolo illustra alcuni strumenti e tecniche che è possibile utilizzare per eseguire il debug e analyz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: c28d63acda6e585f4aa64f294049c1790faac850
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897505"
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Introduzione al debug ASP.NET Web Pages siti (Razor)
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra vari modi per eseguire il debug di pagine in un sito Web ASP.NET Web Pages (Razor). Il debug è il processo di individuazione e correzione degli errori nelle pagine di codice.
> 
> **Illustra quanto segue:** 
> 
> - Come visualizzare informazioni che consente di analizzare ed eseguire il debug di pagine.
> - Gli strumenti di come utilizzare il debug in Visual Studio.
>   
> 
> Queste sono le funzionalità ASP.NET introdotte nell'articolo:
> 
> - Il `ServerInfo` helper.
> - `ObjectInfo` helper.
>   
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> 
> - Pagine Web ASP.NET (Razor) 3
> - Visual Studio 2013
>   
> 
> In questa esercitazione si integra inoltre con ASP.NET Web Pages 2. È possibile utilizzare WebMatrix 3, ma non è supportato il debugger integrato.


Un aspetto importante della risoluzione dei problemi nel codice è in primo luogo evitare. È possibile farlo inserendo sezioni del codice che potrebbe causare errori in `try/catch` blocchi. Per ulteriori informazioni, vedere la sezione sulla gestione degli errori in [Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

Il `ServerInfo` helper è uno strumento di diagnostica che fornisce una panoramica delle informazioni sull'ambiente del server web che ospita la pagina. Viene inoltre informazioni sulla richiesta HTTP viene inviati quando la pagina richieste del browser. Il `ServerInfo` helper Visualizza l'identità dell'utente corrente, il tipo di browser che ha effettuato la richiesta, e così via. Questo tipo di informazioni consentono di risolvere i problemi comuni.

1. Creare una nuova pagina web denominata *ServerInfo.cshtml*.
2. Alla fine della pagina, appena prima della chiusura `</body>` tag, aggiungere `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    È possibile aggiungere il `ServerInfo` codice in un punto qualsiasi della pagina. Ma, aggiungerlo alla fine manterrà il relativo output separato da altro contenuto della pagina, che rende più facile da leggere.

    > [!NOTE] 
    > 
    > **Importante** è necessario rimuovere qualsiasi codice diagnostica dalle pagine web prima di spostare le pagine web in un server di produzione. Questo vale per il `ServerInfo` helper, nonché altre tecniche diagnostiche in questo articolo che implicano l'aggiunta di codice a una pagina. Per evitare che i visitatori del sito Web per visualizzare informazioni sul nome di server, i nomi utente, i percorsi sul server e dettagli simili, perché questo tipo di informazioni potrebbe essere utile per gli utenti con malintenzionati.
3. Salvare la pagina ed eseguirlo in un browser.

    ![Debug-1.](introduction-to-debugging/_static/image1.jpg)

    Il `ServerInfo` helper visualizzate quattro tabelle delle informazioni nella pagina:

   - Configurazione del server. In questa sezione vengono fornite informazioni sui server di hosting web, inclusi nome del computer, la versione di ASP.NET è in esecuzione, il nome di dominio e ora del server.
   - Variabili Server ASP.NET. In questa sezione vengono fornite informazioni dettagliate sui dettagli del protocollo HTTP molti (chiamata HTTP variabili) e i valori che fanno parte di ogni richiesta di pagina web.
   - Informazioni di HTTP Runtime. Questa sezione vengono fornite informazioni dettagliate su che la versione di Microsoft .NET Framework in esecuzione con la pagina web, percorso, i dettagli relativi alla cache e così via. (Come descritto in [Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890), ASP.NET Web Pages utilizzando Razor sintassi si basano sulla tecnologia Microsoft per ASP.NET web server, che a sua volta è basato su un software completo libreria di sviluppo denominato .NET Framework).
   - Variabili di ambiente. In questa sezione fornisce un elenco di tutte le variabili di ambiente locale e i relativi valori nel server web.

     Una descrizione completa di tutte le informazioni di richiesta e server non rientra nell'ambito di questo articolo, ma è possibile vedere che il `ServerInfo` helper restituisce molte delle informazioni di diagnostica. Per ulteriori informazioni sui valori che `ServerInfo` restituisce un valore, vedere [riconosciuto le variabili di ambiente](https://technet.microsoft.com/library/dd560744(WS.10).aspx) nel sito Web Microsoft TechNet e [variabili del Server IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) nel sito Web MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Incorporare le espressioni di Output per visualizzare i valori di pagina

Un altro modo per scoprire cosa avviene nel codice consiste nell'incorporare le espressioni di output della pagina. Come è noto, è possibile creare direttamente il valore di una variabile tramite l'aggiunta di un risultato simile `@myVariable` o `@(subTotal * 12)` alla pagina. Per il debug, è possibile inserire queste espressioni di output in punti strategici nel codice. Ciò consente di visualizzare il valore delle variabili chiave o il risultato dei calcoli durante l'esecuzione della pagina. Al termine il debug, è possibile rimuovere le espressioni o eseguirne l'estrazione di commento. Questa procedura viene illustrato un modo comune di usare espressioni incorporate per facilitare il debug di una pagina.

1. Creare una nuova pagina WebMatrix denominato *OutputExpression.cshtml*.
2. Sostituire la pagina contenuta con quanto segue:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    Nell'esempio viene utilizzato un `switch` istruzione per controllare il valore della `weekday` variabile e quindi visualizzare un messaggio di output diversi a seconda di quale giorno della settimana è. Nell'esempio di `if` all'interno del primo blocco di codice arbitrariamente la modifica del giorno della settimana mediante l'aggiunta di un giorno per il valore corrente del giorno della settimana. Si tratta di un errore introdotto a scopo illustrativo.
3. Salvare la pagina ed eseguirlo in un browser.

    La pagina Visualizza il messaggio per il giorno della settimana non corretto. Qualsiasi giorno della settimana in realtà è, verrà visualizzato il messaggio per un giorno in un secondo momento. Sebbene in questo caso, si sa perché il messaggio è disattivata (perché il codice imposta intenzionalmente il valore del giorno corretto), in realtà è spesso difficile sapere in operazioni di errori nel codice. Per eseguire il debug, è necessario sapere qual è il problema per il valore di variabili e gli oggetti chiave, ad esempio `weekday`.
4. Aggiungere le espressioni di output inserendo `@weekday` come illustrato nelle due posizioni indicate dai commenti nel codice. Queste espressioni di output verranno visualizzati i valori della variabile a questo punto l'esecuzione di codice.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Salvare ed eseguire la pagina in un browser.

    Nella pagina vengono visualizzati il giorno della settimana reale prima di tutto, quindi il giorno della settimana aggiornato che deriva dall'aggiunta di un giorno, quindi il messaggio risultante dal `switch` istruzione. L'output da due espressioni di variabile (`@weekday`) non contenga spazi tra i giorni perché non sono state aggiunte a qualsiasi codice HTML `<p>` tag per l'output; le espressioni sono solo per i test.

    ![Debugging-2](introduction-to-debugging/_static/image2.jpg)

    Ora è possibile visualizzare in cui è l'errore. Quando si visualizza inizialmente la `weekday` variabile nel codice viene illustrato il giorno corretto. Quando si visualizza la seconda volta, dopo il `if` blocco nel codice, il giorno è disattivata per uno. Per sapere che tra il primo e secondo aspetto della variabile del giorno della settimana è avvenuto qualcosa. Se si trattasse di un bug reale, questo tipo di approccio sarebbe consentono di limitare la posizione del codice che causa il problema.
6. Correggere il codice nella pagina rimuovendo le espressioni di due output che è stato aggiunto e rimosso il codice che modifica il giorno della settimana. Il blocco rimanente, completo di codice è simile alla seguente:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Eseguire la pagina in un browser. Questa volta, verrà visualizzato il messaggio corretto visualizzato per il giorno della settimana effettivo.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Utilizzo dell'ObjectInfo Helper per visualizzare i valori di oggetto

Il `ObjectInfo` helper Visualizza il tipo e il valore di ciascun oggetto passato. È possibile utilizzarlo per visualizzare il valore di variabili e oggetti nel codice (come fatto in precedenza con le espressioni di output dell'esempio precedente), più è possibile visualizzare informazioni sul tipo relative all'oggetto dati.

1. Aprire il file denominato *OutputExpression.cshtml* creato in precedenza.
2. Sostituire tutto il codice della pagina con il blocco di codice seguente:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Salvare ed eseguire la pagina in un browser.

    ![Debugging-4](introduction-to-debugging/_static/image3.jpg)

    In questo esempio, il `ObjectInfo` helper Visualizza due elementi:

   - Tipo. Per la prima variabile, il tipo è `DayOfWeek`. Per la seconda variabile, il tipo è `String`.
   - Valore. In questo caso, perché già possibile visualizzare il valore della variabile di saluto nella pagina, il valore viene visualizzato nuovamente quando si passa la variabile `ObjectInfo`.

     Per gli oggetti più complessi, il `ObjectInfo` helper può visualizzare altre informazioni &#8212; in pratica, è possibile visualizzare i tipi e valori di tutte le proprietà dell'oggetto.

## <a name="using-debugging-tools-in-visual-studio"></a>Usando gli strumenti di debug in Visual Studio

Per un'esperienza di debug più completa, usare Visual Studio 2013 o la versione gratuita [Visual Studio Express 2013 per Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express). Con Visual Studio, è possibile impostare un punto di interruzione nel codice nella riga che si desidera controllare.

![Set di punti di interruzione](introduction-to-debugging/_static/image1.png)

Quando si testa il sito web, il codice in esecuzione si interrompe al punto di interruzione.

![raggiungere punti di interruzione](introduction-to-debugging/_static/image2.png)

È possibile esaminare i valori correnti delle variabili e scorrere il codice riga per riga.

![visualizzare i valori](introduction-to-debugging/_static/image3.png)

Per informazioni sull'utilizzo di debugger integrato in Visual Studio per il debug delle pagine ASP.NET Razor, vedere [programmazione nelle pagine Web ASP.NET (Razor) tramite Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Risorse aggiuntive

- [Programmazione di pagine Web ASP.NET (Razor) utilizzando Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Le variabili del Server IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Le variabili di ambiente riconosciuto](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
