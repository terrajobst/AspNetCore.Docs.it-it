---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Lotta contro i bot (VB) | Microsoft Docs
author: wenz
description: BOT automatizzati effetto intonaco i blog e altri siti Web con la posta indesiderata, invio di moduli di commento senza alcuna interazione dell'utente. Controllo NoBot di ASP.NET AJAX svantaggio...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 8b2a2d2d72bfcf3ce8b3b345fda0bad5a37818ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836249"
---
<a name="fighting-bots-vb"></a><span data-ttu-id="5987b-104">Lotta contro i bot (VB)</span><span class="sxs-lookup"><span data-stu-id="5987b-104">Fighting Bots (VB)</span></span>
====================
<span data-ttu-id="5987b-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5987b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5987b-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5987b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="5987b-107">BOT automatizzati effetto intonaco i blog e altri siti Web con la posta indesiderata, invio di moduli di commento senza alcuna interazione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="5987b-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="5987b-108">Il controllo NoBot di ASP.NET AJAX Control Toolkit può combattere i bot.</span><span class="sxs-lookup"><span data-stu-id="5987b-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="5987b-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="5987b-109">Overview</span></span>

<span data-ttu-id="5987b-110">BOT automatizzati effetto intonaco i blog e altri siti Web con la posta indesiderata, invio di moduli di commento senza alcuna interazione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="5987b-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="5987b-111">Il controllo NoBot di ASP.NET AJAX Control Toolkit può combattere i bot.</span><span class="sxs-lookup"><span data-stu-id="5987b-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="5987b-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="5987b-112">Steps</span></span>

<span data-ttu-id="5987b-113">Un approccio comune per sconfiggere i robot consiste nell'utilizzare CAPTCHAs completamente automatizzata pubblico Turing test per individuare i computer e gli esseri umani distanti.</span><span class="sxs-lookup"><span data-stu-id="5987b-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="5987b-114">Un test Turing è stato originariamente un test in cui un utente necessaria stabilire se un partner di comunicazione è un essere umano o in un computer.</span><span class="sxs-lookup"><span data-stu-id="5987b-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="5987b-115">Nel web, un CAPTCHA include in genere di un'immagine con alcune lettere distorte su di esso.</span><span class="sxs-lookup"><span data-stu-id="5987b-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="5987b-116">L'idea è che solo un essere umano può leggere le lettere dell'immagine, mentre gli algoritmi di OCR avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="5987b-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="5987b-117">Esistono diversi vantaggi e svantaggi di questo approccio, ma una discussione di questo esula dall'ambito di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5987b-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="5987b-118">C'è tuttavia un controllo in ASP.NET AJAX Control Toolkit che fornisce un approccio simile: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="5987b-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="5987b-119">È più facile da risolvere rispetto a un CAPTCHA, ma è molto facile da usare e sulle tariffe pagate molto efficace in siti Web, ad esempio blog in cui viene considerato un esito positivo se la maggior parte dalla posta indesiderata tentativi sono annullate, quale la `NoBot` controllo può eseguire operazioni.</span><span class="sxs-lookup"><span data-stu-id="5987b-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="5987b-120">`NoBot` intercetta il postback di ASP.NET web form corrente, se viene soddisfatta almeno una di queste condizioni:</span><span class="sxs-lookup"><span data-stu-id="5987b-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="5987b-121">Il browser non riesce a risolvere un puzzle di JavaScript (ad esempio quando JavaScript è disattivato)</span><span class="sxs-lookup"><span data-stu-id="5987b-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="5987b-122">L'utente ha inviato il modulo a veloce</span><span class="sxs-lookup"><span data-stu-id="5987b-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="5987b-123">Indirizzo IP del client di invio del modulo troppo spesso in un determinato periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="5987b-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="5987b-124">Per verificare la presenza di queste condizioni, il `NoBot` controllo richiede questi attributi (tutti facoltativi):</span><span class="sxs-lookup"><span data-stu-id="5987b-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="5987b-125">`ResponseMinimumDelaySeconds` quantità minima di secondi tra i vari postback</span><span class="sxs-lookup"><span data-stu-id="5987b-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="5987b-126">`CutoffWindowSeconds` lunghezza dell'intervallo di tempo in cui postback da un indirizzo IP sono misure</span><span class="sxs-lookup"><span data-stu-id="5987b-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="5987b-127">`CutoffMaximumInstances` quantità massima di secondi per ogni intervallo di tempo</span><span class="sxs-lookup"><span data-stu-id="5987b-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="5987b-128">Le seguenti richieste di markup che almeno due secondi devono trascorrere tra i vari postback e che sono presenti solo cinque postback o meno all'interno di un intervallo di 30 secondi:</span><span class="sxs-lookup"><span data-stu-id="5987b-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="5987b-129">Quindi come di consueto assicurarsi di includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX viene caricata e il Toolkit di controllo possono essere usato:</span><span class="sxs-lookup"><span data-stu-id="5987b-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="5987b-130">Poiché la maggior parte dei controlli `NoBot` infatti verificarsi sul lato server, è necessario controllare il risultato di queste convalide.</span><span class="sxs-lookup"><span data-stu-id="5987b-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="5987b-131">Questa operazione può essere eseguita chiamando `NoBot`del `IsValid()` (metodo).</span><span class="sxs-lookup"><span data-stu-id="5987b-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="5987b-132">Ha un solo argomento (come un `out` parametro /`ByRef` parametro) che è di tipo `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="5987b-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="5987b-133">Rappresentazione di stringa contiene il motivo, quando il controllo ha esito negativo e `Valid` in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="5987b-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="5987b-134">Il codice seguente genera un messaggio in base alla `NoBot`del risultato:</span><span class="sxs-lookup"><span data-stu-id="5987b-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="5987b-135">Infine, è necessario un modulo per inviare e un elemento etichetta per il messaggio di output e aver!</span><span class="sxs-lookup"><span data-stu-id="5987b-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="5987b-136">Quando si esegue questo script e disattivare JavaScript o inviare il modulo entro i primi due secondi o inviare il modulo di sette volte all'interno di trenta secondi, si otterrà un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="5987b-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="5987b-137">Tuttavia utilizzare questo controllo con cautela, poiché solo 90-95% degli utenti hanno attivato JavaScript, pertanto circa 5-10% degli utenti avrà esito negativo `NoBot`del test.</span><span class="sxs-lookup"><span data-stu-id="5987b-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="5987b-138">[![Questo messaggio di errore potrebbe essere stato causato da un robot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5987b-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="5987b-139">Questo messaggio di errore potrebbe essere stato causato da un bot ([fare clic per visualizzare l'immagine con dimensioni normali](fighting-bots-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5987b-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5987b-140">Precedente</span><span class="sxs-lookup"><span data-stu-id="5987b-140">Previous</span></span>](fighting-bots-cs.md)
