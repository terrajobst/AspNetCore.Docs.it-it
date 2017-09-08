---
title: Dependency Injection in ASP.NET Core
author: ardalis
description: Informazioni su come ASP.NET Core implementa l'inserimento di dipendenze e come utilizzarla.
keywords: Inserimento di dipendenze, di ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fccd69be-7ad1-47fb-b203-b3633b6b9a9b
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/dependency-injection
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2e191a7395110cde7ab5b2f19b6154c96fb496e
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-dependency-injection-in-aspnet-core"></a>Introduzione a Dependency Injection in ASP.NET Core

<a name=fundamentals-dependency-injection></a>

Da [Steve Smith](http://ardalis.com) e [Scott Addie](https://scottaddie.com)

ASP.NET Core è progettato da zero per supportare e sfruttare l'inserimento di dipendenze. Applicazioni ASP.NET Core possono sfruttare servizi framework incorporato, poiché questi vengono inseriti in metodi della classe di avvio, e i servizi delle applicazioni possono essere configurati per l'attacco intrusivo nel codice anche. Il contenitore dei servizi predefiniti fornito da ASP.NET Core fornisce una funzionalità minima impostato e non è destinata a sostituire altri contenitori.

[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample)

## <a name="what-is-dependency-injection"></a>Che cos'è l'inserimento di dipendenze?

Inserimento di dipendenze (DI) è una tecnica per l'implementazione dell'accoppiamento debole tra oggetti e i collaboratori o dipendenze. Anziché creare direttamente un'istanza di collaboratori o utilizzando i riferimenti statici, per la classe in qualche modo sono disponibili gli oggetti di cui necessita di una classe per eseguire le azioni. In genere, le classi verranno dichiarare le relative dipendenze attraverso il relativo costruttore, consentendo loro di seguire il [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/). Questo approccio è noto come "inserimento del costruttore".

Quando le classi sono dotate DI ricordare, essi sono più loosely coupled perché non presentano dipendenze dirette, hardcoded i collaboratori. Questo concetto segue il [principio di inversione dipendenza](http://deviq.com/dependency-inversion-principle/), che indica che *"moduli di livello alti non devono dipendere moduli di livello bassi; entrambi dovrebbero dipendere astrazioni".* Anziché le implementazioni specifiche di riferimento, classi richiesta astrazioni (in genere `interfaces`) che vengono forniti a tali quando la classe viene costruita. L'estrazione delle dipendenze in interfacce fornendo implementazioni di queste interfacce come parametri rappresenta anche un esempio del [progettuale strategia](http://deviq.com/strategy-design-pattern/).

Quando un sistema è basato sull'uso DI, con molte classi che richiede le relative dipendenze tramite i relativi costruttore (o proprietà), è utile creare una classe dedicata alla creazione di queste classi con le relative dipendenze. Queste classi sono definite come *contenitori*, più specificamente, [Inversion of Control (IoC)](http://deviq.com/inversion-of-control/) contenitori o contenitori Dependency Injection (DI). Un contenitore è essenzialmente una factory che è responsabile di fornire istanze di tipi che vengono richiesti da esso. Se un determinato tipo è dichiarato che dispone di dipendenze e il contenitore è stato configurato per fornire i tipi di relazione, verrà creato le dipendenze come parte della creazione dell'istanza richiesta. In questo modo, è possibile garantire grafici complessi delle dipendenze per le classi senza la necessità di costruzione qualsiasi oggetto a livello di codice. Oltre a creare gli oggetti con le relative dipendenze, contenitori in genere la gestione della durata degli oggetti all'interno dell'applicazione.

ASP.NET Core include un semplice contenitore predefinito (rappresentato dal `IServiceProvider` interface) che supporta l'inserimento del costruttore per impostazione predefinita e ASP.NET rende disponibili tramite DI determinati servizi. ASP. Contenitore di rete fa riferimento ai tipi gestiti come *servizi*. Nella parte restante di questo articolo, *servizi* farà riferimento ai tipi gestiti dal contenitore IoC di ASP.NET Core. Si configurano i servizi del contenitore predefinito nel `ConfigureServices` metodo dell'applicazione `Startup` classe.

> [!NOTE]
> Martin Fowler ha scritto un articolo completo su [inversione di contenitori di controlli e il modello di inserimento di dipendenza](http://www.martinfowler.com/articles/injection.html). Microsoft Patterns and Practices ha anche una descrizione di grande [Dependency Injection](https://msdn.microsoft.com/library/dn178469(v=pandp.30).aspx).

> [!NOTE]
> Questo articolo descrive l'inserimento di dipendenze applicata a tutte le applicazioni ASP.NET. Inserimento di dipendenze all'interno di controller MVC è trattata nella [inserimento di dipendenze e i controller](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Comportamento di attacco intrusivo nel codice del costruttore

Inserimento del costruttore richiede che il costruttore in questione sia *pubblica*. In caso contrario, l'applicazione genera un `InvalidOperationException`:

> Non è possibile trova un costruttore adeguato per il tipo 'YourType'. Verificare il tipo è concreto e i servizi vengono registrati per tutti i parametri di un costruttore pubblico.


Inserimento del costruttore necessario sia presente un solo costruttore applicabile. Sono supportati gli overload del costruttore, ma solo uno degli overload possono essere presenti tutti i cui argomenti possono essere soddisfatti dall'inserimento di dipendenze. Se esiste più di uno, l'applicazione genererà un `InvalidOperationException`:

> Trovata più di un costruttore che accetta tutti i tipi di argomento specificato nel tipo 'YourType'. Deve essere presente solo un costruttore applicabile.

I costruttori possono accettare argomenti non disponibili per l'inserimento di dipendenze, ma questi devono supportare i valori predefiniti. Ad esempio:

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

## <a name="using-framework-provided-services"></a>Utilizzo dei servizi forniti dal Framework

Il `ConfigureServices` metodo la `Startup` classe è responsabile per definire i servizi verranno usate dall'applicazione, incluse funzionalità di piattaforma come Entity Framework Core e ASP.NET MVC di base. Inizialmente, il `IServiceCollection` fornito per `ConfigureServices` ha i seguenti servizi definiti (in base alle [come è stato configurato l'host](xref:fundamentals/hosting)):

| Tipo di servizio | Durata |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Temporaneo |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Temporaneo |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | Temporaneo |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | Temporaneo |
| [Microsoft.AspNetCore.Hosting.Server.IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |

Di seguito è riportato un esempio di come aggiungere servizi aggiuntivi per il contenitore utilizzando un numero di metodi di estensione, ad esempio `AddDbContext`, `AddIdentity`, e `AddMvc`.

[!code-csharp[Principale](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Le funzionalità fornite da ASP.NET, ad esempio MVC, middleware seguono una convenzione dell'utilizzo di un singolo componente*ServiceName* metodo di estensione per registrare tutti i servizi richiesti da tale funzionalità.

>[!TIP]
> È possibile richiedere determinati servizi forniti dal framework all'interno di `Startup` metodi tramite i relativi elenchi di parametri, vedere [avvio dell'applicazione](startup.md) per altri dettagli.

## <a name="registering-your-own-services"></a>Registrazione di servizi personalizzati

È possibile registrare i servizi dell'applicazione, come indicato di seguito. Il primo tipo generico rappresenta il tipo (in genere un'interfaccia) che verrà richieste dal contenitore. Il secondo tipo generico rappresenta il tipo concreto che verrà creata un'istanza mediante il contenitore e utilizzato per soddisfare tali richieste.

[!code-csharp[Principale](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Ogni `services.Add<ServiceName>` metodo di estensione aggiunge (e potenzialmente Configura) servizi. Ad esempio, `services.AddMvc()` aggiunge i servizi MVC richiede. È consigliabile attenersi a questa convenzione, inserire i metodi di estensione nel `Microsoft.Extensions.DependencyInjection` dello spazio dei nomi per incapsulare i gruppi di registrazioni di servizio.

Il `AddTransient` metodo viene utilizzato per eseguire il mapping di tipi astratti ai servizi concreti istanze vengono creati separatamente per ogni oggetto che lo richiede. Questo è noto come il servizio *durata*, e le opzioni di durata aggiuntive descritte di seguito. È importante scegliere una durata appropriata per ognuno dei servizi di che registrazione. Una nuova istanza del servizio deve essere fornita a ogni classe che lo richiede. Un'istanza da utilizzare in una richiesta web specificato. O una singola istanza deve essere utilizzata per la durata dell'applicazione?

Nell'esempio di questo articolo, è un controller semplice che visualizza i nomi di caratteri, chiamati `CharactersController`. Il relativo `Index` metodo consente di visualizzare l'elenco corrente di caratteri che sono stati archiviati nell'applicazione e inizializza la raccolta con un numero limitato di caratteri se non esiste. Si noti che anche se questa applicazione usa Entity Framework Core e `ApplicationDbContext` classe per la persistenza, nessuno di questi obiettivi è evidente nel controller. Al contrario, il meccanismo di accesso ai dati specifici è stato indipendenti e possono essere protetti da un'interfaccia, `ICharacterRepository`, che segue il [modello di repository](http://deviq.com/repository-pattern/). Un'istanza di `ICharacterRepository` viene richiesto tramite il costruttore e assegnato a un campo privato, che viene quindi usato per accedere ai caratteri in base alle esigenze.

[!code-csharp[Principale](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

Il `ICharacterRepository` definisce i metodi di due controller deve essere utilizzato con `Character` istanze.

[!code-csharp[Principale](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Questa interfaccia viene implementata a sua volta da un tipo concreto, `CharacterRepository`, che viene utilizzato in fase di esecuzione.

> [!NOTE]
> Viene utilizzata la modalità DI con la `CharacterRepository` classe è un modello generale, è possibile seguire per tutti i servizi dell'applicazione, non solo in "repository" o classi di accesso ai dati.

[!code-csharp[Principale](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Si noti che `CharacterRepository` richieste un `ApplicationDbContext` nel relativo costruttore. Non è insolito per l'inserimento di dipendenze da utilizzare in modo concatenato simile al seguente, con ogni dipendenza richiesta richiede a sua volta relative dipendenze. Il contenitore è responsabile per la risoluzione di tutte le dipendenze nel grafico e la restituzione viene completamente risolto.

> [!NOTE]
> Creare l'oggetto richiesto e tutti gli oggetti richiede e tutti gli oggetti di tali criteri richiedono, è talvolta detta un *oggetto grafico*. Analogamente, il set di dipendenze che devono essere risolti collettivo viene generalmente indicato come un *albero delle dipendenze* o *grafico dipendenze*.

In questo caso, entrambi `ICharacterRepository` e a sua volta `ApplicationDbContext` deve essere registrato con il contenitore dei servizi in `ConfigureServices` in `Startup`. `ApplicationDbContext`è configurato con la chiamata al metodo di estensione `AddDbContext<T>`. Il codice seguente illustra la registrazione del `CharacterRepository` tipo.

[!code-csharp[Principale](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Contesti di Entity Framework devono essere aggiunti al contenitore di servizi usando il `Scoped` durata. Ciò viene preso in considerazione automaticamente se si utilizzano i metodi di supporto, come illustrato in precedenza. I repository che utilizza Entity Framework devono utilizzare la stessa durata.

>[!WARNING]
> Risolve il pericolo principale per assicurarsi di aver un `Scoped` servizio da un singleton. È probabile che in questo caso che il servizio sarà di stato non corretto durante l'elaborazione delle richieste successive.

Servizi che hanno dipendenze devono registrarli nel contenitore. Se il costruttore di un servizio richiede una primitiva, ad esempio un `string`, si possono essere inserita utilizzando il [modello e la configurazione delle opzioni](configuration.md).

## <a name="service-lifetimes-and-registration-options"></a>La durata del servizio e le opzioni di registrazione

Servizi ASP.NET possono essere configurati con le durate seguenti:

**Temporaneo**

Servizi di durata temporanei vengono creati ogni volta che vengono richiesti. Questa durata è consigliata per i servizi senza stati leggeri.

**Ambito**

Servizi di durata con ambito vengono creati una volta per ogni richiesta.

**Singleton**

Servizi di durata singleton vengono creati la prima volta in cui vengono richiesti (o quando `ConfigureServices` viene eseguito se si specifica un'istanza non esiste), quindi tutte le richieste successive utilizzerà la stessa istanza. Se l'applicazione richiede un comportamento singleton, è consigliabile consentire il contenitore dei servizi gestire la durata del servizio anziché implementare il modello di progettazione singleton e gestire la durata dell'oggetto nella classe in prima persona.

I servizi possono essere registrati con il contenitore in diversi modi. Abbiamo già visto come registrare un'implementazione del servizio con un determinato tipo, specificando il tipo concreto da usare. Inoltre, una factory può essere specificato, che verrà quindi utilizzato per creare l'istanza su richiesta. Il terzo approccio consiste nello specificare direttamente l'istanza del tipo da utilizzare, in cui caso il contenitore non tenterà di creare un'istanza (né verrà dispose dell'istanza).

Per illustrare la differenza tra le opzioni di durata e la registrazione, si consideri una semplice interfaccia che rappresenta una o più attività come un *operazione* con un identificatore univoco, `OperationId`. A seconda di come si configura la durata per questo servizio, il contenitore fornirà uguali o diverse istanze del servizio per la classe richiesta. Per indicare chiaramente quali durata viene richiesto, si creerà un tipo per ogni opzione di durata:

[!code-csharp[Principale](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Implementare queste interfacce utilizzando un'unica classe `Operation`, che accetta un `Guid` nel relativo costruttore o utilizza un nuovo `Guid` se non è specificato.

Successivamente, nel `ConfigureServices`, ogni tipo viene aggiunto al contenitore in base alla durata denominata:

[!code-csharp[Principale](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Si noti che il `IOperationSingletonInstance` servizio utilizza un'istanza specifica con un ID noto di `Guid.Empty` in modo che sia chiaro quando questo tipo è in uso (il relativo Guid sarà tutti zeri). È stato registrato anche un `OperationService` che dipende da ognuno degli altri `Operation` tipi, in modo che sia possibile deselezionare all'interno di una richiesta se questo servizio è di ottenere la stessa istanza del controller o un altro, per ogni tipo di operazione. Questo servizio non è di esporre le relative dipendenze come proprietà, in modo che possano essere visualizzate nella vista.

[!code-csharp[Principale](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Per dimostrare la durata degli oggetti all'interno e tra separate singole richieste all'applicazione, l'esempio include un `OperationsController` che richiede ogni tipo di `IOperation` tipo, nonché un `OperationService`. Il `Index` quindi l'azione consente di visualizzare tutti i controller e del servizio `OperationId` valori.

[!code-csharp[Principale](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Ora due richieste separate vengono apportate a questa azione del controller:

![Visualizzazione delle operazioni dell'applicazione web di esempio di inserimento di dipendenza in esecuzione in Microsoft Edge con i valori di ID operazione (GUID) per temporaneo, nell'ambito, Singleton e il Controller di istanza e operazioni del servizio operazione alla prima richiesta.](dependency-injection/_static/lifetimes_request1.png)

![Le operazioni di visualizzazione che mostra i valori di ID operazione per una seconda richiesta.](dependency-injection/_static/lifetimes_request2.png)

Osservare quali il `OperationId` valori variare all'interno di una richiesta e tra le richieste.

* *Temporaneo* sempre gli oggetti sono diversi; viene fornita una nuova istanza per tutti i controller e a ogni servizio.

* *Ambito* oggetti sono uguali all'interno di una richiesta, ma è diversa nelle diverse richieste

* *Singleton* oggetti sono uguali per tutti gli oggetti e ogni richiesta (indipendentemente dal fatto che un'istanza viene fornita `ConfigureServices`)

## <a name="request-services"></a>Servizi di richiesta

Richiedere i servizi disponibili all'interno di un ASP.NET `HttpContext` vengono esposti tramite il `RequestServices` insieme.

![HttpContext richiesta servizi Intellisense contesto finestra di dialogo che informa che servizi di richiesta Ottiene o imposta l'IServiceProvider che fornisce l'accesso al contenitore dei servizi della richiesta.](dependency-injection/_static/request-services.png)

Servizi di richiesta rappresentano i servizi si configurano e richiesta come parte dell'applicazione. Quando gli oggetti di specificano le dipendenze, queste sono soddisfatti dai tipi individuati nella `RequestServices`, non `ApplicationServices`.

In genere, è consigliabile utilizzare queste proprietà direttamente, preferendo invece richiedere i tipi di classi che è necessario tramite il costruttore della classe e lasciare che il framework di inserire queste dipendenze. Si ottengono le classi che sono più facili da testare (vedere [Testing](../testing/index.md)) e non siano più strettamente collegati.

> [!NOTE]
> Preferisce richiedere dipendenze come parametri del costruttore per l'accesso di `RequestServices` insieme.

## <a name="designing-your-services-for-dependency-injection"></a>Progettazione di servizi per l'inserimento di dipendenze

È consigliabile progettare i servizi di utilizzare l'inserimento di dipendenze per ottenere i collaboratori. Ciò significa evitare l'uso di chiamate al metodo statico con stato (che portare a un odore codice noto come [adesive statico](http://deviq.com/static-cling/)) e la creazione di istanze diretto delle classi dipendenti all'interno dei servizi. Può essere utile per ricordare la frase [nuovi è Glue](http://ardalis.com/new-is-glue), quando si sceglie di creare un'istanza di un tipo o di richiederla tramite l'inserimento di dipendenze. Seguendo il [a tinta unita principi dell'oggetto orientato alla progettazione](http://deviq.com/solid/), le classi tendono a essere piccole, facilmente testato e corretto factoring.

Cosa accade se si trova che le classi tendono ad avere modo troppe dipendenze venga introdotto? In genere si tratta di un segno che la classe sta tentando di eccessivo e se è probabile che stanno violando i criteri di restrizione software - il [singolo responsabilità principio](http://deviq.com/single-responsibility-principle/). Vedere se è possibile effettuare il refactoring della classe da spostare alcune delle proprie funzioni in una nuova classe. Tenere presente che il `Controller` classi devono essere incentrate sulle problematiche dell'interfaccia utente, in modo business dati e le regole di accesso ai dettagli di implementazione devono essere conservati in classi appropriate a questi [separare le problematiche](http://deviq.com/separation-of-concerns/).

Per quanto riguarda l'accesso ai dati in particolare, è possibile inserire il `DbContext` il controller (presupponendo che EF aggiunti al contenitore di servizi `ConfigureServices`). Alcuni sviluppatori preferiscono utilizzare un'interfaccia del repository per il database anziché inserendo il `DbContext` direttamente. Utilizzando un'interfaccia per incapsulare i dati della logica di accesso in un'unica posizione può ridurre il numero di posizioni è necessario modificare il database cambia.

### <a name="disposing-of-services"></a>Eliminazione dei servizi

Il contenitore chiamerà `Dispose` per `IDisposable` crea tipi. Tuttavia, se ci si aggiunge un'istanza per il contenitore, non verrà eliminato.

Esempio:

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();

    // container did not create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> Nella versione 1.0, il contenitore chiamato dispose su *tutti* `IDisposable` oggetti, inclusi quelli non è stata creata.

## <a name="replacing-the-default-services-container"></a>Sostituendo il contenitore dei servizi predefiniti

Il contenitore dei servizi predefiniti è progettato per soddisfare le esigenze di base di framework e la maggior parte delle applicazioni consumer compilate su di esso. Tuttavia, gli sviluppatori possono sostituire il contenitore predefinito con il relativo contenitore preferito. Il `ConfigureServices` metodo in genere restituisce `void`, ma se la firma viene modificata per restituire `IServiceProvider`, un diverso contenitore può essere configurato e restituito. Sono disponibili molti contenitori IOC per .NET. In questo esempio, il [Autofac](http://autofac.org/) viene utilizzato un pacchetto.

In primo luogo, installare i pacchetti contenitore appropriato:

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

A questo punto, configurare il contenitore in `ConfigureServices` e restituire un `IServiceProvider`:

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
> Quando si utilizza un contenitore DI terze parti, è necessario modificare `ConfigureServices` in modo che restituisca `IServiceProvider` anziché `void`.

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

In fase di esecuzione Autofac da utilizzare per risolvere i tipi e inserire le dipendenze. [Ulteriori informazioni sull'utilizzo Autofac e ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Thread safety

Servizi singleton devono essere thread-safe. Se un servizio singleton ha una dipendenza da un servizio temporaneo, il servizio temporaneo anche debba essere thread-safe a seconda della modalità di utilizzo per il singleton.

## <a name="recommendations"></a>Suggerimenti

Quando si lavora con inserimento di dipendenze, tenere presente quanto segue:

* SEN è per gli oggetti che hanno dipendenze complesse. I controller, servizi, schede e repository sono tutti esempi di oggetti che possono essere aggiunti alla DI.

* Evitare di archiviare i dati e la configurazione direttamente DI. Ad esempio, carrello acquisti un utente non deve in genere aggiunto per il contenitore dei servizi. Configurazione deve usare il [Opzioni modello](configuration.md#options-config-objects). Evitare in modo analogo, gli oggetti "contenitore di dati" esistenti solo per consentire l'accesso a un altro oggetto. È preferibile richiedere l'elemento effettivo necessaria tramite DI, se possibile.

* Evitare l'accesso ai servizi statico.

* Evitare di posizione del servizio nel codice dell'applicazione.

* Evitare l'accesso statico a `HttpContext`.

> [!NOTE]
> Ad esempio tutti i set di indicazioni, potrebbero verificarsi situazioni in cui è necessario ignorare uno. Sono stati rilevati eccezioni - rari casi principalmente molto speciali all'interno di framework stesso.

Inserimento di dipendenze è un *alternativo* a criteri di accesso di oggetti statici/globali. Non sarà in grado di comprendere i vantaggi delle dipendenze, se possibile combinare con l'accesso agli oggetti statici.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Avvio dell'applicazione](startup.md)

* [Test](../testing/index.md)

* [Scrittura di codice ottimizzato in ASP.NET Core con l'inserimento di dipendenze (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)

* [Progettazione di applicazioni di gestire contenitori, preludio: In cui non il contenitore appartenere?](http://blogs.msdn.com/b/nblumhardt/archive/2008/12/27/container-managed-application-design-prelude-where-does-the-container-belong.aspx)

* [Principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/)

* [Inversione di contenitori di controlli e il criterio di aggiunta della dipendenza](http://www.martinfowler.com/articles/injection.html) (Fowler)
