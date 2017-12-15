---
title: Aggiunta di un modello a un'app ASP.NET Core MVC
author: rick-anderson
description: Aggiungere un modello a una semplice app ASP.NET Core.
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: a29bab9cf0712936fa9c3f2b4bb3b275a46fe6f6
ms.sourcegitcommit: e641c5794525f983485621860926d8ab4e7360c8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

Nota: i modelli ASP.NET Core 2.0 contengono la cartella *Models*.

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **MvcMovie** > **Aggiungi** > **Nuova cartella**. Assegnare il nome *Models* alla cartella.

Fare clic con il pulsante destro del mouse sulla cartella *Models* > **Aggiungi** > **Classe**. Assegnare il nome **Movie** alla classe e aggiungere le proprietà seguenti:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

Il campo `ID` è richiesto dal database per la chiave primaria. 

Compilare il progetto per verificare che non ci siano errori. L'app **M**VC dispone ora di un **M**odello.

## <a name="scaffolding-a-controller"></a>Eseguire lo scaffolding di un controller

In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Controllers*> **Aggiungi > Controller**.

![vista del passaggio precedente](adding-model/_static/add_controller.png)

Nella finestra di dialogo **Aggiungi dipendenze MVC** selezionare **Dipendenze minime**, quindi **Aggiungi**.

![vista del passaggio precedente](adding-model/_static/add_depend.png)

Visual Studio aggiunge le dipendenze necessarie per eseguire lo scaffolding di un controller, ma il controller stesso non viene creato. La successiva chiamata di **> Aggiungi > Controller** crea il controller. 

In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Controllers*> **Aggiungi > Controller**.

![vista del passaggio precedente](adding-model/_static/add_controller.png)

Nella finestra di dialogo **Aggiungi scaffolding** toccare **Controller MVC con visualizzazioni, che usa Entity Framework**.

![Finestra di dialogo Aggiungi scaffolding](adding-model/_static/add_scaffold2.png)

Completare la finestra di dialogo **Aggiungi controller**:

* **Classe di modello:** *Movie (MvcMovie.Models)*
* **Classe del contesto dati:** selezionare l'icona **+** e aggiungere il valore predefinito **MvcMovie.Models.MvcMovieContext**

![Aggiungere il contesto dati](adding-model/_static/dc.png)

* **Viste:** mantenere il valore predefinito di ogni opzione selezionata
* **Nome del controller:** mantenere il valore predefinito *MoviesController*
* Toccare **Aggiungi**

![Finestra di dialogo Aggiungi controller](adding-model/_static/add_controller2.png)

Visual Studio crea:

* Una [classe del contesto di database](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*)
* Un controller di film (*Controllers/MoviesController.cs*)
* File di vista Razor per le pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice) (*Views/Movies/&ast;.cshtml*)

La creazione automatica del contesto di database e di viste e metodi di azione [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, delete) è nota come *scaffolding*. Sarà presto disponibile un'applicazione Web completamente funzionale che consente di gestire un database di film.

Se si esegue l'app e si fa clic sul collegamento **Mvc Movie**, verrà visualizzato un errore simile al seguente:

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

È necessario creare il database e si userà la funzionalità di [migrazioni](xref:data/ef-mvc/migrations) di Entity Framework Core a tale scopo. Le migrazioni consentono di creare un database che corrisponde al modello di dati e aggiornare lo schema del database quando cambia il modello di dati.

## <a name="add-ef-tooling-and-perform-initial-migration"></a>Aggiungere gli strumenti di Entity Framework ed eseguire la migrazione iniziale

In questa sezione viene usata la Console di Gestione pacchetti per:

* Aggiungere il pacchetto degli strumenti di Entity Framework Core. Questo pacchetto è necessario per aggiungere le migrazioni e aggiornare il database.
* Aggiungere una migrazione iniziale.
* Aggiornare il database con la migrazione iniziale.

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Menu della Console di Gestione pacchetti](adding-model/_static/pmc.png)

Nella Console di Gestione pacchetti immettere i comandi seguenti:

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Nota:** se viene visualizzato un errore con il comando `Install-Package`, aprire Gestione pacchetti NuGet e cercare il pacchetto `Microsoft.EntityFrameworkCore.Tools`. In questo modo è possibile installare il pacchetto o verificare se è già installato. In alternativa, in caso di problemi con la Console di Gestione pacchetti vedere l'[approccio con l'interfaccia della riga di comando](#cli).

Il comando `Add-Migration` crea un codice per creare lo schema del database iniziale. Lo schema è basato sul modello specificato in `DbContext` (nel file *Data/MvcMovieContext.cs*). L'argomento `Initial` viene usato per denominare le migrazioni. È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione. Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).

Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_Initial.cs*, che crea il database.

<a name="cli"></a> È possibile eseguire la procedura descritta in precedenza tramite l'interfaccia della riga di comando anziché la Console di Gestione pacchetti:

* Aggiungere gli [strumenti di Entity Framework Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) nel file *.csproj*.
* Dalla console (nella directory del progetto) eseguire i comandi seguenti:

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Menu di scelta rapida IntelliSense su un elemento modello contenente le proprietà disponibili per ID, prezzo, data di rilascio e titolo](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Risorse aggiuntive

* [Helper tag](xref:mvc/views/tag-helpers/intro)
* [Globalizzazione e localizzazione](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Precedente - Aggiunta di una vista](adding-view.md)
[Successivo - Utilizzo del linguaggio SQL](working-with-sql.md)  
