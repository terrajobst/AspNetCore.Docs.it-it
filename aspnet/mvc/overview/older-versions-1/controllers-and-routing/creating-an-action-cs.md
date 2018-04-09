---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Creazione di un'azione (c#) | Documenti Microsoft
author: microsoft
description: Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per un metodo da un'azione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c6145902db59b07e96a5563b138c1a6323946b2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-action-c"></a>Creazione di un'azione (c#)
====================
by [Microsoft](https://github.com/microsoft)

> Informazioni su come aggiungere una nuova azione a un controller MVC ASP.NET. Informazioni sui requisiti per un metodo da un'azione.


L'obiettivo di questa esercitazione è illustrare come è possibile creare una nuova azione del controller. Informazioni sui requisiti di un metodo di azione. Inoltre informazioni su come evitare che un metodo viene esposta come un'azione.

## <a name="adding-an-action-to-a-controller"></a>Aggiunta di un'azione a un Controller

Aggiungere una nuova azione a un controller aggiungendo un nuovo metodo al controller. Ad esempio, il controller nel listato 1 contiene un'azione denominata index () e un'azione denominata sayHello (). Entrambi i metodi vengono esposti come azioni.

**Elenco 1 - controllers\homecontroller.cs.**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

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

Se è necessario creare un metodo pubblico in una classe controller e non si desidera esporre il metodo come un'azione del controller quindi è possibile evitare che il metodo richiamato utilizzando l'attributo [NonAction]. Ad esempio, il controller nel listato 2 contiene un metodo pubblico denominato CompanySecrets() decorata con l'attributo [NonAction].

**Il listato 2 - Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Se si tenta di richiamare l'azione del controller CompanySecrets() digitando /Work/CompanySecrets nella barra degli indirizzi del browser si otterrà il messaggio di errore nella figura 1.


[![Richiamare un metodo NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Figura 01**: chiamata di un metodo NonAction ([fare clic per visualizzare l'immagine ingrandita](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Precedente](creating-a-controller-cs.md)
> [Successivo](asp-net-mvc-routing-overview-vb.md)
