---
title: ASP.NET Core Blazor inserimento delle dipendenze
author: guardrex
description: Scopri in che modo le app Blazor possono inserire i servizi nei componenti.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 4cdde9ee8c9fd9adf00894a067d32965b180e5ec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658074"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>Inserimento delle dipendenze di ASP.NET Core Blazor

Di [Rainer Stropek](https://www.timecockpit.com) e [Mike entusiasmanti](https://github.com/mjrousos)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazer supporta l' [inserimento delle dipendenze (di)](xref:fundamentals/dependency-injection). Le app possono usare i servizi predefiniti inserendoli in componenti. Le app possono anche definire e registrare servizi personalizzati e renderli disponibili nell'app tramite DI.

DI è una tecnica per accedere ai servizi configurati in una posizione centrale. Questa operazione può essere utile nelle app Blazor per:

* Condividere una singola istanza di una classe di servizio in molti componenti, noti come un servizio *singleton* .
* Separare i componenti da classi di servizi concrete usando astrazioni di riferimento. Si consideri, ad esempio, un'interfaccia `IDataAccess` per l'accesso ai dati nell'app. L'interfaccia viene implementata da una classe di `DataAccess` concreta e registrata come servizio nel contenitore del servizio dell'app. Quando un componente usa il per ricevere un'implementazione di `IDataAccess`, il componente non è associato al tipo concreto. L'implementazione può essere scambiata, ad esempio per un'implementazione fittizia negli unit test.

## <a name="default-services"></a>Servizi predefiniti

I servizi predefiniti vengono aggiunti automaticamente alla raccolta di servizi dell'app.

| Service | Durata | Descrizione |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Singleton | Fornisce metodi per l'invio di richieste HTTP e la ricezione di risposte HTTP da una risorsa identificata da un URI.<br><br>L'istanza di `HttpClient` in un'app webassembly Blazer usa il browser per gestire il traffico HTTP in background.<br><br>Per impostazione predefinita, le app del server Blazer non includono un `HttpClient` configurato come servizio. Fornire un `HttpClient` a un'app del server blazer.<br><br>Per altre informazioni, vedere <xref:blazor/call-web-api>. |
| `IJSRuntime` | Singleton (webassembly Blazer)<br>Con ambito (server Blazer) | Rappresenta un'istanza di un runtime JavaScript in cui vengono inviate le chiamate a JavaScript. Per altre informazioni, vedere <xref:blazor/call-javascript-from-dotnet>. |
| `NavigationManager` | Singleton (webassembly Blazer)<br>Con ambito (server Blazer) | Contiene gli helper per lavorare con gli URI e lo stato di navigazione. Per ulteriori informazioni, vedere [URI e Helper dello stato di navigazione](xref:blazor/routing#uri-and-navigation-state-helpers). |

Un provider di servizi personalizzato non fornisce automaticamente i servizi predefiniti elencati nella tabella. Se si usa un provider di servizi personalizzato e si richiede uno dei servizi indicati nella tabella, aggiungere i servizi necessari al nuovo provider di servizi.

## <a name="add-services-to-an-app"></a>Aggiungere servizi a un'app

### <a name="blazor-webassembly"></a>WebAssembly Blazor

Configurare i servizi per la raccolta di servizi dell'app nel metodo `Main` di *Program.cs*. Nell'esempio seguente, l'implementazione del `MyDependency` è registrata per `IMyDependency`:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<IMyDependency, MyDependency>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

Una volta compilato l'host, è possibile accedere ai servizi dall'ambito radice prima DI eseguire il rendering di tutti i componenti. Questa operazione può essere utile per eseguire la logica di inizializzazione prima del rendering del contenuto:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync();

        await host.RunAsync();
    }
}
```

L'host fornisce anche un'istanza di configurazione centrale per l'app. Basandosi sull'esempio precedente, l'URL del servizio meteo viene passato da un'origine di configurazione predefinita, ad esempio *appSettings. JSON*, a `InitializeWeatherAsync`:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync(
            host.Configuration["WeatherServiceUrl"]);

        await host.RunAsync();
    }
}
```

### <a name="blazor-server"></a>Server Blazor

Dopo aver creato una nuova app, esaminare il metodo `Startup.ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Al metodo `ConfigureServices` viene passato un <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, ovvero un elenco di oggetti del descrittore del servizio (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). I servizi vengono aggiunti fornendo descrittori del servizio alla raccolta di servizi. Nell'esempio seguente viene illustrato il concetto con l'interfaccia `IDataAccess` e la relativa implementazione concreta `DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

### <a name="service-lifetime"></a>Durata del servizio

I servizi possono essere configurati con le durate mostrate nella tabella seguente.

| Durata | Descrizione |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | le app webassembly Blazor attualmente non dispongono di un concetto di ambiti di. i servizi registrati `Scoped`si comportano come `Singleton` Services. Tuttavia, il modello di hosting del server Blazor supporta la durata `Scoped`. Nelle app di Blazor server una registrazione del servizio con ambito ha come ambito la *connessione*. Per questo motivo, è preferibile usare i servizi con ambito per i servizi che devono avere come ambito l'utente corrente, anche se l'obiettivo corrente è eseguire sul lato client nel browser. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | La creazione di una *singola istanza* del servizio. Tutti i componenti che richiedono un servizio di `Singleton` ricevono un'istanza dello stesso servizio. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Ogni volta che un componente ottiene un'istanza di un servizio `Transient` dal contenitore dei servizi, riceve una *nuova istanza* del servizio. |

Il sistema DI è basato sul sistema DI ASP.NET Core. Per altre informazioni, vedere <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Richiedere un servizio in un componente

Una volta aggiunti i servizi alla raccolta di servizi, inserire i servizi nei componenti usando la direttiva [\@Inject](xref:mvc/views/razor#inject) Razor. `@inject` dispone di due parametri:

* Digitare &ndash; il tipo di servizio da inserire.
* Proprietà &ndash; il nome della proprietà che riceve il servizio app inserito. La proprietà non richiede la creazione manuale. Il compilatore crea la proprietà.

Per altre informazioni, vedere <xref:mvc/views/dependency-injection>.

Usare più istruzioni `@inject` per inserire servizi diversi.

Nell'esempio riportato di seguito viene illustrato come usare `@inject`. Il servizio che implementa `Services.IDataAccess` viene inserito nel `DataRepository`della proprietà del componente. Si noti che il codice usa solo l'astrazione `IDataAccess`:

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

Internamente, la proprietà generata (`DataRepository`) utilizza l'attributo `InjectAttribute`. In genere, questo attributo non viene utilizzato direttamente. Se è necessaria una classe base per i componenti e le proprietà inserite sono necessarie anche per la classe base, aggiungere manualmente il `InjectAttribute`:

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

Nei componenti derivati dalla classe di base, la direttiva `@inject` non è obbligatoria. Il `InjectAttribute` della classe base è sufficiente:

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>Usare l'inserimento DI dipendenze nei servizi

Servizi complessi potrebbe richiedere servizi aggiuntivi. Nell'esempio precedente, `DataAccess` potrebbe richiedere il `HttpClient` servizio predefinito. `@inject` (o `InjectAttribute`) non è disponibile per l'uso nei servizi. È necessario usare invece l' *inserimento del costruttore* . I servizi necessari vengono aggiunti aggiungendo parametri al costruttore del servizio. Quando si crea il servizio, vengono riconosciuti i servizi richiesti nel costruttore e forniti DI conseguenza.

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

* È necessario che esista un costruttore i cui argomenti possono essere tutti soddisfatti da DI. Sono consentiti parametri aggiuntivi non analizzati da DI se specificano i valori predefiniti.
* Il costruttore applicabile deve essere *pubblico*.
* È necessario che esista un costruttore applicabile. In caso di ambiguità, viene generata un'eccezione.

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a>Classi di componenti di base dell'utilità per gestire un ambito DI

Nelle app ASP.NET Core, i servizi con ambito hanno in genere come ambito la richiesta corrente. Al termine della richiesta, tutti i servizi con ambito o temporaneo vengono eliminati dal sistema DI. Nelle app Blazor server l'ambito della richiesta dura per la durata della connessione client, che può comportare un tempo di permanenza dei servizi temporanei e con ambito maggiore del previsto. Nelle app Blazor webassembly i servizi registrati con una durata con ambito vengono considerati come singleton, quindi vivono più a lungo rispetto ai servizi con ambito nelle app ASP.NET Core tipiche.

Un approccio che limita la durata di un servizio nelle app Blazor è l'uso del tipo di `OwningComponentBase`. `OwningComponentBase` è un tipo astratto derivato da `ComponentBase` che consente di creare un ambito di che corrisponde alla durata del componente. Con questo ambito, è possibile usare i servizi DI i con una durata con ambito e fare in modo che siano attivi fino a quando il componente. Quando il componente viene eliminato definitivamente, vengono eliminati anche i servizi del provider di servizi con ambito. Questa operazione può essere utile per i servizi che:

* È necessario riutilizzarlo all'interno di un componente, perché la durata temporanea non è appropriata.
* Non devono essere condivise tra i componenti, perché la durata singleton non è appropriata.

Sono disponibili due versioni del tipo di `OwningComponentBase`:

* `OwningComponentBase` è un figlio astratto e disposable del tipo di `ComponentBase` con una proprietà `ScopedServices` protetta di tipo `IServiceProvider`. Questo provider può essere utilizzato per risolvere i servizi che hanno come ambito la durata del componente.

  I servizi DI inserimento nel componente usando `@inject` o l'`InjectAttribute` (`[Inject]`) non vengono creati nell'ambito del componente. Per utilizzare l'ambito del componente, è necessario risolvere i servizi utilizzando `ScopedServices.GetRequiredService` o `ScopedServices.GetService`. Tutti i servizi risolti utilizzando il provider di `ScopedServices` hanno le dipendenze fornite dallo stesso ambito.

  ```razor
  @page "/preferences"
  @using Microsoft.Extensions.DependencyInjection
  @inherits OwningComponentBase

  <h1>User (@UserService.Name)</h1>

  <ul>
      @foreach (var setting in SettingService.GetSettings())
      {
          <li>@setting.SettingName: @setting.SettingValue</li>
      }
  </ul>

  @code {
      private IUserService UserService { get; set; }
      private ISettingService SettingService { get; set; }

      protected override void OnInitialized()
      {
          UserService = ScopedServices.GetRequiredService<IUserService>();
          SettingService = ScopedServices.GetRequiredService<ISettingService>();
      }
  }
  ```

* `OwningComponentBase<T>` deriva da `OwningComponentBase` e aggiunge una `Service` di proprietà che restituisce un'istanza di `T` dal provider DI entità. Questo tipo è un modo pratico per accedere ai servizi con ambito senza usare un'istanza di `IServiceProvider` quando è presente un servizio primario richiesto dall'app dal contenitore DI inserimento delle dipendenze usando l'ambito del componente. La proprietà `ScopedServices` è disponibile, in modo che l'app possa ottenere i servizi di altri tipi, se necessario.

  ```razor
  @page "/users"
  @attribute [Authorize]
  @inherits OwningComponentBase<AppDbContext>

  <h1>Users (@Service.Users.Count())</h1>

  <ul>
      @foreach (var user in Service.Users)
      {
          <li>@user.UserName</li>
      }
  </ul>
  ```

## <a name="use-of-entity-framework-dbcontext-from-di"></a>Uso di Entity Framework DbContext da DI

Un tipo di servizio comune da recuperare da un in app Web è Entity Framework (EF) `DbContext` oggetti. Per impostazione predefinita, la registrazione dei servizi EF con `IServiceCollection.AddDbContext` aggiunge il `DbContext` come servizio con ambito. La registrazione come servizio con ambito può causare problemi nelle app Blazor perché causa la lunga durata delle istanze di `DbContext` e la condivisione nell'app. `DbContext` non è thread-safe e non deve essere usato simultaneamente.

A seconda dell'app, l'uso di `OwningComponentBase` per limitare l'ambito di un `DbContext` a un singolo componente *può* risolvere il problema. Se un componente non usa un `DbContext` in parallelo, la derivazione del componente da `OwningComponentBase` e il recupero del `DbContext` da `ScopedServices` è sufficiente perché garantisce quanto segue:

* I componenti separati non condividono un `DbContext`.
* Il `DbContext` risiede solo fino a quando il componente dipende da esso.

Se un singolo componente può usare contemporaneamente un `DbContext` (ad esempio, ogni volta che un utente seleziona un pulsante), anche usando `OwningComponentBase` non si evitano problemi con le operazioni EF simultanee. In tal caso, usare un `DbContext` diverso per ogni operazione EF logica. Usare uno degli approcci seguenti:

* Creare il `DbContext` direttamente usando `DbContextOptions<TContext>` come argomento, che può essere recuperato da DI ed è thread-safe.

    ```razor
    @page "/example"
    @inject DbContextOptions<AppDbContext> DbContextOptions

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = new AppDbContext(DbContextOptions))
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

* Registrare il `DbContext` nel contenitore del servizio con una durata temporanea:
  * Quando si registra il contesto, utilizzare `ServiceLifetime.Transient`. Il metodo di estensione `AddDbContext` accetta due parametri facoltativi di tipo `ServiceLifetime`. Per utilizzare questo approccio, è necessario `ServiceLifetime.Transient`solo il parametro `contextLifetime`. `optionsLifetime` possibile mantenete il valore predefinito di `ServiceLifetime.Scoped`.

    ```csharp
    services.AddDbContext<AppDbContext>(options =>
         options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")),
         ServiceLifetime.Transient);
    ```  

  * I `DbContext` temporanei possono essere inseriti normalmente (usando `@inject`) in componenti che non eseguono più operazioni EF in parallelo. Quelli che possono eseguire contemporaneamente più operazioni EF possono richiedere oggetti `DbContext` distinti per ogni operazione parallela usando `IServiceProvider.GetRequiredService`.

    ```razor
    @page "/example"
    @using Microsoft.Extensions.DependencyInjection
    @inject IServiceProvider ServiceProvider

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = ServiceProvider.GetRequiredService<AppDbContext>())
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
