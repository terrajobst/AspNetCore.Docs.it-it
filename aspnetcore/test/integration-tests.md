---
title: Test di integrazione in ASP.NET Core
author: guardrex
description: Informazioni su come i test di integrazione garantiscono che i componenti dell'app funzionano correttamente a livello di infrastruttura, tra cui database, file system e rete.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2019
uid: test/integration-tests
ms.openlocfilehash: c0fede8f9f46d1b10502055d8e1fe7caa48cf351
ms.sourcegitcommit: 810d5831169770ee240d03207d6671dabea2486e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/22/2019
ms.locfileid: "72779223"
---
# <a name="integration-tests-in-aspnet-core"></a>Test di integrazione in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex) e [Steve Smith](https://ardalis.com/)

::: moniker range=">= aspnetcore-3.0"

I test di integrazione assicurano che i componenti di un'app funzionino correttamente a un livello che include l'infrastruttura di supporto dell'app, ad esempio il database, la file system e la rete. ASP.NET Core supporta i test di integrazione utilizzando un framework unit test con un host Web di prova e un server di prova in memoria.

In questo argomento si presuppone una conoscenza di base degli unit test. Se non si ha familiarità con i concetti di test, vedere gli [unit test in .NET Core e .NET standard](/dotnet/core/testing/) argomento e il contenuto collegato.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

L'app di esempio è un'app Razor Pages e presuppone una conoscenza di base delle Razor Pages. Se non si ha familiarità con Razor Pages, vedere gli argomenti seguenti:

* [Introduzione a Razor Pages in ASP.NET Core](xref:razor-pages/index)
* [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Unit test per Razor Pages](xref:test/razor-pages-tests)

> [!NOTE]
> Per il test di Spa è stato consigliato uno strumento come [Selenium](https://www.seleniumhq.org/), che consente di automatizzare un browser.

## <a name="introduction-to-integration-tests"></a>Introduzione ai test di integrazione

I test di integrazione valutano i componenti di un'app a un livello più ampio rispetto agli [unit test](/dotnet/core/testing/). Gli unit test vengono utilizzati per testare i componenti software isolati, ad esempio i singoli metodi di classe. I test di integrazione confermano che due o più componenti dell'app interagiscono per produrre un risultato previsto, eventualmente includendo tutti i componenti necessari per elaborare completamente una richiesta.

Questi test più ampi vengono usati per testare l'infrastruttura dell'app e l'intero Framework, spesso inclusi i componenti seguenti:

* Database
* File system
* Appliance di rete
* Pipeline richiesta-risposta

Gli unit test utilizzano componenti fabbricati, noti come *Fakes* o *oggetti fittizi*, al posto dei componenti dell'infrastruttura.

Diversamente dagli unit test, i test di integrazione:

* Usare i componenti effettivi usati dall'app nell'ambiente di produzione.
* Richiedere più codice e l'elaborazione dei dati.
* Richiedere più tempo per l'esecuzione.

Pertanto, limitare l'utilizzo dei test di integrazione ai più importanti scenari di infrastruttura. Se è possibile testare un comportamento utilizzando un unit test o un test di integrazione, scegliere il unit test.

> [!TIP]
> Non scrivere test di integrazione per ogni possibile permutazione di dati e accesso ai file con database e file System. Indipendentemente dal numero di posizioni in un'app che interagiscono con i database e i file System, un set mirato di test di integrazione di lettura, scrittura, aggiornamento ed eliminazione è in genere in grado di testare adeguatamente i componenti di database e file system. Usare gli unit test per i test di routine della logica del metodo che interagiscono con questi componenti. Negli unit test, l'uso di Fakes/Mocks dell'infrastruttura comporta una maggiore esecuzione dei test.

> [!NOTE]
> Nelle discussioni sui test di integrazione, il progetto testato viene spesso definito *sistema*sottoposto a test oppure "Sut" per brevità.
>
> *"SUT" viene usato in questo argomento per fare riferimento all'app ASP.NET Core testata.*

## <a name="aspnet-core-integration-tests"></a>Test di integrazione di ASP.NET Core

I test di integrazione in ASP.NET Core richiedono gli elementi seguenti:

* Un progetto di test viene utilizzato per contenere ed eseguire i test. Il progetto di test contiene un riferimento a SUT.
* Il progetto di test crea un host Web di test per SUT e usa un client del server di prova per gestire le richieste e le risposte con SUT.
* Viene usato un test runner per eseguire i test e segnalare i risultati del test.

I test di integrazione seguono una sequenza di eventi che includono i normali passi del test *Arrange*, *Act*e *Assert* :

1. L'host Web di SUT è configurato.
1. Viene creato un client del server di prova per inviare richieste all'app.
1. Il passaggio di *disposizione* del test viene eseguito: l'app di test prepara una richiesta.
1. Viene eseguito il passaggio del test *Act* : il client invia la richiesta e riceve la risposta.
1. Viene eseguito il passaggio del test *Assert* : la risposta *effettiva* viene convalidata come *superato* o *non superato* in base a una risposta *prevista* .
1. Il processo continua fino a quando non vengono eseguiti tutti i test.
1. Vengono restituiti i risultati del test.

In genere, l'host Web di test viene configurato in modo diverso rispetto al normale host web dell'app per le esecuzioni dei test. Ad esempio, è possibile usare un database diverso o impostazioni di app diverse per i test.

I componenti dell'infrastruttura, ad esempio l'host Web di test e il server di prova in memoria ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), vengono forniti o gestiti dal pacchetto [Microsoft. AspNetCore. Mvc. testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) . L'utilizzo di questo pacchetto semplifica la creazione e l'esecuzione di test.

Il pacchetto `Microsoft.AspNetCore.Mvc.Testing` gestisce le attività seguenti:

* Copia il file delle dipendenze ( *. Deps*) da SUT nella directory *bin* del progetto di test.
* Imposta la [radice del contenuto](xref:fundamentals/index#content-root) sulla radice del progetto di Sut in modo che i file e le pagine e le visualizzazioni statiche vengano rilevati durante l'esecuzione dei test.
* Fornisce la classe [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) per semplificare il bootstrap del SUT con `TestServer`.

Nella documentazione relativa agli [unit test](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) viene descritto come configurare un progetto di test e un test runner, oltre a istruzioni dettagliate su come eseguire i test e le indicazioni su come denominare i test e le classi di test.

> [!NOTE]
> Quando si crea un progetto di test per un'app, separare gli unit test dai test di integrazione in progetti diversi. Ciò consente di garantire che i componenti di test dell'infrastruttura non vengano accidentalmente inclusi negli unit test. La separazione dei test di unità e integrazione consente inoltre di controllare il set di test da eseguire.

Non esiste praticamente alcuna differenza tra la configurazione per i test delle app Razor Pages e delle app MVC. L'unica differenza consiste nel modo in cui i test vengono denominati. In un'app Razor Pages, i test degli endpoint della pagina vengono in genere denominati dopo la classe del modello di pagina, ad esempio `IndexPageTests` per testare l'integrazione dei componenti per la pagina di indice. In un'app MVC i test sono in genere organizzati per classi controller e denominati dopo i controller testati, ad esempio `HomeControllerTests` per testare l'integrazione dei componenti per il controller Home.

## <a name="test-app-prerequisites"></a>Test prerequisiti app

Il progetto di test deve:

* Fare riferimento al pacchetto [Microsoft. AspNetCore. Mvc. testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) .
* Specificare SDK Web nel file di progetto (`<Project Sdk="Microsoft.NET.Sdk.Web">`).

Questi prerequisiti possono essere visualizzati nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/). Esaminare il file *tests/RazorPagesProject. tests/RazorPagesProject. tests. csproj* . L'app di esempio usa il Framework di test [xUnit](https://xunit.github.io/) e la libreria del parser [AngleSharp](https://anglesharp.github.io/) , quindi l'app di esempio fa riferimento anche a:

* [xUnit](https://www.nuget.org/packages/xunit)
* [xUnit. Runner. VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp)

Entity Framework Core viene inoltre usato nei test. L'app fa riferimento a:

* [Microsoft. AspNetCore. Diagnostics. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore)
* [Microsoft. AspNetCore. Identity. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity.EntityFrameworkCore)
* [Microsoft. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore)
* [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)
* [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)

## <a name="sut-environment"></a>Ambiente SUT

Se l' [ambiente](xref:fundamentals/environments) di Sut non è impostato, il valore predefinito dell'ambiente è Development.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Test di base con WebApplicationFactory predefinito

[WebApplicationFactory\<TEntryPoint >](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) viene usato per creare un [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) per i test di integrazione. `TEntryPoint` è la classe del punto di ingresso di SUT, in genere la classe `Startup`.

Le classi di test implementano una *classe fixture* Interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) per indicare che la classe contiene test e fornisce istanze di oggetti condivisi tra i test nella classe.

### <a name="basic-test-of-app-endpoints"></a>Test di base degli endpoint dell'app

La classe di test seguente, `BasicTests`, usa il `WebApplicationFactory` per avviare il SUT e fornire un [HttpClient](/dotnet/api/system.net.http.httpclient) a un metodo di test, `Get_EndpointsReturnSuccessAndCorrectContentType`. Il metodo controlla se il codice di stato della risposta ha esito positivo (codici di stato nell'intervallo 200-299) e l'intestazione `Content-Type` è `text/html; charset=utf-8` per diverse pagine dell'app.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) crea un'istanza di `HttpClient` che segue automaticamente i reindirizzamenti e gestisce i cookie.

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

Per impostazione predefinita, i cookie non essenziali non vengono conservati nelle richieste quando i [criteri di consenso GDPR](xref:security/gdpr) sono abilitati. Per mantenere cookie non essenziali, ad esempio quelli usati dal provider TempData, contrassegnarli come essenziali nei test. Per istruzioni su come contrassegnare un cookie come essenziale, vedere [cookie essenziali](xref:security/gdpr#essential-cookies).

### <a name="test-a-secure-endpoint"></a>Testare un endpoint sicuro

Un altro test nella classe `BasicTests` verifica che un endpoint sicuro reindirizza un utente non autenticato alla pagina di accesso dell'app.

In SUT la pagina `/SecurePage` usa una convenzione [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) per applicare un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina. Per ulteriori informazioni, vedere [Razor Pages convenzioni di autorizzazione](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

Nel test `Get_SecurePageRequiresAnAuthenticatedUser`, un [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) è impostato in modo da impedire i reindirizzamenti impostando [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) su `false`:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Se si impedisce al client di seguire il reindirizzamento, è possibile effettuare i controlli seguenti:

* Il codice di stato restituito da SUT può essere controllato rispetto al risultato [HttpStatusCode. Redirect](/dotnet/api/system.net.httpstatuscode) previsto, non al codice di stato finale dopo il reindirizzamento alla pagina di accesso, che sarebbe [HttpStatusCode. OK](/dotnet/api/system.net.httpstatuscode).
* Il valore dell'intestazione `Location` nelle intestazioni di risposta viene controllato per confermare che inizia con `http://localhost/Identity/Account/Login`, non con la risposta finale della pagina di accesso, in cui non è presente l'intestazione `Location`.

Per ulteriori informazioni su `WebApplicationFactoryClientOptions`, vedere la sezione [Opzioni client](#client-options) .

## <a name="customize-webapplicationfactory"></a>Personalizzare WebApplicationFactory

La configurazione dell'host Web può essere creata indipendentemente dalle classi di test ereditando da `WebApplicationFactory` per creare una o più factory personalizzate:

1. Ereditare da `WebApplicationFactory` ed eseguire l'override di [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) consente la configurazione della raccolta di servizi con [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Il seeding del database nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) viene eseguito dal metodo `InitializeDbForTests`. Il metodo è descritto nella sezione esempio di test di [integrazione: organizzazione di app di test](#test-app-organization) .

   Il contesto di database di SUT viene registrato nel relativo metodo `Startup.ConfigureServices`. Il callback `builder.ConfigureServices` dell'app di test viene eseguito *dopo* l'esecuzione del codice `Startup.ConfigureServices` dell'app. Per usare un database diverso per i test rispetto al database dell'app, è necessario sostituire il contesto di database dell'app in `builder.ConfigureServices`.

   L'app di esempio trova il descrittore del servizio per il contesto del database e usa il descrittore per rimuovere la registrazione del servizio. Successivamente, la Factory aggiunge un nuovo `ApplicationDbContext` che usa un database in memoria per i test.

   Per connettersi a un database diverso dal database in memoria, modificare la chiamata `UseInMemoryDatabase` per connettere il contesto a un database diverso. Per utilizzare un database di test SQL Server:

   * Fare riferimento al pacchetto NuGet [Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/) nel file di progetto.
   * Chiamare `UseSqlServer` con una stringa di connessione al database.

   ```csharp
   services.AddDbContext<ApplicationDbContext>((options, context) => 
   {
       context.UseSqlServer(
           Configuration.GetConnectionString("TestingDbConnectionString"));
   });
   ```

2. Usare il `CustomWebApplicationFactory` personalizzato nelle classi di test. Nell'esempio seguente viene usata la Factory nella classe `IndexPageTests`:

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Il client dell'app di esempio è configurato in modo da impedire a `HttpClient` di seguire i reindirizzamenti. Come illustrato nella sezione [testare un endpoint sicuro](#test-a-secure-endpoint) , questo consente ai test di verificare il risultato della prima risposta dell'app. La prima risposta è un reindirizzamento in molti di questi test con un'intestazione `Location`.

3. Un test tipico usa i metodi `HttpClient` e helper per elaborare la richiesta e la risposta:

   [!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Qualsiasi richiesta POST a SUT deve soddisfare il controllo antifalsificazione creato automaticamente dal [sistema antifalsificazione di protezione dei dati](xref:security/data-protection/introduction)dell'app. Per disporre la richiesta POST di un test, l'app di test deve:

1. Effettuare una richiesta per la pagina.
1. Analizzare il cookie antifalsificazione e richiedere il token di convalida dalla risposta.
1. Eseguire la richiesta POST con il cookie antifalsificazione e il token di convalida della richiesta sul posto.

I metodi di estensione dell'helper `SendAsync` (*Helper/Metodo HttpClientExtensions. cs*) e il metodo helper `GetDocumentAsync` (*Helper/htmlhelpers. cs*) nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) usano il parser [AngleSharp](https://anglesharp.github.io/) per gestire il controllo antifalsificazione con il metodi seguenti:

* `GetDocumentAsync` &ndash; riceve il [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) e restituisce un `IHtmlDocument`. `GetDocumentAsync` usa una factory che prepara una *risposta virtuale* basata sul `HttpResponseMessage` originale. Per ulteriori informazioni, vedere la [documentazione di AngleSharp](https://github.com/AngleSharp/AngleSharp#documentation).
* i metodi di estensione `SendAsync` per il `HttpClient` compongono un [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) e chiamano [SendAsync (HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) per inviare richieste a SUT. Gli overload per `SendAsync` accettano il formato HTML (`IHtmlFormElement`) e gli elementi seguenti:
  * Pulsante Invia nel formato (`IHtmlElement`)
  * Raccolta di valori di form (`IEnumerable<KeyValuePair<string, string>>`)
  * Pulsante Invia (`IHtmlElement`) e valori form (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) è una libreria di analisi di terze parti usata a scopo dimostrativo in questo argomento e nell'app di esempio. AngleSharp non è supportato o richiesto per i test di integrazione delle app ASP.NET Core. È possibile usare altri parser, ad esempio [HTML Agility Pack (HAP)](https://html-agility-pack.net/). Un altro approccio consiste nel scrivere codice per gestire direttamente il token di verifica delle richieste del sistema antifalsificazione e il cookie antifalsificazione.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Personalizzare il client con WithWebHostBuilder

Quando è richiesta una configurazione aggiuntiva in un metodo di test, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) crea un nuovo `WebApplicationFactory` con un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) che viene ulteriormente personalizzato in base alla configurazione.

Il metodo di test `Post_DeleteMessageHandler_ReturnsRedirectToRoot` dell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) illustra l'uso di `WithWebHostBuilder`. Questo test esegue l'eliminazione di un record nel database attivando un invio di form in SUT.

Poiché un altro test nella classe `IndexPageTests` esegue un'operazione che elimina tutti i record nel database e può essere eseguita prima del metodo `Post_DeleteMessageHandler_ReturnsRedirectToRoot`, il database viene sottoposta a reseeding in questo metodo di test per assicurarsi che sia presente un record per il SUT da eliminare. La selezione del primo pulsante Elimina del modulo `messages` in SUT viene simulata nella richiesta al SUT:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Opzioni client

Nella tabella seguente viene illustrato il valore predefinito di [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) disponibile durante la creazione di istanze `HttpClient`.

| Opzione | Descrizione | Impostazione predefinita |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Ottiene o imposta un valore che indica se le istanze `HttpClient` devono seguire automaticamente le risposte di reindirizzamento. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Ottiene o imposta l'indirizzo di base delle istanze `HttpClient`. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Ottiene o imposta un valore che indica se le istanze `HttpClient` devono gestire i cookie. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Ottiene o imposta il numero massimo di risposte di reindirizzamento che devono essere seguite dalle istanze `HttpClient`. | 7 |

Creare la classe `WebApplicationFactoryClientOptions` e passarla al metodo [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) (i valori predefiniti sono riportati nell'esempio di codice):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Inserire i servizi fittizi

È possibile eseguire l'override dei servizi in un test con una chiamata a [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) nel generatore host. **Per inserire i servizi fittizi, SUT deve avere una classe `Startup` con un metodo `Startup.ConfigureServices`.**

L'esempio SUT include un servizio con ambito che restituisce un'offerta. La virgoletta è incorporata in un campo nascosto nella pagina di indice quando viene richiesta la pagina di indice.

*Services/IQuoteService. cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService. cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/index. cs*:

[!code-cshtml[](integration-tests/samples/3.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

Quando viene eseguita l'app SUT, viene generato il markup seguente:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Per testare il servizio e l'inserimento di virgolette in un test di integrazione, un servizio fittizio viene inserito in SUT dal test. Il servizio fittizio sostituisce il `QuoteService` dell'app con un servizio fornito dall'app di test, denominato `TestQuoteService`:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

viene chiamato `ConfigureTestServices` e il servizio con ambito viene registrato:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Il markup prodotto durante l'esecuzione del test riflette il testo tra virgolette fornito da `TestQuoteService`, quindi l'asserzione passa:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Come l'infrastruttura di test deduce il percorso radice del contenuto dell'app

Il costruttore `WebApplicationFactory` deduce il percorso [radice del contenuto](xref:fundamentals/index#content-root) dell'app cercando un [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) nell'assembly contenente i test di integrazione con una chiave uguale all'assembly `TEntryPoint` `System.Reflection.Assembly.FullName`. Se non viene trovato un attributo con la chiave corretta, `WebApplicationFactory` esegue il fallback alla ricerca di un file di soluzione (con*estensione sln*) e aggiunge il nome dell'assembly `TEntryPoint` alla directory della soluzione. La directory radice dell'app (percorso radice del contenuto) viene usata per individuare le visualizzazioni e i file di contenuto.

## <a name="disable-shadow-copying"></a>Disabilitare la copia shadow

La copia shadow comporta l'esecuzione dei test in una directory diversa rispetto alla directory di output. Per il corretto funzionamento dei test, è necessario disabilitare la copia shadow. L' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) USA xUnit e Disabilita la copia shadow per xUnit includendo un file *xUnit. Runner. JSON* con l'impostazione di configurazione corretta. Per altre informazioni, vedere [configurazione di xUnit con JSON](https://xunit.github.io/docs/configuring-with-json.html).

Aggiungere il file *xUnit. Runner. JSON* alla radice del progetto di test con il contenuto seguente:

```json
{
  "shadowCopy": false
}
```

## <a name="disposal-of-objects"></a>Eliminazione di oggetti

Dopo aver eseguito i test dell'implementazione di `IClassFixture`, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) e [HttpClient](/dotnet/api/system.net.http.httpclient) vengono eliminati quando xUnit Elimina [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1). Se gli oggetti di cui è stata creata un'istanza dallo sviluppatore richiedono l'eliminazione, eliminarli nell'implementazione `IClassFixture`. Per ulteriori informazioni, vedere [implementazione di un metodo Dispose](/dotnet/standard/garbage-collection/implementing-dispose).

## <a name="integration-tests-sample"></a>Esempio di test di integrazione

L' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) è costituita da due app:

| App | Directory del progetto | Descrizione |
| --- | ----------------- | ----------- |
| App Message (SUT) | *src/RazorPagesProject* | Consente a un utente di aggiungere, eliminare, eliminare tutti i messaggi e analizzarli. |
| App di test | *test/RazorPagesProject. test* | Utilizzato per eseguire il test di integrazione di SUT. |

I test possono essere eseguiti usando le funzionalità di test predefinite di un IDE, ad esempio [Visual Studio](https://visualstudio.microsoft.com). Se si usa [Visual Studio Code](https://code.visualstudio.com/) o la riga di comando, eseguire il comando seguente al prompt dei comandi nella directory *tests/RazorPagesProject. tests* :

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Organizzazione Message app (SUT)

SUT è un sistema di messaggi di Razor Pages con le seguenti caratteristiche:

* La pagina di indice dell'app (*pages/index. cshtml* e *pages/index. cshtml. cs*) fornisce i metodi di interfaccia utente e modello di pagina per controllare l'aggiunta, l'eliminazione e l'analisi dei messaggi (media parole per messaggio).
* Un messaggio viene descritto dalla classe `Message` (*Data/Message. cs*) con due proprietà: `Id` (chiave) e `Text` (messaggio). La proprietà `Text` è obbligatoria e limitata a 200 caratteri.
* I messaggi vengono archiviati utilizzando&#8224; [il database in memoria di Entity Framework](/ef/core/providers/in-memory/).
* L'app contiene un livello di accesso ai dati (DAL) nella classe del contesto di database, `AppDbContext` (*Data/AppDbContext. cs*).
* Se il database è vuoto all'avvio dell'app, l'archivio messaggi viene inizializzato con tre messaggi.
* L'app include un `/SecurePage` a cui è possibile accedere solo da un utente autenticato.

&#8224;L'argomento EF, [test con InMemory](/ef/core/miscellaneous/testing/in-memory), spiega come usare un database in memoria per i test con MSTest. In questo argomento viene usato il Framework di test di [xUnit](https://xunit.github.io/) . I concetti di test e le implementazioni di test in diversi framework di test sono simili, ma non identici.

Anche se l'app non usa il modello di repository e non è un esempio efficace del [modello di unità di lavoro (UOW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supporta questi modelli di sviluppo. Per altre informazioni, vedere [progettazione del livello di persistenza dell'infrastruttura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) e della [logica del controller di test](/aspnet/core/mvc/controllers/testing) (l'esempio implementa il modello di repository).

### <a name="test-app-organization"></a>Testare l'organizzazione dell'app

L'app di test è un'app console all'interno della directory *tests/RazorPagesProject. tests* .

| Testare la directory dell'app | Descrizione |
| ------------------ | ----------- |
| *BasicTests* | *BasicTests.cs* contiene metodi di test per il routing, l'accesso a una pagina sicura da parte di un utente non autenticato e il recupero di un profilo utente GitHub e il controllo dell'accesso utente del profilo. |
| *IntegrationTests* | *IndexPageTests.cs* contiene i test di integrazione per la pagina di indice con la classe `WebApplicationFactory` personalizzata. |
| *Helper/utilità* | <ul><li>*Utilities.cs* contiene il metodo `InitializeDbForTests` utilizzato per eseguire il seeding del database con dati di test.</li><li>*HtmlHelpers.cs* fornisce un metodo per restituire un AngleSharp `IHtmlDocument` per l'uso da parte dei metodi di test.</li><li>*HttpClientExtensions.cs* forniscono overload per `SendAsync` per inviare richieste a SUT.</li></ul> |

Il Framework di test è [xUnit](https://xunit.github.io/). I test di integrazione vengono eseguiti usando [Microsoft. AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost), che include [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Poiché il pacchetto [Microsoft. AspNetCore. Mvc. testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) viene usato per configurare l'host di test e il server di prova, i pacchetti `TestHost` e `TestServer` non richiedono riferimenti ai pacchetti diretti nel file di progetto dell'app di test o nella configurazione dello sviluppatore nel test app.

**Seeding del database per il test**

I test di integrazione richiedono in genere un set di dati di piccole dimensioni nel database prima dell'esecuzione del test. Ad esempio, un test di eliminazione richiede l'eliminazione di un record del database, pertanto è necessario che il database disponga di almeno un record affinché la richiesta di eliminazione abbia esito positivo.

L'app di esempio esegue il seeding del database con tre messaggi in *Utilities.cs* che possono essere usati dai test quando vengono eseguiti:

[!code-csharp[](integration-tests/samples/3.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

Il contesto di database di SUT viene registrato nel relativo metodo `Startup.ConfigureServices`. Il callback `builder.ConfigureServices` dell'app di test viene eseguito *dopo* l'esecuzione del codice `Startup.ConfigureServices` dell'app. Per usare un database diverso per i test, il contesto di database dell'app deve essere sostituito in `builder.ConfigureServices`. Per ulteriori informazioni, vedere la sezione [Customize WebApplicationFactory](#customize-webapplicationfactory) .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

I test di integrazione assicurano che i componenti di un'app funzionino correttamente a un livello che include l'infrastruttura di supporto dell'app, ad esempio il database, la file system e la rete. ASP.NET Core supporta i test di integrazione utilizzando un framework unit test con un host Web di prova e un server di prova in memoria.

In questo argomento si presuppone una conoscenza di base degli unit test. Se non si ha familiarità con i concetti di test, vedere gli [unit test in .NET Core e .NET standard](/dotnet/core/testing/) argomento e il contenuto collegato.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

L'app di esempio è un'app Razor Pages e presuppone una conoscenza di base delle Razor Pages. Se non si ha familiarità con Razor Pages, vedere gli argomenti seguenti:

* [Introduzione a Razor Pages in ASP.NET Core](xref:razor-pages/index)
* [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Unit test per Razor Pages](xref:test/razor-pages-tests)

> [!NOTE]
> Per il test di Spa è stato consigliato uno strumento come [Selenium](https://www.seleniumhq.org/), che consente di automatizzare un browser.

## <a name="introduction-to-integration-tests"></a>Introduzione ai test di integrazione

I test di integrazione valutano i componenti di un'app a un livello più ampio rispetto agli [unit test](/dotnet/core/testing/). Gli unit test vengono utilizzati per testare i componenti software isolati, ad esempio i singoli metodi di classe. I test di integrazione confermano che due o più componenti dell'app interagiscono per produrre un risultato previsto, eventualmente includendo tutti i componenti necessari per elaborare completamente una richiesta.

Questi test più ampi vengono usati per testare l'infrastruttura dell'app e l'intero Framework, spesso inclusi i componenti seguenti:

* Database
* File system
* Appliance di rete
* Pipeline richiesta-risposta

Gli unit test utilizzano componenti fabbricati, noti come *Fakes* o *oggetti fittizi*, al posto dei componenti dell'infrastruttura.

Diversamente dagli unit test, i test di integrazione:

* Usare i componenti effettivi usati dall'app nell'ambiente di produzione.
* Richiedere più codice e l'elaborazione dei dati.
* Richiedere più tempo per l'esecuzione.

Pertanto, limitare l'utilizzo dei test di integrazione ai più importanti scenari di infrastruttura. Se è possibile testare un comportamento utilizzando un unit test o un test di integrazione, scegliere il unit test.

> [!TIP]
> Non scrivere test di integrazione per ogni possibile permutazione di dati e accesso ai file con database e file System. Indipendentemente dal numero di posizioni in un'app che interagiscono con i database e i file System, un set mirato di test di integrazione di lettura, scrittura, aggiornamento ed eliminazione è in genere in grado di testare adeguatamente i componenti di database e file system. Usare gli unit test per i test di routine della logica del metodo che interagiscono con questi componenti. Negli unit test, l'uso di Fakes/Mocks dell'infrastruttura comporta una maggiore esecuzione dei test.

> [!NOTE]
> Nelle discussioni sui test di integrazione, il progetto testato viene spesso definito *sistema*sottoposto a test oppure "Sut" per brevità.
>
> *"SUT" viene usato in questo argomento per fare riferimento all'app ASP.NET Core testata.*

## <a name="aspnet-core-integration-tests"></a>Test di integrazione di ASP.NET Core

I test di integrazione in ASP.NET Core richiedono gli elementi seguenti:

* Un progetto di test viene utilizzato per contenere ed eseguire i test. Il progetto di test contiene un riferimento a SUT.
* Il progetto di test crea un host Web di test per SUT e usa un client del server di prova per gestire le richieste e le risposte con SUT.
* Viene usato un test runner per eseguire i test e segnalare i risultati del test.

I test di integrazione seguono una sequenza di eventi che includono i normali passi del test *Arrange*, *Act*e *Assert* :

1. L'host Web di SUT è configurato.
1. Viene creato un client del server di prova per inviare richieste all'app.
1. Il passaggio di *disposizione* del test viene eseguito: l'app di test prepara una richiesta.
1. Viene eseguito il passaggio del test *Act* : il client invia la richiesta e riceve la risposta.
1. Viene eseguito il passaggio del test *Assert* : la risposta *effettiva* viene convalidata come *superato* o *non superato* in base a una risposta *prevista* .
1. Il processo continua fino a quando non vengono eseguiti tutti i test.
1. Vengono restituiti i risultati del test.

In genere, l'host Web di test viene configurato in modo diverso rispetto al normale host web dell'app per le esecuzioni dei test. Ad esempio, è possibile usare un database diverso o impostazioni di app diverse per i test.

I componenti dell'infrastruttura, ad esempio l'host Web di test e il server di prova in memoria ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), vengono forniti o gestiti dal pacchetto [Microsoft. AspNetCore. Mvc. testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) . L'utilizzo di questo pacchetto semplifica la creazione e l'esecuzione di test.

Il pacchetto `Microsoft.AspNetCore.Mvc.Testing` gestisce le attività seguenti:

* Copia il file delle dipendenze ( *. Deps*) da SUT nella directory *bin* del progetto di test.
* Imposta la [radice del contenuto](xref:fundamentals/index#content-root) sulla radice del progetto di Sut in modo che i file e le pagine e le visualizzazioni statiche vengano rilevati durante l'esecuzione dei test.
* Fornisce la classe [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) per semplificare il bootstrap del SUT con `TestServer`.

Nella documentazione relativa agli [unit test](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) viene descritto come configurare un progetto di test e un test runner, oltre a istruzioni dettagliate su come eseguire i test e le indicazioni su come denominare i test e le classi di test.

> [!NOTE]
> Quando si crea un progetto di test per un'app, separare gli unit test dai test di integrazione in progetti diversi. Ciò consente di garantire che i componenti di test dell'infrastruttura non vengano accidentalmente inclusi negli unit test. La separazione dei test di unità e integrazione consente inoltre di controllare il set di test da eseguire.

Non esiste praticamente alcuna differenza tra la configurazione per i test delle app Razor Pages e delle app MVC. L'unica differenza consiste nel modo in cui i test vengono denominati. In un'app Razor Pages, i test degli endpoint della pagina vengono in genere denominati dopo la classe del modello di pagina, ad esempio `IndexPageTests` per testare l'integrazione dei componenti per la pagina di indice. In un'app MVC i test sono in genere organizzati per classi controller e denominati dopo i controller testati, ad esempio `HomeControllerTests` per testare l'integrazione dei componenti per il controller Home.

## <a name="test-app-prerequisites"></a>Test prerequisiti app

Il progetto di test deve:

* Fare riferimento ai pacchetti seguenti:
  * [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [Microsoft. AspNetCore. Mvc. testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* Specificare SDK Web nel file di progetto (`<Project Sdk="Microsoft.NET.Sdk.Web">`). Il Web SDK è obbligatorio quando si fa riferimento al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

Questi prerequisiti possono essere visualizzati nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/). Esaminare il file *tests/RazorPagesProject. tests/RazorPagesProject. tests. csproj* . L'app di esempio usa il Framework di test [xUnit](https://xunit.github.io/) e la libreria del parser [AngleSharp](https://anglesharp.github.io/) , quindi l'app di esempio fa riferimento anche a:

* [xUnit](https://www.nuget.org/packages/xunit/)
* [xUnit. Runner. VisualStudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a>Ambiente SUT

Se l' [ambiente](xref:fundamentals/environments) di Sut non è impostato, il valore predefinito dell'ambiente è Development.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Test di base con WebApplicationFactory predefinito

[WebApplicationFactory\<TEntryPoint >](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) viene usato per creare un [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) per i test di integrazione. `TEntryPoint` è la classe del punto di ingresso di SUT, in genere la classe `Startup`.

Le classi di test implementano una *classe fixture* Interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) per indicare che la classe contiene test e fornisce istanze di oggetti condivisi tra i test nella classe.

### <a name="basic-test-of-app-endpoints"></a>Test di base degli endpoint dell'app

La classe di test seguente, `BasicTests`, usa il `WebApplicationFactory` per avviare il SUT e fornire un [HttpClient](/dotnet/api/system.net.http.httpclient) a un metodo di test, `Get_EndpointsReturnSuccessAndCorrectContentType`. Il metodo controlla se il codice di stato della risposta ha esito positivo (codici di stato nell'intervallo 200-299) e l'intestazione `Content-Type` è `text/html; charset=utf-8` per diverse pagine dell'app.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) crea un'istanza di `HttpClient` che segue automaticamente i reindirizzamenti e gestisce i cookie.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

Per impostazione predefinita, i cookie non essenziali non vengono conservati nelle richieste quando i [criteri di consenso GDPR](xref:security/gdpr) sono abilitati. Per mantenere cookie non essenziali, ad esempio quelli usati dal provider TempData, contrassegnarli come essenziali nei test. Per istruzioni su come contrassegnare un cookie come essenziale, vedere [cookie essenziali](xref:security/gdpr#essential-cookies).

### <a name="test-a-secure-endpoint"></a>Testare un endpoint sicuro

Un altro test nella classe `BasicTests` verifica che un endpoint sicuro reindirizza un utente non autenticato alla pagina di accesso dell'app.

In SUT la pagina `/SecurePage` usa una convenzione [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) per applicare un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina. Per ulteriori informazioni, vedere [Razor Pages convenzioni di autorizzazione](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

Nel test `Get_SecurePageRequiresAnAuthenticatedUser`, un [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) è impostato in modo da impedire i reindirizzamenti impostando [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) su `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Se si impedisce al client di seguire il reindirizzamento, è possibile effettuare i controlli seguenti:

* Il codice di stato restituito da SUT può essere controllato rispetto al risultato [HttpStatusCode. Redirect](/dotnet/api/system.net.httpstatuscode) previsto, non al codice di stato finale dopo il reindirizzamento alla pagina di accesso, che sarebbe [HttpStatusCode. OK](/dotnet/api/system.net.httpstatuscode).
* Il valore dell'intestazione `Location` nelle intestazioni di risposta viene controllato per confermare che inizia con `http://localhost/Identity/Account/Login`, non con la risposta finale della pagina di accesso, in cui non è presente l'intestazione `Location`.

Per ulteriori informazioni su `WebApplicationFactoryClientOptions`, vedere la sezione [Opzioni client](#client-options) .

## <a name="customize-webapplicationfactory"></a>Personalizzare WebApplicationFactory

La configurazione dell'host Web può essere creata indipendentemente dalle classi di test ereditando da `WebApplicationFactory` per creare una o più factory personalizzate:

1. Ereditare da `WebApplicationFactory` ed eseguire l'override di [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) consente la configurazione della raccolta di servizi con [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Il seeding del database nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) viene eseguito dal metodo `InitializeDbForTests`. Il metodo è descritto nella sezione esempio di test di [integrazione: organizzazione di app di test](#test-app-organization) .

2. Usare il `CustomWebApplicationFactory` personalizzato nelle classi di test. Nell'esempio seguente viene usata la Factory nella classe `IndexPageTests`:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Il client dell'app di esempio è configurato in modo da impedire a `HttpClient` di seguire i reindirizzamenti. Come illustrato nella sezione [testare un endpoint sicuro](#test-a-secure-endpoint) , questo consente ai test di verificare il risultato della prima risposta dell'app. La prima risposta è un reindirizzamento in molti di questi test con un'intestazione `Location`.

3. Un test tipico usa i metodi `HttpClient` e helper per elaborare la richiesta e la risposta:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Qualsiasi richiesta POST a SUT deve soddisfare il controllo antifalsificazione creato automaticamente dal [sistema antifalsificazione di protezione dei dati](xref:security/data-protection/introduction)dell'app. Per disporre la richiesta POST di un test, l'app di test deve:

1. Effettuare una richiesta per la pagina.
1. Analizzare il cookie antifalsificazione e richiedere il token di convalida dalla risposta.
1. Eseguire la richiesta POST con il cookie antifalsificazione e il token di convalida della richiesta sul posto.

I metodi di estensione dell'helper `SendAsync` (*Helper/Metodo HttpClientExtensions. cs*) e il metodo helper `GetDocumentAsync` (*Helper/htmlhelpers. cs*) nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) usano il parser [AngleSharp](https://anglesharp.github.io/) per gestire il controllo antifalsificazione con il metodi seguenti:

* `GetDocumentAsync` &ndash; riceve il [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) e restituisce un `IHtmlDocument`. `GetDocumentAsync` usa una factory che prepara una *risposta virtuale* basata sul `HttpResponseMessage` originale. Per ulteriori informazioni, vedere la [documentazione di AngleSharp](https://github.com/AngleSharp/AngleSharp#documentation).
* i metodi di estensione `SendAsync` per il `HttpClient` compongono un [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) e chiamano [SendAsync (HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) per inviare richieste a SUT. Gli overload per `SendAsync` accettano il formato HTML (`IHtmlFormElement`) e gli elementi seguenti:
  * Pulsante Invia nel formato (`IHtmlElement`)
  * Raccolta di valori di form (`IEnumerable<KeyValuePair<string, string>>`)
  * Pulsante Invia (`IHtmlElement`) e valori form (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) è una libreria di analisi di terze parti usata a scopo dimostrativo in questo argomento e nell'app di esempio. AngleSharp non è supportato o richiesto per i test di integrazione delle app ASP.NET Core. È possibile usare altri parser, ad esempio [HTML Agility Pack (HAP)](https://html-agility-pack.net/). Un altro approccio consiste nel scrivere codice per gestire direttamente il token di verifica delle richieste del sistema antifalsificazione e il cookie antifalsificazione.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Personalizzare il client con WithWebHostBuilder

Quando è richiesta una configurazione aggiuntiva in un metodo di test, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) crea un nuovo `WebApplicationFactory` con un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) che viene ulteriormente personalizzato in base alla configurazione.

Il metodo di test `Post_DeleteMessageHandler_ReturnsRedirectToRoot` dell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) illustra l'uso di `WithWebHostBuilder`. Questo test esegue l'eliminazione di un record nel database attivando un invio di form in SUT.

Poiché un altro test nella classe `IndexPageTests` esegue un'operazione che elimina tutti i record nel database e può essere eseguita prima del metodo `Post_DeleteMessageHandler_ReturnsRedirectToRoot`, il database viene sottoposta a reseeding in questo metodo di test per assicurarsi che sia presente un record per il SUT da eliminare. La selezione del primo pulsante Elimina del modulo `messages` in SUT viene simulata nella richiesta al SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Opzioni client

Nella tabella seguente viene illustrato il valore predefinito di [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) disponibile durante la creazione di istanze `HttpClient`.

| Opzione | Descrizione | Impostazione predefinita |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Ottiene o imposta un valore che indica se le istanze `HttpClient` devono seguire automaticamente le risposte di reindirizzamento. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Ottiene o imposta l'indirizzo di base delle istanze `HttpClient`. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Ottiene o imposta un valore che indica se le istanze `HttpClient` devono gestire i cookie. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Ottiene o imposta il numero massimo di risposte di reindirizzamento che devono essere seguite dalle istanze `HttpClient`. | 7 |

Creare la classe `WebApplicationFactoryClientOptions` e passarla al metodo [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) (i valori predefiniti sono riportati nell'esempio di codice):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Inserire i servizi fittizi

È possibile eseguire l'override dei servizi in un test con una chiamata a [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) nel generatore host. **Per inserire i servizi fittizi, SUT deve avere una classe `Startup` con un metodo `Startup.ConfigureServices`.**

L'esempio SUT include un servizio con ambito che restituisce un'offerta. La virgoletta è incorporata in un campo nascosto nella pagina di indice quando viene richiesta la pagina di indice.

*Services/IQuoteService. cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService. cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/index. cs*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

Quando viene eseguita l'app SUT, viene generato il markup seguente:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Per testare il servizio e l'inserimento di virgolette in un test di integrazione, un servizio fittizio viene inserito in SUT dal test. Il servizio fittizio sostituisce il `QuoteService` dell'app con un servizio fornito dall'app di test, denominato `TestQuoteService`:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

viene chiamato `ConfigureTestServices` e il servizio con ambito viene registrato:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Il markup prodotto durante l'esecuzione del test riflette il testo tra virgolette fornito da `TestQuoteService`, quindi l'asserzione passa:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Come l'infrastruttura di test deduce il percorso radice del contenuto dell'app

Il costruttore `WebApplicationFactory` deduce il percorso [radice del contenuto](xref:fundamentals/index#content-root) dell'app cercando un [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) nell'assembly contenente i test di integrazione con una chiave uguale all'assembly `TEntryPoint` `System.Reflection.Assembly.FullName`. Se non viene trovato un attributo con la chiave corretta, `WebApplicationFactory` esegue il fallback alla ricerca di un file di soluzione (con*estensione sln*) e aggiunge il nome dell'assembly `TEntryPoint` alla directory della soluzione. La directory radice dell'app (percorso radice del contenuto) viene usata per individuare le visualizzazioni e i file di contenuto.

## <a name="disable-shadow-copying"></a>Disabilitare la copia shadow

La copia shadow comporta l'esecuzione dei test in una directory diversa rispetto alla directory di output. Per il corretto funzionamento dei test, è necessario disabilitare la copia shadow. L' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) USA xUnit e Disabilita la copia shadow per xUnit includendo un file *xUnit. Runner. JSON* con l'impostazione di configurazione corretta. Per altre informazioni, vedere [configurazione di xUnit con JSON](https://xunit.github.io/docs/configuring-with-json.html).

Aggiungere il file *xUnit. Runner. JSON* alla radice del progetto di test con il contenuto seguente:

```json
{
  "shadowCopy": false
}
```

Se si usa Visual Studio, impostare la proprietà **copia nella directory di output** del file su **copia sempre**. Se non si usa Visual Studio, aggiungere una destinazione `Content` al file di progetto dell'app di test:

```xml
<ItemGroup>
  <Content Update="xunit.runner.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
  </Content>
</ItemGroup>
```

## <a name="disposal-of-objects"></a>Eliminazione di oggetti

Dopo aver eseguito i test dell'implementazione di `IClassFixture`, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) e [HttpClient](/dotnet/api/system.net.http.httpclient) vengono eliminati quando xUnit Elimina [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1). Se gli oggetti di cui è stata creata un'istanza dallo sviluppatore richiedono l'eliminazione, eliminarli nell'implementazione `IClassFixture`. Per ulteriori informazioni, vedere [implementazione di un metodo Dispose](/dotnet/standard/garbage-collection/implementing-dispose).

## <a name="integration-tests-sample"></a>Esempio di test di integrazione

L' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) è costituita da due app:

| App | Directory del progetto | Descrizione |
| --- | ----------------- | ----------- |
| App Message (SUT) | *src/RazorPagesProject* | Consente a un utente di aggiungere, eliminare, eliminare tutti i messaggi e analizzarli. |
| App di test | *test/RazorPagesProject. test* | Utilizzato per eseguire il test di integrazione di SUT. |

I test possono essere eseguiti usando le funzionalità di test predefinite di un IDE, ad esempio [Visual Studio](https://visualstudio.microsoft.com). Se si usa [Visual Studio Code](https://code.visualstudio.com/) o la riga di comando, eseguire il comando seguente al prompt dei comandi nella directory *tests/RazorPagesProject. tests* :

```dotnetcli
dotnet test
```

### <a name="message-app-sut-organization"></a>Organizzazione Message app (SUT)

SUT è un sistema di messaggi di Razor Pages con le seguenti caratteristiche:

* La pagina di indice dell'app (*pages/index. cshtml* e *pages/index. cshtml. cs*) fornisce i metodi di interfaccia utente e modello di pagina per controllare l'aggiunta, l'eliminazione e l'analisi dei messaggi (media parole per messaggio).
* Un messaggio viene descritto dalla classe `Message` (*Data/Message. cs*) con due proprietà: `Id` (chiave) e `Text` (messaggio). La proprietà `Text` è obbligatoria e limitata a 200 caratteri.
* I messaggi vengono archiviati utilizzando&#8224; [il database in memoria di Entity Framework](/ef/core/providers/in-memory/).
* L'app contiene un livello di accesso ai dati (DAL) nella classe del contesto di database, `AppDbContext` (*Data/AppDbContext. cs*).
* Se il database è vuoto all'avvio dell'app, l'archivio messaggi viene inizializzato con tre messaggi.
* L'app include un `/SecurePage` a cui è possibile accedere solo da un utente autenticato.

&#8224;L'argomento EF, [test con InMemory](/ef/core/miscellaneous/testing/in-memory), spiega come usare un database in memoria per i test con MSTest. In questo argomento viene usato il Framework di test di [xUnit](https://xunit.github.io/) . I concetti di test e le implementazioni di test in diversi framework di test sono simili, ma non identici.

Anche se l'app non usa il modello di repository e non è un esempio efficace del [modello di unità di lavoro (UOW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supporta questi modelli di sviluppo. Per altre informazioni, vedere [progettazione del livello di persistenza dell'infrastruttura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) e della [logica del controller di test](/aspnet/core/mvc/controllers/testing) (l'esempio implementa il modello di repository).

### <a name="test-app-organization"></a>Testare l'organizzazione dell'app

L'app di test è un'app console all'interno della directory *tests/RazorPagesProject. tests* .

| Testare la directory dell'app | Descrizione |
| ------------------ | ----------- |
| *BasicTests* | *BasicTests.cs* contiene metodi di test per il routing, l'accesso a una pagina sicura da parte di un utente non autenticato e il recupero di un profilo utente GitHub e il controllo dell'accesso utente del profilo. |
| *IntegrationTests* | *IndexPageTests.cs* contiene i test di integrazione per la pagina di indice con la classe `WebApplicationFactory` personalizzata. |
| *Helper/utilità* | <ul><li>*Utilities.cs* contiene il metodo `InitializeDbForTests` utilizzato per eseguire il seeding del database con dati di test.</li><li>*HtmlHelpers.cs* fornisce un metodo per restituire un AngleSharp `IHtmlDocument` per l'uso da parte dei metodi di test.</li><li>*HttpClientExtensions.cs* forniscono overload per `SendAsync` per inviare richieste a SUT.</li></ul> |

Il Framework di test è [xUnit](https://xunit.github.io/). I test di integrazione vengono eseguiti usando [Microsoft. AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost), che include [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Poiché il pacchetto [Microsoft. AspNetCore. Mvc. testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) viene usato per configurare l'host di test e il server di prova, i pacchetti `TestHost` e `TestServer` non richiedono riferimenti ai pacchetti diretti nel file di progetto dell'app di test o nella configurazione dello sviluppatore nel test app.

**Seeding del database per il test**

I test di integrazione richiedono in genere un set di dati di piccole dimensioni nel database prima dell'esecuzione del test. Ad esempio, un test di eliminazione richiede l'eliminazione di un record del database, pertanto è necessario che il database disponga di almeno un record affinché la richiesta di eliminazione abbia esito positivo.

L'app di esempio esegue il seeding del database con tre messaggi in *Utilities.cs* che possono essere usati dai test quando vengono eseguiti:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

::: moniker-end

## <a name="additional-resources"></a>Risorse aggiuntive

* [Unit test](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Unit test per Razor Pages](xref:test/razor-pages-tests)
* [Middleware](xref:fundamentals/middleware/index)
* [Test controller](xref:mvc/controllers/testing)
