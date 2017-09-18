---
title: Aggiunta di un nuovo campo
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 16efbacf-fe7b-4b41-84b0-06a1574b95c2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 7d7e7055dd6dc0a2aefd8f4a0a170483b8504267
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="adding-a-new-field"></a>Aggiunta di un nuovo campo

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa sezione si userà Migrazioni Code First di [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) per aggiungere un nuovo campo al modello e migrare questa modifica nel database.

Quando si usa Code First di Entity Framework per creare automaticamente un database, viene aggiunta una tabella al database per rilevare se lo schema del database è sincronizzato con le classi del modello da cui è stato generato. Se questi elementi non sono sincronizzati, Entity Framework genera un'eccezione. In questo modo è più semplice individuare problemi di incoerenza nel database o nel codice.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Aprire il file *Models/Movie.cs* e aggiungere una proprietà `Rating`:

[!code-csharp[Principale](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

Compilare l'app (CTRL+MAIUSC+B).

Poiché è stato aggiunto un nuovo campo alla classe `Movie`, è necessario anche aggiornare l'elenco di elementi di associazione consentiti in modo da includere questa nuova proprietà. In *MoviesController.cs* aggiornare l'attributo `[Bind]` per i metodi di azione `Create` e `Edit` in modo da includere la proprietà `Rating`:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

È necessario anche aggiornare i modelli di vista per visualizzare, creare e modificare la nuova proprietà `Rating` nella vista del browser.

Modificare il file */Views/Movies/Index.cshtml* e aggiungere un campo `Rating`:

[!code-HTML[Principale](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Aggiornare */Views/Movies/Create.cshtml* con un campo `Rating`. È possibile copiare e incollare il "gruppo di moduli" precedente e aggiornare i campi usando intelliSense. IntelliSense funziona con gli [helper tag](xref:mvc/views/tag-helpers/intro). Nota: nella versione RTM di Visual Studio 2017 è necessario installare [Servizi di linguaggio Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) per intelliSense di Razor. Il problema verrà risolto nella versione successiva.

![Lo sviluppatore ha digitato la lettera R per il valore dell'attributo asp-for nel secondo elemento di etichetta della vista. Viene visualizzato un menu a comparsa Intellisense con i campi disponibili, tra cui Rating che viene evidenziato automaticamente nell'elenco. Quando lo sviluppatore fa clic sul campo o preme il tasto INVIO, il valore verrà impostato su Rating.](new-field/_static/cr.png)

L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo. Se si esegue l'app ora, verrà visualizzato il seguente errore `SqlException`:

`SqlException: Invalid column name 'Rating'.`

Questo errore viene visualizzato perché la classe del modello Movie aggiornata è diversa rispetto allo schema della tabella Movie nel database esistente. Nella tabella del database non è presente una colonna Rating.

Per correggere questo errore, esistono alcuni approcci:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Questo approccio è molto utile nelle prime fasi del ciclo di sviluppo in una fase attiva di sviluppo di un database di test e consente di migliorare rapidamente lo schema del modello e il database insieme. Lo svantaggio è che si perdono i dati esistenti nel database e non è quindi possibile usare questo approccio in un database di produzione. Un modo efficace per sviluppare un'applicazione consiste nell'inizializzare automaticamente un database con dati di test usando un inizializzatore.

2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.

3. Usare Migrazioni Code First per aggiornare lo schema del database.

Per questa esercitazione si userà Migrazioni Code First.

Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna. Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni `new Movie`.

[!code-csharp[Principale](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Compilare la soluzione.

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.

  ![Menu della Console di Gestione pacchetti](adding-model/_static/pmc.png)

Nella Console di Gestione pacchetti immettere i comandi seguenti:

```PMC
Add-Migration Rating
Update-Database
```

Il comando `Add-Migration` indica al framework di migrazione di esaminare il modello `Movie` corrente con lo schema di database `Movie` corrente e creare il codice necessario per migrare il database nel nuovo modello. Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione. È consigliabile usare un nome significativo per il file di migrazione.

Se si eliminano tutti i record nel database, il database verrà inizializzato e verrà incluso il campo `Rating`. È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da SSOX.

Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`. Aggiungere anche il campo `Rating` ai modelli di vista `Edit`, `Details` e `Delete`.

>[!div class="step-by-step"]
[Precedente](search.md)
[Successivo](validation.md)  
