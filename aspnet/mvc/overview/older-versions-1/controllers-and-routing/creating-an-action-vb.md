---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Creazione di un'azione (VB) | Documenti Microsoft
author: microsoft
description: Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per un metodo da un'azione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d1d355599c17df597f9c08d9d7f129fffc1a2e4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-action-vb"></a>Creazione di un'azione (VB)
====================
da [Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per un metodo da un'azione.


L'obiettivo di questa esercitazione è illustrare come è possibile creare una nuova azione del controller. Informazioni sui requisiti di un metodo di azione. Inoltre informazioni su come evitare che un metodo viene esposta come un'azione.

## <a name="adding-an-action-to-a-controller"></a>Aggiunta di un'azione a un Controller

Aggiungere una nuova azione a un controller aggiungendo un nuovo metodo al controller. Ad esempio, il controller nel listato 1 contiene un'azione denominata index () e un'azione denominata sayHello (). Entrambi i metodi vengono esposti come azioni.

**Elenco 1 - Controllers\HomeController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Per poter essere esposto all'universo come un'azione, un metodo deve soddisfare determinati requisiti:

- Il metodo deve essere pubblico.
- Il metodo non può essere un metodo statico.
- Il metodo non può essere un metodo di estensione.
- Il metodo non può essere un costruttore, getter o setter.
- Il metodo non può avere tipi generici aperti.
- Il metodo non è un metodo della classe controller.
- Il metodo non può contenere **ref** o **out** parametri.

Si noti che non sono previste restrizioni sul tipo restituito di un'azione del controller. Un'azione del controller può restituire una stringa, un valore DateTime, un'istanza della classe Random, o void. Il framework di MVC ASP.NET verrà convertire un tipo restituito non è il risultato di un'azione in una stringa ed eseguire il rendering di stringa per il browser.

Quando si aggiunge un metodo che non violi questi requisiti per un controller, il metodo viene esposta come un'azione del controller. Prestare attenzione qui. Un'azione del controller può essere richiamata da qualsiasi utente connesso a Internet. Non, ad esempio, creare un'azione del controller DeleteMyWebsite().

## <a name="preventing-a-public-method-from-being-invoked"></a>Impedisce che viene richiamato un metodo pubblico

Se è necessario creare un metodo pubblico in una classe controller e non si desidera esporre il metodo come un'azione del controller, quindi è possibile impedire che il metodo richiamato utilizzando il &lt;NonAction&gt; attributo. Ad esempio, il controller nel listato 2 contiene un metodo pubblico denominato CompanySecrets() decorata con il &lt;NonAction&gt; attributo.

**Elenco di 2 - Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Se si tenta di richiamare l'azione del controller CompanySecrets() digitando /Work/CompanySecrets nella barra degli indirizzi del browser si otterrà il messaggio di errore nella figura 1.


[![Richiama un metodo NonAction](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Figura 01**: chiamata di un metodo NonAction ([fare clic per visualizzare l'immagine ingrandita](creating-an-action-vb/_static/image2.png))

>[!div class="step-by-step"]
[Precedente](creating-a-controller-vb.md)
[Successivo](aspnet-mvc-controllers-overview-cs.md)
