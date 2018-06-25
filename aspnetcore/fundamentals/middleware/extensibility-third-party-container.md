---
title: Attivazione del middleware con un contenitore di terze parti in ASP.NET Core
author: guardrex
description: Informazioni su come usare un middleware fortemente tipizzato con un'attivazione basata su factory e un contenitore di terze parti in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/02/2018
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: 81d52aafd4e4d964aaec1c5fe61e585b023ff915
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279506"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a><span data-ttu-id="af142-103">Attivazione del middleware con un contenitore di terze parti in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af142-103">Middleware activation with a third-party container in ASP.NET Core</span></span>

<span data-ttu-id="af142-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="af142-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="af142-105">In questo articolo viene illustrato come utilizzare [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) e [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) come punto di estendibilità per l'attivazione del [middleware](xref:fundamentals/middleware/index) con un contenitore di terze parti.</span><span class="sxs-lookup"><span data-stu-id="af142-105">This article demonstrates how to use [IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) and [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="af142-106">Per informazioni introduttive su `IMiddlewareFactory` e `IMiddleware`, vedere l'argomento [Attivazione del middleware basata su factory](xref:fundamentals/middleware/extensibility).</span><span class="sxs-lookup"><span data-stu-id="af142-106">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see the [Factory-based middleware activation](xref:fundamentals/middleware/extensibility) topic.</span></span>

<span data-ttu-id="af142-107">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="af142-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="af142-108">L'app di esempio illustra l'attivazione del middleware da parte di un'implementazione `IMiddlewareFactory`, `SimpleInjectorMiddlewareFactory`.</span><span class="sxs-lookup"><span data-stu-id="af142-108">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="af142-109">Nell'esempio viene utilizzato il contenitore di inserimento delle dipendenze [Simple Injector](https://simpleinjector.org).</span><span class="sxs-lookup"><span data-stu-id="af142-109">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="af142-110">L'implementazione del middleware nell'esempio registra il valore fornito da un parametro stringa di query (`key`).</span><span class="sxs-lookup"><span data-stu-id="af142-110">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="af142-111">Il middleware usa un contesto di database inserito, ovvero un servizio con ambito, per registrare il valore della stringa di query in un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="af142-111">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="af142-112">Nell'app di esempio viene utilizzato [Simple Injector](https://github.com/simpleinjector/SimpleInjector) esclusivamente a scopi dimostrativi.</span><span class="sxs-lookup"><span data-stu-id="af142-112">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="af142-113">L'utilizzo di Simple Injector non ne implica la sponsorizzazione.</span><span class="sxs-lookup"><span data-stu-id="af142-113">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="af142-114">Le modalità di attivazione del middleware descritte nella documentazione di Simple Injector e i problemi in GitHub sono consigliati dai gestori di Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="af142-114">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="af142-115">Per ulteriori informazioni, vedere la [documentazione di Simple Injector](https://simpleinjector.readthedocs.io/en/latest/index.html) e il [repository GitHub su Simple Injector](https://github.com/simpleinjector/SimpleInjector).</span><span class="sxs-lookup"><span data-stu-id="af142-115">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="af142-116">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="af142-116">IMiddlewareFactory</span></span>

<span data-ttu-id="af142-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) specifica metodi per creare middleware.</span><span class="sxs-lookup"><span data-stu-id="af142-117">[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) provides methods to create middleware.</span></span>

<span data-ttu-id="af142-118">Nell'app di esempio viene implementato il middleware basato su factory per creare un'istanza di `SimpleInjectorActivatedMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="af142-118">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="af142-119">Il middleware basato su factory usa il contenitore Simple Injector per risolvere il middleware:</span><span class="sxs-lookup"><span data-stu-id="af142-119">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="af142-120">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="af142-120">IMiddleware</span></span>

<span data-ttu-id="af142-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) definisce il middleware per la pipeline di richieste dell'app.</span><span class="sxs-lookup"><span data-stu-id="af142-121">[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="af142-122">Middleware attivato da un'implementazione `IMiddlewareFactory` (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span><span class="sxs-lookup"><span data-stu-id="af142-122">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="af142-123">Per il middleware viene creata un'estensione (*Middleware/MiddlewareExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="af142-123">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="af142-124">`Startup.ConfigureServices` deve eseguire diverse attività:</span><span class="sxs-lookup"><span data-stu-id="af142-124">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="af142-125">Impostare il contenitore Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="af142-125">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="af142-126">Registrare la factory e il middleware.</span><span class="sxs-lookup"><span data-stu-id="af142-126">Register the factory and middleware.</span></span>
* <span data-ttu-id="af142-127">Rendere disponibile il contesto di database dell'app dal contenitore di Simple Injector per una pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="af142-127">Make the app's database context available from the Simple Injector container for a Razor Page.</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="af142-128">Il middleware è registrato nella pipeline di elaborazione della richiesta in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="af142-128">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a><span data-ttu-id="af142-129">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="af142-129">Additional resources</span></span>

* [<span data-ttu-id="af142-130">Middleware</span><span class="sxs-lookup"><span data-stu-id="af142-130">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="af142-131">Attivazione del middleware basata sulla factory</span><span class="sxs-lookup"><span data-stu-id="af142-131">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="af142-132">Repository di GitHub su Simple Injector</span><span class="sxs-lookup"><span data-stu-id="af142-132">Simple Injector GitHub repository</span></span>](https://github.com/simpleinjector/SimpleInjector)
* [<span data-ttu-id="af142-133">Documentazione di Simple Injector</span><span class="sxs-lookup"><span data-stu-id="af142-133">Simple Injector documentation</span></span>](https://simpleinjector.readthedocs.io/en/latest/index.html)
