---
title: Accedere a HttpContext in ASP.NET Core
author: coderandhiker
description: Informazioni su come accedere a HttpContext in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: 0bf40f9cd2554f5ba01ccc06001fa4f1940d51a5
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289055"
---
# <a name="access-httpcontext-in-aspnet-core"></a><span data-ttu-id="9b562-103">Accedere a HttpContext in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b562-103">Access HttpContext in ASP.NET Core</span></span>

<span data-ttu-id="9b562-104">Le app ASP.NET Core accedono a `HttpContext` tramite l'interfaccia [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) e la relativa implementazione predefinita di [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span><span class="sxs-lookup"><span data-stu-id="9b562-104">ASP.NET Core apps access the `HttpContext` through the [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) interface and its default implementation [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor).</span></span> <span data-ttu-id="9b562-105">L'uso di `IHttpContextAccessor` è necessario solo per l'accesso a `HttpContext` all'interno di un servizio.</span><span class="sxs-lookup"><span data-stu-id="9b562-105">It's only necessary to use `IHttpContextAccessor` when you need access to the `HttpContext` inside a service.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a><span data-ttu-id="9b562-106">Usare HttpContext da Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9b562-106">Use HttpContext from Razor Pages</span></span>

<span data-ttu-id="9b562-107">[PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) di Razor Pages espone la proprietà [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext):</span><span class="sxs-lookup"><span data-stu-id="9b562-107">The Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) exposes the [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) property:</span></span>

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

::: moniker-end

## <a name="use-httpcontext-from-a-razor-view"></a><span data-ttu-id="9b562-108">Usare HttpContext da una visualizzazione Razor</span><span class="sxs-lookup"><span data-stu-id="9b562-108">Use HttpContext from a Razor view</span></span>

<span data-ttu-id="9b562-109">Le visualizzazioni Razor espongono `HttpContext` direttamente tramite una proprietà [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9b562-109">Razor views expose the `HttpContext` directly via a [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) property on the view.</span></span> <span data-ttu-id="9b562-110">L'esempio seguente recupera il nome utente corrente in un'app di intranet usando l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="9b562-110">The following example retrieves the current username in an Intranet app using Windows Authentication:</span></span>

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a><span data-ttu-id="9b562-111">Usare HttpContext da un controller</span><span class="sxs-lookup"><span data-stu-id="9b562-111">Use HttpContext from a controller</span></span>

<span data-ttu-id="9b562-112">I controller espongono la proprietà [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext):</span><span class="sxs-lookup"><span data-stu-id="9b562-112">Controllers expose the [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) property:</span></span>

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;
        // Do something with the PathBase.

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a><span data-ttu-id="9b562-113">Usare HttpContext dal middleware</span><span class="sxs-lookup"><span data-stu-id="9b562-113">Use HttpContext from middleware</span></span>

<span data-ttu-id="9b562-114">Quando si utilizzano componenti middleware personalizzati, `HttpContext` viene passato nel metodo `Invoke` o `InvokeAsync` ed è accessibile quando viene configurato il middleware:</span><span class="sxs-lookup"><span data-stu-id="9b562-114">When working with custom middleware components, `HttpContext` is passed into the `Invoke` or `InvokeAsync` method and can be accessed when the middleware is configured:</span></span>

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a><span data-ttu-id="9b562-115">Usare HttpContext da componenti personalizzati</span><span class="sxs-lookup"><span data-stu-id="9b562-115">Use HttpContext from custom components</span></span>

<span data-ttu-id="9b562-116">Per altri framework e componenti personalizzati che richiedono l'accesso a `HttpContext`, l'approccio consigliato consiste nel registrare una dipendenza tramite il contenitore predefinito di [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9b562-116">For other framework and custom components that require access to `HttpContext`, the recommended approach is to register a dependency using the built-in [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="9b562-117">Il contenitore di inserimento delle dipendenze fornisce a `IHttpContextAccessor` qualsiasi classe dichiarata come dipendenza nel relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="9b562-117">The dependency injection container supplies the `IHttpContextAccessor` to any classes that declare it as a dependency in their constructors.</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc();
     services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

<span data-ttu-id="9b562-118">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="9b562-118">In the following example:</span></span>

* <span data-ttu-id="9b562-119">`UserRepository` dichiara la dipendenza da `IHttpContextAccessor`.</span><span class="sxs-lookup"><span data-stu-id="9b562-119">`UserRepository` declares its dependency on `IHttpContextAccessor`.</span></span>
* <span data-ttu-id="9b562-120">La dipendenza viene fornita quando l'inserimento delle dipendenze risolve la catena di dipendenze e crea un'istanza di `UserRepository`.</span><span class="sxs-lookup"><span data-stu-id="9b562-120">The dependency is supplied when dependency injection resolves the dependency chain and creates an instance of `UserRepository`.</span></span>

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

## <a name="httpcontext-access-from-a-background-thread"></a><span data-ttu-id="9b562-121">Accesso a HttpContext da un thread in background</span><span class="sxs-lookup"><span data-stu-id="9b562-121">HttpContext access from a background thread</span></span>

<span data-ttu-id="9b562-122">`HttpContext` non è thread-safe.</span><span class="sxs-lookup"><span data-stu-id="9b562-122">`HttpContext` is not thread-safe.</span></span> <span data-ttu-id="9b562-123">La lettura o scrittura delle proprietà `HttpContext` di fuori dell'elaborazione di una richiesta può provocare un'eccezione `NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="9b562-123">Reading or writing properties of the `HttpContext` outside of processing a request can result in a `NullReferenceException`.</span></span>

> [!NOTE]
> <span data-ttu-id="9b562-124">L'uso di `HttpContext` al di fuori dell'elaborazione di una richiesta provoca spesso un'eccezione `NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="9b562-124">Using `HttpContext` outside of processing a request often results in a `NullReferenceException`.</span></span> <span data-ttu-id="9b562-125">Se l'app genera sporadiche eccezioni `NullReferenceException`, esaminare le parti di codice che avviano l'elaborazione in background o che continuano l'elaborazione dopo il completamento di una richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b562-125">If your app generates sporadic `NullReferenceException`s , review parts of the code that start background processing, or that continue processing after a request completes.</span></span> <span data-ttu-id="9b562-126">Individuare gli errori, ad esempio definendo un metodo del controller come `async void`.</span><span class="sxs-lookup"><span data-stu-id="9b562-126">Look for mistakes, such as defining a controller method as `async void`.</span></span>

<span data-ttu-id="9b562-127">Per eseguire in modo sicuro operazioni in background con dati `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="9b562-127">To safely perform background work with `HttpContext` data:</span></span>

* <span data-ttu-id="9b562-128">Copiare i dati necessari durante l'elaborazione della richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b562-128">Copy the required data during request processing.</span></span>
* <span data-ttu-id="9b562-129">Passare i dati copiati a un'attività in background.</span><span class="sxs-lookup"><span data-stu-id="9b562-129">Pass the copied data to a background task.</span></span>

<span data-ttu-id="9b562-130">Per evitare codice non gestito, non passare mai `HttpContext` in un metodo che esegue operazioni in background, bensì passare i dati necessari.</span><span class="sxs-lookup"><span data-stu-id="9b562-130">To avoid unsafe code, never pass the `HttpContext` into a method that does background work - pass the data you need instead.</span></span>

```csharp
public class EmailController : Controller
{
    public IActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        // Starts sending an email, but doesn't wait for it to complete
        _ = SendEmailCore(correlationId);
        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        // send the email
    }
}
