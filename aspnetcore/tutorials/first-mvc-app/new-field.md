---
title: Aggiungere un nuovo campo a un'app ASP.NET Core MVC
author: rick-anderson
description: Informazioni sull'uso di Migrazioni Code First di Entity Framework per aggiungere un nuovo campo a un modello ed eseguire la migrazione di questa modifica in un database.
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: cf8bb67703b564a711105123117498c94ab44e68
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889716"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a>Aggiungere un nuovo campo a un'app ASP.NET Core MVC

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa sezione viene usato Migrazioni Code First di [Entity Framework](/ef/core/get-started/aspnetcore/new-db) per:

* Aggiungere un nuovo campo al modello.
* Eseguire la migrazione del nuovo campo al database.

Quando si usa EF Code First per creare automaticamente un database, Code First:

* Aggiunge una tabella al database per rilevare lo schema del database.
* Verifica che il database sia sincronizzato con le classi del modello dalle quali è stato generato. Se questi elementi non sono sincronizzati, Entity Framework genera un'eccezione. In questo modo è più semplice individuare problemi di incoerenza nel database o nel codice.

## <a name="add-a-rating-property-to-the-movie-model"></a>Aggiungere una proprietà Rating al modello Movie

Aggiungere una proprietà `Rating` a *Models/Movie.cs*:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

Compilare l'app (CTRL+MAIUSC+B).

Poiché è stato aggiunto un nuovo campo alla classe `Movie`, è necessario aggiornare l'elenco di elementi di associazione consentiti per includere questa nuova proprietà. In *MoviesController.cs* aggiornare l'attributo `[Bind]` per i metodi di azione `Create` e `Edit` in modo da includere la proprietà `Rating`:

```csharp
[Bind("Id,Title,ReleaseDate,Genre,Price,Rating")]
   ```

Aggiornare i modelli di vista per visualizzare, creare e modificare la nuova proprietà `Rating` nella vista del browser.

Modificare il file */Views/Movies/Index.cshtml* e aggiungere un campo `Rating`:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=16,38&range=24-64)]

Aggiornare */Views/Movies/Create.cshtml* con un campo `Rating`.

# <a name="visual-studio--visual-studio-for-mactabvisual-studiovisual-studio-mac"></a>[Visual Studio/Visual Studio per Mac](#tab/visual-studio+visual-studio-mac)

È possibile copiare e incollare il "gruppo di moduli" precedente e aggiornare i campi usando intelliSense. IntelliSense funziona con gli [helper tag](xref:mvc/views/tag-helpers/intro).

![Lo sviluppatore ha digitato la lettera R per il valore dell'attributo asp-for nel secondo elemento di etichetta della vista. Viene visualizzato un menu a comparsa Intellisense con i campi disponibili, tra cui Rating che viene evidenziato automaticamente nell'elenco. Quando lo sviluppatore fa clic sul campo o preme il tasto INVIO, il valore verrà impostato su Rating.](new-field/_static/cr.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- This tab intentionally left blank. -->

---

Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna. Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni `new Movie`.

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo. Se viene eseguita a questo punto, viene generato il seguente errore `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Questo errore si verifica perché la classe del modello Movie aggiornata è diversa dallo schema della tabella Movie nel database esistente. Nella tabella del database non è presente una colonna `Rating`.

Per correggere questo errore, esistono alcuni approcci:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Questo approccio è molto utile nelle prime fasi del ciclo di sviluppo in una fase attiva dello sviluppo di un database di test e consente di migliorare rapidamente lo schema del modello e il database insieme. Lo svantaggio è che si perdono i dati esistenti nel database e non è quindi possibile usare questo approccio in un database di produzione. Un modo efficace per sviluppare un'applicazione consiste nell'inizializzare automaticamente un database con dati di test usando un inizializzatore. Questo è un approccio ottimale per le fasi iniziali dello sviluppo e quando si usa SQLite.

2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.

3. Usare Migrazioni Code First per aggiornare lo schema del database.

Per questa esercitazione viene usato Migrazioni Code First.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.

  ![Menu della Console di Gestione pacchetti](adding-model/_static/pmc.png)

Nella Console di Gestione pacchetti immettere i comandi seguenti:

```powershell
Add-Migration Rating
Update-Database
```

Il comando `Add-Migration` indica al framework di migrazione di esaminare il modello `Movie` corrente con lo schema di database `Movie` corrente e creare il codice necessario per migrare il database nel nuovo modello.

Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione. È consigliabile usare un nome significativo per il file di migrazione.

Se vengono eliminati tutti i record del database, il database viene inizializzato dal metodo di inizializzazione e viene incluso il campo `Rating`.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio per Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

Eliminare il database e usare le migrazioni per ricreare il database. Per eliminare il database, eliminare il file di database (*MvcMovie.db*). Quindi eseguire il comando `ef database update`:

```console
dotnet ef database update
```

---
<!-- End of VS tabs -->

Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`. È consigliabile aggiungere il campo `Rating` ai modelli di vista `Edit`, `Details` e `Delete`.

> [!div class="step-by-step"]
> [Precedente](search.md)
> [Successivo](validation.md)
