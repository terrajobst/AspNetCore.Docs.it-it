---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Aggiunta di un modello (VB) | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e9a271c64347b4004d5cc5d9d91085c4e642e95d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model-vb"></a><span data-ttu-id="d605e-103">Aggiunta di un modello (VB)</span><span class="sxs-lookup"><span data-stu-id="d605e-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="d605e-104">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="d605e-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="d605e-105">In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d605e-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="d605e-106">Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="d605e-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="d605e-107">È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="d605e-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="d605e-108">In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="d605e-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="d605e-109">Prerequisiti di Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="d605e-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="d605e-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="d605e-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="d605e-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)</span><span class="sxs-lookup"><span data-stu-id="d605e-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="d605e-112">Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="d605e-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="d605e-113">Un progetto di Visual Web Developer con codice sorgente VB.NET complemento è disponibile in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="d605e-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="d605e-114">[Scaricare la versione di Visual Basic.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="d605e-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="d605e-115">Se si preferisce c#, passare il [c# versione](../cs/adding-a-model.md) di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d605e-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="d605e-116">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="d605e-116">Adding a Model</span></span>

<span data-ttu-id="d605e-117">In questa sezione si aggiungeranno alcune classi per la gestione di filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="d605e-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="d605e-118">Queste classi sarà la parte "modello" dell'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d605e-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="d605e-119">Utilizzare una tecnologia di accesso ai dati di .NET Framework nota come Entity Framework per definire e utilizzare queste classi di modello.</span><span class="sxs-lookup"><span data-stu-id="d605e-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="d605e-120">Entity Framework (noto anche come EF) supporta un paradigma di sviluppo denominato *Code First*.</span><span class="sxs-lookup"><span data-stu-id="d605e-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="d605e-121">Codice, innanzitutto, consente di creare gli oggetti del modello mediante la scrittura di classi semplici.</span><span class="sxs-lookup"><span data-stu-id="d605e-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="d605e-122">(Sono anche noti come classi POCO, da "oggetti CLR plain-old.") È il database creato al momento dalle classi, che consente a un flusso di lavoro di sviluppo molto pulito e rapido.</span><span class="sxs-lookup"><span data-stu-id="d605e-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="d605e-123">Aggiunta di classi modello</span><span class="sxs-lookup"><span data-stu-id="d605e-123">Adding Model Classes</span></span>

<span data-ttu-id="d605e-124">In **Esplora soluzioni**, fare clic destro la *modelli* cartella, selezionare **Aggiungi**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="d605e-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="d605e-125">Nome della classe "Filmato".</span><span class="sxs-lookup"><span data-stu-id="d605e-125">Name the class "Movie".</span></span>

<span data-ttu-id="d605e-126">Aggiungere le seguenti cinque proprietà per il `Movie` classe:</span><span class="sxs-lookup"><span data-stu-id="d605e-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="d605e-127">Si userà la `Movie` classe per rappresentare filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="d605e-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="d605e-128">Ogni istanza di un `Movie` corrisponde a una riga all'interno di una tabella di database e ogni proprietà dell'oggetto di `Movie` classe verrà eseguito il mapping a una colonna nella tabella.</span><span class="sxs-lookup"><span data-stu-id="d605e-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="d605e-129">Nello stesso file, aggiungere le seguenti `MovieDBContext` classe:</span><span class="sxs-lookup"><span data-stu-id="d605e-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="d605e-130">Il `MovieDBContext` classe rappresenta il contesto del database film Entity Framework, che gestisce il recupero, l'archiviazione e l'aggiornamento `Movie` classe istanze in un database.</span><span class="sxs-lookup"><span data-stu-id="d605e-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="d605e-131">Il `MovieDBContext` deriva il `DbContext` classe forniti da Entity Framework di base.</span><span class="sxs-lookup"><span data-stu-id="d605e-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="d605e-132">Per ulteriori informazioni su `DbContext` e `DbSet`, vedere [miglioramenti per la produttività per Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="d605e-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="d605e-133">Per poter fare riferimento a `DbContext` e `DbSet`, è necessario aggiungere il seguente `imports` istruzione all'inizio del file:</span><span class="sxs-lookup"><span data-stu-id="d605e-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="d605e-134">L'intero *Movie.vb* file è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d605e-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="d605e-135">Creazione di una stringa di connessione e l'utilizzo di SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="d605e-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="d605e-136">Il `MovieDBContext` classe creata gestisce le attività di connessione al database e di mapping `Movie` oggetti per i record del database.</span><span class="sxs-lookup"><span data-stu-id="d605e-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="d605e-137">Una domanda, che si potrebbe chiedere, tuttavia, viene illustrato come specificare il database si connetterà.</span><span class="sxs-lookup"><span data-stu-id="d605e-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="d605e-138">Operazione dovrà essere effettuata mediante l'aggiunta di informazioni di connessione nel *Web. config* file dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d605e-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="d605e-139">Aprire la radice dell'applicazione *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="d605e-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="d605e-140">(Non il *Web. config* file nel *viste* cartella.) Nell'immagine seguente mostra entrambi *Web. config* file; aprire il *Web. config* file racchiuse in un cerchio rosso.</span><span class="sxs-lookup"><span data-stu-id="d605e-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="d605e-141">Aggiungere la seguente stringa di connessione per il `<connectionStrings>` elemento il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="d605e-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="d605e-142">Nell'esempio seguente viene mostrata una parte di *Web. config* file con la nuova stringa di connessione aggiunta:</span><span class="sxs-lookup"><span data-stu-id="d605e-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="d605e-143">Questa piccola quantità di codice e XML è tutto ciò che occorre per scrivere per rappresentare e archiviare i dati dei film in un database.</span><span class="sxs-lookup"><span data-stu-id="d605e-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="d605e-144">Successivamente, si creerà un nuovo `MoviesController` classe che è possibile utilizzare per visualizzare i dati dei film e consentire agli utenti di creare nuove voci di film.</span><span class="sxs-lookup"><span data-stu-id="d605e-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d605e-145">[Precedente](adding-a-view.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="d605e-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
