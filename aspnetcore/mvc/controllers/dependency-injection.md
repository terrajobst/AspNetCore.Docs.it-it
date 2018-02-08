---
title: Inserimento di dipendenze in controller
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 118f504311b58258b5a0510477280505135dd2d9
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-into-controllers"></a>Inserimento di dipendenze in controller

<a name="dependency-injection-controllers"></a>

Di [Steve Smith](https://ardalis.com/)

I controller ASP.NET Core MVC devono richiedere le proprie dipendenze in modo esplicito tramite i rispettivi costruttori. In alcune istanze, singole azioni di controller possono richiedere un servizio che potrebbe non avere senso richiedere a livello di controller. In questo caso, è anche possibile inserire un servizio come parametro nel metodo di azione.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="dependency-injection"></a>Inserimento di dipendenze

L'inserimento di dipendenze è una tecnica che segue il [principio di inversione delle dipendenze](http://deviq.com/dependency-inversion-principle/) e consente di comporre applicazioni con moduli poco accoppiati. ASP.NET Core dispone del supporto incorporato dell'[inserimento di dipendenze](../../fundamentals/dependency-injection.md), rendendo le applicazioni più facili da testare e gestire.

## <a name="constructor-injection"></a>Inserimento di costruttori

Il supporto incorporato di ASP.NET Core per l'inserimento di dipendenze basato sul costruttore si estende ai controller MVC. Aggiungendo semplicemente un tipo di servizio al controller come parametro del costruttore, ASP.NET Core tenta di risolvere il tipo corrispondente tramite il proprio contenitore di servizi incorporato. In genere, ma non sempre, i servizi vengono definiti tramite interfacce. Se, ad esempio, la logica di business dell'applicazione dipende dall'ora corrente, è possibile inserire un servizio che recuperi l'ora, anziché impostarlo come hardcoded. Ciò consente di superare i test nelle implementazioni che usano un'ora impostata.

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


L'implementazione di un'interfaccia come questa che usi l'orologio di sistema in fase di runtime è semplice:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


A questo punto è possibile usare il servizio nel controller. In questo caso, è stata aggiunta al metodo `HomeController` `Index` la logica che consente di visualizzare un saluto a seconda dell'ora del giorno.

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Se si esegue l'applicazione ora, molto probabilmente si verifica un errore:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

Questo errore si verifica se non è stato configurato alcun servizio nel metodo `ConfigureServices` della classe `Startup`. Per specificare che le richieste di `IDateTime` devono essere risolte tramite un'istanza di `SystemDateTime`, aggiungere la riga evidenziata nel codice seguente al metodo `ConfigureServices`:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Questo servizio specifico può essere implementato usando una qualsiasi delle diverse opzioni di durata (`Transient`, `Scoped` o `Singleton`). Per comprendere in che modo ognuna di queste opzioni di ambito influisca sul comportamento del servizio, vedere [Inserimento di dipendenze](../../fundamentals/dependency-injection.md).

Dopo che il servizio è stato configurato, se si esegue l'applicazione e si passa alla home page viene visualizzato il messaggio basato sull'ora, come previsto:

![Saluto del server](dependency-injection/_static/server-greeting.png)

>[!TIP]
> Vedere [Test della logica dei controller](testing.md) per informazioni su come la richiesta esplicita di dipendenze [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) nei controller renda più semplice il test del codice.

L'inserimento di dipendenze predefinito in ASP.NET Core supporta un unico costruttore per la richiesta di servizi da parte delle classi. Se ci sono più costruttori, può essere visualizzata l'eccezione seguente:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Come indicato nel messaggio di errore, è possibile correggere il problema mantenendo un unico costruttore. È anche possibile [sostituire il supporto dell'inserimento di dipendenze predefinito con un'implementazione di terze parti](../../fundamentals/dependency-injection.md#replacing-the-default-services-container). Molte di queste implementazioni supportano più costruttori.

## <a name="action-injection-with-fromservices"></a>Inserimento di azioni con FromServices

Talvolta un servizio è necessario per un sola azione all'interno del controller. In questo caso, può avere senso inserire il servizio come parametro nel metodo di azione. Per eseguire questa operazione, si contrassegna il parametro con l'attributo `[FromServices]`, come illustrato qui:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Accesso alle impostazioni da un controller

L'accesso alle impostazioni di un'applicazione o alle impostazioni di configurazione da un controller è un tipo di azione comune. Questo tipo di accesso deve usare il modello di opzioni descritto in [Configurazione](xref:fundamentals/configuration/index). In genere non si devono richiedere impostazioni direttamente dal controller tramite l'inserimento di dipendenze. Un approccio migliore consiste nella richiesta di un'istanza di `IOptions<T>`, dove `T` è la classe di configurazione necessaria.

Per usare il modello di opzioni, è necessario creare una classe che rappresenti le opzioni, ad esempio questa:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

È quindi necessario configurare l'applicazione in modo che usi il modello di opzioni e aggiungere la classe di configurazione alla raccolta dei servizi in `ConfigureServices`:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> Il codice precedente configura l'applicazione in modo che legga le impostazioni da un file in formato JSON. È anche possibile configurare interamente le impostazioni nel codice, come illustrato nel codice commentato precedente. Per altre opzioni di configurazione, vedere [Configurazione](xref:fundamentals/configuration/index).

Dopo aver specificato un oggetto di configurazione fortemente tipizzato (in questo caso, `SampleWebSettings`) e dopo averlo aggiunto alla raccolta di servizi, è possibile richiederlo da qualsiasi controller o metodo di azione richiedendo un'istanza di `IOptions<T>` (in questo caso, `IOptions<SampleWebSettings>`). Il codice seguente illustra come richiedere le impostazioni da un controller:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Se si segue il modello di opzioni, è possibile disaccoppiare le impostazioni e la configurazione. Ciò garantisce che il controller segua il principio della [separazione delle competenze](http://deviq.com/separation-of-concerns/), poiché non deve sapere come o dove trovare le informazioni sulle impostazioni. È anche più facile eseguire lo unit test del controller (vedere [Test della logica dei controller](testing.md)), poiché non ci sono [vincoli statici](http://deviq.com/static-cling/) o creazione diretta di istanze all'interno della classe del controller.
