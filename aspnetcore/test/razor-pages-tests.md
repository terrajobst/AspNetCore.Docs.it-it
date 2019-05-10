---
title: Gli unit test di Razor Pages in ASP.NET Core
author: guardrex
description: Informazioni su come creare unit test per App Razor Pages.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
uid: test/razor-pages-tests
ms.openlocfilehash: f1526b8803f43ec8cbe77c1d2c100d9daf6cd316
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893718"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Gli unit test di Razor Pages in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

ASP.NET Core supporta unit test di App Razor Pages. Test dei dati è necessario accedere layer (DAL) e modelli di pagina consentono di garantire:

* Parti di un'app Razor Pages funzionano in modo indipendente e insieme come un'unità durante la costruzione di app.
* Classi e metodi sono limitati gli ambiti di responsabilità.
* Esiste documentazione aggiuntiva su modalità di comportamento dell'app.
* Regressioni, che sono gli errori con gli aggiornamenti al codice, sono disponibili durante la distribuzione e compilazione automatizzata.

In questo argomento si presuppone una conoscenza di base dell'App Razor Pages e gli unit test. Se non si ha familiarità con le app Razor Pages o concetti di test, vedere gli argomenti seguenti:

* [Introduzione a Razor Pages in ASP.NET Core](xref:razor-pages/index)
* [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start)
* [Unit test c# in .NET Core tramite test dotnet e xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

Il progetto di esempio è costituito da due App:

| App         | Cartella del progetto                        | Descrizione |
| ----------- | ------------------------------------- | ----------- |
| App messaggi | *src/RazorPagesTestSample*            | Consente all'utente di aggiungere, eliminare uno, eliminare tutti e analizzare i messaggi. |
| Testare l'app    | *tests/RazorPagesTestSample.Tests*    | Utilizzato per l'app messaggio unit test: Livello di accesso ai dati (DAL) e il modello di pagina di indice. |

I test possono essere eseguiti usando le funzionalità di test predefinito di un ambiente IDE, ad esempio [Visual Studio](https://visualstudio.microsoft.com). Se si usa [Visual Studio Code](https://code.visualstudio.com/) o la riga di comando, eseguire il comando seguente al prompt dei comandi nel *tests/RazorPagesTestSample.Tests* cartella:

```console
dotnet test
```

## <a name="message-app-organization"></a>Organizzazione delle app messaggi

L'app di messaggio è un semplice sistema di messaggi di Razor Pages con le caratteristiche seguenti:

* La pagina di indice dell'app (*cshtml* e *Pages/Index.cshtml.cs*) fornisce un'interfaccia utente e pagina alcuni metodi del modello per controllare l'aggiunta, eliminazione e l'analisi dei messaggi (parole medie per ogni messaggio) .
* Un messaggio è descritta dal `Message` classe (*Data/Message.cs*) con due proprietà: `Id` (chiave) e `Text` (messaggio). Il `Text` proprietà è necessaria e limitata a 200 caratteri.
* I messaggi vengono archiviati usando [database in memoria di Entity Framework](/ef/core/providers/in-memory/)&#8224;.
* L'app contiene un livello di accesso ai dati (DAL) nella relativa classe di contesto di database, `AppDbContext` (*Data/AppDbContext.cs*). I metodi DAL sono contrassegnati come `virtual`, che consente i metodi per l'uso nei test di simulazione.
* Se il database è vuoto all'avvio dell'app, l'archivio di messaggi viene inizializzata con tre messaggi. Questi *seeding messaggi* vengono anche usati nei test.

&#8224;L'argomento relativo alla EF [Test con InMemory](/ef/core/miscellaneous/testing/in-memory), spiega come usare un database in memoria per i test con MSTest. Questo argomento viene utilizzato il [xUnit](https://xunit.github.io/) framework di test. Concetti di test e test implementazioni su Framework di test diversi sono simili ma non identici.

Anche se l'app non usa il modello di repository e non è un esempio efficace del [schema Unit of Work (UoW)](https://martinfowler.com/eaaCatalog/unitOfWork.html), pagine Razor supporta questi modelli di sviluppo. Per altre informazioni, vedere [la progettazione del livello di persistenza dell'infrastruttura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) e [logica dei controller di Test](/aspnet/core/mvc/controllers/testing) (l'esempio implementa il modello di repository).

## <a name="test-app-organization"></a>Organizzazione dei test app

L'app di test è un'app console all'interno di *tests/RazorPagesTestSample.Tests* cartella.

| Cartella di app di test | Descrizione |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* contiene gli unit test per DAL.</li><li>*IndexPageTests.cs* contiene gli unit test per il modello di pagina di indice.</li></ul> |
| *Utilità*     | Contiene il `TestingDbContextOptions` metodo utilizzato per creare le opzioni di contesto per ogni unit test DAL nuovo database in modo che il database viene reimpostato alla sua condizione della linea di base per ogni test. |

Il framework di test viene [xUnit](https://xunit.github.io/). L'oggetto framework di simulazione [Moq](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Gli unit test dei dati di accesso layer (DAL)

L'app di messaggio ha DAL con quattro metodi contenuti nel `AppDbContext` classe (*src/RazorPagesTestSample/Data/AppDbContext.cs*). Ogni metodo presenta uno o due unit test nell'app di test.

| Metodo DAL               | Funzione                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Ottiene un `List<Message>` dal database vengono ordinato i `Text` proprietà. |
| `AddMessageAsync`        | Aggiunge un `Message` al database.                                          |
| `DeleteAllMessagesAsync` | Elimina tutti `Message` voci dal database.                           |
| `DeleteMessageAsync`     | Elimina una singola `Message` dal database tramite `Id`.                      |

Gli unit test del valore DAL richiedono [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) quando si crea un nuovo `AppDbContext` per ogni test. Un approccio alla creazione di `DbContextOptions` per ogni test consiste nell'usare un [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Il problema con questo approccio è che ogni test riceve il database in qualsiasi stato lasciato il test precedente in precedenza. Ciò può essere problematico quando si tenta di scrivere test unità atomica che non interferiscano tra loro. Per forzare il `AppDbContext` per usare un nuovo contesto di database per ogni test, fornire un `DbContextOptions` istanza basata su un nuovo provider del servizio. L'app di test viene illustrato come eseguire questa operazione usando il `Utilities` metodo della classe `TestingDbContextOptions` (*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

Uso di `DbContextOptions` dell'unità DAL test consente a ogni test viene eseguito in modo atomico con un'istanza di database aggiornato:

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Ogni metodo di test nel `DataAccessLayerTest` classe (*UnitTests/DataAccessLayerTest.cs*) segue uno schema analogo Arrange-Act-Assert:

1. Disponi: Il database è configurato per il test e/o il risultato previsto è definito.
1. ACT: Viene eseguito il test.
1. L'asserzione: Le asserzioni vengono effettuate per determinare se il risultato del test è un esito positivo.

Ad esempio, il `DeleteMessageAsync` metodo è responsabile della rimozione di un singolo messaggio identificato dal relativo `Id` (*src/RazorPagesTestSample/Data/AppDbContext.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Esistono due test per questo metodo. Un test verifica che il metodo elimina un messaggio quando il messaggio è presente nel database. Gli altri test di metodo che il database non cambia se il messaggio `Id` per l'eliminazione non esiste. Il `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` metodo è illustrato di seguito:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

In primo luogo, il metodo esegue il passaggio di disposizione, in cui avviene la preparazione per il passaggio di Act. I messaggi di seeding vengono ottenuti e conservati in `seedMessages`. I messaggi seeding vengono salvati nel database. Il messaggio con un `Id` di `1` è impostato per l'eliminazione. Quando la `DeleteMessageAsync` metodo viene eseguito, i messaggi previsti devono avere tutti i messaggi tranne quello con un `Id` di `1`. Il `expectedMessages` variabile rappresenta il risultato previsto.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Il metodo opera: Il `DeleteMessageAsync` metodo viene eseguito passando il `recId` di `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Infine, il metodo ottiene il `Messages` dal contesto e confrontato con il `expectedMessages` che asserisce che i due sono uguali:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Per poter confrontare che i due `List<Message>` sono gli stessi:

* I messaggi vengono ordinati in base `Id`.
* Coppie di messaggi vengono confrontate nel `Text` proprietà.

Un metodo di test simile, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` controlla il risultato del tentativo di eliminare un messaggio che non esiste. In questo caso, i messaggi previsti nel database devono essere uguali per i messaggi effettivi dopo il `DeleteMessageAsync` metodo viene eseguito. Non vi sarà alcuna modifica al contenuto del database:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Gli unit test dei metodi di modello di pagina

Un altro set di unit test è responsabile per i test di metodi del modello di pagina. Nell'app messaggi, i modelli di pagina di indice si trovano nel `IndexModel` classe *src/RazorPagesTestSample/Pages/Index.cshtml.cs*.

| Metodo del modello di pagina | Funzione |
| ----------------- | -------- |
| `OnGetAsync` | Ottiene i messaggi per l'interfaccia utente utilizzando il `GetMessagesAsync` (metodo). |
| `OnPostAddMessageAsync` | Se il `ModelState` valido, le chiamate `AddMessageAsync` per aggiungere un messaggio al database. |
| `OnPostDeleteAllMessagesAsync` | Le chiamate `DeleteAllMessagesAsync` per eliminare tutti i messaggi nel database. |
| `OnPostDeleteMessageAsync` | Viene eseguito `DeleteMessageAsync` per eliminare un messaggio con il `Id` specificato. |
| `OnPostAnalyzeMessagesAsync` | Se uno o più messaggi si trovano in database di, calcola il numero medio di parole al messaggio. |

I metodi del modello di pagina vengono testati utilizzando sette test nel `IndexPageTests` classe (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*). I test utilizzano il familiare modello Arrange-Assert-Act. Questi test sono incentrati su:

* Determinare se i metodi seguono il comportamento corretto quando la `ModelState` non è valido.
* Per confermare i metodi producono i valori corretti `IActionResult`.
* Verifica che le assegnazioni dei valori di proprietà vengano eseguiti correttamente.

Questo gruppo di test simulare spesso i metodi di per produrre dati previsto per il passaggio di Act in cui viene eseguito un metodo del modello di pagina. Ad esempio, il `GetMessagesAsync` metodo del `AppDbContext` è fittizie per produrre un output. Quando questo metodo viene eseguito un metodo di modello di pagina, la simulazione restituisce il risultato. I dati non provengono dal database. Ciò consente di creare condizioni di test prevedibili e affidabili per l'utilizzo nei test del modello di pagina.

Il `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` testare viene mostrato come la `GetMessagesAsync` metodo è simulato per il modello di pagina:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Quando la `OnGetAsync` metodo viene eseguito nel passaggio Act, chiama il modello di pagina `GetMessagesAsync` (metodo).

Unit test Act step (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` modello di pagina `OnGetAsync` metodo (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

Il `GetMessagesAsync` DAL metodo non restituisce il risultato per questa chiamata al metodo. La versione fittizia del metodo restituisce il risultato.

Nel `Assert` passaggio, i messaggi effettivi (`actualMessages`) vengono assegnati dal `Messages` proprietà del modello di pagina. Un controllo del tipo viene eseguito anche quando sono assegnati i messaggi. I messaggi previsti ed effettivi vengono confrontati per loro `Text` proprietà. Il test asserisce che i due `List<Message>` istanze contengono gli stessi messaggi.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Altri test in questo gruppo creano pagina oggetti modello che includono il `DefaultHttpContext`, il `ModelStateDictionary`, un `ActionContext` per stabilire la `PageContext`, una `ViewDataDictionary`e un `PageContext`. Sono utili nell'esecuzione dei test. Ad esempio, l'app messaggi stabilisce una `ModelState` errore con `AddModelError` per verificare che un valore valido `PageResult` viene restituito quando `OnPostAddMessageAsync` viene eseguito:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Unit test c# in .NET Core tramite test dotnet e xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Test controller](xref:mvc/controllers/testing)
* [Unit Test del codice](/visualstudio/test/unit-test-your-code) (Visual Studio)
* [Test di integrazione](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [Introduzione a xUnit.net (.NET core/ASP.NET Core)](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Guida introduttiva Moq](https://github.com/Moq/moq4/wiki/Quickstart)
