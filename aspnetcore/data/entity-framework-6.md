---
title: Introduzione a ASP.NET di base e di Entity Framework 6
author: tdykstra
description: In questo articolo viene illustrato come utilizzare Entity Framework 6 in un'applicazione ASP.NET Core.
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 7f3c1f28c1e0b3a68db7f6f84c56b18643b56cc8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a>Introduzione a ASP.NET di base e di Entity Framework 6

Da [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), e [Tom Dykstra](https://github.com/tdykstra)

In questo articolo viene illustrato come utilizzare Entity Framework 6 in un'applicazione ASP.NET Core.

## <a name="overview"></a>Panoramica

Per l'utilizzo di Entity Framework 6, il progetto deve eseguire la compilazione con .NET Framework, come Entity Framework 6 non supporta .NET Core. Se sono necessarie funzionalità multipiattaforma, è necessario eseguire l'aggiornamento a [Entity Framework Core](https://docs.microsoft.com/ef/).

Il metodo consigliato per l'utilizzo di Entity Framework 6 in un'applicazione ASP.NET di base consiste nell'inserire il contesto EF6 e classi del modello in una libreria di classi del progetto che fa riferimento il framework completo. Aggiungere un riferimento alla libreria di classi dal progetto ASP.NET Core. Vedere l'esempio [soluzione di Visual Studio con progetti EF6 e ASP.NET Core](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).

Impossibile inserire un contesto EF6 in un progetto ASP.NET Core perché i progetti .NET Core non supportano tutte le funzionalità, ad esempio i comandi di EF6 *Enable-Migrations* richiedono.

Indipendentemente dal tipo di progetto in cui è individuare il contesto EF6, strumenti da riga di comando di EF6 utilizzano un contesto EF6. Ad esempio, `Scaffold-DbContext` è disponibile solo in Entity Framework Core. Se è necessario il reverse engineering di un database in un modello EF6, vedere [Code First per un Database esistente](https://msdn.microsoft.com/jj200620).

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a>Framework completo di riferimento ed EF6 nel progetto ASP.NET Core

Il progetto ASP.NET Core deve fare riferimento a .NET framework ed EF6. Ad esempio, il *csproj* file del progetto ASP.NET Core avrà un aspetto simile al seguente (sono visualizzate solo le parti pertinenti del file).

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

Se si crea un nuovo progetto, utilizzare il **applicazione Web di ASP.NET Core (.NET Framework)** modello.

## <a name="handle-connection-strings"></a>Gestire le stringhe di connessione

Gli strumenti da riga di comando EF6 che verranno utilizzati nel progetto di libreria di classi EF6 richiedono un costruttore predefinito, quindi è possibile creare un'istanza del contesto. Ma è opportuno specificare la stringa di connessione da utilizzare nel progetto ASP.NET Core, nel qual caso il costruttore di contesto deve avere un parametro che consente di passare la stringa di connessione. Di seguito è riportato un esempio.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

Poiché il contesto EF6 non dispone di un costruttore senza parametri, deve fornire un'implementazione di progetto EF6 [IDbContextFactory](https://msdn.microsoft.com/library/hh506876). Gli strumenti da riga di comando EF6 verranno trovare e utilizzare tale implementazione, pertanto è possibile creare un'istanza del contesto. Di seguito è riportato un esempio.

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

In questo codice di esempio, il `IDbContextFactory` implementazione passa una stringa di connessione hardcoded. Questa è la stringa di connessione che utilizzeranno gli strumenti da riga di comando. È opportuno implementare una strategia per garantire che la libreria di classi Usa la stessa stringa di connessione utilizzata dall'applicazione chiamante. Ad esempio, è possibile ottenere il valore dalla variabile di ambiente in entrambi i progetti.

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a>Impostare l'inserimento di dipendenze del progetto ASP.NET Core

Del progetto principale *Startup.cs* file, impostare il contesto di EF6 per l'inserimento di dipendenze (DI) in `ConfigureServices`. Gli oggetti di contesto di Entity Framework devono essere limitati per una durata di ogni richiesta.

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

È quindi possibile ottenere un'istanza del contesto nei controller di usando DI. Il codice è simile a si scriverebbe per un contesto di EF Core:

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a>Applicazione di esempio

Per un'applicazione di esempio funzionante, vedere il [soluzione di Visual Studio di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) che accompagna questo articolo.

In questo esempio può essere creato da zero per i passaggi seguenti in Visual Studio:

* Creare una soluzione.

* **Aggiungi nuovo progetto > Web > applicazione Web ASP.NET Core (.NET Framework)**

* **Aggiungi nuovo progetto > Windows Desktop classico > (.NET Framework) libreria di classi**

* In **Package Manager Console** (PMC) per entrambi i progetti, eseguire il comando `Install-Package Entityframework`.

* Nel progetto libreria di classi, creare le classi di modello di dati e una classe del contesto e un'implementazione di `IDbContextFactory`.

* In PMC per il progetto libreria di classi, eseguire i comandi `Enable-Migrations` e `Add-Migration Initial`. Se il progetto ASP.NET Core è stata impostata come progetto di avvio, aggiungere `-StartupProjectName EF6` a questi comandi.

* Nel progetto di base, aggiungere un riferimento progetto a progetto libreria di classi.

* Nel progetto di base, in *Startup.cs*, registrare il contesto per DI.

* Nel progetto di base, in *appSettings. JSON*, aggiungere la stringa di connessione.

* Nel progetto di base, aggiungere un controller e visualizzazioni per verificare che è possibile leggere e scrivere i dati. Si noti che lo scaffolding di ASP.NET MVC di base non funzionerà con il contesto EF6 a cui fa riferimento della libreria di classi.

## <a name="summary"></a>Riepilogo

Questo articolo sono presenti linee guida di base per l'utilizzo di Entity Framework 6 in un'applicazione ASP.NET Core.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Entity Framework - configurazione basata su codice](https://msdn.microsoft.com/data/jj680699.aspx)
