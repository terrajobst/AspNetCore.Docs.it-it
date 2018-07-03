---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Creazione di una stringa di connessione e l'utilizzo di LocalDB di SQL Server | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: d6e04de9b1d71a77b4b56b04417d6d4bc24cbd72
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361668"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="e3785-102">Creazione di una stringa di connessione e l'utilizzo di LocalDB di SQL Server</span><span class="sxs-lookup"><span data-stu-id="e3785-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="e3785-103">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="e3785-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="e3785-104">Creazione di una stringa di connessione e l'utilizzo di LocalDB di SQL Server</span><span class="sxs-lookup"><span data-stu-id="e3785-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="e3785-105">Il `MovieDBContext` classe creata gestisce le attività di mapping e la connessione al database `Movie` oggetti per i record del database.</span><span class="sxs-lookup"><span data-stu-id="e3785-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="e3785-106">Una domanda che ci si potrebbe chiedere, tuttavia, è illustrato come specificare i database a cui si connette.</span><span class="sxs-lookup"><span data-stu-id="e3785-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="e3785-107">Non è necessario specificare il database da usare, per impostazione predefinita Entity Framework [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="e3785-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="e3785-108">In questa sezione verrà aggiunto in modo esplicito in una stringa di connessione il *Web. config* file dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e3785-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="e3785-109">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="e3785-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="e3785-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) è una versione leggera del motore di SQL Server Express Database che viene avviato su richiesta ed eseguito in modalità utente.</span><span class="sxs-lookup"><span data-stu-id="e3785-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="e3785-111">LocalDB viene eseguito in una modalità di esecuzione speciale di SQL Server Express che consente di lavorare con i database come *mdf* file.</span><span class="sxs-lookup"><span data-stu-id="e3785-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="e3785-112">In genere, vengono archiviati i database LocalDB di *App\_dati* cartella del progetto web.</span><span class="sxs-lookup"><span data-stu-id="e3785-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="e3785-113">SQL Server Express non è consigliabile per l'uso in applicazioni web di produzione.</span><span class="sxs-lookup"><span data-stu-id="e3785-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="e3785-114">Database locale in particolare non usare per la produzione con un'applicazione web perché non è progettato per funzionare con IIS.</span><span class="sxs-lookup"><span data-stu-id="e3785-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="e3785-115">Tuttavia, un database LocalDB può essere facilmente migrato a SQL Server o SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="e3785-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="e3785-116">In Visual Studio 2017, Local DB viene installato per impostazione predefinita con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e3785-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="e3785-117">Per impostazione predefinita, Entity Framework Cerca una stringa di connessione lo stesso nome classe del contesto di oggetto (`MovieDBContext` per questo progetto).</span><span class="sxs-lookup"><span data-stu-id="e3785-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="e3785-118">Per altre informazioni, vedere [stringhe di connessione di SQL Server per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="e3785-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="e3785-119">Aprire la radice dell'applicazione *Web. config* file riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e3785-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="e3785-120">(Non il *Web. config* del file nei *viste* cartella.)</span><span class="sxs-lookup"><span data-stu-id="e3785-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="e3785-121">Trovare il `<connectionStrings>` elemento:</span><span class="sxs-lookup"><span data-stu-id="e3785-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="e3785-122">Aggiungere la seguente stringa di connessione per il `<connectionStrings>` elemento il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="e3785-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="e3785-123">L'esempio seguente mostra una parte i *Web. config* file con la nuova stringa di connessione aggiunta:</span><span class="sxs-lookup"><span data-stu-id="e3785-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="e3785-124">Due stringhe di connessione sono molto simili.</span><span class="sxs-lookup"><span data-stu-id="e3785-124">The two connection strings are very similar.</span></span> <span data-ttu-id="e3785-125">La prima stringa di connessione è denominata `DefaultConnection` e viene usato per il database di appartenenza per controllare chi può accedere all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e3785-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="e3785-126">È stata aggiunta la stringa di connessione specifica un database LocalDB denominato *Movie.mdf* che si trova nel *App\_dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="e3785-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="e3785-127">È non verrà usato il database delle appartenenze in questa esercitazione, per altre informazioni sull'appartenenza, autenticazione e sicurezza, vedere l'esercitazione [creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio App di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="e3785-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="e3785-128">Il nome della stringa di connessione deve corrispondere al nome del [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="e3785-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="e3785-129">Non è necessario aggiungere il `MovieDBContext` stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="e3785-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="e3785-130">Se non si specifica una stringa di connessione, Entity Framework creerà un database LocalDB nella directory degli utenti con il nome completo del [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe (in questo caso `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="e3785-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="e3785-131">È possibile assegnare un nome del database qualsiasi, purché disponga di *. File MDF* suffisso.</span><span class="sxs-lookup"><span data-stu-id="e3785-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="e3785-132">Ad esempio, è stato possibile assegnare un nome del database *MyFilms.mdf*.</span><span class="sxs-lookup"><span data-stu-id="e3785-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="e3785-133">Successivamente, si creerà un nuovo `MoviesController` classe che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuove voci di filmato.</span><span class="sxs-lookup"><span data-stu-id="e3785-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e3785-134">[Precedente](adding-a-model.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="e3785-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
