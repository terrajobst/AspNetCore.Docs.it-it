---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Introduzione al debug ASP.NET Web Pages (Razor) siti | Microsoft Docs
author: tfitzmac
description: Il debug è il processo di individuazione e correzione degli errori nelle pagine di codice. Questo capitolo illustra alcuni strumenti e tecniche che è possibile utilizzare per eseguire il debug e per analizzare il...
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: b8a492b065902fa10d3e4c5cccd50e63ea356709
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823803"
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Introduzione al debug ASP.NET Web Pages (Razor) siti
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra vari modi per eseguire il debug di pagine in un sito Web di ASP.NET Web Pages (Razor). Il debug è il processo di individuazione e correzione degli errori nelle pagine di codice.
> 
> **Che cosa si apprenderà come:** 
> 
> - Come visualizzare informazioni utili per analizzare ed eseguire il debug di pagine.
> - Come eseguire il debug degli strumenti in Visual Studio.
>   
> 
> Queste sono le funzionalità ASP.NET introdotte nell'articolo:
> 
> - Il `ServerInfo` helper.
> - `ObjectInfo` metodo di supporto.
>   
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
>   
> 
> Questa esercitazione si integra inoltre con ASP.NET Web Pages 2. È possibile utilizzare WebMatrix 3 ma il debugger integrato non è supportato.


Un aspetto importante della risoluzione dei problemi relativi a errori e problemi nel codice è per evitare il problema in primo luogo. È possibile farlo mediante l'inserimento di sezioni del codice che possono causare errori in `try/catch` blocchi. Per altre informazioni, vedere la sezione sulla gestione degli errori nelle [Introduzione alla programmazione Web ASP.NET usando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

Il `ServerInfo` helper è uno strumento diagnostico che ti offre una panoramica delle informazioni sull'ambiente del server web che ospita la pagina. Indica inoltre informazioni sulla richiesta HTTP viene inviati quando la pagina richieste del browser. Il `ServerInfo` helper Visualizza l'identità dell'utente corrente, il tipo di browser che ha effettuato la richiesta, e così via. Questo tipo di informazioni consente di risolvere i problemi comuni.

1. Creare una nuova pagina web denominata *ServerInfo.cshtml*.
2. Alla fine della pagina, subito prima della chiusura `</body>` tag, aggiungere `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    È possibile aggiungere il `ServerInfo` codice in un punto qualsiasi della pagina. Ma viene aggiunta alla fine manterrà il proprio output separato da altro contenuto della pagina, che rende più facile da leggere.

    > [!NOTE] 
    > 
    > **Importante** è necessario rimuovere qualsiasi codice di diagnostica dalle pagine web prima di spostare le pagine web in un server di produzione. Questo vale per il `ServerInfo` helper, nonché altre tecniche diagnostiche in questo articolo che implicano l'aggiunta di codice a una pagina. Per evitare che i visitatori del sito Web per visualizzare informazioni sul nome del server, nomi di utenti, i percorsi nel server e i dettagli simili, perché questo tipo di informazioni potrebbe essere utile per le persone con finalità dannose.
3. Salvare la pagina ed eseguirlo in un browser.

    ![-1 di debug](introduction-to-debugging/_static/image1.jpg)

    Il `ServerInfo` helper Visualizza quattro tabelle delle informazioni nella pagina:

   - Configurazione del server. In questa sezione fornisce informazioni sul server di hosting web, inclusi nome del computer, la versione di ASP.NET in esecuzione, il nome di dominio e ora del server.
   - Variabili del Server ASP.NET. In questa sezione vengono fornite informazioni dettagliate sui dettagli del protocollo HTTP molti (variabili di chiamate HTTP) e i valori che fanno parte di ogni richiesta di pagina web.
   - Informazioni di HTTP Runtime. Questa sezione vengono fornite informazioni dettagliate su che la versione di Microsoft .NET Framework che sta utilizzando la pagina web, il percorso, informazioni dettagliate sulle cache e così via. (Come descritto nella [Introduzione alla programmazione Web ASP.NET usando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890), pagine Web ASP.NET con Razor sintassi vengono compilate sulla tecnologia Microsoft per ASP.NET web server, che a sua volta è basato su una vasta gamma di prodotti libreria per lo sviluppo denominato .NET Framework).
   - Variabili di ambiente. Questa sezione presenta un elenco di tutte le variabili di ambiente locale e i relativi valori nel server web.

     Una descrizione completa di tutte le informazioni di richiesta e server esula dall'ambito di questo articolo, ma è possibile notare che il `ServerInfo` helper restituisce una grande quantità di informazioni di diagnostica. Per altre informazioni sui valori che `ServerInfo` restituisce un valore, vedere [variabili di ambiente riconosciute](https://technet.microsoft.com/library/dd560744(WS.10).aspx) sul sito Web Microsoft TechNet e [variabili del Server IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) sul sito Web MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Incorporare le espressioni di Output per visualizzare i valori di pagina

Un altro modo per vedere cosa accade nel codice consiste nell'incorporare le espressioni di output della pagina. Come è noto, è possibile restituire il valore di una variabile direttamente mediante l'aggiunta di un elemento, ad esempio `@myVariable` o `@(subTotal * 12)` alla pagina. Per eseguire il debug, è possibile inserire queste espressioni output corrispondenza dei punti strategici nel codice. In questo modo è possibile visualizzare il valore delle variabili chiave o il risultato dei calcoli durante l'esecuzione di pagina. Dopo aver completato il debug, è possibile rimuovere le espressioni o impostarle come commenti. Questa procedura viene illustrato un modo tipico per utilizzare le espressioni incorporate per facilitare il debug di una pagina.

1. Creare una nuova pagina WebMatrix denominato *OutputExpression.cshtml*.
2. Sostituire il contenuto con la seguente pagina:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    L'esempio Usa un' `switch` istruzione per controllare il valore della `weekday` variabile e quindi visualizzato un messaggio di output diversi a seconda del giorno della settimana in cui è. Nell'esempio di `if` all'interno del primo blocco di codice, il giorno della settimana viene modificato in modo arbitrario mediante l'aggiunta di un giorno per il valore del giorno della settimana corrente. Si tratta di un errore introdotto a scopo illustrativo.
3. Salvare la pagina ed eseguirlo in un browser.

    La pagina Visualizza il messaggio per il giorno della settimana non corretto. Qualsiasi giorno della settimana in realtà è, verrà visualizzato il messaggio per un giorno in un secondo momento. Sebbene in questo caso si conosce il motivo per cui il messaggio è disattivata (perché il codice imposta deliberatamente il valore del giorno non corretto), in realtà è spesso difficile sapere dove lo stato di avanzamento di errori nel codice. Per eseguire il debug, è necessario scoprire cosa sta succedendo al valore di variabili e gli oggetti chiave, ad esempio `weekday`.
4. Aggiungere espressioni output inserendo `@weekday` come illustrato nelle due posizioni indicate dai commenti nel codice. Queste espressioni di output verranno visualizzati i valori della variabile a quel punto l'esecuzione di codice.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Salvare ed eseguire la pagina in un browser.

    La pagina Visualizza il giorno della settimana reale prima di tutto, quindi il giorno della settimana aggiornato che risultante dall'aggiunta di un giorno e quindi il messaggio risulta dalla `switch` istruzione. L'output di due espressioni variabili (`@weekday`) non ha spazi tra i giorni in quanto non è stato aggiunto codice HTML `<p>` tag per l'output; le espressioni sono solo per i test.

    ![Debug-2](introduction-to-debugging/_static/image2.jpg)

    Ora è possibile visualizzare dove è l'errore. Quando si visualizza inizialmente la `weekday` variabile nel codice, Visualizza il giorno corretto. Quando si visualizza la seconda volta, dopo il `if` blocco nel codice, il giorno è disattivata da uno. Quindi si è sicuri che tra l'aspetto di prima e seconda della variabile del giorno della settimana è accaduto qualcosa. Se si trattasse di un vero bug, questo tipo di approccio sarebbe consentono di restringere il percorso del codice che ha causato il problema.
6. Correggere il codice nella pagina rimuovendo le espressioni di due output che è stato aggiunto e rimuovendo il codice che la modifica del giorno della settimana. Il blocco rimanente, completo di codice è simile al seguente:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Eseguire la pagina in un browser. Questa volta viene visualizzato il messaggio corretto visualizzato per il giorno della settimana effettivo.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Utilizzo dell'ObjectInfo Helper per visualizzare i valori di oggetto

Il `ObjectInfo` helper Visualizza il tipo e il valore di ogni oggetto viene passato. È possibile usarlo per visualizzare il valore delle variabili e gli oggetti nel codice (come fatto in precedenza con le espressioni di output dell'esempio precedente), oltre a poter visualizzare dati digitare le informazioni sull'oggetto.

1. Aprire il file denominato *OutputExpression.cshtml* creato in precedenza.
2. Sostituire tutto il codice nella pagina con il blocco di codice seguente:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Salvare ed eseguire la pagina in un browser.

    ![Debug-4](introduction-to-debugging/_static/image3.jpg)

    In questo esempio, il `ObjectInfo` helper Visualizza due elementi:

   - Tipo. Per la prima variabile, il tipo è `DayOfWeek`. Per la seconda variabile, il tipo è `String`.
   - Valore. In questo caso, perché il valore della variabile di saluto già visualizzata nella pagina, il valore viene visualizzato nuovamente quando si passa la variabile `ObjectInfo`.

     Per gli oggetti più complessi, il `ObjectInfo` helper può visualizzare più informazioni &#8212; in pratica, è possibile visualizzare i tipi e valori di tutte le proprietà di un oggetto.

## <a name="using-debugging-tools-in-visual-studio"></a>Usando gli strumenti di debug in Visual Studio

Per un'esperienza di debug più completa, usare Visual Studio 2013 o la versione gratuita [Visual Studio Express 2013 per Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express). Con Visual Studio, è possibile impostare un punto di interruzione nel codice nella riga che si desidera controllare.

![Imposta punto di interruzione](introduction-to-debugging/_static/image1.png)

Quando si testa il sito web, il codice in esecuzione il processo si interromperà nel punto di interruzione.

![raggiunto punto di interruzione](introduction-to-debugging/_static/image2.png)

È possibile esaminare i valori correnti delle variabili e passaggio attraverso il codice riga per riga.

![visualizzare i valori](introduction-to-debugging/_static/image3.png)

Per informazioni sull'uso del debugger integrato in Visual Studio per eseguire il debug di pagine Razor di ASP.NET, vedere [Programming ASP.NET Web Pages (Razor) con Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Risorse aggiuntive

- [Programmazione di ASP.NET Web Pages (Razor) con Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Variabili del Server IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Le variabili di ambiente riconosciute](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
