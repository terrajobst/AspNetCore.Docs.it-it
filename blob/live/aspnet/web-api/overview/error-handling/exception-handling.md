---
uid: web-api/overview/error-handling/exception-handling
title: Gestione delle eccezioni in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="324b8-102">Gestione delle eccezioni in ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="324b8-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="324b8-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="324b8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="324b8-104">Questo articolo descrive l'errore e gestione delle eccezioni nell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="324b8-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="324b8-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="324b8-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="324b8-106">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="324b8-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="324b8-107">Registrazione di filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="324b8-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="324b8-108">Errore HTTP</span><span class="sxs-lookup"><span data-stu-id="324b8-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="324b8-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="324b8-109">HttpResponseException</span></span>

<span data-ttu-id="324b8-110">Cosa accade se un controller API Web genera un'eccezione non rilevata.</span><span class="sxs-lookup"><span data-stu-id="324b8-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="324b8-111">Per impostazione predefinita, la maggior parte delle eccezioni vengono convertite in una risposta HTTP con codice di stato 500 Errore interno del Server.</span><span class="sxs-lookup"><span data-stu-id="324b8-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="324b8-112">Il **HttpResponseException** tipo è un caso speciale.</span><span class="sxs-lookup"><span data-stu-id="324b8-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="324b8-113">Questa eccezione restituisce qualsiasi codice di stato HTTP specificato nel costruttore eccezione.</span><span class="sxs-lookup"><span data-stu-id="324b8-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="324b8-114">Ad esempio, il metodo seguente restituisce 404, non viene trovato, se il *id* parametro non è valido.</span><span class="sxs-lookup"><span data-stu-id="324b8-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="324b8-115">Per un maggiore controllo sulla risposta, è inoltre possibile costruire l'intero messaggio di risposta e includerla con il **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="324b8-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="324b8-116">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="324b8-116">Exception Filters</span></span>

<span data-ttu-id="324b8-117">È possibile personalizzare come API Web gestisce le eccezioni scrivendo un *filtro eccezioni*.</span><span class="sxs-lookup"><span data-stu-id="324b8-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="324b8-118">Un filtro eccezioni viene eseguito quando un metodo del controller genera qualsiasi eccezione non gestita che è *non* un **HttpResponseException** eccezione.</span><span class="sxs-lookup"><span data-stu-id="324b8-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="324b8-119">Il **HttpResponseException** tipo è un caso speciale, poiché è progettato specificamente per la restituzione di una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="324b8-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="324b8-120">Implementano i filtri eccezioni di **System.Web.Http.Filters.IExceptionFilter** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="324b8-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="324b8-121">È il modo più semplice per scrivere un filtro eccezioni da cui derivare il **System.Web.Http.Filters.ExceptionFilterAttribute** classe ed eseguire l'override di **OnException** metodo.</span><span class="sxs-lookup"><span data-stu-id="324b8-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="324b8-122">I filtri eccezioni nell'API Web ASP.NET sono simili a quelle in ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="324b8-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="324b8-123">Tuttavia, vengono dichiarati in un spazio dei nomi separato e una funzione separatamente.</span><span class="sxs-lookup"><span data-stu-id="324b8-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="324b8-124">In particolare, il **HandleErrorAttribute** classe utilizzata in MVC non gestisce le eccezioni generate dai controller API Web.</span><span class="sxs-lookup"><span data-stu-id="324b8-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="324b8-125">Ecco un filtro che converte **NotImplementedException** eccezioni nel codice di stato HTTP 501-, non di codice:</span><span class="sxs-lookup"><span data-stu-id="324b8-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="324b8-126">Il **risposta** proprietà del **HttpActionExecutedContext** oggetto contiene il messaggio di risposta HTTP che verrà inviato al client.</span><span class="sxs-lookup"><span data-stu-id="324b8-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="324b8-127">Registrazione di filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="324b8-127">Registering Exception Filters</span></span>

<span data-ttu-id="324b8-128">Esistono diversi modi per registrare un filtro eccezioni di API Web:</span><span class="sxs-lookup"><span data-stu-id="324b8-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="324b8-129">Da un'azione</span><span class="sxs-lookup"><span data-stu-id="324b8-129">By action</span></span>
- <span data-ttu-id="324b8-130">Dal controller</span><span class="sxs-lookup"><span data-stu-id="324b8-130">By controller</span></span>
- <span data-ttu-id="324b8-131">A livello globale</span><span class="sxs-lookup"><span data-stu-id="324b8-131">Globally</span></span>

<span data-ttu-id="324b8-132">Per applicare il filtro a un'azione specifica, aggiungere il filtro come attributo per l'azione:</span><span class="sxs-lookup"><span data-stu-id="324b8-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="324b8-133">Per applicare il filtro per tutte le azioni in un controller, aggiungere il filtro come attributo per la classe controller:</span><span class="sxs-lookup"><span data-stu-id="324b8-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="324b8-134">Per applicare il filtro a livello globale in tutti i controller API Web, aggiungere un'istanza del filtro per il **GlobalConfiguration.Configuration.Filters** insieme.</span><span class="sxs-lookup"><span data-stu-id="324b8-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="324b8-135">Filtri di eccezione in questa raccolta si applicano a qualsiasi azione del controller API Web.</span><span class="sxs-lookup"><span data-stu-id="324b8-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="324b8-136">Se si utilizza il modello di progetto "Applicazione Web di ASP.NET MVC 4" per creare il progetto, inserire il codice di configurazione Web API all'interno di `WebApiConfig` (classe), che si trova nell'App\_cartella di avvio:</span><span class="sxs-lookup"><span data-stu-id="324b8-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="324b8-137">Errore HTTP</span><span class="sxs-lookup"><span data-stu-id="324b8-137">HttpError</span></span>

<span data-ttu-id="324b8-138">Il **HttpError** oggetto fornisce un sistema coerente per restituire informazioni sugli errori nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="324b8-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="324b8-139">Nell'esempio seguente viene illustrato come restituire il codice di stato HTTP 404 (non trovato) con un **HttpError** nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="324b8-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="324b8-140">**CreateErrorResponse** è un metodo di estensione definito nel **System.Net.Http.HttpRequestMessageExtensions** classe.</span><span class="sxs-lookup"><span data-stu-id="324b8-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="324b8-141">Internamente, **CreateErrorResponse** crea un **HttpError** istanza e quindi crea un **HttpResponseMessage** che contiene il **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="324b8-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="324b8-142">In questo esempio, se il metodo ha esito positivo, restituisce il prodotto nella risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="324b8-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="324b8-143">Ma se il prodotto richiesto non viene trovato, la risposta HTTP contiene un **HttpError** nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="324b8-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="324b8-144">La risposta potrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="324b8-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="324b8-145">Si noti che il **HttpError** è stato serializzato in JSON in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="324b8-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="324b8-146">Un vantaggio dell'utilizzo **HttpError** è che passano attraverso lo stesso [negoziazione di contenuto](../formats-and-model-binding/content-negotiation.md) e processo di serializzazione come qualsiasi altro modello fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="324b8-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="324b8-147">Errore HTTP e la convalida del modello</span><span class="sxs-lookup"><span data-stu-id="324b8-147">HttpError and Model Validation</span></span>

<span data-ttu-id="324b8-148">Per la convalida del modello, è possibile passare lo stato del modello per **CreateErrorResponse**, per includere gli errori di convalida nella risposta:</span><span class="sxs-lookup"><span data-stu-id="324b8-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="324b8-149">In questo esempio potrebbe restituire la risposta seguente:</span><span class="sxs-lookup"><span data-stu-id="324b8-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="324b8-150">Per ulteriori informazioni sulla convalida del modello, vedere [convalida del modello in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="324b8-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="324b8-151">Con errore HTTP HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="324b8-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="324b8-152">Negli esempi precedenti vengono restituiti un **HttpResponseMessage** messaggio dall'azione del controller, ma è anche possibile usare **HttpResponseException** per restituire un **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="324b8-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="324b8-153">Ciò consente di restituire un modello fortemente tipizzato nel caso di esito positivo normale, pur restituendo **HttpError** se si verifica un errore:</span><span class="sxs-lookup"><span data-stu-id="324b8-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
