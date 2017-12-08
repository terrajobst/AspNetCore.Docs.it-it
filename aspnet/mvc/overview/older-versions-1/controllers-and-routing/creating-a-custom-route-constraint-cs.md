---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Creazione di un vincolo di Route personalizzati (c#) | Documenti Microsoft
author: StephenWalther
description: Stephen Walther viene illustrato come creare un vincolo di route personalizzati. Implementare una semplice personalizzato vincolo che impedisce l'elaborazione di una route corrispondente w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: c31ba3382b9dbe22a6826b9f858944c223efdd9d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-route-constraint-c"></a>Creazione di un vincolo di Route personalizzati (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther viene illustrato come creare un vincolo di route personalizzati. Viene implementato un vincolo personalizzato semplice che impedisce che una route viene cercata la corrispondenza quando viene effettuata una richiesta del browser da un computer remoto.


L'obiettivo di questa esercitazione è dimostrare come creare un vincolo di route personalizzati. Un vincolo di route personalizzati consente di impedire che una route viene cercata la corrispondenza a meno che non esiste una corrispondenza per una condizione personalizzata.

In questa esercitazione viene creato un vincolo di route Localhost. Il vincolo di route Localhost corrisponde solo le richieste effettuate dal computer locale. Richieste remote da su Internet non corrispondono.

Si implementa un vincolo di route personalizzati implementando l'interfaccia IRouteConstraint. Questa è un'interfaccia molto semplice che descrive un singolo metodo:

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

Il metodo restituisce un valore booleano. Se si restituisce false, la route associata con il vincolo non corrisponde alla richiesta di browser.

Il vincolo Localhost è contenuto in elenco 1.

**Elenco 1 - LocalhostConstraint.cs**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

Il vincolo nel listato 1 consente di sfruttare la proprietà IsLocal esposta dalla classe HttpRequest. Questa proprietà restituisce true quando l'indirizzo IP della richiesta è 127.0.0.1 o quando l'indirizzo IP della richiesta corrisponde all'indirizzo IP del server.

Utilizzare un vincolo personalizzato all'interno di una route definita nel file Global. asax. Il file Global. asax listato 2 utilizza il vincolo Localhost per impedire a utenti di richiedere una pagina di amministrazione, a meno che non rendono la richiesta dal server locale. Ad esempio, una richiesta per /Admin/DeleteAll avrà esito negativo quando effettuata da un server remoto.

**Elenco di 2 - Global. asax**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

Il vincolo Localhost viene utilizzato nella definizione della route Admin. Una richiesta del browser remoto non può corrispondere a questa route. Si noti tuttavia che altre route definite in Global. asax potrebbero corrispondere la stessa richiesta. È importante tenere presente che un vincolo impedisce una route specifica una richiesta di corrispondenza e non tutte le route definite nel file Global. asax.

Si noti che la route predefinita è stata impostata come commento nel file Global. asax listato 2. Se si include la route predefinita, la route predefinita corrisponderebbe richieste per il controller di amministrazione. In tal caso, gli utenti remoti può richiamare ancora azioni del controller di amministrazione, anche se le richieste non corrispondono alla route di amministrazione.

>[!div class="step-by-step"]
[Precedente](creating-a-route-constraint-cs.md)
[Successivo](asp-net-mvc-controller-overview-vb.md)
