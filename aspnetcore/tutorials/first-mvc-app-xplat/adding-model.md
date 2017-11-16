---
title: Aggiunta di un modello a un'app ASP.NET Core MVC.
author: rick-anderson
description: Aggiungere un modello a una app semplice di ASP.NET Core.
ms.author: riande
ms.date: 09/18/2017
ms.topic: get-started-article
ms.technology: aspnet
keywords: ASP.NET Core, WebAPI, Web API, REST, Mac, Linux, HTTP, Service, HTTP Service,VS Code
ms.prod: asp.net-core
manager: wpickett
ms.assetid: 8dc28498-eeee-4666-b903-b593059e9f39
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 70aa344ca4ceafacf53907c925fd595e47104d7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* Aggiungere una classe alla cartella *Modello* denominata *Movie.cs*.
* Aggiungere il codice seguente al file *Modelli/Movie.cs*:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

Il campo `ID` è richiesto dal database per la chiave primaria. 

Compilare l'applicazione per verificare di non ottenere errori e di aver aggiunto un **M**odello alla propria app **M**VC.

## <a name="prepare-the-project-for-scaffolding"></a>Preparare il progetto per lo scaffolding

- Aggiungere i pacchetti NuGet evidenziati seguenti al file *MvcMovie.csproj*:
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Salvare il file e selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".
- Creare un file *Models/MvcMovieContext.cs* e aggiungere la classe `MvcMovieContext` seguente:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Aprire il file *Startup.cs* file e aggiungere due using:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Aggiungere il contesto del database al file *Startup.cs*:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Indica a Entity Framework quali classi di modello sono incluse nel modello di dati. Si sta definendo un *set di entità* di oggetti filmato, che verranno rappresentati nel database come tabella di un filmato.

- Compilare il progetto per verificare che non siano presenti errori.

## <a name="scaffold-the-moviecontroller"></a>Eseguire lo scaffolding di MovieController

Aprire una finestra terminale nella cartella di progetto ed eseguire i comandi seguenti:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> Se si verifica un errore durante l'esecuzione del comando di scaffolding, vedere [l'argomento 444 nell'archivio relativo allo scaffolding](https://github.com/aspnet/scaffolding/issues/444) per una soluzione alternativa.

Il motore di scaffolding crea:

* Un controller di filmati (*Controllers/MoviesController.cs*)
* File di vista Razor per le pagine Create, Delete, Details, Edit e Index (*Views/Movies/\*.cshtml*)

La creazione automatica di metodi di azione e visualizzazioni [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, delete) è nota come *scaffolding*. Sarà presto disponibile un'applicazione Web completamente funzionale che consente di gestire un database di filmati.

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

È ora disponibile un database e alcune pagine per visualizzare, modificare, aggiornare ed eliminare dati. Nella prossima esercitazione verrà utilizzato il database.

### <a name="additional-resources"></a>Risorse aggiuntive

* [Helper tag](xref:mvc/views/tag-helpers/intro)
* [Globalizzazione e localizzazione](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Indietro - Aggiunta di una visualizzazione](adding-view.md)
[Avanti - Uso di SQLite](working-with-sql.md)
