---
title: Utilizzo di SQL Server Local DB
author: rick-anderson
description: Utilizzo di SQL Server Local DB con un'app MVC semplice
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: a0aa6fdfa51650628021a4ba6d0533e7e0e39200
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-sql-server-localdb"></a>Utilizzo di SQL Server Local DB

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

L'oggetto `MvcMovieContext` gestisce l'attività di connessione al database e di mapping degli oggetti `Movie` ai record di database. Il contesto del database viene registrato nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection) nel metodo `ConfigureServices` nel file *Startup.cs*:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

Il sistema di [configurazione](xref:fundamentals/configuration/index) di ASP.NET Core legge la `ConnectionString`. Per lo sviluppo locale ottiene la stringa di connessione dal file *appSettings.JSON*:

[!code-json[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

Quando si distribuisce l'app in un server di test o di produzione, è possibile usare una variabile di ambiente o un altro approccio per impostare la stringa di connessione su un SQL Server reale. Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).

## <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

Local DB è una versione leggera del motore di database di SQL Server Express progettata appositamente per lo sviluppo di programmi. Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa. Per impostazione predefinita, il database Local DB crea i file "\*.mdf" nella directory *C:/Users/\<user\>*.

* Dal menu **Visualizzazione** aprire **Esplora oggetti di SQL Server** (SSOX).

  ![Menu View](working-with-sql/_static/ssox.png)

* Fare clic con il pulsante destro del mouse sulla tabella `Movie`**> Visualizza finestra di progettazione**

  ![Menu di scelta rapida aperto per la tabella Movie](working-with-sql/_static/design.png)

  ![Tabella Movie aperta nella finestra di progettazione](working-with-sql/_static/dv.png)

Si noti l'icona a forma di chiave accanto a `ID`. Per impostazione predefinita, Entity Framework userà una proprietà denominata `ID` come chiave primaria.

* Fare clic con il pulsante destro del mouse sulla tabella `Movie`**> Visualizza dati**

  ![Menu di scelta rapida aperto per la tabella Movie](working-with-sql/_static/ssox2.png)

  ![Tabella Movie aperta con i dati della tabella](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a>Specificare il valore di inizializzazione del database

Creare una nuova classe denominata `SeedData` nella cartella *Models*. Sostituire il codice generato con il seguente:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Se sono presenti eventuali film nel database, l'inizializzatore del valore di inizializzazione viene restituito e non vengono aggiunti film.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Aggiungere l'inizializzatore del valore di inizializzazione

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Aggiungere l'inizializzatore del valore di inizializzazione al metodo `Main` nel file *Program.cs*:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Aggiungere l'inizializzatore del valore di inizializzazione alla fine del metodo `Configure` nel file *Startup.cs*.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

Eseguire il test dell'app

* Eliminare tutti i record nel database. È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da SSOX.
* Forzare l'inizializzazione dell'app (chiamare i metodi nella classe `Startup`) in modo che venga eseguito il metodo di inizializzazione. Per forzare l'inizializzazione, IIS Express deve essere arrestato e riavviato. È possibile eseguire questa operazione adottando uno degli approcci seguenti:

  * Fare clic con il pulsante destro del mouse sull'icona dell'area di notifica di IIS Express e toccare **Esci** o **Arresta sito**

    ![Icona dell'area di notifica di IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menu di scelta rapida](working-with-sql/_static/stopIIS.png)

   * Se Visual Studio è in esecuzione in modalità non di debug, premere F5 per attivare la modalità di debug
   * Se Visual Studio è in esecuzione in modalità di debug, arrestare il debugger e premere F5
   
L'app mostra i dati inizializzati.

![App per i film MVC aperta in Microsoft Edge con i dati sui film](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
[Precedente](adding-model.md)
[Successivo](controller-methods-views.md)  
