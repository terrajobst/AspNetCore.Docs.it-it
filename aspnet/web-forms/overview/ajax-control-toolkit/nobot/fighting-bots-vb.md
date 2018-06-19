---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Tori bot (VB) | Documenti Microsoft
author: wenz
description: BOT automatizzati effetto intonaco blog e altri siti Web con la posta indesiderata, invio di form di commento senza alcuna interazione dell'utente. Il controllo NoBot le configurazioni di AJAX ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 35d5984ac7ff3422bab07a759c93fef3914a22f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874057"
---
<a name="fighting-bots-vb"></a>Tori bot (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)

> BOT automatizzati effetto intonaco blog e altri siti Web con la posta indesiderata, invio di form di commento senza alcuna interazione dell'utente. Il controllo NoBot in ASP.NET AJAX Control Toolkit consente di contrastare tali componenti.


## <a name="overview"></a>Panoramica

BOT automatizzati effetto intonaco blog e altri siti Web con la posta indesiderata, invio di form di commento senza alcuna interazione dell'utente. Il controllo NoBot in ASP.NET AJAX Control Toolkit consente di contrastare tali componenti.

## <a name="steps"></a>Passaggi

Un approccio comune per aggirare i robot consiste nell'utilizzare test CAPTCHAs completamente automatizzato pubblica Turing per indicare i computer e comprensibile distanti. Un test Turing era in origine un test in cui un utente necessario per stabilire se un partner di comunicazione è una persona o un computer. Nel web, un CAPTCHA include in genere di un'immagine con alcune lettere distorte su di esso. L'idea è che solo una persona può leggere le lettere dell'immagine, mentre gli algoritmi OCR avrà esito negativo.

Esistono diversi vantaggi e svantaggi di questo approccio, ma una descrizione di questo oggetto non rientra nell'ambito di questa esercitazione. È tuttavia un controllo ASP.NET AJAX Control Toolkit che fornisce un approccio simile: `NoBot`. È più semplice superare rispetto a un CAPTCHA, ma è molto facile da usare e tariffe molto bene in siti Web come blog in cui viene considerato un caso di esito positivo se la maggior parte di inviare posta indesiderata tentativi sono annullati, quale il `NoBot` controllo eseguibili.

`NoBot` Consente di intercettare il postback di web form ASP.NET corrente se viene soddisfatta almeno una delle seguenti condizioni:

- Il browser non riesce a risolvere un puzzle JavaScript (ad esempio quando JavaScript è disattivato)
- Il modulo alla veloce inviati dall'utente
- Indirizzo IP del client di invio del modulo troppo spesso in un determinato periodo di tempo.

Per verificare la presenza di queste condizioni, il `NoBot` controllo richiede questi attributi (tutti facoltativi):

- `ResponseMinimumDelaySeconds` quantità minima di secondi tra i vari postback
- `CutoffWindowSeconds` lunghezza dell'intervallo di tempo in cui i postback da un indirizzo IP sono misure
- `CutoffMaximumInstances` quantità massima di secondi per ogni intervallo di tempo

Le seguenti richieste di markup che almeno due secondi devono trascorrere tra i vari postback e che sono presenti i postback solo cinque o meno all'interno di un intervallo di 30 secondi:

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

Quindi come di consueto assicurarsi di includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX è caricata e può essere utilizzato il Toolkit di controllo:

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

Poiché la maggior parte dei controlli `NoBot` esegue si verificano sul lato server, è necessario controllare il risultato di queste convalide. Questa operazione può essere eseguita chiamando `NoBot`del `IsValid()` metodo. Dispone di un argomento (come un `out` parametro /`ByRef` parametro) che è di tipo `NoBotState`. Rappresentazione di stringa contiene il motivo, quando il controllo ha esito negativo e `Valid` in caso contrario. Il codice seguente genera un messaggio in base alla `NoBot`del risultato:

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

Infine, è necessario un form per l'invio e un elemento di etichetta per il messaggio di output e avere!

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

Quando si esegue questo script e disattivare JavaScript o inviare il modulo entro i primi due secondi o inviare il modulo sette volte all'interno di trenta secondi, si otterrà un messaggio di errore. Tuttavia utilizzare questo controllo con cautela, poiché solo circa 90 95% di utenti hanno attivato JavaScript, 5-10% di utenti non verrà pertanto `NoBot`del test.


[![Questo messaggio di errore potrebbe essere causato da un robot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)

Questo messaggio di errore potrebbe essere causato da un robot ([fare clic per visualizzare l'immagine ingrandita](fighting-bots-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](fighting-bots-cs.md)
