---
title: Razor Pages unit test in ASP.NET Core
author: guardrex
description: Informazioni su come creare unit test per Razor Pages app.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: test/razor-pages-tests
ms.openlocfilehash: afac97d686ef190ebb92d20a55a15dd774b0d1de
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081432"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Razor Pages unit test in ASP.NET Core

Di [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core supporta unit test di Razor Pages app. I test del livello di accesso ai dati (DAL) e dei modelli di pagina consentono di garantire quanto segue:

* Parti di un'app Razor Pages funzionano in modo indipendente e insieme come unità durante la costruzione dell'app.
* Le classi e i metodi hanno ambiti di responsabilità limitati.
* La documentazione aggiuntiva è disponibile nel comportamento dell'app.
* Le regressioni, ovvero gli errori introdotti dagli aggiornamenti al codice, vengono rilevate durante la compilazione e la distribuzione automatiche.

Questo argomento presuppone che l'utente abbia una conoscenza di base delle app Razor Pages e degli unit test. Se non si ha familiarità con Razor Pages app o i concetti di test, vedere gli argomenti seguenti:

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [Testing C# unità in .NET Core usando il test dotnet e xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

Il progetto di esempio è costituito da due app:

| App         | Cartella di progetto                     | Descrizione |
| ----------- | ---------------------------------- | ----------- |
| App messaggio | *src/RazorPagesTestSample*         | Consente a un utente di aggiungere un messaggio, eliminare un messaggio, eliminare tutti i messaggi e analizzare i messaggi (trovare il numero medio di parole per messaggio). |
| App di test    | *tests/RazorPagesTestSample.Tests* | Usato per unit test il modello di pagina DAL e indice dell'app del messaggio. |

I test possono essere eseguiti usando le funzionalità di test predefinite di un IDE, ad esempio [Visual Studio](/visualstudio/test/unit-test-your-code) o [Visual Studio per Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution). Se si usa [Visual Studio Code](https://code.visualstudio.com/) o la riga di comando, eseguire il comando seguente al prompt dei comandi nella cartella *tests/RazorPagesTestSample. tests* :

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a>Organizzazione app messaggi

L'app Message è un sistema di messaggi di Razor Pages con le seguenti caratteristiche:

* La pagina di indice dell'app (*pages/index. cshtml* e *pages/index. cshtml. cs*) fornisce i metodi di interfaccia utente e modello di pagina per controllare l'aggiunta, l'eliminazione e l'analisi dei messaggi (trovare il numero medio di parole per messaggio).
* Un messaggio `Message` viene descritto dalla classe (*Data/Message. cs*) con due proprietà: `Id` (chiave) e `Text` (messaggio). La `Text` proprietà è obbligatoria e limitata a 200 caratteri.
* I messaggi vengono archiviati utilizzando&#8224; [il database in memoria di Entity Framework](/ef/core/providers/in-memory/).
* L'app contiene un dal nella relativa classe del contesto di `AppDbContext` database, (*Data/AppDbContext. cs*). I metodi dal sono contrassegnati `virtual`, che consente di simulare i metodi da usare nei test.
* Se il database è vuoto all'avvio dell'app, l'archivio messaggi viene inizializzato con tre messaggi. Questi *messaggi con seeding* vengono usati anche nei test.

&#8224;L'argomento EF, [test con InMemory](/ef/core/miscellaneous/testing/in-memory), spiega come usare un database in memoria per i test con MSTest. In questo argomento viene usato il Framework di test di [xUnit](https://xunit.github.io/) . I concetti di test e le implementazioni di test in diversi framework di test sono simili, ma non identici.

Sebbene l'app di esempio non usi il modello di repository e non sia un esempio efficace del [modello UOW (Unit of work)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supporta questi modelli di sviluppo. Per ulteriori informazioni, vedere [progettazione del livello di persistenza dell'infrastruttura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) e <xref:mvc/controllers/testing> (l'esempio implementa il modello di repository).

## <a name="test-app-organization"></a>Testare l'organizzazione dell'app

L'app di test è un'app console all'interno della cartella *tests/RazorPagesTestSample. tests* .

| Test cartella app | Descrizione |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* contiene gli unit test per il dal.</li><li>*IndexPageTests.cs* contiene gli unit test per il modello di pagina di indice.</li></ul> |
| *Utilità*     | Contiene il `TestDbContextOptions` metodo utilizzato per creare nuove opzioni del contesto di database per ogni unit test dal, in modo che il database venga reimpostato sulla condizione di base per ogni test. |

Il Framework di test è [xUnit](https://xunit.github.io/). Il Framework di simulazione degli oggetti è [MOQ](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Unit test del livello di accesso ai dati (DAL)

L'app Message dispone di un dal con quattro metodi contenuti nella `AppDbContext` classe (*src/RazorPagesTestSample/data/AppDbContext. cs*). Ogni metodo dispone di uno o due unit test nell'app di test.

| DAL (metodo)               | Funzione                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Ottiene un oggetto `List<Message>` dal database ordinato in base alla `Text` proprietà. |
| `AddMessageAsync`        | Aggiunge un `Message` oggetto al database.                                          |
| `DeleteAllMessagesAsync` | Elimina tutte `Message` le voci dal database.                           |
| `DeleteMessageAsync`     | Elimina un singolo `Message` oggetto dal database da `Id`.                      |

Gli unit test di dal richiedono <xref:Microsoft.EntityFrameworkCore.DbContextOptions> quando si crea un `AppDbContext` nuovo oggetto per ogni test. Un approccio alla creazione `DbContextOptions` di per ogni test consiste nell' <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>usare:

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Il problema di questo approccio è che ogni test riceve il database nello stato in cui il test precedente lo ha lasciato. Questo può risultare problematico quando si tenta di scrivere unit test atomici che non interferiscono tra loro. Per forzare `AppDbContext` l'utilizzo di un nuovo contesto di database per ogni test, `DbContextOptions` fornire un'istanza di basata su un nuovo provider di servizi. L'app di test illustra come eseguire questa operazione usando `Utilities` il metodo `TestDbContextOptions` della classe (*test/RazorPagesTestSample. tests/Utilities/Utilities. cs*):

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

L' `DbContextOptions` uso di negli unit test dal consente a ogni test di essere eseguito in modo atomico con una nuova istanza di database:

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Ogni metodo di test nella `DataAccessLayerTest` classe (*UnitTests/DataAccessLayerTest. cs*) segue un modello di tipo Arrange-Act-Assert simile:

1. Disporre Il database è configurato per il test e/o il risultato previsto è definito.
1. Atto Il test viene eseguito.
1. Assert Vengono eseguite asserzioni per determinare se il risultato del test è riuscito.

Ad esempio, il `DeleteMessageAsync` metodo è responsabile della rimozione di un singolo messaggio identificato `Id` da (*src/RazorPagesTestSample/data/AppDbContext. cs*):

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Sono disponibili due test per questo metodo. Un test verifica che il metodo elimini un messaggio quando il messaggio è presente nel database. L'altro metodo verifica che il database non venga modificato se il `Id` messaggio per l'eliminazione non esiste. Il `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` metodo è illustrato di seguito:

[!code-csharp[](razor-pages-tests/samples_snapshot/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

In primo luogo, il metodo esegue il passaggio di disposizione, in cui viene eseguita la preparazione per il passaggio Act. I messaggi di seeding vengono ottenuti e `seedMessages`conservati in. I messaggi di seeding vengono salvati nel database. Il messaggio con un `Id` oggetto `1` di è impostato per l'eliminazione. Quando viene `DeleteMessageAsync` eseguito il metodo, i messaggi previsti devono contenere tutti i messaggi ad eccezione di quello `Id` con il valore di `1`. La `expectedMessages` variabile rappresenta il risultato previsto.

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Il metodo agisce: Il `DeleteMessageAsync` metodo viene eseguito passando il `recId` di `1`:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Infine, il metodo ottiene `Messages` dal contesto e lo confronta con l' `expectedMessages` asserzione che i due sono uguali:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Per confrontare che i due `List<Message>` sono uguali:

* I messaggi sono ordinati in `Id`base a.
* Le coppie di messaggi vengono confrontate con la `Text` proprietà.

Un metodo di test simile `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` , controlla il risultato del tentativo di eliminazione di un messaggio che non esiste. In questo caso, i messaggi previsti nel database devono essere uguali ai messaggi effettivi dopo l'esecuzione `DeleteMessageAsync` del metodo. Non devono essere apportate modifiche al contenuto del database:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Unit test dei metodi del modello di pagina

Un altro set di unit test è responsabile per i test dei metodi del modello di pagina. Nell'app Message i modelli di pagina di indice si trovano nella `IndexModel` classe in *src/RazorPagesTestSample/pages/index. cshtml. cs*.

| Metodo del modello di pagina | Funzione |
| ----------------- | -------- |
| `OnGetAsync` | Ottiene i messaggi dal dal per l'interfaccia utente usando il `GetMessagesAsync` metodo. |
| `OnPostAddMessageAsync` | Se [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) è valido, chiama `AddMessageAsync` per aggiungere un messaggio al database. |
| `OnPostDeleteAllMessagesAsync` | Chiama `DeleteAllMessagesAsync` per eliminare tutti i messaggi nel database. |
| `OnPostDeleteMessageAsync` | Esegue per eliminare un messaggio con l' `Id` oggetto specificato. `DeleteMessageAsync` |
| `OnPostAnalyzeMessagesAsync` | Se uno o più messaggi si trovano nel database, calcola il numero medio di parole per messaggio. |

I metodi del modello di pagina vengono testati con sette `IndexPageTests` test nella classe (*tests/RazorPagesTestSample. tests/UnitTests/IndexPageTests. cs*). I test usano il noto modello Arrange-Assert-Act. Questi test sono incentrati su:

* Determinare se i metodi seguono il comportamento corretto quando [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) non è valido.
* Verificare che i metodi producano <xref:Microsoft.AspNetCore.Mvc.IActionResult>i corretti.
* Verifica della correttezza delle assegnazioni dei valori delle proprietà.

Questo gruppo di test spesso simula i metodi di DAL per produrre i dati previsti per il passaggio Act in cui viene eseguito un metodo del modello di pagina. Ad esempio, il `GetMessagesAsync` metodo `AppDbContext` di viene simulato per produrre l'output. Quando un metodo del modello di pagina esegue questo metodo, la simulazione restituisce il risultato. I dati non provengono dal database. Questo consente di creare condizioni di test prevedibili e affidabili per l'uso di DAL nei test del modello di pagina.

Il `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test Mostra come il `GetMessagesAsync` metodo viene simulato per il modello di pagina:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Quando il `OnGetAsync` metodo viene eseguito nel passaggio Act, viene chiamato il metodo del modello di `GetMessagesAsync` pagina.

Passaggio di Act unit test (*test/RazorPagesTestSample. tests/UnitTests/IndexPageTests. cs*):

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage`Metodo del modello `OnGetAsync` di pagina (*src/RazorPagesTestSample/pages/index. cshtml. cs*):

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

Il `GetMessagesAsync` metodo nel dal non restituisce il risultato per questa chiamata al metodo. La versione fittizia del metodo restituisce il risultato.

Nel passaggio i messaggi effettivi (`actualMessages` `Messages` ) vengono assegnati dalla proprietà del modello di pagina. `Assert` Viene inoltre eseguito un controllo del tipo quando i messaggi vengono assegnati. I messaggi previsti ed effettivi vengono confrontati con le rispettive `Text` proprietà. Il test dichiara che le due `List<Message>` istanze contengono gli stessi messaggi.

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Altri test in questo gruppo creano <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>oggetti modello `PageContext` `ViewDataDictionary` dipagina<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>che includono,, per stabilire, e. `PageContext` <xref:Microsoft.AspNetCore.Mvc.ActionContext> Queste informazioni sono utili per l'esecuzione di test. Ad esempio, l'app Message stabilisce un `ModelState` errore con <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> per verificare che venga restituito <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> un valore valido `OnPostAddMessageAsync` quando viene eseguito:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Testing C# unità in .NET Core usando il test dotnet e xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [Eseguire unit test del codice](/visualstudio/test/unit-test-your-code) (Visual Studio)
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [Creazione di una soluzione .NET Core completa in macOS con Visual Studio per Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [Introduzione a xUnit.net: Uso di .NET Core con la riga di comando di .NET SDK](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Guida introduttiva di MOQ](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core supporta unit test di Razor Pages app. I test del livello di accesso ai dati (DAL) e dei modelli di pagina consentono di garantire quanto segue:

* Parti di un'app Razor Pages funzionano in modo indipendente e insieme come unità durante la costruzione dell'app.
* Le classi e i metodi hanno ambiti di responsabilità limitati.
* La documentazione aggiuntiva è disponibile nel comportamento dell'app.
* Le regressioni, ovvero gli errori introdotti dagli aggiornamenti al codice, vengono rilevate durante la compilazione e la distribuzione automatiche.

Questo argomento presuppone che l'utente abbia una conoscenza di base delle app Razor Pages e degli unit test. Se non si ha familiarità con Razor Pages app o i concetti di test, vedere gli argomenti seguenti:

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [Testing C# unità in .NET Core usando il test dotnet e xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([procedura per il download](xref:index#how-to-download-a-sample))

Il progetto di esempio è costituito da due app:

| App         | Cartella di progetto                     | Descrizione |
| ----------- | ---------------------------------- | ----------- |
| App messaggio | *src/RazorPagesTestSample*         | Consente a un utente di aggiungere un messaggio, eliminare un messaggio, eliminare tutti i messaggi e analizzare i messaggi (trovare il numero medio di parole per messaggio). |
| App di test    | *tests/RazorPagesTestSample.Tests* | Usato per unit test il modello di pagina DAL e indice dell'app del messaggio. |

I test possono essere eseguiti usando le funzionalità di test predefinite di un IDE, ad esempio [Visual Studio](/visualstudio/test/unit-test-your-code) o [Visual Studio per Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution). Se si usa [Visual Studio Code](https://code.visualstudio.com/) o la riga di comando, eseguire il comando seguente al prompt dei comandi nella cartella *tests/RazorPagesTestSample. tests* :

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a>Organizzazione app messaggi

L'app Message è un sistema di messaggi di Razor Pages con le seguenti caratteristiche:

* La pagina di indice dell'app (*pages/index. cshtml* e *pages/index. cshtml. cs*) fornisce i metodi di interfaccia utente e modello di pagina per controllare l'aggiunta, l'eliminazione e l'analisi dei messaggi (trovare il numero medio di parole per messaggio).
* Un messaggio `Message` viene descritto dalla classe (*Data/Message. cs*) con due proprietà: `Id` (chiave) e `Text` (messaggio). La `Text` proprietà è obbligatoria e limitata a 200 caratteri.
* I messaggi vengono archiviati utilizzando&#8224; [il database in memoria di Entity Framework](/ef/core/providers/in-memory/).
* L'app contiene un dal nella relativa classe del contesto di `AppDbContext` database, (*Data/AppDbContext. cs*). I metodi dal sono contrassegnati `virtual`, che consente di simulare i metodi da usare nei test.
* Se il database è vuoto all'avvio dell'app, l'archivio messaggi viene inizializzato con tre messaggi. Questi *messaggi con seeding* vengono usati anche nei test.

&#8224;L'argomento EF, [test con InMemory](/ef/core/miscellaneous/testing/in-memory), spiega come usare un database in memoria per i test con MSTest. In questo argomento viene usato il Framework di test di [xUnit](https://xunit.github.io/) . I concetti di test e le implementazioni di test in diversi framework di test sono simili, ma non identici.

Sebbene l'app di esempio non usi il modello di repository e non sia un esempio efficace del [modello UOW (Unit of work)](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supporta questi modelli di sviluppo. Per ulteriori informazioni, vedere [progettazione del livello di persistenza dell'infrastruttura](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) e <xref:mvc/controllers/testing> (l'esempio implementa il modello di repository).

## <a name="test-app-organization"></a>Testare l'organizzazione dell'app

L'app di test è un'app console all'interno della cartella *tests/RazorPagesTestSample. tests* .

| Test cartella app | Descrizione |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* contiene gli unit test per il dal.</li><li>*IndexPageTests.cs* contiene gli unit test per il modello di pagina di indice.</li></ul> |
| *Utilità*     | Contiene il `TestDbContextOptions` metodo utilizzato per creare nuove opzioni del contesto di database per ogni unit test dal, in modo che il database venga reimpostato sulla condizione di base per ogni test. |

Il Framework di test è [xUnit](https://xunit.github.io/). Il Framework di simulazione degli oggetti è [MOQ](https://github.com/moq/moq4).

## <a name="unit-tests-of-the-data-access-layer-dal"></a>Unit test del livello di accesso ai dati (DAL)

L'app Message dispone di un dal con quattro metodi contenuti nella `AppDbContext` classe (*src/RazorPagesTestSample/data/AppDbContext. cs*). Ogni metodo dispone di uno o due unit test nell'app di test.

| DAL (metodo)               | Funzione                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | Ottiene un oggetto `List<Message>` dal database ordinato in base alla `Text` proprietà. |
| `AddMessageAsync`        | Aggiunge un `Message` oggetto al database.                                          |
| `DeleteAllMessagesAsync` | Elimina tutte `Message` le voci dal database.                           |
| `DeleteMessageAsync`     | Elimina un singolo `Message` oggetto dal database da `Id`.                      |

Gli unit test di dal richiedono <xref:Microsoft.EntityFrameworkCore.DbContextOptions> quando si crea un `AppDbContext` nuovo oggetto per ogni test. Un approccio alla creazione `DbContextOptions` di per ogni test consiste nell' <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>usare:

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

Il problema di questo approccio è che ogni test riceve il database nello stato in cui il test precedente lo ha lasciato. Questo può risultare problematico quando si tenta di scrivere unit test atomici che non interferiscono tra loro. Per forzare `AppDbContext` l'utilizzo di un nuovo contesto di database per ogni test, `DbContextOptions` fornire un'istanza di basata su un nuovo provider di servizi. L'app di test illustra come eseguire questa operazione usando `Utilities` il metodo `TestDbContextOptions` della classe (*test/RazorPagesTestSample. tests/Utilities/Utilities. cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

L' `DbContextOptions` uso di negli unit test dal consente a ogni test di essere eseguito in modo atomico con una nuova istanza di database:

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

Ogni metodo di test nella `DataAccessLayerTest` classe (*UnitTests/DataAccessLayerTest. cs*) segue un modello di tipo Arrange-Act-Assert simile:

1. Disporre Il database è configurato per il test e/o il risultato previsto è definito.
1. Atto Il test viene eseguito.
1. Assert Vengono eseguite asserzioni per determinare se il risultato del test è riuscito.

Ad esempio, il `DeleteMessageAsync` metodo è responsabile della rimozione di un singolo messaggio identificato `Id` da (*src/RazorPagesTestSample/data/AppDbContext. cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

Sono disponibili due test per questo metodo. Un test verifica che il metodo elimini un messaggio quando il messaggio è presente nel database. L'altro metodo verifica che il database non venga modificato se il `Id` messaggio per l'eliminazione non esiste. Il `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` metodo è illustrato di seguito:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

In primo luogo, il metodo esegue il passaggio di disposizione, in cui viene eseguita la preparazione per il passaggio Act. I messaggi di seeding vengono ottenuti e `seedMessages`conservati in. I messaggi di seeding vengono salvati nel database. Il messaggio con un `Id` oggetto `1` di è impostato per l'eliminazione. Quando viene `DeleteMessageAsync` eseguito il metodo, i messaggi previsti devono contenere tutti i messaggi ad eccezione di quello `Id` con il valore di `1`. La `expectedMessages` variabile rappresenta il risultato previsto.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

Il metodo agisce: Il `DeleteMessageAsync` metodo viene eseguito passando il `recId` di `1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

Infine, il metodo ottiene `Messages` dal contesto e lo confronta con l' `expectedMessages` asserzione che i due sono uguali:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

Per confrontare che i due `List<Message>` sono uguali:

* I messaggi sono ordinati in `Id`base a.
* Le coppie di messaggi vengono confrontate con la `Text` proprietà.

Un metodo di test simile `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` , controlla il risultato del tentativo di eliminazione di un messaggio che non esiste. In questo caso, i messaggi previsti nel database devono essere uguali ai messaggi effettivi dopo l'esecuzione `DeleteMessageAsync` del metodo. Non devono essere apportate modifiche al contenuto del database:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>Unit test dei metodi del modello di pagina

Un altro set di unit test è responsabile per i test dei metodi del modello di pagina. Nell'app Message i modelli di pagina di indice si trovano nella `IndexModel` classe in *src/RazorPagesTestSample/pages/index. cshtml. cs*.

| Metodo del modello di pagina | Funzione |
| ----------------- | -------- |
| `OnGetAsync` | Ottiene i messaggi dal dal per l'interfaccia utente usando il `GetMessagesAsync` metodo. |
| `OnPostAddMessageAsync` | Se [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) è valido, chiama `AddMessageAsync` per aggiungere un messaggio al database. |
| `OnPostDeleteAllMessagesAsync` | Chiama `DeleteAllMessagesAsync` per eliminare tutti i messaggi nel database. |
| `OnPostDeleteMessageAsync` | Esegue per eliminare un messaggio con l' `Id` oggetto specificato. `DeleteMessageAsync` |
| `OnPostAnalyzeMessagesAsync` | Se uno o più messaggi si trovano nel database, calcola il numero medio di parole per messaggio. |

I metodi del modello di pagina vengono testati con sette `IndexPageTests` test nella classe (*tests/RazorPagesTestSample. tests/UnitTests/IndexPageTests. cs*). I test usano il noto modello Arrange-Assert-Act. Questi test sono incentrati su:

* Determinare se i metodi seguono il comportamento corretto quando [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) non è valido.
* Verificare che i metodi producano <xref:Microsoft.AspNetCore.Mvc.IActionResult>i corretti.
* Verifica della correttezza delle assegnazioni dei valori delle proprietà.

Questo gruppo di test spesso simula i metodi di DAL per produrre i dati previsti per il passaggio Act in cui viene eseguito un metodo del modello di pagina. Ad esempio, il `GetMessagesAsync` metodo `AppDbContext` di viene simulato per produrre l'output. Quando un metodo del modello di pagina esegue questo metodo, la simulazione restituisce il risultato. I dati non provengono dal database. Questo consente di creare condizioni di test prevedibili e affidabili per l'uso di DAL nei test del modello di pagina.

Il `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test Mostra come il `GetMessagesAsync` metodo viene simulato per il modello di pagina:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

Quando il `OnGetAsync` metodo viene eseguito nel passaggio Act, viene chiamato il metodo del modello di `GetMessagesAsync` pagina.

Passaggio di Act unit test (*test/RazorPagesTestSample. tests/UnitTests/IndexPageTests. cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage`Metodo del modello `OnGetAsync` di pagina (*src/RazorPagesTestSample/pages/index. cshtml. cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

Il `GetMessagesAsync` metodo nel dal non restituisce il risultato per questa chiamata al metodo. La versione fittizia del metodo restituisce il risultato.

Nel passaggio i messaggi effettivi (`actualMessages` `Messages` ) vengono assegnati dalla proprietà del modello di pagina. `Assert` Viene inoltre eseguito un controllo del tipo quando i messaggi vengono assegnati. I messaggi previsti ed effettivi vengono confrontati con le rispettive `Text` proprietà. Il test dichiara che le due `List<Message>` istanze contengono gli stessi messaggi.

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

Altri test in questo gruppo creano <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>oggetti modello `PageContext` `ViewDataDictionary` dipagina<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>che includono,, per stabilire, e. `PageContext` <xref:Microsoft.AspNetCore.Mvc.ActionContext> Queste informazioni sono utili per l'esecuzione di test. Ad esempio, l'app Message stabilisce un `ModelState` errore con <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> per verificare che venga restituito <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> un valore valido `OnPostAddMessageAsync` quando viene eseguito:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>Risorse aggiuntive

* [Testing C# unità in .NET Core usando il test dotnet e xUnit](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [Eseguire unit test del codice](/visualstudio/test/unit-test-your-code) (Visual Studio)
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [Creazione di una soluzione .NET Core completa in macOS con Visual Studio per Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [Introduzione a xUnit.net: Uso di .NET Core con la riga di comando di .NET SDK](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Guida introduttiva di MOQ](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end
