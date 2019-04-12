---
title: Inserimento di dipendenze componenti Razor
author: guardrex
description: Scopri App Blazor e Razor componenti possono utilizzare servizi inserendole in componenti.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 40aec2e3a5032039c7d921f67d7d333b03c07fb1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2019
ms.locfileid: "59515488"
---
# <a name="razor-components-dependency-injection"></a>Inserimento di dipendenze componenti Razor

Da [Stropek Rainer](https://www.timecockpit.com)

Supporta i componenti di Razor [inserimento delle dipendenze (dipendenze)](xref:fundamentals/dependency-injection). Le app possono usare i servizi incorporati inserendole in componenti. Le app possono anche definire e registrare servizi personalizzati e renderli disponibili in tutta l'app tramite l'inserimento delle dipendenze.

## <a name="dependency-injection"></a>Inserimento di dipendenze

L'inserimento delle dipendenze è una tecnica per l'accesso ai servizi configurati in una posizione centrale. Ciò può risultare utile nelle App Razor componenti per:

* Condividere una singola istanza di una classe di servizio tra molti componenti, noti come un *singleton* servizio.
* Separare i componenti dalle classi concrete servizio utilizzando le astrazioni di riferimento. Si consideri ad esempio un'interfaccia `IDataAccess` per l'accesso ai dati nell'app. L'interfaccia è implementata da un concreto `DataAccess` classe e registrato come servizio nel contenitore dei servizi dell'app. Quando un componente Usa l'inserimento delle dipendenze per ricevere un `IDataAccess` implementazione, il componente non è associato al tipo concreto. L'implementazione è possibile scambiare, forse a un'implementazione fittizia negli unit test.

Per altre informazioni, vedere <xref:fundamentals/dependency-injection>.

## <a name="add-services-to-di"></a>Aggiungere servizi per l'inserimento delle dipendenze

Dopo aver creato una nuova app, esaminare il `Startup.ConfigureServices` metodo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Il `ConfigureServices` viene passato un <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, ovvero un elenco di oggetti descrittore di servizio (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). I servizi vengono aggiunti, fornendo i descrittori di servizio per l'insieme al servizio. Nell'esempio seguente viene illustrato il concetto con il `IDataAccess` interfaccia e la relativa implementazione concreta `DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Servizi possono essere configurati con le durate illustrate nella tabella seguente.

| Durata | Descrizione |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | L'inserimento delle dipendenze consente di creare un *singola istanza* del servizio. Tutti i componenti che richiedono un `Singleton` servizio ricevere un'istanza dello stesso servizio. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Ogni volta che un componente ottiene un'istanza di un `Transient` servizio dal contenitore dei servizi, riceve un *nuova istanza* del servizio. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor lato client non ha attualmente il concetto di ambiti di inserimento delle dipendenze. `Scoped` si comporta come `Singleton`. Tuttavia, i componenti di ASP.NET Core Razor supporta il `Scoped` durata. In un componente di Razor, la registrazione di un servizio con ambito ha come ambita la connessione. Per questo motivo, tramite i servizi con ambiti è preferibile per i servizi che devono avere come ambiti l'utente corrente, anche se si intende corrente per l'esecuzione del client nel browser. |

Il sistema di inserimento delle dipendenze si basa sul sistema di inserimento delle dipendenze in ASP.NET Core. Per altre informazioni, vedere <xref:fundamentals/dependency-injection>.

## <a name="default-services"></a>Servizi predefiniti

I servizi predefiniti vengono aggiunti automaticamente alla raccolta di servizio dell'app.

| Service | Descrizione |
| ------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Fornisce metodi per inviare richieste HTTP e ricevere risposte HTTP da una risorsa identificata da un URI (singleton). Si noti che questa istanza di `HttpClient` Usa il browser per la gestione del traffico HTTP in background. [HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) viene impostata automaticamente per il prefisso URI di base dell'app. `HttpClient` viene fornito solo per le app Blazor lato client. |
| `IJSRuntime` | Rappresenta un'istanza di un runtime JavaScript a cui possono essere inviate chiamate. Per altre informazioni, vedere <xref:razor-components/javascript-interop>. |
| `IUriHelper` | Contiene gli helper per l'uso di URI e l'esplorazione dello stato (singleton). `IUriHelper` viene fornito all'App Blazor sia componenti Razor. |

È possibile usare un provider di servizi personalizzati anziché il provider di servizi predefiniti aggiunto dal modello predefinito. Un provider di servizi personalizzati non fornisce automaticamente i servizi predefiniti elencati nella tabella. Se si usa un provider di servizi personalizzati e si richiedono i servizi illustrati nella tabella, aggiungere i servizi necessari per il nuovo provider del servizio.

## <a name="request-a-service-in-a-component"></a>Richiesta di un servizio in un componente

Dopo che i servizi vengono aggiunti alla raccolta di servizio, inserire i modelli Razor dei componenti tramite i servizi di [ @inject ](xref:mvc/views/razor#section-4) direttiva Razor. `@inject` presenta due parametri:

* Nome del tipo: Il tipo del servizio da inserire.
* Nome proprietà: Il nome della proprietà la ricezione del servizio app inserito. Si noti che la proprietà non richiede la creazione manuale. Il compilatore crea la proprietà.

Per altre informazioni, vedere <xref:mvc/views/dependency-injection>.

Usare più `@inject` istruzioni per inserire diversi servizi.

Nell'esempio riportato di seguito viene illustrato come usare `@inject`. Il servizio che implementa `Services.IDataAccess` viene inserito nelle proprietà del componente `DataRepository`. Si noti come Usa solo il codice di `IDataAccess` astrazione:

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.cshtml?highlight=2-3,23)]

Internamente, la proprietà generata (`DataRepository`) è dotato di `InjectAttribute` attributo. In genere, questo attributo non viene utilizzato direttamente. Se è necessaria per i componenti di una classe base e inserito le proprietà sono necessari anche per la classe di base, `InjectAttribute` possono essere aggiunte manualmente:

```csharp
public class ComponentBase : IComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

In componenti derivati dalla classe di base, il `@inject` direttiva non è obbligatorio. Il `InjectAttribute` della classe di base è sufficiente:

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a>Inserimento delle dipendenze in servizi

Servizi complessi potrebbero richiedere servizi aggiuntivi. Nell'esempio precedente `DataAccess` potrebbe richiedere il `HttpClient` del servizio predefinito. `@inject` (o `InjectAttribute`) non è disponibile per l'utilizzo nei servizi. *Inserimento del costruttore* devono quindi essere utilizzate. I servizi necessari vengono aggiunti tramite l'aggiunta di parametri al costruttore del servizio. Quando l'inserimento delle dipendenze consente di creare il servizio, riconosce i servizi richiede nel costruttore e consente loro di conseguenza.

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

Prerequisiti per l'inserimento del costruttore:

* Deve essere presente un costruttore cui argomenti possono essere tutti specificati tramite inserimento delle dipendenze. Si noti che sono consentiti parametri aggiuntivi non coperti da inserimento delle dipendenze, se si specificano valori predefiniti.
* Il costruttore applicabile deve essere *pubblica*.
* Deve essere presente solo un costruttore applicabile. In caso di ambiguità, l'inserimento delle dipendenze genera un'eccezione.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
