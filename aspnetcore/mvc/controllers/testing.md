---
title: Logica di controller di test in ASP.NET Core
author: ardalis
description: Informazioni sulla logica di controller di test in ASP.NET Core con Moq e xUnit.
keywords: ASP.NET Core, controller di test, test, unit test di, unit test, test di integrazione, il test di integrazione, xUnit
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: dd4135ec-2b15-410c-b3fb-3d12eed4a1ac
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/testing
ms.openlocfilehash: e8a464e75dea3a0ec08c13a11888884e6bb6a4c7
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="testing-controller-logic-in-aspnet-core"></a>Logica di controller di test in ASP.NET Core

Da [Steve Smith](https://ardalis.com/)

Nelle applicazioni ASP.NET MVC controller deve essere piccoli e sono specifici per i problemi dell'interfaccia utente. I controller di grandi dimensioni che gestiscono i problemi dell'interfaccia utente non sono più difficili da testare e gestire.

[Consente di visualizzare o scaricare l'esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>Controller di test

I controller sono una parte essenziale di qualsiasi applicazione ASP.NET MVC di base. Di conseguenza, è necessario confidenza che si comportano come previsto per l'app. Test automatizzati possono fornire questo confidenza e consente di rilevare errori prima che raggiungano di produzione. È importante evitare di inserire inutili responsabilità all'interno dei controller e verificare lo stato attivo al test solo sulle responsabilità di controller.

Logica del controller deve essere minima e non essere incentrata sull'infrastruttura o logica problematiche aziendali (ad esempio, accesso ai dati). La logica del controller, non il framework di test. Test come controller di *si comporta* in base a input valido o non valido. In base al risultato dell'operazione di business che esegue le risposte di controller di test.

Responsabilità tipiche controller:

* Verificare `ModelState.IsValid`.
* Restituire una risposta di errore se `ModelState` non è valido.
* Recuperare un'entità di business dalla persistenza.
* Eseguire un'azione nell'entità di business.
* Salvare l'entità business per la persistenza.
* Restituisce un oggetto appropriato `IActionResult`.

## <a name="unit-testing"></a>Testing unità

[Unit test](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) prevede una parte di un'app di test in isolamento dalle relative dipendenze e l'infrastruttura. Quando è sottoposto a test unit test di logica di controller, solo il contenuto di una singola azione, non il comportamento delle dipendenze o di framework stesso. Come si esegue uno unit test le azioni del controller, assicurarsi di che concentrarsi solo il relativo funzionamento. Uno unit test controller evita elementi quali [filtri](filters.md), [routing](../../fundamentals/routing.md), o [associazione del modello](../models/model-binding.md). Per porre l'attenzione sul test solo una cosa, unit test sono in genere semplici da scrivere e rapido per l'esecuzione. È possibile eseguire un set ben scritto di unit test spesso senza quantità di overhead. Tuttavia, gli unit test non vengono rilevati problemi nell'interazione tra componenti, ovvero lo scopo di [test di integrazione](xref:mvc/controllers/testing#integration-testing).

Se si sta scrivendo filtri personalizzati, route e così via, si consiglia di unit test di loro, ma non come parte dei test per un'azione del controller specifico. Essi devono essere testate in isolamento.

> [!TIP]
> [Creare ed eseguire unit test con Visual Studio](https://www.visualstudio.com/docs/code/create-and-run-unit-tests-vs).

Per illustrare gli unit test, esaminare il seguente controller. Visualizza un elenco di sessioni di confronto e consente nuove sessioni vengono creati con un POST di confronto:

[!code-csharp[Principale](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

Il controller di seguito è illustrata la [principio dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/), prevede l'inserimento di dipendenze per fornire un'istanza di `IBrainstormSessionRepository`. In questo modo abbastanza semplice eseguire il test con un framework di oggetti fittizi, ad esempio [Moq](https://www.nuget.org/packages/Moq/). Il `HTTP GET Index` non dispone di alcun ciclo o diramazione e solo chiama un metodo. Per eseguire il test `Index` (metodo), è necessario verificare che un `ViewResult` viene restituito, con un `ViewModel` del repository `List` metodo.

[!code-csharp[Principale](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

Il `HomeController` `HTTP POST Index` metodo (illustrato in precedenza) è necessario verificare:

* Il metodo di azione restituisce una risposta richiesta non valida `ViewResult` con i dati appropriati quando `ModelState.IsValid` è`false`

* Il `Add` nel repository viene chiamato e un `RedirectToActionResult` viene restituito con gli argomenti appropriati quando `ModelState.IsValid` è true.

Stato del modello non valido può essere testato aggiungendo gli errori utilizzando `AddModelError` come illustrato nel primo test riportato di seguito.

[!code-csharp[Principale](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

Il primo test conferma quando `ModelState` non è valido, lo stesso `ViewResult` viene restituito come per un `GET` richiesta. Si noti che il test non tenta di passare in un modello non valido. Che non funzionerebbe comunque dall'associazione del modello non è in esecuzione (anche se un [test di integrazione](xref:mvc/controllers/testing#integration-testing) utilizzerebbe l'associazione di modelli esercizio). In questo caso, l'associazione di modelli non testato. Queste unità solo test operazioni eseguite dal codice nel metodo di azione.

Il secondo test verifica che, quando `ModelState` è valido, un nuovo `BrainstormSession` viene aggiunto (tramite il repository), e il metodo restituisce un `RedirectToActionResult` con le proprietà previsto. Chiamate simulate che non sono chiamate sono in genere ignorati, ma chiamante `Verifiable` al termine dell'installazione chiamata consente di verificare il test. Questa operazione viene eseguita con la chiamata a `mockRepo.Verify`, che avrà esito negativo del test se non è stato chiamato il metodo previsto.

> [!NOTE]
> La libreria Moq utilizzata in questo esempio consente facilmente di combinare le simulazioni verificabile, o "strict", con le simulazioni non verificabile (detta anche "libero" mocks o stub). Altre informazioni, vedere [personalizzazione del comportamento di simulazione con Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Un altro controller nell'app Visualizza le informazioni correlate a una sessione di brainstorming particolare. Include la logica per gestire i valori di id non valido:

[!code-csharp[Principale](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

Azione del controller è tre casi da testare, uno per ogni `return` istruzione:

[!code-csharp[Principale](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

L'app espone la funzionalità di una web API (un elenco di idee associata a una sessione e un metodo per l'aggiunta di nuove idee a una sessione):

<a name=ideas-controller></a>

[!code-csharp[Principale](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

Il `ForSession` il metodo restituisce un elenco di `IdeaDTO` tipi. Evitare di restituire le entità di dominio aziendali direttamente tramite chiamate API, poiché spesso includono più dati richiede il client API, e vengono inutilmente associare il modello di dominio interno dell'app con l'API Esponi esternamente. Mapping tra le entità di dominio e i tipi si tornerà in rete può essere eseguito manualmente (utilizzando un LINQ `Select` come illustrato di seguito) o tramite una libreria come [AutoMapper](https://github.com/AutoMapper/AutoMapper)

Unit test per il `Create` e `ForSession` metodi API:

[!code-csharp[Principale](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Come indicato in precedenza, per testare il comportamento del metodo quando `ModelState` non è valido, aggiungere un errore del modello al controller come parte del test. Non tentare di test di convalida o modello di associazione del modello negli unit test - test solo il comportamento del metodo di azione se confrontate con un particolare `ModelState` valore.

Il secondo test varia a seconda del repository, restituendo null, pertanto il repository fittizio è configurato per restituire null. Non è necessario creare un database di prova (in memoria o in caso contrario) e creare una query che restituirà questo risultato: può essere eseguita in una singola istruzione come illustrato.

L'ultima verifica che il repository `Update` metodo viene chiamato. Come è stato fatto in precedenza, la simulazione viene chiamata con `Verifiable` e quindi simulati del repository `Verify` metodo viene chiamato per confermare che è stato eseguito il metodo verificabile. Non è una responsabilità di unit test per verificare che il `Update` metodo salvati i dati, che può essere eseguita con un test di integrazione.

## <a name="integration-testing"></a>Test di integrazione

[Test di integrazione](../../testing/integration-testing.md) viene eseguita per assicurare correttamente insieme di moduli separati all'interno del lavoro di app. In generale, qualsiasi che è possibile verificare con uno unit test, è anche possibile testare con un test di integrazione, ma non accade il contrario. Tuttavia, i test di integrazione tendono a essere molto più lento rispetto agli unit test. Pertanto, è consigliabile testare qualsiasi è possibile con gli unit test e usare i test di integrazione per scenari che coinvolgono più collaboratori.

Sebbene potrebbero comunque essere utile, oggetti fittizi utilizzati raramente nei test di integrazione. Gli unit test, oggetti fittizi di un metodo efficace per controllare il comportamento collaboratori all'esterno dell'unità testato ai fini del test. In un test di integrazione, collaboratori reali vengono utilizzati per confermare che l'intero sottosistema insieme funziona correttamente.

### <a name="application-state"></a>Stato dell'applicazione

Una considerazione importante quando si esegue il test di integrazione viene illustrato come impostare lo stato dell'app. I test devono essere eseguiti indipendenti una da altra, e pertanto, ogni test deve iniziare con l'app in uno stato noto. Se l'app non utilizzare un database o disporre di persistenza, non è possibile un problema. Tuttavia, la maggior parte delle applicazioni reali mantenere il loro stato a un tipo di archivio dati, pertanto le modifiche apportate da un test potrebbe influire su un altro test a meno che non viene reimpostato l'archivio dati. Utilizza il programma `TestServer`, è molto semplice per le applicazioni ASP.NET Core host all'interno di questo test di integrazione, ma che non necessariamente concedere l'accesso ai dati verrà utilizzato. Se si utilizza un database effettivo, è possibile che l'app di connettersi a un database di test, che i test possono accedere e verificare viene reimpostati su uno stato noto prima dell'esecuzione di ogni test.

In questa applicazione di esempio, si sta utilizzando il supporto di InMemoryDatabase del Core di Entity Framework, in modo non è possibile connettersi ad esso solo da un progetto di test. Invece, espongono un `InitializeDatabase` metodo l'app `Startup` (classe), che viene chiamato quando viene avviata l'app se si trova nel `Development` ambiente. Il test di integrazione di trarre vantaggio automaticamente da questo, purché l'ambiente è impostato su `Development`. Non è necessario preoccuparsi di ripristino del database, poiché il InMemoryDatabase viene reimpostato ogni volta che l'app viene riavviata.

La `Startup` classe:

[!code-csharp[Principale](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

Verrà visualizzato il `GetTestSession` metodo usata di frequente nei test di integrazione riportati di seguito.

### <a name="accessing-views"></a>Accesso alle visualizzazioni

Ogni classe di test di integrazione consente di configurare il `TestServer` che eseguirà l'app ASP.NET Core. Per impostazione predefinita, `TestServer` ospita l'app web nella cartella in cui è in esecuzione, in questo caso, la cartella di progetto di test. Pertanto, quando si tenta di verificare le azioni del controller che restituiscono `ViewResult`, potrebbe essere visualizzato questo errore:

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "none"} -->

```none
The view 'Index' was not found. The following locations were searched:
(list of locations)
```

Per risolvere questo problema, è necessario configurare radice del contenuto del server, in modo da consentirne l'individuazione le viste per il progetto sottoposto a test. Questa operazione viene eseguita da una chiamata a `UseContentRoot` nel `TestFixture` (classe), illustrato di seguito:

[!code-csharp[Principale](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

Il `TestFixture` classe è responsabile della configurazione e la creazione di `TestServer`, impostare un `HttpClient` per comunicare con il `TestServer`. Ogni dell'integrazione di test Usa il `Client` proprietà per connettersi al server di prova e di effettuare una richiesta.

[!code-csharp[Principale](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

Il primo test precedente, il `responseString` contiene l'effettivo eseguito il rendering HTML dalla visualizzazione, che può essere controllata per verificare contenga i risultati previsti.

Il secondo test costruisce un POST form con un nome univoco della sessione e invia all'app, quindi verifica che venga restituito il reindirizzamento previsto.

### <a name="api-methods"></a>Metodi dell'API

Se l'app espone web API, è consigliabile automatizzare i test conferma vengono eseguiti come previsto. L'elemento predefinito `TestServer` semplifica l'API web di test. Se i metodi API utilizza l'associazione di modelli, è necessario controllare sempre `ModelState.IsValid`, e i test di integrazione sono il posto giusto per confermare che la convalida del modello funzioni correttamente.

Il seguente set di test di `Create` metodo il [IdeasController](xref:mvc/controllers/testing#ideas-controller) classe illustrato in precedenza:

[!code-csharp[Principale](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

A differenza dei test di integrazione delle azioni che restituisce viste HTML, i metodi API web che restituiscono risultati in genere possibile deserializzati come oggetti fortemente tipizzati, come illustrato sopra l'ultimo test. In questo caso, il test deserializza il risultato a un `BrainstormSession` istanza e conferma che l'idea è stato aggiunto correttamente alla relativa raccolta di idee.

In questo articolo sono disponibili esempi aggiuntivi di test di integrazione [progetto di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample).
