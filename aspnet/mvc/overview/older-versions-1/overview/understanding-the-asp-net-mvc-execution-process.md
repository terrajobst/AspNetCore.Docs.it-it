---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Informazioni sul processo di esecuzione di ASP.NET MVC | Documenti Microsoft
author: microsoft
description: Informazioni su come framework di MVC ASP.NET elabora una richiesta del browser dettagliata.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26500730"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="b6175-103">Informazioni sul processo di esecuzione di ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b6175-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="b6175-104">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b6175-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="b6175-105">Informazioni su come framework di MVC ASP.NET elabora una richiesta del browser dettagliata.</span><span class="sxs-lookup"><span data-stu-id="b6175-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="b6175-106">Le richieste a un'applicazione Web basata su MVC ASP.NET passano innanzitutto attraverso il **UrlRoutingModule** oggetto, ovvero un modulo HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6175-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="b6175-107">Questo modulo analizza la richiesta ed esegue la selezione delle route.</span><span class="sxs-lookup"><span data-stu-id="b6175-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="b6175-108">Il **UrlRoutingModule** oggetto seleziona il primo oggetto route che corrisponde alla richiesta corrente.</span><span class="sxs-lookup"><span data-stu-id="b6175-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="b6175-109">(Un oggetto route è una classe che implementa **RouteBase**, e in genere è un'istanza di **Route** classe.) Se non viene indirizzato corrisponda, il **UrlRoutingModule** oggetto non ha alcun effetto e consente di eseguire il fallback alla richiesta di ASP.NET o IIS regolare l'elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="b6175-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="b6175-110">Dall'oggetto selezionato **Route** oggetto, il **UrlRoutingModule** oggetto Ottiene il **IRouteHandler** oggetto a cui è associato il **Route**oggetto.</span><span class="sxs-lookup"><span data-stu-id="b6175-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="b6175-111">In genere, in un'applicazione MVC, questo sarà un'istanza di **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="b6175-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="b6175-112">Il **IRouteHandler** istanza crea un **IHttpHandler** dell'oggetto e lo passa il **IHttpContext** oggetto.</span><span class="sxs-lookup"><span data-stu-id="b6175-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="b6175-113">Per impostazione predefinita, il **IHttpHandler** istanza per MVC è il **MvcHandler** oggetto.</span><span class="sxs-lookup"><span data-stu-id="b6175-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="b6175-114">Il **MvcHandler** oggetto seleziona quindi il controller che gestirà infine la richiesta.</span><span class="sxs-lookup"><span data-stu-id="b6175-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="b6175-115">Quando un'applicazione Web MVC ASP.NET viene eseguito in IIS 7.0, estensione non è necessaria per i progetti MVC.</span><span class="sxs-lookup"><span data-stu-id="b6175-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="b6175-116">Tuttavia, in IIS 6.0, il gestore richiede il mapping di estensione del nome di file MVC alla DLL ISAPI ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b6175-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="b6175-117">Il modulo e il gestore sono i punti di ingresso per il framework di MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b6175-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="b6175-118">Eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="b6175-118">They perform the following actions:</span></span>

- <span data-ttu-id="b6175-119">Selezionare il controller appropriato in un'applicazione Web MVC.</span><span class="sxs-lookup"><span data-stu-id="b6175-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="b6175-120">Ottenere un'istanza di un controller specifico.</span><span class="sxs-lookup"><span data-stu-id="b6175-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="b6175-121">Chiamare il controller **Execute** metodo.</span><span class="sxs-lookup"><span data-stu-id="b6175-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="b6175-122">Di seguito sono elencate le fasi di esecuzione per un progetto Web MVC:</span><span class="sxs-lookup"><span data-stu-id="b6175-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="b6175-123">Ricezione della prima richiesta per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="b6175-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="b6175-124">Nel file Global. asax, **Route** gli oggetti vengono aggiunti per il **RouteTable** oggetto.</span><span class="sxs-lookup"><span data-stu-id="b6175-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="b6175-125">Eseguire il routing</span><span class="sxs-lookup"><span data-stu-id="b6175-125">Perform routing</span></span> 

    - <span data-ttu-id="b6175-126">Il **UrlRoutingModule** modulo Usa la prima corrispondenza **Route** oggetto di **RouteTable** insieme per creare il **RouteData** oggetto che viene quindi utilizzato per creare un **RequestContext** (**IHttpContext**) oggetto.</span><span class="sxs-lookup"><span data-stu-id="b6175-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="b6175-127">Creare il gestore di richieste MVC</span><span class="sxs-lookup"><span data-stu-id="b6175-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="b6175-128">Il **MvcRouteHandler** oggetto crea un'istanza di **MvcHandler** classe e la passa il **RequestContext** istanza.</span><span class="sxs-lookup"><span data-stu-id="b6175-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="b6175-129">Creare controller</span><span class="sxs-lookup"><span data-stu-id="b6175-129">Create controller</span></span> 

    - <span data-ttu-id="b6175-130">Il **MvcHandler** viene utilizzata da object il **RequestContext** istanza per identificare il **IControllerFactory** oggetto (in genere un'istanza di  **DefaultControllerFactory** classe) per creare l'istanza del controller.</span><span class="sxs-lookup"><span data-stu-id="b6175-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="b6175-131">Esegui controller - il **MvcHandler** istanza chiama controller s **Execute** metodo.</span><span class="sxs-lookup"><span data-stu-id="b6175-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="b6175-132">Richiama l'azione</span><span class="sxs-lookup"><span data-stu-id="b6175-132">Invoke action</span></span> 

    - <span data-ttu-id="b6175-133">La maggior parte dei controller di ereditare il **Controller** classe di base.</span><span class="sxs-lookup"><span data-stu-id="b6175-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="b6175-134">Per i controller che a tale scopo, il **ControllerActionInvoker** oggetto associato con il controller determina quale metodo di azione della classe controller da chiamare, quindi chiama tale metodo.</span><span class="sxs-lookup"><span data-stu-id="b6175-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="b6175-135">Esecuzione del risultato</span><span class="sxs-lookup"><span data-stu-id="b6175-135">Execute result</span></span> 

    - <span data-ttu-id="b6175-136">Un metodo di azione tipica potrebbe ricevere l'input dell'utente, preparare i dati di risposta appropriato e quindi eseguire il risultato restituendo un tipo di risultato.</span><span class="sxs-lookup"><span data-stu-id="b6175-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="b6175-137">Tipi di risultato incorporati che è possono eseguire includono: **ViewResult** (che esegue il rendering di una vista ed è il tipo di risultato utilizzato più frequentemente), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, e **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="b6175-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
