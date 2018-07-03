---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Lotta contro i bot (c#) | Microsoft Docs
author: wenz
description: BOT automatizzati effetto intonaco i blog e altri siti Web con la posta indesiderata, invio di moduli di commento senza alcuna interazione dell'utente. Controllo NoBot di ASP.NET AJAX svantaggio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 41e8fda5138e4a94e7b8c4af0a5c2bd68e50e9e1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402720"
---
<a name="fighting-bots-c"></a>Lotta contro i bot (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> BOT automatizzati effetto intonaco i blog e altri siti Web con la posta indesiderata, invio di moduli di commento senza alcuna interazione dell'utente. Il controllo NoBot di ASP.NET AJAX Control Toolkit può combattere i bot.


## <a name="overview"></a>Panoramica

BOT automatizzati effetto intonaco i blog e altri siti Web con la posta indesiderata, invio di moduli di commento senza alcuna interazione dell'utente. Il controllo NoBot di ASP.NET AJAX Control Toolkit può combattere i bot.

## <a name="steps"></a>Passaggi

Un approccio comune per sconfiggere i robot consiste nell'utilizzare CAPTCHAs completamente automatizzata pubblico Turing test per individuare i computer e gli esseri umani distanti. Un test Turing è stato originariamente un test in cui un utente necessaria stabilire se un partner di comunicazione è un essere umano o in un computer. Nel web, un CAPTCHA include in genere di un'immagine con alcune lettere distorte su di esso. L'idea è che solo un essere umano può leggere le lettere dell'immagine, mentre gli algoritmi di OCR avrà esito negativo.

Esistono diversi vantaggi e svantaggi di questo approccio, ma una discussione di questo esula dall'ambito di questa esercitazione. C'è tuttavia un controllo in ASP.NET AJAX Control Toolkit che fornisce un approccio simile: `NoBot`. È più facile da risolvere rispetto a un CAPTCHA, ma è molto facile da usare e sulle tariffe pagate molto efficace in siti Web, ad esempio blog in cui viene considerato un esito positivo se la maggior parte dalla posta indesiderata tentativi sono annullate, quale la `NoBot` controllo può eseguire operazioni.

`NoBot` intercetta il postback di ASP.NET web form corrente, se viene soddisfatta almeno una di queste condizioni:

- Il browser non riesce a risolvere un puzzle di JavaScript (ad esempio quando JavaScript è disattivato)
- L'utente ha inviato il modulo a veloce
- Indirizzo IP del client di invio del modulo troppo spesso in un determinato periodo di tempo.

Per verificare la presenza di queste condizioni, il `NoBot` controllo richiede questi attributi (tutti facoltativi):

- `ResponseMinimumDelaySeconds` quantità minima di secondi tra i vari postback
- `CutoffWindowSeconds` lunghezza dell'intervallo di tempo in cui postback da un indirizzo IP sono misure
- `CutoffMaximumInstances` quantità massima di secondi per ogni intervallo di tempo

Le seguenti richieste di markup che almeno due secondi devono trascorrere tra i vari postback e che sono presenti solo cinque postback o meno all'interno di un intervallo di 30 secondi:

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

Quindi come di consueto assicurarsi di includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX viene caricata e il Toolkit di controllo possono essere usato:

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

Poiché la maggior parte dei controlli `NoBot` infatti verificarsi sul lato server, è necessario controllare il risultato di queste convalide. Questa operazione può essere eseguita chiamando `NoBot`del `IsValid()` (metodo). Ha un solo argomento (come un `out` parametro /`ByRef` parametro) che è di tipo `NoBotState`. Rappresentazione di stringa contiene il motivo, quando il controllo ha esito negativo e `Valid` in caso contrario. Il codice seguente genera un messaggio in base alla `NoBot`del risultato:

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

Infine, è necessario un modulo per inviare e un elemento etichetta per il messaggio di output e aver!

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

Quando si esegue questo script e disattivare JavaScript o inviare il modulo entro i primi due secondi o inviare il modulo di sette volte all'interno di trenta secondi, si otterrà un messaggio di errore. Tuttavia utilizzare questo controllo con cautela, poiché solo 90-95% degli utenti hanno attivato JavaScript, pertanto circa 5-10% degli utenti avrà esito negativo `NoBot`del test.


[![Questo messaggio di errore potrebbe essere stato causato da un robot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

Questo messaggio di errore potrebbe essere stato causato da un bot ([fare clic per visualizzare l'immagine con dimensioni normali](fighting-bots-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](fighting-bots-vb.md)
