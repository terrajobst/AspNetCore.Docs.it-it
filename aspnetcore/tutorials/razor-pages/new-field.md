---
title: Aggiungere un nuovo campo a una pagina Razor in ASP.NET Core
author: rick-anderson
description: Illustra come aggiungere un nuovo campo a una pagina Razor con Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: d9bf8c7cea20bf38aacf432465d7b33514bcd64d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277293"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a>Aggiungere un nuovo campo a una pagina Razor in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa sezione viene usato Migrazioni Code First di [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) per aggiungere un nuovo campo al modello e migrare questa modifica nel database.

Quando si usa Code First di Entity Framework per creare automaticamente un database, Code First:

* Aggiunge una tabella al database per rilevare se lo schema del database è sincronizzato con le classi di modelli da cui è stato generato.
* Se le classi di modelli non sono sincronizzate con il database, Entity Framework genera un'eccezione. 

La verifica automatica del modello o schema sincronizzato rende più semplice individuare i problemi di codice o database incoerente.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Aprire il file *Models/Movie.cs* e aggiungere una proprietà `Rating`:
::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

Compilare l'app (CTRL+MAIUSC+B).

Modificare *Pages/Movies/Index.cshtml* e aggiungere un campo `Rating`:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

Aggiungere il campo `Rating` alle pagine Delete (Elimina) e Details (Dettagli).

Aggiornare *Create.cshtml* con un campo `Rating`. È possibile copiare e incollare l'elemento `<div>` precedente e aggiornare i campi usando intelliSense. IntelliSense funziona con gli [helper tag](xref:mvc/views/tag-helpers/intro).

![Lo sviluppatore ha digitato la lettera R per il valore dell'attributo asp-for nel secondo elemento di etichetta della vista. Viene visualizzato un menu a comparsa Intellisense con i campi disponibili, tra cui Rating che viene evidenziato automaticamente nell'elenco. Quando lo sviluppatore fa clic sul campo o preme il tasto INVIO, il valore verrà impostato su Rating.](new-field/_static/cr.png)

Il codice seguente mostra il file *Create.cshtml* con un campo `Rating`:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

Aggiungere il campo `Rating` alla pagina Edit (Modifica).

L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo. Se si esegue l'app ora, verrà visualizzato un errore `SqlException`:

```
SqlException: Invalid column name 'Rating'.
```

Questo errore viene visualizzato perché la classe del modello Movie aggiornata è diversa rispetto allo schema della tabella Movie nel database. Nella tabella del database non è presente una colonna `Rating`.

Per correggere questo errore, esistono alcuni approcci:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database usando il nuovo schema di classi del modello. Questo approccio è utile nelle prime fasi del ciclo di sviluppo e consente di migliorare rapidamente lo schema del modello e il database insieme. Lo svantaggio è che si perdono i dati esistenti nel database. Non usare questo approccio in un database di produzione. L'eliminazione del database per la modifica dello schema e l'uso di un inizializzatore per inizializzare automaticamente un database con i dati di test è spesso un modo produttivo per sviluppare un'app.

2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.

3. Usare Migrazioni Code First per aggiornare lo schema del database.

Per questa esercitazione usare Migrazioni Code First.

Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna. Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni blocco `new Movie`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"
Vedere il [file SeedData.cs completato](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
Vedere il [file SeedData.cs completato](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).
::: moniker-end

Compilare la soluzione.

<a name="pmc"></a> Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.
Nella Console di Gestione pacchetti immettere i comandi seguenti:

```powershell
Add-Migration Rating
Update-Database
```

Il comando `Add-Migration` indica al framework di:

* Confrontare il modello `Movie` con lo schema di database `Movie`.
* Creare codice per migrare lo schema del database nel nuovo modello.

Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione. È consigliabile usare un nome significativo per il file di migrazione.

<a name="ssox"></a> Se si eliminano tutti i record nel database, il database verrà inizializzato e verrà incluso il campo `Rating`. È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX). Per eliminare il database da SSOX:

* Selezionare il database in SSOX.
* Fare clic con il pulsante destro del mouse sul database e selezionare *Elimina*.
* Selezionare **Chiudi connessioni esistenti**.
* Scegliere **OK**.
* Nella [Console di Gestione pacchetti](xref:tutorials/razor-pages/new-field#pmc) aggiornare il database:

  ```powershell
  Update-Database
  ```

Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`. Se il database non viene inizializzato, arrestare IIS Express e quindi eseguire l'app.

> [!div class="step-by-step"]
> [Precedente: Aggiunta della funzionalità di ricerca](xref:tutorials/razor-pages/search)
> [Successivo: Aggiunta della funzionalità di convalida](xref:tutorials/razor-pages/validation)
