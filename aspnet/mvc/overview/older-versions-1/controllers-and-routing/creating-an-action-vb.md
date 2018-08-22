---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Creazione di un'azione (VB) | Microsoft Docs
author: microsoft
description: Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per un metodo su cui eseguire un'azione.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 070dd4c9d68327eec52fe385000b9ca3907eaa9f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826990"
---
<a name="creating-an-action-vb"></a>Creazione di un'azione (VB)
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per un metodo su cui eseguire un'azione.


L'obiettivo di questa esercitazione è illustrare come è possibile creare una nuova azione del controller. Informazioni sui requisiti di un metodo di azione. Anche informazioni su come impedire che un metodo viene esposta come un'azione.

## <a name="adding-an-action-to-a-controller"></a>Aggiunta di un'azione a un Controller

Aggiungere una nuova azione a un controller aggiungendo un nuovo metodo al controller. Ad esempio, il controller nel listato 1 contiene un'azione denominata index () e un'azione denominata sayHello (). Entrambi i metodi vengono esposti come azioni.

**Listato 1 - Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Per poter essere esposto di Universe (universo) come un'azione, un metodo deve soddisfare determinati requisiti:

- Il metodo deve essere pubblico.
- Il metodo non può essere un metodo statico.
- Il metodo non può essere un metodo di estensione.
- Il metodo non può essere un costruttore, getter o setter.
- Il metodo non può avere tipi generici aperti.
- Il metodo non è un metodo della classe di base di controller.
- Il metodo non può contenere **ref** oppure **out** parametri.

Si noti che non sono previste restrizioni sul tipo restituito di un'azione del controller. Un'azione del controller può restituire una stringa, un valore DateTime, un'istanza della classe Random, o void. Il framework ASP.NET MVC convertirà qualsiasi tipo restituito non è il risultato di un'azione in una stringa e la stringa al browser di eseguire il rendering.

Quando si aggiunge un metodo che non violano tali requisiti per un controller, il metodo viene esposta come un'azione del controller. Prestare attenzione qui. Un'azione del controller può essere richiamata da qualsiasi utente connesso a Internet. Non, ad esempio, creare un'azione del controller DeleteMyWebsite().

## <a name="preventing-a-public-method-from-being-invoked"></a>Impedisce che un metodo pubblico da richiamare

Se è necessario creare un metodo pubblico in una classe controller e non si vuole esporre il metodo come un'azione del controller, quindi è possibile impedire che il metodo viene richiamato usando il &lt;NonAction&gt; attributo. Ad esempio, il controller nel listato 2 contiene un metodo pubblico denominato CompanySecrets() decorata con il &lt;NonAction&gt; attributo.

**Listato 2 - Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Se si prova a richiamare l'azione del controller CompanySecrets() digitando /Work/CompanySecrets nella barra degli indirizzi del browser si riceverà il messaggio di errore nella figura 1.


[![Richiama un metodo NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Figura 01**: richiama un metodo NonAction ([fare clic per visualizzare l'immagine con dimensioni normali](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Precedente](creating-a-controller-vb.md)
> [Successivo](aspnet-mvc-controllers-overview-cs.md)
