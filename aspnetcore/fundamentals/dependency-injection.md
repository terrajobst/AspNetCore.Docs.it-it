---
title: Inserimento delle dipendenze in ASP.NET Core
author: guardrex
description: Informazioni su come ASP.NET Core implementa l'inserimento delle dipendenze e su come usare questa funzionalità.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: da6ddf1f0efd164a58f017ff55ce216bbefa7cc6
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068323"
---
# <a name="dependency-injection-in-aspnet-core"></a>Inserimento delle dipendenze in ASP.NET Core

Di [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) e [Luke Latham](https://github.com/guardrex)

ASP.NET Core supporta il modello progettuale software per l'inserimento delle dipendenze, ovvero una tecnica per ottenere l'[inversione del controllo (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) tra classi e relative dipendenze.

Per altre informazioni specifiche per l'inserimento delle dipendenze all'interno di controller MVC, vedere <xref:mvc/controllers/dependency-injection>.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="overview-of-dependency-injection"></a>Panoramica dell'inserimento delle dipendenze

Una *dipendenza* è qualsiasi oggetto richiesto da un altro oggetto. Esaminare la classe `MyDependency` seguente con un metodo `WriteMessage` da cui dipendono altre classi in un'app:

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

È possibile creare un'istanza della classe `MyDependency` per rendere il metodo `WriteMessage`disponibile per una classe. La classe `MyDependency` è una dipendenza della classe `IndexModel`:

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

La classe crea e dipende direttamente dall'istanza `MyDependency`. Le dipendenze del codice (come nell'esempio precedente) sono problematiche e devono essere evitate per i motivi seguenti:

* Per sostituire `MyDependency` con un'implementazione diversa, la classe deve essere modificata.
* Se `MyDependency` ha dipendenze, devono essere configurate dalla classe. In un progetto di grandi dimensioni con più classi che dipendono da `MyDependency`, il codice di configurazione diventa sparso in tutta l'app.
* È difficile eseguire unit test di questa implementazione. L'app dovrebbe usare una classe `MyDependency` fittizia o stub, ma ciò non è possibile con questo approccio.

L'inserimento delle dipendenze consente di risolvere questi problemi tramite:

* L'uso di un'interfaccia per astrarre l'implementazione delle dipendenze.
* La registrazione della dipendenza in un contenitore di servizi. ASP.NET Core offre il contenitore di servizi predefinito [IServiceProvider](/dotnet/api/system.iserviceprovider). I servizi vengono registrati nel metodo `Startup.ConfigureServices` dell'app.
* L'*inserimento* del servizio nel costruttore della classe in cui viene usato. Il framework si assume la responsabilità della creazione di un'istanza della dipendenza e della sua eliminazione quando non è più necessaria.

Nell'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), l'interfaccia `IMyDependency` definisce un metodo che il servizio fornisce all'app:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

Questa interfaccia viene implementata da un tipo concreto, `MyDependency`:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

`MyDependency` richiede un'istanza di [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) nel relativo costruttore. Non è insolito usare l'inserimento delle dipendenze in modo concatenato. Ogni dipendenza richiesta richiede a sua volta le proprie dipendenze. Il contenitore risolve le dipendenze nel grafico e restituisce il servizio completamente risolto. Il set di dipendenze che devono essere risolte viene generalmente chiamato *albero delle dipendenze* o *grafico dipendenze* o *grafico degli oggetti*.

`IMyDependency` e `ILogger<TCategoryName>` devono essere registrati nel contenitore di servizi. `IMyDependency` viene registrato in `Startup.ConfigureServices`. `ILogger<TCategoryName>` viene registrato dall'infrastruttura di astrazioni di registrazione, pertanto si tratta di un [servizio fornito dal framework](#framework-provided-services) registrato per impostazione predefinita dal framework.

Il contenitore risolve `ILogger<TCategoryName>` avvalendosi di [tipi aperti (generici)](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminando l'esigenza di registrare ogni singolo [tipo costruito (generico)](/dotnet/csharp/language-reference/language-specification/types#constructed-types):

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

Nell'app di esempio, il servizio `IMyDependency` viene registrato con il tipo concreto `MyDependency`. La registrazione definisce come ambito della durata di servizio la durata di una singola richiesta. Le [durate dei servizi](#service-lifetimes) sono descritte più avanti in questo argomento.

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> Ogni metodo di estensione `services.Add{SERVICE_NAME}` aggiunge (e potenzialmente configura) i servizi. Ad esempio, `services.AddMvc()` aggiunge i servizi richiesti da Razor Pages e MVC. È consigliabile che le app seguano questa convenzione. Inserire i metodi di estensione nello spazio dei nomi [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) per incapsulare i gruppi di registrazioni di servizio.

Se il costruttore del servizio richiede un [tipo incorporato](/dotnet/csharp/language-reference/keywords/built-in-types-table), ad esempio `string`, è possibile inserirlo usando la [configurazione](xref:fundamentals/configuration/index) o il [modello di opzioni](xref:fundamentals/configuration/options):

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

È richiesta un'istanza del servizio tramite il costruttore di una classe in cui il servizio viene usato e assegnato a un campo privato. Il campo viene usato per accedere al servizio in base alle esigenze per l'intera classe.

Nell'app di esempio, viene richiesta l'istanza `IMyDependency` usata per chiamare il metodo `WriteMessage` del servizio:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a>Servizi forniti dal framework

Il metodo `Startup.ConfigureServices` è responsabile della definizione dei servizi usati dall'app, incluse le funzionalità di piattaforma come Entity Framework Core e ASP.NET Core MVC. Inizialmente, l'interfaccia `IServiceCollection` fornita a `ConfigureServices` ha i seguenti servizi definiti (in base alla [modalità di configurazione dell'host](xref:fundamentals/index#host)):

| Tipo di servizio | Durata |
| ------------ | -------- |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Temporaneo |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | Temporaneo |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Temporaneo |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | Temporaneo |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |

Quando è disponibile un metodo di estensione della raccolta di servizi per la registrazione di un servizio (e dei servizi dipendenti, se necessario), la convenzione consiste nell'usare un singolo metodo di estensione `Add{SERVICE_NAME}` per registrare tutti i servizi richiesti da tale servizio. Il codice seguente è un esempio di come aggiungere servizi aggiuntivi al contenitore usando i metodi di estensione [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity) e [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddMvc();
}
```

Per altre informazioni, vedere la [classe ServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) nella documentazione dell'API.

## <a name="service-lifetimes"></a>Durate dei servizi

Scegliere una durata appropriata per ogni servizio registrato. I servizi ASP.NET Core possono essere configurati con le impostazioni di durata seguenti:

**Temporaneo**

I servizi con durata temporanea vengono creati ogni volta che vengono richiesti dal contenitore dei servizi. Questa impostazione di durata è consigliata per i servizi semplici senza stati.

**Con ambito**

I servizi con durata con ambito vengono creati una volta per ogni richiesta client (connessione).

> [!WARNING]
> Quando si usa un servizio con ambito in un middleware, inserire il servizio nel metodo `Invoke` o `InvokeAsync`. Evitare l'inserimento tramite il costruttore, che impone al servizio il comportamento singleton. Per ulteriori informazioni, vedere <xref:fundamentals/middleware/index>.

**Singleton**

I servizi con durata singleton vengono creati la prima volta che vengono richiesti (o quando si esegue `ConfigureServices` e viene specificata un'istanza con la registrazione del servizio). Tutte le richieste successive usano la stessa istanza. Se l'app richiede un comportamento singleton, è consigliabile consentire al contenitore del servizio di gestire la durata del servizio. Non implementare lo schema progettuale singleton e fornire codice utente per gestire la durata dell'oggetto nella classe.

> [!WARNING]
> Può essere pericoloso risolvere un servizio con ambito da un singleton. Ciò potrebbe causare uno stato non corretto per il servizio durante l'elaborazione delle richieste successive.

### <a name="constructor-injection-behavior"></a>Comportamento dell'inserimento del costruttore

I servizi possono essere risolti con due meccanismi:

* `IServiceProvider`
* [ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Consente la creazione di oggetti senza registrazione del servizio nel contenitore di inserimento delle dipendenze. `ActivatorUtilities` viene usato con astrazioni esposte all'utente, ad esempio helper tag, controller MVC e strumenti di associazione di modelli.

I costruttori possono accettare argomenti non forniti tramite l'inserimento di dipendenze, ma gli argomenti devono assegnare valori predefiniti.

Quando i servizi vengono risolti da `IServiceProvider` o `ActivatorUtilities`, l'inserimento del costruttore richiede un costruttore *pubblico*.

Quando i servizi vengono risolti da `ActivatorUtilities`, l'inserimento del costruttore richiede l'esistenza di un solo costruttore applicabile. Sebbene siano supportati gli overload dei costruttori, può essere presente un solo overload i cui argomenti possono essere tutti specificati tramite l'inserimento delle dipendenze.

## <a name="entity-framework-contexts"></a>Contesti di Entity Framework

I contesti di Entity Framework vengono in genere aggiunti al contenitore di servizi usando la [durata con ambito](#service-lifetimes) perché le operazioni di database delle app Web hanno di solito come ambito la richiesta client. La durata predefinita è con ambito se non viene specificata una durata con un overload <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*> quando si registra il contesto del database. I servizi con una durata specificata non devono usare un contesto di database con una durata più breve rispetto al servizio.

## <a name="lifetime-and-registration-options"></a>Opzioni di durata e di registrazione

Per illustrare la differenza tra le opzioni di durata e registrazione, si considerino le interfacce seguenti che rappresentano le attività come un'operazione con un identificatore univoco, `OperationId`. A seconda del modo in cui la durata di un servizio operativo viene configurata per le interfacce seguenti, il contenitore fornisce la stessa istanza o un'istanza diversa del servizio quando richiesto da una classe:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

Le interfacce vengono implementate nella classe `Operation`. Il costruttore `Operation` genera un GUID se non ne viene fornito uno:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

Viene registrato un `OperationService` che dipende da ognuno degli altri tipi `Operation`. Quando viene richiesto `OperationService` tramite l'inserimento delle dipendenze, riceve una nuova istanza di ogni servizio o un'istanza esistente in base alla durata del servizio dipendente.

* Se i servizi temporanei vengono creati quando vengono richiesti dal contenitore, `OperationId` del servizio `IOperationTransient` è diverso da `OperationId` di `OperationService`. `OperationService` riceve una nuova istanza della classe `IOperationTransient`. La nuova istanza restituisce un `OperationId` diverso.
* Se i servizi con ambito vengono creati per ogni richiesta client, `OperationId` del servizio `IOperationScoped` corrisponde a `OperationService` all'interno di una richiesta client. In tutte le richieste client entrambi i servizi condividono un valore `OperationId` diverso.
* Se i servizi singleton e con istanza singleton vengono creati una sola volta e usati per tutte le richieste client e tutti i servizi, `OperationId` rimane costante in tutte le richieste di servizio.

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

In `Startup.ConfigureServices`, ogni tipo viene aggiunto al contenitore in base alla durata denominata:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

Il servizio `IOperationSingletonInstance` usa un'istanza specifica con un ID noto di `Guid.Empty`. È chiaro quando questo tipo è in uso (il relativo GUID è composto da tutti zero).

L'app di esempio dimostra le durate degli oggetti all'interno e tra le singole richieste. L'`IndexModel` dell'app di esempio richiede ogni genere di tipo `IOperation` e `OperationService`. La pagina visualizza quindi tutti i valori `OperationId` della classe del modello di pagina e del servizio tramite assegnazioni di proprietà:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

L'output seguente mostra i risultati delle due richieste:

**Prima richiesta:**

Operazioni del controller:

Transient: d233e165-f417-469b-a866-1cf1935d2518  
Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

`OperationService` Operazioni di OperationService:

Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64  
Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

**Seconda richiesta:**

Operazioni del controller:

Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0  
Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

`OperationService` Operazioni di OperationService:

Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf  
Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Instance: 00000000-0000-0000-0000-000000000000

Osservare i valori `OperationId` che variano all'interno della richiesta e da una richiesta e l'altra:

* Gli oggetti *temporanei* sono sempre diversi. Il valore `OperationId` temporaneo per la prima e la seconda richiesta client è diverso per entrambe le operazioni `OperationService` e in tutte le richieste client. Viene fornita una nuova istanza per ogni richiesta di servizio e richiesta client.
* Gli oggetti *con ambito* sono gli stessi all'interno di una richiesta client, ma variano nelle diverse richieste client.
* Gli oggetti *singleton* sono gli stessi per ogni oggetto e ogni richiesta, indipendentemente dal fatto che venga fornita un'istanza `Operation` in `ConfigureServices`.

## <a name="call-services-from-main"></a>Chiamare i servizi da main

Creare un [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) con [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) per risolvere un servizio con ambito nell'ambito dell'app. Questo approccio è utile per l'accesso a un servizio con ambito all'avvio e per l'esecuzione di attività di inizializzazione. L'esempio seguente indica come ottenere un contesto per `MyScopedService` in `Program.Main`:

```csharp
public static void Main(string[] args)
{
    var host = CreateWebHostBuilder(args).Build();

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a>Convalida dell'ambito

Quando l'app viene eseguita nell'ambiente di sviluppo, il provider di servizi predefinito esegue controlli per verificare che:

* I servizi con ambito non vengano risolti direttamente o indirettamente dal provider di servizi radice.
* I servizi con ambito non vengano inseriti direttamente o indirettamente in singleton.

Il provider di servizi radice venga creato con la chiamata di [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider). La durata del provider di servizi radice corrisponde alla durata dell'app/server quando il provider viene avviato con l'app e viene eliminato alla chiusura dell'app.

I servizi con ambito vengono eliminati dal contenitore che li ha creati. Se un servizio con ambito viene creato nel contenitore radice, la durata del servizio viene di fatto convertita in singleton, perché il servizio viene eliminato solo dal contenitore radice alla chiusura dell'app/server. La convalida degli ambiti servizio rileva queste situazioni nel contesto della chiamata di `BuildServiceProvider`.

Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host#scope-validation>.

## <a name="request-services"></a>Servizi di richiesta

I servizi disponibili all'interno di una richiesta ASP.NET Core da `HttpContext` vengono esposti tramite la raccolta [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices).

I servizi di richiesta rappresentano i servizi configurati e richiesti come parte dell'app. Quando gli oggetti specificano dipendenze, le dipendenze sono soddisfatte dai tipi individuati in `RequestServices`, non in `ApplicationServices`.

In generale, l'app non deve usare queste proprietà direttamente. Richiedere invece i tipi richiesti dalle classi tramite i costruttori di classi e consentire al framework di inserire le dipendenze. Si ottengono classi più facili da testare.

> [!NOTE]
> È consigliabile richiedere le dipendenze come parametri del costruttore per l'accesso alla raccolta `RequestServices`.

## <a name="design-services-for-dependency-injection"></a>Progettare i servizi per l'inserimento di dipendenze

Le procedure consigliate prevedono di:

* Progettare i servizi per usare l'inserimento delle dipendenze per ottenere le relative dipendenze.
* Evitare chiamate di metodo statico, con stato.
* Evitare la creazione diretta di istanze delle classi dipendenti all'interno di servizi. La creazione diretta di istanze associa il codice a una determinata implementazione.
* Fare in modo che le classi siano piccole, con factoring corretto e facili da testare.

Se una classe sembra avere troppe dipendenze inserite, ciò significa in genere che la classe ha un numero eccessivo di responsabilità e sta violando il [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility) (principio di responsabilità singola). Tentare di effettuare il refactoring della classe spostando alcune delle responsabilità in una nuova classe. Tenere presente che le classi del modello di pagina Razor Pages e le classi del controller MVC devono essere incentrate sulle problematiche dell'interfaccia utente. Di conseguenza, le regole business e i dettagli di implementazione di accesso ai dati devono essere inseriti in classi appropriate per queste [problematiche separate](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).

### <a name="disposal-of-services"></a>Eliminazione dei servizi

Il contenitore chiama `Dispose` per i tipi `IDisposable` creati. Se un'istanza viene aggiunta al contenitore dal codice utente, non viene eliminata automaticamente.

```csharp
// Services that implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // The container creates the following instances and disposes them automatically:
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // The container doesn't create the following instances, so it doesn't dispose of
    // the instances automatically:
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

## <a name="default-service-container-replacement"></a>Sostituzione del contenitore di servizi predefinito

Il contenitore di servizi predefinito è progettato per soddisfare le esigenze del framework e della maggior parte delle app. È consigliabile usare il contenitore predefinito, a meno che non sia necessaria una funzionalità specifica non supportata. Alcune delle funzionalità supportate nei contenitori di terze parti non trovati nel contenitore predefinito:

* Inserimento di proprietà
* Inserimento in base al nome
* Contenitori figlio
* Gestione della durata personalizzata
* `Func<T>` Supporto di Func<T> per l'inizializzazione differita

Vedere il [file Dependency Injection readme.md](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) per un elenco di alcuni dei contenitori che supportano gli adattatori.

L'esempio seguente consente di sostituire il contenitore predefinito con [Autofac](https://autofac.org/):

* Installare i pacchetti contenitore appropriati:

  * [Autofac](https://www.nuget.org/packages/Autofac/)
  * [Autofac.Extensions.DependencyInjection](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* Configurare il contenitore in `Startup.ConfigureServices` e restituire `IServiceProvider`:

    ```csharp
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        // Add other framework services

        // Add Autofac
        var containerBuilder = new ContainerBuilder();
        containerBuilder.RegisterModule<DefaultModule>();
        containerBuilder.Populate(services);
        var container = containerBuilder.Build();
        return new AutofacServiceProvider(container);
    }
    ```

    Per usare un contenitore di terze parti, `Startup.ConfigureServices` deve restituire `IServiceProvider`.

* Configurare Autofac in `DefaultModule`:

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

In fase di esecuzione, Autofac viene usato per risolvere i tipi e inserire le dipendenze. Per altre informazioni sull'uso di Autofac con ASP.NET Core, vedere la [documentazione di Autofac](https://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Thread safety

Creare servizi singleton thread-safe. Se un servizio singleton ha una dipendenza in un servizio temporaneo, è necessario che anche il servizio temporaneo sia thread-safe, a seconda di come viene usato dal singleton.

Non è necessario che il metodo factory del singolo servizio, ad esempio il secondo argomento per [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), sia thread-safe. Come nel caso di un costruttore di tipo (`static`), è garantito che venga chiamato una sola volta da un singolo thread.

## <a name="recommendations"></a>Suggerimenti

* `async/await` La risoluzione del servizio basata su async/await e `Task` non è supportata. Dato che C# non supporta i costruttori asincroni, il modello consigliato consiste nell'usare i metodi asincroni dopo avere risolto in modo sincrono il servizio.

* Evitare di archiviare i dati e la configurazione direttamente nel contenitore del servizio. Ad esempio, il carrello acquisti di un utente non dovrebbe in genere essere aggiunto al contenitore del servizio. La configurazione deve usare il [modello di opzioni](xref:fundamentals/configuration/options). Analogamente, evitare gli oggetti "contenitori di dati" che hanno la sola funzione di consentire l'accesso ad altri oggetti. È preferibile richiedere l'elemento effettivo tramite inserimento delle dipendenze.

* Evitare l'accesso statico ai servizi (ad esempio, la tipizzazione statica [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) da usare in altre posizioni).

* Evitare di usare il *modello di localizzatore del servizio*. Ad esempio, non richiamare <xref:System.IServiceProvider.GetService*> per ottenere un'istanza del servizio quando è invece possibile usare l'inserimento delle dipendenze:

  **Non corretto:**

  ```csharp
  public void MyMethod()
  {
      var options = 
          _services.GetService<IOptionsMonitor<MyOptions>>();
      var option = options.CurrentValue.Option;

      ...
  }
  ```

  **Corretto**:

  ```csharp
  private readonly MyOptions _options;

  public MyClass(IOptionsMonitor<MyOptions> options)
  {
      _options = options.CurrentValue;
  }

  public void MyMethod()
  {
      var option = _options.Option;

      ...
  }
  ```

* Un'altra variazione del localizzatore del servizio da evitare è inserire una factory che risolve le dipendenze in fase di esecuzione. Queste procedure combinano le strategie di [inversione del controllo](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion).

* Evitare l'accesso statico a `HttpContext` (ad esempio [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).

È tuttavia possibile che in alcuni casi queste raccomandazioni debbano essere ignorate. Le eccezioni sono rare e principalmente si tratta di casi speciali all'interno del framework stesso.

L'inserimento di dipendenze è un'*alternativa* ai modelli di accesso agli oggetti statici/globali. Se l'inserimento di dipendenze viene usato con l'accesso agli oggetti statico i vantaggi non saranno evidenti.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [Scrittura di codice pulito in ASP.NET Core con inserimento delle dipendenze (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Explicit Dependencies Principle (Principio delle dipendenze esplicite)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler) (Contenitori di inversione del controllo e modello di inserimento delle dipendenze)](https://www.martinfowler.com/articles/injection.html)
* [How to register a service with multiple interfaces in ASP.NET Core DI (Come registrare un servizio con più interfacce nell'inserimento delle dipendenze di ASP.NET Core)](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
