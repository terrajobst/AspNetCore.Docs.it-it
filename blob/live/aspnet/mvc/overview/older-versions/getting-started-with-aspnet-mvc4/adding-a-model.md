---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Aggiunta di un modello | Documenti Microsoft
author: Rick-Anderson
description: "Nota: Una versione aggiornata di questa esercitazione è disponibile qui che utilizza ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice seguire e demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 1d066e4bab866a2195647f43aa886279fee941db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model"></a><span data-ttu-id="5eb88-104">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="5eb88-104">Adding a Model</span></span>
====================
<span data-ttu-id="5eb88-105">Da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="5eb88-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="5eb88-106">È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="5eb88-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="5eb88-107">È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="5eb88-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="5eb88-108">In questa sezione si aggiungeranno alcune classi per la gestione di filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="5eb88-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="5eb88-109">Queste classi sarà il &quot;modello&quot; parte dell'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5eb88-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="5eb88-110">Si utilizzerà una tecnologia di accesso ai dati di .NET Framework nota come il [Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572(VS.110).aspx) per definire e utilizzare queste classi di modello.</span><span class="sxs-lookup"><span data-stu-id="5eb88-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="5eb88-111">Entity Framework (noto anche come EF) supporta un paradigma di sviluppo denominato *Code First*.</span><span class="sxs-lookup"><span data-stu-id="5eb88-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="5eb88-112">Codice, innanzitutto, consente di creare gli oggetti del modello mediante la scrittura di classi semplici.</span><span class="sxs-lookup"><span data-stu-id="5eb88-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="5eb88-113">(Sono anche noti come classi POCO, da &quot;oggetti CLR normale precedente.&quot;) È il database creato al momento dalle classi, che consente a un flusso di lavoro di sviluppo molto pulito e rapido.</span><span class="sxs-lookup"><span data-stu-id="5eb88-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="5eb88-114">Aggiunta di classi modello</span><span class="sxs-lookup"><span data-stu-id="5eb88-114">Adding Model Classes</span></span>

<span data-ttu-id="5eb88-115">In **Esplora soluzioni**, fare clic destro la *modelli* cartella, selezionare **Aggiungi**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="5eb88-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="5eb88-116">Immettere il *classe* nome &quot;film&quot;.</span><span class="sxs-lookup"><span data-stu-id="5eb88-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="5eb88-117">Aggiungere le seguenti cinque proprietà per il `Movie` classe:</span><span class="sxs-lookup"><span data-stu-id="5eb88-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="5eb88-118">Si userà la `Movie` classe per rappresentare filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="5eb88-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="5eb88-119">Ogni istanza di un `Movie` corrisponde a una riga all'interno di una tabella di database e ogni proprietà dell'oggetto di `Movie` classe verrà eseguito il mapping a una colonna nella tabella.</span><span class="sxs-lookup"><span data-stu-id="5eb88-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="5eb88-120">Nello stesso file, aggiungere le seguenti `MovieDBContext` classe:</span><span class="sxs-lookup"><span data-stu-id="5eb88-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="5eb88-121">Il `MovieDBContext` classe rappresenta il contesto del database film Entity Framework, che gestisce il recupero, l'archiviazione e l'aggiornamento `Movie` classe istanze in un database.</span><span class="sxs-lookup"><span data-stu-id="5eb88-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="5eb88-122">Il `MovieDBContext` deriva il `DbContext` classe forniti da Entity Framework di base.</span><span class="sxs-lookup"><span data-stu-id="5eb88-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="5eb88-123">Per poter fare riferimento a `DbContext` e `DbSet`, è necessario aggiungere il seguente `using` istruzione all'inizio del file:</span><span class="sxs-lookup"><span data-stu-id="5eb88-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="5eb88-124">L'intero *Movie.cs* file è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="5eb88-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="5eb88-125">(Più utilizzando le istruzioni che non sono necessarie sono state rimosse.)</span><span class="sxs-lookup"><span data-stu-id="5eb88-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="5eb88-126">Creazione di una stringa di connessione e l'utilizzo di SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="5eb88-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="5eb88-127">Il `MovieDBContext` classe creata gestisce le attività di connessione al database e di mapping `Movie` oggetti per i record del database.</span><span class="sxs-lookup"><span data-stu-id="5eb88-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="5eb88-128">Una domanda, che si potrebbe chiedere, tuttavia, viene illustrato come specificare il database si connetterà.</span><span class="sxs-lookup"><span data-stu-id="5eb88-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="5eb88-129">Operazione dovrà essere effettuata mediante l'aggiunta di informazioni di connessione nel *Web. config* file dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5eb88-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="5eb88-130">Aprire la radice dell'applicazione *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="5eb88-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="5eb88-131">(Non il *Web. config* file nel *viste* cartella.) Aprire il *Web. config* file indicati in rosso.</span><span class="sxs-lookup"><span data-stu-id="5eb88-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="5eb88-132">Aggiungere la seguente stringa di connessione per il `<connectionStrings>` elemento il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="5eb88-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="5eb88-133">Nell'esempio seguente viene mostrata una parte di *Web. config* file con la nuova stringa di connessione aggiunta:</span><span class="sxs-lookup"><span data-stu-id="5eb88-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="5eb88-134">Questa piccola quantità di codice e XML è tutto ciò che occorre per scrivere per rappresentare e archiviare i dati dei film in un database.</span><span class="sxs-lookup"><span data-stu-id="5eb88-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="5eb88-135">Successivamente, si creerà un nuovo `MoviesController` classe che è possibile utilizzare per visualizzare i dati dei film e consentire agli utenti di creare nuove voci di film.</span><span class="sxs-lookup"><span data-stu-id="5eb88-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5eb88-136">[Precedente](adding-a-view.md)
[Successivo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="5eb88-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
