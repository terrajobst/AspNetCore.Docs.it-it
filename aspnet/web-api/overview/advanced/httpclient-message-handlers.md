---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Gestori di messaggi HttpClient nell'API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: db9edcf4fb31e967c3d4e7635f96c68829aec97d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831372"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="c8fb7-102">Gestori di messaggi HttpClient nell'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c8fb7-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="c8fb7-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c8fb7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c8fb7-104">Oggetto *gestore di messaggi* è una classe che riceve una richiesta HTTP e restituisce una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="c8fb7-105">In genere, una serie di gestori di messaggi vengono concatenati.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="c8fb7-106">Il primo gestore riceve una richiesta HTTP, esegue alcune operazioni di elaborazione e offre la richiesta al gestore successivo.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="c8fb7-107">A un certo punto, la risposta viene creata e viene reimpostato fino alla catena.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="c8fb7-108">Questo modello viene chiamato un *delega* gestore.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="c8fb7-109">Sul lato client, il **HttpClient** classe Usa un gestore di messaggi per elaborare le richieste.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="c8fb7-110">Il gestore predefinito è **HttpClientHandler**, che invia la richiesta attraverso la rete e ottiene la risposta dal server.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="c8fb7-111">È possibile inserire i gestori di messaggi personalizzato nella pipeline del client:</span><span class="sxs-lookup"><span data-stu-id="c8fb7-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="c8fb7-112">API Web ASP.NET usa anche i gestori di messaggi sul lato server.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="c8fb7-113">Per altre informazioni, vedere [gestori di messaggi HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="c8fb7-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="c8fb7-114">Gestori di messaggi personalizzato</span><span class="sxs-lookup"><span data-stu-id="c8fb7-114">Custom Message Handlers</span></span>

<span data-ttu-id="c8fb7-115">Per scrivere un gestore di messaggi personalizzato, derivare da **System.Net.Http.DelegatingHandler** ed eseguire l'override di **SendAsync** (metodo).</span><span class="sxs-lookup"><span data-stu-id="c8fb7-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="c8fb7-116">Ecco la firma del metodo:</span><span class="sxs-lookup"><span data-stu-id="c8fb7-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="c8fb7-117">Il metodo accetta un **HttpRequestMessage** come input e in modo asincrono restituisce un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="c8fb7-118">Una tipica implementazione esegue le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8fb7-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="c8fb7-119">Elaborare il messaggio di richiesta.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-119">Process the request message.</span></span>
2. <span data-ttu-id="c8fb7-120">Chiamare `base.SendAsync` per inviare la richiesta per il gestore interno.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="c8fb7-121">Il gestore interno restituisce un messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-121">The inner handler returns a response message.</span></span> <span data-ttu-id="c8fb7-122">(Questo passaggio è asincrono).</span><span class="sxs-lookup"><span data-stu-id="c8fb7-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="c8fb7-123">Elaborare la risposta e restituirlo al chiamante.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="c8fb7-124">Nell'esempio seguente viene illustrato un gestore di messaggi che aggiunge un'intestazione personalizzata per la richiesta in uscita:</span><span class="sxs-lookup"><span data-stu-id="c8fb7-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="c8fb7-125">La chiamata a `base.SendAsync` è asincrona.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="c8fb7-126">Se il gestore esegue qualsiasi attività dopo questa chiamata, usare il **await** parola chiave per riprendere l'esecuzione dopo il completamento del metodo.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="c8fb7-127">Nell'esempio seguente illustra un gestore che registra i codici di errore.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="c8fb7-128">La registrazione se stessa non è molto interessante, ma nell'esempio viene illustrato come ottenere la risposta all'interno del gestore.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="c8fb7-129">Aggiunta di gestori di messaggi alla Pipeline di Client</span><span class="sxs-lookup"><span data-stu-id="c8fb7-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="c8fb7-130">Per aggiungere gestori personalizzati per **HttpClient**, utilizzare il **HttpClientFactory.Create** metodo:</span><span class="sxs-lookup"><span data-stu-id="c8fb7-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="c8fb7-131">Gestori di messaggi vengono chiamati nell'ordine in cui vengono trasmessi nel **Create** (metodo).</span><span class="sxs-lookup"><span data-stu-id="c8fb7-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="c8fb7-132">Perché vengono annidati i gestori eventi, il messaggio di risposta viene trasferita in altra direzione.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="c8fb7-133">L'ultimo gestore, ovvero è il primo per ottenere il messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="c8fb7-133">That is, the last handler is the first to get the response message.</span></span>
