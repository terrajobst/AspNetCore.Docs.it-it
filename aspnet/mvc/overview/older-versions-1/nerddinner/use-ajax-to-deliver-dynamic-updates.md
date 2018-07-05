---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usare AJAX per distribuire gli aggiornamenti dinamici | Microsoft Docs
author: microsoft
description: Passaggio 10 implementa il supporto per gli utenti connessi a RSVP il proprio interesse si partecipa a un dinner, con un approccio basate su Ajax integrato in dettaglio la cena...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 789c9880dc43c5d15ee510d9856a4c4378019b71
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372330"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Usare AJAX per distribuire gli aggiornamenti dinamici
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 10 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.
> 
> Passaggio 10 implementa il supporto per gli utenti connessi a RSVP si partecipa a un dinner, usando un approccio basato su Ajax integrato all'interno della pagina di dettagli dinner il proprio interesse.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner passaggio 10: Accetta l'abilitazione di servizi AJAX

Verrà ora implementata ha eseguito l'accesso agli utenti di conferma la tua partecipazione si partecipa a un dinner il proprio interesse. Verrà ora abilitato questo usando un approccio basato su AJAX integrato all'interno della pagina di dettagli cena.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Che indica se l'utente è dato

Gli utenti possono visitare il */Dinners/dettagli / [id*] URL per visualizzare informazioni dettagliate su un particolare dinner:

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

Il Details() metodo di azione viene implementato come segue:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Il primo passaggio per implementare il supporto RSVP sarà possibile aggiungere un metodo di supporto "IsUserRegistered(username)" per l'oggetto Dinner (all'interno della classe parziale di Dinner.cs che abbiamo creato in precedenza). Questo metodo helper restituisce true o false a seconda del fatto che l'utente è attualmente dato per la cena:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

È quindi possibile aggiungere il codice seguente per il modello di visualizzazione Details per visualizzare un messaggio appropriato che indica se l'utente è registrato o meno per l'evento:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Oltre a questo punto quando un utente visita un Dinner sono registrati per a visualizzare questo messaggio:

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

E che visitano un Dinner non vengono registrati per vedranno il messaggio seguente:

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>L'implementazione del metodo di azione Register

Questo punto, aggiungere le funzionalità necessarie per consentire agli utenti di conferma la tua partecipazione un dinner dalla pagina dei dettagli.

A tale scopo, si creerà una nuova classe "RSVPController" facendo clic sulla directory \Controllers e scegliendo Aggiungi -&gt;comando di menu Controller.

Viene implementato un metodo di azione "Register" all'interno della nuova classe RSVPController che accetta un id per un Dinner come argomento, recupera l'oggetto Dinner appropriato, verifica se si trova nell'elenco di utenti che hanno registrato per tale utente ha eseguito l'accesso e se non aggiunge un oggetto RSVP per essi:

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Si noti che in precedenza come si sta restituendo una stringa semplice come l'output del metodo di azione. È stato possibile sono incorporati questo messaggio all'interno di un modello di vista, ma poiché è così piccolo appena utilizzeremo il metodo helper Content() sulla classe di base di controller e restituiscono una stringa di messaggio, ad esempio sopra.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>La chiamata al metodo di azione RSVPForEvent tramite AJAX

Useremo AJAX per richiamare il metodo di azione Register dalla visualizzazione dettagli. Questa implementazione è piuttosto semplice. Innanzitutto saranno aggiunti due riferimenti alla libreria di script:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

La prima libreria fa riferimento la libreria di script sul lato client AJAX ASP.NET core. Questo file è di circa 24 KB (compresso) e contiene funzionalità AJAX lato client principali. La libreria secondo contiene funzioni di utilità che si integrano con AJAX helper metodi predefiniti di ASP.NET MVC (che verrà usata a breve).

È possibile aggiornare il codice del modello di vista aggiunto in precedenza, in modo che invece l'output un messaggio "Non si è registrati per questo evento", è invece il rendering di un collegamento che al push esegue una chiamata AJAX che richiama il metodo di azione RSVPForEvent nel nostro controller RSVP e RSVPs utente:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

Il metodo helper Ajax.ActionLink() usato in precedenza è incorporato in ASP.NET MVC ed è simile al metodo helper Html.ActionLink() ad eccezione del fatto che invece di eseguire una navigazione standard effettua una chiamata AJAX al metodo di azione quando viene selezionato il collegamento. Sopra stiamo chiamando il metodo di azione "Register" per il controller "RSVP" e passandogli il DinnerID come parametro "id". Si passa al parametro AjaxOptions finale indica che si desidera richiedere il contenuto restituito dal metodo di azione e aggiornare il codice HTML &lt;div&gt; elemento nella pagina il cui id sia "rsvpmsg".

E ora quando un utente passa a un dinner che non sono registrati per ma visualizzano un collegamento a RSVP per tale:

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

Facendo clic sul collegamento "RSVP per questo evento di" verrà effettuare una chiamata AJAX al metodo di azione Register sul controller RSVP e quando viene completato verrà visualizzato un messaggio aggiornato simile alla seguente:

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

La larghezza di banda di rete e il traffico interessati durante la chiamata AJAX è davvero semplice. Quando l'utente fa clic sul collegamento "RSVP per questo evento", viene effettuata una richiesta di rete HTTP POST piccola per il */Dinners/Register/1* URL che ha un aspetto simile al seguente in transito:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

E la risposta dal nostro metodo di azione Register è semplicemente:

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Questa chiamata lightweight è veloce e funzionerà anche in una rete lenta.

### <a name="adding-a-jquery-animation"></a>Aggiunta di un'animazione di jQuery

La funzionalità AJAX che viene implementati funziona bene e fast. In alcuni casi può accadere in modo veloce, tuttavia, che un utente non è possibile notare che il collegamento RSVP è stato sostituito con il nuovo testo. Per rendere un po' più ovvi il risultato è possibile aggiungere un'animazione semplice per attirare l'attenzione al messaggio di aggiornamento.

L'impostazione predefinita, il modello di progetto ASP.NET MVC include jQuery: una libreria JavaScript open source eccellente (e molto diffuso) che è anche supportata da Microsoft. jQuery fornisce una serie di funzionalità, tra cui una libreria di selezione ed effetti DOM HTML nice.

Per l'uso di jQuery prima di tutto si aggiungerà un riferimento a script ad esso. Dato che si prevede di usare jQuery in un'ampia varietà di posizioni all'interno del nostro sito, si aggiungerà il riferimento allo script all'interno di nostro file pagina master Site. master, in modo che tutte le pagine possono usarlo.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Suggerimento: assicurarsi che sia installato l'hotfix di JavaScript intellisense per Visual Studio 2008 SP1 che consente il supporto di intellisense più completo per i file JavaScript (inclusi jQuery). È possibile scaricarlo da: http://tinyurl.com/vs2008javascripthotfix*

Codice scritto con JQuery spesso utilizza una globale "$ ()" metodo di JavaScript che recupera uno o più elementi HTML mediante un selettore CSS. Ad esempio, <em>$("#rsvpmsg")</em> consente di selezionare qualsiasi elemento HTML con l'id di rsvpmsg, mentre <em>$(".something")</em> selezionerebbe tutti gli elementi con la "cosa" CSS nome della classe. È anche possibile scrivere query più avanzate, ad esempio "restituire tutti i pulsanti di opzione selezionato" usando una query di selezione, ad esempio: <em>$("input [@type= radio] [@checked]")</em>.

Dopo aver selezionato gli elementi, è possibile chiamare metodi su di essi per agire, ad esempio nasconderli: *$("#rsvpmsg").hide();*

Per questo scenario RSVP, si definirà una semplice funzione JavaScript denominata "AnimateRSVPMessage" che consente di selezionare "rsvpmsg" &lt;div&gt; e aggiunge un'animazione le dimensioni del contenuto di testo. Il codice seguente viene avviato il testo di piccole dimensioni e quindi le cause che aumenti nel corso di un intervallo di tempo di 400 millisecondi:

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

È possibile quindi wireup questa funzione JavaScript da chiamare dopo la chiamata AJAX viene completato correttamente passando il nome per il metodo helper Ajax.ActionLink() (tramite AjaxOptions "OnSuccess" proprietà dell'evento):

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

E quando si fa clic sul collegamento "RSVP per questo evento" e la chiamata AJAX viene completato correttamente, il contenuto messaggio inviato a questo punto verrà animare back e aumento delle dimensioni:

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

Oltre a fornire un evento "OnSuccess", l'oggetto AjaxOptions espone metodi OnBegin OnFailure e OnComplete eventi che è possibile gestire (insieme a un'ampia gamma di altre proprietà e opzioni utili).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Pulizia - refactoring out una visualizzazione parziale RSVP

Il modello di visualizzazione dei dettagli sta iniziando a diventare più lunghi, quali straordinario renderà un po' più difficile da comprendere. Per contribuire a migliorare la leggibilità del codice, è possibile terminare mediante la creazione di una visualizzazione parziale: RSVPStatus.ascx – che incapsulano tutto il codice di visualizzazione RSVP per la pagina dei dettagli.

È possibile farlo facendo clic sulla cartella \Views\Dinners e quindi scegliendo Aggiungi -&gt;visualizzare comando di menu. È necessario accettano un oggetto Dinner come relativo elemento ViewModel fortemente tipizzato. È possibile quindi copiare e incollare il contenuto RSVP dalla visualizzazione Details. aspx al suo interno.

Una volta che viene completata l'operazione, creiamo inoltre un'altra visualizzazione parziale – EditAndDeleteLinks.ascx - che incapsula il codice di visualizzazione collegamento Edit e Delete. È prevista anche accettano un oggetto Dinner come relativo elemento ViewModel fortemente tipizzato e copiare e incollare la logica di modifica e l'eliminazione dalla visualizzazione Details. aspx al suo interno.

I dettagli modello consente di visualizzare, includono solo due chiamate al metodo Html.RenderPartial() nella parte inferiore:

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Questo rende più facili da leggere e gestire il codice.

### <a name="next-step"></a>Passo successivo

Ora esaminiamo come utilizzare AJAX ulteriormente e aggiungere il supporto di mapping interattivi all'applicazione.

> [!div class="step-by-step"]
> [Precedente](secure-applications-using-authentication-and-authorization.md)
> [Successivo](use-ajax-to-implement-mapping-scenarios.md)
