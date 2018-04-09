---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Creazione di un vincolo di Route (VB) | Documenti Microsoft
author: StephenWalther
description: In questa esercitazione, Stephen Walther viene illustrato come sia possibile controllare il browser richiede route corrispondenza creando vincoli della route con espressioni regolari.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f50b371ac679218b06c4848e6d33516d29d3a82
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-route-constraint-vb"></a>Creazione di un vincolo di Route (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther viene illustrato come sia possibile controllare il browser richiede route corrispondenza creando vincoli della route con espressioni regolari.


Utilizzare i vincoli della route per limitare le richieste del browser che corrispondono a una route specifica. È possibile utilizzare un'espressione regolare per specificare un vincolo di route.

Ad esempio, si supponga che sia stata definita la route nel listato 1 nel file Global. asax.

**Listing 1 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

Elenco 1 contiene una route denominata Product. È possibile utilizzare la route di prodotto per eseguire il mapping alle richieste del browser il ProductController contenuti nel listato 2.

**Listing 2 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Si noti che l'azione Details() esposto dal controller di prodotto accetta un singolo parametro denominato productId. Questo parametro è un parametro di tipo integer.

La route definita nel listato 1 corrisponderà a uno degli URL seguenti:

- / Prodotto/23
- / Prodotto/7

Purtroppo, la route corrisponde anche i seguenti URL:

- / Prodotti/bLa
- / Prodotti/apple

Poiché l'azione Details() prevede un parametro intero, effettua una richiesta che contiene un valore diverso da un valore intero verrà generato un errore. Ad esempio, se si digita il /Product/apple URL nel browser si otterranno la pagina di errore nella figura 1.


[![La finestra di dialogo Nuovo progetto](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Figura 01**: una pagina visualizzata esplodere ([fare clic per visualizzare l'immagine ingrandita](creating-a-route-constraint-vb/_static/image2.png))


Cosa si vuole eseguire è trovare solo gli URL che contengono un productId intero appropriato. È possibile utilizzare un vincolo quando si definisce una route per limitare gli URL che corrispondono alla route. La route di prodotto modificata nel listato 3 contiene un vincolo di espressione regolare che corrisponde solo numeri interi.

**Listing 3 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

\D+ l'espressione regolare corrisponde a uno o più numeri interi. Questo vincolo fa sì che la route prodotto corrispondere i seguenti URL:

- / Prodotto/3
- / Prodotti/8999

Ma non gli URL seguenti:

- / Prodotti/apple
- / Prodotto

Queste richieste del browser verranno gestite da un'altra route o, se non esistono route corrispondente, un *non è stata trovata la risorsa* viene restituito l'errore.

> [!div class="step-by-step"]
> [Precedente](creating-custom-routes-vb.md)
> [Successivo](creating-a-custom-route-constraint-vb.md)
