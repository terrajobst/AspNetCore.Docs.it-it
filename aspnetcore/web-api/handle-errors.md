---
title: Gestire gli errori nelle API Web di ASP.NET Core
author: pranavkm
description: Informazioni sulla gestione degli errori con ASP.NET Core le API Web.
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/25/2019
uid: web-api/handle-errors
ms.openlocfilehash: 9c5dd2f89e7351f386d1f0633c831952dc58e568
ms.sourcegitcommit: 994da92edb0abf856b1655c18880028b15a28897
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2019
ms.locfileid: "71278724"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="8cf42-103">Gestire gli errori nelle API Web di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8cf42-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="8cf42-104">Questo articolo descrive come gestire e personalizzare la gestione degli errori con ASP.NET Core le API Web.</span><span class="sxs-lookup"><span data-stu-id="8cf42-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

<span data-ttu-id="8cf42-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([Come scaricare](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8cf42-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([How to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="8cf42-106">Pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="8cf42-106">Developer Exception Page</span></span>

<span data-ttu-id="8cf42-107">La [pagina di eccezione Developer](xref:fundamentals/error-handling) è uno strumento utile per ottenere analisi dettagliate dello stack per gli errori del server.</span><span class="sxs-lookup"><span data-stu-id="8cf42-107">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span>

<span data-ttu-id="8cf42-108">La pagina delle eccezioni per sviluppatori Visualizza una risposta di testo normale se il client non accetta output formattato in HTML.</span><span class="sxs-lookup"><span data-stu-id="8cf42-108">The Developer Exception Page displays a plain-text response if the client doesn't accept HTML-formatted output.</span></span> <span data-ttu-id="8cf42-109">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="8cf42-109">For example:</span></span>

```
> curl https://localhost:5001/weatherforecast
System.ArgumentException: count
   at errorhandling.Controllers.WeatherForecastController.Get(Int32 x) in D:\work\Samples\samples\aspnetcore\mvc\errorhandling\Controllers\WeatherForecastController.cs:line 35
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
...
```

> [!WARNING]
> <span data-ttu-id="8cf42-110">Abilitare la pagina delle eccezioni per gli sviluppatori **solo quando l'app è in esecuzione nell'ambiente di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="8cf42-110">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="8cf42-111">per non condividere pubblicamente le informazioni dettagliate sulle eccezioni quando l'app è in esecuzione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="8cf42-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="8cf42-112">Per altre informazioni sulla configurazione di ambienti, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="8cf42-112">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="8cf42-113">Gestore di eccezioni</span><span class="sxs-lookup"><span data-stu-id="8cf42-113">Exception handler</span></span>

<span data-ttu-id="8cf42-114">Negli ambienti non di sviluppo, è possibile usare il [middleware di gestione delle eccezioni](xref:fundamentals/error-handling) per produrre un payload degli errori:</span><span class="sxs-lookup"><span data-stu-id="8cf42-114">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="8cf42-115">In `Startup.Configure` richiamare<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> per usare il middleware:</span><span class="sxs-lookup"><span data-stu-id="8cf42-115">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> to use the middleware:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

1. <span data-ttu-id="8cf42-116">Configurare un'azione del `/error` controller per rispondere alla route:</span><span class="sxs-lookup"><span data-stu-id="8cf42-116">Configure a controller action to respond to the `/error` route:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

<span data-ttu-id="8cf42-117">L'azione `Error` precedente invia al client un payload conforme a [RFC7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="8cf42-117">The preceding `Error` action sends an [RFC7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="8cf42-118">Il middleware di gestione delle eccezioni può inoltre fornire un output più dettagliato del contenuto negoziato nell'ambiente di sviluppo locale.</span><span class="sxs-lookup"><span data-stu-id="8cf42-118">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="8cf42-119">Usare la procedura seguente per produrre un formato di payload coerente negli ambienti di sviluppo e di produzione:</span><span class="sxs-lookup"><span data-stu-id="8cf42-119">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="8cf42-120">In `Startup.Configure`registrare le istanze del middleware di gestione delle eccezioni specifiche dell'ambiente:</span><span class="sxs-lookup"><span data-stu-id="8cf42-120">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    <span data-ttu-id="8cf42-121">Nel codice precedente il middleware è registrato con:</span><span class="sxs-lookup"><span data-stu-id="8cf42-121">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="8cf42-122">Route di nell' `/error-local-development` ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="8cf42-122">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="8cf42-123">Una route di `/error` in ambienti che non sono in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="8cf42-123">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="8cf42-124">Applicare il routing degli attributi alle azioni del controller:</span><span class="sxs-lookup"><span data-stu-id="8cf42-124">Apply attribute routing to controller actions:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="8cf42-125">Usare le eccezioni per modificare la risposta</span><span class="sxs-lookup"><span data-stu-id="8cf42-125">Use exceptions to modify the response</span></span>

<span data-ttu-id="8cf42-126">Il contenuto della risposta può essere modificato dall'esterno del controller.</span><span class="sxs-lookup"><span data-stu-id="8cf42-126">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="8cf42-127">Nell'API Web ASP.NET 4. x, un modo per eseguire questa operazione consiste nell' <xref:System.Web.Http.HttpResponseException> usare il tipo.</span><span class="sxs-lookup"><span data-stu-id="8cf42-127">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="8cf42-128">ASP.NET Core non include un tipo equivalente.</span><span class="sxs-lookup"><span data-stu-id="8cf42-128">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="8cf42-129">È possibile `HttpResponseException` aggiungere il supporto per con i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8cf42-129">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="8cf42-130">Creare un tipo di eccezione noto denominato `HttpResponseException`:</span><span class="sxs-lookup"><span data-stu-id="8cf42-130">Create a well-known exception type named `HttpResponseException`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Exceptions/HttpResponseException.cs?name=snippet_HttpResponseException)]

1. <span data-ttu-id="8cf42-131">Creare un filtro azione denominato `HttpResponseExceptionFilter`:</span><span class="sxs-lookup"><span data-stu-id="8cf42-131">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Filters/HttpResponseExceptionFilter.cs?name=snippet_HttpResponseExceptionFilter)]

1. <span data-ttu-id="8cf42-132">In `Startup.ConfigureServices`aggiungere il filtro azioni alla raccolta filters:</span><span class="sxs-lookup"><span data-stu-id="8cf42-132">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.2"
    
    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.1"

    [!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

## <a name="validation-failure-error-response"></a><span data-ttu-id="8cf42-133">Risposta errore di convalida non riuscita</span><span class="sxs-lookup"><span data-stu-id="8cf42-133">Validation failure error response</span></span>

<span data-ttu-id="8cf42-134">Per i controller API Web, MVC risponde con un <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> tipo di risposta quando la convalida del modello ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="8cf42-134">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="8cf42-135">MVC usa i risultati di <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> per costruire la risposta di errore per un errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="8cf42-135">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="8cf42-136">Nell'esempio seguente viene usata la factory per modificare il tipo <xref:Microsoft.AspNetCore.Mvc.SerializableError> di risposta predefinito in: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="8cf42-136">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=6-15)]

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="8cf42-137">Risposta errore client</span><span class="sxs-lookup"><span data-stu-id="8cf42-137">Client error response</span></span>

<span data-ttu-id="8cf42-138">Un *risultato di errore* viene definito come risultato con un codice di stato HTTP 400 o superiore.</span><span class="sxs-lookup"><span data-stu-id="8cf42-138">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="8cf42-139">Per i controller API Web, MVC trasforma un risultato di errore in un risultato con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="8cf42-139">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8cf42-140">La risposta di errore può essere configurata in uno dei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8cf42-140">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="8cf42-141">Implementare ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="8cf42-141">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="8cf42-142">Usare ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="8cf42-142">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="8cf42-143">Implementare ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="8cf42-143">Implement ProblemDetailsFactory</span></span>

<span data-ttu-id="8cf42-144">MVC utilizza `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` per produrre tutte le istanze <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> di <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>e.</span><span class="sxs-lookup"><span data-stu-id="8cf42-144">MVC uses `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="8cf42-145">Sono incluse le risposte degli errori del client, le risposte degli errori `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` di <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> convalida e i metodi helper e.</span><span class="sxs-lookup"><span data-stu-id="8cf42-145">This includes client error responses, validation failure error responses, and the `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper methods.</span></span>

<span data-ttu-id="8cf42-146">Per personalizzare la risposta dettagliata al problema, registrare un'implementazione personalizzata `ProblemDetailsFactory` di `Startup.ConfigureServices`in:</span><span class="sxs-lookup"><span data-stu-id="8cf42-146">To customize the problem details response, register a custom implementation of `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="8cf42-147">La risposta di errore può essere configurata come descritto nella sezione [use ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) .</span><span class="sxs-lookup"><span data-stu-id="8cf42-147">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="8cf42-148">Usare ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="8cf42-148">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="8cf42-149">Usare la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> per configurare il contenuto della risposta `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="8cf42-149">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="8cf42-150">Ad esempio, il codice seguente in `Startup.ConfigureServices` aggiorna la `type` proprietà per le risposte 404:</span><span class="sxs-lookup"><span data-stu-id="8cf42-150">For example, the following code in `Startup.ConfigureServices` updates the `type` property for 404 responses:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
