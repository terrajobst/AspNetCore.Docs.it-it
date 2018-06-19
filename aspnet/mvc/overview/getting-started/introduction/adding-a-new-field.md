---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Aggiunta di un nuovo campo | Documenti Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 0dac798eba586cdcc232cedd262e610b954004df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874184"
---
<a name="adding-a-new-field"></a><span data-ttu-id="031ff-102">Aggiunta di un nuovo campo</span><span class="sxs-lookup"><span data-stu-id="031ff-102">Adding a New Field</span></span>
====================
<span data-ttu-id="031ff-103">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="031ff-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="031ff-104">In questa sezione si userà le migrazioni di Entity Framework Code First per eseguire la migrazione di alcune modifiche alle classi di modello, pertanto la modifica venga applicata al database.</span><span class="sxs-lookup"><span data-stu-id="031ff-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="031ff-105">Per impostazione predefinita, quando si utilizza Code First di Entity Framework per creare automaticamente un database, come in precedenza in questa esercitazione, il primo codice aggiunge una tabella nel database per rilevare se lo schema del database è sincronizzato con le classi di modello da che è stato generato.</span><span class="sxs-lookup"><span data-stu-id="031ff-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="031ff-106">Se non sono sincronizzati, Entity Framework genera un errore.</span><span class="sxs-lookup"><span data-stu-id="031ff-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="031ff-107">Questo rende più semplice individuare i problemi in fase di sviluppo che potrebbe essere in caso contrario solo (da errori oscuri) in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="031ff-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="031ff-108">Configurazione di migrazioni Code First per le modifiche al modello</span><span class="sxs-lookup"><span data-stu-id="031ff-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="031ff-109">Passare a Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="031ff-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="031ff-110">Fare clic con il pulsante destro sul *Movies.mdf* file e selezionare **eliminare** per rimuovere il database di film.</span><span class="sxs-lookup"><span data-stu-id="031ff-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="031ff-111">Se non viene visualizzato il *Movies.mdf* file, fare clic su di **Mostra tutti i file** icona visualizzata di sotto della struttura di colore rosso.</span><span class="sxs-lookup"><span data-stu-id="031ff-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="031ff-112">Compilare l'applicazione per assicurarsi che non siano presenti errori.</span><span class="sxs-lookup"><span data-stu-id="031ff-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="031ff-113">Dal **strumenti** menu, fare clic su **Gestione pacchetti NuGet** e quindi **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="031ff-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Aggiungere i Management Pack](adding-a-new-field/_static/image2.png)

<span data-ttu-id="031ff-115">Nel **Package Manager Console** finestra il `PM>` prompt dei comandi immettere</span><span class="sxs-lookup"><span data-stu-id="031ff-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="031ff-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="031ff-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="031ff-117">Il **Enable-Migrations** comando (illustrato in precedenza) crea un *Configuration.cs* file in un nuovo *migrazioni* cartella.</span><span class="sxs-lookup"><span data-stu-id="031ff-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="031ff-118">Visual Studio apre il *Configuration.cs* file.</span><span class="sxs-lookup"><span data-stu-id="031ff-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="031ff-119">Sostituire il `Seed` metodo il *Configuration.cs* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="031ff-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="031ff-120">Passare il mouse sulla riga rossa ondulata sotto `Movie` e fare clic su `Show Potential Fixes` e quindi fare clic su **utilizzando** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="031ff-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="031ff-121">In questo modo consente di aggiungere la seguente istruzione using:</span><span class="sxs-lookup"><span data-stu-id="031ff-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> <span data-ttu-id="031ff-122">Codice viene chiamato prima di migrazioni di `Seed` metodo dopo ogni migrazione (la chiamata al metodo **update-database** nella Console di gestione pacchetti), e questo metodo aggiorna le righe che sono già state inserite o li inserisce se sono non esistono ancora.</span><span class="sxs-lookup"><span data-stu-id="031ff-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="031ff-123">Il [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo nel codice seguente esegue un'operazione "upsert":</span><span class="sxs-lookup"><span data-stu-id="031ff-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="031ff-124">Poiché il [valore di inizializzazione](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodo viene eseguito con ogni tipo di migrazione, è possibile inserire solo dati, in quanto le righe che si sta tentando di aggiungere già sarà disponibile dopo la migrazione prima che crea il database.</span><span class="sxs-lookup"><span data-stu-id="031ff-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="031ff-125">Il "[upsert](http://en.wikipedia.org/wiki/Upsert)" operazione impedisce gli errori che accadrebbe se si tenta di inserire una riga già esistente, ma esegue l'override di qualsiasi modifica ai dati apportate durante il test dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="031ff-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="031ff-126">I dati di test in alcune tabelle è possibile evitare di raggiungere tale obiettivo: in alcuni casi quando si modificano i dati durante il test le proprie modifiche rimangano dopo gli aggiornamenti del database.</span><span class="sxs-lookup"><span data-stu-id="031ff-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="031ff-127">In questo caso si desidera eseguire un'operazione di inserimento condizionale: inserire una riga solo se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="031ff-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
> 
> <span data-ttu-id="031ff-128">Il primo parametro passato per il [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo consente di specificare la proprietà da utilizzare per verificare se esiste già una riga.</span><span class="sxs-lookup"><span data-stu-id="031ff-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="031ff-129">Per i dati di test filmato che si desidera fornire, il `Title` proprietà può essere utilizzata per questo scopo poiché ogni libro nell'elenco è univoco:</span><span class="sxs-lookup"><span data-stu-id="031ff-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="031ff-130">Questo codice si presuppone che i titoli siano univoci.</span><span class="sxs-lookup"><span data-stu-id="031ff-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="031ff-131">Se si aggiunge manualmente un titolo duplicato, si otterrà la seguente eccezione alla successiva che si eseguire la migrazione.</span><span class="sxs-lookup"><span data-stu-id="031ff-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
> 
>  <span data-ttu-id="031ff-132">*La sequenza contiene più di un elemento*</span><span class="sxs-lookup"><span data-stu-id="031ff-132">*Sequence contains more than one element*</span></span>  
> 
> <span data-ttu-id="031ff-133">Per ulteriori informazioni sul [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo, vedere [prestare attenzione con EF 4.3 AddOrUpdate metodo](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...</span><span class="sxs-lookup"><span data-stu-id="031ff-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>


<span data-ttu-id="031ff-134">**Premere CTRL-MAIUSC-B per compilare il progetto.** (La procedura seguente avrà esito negativo se a questo punto.)</span><span class="sxs-lookup"><span data-stu-id="031ff-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="031ff-135">Il passaggio successivo consiste nel creare un `DbMigration` classe per la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="031ff-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="031ff-136">Questa migrazione crea un nuovo database, perché è eliminato il *movie.mdf* file in un passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="031ff-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="031ff-137">Nel **Package Manager Console** finestra, immettere il comando `add-migration Initial` per creare la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="031ff-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="031ff-138">Il nome "Iniziale" è arbitrario e viene utilizzato per denominare il file di migrazione creato.</span><span class="sxs-lookup"><span data-stu-id="031ff-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="031ff-139">Migrazioni Code First consente di creare un altro file di classe di *migrazioni* cartella (con il nome *{DateStamp}\_Initial.cs* ), e questa classe contiene codice che crea lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="031ff-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="031ff-140">Il nome del file di migrazione è pre-fissa con un timestamp per agevolare l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="031ff-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="031ff-141">Esaminare il *{DateStamp}\_Initial.cs* file, contiene le istruzioni per creare il `Movies` tabella per il database di film.</span><span class="sxs-lookup"><span data-stu-id="031ff-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="031ff-142">Quando si aggiorna il database nelle istruzioni seguenti, *{DateStamp}\_Initial.cs* file verrà eseguito e creare lo schema di database.</span><span class="sxs-lookup"><span data-stu-id="031ff-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="031ff-143">Il **valore di inizializzazione** metodo verrà eseguito per popolare il database con dati di test.</span><span class="sxs-lookup"><span data-stu-id="031ff-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="031ff-144">Nel **Package Manager Console**, immettere il comando `update-database` per creare il database ed eseguire il `Seed` metodo.</span><span class="sxs-lookup"><span data-stu-id="031ff-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="031ff-145">Se si verifica un errore che indica una tabella esiste già e non può essere creata, è probabilmente perché è stata eseguita l'applicazione dopo l'eliminazione di database e prima di eseguire `update-database`.</span><span class="sxs-lookup"><span data-stu-id="031ff-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="031ff-146">In tal caso, eliminare il *Movies.mdf* file nuovo, quindi ripetere il `update-database` comando.</span><span class="sxs-lookup"><span data-stu-id="031ff-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="031ff-147">Se viene ancora visualizzato un errore, eliminare la cartella migrations e il contenuto, quindi iniziare con le istruzioni nella parte superiore della pagina (che è l'eliminazione di *Movies.mdf* file, quindi procedere con il comando Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="031ff-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="031ff-148">Se viene visualizzato ancora un se, aprire Esplora oggetti di SQL Server e rimuovere il database dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="031ff-148">If you still get an eror, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="031ff-149">Eseguire l'applicazione e passare il */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="031ff-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="031ff-150">Visualizzazione dei dati del valore di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="031ff-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="031ff-151">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="031ff-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="031ff-152">Per iniziare, aggiungere un nuovo `Rating` proprietà esistente `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="031ff-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="031ff-153">Aprire il *Models\Movie.cs* file e aggiungere il `Rating` proprietà simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="031ff-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="031ff-154">L'intero `Movie` classe ora ha un aspetto simile al codice seguente:</span><span class="sxs-lookup"><span data-stu-id="031ff-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="031ff-155">Compilare l'applicazione (Ctrl + MAIUSC + B).</span><span class="sxs-lookup"><span data-stu-id="031ff-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="031ff-156">Poiché hai aggiunto un nuovo campo di `Movie` (classe), è inoltre necessario aggiornare l'associazione *elenco approvato* in modo che sia inclusa la nuova proprietà.</span><span class="sxs-lookup"><span data-stu-id="031ff-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="031ff-157">Aggiornamento di `bind` attributo per `Create` e `Edit` metodi di azione per includere il `Rating` proprietà:</span><span class="sxs-lookup"><span data-stu-id="031ff-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="031ff-158">È necessario anche aggiornare i modelli di vista per visualizzare, creare e modificare la nuova proprietà `Rating` nella vista del browser.</span><span class="sxs-lookup"><span data-stu-id="031ff-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="031ff-159">Aprire il *\Views\Movies\Index.cshtml* file e aggiungere un `<th>Rating</th>` intestazione di colonna subito dopo il **prezzo** colonna.</span><span class="sxs-lookup"><span data-stu-id="031ff-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="031ff-160">Aggiungere quindi una `<td>` colonna verso la fine del modello per il rendering di `@item.Rating` valore.</span><span class="sxs-lookup"><span data-stu-id="031ff-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="031ff-161">Di seguito è riportato l'aggiornato *cshtml* modello di visualizzazione è simile a:</span><span class="sxs-lookup"><span data-stu-id="031ff-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="031ff-162">Aprire quindi il *\Views\Movies\Create.cshtml* file e aggiungere il `Rating` campo con il seguente markup highlighed.</span><span class="sxs-lookup"><span data-stu-id="031ff-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighed markup.</span></span> <span data-ttu-id="031ff-163">Si esegue il rendering di una casella di testo in modo che quando viene creato un nuovo filmato, è possibile specificare una classificazione.</span><span class="sxs-lookup"><span data-stu-id="031ff-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="031ff-164">Ora è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` proprietà.</span><span class="sxs-lookup"><span data-stu-id="031ff-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="031ff-165">Eseguire l'applicazione e passare il */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="031ff-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="031ff-166">Quando si esegue questa operazione, tuttavia, verrà visualizzato uno dei seguenti errori:</span><span class="sxs-lookup"><span data-stu-id="031ff-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="031ff-167">Il modello di esecuzione del backup il contesto 'MovieDBContext' è stato modificato dopo la creazione del database.</span><span class="sxs-lookup"><span data-stu-id="031ff-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="031ff-168">Provare a utilizzare migrazioni Code First per aggiornare il database (https://go.microsoft.com/fwlink/?LinkId=238269).</span><span class="sxs-lookup"><span data-stu-id="031ff-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="031ff-169">Viene visualizzato questo errore perché l'aggiornamento `Movie` classe del modello nell'applicazione è diverso rispetto allo schema di ora il `Movie` tabella del database esistente.</span><span class="sxs-lookup"><span data-stu-id="031ff-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="031ff-170">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="031ff-170">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="031ff-171">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="031ff-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="031ff-172">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="031ff-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="031ff-173">Questo approccio è molto utile nelle prime fasi del ciclo di sviluppo in una fase attiva di sviluppo di un database di test e consente di migliorare rapidamente lo schema del modello e il database insieme.</span><span class="sxs-lookup"><span data-stu-id="031ff-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="031ff-174">Lo svantaggio, tuttavia, è che si perdono i dati esistenti nel database, in modo si *non* per usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="031ff-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="031ff-175">Un modo efficace per sviluppare un'applicazione consiste nell'inizializzare automaticamente un database con dati di test usando un inizializzatore.</span><span class="sxs-lookup"><span data-stu-id="031ff-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="031ff-176">Per ulteriori informazioni sugli inizializzatori di database di Entity Framework, vedere [esercitazione ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="031ff-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="031ff-177">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="031ff-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="031ff-178">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="031ff-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="031ff-179">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="031ff-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="031ff-180">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="031ff-180">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="031ff-181">Per questa esercitazione si userà Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="031ff-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="031ff-182">Aggiornare il metodo di inizializzazione in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="031ff-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="031ff-183">Aprire il file Migrations\Configuration.cs e aggiungere un campo di classificazione per ogni oggetto del film.</span><span class="sxs-lookup"><span data-stu-id="031ff-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="031ff-184">Compilare la soluzione e quindi aprire il **Package Manager Console** finestra e immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="031ff-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="031ff-185">Il `add-migration` comando indica al framework di migrazione per esaminare il modello di film corrente con lo schema di film DB corrente e creare il codice necessario per eseguire la migrazione del database al nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="031ff-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="031ff-186">Il nome *classificazione* è arbitrario e viene utilizzata per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="031ff-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="031ff-187">È consigliabile utilizzare un nome significativo per il passaggio della migrazione.</span><span class="sxs-lookup"><span data-stu-id="031ff-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="031ff-188">Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMigration` classe derivata e il `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="031ff-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="031ff-189">Compilare la soluzione e quindi immettere il `update-database` comando il **Package Manager Console** finestra.</span><span class="sxs-lookup"><span data-stu-id="031ff-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="031ff-190">La figura seguente mostra l'output nel **Package Manager Console** finestra (il timbro data anteponendo *classificazione* sarà diverso.)</span><span class="sxs-lookup"><span data-stu-id="031ff-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="031ff-191">Eseguire nuovamente l'applicazione e passare all'URL /Movies.</span><span class="sxs-lookup"><span data-stu-id="031ff-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="031ff-192">È possibile visualizzare il nuovo campo di classificazione.</span><span class="sxs-lookup"><span data-stu-id="031ff-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="031ff-193">Fare clic su di **Crea nuovo** collegamento per aggiungere un nuovo film.</span><span class="sxs-lookup"><span data-stu-id="031ff-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="031ff-194">Si noti che è possibile aggiungere una classificazione.</span><span class="sxs-lookup"><span data-stu-id="031ff-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="031ff-196">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="031ff-196">Click **Create**.</span></span> <span data-ttu-id="031ff-197">Il nuovo filmato, tra cui la valutazione, viene inserito nei film elenco:</span><span class="sxs-lookup"><span data-stu-id="031ff-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="031ff-199">Ora che il progetto utilizza le migrazioni, non è necessario eliminare il database quando si aggiunge un nuovo campo o in caso contrario, aggiornare lo schema.</span><span class="sxs-lookup"><span data-stu-id="031ff-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="031ff-200">Nella sezione successiva, si sarà apportare ulteriori modifiche allo schema e utilizzare migrazioni per aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="031ff-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="031ff-201">È inoltre necessario aggiungere il `Rating` campo per i modelli di visualizzazione di modifica, eliminazione e dettagli.</span><span class="sxs-lookup"><span data-stu-id="031ff-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="031ff-202">È possibile immettere il comando "Aggiorna database" di **Package Manager Console** nuovamente finestra e nessun codice di migrazione potrebbe eseguire l'installazione perché lo schema corrisponde al modello.</span><span class="sxs-lookup"><span data-stu-id="031ff-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="031ff-203">Tuttavia, in esecuzione "Aggiorna database" verrà eseguito il `Seed` nuovamente, metodo e se è stato modificato, i dati del valore di inizializzazione, le modifiche andranno perse perché il `Seed` dati upserts metodo.</span><span class="sxs-lookup"><span data-stu-id="031ff-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="031ff-204">Ulteriori informazioni sul `Seed` metodo di Tom Dykstra diffuso [esercitazione ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="031ff-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="031ff-205">In questa sezione è stato illustrato come è possibile modificare gli oggetti del modello e sincronizzare il database con le modifiche.</span><span class="sxs-lookup"><span data-stu-id="031ff-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="031ff-206">È stato inoltre un modo per popolare un database appena creato con dati di esempio in modo è possibile provare gli scenari.</span><span class="sxs-lookup"><span data-stu-id="031ff-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="031ff-207">Questo è solo una rapida introduzione alle Code First, vedere [la creazione di un modello di dati di Entity Framework per un'applicazione MVC ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) per un'esercitazione più completa sull'oggetto.</span><span class="sxs-lookup"><span data-stu-id="031ff-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="031ff-208">Successivamente, si esaminerà come è possibile aggiungere la logica di convalida più completa per le classi di modello e abilitare alcune regole di business da applicare.</span><span class="sxs-lookup"><span data-stu-id="031ff-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="031ff-209">[Precedente](adding-search.md)
> [Successivo](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="031ff-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
