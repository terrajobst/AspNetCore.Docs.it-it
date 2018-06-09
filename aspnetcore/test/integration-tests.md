---
title: Test di integrazione in ASP.NET Core
author: guardrex
description: Informazioni su come i test di integrazione garantire che i componenti dell'app funzionano correttamente a livello di infrastruttura, tra cui il database, file system e rete.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: test/integration-tests
ms.openlocfilehash: a402c3f5f6a75917eba4058e6cc6926f25b214d4
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "35217686"
---
# <a name="integration-tests-in-aspnet-core"></a>Test di integrazione in ASP.NET Core

Dal [Luke Latham](https://github.com/guardrex) e [Steve Smith](https://ardalis.com/)

Test di integrazione di garantire che i componenti dell'applicazione funzionano correttamente a un livello che include l'infrastruttura di supporto dell'app, ad esempio il database, file system e rete. ASP.NET Core supporta i test di integrazione tramite un framework di unit test con un host di test web e un server di prova in memoria.

Questo argomento si presuppone una conoscenza di base di unit test. Se non si ha dimestichezza con i concetti di test, vedere la [Unit test in .NET Core e .NET Standard](/dotnet/core/testing/) argomento e il relativo contenuto collegato.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

L'app di esempio è un'app di pagine Razor e presuppone una conoscenza di base di pagine Razor. Se non si ha dimestichezza con le pagine Razor, vedere gli argomenti seguenti:

* [Introduzione a Razor Pages in ASP.NET Core](xref:mvc/razor-pages/index)
* [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Razor pagine unit test](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>Introduzione ai test di integrazione

Test di integrazione di valutare i componenti dell'app su un livello più ampio rispetto [unit test](/dotnet/core/testing/). Consente di testare i componenti software isolata, ad esempio i metodi della classe singoli unit test. Test di integrazione di confermare che due o più componenti delle app interagiscano per produrre un risultato previsto, magari con ogni componente necessario per l'elaborazione completa di una richiesta.

Questi test più ampi vengono usati per testare l'app dell'infrastruttura e intero framework, spesso con i componenti seguenti:

* Database
* File system
* Ai dispositivi di rete
* Pipeline di richiesta-risposta

Gli unit test componenti utilizzo creato, noti come *fakes* oppure *simulare oggetti*, al posto di componenti dell'infrastruttura.

A differenza di unit test, test di integrazione:

* Utilizzare i componenti effettivi che usa l'app nell'ambiente di produzione.
* Richiedono più codice e l'elaborazione dati.
* Richiedere più tempo per l'esecuzione.

Di conseguenza, limitare l'utilizzo di test di integrazione per scenari dell'infrastruttura più importanti. Se un comportamento può essere testato usando uno unit test o un test di integrazione, scegliere lo unit test.

> [!TIP]
> Non scrivere i test di integrazione per ogni permutazione possibili dei dati e file di access con database e file System. Indipendentemente da quante inserisce in un'app interagire con i database e file System, un set con lo stato attivo di lettura, scrittura, aggiornamento ed eliminazione integrazione test sono in genere in grado di database di test in modo adeguato e componenti del file system. Utilizzo di unit test per test routine della logica del metodo che interagiscono con questi componenti. Negli unit test, l'utilizzo dell'infrastruttura fakes/mocks comportare l'esecuzione dei test più veloce.

> [!NOTE]
> Quando si parla di test di integrazione, è spesso definito il progetto testato il *sistema sottoposto a test*, o "SUT", nella forma abbreviata.

## <a name="aspnet-core-integration-tests"></a>Test di integrazione ASP.NET Core

I test di integrazione in ASP.NET Core necessario quanto segue:

* Un progetto di test viene utilizzato per contenere ed eseguire i test. Il progetto di test contiene un riferimento al progetto ASP.NET Core testato, denominato il *sistema sottoposto a test* (SUT). _"SUT" viene utilizzata in questo argomento per fare riferimento all'app testata._
* Il progetto di test crea un host del test web per il SUT e utilizza un client di server di test per gestire le richieste e risposte per la SUT.
* Un test runner viene utilizzato per eseguire il test e il report, i risultati del test.

I test di integrazione seguono una sequenza di eventi che includono le consuete *disponi*, *Act*, e *Assert* passi di test:

1. Host web del SUT è configurato.
1. Un client di server di prova viene creato per inviare richieste all'app.
1. Il *disponi* passo del test viene eseguito: una richiesta di preparazione dell'app di test.
1. Il *Act* passo del test viene eseguito: il client invia la richiesta e riceve la risposta.
1. Il *Assert* passo del test viene eseguito: il *effettivo* risposta viene convalidata come una *passare* o *esito negativo* in base a un *previsto*  risposta.
1. Il processo continua fino a tutti i test vengono eseguiti.
1. Vengono segnalati i risultati del test.

In genere, l'host del test web è configurato in modo diverso rispetto a dell'host dell'applicazione web normale per il test viene eseguito. Ad esempio, un database diverso o le impostazioni dell'app diverse possono essere utilizzate per i test.

Componenti dell'infrastruttura, ad esempio l'host del test web e server di prova in memoria ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), vengono forniti o gestiti dal [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) pacchetto. Utilizzo di questo pacchetto semplifica la creazione di test e l'esecuzione.

Il `Microsoft.AspNetCore.Mvc.Testing` pacchetto gestisce le attività seguenti:

* Copia il file di dipendenze (*\*.deps*) da SUT nel progetto di test *bin* cartella.
* Imposta la radice del contenuto alla radice del progetto del SUT in modo che i file statici e pagine o nelle viste vengono trovate quando vengono eseguiti i test.
* Fornisce la [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) classe per semplificare l'avvio automatico SUT con `TestServer`.

Il [unit test](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentazione viene descritto come configurare un test progetto e test runner, insieme a istruzioni dettagliate su come eseguire test e le indicazioni relative ai test di nome e classi di test.

> [!NOTE]
> Quando si crea un progetto di test per un'app, separare gli unit test dei test di integrazione in progetti diversi. Ciò garantisce che componenti dell'infrastruttura di test non accidentalmente sono inclusi negli unit test. La separazione di unit test e integrazione test consente anche di controllo su quali set di test vengono eseguiti.

Non è praticamente alcuna differenza tra la configurazione per i test delle pagine Razor App e App MVC. L'unica differenza è in modalità di denominazione dei test. In un'app di pagine Razor, sono in genere denominati test degli endpoint di pagina dopo la classe di modello di pagina (ad esempio, `IndexPageTests` per verificare l'integrazione di componenti per la pagina di indice). In un'applicazione MVC, i test sono in genere organizzati per classi controller e denominati dopo i controller eseguono il test (ad esempio, `HomeControllerTests` per verificare l'integrazione di componenti per il controller Home).

## <a name="test-app-prerequisites"></a>Prerequisiti di app di test

Il progetto di test deve:

* Disporre di un riferimento pacchetto per [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/).
* Usare il SDK Web nel file di progetto (`<Project Sdk="Microsoft.NET.Sdk.Web">`).

Questi prerequesities possono essere visti nel [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/). Controllare la *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>Test di base con il valore predefinito WebApplicationFactory

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) viene utilizzato per creare un [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) per i test di integrazione. `TEntryPoint` è la classe del punto di ingresso di SUT, in genere il `Startup` classe.

Test sono classi che implementano un *fixture di classe* interfaccia (`IClassFixture`) per indicare la classe contiene i test e fornisce alle istanze di oggetti condivisi tra i diversi test nella classe.

### <a name="basic-test-of-app-endpoints"></a>Test di base dell'endpoint dell'app

Il seguente test (classe), `BasicTests`, Usa la `WebApplicationFactory` per avviare il SUT e fornire un [HttpClient](/dotnet/api/system.net.http.httpclient) a un metodo di test, `Get_EndpointsReturnSuccessAndCorrectContentType`. Il metodo controlla se il codice di stato risposta ha esito positivo (codici di stato nell'intervallo di 200 299) e il `Content-Type` intestazione `text/html; charset=utf-8` per diverse pagine dell'app.

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) crea un'istanza di `HttpClient` che segue i reindirizzamenti di automaticamente e gestisce i cookie.

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>Testare un endpoint protetto

Un altro test nel `BasicTests` classe verifica che un endpoint protetto reindirizza un utente non autenticato alla pagina di accesso dell'app.

In SUT, il `/SecurePage` pagina utilizzi un [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenzione di chiamata per applicare un [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) alla pagina. Per altre informazioni, vedere [convenzioni autorizzazione pagine Razor](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

Nel `Get_SecurePageRequiresAnAuthenticatedUser` testare, una [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) è impostato per non consentire reindirizzamenti impostando [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) a `false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

Disabilitando il client per eseguire il reindirizzamento, è possono effettuare i controlli seguenti:

* È possibile controllare il codice di stato restituito dal SUT rispetto a quella prevista [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) risultati, non il codice di stato finale dopo il reindirizzamento alla pagina di accesso, che dovrebbe essere [HttpStatusCode](/dotnet/api/system.net.httpstatuscode).
* Il `Location` valore di intestazione nelle intestazioni di risposta viene controllata per verificare che inizia con `http://localhost/Identity/Account/Login`, non l'account di accesso pagina risposta finale, in cui il `Location` intestazione sarebbe presenta.

Per ulteriori informazioni sul `WebApplicationFactoryClientOptions`, vedere la [opzioni Client](#client-options) sezione.

## <a name="customize-webapplicationfactory"></a>Personalizzare WebApplicationFactory

Configurazione host Web possa essere creati indipendentemente dalle classi di test tramite l'eredità da `WebApplicationFactory` per creare uno o più factory personalizzate:

1. Ereditare da `WebApplicationFactory` ed eseguire l'override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost). Il [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) consente la configurazione della raccolta di servizio con [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   Il seeding nel database di [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) viene eseguita dal `InitializeDbForTests` metodo. Il metodo descritto nel [esempio di test di integrazione: organizzazione di app di Test](#test-app-organization) sezione.

2. Utilizzare l'oggetto personalizzato `CustomWebApplicationFactory` nelle classi di test. L'esempio seguente usa la factory nel `IndexPageTests` classe:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   Client dell'app di esempio è configurato per impedire la `HttpClient` da reindirizzamenti seguenti. Come spiegato nel [testare un endpoint protetto](#test-a-secure-endpoint) sezione, in tal modo i test per verificare il risultato della prima risposta dell'app. La prima risposta è un reindirizzamento in molti di questi test con un `Location` intestazione.

3. Usa un test tipico di `HttpClient` e i metodi helper per elaborare la richiesta e risposta:

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

Qualsiasi richiesta POST per il SUT deve soddisfare il controllo non riproducibili automaticamente effettuato tramite l'app [sistema non riproducibili protezione dei dati](xref:security/data-protection/introduction). Per disporre di una richiesta POST di un test, è necessario l'app di test:

1. Effettuare una richiesta per la pagina.
1. Analizzare i cookie non riproducibili e un token di convalida richiesta dalla risposta.
1. Eseguire la richiesta POST con la convalida non riproducibili cookie e richiesta di token sul posto.

Il `SendAsync` metodi di estensione helper (*Helpers/HttpClientExtensions.cs*) e il `GetDocumentAsync` metodo helper (*Helpers/HtmlHelpers.cs*) nei [appdiesempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) usare il [AngleSharp](https://anglesharp.github.io/) parser per gestire la verifica non riproducibili con i metodi seguenti:

* `GetDocumentAsync` &ndash; Riceve il [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) e restituisce un `IHtmlDocument`. `GetDocumentAsync` Usa una factory che consente di preparare una *virtuale risposta* basato sull'originale `HttpResponseMessage`. Per altre informazioni, vedere la [AngleSharp documentazione](https://github.com/AngleSharp/AngleSharp#documentation).
* `SendAsync` metodi di estensione per il `HttpClient` comporre un [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) e chiamare [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) per inviare le richieste di SUT. Gli overload per `SendAsync` accettare il form HTML (`IHtmlFormElement`) e le seguenti:
  - Pulsante del form di invio (`IHtmlElement`)
  - Insieme di valori di modulo (`IEnumerable<KeyValuePair<string, string>>`)
  - Pulsante di invio (`IHtmlElement`) e i valori del modulo (`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/) è una terza parte durante l'analisi della libreria utilizzata per scopi dimostrativi in questo argomento e l'app di esempio. AngleSharp non supportati o richiesti per il test di integrazione di applicazioni ASP.NET Core. Altro parser può essere utilizzato, ad esempio il [Html agilità Pack (HAP)](http://html-agility-pack.net/). Un altro approccio consiste nello scrivere codice per gestire direttamente i token di verifica richiesta il sistema non riproducibili e cookie non riproducibili.

## <a name="customize-the-client-with-withwebhostbuilder"></a>Personalizzare il client con WithWebHostBuilder

Quando è richiesto all'interno di un metodo di test, una configurazione aggiuntiva [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) crea un nuovo `WebApplicationFactory` con un [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ulteriormente personalizzato dalla configurazione.

Il `Post_DeleteMessageHandler_ReturnsRedirectToRoot` metodo di test di [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) illustra l'uso di `WithWebHostBuilder`. Questo test esegue un'operazione di eliminazione record nel database attivando l'invio di un form nel SUT.

Perché un altro test nel `IndexPageTests` classe esegue un'operazione che elimina tutti i record nel database e possono essere eseguite prima di `Post_DeleteMessageHandler_ReturnsRedirectToRoot` viene effettuato il seeding del metodo, il database in questo metodo di test per assicurarsi che sia presente per SUT eliminare un record. Selezionare il `deleteBtn1` pulsante del `messages` modulo il SUT viene simulato nella richiesta per il SUT:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>Opzioni del client

Nella tabella seguente viene illustrato il valore predefinito [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) disponibili durante la creazione di `HttpClient` istanze.

| Opzione | Descrizione | Impostazione predefinita |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | Ottiene o imposta o meno `HttpClient` istanze devono seguire automaticamente le risposte di reindirizzamento. | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | Ottiene o imposta l'indirizzo di base di `HttpClient` istanze. | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | Ottiene o imposta se `HttpClient` istanze devono gestire i cookie. | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | Ottiene o imposta il numero massimo di risposte di reindirizzamento che `HttpClient` le istanze devono seguire. | 7 |

Creare il `WebApplicationFactoryClientOptions` classe e passarlo al [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) metodo (impostazione predefinita vengono visualizzati i valori nell'esempio di codice):

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>Modo in cui l'infrastruttura di test viene dedotto il percorso radice del contenuto dell'app

Il `WebApplicationFactory` costruttore deduce il percorso radice del contenuto dell'app eseguendo una ricerca per un [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) sull'assembly contenente il test di integrazione con una chiave uguale al `TEntryPoint` assembly `System.Reflection.Assembly.FullName`. Nel caso in cui non viene trovato un attributo con la chiave corretta, `WebApplicationFactory` opta per la ricerca di un file di soluzione (*\*sln*) e aggiunge il `TEntryPoint` nome dell'assembly alla directory della soluzione. La directory radice dell'applicazione (il percorso radice del contenuto) viene utilizzata per individuare le viste e i file di contenuto.

Nella maggior parte dei casi, non è necessario impostare in modo esplicito la radice del contenuto di app, come la logica di ricerca in genere trova la radice del contenuto corretta in fase di esecuzione. In particolari scenari in cui non vengono trovata la radice del contenuto usando l'algoritmo di ricerca predefinite, l'app contenuto radice può essere specificata in modo esplicito o utilizzando la logica personalizzata. Per impostare la radice del contenuto di app in tali scenari, chiamare il `UseSolutionRelativeContentRoot` metodo di estensione dal [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) pacchetto. Specificare nome o glob modello di file di soluzione facoltativo e il percorso relativo della soluzione (impostazione predefinita = `*.sln`).

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

* Quando si configurano le classi di test con un oggetto personalizzato `WebApplicationFactory`, ereditano da `WebApplicationFactory` ed eseguire l'override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

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

## <a name="disable-shadow-copying"></a>Disabilitare la copia shadow

Creazione di copie shadow fa sì che i test da eseguire in una cartella diversa da quella cartella di output. Per i test per il corretto funzionamento, la copia shadow deve essere disabilitata. Il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) utilizza xUnit e disabilita la creazione di copie shadow per xUnit includendo un *xunit.runner.json* file con l'impostazione di configurazione corretto. Per altre informazioni, vedere [configurazione xUnit.net con JSON](https://xunit.github.io/docs/configuring-with-json.html).

Aggiungere il *xunit.runner.json* file radice del progetto di test con il seguente contenuto:

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>Esempio di test di integrazione

Il [app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) è costituito da due App:

| App | Cartella di progetto | Descrizione |
| --- | -------------- | ----------- |
| App di messaggio (SUT) | *src/RazorPagesProject* | Consente di aggiungere, eliminare uno, eliminare tutti e analizzare i messaggi. |
| App di test | *tests/RazorPagesProject.Tests* | Utilizzato per il test di integrazione di SUT. |

È possibile eseguire i test utilizzando le funzionalità di test predefinite di IDE, ad esempio [Visual Studio](https://www.visualstudio.com/vs/). Se si utilizza [Visual Studio Code](https://code.visualstudio.com/) o la riga di comando, eseguire il comando seguente al prompt dei comandi nel *tests/RazorPagesProject.Tests* cartella:

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>Organizzazione di app (SUT) messaggio

Il SUT è un sistema di messaggio Razor pagine con le caratteristiche seguenti:

* La pagina di indice dell'app (*Pages/Index.cshtml* e *Pages/Index.cshtml.cs*) fornisce un'interfaccia utente e una pagina di metodi del modello per controllare l'aggiunta, eliminazione e l'analisi dei messaggi (parole medie per ogni messaggio) .
* Viene descritto un messaggio dal `Message` classe (*Data/Message.cs*) con due proprietà: `Id` (chiave) e `Text` (messaggio). Il `Text` proprietà è necessaria e limitata a 200 caratteri.
* I messaggi vengono archiviati utilizzando [database in memoria di Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* L'app contiene un livello di accesso ai dati (DAL) nella relativa classe di contesto di database, `AppDbContext` (*Data/AppDbContext.cs*).
* Se il database è vuoto all'avvio dell'app, l'archivio di messaggi viene inizializzato con tre messaggi.
* L'applicazione include un `/SecurePage` che accessibile solo da un utente autenticato.

&#8224;L'argomento EF [Test con InMemory](/ef/core/miscellaneous/testing/in-memory), viene illustrato come utilizzare un database in memoria per i test con MSTest. Questo argomento viene utilizzato il [xUnit](https://xunit.github.io/) framework di test. Concetti di test e le implementazioni di test tra test diversi Framework sono simili ma non identica.

Anche se l'app non usa il [modello di repository](http://martinfowler.com/eaaCatalog/repository.html) e non è un esempio efficace del [modello unità di lavoro (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), pagine Razor supporta questi modelli di sviluppo. Per altre informazioni, vedere [progetta il livello di persistenza infrastruttura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [implementazione del Repository e modelli di unità di lavoro in un'applicazione MVC ASP.NET](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), e [controller di Test logica](/aspnet/core/mvc/controllers/testing) (l'esempio implementa il modello di repository).

### <a name="test-app-organization"></a>Organizzazione di app di test

L'app di test è un'applicazione console all'interno di *tests/RazorPagesProject.Tests* cartella.

| Cartella di app di test | Descrizione |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs* contiene metodi di test per il routing, l'accesso a una pagina protetta da un utente autenticato e ottenere un profilo utente GitHub e il controllo di accesso dell'utente del profilo. |
| *IntegrationTests* | *IndexPageTests.cs* contiene i test di integrazione per la pagina di indice utilizzando personalizzata `WebApplicationFactory` classe. |
| *Gli helper/utilità* | <ul><li>*Utilities.cs* contiene il `InitializeDbForTests` metodo usato per il seeding del database con dati di test.</li><li>*HtmlHelpers.cs* offre un metodo per restituire un AngleSharp `IHtmlDocument` per l'utilizzo con i metodi di test.</li><li>*HttpClientExtensions.cs* forniscono gli overload per `SendAsync` per inviare le richieste di SUT.</li></ul> |

Il framework di test è [xUnit](https://xunit.github.io/). I test di integrazione vengono effettuati con il [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), che include il [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver). Poiché il [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) pacchetto venga utilizzato per configurare il server host e test di test, la `TestHost` e `TestServer` pacchetti non richiedono riferimenti diretti pacchetto nel file di progetto dell'app di test o configurazione di sviluppatore nell'applicazione di test.

**Il seeding del database per il test**

I test di integrazione in genere richiedono un piccolo set di dati nel database prima dell'esecuzione del test. Ad esempio, un'operazione di eliminazione test chiama un'eliminazione di record di database, in modo che il database deve avere almeno un record per la richiesta di eliminazione abbia esito positivo.

L'app di esempio esegue il seeding del database con tre messaggi in *Utilities.cs* usato test quando vengono eseguiti:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Unit test](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Razor pagine unit test](xref:test/razor-pages-tests)
* [Middleware](xref:fundamentals/middleware/index)
* [Test controller](xref:mvc/controllers/testing)
