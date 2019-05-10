---
title: Provider di criteri di autorizzazione personalizzate in ASP.NET Core
author: mjrousos
description: Informazioni su come usare un IAuthorizationPolicyProvider personalizzato in un'app ASP.NET Core per generare in modo dinamico i criteri di autorizzazione.
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: e17372bb0ec9091c385a70b1e907eaa3cff24003
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896358"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Provider di criteri di autorizzazione personalizzati utilizzando IAuthorizationPolicyProvider in ASP.NET Core 

Da [Mike Rousos](https://github.com/mjrousos)

In genere quando si usa [basata su criteri di autorizzazione](xref:security/authorization/policies), i criteri vengono registrati tramite la chiamata `AuthorizationOptions.AddPolicy` come parte della configurazione del servizio di autorizzazione. In alcuni scenari, potrebbe non essere possibile (o auspicabile) per registrare tutti i criteri di autorizzazione in questo modo. In questi casi, è possibile usare un oggetto personalizzato `IAuthorizationPolicyProvider` per controllare come vengono specificati i criteri di autorizzazione.

Esempi di scenari in cui una classe personalizzata [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) può essere utile includere:

* Usa un servizio esterno per fornire la valutazione dei criteri.
* Usando un'ampia gamma di criteri (per i numeri di chat room diverso o età, ad esempio), pertanto non ha senso aggiungere ciascun criterio di autorizzazione singoli con un `AuthorizationOptions.AddPolicy` chiamare.
* Creazione di criteri in fase di esecuzione basato sulle informazioni contenute in un'origine dati esterna (ad esempio, un database) o determinare i requisiti di autorizzazione in modo dinamico tramite un altro meccanismo.

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) dal [repository GitHub di AspNetCore](https://github.com/aspnet/AspNetCore). Scaricare il file ZIP del repository aspnet/AspNetCore. Decomprimere il file. Passare il *src/Security/samples/CustomPolicyProvider* cartella del progetto.

## <a name="customize-policy-retrieval"></a>Personalizzare il recupero dei criteri

Le app ASP.NET Core usano un'implementazione del `IAuthorizationPolicyProvider` interfaccia da cui recuperare i criteri di autorizzazione. Per impostazione predefinita [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) viene registrata e usata. `DefaultAuthorizationPolicyProvider` Restituisce i criteri dal `AuthorizationOptions` fornito in un `IServiceCollection.AddAuthorization` chiamare.

È possibile personalizzare questo comportamento registrando un diverso `IAuthorizationPolicyProvider` implementazione dell'app [inserimento delle dipendenze](xref:fundamentals/dependency-injection) contenitore. 

Il `IAuthorizationPolicyProvider` interfaccia contiene due API:

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) restituisce i criteri di autorizzazione per un determinato nome.
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) restituisce i criteri di autorizzazione predefinito (il criterio utilizzato per `[Authorize]` attributi senza un criterio specificato). 

Implementando queste due API, è possibile personalizzare la modalità in cui vengono forniti i criteri di autorizzazione.

## <a name="parameterized-authorize-attribute-example"></a>Aggiungere parametri autorizzare l'esempio di attributo

Uno scenario in cui `IAuthorizationPolicyProvider` risulta utile viene personalizzato per consentire `[Authorize]` attributi di cui i requisiti dipendono da un parametro. Ad esempio, nella [basata su criteri di autorizzazione](xref:security/authorization/policies) documentazione, un age-base ("AtLeast21") dei criteri è stato usato come esempio. Se le azioni del controller diversa in un'app devono essere rese disponibili agli utenti di *diversi* età, potrebbe essere utile disporre di molti diversi criteri basati sull'età. Anziché registrare tutti i diversi basati sull'età criteri che l'applicazione sarà necessario nel `AuthorizationOptions`, è possibile generare i criteri in modo dinamico con un oggetto personalizzato `IAuthorizationPolicyProvider`. Per rendere semplificano notevolmente l'utilizzo di criteri, è possibile annotare le azioni con l'attributo di autorizzazione personalizzati, ad esempio `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Attributi di autorizzazione personalizzati

I criteri di autorizzazione vengono identificati con i relativi nomi. L'oggetto personalizzato `MinimumAgeAuthorizeAttribute` descritto in precedenza deve eseguire il mapping di argomenti in una stringa che può essere utilizzata per recuperare i criteri di autorizzazione corrispondente. È possibile farlo mediante la derivazione dalla `AuthorizeAttribute` e il `Age` incapsulamento della proprietà di `AuthorizeAttribute.Policy` proprietà.

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

Questo tipo di attributo ha un `Policy` stringa basata sul prefisso hardcoded (`"MinimumAge"`) e un numero intero passato tramite il costruttore.

È possibile applicarlo alle azioni nello stesso modo delle altre `Authorize` attributi ad eccezione del fatto che accetta un numero intero come parametro.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>IAuthorizationPolicyProvider personalizzato

L'oggetto personalizzato `MinimumAgeAuthorizeAttribute` semplifica i criteri di autorizzazione richiesta per qualsiasi numero minimo di giorni desiderato. Il problema successivo da risolvere è assicurarsi che i criteri di autorizzazione disponibili per tutte le età diversi. Si tratta di dove un `IAuthorizationPolicyProvider` è utile.

Quando si usa `MinimumAgeAuthorizationAttribute`, i nomi dei criteri di autorizzazione seguirà il modello `"MinimumAge" + Age`, quindi l'oggetto personalizzato `IAuthorizationPolicyProvider` deve generare i criteri di autorizzazione per:

* L'età dal nome dei criteri di analisi.
* Usando `AuthorizationPolicyBuilder` per creare un nuovo `AuthorizationPolicy`
* Aggiunta di requisiti per i criteri in base alla data con `AuthorizationPolicyBuilder.AddRequirements`. In altri scenari, è possibile usare `RequireClaim`, `RequireRole`, o `RequireUserName` invece.

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
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a>Più provider di criteri di autorizzazione

Quando si usa custom `IAuthorizationPolicyProvider` implementazioni, tenere presente che ASP.NET Core Usa solo un'istanza di `IAuthorizationPolicyProvider`. Se un provider personalizzato non è in grado di fornire i criteri di autorizzazione per tutti i nomi dei criteri che verrà usati, dovrebbe eseguire il fallback a un provider di backup. 

Ad esempio, prendere in considerazione un'applicazione che richiede criteri di durata personalizzati sia il recupero dei criteri basate sul ruolo più tradizionale. Tale applicazione è stato possibile usare un provider di criteri di autorizzazione personalizzato che:

* Tenta di analizzare i nomi dei criteri. 
* Chiama un provider di criteri diversi (ad esempio `DefaultAuthorizationPolicyProvider`) se il nome dei criteri non contiene un'età.

L'esempio `IAuthorizationPolicyProvider` implementazione illustrata in precedenza può essere aggiornato per usare il `DefaultAuthorizationPolicyProvider` mediante la creazione di un provider di criteri di fallback nel relativo costruttore (da utilizzare nel caso in cui il nome del criterio non corrisponde al modello previsto di 'MinimumAge' + età).

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

Successivamente, il `GetPolicyAsync` metodo può essere aggiornato per usare il `FallbackPolicyProvider` anziché restituire null:

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>Criteri predefiniti

Oltre a fornire i criteri di autorizzazione denominati, una classe personalizzata `IAuthorizationPolicyProvider` deve implementare `GetDefaultPolicyAsync` per fornire un criterio di autorizzazione per `[Authorize]` attributi senza un nome di criterio specificato.

In molti casi, questo attributo di autorizzazione richiede un utente autenticato, solo per sfruttare i criteri necessari con una chiamata a `RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Come con tutti gli aspetti di un oggetto personalizzato `IAuthorizationPolicyProvider`, è possibile personalizzare questa operazione, in base alle esigenze. In alcuni casi, potrebbe essere utile per recuperare i criteri predefiniti da fallback `IAuthorizationPolicyProvider`.

## <a name="required-policy"></a>Necessarie dei criteri

Una classe personalizzata `IAuthorizationPolicyProvider` deve implementare `GetRequiredPolicyAsync` per, facoltativamente, fornire un criterio che è sempre obbligatorio. Se `GetRequiredPolicyAsync` restituisce un criterio diverso da null, tale criterio verrà combinato con qualsiasi altro (denominato o predefinito) dei criteri che sono richiesto.

Se non è necessario alcun criterio obbligatorio, il provider può semplicemente restituiscano null o rinviare al provider di fallback:

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Usare un IAuthorizationPolicyProvider personalizzato

Per usare i criteri personalizzati da un `IAuthorizationPolicyProvider`, è necessario:

* Registrare l'appropriato `AuthorizationHandler` tipi con inserimento delle dipendenze (descritto in [autorizzazione basata su criteri](xref:security/authorization/policies#authorization-handlers)), come con tutti gli scenari di autorizzazione basata su criteri.
* Registrare l'oggetto personalizzato `IAuthorizationPolicyProvider` tipo di raccolta di servizi di inserimento delle dipendenze dell'app (in `Startup.ConfigureServices`) per sostituire il provider di criteri predefiniti.

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Una classe personalizzata completa `IAuthorizationPolicyProvider` è disponibile nell'esempio il [repository di GitHub aspnet/AuthSamples](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).
