---
uid: web-api/overview/error-handling/exception-handling
title: Gestione delle eccezioni in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 2db00c1ab241e0dad051c2a3bcd3b0e4bbf4d5c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807661"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="1acc3-102">Gestione delle eccezioni in API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1acc3-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="1acc3-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1acc3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1acc3-104">Questo articolo descrive l'errore e gestione delle eccezioni in API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1acc3-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="1acc3-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="1acc3-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="1acc3-106">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="1acc3-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="1acc3-107">La registrazione di filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="1acc3-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="1acc3-108">Errore HTTP</span><span class="sxs-lookup"><span data-stu-id="1acc3-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="1acc3-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="1acc3-109">HttpResponseException</span></span>

<span data-ttu-id="1acc3-110">Cosa accade se un controller Web API genera un'eccezione non rilevata.</span><span class="sxs-lookup"><span data-stu-id="1acc3-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="1acc3-111">Per impostazione predefinita, la maggior parte delle eccezioni vengono convertite in una risposta HTTP con codice di stato 500, errore interno del Server.</span><span class="sxs-lookup"><span data-stu-id="1acc3-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="1acc3-112">Il **HttpResponseException** tipo è un caso speciale.</span><span class="sxs-lookup"><span data-stu-id="1acc3-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="1acc3-113">Questa eccezione restituisce qualsiasi codice di stato HTTP specificato nel costruttore di eccezione.</span><span class="sxs-lookup"><span data-stu-id="1acc3-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="1acc3-114">Ad esempio, il metodo seguente restituisce 404, non viene trovato, se il *id* parametro non è valido.</span><span class="sxs-lookup"><span data-stu-id="1acc3-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="1acc3-115">Per un maggiore controllo sulla risposta, è possibile costruire l'intero messaggio di risposta e includerla con il **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="1acc3-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="1acc3-116">Filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="1acc3-116">Exception Filters</span></span>

<span data-ttu-id="1acc3-117">È possibile personalizzare la modalità di gestione eccezioni API Web mediante la scrittura di un *filtro eccezioni*.</span><span class="sxs-lookup"><span data-stu-id="1acc3-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="1acc3-118">Un filtro eccezioni viene eseguito quando un metodo del controller genera un'eccezione non gestita viene *non* un' **HttpResponseException** eccezione.</span><span class="sxs-lookup"><span data-stu-id="1acc3-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="1acc3-119">Il **HttpResponseException** tipo è un caso speciale, poiché è progettato specificamente per restituire una risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="1acc3-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="1acc3-120">I filtri eccezioni implementano il **System.Web.Http.Filters.IExceptionFilter** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="1acc3-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="1acc3-121">Il modo più semplice per scrivere un filtro eccezioni è derivare dal **System.Web.Http.Filters.ExceptionFilterAttribute** classe ed eseguire l'override di **OnException** (metodo).</span><span class="sxs-lookup"><span data-stu-id="1acc3-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="1acc3-122">I filtri eccezioni in API Web ASP.NET sono simili a quelle di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1acc3-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="1acc3-123">Tuttavia, vengono dichiarati in un spazio dei nomi separato e funzione separatamente.</span><span class="sxs-lookup"><span data-stu-id="1acc3-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="1acc3-124">In particolare, il **HandleErrorAttribute** classe usata nel MVC non gestisce le eccezioni generate da controller API Web.</span><span class="sxs-lookup"><span data-stu-id="1acc3-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="1acc3-125">Ecco un filtro che converte **NotImplementedException** eccezioni nel codice di stato HTTP 501, non è implementato del codice:</span><span class="sxs-lookup"><span data-stu-id="1acc3-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="1acc3-126">Il **risposta** proprietà delle **HttpActionExecutedContext** oggetto contiene il messaggio di risposta HTTP che verrà inviato al client.</span><span class="sxs-lookup"><span data-stu-id="1acc3-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="1acc3-127">La registrazione di filtri eccezioni</span><span class="sxs-lookup"><span data-stu-id="1acc3-127">Registering Exception Filters</span></span>

<span data-ttu-id="1acc3-128">Esistono diversi modi per registrare un filtro eccezioni API Web:</span><span class="sxs-lookup"><span data-stu-id="1acc3-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="1acc3-129">Mediante un'azione</span><span class="sxs-lookup"><span data-stu-id="1acc3-129">By action</span></span>
- <span data-ttu-id="1acc3-130">Dal controller</span><span class="sxs-lookup"><span data-stu-id="1acc3-130">By controller</span></span>
- <span data-ttu-id="1acc3-131">A livello globale</span><span class="sxs-lookup"><span data-stu-id="1acc3-131">Globally</span></span>

<span data-ttu-id="1acc3-132">Per applicare il filtro a un'azione specifica, aggiungere il filtro come attributo per l'azione:</span><span class="sxs-lookup"><span data-stu-id="1acc3-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="1acc3-133">Per applicare il filtro a tutte le azioni in un controller, aggiungere il filtro come attributo alla classe controller:</span><span class="sxs-lookup"><span data-stu-id="1acc3-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="1acc3-134">Per applicare il filtro a livello globale in tutti i controller API Web, aggiungere un'istanza del filtro per il **GlobalConfiguration.Configuration.Filters** raccolta.</span><span class="sxs-lookup"><span data-stu-id="1acc3-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="1acc3-135">Eccezione filtri in questa raccolta si applicano a qualsiasi azione del controller API Web.</span><span class="sxs-lookup"><span data-stu-id="1acc3-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="1acc3-136">Se si usa il modello di progetto "Applicazione Web di ASP.NET MVC 4" per creare il progetto, inserire il codice di configurazione Web API all'interno di `WebApiConfig` (classe), che si trova nell'App\_cartella di avvio:</span><span class="sxs-lookup"><span data-stu-id="1acc3-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="1acc3-137">Errore HTTP</span><span class="sxs-lookup"><span data-stu-id="1acc3-137">HttpError</span></span>

<span data-ttu-id="1acc3-138">Il **HttpError** oggetto fornisce un modo coerente per restituire informazioni sull'errore nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="1acc3-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="1acc3-139">Nell'esempio seguente viene illustrato come restituire il codice di stato HTTP 404 (non trovato) con un **HttpError** nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="1acc3-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="1acc3-140">**CreateErrorResponse** è un metodo di estensione definito nel **System.Net.Http.HttpRequestMessageExtensions** classe.</span><span class="sxs-lookup"><span data-stu-id="1acc3-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="1acc3-141">Internamente, **CreateErrorResponse** consente di creare un **HttpError** dell'istanza e quindi crea un' **HttpResponseMessage** che contiene il **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="1acc3-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="1acc3-142">In questo esempio, se il metodo ha esito positivo, restituisce il prodotto nella risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="1acc3-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="1acc3-143">Ma se il prodotto richiesto non viene trovato, la risposta HTTP contiene un **HttpError** nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="1acc3-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="1acc3-144">La risposta potrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="1acc3-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="1acc3-145">Si noti che il **HttpError** è stato serializzato in JSON in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="1acc3-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="1acc3-146">Un vantaggio dell'utilizzo **HttpError** è che passano attraverso lo stesso [la negoziazione del contenuto](../formats-and-model-binding/content-negotiation.md) e processo di serializzazione come qualsiasi altro modello fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="1acc3-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="1acc3-147">Errore HTTP e la convalida del modello</span><span class="sxs-lookup"><span data-stu-id="1acc3-147">HttpError and Model Validation</span></span>

<span data-ttu-id="1acc3-148">Per la convalida del modello, è possibile passare lo stato del modello per **CreateErrorResponse**per includere gli errori di convalida nella risposta:</span><span class="sxs-lookup"><span data-stu-id="1acc3-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="1acc3-149">In questo esempio potrebbe restituire la risposta seguente:</span><span class="sxs-lookup"><span data-stu-id="1acc3-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="1acc3-150">Per altre informazioni sulla convalida del modello, vedere [convalida del modello in API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1acc3-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="1acc3-151">Uso di errore HTTP con HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="1acc3-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="1acc3-152">Gli esempi precedenti restituiscono un **HttpResponseMessage** messaggio dall'azione del controller, ma è anche possibile usare **HttpResponseException** per restituire un' **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="1acc3-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="1acc3-153">Ciò consente di restituire un modello fortemente tipizzato in caso di esito positivo normale, pur restituendo **HttpError** se si è verificato un errore:</span><span class="sxs-lookup"><span data-stu-id="1acc3-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
