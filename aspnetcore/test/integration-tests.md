---
title: Test di integrazione in ASP.NET Core
author: guardrex
description: Informazioni su come i test di integrazione garantiscono che i componenti dell'app funzionano correttamente a livello di infrastruttura, tra cui database, file system e rete.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/05/2019
uid: test/integration-tests
ms.openlocfilehash: a86bf2b183a81f0b903a12f9d1660fb32faa6c03
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819937"
---
# <a name="integration-tests-in-aspnet-core"></a>Test di integrazione in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex) e [Steve Smith](https://ardalis.com/)

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

Gli unit test utilizzano componenti fabbricati, noti come Fakes o *oggetti fittizi*, al posto dei componenti dell'infrastruttura.

Diversamente dagli unit test, i test di integrazione:

* Usare i componenti effettivi usati dall'app nell'ambiente di produzione.
* Richiedere più codice e l'elaborazione dei dati.
* Richiedere più tempo per l'esecuzione.

Pertanto, limitare l'utilizzo dei test di integrazione ai più importanti scenari di infrastruttura. Se è possibile testare un comportamento utilizzando un unit test o un test di integrazione, scegliere il unit test.

> [!TIP]
> Non scrivere test di integrazione per ogni possibile permutazione di dati e accesso ai file con database e file System. Indipendentemente dal numero di posizioni in un'app che interagiscono con i database e i file System, un set mirato di test di integrazione di lettura, scrittura, aggiornamento ed eliminazione è in genere in grado di testare adeguatamente i componenti di database e file system. Usare gli unit test per i test di routine della logica del metodo che interagiscono con questi componenti. Negli unit test, l'uso di Fakes/Mocks dell'infrastruttura comporta una maggiore esecuzione dei test.

> [!NOTE]
> Nelle discussioni sui test di integrazione, il progetto testato viene spesso definito *sistema*sottoposto a test oppure "Sut" per brevità.

## <a name="aspnet-core-integration-tests"></a>Test di integrazione di ASP.NET Core

I test di integrazione in ASP.NET Core richiedono gli elementi seguenti:

* Un progetto di test viene utilizzato per contenere ed eseguire i test. Il progetto di test contiene un riferimento al progetto ASP.NET Core testato, denominato sistema sottoposto a *test* (SUT). _"SUT" viene usato in questo argomento per fare riferimento all'app testata._
* Il progetto di test crea un host Web di test per SUT e utilizza un client del server di prova per gestire le richieste e le risposte a SUT.
* Viene usato un test runner per eseguire i test e segnalare i risultati del test.

I test di integrazione seguono una sequenza di eventi che includono i normali passi del test *Arrange*, *Act*e *Assert* :

1. L'host Web di SUT è configurato.
1. Viene creato un client del server di prova per inviare richieste all'app.
1. Il passaggio di *disposizione* del test viene eseguito: L'app di test prepara una richiesta.
1. Il passaggio del test *Act* viene eseguito: Il client invia la richiesta e riceve la risposta.
1. Viene eseguito il passaggio del test *Assert* : La risposta *effettiva* viene convalidata come *superato* o *non superato* in base a una risposta *prevista* .
1. Il processo continua fino a quando non vengono eseguiti tutti i test.
1. Vengono restituiti i risultati del test.

In genere, l'host Web di test viene configurato in modo diverso rispetto al normale host web dell'app per le esecuzioni dei test. Ad esempio, è possibile usare un database diverso o impostazioni di app diverse per i test.

I componenti dell'infrastruttura, ad esempio l'host Web di test e il server di prova in memoria ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), vengono forniti o gestiti dal pacchetto [Microsoft. AspNetCore. Mvc. testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) . L'utilizzo di questo pacchetto semplifica la creazione e l'esecuzione di test.

Il `Microsoft.AspNetCore.Mvc.Testing` pacchetto gestisce le attività seguenti:

* Copia il file delle dipendenze ( *\*. Deps*) da SUT nella directory *bin* del progetto di test.
* Imposta la radice del contenuto sulla radice del progetto di SUT in modo che i file e le pagine e le visualizzazioni statiche vengano rilevati durante l'esecuzione dei test.
* Fornisce la classe [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) per semplificare l'avvio del SUT con `TestServer`.

Nella documentazione relativa agli [unit test](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) viene descritto come configurare un progetto di test e un test runner, oltre a istruzioni dettagliate su come eseguire i test e le indicazioni su come denominare i test e le classi di test.

> [!NOTE]
> Quando si crea un progetto di test per un'app, separare gli unit test dai test di integrazione in progetti diversi. Ciò consente di garantire che i componenti di test dell'infrastruttura non vengano accidentalmente inclusi negli unit test. La separazione dei test di unità e integrazione consente inoltre di controllare il set di test da eseguire.

Non esiste praticamente alcuna differenza tra la configurazione per i test delle app Razor Pages e delle app MVC. L'unica differenza consiste nel modo in cui i test vengono denominati. In un'app Razor Pages, i test degli endpoint della pagina vengono in genere denominati dopo la classe del modello `IndexPageTests` di pagina, ad esempio per testare l'integrazione dei componenti per la pagina di indice. In un'app MVC i test sono in genere organizzati per classi controller e denominati dopo i controller testati, `HomeControllerTests` ad esempio per testare l'integrazione dei componenti per il controller Home.

## <a name="test-app-prerequisites"></a>Test prerequisiti app

Il progetto di test deve:

* Fare riferimento ai pacchetti seguenti:
  * [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  * [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* Specificare il Web SDK nel file di progetto (`<Project Sdk="Microsoft.NET.Sdk.Web">`). Il Web SDK è obbligatorio quando si fa riferimento al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

Questi prerequisiti possono essere visualizzati nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/). Esaminare il file *tests/RazorPagesProject. tests/RazorPagesProject. tests. csproj* . L'app di esempio usa il Framework di test [xUnit](https://xunit.github.io/) e la libreria del parser [AngleSharp](https://anglesharp.github.io/) , quindi l'app di esempio fa riferimento anche a:

* [xunit](https://www.nuget.org/packages/xunit/)
* [xunit.runner.visualstudio](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [AngleSharp](https://www.nuget.org/packages/AngleSharp/)

## <a name="sut-environment"></a>Ambiente SUT

Se l' [ambiente](xref:fundamentals/environments) di Sut non è impostato, il valore predefinito dell'ambiente è Development.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Test di base con WebApplicationFactory predefinito

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) viene usato per creare un [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) per i test di integrazione. `TEntryPoint`è la classe del punto di ingresso di Sut, in `Startup` genere la classe.

Le classi di test implementano una *classe fixture* Interface ([IClassFixture](https://xunit.github.io/docs/shared-context#class-fixture)) per indicare che la classe contiene test e fornisce istanze di oggetti condivisi tra i test nella classe.

### <a name="basic-test-of-app-endpoints"></a>Test di base degli endpoint dell'app

La classe `WebApplicationFactory` di test seguente `BasicTests`,, USA per avviare il SUT e fornire un [HttpClient](/dotnet/api/system.net.http.httpclient) a un metodo di test `Get_EndpointsReturnSuccessAndCorrectContentType`,. Il metodo controlla se il codice di stato della risposta ha esito positivo (codici di stato nell'intervallo `Content-Type` 200-299) `text/html; charset=utf-8` e l'intestazione è per diverse pagine dell'app.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) crea un'istanza di `HttpClient` che segue automaticamente i reindirizzamenti e gestisce i cookie.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

Per impostazione predefinita, i cookie non essenziali non vengono conservati nelle richieste quando i [criteri di consenso GDPR](xref:security/gdpr) sono abilitati. Per mantenere cookie non essenziali, ad esempio quelli usati dal provider TempData, contrassegnarli come essenziali nei test. Per istruzioni su come contrassegnare un cookie come essenziale, vedere [cookie essenziali](xref:security/gdpr#essential-cookies).

### <a name="test-a-secure-endpoint"></a>Testare un endpoint sicuro

Un altro test della `BasicTests` classe verifica che un endpoint sicuro reindirizza un utente non autenticato alla pagina di accesso dell'app.

In Sut la `/SecurePage` pagina usa una convenzione [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) per applicare un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina. Per ulteriori informazioni, vedere [Razor Pages convenzioni di autorizzazione](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

Nel test un [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) è impostato in modo da non consentire i reindirizzamenti impostando [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) su `false`: `Get_SecurePageRequiresAnAuthenticatedUser`

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Se si impedisce al client di seguire il reindirizzamento, è possibile effettuare i controlli seguenti:

* Il codice di stato restituito da SUT può essere controllato rispetto al risultato [HttpStatusCode. Redirect](/dotnet/api/system.net.httpstatuscode) previsto, non al codice di stato finale dopo il reindirizzamento alla pagina di accesso, che sarebbe [HttpStatusCode. OK](/dotnet/api/system.net.httpstatuscode).
* Il `Location` valore dell'intestazione nelle intestazioni della risposta viene controllato per confermare che inizia con `http://localhost/Identity/Account/Login`, non con la risposta finale della pagina di accesso `Location` , in cui l'intestazione non è presente.

Per ulteriori informazioni su `WebApplicationFactoryClientOptions`, vedere la sezione [Opzioni client](#client-options) .

## <a name="customize-webapplicationfactory"></a>Personalizzare WebApplicationFactory

La configurazione dell'host Web può essere creata indipendentemente dalle classi di test ereditando `WebApplicationFactory` da per creare una o più factory personalizzate:

1. Ereditare `WebApplicationFactory` da ed eseguire l'override di [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) consente la configurazione della raccolta di servizi con [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Il`InitializeDbForTests` seeding del database nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) viene eseguito dal metodo. Il metodo è descritto nell'esempio [relativo ai test di integrazione: Sezione test organizzazione](#test-app-organization) app.

2. Usare l'oggetto `CustomWebApplicationFactory` personalizzato nelle classi di test. Nell'esempio seguente viene usata la Factory nella `IndexPageTests` classe:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Il client dell'app di esempio è configurato per impedire `HttpClient` l'esecuzione dei reindirizzamenti seguenti. Come illustrato nella sezione [testare un endpoint sicuro](#test-a-secure-endpoint) , questo consente ai test di verificare il risultato della prima risposta dell'app. La prima risposta è un reindirizzamento in molti di questi test con un' `Location` intestazione.

3. Un test tipico usa i `HttpClient` metodi helper e per elaborare la richiesta e la risposta:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Qualsiasi richiesta POST a SUT deve soddisfare il controllo antifalsificazione creato automaticamente dal sistema antifalsificazione di [protezione dei dati](xref:security/data-protection/introduction)dell'app. Per disporre la richiesta POST di un test, l'app di test deve:

1. Effettuare una richiesta per la pagina.
1. Analizzare il cookie antifalsificazione e richiedere il token di convalida dalla risposta.
1. Eseguire la richiesta POST con il cookie antifalsificazione e il token di convalida della richiesta sul posto.

I `SendAsync` metodi di estensione Helper (*Helper/Metodo HttpClientExtensions. cs*) `GetDocumentAsync` e il metodo helper (*Helper/htmlhelpers. cs*) nell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples/) usano il parser [AngleSharp](https://anglesharp.github.io/) per gestire l'antifalsificazione verificare con i metodi seguenti:

* `GetDocumentAsync`Riceve il [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) e restituisce un `IHtmlDocument`oggetto. &ndash; `GetDocumentAsync`Usa una factory che prepara una *risposta virtuale* basata sull'originale `HttpResponseMessage`. Per ulteriori informazioni, vedere la [documentazione di AngleSharp](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync`i `HttpClient` metodi di estensione per compongono un [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) e chiamano [SendAsync (HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) per inviare richieste a SUT. Gli overload per `SendAsync` accettano il formato HTML (`IHtmlFormElement`) e gli elementi seguenti:
  * Pulsante Invia nel formato (`IHtmlElement`)
  * Raccolta di valori di`IEnumerable<KeyValuePair<string, string>>`form ()
  * Pulsante Invia (`IHtmlElement`) e valori form (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) è una libreria di analisi di terze parti usata a scopo dimostrativo in questo argomento e nell'app di esempio. AngleSharp non è supportato o richiesto per i test di integrazione delle app ASP.NET Core. È possibile usare altri parser, ad esempio [HTML Agility Pack (HAP)](https://html-agility-pack.net/). Un altro approccio consiste nel scrivere codice per gestire direttamente il token di verifica delle richieste del sistema antifalsificazione e il cookie antifalsificazione.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Personalizzare il client con WithWebHostBuilder

Quando è richiesta una configurazione aggiuntiva in un metodo di test, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) crea un nuovo oggetto `WebApplicationFactory` con un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) che viene ulteriormente personalizzato in base alla configurazione.

Il `Post_DeleteMessageHandler_ReturnsRedirectToRoot` metodo di test dell' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) illustra l'uso `WithWebHostBuilder`di. Questo test esegue l'eliminazione di un record nel database attivando un invio di form in SUT.

Poiché un altro test della `IndexPageTests` classe esegue un'operazione che elimina tutti i record nel database e può essere eseguita prima del `Post_DeleteMessageHandler_ReturnsRedirectToRoot` metodo, il database viene sottoposta a seeding in questo metodo di test per assicurarsi che sia presente un record per l'eliminazione da parte di Sut. La selezione del `messages`pulsantedel form in SUT viene simulata nella richiesta al SUT: `deleteBtn1`

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Opzioni client

Nella tabella seguente viene illustrato il valore predefinito di [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) disponibile durante la creazione di istanze `HttpClient`.

| Opzione | Descrizione | Predefinito |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Ottiene o imposta un valore che `HttpClient` indica se le istanze devono seguire automaticamente le risposte di reindirizzamento. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Ottiene o imposta l'indirizzo di base `HttpClient` delle istanze di. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Ottiene o imposta un `HttpClient` valore che indica se le istanze devono gestire i cookie. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Ottiene o imposta il numero massimo di risposte di Reindirizzamento `HttpClient` che le istanze devono seguire. | 7 |

Creare la `WebApplicationFactoryClientOptions` classe e passarla al metodo [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) (i valori predefiniti sono mostrati nell'esempio di codice):

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

È possibile eseguire l'override dei servizi in un test con una chiamata a [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) nel generatore host. **Per inserire i servizi fittizi, Sut deve disporre di `Startup` una classe con `Startup.ConfigureServices` un metodo.**

L'esempio SUT include un servizio con ambito che restituisce un'offerta. La virgoletta è incorporata in un campo nascosto nella pagina di indice quando viene richiesta la pagina di indice.

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

Quando viene eseguita l'app SUT, viene generato il markup seguente:

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

Per testare il servizio e l'inserimento di virgolette in un test di integrazione, un servizio fittizio viene inserito in SUT dal test. Il servizio fittizio sostituisce l'app `QuoteService` con un servizio fornito dall'app di test, denominato `TestQuoteService`:

*IntegrationTests.IndexPageTests.cs*:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

`ConfigureTestServices`viene chiamato e il servizio con ambito viene registrato:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

Il markup prodotto durante l'esecuzione del test riflette il testo tra virgolette `TestQuoteService`fornito da, quindi l'asserzione passa:

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Come l'infrastruttura di test deduce il percorso radice del contenuto dell'app

Il `WebApplicationFactory` Costruttore deduce il percorso radice del contenuto dell'app cercando un [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) nell'assembly contenente i test di integrazione con una `TEntryPoint` chiave uguale all'assembly. `System.Reflection.Assembly.FullName` Se non viene trovato un attributo con la chiave corretta, `WebApplicationFactory` viene eseguito il fallback alla ricerca di un file di soluzione ( *\*con estensione sln*) `TEntryPoint` e il nome dell'assembly viene aggiunto alla directory della soluzione. La directory radice dell'app (percorso radice del contenuto) viene usata per individuare le visualizzazioni e i file di contenuto.

## <a name="disable-shadow-copying"></a>Disabilitare la copia shadow

La copia shadow comporta l'esecuzione dei test in una directory diversa rispetto alla directory di output. Per il corretto funzionamento dei test, è necessario disabilitare la copia shadow. L' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) USA xUnit e Disabilita la copia shadow per xUnit includendo un file *xUnit. Runner. JSON* con l'impostazione di configurazione corretta. Per altre informazioni, vedere [configurazione di xUnit con JSON](https://xunit.github.io/docs/configuring-with-json.html).

Aggiungere il file *xUnit. Runner. JSON* alla radice del progetto di test con il contenuto seguente:

```json
{
  "shadowCopy": false
}
```

## <a name="disposal-of-objects"></a>Eliminazione di oggetti

Dopo l'esecuzione dei test `IClassFixture` dell'implementazione, [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) e [HttpClient](/dotnet/api/system.net.http.httpclient) vengono eliminati quando xUnit Elimina [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1). Se gli oggetti di cui è stata creata un'istanza dallo sviluppatore richiedono l'eliminazione `IClassFixture` , eliminarli nell'implementazione. Per ulteriori informazioni, vedere [implementazione di un metodo Dispose](/dotnet/standard/garbage-collection/implementing-dispose).

## <a name="integration-tests-sample"></a>Esempio di test di integrazione

L' [app di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/integration-tests/samples) è costituita da due app:

| App | Directory del progetto | Descrizione |
| --- | ----------------- | ----------- |
| App Message (SUT) | *src/RazorPagesProject* | Consente a un utente di aggiungere, eliminare, eliminare tutti i messaggi e analizzarli. |
| App di test | *tests/RazorPagesProject.Tests* | Utilizzato per eseguire il test di integrazione di SUT. |

I test possono essere eseguiti usando le funzionalità di test predefinite di un IDE, ad esempio [Visual Studio](https://visualstudio.microsoft.com). Se si usa [Visual Studio Code](https://code.visualstudio.com/) o la riga di comando, eseguire il comando seguente al prompt dei comandi nella directory *tests/RazorPagesProject. tests* :

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Organizzazione Message app (SUT)

SUT è un sistema di messaggi di Razor Pages con le seguenti caratteristiche:

* La pagina di indice dell'app (*pages/index. cshtml* e *pages/index. cshtml. cs*) fornisce i metodi di interfaccia utente e modello di pagina per controllare l'aggiunta, l'eliminazione e l'analisi dei messaggi (media parole per messaggio).
* Un messaggio `Message` viene descritto dalla classe (*Data/Message. cs*) con due proprietà: `Id` (chiave) e `Text` (messaggio). La `Text` proprietà è obbligatoria e limitata a 200 caratteri.
* I messaggi vengono archiviati utilizzando&#8224; [il database in memoria di Entity Framework](/ef/core/providers/in-memory/).
* L'app contiene un livello di accesso ai dati (dal) nella classe del contesto `AppDbContext` di database, (*Data/AppDbContext. cs*).
* Se il database è vuoto all'avvio dell'app, l'archivio messaggi viene inizializzato con tre messaggi.
* L'app include un `/SecurePage` oggetto a cui è possibile accedere solo da un utente autenticato.

&#8224;L'argomento EF, [test con InMemory](/ef/core/miscellaneous/testing/in-memory), spiega come usare un database in memoria per i test con MSTest. In questo argomento viene usato il Framework di test di [xUnit](https://xunit.github.io/) . I concetti di test e le implementazioni di test in diversi framework di test sono simili, ma non identici.

Anche se l'app non usa il modello di repository e non è un esempio efficace del [modello di unità di lavoro (UOW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supporta questi modelli di sviluppo. Per altre informazioni, vedere [progettazione del livello](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) di persistenza dell'infrastruttura e della [logica del controller di test](/aspnet/core/mvc/controllers/testing) (l'esempio implementa il modello di repository).

### <a name="test-app-organization"></a>Testare l'organizzazione dell'app

L'app di test è un'app console all'interno della directory *tests/RazorPagesProject. tests* .

| Testare la directory dell'app | DESCRIZIONE |
| ------------------ | ----------- |
| *BasicTests* | *BasicTests.cs* contiene metodi di test per il routing, l'accesso a una pagina sicura da parte di un utente non autenticato e il recupero di un profilo utente GitHub e il controllo dell'accesso utente del profilo. |
| *IntegrationTests* | *IndexPageTests.cs* contiene i test di integrazione per la pagina di indice `WebApplicationFactory` utilizzando la classe personalizzata. |
| *Helper/utilità* | <ul><li>*Utilities.cs* contiene il `InitializeDbForTests` metodo utilizzato per eseguire il seeding del database con dati di test.</li><li>*HtmlHelpers.cs* fornisce un metodo per restituire un AngleSharp `IHtmlDocument` per l'uso da parte dei metodi di test.</li><li>*HttpClientExtensions.cs* forniscono overload per `SendAsync` per inviare richieste a SUT.</li></ul> |

Il Framework di test è [xUnit](https://xunit.github.io/). I test di integrazione vengono eseguiti usando [Microsoft. AspNetCore. TestHost](/dotnet/api/microsoft.aspnetcore.testhost), che include [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Poiché il pacchetto [Microsoft. AspNetCore. Mvc. testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) viene usato per configurare l'host di test e il server `TestHost` di `TestServer` prova, i pacchetti e non richiedono riferimenti diretti al pacchetto nel file di progetto o nello sviluppatore dell'app di test configurazione nell'app di test.

**Seeding del database per il test**

I test di integrazione richiedono in genere un set di dati di piccole dimensioni nel database prima dell'esecuzione del test. Ad esempio, un test di eliminazione richiede l'eliminazione di un record del database, pertanto è necessario che il database disponga di almeno un record affinché la richiesta di eliminazione abbia esito positivo.

L'app di esempio esegue il seeding del database con tre messaggi in *Utilities.cs* che possono essere usati dai test quando vengono eseguiti:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Unit test](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Unit test per Razor Pages](xref:test/razor-pages-tests)
* [Middleware](xref:fundamentals/middleware/index)
* [Test controller](xref:mvc/controllers/testing)
