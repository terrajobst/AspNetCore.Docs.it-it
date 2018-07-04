---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Panoramica del Controller ASP.NET MVC (c#) | Microsoft Docs
author: StephenWalther
description: In questa esercitazione, Stephen Walther presenta controller ASP.NET MVC. Descrive come creare nuovi controller e di restituire tipi diversi di res azione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: e1834498d5a0f5d84f650faf32bb6352f2ad3e70
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370087"
---
<a name="aspnet-mvc-controller-overview-c"></a>Panoramica del Controller ASP.NET MVC (c#)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> In questa esercitazione, Stephen Walther presenta controller ASP.NET MVC. Descrive come creare nuovi controller e restituire diversi tipi di risultati dell'azione.


Questa esercitazione illustra l'argomento del controller ASP.NET MVC, le azioni del controller e i risultati dell'azione. Dopo aver completato questa esercitazione, è possibile comprendere come i controller vengono usati per controllare il modo in cui che un visitatore interagisce con un sito Web ASP.NET MVC.

## <a name="understanding-controllers"></a>Informazioni sui controller

Controller MVC sono responsabili per rispondere alle richieste effettuate per un sito Web ASP.NET MVC. Ogni richiesta del browser viene eseguito il mapping a un controller specifico. Si supponga, ad esempio, che si immette l'URL seguente nella barra degli indirizzi del browser:

`http://localhost/Product/Index/3`

In questo caso, viene richiamato un controller denominato ProductController. Il ProductController è responsabile della generazione la risposta alla richiesta del browser. Ad esempio, il controller potrebbe restituire una visualizzazione specifica al browser o il controller potrebbe reindirizzare l'utente a un altro controller.

L'elenco 1 contiene un controller semplice denominato ProductController.

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Come può notare da listato 1, un controller è semplicemente una classe (una classe Visual Basic .NET o c#). Un controller è una classe che deriva dalla classe di base MVC. Poiché un controller eredita da questa classe di base, un controller eredita diversi metodi utili gratuitamente (questi metodi sono illustrati di seguito).

## <a name="understanding-controller-actions"></a>Informazioni sulle azioni del Controller

Un controller espone le azioni del controller. Un'azione è un metodo in un controller che viene chiamato quando si immette un URL specifico nella barra degli indirizzi del browser. Si supponga, ad esempio, si effettua una richiesta per l'URL seguente:

`http://localhost/Product/Index/3`

In questo caso, viene chiamato il metodo Index () sulla classe ProductController. Il metodo Index () è un esempio di un'azione del controller.

Un'azione del controller deve essere un metodo pubblico di una classe controller. I metodi c#, per impostazione predefinita, sono metodi privati. Tenere presente che qualsiasi metodo pubblico che aggiunta a una classe controller viene esposta come un'azione del controller automaticamente (è necessario prestare attenzione su questo perché un'azione del controller può essere richiamata da chiunque nell'universo semplicemente digitando l'URL corretto in una barra degli indirizzi del browser).

Esistono alcuni requisiti aggiuntivi che devono essere soddisfatti da un'azione del controller. Un metodo usato come un'azione del controller non possa essere sottoposti a overload. Inoltre, un'azione del controller non può essere un metodo statico. A parte questo, è possibile usare qualsiasi metodo come un'azione del controller.

## <a name="understanding-action-results"></a>Informazioni sui risultati delle azioni

Un'azione del controller restituisce un valore chiamato un' *risultato dell'azione*. Risultato di un'azione è ciò che restituisce un'azione del controller in risposta a una richiesta del browser.

Il framework ASP.NET MVC supporta diversi tipi di risultati delle azioni inclusi:

1. ViewResult - rappresenta HTML e il markup.
2. EmptyResult - non rappresenta alcun risultato.
3. RedirectResult - rappresenta un reindirizzamento a un nuovo URL.
4. JsonResult - rappresenta un risultato di JavaScript Object Notation che può essere utilizzato in un'applicazione AJAX.
5. JavaScriptResult - rappresenta uno script JavaScript.
6. ContentResult - rappresenta un risultato di testo.
7. FileContentResult - rappresenta un file scaricabile (con il contenuto binario).
8. FilePathResult - rappresenta un file scaricabile (con un percorso).
9. FileStreamResult - rappresenta un file scaricabile (con un flusso di file).

Tutti questi risultati azione ereditano dalla classe ActionResult di base.

Nella maggior parte dei casi, un'azione del controller restituisce l'elemento ViewResult. Ad esempio, l'azione Index del controller nel listato 2 restituisce l'elemento ViewResult.

**Listato 2 - Controllers\BookController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Quando un'azione restituisce l'elemento ViewResult, HTML viene restituito al browser. Il metodo Index () nel listato 2 restituisce una visualizzazione denominata indice al browser.

Si noti che l'azione Index () nel listato 2 non viene restituito un ViewResult(). Al contrario, il metodo View() della classe Controller è chiamato. In genere, non restituiscono direttamente un risultato dell'azione. Al contrario, si chiama uno dei seguenti metodi della classe di base di Controller:

1. Vista - restituisce un risultato dell'azione ViewResult.
2. Reindirizzare - restituisce un risultato dell'azione RedirectResult.
3. RedirectToAction - restituisce un risultato dell'azione RedirectToRouteResult.
4. RedirectToRoute - restituisce un risultato dell'azione RedirectToRouteResult.
5. JSON - restituisce un risultato dell'azione JsonResult.
6. JavaScriptResult - restituisce un JavaScriptResult.
7. Content: restituisce un risultato dell'azione ContentResult.
8. File - restituisce un FileContentResult, FilePathResult o FileStreamResult a seconda dei parametri passati al metodo.

Pertanto, se si desidera restituire una visualizzazione nel browser, chiamare il metodo View(). Se si desidera reindirizzare l'utente dall'azione di un controller a un'altra, chiamare il metodo RedirectToAction(). Ad esempio, l'azione Details() nel listato 3 viene mostrata una visualizzazione o reindirizza l'utente per l'azione Index () a seconda che il parametro Id abbia un valore.

**Listato 3 - CustomerController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

Il risultato dell'azione ContentResult è speciale. È possibile usare il risultato dell'azione ContentResult per restituire un risultato dell'azione come testo normale. Ad esempio, il metodo Index () nel listato 4 restituisce un messaggio come testo normale e non come HTML.

**Listato 4 - Controllers\StatusController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

Quando viene richiamata l'azione StatusController.Index(), una vista non viene restituita. Al contrario, il testo "Hello World!" non elaborato viene restituito al browser.

Se un'azione del controller restituisce un risultato che è non un risultato dell'azione, ad esempio, una data o un numero intero, quindi il risultato viene inserito in un ContentResult automaticamente. Ad esempio, quando viene richiamata l'azione Index () di WorkController nel listato 5, la data viene restituita come un ContentResult automaticamente.

**Listato 5 - WorkController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

L'azione Index () nel listato 5 restituisce un oggetto DateTime. Il framework ASP.NET MVC Converte l'oggetto DateTime in una stringa ed esegue il wrapping del valore DateTime in un ContentResult automaticamente. Il browser riceve la data e ora come testo normale.

## <a name="summary"></a>Riepilogo

Lo scopo di questa esercitazione è stato per un'introduzione ai concetti del controller, azioni del controller e i risultati dell'azione controller ASP.NET MVC. Nella prima sezione, è stato descritto come aggiungere nuovi controller per un progetto ASP.NET MVC. Successivamente, si è appreso come pubblici metodi di un controller vengono esposti di Universe (universo) come le azioni del controller. Infine, abbiamo parlato di diversi tipi di risultati delle azioni che possono essere restituiti da un'azione del controller. In particolare, viene illustrato come restituire un elemento ViewResult, RedirectToActionResult e ContentResult da un'azione del controller.

> [!div class="step-by-step"]
> [Precedente](creating-an-action-vb.md)
> [Successivo](creating-custom-routes-cs.md)
