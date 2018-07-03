---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Prevenzione degli attacchi Injection JavaScript (VB) | Microsoft Docs
author: StephenWalther
description: Evitare attacchi Injection JavaScript e gli attacchi di Cross-Site Scripting accada all'utente. In questa esercitazione, Stephen Walther spiega come è possibile eseguire facilmente Germania...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c022847462239624d5b84816999918ec77f8337
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402341"
---
<a name="preventing-javascript-injection-attacks-vb"></a>Prevenzione degli attacchi Injection JavaScript (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

[Scaricare PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Evitare attacchi Injection JavaScript e gli attacchi di Cross-Site Scripting accada all'utente. In questa esercitazione, Stephen Walther spiega come si possono annullare facilmente questi tipi di attacchi di codifica del contenuto HTML.


L'obiettivo di questa esercitazione è illustrare come è possibile impedire attacchi intrusivi nel codice JavaScript nelle applicazioni ASP.NET MVC. Questa esercitazione illustra due approcci per la difesa contro il sito Web da attacchi intrusivi nel codice JavaScript. Descrive come evitare attacchi injection JavaScript codificando i dati visualizzati. Anche informazioni su come evitare attacchi injection JavaScript codificando i dati che accettano.

## <a name="what-is-a-javascript-injection-attack"></a>Che cos'è un attacco Injection JavaScript?

Ogni volta che si accettano input dall'utente e quindi visualizzare di nuovo l'input dell'utente, si apre il sito Web da attacchi intrusivi nel codice JavaScript. Verrà ora esaminato un'applicazione concreta aperta agli attacchi intrusivi nel codice JavaScript.

Si supponga che sia stato creato un sito di commenti e suggerimenti dei clienti (vedere la figura 1). I clienti possono visitare il sito Web e inserire commenti e suggerimenti sulla loro esperienza usando i propri prodotti. Quando un cliente invia i commenti e suggerimenti, commenti e suggerimenti viene nuovamente visualizzato la pagina commenti e suggerimenti.


[![Sito Web di commenti e suggerimenti dei clienti](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Figura 01**: sito Web di commenti e suggerimenti dei clienti ([fare clic per visualizzare l'immagine con dimensioni normali](preventing-javascript-injection-attacks-vb/_static/image3.png))


Il sito Web commenti e suggerimenti dei clienti Usa il `controller` nel listato 1. Ciò `controller` contiene due azioni denominate `Index()` e `Create()`.

**Listato 1: `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

Il `Index()` metodo visualizza il `Index` vista. Questo metodo passa tutti i commenti e suggerimenti dei clienti precedenti per il `Index` vista recuperando i commenti e suggerimenti dal database (tramite una query LINQ to SQL).

Il `Create()` metodo crea un nuovo elemento di Feedback e lo aggiunge al database. Il messaggio che il cliente immette del modulo viene passato per il `Create()` metodo nel parametro del messaggio. Viene creato un elemento di Feedback e il messaggio viene assegnato all'elemento dei commenti `Message` proprietà. L'elemento di Feedback viene inviato al database con il `DataContext.SubmitChanges()` chiamata al metodo. Infine, il visitatore viene reindirizzato al `Index` Visualizza tutti i commenti e suggerimenti in cui viene visualizzato.

Il `Index` Vista è contenuta nel listato 2.

**Listato 2: `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

Il `Index` visualizzazione composta da due sezioni. Nella sezione superiore contiene la forma di commenti e suggerimenti di clienti reali. Nella sezione inferiore contiene un ciclo For.. Ciclo foreach che scorre in ciclo tutti gli elementi di commenti e suggerimenti dei clienti precedenti e visualizza le proprietà EntryDate e messaggio per ogni elemento dei commenti.

Il sito Web dei clienti è un semplice sito Web. Sfortunatamente, il sito Web è vulnerabile agli attacchi injection JavaScript.

Si supponga che si immette il testo seguente nel modulo di commenti e suggerimenti dei clienti:

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Questo testo rappresenta uno script JavaScript che visualizza una finestra di messaggio di avviso. Dopo che un utente invia lo script in commenti e suggerimenti modulo, il messaggio <em>Boo!</em> verrà visualizzata ogni volta che chiunque visiti il sito Web dei clienti che a in futuro (vedere la figura 2).


[![Attacchi Injection JavaScript](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Figura 02**: attacchi Injection JavaScript ([fare clic per visualizzare l'immagine con dimensioni normali](preventing-javascript-injection-attacks-vb/_static/image6.png))


A questo punto, la risposta iniziale a attacchi injection JavaScript potrebbe essere apatia. Si potrebbe pensare che degli attacchi injection JavaScript sono semplicemente un tipo di *al danneggiamento* attacco. Si potrebbe ritenere che non possono eseguire qualsiasi operazione davvero evil eseguendo il commit di un attacco injection JavaScript.

Sfortunatamente, un pirata informatico può eseguire tutte o alcune davvero, davvero evil cose inserendo JavaScript in un sito Web. È possibile usare un attacco injection JavaScript per eseguire un attacco di tipo Cross-Site Scripting (XSS). In un attacco di tipo Cross-Site Scripting viene rubare le informazioni utente riservate e invierà tali informazioni a un altro sito Web.

Ad esempio, un pirata informatico può usare un attacco injection JavaScript per rubare i valori dei cookie del browser da altri utenti. Se le informazioni sensibili, ad esempio password, numeri di carta di credito o codici fiscali – sono archiviate nei cookie del browser, un pirata informatico può usare un attacco injection JavaScript per rubare le informazioni. In alternativa, se un utente immette le informazioni riservate in un campo del form contenuto in una pagina in cui è stato compromesso da un attacco di JavaScript, quindi l'utente malintenzionato può usare il codice JavaScript inserito per recuperare i dati del form e inviarlo a un altro sito Web.

*. È anche possibile mischiare*. Seriamente degli attacchi injection JavaScript e proteggere le informazioni riservate dell'utente. In due sezioni successive, vengono illustrati due tecniche che è possibile usare per difendere le applicazioni MVC ASP.NET da attacchi intrusivi nel codice JavaScript.

## <a name="approach-1-html-encode-in-the-view"></a>Approccio 1 #: Codifica HTML nella visualizzazione

Un metodo semplice di prevenzione degli attacchi injection JavaScript è in formato HTML codificare tutti i dati immessi dagli utenti di sito Web quando vengono visualizzati nuovamente i dati in una vista. Aggiornato `Index` visualizzazione nel listato 3 segue questo approccio.

**Listato 3 – `Index.aspx` (codificato in formato HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Si noti che il valore di `feedback.Message` è in formato HTML con codificata prima che venga visualizzato il valore con il codice seguente:

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

Che cosa significa in formato HTML codificare una stringa? Quando si HTML codifica una stringa, pericoloso caratteri, ad esempio `<` e `>` vengono sostituiti dai riferimenti alle entità HTML, ad esempio `&lt;` e `&gt;`. Pertanto quando la stringa `<script>alert("Boo!")</script>` è in formato HTML con codifica, verranno convertiti in `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. La stringa con codifica non viene più eseguito come script JavaScript quando vengono interpretati da un browser. Al contrario, viene visualizzata la pagina innocua nella figura 3.


[![Attacco JavaScript annullato](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Figura 03**: annullato attacco JavaScript ([fare clic per visualizzare l'immagine con dimensioni normali](preventing-javascript-injection-attacks-vb/_static/image9.png))


Si noti che il `Index` visualizzare nel listato 3 solo il valore di `feedback.Message` è codificato. Il valore di `feedback.EntryDate` non viene codificato. Devi solo codificare i dati immessi dall'utente. Poiché il valore di EntryDate è stato generato nel controller, è non necessario in formato HTML codificare questo valore.

## <a name="approach-2-html-encode-in-the-controller"></a>Approccio #2: Codifica HTML del controller

Anziché la codifica dei dati quando si visualizzano i dati in una visualizzazione HTML, si può HTML codificare i dati prima di inviare i dati nel database. Questo secondo approccio viene adottato nel caso del `controller` nel listato 4.

**Listato 4 – `HomeController.cs` (codificato in formato HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Si noti che il valore del messaggio è HTML con codificata prima che il valore venga inviato al database all'interno di `Create()` azione. Quando il messaggio viene nuovamente visualizzato nella vista, il messaggio è codificato in formato HTML e codice JavaScript inserito nel messaggio non viene eseguito.

In genere, è consigliabile favorire il primo approccio descritto in questa esercitazione in questo secondo approccio. Il problema con questo secondo approccio è che si otterrà i dati con codificata HTML del database. In altre parole, i dati del database sono danneggiati con caratteri dall'aspetto professionale divertenti.

Perché è non valido? Se è necessario visualizzare i dati del database in un valore diverso da una pagina web, si avranno problemi. Ad esempio, è possibile visualizzare non è più facilmente i dati in un'applicazione Windows Form.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione è stata spaventare sulla prospettiva di un attacco injection JavaScript. Questa esercitazione descritti due approcci per la difesa contro le applicazioni MVC ASP.NET da attacchi injection JavaScript: è possibile entrambi HTML codificare utente inviate dati nella vista del oppure si possono HTML codificare utente inviati dati nel controller.

> [!div class="step-by-step"]
> [Precedente](authenticating-users-with-windows-authentication-vb.md)
