---
uid: web-api/overview/advanced/dependency-injection
title: Inserimento delle dipendenze in ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come inserire le dipendenze nel controller dell'API Web ASP.NET. Versioni del software utilizzate nell'esercitazione di Web API 2 Unity Application Block...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 92ce5eadc7f371540295c1c4279f817dba09f8e3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369172"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>Inserimento delle dipendenze in ASP.NET Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Questa esercitazione illustra come inserire le dipendenze nel controller dell'API Web ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - API Web 2
> - [Unity Application Block](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (versione 5 funziona anche)


## <a name="what-is-dependency-injection"></a>Che cos'è l'inserimento delle dipendenze?

Oggetto *dipendenza* è qualsiasi oggetto che richiede un altro oggetto. Ad esempio, è comune per definire un [repository](http://martinfowler.com/eaaCatalog/repository.html) che gestisce l'accesso ai dati. Di seguito è illustrato un esempio. In primo luogo, si definirà un modello di dominio:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Di seguito è una classe di repository semplice che archivia gli elementi in un database usando Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

A questo punto è possibile definire un controller API Web che supporta le richieste GET per `Product` entità. (Spese di spedizione POST e altri metodi per motivi di semplicità.) Di seguito è un primo tentativo:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Si noti che la classe controller dipende `ProductRepository`, e si sta consentendo il controller crea il `ProductRepository` istanza. Tuttavia, è opportuno evitare di livello di codice in questo modo, la dipendenza per diversi motivi.

- Se si desidera sostituire `ProductRepository` con un'implementazione diversa, è anche necessario modificare la classe controller.
- Se il `ProductRepository` ha dipendenze, è necessario configurare questi all'interno del controller. Per un progetto di grandi dimensioni con più controller, il codice di configurazione diventa sparsi tra il progetto.
- È difficile allo unit test, perché il controller è hardcoded su query del database. Per uno unit test, è consigliabile usare un repository fittizi o stub, che non è possibile eseguire con la progettazione di imposta.

Possiamo risolvere questi problemi dal *inserimento* il repository nel controller. In primo luogo, eseguire il refactoring di `ProductRepository` classe in un'interfaccia:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Fornire quindi il `IProductRepository` come parametro di costruttore:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Questo esempio viene usato [inserimento del costruttore](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). È anche possibile usare *inserimento di setter*, in cui è necessario impostare la dipendenza attraverso un metodo di impostazione o una proprietà.

Ma Ecco presentarsi un problema, perché l'applicazione non crea direttamente il controller. API Web crea il controller quando indirizza la richiesta e API Web non riconosce i `IProductRepository`. Questo è il resolver di dipendenza di API Web entra in gioco.

## <a name="the-web-api-dependency-resolver"></a>Il Resolver di dipendenza di API Web

API Web definisce la **IDependencyResolver** interfaccia per la risoluzione delle dipendenze. Ecco la definizione dell'interfaccia:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Il **IDependencyScope** interfaccia dispone di due metodi:

- **GetService** crea un'istanza di un tipo.
- **GetServices** crea una raccolta di oggetti di un tipo specificato.

Il **IDependencyResolver** metodo eredita **IDependencyScope** e aggiunge i **BeginScope** (metodo). Gli ambiti saranno trattati più avanti in questa esercitazione.

Quando l'API Web crea un'istanza del controller, viene prima chiamato **IDependencyResolver.GetService**, passando il tipo di controller. È possibile utilizzare questo hook di estensibilità per creare il controller, la risoluzione di eventuali dipendenze. Se **GetService** restituisce null, API Web cerca un costruttore senza parametri nella classe controller.

## <a name="dependency-resolution-with-the-unity-container"></a>Risoluzione delle dipendenze con il contenitore di Unity

Anche se è possibile scrivere una completa **IDependencyResolver** implementazione da zero, l'interfaccia è realmente progettato per fungere da ponte tra API Web e i contenitori IoC esistenti.

Un contenitore IoC è un componente software che è responsabile della gestione delle dipendenze. Registrare i tipi con il contenitore e quindi usare il contenitore per creare oggetti. Il contenitore determina automaticamente le relazioni di dipendenza. Numerosi contenitori IoC consentono anche di controllare elementi quali la durata degli oggetti e l'ambito.

> [!NOTE]
> "IoC" è l'acronimo di "inversione di controllo", ovvero un modello generale in cui un framework chiama il codice dell'applicazione. Un contenitore IoC costruisce oggetti, quali "inverte" il normale flusso di controllo.


Per questa esercitazione, useremo [Unity](https://msdn.microsoft.com/library/ff647202.aspx) da Microsoft Patterns &amp; procedure consigliate. (Includono altre librerie molto diffuse [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), e [StructureMap ](http://docs.structuremap.net/).) È possibile usare Gestione pacchetti NuGet per installare Unity. Dal **degli strumenti** dal menu di Visual Studio, selezionare **Library Package Manager**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti, digitare il comando seguente:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Ecco un'implementazione di **IDependencyResolver** che esegue il wrapping di un contenitore di Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Se il **GetService** metodo non è possibile risolvere un tipo, deve restituire **null**. Se il **GetServices** (metodo) non è possibile risolvere un tipo, deve restituire un oggetto raccolta vuoto. Non generare eccezioni per i tipi sconosciuti.


## <a name="configuring-the-dependency-resolver"></a>Configurare il Resolver di dipendenza

Impostare il resolver di dipendenza per il **DependencyResolver** proprietà dell'oggetto globale **HttpConfiguration** oggetto.

Il codice seguente registra il `IProductRepository` interfacciarsi con Unity e quindi crea un `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Ambito delle dipendenze e la durata di Controller

I controller vengono creati per ogni richiesta. Per la gestione della durata degli oggetti, **IDependencyResolver** Usa il concetto di un *ambito*.

Il resolver di dipendenza associata ai **HttpConfiguration** oggetto ha un ambito globale. Quando l'API Web crea un controller, viene chiamato **BeginScope**. Questo metodo restituisce un **IDependencyScope** che rappresenta un ambito figlio.

API Web chiamerà **GetService** nell'ambito figlio per creare il controller. Quando richiesta risulta completata, l'API Web chiama **Dispose** nell'ambito figlio. Usare la **Dispose** metodo per l'eliminazione delle dipendenze del controller.

Modalità di implementazione **BeginScope** dipende dal contenitore IoC. Per Unity, ambito corrisponde a un contenitore figlio:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

La maggior parte dei contenitori IoC dispongono di equivalenti simile.
