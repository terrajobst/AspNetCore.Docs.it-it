---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Creazione di un vincolo di Route personalizzati (VB) | Documenti Microsoft
author: StephenWalther
description: Stephen Walther viene illustrato come creare un vincolo di route personalizzati. Implementare una semplice personalizzato vincolo che impedisce l'elaborazione di una route corrispondente w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 094077fa0cb546f4cc91dbf074f8014e62b3b19c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-route-constraint-vb"></a>Creazione di un vincolo di Route personalizzati (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther viene illustrato come creare un vincolo di route personalizzati. Viene implementato un vincolo personalizzato semplice che impedisce che una route viene cercata la corrispondenza quando viene effettuata una richiesta del browser da un computer remoto.


L'obiettivo di questa esercitazione è dimostrare come creare un vincolo di route personalizzati. Un vincolo di route personalizzati consente di impedire che una route viene cercata la corrispondenza a meno che non esiste una corrispondenza per una condizione personalizzata.

In questa esercitazione viene creato un vincolo di route Localhost. Il vincolo di route Localhost corrisponde solo le richieste effettuate dal computer locale. Richieste remote da su Internet non corrispondono.

Si implementa un vincolo di route personalizzati implementando l'interfaccia IRouteConstraint. Questa è un'interfaccia molto semplice che descrive un singolo metodo:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

Il metodo restituisce un valore booleano. Se si restituisce False, la route associata con il vincolo non corrisponde alla richiesta di browser.

Il vincolo Localhost è contenuto in elenco 1.

**Elenco 1 - LocalhostConstraint.vb**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

Il vincolo nel listato 1 consente di sfruttare la proprietà IsLocal esposta dalla classe HttpRequest. Questa proprietà restituisce true quando l'indirizzo IP della richiesta è 127.0.0.1 o quando l'indirizzo IP della richiesta corrisponde all'indirizzo IP del server.

Utilizzare un vincolo personalizzato all'interno di una route definita nel file Global. asax. Il file Global. asax listato 2 utilizza il vincolo Localhost per impedire a utenti di richiedere una pagina di amministrazione, a meno che non rendono la richiesta dal server locale. Ad esempio, una richiesta per /Admin/DeleteAll avrà esito negativo quando effettuata da un server remoto.

**Listing 2 - Global.asax**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Il vincolo Localhost viene utilizzato nella definizione della route Admin. Una richiesta del browser remoto non può corrispondere a questa route. Si noti tuttavia che altre route definite in Global. asax potrebbero corrispondere la stessa richiesta. È importante tenere presente che un vincolo impedisce una route specifica una richiesta di corrispondenza e non tutte le route definite nel file Global. asax.

Si noti che la route predefinita è stata impostata come commento nel file Global. asax listato 2. Se si include la route predefinita, la route predefinita corrisponderebbe richieste per il controller di amministrazione. In tal caso, gli utenti remoti può richiamare ancora azioni del controller di amministrazione, anche se le richieste non corrispondono alla route di amministrazione.

> [!div class="step-by-step"]
> [Precedente](creating-a-route-constraint-vb.md)
