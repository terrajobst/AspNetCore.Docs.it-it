---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Utilizzare AJAX per distribuire gli aggiornamenti dinamici | Documenti Microsoft
author: microsoft
description: Passaggio 10 implementa il supporto per gli utenti connessi a RSVP desiderano che frequentano una cena, con un approccio basato su Ajax integrato in dettaglio la cena...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7cea3ee2ec52261521941efac484e91a53f6310b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870180"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Utilizzare AJAX per distribuire gli aggiornamenti dinamici
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 10 della procedura gratuito [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.
> 
> Passaggio 10 implementa il supporto per gli utenti connessi a RSVP che frequentano una cena, con un approccio basato su Ajax integrato all'interno della pagina dettagli dinner il proprio interesse.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner passaggio 10: Accetta AJAX abilitazione inviate risposte

Verrà ora implementata il supporto per gli utenti connessi a RSVP desiderano che frequentano una cena. Verrà scopo a con un approccio basato su AJAX integrato all'interno della pagina dettagli dinner.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Che indica se l'utente è RSVP'd

Gli utenti possono visitare il */Dinners/dettagli / [id*] URL per visualizzare informazioni dettagliate su un particolare dinner:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Ad esempio il Details() viene implementato il metodo di azione:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Il primo passaggio per implementare il supporto RSVP sarà per aggiungere un metodo di supporto "IsUserRegistered(username)" all'oggetto Dinner (all'interno della classe parziale di Dinner.cs che è stato creato in precedenza). Questo metodo di supporto restituisce true o false a seconda se l'utente è attualmente RSVP'd per la cena:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

È quindi possibile aggiungere il codice seguente per il modello di visualizzazione Details.aspx per visualizzare un messaggio appropriato che indica se l'utente è registrato o meno per l'evento:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

E ora quando un utente visita un Dinner sono registrati per cui verrà visualizzato il messaggio:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

E quando accedono a una cena non sono registrati per cui verrà visualizzato il messaggio seguente:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implementazione del metodo di azione di registrazione

A questo punto aggiungere le funzionalità necessarie per consentire agli utenti a RSVP per una cena dalla pagina dei dettagli.

A questo scopo, si creerà una nuova classe "RSVPController" facendo clic sulla directory \Controllers e scegliendo Aggiungi -&gt;comando di menu Controller.

Verrà implementato un metodo di azione "Register" all'interno della nuova classe RSVPController che accetta un id per un Dinner come argomento, recupera l'oggetto Dinner appropriato, controlla se l'utente connesso è attualmente nell'elenco di utenti che hanno registrato per tale e non aggiunge un oggetto RSVP per essi:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Si noti in precedenza come ci stiamo restituendo una stringa semplice come l'output del metodo di azione. È stato possibile incorporato il messaggio all'interno di un modello di visualizzazione: ma perché è così piccolo verrà utilizzato il metodo di supporto Content() solo sulla classe di base di controller e restituiscono una stringa di messaggio come sopra.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>La chiamata al metodo di azione RSVPForEvent con AJAX

Si userà AJAX per richiamare il metodo di azione di registrare la visualizzazione dettagli. Questa implementazione è piuttosto semplice. Innanzitutto saranno aggiunti i riferimenti alla libreria di due script:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

La prima libreria fa riferimento la libreria di script sul lato client AJAX ASP.NET di base. Questo file è di circa 24 KB in dimensioni (compressa) e contiene funzionalità AJAX di base sul lato client. La libreria secondo contiene le funzioni di utilità che si integrano con AJAX helper metodi incorporati di ASP.NET MVC (che verrà utilizzata più avanti).

È possibile quindi aggiorna il codice del modello di visualizzazione aggiunto in precedenza in modo che anziché outputing un messaggio "Non si è registrati per questo evento", è invece il rendering di un collegamento che quando inserito esegue una chiamata AJAX che richiama il metodo di azione RSVPForEvent nel nostro controller RSVP e RSVPs l'utente:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Il metodo di supporto Ajax.ActionLink() precedente è integrata in MVC ASP.NET ed è simile al metodo di supporto Html.ActionLink() ad eccezione del fatto che invece di eseguire un'operazione di navigazione standard esegue una chiamata AJAX per il metodo di azione quando viene selezionato il collegamento. Sopra stiamo chiamando il metodo di azione "Register" su "RSVP" controller e passandogli il DinnerID come parametro "id". Il parametro AjaxOptions finale viene passato indica che si desidera richiedere il contenuto restituito dal metodo di azione e aggiornare il codice HTML &lt;div&gt; elemento nella pagina il cui id è "rsvpmsg".

E l'ora, quando un utente passa a una cena che non sono registrati per ancora che verrà visualizzato un collegamento a RSVP per tale:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Se si fa clic sul collegamento "RSVP per questo evento" sarà rendono una chiamata AJAX per il metodo di azione di registrazione nel controller di RSVP, e quando viene completato verrà visualizzato un messaggio aggiornato come il seguente:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

La larghezza di banda e il traffico quando si effettua questa chiamata AJAX è davvero semplice. Quando l'utente fa clic sul collegamento "RSVP per questo evento", viene effettuata una richiesta di rete di HTTP POST piccola per il */Dinners/Register/1* URL che sembra sotto in transito:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

E la risposta dal metodo di azione di registro è semplicemente:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Questa chiamata lightweight è rapida e funzionerà anche in una rete lenta.

### <a name="adding-a-jquery-animation"></a>Aggiunta di un'animazione jQuery

La funzionalità AJAX che sono implementati funziona in modo corretto e veloce. Talvolta può verificarsi in modo rapido, tuttavia, che un utente non è possibile notare che il collegamento RSVP è stato sostituito con il nuovo testo. Per rendere un po' più ovvio il risultato è possibile aggiungere un'animazione semplice per richiamare l'attenzione al messaggio di aggiornamento.

L'impostazione predefinita, il modello di progetto ASP.NET MVC include jQuery: una libreria JavaScript open source eccellente (e molto diffuso) che è anche supportata da Microsoft. jQuery fornisce una serie di funzionalità, tra cui una libreria di selezione e gli effetti DOM HTML nice.

Per utilizzare jQuery innanzitutto verrà aggiunto un riferimento a script a esso. Poiché verrà utilizzato jQuery in diverse posizioni all'interno di questo sito, verrà aggiunto il riferimento allo script all'interno di questo file pagina master Site. master in modo che tutte le pagine è possono utilizzarlo.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Suggerimento: verificare che sia installato l'hotfix di JavaScript intellisense per Visual Studio 2008 SP1 che abilita il supporto intellisense più completo per i file JavaScript (inclusi jQuery). È possibile scaricarlo da: http://tinyurl.com/vs2008javascripthotfix*

Codice scritto con JQuery spesso viene utilizzata una globale "$ ()" metodo JavaScript che recupera uno o più elementi HTML mediante un selettore CSS. Ad esempio, <em>$("#rsvpmsg")</em> Seleziona tutti gli elementi HTML con l'id di rsvpmsg, mentre <em>$(".something")</em> selezionare tutti gli elementi con la "cosa" CSS nome della classe. È anche possibile scrivere query più avanzate, ad esempio "restituirà tutti i pulsanti di opzione selezionato" utilizzando una query di selezione come: <em>$("input [@type= radio] [@checked]")</em>.

Dopo aver selezionato gli elementi, è possibile chiamare metodi su di esse per eseguire l'azione, ad esempio nasconderli: *$("#rsvpmsg").hide();*

Per questo scenario RSVP, definiamo una semplice funzione JavaScript denominata "AnimateRSVPMessage" che consente di selezionare "rsvpmsg" &lt;div&gt; e aggiunge un'animazione le dimensioni del contenuto di testo. Il codice riportato di seguito viene avviato il testo di piccole dimensioni e cause per aumentare in un intervallo di tempo 400 millisecondi:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

È possibile quindi transito per questa funzione JavaScript da chiamare una volta completata la chiamata di AJAX passando il nome di metodo di supporto Ajax.ActionLink() (tramite AjaxOptions "OnSuccess" proprietà di evento):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

E ora quando viene selezionato il collegamento "RSVP per questo evento" e la chiamata di AJAX viene completato correttamente, il messaggio contenuto inviato sarà animare e aumento delle dimensioni grandi:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Oltre a fornire un evento "OnSuccess", l'oggetto AjaxOptions espone metodi OnBegin OnFailure e OnComplete eventi che è possibile gestire (insieme a un'ampia gamma di altre proprietà e opzioni utili).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Pulizia - effettuare il refactoring out una visualizzazione parziale RSVP

Il modello di visualizzazione dei dettagli avvio in corso ottenere un po' di tempo, quale straordinario renderà un po' più difficile da comprendere. Per migliorare la leggibilità del codice, completare mediante la creazione di una visualizzazione parziale – RSVPStatus.ascx – che incapsulano tutto il codice di visualizzazione RSVP per la pagina dei dettagli.

È possibile farlo facendo clic sulla cartella \Views\Dinners e quindi scegliendo Aggiungi -&gt;consente di visualizzare il comando di menu. È necessario accettano un oggetto Dinner come relativo ViewModel fortemente tipizzata. È possibile quindi copiare e incollare il contenuto RSVP dal nostro visualizzazione Details.aspx al suo interno.

Una volta che abbiamo realizzato che, verrà anche creare un'altra visualizzazione parziale – EditAndDeleteLinks.ascx - che incapsula il codice di visualizzazione di collegamento Edit e Delete. È anche necessario accettano un oggetto Dinner come relativo ViewModel fortemente tipizzati e copiare e incollare la logica di modifica e l'eliminazione dal nostro visualizzazione Details.aspx al suo interno.

Modello consente di visualizzare i dettagli, quindi includono solo due chiamate al metodo Html.RenderPartial() nella parte inferiore:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

In questo modo il codice di pulizia da leggere e gestire.

### <a name="next-step"></a>Passo successivo

Ora esaminiamo come è possibile utilizzare ulteriormente AJAX e aggiungere il supporto di mapping interattivo per l'applicazione.

> [!div class="step-by-step"]
> [Precedente](secure-applications-using-authentication-and-authorization.md)
> [Successivo](use-ajax-to-implement-mapping-scenarios.md)
