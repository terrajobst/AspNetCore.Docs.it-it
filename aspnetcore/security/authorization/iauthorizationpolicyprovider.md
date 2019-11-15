---
title: Provider di criteri di autorizzazione personalizzati in ASP.NET Core
author: mjrousos
description: Informazioni su come usare un IAuthorizationPolicyProvider personalizzato in un'app ASP.NET Core per generare dinamicamente i criteri di autorizzazione.
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 4f6a4ea209ebe30759f9f14b15b0385399b36ead
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2019
ms.locfileid: "74116063"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Provider di criteri di autorizzazione personalizzati che usano IAuthorizationPolicyProvider in ASP.NET Core 

Di [Mike risveglia](https://github.com/mjrousos)

In genere, quando si utilizza l' [autorizzazione basata su criteri](xref:security/authorization/policies), i criteri vengono registrati chiamando `AuthorizationOptions.AddPolicy` come parte della configurazione del servizio di autorizzazione. In alcuni scenari potrebbe non essere possibile (o auspicabile) registrare tutti i criteri di autorizzazione in questo modo. In questi casi, è possibile usare un `IAuthorizationPolicyProvider` personalizzato per controllare come vengono forniti i criteri di autorizzazione.

Esempi di scenari in cui un [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) personalizzato può essere utile includono:

* Uso di un servizio esterno per fornire la valutazione dei criteri.
* Usando un'ampia gamma di criteri (ad esempio, per diversi numeri di stanza o età), non ha senso aggiungere ogni singolo criterio di autorizzazione con una chiamata `AuthorizationOptions.AddPolicy`.
* Creazione di criteri in fase di esecuzione in base alle informazioni in un'origine dati esterna, ad esempio un database, o determinazione dinamica dei requisiti di autorizzazione tramite un altro meccanismo.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) dal [repository GitHub AspNetCore](https://github.com/aspnet/AspNetCore). Scaricare il file ZIP del repository ASPNET/AspNetCore. Decomprimere il file. Passare alla cartella di progetto *SRC/Security/Samples/CustomPolicyProvider* .

## <a name="customize-policy-retrieval"></a>Personalizzare il recupero dei criteri

Le app ASP.NET Core usano un'implementazione dell'interfaccia `IAuthorizationPolicyProvider` per recuperare i criteri di autorizzazione. Per impostazione predefinita, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) è registrato e usato. `DefaultAuthorizationPolicyProvider` restituisce i criteri dal `AuthorizationOptions` fornito in una chiamata `IServiceCollection.AddAuthorization`.

È possibile personalizzare questo comportamento registrando un'implementazione di `IAuthorizationPolicyProvider` diversa nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app. 

L'interfaccia `IAuthorizationPolicyProvider` contiene tre API:

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) restituisce un criterio di autorizzazione per un nome specificato.
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) restituisce i criteri di autorizzazione predefiniti (i criteri utilizzati per `[Authorize]` attributi senza un criterio specificato). 
* [GetFallbackPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync) restituisce i criteri di autorizzazione di fallback (il criterio utilizzato dal middleware di autorizzazione quando non è specificato alcun criterio). 

Implementando queste API, è possibile personalizzare il modo in cui vengono forniti i criteri di autorizzazione.

## <a name="parameterized-authorize-attribute-example"></a>Esempio di attributo di autorizzazione con parametri

Uno scenario in cui `IAuthorizationPolicyProvider` è utile è l'abilitazione di attributi `[Authorize]` personalizzati i cui requisiti dipendono da un parametro. Nella documentazione [sull'autorizzazione basata su criteri](xref:security/authorization/policies) , ad esempio, è stato usato un criterio basato sull'età ("AtLeast21") come esempio. Se le azioni del controller diverse in un'app devono essere rese disponibili agli utenti con età *diverse* , potrebbe essere utile avere molti criteri basati sull'età diversi. Anziché registrare tutti i diversi criteri basati sull'età necessari per l'applicazione in `AuthorizationOptions`, è possibile generare i criteri in modo dinamico con un `IAuthorizationPolicyProvider`personalizzato. Per semplificare l'uso dei criteri, è possibile annotare le azioni con attributi di autorizzazione personalizzati come `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Attributi di autorizzazione personalizzati

I criteri di autorizzazione sono identificati dai rispettivi nomi. La `MinimumAgeAuthorizeAttribute` personalizzata descritta in precedenza deve eseguire il mapping degli argomenti in una stringa che può essere utilizzata per recuperare i criteri di autorizzazione corrispondenti. È possibile eseguire questa operazione derivando da `AuthorizeAttribute` e impostando la proprietà `Age` per eseguire il wrapping della proprietà `AuthorizeAttribute.Policy`.

```csharp
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

Questo tipo di attributo dispone di una stringa di `Policy` basata sul prefisso hardcoded (`"MinimumAge"`) e su un intero passato tramite il costruttore.

È possibile applicarlo alle azioni in modo analogo ad altri `Authorize` attributi, ad eccezione del fatto che accetta un numero intero come parametro.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>IAuthorizationPolicyProvider personalizzato

La `MinimumAgeAuthorizeAttribute` personalizzata rende più semplice richiedere i criteri di autorizzazione per qualsiasi età minima desiderata. Il prossimo problema da risolvere è garantire che i criteri di autorizzazione siano disponibili per tutte le epoche diverse. Questa è la posizione in cui è utile un `IAuthorizationPolicyProvider`.

Quando si usa `MinimumAgeAuthorizationAttribute`, i nomi dei criteri di autorizzazione seguiranno il criterio `"MinimumAge" + Age`, quindi il `IAuthorizationPolicyProvider` personalizzato deve generare i criteri di autorizzazione per:

* Analisi dell'età dal nome del criterio.
* Uso di `AuthorizationPolicyBuilder` per creare un nuovo `AuthorizationPolicy`
* In questo e negli esempi seguenti si presuppone che l'utente venga autenticato tramite un cookie. Il `AuthorizationPolicyBuilder` deve essere costruito con almeno un nome di schema di autorizzazione o avere sempre esito positivo. In caso contrario, non sono disponibili informazioni su come fornire un problema all'utente e verrà generata un'eccezione.
* Aggiunta di requisiti ai criteri in base all'età con `AuthorizationPolicyBuilder.AddRequirements`. In altri scenari, è possibile usare invece `RequireClaim`, `RequireRole`o `RequireUserName`.

```csharp
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
            var policy = new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme);
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a>Provider di criteri di autorizzazione multipli

Quando si utilizzano implementazioni di `IAuthorizationPolicyProvider` personalizzate, tenere presente che ASP.NET Core utilizza solo un'istanza di `IAuthorizationPolicyProvider`. Se un provider personalizzato non è in grado di fornire i criteri di autorizzazione per tutti i nomi di criteri che verranno usati, deve rinviare a un provider di backup. 

Si consideri, ad esempio, un'applicazione che richiede criteri di validità personalizzati e il recupero di criteri più tradizionali basati sui ruoli. Tale app potrebbe usare un provider di criteri di autorizzazione personalizzato che:

* Tenta di analizzare i nomi dei criteri. 
* Chiama un provider di criteri diverso (ad esempio `DefaultAuthorizationPolicyProvider`) se il nome dei criteri non contiene un valore Age.

L'implementazione di esempio `IAuthorizationPolicyProvider` illustrata in precedenza può essere aggiornata per usare il `DefaultAuthorizationPolicyProvider` creando un provider di criteri di backup nel relativo costruttore (da usare nel caso in cui il nome dei criteri non corrisponda al modello previsto di "minime" + Age).

```csharp
private DefaultAuthorizationPolicyProvider BackupPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    BackupPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

Quindi, è possibile aggiornare il metodo `GetPolicyAsync` per usare il `BackupPolicyProvider` anziché restituire null:

```csharp
...
return BackupPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>Criterio predefinito

Oltre a fornire i criteri di autorizzazione denominati, un `IAuthorizationPolicyProvider` personalizzato deve implementare `GetDefaultPolicyAsync` per fornire un criterio di autorizzazione per `[Authorize]` attributi senza specificare un nome di criterio.

In molti casi, questo attributo di autorizzazione richiede solo un utente autenticato, pertanto è possibile effettuare i criteri necessari con una chiamata a `RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

Come per tutti gli aspetti di un `IAuthorizationPolicyProvider`personalizzato, è possibile personalizzarlo in base alle esigenze. In alcuni casi, potrebbe essere opportuno recuperare i criteri predefiniti da un `IAuthorizationPolicyProvider`di fallback.

## <a name="fallback-policy"></a>Criteri di fallback

Una `IAuthorizationPolicyProvider` personalizzata può implementare facoltativamente `GetFallbackPolicyAsync` per fornire un criterio usato durante la [combinazione di criteri](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine) e quando non vengono specificati criteri. Se `GetFallbackPolicyAsync` restituisce un criterio non null, il criterio restituito viene utilizzato dal middleware di autorizzazione quando non vengono specificati criteri per la richiesta.

Se non è richiesto alcun criterio di fallback, il provider può restituire `null` o rinviare al provider di fallback:

```csharp
public Task<AuthorizationPolicy> GetFallbackPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Usare un IAuthorizationPolicyProvider personalizzato

Per usare criteri personalizzati da una `IAuthorizationPolicyProvider`, è necessario:

* Registrare i tipi di `AuthorizationHandler` appropriati con l'inserimento di dipendenze (descritto in [autorizzazione basata su criteri](xref:security/authorization/policies#authorization-handlers)), come per tutti gli scenari di autorizzazione basata su criteri.
* Registrare il tipo di `IAuthorizationPolicyProvider` personalizzato nella raccolta di servizi di inserimento delle dipendenze dell'app (in `Startup.ConfigureServices`) per sostituire il provider di criteri predefinito.

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Un esempio di `IAuthorizationPolicyProvider` personalizzato completo è disponibile nel [repository GitHub ASPNET/AuthSamples](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).
