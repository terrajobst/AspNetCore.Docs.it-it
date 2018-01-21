---
title: Inserimento di dipendenze controller
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 21cffcd285879fdca81cb7d92d0f079d4bf7756c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-into-controllers"></a>Inserimento di dipendenze controller

<a name="dependency-injection-controllers"></a>

Da [Steve Smith](https://ardalis.com/)

Controller MVC ASP.NET Core deve richiedere le relative dipendenze in modo esplicito tramite i relativi costruttori. In alcuni casi, le azioni del controller singoli potrebbero richiedere un servizio e non può essere utile per richiedere a livello di controller. In questo caso, è possibile scegliere di inserire un servizio come parametro al metodo di azione.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="dependency-injection"></a>Inserimento di dipendenze

Inserimento di dipendenze è una tecnica che segue il [principio di inversione dipendenza](http://deviq.com/dependency-inversion-principle/), consentendo di applicazioni deve essere composta da moduli regime di controllo. ASP.NET di base include il supporto incorporato per [inserimento di dipendenze](../../fundamentals/dependency-injection.md), rendendo più facile da testare e gestire le applicazioni.

## <a name="constructor-injection"></a>Inserimento del costruttore

Supporto incorporato dell'ASP.NET Core inserimento di dipendenza basato sul costruttore si estende al controller MVC. Aggiungendo semplicemente un tipo di servizio al controller come parametro costruttore, ASP.NET Core tenterà di risolvere il tipo in questione utilizzando incorporato nel contenitore dei servizi. I servizi sono in genere, ma non sempre, definiti utilizzando le interfacce. Ad esempio, se l'applicazione dispone di logica di business che dipende dal tempo corrente, è possibile inserire un servizio che recupera il tempo (anziché a livello di codice), quali i test passare le implementazioni che usano un determinato periodo di tempo.

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


Implementazione di un'interfaccia come questo in modo che utilizzi l'orologio di sistema in fase di esecuzione è piuttosto semplice:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


A questo punto, è possibile usare il servizio in questo controller. In questo caso, è stata aggiunta logica per il `HomeController` `Index` metodo per visualizzare un saluto all'utente in base all'ora del giorno.

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Se si esegue ora l'applicazione, si verificherà probabilmente un errore:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

Questo errore si verifica quando non è stato configurato un servizio nel `ConfigureServices` metodo nel nostro `Startup` classe. Per specificare che le richieste di `IDateTime` deve essere risolto utilizzando un'istanza di `SystemDateTime`, aggiungere la riga evidenziata nell'elenco seguente per il `ConfigureServices` metodo:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Questo particolare servizio può essere implementato usando una qualsiasi delle diverse opzioni di durata diversa (`Transient`, `Scoped`, o `Singleton`). Vedere [Dependency Injection](../../fundamentals/dependency-injection.md) comprendere ognuna di queste opzioni di ambito verrà influenza il comportamento del servizio.

Una volta configurato il servizio, che esegue l'applicazione e passare alla home page dovrebbe essere visualizzato il messaggio basato sul tempo come previsto:

![Messaggio introduttivo di server](dependency-injection/_static/server-greeting.png)

>[!TIP]
> Vedere [logica del Controller di test](testing.md) per informazioni su come richiedere in modo esplicito le dipendenze [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) nei controller di codice risulta più facile da testare.

Inserimento di dipendenze predefinito del ASP.NET Core supporta solo un singolo costruttore per le classi di richiesta di servizi. Se si dispone di più di un costruttore, si potrebbe ottenere un'eccezione indicante:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Come indicato nel messaggio di errore, è possibile correggere il problema con solo un singolo costruttore. È anche possibile [sostituire il supporto di inserimento di dipendenza predefinito con un'implementazione di terze parti](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), molti dei quali supporta più costruttori.

## <a name="action-injection-with-fromservices"></a>Azione di inserimento con FromServices

In alcuni casi non è necessario un servizio per più di un'azione all'interno del controller. In questo caso, può avere senso per inserire il servizio come parametro al metodo di azione. Questa operazione viene eseguita contrassegnando il parametro con l'attributo `[FromServices]` come illustrato di seguito:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Accesso alle impostazioni da un Controller

Accesso alle impostazioni dell'applicazione o di configurazione all'interno di un controller è un modello comune. Questo tipo di accesso deve utilizzare il modello di opzioni descritto [configurazione](xref:fundamentals/configuration/index). È in genere consigliabile non richiedere impostazioni direttamente dal controller di mediante l'inserimento di dipendenza. Un approccio migliore consiste nella richiesta un `IOptions<T>` istanza, in cui `T` è la classe di configurazione è necessario.

Per utilizzare il modello di opzioni, è necessario creare una classe che rappresenta le opzioni, ad esempio questo:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

È quindi necessario configurare l'applicazione per utilizzare il modello di opzioni e aggiungere la classe di configurazione per la raccolta di servizi in `ConfigureServices`:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> Nell'elenco precedente, è in fase di configurazione dell'applicazione per leggere le impostazioni da un file in formato JSON. È anche possibile configurare le impostazioni interamente nel codice, come illustrato nel codice commentato precedente. Vedere [configurazione](xref:fundamentals/configuration/index) per ulteriori opzioni di configurazione.

Dopo aver specificato un oggetto fortemente tipizzato configurazione (in questo caso, `SampleWebSettings`) e aggiungerlo alla raccolta di servizi, è possibile richiedere qualsiasi metodo di azione o Controller richiedendo un'istanza di `IOptions<T>` (in questo caso, `IOptions<SampleWebSettings>`) . Il codice seguente viene illustrato come uno richiede le impostazioni da un controller:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Modello di opzioni consente le impostazioni e configurazione per essere separato rispetto a altro e assicura il controller è seguito [separazione delle problematiche](http://deviq.com/separation-of-concerns/), poiché non è necessario sapere come o dove trovare le impostazioni informazioni. Inoltre semplifica il controller di test unità [logica del Controller di test](testing.md), poiché non esiste alcun [adesive statico](http://deviq.com/static-cling/) o diretta creazione di istanze di classi di impostazioni all'interno della classe controller.
