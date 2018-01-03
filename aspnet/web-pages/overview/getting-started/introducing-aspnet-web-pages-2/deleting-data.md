---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: 'Introduzione a ASP.NET Web Pages: eliminazione di dati di Database | Documenti Microsoft'
author: tfitzmac
description: In questa esercitazione viene illustrato come eliminare una voce di singoli database. Si presuppone di che aver completato la serie tramite l'aggiornamento di dati di Database in PA Web di ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 5bc92b5d40e7a55dcd730d552c71031d913b277e
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2018
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="0da45-104">Introduzione a ASP.NET Web Pages: eliminazione di dati di Database</span><span class="sxs-lookup"><span data-stu-id="0da45-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="0da45-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0da45-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0da45-106">In questa esercitazione viene illustrato come eliminare una voce di singoli database.</span><span class="sxs-lookup"><span data-stu-id="0da45-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="0da45-107">Si presuppone di aver completato la serie tramite [aggiornamento dei dati del Database in ASP.NET Web Pages](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="0da45-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="0da45-108">Illustra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="0da45-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="0da45-109">Come selezionare un singolo record da un elenco di record.</span><span class="sxs-lookup"><span data-stu-id="0da45-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="0da45-110">Come eliminare un singolo record da un database.</span><span class="sxs-lookup"><span data-stu-id="0da45-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="0da45-111">Come verificare che un pulsante specifico è stato fatto clic in un form.</span><span class="sxs-lookup"><span data-stu-id="0da45-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="0da45-112">Funzionalità/tecnologie descritte:</span><span class="sxs-lookup"><span data-stu-id="0da45-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="0da45-113">Il `WebGrid` helper.</span><span class="sxs-lookup"><span data-stu-id="0da45-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="0da45-114">L'istruzione SQL `Delete` comando.</span><span class="sxs-lookup"><span data-stu-id="0da45-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="0da45-115">Il `Database.Execute` metodo per l'esecuzione di un database SQL `Delete` comando.</span><span class="sxs-lookup"><span data-stu-id="0da45-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="0da45-116">Cosa Compilerai</span><span class="sxs-lookup"><span data-stu-id="0da45-116">What You'll Build</span></span>

<span data-ttu-id="0da45-117">Nell'esercitazione precedente, è stato descritto come aggiornare un record di database esistente.</span><span class="sxs-lookup"><span data-stu-id="0da45-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="0da45-118">In questa esercitazione è simile, ad eccezione del fatto che invece di aggiornare il record, verranno eliminati.</span><span class="sxs-lookup"><span data-stu-id="0da45-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="0da45-119">I processi sono in modo analogo, ad eccezione del fatto che l'eliminazione è più semplice, in modo che sia breve in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="0da45-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="0da45-120">Nel *filmati* pagina verrà aggiornata la `WebGrid` helper in modo che venga visualizzato un **eliminare** collegamento accanto a ogni film di accompagnamento per il **modifica** collegamento aggiunto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="0da45-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Pagina di filmati che mostra un collegamento di eliminazione per ogni film](deleting-data/_static/image1.png)

<span data-ttu-id="0da45-122">Come con la modifica, quando si sceglie il **eliminare** collegamento, consente di visualizzare una pagina diversa, in cui le informazioni di film sono già in un formato:</span><span class="sxs-lookup"><span data-stu-id="0da45-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Elimina pagina film con un filmato visualizzato](deleting-data/_static/image2.png)

<span data-ttu-id="0da45-124">È quindi possibile fare clic sul pulsante per eliminare definitivamente il record.</span><span class="sxs-lookup"><span data-stu-id="0da45-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="0da45-125">Aggiunta di un collegamento di eliminazione per l'elenco di film</span><span class="sxs-lookup"><span data-stu-id="0da45-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="0da45-126">Iniziare aggiungendo un **eliminare** collegare il `WebGrid` helper.</span><span class="sxs-lookup"><span data-stu-id="0da45-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="0da45-127">Questo collegamento è simile al **modifica** collegamento è stato aggiunto in un'esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="0da45-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="0da45-128">Aprire il *Movies.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="0da45-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="0da45-129">Modifica il `WebGrid` markup nel corpo della pagina aggiungendo una colonna.</span><span class="sxs-lookup"><span data-stu-id="0da45-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="0da45-130">Di seguito è riportato il codice modificato:</span><span class="sxs-lookup"><span data-stu-id="0da45-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="0da45-131">Questa è la nuova colonna:</span><span class="sxs-lookup"><span data-stu-id="0da45-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="0da45-132">La configurazione della griglia, il **modifica** colonna è più a sinistra nella griglia e **eliminare** colonna è più a destra.</span><span class="sxs-lookup"><span data-stu-id="0da45-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="0da45-133">(È presente una virgola dopo il `Year` colonna ora, nel caso in cui non si noti che.) Non c'è niente di speciale su esiti di queste colonne di collegamento e, è possibile inserire facilmente uno accanto a altro.</span><span class="sxs-lookup"><span data-stu-id="0da45-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="0da45-134">In questo caso, ma sono separati in modo da renderle più difficile ottenere confuse.</span><span class="sxs-lookup"><span data-stu-id="0da45-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Pagina di filmati con collegamenti di modifica e dettagli contrassegnata per mostrare che non sono uno accanto a altro](deleting-data/_static/image3.png)

<span data-ttu-id="0da45-136">La nuova colonna viene illustrato un collegamento (`<a>` elemento) con il testo "Delete".</span><span class="sxs-lookup"><span data-stu-id="0da45-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="0da45-137">La destinazione del collegamento (relativo `href` attributo) è il codice che alla fine si risolve in un risultato simile a questo URL, con la `id` valore diverso per ogni film:</span><span class="sxs-lookup"><span data-stu-id="0da45-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="0da45-138">Questo collegamento verrà richiamato una pagina denominata *DeleteMovie* e passare l'ID del filmato selezionata.</span><span class="sxs-lookup"><span data-stu-id="0da45-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="0da45-139">In questa esercitazione non descritte in dettaglio come questo collegamento viene costruito, perché è quasi identica a quella di **modifica** collegamento dall'esercitazione precedente ([aggiornamento dei dati del Database in ASP.NET Web Pages](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="0da45-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="0da45-140">Creazione pagina di eliminazione</span><span class="sxs-lookup"><span data-stu-id="0da45-140">Creating the Delete Page</span></span>

<span data-ttu-id="0da45-141">Ora è possibile creare la pagina che sarà la destinazione per il **eliminare** collegamento nella griglia.</span><span class="sxs-lookup"><span data-stu-id="0da45-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="0da45-142">**Importante** la tecnica di selezionando prima un record da eliminare e quindi utilizzando una pagina separata e un pulsante per verificare il processo è estremamente importante per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="0da45-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="0da45-143">Come è stato letto esercitazioni precedenti, rendendo *qualsiasi* tipo di modifica per il sito Web deve *sempre* essere eseguita tramite un modulo &mdash; , ovvero tramite un'operazione HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="0da45-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="0da45-144">Se si è stato possibile modificare il sito semplicemente facendo clic su un collegamento (con un'operazione GET), utenti potrebbero apportare semplici richieste al sito ed eliminare i dati.</span><span class="sxs-lookup"><span data-stu-id="0da45-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="0da45-145">Anche un crawler di motore di ricerca che l'indicizzazione del sito è stato possibile eliminare accidentalmente dati solo per i collegamenti seguenti.</span><span class="sxs-lookup"><span data-stu-id="0da45-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="0da45-146">Quando l'app consente a persone di modificare un record, è necessario presentare all'utente per la modifica comunque il record.</span><span class="sxs-lookup"><span data-stu-id="0da45-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="0da45-147">Ma potrebbe essere tentati di ignorare questo passaggio per l'eliminazione di un record.</span><span class="sxs-lookup"><span data-stu-id="0da45-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="0da45-148">Non ignorare questo passaggio, tuttavia.</span><span class="sxs-lookup"><span data-stu-id="0da45-148">Don't skip that step, though.</span></span> <span data-ttu-id="0da45-149">(È anche utile per gli utenti di visualizzare il record e confermare che si vuole eliminare il record che devono.)</span><span class="sxs-lookup"><span data-stu-id="0da45-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="0da45-150">In un set di esercitazioni successive, si noterà come aggiungere funzionalità di accesso in modo che un utente deve accedere prima di eliminare un record.</span><span class="sxs-lookup"><span data-stu-id="0da45-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="0da45-151">Creare una pagina denominata *DeleteMovie.cshtml* e sostituire quello presente nel file con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="0da45-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="0da45-152">Questo tag è simile al *EditMovie* pagine, con la differenza che anziché utilizzare le caselle di testo (`<input type="text">`), incluso il markup `<span>` elementi.</span><span class="sxs-lookup"><span data-stu-id="0da45-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="0da45-153">Non è seguito da modificare.</span><span class="sxs-lookup"><span data-stu-id="0da45-153">There's nothing here to edit.</span></span> <span data-ttu-id="0da45-154">Sarà sufficiente è di visualizzare i dettagli del film in modo che gli utenti possono assicurarsi elimini i film destra.</span><span class="sxs-lookup"><span data-stu-id="0da45-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="0da45-155">Il markup contiene già un collegamento che consente all'utente di tornare alla pagina di elenco di film.</span><span class="sxs-lookup"><span data-stu-id="0da45-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="0da45-156">Ad esempio il *EditMovie* pagina, l'ID del filmato selezionato viene archiviato in un campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="0da45-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="0da45-157">(Viene passato alla pagina in primo luogo come un valore di stringa di query.) È presente un `Html.ValidationSummary` chiamata che consente di visualizzare gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="0da45-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="0da45-158">In questo caso, l'errore potrebbe essere che nessun ID film è stato passato alla pagina o che l'ID del film non è valido.</span><span class="sxs-lookup"><span data-stu-id="0da45-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="0da45-159">Questa situazione può verificarsi se un utente è stato eseguito in questa pagina senza selezionare un filmato nel *filmati* pagina.</span><span class="sxs-lookup"><span data-stu-id="0da45-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="0da45-160">Didascalia del pulsante è **eliminare film**, e il relativo nome di attributo è impostato su `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="0da45-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="0da45-161">Il `name` verrà utilizzato l'attributo nel codice per identificare il pulsante di invio del modulo.</span><span class="sxs-lookup"><span data-stu-id="0da45-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="0da45-162">È necessario scrivere codice per 1) leggere i dettagli del film quando la pagina viene visualizzata prima e 2) le elimina il filmato quando l'utente fa clic sul pulsante.</span><span class="sxs-lookup"><span data-stu-id="0da45-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="0da45-163">Aggiungere il codice per leggere un singolo filmato</span><span class="sxs-lookup"><span data-stu-id="0da45-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="0da45-164">Nella parte superiore del *DeleteMovie.cshtml* pagina, aggiungere il blocco di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0da45-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="0da45-165">In questo markup corrisponde al codice corrispondente nella *EditMovie* pagina.</span><span class="sxs-lookup"><span data-stu-id="0da45-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="0da45-166">Ottiene l'ID film fuori la stringa di query e Usa l'ID per leggere un record dal database.</span><span class="sxs-lookup"><span data-stu-id="0da45-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="0da45-167">Il codice include il test di convalida (`IsInt()` e `row != null`) per assicurarsi che l'ID di film passato alla pagina è valido.</span><span class="sxs-lookup"><span data-stu-id="0da45-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="0da45-168">Tenere presente che questo codice deve essere eseguito solo alla prima che esecuzione della pagina.</span><span class="sxs-lookup"><span data-stu-id="0da45-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="0da45-169">Non si desidera leggere nuovamente il record di film dal database quando l'utente sceglie il **eliminare film** pulsante.</span><span class="sxs-lookup"><span data-stu-id="0da45-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="0da45-170">Pertanto, il codice per leggere il film si trova all'interno di un test che indica che `if(!IsPost)` &mdash; , ovvero *se la richiesta non è un'operazione post (invio)*.</span><span class="sxs-lookup"><span data-stu-id="0da45-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="0da45-171">Aggiungere il codice per eliminare il filmato selezionato</span><span class="sxs-lookup"><span data-stu-id="0da45-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="0da45-172">Per eliminare il filmato quando l'utente fa clic sul pulsante, aggiungere il seguente codice all'interno di parentesi graffa di chiusura di `@` blocco:</span><span class="sxs-lookup"><span data-stu-id="0da45-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="0da45-173">Questo codice è simile al codice per l'aggiornamento di un record esistente, ma è più semplice.</span><span class="sxs-lookup"><span data-stu-id="0da45-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="0da45-174">In pratica viene eseguito il codice SQL `Delete` istruzione.</span><span class="sxs-lookup"><span data-stu-id="0da45-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="0da45-175">Ad esempio il *EditMovie* pagina, il codice è in un `if(IsPost)` blocco.</span><span class="sxs-lookup"><span data-stu-id="0da45-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="0da45-176">Questa volta il `if()` condizione è leggermente più complessa:</span><span class="sxs-lookup"><span data-stu-id="0da45-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="0da45-177">Esistono due condizioni.</span><span class="sxs-lookup"><span data-stu-id="0da45-177">There are two conditions here.</span></span> <span data-ttu-id="0da45-178">Il primo è che la pagina viene inviata, come si è visto prima &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="0da45-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="0da45-179">È la seconda condizione `!Request["buttonDelete"].IsEmpty()`, vale a dire che la richiesta include un oggetto denominato `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="0da45-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="0da45-180">È vero, è un modo indiretto del test il pulsante di invio del modulo.</span><span class="sxs-lookup"><span data-stu-id="0da45-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="0da45-181">Se un modulo contiene più pulsanti di invio, viene visualizzato solo il nome del pulsante su cui è stato fatto clic nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="0da45-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="0da45-182">Pertanto, in modo logico, se il nome di un pulsante viene visualizzato nella richiesta &mdash; o come indicato nel codice, se tale pulsante non è vuota &mdash; che è il pulsante di invio del modulo.</span><span class="sxs-lookup"><span data-stu-id="0da45-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="0da45-183">Il `&&` operatore significa "e" (AND logico).</span><span class="sxs-lookup"><span data-stu-id="0da45-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="0da45-184">Pertanto l'intero `if` condizione è...</span><span class="sxs-lookup"><span data-stu-id="0da45-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="0da45-185">*Questa richiesta è una richiesta post (non una richiesta prima)*</span><span class="sxs-lookup"><span data-stu-id="0da45-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="0da45-186">AND</span><span class="sxs-lookup"><span data-stu-id="0da45-186">AND</span></span>  
  
<span data-ttu-id="0da45-187">*Il* `buttonDelete` *pulsante è il pulsante di invio del modulo.*</span><span class="sxs-lookup"><span data-stu-id="0da45-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="0da45-188">Questo modulo (in effetti, questa pagina) contiene un solo pulsante, pertanto, il test per `buttonDelete` tecnicamente non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="0da45-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="0da45-189">Tuttavia, si sta per eseguire un'operazione che rimuove in modo permanente i dati.</span><span class="sxs-lookup"><span data-stu-id="0da45-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="0da45-190">Pertanto, si desidera come assicurarsi che si sta eseguendo l'operazione solo quando l'utente ha richiesto in modo esplicito possibile.</span><span class="sxs-lookup"><span data-stu-id="0da45-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="0da45-191">Si supponga, ad esempio, che espanso in un secondo momento questa pagina e aggiungere altri pulsanti.</span><span class="sxs-lookup"><span data-stu-id="0da45-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="0da45-192">Anche in questo caso, il codice che elimina il filmato verrà eseguita solo se il `buttonDelete` è stato fatto clic sul pulsante.</span><span class="sxs-lookup"><span data-stu-id="0da45-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="0da45-193">Ad esempio il *EditMovie* pagina, ottenere l'ID del campo nascosto e quindi eseguire il comando SQL.</span><span class="sxs-lookup"><span data-stu-id="0da45-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="0da45-194">La sintassi per la `Delete` istruzione:</span><span class="sxs-lookup"><span data-stu-id="0da45-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="0da45-195">È importante includere la `WHERE` clausola e l'ID.</span><span class="sxs-lookup"><span data-stu-id="0da45-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="0da45-196">Se si omette la clausola WHERE, *verranno eliminati tutti i record nella tabella*.</span><span class="sxs-lookup"><span data-stu-id="0da45-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="0da45-197">Come si è visto, passare il valore ID per il comando SQL utilizzando un segnaposto.</span><span class="sxs-lookup"><span data-stu-id="0da45-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="0da45-198">Verifica il processo di eliminazione del film</span><span class="sxs-lookup"><span data-stu-id="0da45-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="0da45-199">Ora è possibile eseguire il test.</span><span class="sxs-lookup"><span data-stu-id="0da45-199">Now you can test.</span></span> <span data-ttu-id="0da45-200">Eseguire il *filmati* pagina e fare clic su **eliminare** accanto a un filmato.</span><span class="sxs-lookup"><span data-stu-id="0da45-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="0da45-201">Quando il *DeleteMovie* verrà visualizzata la pagina, fare clic su **eliminare film**.</span><span class="sxs-lookup"><span data-stu-id="0da45-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Elimina pagina film con il pulsante Elimina film evidenziato](deleting-data/_static/image4.png)

<span data-ttu-id="0da45-203">Quando si fa clic sul pulsante, il codice consente di eliminare i film e restituisce l'elenco di film.</span><span class="sxs-lookup"><span data-stu-id="0da45-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="0da45-204">Non esiste è possibile cercare il film eliminato e confermare che è stato eliminato.</span><span class="sxs-lookup"><span data-stu-id="0da45-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="0da45-205">Esercitazione seguente</span><span class="sxs-lookup"><span data-stu-id="0da45-205">Coming Up Next</span></span>

<span data-ttu-id="0da45-206">L'esercitazione successiva viene illustrato come assegnare tutte le pagine nel sito aspetto e un layout comuni.</span><span class="sxs-lookup"><span data-stu-id="0da45-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="0da45-207">Elenco completo per la pagina di film (aggiornata con collegamenti Elimina)</span><span class="sxs-lookup"><span data-stu-id="0da45-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="0da45-208">Elenco completo per la pagina DeleteMovie</span><span class="sxs-lookup"><span data-stu-id="0da45-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="0da45-209">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0da45-209">Additional Resources</span></span>

- [<span data-ttu-id="0da45-210">Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="0da45-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="0da45-211">[Istruzione DELETE SQL](http://www.w3schools.com/sql/sql_delete.asp) nel sito W3Schools</span><span class="sxs-lookup"><span data-stu-id="0da45-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0da45-212">[Precedente](updating-data.md)
[Successivo](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="0da45-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
