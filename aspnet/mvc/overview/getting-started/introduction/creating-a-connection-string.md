---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Creazione di una stringa di connessione e l'utilizzo di SQL Server LocalDB | Documenti Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 25d1c1c9954baaca9ef91eff3dd3c853930a5893
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Creazione di una stringa di connessione e l'utilizzo di SQL Server LocalDB
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Creazione di una stringa di connessione e l'utilizzo di SQL Server LocalDB

Il `MovieDBContext` classe creata gestisce le attività di connessione al database e di mapping `Movie` oggetti per i record del database. Una domanda, che si potrebbe chiedere, tuttavia, viene illustrato come specificare il database si connetterà. Non è necessario specificare il database da utilizzare, per impostazione predefinita Entity Framework [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). In questa sezione verrà aggiunto in modo esplicito in una stringa di connessione di *Web. config* file dell'applicazione.

## <a name="sql-server-express-localdb"></a>LocalDB di SQL Server Express

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) è una versione leggera di SQL Server Express motore di Database che viene avviato su richiesta e viene eseguito in modalità utente. LocalDB viene eseguito in modalità speciali di esecuzione di SQL Server Express che consente di lavorare con i database come *con estensione mdf* file. In genere, vengono archiviati i database LocalDB di *App\_dati* cartella del progetto web.

SQL Server Express non è consigliabile per le applicazioni web di produzione. LocalDB in particolare non utilizzare per la produzione con un'applicazione web poiché non è stato progettato per funzionare con IIS. Tuttavia, un database LocalDB può essere facilmente la migrazione a SQL Server o SQL Azure.

In Visual Studio 2017, LocalDB è installato per impostazione predefinita con Visual Studio.

Per impostazione predefinita, Entity Framework Cerca una stringa di connessione lo stesso nome di classe del contesto di oggetto (`MovieDBContext` per questo progetto). Per ulteriori informazioni vedere [stringhe di connessione di SQL Server per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Aprire la radice dell'applicazione *Web. config* file riportato di seguito. (Non il *Web. config* file nel *viste* cartella.)

![](creating-a-connection-string/_static/image1.png)

Trovare il `<connectionStrings>` elemento:

![](creating-a-connection-string/_static/image2.png)

Aggiungere la seguente stringa di connessione per il `<connectionStrings>` elemento il *Web. config* file.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

Nell'esempio seguente viene mostrata una parte di *Web. config* file con la nuova stringa di connessione aggiunta:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Due stringhe di connessione sono molto simili. La prima stringa di connessione è denominata `DefaultConnection` e viene utilizzato per il database delle appartenenze per controllare chi può accedere all'applicazione. È stata aggiunta la stringa di connessione specifica un database LocalDB denominato *Movie.mdf* nella *App\_dati* cartella. È non utilizzare il database di appartenenza in questa esercitazione, per ulteriori informazioni sull'appartenenza, autenticazione e sicurezza, vedere l'esercitazione [creare un'applicazione MVC ASP.NET con autenticazione e il database di SQL Server e distribuire in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Il nome della stringa di connessione deve corrispondere al nome del [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Non è necessario aggiungere il `MovieDBContext` stringa di connessione. Se non si specifica una stringa di connessione, Entity Framework creerà un database LocalDB nella directory degli utenti con il nome completo del [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe (in questo caso `MvcMovie.Models.MovieDBContext`). È possibile assegnare il database desiderato, purché abbia il *. MDF* suffisso. Ad esempio, è possibile assegnare un nome del database *MyFilms.mdf*.

Successivamente, si creerà un nuovo `MoviesController` classe che è possibile utilizzare per visualizzare i dati dei film e consentire agli utenti di creare nuove voci di film.

>[!div class="step-by-step"]
[Precedente](adding-a-model.md)
[Successivo](accessing-your-models-data-from-a-controller.md)
