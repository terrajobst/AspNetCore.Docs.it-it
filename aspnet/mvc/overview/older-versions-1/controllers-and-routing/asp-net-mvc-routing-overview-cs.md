---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Panoramica del Routing di ASP.NET MVC (c#) | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther spiega come il framework ASP.NET MVC esegue il mapping alle richieste del browser per le azioni del controller.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: ff6d7a1540ee7e1de625f692bb5da2c28fdc57f7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388779"
---
<a name="aspnet-mvc-routing-overview-c"></a>Panoramica del Routing di ASP.NET MVC (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther spiega come il framework ASP.NET MVC esegue il mapping alle richieste del browser per le azioni del controller.


In questa esercitazione viene presentato una caratteristica importante di tutte le applicazioni ASP.NET MVC chiamata *Routing ASP.NET*. Il modulo di Routing di ASP.NET è responsabile del mapping di richieste del browser in ingresso alle azioni del controller MVC specifiche. Al termine di questa esercitazione, è possibile comprendere come la tabella di routing standard dei esegue il mapping delle richieste alle azioni del controller.

## <a name="using-the-default-route-table"></a>Usando la tabella di Route predefinita

Quando si crea una nuova applicazione MVC ASP.NET, l'applicazione è già configurata per usare il Routing di ASP.NET. Il Routing ASP.NET è configurata in due posizioni.

In primo luogo, il Routing di ASP.NET è abilitato nel file di configurazione dell'applicazione Web (file Web. config). Esistono quattro sezioni nel file di configurazione che sono rilevanti per il routing: la sezione system.web.httpModules, la sezione system.web.httpHandlers, la sezione system.webserver.modules e la sezione system.webserver.handlers. Prestare attenzione a non eliminare queste sezioni, perché senza queste sezioni routing non funzionerà più.

In secondo luogo e ancora più importante, viene creata una tabella di route nel file Global. asax dell'applicazione. Il file Global. asax è un file speciale che contiene i gestori eventi per eventi del ciclo di vita dell'applicazione ASP.NET. La tabella di route viene creata durante l'evento di avvio dell'applicazione.

Il file nel listato 1 contiene il file Global. asax predefinito per un'applicazione ASP.NET MVC.

**Listato 1 - Global.asax.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

Quando un'applicazione MVC primo avvio, l'applicazione\_viene chiamato il metodo Start (). Questo metodo, a sua volta, chiama il metodo RegisterRoutes(). Il metodo RegisterRoutes() crea la tabella di route.

La tabella di route predefinito contiene una singola route (denominata Default). La route predefinita mappa il primo segmento di URL per un nome di controller, il secondo segmento di URL di un'azione del controller e il terzo segmento per un parametro denominato **id**.

Si supponga che si immette l'URL seguente nella barra degli indirizzi del web browser:

/ Home/indice/3

La route predefinita esegue il mapping di questo URL per i parametri seguenti:

- controller = Home

- azione = indice

- id = 3

Quando si richiede l'URL avremo/indice/3, viene eseguito il codice seguente:

HomeController.Index(3)

La route predefinita include le impostazioni predefinite per tutti i tre parametri. Se non si fornisce un controller, quindi il parametro controller viene impostato sul valore **Home**. Se non viene fornita un'azione, il parametro action viene impostata sul valore **indice**. Infine, se non si fornisce un id, il parametro id viene impostato su una stringa vuota.

Esaminiamo alcuni esempi di come la route predefinita esegue il mapping degli URL alle azioni del controller. Si supponga che si immette l'URL seguente nella barra degli indirizzi del browser:

/Home

A causa di valori di parametro predefiniti della route predefinita, immettere questo URL causerà il metodo Index () della classe HomeController nel listato 2 da chiamare.

**Listato 2 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

Nel listato 2, la classe HomeController include un metodo denominato Index () che accetta un singolo parametro denominato ID. Avremo l'URL, il metodo Index () essere chiamato con una stringa vuota come valore del parametro Id.

A causa della modalità che il framework di MVC richiama le azioni del controller, avremo l'URL corrisponda anche al metodo Index () della classe HomeController nel listato 3.

**Listato 3 - HomeController.cs (operazione sull'indice senza parametri)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

Il metodo Index () nel listato 3 non accetta alcun parametro. L'URL avremo causerà questa chiamata al metodo Index (). Inoltre, l'URL avremo/indice/3 richiama questo metodo (l'Id viene ignorato).

Avremo l'URL corrisponda anche al metodo Index () della classe HomeController nel listato 4.

**Listato 4 - HomeController.cs (operazione sull'indice con il parametro ammette valori null)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

Nel listato 4, il metodo Index () ha un solo parametro Integer. Poiché il parametro è un parametro nullable (può avere il valore Null), l'indice può essere chiamato senza generare un errore.

Infine, richiamare il metodo Index () nel listato 5 con l'URL avremo genera un'eccezione poiché il parametro Id *non è* parametro ammette valori null. Se si prova a richiamare il metodo Index () viene visualizzato l'errore visualizzato nella figura 1.

**Listato 5 - HomeController.cs (operazione sull'indice con il parametro Id)**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


[![Richiamo di un'azione del controller che prevede un valore di parametro](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**Figura 01**: richiamare un'azione del controller che prevede un valore di parametro ([fare clic per visualizzare l'immagine con dimensioni normali](asp-net-mvc-routing-overview-cs/_static/image2.png))


L'URL avremo/indice/3, d'altra parte, funziona perfettamente con l'azione Index del controller nel listato 5. La richiesta /Home/Index/3 induce il metodo Index () essere chiamato con un parametro di Id con il valore 3.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è stata per fornire una breve introduzione al Routing di ASP.NET. Abbiamo esaminato la tabella di route predefinito che si ottiene con una nuova applicazione MVC ASP.NET. Si è appreso come la route predefinita esegue il mapping degli URL alle azioni del controller.

> [!div class="step-by-step"]
> [avanti](understanding-action-filters-cs.md)
