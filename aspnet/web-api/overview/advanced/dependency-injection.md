---
uid: web-api/overview/advanced/dependency-injection
title: Dependency Injection in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: In questa esercitazione viene illustrato come inserire le dipendenze nel controller di ASP.NET Web API. Versioni del software utilizzate nell'esercitazione di Web API 2 Unity Application Block...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: b4cf39c59ed257b0014dbdbecef3eb7bc48f410d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>Dependency Injection in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> In questa esercitazione viene illustrato come inserire le dipendenze nel controller di ASP.NET Web API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Web API 2
> - [Blocco applicazione per Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (versione 5 funziona anche)


## <a name="what-is-dependency-injection"></a>Che cos'è l'inserimento di dipendenze?

Oggetto *dipendenza* è qualsiasi oggetto che richiede un altro oggetto. Ad esempio, è comune per definire un [repository](http://martinfowler.com/eaaCatalog/repository.html) che gestisce l'accesso ai dati. Di seguito è illustrato un esempio. In primo luogo, definiamo un modello di dominio:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Di seguito è una classe semplice di repository che contiene gli elementi in un database, mediante Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Ora è pertanto possibile definire un controller API Web che supporta le richieste GET per `Product` entità. (Spese di spedizione POST e altri metodi per motivi di semplicità.) Di seguito è riportato un primo tentativo:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Si noti che la classe controller dipende `ProductRepository`, e ci stiamo consentendo il controller di creare il `ProductRepository` istanza. Tuttavia, è una buona idea a livello di codice in questo modo, la dipendenza per diversi motivi.

- Se si desidera sostituire `ProductRepository` con un'implementazione diversa, è inoltre necessario modificare la classe controller.
- Se il `ProductRepository` dispone di dipendenze, è necessario configurare questi all'interno del controller. Per un progetto di grandi dimensioni con più controller, il codice di configurazione diventa distribuito il progetto.
- È difficile allo unit test, perché il controller è hardcoded alla query del database. Per uno unit test, è consigliabile utilizzare un repository non è possibile con la progettazione imposta fittizi o stub.

È possibile risolvere questi problemi da *inserendo* repository nel controller. In primo luogo, eseguire il refactoring di `ProductRepository` classe in un'interfaccia:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Fornire quindi il `IProductRepository` come parametro di costruttore:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Questo esempio viene utilizzato [inserimento costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). È inoltre possibile utilizzare *inserimento di setter*, in cui è necessario impostare la dipendenza tramite una proprietà o metodo setter.

Ma ora si verifica un problema, perché l'applicazione non è possibile creare il controller direttamente. API Web crea il controller quando si indirizza la richiesta e l'API Web non riconosce i `IProductRepository`. Si tratta di dove è il resolver di dipendenza di Web API disponibile in.

## <a name="the-web-api-dependency-resolver"></a>Il Resolver di dipendenza di API Web

API Web definisce il **resolver di dipendenza** interfaccia per la risoluzione delle dipendenze. Di seguito è riportata la definizione dell'interfaccia:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Il **IDependencyScope** interfaccia dispone di due metodi:

- **GetService** crea un'istanza di un tipo.
- **GetServices** crea una raccolta di oggetti di un tipo specificato.

Il **resolver di dipendenza** metodo eredita **IDependencyScope** e aggiunge il **BeginScope** metodo. Gli ambiti verrà trattato più avanti in questa esercitazione.

Quando l'API Web crea un'istanza del controller, chiama innanzitutto **IDependencyResolver.GetService**, passando il tipo di controller. È possibile utilizzare questo hook di estensibilità per creare il controller, di risoluzione di eventuali dipendenze. Se **GetService** restituisce null, API Web cerca un costruttore senza parametri nella classe controller.

## <a name="dependency-resolution-with-the-unity-container"></a>Risoluzione delle dipendenze con il contenitore di Unity

Sebbene sia possibile scrivere un completo **resolver di dipendenza** implementazione partendo da zero, l'interfaccia è destinato funga da ponte tra API Web e i contenitori IoC esistenti.

Un contenitore di inversione di controllo è un componente software che è responsabile della gestione delle dipendenze. Si registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti. Le relazioni di dipendenza determina automaticamente il contenitore. Numero di contenitori IoC consentono inoltre di controllare elementi quali la durata degli oggetti e ambito.

> [!NOTE]
> "IoC" è l'acronimo di "inversione di controllo", che è un modello generale in cui un framework chiama il codice dell'applicazione. Un contenitore IoC costruisce oggetti, quali "inverte" il normale flusso di controllo.


Per questa esercitazione si userà [Unity](https://msdn.microsoft.com/en-us/library/ff647202.aspx) da Microsoft Patterns &amp; consigliate. (Altre librerie comuni includono [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), e [StructureMap ](http://docs.structuremap.net/).) Per installare Unity, è possibile utilizzare Gestione pacchetti NuGet. Dal **strumenti** menu in Visual Studio, selezionare **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti, digitare il comando seguente:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Di seguito è un'implementazione di **resolver di dipendenza** che esegue il wrapping di un contenitore di Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Se il **GetService** (metodo) non è possibile risolvere un tipo, deve restituire **null**. Se il **GetServices** metodo non è possibile risolvere un tipo, deve restituire un oggetto raccolta vuoto. Non generano eccezioni per i tipi sconosciuti.


## <a name="configuring-the-dependency-resolver"></a>Configurare il Resolver di dipendenza

Impostare il resolver di dipendenza per il **DependencyResolver** proprietà dell'oggetto globale **HttpConfiguration** oggetto.

Nell'esempio di codice registri il `IProductRepository` interfacciarsi con Unity e crea quindi un `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Ambito di dipendenza e la durata di Controller

I controller vengono creati per ogni richiesta. Per la gestione della durata degli oggetti, **resolver di dipendenza** viene utilizzato il concetto di un *ambito*.

Il resolver di dipendenza è collegata la **HttpConfiguration** oggetto ha un ambito globale. Quando l'API Web viene creato un controller, chiama **BeginScope**. Questo metodo restituisce un **IDependencyScope** che rappresenta un ambito figlio.

API Web chiama quindi **GetService** nell'ambito figlio per creare il controller. Una volta completata, richiesta di chiamate all'API Web **Dispose** nell'ambito figlio. Utilizzare il **Dispose** metodo per eliminare le dipendenze del controller.

Modalità di implementazione di **BeginScope** varia a seconda del contenitore di inversione di controllo. Per Unity, ambito corrisponde a un contenitore figlio:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

La maggior parte dei contenitori IoC dispongono di equivalenti simili.
