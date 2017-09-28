---
title: Utilizzo di SQL Server Local DB e ASP.NET Core
author: rick-anderson
description: Descrive l'utilizzo di SQL Server Local DB e ASP.NET Core.
keywords: ASP.NET Core, pagine Razor, Razor, MVC, SQL, Local DB
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 852bd2dff96c951f55a9b142d8e15b6ec5856921
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2017
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a>Utilizzo di SQL Server Local DB e ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette) 

L'oggetto `MovieContext` gestisce l'attività di connessione al database e di mapping degli oggetti `Movie` ai record di database. Il contesto del database viene registrato nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection) nel metodo `ConfigureServices` nel file *Startup.cs*:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

Il sistema di [configurazione](xref:fundamentals/configuration) di ASP.NET Core legge la `ConnectionString`. Per lo sviluppo locale ottiene la stringa di connessione dal file *appSettings.JSON*:

[!code-javascript[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

Quando si distribuisce l'app in un server di test o di produzione, è possibile usare una variabile di ambiente o un altro approccio per impostare la stringa di connessione su un SQL Server reale. Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration).

## <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

Local DB è una versione leggera del motore di database di SQL Server Express appositamente pensato per lo sviluppo di programmi. Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa. Per impostazione predefinita, il database Local DB crea i file "\*.mdf" nella directory *C:/Users/\<user\>*.

<a name="ssox"></a>
* Dal menu **Visualizzazione** aprire **Esplora oggetti di SQL Server** (SSOX).

  ![Menu View](sql/_static/ssox.png)

* Fare clic con il pulsante destro del mouse sulla tabella `Movie`**> Visualizza finestra di progettazione**

  ![Menu di scelta rapida aperto per la tabella Movie](sql/_static/design.png)

  ![Tabella Movie aperta nella finestra di progettazione](sql/_static/dv.png)

Si noti l'icona a forma di chiave accanto a `ID`. Per impostazione predefinita, Entity Framework userà una proprietà denominata `ID` come chiave primaria.

* Fare clic con il pulsante destro del mouse sulla tabella `Movie`**> Visualizza dati**

  ![Tabella Movie aperta con i dati della tabella](sql/_static/vd22.png)

## <a name="seed-the-database"></a>Specificare il valore di inizializzazione del database

Creare una nuova classe denominata `SeedData` nella cartella *Models*. Sostituire il codice generato con il seguente:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

Se sono presenti eventuali film nel database, l'inizializzatore del valore di inizializzazione viene restituito e non vengono aggiunti film.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Aggiungere l'inizializzatore del valore di inizializzazione

Aggiungere l'inizializzatore del valore di inizializzazione alla fine del metodo `Main` nel file *Program.cs*:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs?highlight=6,17-32)]

Eseguire il test dell'app

* Eliminare tutti i record nel database. È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [SSOX](xref:tutorials/razor-pages/new-field#ssox)
* Forzare l'inizializzazione dell'app (chiamare i metodi nella classe `Startup`) in modo che venga eseguito il metodo di inizializzazione. Per forzare l'inizializzazione, IIS Express deve essere arrestato e riavviato. È possibile eseguire questa operazione adottando uno degli approcci seguenti:

  * Fare clic con il pulsante destro del mouse sull'icona dell'area di notifica di IIS Express e toccare **Esci** o **Arresta sito**

    ![Icona dell'area di notifica di IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu di scelta rapida](sql/_static/stopIIS.png)

   * Se Visual Studio è in esecuzione in modalità non di debug, premere F5 per attivare la modalità di debug.
   * Se Visual Studio è in esecuzione in modalità di debug, arrestare il debugger e premere F5.
   
L'app mostra i dati inizializzati.

![App per i film aperta in Chrome con i dati sui film](sql/_static/m55.png)

L'esercitazione successiva consentirà di pulire la presentazione dei dati.

>[!div class="step-by-step"]
[Precedente: Pagine Razor di scaffolding](xref:tutorials/razor-pages/page)   
[Successivo: Aggiornamento delle pagine](xref:tutorials/razor-pages/da1)
