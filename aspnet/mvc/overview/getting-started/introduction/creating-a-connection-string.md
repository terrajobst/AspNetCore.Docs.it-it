---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Creazione di una stringa di connessione e l'utilizzo di LocalDB di SQL Server | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 73a3149c8c789221baa2e3804ebf15ed5a509ddf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831574"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Creazione di una stringa di connessione e l'utilizzo di LocalDB di SQL Server
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Creazione di una stringa di connessione e l'utilizzo di LocalDB di SQL Server

Il `MovieDBContext` classe creata gestisce le attività di mapping e la connessione al database `Movie` oggetti per i record del database. Una domanda che ci si potrebbe chiedere, tuttavia, è illustrato come specificare i database a cui si connette. Non è necessario specificare il database da usare, per impostazione predefinita Entity Framework [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). In questa sezione verrà aggiunto in modo esplicito in una stringa di connessione il *Web. config* file dell'applicazione.

## <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) è una versione leggera del motore di SQL Server Express Database che viene avviato su richiesta ed eseguito in modalità utente. LocalDB viene eseguito in una modalità di esecuzione speciale di SQL Server Express che consente di lavorare con i database come *mdf* file. In genere, vengono archiviati i database LocalDB di *App\_dati* cartella del progetto web.

SQL Server Express non è consigliabile per l'uso in applicazioni web di produzione. Database locale in particolare non usare per la produzione con un'applicazione web perché non è progettato per funzionare con IIS. Tuttavia, un database LocalDB può essere facilmente migrato a SQL Server o SQL Azure.

In Visual Studio 2017, Local DB viene installato per impostazione predefinita con Visual Studio.

Per impostazione predefinita, Entity Framework Cerca una stringa di connessione lo stesso nome classe del contesto di oggetto (`MovieDBContext` per questo progetto). Per altre informazioni, vedere [stringhe di connessione di SQL Server per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Aprire la radice dell'applicazione *Web. config* file riportato di seguito. (Non il *Web. config* del file nei *viste* cartella.)

![](creating-a-connection-string/_static/image1.png)

Trovare il `<connectionStrings>` elemento:

![](creating-a-connection-string/_static/image2.png)

Aggiungere la seguente stringa di connessione per il `<connectionStrings>` elemento il *Web. config* file.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

L'esempio seguente mostra una parte i *Web. config* file con la nuova stringa di connessione aggiunta:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Due stringhe di connessione sono molto simili. La prima stringa di connessione è denominata `DefaultConnection` e viene usato per il database di appartenenza per controllare chi può accedere all'applicazione. È stata aggiunta la stringa di connessione specifica un database LocalDB denominato *Movie.mdf* che si trova nel *App\_dati* cartella. È non verrà usato il database delle appartenenze in questa esercitazione, per altre informazioni sull'appartenenza, autenticazione e sicurezza, vedere l'esercitazione [creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio App di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Il nome della stringa di connessione deve corrispondere al nome del [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Non è necessario aggiungere il `MovieDBContext` stringa di connessione. Se non si specifica una stringa di connessione, Entity Framework creerà un database LocalDB nella directory degli utenti con il nome completo del [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe (in questo caso `MvcMovie.Models.MovieDBContext`). È possibile assegnare un nome del database qualsiasi, purché disponga di *. File MDF* suffisso. Ad esempio, è stato possibile assegnare un nome del database *MyFilms.mdf*.

Successivamente, si creerà un nuovo `MoviesController` classe che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuove voci di filmato.

> [!div class="step-by-step"]
> [Precedente](adding-a-model.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)
