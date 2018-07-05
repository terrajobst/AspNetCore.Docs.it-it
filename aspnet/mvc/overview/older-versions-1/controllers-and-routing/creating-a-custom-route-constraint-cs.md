---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Creazione di un vincolo di Route personalizzato (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther viene illustrato come creare un vincolo di route personalizzati. Abbiamo implementato una semplice personalizzato vincolo che impedisce a una route corrispondente w...
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: e21e7e027cf66f390fc37ec08a07ae007e8242c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831813"
---
<a name="creating-a-custom-route-constraint-c"></a>Creazione di un vincolo di Route personalizzato (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther viene illustrato come creare un vincolo di route personalizzati. Si implementa un vincolo personalizzato semplice che impedisce che una route viene cercata la corrispondenza quando viene effettuata una richiesta del browser da un computer remoto.


L'obiettivo di questa esercitazione è dimostrare come è possibile creare un vincolo di route personalizzati. Un vincolo di route personalizzati consente di impedire che una route viene cercata la corrispondenza a meno che non esiste una corrispondenza per una condizione personalizzata.

In questa esercitazione viene creato un vincolo di route Localhost. Il vincolo di route Localhost corrisponde solo le richieste effettuate dal computer locale. Richieste remote da attraverso la rete Internet non corrispondono.

Si implementa un vincolo di route personalizzate implementando l'interfaccia IRouteConstraint. Si tratta di un'interfaccia estremamente semplice che descrive un singolo metodo:

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

Il metodo restituisce un valore booleano. Se si restituisce false, la route associata con il vincolo non corrisponde alla richiesta del browser.

Il vincolo Localhost è contenuto nel listato 1.

**Listato 1 - LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

Il vincolo nel listato 1 consente di sfruttare la proprietà IsLocal esposta dalla classe HttpRequest. Questa proprietà restituisce true quando l'indirizzo IP della richiesta è 127.0.0.1 o quando l'indirizzo IP della richiesta è lo stesso indirizzo IP del server.

Si usa vincolo personalizzato all'interno di una route definita nel file Global. asax. Il file Global. asax nel listato 2 Usa il vincolo di Localhost per impedire agli utenti di richiedere una pagina di amministrazione, a meno che non effettuano la richiesta dal server locale. Ad esempio, una richiesta per /Admin/DeleteAll avrà esito negativo quando effettuata da un server remoto.

**Listato 2 - Global. asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

Il vincolo Localhost viene usato nella definizione della route Admin. Questa route non corrispondere a una richiesta del browser remoto. Tenere presente, tuttavia, che altri route definite in Global. asax potrebbero corrispondere alla stessa richiesta. È importante comprendere che un vincolo impedisce a una particolare route di una richiesta di corrispondenza e non tutte le route definite nel file Global. asax.

Si noti che la route predefinita è stata commento dal file Global. asax nel listato 2. Se si include la route predefinita, la route predefinita corrisponderebbe richieste per il controller di amministrazione. In tal caso, gli utenti remoti potrebbe ancora richiamare azioni del controller di amministrazione anche se le richieste non corrispondono alla route di amministratore.

> [!div class="step-by-step"]
> [Precedente](creating-a-route-constraint-cs.md)
> [Successivo](asp-net-mvc-controller-overview-vb.md)
