---
title: Test della logica dei controller in ASP.NET Core
author: ardalis
description: Informazioni sul test della logica dei controller in ASP.NET Core con Moq e xUnit.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/testing
ms.openlocfilehash: d0b2a25d00187c088671be147844aa892f824c6e
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2018
ms.locfileid: "41751504"
---
# <a name="test-controller-logic-in-aspnet-core"></a>Test della logica dei controller in ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

I controller sono un componente essenziale di qualsiasi applicazione ASP.NET Core MVC. Di conseguenza è importante verificare che funzionino correttamente nell'app. I test automatizzati eseguono questa verifica e rilevano gli errori dei controller prima che questi raggiungano la fase di produzione. È importante evitare di includere responsabilità non necessarie nei controller e verificare che i test riguardino esclusivamente le responsabilità dei controller.

La logica del controller deve essere minima e non incentrata su logica di business o aspetti dell'infrastruttura (ad esempio l'accesso ai dati). Il test deve riguardare la logica del controller e non il framework. Eseguire test del *comportamento* del controller con input validi e non validi. Eseguire il test delle risposte del controller in base ai risultati dell'operazione di business effettuata dal controller.

Responsabilità tipiche del controller:

* Verificare `ModelState.IsValid`.
* Restituire un errore se `ModelState` non è valido.
* Recuperare un'entità di business dalla persistenza.
* Eseguire un'azione nell'entità di business.
* Salvare l'entità di business nella persistenza.
* Restituire un risultato `IActionResult` appropriato.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>Unit test della logica dei controller

Gli [unit test](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) implicano l'esecuzione di test su una parte di un'app isolandola dall'infrastruttura e dalle dipendenze. Quando si sottopone a unit test la logica del controller, si verifica solo il contenuto di una singola azione e non il comportamento delle relative dipendenze o del framework. Quando si sottopongono a unit test le azioni del controller, è importante concentrarsi esclusivamente sul funzionamento del controller. Uno unit test del controller non prende in esame elementi come [filtri](xref:mvc/controllers/filters), [routing](xref:fundamentals/routing) o [associazione di modelli](xref:mvc/models/model-binding). Gli unit test risultano in genere facili da creare e rapidi da eseguire, in quanto verificano un solo aspetto. Un set di unit test ben organizzato può essere eseguito spesso senza sovraccarichi eccessivi. Tuttavia gli unit test non rilevano i problemi dell'interazione tra componenti: a questo scopo esistono i [test di integrazione](xref:test/integration-tests).

Se si scrivono route e filtri personalizzati, è opportuno sottoporli a unit test in isolamento e non durante i test relativi a una determinata azione del controller.

> [!TIP]
> [Creare ed eseguire unit test con Visual Studio](/visualstudio/test/unit-test-your-code).

Per una dimostrazione di unit test, esaminare il controller seguente. Il controller visualizza un elenco di sessioni di brainstorming e consente di creare nuove sessioni con un elemento POST:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

Il controller si basa sul [principio delle dipendenze esplicite](http://deviq.com/explicit-dependencies-principle/) e prevede di ricevere un'istanza di `IBrainstormSessionRepository` come conseguenza dell'inserimento di dipendenze. Di conseguenza è relativamente semplice eseguire il test con il framework di un oggetto fittizio, ad esempio [Moq](https://www.nuget.org/packages/Moq/). Il metodo `HTTP GET Index` non dispone di cicli o rami e chiama un solo metodo. Per il test di questo metodo `Index` è necessario verificare che venga restituito un elemento `ViewResult` con un `ViewModel` dal metodo `List` del repository.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

Il metodo `HomeController` `HTTP POST Index` (visualizzato sopra) deve verificare quanto segue:

* Il metodo di azione restituisce un elemento `ViewResult` di tipo Richiesta non valida con i dati appropriati quando `ModelState.IsValid` è `false`.

* Viene chiamato il metodo `Add` del repository e viene restituito un `RedirectToActionResult` con gli argomenti appropriati quando `ModelState.IsValid` è true.

È possibile eseguire il test dello stato del modello non valido aggiungendo gli errori con `AddModelError` come illustrato nel primo test riportato di seguito.

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

Il primo test conferma quando `ModelState` non è valido e restituisce lo stesso risultato `ViewResult` ottenuto in una richiesta `GET`. Si noti che il test non prova a passare un modello non valido. L'operazione non funzionerebbe in ogni caso, dato che l'associazione di modelli non è in esecuzione (tuttavia un [test di integrazione](xref:test/integration-tests) userebbe l'associazione di modelli dell'esercizio). In questo caso, l'associazione di modelli non viene sottoposta a test. Questi unit test verificano solo le operazioni eseguite dal codice del metodo di azione.

Il secondo test verifica che quando `ModelState` è valido viene aggiunto un nuovo elemento `BrainstormSession` (tramite il repository) e il metodo restituisce un elemento `RedirectToActionResult` con le proprietà previste. Le chiamate fittizie che non vengono attivate sono in genere ignorate, ma la chiamata di `Verifiable` al termine della chiamata setup consente di verificarle nel test. Questa operazione viene eseguita con la chiamata a `mockRepo.Verify`, che non supera il test se non è stato chiamato il metodo previsto.

> [!NOTE]
> La libreria Moq usata in questo esempio facilita la combinazione di simulazioni verificabili o "rigide" con simulazioni non verificabili (dette anche simulazioni "generiche" o stub "generici"). Altre informazioni sulla [personalizzazione del comportamento di simulazione con Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).

Un altro controller dell'app visualizza informazioni correlate a una sessione di brainstorming specifica. Include logica per gestire i valori ID non validi:

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

L'azione del controller deve eseguire il test di tre casi, uno per ogni istruzione `return`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

L'app espone la funzionalità di un'API Web (un elenco di idee associate a una sessione di brainstorming e un metodo per aggiungere nuove idee a una sessione):

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

Il metodo `ForSession` restituisce un elenco di tipi `IdeaDTO`. Evitare di restituire direttamente le entità di dominio aziendali tramite chiamate API, poiché spesso includono più dati di quelli richiesti dal client API e associano senza necessità il modello di dominio interno dell'app con l'API esposta esternamente. Il mapping tra le entità di dominio e i tipi restituiti in rete può essere eseguito manualmente (usando un LINQ `Select` come illustrato di seguito) o tramite una libreria come [AutoMapper](https://github.com/AutoMapper/AutoMapper).

Unit test per i metodi dell'API `Create` e `ForSession`:

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

Come indicato in precedenza, per il test del comportamento del metodo quando `ModelState` non è valido, aggiungere un errore del modello al controller come parte del test. Non provare il test della convalida o dell'associazione di modelli negli unit test. Eseguire solo il test del comportamento del metodo di azione rispetto a un valore `ModelState` specifico.

Il secondo test dipende dal fatto che il repository restituisca null, pertanto il repository fittizio è configurato per restituire null. Non è necessario creare un database di test (in memoria o con un altro approccio) e creare una query che restituisca questo risultato: l'operazione può essere eseguita in una singola istruzione, come indicato.

L'ultimo test verifica che venga chiamato il metodo `Update` del repository. Come in precedenza, la simulazione viene chiamata con `Verifiable` e quindi il viene chiamato il metodo `Verify` del repository fittizio per confermare che il metodo verificabile sia stato eseguito. Non spetta a un unit test verificare che il metodo `Update` abbia salvato i dati. Questo compito può essere eseguito con un test di integrazione.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:test/integration-tests>
