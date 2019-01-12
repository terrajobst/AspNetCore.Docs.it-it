---
title: Provider di criteri di autorizzazione personalizzate in ASP.NET Core
author: mjrousos
description: Informazioni su come usare un IAuthorizationPolicyProvider personalizzato in un'app ASP.NET Core per generare in modo dinamico i criteri di autorizzazione.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: ef3e81da6fb9e2e332b553607be35fcd79e9362d
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249373"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Provider di criteri di autorizzazione personalizzati utilizzando IAuthorizationPolicyProvider in ASP.NET Core 

Da [Mike Rousos](https://github.com/mjrousos)

In genere quando si usa [basata su criteri di autorizzazione](xref:security/authorization/policies), i criteri vengono registrati tramite la chiamata `AuthorizationOptions.AddPolicy` come parte della configurazione del servizio di autorizzazione. In alcuni scenari, potrebbe non essere possibile (o auspicabile) per registrare tutti i criteri di autorizzazione in questo modo. In questi casi, è possibile usare un oggetto personalizzato `IAuthorizationPolicyProvider` per controllare come vengono specificati i criteri di autorizzazione.

Esempi di scenari in cui una classe personalizzata [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) può essere utile includere:

* Usa un servizio esterno per fornire la valutazione dei criteri.
* Usando un'ampia gamma di criteri (per i numeri di chat room diverso o età, ad esempio), pertanto non ha senso aggiungere ciascun criterio di autorizzazione singoli con un `AuthorizationOptions.AddPolicy` chiamare.
* Creazione di criteri in fase di esecuzione basato sulle informazioni contenute in un'origine dati esterna (ad esempio, un database) o determinare i requisiti di autorizzazione in modo dinamico tramite un altro meccanismo.

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/AuthSamples/) dal [repository GitHub di AspNetCore](https://github.com/aspnet/AspNetCore). Scaricare il file ZIP del repository aspnet/AuthSamples.
Decomprimere il *AuthSamples-master. zip* file. Passare il *campioni/CustomPolicyProvider* cartella del progetto.

## <a name="customize-policy-retrieval"></a>Personalizzare il recupero dei criteri

Le app ASP.NET Core usano un'implementazione del `IAuthorizationPolicyProvider` interfaccia da cui recuperare i criteri di autorizzazione. Per impostazione predefinita [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) viene registrata e usata. `DefaultAuthorizationPolicyProvider` Restituisce i criteri dal `AuthorizationOptions` fornito in un `IServiceCollection.AddAuthorization` chiamare.

È possibile personalizzare questo comportamento registrando un diverso `IAuthorizationPolicyProvider` implementazione dell'app [inserimento delle dipendenze](xref:fundamentals/dependency-injection) contenitore. 

Il `IAuthorizationPolicyProvider` interfaccia contiene due API:

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) restituisce i criteri di autorizzazione per un determinato nome.
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) restituisce i criteri di autorizzazione predefinito (il criterio utilizzato per `[Authorize]` attributi senza un criterio specificato). 

Implementando queste due API, è possibile personalizzare la modalità in cui vengono forniti i criteri di autorizzazione.

## <a name="parameterized-authorize-attribute-example"></a>Aggiungere parametri autorizzare l'esempio di attributo

Uno scenario in cui `IAuthorizationPolicyProvider` risulta utile viene personalizzato per consentire `[Authorize]` attributi di cui i requisiti dipendono da un parametro. Ad esempio, nella [basata su criteri di autorizzazione](xref:security/authorization/policies) documentazione, un age-base ("AtLeast21") dei criteri è stato usato come esempio. Se le azioni del controller diversa in un'app devono essere rese disponibili agli utenti di *diversi* età, potrebbe essere utile disporre di molti diversi criteri basati sull'età. Anziché registrare tutti i diversi basati sull'età criteri che l'applicazione sarà necessario nel `AuthorizationOptions`, è possibile generare i criteri in modo dinamico con un oggetto personalizzato `IAuthorizationPolicyProvider`. Per rendere semplificano notevolmente l'utilizzo di criteri, è possibile annotare le azioni con l'attributo di autorizzazione personalizzati, ad esempio `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Attributi di autorizzazione personalizzato

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

Quando si usa custom `IAuthorizationPolicyProvider` implementazioni, tenere presente che ASP.NET Core Usa solo un'istanza di `IAuthorizationPolicyProvider`. Se un provider personalizzato non è in grado di fornire i criteri di autorizzazione per tutti i nomi dei criteri, è necessario eseguire il fallback a un provider di backup. I nomi dei criteri potrebbero comprendono quelli che provengono da un criterio predefinito per `[Authorize]` attributi senza nome.

Si consideri, ad esempio, che un'applicazione necessaria sia criteri di durata personalizzata e il recupero dei criteri basate sul ruolo più tradizionale. Tale applicazione è stato possibile usare un provider di criteri di autorizzazione personalizzato che:

* Tenta di analizzare i nomi dei criteri. 
* Chiama un provider di criteri diversi (ad esempio `DefaultAuthorizationPolicyProvider`) se il nome dei criteri non contiene un'età.

## <a name="default-policy"></a>Criteri predefiniti

Oltre a fornire i criteri di autorizzazione denominati, una classe personalizzata `IAuthorizationPolicyProvider` deve implementare `GetDefaultPolicyAsync` per fornire un criterio di autorizzazione per `[Authorize]` attributi senza un nome di criterio specificato.

In molti casi, questo attributo di autorizzazione richiede un utente autenticato, solo per sfruttare i criteri necessari con una chiamata a `RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Come con tutti gli aspetti di un oggetto personalizzato `IAuthorizationPolicyProvider`, è possibile personalizzare questa operazione, in base alle esigenze. In alcuni casi:

* I criteri di autorizzazione predefiniti potrebbero non essere usati.
* Recuperare il criterio predefinito può essere delegato a fallback `IAuthorizationPolicyProvider`.

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Usare un IAuthorizationPolicyProvider personalizzato

Per usare i criteri personalizzati da un `IAuthorizationPolicyProvider`, è necessario:

* Registrare l'appropriato `AuthorizationHandler` tipi con inserimento delle dipendenze (descritto in [autorizzazione basata su criteri](xref:security/authorization/policies#authorization-handlers)), come con tutti gli scenari di autorizzazione basata su criteri.
* Registrare l'oggetto personalizzato `IAuthorizationPolicyProvider` tipo di raccolta di servizi di inserimento delle dipendenze dell'app (in `Startup.ConfigureServices`) per sostituire il provider di criteri predefiniti.

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Una classe personalizzata completa `IAuthorizationPolicyProvider` è disponibile nell'esempio il [repository di GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider).
