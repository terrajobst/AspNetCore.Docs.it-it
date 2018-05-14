---
title: Inserimento delle dipendenze in ASP.NET Core
author: ardalis
description: Informazioni su come ASP.NET Core implementa l'inserimento delle dipendenze e su come usare questa funzionalità.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 8a105f835dddfcd0e9f32059e644f60dc1fdbbe1
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="dependency-injection-in-aspnet-core"></a>Inserimento delle dipendenze in ASP.NET Core

<a name="fundamentals-dependency-injection"></a>

[Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)

ASP.NET Core è stato interamente progettato per supportare e usare l'inserimento delle dipendenze. Le applicazioni ASP.NET Core possono usare servizi di framework incorporati inserendoli nei metodi nella classe Startup, mentre anche i servizi di applicazione possono essere configurati per l'inserimento. Il contenitore di servizi predefinito incluso in ASP.NET Core offre un insieme di funzionalità di base e non è stato progettato per sostituire altri contenitori.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>Che cos'è l'inserimento delle dipendenze?

L'inserimento delle dipendenze (Dependency Injection, DI)è una tecnica per ottenere un regime di controllo libero tra gli oggetti e i relativi collaboratori o dipendenze. Anziché creare direttamente un'istanza dei collaboratori o usare riferimenti statici, gli oggetti necessari a una classe per eseguire le azioni vengono forniti alla classe in qualche modo. Nella maggior parte dei casi le classi dichiarano le loro dipendenze tramite il costruttore consentendo di seguire il [principio delle dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/). Questo approccio è chiamato "inserimento del costruttore".

Quando vengono progettate tenendo presente l'inserimento delle dipendenze, le classi risultano accoppiate in modo più libero poiché non hanno dipendenze dirette e hardcoded sui relativi collaboratori. Ciò segue il [principio di inversione delle dipendenze](http://deviq.com/dependency-inversion-principle/) che afferma che *"i moduli di alto livello non devono dipendere da quelli di basso livello; entrambi devono dipendere da astrazioni".* Anziché fare riferimento a implementazioni specifiche, le classi richiedono astrazioni (in genere `interfaces`) che vengono fornite quando viene costruita la classe. Anche l'estrazione delle dipendenze nelle interfacce e la specifica delle implementazioni di queste interfacce come parametri sono un esempio dello [schema progettuale di strategia](http://deviq.com/strategy-design-pattern/).

Quando un sistema è progettato per l'utilizzo dell'inserimento delle dipendenze, con numerose classi che richiedono le relative dipendenze tramite il costruttore (o le proprietà), è utile avere una classe dedicata alla creazione di queste classi con le relative dipendenze associate. Queste classi vengono chiamate *contenitori*, o più specificatamente contenitori di [inversione del controllo (IoC)](http://deviq.com/inversion-of-control/) o contenitori di inserimento delle dipendenze. Un contenitore è essenzialmente una factory che è responsabile di fornire le istanze dei tipi richiesti. Se un determinato tipo ha dichiarato di avere dipendenze e il contenitore è stato configurato per specificare i tipi di dipendenze, il tipo creerà le dipendenze come parte del processo di creazione dell'istanza richiesta. In questo modo è possibile fornire alle classi grafici delle dipendenze complessi senza la necessità di costruire oggetti hardcoded. Oltre a creare gli oggetti con le relative dipendenze, i contenitori gestiscono in genere la durata degli oggetti all'interno dell'applicazione.

ASP.NET Core include un semplice contenitore incorporato (rappresentato dall'interfaccia `IServiceProvider`) che supporta l'inserimento del costruttore per impostazione predefinita, mentre ASP.NET rende disponibili determinati servizi tramite l'inserimento delle dipendenze. Il contenitore di ASP.NET fa riferimento ai tipi gestiti come *servizi*. Nella parte restante di questo articolo il termine *servizi* fa riferimento ai tipi gestiti dal contenitore IoC di ASP.NET Core. I servizi del contenitore incorporato vengono configurati nel metodo `ConfigureServices` nella classe `Startup` dell'applicazione.

> [!NOTE]
> Martin Fowler ha scritto l'articolo [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (Contenitori di inversione del controllo e modello di inserimento delle dipendenze). Una descrizione dell'[inserimento delle dipendenze](https://msdn.microsoft.com/library/hh323705.aspx) è disponibile anche in Microsoft Patterns and Practices.

> [!NOTE]
> Questo articolo descrive l'inserimento delle dipendenze poiché si applica a tutte le applicazioni ASP.NET. L'inserimento delle dipendenze all'interno di controller MVC è descritto in [Inserimento di dipendenze e controller](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Comportamento dell'inserimento del costruttore

Per eseguire l'inserimento del costruttore è necessario che il costruttore sia *pubblico*. In caso contrario, l'applicazione genera `InvalidOperationException`:

> Impossibile trovare un costruttore adatto per il tipo 'TipoPersonale'. Verificare che il tipo sia concreto e che i servizi siano registrati per tutti i parametri di un costruttore pubblico.

L'inserimento del costruttore richiede l'esistenza di un solo costruttore applicabile. Sebbene siano supportati gli overload dei costruttori, può essere presente un solo overload i cui argomenti possono essere tutti specificati tramite l'inserimento delle dipendenze. Se sono presenti più overload, l'app genererà `InvalidOperationException`:

> Nel tipo 'TipoPersonale' sono stati individuati più costruttori che accettano tutti i tipi di argomenti specificati. Deve essere presente solo un costruttore applicabile.

I costruttori possono accettare argomenti non forniti tramite l'inserimento di dipendenze, ma devono supportare i valori predefiniti. Ad esempio:

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a>Uso dei servizi forniti dal framework

Il metodo `ConfigureServices` nella classe `Startup` è responsabile della definizione dei servizi che verranno usati dall'applicazione, incluse le funzionalità di piattaforma come Entity Framework Core e ASP.NET Core MVC. Inizialmente, l'interfaccia `IServiceCollection` fornita a `ConfigureServices` ha i seguenti servizi definiti (in base alla [modalità di configurazione dell'host](xref:fundamentals/hosting)):

| Tipo di servizio | Durata |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Temporaneo |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Temporaneo |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | Temporaneo |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | Temporaneo |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |

L'esempio seguente illustra come aggiungere servizi aggiuntivi al contenitore usando metodi di estensione come `AddDbContext`, `AddIdentity` e `AddMvc`.

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Le funzionalità e il middleware forniti da ASP.NET, ad esempio MVC, seguono una convenzione per l'uso di un singolo metodo di estensione Add*ServiceName* per registrare tutti i servizi richiesti dalla funzionalità.

> [!TIP]
> È possibile richiedere determinati servizi forniti dal framework all'interno di metodi `Startup` tramite i relativi elenchi di parametri. Per informazioni dettagliate, vedere [Avvio dell'applicazione](startup.md).

## <a name="registering-services"></a>Registrazione di servizi

È possibile registrare i servizi dell'applicazione come descritto di seguito. Il primo tipo generico rappresenta il tipo (in genere un'interfaccia) che verrà richiesto dal contenitore. Il secondo tipo generico rappresenta il tipo concreto di cui verrà creata un'istanza mediante il contenitore e che verrà usato per soddisfare le richieste.

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Ogni metodo di estensione `services.Add<ServiceName>` aggiunge (e potenzialmente configura) i servizi. Ad esempio, `services.AddMvc()` aggiunge i servizi richiesti da MVC. È consigliabile attenersi a questa convenzione, inserendo i metodi di estensione nello spazio dei nomi `Microsoft.Extensions.DependencyInjection` per incapsulare i gruppi di registrazioni di servizi.

Il metodo `AddTransient` viene usato per eseguire il mapping di tipi astratti a servizi concreti di cui viene creata un'istanza separatamente per ogni oggetto che lo richiede. Viene definita *durata* del servizio. Ulteriori opzioni di durata sono descritte di seguito. È importante scegliere una durata appropriata per ognuno dei servizi registrati. È necessario fornire una nuova istanza del servizio a ogni classe che la richiede? È necessario usare un'unica istanza in una richiesta Web specificata? Oppure è necessario usare una sola istanza per la durata dell'applicazione?

Nell'esempio di questo articolo è presente un controller semplice che visualizza i nomi dei caratteri denominato `CharactersController`. Il metodo `Index` visualizza l'elenco di caratteri corrente memorizzati nell'applicazione e, se necessario, inizializza la raccolta con alcuni caratteri. Si noti che sebbene questa applicazione usi Entity Framework Core e la classe `ApplicationDbContext` per la relativa persistenza, non appare nel controller. Al contrario, il meccanismo di accesso ai dati specifici è stato astratto da un'interfaccia, `ICharacterRepository`, che segue il [modello di repository](http://deviq.com/repository-pattern/). Un'istanza di `ICharacterRepository` viene richiesta tramite il costruttore e assegnata a un campo privato che viene quindi usato per accedere ai caratteri in base alle esigenze.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

`ICharacterRepository` definisce i due metodi necessari al controller per l'uso delle istanze `Character`.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Questa interfaccia viene a sua volta implementata da un tipo concreto, `CharacterRepository`, usato durante il runtime.

> [!NOTE]
> La modalità di utilizzo dell'inserimento delle dipendenze con la classe `CharacterRepository` è un modello generale che è possibile seguire per tutti i servizi dell'applicazione, non solo nei "repository" o nelle classi di accesso ai dati.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Si noti che `CharacterRepository` richiede `ApplicationDbContext` nel costruttore. Non è raro che l'inserimento delle dipendenze venga usato in modo concatenato e che ogni dipendenza richiesta richieda a sua volta le proprie dipendenze. Il contenitore è responsabile della risoluzione di tutte le dipendenze nel grafico e della restituzione del servizio completamente risolto.

> [!NOTE]
> La creazione dell'oggetto richiesto, di tutti gli oggetti che l'oggetto richiede e di tutti gli oggetti richiesti da quest'ultimi viene a volte chiamata *grafico oggetto*. Analogamente, l'insieme di dipendenze che devono essere risolte viene generalmente chiamato *albero delle dipendenze* o *grafico dipendenze*.

In questo caso `ICharacterRepository` e `ApplicationDbContext` devono essere entrambi registrati nel contenitore di servizi in `ConfigureServices` in `Startup`. `ApplicationDbContext` è configurato con la chiamata al metodo di estensione `AddDbContext<T>`. Il codice seguente illustra la registrazione del tipo `CharacterRepository`.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Aggiungere i contesti di Entity Framework al contenitore di servizi usando la durata `Scoped`. Questa operazione viene eseguita automaticamente se si usano i metodi helper come illustrato di seguito. I repository che useranno Entity Framework devono usare la stessa durata.

> [!WARNING]
> L'unico rischio da tenere presente è rappresentato dalla risoluzione di un servizio `Scoped` da un singleton. È probabile che in tal caso il servizio abbia uno stato non corretto durante l'elaborazione delle richieste successive.

I servizi che hanno dipendenze devono registrarle nel contenitore. Se il costruttore di un servizio richiede una primitiva, ad esempio `string`, è possibile inserirla usando la [configurazione](xref:fundamentals/configuration/index) e il [modello di opzioni](xref:fundamentals/configuration/options).

## <a name="service-lifetimes-and-registration-options"></a>Opzioni di durata e di registrazione dei servizi

I servizi ASP.NET possono essere configurati con le impostazioni di durata seguenti:

**Temporaneo**

I servizi con durata temporanea vengono creati ogni volta che vengono richiesti. Questa impostazione di durata è consigliata per i servizi semplici senza stati.

**Con ambito**

I servizi con durata con ambito vengono creati una volta per ogni richiesta.

> [!WARNING]
> Se si usa un servizio con ambito in un middleware, inserire il servizio nel metodo `Invoke` o `InvokeAsync`. Evitare l'inserimento tramite il costruttore, che impone al servizio il comportamento singleton.

**Singleton**

I servizi con durata singleton vengono creati la prima volta che vengono richiesti (o quando viene eseguito `ConfigureServices` se si specifica un'istanza). La stessa istanza viene usata in seguito da tutte le richieste successive. Se l'applicazione richiede un comportamento singleton, è consigliabile consentire al contenitore di servizi di gestire la durata del servizio anziché implementare lo schema progettuale singleton e gestire la durata dell'oggetto nella classe.

I servizi possono essere registrati nel contenitore in diversi modi. In precedenza è già stato descritto come registrare l'implementazione di un servizio con un determinato tipo specificando il tipo concreto da usare. È anche possibile specificare una factory che verrà usata in seguito per creare l'istanza su richiesta. Il terzo approccio consiste nello specificare direttamente l'istanza del tipo da usare. In tal caso il contenitore non tenterà di creare un'istanza (né eliminerà l'istanza).

Per illustrare la differenza tra queste opzioni di durata e registrazione, si consideri un'interfaccia semplice che rappresenta una o più attività come *operazione* con un identificatore univoco, `OperationId`. A seconda di come viene configurata la durata di questo servizio, il contenitore offre la stessa istanza o istanze diverse del servizio alla classe richiedente. Per indicare la durata richiesta, verrà creato un singolo tipo per ogni opzione di durata:

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Queste interfacce vengono implementate usando un'unica classe, `Operation`, che accetta `Guid` nel costruttore o usa un nuovo `Guid` se non ne viene specificato uno.

Successivamente, in `ConfigureServices`, ogni tipo viene aggiunto al contenitore in base alla durata denominata:

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Si noti che poiché il servizio `IOperationSingletonInstance` usa un'istanza specifica con un ID noto di `Guid.Empty`, è facile individuare quando il tipo è in uso (il GUID includerà solo zeri). È stato registrato anche `OperationService` che dipende da ognuno degli altri tipi `Operation`, in modo che sia possibile individuare all'interno di una richiesta se il servizio ottiene la stessa istanza del controller o una nuova istanza per ogni tipo di operazione. Il servizio si limita a esporre le relative dipendenze come proprietà in modo che possano essere incluse nella visualizzazione.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Per indicare le durata degli oggetti nelle singole richieste separate all'applicazione, l'esempio include `OperationsController` che richiede ogni tipo di `IOperation` e `OperationService`. L'azione `Index` visualizza quindi tutti i valori `OperationId` del controller e del servizio.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Sono ora presenti due richieste separate all'azione del controller seguente:

![Visualizzazione delle operazioni dell'applicazione Web di esempio di inserimento delle dipendenze eseguita in Microsoft Edge che mostra i valori di ID operazione (GUID) del controller temporaneo, con ambito, singleton e di istanza e delle operazioni del servizio nella prima richiesta.](dependency-injection/_static/lifetimes_request1.png)

![Visualizzazione delle operazioni con i valori di ID operazione per una seconda richiesta.](dependency-injection/_static/lifetimes_request2.png)

Osservare i valori `OperationId` che variano all'interno della richiesta e da una richiesta e l'altra.

* Gli oggetti *temporanei* sono sempre diversi; a ogni controller e a ogni servizio viene fornita una nuova istanza.

* Gli oggetti *con ambito* sono gli stessi all'interno di una richiesta, ma variano nelle diverse richieste

* Gli oggetti *singleton* sono gli stessi per ogni oggetto e ogni richiesta, indipendentemente dall'istanza fornita in `ConfigureServices`

## <a name="resolve-a-scoped-service-within-the-application-scope"></a>Risolvere un servizio con ambito nell'ambito dell'applicazione

Creare un [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) con [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) per risolvere un servizio con ambito nell'ambito dell'app. Questo approccio è utile per l'accesso a un servizio con ambito all'avvio e per l'esecuzione di attività di inizializzazione. L'esempio seguente indica come ottenere un contesto per `MyScopedService` in `Program.Main`:

```csharp
public static void Main(string[] args)
{
    var host = BuildWebHost(args);

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

Quando l'app viene eseguita nell'ambiente di sviluppo in ASP.NET Core 2.0 o versioni successive, il provider di servizi predefinito esegue controlli per verificare che:

* I servizi con ambito non vengano risolti direttamente o indirettamente dal provider di servizi radice.
* I servizi con ambito non vengano inseriti direttamente o indirettamente in singleton.

Il provider di servizi radice viene creato con la chiamata di [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider). La durata del provider di servizi radice corrisponde alla durata dell'app/server quando il provider viene avviato con l'app e viene eliminato alla chiusura dell'app.

I servizi con ambito vengono eliminati dal contenitore che li ha creati. Se un servizio con ambito viene creato nel contenitore radice, la durata del servizio viene di fatto convertita in singleton, perché il servizio viene eliminato solo dal contenitore radice alla chiusura dell'app/server. La convalida degli ambiti servizio rileva queste situazioni nel contesto della chiamata di `BuildServiceProvider`.

Per altre informazioni, vedere [Scope validation](xref:fundamentals/hosting#scope-validation) (Convalida dell'ambito) nell'argomento relativo all'hosting.

## <a name="request-services"></a>Servizi di richiesta

I servizi disponibili all'interno di una richiesta ASP.NET da `HttpContext` vengono esposti tramite la raccolta `RequestServices`.

![Finestra di dialogo contestuale di Intellisense dei servizi di richiesta HttpContext che indica che i servizi di richiesta ottengono o impostano IServiceProvider che offre l'accesso al contenitore di servizi della richiesta.](dependency-injection/_static/request-services.png)

I servizi di richiesta rappresentano i servizi configurati e richiesti come parte dell'applicazione. Quando gli oggetti specificano le dipendenze, le dipendenze sono soddisfatte dai tipi individuati in `RequestServices`, non in `ApplicationServices`.

In genere, è consigliabile non usare queste proprietà direttamente e richiedere invece i tipi necessari alle classi tramite il costruttore della classe e lasciare che il framework inserisca le dipendenze. Si ottengono classi più facili da testare (vedere [Test e debug](../testing/index.md)) e abbinate in modo più libero.

> [!NOTE]
> È consigliabile richiedere le dipendenze come parametri del costruttore per l'accesso alla raccolta `RequestServices`.

## <a name="designing-services-for-dependency-injection"></a>Progettazione dei servizi per l'inserimento di dipendenze

È consigliabile progettare i servizi in modo che venga usato l'inserimento di dipendenze per ottenere i collaboratori. Ciò significa evitare l'uso di chiamate ai metodi statici con stato (che producono un codice [static cling](http://deviq.com/static-cling/)) e la creazione di istanze delle classi dipendenti all'interno dei servizi. Può essere utile ricordare la frase [New is Glue](https://ardalis.com/new-is-glue) quando si sceglie se creare un'istanza di un tipo oppure richiederlo usando l'inserimento di dipendenze. Se si seguono i [principi di programmazione ad oggetti SOLID](http://deviq.com/solid/), le classi tenderanno a essere piccole, con refactoring corretto e facili da testare.

Cosa accade se le classi tendono ad avere troppe dipendenze inserite? Ciò significa in genere che la classe sta tentando di eseguire troppe operazioni e probabilmente sta violando il principio di singola responsabilità SRP ([Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)). Verificare se è possibile effettuare il refactoring della classe spostando alcune delle responsabilità in una nuova classe. Tenere presente che le classi `Controller` devono essere incentrate sulle problematiche dell'interfaccia utente. Di conseguenza, le regole business e i dettagli di implementazione di accesso ai dati devono essere inseriti in classi appropriate per queste [problematiche separate](http://deviq.com/separation-of-concerns/).

Per quanto riguarda l'accesso ai dati in particolare, è possibile inserire `DbContext` nei controller (presupponendo che sia stato aggiunto EF al contenitore di servizi in `ConfigureServices`). Alcuni sviluppatori preferiscono usare un'interfaccia di repository per il database anziché inserire `DbContext` direttamente. L'uso di un'interfaccia per incapsulare la logica di accesso ai dati in un'unica posizione consente di ridurre i punti da modificare quando vengono eseguite modifiche al database.

### <a name="disposing-of-services"></a>Eliminazione dei servizi

Il contenitore chiamerà `Dispose` per i tipi `IDisposable` creati. Tuttavia, se si aggiunge un'istanza al contenitore, l'istanza non verrà eliminata.

Esempio:

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> Nella versione 1.0 il contenitore chiama Dispose per *tutti* gli oggetti `IDisposable`, inclusi quelli non creati dal contenitore.

## <a name="replacing-the-default-services-container"></a>Sostituzione del contenitore di servizi predefinito

Il contenitore di servizi incorporato è progettato per soddisfare le esigenze di base del framework e della maggior parte delle applicazioni basate su di esso. Tuttavia, gli sviluppatori possono sostituire il contenitore incorporato con il contenitore preferito. Il metodo `ConfigureServices` restituisce in genere `void`. Tuttavia, se la firma del metodo viene modificata per restituire `IServiceProvider`, è possibile configurare e restituire un contenitore diverso. Sono disponibili numerosi contenitori IOC per .NET. In questo esempio, viene usato il pacchetto [Autofac](https://autofac.org/).

Installare innanzitutto i pacchetti contenitore appropriati:

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

Configurare quindi il contenitore in `ConfigureServices` e restituire `IServiceProvider`:

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

> [!NOTE]
> Quando si usa un contenitore di inserimento delle dipendenze di terze parti, è necessario modificare `ConfigureServices` in modo che restituisca `IServiceProvider` anziché `void`.

Configurare infine Autofac normalmente in `DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

Durante il runtime, Autofac viene usato per risolvere i tipi e inserire le dipendenze. [Altre informazioni sull'uso di Autofac e ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Thread safety

I servizi singleton devono essere thread-safe. Se un servizio singleton ha una dipendenza in un servizio temporaneo, è necessario che anche il servizio temporaneo sia thread-safe, a seconda di come viene usato dal singleton.

## <a name="recommendations"></a>Suggerimenti

Quando si usa l'inserimento di dipendenze, tenere presente le raccomandazioni seguenti:

* L'inserimento di dipendenze è adatto agli oggetti con dipendenze complesse. I controller, i servizi, gli adattatori e i repository sono tutti esempi di oggetti che possono essere aggiunti all'inserimento di dipendenze.

* Evitare di memorizzare i dati e la configurazione direttamente nell'inserimento di dipendenze. Ad esempio, il carrello acquisti di un utente non dovrebbe in genere essere aggiunto al contenitore dei servizi. La configurazione deve usare il [modello di opzioni](xref:fundamentals/configuration/options). Analogamente, evitare gli oggetti "contenitori di dati" che hanno la sola funzione di consentire l'accesso ad altri oggetti. È preferibile richiedere l'elemento necessario tramite l'inserimento di dipendenze, se possibile.

* Evitare l'accesso statico ai servizi.

* Evitare la posizione del servizio nel codice dell'applicazione.

* Evitare l'accesso statico a `HttpContext`.

> [!NOTE]
> È tuttavia possibile che in alcuni casi queste raccomandazioni debbano essere ignorate. Le eccezioni sono rare, in genere casi speciali all'interno del framework.

Tenere presente che l'inserimento di dipendenze è un'*alternativa* ai modelli di accesso agli oggetti statici/globali. Se l'inserimento di dipendenze viene usato con l'accesso agli oggetti statico i vantaggi non saranno evidenti.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Avvio dell'applicazione](xref:fundamentals/startup)
* [Test e debug](xref:testing/index)
* [Attivazione del middleware basata sulla factory](xref:fundamentals/middleware/extensibility)
* [Scrittura di codice pulito in ASP.NET Core con inserimento delle dipendenze (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Container-Managed Application Design, Prelude: Where does the Container Belong?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/) (Progettazione di applicazioni gestite da contenitori. Prologo: qual è la posizione del contenitore?)
* [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) (Principio delle dipendenze esplicite)
* [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (Fowler) (Contenitori di inversione del controllo e modello di inserimento delle dipendenze)
