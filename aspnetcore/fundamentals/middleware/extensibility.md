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
ms.openlocfilehash: 57ff9db2edbf307f2442443dc14e69b0498f7475
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2018
---
# <a name="factory-based-middleware-activation-in-aspnet-core"></a><span data-ttu-id="abb3e-103">Attivazione del middleware basata su factory in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="abb3e-103">Factory-based middleware activation in ASP.NET Core</span></span>

<span data-ttu-id="abb3e-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="abb3e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="abb3e-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) è un punto di estendibilità per l'attivazione del [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="abb3e-105">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)/[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) is an extensibility point for [middleware](xref:fundamentals/middleware/index) activation.</span></span>

<span data-ttu-id="abb3e-106">I metodi di estensione `UseMiddleware` verificano se il tipo registrato del middleware implementa `IMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="abb3e-106">`UseMiddleware` extension methods check if a middleware's registered type implements `IMiddleware`.</span></span> <span data-ttu-id="abb3e-107">In caso affermativo, viene usata l'istanza di `IMiddlewareFactory` registrata nel contenitore per risolvere l'implementazione `IMiddleware`, invece di usare la logica di attivazione del middleware basata sulle convenzioni.</span><span class="sxs-lookup"><span data-stu-id="abb3e-107">If it does, the `IMiddlewareFactory` instance registered in the container is used to resolve the `IMiddleware` implementation instead of using the convention-based middleware activation logic.</span></span> <span data-ttu-id="abb3e-108">Il middleware è registrato come un servizio con ambito o temporaneo nel contenitore dei servizi dell'app.</span><span class="sxs-lookup"><span data-stu-id="abb3e-108">The middleware is registered as a scoped or transient service in the app's service container.</span></span>

<span data-ttu-id="abb3e-109">I vantaggi sono:</span><span class="sxs-lookup"><span data-stu-id="abb3e-109">Benefits:</span></span>

* <span data-ttu-id="abb3e-110">Attivazione per richiesta (aggiunta di servizi con ambiti)</span><span class="sxs-lookup"><span data-stu-id="abb3e-110">Activation per request (injection of scoped services)</span></span>
* <span data-ttu-id="abb3e-111">Tipizzazione forte del middleware</span><span class="sxs-lookup"><span data-stu-id="abb3e-111">Strong typing of middleware</span></span>

<span data-ttu-id="abb3e-112">`IMiddleware` viene attivato per ogni richiesta, in modo che i servizi con ambiti possano essere inseriti nel costruttore del middleware.</span><span class="sxs-lookup"><span data-stu-id="abb3e-112">`IMiddleware` is activated per request, so scoped services can be injected into the middleware's constructor.</span></span>

<span data-ttu-id="abb3e-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="abb3e-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="abb3e-114">L'app di esempio illustra il middleware attivato da:</span><span class="sxs-lookup"><span data-stu-id="abb3e-114">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="abb3e-115">Convenzione (`ConventionalMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="abb3e-115">Convention (`ConventionalMiddleware`).</span></span> <span data-ttu-id="abb3e-116">Per altre informazioni sull'attivazione del middleware basata su convenzione, vedere l'argomento [Middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="abb3e-116">For more information on conventional middleware activation, see the [Middleware](xref:fundamentals/middleware/index) topic.</span></span>
* <span data-ttu-id="abb3e-117">Un'implementazione di [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) (`IMiddlewareMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="abb3e-117">An [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) implementation (`IMiddlewareMiddleware`).</span></span> <span data-ttu-id="abb3e-118">La [classe MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) predefinita attiva il middleware.</span><span class="sxs-lookup"><span data-stu-id="abb3e-118">The default [MiddlewareFactory class](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory) activates the middleware.</span></span>

<span data-ttu-id="abb3e-119">Le implementazioni del middleware funzionano in modo identico e registrano il valore specificato da un parametro di stringa di query (`key`).</span><span class="sxs-lookup"><span data-stu-id="abb3e-119">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="abb3e-120">I middleware usano un contesto di database inserito, ovvero un servizio con ambito, per registrare il valore di stringa di query in un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="abb3e-120">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

## <a name="imiddleware"></a><span data-ttu-id="abb3e-121">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="abb3e-121">IMiddleware</span></span>

<span data-ttu-id="abb3e-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definisce il middleware per la pipeline di richieste dell'app.</span><span class="sxs-lookup"><span data-stu-id="abb3e-122">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span> <span data-ttu-id="abb3e-123">Il metodo [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) gestisce le richieste e restituisce un oggetto `Task` che rappresenta l'esecuzione del middleware.</span><span class="sxs-lookup"><span data-stu-id="abb3e-123">The [InvokeAsync(HttpContext, RequestDelegate)](/dotnet/api/microsoft.aspnetcore.http.imiddleware.invokeasync#Microsoft_AspNetCore_Http_IMiddleware_InvokeAsync_Microsoft_AspNetCore_Http_HttpContext_Microsoft_AspNetCore_Http_RequestDelegate_) method handles requests and returns a `Task` that represents the execution of the middleware.</span></span>

<span data-ttu-id="abb3e-124">Middleware attivato da convenzione:</span><span class="sxs-lookup"><span data-stu-id="abb3e-124">Middleware activated by convention:</span></span>

[!code-csharp[Main](extensibility/sample/Middleware/ConventionalMiddleware.cs?name=snippet1)]

<span data-ttu-id="abb3e-125">Middleware attivato da `MiddlewareFactory`:</span><span class="sxs-lookup"><span data-stu-id="abb3e-125">Middleware activated by `MiddlewareFactory`:</span></span>

[!code-csharp[Main](extensibility/sample/Middleware/IMiddlewareMiddleware.cs?name=snippet1)]

<span data-ttu-id="abb3e-126">Vengono create estensioni per i middleware:</span><span class="sxs-lookup"><span data-stu-id="abb3e-126">Extensions are created for the middlewares:</span></span>

[!code-csharp[Main](extensibility/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="abb3e-127">Non è possibile passare gli oggetti al middleware attivato da factory con `UseMiddleware`:</span><span class="sxs-lookup"><span data-stu-id="abb3e-127">It isn't possible to pass objects to the factory-activated middleware with `UseMiddleware`:</span></span>

```csharp
public static IApplicationBuilder UseIMiddlewareMiddleware(
    this IApplicationBuilder builder, bool option)
{
    // Passing 'option' as an argument throws a NotSupportedException at runtime.
    return builder.UseMiddleware<IMiddlewareMiddleware>(option);
}
```

<span data-ttu-id="abb3e-128">Il middleware attivato da factory viene aggiunto al contenitore predefinito in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="abb3e-128">The factory-activated middleware is added to the built-in container in *Startup.cs*:</span></span>

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="abb3e-129">Entrambi i middleware vengono registrati nella pipeline di elaborazione della richiesta in `Configure`:</span><span class="sxs-lookup"><span data-stu-id="abb3e-129">Both middlewares are registered in the request processing pipeline in `Configure`:</span></span>

[!code-csharp[Main](extensibility/sample/Startup.cs?name=snippet2&highlight=12-13)]

## <a name="imiddlewarefactory"></a><span data-ttu-id="abb3e-130">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="abb3e-130">IMiddlewareFactory</span></span>

<span data-ttu-id="abb3e-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) specifica metodi per creare middleware.</span><span class="sxs-lookup"><span data-stu-id="abb3e-131">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span> <span data-ttu-id="abb3e-132">L'implementazione del middleware basata su factory viene registrata nel contenitore come un servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="abb3e-132">The middleware factory implementation is registered in the container as a scoped service.</span></span>

<span data-ttu-id="abb3e-133">L'implementazione predefinita di `IMiddlewareFactory`, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), è presente nel pacchetto [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) ([origine riferimento](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span><span class="sxs-lookup"><span data-stu-id="abb3e-133">The default `IMiddlewareFactory` implementation, [MiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.middlewarefactory), is found in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package ([reference source](https://github.com/aspnet/HttpAbstractions/blob/release/2.0/src/Microsoft.AspNetCore.Http/MiddlewareFactory.cs)).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="abb3e-134">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="abb3e-134">Additional resources</span></span>

* [<span data-ttu-id="abb3e-135">Middleware</span><span class="sxs-lookup"><span data-stu-id="abb3e-135">Middleware</span></span>](xref:fundamentals/middleware/index)
