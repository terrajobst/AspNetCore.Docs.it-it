---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Aggiunta di un modello (VB) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 138d21d5e33384fbc0dd99c33a9e43a95976ed06
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827806"
---
<a name="adding-a-model-vb"></a><span data-ttu-id="7098c-103">Aggiunta di un modello (VB)</span><span class="sxs-lookup"><span data-stu-id="7098c-103">Adding a Model (VB)</span></span>
====================
<span data-ttu-id="7098c-104">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7098c-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="7098c-105">Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7098c-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="7098c-106">Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="7098c-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="7098c-107">È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="7098c-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="7098c-108">In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="7098c-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="7098c-109">Prerequisiti di Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="7098c-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="7098c-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="7098c-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="7098c-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)</span><span class="sxs-lookup"><span data-stu-id="7098c-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="7098c-112">Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="7098c-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="7098c-113">Un progetto di Visual Web Developer con codice sorgente Visual Basic.NET è disponibile a complemento di questo argomento.</span><span class="sxs-lookup"><span data-stu-id="7098c-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="7098c-114">[Scaricare la versione VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="7098c-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="7098c-115">Se si preferisce c#, passare al [c# versione](../cs/adding-a-model.md) di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7098c-115">If you prefer C#, switch to the [C# version](../cs/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="7098c-116">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="7098c-116">Adding a Model</span></span>

<span data-ttu-id="7098c-117">In questa sezione si aggiungeranno alcune classi per la gestione di film in un database.</span><span class="sxs-lookup"><span data-stu-id="7098c-117">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="7098c-118">Queste classi saranno la parte "modello" dell'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7098c-118">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="7098c-119">Si userà una tecnologia di accesso ai dati di .NET Framework nota come Entity Framework per definire e usare queste classi del modello.</span><span class="sxs-lookup"><span data-stu-id="7098c-119">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="7098c-120">Entity Framework (noto anche come Entity Framework) supporta un paradigma di sviluppo denominato *Code First*.</span><span class="sxs-lookup"><span data-stu-id="7098c-120">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="7098c-121">Prima di tutto i codice consente di creare gli oggetti del modello mediante la scrittura di classi semplici.</span><span class="sxs-lookup"><span data-stu-id="7098c-121">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="7098c-122">(Queste sono anche note come classi POCO, da "plain-old CLR objects".) È quindi possibile avere il database creato in tempo reale dalle classi, che consente a un flusso di lavoro molto pulito e rapida evoluzione.</span><span class="sxs-lookup"><span data-stu-id="7098c-122">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="7098c-123">Aggiunta di classi di modello</span><span class="sxs-lookup"><span data-stu-id="7098c-123">Adding Model Classes</span></span>

<span data-ttu-id="7098c-124">Nella **Esplora soluzioni**, fare clic il *modelli* cartella, selezionare **Add**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="7098c-124">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="7098c-125">Denominare la classe "Movie".</span><span class="sxs-lookup"><span data-stu-id="7098c-125">Name the class "Movie".</span></span>

<span data-ttu-id="7098c-126">Aggiungere le seguenti cinque proprietà per il `Movie` classe:</span><span class="sxs-lookup"><span data-stu-id="7098c-126">Add the following five properties to the `Movie` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

<span data-ttu-id="7098c-127">Si userà il `Movie` classe per rappresentare i film in un database.</span><span class="sxs-lookup"><span data-stu-id="7098c-127">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="7098c-128">Ogni istanza di un `Movie` oggetto corrisponderà a una riga all'interno di una tabella di database e ogni proprietà del `Movie` classe verrà eseguito il mapping a una colonna nella tabella.</span><span class="sxs-lookup"><span data-stu-id="7098c-128">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="7098c-129">Nello stesso file, aggiungere il codice seguente `MovieDBContext` classe:</span><span class="sxs-lookup"><span data-stu-id="7098c-129">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

<span data-ttu-id="7098c-130">Il `MovieDBContext` classe rappresenta il contesto di database di film Entity Framework, che gestisce il recupero, l'archiviazione e aggiornando `Movie` classe istanze in un database.</span><span class="sxs-lookup"><span data-stu-id="7098c-130">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="7098c-131">Il `MovieDBContext` deriva dal `DbContext` classe fornita da Entity Framework di base.</span><span class="sxs-lookup"><span data-stu-id="7098c-131">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="7098c-132">Per altre informazioni sulle `DbContext` e `DbSet`, vedere [miglioramenti della produttività per Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="7098c-132">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="7098c-133">Per poter fare riferimento a `DbContext` e `DbSet`, è necessario aggiungere il codice seguente `imports` informativa nella parte superiore del file:</span><span class="sxs-lookup"><span data-stu-id="7098c-133">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `imports` statement at the top of the file:</span></span>

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

<span data-ttu-id="7098c-134">L'intero *Movie.vb* file è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7098c-134">The complete *Movie.vb* file is shown below.</span></span>

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="7098c-135">Creazione di una stringa di connessione e l'utilizzo di SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="7098c-135">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="7098c-136">Il `MovieDBContext` classe creata gestisce le attività di mapping e la connessione al database `Movie` oggetti per i record del database.</span><span class="sxs-lookup"><span data-stu-id="7098c-136">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="7098c-137">Una domanda che ci si potrebbe chiedere, tuttavia, è illustrato come specificare i database a cui si connette.</span><span class="sxs-lookup"><span data-stu-id="7098c-137">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="7098c-138">È necessario usare l'aggiunta di informazioni di connessione nel *Web. config* file dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7098c-138">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="7098c-139">Aprire la radice dell'applicazione *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="7098c-139">Open the application root *Web.config* file.</span></span> <span data-ttu-id="7098c-140">(Non il *Web. config* del file nei *viste* cartella.) Nell'immagine seguente mostra entrambi *Web. config* i file, aprire il *Web. config* file cerchiata in rosso.</span><span class="sxs-lookup"><span data-stu-id="7098c-140">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image2.png)

## 

<span data-ttu-id="7098c-141">Aggiungere la seguente stringa di connessione per il `<connectionStrings>` elemento il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="7098c-141">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="7098c-142">L'esempio seguente mostra una parte i *Web. config* file con la nuova stringa di connessione aggiunta:</span><span class="sxs-lookup"><span data-stu-id="7098c-142">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="7098c-143">Questa piccola quantità di codice e XML è tutto ciò che occorre per scrivere allo scopo di rappresentare e archiviare i dati dei film in un database.</span><span class="sxs-lookup"><span data-stu-id="7098c-143">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="7098c-144">Successivamente, si creerà un nuovo `MoviesController` classe che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuove voci di filmato.</span><span class="sxs-lookup"><span data-stu-id="7098c-144">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7098c-145">[Precedente](adding-a-view.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="7098c-145">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
