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
# <a name="handle-errors-in-aspnet-core-web-apis"></a>Gestire gli errori nelle API Web di ASP.NET Core

Questo articolo descrive come gestire e personalizzare la gestione degli errori con ASP.NET Core le API Web.

## <a name="developer-exception-page"></a>Pagina delle eccezioni per gli sviluppatori

La [pagina di eccezione Developer](xref:fundamentals/error-handling) è uno strumento utile per ottenere analisi dettagliate dello stack per gli errori del server.

::: moniker range=">= aspnetcore-3.0"

La pagina delle eccezioni per sviluppatori Visualizza una risposta di testo normale se il client non accetta output formattato in formato HTML:

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
> Abilitare la pagina delle eccezioni per gli sviluppatori **solo quando l'app è in esecuzione nell'ambiente di sviluppo**. per non condividere pubblicamente le informazioni dettagliate sulle eccezioni quando l'app è in esecuzione nell'ambiente di produzione. Per altre informazioni sulla configurazione di ambienti, vedere <xref:fundamentals/environments>.

## <a name="exception-handler"></a>Gestore di eccezioni

Negli ambienti non di sviluppo, è possibile usare il [middleware di gestione delle eccezioni](xref:fundamentals/error-handling) per produrre un payload degli errori:

1. In `Startup.Configure` richiamare<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> per usare il middleware:

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

1. Configurare un'azione del `/error` controller per rispondere alla route:

    ```csharp
    [ApiController]
    public class ErrorController : ControllerBase
    {
        [Route("/error")]
        public IActionResult Error() => Problem();
    }
    ```

L'azione `Error` precedente invia al client un payload conforme a [RFC7807](https://tools.ietf.org/html/rfc7807).

Il middleware di gestione delle eccezioni può inoltre fornire un output più dettagliato del contenuto negoziato nell'ambiente di sviluppo locale. Usare la procedura seguente per produrre un formato di payload coerente negli ambienti di sviluppo e di produzione:

1. In `Startup.Configure`registrare le istanze del middleware di gestione delle eccezioni specifiche dell'ambiente:

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

    Nel codice precedente il middleware è registrato con:

    * Route di nell' `/error-local-development` ambiente di sviluppo.
    * Una route di `/error` in ambienti che non sono in fase di sviluppo.
    
1. Applicare il routing degli attributi alle azioni del controller:

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

## <a name="use-exceptions-to-modify-the-response"></a>Usare le eccezioni per modificare la risposta

Il contenuto della risposta può essere modificato dall'esterno del controller. Nell'API Web ASP.NET 4. x, un modo per eseguire questa operazione consiste nell' <xref:System.Web.Http.HttpResponseException> usare il tipo. ASP.NET Core non include un tipo equivalente. È possibile `HttpResponseException` aggiungere il supporto per con i passaggi seguenti:

1. Creare un tipo di eccezione noto denominato `HttpResponseException`:

    ```csharp
    public class HttpResponseException : Exception
    {
        public int Status { get; set; } = 500;
    
        public object Value { get; set; }
    }
    ```

1. Creare un filtro azione denominato `HttpResponseExceptionFilter`:

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

1. In `Startup.ConfigureServices`aggiungere il filtro azioni alla raccolta filters:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers(options => 
            options.Filters.Add(new HttpResponseExceptionFilter()));
    }
    ```

## <a name="validation-failure-error-response"></a>Risposta errore di convalida non riuscita

Per i controller API Web, MVC risponde con un <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> tipo di risposta quando la convalida del modello ha esito negativo. MVC usa i risultati di <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> per costruire la risposta di errore per un errore di convalida. Nell'esempio seguente viene usata la factory per modificare il tipo <xref:Microsoft.AspNetCore.Mvc.SerializableError> di risposta predefinito in: `Startup.ConfigureServices`

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

## <a name="client-error-response"></a>Risposta errore client

Un *risultato di errore* viene definito come risultato con un codice di stato HTTP 400 o superiore. Per i controller API Web, MVC trasforma un risultato di errore in un risultato con <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.

::: moniker range=">= aspnetcore-3.0"

La risposta di errore può essere configurata in uno dei modi seguenti:

1. [Implementare ProblemDetailsFactory](#implement-problemdetailsfactory)
1. [Usare ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a>Implementare ProblemDetailsFactory

MVC utilizza `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` per produrre tutte le istanze <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> di <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>e. Sono incluse le risposte degli errori del client, le risposte degli errori `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` di <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> convalida e i metodi helper e.

Per personalizzare la risposta dettagliata al problema, registrare un'implementazione personalizzata `ProblemDetailsFactory` di `Startup.ConfigureServices`in:

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

La risposta di errore può essere configurata come descritto nella sezione [use ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) .

::: moniker-end

### <a name="use-apibehavioroptionsclienterrormapping"></a>Usare ApiBehaviorOptions. ClientErrorMapping

Usare la proprietà <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> per configurare il contenuto della risposta `ProblemDetails`. Ad esempio, il codice seguente aggiorna la proprietà `type` per le risposte 404:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
