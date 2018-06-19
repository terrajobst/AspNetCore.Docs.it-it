---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Creazione di Unit test per le applicazioni MVC ASP.NET (VB) | Documenti Microsoft
author: StephenWalther
description: Informazioni su come creare unit test per le azioni del controller. In questa esercitazione, Stephen Walther viene illustrato come verificare se un'azione del controller restituisce un parti...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 299665f45d72fee33f92344ed53c87dfb1a76d60
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869673"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Creazione di Unit test per le applicazioni MVC ASP.NET (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

[Scarica il PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Informazioni su come creare unit test per le azioni del controller. In questa esercitazione, Stephen Walther viene illustrato come verificare se un'azione del controller restituisce una visualizzazione particolare, restituisce un set specifico di dati oppure un altro tipo di risultato dell'azione.


L'obiettivo di questa esercitazione è dimostrare come è possibile scrivere unit test per i controller in ASP.NET MVC applicazioni. È descritto come creare tre tipi diversi di unit test. È illustrato come testare la vista restituita da un'azione del controller, come i dati della visualizzazione restituito da un'azione del controller di test e come testare o meno un'azione del controller si viene reindirizzati a una seconda azione del controller.

## <a name="creating-the-controller-under-test"></a>Creazione di Controller sottoposta a Test

Iniziamo creando il controller che si desidera testare. Il controller, denominato il `ProductController`, è contenuto in elenco 1.

**Elenco 1: `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

Il `ProductController` contiene due metodi di azione denominati `Index()` e `Details()`. Entrambi i metodi di azione restituiscono una visualizzazione. Si noti che il `Details()` azione accetta un parametro denominato ID.

## <a name="testing-the-view-returned-by-a-controller"></a>Test della visualizzazione restituito da un Controller

Si supponga che si desidera testare o meno il `ProductController` restituisce della vista destra. Si desidera assicurarsi che quando il `ProductController.Details()` azione viene richiamata, viene restituita la visualizzazione dei dettagli. La classe di test nel listato 2 contiene uno unit test per la visualizzazione restituita da test di `ProductController.Details()` azione.

**Elenco 2: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

La classe nel listato 2 include un metodo di test denominato `TestDetailsView()`. Questo metodo contiene tre righe di codice. La prima riga di codice crea una nuova istanza di `ProductController` classe. La seconda riga di codice richiama il controller `Details()` metodo di azione. Infine, l'ultima riga di codice controlli o meno la visualizzazione restituita dal `Details()` azione è la visualizzazione dei dettagli.

Il `ViewResult.ViewName` proprietà rappresenta il nome della visualizzazione restituito da un controller. Un avviso di grandi dimensioni sul test di questa proprietà. Esistono due modi che un controller può restituire una visualizzazione. Un controller può restituire in modo esplicito una visualizzazione simile al seguente:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

In alternativa, il nome della visualizzazione può essere dedotto dal nome dell'azione del controller simile al seguente:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Questa azione del controller restituisce inoltre una visualizzazione denominata `Details`. Tuttavia, il nome della visualizzazione viene dedotto dal nome dell'azione. Se si desidera verificare il nome della vista, è necessario restituire in modo esplicito il nome della visualizzazione dall'azione del controller.

È possibile eseguire lo unit test nel listato 2 immettendo la combinazione di tasti **Ctrl-R, A** o facendo clic di **eseguire tutti i test nella soluzione** pulsante (vedere Figura 1). Se il test viene superato, verrà visualizzato nella finestra Risultati Test nella figura 2.


[![Eseguire i test nella soluzione](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Figura 01**: eseguire tutti i test nella soluzione ([fare clic per visualizzare l'immagine ingrandita](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))


[![Operazione completata](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Figura 02**: esito positivo. ([Fare clic per visualizzare l'immagine ingrandita](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))


## <a name="testing-the-view-data-returned-by-a-controller"></a>Verifica dei dati della visualizzazione restituito da un Controller

Controller MVC passa i dati in una vista utilizzando uno strumento denominato *`View Data`*. Ad esempio, si supponga che si desidera visualizzare i dettagli per un determinato prodotto quando si richiama il `ProductController Details()` azione. In tal caso, è possibile creare un'istanza di un `Product` classe (definito nel modello) e passare l'istanza di `Details` visualizzazione sfruttando `View Data`.

Modificato `ProductController` listato 3 include una versione aggiornata `Details()` azione che restituisce un prodotto.

**Elenco di 3: `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

Prima di tutto, il `Details()` azione viene creata una nuova istanza di `Product` classe che rappresenta un computer portatile. Successivamente, l'istanza del `Product` classe viene passata come secondo parametro per il `View()` metodo.

È possibile scrivere unit test per verificare se i dati previsti sono contenuto nella visualizzazione dati. Lo unit test in test listato 4 o meno un prodotto che rappresenta un computer portatile viene restituito quando si chiama il `ProductController Details()` metodo di azione.

**Elenco di 4: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

Listato 4 il `TestDetailsView()` metodo verifica i dati della visualizzazione restituito richiamando il `Details()` metodo. Il `ViewData` esposto come proprietà nel `ViewResult` restituito richiamando il `Details()` metodo. Il `ViewData.Model` proprietà contiene il prodotto passato alla visualizzazione. Il test di verifica semplicemente che il prodotto contenuto nei dati di visualizzazione con il nome di computer portatile.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Verifica il risultato dell'azione restituito da un Controller

Un'azione del controller più complessa può restituire diversi tipi di risultati dell'azione in base ai valori dei parametri passati all'azione del controller. Un'azione del controller può restituire un'ampia gamma di tipi di risultati dell'azione tra una `ViewResult`, `RedirectToRouteResult`, o `JsonResult`.

Ad esempio, l'elemento modificato `Details()` azione listato 5 restituisce il `Details` visualizzare quando si passa un Id di prodotto valido per l'azione. Se si passa un prodotto valido Id - Id con un valore minore di - 1, quindi si viene reindirizzati al `Index()` azione.

**Elenco 5: `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

È possibile testare il comportamento del `Details()` azione con lo unit test nel listato 6. Lo unit test nel listato 6 verifica che si verrà reindirizzati al `Index` visualizzare quando viene passato un Id con il valore -1 per il `Details()` metodo.

**Elenco 6: `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Quando si chiama il `RedirectToAction()` azione del controller di metodo in un'azione del controller, restituisce un `RedirectToRouteResult`. I controlli di test se il `RedirectToRouteResult` reindirizzerà l'utente a un'azione del controller denominata `Index`.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato descritto come creare unit test per le azioni del controller MVC. In primo luogo, è stato descritto come verificare se la vista destra verrà restituita da un'azione del controller. Si è appreso come utilizzare il `ViewResult.ViewName` proprietà per verificare il nome di una vista.

Successivamente, viene esaminato come è possibile testare il contenuto di `View Data`. È stato descritto come controllare se il prodotto di destra è stato restituito `View Data` dopo la chiamata a un'azione del controller.

Infine, è illustrato come è possibile verificare se vengono restituiti tipi diversi di risultati dell'azione da un'azione del controller. È stato descritto come verificare se un controller restituisce un `ViewResult` o `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Precedente](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
