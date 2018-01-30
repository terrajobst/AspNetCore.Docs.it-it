---
title: "Unità di pagine Razor e integrazione test in ASP.NET Core"
author: guardrex
description: Informazioni su come creare unit test e integrazione test per App pagine Razor.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/razor-pages-testing
ms.openlocfilehash: 5891b236306cd3790cbba14919796d6aa894ad53
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="razor-pages-unit-and-integration-testing-in-aspnet-core"></a>Unità di pagine Razor e integrazione test in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

ASP.NET Core supporta unità e test di integrazione di App pagine Razor. Il livello di accesso ai dati (DAL), modelli di pagina e componenti della pagina integrata garantirà:

* Parti di un'app di pagine Razor funzionano in modo indipendente e insieme come una singola unità durante la costruzione di app.
* Classi e metodi limitata gli ambiti di responsabilità.
* Documentazione aggiuntiva esiste sul comportamento di app.
* Regressioni, ovvero errori nasce da aggiornamenti al codice, sono disponibili durante la distribuzione e la compilazione automatizzata.

In questo argomento si presuppone una conoscenza di base delle pagine Razor App unit test e integrazione test. Se non si ha familiarità con App pagine Razor o test concetti, vedere gli argomenti seguenti:

* [Introduzione a Razor Pages in ASP.NET Core](xref:mvc/razor-pages/index)
* [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Unit test c# in .NET Core usando xUnit e test dotnet](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Test di integrazione](xref:testing/integration-testing)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

Il progetto di esempio è costituito da due applicazioni:

| App         | Cartella di progetto                        | Descrizione |
| ----------- | ------------------------------------- | ----------- |
| App di messaggio | *src/RazorPagesTestingSample*         | Consente di aggiungere, eliminare uno, eliminare tutti e analizzare i messaggi. |
| App di test    | *tests/RazorPagesTestingSample.Tests* | Usato per testare l'app del messaggio.<ul><li>Unit test: livello di accesso ai dati (DAL), il modello di pagina di indice</li><li>Test di integrazione: pagina di indice</li></ul> |

È possibile eseguire i test utilizzando le funzionalità di test predefinite dell'IDE, ad esempio [Visual Studio](https://www.visualstudio.com/vs/). Se si utilizza [codice di Visual Studio](https://code.visualstudio.com/) o la riga di comando, eseguire il comando seguente al prompt dei comandi nel *tests/RazorPagesTestingSample.Tests* cartella:

```console
dotnet test
```

## <a name="message-app-organization"></a>Organizzazione di app di messaggio

L'applicazione di messaggio è un semplice sistema messaggio Razor pagine con le caratteristiche seguenti:

* La pagina di indice dell'app (*Pages/Index.cshtml* e *Pages/Index.cshtml.cs*) fornisce un'interfaccia utente e una pagina di metodi del modello per controllare l'aggiunta, eliminazione e l'analisi dei messaggi (parole medie per ogni messaggio) .
* Viene descritto un messaggio dal `Message` classe (*Data/Message.cs*) con due proprietà: `Id` (chiave) e `Text` (messaggio). Il `Text` proprietà è necessaria e limitata a 200 caratteri.
* I messaggi vengono archiviati utilizzando [database in memoria di Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* L'app contiene un livello di accesso ai dati (DAL) nella relativa classe di contesto di database, `AppDbContext` (*Data/AppDbContext.cs*). I metodi DAL sono contrassegnati `virtual`, che consente a tali i metodi da utilizzare nei test.
* Se il database è vuoto all'avvio dell'app, l'archivio di messaggi viene inizializzato con tre messaggi. Questi *seeding messaggi* vengono inoltre utilizzati nel test.

&#8224; L'argomento EF [test con InMemory](/ef/core/miscellaneous/testing/in-memory), viene illustrato come utilizzare un database in memoria per il test con MSTest. Questo argomento viene utilizzato il [xUnit](https://xunit.github.io/) framework di test. Le implementazioni di test in diversi framework di test e test concetti sono simili ma non identica.

Anche se l'app non usa il [modello di repository](http://martinfowler.com/eaaCatalog/repository.html) e non è un esempio efficace del [modello unità di lavoro (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), pagine Razor supporta questi modelli di sviluppo. Per ulteriori informazioni, vedere [progettazione del livello di persistenza infrastruttura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [implementazione del Repository e schemi di unità di lavoro in un'applicazione MVC ASP.NET](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), e [test logica di controller](/aspnet/core/mvc/controllers/testing) (l'esempio implementa il modello di repository).

## <a name="test-app-organization"></a>Organizzazione di app di test

L'applicazione di test è un'applicazione console all'interno di *tests/RazorPagesTestingSample.Tests* cartella:

| Cartella di app di test    | Descrizione |
| ------------------ | ----------- |
| *IntegrationTests* | <ul><li>*IndexPageTest.cs* contiene i test di integrazione per la pagina di indice.</li><li>*TestFixture.cs* crea l'host di test per testare l'app del messaggio.</li></ul> |
| *UnitTests*        | <ul><li>*DataAccessLayerTest.cs* contiene unit test per DAL.</li><li>*IndexPageTest.cs* contiene unit test per il modello di pagina di indice.</li></ul> |
| *Utilità*        | *Utilities.cs* contiene il:<ul><li>`TestingDbContextOptions`metodo utilizzato per creare le opzioni di contesto per ogni unit test DAL nuovo database in modo che il database viene reimpostato per la condizione della linea di base per ogni test.</li><li>`GetRequestContentAsync`metodo utilizzato per preparare il `HttpClient` e contenuto per le richieste inviate all'app messaggio durante il test di integrazione.</li></ul>

Il framework di test è [xUnit](https://xunit.github.io/). L'oggetto tali framework è [Moq](https://github.com/moq/moq4). Test di integrazione vengono effettuate con il [Host di Test ASP.NET Core](xref:testing/integration-testing#the-test-host).

## <a name="unit-testing-the-data-access-layer-dal"></a>Unit test a livello di accesso ai dati (DAL)

L'app del messaggio ha DAL con quattro metodi contenuti nel `AppDbContext` classe (*src/RazorPagesTestingSample/Data/AppDbContext.cs*). Ogni metodo presenta uno o due unit test nell'applicazione di test.

| Metodo DAL               | Funzione                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Ottiene un `List<Message>` dal database vengono ordinato i `Text` proprietà. |
| `AddMessageAsync`        | Aggiunge un `Message` al database.                                          |
| `DeleteAllMessagesAsync` | Elimina tutti `Message` voci dal database.                           |
| `DeleteMessageAsync`     | Elimina una singola `Message` dal database tramite `Id`.                      |

Unit test del valore DAL richiedono [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) quando si crea un nuovo `AppDbContext` per ogni test. Un approccio alla creazione di `DbContextOptions` per ogni test è possibile utilizzare un [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Il problema con questo approccio è che ogni test riceve il database in qualsiasi stato test precedente ha lasciato. Può essere problematico durante la scrittura atomica di unit test che non interferiscano tra loro. Per forzare il `AppDbContext` per utilizzare un nuovo contesto di database per ogni test, fornire un `DbContextOptions` istanza basata su un nuovo provider del servizio. L'app di test viene illustrato come eseguire questa operazione usando il relativo `Utilities` metodo della classe `TestingDbContextOptions` (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Utilizzo di `DbContextOptions` nell'unità di DAL test consente a ogni test da eseguire in modo atomico con un un'istanza aggiornata del database:

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Ogni metodo di test nel `DataAccessLayerTest` classe (*UnitTests/DataAccessLayerTest.cs*) segue uno schema analogo Arrange-Act-Assert:

1. Disponi: Il database è configurato per il test e/o il risultato previsto è definito.
1. ACT: Viene eseguito il test.
1. Asserzione: Asserzioni vengono effettuate per determinare se il risultato del test viene eseguita correttamente.

Ad esempio, il `DeleteMessageAsync` metodo è responsabile della rimozione di un singolo messaggio identificato dal relativo `Id` (*src/RazorPagesTestingSample/Data/AppDbContext.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/AppDbContext.cs?name=snippet4)]

Esistono due test per questo metodo. Un test di verifica che il metodo elimina un messaggio quando il messaggio è presente nel database. Gli altri test di metodo che il database non cambia se il messaggio `Id` per l'eliminazione non esiste. Il `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` metodo è illustrato di seguito:

[!code-csharp[Main](razor-pages-testing/sample_snapshot/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

In primo luogo, il metodo esegue il passaggio di disposizione, in cui avviene la preparazione per il passaggio di Act. I messaggi di seeding vengono ottenuti e contenuti in `seedMessages`. I messaggi di seeding vengono salvati nel database. Il messaggio con un `Id` di `1` è impostato per l'eliminazione. Quando il `DeleteMessageAsync` metodo viene eseguito, dei messaggi previsti devono avere tutti i messaggi, ad eccezione di quello con un `Id` di `1`. Il `expectedMessages` variabile rappresenta il risultato previsto.

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Il metodo agisce: il `DeleteMessageAsync` metodo viene eseguito passando il `recId` di `1`:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Infine, il metodo ottiene il `Messages` dal contesto e confrontato con il `expectedMessages` asserzione che sono uguali:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Per confrontare che i due `List<Message>` sono gli stessi:

* I messaggi vengono ordinati in base `Id`.
* Coppie di messaggi vengono confrontate nel `Text` proprietà.

Un metodo di test simile, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` controlla il risultato del tentativo di eliminare un messaggio che non esiste. In questo caso, i messaggi previsti nel database devono essere uguali per i messaggi effettivi dopo il `DeleteMessageAsync` metodo viene eseguito. Non deve esistere alcuna modifica al contenuto del database:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-testing-the-page-model-methods"></a>Unit test i metodi del modello di pagina

Un altro set di unit test è responsabile per il test di metodi del modello di pagina. Nell'applicazione di messaggio, i modelli di pagina di indice, vedere il `IndexModel` classe *src/RazorPagesTestingSample/Pages/Index.cshtml.cs*.

| Metodo del modello di pagina | Funzione |
| ----------------- | -------- | 
| `OnGetAsync` | Ottiene i messaggi per l'interfaccia utente utilizzando il `GetMessagesAsync` metodo. |
| `OnPostAddMessageAsync` | Se il `ModelState` è valido, le chiamate `AddMessageAsync` per aggiungere un messaggio al database. | 
| `OnPostDeleteAllMessagesAsync` | Chiamate `DeleteAllMessagesAsync` per eliminare tutti i messaggi nel database. |
| `OnPostDeleteMessageAsync` | Esegue `DeleteMessageAsync` per eliminare un messaggio con il `Id` specificato. |
| `OnPostAnalyzeMessagesAsync` | Se uno o più messaggi nel database, calcola il numero medio di parole per ogni messaggio. |

I metodi del modello di pagina vengono testati utilizzando sette test il `IndexPageTest` classe (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*). I test utilizzano il modello di Act Assert Disponi familiarità. Questi test sono incentrati su:

* Determinare se i metodi di seguono il comportamento corretto quando il `ModelState` non è valido.
* I metodi di conferma produrre corrette `IActionResult`.
* Verifica che le assegnazioni dei valori di proprietà vengono effettuate correttamente.

Questo gruppo di test spesso simulare i metodi di per produrre dati previsto per il passaggio di Act, in cui viene eseguito un metodo di modello di pagina. Ad esempio, il `GetMessagesAsync` metodo il `AppDbContext` è esempio per generare un output. Quando un metodo di modello di pagina viene eseguito questo metodo, la simulazione restituisce il risultato. I dati non provengono dal database. Consente di creare condizioni di test prevedibili e affidabili per l'utilizzo dai test del modello di pagina.

Il `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test Mostra come `GetMessagesAsync` metodo è esempio per il modello di pagina:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet1&highlight=3-4)]

Quando il `OnGetAsync` metodo viene eseguito nel passaggio Act, chiama il modello di pagina `GetMessagesAsync` metodo.

Unit test Act step (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet2)]

`IndexPage`modello di pagina `OnGetAsync` metodo (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

Il `GetMessagesAsync` DAL metodo non restituisce il risultato per questa chiamata al metodo. La versione del metodo simulata restituisce il risultato.

Nel `Assert` passaggio, i messaggi effettivi (`actualMessages`) vengono assegnati dal `Messages` proprietà del modello di pagina. Un controllo del tipo viene eseguito anche quando vengono assegnati i messaggi. I messaggi previsti ed effettivi vengono confrontati per loro `Text` proprietà. Il test asserisce che i due `List<Message>` istanze contengono gli stessi messaggi.

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet3)]

Altri test in questo gruppo di creare pagina di oggetti modello che includono il `DefaultHttpContext`, `ModelStateDictionary`, un `ActionContext` per stabilire il `PageContext`, un `ViewDataDictionary`e un `PageContext`. Questi sono utili per l'esecuzione di test. Ad esempio, l'app messaggio stabilisce un `ModelState` errore con `AddModelError` per verificare che un oggetto valido `PageResult` viene restituito quando `OnPostAddMessageAsync` viene eseguita:

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="integration-testing-the-app"></a>L'app di test di integrazione

L'integrazione verifica lo stato attivo sul test che interagiscono componenti dell'applicazione. Test di integrazione vengono effettuate con il [Host di Test ASP.NET Core](xref:testing/integration-testing#the-test-host). È possibile testare l'elaborazione del ciclo di vita completo di richiesta-risposta. Questi test asserire che la pagina genera il codice di stato corretto e `Location` intestazione, se impostata.

Un esempio tratto dall'esempio di test di integrazione controlla il risultato della richiesta della pagina di indice dell'app messaggio (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet1)]

Non vi è alcun passaggio di disposizione. Il `GetAsync` metodo viene chiamato sul `HttpClient` per inviare una richiesta GET all'endpoint. Il test indica che il risultato è un codice di stato 200 OK.

Qualsiasi richiesta POST per l'app del messaggio deve soddisfare il controllo non riproducibili automaticamente effettuato tramite l'app [sistema di protezione dei dati non riproducibili](xref:security/data-protection/introduction). Per disporre di una richiesta POST di un test, è necessario l'app di test:

1. Effettuare una richiesta per la pagina.
1. Analizzare i cookie non riproducibili e un token di convalida richiesta dalla risposta.
1. Eseguire la richiesta POST con la convalida non riproducibili cookie e richiesta di token sul posto.

Il `Post_AddMessageHandler_ReturnsRedirectToRoot` metodo di test:

* Prepara un messaggio e `HttpClient`.
* Effettua una richiesta POST all'app.
* Controlla che la risposta è un reindirizzamento alla pagina di indice.

`Post_AddMessageHandler_ReturnsRedirectToRoot `metodo (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet2)]

Il `GetRequestContentAsync` metodo di utilità gestisce preparazione del client con il cookie non riproducibili e token di richiesta di verifica. Si noti come il metodo riceve un `IDictionary` che consente di passare i dati per la richiesta per la codifica con il token di richiesta di verifica la chiamata al metodo di test (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet2&highlight=1-2,8-9,29)]

Test di integrazione dati errati possono anche passare all'app per testare il comportamento di risposta dell'app. L'app messaggio limita la lunghezza del messaggio a 200 caratteri (*src/RazorPagesTestingSample/Data/Message.cs*):

[!code-csharp[Main](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/Message.cs?name=snippet1&highlight=7)]

Il `Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong` test `Message` passa in modo esplicito come testo con caratteri "X" 201. Ciò comporta un `ModelState` errore. Il POST non reindirizza alla pagina di indice. Restituisce un 200 OK con un `null` `Location` intestazione (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):

[!code-csharp[Main](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet3&highlight=7,16-17)]

## <a name="see-also"></a>Vedere anche

* [Unit test c# in .NET Core usando xUnit e test dotnet](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Test di integrazione](xref:testing/integration-testing)
* [Test dei controller](xref:mvc/controllers/testing)
* [Il codice dello unit Test](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [xUnit.net](https://xunit.github.io/)
* [Introduzione a xUnit.net (.NET Core/ASP.NET Core)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Guida introduttiva Moq](https://github.com/Moq/moq4/wiki/Quickstart)
