---
title: Provider di criteri di autorizzazione personalizzate in ASP.NET Core
author: mjrousos
description: Informazioni su come usare un IAuthorizationPolicyProvider personalizzato in un'app ASP.NET Core per generare in modo dinamico i criteri di autorizzazione.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 6e46172ec8c5271ffcbad87e4ea5cc98465b78b0
ms.sourcegitcommit: 41d3c4b27309d56f567fd1ad443929aab6587fb1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37910250"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="f6420-103">Provider di criteri di autorizzazione personalizzati utilizzando IAuthorizationPolicyProvider in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6420-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="f6420-104">Da [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="f6420-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="f6420-105">In genere quando si usa [basata su criteri di autorizzazione](xref:security/authorization/policies), i criteri vengono registrati tramite la chiamata `AuthorizationOptions.AddPolicy` come parte della configurazione del servizio di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f6420-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="f6420-106">In alcuni scenari, potrebbe non essere possibile (o auspicabile) per registrare tutti i criteri di autorizzazione in questo modo.</span><span class="sxs-lookup"><span data-stu-id="f6420-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="f6420-107">In questi casi, è possibile usare un oggetto personalizzato `IAuthorizationPolicyProvider` per controllare come vengono specificati i criteri di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f6420-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="f6420-108">Esempi di scenari in cui una classe personalizzata [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) può essere utile includere:</span><span class="sxs-lookup"><span data-stu-id="f6420-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="f6420-109">Usa un servizio esterno per fornire la valutazione dei criteri.</span><span class="sxs-lookup"><span data-stu-id="f6420-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="f6420-110">Usando un'ampia gamma di criteri (per i numeri di chat room diverso o età, ad esempio), pertanto non ha senso aggiungere ciascun criterio di autorizzazione singoli con un `AuthorizationOptions.AddPolicy` chiamare.</span><span class="sxs-lookup"><span data-stu-id="f6420-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="f6420-111">Creazione di criteri in fase di esecuzione basato sulle informazioni contenute in un'origine dati esterna (ad esempio, un database) o determinare i requisiti di autorizzazione in modo dinamico tramite un altro meccanismo.</span><span class="sxs-lookup"><span data-stu-id="f6420-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

## <a name="customizing-policy-retrieval"></a><span data-ttu-id="f6420-112">Personalizzare il recupero dei criteri</span><span class="sxs-lookup"><span data-stu-id="f6420-112">Customizing policy retrieval</span></span>

<span data-ttu-id="f6420-113">Le app ASP.NET Core usano un'implementazione del `IAuthorizationPolicyProvider` interfaccia da cui recuperare i criteri di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f6420-113">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="f6420-114">Per impostazione predefinita [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) viene registrata e usata.</span><span class="sxs-lookup"><span data-stu-id="f6420-114">By default, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="f6420-115">`DefaultAuthorizationPolicyProvider` Restituisce i criteri dal `AuthorizationOptions` fornito in un `IServiceCollection.AddAuthorization` chiamare.</span><span class="sxs-lookup"><span data-stu-id="f6420-115">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="f6420-116">È possibile personalizzare questo comportamento registrando un diverso `IAuthorizationPolicyProvider` implementazione dell'app [inserimento delle dipendenze](xref:fundamentals/dependency-injection) contenitore.</span><span class="sxs-lookup"><span data-stu-id="f6420-116">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="f6420-117">Il `IAuthorizationPolicyProvider` interfaccia contiene due API:</span><span class="sxs-lookup"><span data-stu-id="f6420-117">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="f6420-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) restituisce i criteri di autorizzazione per un determinato nome.</span><span class="sxs-lookup"><span data-stu-id="f6420-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="f6420-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) restituisce i criteri di autorizzazione predefinito (il criterio utilizzato per `[Authorize]` attributi senza un criterio specificato).</span><span class="sxs-lookup"><span data-stu-id="f6420-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="f6420-120">Implementando queste due API, è possibile personalizzare la modalità in cui vengono forniti i criteri di autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="f6420-120">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="f6420-121">Aggiungere parametri autorizzare l'esempio di attributo</span><span class="sxs-lookup"><span data-stu-id="f6420-121">Parameterized authorize attribute example</span></span>

<span data-ttu-id="f6420-122">Uno scenario in cui `IAuthorizationPolicyProvider` risulta utile viene personalizzato per consentire `[Authorize]` attributi di cui i requisiti dipendono da un parametro.</span><span class="sxs-lookup"><span data-stu-id="f6420-122">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="f6420-123">Ad esempio, nella [basata su criteri di autorizzazione](xref:security/authorization/policies) documentazione, un age-base ("AtLeast21") dei criteri è stato usato come esempio.</span><span class="sxs-lookup"><span data-stu-id="f6420-123">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="f6420-124">Se le azioni del controller diversa in un'app devono essere rese disponibili agli utenti di *diversi* età, potrebbe essere utile disporre di molti diversi criteri basati sull'età.</span><span class="sxs-lookup"><span data-stu-id="f6420-124">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="f6420-125">Anziché registrare tutti i diversi basati sull'età criteri che l'applicazione sarà necessario nel `AuthorizationOptions`, è possibile generare i criteri in modo dinamico con un oggetto personalizzato `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="f6420-125">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="f6420-126">Per rendere semplificano notevolmente l'utilizzo di criteri, è possibile annotare le azioni con l'attributo di autorizzazione personalizzati, ad esempio `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="f6420-126">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="f6420-127">Attributi di autorizzazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="f6420-127">Custom Authorization Attributes</span></span>

<span data-ttu-id="f6420-128">I criteri di autorizzazione vengono identificati con i relativi nomi.</span><span class="sxs-lookup"><span data-stu-id="f6420-128">Authorization policies are identified by their names.</span></span> <span data-ttu-id="f6420-129">L'oggetto personalizzato `MinimumAgeAuthorizeAttribute` descritto in precedenza deve eseguire il mapping di argomenti in una stringa che può essere utilizzata per recuperare i criteri di autorizzazione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="f6420-129">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="f6420-130">È possibile farlo mediante la derivazione dalla `AuthorizeAttribute` e il `Age` incapsulamento della proprietà di `AuthorizeAttribute.Policy` proprietà.</span><span class="sxs-lookup"><span data-stu-id="f6420-130">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```CSharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

<span data-ttu-id="f6420-131">Questo tipo di attributo ha un `Policy` stringa basata sul prefisso hardcoded (`"MinimumAge"`) e un numero intero passato tramite il costruttore.</span><span class="sxs-lookup"><span data-stu-id="f6420-131">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="f6420-132">È possibile applicarlo alle azioni nello stesso modo delle altre `Authorize` attributi ad eccezione del fatto che accetta un numero intero come parametro.</span><span class="sxs-lookup"><span data-stu-id="f6420-132">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="f6420-133">IAuthorizationPolicyProvider personalizzato</span><span class="sxs-lookup"><span data-stu-id="f6420-133">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="f6420-134">L'oggetto personalizzato `MinimumAgeAuthorizeAttribute` semplifica i criteri di autorizzazione richiesta per qualsiasi numero minimo di giorni desiderato.</span><span class="sxs-lookup"><span data-stu-id="f6420-134">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="f6420-135">Il problema successivo da risolvere è assicurarsi che i criteri di autorizzazione disponibili per tutte le età diversi.</span><span class="sxs-lookup"><span data-stu-id="f6420-135">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="f6420-136">Si tratta di dove un `IAuthorizationPolicyProvider` è utile.</span><span class="sxs-lookup"><span data-stu-id="f6420-136">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="f6420-137">Quando si usa `MinimumAgeAuthorizationAttribute`, i nomi dei criteri di autorizzazione seguirà il modello `"MinimumAge" + Age`, quindi l'oggetto personalizzato `IAuthorizationPolicyProvider` deve generare i criteri di autorizzazione per:</span><span class="sxs-lookup"><span data-stu-id="f6420-137">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="f6420-138">L'età dal nome dei criteri di analisi.</span><span class="sxs-lookup"><span data-stu-id="f6420-138">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="f6420-139">Usando `AuthorizationPolicyBuilder` per creare un nuovo `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="f6420-139">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="f6420-140">Aggiunta di requisiti per i criteri in base alla data con `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="f6420-140">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="f6420-141">In altri scenari, è possibile usare `RequireClaim`, `RequireRole`, o `RequireUserName` invece.</span><span class="sxs-lookup"><span data-stu-id="f6420-141">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```CSharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="f6420-142">Più provider di criteri di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="f6420-142">Multiple authorization policy providers</span></span>

<span data-ttu-id="f6420-143">Quando si usa custom `IAuthorizationPolicyProvider` implementazioni, tenere presente che ASP.NET Core Usa solo un'istanza di `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="f6420-143">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="f6420-144">Se un provider personalizzato non è in grado di fornire i criteri di autorizzazione per tutti i nomi dei criteri, è necessario eseguire il fallback a un provider di backup.</span><span class="sxs-lookup"><span data-stu-id="f6420-144">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="f6420-145">I nomi dei criteri potrebbero comprendono quelli che provengono da un criterio predefinito per `[Authorize]` attributi senza nome.</span><span class="sxs-lookup"><span data-stu-id="f6420-145">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="f6420-146">Si consideri, ad esempio, che un'applicazione necessaria sia criteri di durata personalizzata e il recupero dei criteri basate sul ruolo più tradizionale.</span><span class="sxs-lookup"><span data-stu-id="f6420-146">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="f6420-147">Tale applicazione è stato possibile usare un provider di criteri di autorizzazione personalizzato che:</span><span class="sxs-lookup"><span data-stu-id="f6420-147">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="f6420-148">Tenta di analizzare i nomi dei criteri.</span><span class="sxs-lookup"><span data-stu-id="f6420-148">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="f6420-149">Chiama un provider di criteri diversi (ad esempio `DefaultAuthorizationPolicyProvider`) se il nome dei criteri non contiene un'età.</span><span class="sxs-lookup"><span data-stu-id="f6420-149">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="f6420-150">Criteri predefiniti</span><span class="sxs-lookup"><span data-stu-id="f6420-150">Default policy</span></span>

<span data-ttu-id="f6420-151">Oltre a fornire i criteri di autorizzazione denominati, una classe personalizzata `IAuthorizationPolicyProvider` deve implementare `GetDefaultPolicyAsync` per fornire un criterio di autorizzazione per `[Authorize]` attributi senza un nome di criterio specificato.</span><span class="sxs-lookup"><span data-stu-id="f6420-151">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="f6420-152">In molti casi, questo attributo di autorizzazione richiede un utente autenticato, solo per sfruttare i criteri necessari con una chiamata a `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="f6420-152">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="f6420-153">Come con tutti gli aspetti di un oggetto personalizzato `IAuthorizationPolicyProvider`, è possibile personalizzare questa operazione, in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="f6420-153">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="f6420-154">In alcuni casi:</span><span class="sxs-lookup"><span data-stu-id="f6420-154">In some cases:</span></span>

* <span data-ttu-id="f6420-155">I criteri di autorizzazione predefiniti potrebbero non essere usati.</span><span class="sxs-lookup"><span data-stu-id="f6420-155">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="f6420-156">Recuperare il criterio predefinito può essere delegato a fallback `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="f6420-156">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="using-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="f6420-157">Usando un IAuthorizationPolicyProvider personalizzato</span><span class="sxs-lookup"><span data-stu-id="f6420-157">Using a Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="f6420-158">Per usare i criteri personalizzati da un `IAuthorizationPolicyProvider`, è necessario:</span><span class="sxs-lookup"><span data-stu-id="f6420-158">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="f6420-159">Registrare l'appropriato `AuthorizationHandler` tipi con inserimento delle dipendenze (descritto in [autorizzazione basata su criteri](xref:security/authorization/policies#authorization-handlers)), come con tutti gli scenari di autorizzazione basata su criteri.</span><span class="sxs-lookup"><span data-stu-id="f6420-159">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="f6420-160">Registrare l'oggetto personalizzato `IAuthorizationPolicyProvider` tipo di raccolta di servizi di inserimento delle dipendenze dell'app (in `Startup.ConfigureServices`) per sostituire il provider di criteri predefiniti.</span><span class="sxs-lookup"><span data-stu-id="f6420-160">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="f6420-161">Una classe personalizzata completa `IAuthorizationPolicyProvider` è disponibile nell'esempio il [repository di GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="f6420-161">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider).</span></span>
