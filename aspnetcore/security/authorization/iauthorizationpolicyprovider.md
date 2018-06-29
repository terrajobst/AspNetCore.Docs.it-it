---
title: Provider di criteri di autorizzazione personalizzato in ASP.NET Core
author: mjrousos
description: Imparare a usare un IAuthorizationPolicyProvider personalizzato in un'applicazione ASP.NET Core per generare in modo dinamico i criteri di autorizzazione.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 218d7a495655598046671093c0cfe7b9622aca5e
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077602"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Provider di criteri di autorizzazione personalizzati utilizzando IAuthorizationPolicyProvider in ASP.NET Core 

Da [Mike Rousos](https://github.com/mjrousos)

In genere quando si utilizza [autorizzazione basata su criteri](xref:security/authorization/policies), i criteri vengono registrati chiamando `AuthorizationOptions.AddPolicy` come parte della configurazione del servizio di autorizzazione. In alcuni scenari, potrebbe non essere possibili (o auspicabile) per registrare tutti i criteri di autorizzazione in questo modo. In questi casi, è possibile utilizzare un oggetto personalizzato `IAuthorizationPolicyProvider` per controllare il modo in cui vengono specificati criteri di autorizzazione.

Esempi di scenari in cui un oggetto personalizzato [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) può essere utile includere:

* Utilizzo di un servizio esterno per fornire la valutazione dei criteri.
* Utilizza un'ampia gamma di criteri (per i numeri di chat diverso o età, ad esempio), pertanto può non essere opportuno aggiungere ciascun criterio di autorizzazione singoli con un `AuthorizationOptions.AddPolicy` chiamare.
* Creazione di criteri in fase di esecuzione in base alle informazioni contenute in un'origine dati esterna (ad esempio, un database) o la determinazione dei requisiti di autorizzazione in modo dinamico tramite un altro meccanismo.

## <a name="customizing-policy-retrieval"></a>Personalizzazione di recupero dei criteri

Le app ASP.NET Core usano un'implementazione del `IAuthorizationPolicyProvider` interfaccia da cui recuperare i criteri di autorizzazione. Per impostazione predefinita [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) è registrato e utilizzato. `DefaultAuthorizationPolicyProvider` Restituisce i criteri dal `AuthorizationOptions` fornito un `IServiceCollection.AddAuthorization` chiamare.

È possibile personalizzare questo comportamento registrando un diverso `IAuthorizationPolicyProvider` implementazione dell'app [inserimento di dipendenze](xref:fundamentals/dependency-injection) contenitore. 

Il `IAuthorizationPolicyProvider` interfaccia contiene due API:

* [GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) restituisce un criterio di autorizzazione per un determinato nome.
* [GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) restituisce i criteri di autorizzazione predefiniti (i criteri utilizzati per `[Authorize]` attributi senza un criterio specificato). 

Implementando queste due API, è possibile personalizzare come vengono forniti i criteri di autorizzazione.

## <a name="parameterized-authorize-attribute-example"></a>Parametri autorizzare l'esempio di attributo

Uno scenario in cui `IAuthorizationPolicyProvider` è utile l'attivazione personalizzato `[Authorize]` attributi cui i requisiti dipendono da un parametro. Ad esempio, nel [autorizzazione basata su criteri](xref:security/authorization/policies) documentazione, un'in base all'età ("AtLeast21") dei criteri è stato utilizzato come campione. Se le azioni del controller diverso in un'applicazione devono essere rese disponibili per gli utenti della *diversi* età, potrebbe essere utile disporre di molti criteri diversi in base all'età. Anziché registrare tutti i diversi in base all'età criteri l'applicazione sarà necessarie nella `AuthorizationOptions`, è possibile generare i criteri in modo dinamico con un oggetto personalizzato `IAuthorizationPolicyProvider`. Per rendere semplificano notevolmente l'utilizzo di criteri, è possibile annotare le azioni con l'attributo di autorizzazione personalizzati, ad esempio `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Attributi di autorizzazione personalizzato

Criteri di autorizzazione vengono identificati con i relativi nomi. L'oggetto personalizzato `MinimumAgeAuthorizeAttribute` descritto in precedenza deve eseguire il mapping di argomenti in una stringa che può essere utilizzata per recuperare i criteri di autorizzazione corrispondente. È possibile farlo mediante derivazione da `AuthorizeAttribute` e apportando le `Age` incapsulamento di proprietà di `AuthorizeAttribute.Policy` proprietà.

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

Questo tipo di attributo ha un `Policy` basato su stringa in base al prefisso a livello di codice (`"MinimumAge"`) e un numero intero passato tramite il costruttore.

È possibile applicare alle azioni nello stesso modo di altri `Authorize` attributi ad eccezione del fatto che accetta un numero intero come parametro.

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>IAuthorizationPolicyProvider personalizzato

L'oggetto personalizzato `MinimumAgeAuthorizeAttribute` semplifica i criteri di autorizzazione richiesta per qualsiasi periodo di memorizzazione minimo desiderato. Il problema da risolvere successivo consiste nel garantire che i criteri di autorizzazione sono disponibili per tutte le diverse età. Questa opzione è quando un `IAuthorizationPolicyProvider` è utile.

Quando si utilizza `MinimumAgeAuthorizationAttribute`, i nomi dei criteri di autorizzazione seguiranno il modello `"MinimumAge" + Age`, quindi l'oggetto personalizzato `IAuthorizationPolicyProvider` deve generare i criteri di autorizzazione da:

* L'età dal nome dei criteri di analisi.
* Utilizzo `AuthorizationPolicyBuilder` per creare un nuovo `AuthorizationPolicy`
* Aggiunta di requisiti per i criteri in base all'età con `AuthorizationPolicyBuilder.AddRequirements`. In altri scenari, è possibile utilizzare `RequireClaim`, `RequireRole`, o `RequireUserName` invece.

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

## <a name="multiple-authorization-policy-providers"></a>Più provider di criteri di autorizzazione

Quando si utilizza custom `IAuthorizationPolicyProvider` implementazioni, tenere presente che ASP.NET Core utilizza solo un'istanza `IAuthorizationPolicyProvider`. Se un provider personalizzato non è in grado di fornire i criteri di autorizzazione per tutti i nomi dei criteri, è necessario eseguire il fallback a un provider di backup. I nomi dei criteri potrebbero comprendono quelli che provengono da un criterio predefinito per `[Authorize]` attributi senza nome.

Ad esempio, si consideri che un'applicazione necessaria sia criteri di durata personalizzata e il recupero dei criteri basate sul ruolo più tradizionale. Tale applicazione potrebbe utilizzare un provider di criteri di autorizzazione personalizzato che:

* Tenta di analizzare i nomi dei criteri. 
* Le chiamate a un provider di criteri diverso (ad esempio `DefaultAuthorizationPolicyProvider`) se il nome del criterio non contiene un'età.

## <a name="default-policy"></a>Criterio predefinito

Oltre a fornire criteri di autorizzazione denominati, un oggetto personalizzato `IAuthorizationPolicyProvider` deve implementare `GetDefaultPolicyAsync` per fornire un criterio di autorizzazione per `[Authorize]` attributi senza un nome di criterio specificato.

In molti casi, l'attributo di autorizzazione sufficiente un utente autenticato, pertanto è possibile apportare i criteri necessari con una chiamata a `RequireAuthenticatedUser`:

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Come con tutti gli aspetti di un oggetto personalizzato `IAuthorizationPolicyProvider`, è possibile personalizzare questa, in base alle esigenze. In alcuni casi:

* I criteri di autorizzazione predefiniti potrebbero non essere usati.
* Il recupero dei criteri predefiniti può essere delegato a fallback `IAuthorizationPolicyProvider`.

## <a name="using-a-custom-iauthorizationpolicyprovider"></a>Utilizzando un IAuthorizationPolicyProvider personalizzato

Per usare i criteri personalizzati da un `IAuthorizationPolicyProvider`, è necessario:

* Registrare l'appropriato `AuthorizationHandler` tipi con inserimento di dipendenze (descritto in [autorizzazione basata su criteri](xref:security/authorization/policies#authorization-handlers)), come con tutti gli scenari di autorizzazione basata su criteri.
* Registrare l'oggetto personalizzato `IAuthorizationPolicyProvider` tipo dipendenza attacco intrusivo nel codice del servizio raccolta dell'app (in `Startup.ConfigureServices`) per sostituire il provider di criteri predefinito.

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Personalizzato completo `IAuthorizationPolicyProvider` è disponibile nell'esempio il [repository GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).
