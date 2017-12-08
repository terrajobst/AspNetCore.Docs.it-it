---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Panoramica del Controller MVC ASP.NET (VB) | Documenti Microsoft
author: StephenWalther
description: In questa esercitazione, Stephen Walther presenta controller MVC ASP.NET. Informazioni su come creare nuovi controller e restituire tipi diversi di res azione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 452e2cf771e8b1f298ce9693ec2a707e7c1d4dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-controller-overview-vb"></a>Panoramica del Controller MVC ASP.NET (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther presenta controller MVC ASP.NET. Informazioni su come creare nuovi controller e restituire diversi tipi di risultati dell'azione.


In questa esercitazione viene illustrato l'argomento di controller MVC ASP.NET, le azioni del controller e i risultati dell'azione. Dopo aver completato questa esercitazione, è possibile comprendere come i controller vengono utilizzati per controllare il modo in cui che un visitatore interagisce con un sito Web ASP.NET MVC.

## <a name="understanding-controllers"></a>Informazioni sui controller

Controller MVC sono responsabili per rispondere alle richieste effettuate per un sito Web ASP.NET MVC. Ogni richiesta del browser viene eseguito il mapping a un controller specifico. Si supponga, ad esempio, che si immette l'URL seguente nella barra degli indirizzi del browser:

`http://localhost/Product/Index/3`

In questo caso, viene richiamato un controller denominato ProductController. Il ProductController è responsabile della generazione la risposta alla richiesta del browser. Ad esempio, il controller potrebbe restituire una visualizzazione specifica al browser o il controller può reindirizzare l'utente a un altro controller.

Elenco 1 contiene un controller semplice denominato ProductController.

**Listing1 - Controllers\ProductController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

Come si può vedere dal listato 1, un controller è semplicemente una classe (una classe .NET di Visual Basic o c#). Un controller è una classe che deriva dalla classe di base System. Poiché un controller eredita da questa classe di base, un controller eredita numerosi metodi utili gratuitamente (approfondiremo questi metodi in un momento).

## <a name="understanding-controller-actions"></a>Informazioni sulle azioni del Controller

Un controller espone le azioni del controller. Un'azione è un metodo in un controller che viene chiamato quando si immette un particolare URL nella barra degli indirizzi del browser. Si supponga, ad esempio, effettuare una richiesta per l'URL seguente:

`http://localhost/Product/Index/3`

In questo caso, il metodo Index () viene chiamato nella classe ProductController. Il metodo Index () è un esempio di un'azione del controller.

Un'azione del controller deve essere un metodo pubblico di una classe controller. Metodi di Visual Basic.NET, per impostazione predefinita, sono pubblici. Tenere presente che qualsiasi metodo pubblico che si aggiunge a una classe controller viene esposta come un'azione del controller automaticamente (è necessario prestare attenzione su questo poiché un'azione del controller può essere richiamata da qualsiasi utente nell'universo digitando l'URL corretto in una barra degli indirizzi del browser).

Esistono alcuni requisiti aggiuntivi che devono essere soddisfatti da un'azione del controller. Un metodo utilizzato come un'azione del controller non possa essere sottoposti a overload. Inoltre, un'azione del controller non può essere un metodo statico. A parte ciò, è possibile utilizzare qualsiasi metodo come un'azione del controller.

## <a name="understanding-action-results"></a>Informazioni sui risultati dell'azione

Un'azione del controller restituisce un elemento è stato chiamato un *risultato dell'azione*. Risultato di un'azione è ciò che restituisce un'azione del controller in risposta a una richiesta del browser.

Il framework ASP.NET MVC supporta diversi tipi di risultati dell'azione tra cui:

1. ViewResult - rappresenta HTML e markup.
2. EmptyResult - non rappresenta alcun risultato.
3. RedirectResult - rappresenta un reindirizzamento a un nuovo URL.
4. JsonResult - rappresenta un risultato di JavaScript Object Notation che può essere utilizzato in un'applicazione AJAX.
5. JavaScriptResult - rappresenta uno script di JavaScript.
6. ContentResult - rappresenta un risultato di testo.
7. FileContentResult - rappresenta un file scaricabile (con il contenuto binario).
8. FilePathResult - rappresenta un file scaricabile (con un percorso).
9. FileStreamResult - rappresenta un file scaricabile (con un flusso di file).

Tutti questi risultati di azioni ereditano dalla classe ActionResult.

Nella maggior parte dei casi, un'azione del controller restituisce un ViewResult. Ad esempio, l'azione del controller di indice nel listato 2 restituisce un ViewResult.

**Elenco di 2 - Controllers\BookController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

Quando un'operazione restituisce un ViewResult, HTML viene restituito al browser. Il metodo Index () nel listato 2 restituisce una visualizzazione denominata indice nel browser.

Si noti che l'azione Index () nel listato 2 non viene restituito un ViewResult(). Al contrario, viene chiamato il metodo View() della classe di base Controller. In genere, non restituiscono direttamente un risultato dell'azione. Chiamare invece uno dei seguenti metodi della classe di base Controller:

1. Vista - restituisce un risultato dell'azione ViewResult.
2. Reindirizzare - restituisce un risultato dell'azione RedirectResult.
3. RedirectToAction - restituisce un risultato dell'azione RedirectToRouteResult.
4. RedirectToRoute - restituisce un risultato dell'azione RedirectToRouteResult.
5. JSON - restituisce un risultato dell'azione JsonResult.
6. JavaScriptResult - restituisce un JavaScriptResult.
7. Contenuto - restituisce un risultato dell'azione ContentResult.
8. File - restituisce un FileContentResult, FilePathResult o FileStreamResult a seconda dei parametri passati al metodo.

In tal caso, se si desidera restituire una visualizzazione nel browser, chiamare il metodo View(). Se si desidera reindirizzare l'utente dall'azione di un controller per un altro, chiamare il metodo RedirectToAction(). Ad esempio, l'azione Details() listato 3 consente di visualizzare una vista o reindirizza l'utente per l'azione Index () a seconda se il parametro Id è un valore.

**Elenco di 3 - CustomerController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

Il risultato dell'azione ContentResult è speciale. È possibile utilizzare il risultato dell'azione ContentResult per restituire il risultato di un'azione come testo normale. Ad esempio, il metodo Index () nel listato 4 restituisce un messaggio come testo normale e non come HTML.

**Elenco di 4 - Controllers\StatusController.vb**

> StatusController


> MVC


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

Quando viene richiamata l'azione StatusController.Index(), una vista non viene restituita. Al contrario, il testo "Hello World!" non elaborato viene restituito al browser.

Se un'azione del controller restituisce un risultato non risultato di un'azione, ad esempio, una data o un numero intero, quindi il risultato viene inserito in un ContentResult automaticamente. Ad esempio, quando viene richiamata l'azione Index () di WorkController listato 5, la data viene restituita come un ContentResult automaticamente.

**Elenco di 5 - WorkController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

L'azione Index () nel listato 5 restituisce un oggetto DateTime. Il framework di MVC ASP.NET converte l'oggetto DateTime in una stringa e include il valore DateTime in un ContentResult automaticamente. Il browser riceve la data e l'ora come testo normale.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione è stato per un'introduzione ai concetti del controller, le azioni del controller e i risultati dell'azione controller MVC ASP.NET. Nella prima sezione, è stato descritto come aggiungere nuovi controller per un progetto ASP.NET MVC. Successivamente, si è appreso pubblici come metodi di un controller vengono esposti all'universo come azioni del controller. Infine, abbiamo parlato i diversi tipi di risultati dell'azione che possono essere restituiti da un'azione del controller. In particolare, è descritto come restituire un ViewResult RedirectToActionResult e ContentResult da un'azione del controller.

>[!div class="step-by-step"]
[Precedente](creating-a-custom-route-constraint-cs.md)
[Successivo](creating-custom-routes-vb.md)
