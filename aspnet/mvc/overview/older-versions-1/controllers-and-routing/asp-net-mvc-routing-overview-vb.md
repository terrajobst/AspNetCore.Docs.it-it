---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: Panoramica di Routing di ASP.NET MVC (VB) | Documenti Microsoft
author: StephenWalther
description: In questa esercitazione, Stephen Walther Mostra come framework di MVC ASP.NET esegue il mapping alle richieste del browser per le azioni del controller.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 3de0e21552a4aa03aa21f21a4e26028f1475f3e9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879206"
---
<a name="aspnet-mvc-routing-overview-vb"></a>Panoramica di Routing di ASP.NET MVC (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther Mostra come framework di MVC ASP.NET esegue il mapping alle richieste del browser per le azioni del controller.


In questa esercitazione vengono introdotte a una caratteristica importante di ogni applicazione MVC ASP.NET denominato *Routing ASP.NET*. Il modulo di Routing di ASP.NET è responsabile per il mapping in ingresso alle richieste del browser per le azioni del controller MVC specifiche. Al termine di questa esercitazione, è possibile comprendere come tabella di routing standard esegue il mapping delle richieste per le azioni del controller.

## <a name="using-the-default-route-table"></a>Utilizzando la tabella di Route predefinita

Quando si crea una nuova applicazione MVC ASP.NET, l'applicazione è già configurato per utilizzare il Routing di ASP.NET. Routing ASP.NET sia configurato in due posizioni.

In primo luogo, il Routing di ASP.NET è abilitato nel file di configurazione dell'applicazione Web (file Web. config). Sono presenti quattro sezioni nel file di configurazione rilevanti per il routing: la sezione system.web.httpModules, la sezione system.web.httpHandlers, la sezione system.webserver.modules e la sezione system.webserver.handlers. Prestare attenzione a non eliminare queste sezioni perché senza queste sezioni routing non funzionerà più.

In secondo luogo e soprattutto, nel file Global. asax dell'applicazione viene creata una tabella di route. Il file Global. asax è un file speciale che contiene i gestori eventi per eventi del ciclo di vita dell'applicazione ASP.NET. La tabella di route viene creata durante l'evento di avvio dell'applicazione.

Il file di listato 1 contiene il file Global. asax predefinito per un'applicazione MVC ASP.NET.

**Listing 1 - Global.asax.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

Quando un'applicazione MVC prima avvia, l'applicazione\_viene chiamato il metodo Start (). Questo metodo, a sua volta chiama il metodo di RegisterRoutes(). Il metodo RegisterRoutes() crea la tabella di route.

La tabella di route predefinita contiene una singola route (denominata valore predefinito). La route predefinita esegue il mapping il primo segmento di un URL a un nome del controller, il secondo segmento di un URL per un'azione del controller e il terzo segmento in un parametro denominato **id**.

Si supponga che si immette l'URL seguente nella barra degli indirizzi del web browser:

/Home/Index/3

La route predefinita esegue il mapping di questo URL per i parametri seguenti:

- controller = Home

- Azione = indice

- id = 3

Quando si richiede il 3/indice/URL /Home, viene eseguito il codice seguente:

HomeController.Index(3)

La route predefinita include le impostazioni predefinite per tutti e tre i parametri. Se non si specifica un controller, il parametro controller per impostazione predefinita sarà il valore **Home**. Se non si specifica un'azione, il parametro di azione viene impostata sul valore **indice**. Infine, se non si specifica un id, il parametro id per impostazione predefinita in una stringa vuota.

Ecco alcuni esempi di come la route predefinita esegue il mapping degli URL per le azioni del controller. Si supponga che si immette l'URL seguente nella barra degli indirizzi del browser:

/Home

A causa di valori di parametro predefiniti della route predefinito, immettere l'URL verrà causa l'index () del metodo della classe HomeController listato 2 da chiamare.

**Listing 2 - HomeController.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

Nel listato 2, la classe HomeController include un metodo denominato Index () che accetta un singolo parametro denominato ID. /Home l'URL, il metodo Index () essere chiamato con il valore Nothing come valore del parametro Id.

A causa della modalità che il framework di MVC richiama le azioni del controller, l'URL di /Home corrisponde anche il metodo Index () della classe HomeController listato 3.

**Elenco di 3 - HomeController.vb (operazione sull'indice senza parametri)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

Il metodo Index () nel listato 3 non accetta parametri. L'URL di /Home causerà questa chiamata al metodo Index (). URL /Home/indice/3 richiama anche questo metodo (l'Id viene ignorato).

L'URL di /Home corrisponde anche il metodo Index () della classe HomeController listato 4.

**Listato 4 - HomeController.vb (operazione sull'indice con il parametro ammette valori null)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

Listato 4, il metodo Index () ha un parametro di tipo Integer. Poiché il parametro è un parametro ammette valori null (può avere il valore Nothing), l'indice può essere chiamato senza che venga generato un errore.

Infine, richiamare il metodo Index () nel listato 5 con l'URL di /Home genera un'eccezione perché il parametro Id *non* un parametro ammette valori null. Se si tenta di richiamare il metodo Index () restituito l'errore visualizzato nella figura 1.

**Nel listato 5 - HomeController.vb (operazione sull'indice con il parametro Id)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


[![Richiamare un'azione del controller che è previsto un valore di parametro](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)

**Figura 01**: richiamare un'azione del controller che è previsto un valore di parametro ([fare clic per visualizzare l'immagine ingrandita](asp-net-mvc-routing-overview-vb/_static/image2.png))


URL /Home/indice/3, d'altra parte, funziona perfettamente con l'azione del controller di indice nel listato 5. /Home/Index/3 la richiesta, il metodo Index () essere chiamato con un parametro Id con il valore 3.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è stata per fornire una breve introduzione al Routing di ASP.NET. Abbiamo esaminato la tabella di route predefinita che ottiene con una nuova applicazione MVC ASP.NET. Si è appreso come la route predefinita esegue il mapping degli URL per le azioni del controller.

> [!div class="step-by-step"]
> [Precedente](creating-an-action-cs.md)
> [Successivo](understanding-action-filters-vb.md)
