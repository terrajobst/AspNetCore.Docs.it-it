---
title: Attivazione del middleware basata su factory in ASP.NET Core
author: guardrex
description: Informazioni su come usare middleware fortemente tipizzato con un'implementazione di attivazione basata su factory in ASP.NET Core.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/29/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility
ms.openlocfilehash: 76ba257abfb11e0c2950b974f837c6ae5818a6a1
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a>Attivazione del middleware basata su factory in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) è un punto di estendibilità per l'attivazione del [middleware](xref:fundamentals/middleware/index).

I metodi di estensione `UseMiddleware` verificano se il tipo registrato del middleware implementa `IMiddleware`. In caso affermativo, viene usata l'istanza di `IMiddlewareFactory` registrata nel contenitore per risolvere l'implementazione `IMiddleware`, invece di usare la logica di attivazione del middleware basata sulle convenzioni. Il middleware è registrato come un servizio con ambito o temporaneo nel contenitore dei servizi dell'app.

I vantaggi sono:

* Attivazione per richiesta (aggiunta di servizi con ambiti)
* Tipizzazione forte del middleware

`IMiddleware` viene attivato per ogni richiesta, in modo che i servizi con ambiti possano essere inseriti nel costruttore del middleware.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

L'app di esempio illustra il middleware attivato da:

* Convenzione. Per altre informazioni sull'attivazione del middleware basata su convenzione, vedere l'argomento [Middleware](xref:fundamentals/middleware/index).
* Un'implementazione [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware). La [classe MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) predefinita attiva il middleware.

Le implementazioni del middleware funzionano in modo identico e registrano il valore specificato da un parametro di stringa di query (`key`). I middleware usano un contesto di database inserito, ovvero un servizio con ambito, per registrare il valore di stringa di query in un database in memoria.

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definisce il middleware per la pipeline di richieste dell'app. Il metodo [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) gestisce le richieste e restituisce un oggetto `Task` che rappresenta l'esecuzione del middleware.

Middleware attivato da convenzione:

[!code-csharp[](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

Middleware attivato da `MiddlewareFactory`:

[!code-csharp[](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

Vengono create estensioni per i middleware:

[!code-csharp[](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

Non è possibile passare gli oggetti al middleware attivato da factory con `UseMiddleware`:

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

Il middleware attivato da factory viene aggiunto al contenitore predefinito in *Startup.cs*:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

Entrambi i middleware vengono registrati nella pipeline di elaborazione della richiesta in `Configure`:

[!code-csharp[](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) specifica metodi per creare middleware. L'implementazione del middleware basata su factory viene registrata nel contenitore come un servizio con ambito.

L'implementazione predefinita di `IMiddlewareFactory`, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), è presente nel pacchetto [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) ([origine riferimento](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Middleware](xref:fundamentals/middleware/index)
* [Attivazione del middleware con un contenitore di terze parti](xref:fundamentals/middleware/extensibility-third-party-container)
