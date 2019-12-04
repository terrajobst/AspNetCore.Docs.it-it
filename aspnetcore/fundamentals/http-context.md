---
title: Accedere a HttpContext in ASP.NET Core
author: coderandhiker
description: Informazioni su come accedere a HttpContext in ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
uid: fundamentals/httpcontext
ms.openlocfilehash: 8a7ee180380c42ea745c91b8e6a18c1baa820220
ms.sourcegitcommit: 5974e3e66dab3398ecf2324fbb82a9c5636f70de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/03/2019
ms.locfileid: "74778739"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="e35b1-103">Accedere a HttpContext in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e35b1-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="e35b1-104">ASP.NET Core app accedono `HttpContext` tramite l'interfaccia <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> e la <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>di implementazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e35b1-104">ASP.NET Core apps access `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span> <span data-ttu-id="e35b1-105">L'uso di `IHttpContextAccessor` è necessario solo per l'accesso a `HttpContext` all'interno di un servizio.</span><span class="sxs-lookup"><span data-stu-id="e35b1-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="e35b1-106">Usare HttpContext da Razor Pages</span><span class="sxs-lookup"><span data-stu-id="e35b1-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="e35b1-107">Il <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> Razor Pages espone la proprietà <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext>:</span><span class="sxs-lookup"><span data-stu-id="e35b1-107">The Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> exposes the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> property:</span></span>

```csharp
public class AboutModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = HttpContext.Request.PathBase;
    }
}
```

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="e35b1-108">Usare HttpContext da una visualizzazione Razor</span><span class="sxs-lookup"><span data-stu-id="e35b1-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="e35b1-109">Le visualizzazioni Razor espongono `HttpContext` direttamente tramite una proprietà [RazorPage.Context](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context) nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="e35b1-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context) property on the view.</span></span> <span data-ttu-id="e35b1-110">Nell'esempio seguente viene recuperato il nome utente corrente in un'app Intranet utilizzando l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="e35b1-110">The following example retrieves the current username in an intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
    
    ...
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="e35b1-111">Usare HttpContext da un controller</span><span class="sxs-lookup"><span data-stu-id="e35b1-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="e35b1-112">I controller espongono la proprietà [ControllerBase.HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext):</span><span class="sxs-lookup"><span data-stu-id="e35b1-112">Controllers expose the [ControllerBase.HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext) property:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;

        ...

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="e35b1-113">Usare HttpContext dal middleware</span><span class="sxs-lookup"><span data-stu-id="e35b1-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="e35b1-114">Quando si utilizzano componenti middleware personalizzati, `HttpContext` viene passato nel metodo `Invoke` o `InvokeAsync` ed è accessibile quando viene configurato il middleware:</span><span class="sxs-lookup"><span data-stu-id="e35b1-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        ...
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="e35b1-115">Usare HttpContext da componenti personalizzati</span><span class="sxs-lookup"><span data-stu-id="e35b1-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="e35b1-116">Per altri framework e componenti personalizzati che richiedono l'accesso a `HttpContext`, l'approccio consigliato consiste nel registrare una dipendenza tramite il contenitore predefinito di [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e35b1-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e35b1-117">Il contenitore di inserimento delle dipendenze fornisce il `IHttpContextAccessor` a tutte le classi che lo dichiarano come dipendenza nei costruttori:</span><span class="sxs-lookup"><span data-stu-id="e35b1-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddControllersWithViews();
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

<span data-ttu-id="e35b1-118">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e35b1-118">In the following example:</span></span>

* <span data-ttu-id="e35b1-119">`UserRepository` dichiara la dipendenza da `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="e35b1-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="e35b1-120">La dipendenza viene fornita quando l'inserimento delle dipendenze risolve la catena di dipendenze e crea un'istanza di `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="e35b1-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

```csharp
public class UserRepository : IUserRepository
{
    private readonly IHttpContextAccessor _httpContextAccessor;

    public UserRepository(IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
    }

    public void LogCurrentUser()
    {
        var username = _httpContextAccessor.HttpContext.User.Identity.Name;
        service.LogAccessRequest(username);
    }
}
```

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="e35b1-121">Accesso a HttpContext da un thread in background</span><span class="sxs-lookup"><span data-stu-id="e35b1-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="e35b1-122">`HttpContext` non è thread-safe.</span><span class="sxs-lookup"><span data-stu-id="e35b1-122">`HttpContext` isn't thread-safe.</span></span> <span data-ttu-id="e35b1-123">La lettura o scrittura delle proprietà `HttpContext` di fuori dell'elaborazione di una richiesta può provocare un'eccezione <xref:System.NullReferenceException>.</span><span class="sxs-lookup"><span data-stu-id="e35b1-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a <xref:System.NullReferenceException>.</span></span>

> [!NOTE]
> <span data-ttu-id="e35b1-124">Se l'app genera errori sporadici `NullReferenceException`, esaminare le parti del codice che avviano l'elaborazione in background o che continuano l'elaborazione dopo il completamento di una richiesta.</span><span class="sxs-lookup"><span data-stu-id="e35b1-124">If your app generates sporadic `NullReferenceException` errors, review parts of the code that start background processing or that continue processing after a request completes.</span></span> <span data-ttu-id="e35b1-125">Individuare gli errori, ad esempio definendo un metodo del controller come `async void`.</span><span class="sxs-lookup"><span data-stu-id="e35b1-125">Look for mistakes, such as defining a controller method as `async void`.</span></span>

<span data-ttu-id="e35b1-126">Per eseguire in modo sicuro operazioni in background con dati `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="e35b1-126">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="e35b1-127">Copiare i dati necessari durante l'elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="e35b1-127">Copy the required data during request processing.</span></span>
* <span data-ttu-id="e35b1-128">Passare i dati copiati a un'attività in background.</span><span class="sxs-lookup"><span data-stu-id="e35b1-128">Pass the copied data to a background task.</span></span>

<span data-ttu-id="e35b1-129">Per evitare codice non sicuro, non passare mai il `HttpContext` in un metodo che esegua il lavoro in background.</span><span class="sxs-lookup"><span data-stu-id="e35b1-129">To avoid unsafe code, never pass the `HttpContext` into a method that performs background work.</span></span> <span data-ttu-id="e35b1-130">Passare invece i dati necessari.</span><span class="sxs-lookup"><span data-stu-id="e35b1-130">Pass the required data instead.</span></span> <span data-ttu-id="e35b1-131">Nell'esempio seguente viene chiamato `SendEmailCore` per iniziare a inviare un messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="e35b1-131">In the following example, `SendEmailCore` is called to start sending an email.</span></span> <span data-ttu-id="e35b1-132">Il `correlationId` viene passato al `SendEmailCore`, non al `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="e35b1-132">The `correlationId` is passed to `SendEmailCore`, not the `HttpContext`.</span></span> <span data-ttu-id="e35b1-133">L'esecuzione del codice non attende il completamento di `SendEmailCore`:</span><span class="sxs-lookup"><span data-stu-id="e35b1-133">Code execution doesn't wait for `SendEmailCore` to complete:</span></span>

```csharp
public class EmailController : Controller
{
    public IActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        _ = SendEmailCore(correlationId);

        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        ...
    }
}
