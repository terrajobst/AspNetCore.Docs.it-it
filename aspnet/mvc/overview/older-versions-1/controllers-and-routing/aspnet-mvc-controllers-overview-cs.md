---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Panoramica di ASP.NET MVC Controller (c#) | Documenti Microsoft
author: StephenWalther
description: In questa esercitazione, Stephen Walther presenta controller MVC ASP.NET. Informazioni su come creare nuovi controller e restituire tipi diversi di res azione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 95e7c555a52c8c3b765a6fffab15276491cf5714
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-controller-overview-c"></a>Panoramica di ASP.NET MVC Controller (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther presenta controller MVC ASP.NET. Informazioni su come creare nuovi controller e restituire diversi tipi di risultati dell'azione.


In questa esercitazione viene illustrato l'argomento di controller MVC ASP.NET, le azioni del controller e i risultati dell'azione. Dopo aver completato questa esercitazione, è possibile comprendere come i controller vengono utilizzati per controllare il modo in cui che un visitatore interagisce con un sito Web ASP.NET MVC.

## <a name="understanding-controllers"></a>Informazioni sui controller

Controller MVC sono responsabili per rispondere alle richieste effettuate per un sito Web ASP.NET MVC. Ogni richiesta del browser viene eseguito il mapping a un controller specifico. Si supponga, ad esempio, che si immette l'URL seguente nella barra degli indirizzi del browser:

`http://localhost/Product/Index/3`

In questo caso, viene richiamato un controller denominato ProductController. Il ProductController è responsabile della generazione la risposta alla richiesta del browser. Ad esempio, il controller potrebbe restituire una visualizzazione specifica al browser o il controller può reindirizzare l'utente a un altro controller.

Elenco 1 contiene un controller semplice denominato ProductController.

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Come si può vedere dal listato 1, un controller è semplicemente una classe (una classe .NET di Visual Basic o c#). Un controller è una classe che deriva dalla classe di base System. Poiché un controller eredita da questa classe di base, un controller eredita numerosi metodi utili gratuitamente (approfondiremo questi metodi in un momento).

## <a name="understanding-controller-actions"></a>Informazioni sulle azioni del Controller

Un controller espone le azioni del controller. Un'azione è un metodo in un controller che viene chiamato quando si immette un particolare URL nella barra degli indirizzi del browser. Si supponga, ad esempio, effettuare una richiesta per l'URL seguente:

`http://localhost/Product/Index/3`

In questo caso, il metodo Index () viene chiamato nella classe ProductController. Il metodo Index () è un esempio di un'azione del controller.

Un'azione del controller deve essere un metodo pubblico di una classe controller. Metodi di c#, per impostazione predefinita, sono metodi privati. Tenere presente che qualsiasi metodo pubblico che si aggiunge a una classe controller viene esposta come un'azione del controller automaticamente (è necessario prestare attenzione su questo poiché un'azione del controller può essere richiamata da qualsiasi utente nell'universo digitando l'URL corretto in una barra degli indirizzi del browser).

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

**Il listato 2 - Controllers\BookController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

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

**Elenco di 3 - CustomerController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

Il risultato dell'azione ContentResult è speciale. È possibile utilizzare il risultato dell'azione ContentResult per restituire il risultato di un'azione come testo normale. Ad esempio, il metodo Index () nel listato 4 restituisce un messaggio come testo normale e non come HTML.

**Listato 4 - Controllers\StatusController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

Quando viene richiamata l'azione StatusController.Index(), una vista non viene restituita. Al contrario, il testo "Hello World!" non elaborato viene restituito al browser.

Se un'azione del controller restituisce un risultato non risultato di un'azione, ad esempio, una data o un numero intero, quindi il risultato viene inserito in un ContentResult automaticamente. Ad esempio, quando viene richiamata l'azione Index () di WorkController listato 5, la data viene restituita come un ContentResult automaticamente.

**Nel listato 5 - WorkController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

L'azione Index () nel listato 5 restituisce un oggetto DateTime. Il framework di MVC ASP.NET converte l'oggetto DateTime in una stringa e include il valore DateTime in un ContentResult automaticamente. Il browser riceve la data e l'ora come testo normale.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione è stato per un'introduzione ai concetti del controller, le azioni del controller e i risultati dell'azione controller MVC ASP.NET. Nella prima sezione, è stato descritto come aggiungere nuovi controller per un progetto ASP.NET MVC. Successivamente, si è appreso pubblici come metodi di un controller vengono esposti all'universo come azioni del controller. Infine, abbiamo parlato i diversi tipi di risultati dell'azione che possono essere restituiti da un'azione del controller. In particolare, è descritto come restituire un ViewResult RedirectToActionResult e ContentResult da un'azione del controller.

> [!div class="step-by-step"]
> [Precedente](creating-an-action-vb.md)
> [Successivo](creating-custom-routes-cs.md)
