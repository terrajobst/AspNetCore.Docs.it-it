---
title: Aggiungere un modello a un'app ASP.NET Core MVC con Visual Studio per Mac
author: rick-anderson
description: Aggiungere un modello a una app semplice di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.devlang: csharp
ms.prod: .net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 6792dbc7c9ab063d85c0c4145481b8fd6b40da63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app-with-visual-studio-for-mac"></a>Aggiungere un modello a un'app ASP.NET Core MVC con Visual Studio per Mac

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model1.md)]

* Fare clic con il pulsante destro del mouse sulla cartella *Modelli* e scegliere **Aggiungi** > **Nuovo file**. 
* Nella finestra di dialogo **Nuovo file**:

  * Selezionare **Generale** nel riquadro a sinistra.
  * Selezionare **Classe vuota** nel riquadro centrale.
  * Denominare la classe **Filmato** e selezionare **Nuovo**.

Aggiungere le proprietà seguenti alla classe `Movie`:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

Il campo `ID` è richiesto dal database per la chiave primaria.

Compilare il progetto per verificare che non ci siano errori. L'app **M**VC dispone ora di un **M**odello.

## <a name="prepare-the-project-for-scaffolding"></a>Preparare il progetto per lo scaffolding

- Fare clic con il pulsante destro del mouse sul file di progetto, quindi selezionare **Strumenti > Modifica File**.

  ![vista del passaggio precedente](adding-model/_static/1.png)

- Aggiungere i pacchetti NuGet evidenziati seguenti al file *MvcMovie.csproj*:
             
  [!code-csharp[](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Salvare il file.

- Creare un file *Models/MvcMovieContext.cs* e aggiungere la classe `MvcMovieContext` seguente: [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Aprire il file *Startup.cs* e aggiungere due using: [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Aggiungere il contesto del database al file *Startup.cs*:

   [!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Indica a Entity Framework quali classi di modello sono incluse nel modello di dati. Si sta definendo un *set di entità* di oggetti filmato, che verranno rappresentati nel database come tabella di un filmato.

- Compilare il progetto per verificare che non siano presenti errori.

## <a name="scaffold-the-moviecontroller"></a>Eseguire lo scaffolding di MovieController

Aprire una finestra terminale nella cartella di progetto ed eseguire i comandi seguenti:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
Se viene visualizzato l'errore `No executable found matching command "dotnet-aspnet-codegenerator", verify`:

 * Si è nella directory del progetto. La directory del progetto ha i file *Program.cs*, *Startup.cs* e *.csproj*.
 * La versione dotnet è 1.1 o versione successiva. Eseguire `dotnet` per ottenere la versione.
 * È stato aggiunto l'elemento `<DotNetCliToolReference>` al [file MvcMovie.csproj](#prepare-the-project-for-scaffolding).
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

Il motore di scaffolding crea:

* Un controller di filmati (*Controllers/MoviesController.cs*)
* File di vista Razor per le pagine Create, Delete, Details, Edit e Index (*Views/Movies/\*.cshtml*)

La creazione automatica di metodi di azione e visualizzazioni [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, delete) è nota come *scaffolding*. Sarà presto disponibile un'applicazione Web completamente funzionale che consente di gestire un database di film.

### <a name="add-the-files-to-visual-studio"></a>Aggiungere i file a Visual Studio

* Aggiungere il file *MovieController.cs* al progetto di Visual Studio:

  * Fare clic con il pulsante destro del mouse sulla cartella *Controller* e selezionare **Aggiungi > Aggiungi file**.
  * Selezionare il file *MovieController.cs*.

* Aggiungere la cartella *Filmati* e le viste:

  * Fare clic con il pulsante destro del mouse sulla cartella *Viste* e selezionare **Aggiungi > Aggiungi cartella esistente**.
  * Passare alla cartella *Viste*, selezionare *Viste\Filmati*, quindi selezionare **Apri**.
  * Nella finestra di dialogo **Select files to add from Movies** (Seleziona file da aggiungere dai filmati), selezionare **Include All** (Includi tutto) e quindi **OK**.

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

È ora disponibile un database e alcune pagine per visualizzare, modificare, aggiornare ed eliminare dati. Nella prossima esercitazione verrà utilizzato il database.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Helper tag](xref:mvc/views/tag-helpers/intro)
* [Globalizzazione e localizzazione](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Precedente - Aggiunta di una vista](adding-view.md)
> [Successivo - Utilizzo del linguaggio SQL](working-with-sql.md)  
