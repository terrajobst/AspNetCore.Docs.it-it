---
title: Gestire gli errori nelle API Web di ASP.NET Core
author: pranavkm
description: Informazioni sulla gestione degli errori con ASP.NET Core le API Web.
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/18/2019
uid: web-api/handle-errors
ms.openlocfilehash: bf8229d37ef15c42b5d7bb4a12737ac65d7b753c
ms.sourcegitcommit: a11f09c10ef3d4eeab7ae9ce993e7f30427741c1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150908"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="fa416-103">Gestire gli errori nelle API Web di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa416-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="fa416-104">Questo articolo descrive come gestire e personalizzare la gestione degli errori con ASP.NET Core le API Web.</span><span class="sxs-lookup"><span data-stu-id="fa416-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="fa416-105">Pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="fa416-105">Developer Exception Page</span></span>

<span data-ttu-id="fa416-106">La [pagina di eccezione Developer](xref:fundamentals/error-handling) è uno strumento utile per ottenere analisi dettagliate dello stack per gli errori del server.</span><span class="sxs-lookup"><span data-stu-id="fa416-106">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fa416-107">La pagina delle eccezioni per sviluppatori Visualizza una risposta di testo normale se il client non accetta output formattato in formato HTML:</span><span class="sxs-lookup"><span data-stu-id="fa416-107">The Developer Exception Page displays a plain-text response if the client doesn't accept HTML-formatted output:</span></span>

```
> curl https://localhost:5001/weatherforecast
System.ArgumentException: count
   at errorhandling.Controllers.WeatherForecastController.Get(Int32 x) in D:\work\Samples\samples\aspnetcore\mvc\errorhandling\Controllers\WeatherForecastController.cs:line 35
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
...
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="fa416-108">Abilitare la pagina delle eccezioni per gli sviluppatori **solo quando l'app è in esecuzione nell'ambiente di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="fa416-108">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="fa416-109">per non condividere pubblicamente le informazioni dettagliate sulle eccezioni quando l'app è in esecuzione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="fa416-109">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="fa416-110">Per altre informazioni sulla configurazione di ambienti, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="fa416-110">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="fa416-111">Gestore di eccezioni</span><span class="sxs-lookup"><span data-stu-id="fa416-111">Exception handler</span></span>

<span data-ttu-id="fa416-112">Negli ambienti non di sviluppo, è possibile usare il [middleware di gestione delle eccezioni](xref:fundamentals/error-handling) per produrre un payload degli errori:</span><span class="sxs-lookup"><span data-stu-id="fa416-112">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="fa416-113">In `Startup.Configure` richiamare<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> per usare il middleware:</span><span class="sxs-lookup"><span data-stu-id="fa416-113">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> to use the middleware:</span></span>

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

1. <span data-ttu-id="fa416-114">Configurare un'azione del `/error` controller per rispondere alla route:</span><span class="sxs-lookup"><span data-stu-id="fa416-114">Configure a controller action to respond to the `/error` route:</span></span>

    ```csharp
    [ApiController]
    public class ErrorController : ControllerBase
    {
        [Route("/error")]
        public IActionResult Error() => Problem();
    }
    ```

<span data-ttu-id="fa416-115">L'azione `Error` precedente invia al client un payload conforme a [RFC7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="fa416-115">The preceding `Error` action sends an [RFC7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="fa416-116">Il middleware di gestione delle eccezioni può inoltre fornire un output più dettagliato del contenuto negoziato nell'ambiente di sviluppo locale.</span><span class="sxs-lookup"><span data-stu-id="fa416-116">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="fa416-117">Usare la procedura seguente per produrre un formato di payload coerente negli ambienti di sviluppo e di produzione:</span><span class="sxs-lookup"><span data-stu-id="fa416-117">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="fa416-118">In `Startup.Configure`registrare le istanze del middleware di gestione delle eccezioni specifiche dell'ambiente:</span><span class="sxs-lookup"><span data-stu-id="fa416-118">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

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

    <span data-ttu-id="fa416-119">Nel codice precedente il middleware è registrato con:</span><span class="sxs-lookup"><span data-stu-id="fa416-119">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="fa416-120">Route di nell' `/error-local-development` ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="fa416-120">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="fa416-121">Una route di `/error` in ambienti che non sono in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="fa416-121">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="fa416-122">Applicare il routing degli attributi alle azioni del controller:</span><span class="sxs-lookup"><span data-stu-id="fa416-122">Apply attribute routing to controller actions:</span></span>

    ```csharp
    [ApiController]
    public class ErrorController : ControllerBase
    {
        [Route("/error-local-development")]
        public IActionResult ErrorLocalDevelopment(
            [FromServices] IWebHostEnvironment webHostEnvironment)
        {
            if (!webHostEnvironment.IsDevelopment())
            {
                throw new InvalidOperationException(
                    "This shouldn't be invoked in non-development environments.");
            }
    
            var context = HttpContext.Features.Get<IExceptionHandlerFeature>();

            return Problem(
                detail: context.Error.StackTrace,
                title: context.Error.Message);
        }
    
        [Route("/error")]
        public IActionResult Error() => Problem();
    }
    ```

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="fa416-123">Usare le eccezioni per modificare la risposta</span><span class="sxs-lookup"><span data-stu-id="fa416-123">Use exceptions to modify the response</span></span>

<span data-ttu-id="fa416-124">Il contenuto della risposta può essere modificato dall'esterno del controller.</span><span class="sxs-lookup"><span data-stu-id="fa416-124">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="fa416-125">Nell'API Web ASP.NET 4. x, un modo per eseguire questa operazione consiste nell' <xref:System.Web.Http.HttpResponseException> usare il tipo.</span><span class="sxs-lookup"><span data-stu-id="fa416-125">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="fa416-126">ASP.NET Core non include un tipo equivalente.</span><span class="sxs-lookup"><span data-stu-id="fa416-126">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="fa416-127">È possibile `HttpResponseException` aggiungere il supporto per con i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="fa416-127">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="fa416-128">Creare un tipo di eccezione noto denominato `HttpResponseException`:</span><span class="sxs-lookup"><span data-stu-id="fa416-128">Create a well-known exception type named `HttpResponseException`:</span></span>

    ```csharp
    public class HttpResponseException : Exception
    {
        public int Status { get; set; } = 500;
    
        public object Value { get; set; }
    }
    ```

1. <span data-ttu-id="fa416-129">Creare un filtro azione denominato `HttpResponseExceptionFilter`:</span><span class="sxs-lookup"><span data-stu-id="fa416-129">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    ```csharp
    public class HttpResponseExceptionFilter : IActionFilter, IOrderedFilter
    {
        public int Order { get; set; } = int.MaxValue - 10;
    
        public void OnActionExecuting(ActionExecutingContext context) {}
    
        public void OnActionExecuted(ActionExecutedContext context)
        {
            if (context.Exception is HttpResponseException exception)
            {
                context.Result = new ObjectResult(exception.Value)
                {
                    Status = exception.Status,
                };
                context.ExceptionHandled = true;
            }
        }
    }
    ```

1. <span data-ttu-id="fa416-130">In `Startup.ConfigureServices`aggiungere il filtro azioni alla raccolta filters:</span><span class="sxs-lookup"><span data-stu-id="fa416-130">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers(options => 
            options.Filters.Add(new HttpResponseExceptionFilter()));
    }
    ```

## <a name="validation-failure-error-response"></a><span data-ttu-id="fa416-131">Risposta errore di convalida non riuscita</span><span class="sxs-lookup"><span data-stu-id="fa416-131">Validation failure error response</span></span>

<span data-ttu-id="fa416-132">Per i controller API Web, MVC risponde con un <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> tipo di risposta quando la convalida del modello ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="fa416-132">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="fa416-133">MVC usa i risultati di <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> per costruire la risposta di errore per un errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="fa416-133">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="fa416-134">Nell'esempio seguente viene usata la factory per modificare il tipo <xref:Microsoft.AspNetCore.Mvc.SerializableError> di risposta predefinito in: `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="fa416-134">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);

services.Configure<ApiBehaviorOptions>(options =>
{
    options.InvalidModelStateResponseFactory = context =>
    {
        var result = new BadRequestObjectResult(context.ModelState);

        // TODO: add `using using System.Net.Mime;` to resolve MediaTypeNames
        result.ContentTypes.Add(MediaTypeNames.Application.Json);
        result.ContentTypes.Add(MediaTypeNames.Application.Xml);

        return result;
    };
});
```

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="fa416-135">Risposta errore client</span><span class="sxs-lookup"><span data-stu-id="fa416-135">Client error response</span></span>

<span data-ttu-id="fa416-136">Un *risultato di errore* viene definito come risultato con un codice di stato HTTP 400 o superiore.</span><span class="sxs-lookup"><span data-stu-id="fa416-136">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="fa416-137">Per i controller API Web, MVC trasforma un risultato di errore in un risultato con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="fa416-137">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fa416-138">La risposta di errore può essere configurata in uno dei modi seguenti:</span><span class="sxs-lookup"><span data-stu-id="fa416-138">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="fa416-139">Implementare ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="fa416-139">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="fa416-140">Usare ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="fa416-140">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="fa416-141">Implementare ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="fa416-141">Implement ProblemDetailsFactory</span></span>

<span data-ttu-id="fa416-142">MVC utilizza `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` per produrre tutte le istanze <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> di <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>e.</span><span class="sxs-lookup"><span data-stu-id="fa416-142">MVC uses `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="fa416-143">Sono incluse le risposte degli errori del client, le risposte degli errori `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` di <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> convalida e i metodi helper e.</span><span class="sxs-lookup"><span data-stu-id="fa416-143">This includes client error responses, validation failure error responses, and the `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper methods.</span></span>

<span data-ttu-id="fa416-144">Per personalizzare la risposta dettagliata al problema, registrare un'implementazione personalizzata `ProblemDetailsFactory` di `Startup.ConfigureServices`in:</span><span class="sxs-lookup"><span data-stu-id="fa416-144">To customize the problem details response, register a custom implementation of `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="fa416-145">La risposta di errore può essere configurata come descritto nella sezione [use ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) .</span><span class="sxs-lookup"><span data-stu-id="fa416-145">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="fa416-146">Usare ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="fa416-146">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="fa416-147">Usare la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> per configurare il contenuto della risposta `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="fa416-147">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="fa416-148">Ad esempio, il codice seguente aggiorna la proprietà `type` per le risposte 404:</span><span class="sxs-lookup"><span data-stu-id="fa416-148">For example, the following code updates the `type` property for 404 responses:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
