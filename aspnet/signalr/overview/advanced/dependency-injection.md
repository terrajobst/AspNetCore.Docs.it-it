---
uid: signalr/overview/advanced/dependency-injection
title: Inserimento delle dipendenze in SignalR | Microsoft Docs
author: MikeWasson
description: Le versioni del software utilizzato in questo argomento di Visual Studio 2013 .NET 4.5 SignalR le versioni precedenti la versione 2 di questo argomento per informazioni sulle versioni precedenti di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 0b4276d25c999c2a78864a856f7f3de233c9ac87
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397759"
---
<a name="dependency-injection-in-signalr"></a>Inserimento delle dipendenze in SignalR
====================
dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
> 
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina. Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).


Inserimento di dipendenze è un modo per rimuovere le dipendenze hardcoded tra gli oggetti, rendendo più semplice per sostituire le dipendenze di un oggetto, uno per i test (tramite oggetti fittizi) o per modificare il comportamento in fase di esecuzione. Questa esercitazione illustra come eseguire l'inserimento di dipendenze sugli hub SignalR. Viene inoltre illustrato come usare contenitori IoC con SignalR. Un contenitore IoC è un framework generale per l'inserimento di dipendenze.

## <a name="what-is-dependency-injection"></a>Che cos'è l'inserimento delle dipendenze?

Se si ha già familiarità con inserimento delle dipendenze, ignorare questa sezione.

*Inserimento di dipendenze* (inserimento delle dipendenze) è un modello in cui gli oggetti non sono responsabili della creazione le proprie dipendenze. Ecco un esempio semplice per motivare l'inserimento delle dipendenze. Si supponga di che avere un oggetto che è necessario registrare i messaggi. È possibile definire un'interfaccia di registrazione:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

L'oggetto, è possibile creare un `ILogger` per registrare i messaggi:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Questo procedimento funziona, ma non è la migliore struttura. Se si desidera sostituire `FileLogger` con un'altra `ILogger` implementazione, sarà necessario modificare `SomeComponent`. Presumere che molti degli altri oggetti usare `FileLogger`, sarà necessario modificare tutti gli elementi. O se si decide di apportare `FileLogger` un singleton, è inoltre necessario apportare modifiche in tutta l'applicazione.

Un approccio migliore consiste nel "inserire" un `ILogger` nell'oggetto, ad esempio, usando un argomento del costruttore:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

A questo punto l'oggetto non è responsabile della selezione che `ILogger` da usare. È possibile swich `ILogger` implementazioni senza modificare gli oggetti che dipendono da esso.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Questo modello viene denominato [inserimento del costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Un altro modello è l'inserimento di setter, in cui è impostare la dipendenza attraverso un metodo di impostazione o una proprietà.

## <a name="simple-dependency-injection-in-signalr"></a>Inserimento di dipendenze semplice in SignalR

Prendere in considerazione l'applicazione di Chat nell'esercitazione [Introduzione a SignalR](../getting-started/tutorial-getting-started-with-signalr.md). Questa è la classe dell'hub da quell'applicazione:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Si supponga che si desidera archiviare i messaggi di chat nel server prima di inviarli. Si potrebbe definire un'interfaccia che consente di astrarre questa funzionalità e usare l'inserimento delle dipendenze per inserire l'interfaccia nel `ChatHub` classe.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

L'unico problema è che un'applicazione di SignalR non crea direttamente hub; SignalR verranno creati automaticamente. Per impostazione predefinita, SignalR si aspetta che una classe di hub per avere un costruttore senza parametri. Tuttavia, è possibile facilmente registrare una funzione per creare istanze di hub e usare questa funzione per eseguire l'inserimento delle dipendenze. Registrare la funzione chiamando **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

A questo punto SignalR richiama questa funzione anonima ogni volta che è necessario crearne un `ChatHub` istanza.

## <a name="ioc-containers"></a>Contenitori IoC

Il codice precedente è appropriato per i casi semplici. Ma era ancora necessario scrivere questo codice:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

In un'applicazione complessa con numerose dipendenze, si potrebbe essere necessario scrivere una grande quantità di questo codice di "collegamento". Questo codice può essere difficile da gestire, soprattutto se le dipendenze sono annidate. È anche difficile lo unit test.

Un'unica soluzione consiste nell'usare un contenitore IoC. Un contenitore IoC è un componente software che è responsabile della gestione delle dipendenze. Registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti. Il contenitore determina automaticamente le relazioni di dipendenza. Numerosi contenitori IoC consentono anche di controllare elementi quali la durata degli oggetti e l'ambito.

> [!NOTE]
> "IoC" è l'acronimo di "inversione di controllo", ovvero un modello generale in cui un framework chiama il codice dell'applicazione. Un contenitore IoC costruisce oggetti, quali "inverte" il normale flusso di controllo.


## <a name="using-ioc-containers-in-signalr"></a>Uso dei contenitori IoC in SignalR

L'applicazione di Chat è probabilmente troppo semplice per trarre vantaggio da un contenitore IoC. Al contrario, verrà ora esaminato il [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) esempio.

L'esempio StockTicker definisce due classi principali:

- `StockTickerHub`: La classe dell'hub, che gestisce le connessioni client.
- `StockTicker`: Un singleton che contiene quotazioni di titoli e li aggiorna periodicamente.

`StockTickerHub` contiene un riferimento al `StockTicker` singleton, mentre `StockTicker` contiene un riferimento al **IHubConnectionContext** per il `StockTickerHub`. Usa questa interfaccia per comunicare con `StockTickerHub` istanze. (Per altre informazioni, vedere [trasmissione Server con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)

È possibile usare un contenitore IoC interfoliati un po' queste dipendenze. In primo luogo, è possibile semplificare la `StockTickerHub` e `StockTicker` classi. Nel codice seguente, ho ho aggiunto le parti che non è necessario.

Rimuovere il costruttore senza parametri da `StockTickerHub`. In alternativa, sempre si userà l'inserimento delle dipendenze per creare l'hub.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Per StockTicker, rimuovere l'istanza singleton. In un secondo momento, si userà il contenitore IoC per controllare la durata StockTicker. Verificare inoltre che il costruttore pubblico.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Successivamente, è possibile effettuare il refactoring di codice tramite la creazione di un'interfaccia per `StockTicker`. Si userà questa interfaccia per disaccoppiare le `StockTickerHub` dal `StockTicker` classe.

Visual Studio rende questo tipo di refactoring semplice. Aprire il file StockTicker.cs, fare clic sui `StockTicker` dichiarazione di classe, quindi selezionare **effettuare il refactoring** ... **Estrai interfaccia**.

![](dependency-injection/_static/image1.png)

Nel **Estrai interfaccia** finestra di dialogo, fare clic su **Seleziona tutto**. Lasciare le altre impostazioni predefinite. Fare clic su **OK**.

![](dependency-injection/_static/image2.png)

Visual Studio crea una nuova interfaccia denominata `IStockTicker`e viene modificato anche `StockTicker` da cui derivare `IStockTicker`.

Aprire il file IStockTicker.cs e modificare l'interfaccia **pubblica**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

Nel `StockTickerHub` classe, modificare le due istanze di `StockTicker` a `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Creazione di un `IStockTicker` interfaccia non è strettamente necessaria, ma ho preferito dimostrare come l'inserimento delle dipendenze può contribuire a ridurre l'accoppiamento tra componenti dell'applicazione.

## <a name="add-the-ninject-library"></a>Aggiungere la libreria Ninject

Esistono numerosi contenitori IoC open source per .NET. Per questa esercitazione userà [Ninject](http://www.ninject.org/). (Includono altre librerie molto diffuse [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), e [StructureMap ](http://docs.structuremap.net).)

Usare Gestione pacchetti NuGet per installare il [Ninject libreria](https://nuget.org/packages/Ninject/3.0.1.10). In Visual Studio dal **strumenti** dal menu **Library Package Manager** | **Package Manager Console**. Nella finestra della Console di gestione pacchetti immettere il comando seguente:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Sostituire il Resolver di dipendenza di SignalR

Per usare Ninject all'interno di SignalR, creare una classe che deriva da **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Questa classe esegue l'override di **GetService** e **GetServices** metodi **DefaultDependencyResolver**. SignalR chiama questi metodi per creare vari oggetti in fase di esecuzione, tra cui le istanze di hub, nonché diversi servizi utilizzati internamente da SignalR.

- Il **GetService** metodo crea un'istanza di un tipo singola. Eseguire l'override di questo metodo per chiamare il kernel di Ninject **TryGet** (metodo). Se tale metodo restituisce null, eseguire il fallback per il resolver predefinito.
- Il **GetServices** metodo crea una raccolta di oggetti di un tipo specificato. Eseguire l'override di questo metodo per concatenare i risultati da Ninject con i risultati dal sistema di risoluzione predefinito.

## <a name="configure-ninject-bindings"></a>Configurare i binding Ninject

A questo punto si userà Ninject per dichiarare associazioni di tipi.

Aprire classe Startup.cs dell'applicazione (che si sia creato manualmente seguendo le istruzioni di pacchetto in `readme.txt`, o che sia stato creato aggiungendo l'autenticazione al progetto). Nel `Startup.Configuration` metodo, creare il contenitore Ninject, che chiama Ninject il *kernel*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Creare un'istanza del nostro resolver di dipendenza personalizzata:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Creare un'associazione per `IStockTicker` come indicato di seguito:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Questo codice indica che due cose. Innanzitutto, ogni volta che l'applicazione necessita di un `IStockTicker`, il kernel deve creare un'istanza di `StockTicker`. Secondo, il `StockTicker` classe deve essere un oggetto creato come un oggetto singleton. Ninject verranno creare un'istanza dell'oggetto e restituire la stessa istanza per ogni richiesta.

Creare un'associazione per **IHubConnectionContext** come indicato di seguito:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Questo codice creatres una funzione anonima che restituisce un **IHubConnection**. Il **WhenInjectedInto** metodo indica a Ninject usare questa funzione solo quando si crea `IStockTicker` istanze. Il motivo è che consente di creare SignalR **IHubConnectionContext** istanze internamente e non si desidera eseguire l'override come SignalR li crea. Questa funzione si applica solo ai nostri `StockTicker` classe.

Passare il resolver di dipendenza nel **MapSignalR** metodo mediante l'aggiunta di una configurazione di hub:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Aggiornare il metodo Startup.ConfigureSignalR nella classe Startup dell'esempio con il nuovo parametro:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

A questo punto SignalR utilizzerà il sistema di risoluzione specificato nel **MapSignalR**, anziché il resolver predefinito.

Di seguito è riportato per il codice completo `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Per eseguire l'applicazione StockTicker in Visual Studio, premere F5. Nella finestra del browser, passare a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

L'applicazione ha esattamente la stessa funzionalità come prima. (Per una descrizione, vedere [trasmissione Server con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) È ancora stato modificato il comportamento. semplicemente più semplice da testare, gestire e sviluppare il codice.
