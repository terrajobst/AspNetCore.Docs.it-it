---
title: Schema di repository con ASP.NET Core
author: ardalis
description: Informazioni su come implementare lo schema progettuale del repository in un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342631"
---
# <a name="repository-pattern-with-aspnet-core"></a>Schema di repository con ASP.NET Core

Di [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com) e [Luke Latham](https://github.com/guardrex)

Lo *schema di repository* è uno schema progettuale che consente di isolare l'accesso ai dati dietro astrazioni dell'interfaccia. La connessione al database e la modifica degli oggetti di archiviazione dei dati vengono eseguite tramite i metodi forniti dall'implementazione dell'interfaccia. Di conseguenza, non è necessario chiamare codice per gestire le esigenze del database, ad esempio connessioni, comandi e lettori.

L'implementazione dello schema di repository con ASP.NET Core offre i vantaggi seguenti:

* L'organizzazione dell'app è meno complessa, senza interdipendenze dirette tra il livello aziendale e il livello di accesso ai dati.
* È più semplice riutilizzare il codice di accesso del database perché è gestito in modo centralizzato da uno o più repository.
* È possibile eseguire unit test del dominio aziendale in modo indipendente dal livello del database.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

L'[app di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) usa lo schema del repository per inizializzare e visualizzare un elenco di nomi di personaggi di film. L'app usa [Entity Framework Core](/ef/core/) e una classe `ApplicationDbContext` per la persistenza dei dati, ma l'infrastruttura di database non è evidente nella posizione di accesso ai dati. Per gli oggetti di accesso ai dati e di database è prevista l'astrazione dietro un [repository](https://martinfowler.com/eaaCatalog/repository.html).

## <a name="repository-interface"></a>Interfaccia del repository

Un'interfaccia del repository definisce le proprietà e i metodi per l'implementazione. Nell'app di esempio, l'interfaccia del repository per i dati sui personaggi dei film è `ICharacterRepository`. `ICharacterRepository` definisce i metodi `ListAll` e `Add` necessari per utilizzare le istanze di `Character` nell'app:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

L'oggetto `Character` viene definito come:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a>Tipo concreto del repository

L'interfaccia viene implementata da un tipo concreto. Nell'app di esempio, `CharacterRepository` gestisce il contesto del database e implementa i metodi `ListAll` e `Add`:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a>Registrare il servizio del repository

Il repository e il contesto del database vengono registrati nel contenitore dei servizi in `Startup.ConfigureServices`. Nell'app di esempio, `ApplicationDbContext` è configurato con la chiamata al metodo di estensione [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext). `ICharacterRepository` viene registrato come servizio con ambito:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a>Inserire un'istanza del repository

In una classe in cui è richiesto l'accesso al database, un'istanza del repository viene richiesta tramite il costruttore e assegnata a un campo privato per l'uso da parte dei metodi della classe. Nell'app di esempio, `ICharacterRepository` viene usato per:

* Popolare il database se non esiste alcun personaggio.
* Ottenere un elenco dei personaggi per la visualizzazione.

Si noti come il codice chiamante interagisca solo con l'implementazione dell'interfaccia, `CharacterRepository`. Il codice chiamante non usa `ApplicationDbContext` direttamente:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a>Interfaccia del repository generica

Questo argomento e la relativa app di esempio illustrano l'implementazione più semplice dello schema del repository, in cui viene creato un repository per ogni oggetto business. Se l'app comprende più di pochi oggetti, un'*interfaccia del repository generica* potrebbe ridurre la quantità di codice necessaria per implementare lo schema del repository. Per altre informazioni, vedere [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/) (DevIQ: Schema del repository: Interfaccia del repository generica).

## <a name="additional-resources"></a>Risorse aggiuntive

* [DevIQ: Repository Pattern](https://deviq.com/repository-pattern/) (DevIQ: Schema del repository)
* [Inserimento di dipendenze](xref:fundamentals/dependency-injection)
* [Inserimento di dipendenze in visualizzazioni](xref:mvc/views/dependency-injection)
* [Inserimento di dipendenze in controller](xref:mvc/controllers/dependency-injection)
* [Inserimento di dipendenze nei gestori di requisiti](xref:security/authorization/dependencyinjection)
* [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (Contenitori di inversione del controllo e schema di inserimento delle dipendenze)
