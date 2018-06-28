---
title: Usare SQL Server Local DB e ASP.NET Core
author: rick-anderson
description: Descrive l'utilizzo di SQL Server Local DB e ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 92a5965e7a535ca729c0bec13911b6bf051a7b19
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582869"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a>Usare SQL Server Local DB e ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette) 

L'oggetto `MovieContext` gestisce l'attività di connessione al database e di mapping degli oggetti `Movie` ai record di database. Il contesto del database viene registrato nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection) nel metodo `ConfigureServices` nel file *Startup.cs*:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Per altre informazioni sui metodi usati in `ConfigureServices`, vedere:

* [Supporto per il Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea in ASP.NET Core](xref:security/gdpr) per `CookiePolicyOptions`.
* [SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

Il sistema di [configurazione](xref:fundamentals/configuration/index) di ASP.NET Core legge la `ConnectionString`. Per lo sviluppo locale, ottiene la stringa di connessione dal file *appsettings.json*. Il valore del nome per il database (`Database={Database name}`) sarà diverso per il codice generato. Il valore del nome è arbitrario.

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

Quando si distribuisce l'app in un server di test o di produzione, è possibile usare una variabile di ambiente o un altro approccio per impostare la stringa di connessione su un SQL Server reale. Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).

## <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

Local DB è una versione leggera del motore di database di SQL Server Express progettata appositamente per lo sviluppo di programmi. Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa. Per impostazione predefinita, il database Local DB crea i file "\*.mdf" nella directory *C:/Users/\<user\>*.

<a name="ssox"></a>
* Dal menu **Visualizzazione** aprire **Esplora oggetti di SQL Server** (SSOX).

  ![Menu View](sql/_static/ssox.png)

* Fare clic con il pulsante destro del mouse sulla tabella `Movie` e selezionare**Progettazione viste**:

  ![Menu di scelta rapida aperto per la tabella Movie](sql/_static/design.png)

  ![Tabella Movie aperta nella finestra di progettazione](sql/_static/dv.png)

Si noti l'icona a forma di chiave accanto a `ID`. Per impostazione predefinita, Entity Framework crea una proprietà denominata `ID` per la chiave primaria.

* Fare clic con il pulsante destro del mouse sulla tabella `Movie` e selezionare**Visualizza dati**:

  ![Tabella Movie aperta con i dati della tabella](sql/_static/vd22.png)

## <a name="seed-the-database"></a>Specificare il valore di inizializzazione del database

Creare una nuova classe denominata `SeedData` nella cartella *Models*. Sostituire il codice generato con il seguente:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

Se sono presenti eventuali film nel database, l'inizializzatore del valore di inizializzazione viene restituito e non vengono aggiunti film.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Aggiungere l'inizializzatore del valore di inizializzazione

In *Program.cs* modificare il metodo `Main` per eseguire le operazioni seguenti:

* Ottenere un'istanza del contesto di database dal contenitore di inserimento delle dipendenze.
* Chiamare il metodo di inizializzazione, passandolo al contesto.
* Eliminare il contesto dopo che il metodo di inizializzazione è stato completato.

Il codice seguente illustra il file *Program.cs* aggiornato.

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

Un'app di produzione non chiamerà `Database.Migrate`. Viene aggiunto al codice precedente per evitare l'eccezione seguente quando `Update-Database` non è stato eseguito:

SqlException: Impossibile aprire il database "RazorPagesMovieContext-21" richiesto dall'account di accesso. Accesso non riuscito.
Accesso non riuscito per l'utente "nome-utente".

### <a name="test-the-app"></a>Eseguire il test dell'app

* Eliminare tutti i record nel database. È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [SSOX](xref:tutorials/razor-pages/new-field#ssox)
* Forzare l'inizializzazione dell'app (chiamare i metodi nella classe `Startup`) in modo che venga eseguito il metodo di inizializzazione. Per forzare l'inizializzazione, IIS Express deve essere arrestato e riavviato. È possibile eseguire questa operazione adottando uno degli approcci seguenti:

  * Fare clic con il pulsante destro del mouse sull'icona dell'area di notifica di IIS Express e toccare **Esci** o **Arresta sito**:

    ![Icona dell'area di notifica di IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu di scelta rapida](sql/_static/stopIIS.png)

    * Se Visual Studio è in esecuzione in modalità non di debug, premere F5 per attivare la modalità di debug.
    * Se Visual Studio è in esecuzione in modalità di debug, arrestare il debugger e premere F5.
   
L'app visualizza i dati sottoposti a seed:

![App per i film aperta in Chrome con i dati sui film](sql/_static/m55.png)

L'esercitazione successiva consentirà di pulire la presentazione dei dati.

> [!div class="step-by-step"]
> [Articolo precedente: Pagine Razor di scaffolding](xref:tutorials/razor-pages/page)
> [Articolo successivo: Aggiornamento delle pagine](xref:tutorials/razor-pages/da1)
