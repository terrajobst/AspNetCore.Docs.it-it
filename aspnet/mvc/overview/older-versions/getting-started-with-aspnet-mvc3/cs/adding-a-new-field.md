---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Aggiunge un nuovo campo al modello di filmato e tabella (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 3408afe6665d353e9570acc2740b5f1b7ba916da
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367157"
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a><span data-ttu-id="491d8-103">Aggiunge un nuovo campo al modello di filmato e tabella (c#)</span><span class="sxs-lookup"><span data-stu-id="491d8-103">Adding a New Field to the Movie Model and Table (C#)</span></span>
====================
<span data-ttu-id="491d8-104">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="491d8-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="491d8-105">È disponibile una versione aggiornata di questa esercitazione [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="491d8-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="491d8-106">È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="491d8-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="491d8-107">Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="491d8-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="491d8-108">Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="491d8-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="491d8-109">È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="491d8-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="491d8-110">In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="491d8-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="491d8-111">Prerequisiti di Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="491d8-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="491d8-112">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="491d8-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="491d8-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)</span><span class="sxs-lookup"><span data-stu-id="491d8-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="491d8-114">Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="491d8-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="491d8-115">Un progetto di Visual Web Developer con codice sorgente c# è disponibile a complemento di questo argomento.</span><span class="sxs-lookup"><span data-stu-id="491d8-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="491d8-116">[Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="491d8-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="491d8-117">Se si preferisce Visual Basic, passare al [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="491d8-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="491d8-118">In questa sezione verrà apportare alcune modifiche alle classi del modello e informazioni su come è possibile aggiornare lo schema del database in modo da corrispondere le modifiche al modello.</span><span class="sxs-lookup"><span data-stu-id="491d8-118">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="491d8-119">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="491d8-119">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="491d8-120">Iniziare aggiungendo una nuova `Rating` proprietà esistente `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="491d8-120">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="491d8-121">Aprire il *Movie.cs* file e aggiungere il `Rating` proprietà simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="491d8-121">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="491d8-122">L'intero `Movie` classe avrà l'aspetto simile al codice seguente:</span><span class="sxs-lookup"><span data-stu-id="491d8-122">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

<span data-ttu-id="491d8-123">Ricompilare l'applicazione usando il **Debug** &gt; **compilare film** comando di menu.</span><span class="sxs-lookup"><span data-stu-id="491d8-123">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="491d8-124">Ora che è stato aggiornato il `Model` (classe), è anche necessario aggiornare il *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* consente di visualizzare i modelli per supportare la nuova `Rating`proprietà.</span><span class="sxs-lookup"><span data-stu-id="491d8-124">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="491d8-125">Aprire il *\Views\Movies\Index.cshtml* file e aggiungere un `<th>Rating</th>` intestazione di colonna subito dopo il **prezzo** colonna.</span><span class="sxs-lookup"><span data-stu-id="491d8-125">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="491d8-126">Aggiungere quindi una `<td>` colonna verso la fine del modello per eseguire il rendering di `@item.Rating` valore.</span><span class="sxs-lookup"><span data-stu-id="491d8-126">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="491d8-127">Ecco quali aggiornato *index. cshtml* modello di visualizzazione è simile a:</span><span class="sxs-lookup"><span data-stu-id="491d8-127">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

<span data-ttu-id="491d8-128">Successivamente, aprire il *\Views\Movies\Create.cshtml* file e aggiungere il markup seguente verso la fine del form.</span><span class="sxs-lookup"><span data-stu-id="491d8-128">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="491d8-129">Si esegue il rendering di una casella di testo in modo che sia possibile specificare una classificazione uguale a quando viene creato un nuovo film.</span><span class="sxs-lookup"><span data-stu-id="491d8-129">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="491d8-130">Modello di gestione e differenze di Schema di Database</span><span class="sxs-lookup"><span data-stu-id="491d8-130">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="491d8-131">A questo punto è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` proprietà.</span><span class="sxs-lookup"><span data-stu-id="491d8-131">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="491d8-132">Ora eseguire l'applicazione e passare al */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="491d8-132">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="491d8-133">Quando si esegue questa operazione, tuttavia, si noterà l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="491d8-133">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="491d8-134">Viene visualizzato questo errore perché aggiornato `Movie` classe di modello nell'applicazione ora è diverso rispetto allo schema del `Movie` tabella del database esistente.</span><span class="sxs-lookup"><span data-stu-id="491d8-134">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="491d8-135">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="491d8-135">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="491d8-136">Per impostazione predefinita, quando si utilizza Code First di Entity Framework per creare automaticamente un database, come è stato fatto in precedenza in questa esercitazione, Code First consente di aggiungere una tabella nel database per rilevare se lo schema del database è sincronizzato con le classi del modello da che è stato generato.</span><span class="sxs-lookup"><span data-stu-id="491d8-136">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="491d8-137">Se non sono sincronizzati, Entity Framework genera un errore.</span><span class="sxs-lookup"><span data-stu-id="491d8-137">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="491d8-138">Questo rende più semplice individuare i problemi in fase di sviluppo che potrebbero altrimenti solo risultare (da errori sconosciuti) in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="491d8-138">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="491d8-139">La funzionalità di controllo della sincronizzazione è ciò che il messaggio di errore da visualizzare i risultati che abbiamo appena visto.</span><span class="sxs-lookup"><span data-stu-id="491d8-139">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="491d8-140">Esistono due approcci per la risoluzione dell'errore:</span><span class="sxs-lookup"><span data-stu-id="491d8-140">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="491d8-141">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="491d8-141">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="491d8-142">Questo approccio è molto utile quando si esegue lo sviluppo attivo in un database di prova, perché consente di migliorare rapidamente lo schema di modello e il database insieme.</span><span class="sxs-lookup"><span data-stu-id="491d8-142">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="491d8-143">Lo svantaggio, tuttavia, è che si perdono i dati esistenti nel database, in modo che è *non* vuole usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="491d8-143">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="491d8-144">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="491d8-144">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="491d8-145">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="491d8-145">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="491d8-146">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="491d8-146">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="491d8-147">Per questa esercitazione si userà il primo approccio, è possibile Code First di Entity Framework crei di nuovo automaticamente il database ogni volta che viene modificato il modello.</span><span class="sxs-lookup"><span data-stu-id="491d8-147">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="491d8-148">Ricreare automaticamente il Database su modifiche al modello</span><span class="sxs-lookup"><span data-stu-id="491d8-148">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="491d8-149">È possibile aggiornare l'applicazione in modo che Code First automaticamente viene eliminato e ricreato il database ogni volta che si modifica il modello per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="491d8-149">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="491d8-150">**Avviso** è consigliabile abilitare questo approccio di automaticamente eliminare e ricreare il database solo quando si usa un database di sviluppo o test, e *mai* in un database di produzione che contiene dati reali.</span><span class="sxs-lookup"><span data-stu-id="491d8-150">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="491d8-151">Usarla in un server di produzione può causare la perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="491d8-151">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="491d8-152">Nella **Esplora soluzioni**, fare clic il *modelli* cartella, selezionare **Add**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="491d8-152">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="491d8-153">Denominare la classe "MovieInitializer".</span><span class="sxs-lookup"><span data-stu-id="491d8-153">Name the class "MovieInitializer".</span></span> <span data-ttu-id="491d8-154">Aggiornamento di `MovieInitializer` classe destinata a contenere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="491d8-154">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="491d8-155">Il `MovieInitializer` classe specifica che il database utilizzato dal modello deve essere eliminato e ricreato automaticamente se cambiano le classi del modello.</span><span class="sxs-lookup"><span data-stu-id="491d8-155">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="491d8-156">Il codice include un `Seed` metodo per specificare alcuni dati predefiniti per aggiungere automaticamente al database una volta che ha creato (o ricreato).</span><span class="sxs-lookup"><span data-stu-id="491d8-156">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="491d8-157">Ciò fornisce un modo utile per popolare il database con alcuni dati di esempio, senza che sia necessario popolarlo manualmente ogni volta che si esegue un modello di modifica.</span><span class="sxs-lookup"><span data-stu-id="491d8-157">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="491d8-158">Ora che sono stati definiti i `MovieInitializer` (classe), è opportuno collegare i in modo che ogni volta che viene eseguita l'applicazione, controlla se le classi del modello sono diverse dallo schema del database.</span><span class="sxs-lookup"><span data-stu-id="491d8-158">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="491d8-159">In tal caso, è possibile eseguire l'inizializzatore per ricreare il database per corrispondere al modello e popolare il database con i dati di esempio.</span><span class="sxs-lookup"><span data-stu-id="491d8-159">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="491d8-160">Aprire il *Global. asax* file che si trova nella radice del `MvcMovies` progetto:</span><span class="sxs-lookup"><span data-stu-id="491d8-160">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

<span data-ttu-id="491d8-161">Il *Global. asax* file contiene la classe che definisce l'intera applicazione per il progetto e contiene un `Application_Start` gestore dell'evento che viene eseguito quando il primo avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="491d8-161">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="491d8-162">Aggiungiamo due istruzioni using all'inizio del file.</span><span class="sxs-lookup"><span data-stu-id="491d8-162">Let's add two using statements to the top of the file.</span></span> <span data-ttu-id="491d8-163">Il primo fa riferimento lo spazio dei nomi di Entity Framework e il secondo fa riferimento a spazio dei nomi in cui il `MovieInitializer` vite di classe:</span><span class="sxs-lookup"><span data-stu-id="491d8-163">The first references the Entity Framework namespace, and the second references the namespace where our `MovieInitializer` class lives:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

<span data-ttu-id="491d8-164">Trovare quindi il `Application_Start` metodo e aggiungere una chiamata a `Database.SetInitializer` all'inizio del metodo, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="491d8-164">Then find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

<span data-ttu-id="491d8-165">Il `Database.SetInitializer` istruzione appena aggiunta indica che il database utilizzando la `MovieDBContext` istanza deve essere automaticamente eliminata e ricreata se lo schema e il database non corrispondono.</span><span class="sxs-lookup"><span data-stu-id="491d8-165">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="491d8-166">E come si è visto, verrà inoltre popolato il database con i dati di esempio che viene specificati nel `MovieInitializer` classe.</span><span class="sxs-lookup"><span data-stu-id="491d8-166">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="491d8-167">Chiudi il *Global. asax* file.</span><span class="sxs-lookup"><span data-stu-id="491d8-167">Close the *Global.asax* file.</span></span>

<span data-ttu-id="491d8-168">Eseguire nuovamente l'applicazione e passare al */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="491d8-168">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="491d8-169">All'avvio dell'applicazione, rileva che la struttura del modello non corrisponde più lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="491d8-169">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="491d8-170">Automaticamente ricrea il database in modo da corrispondere la nuova struttura di modello e lo popola con i film di esempio:</span><span class="sxs-lookup"><span data-stu-id="491d8-170">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

<span data-ttu-id="491d8-172">Scegliere il **Crea nuovo** collegamento per aggiungere un nuovo film.</span><span class="sxs-lookup"><span data-stu-id="491d8-172">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="491d8-173">Si noti che è possibile aggiungere una classificazione.</span><span class="sxs-lookup"><span data-stu-id="491d8-173">Note that you can add a rating.</span></span>

<span data-ttu-id="491d8-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="491d8-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span></span>

<span data-ttu-id="491d8-175">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="491d8-175">Click **Create**.</span></span> <span data-ttu-id="491d8-176">Questo nuovo film, tra cui la classificazione, ora viene visualizzata nell'elenco di film:</span><span class="sxs-lookup"><span data-stu-id="491d8-176">The new movie, including the rating, now shows up in the movies listing:</span></span>

<span data-ttu-id="491d8-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="491d8-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span></span>

<span data-ttu-id="491d8-178">In questa sezione è stato illustrato come è possibile modificare gli oggetti modello e sincronizzare il database con le modifiche.</span><span class="sxs-lookup"><span data-stu-id="491d8-178">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="491d8-179">Si è appreso anche un modo per popolare un database appena creato con dati di esempio in modo che è possibile provare gli scenari.</span><span class="sxs-lookup"><span data-stu-id="491d8-179">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="491d8-180">Successivamente, diamo un'occhiata a come è possibile aggiungere più completa della logica di convalida alle classi di modello e abilitare alcune regole di business possano essere applicate.</span><span class="sxs-lookup"><span data-stu-id="491d8-180">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="491d8-181">[Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="491d8-181">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
