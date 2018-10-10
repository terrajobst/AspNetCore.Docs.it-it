---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Aggiunge un nuovo campo al modello di filmato e tabella | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro e molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 0f9b659b67a9a62635091b1e87169bce1218281a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911413"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="70f25-104">Aggiunge un nuovo campo al modello di filmato e tabella</span><span class="sxs-lookup"><span data-stu-id="70f25-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="70f25-105">da [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="70f25-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> > [!NOTE]
> > <span data-ttu-id="70f25-106">È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="70f25-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="70f25-107">È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="70f25-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="70f25-108">In questa sezione si userà migrazioni di Entity Framework Code First per eseguire la migrazione di alcune modifiche alle classi di modello, pertanto la modifica venga applicata nel database.</span><span class="sxs-lookup"><span data-stu-id="70f25-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="70f25-109">Per impostazione predefinita, quando si utilizza Code First di Entity Framework per creare automaticamente un database, come è stato fatto in precedenza in questa esercitazione, Code First consente di aggiungere una tabella nel database per rilevare se lo schema del database è sincronizzato con le classi del modello da che è stato generato.</span><span class="sxs-lookup"><span data-stu-id="70f25-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="70f25-110">Se non sono sincronizzati, Entity Framework genera un errore.</span><span class="sxs-lookup"><span data-stu-id="70f25-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="70f25-111">Questo rende più semplice individuare i problemi in fase di sviluppo che potrebbero altrimenti solo risultare (da errori sconosciuti) in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="70f25-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="70f25-112">Configurazione di migrazioni Code First per le modifiche al modello</span><span class="sxs-lookup"><span data-stu-id="70f25-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="70f25-113">Se si usa Visual Studio 2012, fare doppio clic il *Movies.mdf* file da Esplora soluzioni per aprire lo strumento di database.</span><span class="sxs-lookup"><span data-stu-id="70f25-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="70f25-114">Visual Studio Express per Web verrà visualizzato Esplora Database, che Visual Studio 2012 mostrerà Esplora Server.</span><span class="sxs-lookup"><span data-stu-id="70f25-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="70f25-115">Se si usa Visual Studio 2010, utilizzare Esplora oggetti di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="70f25-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="70f25-116">Nello strumento di database (Database Explorer, Esplora Server o Esplora oggetti di SQL Server), fare clic sulla `MovieDBContext` e selezionare **eliminare** per eliminare il database di film.</span><span class="sxs-lookup"><span data-stu-id="70f25-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="70f25-117">Passare a Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="70f25-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="70f25-118">Fare clic con il pulsante destro sul *Movies.mdf* del file e selezionare **eliminare** per rimuovere il database di film.</span><span class="sxs-lookup"><span data-stu-id="70f25-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="70f25-119">Compilare l'applicazione per assicurarsi che non siano presenti errori.</span><span class="sxs-lookup"><span data-stu-id="70f25-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="70f25-120">Dal **degli strumenti** menu, fare clic su **Gestione pacchetti NuGet** e quindi **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="70f25-120">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Aggiungere Pack Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="70f25-122">Nel **Console di gestione pacchetti** la finestra al `PM>` prompt dei comandi immettere "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="70f25-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="70f25-123">Il **Enable-Migrations** comando (illustrato in precedenza) consente di creare un *Configuration.cs* file in una nuova *migrazioni* cartella.</span><span class="sxs-lookup"><span data-stu-id="70f25-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="70f25-124">Visual Studio apre il *Configuration.cs* file.</span><span class="sxs-lookup"><span data-stu-id="70f25-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="70f25-125">Sostituire il `Seed` metodo di *Configuration.cs* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="70f25-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="70f25-126">Fare clic con il pulsante destro sulla riga rossa ondulata sotto `Movie` e selezionare **risolvere** quindi **usando** **mvcmovie. Models;**</span><span class="sxs-lookup"><span data-stu-id="70f25-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="70f25-127">In questo modo consente di aggiungere la seguente istruzione using:</span><span class="sxs-lookup"><span data-stu-id="70f25-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="70f25-128">Codice prima le migrazioni chiamano il `Seed` metodo dopo ogni migrazione (vale a dire, la chiamata **update-database** nella Console di gestione pacchetti), e questo metodo aggiorna le righe che sono già state inserite o li inserisce se sono non esistono ancora.</span><span class="sxs-lookup"><span data-stu-id="70f25-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="70f25-129">**Premere CTRL + MAIUSC + B per compilare il progetto.** (La procedura seguente avrà esito negativo se il non vengono compilati a questo punto.)</span><span class="sxs-lookup"><span data-stu-id="70f25-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="70f25-130">Il passaggio successivo consiste nel creare un `DbMigration` classe per la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="70f25-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="70f25-131">Questa migrazione a viene creato un nuovo database, che è per questo motivo hai eliminato il *movie.mdf* file in un passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="70f25-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="70f25-132">Nel **Console di gestione pacchetti** finestra, immettere il comando "add-migration Initial" per creare la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="70f25-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="70f25-133">Il nome "Iniziale" è arbitrario e viene usato per denominare il file di migrazione creato.</span><span class="sxs-lookup"><span data-stu-id="70f25-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="70f25-134">Migrazioni Code First consente di creare un altro file di classe nel *migrazioni* cartella (con il nome *{DateStamp}\_Initial.cs* ), e questa classe contiene codice che crea lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="70f25-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="70f25-135">Il nome del file di migrazione è pre-fissa con un timestamp nella Guida in linea con l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="70f25-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="70f25-136">Esaminare i *{DateStamp}\_Initial.cs* file, contiene le istruzioni per creare la tabella di film per i database dei film.</span><span class="sxs-lookup"><span data-stu-id="70f25-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="70f25-137">Quando si aggiorna il database nelle versioni precedenti, queste istruzioni *{DateStamp}\_Initial.cs* file verrà eseguito e creare lo schema di database.</span><span class="sxs-lookup"><span data-stu-id="70f25-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="70f25-138">L'oggetto **Seed** metodo verrà eseguite per popolare il database con dati di test.</span><span class="sxs-lookup"><span data-stu-id="70f25-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="70f25-139">Nel **Console di gestione pacchetti**, immettere il comando "update-database" per creare il database ed eseguire il **Seed** (metodo).</span><span class="sxs-lookup"><span data-stu-id="70f25-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="70f25-140">Se si verifica un errore che indica una tabella esiste già e non può essere creata, è probabilmente perché è stata eseguita l'applicazione dopo che è stato eliminato il database e prima di eseguire `update-database`.</span><span class="sxs-lookup"><span data-stu-id="70f25-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="70f25-141">In tal caso, eliminare il *Movies.mdf* nuovamente file e ripetere il `update-database` comando.</span><span class="sxs-lookup"><span data-stu-id="70f25-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="70f25-142">Se viene ancora visualizzato un errore, eliminare la cartella migrations e il contenuto, iniziare con le istruzioni nella parte superiore della pagina (eliminazione che è il *Movies.mdf* file, quindi procedere a Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="70f25-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="70f25-143">Eseguire l'applicazione e passare al */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="70f25-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="70f25-144">I dati di seeding viene visualizzati.</span><span class="sxs-lookup"><span data-stu-id="70f25-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="70f25-145">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="70f25-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="70f25-146">Iniziare aggiungendo una nuova `Rating` proprietà esistente `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="70f25-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="70f25-147">Aprire il *Models\Movie.cs* file e aggiungere il `Rating` proprietà simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="70f25-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="70f25-148">L'intero `Movie` classe avrà l'aspetto simile al codice seguente:</span><span class="sxs-lookup"><span data-stu-id="70f25-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="70f25-149">Compilare l'applicazione usando il **compilare** &gt; **compilare film** menu comando o premendo CTRL-MAIUSC-B.</span><span class="sxs-lookup"><span data-stu-id="70f25-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="70f25-150">Ora che è stato aggiornato il `Model` (classe), è anche necessario aggiornare il *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* consente di visualizzare i modelli per visualizzare il nuovo `Rating`proprietà nella vista del browser.</span><span class="sxs-lookup"><span data-stu-id="70f25-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="70f25-151">Aprire il<em>\Views\Movies\Index.cshtml</em> file e aggiungere un `<th>Rating</th>` intestazione di colonna subito dopo il <strong>prezzo</strong> colonna.</span><span class="sxs-lookup"><span data-stu-id="70f25-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="70f25-152">Aggiungere quindi una `<td>` colonna verso la fine del modello per eseguire il rendering di `@item.Rating` valore.</span><span class="sxs-lookup"><span data-stu-id="70f25-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="70f25-153">Ecco quali aggiornato <em>index. cshtml</em> modello di visualizzazione è simile a:</span><span class="sxs-lookup"><span data-stu-id="70f25-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="70f25-154">Successivamente, aprire il *\Views\Movies\Create.cshtml* file e aggiungere il markup seguente verso la fine del form.</span><span class="sxs-lookup"><span data-stu-id="70f25-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="70f25-155">Si esegue il rendering di una casella di testo in modo che sia possibile specificare una classificazione uguale a quando viene creato un nuovo film.</span><span class="sxs-lookup"><span data-stu-id="70f25-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="70f25-156">A questo punto è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` proprietà.</span><span class="sxs-lookup"><span data-stu-id="70f25-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="70f25-157">Ora eseguire l'applicazione e passare al */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="70f25-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="70f25-158">Quando si esegue questa operazione, tuttavia, si noterà uno dei seguenti errori:</span><span class="sxs-lookup"><span data-stu-id="70f25-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="70f25-159">Viene visualizzato questo errore perché aggiornato `Movie` classe di modello nell'applicazione ora è diverso rispetto allo schema del `Movie` tabella del database esistente.</span><span class="sxs-lookup"><span data-stu-id="70f25-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="70f25-160">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="70f25-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="70f25-161">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="70f25-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="70f25-162">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="70f25-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="70f25-163">Questo approccio è molto utile quando si esegue lo sviluppo attivo in un database di test. Consente di migliorare rapidamente lo schema di modello e il database insieme.</span><span class="sxs-lookup"><span data-stu-id="70f25-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="70f25-164">Lo svantaggio, tuttavia, è che si perdono i dati esistenti nel database, in modo che è *non* vuole usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="70f25-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="70f25-165">Usare un inizializzatore per inizializzare automaticamente un database con dati di test è spesso un modo produttivo per sviluppare un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="70f25-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="70f25-166">Per altre informazioni sugli inizializzatori di database di Entity Framework, vedere di Tom Dykstra [esercitazione su ASP.NET MVC o Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="70f25-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="70f25-167">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="70f25-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="70f25-168">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="70f25-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="70f25-169">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="70f25-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="70f25-170">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="70f25-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="70f25-171">Per questa esercitazione si userà Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="70f25-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="70f25-172">Aggiornare il metodo di inizializzazione in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="70f25-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="70f25-173">Aprire il file Migrations\Configuration.cs e aggiungere un campo di classificazione per ogni oggetto del film.</span><span class="sxs-lookup"><span data-stu-id="70f25-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="70f25-174">Compilare la soluzione e quindi aprire il **Console di gestione pacchetti** finestra e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="70f25-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="70f25-175">Il `add-migration` comando indica al framework di migrazione per esaminare il modello di filmato corrente con lo schema di film DB corrente e creare il codice necessario per eseguire la migrazione del database al nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="70f25-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="70f25-176">Il AddRatingMig è arbitrario e viene usato per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="70f25-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="70f25-177">È utile usare un nome significativo per il passaggio della migrazione.</span><span class="sxs-lookup"><span data-stu-id="70f25-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="70f25-178">Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` classe derivata e il `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="70f25-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="70f25-179">Compilare la soluzione e quindi immettere il comando "update-database" nel **Console di gestione pacchetti** finestra.</span><span class="sxs-lookup"><span data-stu-id="70f25-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="70f25-180">L'immagine seguente mostra l'output nel **Console di gestione pacchetti** finestra (l'indicatore di data anteponendo AddRatingMig sarà diverso.)</span><span class="sxs-lookup"><span data-stu-id="70f25-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="70f25-181">Eseguire nuovamente l'applicazione e passare all'URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="70f25-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="70f25-182">È possibile visualizzare il nuovo campo di classificazione.</span><span class="sxs-lookup"><span data-stu-id="70f25-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="70f25-183">Scegliere il **Crea nuovo** collegamento per aggiungere un nuovo film.</span><span class="sxs-lookup"><span data-stu-id="70f25-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="70f25-184">Si noti che è possibile aggiungere una classificazione.</span><span class="sxs-lookup"><span data-stu-id="70f25-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="70f25-186">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="70f25-186">Click **Create**.</span></span> <span data-ttu-id="70f25-187">Questo nuovo film, tra cui la classificazione, ora viene visualizzata nell'elenco di film:</span><span class="sxs-lookup"><span data-stu-id="70f25-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="70f25-189">È necessario aggiungere anche il `Rating` campo per la modifica, dettagli e SearchIndex modelli di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="70f25-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="70f25-190">È possibile immettere il comando "update-database" nel **Console di gestione pacchetti** finestra nuovamente e senza apportare modifiche al sarebbe Impossibile stabilire la, lo schema corrisponde al modello.</span><span class="sxs-lookup"><span data-stu-id="70f25-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="70f25-191">In questa sezione è stato illustrato come è possibile modificare gli oggetti modello e sincronizzare il database con le modifiche.</span><span class="sxs-lookup"><span data-stu-id="70f25-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="70f25-192">Si è appreso anche un modo per popolare un database appena creato con dati di esempio in modo che è possibile provare gli scenari.</span><span class="sxs-lookup"><span data-stu-id="70f25-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="70f25-193">Successivamente, diamo un'occhiata a come è possibile aggiungere più completa della logica di convalida alle classi di modello e abilitare alcune regole di business possano essere applicate.</span><span class="sxs-lookup"><span data-stu-id="70f25-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="70f25-194">[Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="70f25-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
