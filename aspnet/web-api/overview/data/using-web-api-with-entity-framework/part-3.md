---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Utilizzare migrazioni Code First per il seeding del Database | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 1ca627397f0f100d13388f9afc27ff481886e098
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Utilizzare migrazioni Code First per il seeding del Database
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione, si utilizzerà [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) in Entity Framework per il seeding del database con dati di test.

Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti, immettere il comando seguente:

[!code-console[Main](part-3/samples/sample1.cmd)]

Questo comando aggiunge una cartella denominata migrazioni al progetto, oltre a un file di codice denominato Configuration.cs nella cartella migrazioni.

![](part-3/_static/image1.png)

Aprire il file Configuration.cs. Aggiungere il seguente **utilizzando** istruzione.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Quindi aggiungere il codice seguente per il **Configuration.Seed** metodo:

[!code-csharp[Main](part-3/samples/sample3.cs)]

Nella finestra della Console di gestione pacchetti, digitare i comandi seguenti:

[!code-console[Main](part-3/samples/sample4.cmd)]

Il primo comando genera codice che crea il database e il secondo comando esegue il codice. Il database viene creato localmente, utilizzando [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Esplorare l'API (facoltativo)

Premere F5 per eseguire l'applicazione in modalità di debug. Visual Studio avvia IIS Express ed esegue l'app web. Visual Studio, quindi, viene avviato un browser e verrà visualizzata la home page dell'app.

Quando Visual Studio viene eseguito un progetto web, viene assegnato un numero di porta. Nell'immagine seguente, il numero di porta è 50524. Quando si esegue l'applicazione, verrà visualizzato un numero di porta diverso.

![](part-3/_static/image3.png)

Home page viene implementata utilizzando ASP.NET MVC. Nella parte superiore della pagina, è presente un collegamento che afferma "API". Questo collegamento consente di visualizzare una pagina della Guida generato automaticamente per l'API web. (Per informazioni su come aggiungere la propria documentazione per la pagina e la modalità di generazione di questa pagina della Guida, vedere [la creazione di pagine della Guida per ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) È possibile fare clic su Guida di collegamenti della pagina per visualizzare i dettagli sull'API, incluso il formato di richiesta e risposta.

![](part-3/_static/image4.png)

L'API consente le operazioni CRUD nel database. Di seguito viene riepilogata l'API.

| Autori |  |
| --- | -- |
| OTTENERE l'api o gli autori | Ottenere tutti gli autori. |
| Api GET/autori / {id} | Ottenere un autore dall'ID. |
| Autori di POST/api / | Creare un nuovo autore. |
| Inserire /api autori / {id} | Aggiornare un autore esistente. |
| ELIMINARE /api autori / {id} | Eliminare un autore. |

| Documentazione |  |
| --- | -- |
| OTTENERE /api/books | Ottenere tutti i libri. |
| GET /api/books/{id} | Ottenere un libro di ID. |
| REGISTRARE/api/documentazione | Creare una nuova rubrica. |
| Inserire /api documentazione / {id} | Aggiornare un libro esistente. |
| DELETE /api/books/{id} | Eliminare un libro. |

## <a name="view-the-database-optional"></a>Visualizzare il Database (facoltativo)

Quando è stato eseguito il comando Update-Database, EF creato il database e chiamare il `Seed` metodo. Quando si esegue l'applicazione localmente, Entity Framework Usa [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). È possibile visualizzare il database in Visual Studio. Dal **vista** dal menu **Esplora oggetti di SQL Server**.

![](part-3/_static/image5.png)

Nel **Connetti al Server** finestra di dialogo, nel **nome Server** casella di modifica, digitare "(localdb) \v11.0". Lasciare il **autenticazione** opzione come "Autenticazione di Windows". Fare clic su **Connetti**.

![](part-3/_static/image6.png)

Visual Studio si connette al database locale e Mostra i database esistenti nella finestra Esplora oggetti di SQL Server. È possibile espandere i nodi per visualizzare le tabelle create Entity Framework.

![](part-3/_static/image7.png)

Per visualizzare i dati, fare doppio clic su una tabella e selezionare **Visualizza dati**.

![](part-3/_static/image8.png)

Nella schermata seguente mostra i risultati per la tabella di documentazione. Si noti che EF popolato il database con i dati del valore di inizializzazione e la tabella contiene la chiave esterna alla tabella degli autori.

![](part-3/_static/image9.png)

>[!div class="step-by-step"]
[Precedente](part-2.md)
[Successivo](part-4.md)
