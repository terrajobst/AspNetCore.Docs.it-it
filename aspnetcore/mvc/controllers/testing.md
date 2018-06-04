---
title: Test della logica dei controller in ASP.NET Core
author: ardalis
description: Informazioni sul test della logica dei controller in ASP.NET Core con Moq e xUnit.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/testing
ms.openlocfilehash: a0073e4de361c37a6854ceaf54ffd9eaea4837d4
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34567049"
---
# <a name="test-controller-logic-in-aspnet-core"></a>Test della logica dei controller in ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

I controller nelle app ASP.NET MVC devono avere dimensioni ridotte ed essere incentrati sugli aspetti dell'interfaccia utente. Il test e la gestione risultano più complessi con controller di grandi dimensioni che gestiscono aspetti non associati all'interfaccia utente.

[Visualizzare o scaricare un esempio da GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>Test dei controller

I controller sono un componente essenziale di qualsiasi applicazione ASP.NET Core MVC. Di conseguenza è importante verificare che funzionino correttamente nell'app. I test automatizzati eseguono questa verifica e rilevano gli errori dei controller prima che questi raggiungano la fase di produzione. È importante evitare di includere responsabilità non necessarie nei controller e verificare che i test riguardino esclusivamente le responsabilità dei controller.

La logica del controller deve essere minima e non incentrata su logica di business o aspetti dell'infrastruttura (ad esempio l'accesso ai dati). Il test deve riguardare la logica del controller e non il framework. Eseguire test del *comportamento* del controller con input validi e non validi. Eseguire il test delle risposte del controller in base ai risultati dell'operazione di business effettuata dal controller.

Responsabilità tipiche del controller:

* Verificare `ModelState.IsValid`.
* Restituire un errore se `ModelState` non è valido.
* Recuperare un'entità di business dalla persistenza.
* Eseguire un'azione nell'entità di business.
* Salvare l'entità di business nella persistenza.
* Restituire un risultato `IActionResult` appropriato.

## <a name="unit-testing"></a>Testing unità

Lo [unit test](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) prevede l'esecuzione di test su una parte di un'app isolandola dall'infrastruttura e dalle dipendenze. Quando si sottopone a unit test la logica del controller, si verifica solo il contenuto di una singola azione e non il comportamento delle relative dipendenze o del framework. Quando si sottopongono a unit test le azioni del controller, è importante concentrarsi esclusivamente sul funzionamento del controller. Uno unit test del controller non prende in esame elementi come [filtri](filters.md), [routing](../../fundamentals/routing.md) o [associazione di modelli](../models/model-binding.md). Gli unit test risultano in genere facili da creare e rapidi da eseguire, in quanto verificano un solo aspetto. Un set di unit test ben organizzato può essere eseguito spesso senza sovraccarichi eccessivi. Tuttavia gli unit test non rilevano i problemi dell'interazione tra componenti: a questo scopo esistono i [test di integrazione](xref:mvc/controllers/testing#integration-testing).

Se si scrivono filtri personalizzati, route personalizzate e così via è importante sottoporli a unit test, ma non nel quadro dei test relativi a una determinata azione del controller. Il test di questi elementi va eseguito in isolamento.

> [!TIP]
> [Creare ed eseguire unit test con Visual Studio](/visualstudio/test/unit-test-your-code).

Per una dimostrazione di unit test, esaminare il controller seguente. Il controller visualizza un elenco di sessioni di brainstorming e consente di creare nuove sessioni con un elemento POST:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

Il controller si basa sul [principio delle dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/) e prevede di ricevere un'istanza di `IBrainstormSessionRepository` come conseguenza dell'inserimento di dipendenze. Di conseguenza è relativamente semplice eseguire il test con il framework di un oggetto fittizio, ad esempio [Moq](https://www.nuget.org/packages/Moq/). Il metodo `HTTP GET Index` non dispone di cicli o rami e chiama un solo metodo. Per il test di questo metodo `Index` è necessario verificare che venga restituito un elemento `ViewResult` con un `ViewModel` dal metodo `List` del repository.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

Il metodo `HomeController` `HTTP POST Index` (visualizzato sopra) deve verificare quanto segue:

* Il metodo di azione restituisce un `ViewResult` di tipo Richiesta non valida con i dati appropriati quando `ModelState.IsValid` è `false`

* Viene chiamato il metodo `Add` del repository e viene restituito un `RedirectToActionResult` con gli argomenti appropriati quando `ModelState.IsValid` è true.

È possibile eseguire il test dello stato del modello non valido aggiungendo gli errori con `AddModelError` come illustrato nel primo test riportato di seguito.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

Il primo test conferma quando `ModelState` non è valido e restituisce lo stesso risultato `ViewResult` ottenuto in una richiesta `GET`. Si noti che il test non prova a passare un modello non valido. L'operazione non funzionerebbe in ogni caso, dato che l'associazione di modelli non è in esecuzione (tuttavia un [test di integrazione](xref:mvc/controllers/testing#integration-testing) userebbe l'associazione di modelli dell'esercizio). In questo caso, l'associazione di modelli non viene sottoposta a test. Questi unit test verificano solo le operazioni eseguite dal codice del metodo di azione.

Il secondo test verifica che quando `ModelState` è valido viene aggiunto un nuovo elemento `BrainstormSession` (tramite il repository) e il metodo restituisce un elemento `RedirectToActionResult` con le proprietà previste. Le chiamate fittizie che non vengono attivate sono in genere ignorate, ma la chiamata di `Verifiable` al termine della chiamata setup consente di verificarle nel test. Questa operazione viene eseguita con la chiamata a `mockRepo.Verify`, che non supera il test se non è stato chiamato il metodo previsto.

> [!NOTE]
> La libreria Moq usata in questo esempio facilita la combinazione di simulazioni verificabili o "rigide" con simulazioni non verificabili (dette anche simulazioni "generiche" o stub "generici"). Altre informazioni sulla [personalizzazione del comportamento di simulazione con Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Un altro controller dell'app visualizza informazioni correlate a una sessione di brainstorming specifica. Include logica per gestire i valori ID non validi:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

L'azione del controller deve eseguire il test di tre casi, uno per ogni istruzione `return`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

L'app espone la funzionalità di un'API Web (un elenco di idee associate a una sessione di brainstorming e un metodo per aggiungere nuove idee a una sessione):

<a name="ideas-controller"></a>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

Il metodo `ForSession` restituisce un elenco di tipi `IdeaDTO`. Evitare di restituire direttamente le entità di dominio aziendali tramite chiamate API, poiché spesso includono più dati di quelli richiesti dal client API e associano senza necessità il modello di dominio interno dell'app con l'API esposta esternamente. Il mapping tra le entità di dominio e i tipi restituiti in rete può essere eseguito manualmente (usando un LINQ `Select` come illustrato di seguito) o tramite una libreria come [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Unit test per i metodi dell'API `Create` e `ForSession`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Come indicato in precedenza, per il test del comportamento del metodo quando `ModelState` non è valido, aggiungere un errore del modello al controller come parte del test. Non provare il test della convalida o dell'associazione di modelli negli unit test. Eseguire solo il test del comportamento del metodo di azione rispetto a un valore `ModelState` specifico.

Il secondo test dipende dal fatto che il repository restituisca null, pertanto il repository fittizio è configurato per restituire null. Non è necessario creare un database di test (in memoria o con un altro approccio) e creare una query che restituisca questo risultato: l'operazione può essere eseguita in una singola istruzione, come indicato.

L'ultimo test verifica che venga chiamato il metodo `Update` del repository. Come in precedenza, la simulazione viene chiamata con `Verifiable` e quindi il viene chiamato il metodo `Verify` del repository fittizio per confermare che il metodo verificabile sia stato eseguito. Non spetta a un unit test verificare che il metodo `Update` abbia salvato i dati. Questo compito può essere eseguito con un test di integrazione.

## <a name="integration-testing"></a>Test di integrazione

I [test di integrazione](xref:test/integration-tests) vengono eseguiti per verificare che moduli separati dell'app interagiscano correttamente. In generale tutto ciò che è possibile verificare con uno unit test può essere verificato anche con un test di integrazione, ma non viceversa. I test di integrazione tendono tuttavia a essere molto più lenti degli unit test. Pertanto è consigliabile verificare tutti gli elementi possibili con unit test e limitare i test di integrazione agli scenari che coinvolgono più collaboratori.

Anche se possono risultare utili, gli oggetti fittizi vengono usati di rado nei test di integrazione. Negli unit test gli oggetti fittizi sono un metodo efficace per verificare il comportamento previsto dei collaboratori all'esterno dell'unità oggetto del test. In un test di integrazione vengono usati collaboratori reali per confermare che l'intero sottosistema interagisca correttamente.

### <a name="application-state"></a>Stato dell'applicazione

Una considerazione importante quando si eseguono test di integrazione è la modalità di impostazione dello stato dell'app. I test vanno eseguiti indipendentemente l'uno dall'altro, pertanto ogni test deve iniziare con l'app in uno stato noto. Se l'app non usa un database o non dispone di persistenza, questo non dovrebbe rappresentare un problema. Tuttavia la maggior parte delle app reali gestisce la persistenza del proprio stato in un archivio dati, pertanto le modifiche apportate da un test possono influire su un altro test se l'archivio dati non viene reimpostato. Se si usa l'elemento `TestServer` incorporato, l'hosting delle app ASP.NET Core nei test di integrazione è intuitivo, ma non garantisce necessariamente l'accesso ai dati che verranno usati. Se si usa un database, un approccio possibile è fare connettere l'app a un database di test accessibile anche dai test, quindi verificare venga reimpostato su uno stato noto prima dell'esecuzione di ogni singolo test.

In questa applicazione di esempio si usa il supporto InMemoryDatabase di Entity Framework Core, pertanto non è possibile stabilire la connessione direttamente dal progetto di test. Invece si espone un metodo `InitializeDatabase` della classe `Startup` che viene chiamato all'avvio dell'app se si trova nell'ambiente `Development`. I test di integrazione traggono automaticamente vantaggio da questa situazione se impostano l'ambiente su `Development`. Non è necessario preoccuparsi di reimpostare il database, poiché InMemoryDatabase viene reimpostato ad ogni riavvio dell'app.

La classe `Startup`:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

Il metodo `GetTestSession` viene usato spesso nei test di integrazione che seguono.

### <a name="accessing-views"></a>Accesso alle visualizzazioni

Ogni classe di test di integrazione configura l'elemento `TestServer` che esegue l'app ASP.NET Core. Per impostazione predefinita `TestServer` ospita l'app Web nella cartella in cui è in esecuzione, in questo caso la cartella del progetto di test. Di conseguenza quando si prova il test di azioni del controller che restituiscono `ViewResult` può essere visualizzato questo errore:

```
The view 'Index' wasn't found. The following locations were searched:
(list of locations)
```

Per risolvere questo problema, è necessario configurare la radice del contenuto del server in modo che individui le visualizzazioni del progetto sottoposto a test. Questa operazione viene eseguita mediante una chiamata a `UseContentRoot` nella classe `TestFixture`, visualizzata di seguito:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

La classe `TestFixture` è responsabile della configurazione e della creazione di `TestServer` e imposta un `HttpClient` per comunicare con `TestServer`. Ogni test di integrazione usa la proprietà `Client` per connettersi al server di test e creare una richiesta.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

Nel primo dei test precedenti `responseString` contiene il codice HTML sottoposto a rendering della vista, che può essere verificato per confermare che contenga i risultati previsti.

Il secondo test costruisce un'operazione POST del modulo con un nome di sessione univoco e lo pubblica (POST) nell'app, quindi verifica che venga restituito il reindirizzamento previsto.

### <a name="api-methods"></a>Metodi dell'API

Se l'app espone API Web, è consigliabile impostare test automatizzati per confermare che le API vengano eseguite come previsto. L'elemento `TestServer` predefinito semplifica il test delle API Web. Se i metodi API usano l'associazione di modelli è necessario controllare sempre `ModelState.IsValid`. I test di integrazione sono lo strumento ottimale per confermare che la convalida del modello funzioni correttamente.

Il seguente set di test opera sul metodo `Create` nella classe [IdeasController](xref:mvc/controllers/testing#ideas-controller) illustrata in precedenza:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

A differenza dei test di integrazione delle azioni che restituiscono visualizzazioni HTML, i metodi per le API Web che restituiscono risultati sono in genere deserializzabili come oggetti fortemente tipizzati, come illustrato nell'ultimo dei test precedenti. In questo caso il test deserializza il risultato in un'istanza `BrainstormSession` e conferma che l'idea è stata aggiunta correttamente alla relativa raccolta di idee.

Altri esempi di test di integrazione sono disponibili nel [progetto di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) di questo articolo.
