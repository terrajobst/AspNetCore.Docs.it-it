---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Creazione di un vincolo di Route personalizzato (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther viene illustrato come creare un vincolo di route personalizzati. Abbiamo implementato una semplice personalizzato vincolo che impedisce a una route corrispondente w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: d088380152adcb025857176b4396cab48fa64b66
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833421"
---
<a name="creating-a-custom-route-constraint-vb"></a>Creazione di un vincolo di Route personalizzato (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther viene illustrato come creare un vincolo di route personalizzati. Si implementa un vincolo personalizzato semplice che impedisce che una route viene cercata la corrispondenza quando viene effettuata una richiesta del browser da un computer remoto.


L'obiettivo di questa esercitazione è dimostrare come è possibile creare un vincolo di route personalizzati. Un vincolo di route personalizzati consente di impedire che una route viene cercata la corrispondenza a meno che non esiste una corrispondenza per una condizione personalizzata.

In questa esercitazione viene creato un vincolo di route Localhost. Il vincolo di route Localhost corrisponde solo le richieste effettuate dal computer locale. Richieste remote da attraverso la rete Internet non corrispondono.

Si implementa un vincolo di route personalizzate implementando l'interfaccia IRouteConstraint. Si tratta di un'interfaccia estremamente semplice che descrive un singolo metodo:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

Il metodo restituisce un valore booleano. Se si restituisce False, la route associata con il vincolo non corrisponde alla richiesta del browser.

Il vincolo Localhost è contenuto nel listato 1.

**Listato 1 - LocalhostConstraint.vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

Il vincolo nel listato 1 consente di sfruttare la proprietà IsLocal esposta dalla classe HttpRequest. Questa proprietà restituisce true quando l'indirizzo IP della richiesta è 127.0.0.1 o quando l'indirizzo IP della richiesta è lo stesso indirizzo IP del server.

Si usa vincolo personalizzato all'interno di una route definita nel file Global. asax. Il file Global. asax nel listato 2 Usa il vincolo di Localhost per impedire agli utenti di richiedere una pagina di amministrazione, a meno che non effettuano la richiesta dal server locale. Ad esempio, una richiesta per /Admin/DeleteAll avrà esito negativo quando effettuata da un server remoto.

**Listato 2 - Global. asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Il vincolo Localhost viene usato nella definizione della route Admin. Questa route non corrispondere a una richiesta del browser remoto. Tenere presente, tuttavia, che altri route definite in Global. asax potrebbero corrispondere alla stessa richiesta. È importante comprendere che un vincolo impedisce a una particolare route di una richiesta di corrispondenza e non tutte le route definite nel file Global. asax.

Si noti che la route predefinita è stata commento dal file Global. asax nel listato 2. Se si include la route predefinita, la route predefinita corrisponderebbe richieste per il controller di amministrazione. In tal caso, gli utenti remoti potrebbe ancora richiamare azioni del controller di amministrazione anche se le richieste non corrispondono alla route di amministratore.

> [!div class="step-by-step"]
> [Precedente](creating-a-route-constraint-vb.md)
