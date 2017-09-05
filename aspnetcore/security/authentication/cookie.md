---
title: "Utilizzo dell'autenticazione Cookie senza identità ASP.NET Core"
author: rick-anderson
description: Una spiegazione dell'utilizzo dell'autenticazione cookie senza ASP.NET Identity Core
keywords: ASP.NET Core, i cookie
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: 60ac318cb47b5a5b4c651c88e60d43772ce59958
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="ab3bb-104">Utilizzo dell'autenticazione Cookie senza identità ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab3bb-104">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<a name="security-authentication-cookie-middleware"></a>

<span data-ttu-id="ab3bb-105">ASP.NET Core 1. x fornisce il cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) che serializza un'entità utente in un cookie crittografato e nelle richieste successive convalida quindi il cookie, ricrea l'entità e lo assegna al `HttpContext.User` proprietà .</span><span class="sxs-lookup"><span data-stu-id="ab3bb-105">ASP.NET Core 1.x provides cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal, and assigns it to the `HttpContext.User` property.</span></span> <span data-ttu-id="ab3bb-106">Se si desidera fornire schermate di accesso e i database utente, è possibile utilizzare il middleware del cookie come funzionalità autonoma.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-106">If you want to provide your own login screens and user databases, you can use the cookie middleware as a standalone feature.</span></span>

<span data-ttu-id="ab3bb-107">Una modifica importante in ASP.NET Core 2. x è che il middleware del cookie è assente.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-107">A major change in ASP.NET Core 2.x is that the cookie middleware is absent.</span></span> <span data-ttu-id="ab3bb-108">Invece di `UseAuthentication` chiamata al metodo nel `Configure` metodo *Startup.cs* aggiunge AuthenticationMiddleware che imposta il `HttpContext.User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-108">Instead, the `UseAuthentication` method invocation in the `Configure` method of *Startup.cs* adds the AuthenticationMiddleware which sets the `HttpContext.User` property.</span></span>

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a><span data-ttu-id="ab3bb-109">Aggiunta e configurazione</span><span class="sxs-lookup"><span data-stu-id="ab3bb-109">Adding and configuring</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab3bb-110">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ab3bb-111">Completare i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-111">Complete the following steps:</span></span>

- <span data-ttu-id="ab3bb-112">Se non si utilizza il `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), installare la versione 2.0 del `Microsoft.AspNetCore.Authentication.Cookies` pacchetto NuGet nel progetto.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-112">If not using the `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), install version 2.0+ of the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span>

- <span data-ttu-id="ab3bb-113">Richiamare il `UseAuthentication` metodo il `Configure` metodo il *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-113">Invoke the `UseAuthentication` method in the `Configure` method of the *Startup.cs* file:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ab3bb-114">Richiamare il `AddAuthentication` e `AddCookie` metodi il `ConfigureServices` metodo il *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-114">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method of the *Startup.cs* file:</span></span>

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie(options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab3bb-115">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ab3bb-116">Completare i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-116">Complete the following steps:</span></span>

- <span data-ttu-id="ab3bb-117">Installare il `Microsoft.AspNetCore.Authentication.Cookies` pacchetto NuGet nel progetto.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-117">Install the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span> <span data-ttu-id="ab3bb-118">Questo pacchetto contiene middleware del cookie.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-118">This package contains the cookie middleware.</span></span>

- <span data-ttu-id="ab3bb-119">Aggiungere le seguenti righe al `Configure` metodo nel *Startup.cs* file prima di `app.UseMvc()` istruzione:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-119">Add the following lines to the `Configure` method in your *Startup.cs* file before the `app.UseMvc()` statement:</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AccessDeniedPath = "/Account/Forbidden/",
        AuthenticationScheme = "MyCookieAuthenticationScheme",
        AutomaticAuthenticate = true,
        AutomaticChallenge = true,
        LoginPath = "/Account/Unauthorized/"
    });
    ```

---

<span data-ttu-id="ab3bb-120">I frammenti di codice precedenti per configurare alcune o tutte le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-120">The code snippets above configure some or all of the following options:</span></span>

* <span data-ttu-id="ab3bb-121">`AccessDeniedPath`-Si tratta del percorso relativo in cui le richieste di reindirizzamento quando un utente tenta di accedere a una risorsa ma non supera [criteri di autorizzazione](xref:security/authorization/policies#security-authorization-policies-based) per tale risorsa.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-121">`AccessDeniedPath` - This is the relative path to which requests redirect when a user attempts to access a resource but does not pass any [authorization policies](xref:security/authorization/policies#security-authorization-policies-based) for that resource.</span></span>

* <span data-ttu-id="ab3bb-122">`AuthenticationScheme`-Si tratta di un valore con cui è noto uno schema particolare cookie di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-122">`AuthenticationScheme` - This is a value by which a particular cookie authentication scheme is known.</span></span> <span data-ttu-id="ab3bb-123">Ciò è utile quando sono presenti più istanze di autenticazione dei cookie e si desidera [limitare l'autorizzazione a un'istanza](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).</span><span class="sxs-lookup"><span data-stu-id="ab3bb-123">This is useful when there are multiple instances of cookie authentication and you want to [limit authorization to one instance](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).</span></span>

* <span data-ttu-id="ab3bb-124">`AutomaticAuthenticate`-Questo flag è rilevante solo per ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-124">`AutomaticAuthenticate` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="ab3bb-125">Indica che l'autenticazione dei cookie deve eseguire in ogni richiesta e tentare di convalidare e ricostruire qualsiasi entità serializzato che è creato.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-125">It indicates that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

* <span data-ttu-id="ab3bb-126">`AutomaticChallenge`-Questo flag è rilevante solo per ASP.NET Core 1. x.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-126">`AutomaticChallenge` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="ab3bb-127">Indica che l'autenticazione dei cookie 1. x deve reindirizzare il browser per il `LoginPath` o `AccessDeniedPath` quando autorizzazione ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-127">It indicates that the 1.x cookie authentication should redirect the browser to the `LoginPath` or `AccessDeniedPath` when authorization fails.</span></span>

* <span data-ttu-id="ab3bb-128">`LoginPath`-Questo è il percorso relativo in cui le richieste di reindirizzamento quando un utente tenta di accedere a una risorsa ma non è stato autenticato.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-128">`LoginPath` - This is the relative path to which requests redirect when a user attempts to access a resource but has not been authenticated.</span></span>

<span data-ttu-id="ab3bb-129">[Altre opzioni](xref:security/authentication/cookie#security-authentication-cookie-options) includono la possibilità di impostare l'autorità emittente per tutte le attestazioni di autenticazione dei cookie crea, Elimina il nome del cookie di autenticazione, il dominio per il cookie e varie proprietà di sicurezza nel cookie.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-129">[Other options](xref:security/authentication/cookie#security-authentication-cookie-options) include the ability to set the issuer for any claims the cookie authentication creates, the name of the cookie the authentication drops, the domain for the cookie and various security properties on the cookie.</span></span> <span data-ttu-id="ab3bb-130">Per impostazione predefinita, l'autenticazione dei cookie utilizza le opzioni di sicurezza appropriate per i cookie che crea, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-130">By default, the cookie authentication uses appropriate security options for any cookies it creates, such as:</span></span>
- <span data-ttu-id="ab3bb-131">L'impostazione del contrassegno HttpOnly per impedire l'accesso di cookie in JavaScript sul lato client</span><span class="sxs-lookup"><span data-stu-id="ab3bb-131">Setting the HttpOnly flag to prevent cookie access in client-side JavaScript</span></span>
- <span data-ttu-id="ab3bb-132">Limitare il cookie a HTTPS, se una richiesta è stato trasferito su HTTPS</span><span class="sxs-lookup"><span data-stu-id="ab3bb-132">Limiting the cookie to HTTPS if a request has traveled over HTTPS</span></span>

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a><span data-ttu-id="ab3bb-133">Creazione di un cookie di identità</span><span class="sxs-lookup"><span data-stu-id="ab3bb-133">Creating an Identity cookie</span></span>

<span data-ttu-id="ab3bb-134">Per creare un cookie che contiene le informazioni sull'utente, è necessario costruire un [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) che contiene le informazioni che si desidera essere serializzati nel cookie.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-134">To create a cookie holding your user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) holding the information you wish to be serialized in the cookie.</span></span> <span data-ttu-id="ab3bb-135">Dopo aver adatto `ClaimsPrincipal` di oggetto, chiamare il seguente all'interno del metodo del controller:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-135">Once you have a suitable `ClaimsPrincipal` object, call the following inside your controller method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab3bb-136">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-136">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab3bb-137">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

<span data-ttu-id="ab3bb-138">Questo crea un cookie crittografato e lo aggiunge alla risposta corrente.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-138">This creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="ab3bb-139">Il `AuthenticationScheme` specificato durante [configurazione](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) deve essere utilizzato quando si chiama `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-139">The `AuthenticationScheme` specified during [configuration](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) must be used when calling `SignInAsync`.</span></span>

<span data-ttu-id="ab3bb-140">Dietro le quinte, la crittografia utilizzata è ASP.NET Core [la protezione dei dati](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-140">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="ab3bb-141">Se si trovano in più computer, carico bilanciamento del carico o utilizzare una web farm, è quindi necessario [configurare la protezione dati](xref:security/data-protection/configuration/overview#data-protection-configuring) come utilizzare la stessa chiave anello e identificatore dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-141">If you are hosting on multiple machines, load balancing, or using a web farm, then you need to [configure data protection](xref:security/data-protection/configuration/overview#data-protection-configuring) to use the same key ring and application identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="ab3bb-142">La disconnessione</span><span class="sxs-lookup"><span data-stu-id="ab3bb-142">Signing out</span></span>

<span data-ttu-id="ab3bb-143">Per effettuare la disconnessione dell'utente corrente ed eliminare i cookie, chiamare il seguente all'interno del controller:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-143">To sign out the current user and delete their cookie, call the following inside your controller:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab3bb-144">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab3bb-145">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="ab3bb-146">Reazione alle modifiche di back-end</span><span class="sxs-lookup"><span data-stu-id="ab3bb-146">Reacting to back-end changes</span></span>

>[!WARNING]
> <span data-ttu-id="ab3bb-147">Dopo aver creato un cookie principale, diventa l'unica origine di identità.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-147">Once a principal cookie has been created, it becomes the single source of identity.</span></span> <span data-ttu-id="ab3bb-148">Anche se si disabilita un utente nei sistemi back-end, l'autenticazione dei cookie non ha alcuna conoscenza di questo oggetto e un utente rimane connesso, purché i cookie è valido.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-148">Even if you disable a user in your back-end systems, the cookie authentication has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="ab3bb-149">L'autenticazione dei cookie viene fornita una serie di eventi nella relativa classe di opzione.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-149">The cookie authentication provides a series of events in its option class.</span></span> <span data-ttu-id="ab3bb-150">Il `ValidateAsync()` evento consente di intercettare ed eseguire l'override di convalida dell'identità del cookie.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-150">The `ValidateAsync()` event can be used to intercept and override validation of the cookie identity.</span></span>

<span data-ttu-id="ab3bb-151">Prendere in considerazione un database utente di back-end che può contenere una colonna "LastChanged".</span><span class="sxs-lookup"><span data-stu-id="ab3bb-151">Consider a back-end user database that may have a "LastChanged" column.</span></span> <span data-ttu-id="ab3bb-152">Per invalidare un cookie quando viene modificato il database, è necessario innanzitutto, quando [creazione il cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), aggiungere un'attestazione "LastChanged" contenente il valore corrente.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-152">In order to invalidate a cookie when the database changes, you should first, when [creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), add a "LastChanged" claim containing the current value.</span></span> <span data-ttu-id="ab3bb-153">Quando viene modificato il database, è necessario aggiornare il valore "LastChanged".</span><span class="sxs-lookup"><span data-stu-id="ab3bb-153">When the database changes, the "LastChanged" value should be updated.</span></span>

<span data-ttu-id="ab3bb-154">Per implementare un override per il `ValidateAsync()` evento, è necessario scrivere un metodo con la firma seguente:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-154">To implement an override for the `ValidateAsync()` event, you must write a method with the following signature:</span></span>

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

<span data-ttu-id="ab3bb-155">ASP.NET Identity Core implementa questo controllo come parte del relativo `SecurityStampValidator`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-155">ASP.NET Core Identity implements this check as part of its `SecurityStampValidator`.</span></span> <span data-ttu-id="ab3bb-156">Un esempio è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-156">An example looks like the following:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab3bb-157">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

<span data-ttu-id="ab3bb-158">Questo sarebbe quindi necessario collegare durante la registrazione del servizio nel cookie di `ConfigureServices` metodo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-158">This would then be wired up during cookie service registration in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie(options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab3bb-159">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

<span data-ttu-id="ab3bb-160">Questo sarebbe quindi necessario collegare durante la configurazione dell'autenticazione nel cookie di `Configure` metodo *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-160">This would then be wired up during cookie authentication configuration in the `Configure` method of *Startup.cs*:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="ab3bb-161">Si consideri l'esempio in cui è stato aggiornato il proprio nome &mdash; una decisione che non influisce sulla sicurezza in alcun modo.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-161">Consider the example in which their name has been updated &mdash; a decision which doesn't affect security in any way.</span></span> <span data-ttu-id="ab3bb-162">Se si desidera aggiornare distruttivo l'entità utente, è possibile chiamare `context.ReplacePrincipal()` e impostare il `context.ShouldRenew` proprietà `true`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-162">If you want to non-destructively update the user principal, you can call `context.ReplacePrincipal()` and set the `context.ShouldRenew` property to `true`.</span></span>

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a><span data-ttu-id="ab3bb-163">Controllo delle opzioni di cookie</span><span class="sxs-lookup"><span data-stu-id="ab3bb-163">Controlling cookie options</span></span>

<span data-ttu-id="ab3bb-164">Il [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) classe dotata di varie opzioni di configurazione per ottimizzare i cookie viene creati.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-164">The [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) class comes with various configuration options to fine-tune the cookies being created.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab3bb-165">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-165">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ab3bb-166">2. x unifica le API utilizzate per i cookie di configurazione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-166">ASP.NET Core 2.x unifies the APIs used for configuring cookies.</span></span> <span data-ttu-id="ab3bb-167">Il 1. x API sono state contrassegnate come obsolete e un nuovo `Cookie` proprietà di tipo `CookieBuilder` è stato introdotto nel `CookieAuthenticationOptions` classe.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-167">The 1.x APIs have been marked as obsolete, and a new `Cookie` property of type `CookieBuilder` has been introduced in the `CookieAuthenticationOptions` class.</span></span> <span data-ttu-id="ab3bb-168">È consigliabile eseguire la migrazione a 2. x API.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-168">It's recommended that you migrate to the 2.x APIs.</span></span>

* <span data-ttu-id="ab3bb-169">`ClaimsIssuer`è l'autorità emittente da utilizzare per il [dell'autorità di certificazione](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) proprietà in tutte le attestazioni creato per l'autenticazione dei cookie.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-169">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication.</span></span>

* <span data-ttu-id="ab3bb-170">`CookieBuilder.Domain`è il nome di dominio a cui il cookie viene servito.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-170">`CookieBuilder.Domain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="ab3bb-171">Per impostazione predefinita, questo è il nome host che è stata inviata la richiesta.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-171">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="ab3bb-172">Il browser opera solo il cookie a un nome host corrispondente.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-172">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="ab3bb-173">Si potrebbe voler modificare questa opzione per disporre i cookie disponibili per tutti gli host nel dominio.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-173">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="ab3bb-174">Ad esempio, impostando il dominio del cookie `.contoso.com` rende disponibili agli `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`e così via.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-174">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="ab3bb-175">`CookieBuilder.HttpOnly`flag che indica se il cookie deve essere accessibile solo ai server.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-175">`CookieBuilder.HttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="ab3bb-176">L'impostazione predefinita è `true`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-176">This defaults to `true`.</span></span> <span data-ttu-id="ab3bb-177">Se si modifica questo valore può aprire l'applicazione per il furto di cookie l'applicazione deve disporre di un bug Cross-Site Scripting.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-177">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="ab3bb-178">`CookieBuilder.Path`Consente di isolare le applicazioni eseguono lo stesso nome host.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-178">`CookieBuilder.Path` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="ab3bb-179">Se si dispone di un'app in esecuzione in `/app1` per limitare i cookie per essere inviati solo a tale applicazione, quindi è necessario impostare il `CookiePath` proprietà `/app1`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-179">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="ab3bb-180">In questo modo, il cookie è disponibile solo per le richieste a `/app1` o qualsiasi elemento in esso contenuti.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-180">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="ab3bb-181">`CookieBuilder.SameSite`indica se il browser deve consentire i cookie da collegare alle richieste nello stesso sito o tra siti.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-181">`CookieBuilder.SameSite` indicates whether the browser should allow the cookie to be attached to same-site or cross-site requests.</span></span> <span data-ttu-id="ab3bb-182">L'impostazione predefinita è `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-182">This defaults to `SameSiteMode.Lax`.</span></span>

* <span data-ttu-id="ab3bb-183">`CookieBuilder.SecurePolicy`flag che indica se il cookie creato deve essere limitato a HTTPS, HTTP o HTTPS o lo stesso protocollo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-183">`CookieBuilder.SecurePolicy` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="ab3bb-184">L'impostazione predefinita è `SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-184">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="ab3bb-185">`ExpireTimeSpan`è il `TimeSpan` dopo che il cookie scade.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-185">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="ab3bb-186">Viene aggiunto per la data e ora correnti per creare la data di scadenza del cookie.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-186">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="ab3bb-187">`SlidingExpiration`flag che indica se la data di scadenza del cookie viene reimpostato quando più di metà del `ExpireTimeSpan` trascorso l'intervallo.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-187">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="ab3bb-188">La nuova data di scadenza viene spostata in avanti la data corrente più il `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-188">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="ab3bb-189">Un [ora di scadenza assoluto](xref:security/authentication/cookie#security-authentication-absolute-expiry) può essere impostato utilizzando il `AuthenticationProperties` classe quando si chiama `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-189">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="ab3bb-190">Una scadenza assoluta può migliorare la sicurezza dell'applicazione per limitare la quantità di tempo per cui il cookie di autenticazione è valido.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-190">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="ab3bb-191">Un esempio di utilizzo `CookieAuthenticationOptions` nel `ConfigureServices` metodo *Startup.cs* segue:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-191">An example of using `CookieAuthenticationOptions` in the `ConfigureServices` method of *Startup.cs* follows:</span></span>

```csharp
services.AddAuthentication()
        .AddCookie(options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab3bb-192">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="ab3bb-193">`ClaimsIssuer`è l'autorità emittente da utilizzare per il [dell'autorità di certificazione](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) proprietà in tutte le attestazioni creato dal middleware.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-193">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the middleware.</span></span>

* <span data-ttu-id="ab3bb-194">`CookieDomain`è il nome di dominio a cui il cookie viene servito.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-194">`CookieDomain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="ab3bb-195">Per impostazione predefinita, questo è il nome host che è stata inviata la richiesta.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-195">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="ab3bb-196">Il browser opera solo il cookie a un nome host corrispondente.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-196">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="ab3bb-197">Si potrebbe voler modificare questa opzione per disporre i cookie disponibili per tutti gli host nel dominio.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-197">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="ab3bb-198">Ad esempio, impostando il dominio del cookie `.contoso.com` rende disponibili agli `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`e così via.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-198">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="ab3bb-199">`CookieHttpOnly`flag che indica se il cookie deve essere accessibile solo ai server.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-199">`CookieHttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="ab3bb-200">L'impostazione predefinita è `true`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-200">This defaults to `true`.</span></span> <span data-ttu-id="ab3bb-201">Se si modifica questo valore può aprire l'applicazione per il furto di cookie l'applicazione deve disporre di un bug Cross-Site Scripting.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-201">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="ab3bb-202">`CookiePath`Consente di isolare le applicazioni eseguono lo stesso nome host.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-202">`CookiePath` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="ab3bb-203">Se si dispone di un'app in esecuzione in `/app1` per limitare i cookie per essere inviati solo a tale applicazione, quindi è necessario impostare il `CookiePath` proprietà `/app1`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-203">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="ab3bb-204">In questo modo, il cookie è disponibile solo per le richieste a `/app1` o qualsiasi elemento in esso contenuti.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-204">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="ab3bb-205">`CookieSecure`flag che indica se il cookie creato deve essere limitato a HTTPS, HTTP o HTTPS o lo stesso protocollo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-205">`CookieSecure` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="ab3bb-206">L'impostazione predefinita è `SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-206">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="ab3bb-207">`ExpireTimeSpan`è il `TimeSpan` dopo che il cookie scade.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-207">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="ab3bb-208">Viene aggiunto per la data e ora correnti per creare la data di scadenza del cookie.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-208">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="ab3bb-209">`SlidingExpiration`flag che indica se la data di scadenza del cookie viene reimpostato quando più di metà del `ExpireTimeSpan` trascorso l'intervallo.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-209">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="ab3bb-210">La nuova data di scadenza viene spostata in avanti la data corrente più il `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-210">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="ab3bb-211">Un [ora di scadenza assoluto](xref:security/authentication/cookie#security-authentication-absolute-expiry) può essere impostato utilizzando il `AuthenticationProperties` classe quando si chiama `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-211">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="ab3bb-212">Una scadenza assoluta può migliorare la sicurezza dell'applicazione per limitare la quantità di tempo per cui il cookie di autenticazione è valido.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-212">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="ab3bb-213">Un esempio di utilizzo `CookieAuthenticationOptions` nel `Configure` metodo *Startup.cs* segue:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-213">An example of using `CookieAuthenticationOptions` in the `Configure` method of *Startup.cs* follows:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    CookieName = "AuthCookie",
    CookieDomain = "contoso.com",
    CookiePath = "/",
    CookieHttpOnly = true,
    CookieSecure = CookieSecurePolicy.Always
});
```

---

## <a name="persistent-cookies-and-absolute-expiry-times"></a><span data-ttu-id="ab3bb-214">I cookie permanenti e ora di scadenza assoluto</span><span class="sxs-lookup"><span data-stu-id="ab3bb-214">Persistent cookies and absolute expiry times</span></span>

<span data-ttu-id="ab3bb-215">È la scadenza del cookie per rendere persistenti le sessioni del browser e si desidera una scadenza assoluta per l'identità e il cookie il trasporto.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-215">You may want the cookie expiry to persist across browser sessions and want an absolute expiry to the identity and the cookie transporting it.</span></span> <span data-ttu-id="ab3bb-216">Questo tipo di persistenza deve essere abilitata solo con consenso esplicito, tramite un controllo checkbox "Memorizza dati" su account di accesso o un meccanismo simile.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-216">This persistence should only be enabled with explicit user consent, via a "Remember Me" checkbox on login or a similar mechanism.</span></span> <span data-ttu-id="ab3bb-217">È possibile eseguire queste operazioni utilizzando il `AuthenticationProperties` parametro il `SignInAsync` metodo chiamato quando [un'identità di firma e la creazione del cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie).</span><span class="sxs-lookup"><span data-stu-id="ab3bb-217">You can do these things by using the `AuthenticationProperties` parameter on the `SignInAsync` method called when [signing in an identity and creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie).</span></span> <span data-ttu-id="ab3bb-218">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ab3bb-218">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab3bb-219">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="ab3bb-220">Il `AuthenticationProperties` (classe), utilizzato nel frammento di codice precedente, si trova nel `Microsoft.AspNetCore.Authentication` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-220">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab3bb-221">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-221">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="ab3bb-222">Il `AuthenticationProperties` (classe), utilizzato nel frammento di codice precedente, si trova nel `Microsoft.AspNetCore.Http.Authentication` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-222">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

<span data-ttu-id="ab3bb-223">Frammento di codice precedente crea un'identità e i cookie corrispondente che resta tramite chiusure browser.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-223">The preceding code snippet creates an identity and corresponding cookie which survives through browser closures.</span></span> <span data-ttu-id="ab3bb-224">Qualsiasi variabile configurate in precedenza tramite le impostazioni di scadenza [opzioni del cookie](xref:security/authentication/cookie#security-authentication-cookie-options) sono comunque supportati.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-224">Any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options) are still honored.</span></span> <span data-ttu-id="ab3bb-225">Se il cookie scade al contempo il browser viene chiuso, il browser cancellato quando viene riavviato.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-225">If the cookie expires whilst the browser is closed, the browser clears it once it is restarted.</span></span>

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ab3bb-226">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ab3bb-227">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="ab3bb-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

<span data-ttu-id="ab3bb-228">Frammento di codice precedente crea un'identità e i cookie corrispondente che la durata è di 20 minuti.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-228">The preceding code snippet creates an identity and corresponding cookie which lasts for 20 minutes.</span></span> <span data-ttu-id="ab3bb-229">Questo ignora eventuali impostazioni di scadenza variabile configurati in precedenza tramite [opzioni del cookie](xref:security/authentication/cookie#security-authentication-cookie-options).</span><span class="sxs-lookup"><span data-stu-id="ab3bb-229">This ignores any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options).</span></span>

<span data-ttu-id="ab3bb-230">Il `ExpiresUtc` e `IsPersistent` si escludono a vicenda.</span><span class="sxs-lookup"><span data-stu-id="ab3bb-230">The `ExpiresUtc` and `IsPersistent` properties are mutually exclusive.</span></span>