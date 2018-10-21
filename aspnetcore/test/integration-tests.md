---
title: Test di integrazione in ASP.NET Core
author: guardrex
description: Informazioni su come i test di integrazione garantiscono che i componenti dell'app funzionano correttamente a livello di infrastruttura, tra cui database, file system e rete.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: 15186383497f8dd9a9766f8fe7cf32f70a121c8e
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477592"
---
# <a name="integration-tests-in-aspnet-core"></a>Test di integrazione in ASP.NET Core

Dal [Luke Latham](https://github.com/guardrex) e [Steve Smith](https://ardalis.com/)

Test di integrazione di garantiscono che i componenti di un'app funzionano correttamente a un livello che include l'infrastruttura di supporto dell'app, ad esempio il database, file system e rete. ASP.NET Core supporta i test di integrazione tramite un framework unit test con un host web di test e un server di prova in memoria.

Questo argomento presuppone una conoscenza di base di unit test. Familiarità con i concetti di test, vedere la [Unit Testing in .NET Core e .NET Standard](/dotnet/core/testing/) argomento e il relativo contenuto collegato.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

L'app di esempio è un'app Razor Pages e presuppone una conoscenza di base di Razor Pages. Se ha dimestichezza con le pagine Razor, vedere gli argomenti seguenti:

* [Introduzione a Razor Pages in ASP.NET Core](xref:razor-pages/index)
* [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Unit test per Razor Pages](xref:test/razor-pages-tests)

> [!NOTE]
> Per i test SPAs, si consiglia di uno strumento, ad esempio [Selenium](https://www.seleniumhq.org/), che consente di automatizzare un browser.

## <a name="introduction-to-integration-tests"></a>Introduzione al test di integrazione

Test di integrazione di valutare i componenti di un'app su un livello più ampio rispetto [unit test](/dotnet/core/testing/). Gli unit test vengono utilizzati per testare i componenti software isolati, ad esempio i metodi della classe singoli. Test di integrazione di verificare che due o più componenti dell'app interagiscano per produrre un risultato previsto, magari con ogni componente necessario per l'elaborazione completa una richiesta.

Questi test più ampi vengono usati per testare l'app dell'infrastruttura e intero framework, spesso con i componenti seguenti:

* Database
* File system
* Appliance di rete
* Pipeline di richiesta-risposta

Unit test creato usa i componenti, noti come *fakes* oppure *oggetti fittizi*, al posto di componenti dell'infrastruttura.

A differenza di unit test, test di integrazione:

* Usare i componenti effettivi usati dall'app nell'ambiente di produzione.
* Richiedono più codice e l'elaborazione dei dati.
* Richiedere più tempo per l'esecuzione.

Di conseguenza, limitare l'utilizzo di test di integrazione agli scenari dell'infrastruttura più importanti. Se un comportamento può essere testato tramite unit test o un test di integrazione, scegliere lo unit test.

> [!TIP]
> Non scrivere test di integrazione per ogni permutazione possibili di accesso ai dati e file con database e file System. Indipendentemente dal fatto il numero di cifre in un'app per interagire con i database e file System, un set con lo stato attivo di lettura, scrittura, aggiornamento e integrazione Elimina i test sono in genere in grado di database in modo adeguato test e i componenti del sistema del file. Utilizzare gli unit test per test di routine della logica del metodo che interagiscono con questi componenti. Negli unit test, l'uso dell'infrastruttura fakes/simulazioni risultato nell'esecuzione dei test più veloce.

> [!NOTE]
> Discussioni dei test di integrazione, il progetto testato viene spesso chiamato il *sistema sottoposto a test*, o "SUT" breve.

## <a name="aspnet-core-integration-tests"></a>Test di integrazione ASP.NET Core

Test di integrazione in ASP.NET Core richiedono gli elementi seguenti:

* Un progetto di test viene usato per contenere ed eseguire i test. Il progetto di test contiene un riferimento al progetto ASP.NET Core testato, denominato il *sistema sottoposto a test* (SUT). _"SUT" viene utilizzata in questo argomento per fare riferimento all'app testati._
* Il progetto di test crea un host web di test per il SUT e Usa un client di server di prova per gestire le richieste e risposte per la SUT.
* Un test runner viene utilizzato per eseguire il test e il report, i risultati del test.

Test di integrazione seguono una sequenza di eventi che includono le consuete *disponi*, *Act*, e *Assert* passi del test:

1. Host web del SUT è configurato.
1. Per inviare le richieste per l'app viene creato un client di server di prova.
1. Il *disponi* passo del test viene eseguito: l'app test prepara una richiesta.
1. Il *Act* passo del test viene eseguito: il client invia la richiesta e riceve la risposta.
1. Il *Assert* passo del test viene eseguito: il *effettivo* risposta viene convalidata come una *passare* o *esito negativo* basato su un *previsto*  risposta.
1. Il processo continua fino a quando tutti i test vengono eseguiti.
1. Vengono segnalati i risultati del test.

In genere, l'host web di test è configurato in modo diverso rispetto a host web normale dell'app per il test viene eseguito. Ad esempio, un database diverso o le impostazioni dell'app diverse potrebbero essere usate per i test.

Componenti dell'infrastruttura, ad esempio l'host web di test e il server di prova in memoria ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), vengono fornite o gestiti dalle [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) pacchetto. Uso di questo pacchetto semplifica la creazione di test e l'esecuzione.

Il `Microsoft.AspNetCore.Mvc.Testing` pacchetto gestisce le attività seguenti:

* Copia il file delle dipendenze (*\*.deps*) dal SUT nel progetto di test *bin* cartella.
* Imposta la radice del contenuto alla radice di progetto del SUT in modo che i file statici e pagine/visualizzazioni sono disponibili quando vengono eseguiti i test.
* Fornisce il [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) classe per semplificare il bootstrap di SUT con `TestServer`.

Il [unit test](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentazione viene descritto come configurare un test progetto e test runner, oltre a istruzioni dettagliate su come eseguire i test e indicazioni sulle modalità per i test di nome e le classi di test.

> [!NOTE]
> Quando si crea un progetto di test per un'app, separare gli unit test dal test di integrazione in progetti diversi. Ciò garantisce che componenti dell'infrastruttura di test non accidentalmente sono inclusi negli unit test. La separazione di unit test e integrazione test consente anche controllare su quali set di test vengono eseguiti.

Non è praticamente alcuna differenza tra la configurazione per i test delle App Razor Pages e App MVC. L'unica differenza è in modalità di denominazione dei test. In un'app Razor Pages, sono in genere denominati test degli endpoint di pagina dopo la classe di modello di pagina (ad esempio, `IndexPageTests` per verificare l'integrazione di componenti per la pagina di indice). In un'app MVC, i test sono in genere organizzati per classi controller e denominati dopo i controller eseguono il test (ad esempio, `HomeControllerTests` per verificare l'integrazione di componenti per il controller Home).

## <a name="test-app-prerequisites"></a>Prerequisiti dell'app di test

Il progetto di test deve:

* Fare riferimento a pacchetti seguenti:
  - [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  - [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* Specificare il SDK Web nel file di progetto (`<Project Sdk="Microsoft.NET.Sdk.Web">`). il SDK Web è obbligatorio quando si fa riferimento il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Questi prerequisiti possono essere visualizzati nel [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Esaminare i *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file. L'app di esempio Usa la [xUnit](https://xunit.github.io/) framework di test e il [AngleSharp](https://anglesharp.github.io/) libreria parser, in modo che l'app di esempio fa anche riferimento:

* [xUnit](https://www.nuget.org/packages/xunit/)
* [xUnit](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Test di base con il valore predefinito WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) viene usato per creare una [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) per i test di integrazione. `TEntryPoint` è la classe del punto di ingresso di SUT, in genere il `Startup` classe.

Test sono classi che implementano un *fixture di classe* interface (`IClassFixture`) per indicare la classe contiene i test e specificare le istanze di oggetti condivisi tra i test nella classe.

### <a name="basic-test-of-app-endpoints"></a>Test di base dell'endpoint dell'app

Il seguente test (classe), `BasicTests`, viene utilizzato il `WebApplicationFactory` per eseguire il bootstrap di SUT e fornire un' [HttpClient](/dotnet/api/system.net.http.httpclient) a un metodo di test, `Get_EndpointsReturnSuccessAndCorrectContentType`. Il metodo controlla se il codice di stato risposta ha esito positivo (codici di stato compreso nell'intervallo 200-299) e il `Content-Type` intestazione è `text/html; charset=utf-8` per diverse pagine dell'app.

[Createclient utile](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) crea un'istanza di `HttpClient` che automaticamente segue i reindirizzamenti e gestisce i cookie.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Testare un endpoint protetto

Un altro test nel `BasicTests` classe verifica che un endpoint protetto reindirizza l'utente non autenticato alla pagina di accesso dell'app.

In SUT, il `/SecurePage` pagina viene utilizzato un [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenzione per applicare un' [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina. Per altre informazioni, vedere [convenzioni di autorizzazione di Razor Pages](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

Nel `Get_SecurePageRequiresAnAuthenticatedUser` testare, una [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) è impostata per non consentire i reindirizzamenti impostando [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) a `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Disabilitando il client per eseguire il reindirizzamento, è possono eseguire i controlli seguenti:

* Il codice di stato restituito dal SUT può essere controllato rispetto a quella prevista [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) risultato, non il codice di stato finale dopo il reindirizzamento alla pagina di accesso, che dovrebbe essere [HttpStatusCode](/dotnet/api/system.net.httpstatuscode).
* Il `Location` valore dell'intestazione nelle intestazioni della risposta viene controllato per verificare che iniziano con `http://localhost/Identity/Account/Login`, non l'account di accesso pagina risposta finale, in cui il `Location` intestazione sarebbe presenta.

Per ulteriori informazioni sul `WebApplicationFactoryClientOptions`, vedere la [le opzioni Client](#client-options) sezione.

## <a name="customize-webapplicationfactory"></a>Personalizzare WebApplicationFactory

Configurazione host Web può essere creato indipendentemente dalle classi di test tramite l'eredità dalla `WebApplicationFactory` per creare uno o più factory personalizzate:

1. Ereditare `WebApplicationFactory` ed eseguire l'override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). Il [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) consente la configurazione dell'insieme di servizio con [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Seeding nel database di [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) avviene mediante il `InitializeDbForTests` (metodo). Il metodo descritto nel [esempio di test di integrazione: organizzazione delle app di Test](#test-app-organization) sezione.

2. Usare l'oggetto personalizzato `CustomWebApplicationFactory` nelle classi di test. L'esempio seguente usa la factory nel `IndexPageTests` classe:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Client dell'app di esempio è configurato per impedire la `HttpClient` da seguito reindirizzamenti. Come spiegato nel [testare un endpoint protetto](#test-a-secure-endpoint) sezione, in questo modo i test per verificare il risultato della prima risposta dell'app. La prima risposta è un reindirizzamento in molti di questi test con un `Location` intestazione.

3. Usa un test tipico di `HttpClient` e metodi helper per elaborare la richiesta e risposta:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Qualsiasi richiesta POST per il SUT deve soddisfare il controllo anti falsificazione automaticamente effettuato tramite l'app [sistema antifalsificazione di protezione dati](xref:security/data-protection/introduction). Per poter disporre per la richiesta POST di un test, l'app di test deve:

1. Effettuare una richiesta per la pagina.
1. Analizzare il cookie antifalsificazione e token di convalida richiesta dalla risposta.
1. Eseguire la richiesta POST con la convalida antifalsificazione del cookie e richiesta di token posto.

Il `SendAsync` metodi di estensione helper (*Helpers/HttpClientExtensions.cs*) e il `GetDocumentAsync` metodo helper (*Helpers/HtmlHelpers.cs*) nel [appdiesempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) usare il [AngleSharp](https://anglesharp.github.io/) parser per gestire la verifica non riproducibili con i metodi seguenti:

* `GetDocumentAsync` &ndash; Riceve la [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) e restituisce un `IHtmlDocument`. `GetDocumentAsync` viene utilizzata una factory che consente di preparare una *virtuale risposta* basato su originale `HttpResponseMessage`. Per altre informazioni, vedere la [AngleSharp documentazione](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` metodi di estensione per il `HttpClient` comporre un' [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) e chiamare [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) per inviare le richieste di SUT. Esegue l'overload per `SendAsync` accettare il form HTML (`IHtmlFormElement`) e le seguenti:
  - Pulsante del form di invio (`IHtmlElement`)
  - Insieme dei valori di form (`IEnumerable<KeyValuePair<string, string>>`)
  - Pulsante di invio (`IHtmlElement`) e i valori del modulo (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) sono libreria usata a scopo dimostrativo in questo argomento e l'app di esempio di analisi terze parti. AngleSharp non è supportato o necessario per il test di integrazione delle App ASP.NET Core. Altro parser può essere utilizzato, ad esempio la [Html agilità Pack (HAP)](http://html-agility-pack.net/). Un altro approccio consiste nello scrivere codice per gestire direttamente i token di verifica della richiesta e il cookie antifalsificazione il sistema antifalsificazione.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Personalizzare il client con WithWebHostBuilder

Quando è richiesto all'interno di un metodo di test, una configurazione aggiuntiva [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) crea un nuovo `WebApplicationFactory` con un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ulteriormente personalizzato dalla configurazione.

Il `Post_DeleteMessageHandler_ReturnsRedirectToRoot` metodo di test di [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) illustra l'uso di `WithWebHostBuilder`. Questo test esegue un'operazione di eliminazione record nel database per attivare l'invio di un form nel SUT.

Perché un altro test nel `IndexPageTests` classe esegue un'operazione che elimina tutti i record nel database e possono essere eseguite prima di `Post_DeleteMessageHandler_ReturnsRedirectToRoot` metodo, il database viene effettuato il seeding di questo metodo di test per assicurarsi che sia presente per SUT eliminare un record. Selezionando il `deleteBtn1` pulsante del `messages` modulo nel SUT viene simulata nella richiesta per il SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Opzioni client

La tabella seguente mostra il valore predefinito [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) disponibili durante la creazione di `HttpClient` istanze.

| Opzione | Descrizione | Impostazione predefinita |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Ottiene o imposta se `HttpClient` istanze devono seguire automaticamente le risposte di reindirizzamento. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Ottiene o imposta l'indirizzo di base di `HttpClient` istanze. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Ottiene o imposta se `HttpClient` istanze devono gestire i cookie. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Ottiene o imposta il numero massimo di risposte di reindirizzamento che `HttpClient` sono tenuti a istanze. | 7 |

Creare il `WebApplicationFactoryClientOptions` classi e passarlo al [createclient utile](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) (metodo) (impostazione predefinita vengono visualizzati i valori nell'esempio di codice):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a>Inserire servizi fittizi

I servizi possono essere sostituiti in un test con una chiamata a [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) nel generatore di host. **Per inserire servizi fittizi, è necessario disporre di SUT una `Startup` classe con un `Startup.ConfigureServices` (metodo).**

L'esempio SUT include un servizio con ambito che restituisce un'offerta. L'offerta è incorporato in un campo nascosto nella pagina di indice quando viene richiesta la pagina di indice.

*Services/IQuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

*Services/QuoteService.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

*Startup.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

*Pages/Index.cshtml.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

*Pages/Index.cs*:

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

Il markup seguente viene generato quando viene eseguita l'app SUT:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Per testare l'inserimento del servizio e delle virgolette in un test di integrazione, un servizio fittizio viene inserito nel SUT per il test. Il servizio fittizio sostituisce l'app `QuoteService` con un servizio fornito dall'app di test, denominato `TestQuoteService`:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices` viene chiamato, e viene registrato il servizio con ambito:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Il markup generato durante l'esecuzione del test corrisponde al testo offerta fornito da `TestQuoteService`, pertanto i passaggi di asserzione:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Come l'infrastruttura di test viene dedotto il percorso radice del contenuto dell'app

Il `WebApplicationFactory` costruttore deduce che il percorso radice del contenuto dell'app eseguendo una ricerca di un [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) nell'assembly che contiene i test di integrazione con una chiave uguale al `TEntryPoint` assembly `System.Reflection.Assembly.FullName`. Nel caso in cui non viene trovato un attributo con la chiave corretta, `WebApplicationFactory` opta per la ricerca di un file di soluzione (*\*sln*) e aggiunge il `TEntryPoint` nome dell'assembly alla directory della soluzione. Directory radice dell'app (il percorso radice del contenuto) viene usata per individuare le visualizzazioni e i file di contenuto.

Nella maggior parte dei casi, non è necessario impostare in modo esplicito la radice del contenuto di app, come la logica di ricerca in genere trova la radice del contenuto corretta in fase di esecuzione. Negli scenari speciali in cui non viene trovata la radice del contenuto usando l'algoritmo di ricerca predefinite, l'app contenuto radice può essere specificato in modo esplicito o tramite logica personalizzata. Per impostare la radice del contenuto dell'applicazione in tali scenari, chiamare il `UseSolutionRelativeContentRoot` metodo di estensione dal [testhost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) pacchetto. Specificare percorso relativo della soluzione e il criterio glob o nome file soluzione facoltativo (impostazione predefinita = `*.sln`).

Chiamare il [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) metodo di estensione usando *uno* degli approcci seguenti:

* Quando si configurano le classi di test con `WebApplicationFactory`, fornire una configurazione personalizzata con il [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* Quando si configurano le classi di test con una classe personalizzata `WebApplicationFactory`, ereditano da `WebApplicationFactory` ed eseguire l'override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a>Disabilitare la creazione di copie shadow

Creazione di copie shadow fa sì che i test da eseguire in una cartella diversa da quella cartella di output. Per i test per il corretto funzionamento, è necessario disabilitare la copia shadow. Il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) Usa xUnit e disabilita la creazione di copie shadow per xUnit includendo un' *xunit.runner.json* file con l'impostazione di configurazione corretto. Per altre informazioni, vedere [configurazione xUnit.net con JSON](https://xunit.github.io/docs/configuring-with-json.html).

Aggiungere il *xunit.runner.json* file alla radice del progetto di test con il contenuto seguente:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Esempio di test di integrazione

Il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) è costituito da due App:

| App | Cartella del progetto | Descrizione |
| --- | -------------- | ----------- |
| App di messaggio (il SUT) | *src/RazorPagesProject* | Consente all'utente di aggiungere, eliminare uno, eliminare tutti e analizzare i messaggi. |
| Testare l'app | *tests/RazorPagesProject.Tests* | Usato per test di integrazione di SUT. |

I test possono essere eseguiti usando le funzionalità di test predefinito di un ambiente IDE, ad esempio [Visual Studio](https://www.visualstudio.com/vs/). Se si usa [Visual Studio Code](https://code.visualstudio.com/) o la riga di comando, eseguire il comando seguente al prompt dei comandi nel *tests/RazorPagesProject.Tests* cartella:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Organizzazione delle app (SUT) messaggio

Il SUT è un sistema di messaggi di Razor Pages con le caratteristiche seguenti:

* La pagina di indice dell'app (*cshtml* e *Pages/Index.cshtml.cs*) fornisce un'interfaccia utente e pagina alcuni metodi del modello per controllare l'aggiunta, eliminazione e l'analisi dei messaggi (parole medie per ogni messaggio) .
* Un messaggio è descritta dal `Message` classe (*Data/Message.cs*) con due proprietà: `Id` (chiave) e `Text` (messaggio). Il `Text` proprietà è necessaria e limitata a 200 caratteri.
* I messaggi vengono archiviati usando [database in memoria di Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* L'app contiene un livello di accesso ai dati (DAL) nella relativa classe di contesto di database, `AppDbContext` (*Data/AppDbContext.cs*).
* Se il database è vuoto all'avvio dell'app, l'archivio di messaggi viene inizializzata con tre messaggi.
* L'app include un `/SecurePage` che è accessibile solo da un utente autenticato.

&#8224;L'argomento relativo alla EF [Test con InMemory](/ef/core/miscellaneous/testing/in-memory), spiega come usare un database in memoria per i test con MSTest. Questo argomento viene utilizzato il [xUnit](https://xunit.github.io/) framework di test. Concetti di test e test implementazioni su Framework di test diversi sono simili ma non identici.

Anche se l'app non usa il modello di repository e non è un esempio efficace del [schema Unit of Work (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), pagine Razor supporta questi modelli di sviluppo. Per altre informazioni, vedere [la progettazione del livello di persistenza dell'infrastruttura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) e [logica dei controller di Test](/aspnet/core/mvc/controllers/testing) (l'esempio implementa il modello di repository).

### <a name="test-app-organization"></a>Organizzazione dei test app

L'app di test è un'app console all'interno di *tests/RazorPagesProject.Tests* cartella.

| Cartella di app di test | Descrizione |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* contiene metodi di test per il routing, l'accesso a una pagina protetta da un utente non autenticato e ottenere un profilo utente di GitHub e il controllo di accesso utente del profilo. |
| *IntegrationTests* | *IndexPageTests.cs* contiene i test di integrazione per la pagina di indice usando custom `WebApplicationFactory` classe. |
| *Gli helper/Utilities* | <ul><li>*Utilities.cs* contiene il `InitializeDbForTests` metodo usato per effettuare il seeding del database con dati di test.</li><li>*HtmlHelpers.cs* fornisce un metodo per restituire un AngleSharp `IHtmlDocument` per l'uso da metodi di test.</li><li>*HttpClientExtensions.cs* forniscono overload per `SendAsync` per inviare le richieste di SUT.</li></ul> |

Il framework di test viene [xUnit](https://xunit.github.io/). Test di integrazione vengono effettuati con il [testhost](/dotnet/api/microsoft.aspnetcore.testhost), che include il [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Poiché il [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) pacchetto viene utilizzato per configurare il server host e test di test, il `TestHost` e `TestServer` pacchetti non richiedono i riferimenti ai pacchetti direttamente nel file di progetto di test dell'app o configurazione per gli sviluppatori nell'app di test.

**Il seeding del database per i test**

Test di integrazione in genere richiedono un piccolo set di dati nel database prima dell'esecuzione di test. Ad esempio, un'operazione di eliminazione testare le chiamate per un'eliminazione di record di database, in modo che il database deve avere almeno un record per la richiesta di eliminazione abbia esito positivo.

L'app di esempio esegue il seeding del database con i tre messaggi *Utilities.cs* che i test è possono usare quando vengono eseguite:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Gli unit test](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Unit test per Razor Pages](xref:test/razor-pages-tests)
* [Middleware](xref:fundamentals/middleware/index)
* [Test controller](xref:mvc/controllers/testing)
