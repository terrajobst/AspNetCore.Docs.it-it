---
title: Scrivere middleware di ASP.NET Core personalizzato
author: rick-anderson
description: Informazioni su come scrivere middleware di ASP.NET Core personalizzato.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2019
uid: fundamentals/middleware/write
ms.openlocfilehash: 2c5577394a10370d92c8a83f9d806b63f3245c8b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889166"
---
# <a name="write-custom-aspnet-core-middleware"></a><span data-ttu-id="f4879-103">Scrivere middleware di ASP.NET Core personalizzato</span><span class="sxs-lookup"><span data-stu-id="f4879-103">Write custom ASP.NET Core middleware</span></span>

<span data-ttu-id="f4879-104">[Rick Anderson](https://twitter.com/RickAndMSFT) e [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f4879-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f4879-105">Il middleware è un software che viene assemblato in una pipeline dell'app per gestire richieste e risposte.</span><span class="sxs-lookup"><span data-stu-id="f4879-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="f4879-106">ASP.NET Core offre un ampio set di componenti middleware integrati, ma in alcuni scenari si potrebbe voler scrivere un middleware personalizzato.</span><span class="sxs-lookup"><span data-stu-id="f4879-106">ASP.NET Core provides a rich set of built-in middleware components, but in some scenarios you might want to write a custom middleware.</span></span>

## <a name="middleware-class"></a><span data-ttu-id="f4879-107">Classe middleware</span><span class="sxs-lookup"><span data-stu-id="f4879-107">Middleware class</span></span>

<span data-ttu-id="f4879-108">Il middleware è in genere incapsulato in una classe ed esposto con un metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="f4879-108">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="f4879-109">Si consideri il middleware seguente, che specifica le impostazioni cultura per la richiesta corrente da una stringa di query:</span><span class="sxs-lookup"><span data-stu-id="f4879-109">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="f4879-110">Il codice di esempio precedente viene usato per illustrare la creazione di un componente middleware.</span><span class="sxs-lookup"><span data-stu-id="f4879-110">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="f4879-111">Per il supporto di localizzazione incorporato di ASP.NET Core, vedere <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="f4879-111">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="f4879-112">È possibile testare il middleware passando le impostazioni cultura.</span><span class="sxs-lookup"><span data-stu-id="f4879-112">You can test the middleware by passing in the culture.</span></span> <span data-ttu-id="f4879-113">Ad esempio `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="f4879-113">For example, `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="f4879-114">Il codice seguente sposta il delegato middleware in una classe:</span><span class="sxs-lookup"><span data-stu-id="f4879-114">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f4879-115">Il nome del metodo `Task` del middleware deve essere `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="f4879-115">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="f4879-116">In ASP.NET Core 2.0 o versioni successive il nome può essere `Invoke` o `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="f4879-116">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

## <a name="middleware-extension-method"></a><span data-ttu-id="f4879-117">Metodo di estensione del middleware</span><span class="sxs-lookup"><span data-stu-id="f4879-117">Middleware extension method</span></span>

<span data-ttu-id="f4879-118">Il metodo di estensione seguente espone il middleware tramite <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="f4879-118">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="f4879-119">Il codice seguente chiama il middleware da `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="f4879-119">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="f4879-120">Il middleware deve seguire il [principio delle dipendenze esplicite](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) esponendo le dipendenze nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="f4879-120">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="f4879-121">Il middleware viene costruito una volta per ogni *durata applicazione*.</span><span class="sxs-lookup"><span data-stu-id="f4879-121">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="f4879-122">Se è necessario condividere servizi con il middleware all'interno di una richiesta, vedere la sezione [Dipendenze per richiesta](#per-request-dependencies).</span><span class="sxs-lookup"><span data-stu-id="f4879-122">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="f4879-123">I componenti middleware possono risolvere le dipendenze dall'[inserimento di dipendenze](xref:fundamentals/dependency-injection) mediante i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="f4879-123">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="f4879-124">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) può anche accettare direttamente parametri aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="f4879-124">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

## <a name="per-request-dependencies"></a><span data-ttu-id="f4879-125">Dipendenze per richiesta</span><span class="sxs-lookup"><span data-stu-id="f4879-125">Per-request dependencies</span></span>

<span data-ttu-id="f4879-126">Poiché il middleware viene creato all'avvio dell'app e non per richiesta, i servizi di durata *con ambito* usati dai costruttori del middleware non vengono condivisi con altri tipi di inserimento di dipendenze durante ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="f4879-126">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="f4879-127">Se è necessario condividere un servizio *con ambito* tra il proprio middleware e altri tipi, aggiungere i servizi alla firma del metodo `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="f4879-127">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="f4879-128">Il metodo `Invoke` può accettare parametri aggiuntivi popolati dall'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="f4879-128">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="f4879-129">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f4879-129">Additional resources</span></span>

* <xref:fundamentals/middleware/index>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
