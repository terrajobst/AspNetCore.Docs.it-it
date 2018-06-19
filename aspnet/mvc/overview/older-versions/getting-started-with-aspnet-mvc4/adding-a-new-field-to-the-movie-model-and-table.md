---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Aggiunta di un nuovo campo al modello di film e di tabella | Documenti Microsoft
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui che utilizza ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice seguire e demo...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d8a42e9acdce687ab6e9742071dd2949f244622f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872790"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="a4396-104">Aggiunta di un nuovo campo al modello di film e di tabella</span><span class="sxs-lookup"><span data-stu-id="a4396-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="a4396-105">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a4396-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="a4396-106">È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="a4396-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="a4396-107">È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="a4396-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="a4396-108">In questa sezione si userà le migrazioni di Entity Framework Code First per eseguire la migrazione di alcune modifiche alle classi di modello, pertanto la modifica venga applicata al database.</span><span class="sxs-lookup"><span data-stu-id="a4396-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="a4396-109">Per impostazione predefinita, quando si utilizza Code First di Entity Framework per creare automaticamente un database, come in precedenza in questa esercitazione, il primo codice aggiunge una tabella nel database per rilevare se lo schema del database è sincronizzato con le classi di modello da che è stato generato.</span><span class="sxs-lookup"><span data-stu-id="a4396-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="a4396-110">Se non sono sincronizzati, Entity Framework genera un errore.</span><span class="sxs-lookup"><span data-stu-id="a4396-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="a4396-111">Questo rende più semplice individuare i problemi in fase di sviluppo che potrebbe essere in caso contrario solo (da errori oscuri) in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="a4396-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="a4396-112">Configurazione di migrazioni Code First per le modifiche al modello</span><span class="sxs-lookup"><span data-stu-id="a4396-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="a4396-113">Se si utilizza Visual Studio 2012, fare doppio clic il *Movies.mdf* file da Esplora soluzioni per aprire lo strumento di database.</span><span class="sxs-lookup"><span data-stu-id="a4396-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="a4396-114">Visual Studio Express per Web verrà visualizzato Esplora Database, che Visual Studio 2012 verranno visualizzati Esplora Server.</span><span class="sxs-lookup"><span data-stu-id="a4396-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="a4396-115">Se si utilizza Visual Studio 2010, utilizzare Esplora oggetti di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a4396-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="a4396-116">Lo strumento di database (Database Explorer, Esplora Server o Esplora oggetti di SQL Server), fare clic sul `MovieDBContext` e selezionare **eliminare** per eliminare il database di film.</span><span class="sxs-lookup"><span data-stu-id="a4396-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="a4396-117">Tornare alla Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="a4396-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="a4396-118">Fare clic con il pulsante destro sul *Movies.mdf* file e selezionare **eliminare** per rimuovere il database di film.</span><span class="sxs-lookup"><span data-stu-id="a4396-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="a4396-119">Compilare l'applicazione per assicurarsi che non siano presenti errori.</span><span class="sxs-lookup"><span data-stu-id="a4396-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="a4396-120">Dal **strumenti** menu, fare clic su **Gestione pacchetti libreria** e quindi **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="a4396-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Aggiungere i Management Pack](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="a4396-122">Nel **Package Manager Console** finestra il `PM>` prompt dei comandi immettere "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="a4396-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="a4396-123">Il **Enable-Migrations** comando (illustrato in precedenza) crea un *Configuration.cs* file in un nuovo *migrazioni* cartella.</span><span class="sxs-lookup"><span data-stu-id="a4396-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="a4396-124">Visual Studio apre il *Configuration.cs* file.</span><span class="sxs-lookup"><span data-stu-id="a4396-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="a4396-125">Sostituire il `Seed` metodo il *Configuration.cs* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a4396-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="a4396-126">Fare clic con il pulsante destro sulla riga rossa ondulata sotto `Movie` e selezionare **risolvere** quindi **utilizzando** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="a4396-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="a4396-127">In questo modo consente di aggiungere la seguente istruzione using:</span><span class="sxs-lookup"><span data-stu-id="a4396-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="a4396-128">Codice viene chiamato prima di migrazioni di `Seed` metodo dopo ogni migrazione (la chiamata al metodo **update-database** nella Console di gestione pacchetti), e questo metodo aggiorna le righe che sono già state inserite o li inserisce se sono non esistono ancora.</span><span class="sxs-lookup"><span data-stu-id="a4396-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="a4396-129">**Premere CTRL-MAIUSC-B per compilare il progetto.** (La procedura seguente avrà esito negativo se il non vengono compilati a questo punto.)</span><span class="sxs-lookup"><span data-stu-id="a4396-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="a4396-130">Il passaggio successivo consiste nel creare un `DbMigration` classe per la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="a4396-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="a4396-131">La migrazione a Crea un nuovo database, perché è eliminato il *movie.mdf* file in un passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="a4396-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="a4396-132">Nel **Package Manager Console** finestra, immettere il comando "migrazione aggiungere iniziale" per creare la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="a4396-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="a4396-133">Il nome "Iniziale" è arbitrario e viene utilizzato per denominare il file di migrazione creato.</span><span class="sxs-lookup"><span data-stu-id="a4396-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="a4396-134">Migrazioni Code First consente di creare un altro file di classe di *migrazioni* cartella (con il nome *{DateStamp}\_Initial.cs* ), e questa classe contiene codice che crea lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="a4396-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="a4396-135">Il nome del file di migrazione è pre-fissa con un timestamp per agevolare l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="a4396-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="a4396-136">Esaminare il *{DateStamp}\_Initial.cs* file, contiene le istruzioni per creare la tabella di filmati per il database di film.</span><span class="sxs-lookup"><span data-stu-id="a4396-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="a4396-137">Quando si aggiorna il database nelle istruzioni seguenti, *{DateStamp}\_Initial.cs* file verrà eseguito e creare lo schema di database.</span><span class="sxs-lookup"><span data-stu-id="a4396-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="a4396-138">Il **valore di inizializzazione** metodo verrà eseguito per popolare il database con dati di test.</span><span class="sxs-lookup"><span data-stu-id="a4396-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="a4396-139">Nel **Package Manager Console**, immettere il comando "update-database" per creare il database ed eseguire il **valore di inizializzazione** metodo.</span><span class="sxs-lookup"><span data-stu-id="a4396-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="a4396-140">Se si verifica un errore che indica una tabella esiste già e non può essere creata, è probabilmente perché è stata eseguita l'applicazione dopo l'eliminazione di database e prima di eseguire `update-database`.</span><span class="sxs-lookup"><span data-stu-id="a4396-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="a4396-141">In tal caso, eliminare il *Movies.mdf* file nuovo, quindi ripetere il `update-database` comando.</span><span class="sxs-lookup"><span data-stu-id="a4396-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="a4396-142">Se viene ancora visualizzato un errore, eliminare la cartella migrations e il contenuto, quindi iniziare con le istruzioni nella parte superiore della pagina (che è l'eliminazione di *Movies.mdf* file, quindi procedere con il comando Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="a4396-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="a4396-143">Eseguire l'applicazione e passare il */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="a4396-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="a4396-144">Visualizzazione dei dati del valore di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="a4396-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="a4396-145">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="a4396-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="a4396-146">Per iniziare, aggiungere un nuovo `Rating` proprietà esistente `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="a4396-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="a4396-147">Aprire il *Models\Movie.cs* file e aggiungere il `Rating` proprietà simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="a4396-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="a4396-148">L'intero `Movie` classe ora ha un aspetto simile al codice seguente:</span><span class="sxs-lookup"><span data-stu-id="a4396-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="a4396-149">Compilare l'applicazione tramite il **compilare** &gt; **compilare film** menu comando oppure premendo CTRL-MAIUSC-B.</span><span class="sxs-lookup"><span data-stu-id="a4396-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="a4396-150">Ora che è stato aggiornato il `Model` (classe), è anche necessario aggiornare il *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* consente di visualizzare i modelli per visualizzare il nuovo `Rating`proprietà nella vista del browser.</span><span class="sxs-lookup"><span data-stu-id="a4396-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="a4396-151">Aprire il<em>\Views\Movies\Index.cshtml</em> file e aggiungere un `<th>Rating</th>` intestazione di colonna subito dopo il <strong>prezzo</strong> colonna.</span><span class="sxs-lookup"><span data-stu-id="a4396-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="a4396-152">Aggiungere quindi una `<td>` colonna verso la fine del modello per il rendering di `@item.Rating` valore.</span><span class="sxs-lookup"><span data-stu-id="a4396-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="a4396-153">Di seguito è riportato l'aggiornato <em>cshtml</em> modello di visualizzazione è simile a:</span><span class="sxs-lookup"><span data-stu-id="a4396-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="a4396-154">Aprire quindi il *\Views\Movies\Create.cshtml* file e aggiungere il markup seguente verso la fine del form.</span><span class="sxs-lookup"><span data-stu-id="a4396-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="a4396-155">Si esegue il rendering di una casella di testo in modo che quando viene creato un nuovo filmato, è possibile specificare una classificazione.</span><span class="sxs-lookup"><span data-stu-id="a4396-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="a4396-156">Ora è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` proprietà.</span><span class="sxs-lookup"><span data-stu-id="a4396-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="a4396-157">Eseguire l'applicazione e passare al */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="a4396-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="a4396-158">Quando si esegue questa operazione, tuttavia, verrà visualizzato uno dei seguenti errori:</span><span class="sxs-lookup"><span data-stu-id="a4396-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="a4396-159">Viene visualizzato questo errore perché l'aggiornamento `Movie` classe del modello nell'applicazione è diverso rispetto allo schema di ora il `Movie` tabella del database esistente.</span><span class="sxs-lookup"><span data-stu-id="a4396-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="a4396-160">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="a4396-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="a4396-161">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="a4396-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="a4396-162">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="a4396-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="a4396-163">Questo approccio è molto utile durante la fase di sviluppo attivo su un database di test. Consente di rapidamente evolvere lo schema del modello e il database insieme.</span><span class="sxs-lookup"><span data-stu-id="a4396-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="a4396-164">Lo svantaggio, tuttavia, è che si perdono i dati esistenti nel database, in modo si *non* per usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="a4396-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="a4396-165">Utilizzando un inizializzatore per inizializzare automaticamente un database con dati di test è spesso un modo produttivo per sviluppare un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a4396-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="a4396-166">Per ulteriori informazioni sugli inizializzatori di database di Entity Framework, vedere di Tom Dykstra [esercitazione ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="a4396-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="a4396-167">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="a4396-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="a4396-168">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="a4396-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="a4396-169">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="a4396-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="a4396-170">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="a4396-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="a4396-171">Per questa esercitazione si userà Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="a4396-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="a4396-172">Aggiornare il metodo di inizializzazione in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="a4396-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="a4396-173">Aprire il file Migrations\Configuration.cs e aggiungere un campo di classificazione per ogni oggetto del film.</span><span class="sxs-lookup"><span data-stu-id="a4396-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="a4396-174">Compilare la soluzione e quindi aprire il **Package Manager Console** finestra e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a4396-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="a4396-175">Il `add-migration` comando indica al framework di migrazione per esaminare il modello di film corrente con lo schema di film DB corrente e creare il codice necessario per eseguire la migrazione del database al nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="a4396-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="a4396-176">Il AddRatingMig è arbitrario e viene utilizzato per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="a4396-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="a4396-177">È consigliabile utilizzare un nome significativo per il passaggio della migrazione.</span><span class="sxs-lookup"><span data-stu-id="a4396-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="a4396-178">Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` classe derivata e il `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="a4396-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="a4396-179">Compilare la soluzione e quindi immettere il comando "Aggiorna database" di **Package Manager Console** finestra.</span><span class="sxs-lookup"><span data-stu-id="a4396-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="a4396-180">La figura seguente mostra l'output di **Console di gestione pacchetti** finestra (l'indicatore di data anteponendo AddRatingMig saranno diverso.)</span><span class="sxs-lookup"><span data-stu-id="a4396-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="a4396-181">Eseguire nuovamente l'applicazione e passare all'URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="a4396-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="a4396-182">È possibile visualizzare il nuovo campo di classificazione.</span><span class="sxs-lookup"><span data-stu-id="a4396-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="a4396-183">Fare clic su di **Crea nuovo** collegamento per aggiungere un nuovo film.</span><span class="sxs-lookup"><span data-stu-id="a4396-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="a4396-184">Si noti che è possibile aggiungere una classificazione.</span><span class="sxs-lookup"><span data-stu-id="a4396-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="a4396-186">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="a4396-186">Click **Create**.</span></span> <span data-ttu-id="a4396-187">Il nuovo filmato, tra cui la valutazione, viene inserito nei film elenco:</span><span class="sxs-lookup"><span data-stu-id="a4396-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="a4396-189">È inoltre necessario aggiungere il `Rating` campo per la modifica, dettagli e SearchIndex modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="a4396-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="a4396-190">È possibile immettere il comando "Aggiorna database" di **Package Manager Console** nuovamente finestra e modifiche non verranno apportate, perché lo schema corrisponde al modello.</span><span class="sxs-lookup"><span data-stu-id="a4396-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="a4396-191">In questa sezione è stato illustrato come è possibile modificare gli oggetti del modello e sincronizzare il database con le modifiche.</span><span class="sxs-lookup"><span data-stu-id="a4396-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="a4396-192">È stato inoltre un modo per popolare un database appena creato con dati di esempio in modo è possibile provare gli scenari.</span><span class="sxs-lookup"><span data-stu-id="a4396-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="a4396-193">Successivamente, si esaminerà come è possibile aggiungere la logica di convalida più completa per le classi di modello e abilitare alcune regole di business da applicare.</span><span class="sxs-lookup"><span data-stu-id="a4396-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a4396-194">[Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="a4396-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
