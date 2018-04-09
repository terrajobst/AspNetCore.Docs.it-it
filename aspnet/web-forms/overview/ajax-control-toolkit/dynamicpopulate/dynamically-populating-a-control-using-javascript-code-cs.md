---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Compilazione dinamica di un controllo utilizzando il codice JavaScript (c#) | Documenti Microsoft
author: wenz
description: Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: c9fd11ea0348eb7fe9a7634f7b26031339146828
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a><span data-ttu-id="f0bd4-103">Compilazione dinamica di un controllo utilizzando il codice JavaScript (c#)</span><span class="sxs-lookup"><span data-stu-id="f0bd4-103">Dynamically Populating a Control Using JavaScript Code (C#)</span></span>
====================
<span data-ttu-id="f0bd4-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f0bd4-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f0bd4-105">[Scaricare codice](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f0bd4-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span></span>

> <span data-ttu-id="f0bd4-106">Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e riempie il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="f0bd4-107">È inoltre possibile attivare la popolazione utilizzando codice JavaScript sul lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="f0bd4-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f0bd4-108">Overview</span></span>

<span data-ttu-id="f0bd4-109">Il `DynamicPopulate` controllo ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e riempie il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="f0bd4-110">È inoltre possibile attivare la popolazione utilizzando codice JavaScript sul lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="f0bd4-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="f0bd4-111">Steps</span></span>

<span data-ttu-id="f0bd4-112">Prima di tutto, è necessario un servizio Web ASP.NET che implementa il metodo da chiamare `DynamicPopulateExtender` controllo.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="f0bd4-113">Il servizio web implementa il metodo `getDate()` che prevede un argomento di tipo string, denominato `contextKey`, poiché il `DynamicPopulate` controllo invia una parte delle informazioni di contesto con ogni chiamata al servizio web.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="f0bd4-114">Ecco il codice (file `DynamicPopulate.cs.asmx`) che consente di recuperare la data corrente in uno dei tre formati:</span><span class="sxs-lookup"><span data-stu-id="f0bd4-114">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

<span data-ttu-id="f0bd4-115">Nel passaggio successivo, creare un nuovo sito ASP.NET e iniziare con il controllo ScriptManager di AJAX ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="f0bd4-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

<span data-ttu-id="f0bd4-116">Quindi, aggiungere un controllo etichetta (ad esempio mediante il controllo HTML con lo stesso nome, o `<asp:Label />` controllo web) che in un secondo momento visualizzerà il risultato della chiamata al servizio web.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

<span data-ttu-id="f0bd4-117">Successivamente, includere un `DynamicPopulateExtender` controllano e forniscono informazioni sui servizi web, controllo di destinazione, ma non il nome del controllo che attiva la popolazione questa operazione verrà eseguita in un secondo momento utilizzando JavaScript personalizzato.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

<span data-ttu-id="f0bd4-118">Ora per la parte di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-118">Now to the JavaScript part.</span></span> <span data-ttu-id="f0bd4-119">Il `$find()` funzione, definito dalla libreria ASP.NET AJAX, restituisce un riferimento a oggetti lato server di ASP.NET AJAX Control Toolkit, ad esempio `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="f0bd4-120">Nel file corrente, `$find("dpe")` restituisce un riferimento a quella `DynamicPopulateExtender` controllo nella pagina.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="f0bd4-121">Espone un metodo denominato `populate()` che attiva il processo di compilazione dinamica.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="f0bd4-122">Il `populate()` metodo richiede un argomento: la chiave di contesto che verrà utilizzata come argomento per il `getDate()` metodo web.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="f0bd4-123">Quindi, ad esempio, `$find("dpe").populate("format1")` popolerebbe il file etichetta con la data corrente nel formato mese-giorno-anno.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="f0bd4-124">Per semplificare l'esempio più flessibile, l'utente può scegliere tra diversi formati di Data.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="f0bd4-125">Per ognuna di esse, viene visualizzato un pulsante di opzione.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="f0bd4-126">Una volta l'utente fa clic su un pulsante di opzione, il codice JavaScript compila in modo dinamico l'etichetta con il formato della data selezionata.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="f0bd4-127">Ecco i pulsanti di opzione:</span><span class="sxs-lookup"><span data-stu-id="f0bd4-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

<span data-ttu-id="f0bd4-128">Si noti che nel contesto di un pulsante di opzione, l'espressione JavaScript `this.value` fa riferimento al valore del pulsante corrente, che possono essere esattamente le stesse informazioni il `getDate()` metodo utilizzare.</span><span class="sxs-lookup"><span data-stu-id="f0bd4-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="f0bd4-129">[![Un clic sul pulsante Recupera la data dal server, nel formato specificato](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f0bd4-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="f0bd4-130">Un clic sul pulsante Recupera la data dal server, nel formato specificato ([fare clic per visualizzare l'immagine ingrandita](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f0bd4-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f0bd4-131">[Precedente](dynamically-populating-a-control-cs.md)
> [Successivo](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f0bd4-131">[Previous](dynamically-populating-a-control-cs.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span></span>
