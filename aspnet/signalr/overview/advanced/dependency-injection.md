---
uid: signalr/overview/advanced/dependency-injection
title: Inserimento di dipendenze in SignalR | Documenti Microsoft
author: MikeWasson
description: Versioni del software utilizzato in questo argomento di Visual Studio 2013 .NET 4.5 SignalR le versioni precedenti di versione 2 di questo argomento per informazioni sulle versioni precedenti di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26504180"
---
<a name="dependency-injection-in-signalr"></a>Inserimento di dipendenze in SignalR
====================
da [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

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
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


Inserimento di dipendenze è un modo per rimuovere le dipendenze hardcoded tra gli oggetti, rendendo più semplice per sostituire le dipendenze di un oggetto, uno per i test (tramite oggetti fittizi) o per modificare il comportamento in fase di esecuzione. In questa esercitazione viene illustrato come eseguire l'inserimento di dipendenze su hub SignalR. Viene inoltre illustrato come utilizzare i contenitori IoC con SignalR. Un contenitore di inversione di controllo è un framework generale per l'inserimento di dipendenze.

## <a name="what-is-dependency-injection"></a>Che cos'è l'inserimento di dipendenze?

Se si ha già familiarità con l'inserimento di dipendenze, ignorare questa sezione.

*Inserimento di dipendenze* (DI) è un modello in cui gli oggetti non sono responsabili della creazione le proprie dipendenze. Di seguito è riportato un esempio semplice di motivare DI. Si supponga di che avere un oggetto che è necessario registrare i messaggi. È possibile definire un'interfaccia di registrazione:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

L'oggetto, è possibile creare un `ILogger` per registrare i messaggi:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Questo procedimento funziona, ma non è l'approccio migliore. Se si desidera sostituire `FileLogger` con un altro `ILogger` implementazione, sarà necessario modificare `SomeComponent`. Presupponendo che molti altri oggetti di uso `FileLogger`, sarà necessario modificare tutti gli elementi. O se si decide di creare `FileLogger` un singleton, è inoltre necessario apportare modifiche in tutta l'applicazione.

Un approccio migliore consiste nel "inserire" un `ILogger` nell'oggetto, ad esempio, usando un argomento del costruttore:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Dopo l'oggetto non è responsabile della selezione che `ILogger` da utilizzare. È possibile swich `ILogger` implementazioni senza modificare gli oggetti che dipendono da esso.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Questo modello viene denominato [inserimento costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Un altro modello è inserimento di setter, in cui impostare la dipendenza tramite una proprietà o metodo setter.

## <a name="simple-dependency-injection-in-signalr"></a>Inserimento di dipendenze semplice in SignalR

Si consideri l'applicazione di Chat dall'esercitazione [Introduzione a SignalR](../getting-started/tutorial-getting-started-with-signalr.md). Di seguito è la classe dell'hub da quell'applicazione:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Si supponga che si desidera archiviare i messaggi di chat nel server prima di inviarli. Si potrebbe definire un'interfaccia che astrae questa funzionalità e usare DI per inserire l'interfaccia nella `ChatHub` classe.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Il solo problema è che un'applicazione di SignalR non crea direttamente hub; SignalR verranno creati automaticamente. Per impostazione predefinita, SignalR presuppone una classe di hub per avere un costruttore senza parametri. Tuttavia, si può facilmente registrare una funzione per creare istanze di hub e utilizzare questa funzione per eseguire DI. Registrare la funzione chiamando **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Ora SignalR richiamerà questa funzione anonima ogni volta che è necessario creare un `ChatHub` istanza.

## <a name="ioc-containers"></a>Contenitori IoC

Il codice precedente è appropriato per i casi più semplici. Ma se è ancora necessario scrivere questo:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

In un'applicazione complessa con numerose dipendenze, potrebbe essere necessario scrivere una grande quantità di questo codice di "collegamento". Questo codice può essere difficile da gestire, soprattutto se le dipendenze sono annidate. È inoltre difficile allo unit test.

Una soluzione consiste nell'utilizzare un contenitore di inversione di controllo. Un contenitore di inversione di controllo è un componente software che è responsabile della gestione delle dipendenze. Si registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti. Le relazioni di dipendenza determina automaticamente il contenitore. Numero di contenitori IoC consentono inoltre di controllare elementi quali la durata degli oggetti e ambito.

> [!NOTE]
> "IoC" è l'acronimo di "inversione di controllo", che è un modello generale in cui un framework chiama il codice dell'applicazione. Un contenitore IoC costruisce oggetti, quali "inverte" il normale flusso di controllo.


## <a name="using-ioc-containers-in-signalr"></a>Utilizzo di contenitori IoC in SignalR

L'applicazione di Chat è probabilmente troppo semplice per trarre vantaggio da un contenitore di inversione di controllo. Al contrario, verrà ora esaminata la [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) esempio.

L'esempio StockTicker definisce due classi principali:

- `StockTickerHub`: La classe di hub, che gestisce le connessioni client.
- `StockTicker`: Un singleton che contiene i prezzi azionari e li aggiorna periodicamente.

`StockTickerHub`contiene un riferimento di `StockTicker` singleton, mentre `StockTicker` contiene un riferimento al **IHubConnectionContext** per il `StockTickerHub`. Usa questa interfaccia per comunicare con `StockTickerHub` istanze. (Per ulteriori informazioni, vedere [Server Broadcast con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)

È possibile usare un contenitore IoC interfoliati un po' queste dipendenze. In primo luogo, consente di semplificare il `StockTickerHub` e `StockTicker` classi. Nel codice seguente, ho impostata come commento le parti che non è necessaria.

Rimuovere il costruttore senza parametri da `StockTickerHub`. Invece, si utilizzerà sempre DI creare l'hub.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Per StockTicker, rimuovere l'istanza singleton. In un secondo momento, si userà il contenitore di inversione di controllo per controllare la durata di StockTicker. Inoltre, rendere il costruttore pubblico.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Successivamente, è possibile effettuare il refactoring di codice tramite la creazione di un'interfaccia per `StockTicker`. Questa interfaccia verrà usato per separare il `StockTickerHub` dalla `StockTicker` classe.

Visual Studio rende questo tipo di refactoring semplice. Aprire il file StockTicker.cs, fare clic su di `StockTicker` dichiarazione di classe, quindi selezionare **refactoring** ... **Estrai interfaccia**.

![](dependency-injection/_static/image1.png)

Nel **Estrai interfaccia** finestra di dialogo, fare clic su **Seleziona tutto**. Lasciare le altre impostazioni predefinite. Fare clic su **OK**.

![](dependency-injection/_static/image2.png)

Visual Studio crea una nuova interfaccia denominata `IStockTicker`e viene modificato anche `StockTicker` da cui derivare `IStockTicker`.

Aprire il file IStockTicker.cs e modificare l'interfaccia per **pubblica**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

Nel `StockTickerHub` classe, modificare le due istanze di `StockTicker` a `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Creazione di un `IStockTicker` interfaccia non è strettamente necessaria, ma si desidera mostrare come SEN consente di ridurre l'accoppiamento tra componenti dell'applicazione.

## <a name="add-the-ninject-library"></a>Aggiungere la libreria Ninject

Esistono molti contenitori di IoC open source per .NET. Per questa esercitazione si utilizzerà [Ninject](http://www.ninject.org/). (Altre librerie comuni includono [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), e [StructureMap ](http://docs.structuremap.net).)

Usare NuGet Package Manager per installare il [Ninject libreria](https://nuget.org/packages/Ninject/3.0.1.10). In Visual Studio, dal **strumenti** menu selezionare **Gestione pacchetti libreria** | **Package Manager Console**. Nella finestra della Console di gestione pacchetti, immettere il comando seguente:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Sostituire il Resolver di dipendenza di SignalR

Per utilizzare Ninject all'interno di SignalR, creare una classe che deriva da **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Questa classe esegue l'override di **GetService** e **GetServices** metodi di **DefaultDependencyResolver**. SignalR chiama questi metodi per creare vari oggetti in fase di esecuzione, incluse le istanze di hub, nonché diversi servizi utilizzati internamente da SignalR.

- Il **GetService** metodo crea una singola istanza di un tipo. Eseguire l'override di questo metodo per chiamare il kernel di Ninject **TryGet** metodo. Se tale metodo restituisce null, eseguire il fallback per il resolver predefinito.
- Il **GetServices** metodo crea una raccolta di oggetti di un tipo specificato. Eseguire l'override di questo metodo per concatenare i risultati di Ninject e i risultati dal sistema di risoluzione predefinito.

## <a name="configure-ninject-bindings"></a>Configurare le associazioni Ninject

Ora Ninject verrà utilizzata per dichiarare associazioni del tipo.

Aprire classe Startup.cs dell'applicazione (uno creato manualmente in base alle istruzioni pacchetto `readme.txt`, o che è stato creato mediante l'aggiunta di autenticazione per il progetto). Nel `Startup.Configuration` (metodo), creare il contenitore Ninject, che chiama Ninject il *kernel*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Creare un'istanza di questo resolver di dipendenza personalizzata:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Creare un'associazione per `IStockTicker` come indicato di seguito:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Questo codice indica che due elementi. Primo, ogni volta che l'applicazione richiede un `IStockTicker`, il kernel deve creare un'istanza di `StockTicker`. In secondo luogo, la `StockTicker` classe deve essere creato come un oggetto singleton. Ninject verrà creata un'istanza dell'oggetto e restituire la stessa istanza per ogni richiesta.

Creare un'associazione per **IHubConnectionContext** come indicato di seguito:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Questo creatres codice una funzione anonima che restituisce un **IHubConnection**. Il **WhenInjectedInto** metodo indica Ninject per utilizzare questa funzione solo durante la creazione di `IStockTicker` istanze. Il motivo è che consente di creare SignalR **IHubConnectionContext** internamente, istanze e non si desidera eseguire l'override come SignalR li crea. Questa funzione si applica solo ai nostri `StockTicker` classe.

Passare il resolver di dipendenza nel **MapSignalR** metodo mediante l'aggiunta di una configurazione di hub:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Aggiornare il metodo Startup.ConfigureSignalR nella classe di avvio dell'esempio con il nuovo parametro:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Ora SignalR userà il sistema di risoluzione specificato **MapSignalR**, anziché il resolver predefinito.

Ecco l'elenco di codice per `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Per eseguire l'applicazione StockTicker in Visual Studio, premere F5. Nella finestra del browser, passare a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

L'applicazione ha esattamente la stessa funzionalità come prima. (Per una descrizione, vedere [Server Broadcast con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) Non è stato modificato il comportamento. appena creati facili da testare, gestire e sviluppare il codice.
