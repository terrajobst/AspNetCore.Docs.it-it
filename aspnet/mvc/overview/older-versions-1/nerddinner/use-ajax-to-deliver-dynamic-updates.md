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
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="8e244-103">Utilizzare AJAX per distribuire gli aggiornamenti dinamici</span><span class="sxs-lookup"><span data-stu-id="8e244-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="8e244-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8e244-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="8e244-105">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="8e244-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="8e244-106">Si tratta di passaggio 10 della procedura gratuito [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="8e244-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="8e244-107">Passaggio 10 implementa il supporto per gli utenti connessi a RSVP che frequentano una cena, con un approccio basato su Ajax integrato all'interno della pagina dettagli dinner il proprio interesse.</span><span class="sxs-lookup"><span data-stu-id="8e244-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="8e244-108">Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="8e244-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="8e244-109">NerdDinner passaggio 10: Accetta AJAX abilitazione inviate risposte</span><span class="sxs-lookup"><span data-stu-id="8e244-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="8e244-110">Verrà ora implementata il supporto per gli utenti connessi a RSVP desiderano che frequentano una cena.</span><span class="sxs-lookup"><span data-stu-id="8e244-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="8e244-111">Verrà scopo a con un approccio basato su AJAX integrato all'interno della pagina dettagli dinner.</span><span class="sxs-lookup"><span data-stu-id="8e244-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="8e244-112">Che indica se l'utente è RSVP'd</span><span class="sxs-lookup"><span data-stu-id="8e244-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="8e244-113">Gli utenti possono visitare il */Dinners/dettagli / [id*] URL per visualizzare informazioni dettagliate su un particolare dinner:</span><span class="sxs-lookup"><span data-stu-id="8e244-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="8e244-114">Ad esempio il Details() viene implementato il metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="8e244-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="8e244-115">Il primo passaggio per implementare il supporto RSVP sarà per aggiungere un metodo di supporto "IsUserRegistered(username)" all'oggetto Dinner (all'interno della classe parziale di Dinner.cs che è stato creato in precedenza).</span><span class="sxs-lookup"><span data-stu-id="8e244-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="8e244-116">Questo metodo di supporto restituisce true o false a seconda se l'utente è attualmente RSVP'd per la cena:</span><span class="sxs-lookup"><span data-stu-id="8e244-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="8e244-117">È quindi possibile aggiungere il codice seguente per il modello di visualizzazione Details.aspx per visualizzare un messaggio appropriato che indica se l'utente è registrato o meno per l'evento:</span><span class="sxs-lookup"><span data-stu-id="8e244-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="8e244-118">E ora quando un utente visita un Dinner sono registrati per cui verrà visualizzato il messaggio:</span><span class="sxs-lookup"><span data-stu-id="8e244-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="8e244-119">E quando accedono a una cena non sono registrati per cui verrà visualizzato il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="8e244-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="8e244-120">Implementazione del metodo di azione di registrazione</span><span class="sxs-lookup"><span data-stu-id="8e244-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="8e244-121">A questo punto aggiungere le funzionalità necessarie per consentire agli utenti a RSVP per una cena dalla pagina dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="8e244-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="8e244-122">A questo scopo, si creerà una nuova classe "RSVPController" facendo clic sulla directory \Controllers e scegliendo Aggiungi -&gt;comando di menu Controller.</span><span class="sxs-lookup"><span data-stu-id="8e244-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="8e244-123">Verrà implementato un metodo di azione "Register" all'interno della nuova classe RSVPController che accetta un id per un Dinner come argomento, recupera l'oggetto Dinner appropriato, controlla se l'utente connesso è attualmente nell'elenco di utenti che hanno registrato per tale e non aggiunge un oggetto RSVP per essi:</span><span class="sxs-lookup"><span data-stu-id="8e244-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="8e244-124">Si noti in precedenza come ci stiamo restituendo una stringa semplice come l'output del metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="8e244-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="8e244-125">È stato possibile incorporato il messaggio all'interno di un modello di visualizzazione: ma perché è così piccolo verrà utilizzato il metodo di supporto Content() solo sulla classe di base di controller e restituiscono una stringa di messaggio come sopra.</span><span class="sxs-lookup"><span data-stu-id="8e244-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="8e244-126">La chiamata al metodo di azione RSVPForEvent con AJAX</span><span class="sxs-lookup"><span data-stu-id="8e244-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="8e244-127">Si userà AJAX per richiamare il metodo di azione di registrare la visualizzazione dettagli.</span><span class="sxs-lookup"><span data-stu-id="8e244-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="8e244-128">Questa implementazione è piuttosto semplice.</span><span class="sxs-lookup"><span data-stu-id="8e244-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="8e244-129">Innanzitutto saranno aggiunti i riferimenti alla libreria di due script:</span><span class="sxs-lookup"><span data-stu-id="8e244-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="8e244-130">La prima libreria fa riferimento la libreria di script sul lato client AJAX ASP.NET di base.</span><span class="sxs-lookup"><span data-stu-id="8e244-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="8e244-131">Questo file è di circa 24 KB in dimensioni (compressa) e contiene funzionalità AJAX di base sul lato client.</span><span class="sxs-lookup"><span data-stu-id="8e244-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="8e244-132">La libreria secondo contiene le funzioni di utilità che si integrano con AJAX helper metodi incorporati di ASP.NET MVC (che verrà utilizzata più avanti).</span><span class="sxs-lookup"><span data-stu-id="8e244-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="8e244-133">È possibile quindi aggiorna il codice del modello di visualizzazione aggiunto in precedenza in modo che anziché outputing un messaggio "Non si è registrati per questo evento", è invece il rendering di un collegamento che quando inserito esegue una chiamata AJAX che richiama il metodo di azione RSVPForEvent nel nostro controller RSVP e RSVPs l'utente:</span><span class="sxs-lookup"><span data-stu-id="8e244-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="8e244-134">Il metodo di supporto Ajax.ActionLink() precedente è integrata in MVC ASP.NET ed è simile al metodo di supporto Html.ActionLink() ad eccezione del fatto che invece di eseguire un'operazione di navigazione standard esegue una chiamata AJAX per il metodo di azione quando viene selezionato il collegamento.</span><span class="sxs-lookup"><span data-stu-id="8e244-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="8e244-135">Sopra stiamo chiamando il metodo di azione "Register" su "RSVP" controller e passandogli il DinnerID come parametro "id".</span><span class="sxs-lookup"><span data-stu-id="8e244-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="8e244-136">Il parametro AjaxOptions finale viene passato indica che si desidera richiedere il contenuto restituito dal metodo di azione e aggiornare il codice HTML &lt;div&gt; elemento nella pagina il cui id è "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="8e244-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="8e244-137">E l'ora, quando un utente passa a una cena che non sono registrati per ancora che verrà visualizzato un collegamento a RSVP per tale:</span><span class="sxs-lookup"><span data-stu-id="8e244-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="8e244-138">Se si fa clic sul collegamento "RSVP per questo evento" sarà rendono una chiamata AJAX per il metodo di azione di registrazione nel controller di RSVP, e quando viene completato verrà visualizzato un messaggio aggiornato come il seguente:</span><span class="sxs-lookup"><span data-stu-id="8e244-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="8e244-139">La larghezza di banda e il traffico quando si effettua questa chiamata AJAX è davvero semplice.</span><span class="sxs-lookup"><span data-stu-id="8e244-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="8e244-140">Quando l'utente fa clic sul collegamento "RSVP per questo evento", viene effettuata una richiesta di rete di HTTP POST piccola per il */Dinners/Register/1* URL che sembra sotto in transito:</span><span class="sxs-lookup"><span data-stu-id="8e244-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="8e244-141">E la risposta dal metodo di azione di registro è semplicemente:</span><span class="sxs-lookup"><span data-stu-id="8e244-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="8e244-142">Questa chiamata lightweight è rapida e funzionerà anche in una rete lenta.</span><span class="sxs-lookup"><span data-stu-id="8e244-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="8e244-143">Aggiunta di un'animazione jQuery</span><span class="sxs-lookup"><span data-stu-id="8e244-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="8e244-144">La funzionalità AJAX che sono implementati funziona in modo corretto e veloce.</span><span class="sxs-lookup"><span data-stu-id="8e244-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="8e244-145">Talvolta può verificarsi in modo rapido, tuttavia, che un utente non è possibile notare che il collegamento RSVP è stato sostituito con il nuovo testo.</span><span class="sxs-lookup"><span data-stu-id="8e244-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="8e244-146">Per rendere un po' più ovvio il risultato è possibile aggiungere un'animazione semplice per richiamare l'attenzione al messaggio di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="8e244-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="8e244-147">L'impostazione predefinita, il modello di progetto ASP.NET MVC include jQuery: una libreria JavaScript open source eccellente (e molto diffuso) che è anche supportata da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8e244-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="8e244-148">jQuery fornisce una serie di funzionalità, tra cui una libreria di selezione e gli effetti DOM HTML nice.</span><span class="sxs-lookup"><span data-stu-id="8e244-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="8e244-149">Per utilizzare jQuery innanzitutto verrà aggiunto un riferimento a script a esso.</span><span class="sxs-lookup"><span data-stu-id="8e244-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="8e244-150">Poiché verrà utilizzato jQuery in diverse posizioni all'interno di questo sito, verrà aggiunto il riferimento allo script all'interno di questo file pagina master Site. master in modo che tutte le pagine è possono utilizzarlo.</span><span class="sxs-lookup"><span data-stu-id="8e244-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="8e244-151">*Suggerimento: verificare che sia installato l'hotfix di JavaScript intellisense per Visual Studio 2008 SP1 che abilita il supporto intellisense più completo per i file JavaScript (inclusi jQuery). È possibile scaricarlo da: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="8e244-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="8e244-152">Codice scritto con JQuery spesso viene utilizzata una globale "$ ()" metodo JavaScript che recupera uno o più elementi HTML mediante un selettore CSS.</span><span class="sxs-lookup"><span data-stu-id="8e244-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="8e244-153">Ad esempio, <em>$("#rsvpmsg")</em> Seleziona tutti gli elementi HTML con l'id di rsvpmsg, mentre <em>$(".something")</em> selezionare tutti gli elementi con la "cosa" CSS nome della classe.</span><span class="sxs-lookup"><span data-stu-id="8e244-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="8e244-154">È anche possibile scrivere query più avanzate, ad esempio "restituirà tutti i pulsanti di opzione selezionato" utilizzando una query di selezione come: <em>$("input [@type= radio] [@checked]")</em>.</span><span class="sxs-lookup"><span data-stu-id="8e244-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="8e244-155">Dopo aver selezionato gli elementi, è possibile chiamare metodi su di esse per eseguire l'azione, ad esempio nasconderli: *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="8e244-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="8e244-156">Per questo scenario RSVP, definiamo una semplice funzione JavaScript denominata "AnimateRSVPMessage" che consente di selezionare "rsvpmsg" &lt;div&gt; e aggiunge un'animazione le dimensioni del contenuto di testo.</span><span class="sxs-lookup"><span data-stu-id="8e244-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="8e244-157">Il codice riportato di seguito viene avviato il testo di piccole dimensioni e cause per aumentare in un intervallo di tempo 400 millisecondi:</span><span class="sxs-lookup"><span data-stu-id="8e244-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="8e244-158">È possibile quindi transito per questa funzione JavaScript da chiamare una volta completata la chiamata di AJAX passando il nome di metodo di supporto Ajax.ActionLink() (tramite AjaxOptions "OnSuccess" proprietà di evento):</span><span class="sxs-lookup"><span data-stu-id="8e244-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="8e244-159">E ora quando viene selezionato il collegamento "RSVP per questo evento" e la chiamata di AJAX viene completato correttamente, il messaggio contenuto inviato sarà animare e aumento delle dimensioni grandi:</span><span class="sxs-lookup"><span data-stu-id="8e244-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="8e244-160">Oltre a fornire un evento "OnSuccess", l'oggetto AjaxOptions espone metodi OnBegin OnFailure e OnComplete eventi che è possibile gestire (insieme a un'ampia gamma di altre proprietà e opzioni utili).</span><span class="sxs-lookup"><span data-stu-id="8e244-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="8e244-161">Pulizia - effettuare il refactoring out una visualizzazione parziale RSVP</span><span class="sxs-lookup"><span data-stu-id="8e244-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="8e244-162">Il modello di visualizzazione dei dettagli avvio in corso ottenere un po' di tempo, quale straordinario renderà un po' più difficile da comprendere.</span><span class="sxs-lookup"><span data-stu-id="8e244-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="8e244-163">Per migliorare la leggibilità del codice, completare mediante la creazione di una visualizzazione parziale – RSVPStatus.ascx – che incapsulano tutto il codice di visualizzazione RSVP per la pagina dei dettagli.</span><span class="sxs-lookup"><span data-stu-id="8e244-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="8e244-164">È possibile farlo facendo clic sulla cartella \Views\Dinners e quindi scegliendo Aggiungi -&gt;consente di visualizzare il comando di menu.</span><span class="sxs-lookup"><span data-stu-id="8e244-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="8e244-165">È necessario accettano un oggetto Dinner come relativo ViewModel fortemente tipizzata.</span><span class="sxs-lookup"><span data-stu-id="8e244-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="8e244-166">È possibile quindi copiare e incollare il contenuto RSVP dal nostro visualizzazione Details.aspx al suo interno.</span><span class="sxs-lookup"><span data-stu-id="8e244-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="8e244-167">Una volta che abbiamo realizzato che, verrà anche creare un'altra visualizzazione parziale – EditAndDeleteLinks.ascx - che incapsula il codice di visualizzazione di collegamento Edit e Delete.</span><span class="sxs-lookup"><span data-stu-id="8e244-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="8e244-168">È anche necessario accettano un oggetto Dinner come relativo ViewModel fortemente tipizzati e copiare e incollare la logica di modifica e l'eliminazione dal nostro visualizzazione Details.aspx al suo interno.</span><span class="sxs-lookup"><span data-stu-id="8e244-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="8e244-169">Modello consente di visualizzare i dettagli, quindi includono solo due chiamate al metodo Html.RenderPartial() nella parte inferiore:</span><span class="sxs-lookup"><span data-stu-id="8e244-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="8e244-170">In questo modo il codice di pulizia da leggere e gestire.</span><span class="sxs-lookup"><span data-stu-id="8e244-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="8e244-171">Passo successivo</span><span class="sxs-lookup"><span data-stu-id="8e244-171">Next Step</span></span>

<span data-ttu-id="8e244-172">Ora esaminiamo come è possibile utilizzare ulteriormente AJAX e aggiungere il supporto di mapping interattivo per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8e244-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8e244-173">[Precedente](secure-applications-using-authentication-and-authorization.md)
> [Successivo](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="8e244-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
