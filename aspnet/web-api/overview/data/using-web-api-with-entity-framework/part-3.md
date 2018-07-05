---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Usare migrazioni Code First per effettuare il seeding del Database | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: bce662110fa63a9aee8e23e7b9fcc9ba9a8d2857
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805457"
---
<a name="use-code-first-migrations-to-seed-the-database"></a>Usare migrazioni Code First per effettuare il seeding del Database
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione si userà [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) in Entity Framework per effettuare il seeding del database con dati di test.

Dal **degli strumenti** dal menu **Library Package Manager**, quindi selezionare **Package Manager Console**. Nella finestra della Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](part-3/samples/sample1.cmd)]

Questo comando aggiunge una cartella denominata migrazioni al progetto, oltre a un file di codice denominato Configuration.cs nella cartella migrazioni.

![](part-3/_static/image1.png)

Aprire il file Configuration.cs. Aggiungere il codice seguente **usando** istruzione.

[!code-csharp[Main](part-3/samples/sample2.cs)]

Quindi aggiungere il codice seguente per il **Configuration.Seed** metodo:

[!code-csharp[Main](part-3/samples/sample3.cs)]

Nella finestra della Console di gestione pacchetti, digitare i comandi seguenti:

[!code-console[Main](part-3/samples/sample4.cmd)]

Il primo comando genera codice che crea il database e il secondo comando esegue tale codice. Il database viene creato in locale, usando [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Esplorare l'API (facoltativo)

Premere F5 per eseguire l'applicazione in modalità di debug. Visual Studio avvia IIS Express ed esegue l'app web. Quindi, Visual Studio avvia un browser e verrà visualizzata la home page dell'app.

Quando Visual Studio viene eseguito un progetto web, assegna un numero di porta. Nell'immagine seguente, il numero di porta è 50524. Quando si esegue l'applicazione, si noterà un numero di porta diverso.

![](part-3/_static/image3.png)

Home page viene implementata tramite ASP.NET MVC. Nella parte superiore della pagina, è presente un collegamento con la dicitura "API". Questo collegamento consente di accedere a una pagina della Guida generata automaticamente per l'API web. (Per altre modalità di generazione di questa pagina della Guida e come è possibile aggiungere il proprio documentazione alla pagina, vedere [creazione di pagine della Guida per l'API Web ASP.NET](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) È possibile fare clic su Guida di collegamenti della pagina per visualizzare informazioni dettagliate sulle API, incluso il formato della richiesta e risposta.

![](part-3/_static/image4.png)

L'API consente le operazioni CRUD sul database. Di seguito sono riepilogate le API.

| Autori |  |
| --- | -- |
| OTTENERE/autori di api | Ottenere tutti gli autori. |
| Api/autori di GET / {id} | Ottenere un autore per ID. |
| Autori di POST/api / | Creare un nuovo autore. |
| PUT/API/autori / {id} | Aggiornare un autore esistente. |
| ELIMINARE/API/autori / {id} | Eliminare un autore. |

| Libri |  |
| --- | -- |
| OTTENERE /api/books | Ottenere tutti i libri. |
| GET /api/books/{id} | Ottenere un libro di ID. |
| POST/api/documentazione | Creare un nuovo libro. |
| PUT/API/documentazione / {id} | Aggiornare un libro esistente. |
| ELIMINARE/API/documentazione / {id} | Eliminare un libro. |

## <a name="view-the-database-optional"></a>Visualizzare il Database (facoltativo)

Quando è stato eseguito il comando Update-Database, Entity Framework è stato creato il database e chiamato il `Seed` (metodo). Quando si esegue l'applicazione in locale, Usa EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). È possibile visualizzare il database in Visual Studio. Dal **View** dal menu **Esplora oggetti di SQL Server**.

![](part-3/_static/image5.png)

Nel **Connetti al Server** finestra di dialogo, nella **nome Server** casella di modifica, digitare "(localdb) \v11.0". Lasciare il **autenticazione** opzione come "Autenticazione di Windows". Fare clic su **Connetti**.

![](part-3/_static/image6.png)

Visual Studio si connette al database locale e visualizza i database esistenti nella finestra di Esplora oggetti di SQL Server. È possibile espandere i nodi per visualizzare le tabelle create di Entity Framework.

![](part-3/_static/image7.png)

Per visualizzare i dati, fare doppio clic su una tabella e selezionare **dati della visualizzazione**.

![](part-3/_static/image8.png)

Lo screenshot seguente mostra i risultati per la tabella di libri. Si noti che EF popolato il database con i dati di seeding e la tabella contiene la chiave esterna alla tabella degli autori.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Precedente](part-2.md)
> [Successivo](part-4.md)
