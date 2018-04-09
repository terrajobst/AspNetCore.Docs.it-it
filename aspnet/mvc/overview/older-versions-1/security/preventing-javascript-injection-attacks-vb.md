---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Prevenzione degli attacchi Injection JavaScript (VB) | Documenti Microsoft
author: StephenWalther
description: Evitare attacchi Injection JavaScript e Cross-Site Scripting attacchi accada all'utente. In questa esercitazione, Stephen Walther viene spiegato come aggiungere facilmente de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: cb19236b22abd455472621ce74a8cddf9752d6c5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="preventing-javascript-injection-attacks-vb"></a>Prevenzione degli attacchi Injection JavaScript (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

[Scarica il PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Evitare attacchi Injection JavaScript e Cross-Site Scripting attacchi accada all'utente. In questa esercitazione, Stephen Walther viene illustrato come è possibile annullare facilmente questi tipi di attacchi di codifica del contenuto HTML.


L'obiettivo di questa esercitazione è illustrare come è possibile impedire attacchi intrusivi nel codice JavaScript nelle applicazioni ASP.NET MVC. In questa esercitazione vengono descritti due approcci per difendersi il sito Web da attacchi intrusivi nel codice JavaScript. Informazioni su come impedire attacchi injection JavaScript la codifica dei dati che si visualizzano. Inoltre informazioni su come evitare attacchi injection JavaScript la codifica dei dati che si accettano.

## <a name="what-is-a-javascript-injection-attack"></a>Che cos'è un attacco intrusivo nel codice JavaScript?

Ogni volta che si accetta l'input dell'utente e visualizzare nuovamente l'input dell'utente, si apre il sito Web da attacchi intrusivi nel codice JavaScript. Verrà ora esaminata un'applicazione concreta vulnerabili agli attacchi intrusivi nel codice JavaScript.

Si supponga di aver creato un sito di commenti e suggerimenti del cliente (vedere la figura 1). I clienti possono visitare il sito Web e immettere i commenti e suggerimenti sull'esperienza relativa utilizzando i prodotti. Quando un cliente invia i commenti e suggerimenti, commenti e suggerimenti viene nuovamente visualizzato la pagina dei commenti e suggerimenti.


[![Sito Web di commenti e suggerimenti clienti](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Figura 01**: sito Web di commenti e suggerimenti cliente ([fare clic per visualizzare l'immagine ingrandita](preventing-javascript-injection-attacks-vb/_static/image3.png))


Il sito di commenti e suggerimenti del cliente utilizza il `controller` nel listato 1. Questo `controller` contiene due azioni denominate `Index()` e `Create()`.

**Elenco 1: `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

Il `Index()` metodo visualizza il `Index` visualizzazione. Questo metodo passa tutti i suggerimenti dei clienti precedenti per il `Index` visualizzazione recuperando i commenti dal database (tramite una query LINQ to SQL).

Il `Create()` metodo crea un nuovo elemento di Feedback e lo aggiunge al database. Il messaggio che il cliente viene inserito nel modulo viene passato per il `Create()` metodo nel parametro del messaggio. Viene creato un elemento di Feedback e il messaggio viene assegnato all'elemento di Feedback `Message` proprietà. L'elemento di Feedback viene inviato al database con il `DataContext.SubmitChanges()` chiamata al metodo. Infine, il visitatore viene reindirizzato al `Index` Visualizza tutti i commenti e suggerimenti in cui vengono visualizzati.

Il `Index` Vista è contenuta nel listato 2.

**Elenco 2: `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

Il `Index` Vista dispone di due sezioni. Nella sezione superiore contiene la forma di commenti e suggerimenti dei clienti. Nella sezione inferiore contiene un ciclo For... Ogni ciclo che esegue il ciclo di tutti gli elementi di commenti e suggerimenti cliente precedente e visualizza le proprietà EntryDate e messaggio per ogni elemento di feedback.

Sito Web di commenti e suggerimenti del cliente è un semplice sito Web. Purtroppo, il sito Web sono vulnerabili agli attacchi intrusivi nel codice JavaScript.

Si supponga che in form di commenti e suggerimenti dei clienti è immettere il testo seguente:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Questo testo rappresenta uno script JavaScript che visualizza una finestra di messaggio di avviso. Modulo dopo che un utente invia commenti e suggerimenti questo script, il messaggio <em>Boo!</em> verrà visualizzata ogni volta che chiunque visite in sito Web di commenti e suggerimenti del cliente in futuro (vedere la figura 2).


[![Attacco intrusivo nel codice JavaScript](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Figura 02**: Injection JavaScript ([fare clic per visualizzare l'immagine ingrandita](preventing-javascript-injection-attacks-vb/_static/image6.png))


A questo punto, la risposta iniziale per attacchi intrusivi nel codice JavaScript può essere apathy. Si potrebbe pensare che gli attacchi injection JavaScript sono semplicemente un tipo di *alterazione* attacco. Potresti ritenere che nessuno possa eseguire alcuna operazione realmente male eseguendo il commit di un attacco intrusivo nel codice JavaScript.

Purtroppo, un pirata informatico effettuare alcune effettivamente, operazioni effettivamente male inserendo JavaScript in un sito Web. È possibile utilizzare un attacco intrusivo nel codice JavaScript per eseguire un attacco di Cross-Site Scripting (XSS). In un attacco di tipo Cross-Site Scripting rubare informazioni utente riservate e inviare le informazioni a un altro sito Web.

Ad esempio, un pirata informatico può usare un attacco intrusivo nel codice JavaScript per rubare i valori dei cookie del browser da altri utenti. Se le informazioni riservate, ad esempio password, numeri di carta di credito o numeri di previdenza sociale: sono archiviate nei cookie del browser, un pirata informatico può utilizzare un attacco intrusivo nel codice JavaScript per rubare le informazioni. In alternativa, se un utente immette le informazioni riservate in un campo del form contenuto in una pagina che sia stata compromessa con un attacco di JavaScript, il pirata informatico è possibile usare il codice JavaScript inserito per acquisire i dati del form e inviarlo a un altro sito Web.

*Per essere impaurito*. Richiedere gravemente attacchi intrusivi nel codice JavaScript e proteggere le informazioni riservate dell'utente. Nelle due sezioni, esaminati due tecniche che è possibile utilizzare per proteggere le applicazioni ASP.NET MVC da attacchi intrusivi nel codice JavaScript.

## <a name="approach-1-html-encode-in-the-view"></a>Approccio #1: Codifica HTML nella visualizzazione

Un metodo semplice per impedire attacchi injection JavaScript è in formato HTML codificare tutti i dati immessi dagli utenti del sito Web quando vengono visualizzati nuovamente i dati in una vista. L'aggiornamento `Index` visualizzazione listato 3 segue questo approccio.

**Listato 3 – `Index.aspx` (codificato in formato HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Si noti che il valore di `feedback.Message` HTML con codificata prima che venga visualizzato il valore con il codice seguente:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

Cosa Media in formato HTML codificare una stringa? Quando si HTML codifica una stringa, pericolose caratteri, ad esempio `<` e `>` sono sostituiti dai riferimenti alle entità HTML, ad esempio `&lt;` e `&gt;`. Quando la stringa `<script>alert("Boo!")</script>` HTML con codifica, viene convertito in `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. La stringa codificata non viene più eseguito come script JavaScript quando vengono interpretati da un browser. Ottenere invece la pagina innocua nella figura 3.


[![Attacco JavaScript annullato](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Figura 03**: aggirato attacco JavaScript ([fare clic per visualizzare l'immagine ingrandita](preventing-javascript-injection-attacks-vb/_static/image9.png))


Si noti che il `Index` visualizzare listato 3 solo il valore di `feedback.Message` viene codificato. Il valore di `feedback.EntryDate` non viene codificato. È necessario solo codificare i dati immessi da un utente. Poiché il valore di EntryDate è stato generato nel controller, è non necessario in formato HTML codificare questo valore.

## <a name="approach-2-html-encode-in-the-controller"></a>Approccio #2: Codifica HTML nel Controller

Anziché alla codifica HTML dati quando si visualizzano i dati in una vista, è possibile HTML codificare i dati appena prima di inviare i dati al database. In caso di adottare questo approccio secondo il `controller` listato 4.

**Listato 4 – `HomeController.cs` (codificato in formato HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Si noti che il valore del messaggio è HTML con codificato prima che il valore viene inviato al database all'interno di `Create()` azione. Quando il messaggio verrà nuovamente visualizzato nella visualizzazione, il messaggio è codificato in formato HTML e codice JavaScript inserito nel messaggio non viene eseguito.

È preferibile in genere, il primo approccio descritto in questa esercitazione su questo secondo approccio. Il problema con questo secondo approccio è che alla fine si dati codificato in formato HTML nel database. In altre parole, i dati di database sono stati modificati con divertenti caratteri professionale.

Perché è errato? Se è necessario visualizzare i dati di database in un valore diverso da una pagina web, sarà necessario problemi. Ad esempio, è possibile visualizzare non è più facilmente i dati in un'applicazione Windows Form.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione era spaventare sulla prospettiva di un attacco intrusivo nel codice JavaScript. In questa esercitazione descritti due approcci per proteggere le applicazioni ASP.NET MVC da attacchi injection JavaScript il: è possibile entrambi HTML codificare utente inviati dati nella vista del oppure è possono HTML codificare utente inviati dati nel controller.

> [!div class="step-by-step"]
> [Precedente](authenticating-users-with-windows-authentication-vb.md)
