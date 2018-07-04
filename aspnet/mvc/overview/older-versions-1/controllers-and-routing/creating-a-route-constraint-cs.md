---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Creazione di un vincolo di Route (c#) | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther spiega come è possibile controllare la modalità browser richiede le route di corrispondenza creando vincoli di route con espressioni regolari.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 8c977df126ce79f6ca20bd3941009ae7295ae0a5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366532"
---
<a name="creating-a-route-constraint-c"></a>Creazione di un vincolo di Route (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther spiega come è possibile controllare la modalità browser richiede le route di corrispondenza creando vincoli di route con espressioni regolari.


Vincoli di route vengono utilizzati per limitare le richieste del browser che corrispondono a una particolare route. È possibile usare un'espressione regolare per specificare un vincolo di route.

Si supponga, ad esempio, che è stata definita la route nel listato 1 nel file Global. asax.

**Listato 1 - Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

L'elenco 1 contiene una route denominata Product. È possibile usare la route di prodotto per eseguire il mapping alle richieste del browser il ProductController contenuta nel listato 2.

**Listato 2 - Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

Si noti che l'azione Details() esposto dal controller di prodotto accetta un singolo parametro denominato productId. Questo parametro è un parametro integer.

La route definita nel listato 1 corrisponderà a uno degli URL seguenti:

- / Prodotti/23
- / Prodotti/7

Sfortunatamente, la route corrisponde anche agli URL seguenti:

- / Prodotti/bLa
- / Prodotti/apple

Poiché l'azione Details() prevede un parametro intero, una richiesta che contiene un valore diverso da un valore intero verrà generato un errore. Ad esempio, se si digita il /Product/apple URL nel browser quindi si otterrà la pagina di errore nella figura 1.


[![La finestra di dialogo Nuovo progetto](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**Figura 01**: viene visualizzata una pagina explode ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-route-constraint-cs/_static/image2.png))


Che cosa si vuole eseguire è trovare solo gli URL contenenti un productId integer appropriata. È possibile usare un vincolo quando si definisce una route per limitare gli URL che corrispondono alla route. La route di prodotto modificata nel listato 3 contiene un vincolo di espressione regolare che corrisponde solo numeri interi.

**Listato 3 - Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

L'espressione regolare \d+ corrisponde a uno o più numeri interi. Questo vincolo fa sì che la route di prodotto in modo che corrispondano gli URL seguenti:

- / Prodotto/3
- / Prodotti/8999

Ma non gli URL seguenti:

- / Prodotti/apple
- / Prodotto

- Queste richieste del browser verranno gestite da un'altra route o, se non esistono alcuna route corrispondente, un *la risorsa non è stata trovata* viene restituito l'errore.

> [!div class="step-by-step"]
> [Precedente](creating-custom-routes-cs.md)
> [Successivo](creating-a-custom-route-constraint-cs.md)
