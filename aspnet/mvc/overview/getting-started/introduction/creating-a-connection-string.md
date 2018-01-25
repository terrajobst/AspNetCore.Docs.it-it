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
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="d5f9b-102">Creazione di una stringa di connessione e l'utilizzo di SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="d5f9b-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="d5f9b-103">Da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d5f9b-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="d5f9b-104">Creazione di una stringa di connessione e l'utilizzo di SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="d5f9b-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="d5f9b-105">Il `MovieDBContext` classe creata gestisce le attività di connessione al database e di mapping `Movie` oggetti per i record del database.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="d5f9b-106">Una domanda, che si potrebbe chiedere, tuttavia, viene illustrato come specificare il database si connetterà.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="d5f9b-107">Non è necessario specificare il database da utilizzare, per impostazione predefinita Entity Framework [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="d5f9b-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="d5f9b-108">In questa sezione verrà aggiunto in modo esplicito in una stringa di connessione di *Web. config* file dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="d5f9b-109">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="d5f9b-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="d5f9b-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) è una versione leggera di SQL Server Express motore di Database che viene avviato su richiesta e viene eseguito in modalità utente.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="d5f9b-111">LocalDB viene eseguito in modalità speciali di esecuzione di SQL Server Express che consente di lavorare con i database come *con estensione mdf* file.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="d5f9b-112">In genere, vengono archiviati i database LocalDB di *App\_dati* cartella del progetto web.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="d5f9b-113">SQL Server Express non è consigliabile per le applicazioni web di produzione.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="d5f9b-114">LocalDB in particolare non utilizzare per la produzione con un'applicazione web poiché non è stato progettato per funzionare con IIS.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="d5f9b-115">Tuttavia, un database LocalDB può essere facilmente la migrazione a SQL Server o SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="d5f9b-116">In Visual Studio 2017, LocalDB è installato per impostazione predefinita con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="d5f9b-117">Per impostazione predefinita, Entity Framework Cerca una stringa di connessione lo stesso nome di classe del contesto di oggetto (`MovieDBContext` per questo progetto).</span><span class="sxs-lookup"><span data-stu-id="d5f9b-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="d5f9b-118">Per ulteriori informazioni vedere [stringhe di connessione di SQL Server per le applicazioni Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5f9b-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="d5f9b-119">Aprire la radice dell'applicazione *Web. config* file riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="d5f9b-120">(Non il *Web. config* file nel *viste* cartella.)</span><span class="sxs-lookup"><span data-stu-id="d5f9b-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="d5f9b-121">Trovare il `<connectionStrings>` elemento:</span><span class="sxs-lookup"><span data-stu-id="d5f9b-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="d5f9b-122">Aggiungere la seguente stringa di connessione per il `<connectionStrings>` elemento il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="d5f9b-123">Nell'esempio seguente viene mostrata una parte di *Web. config* file con la nuova stringa di connessione aggiunta:</span><span class="sxs-lookup"><span data-stu-id="d5f9b-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="d5f9b-124">Due stringhe di connessione sono molto simili.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-124">The two connection strings are very similar.</span></span> <span data-ttu-id="d5f9b-125">La prima stringa di connessione è denominata `DefaultConnection` e viene utilizzato per il database delle appartenenze per controllare chi può accedere all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="d5f9b-126">È stata aggiunta la stringa di connessione specifica un database LocalDB denominato *Movie.mdf* nella *App\_dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="d5f9b-127">È non utilizzare il database di appartenenza in questa esercitazione, per ulteriori informazioni sull'appartenenza, autenticazione e sicurezza, vedere l'esercitazione [creare un'applicazione MVC ASP.NET con autenticazione e il database di SQL Server e distribuire in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="d5f9b-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="d5f9b-128">Il nome della stringa di connessione deve corrispondere al nome del [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="d5f9b-129">Non è necessario aggiungere il `MovieDBContext` stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="d5f9b-130">Se non si specifica una stringa di connessione, Entity Framework creerà un database LocalDB nella directory degli utenti con il nome completo del [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe (in questo caso `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="d5f9b-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="d5f9b-131">È possibile assegnare il database desiderato, purché abbia il *. MDF* suffisso.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="d5f9b-132">Ad esempio, è possibile assegnare un nome del database *MyFilms.mdf*.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="d5f9b-133">Successivamente, si creerà un nuovo `MoviesController` classe che è possibile utilizzare per visualizzare i dati dei film e consentire agli utenti di creare nuove voci di film.</span><span class="sxs-lookup"><span data-stu-id="d5f9b-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d5f9b-134">[Precedente](adding-a-model.md)
[Successivo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="d5f9b-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
