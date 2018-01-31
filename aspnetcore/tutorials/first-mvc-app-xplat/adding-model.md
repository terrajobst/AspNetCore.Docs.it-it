---
title: Aggiunta di un modello a un'app ASP.NET Core MVC.
author: rick-anderson
description: Aggiungere un modello a una app semplice di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 81511b05a3cc11a58b93452d3c6e5305e7ee4357
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
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
