---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: "Introduzione a ASP.NET Web Pages: l'eliminazione dei dati di Database | Microsoft Docs"
author: Rick-Anderson
description: Questa esercitazione illustra come eliminare una voce di singoli database. Si presuppone di che aver completato la serie tramite l'aggiornamento di dati del Database nell'indirizzo PA Web di ASP.NET....
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: b2ef8fcc8cc534bd31fea83bf0b085b85995f417
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020442"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="5346c-104">Introduzione a pagine Web ASP.NET: l'eliminazione dei dati di Database</span><span class="sxs-lookup"><span data-stu-id="5346c-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>
====================
<span data-ttu-id="5346c-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5346c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5346c-106">Questa esercitazione illustra come eliminare una voce di singoli database.</span><span class="sxs-lookup"><span data-stu-id="5346c-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="5346c-107">Si presuppone di aver completato la serie attraverso [l'aggiornamento dei dati di un Database in ASP.NET Web Pages](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="5346c-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="5346c-108">Che cosa si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="5346c-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="5346c-109">Come selezionare un singolo record da un elenco di record.</span><span class="sxs-lookup"><span data-stu-id="5346c-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="5346c-110">Come eliminare un record singolo da un database.</span><span class="sxs-lookup"><span data-stu-id="5346c-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="5346c-111">Come verificare che un pulsante specifico è stato fatto clic in un form.</span><span class="sxs-lookup"><span data-stu-id="5346c-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="5346c-112">Le funzionalità/tecnologie illustrate:</span><span class="sxs-lookup"><span data-stu-id="5346c-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="5346c-113">Il `WebGrid` helper.</span><span class="sxs-lookup"><span data-stu-id="5346c-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="5346c-114">Il codice SQL `Delete` comando.</span><span class="sxs-lookup"><span data-stu-id="5346c-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="5346c-115">Il `Database.Execute` metodo per l'esecuzione di un database SQL `Delete` comando.</span><span class="sxs-lookup"><span data-stu-id="5346c-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="5346c-116">Scopo dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="5346c-116">What You'll Build</span></span>

<span data-ttu-id="5346c-117">Nell'esercitazione precedente, è stato descritto come aggiornare un record di database esistente.</span><span class="sxs-lookup"><span data-stu-id="5346c-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="5346c-118">Questa esercitazione è simile, ad eccezione del fatto che anziché aggiornare il record, è possibile eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="5346c-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="5346c-119">I processi sono molto simile, ad eccezione del fatto che l'eliminazione è più semplice, in modo che questa esercitazione saranno a breve.</span><span class="sxs-lookup"><span data-stu-id="5346c-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="5346c-120">Nel *film* pagina, si aggiornerà il `WebGrid` helper, in modo che venga visualizzato un **eliminare** collegamento accanto a ogni film di accompagnamento per il **modifica** collegamento aggiunto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="5346c-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Pagina Movies che mostra un collegamento di eliminazione per ogni film](deleting-data/_static/image1.png)

<span data-ttu-id="5346c-122">Come con la modifica, quando si sceglie la **eliminare** collegamento, consente a un'altra pagina, in cui le informazioni relative al filmato è già in un form:</span><span class="sxs-lookup"><span data-stu-id="5346c-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Elimina pagina film con un film visualizzato](deleting-data/_static/image2.png)

<span data-ttu-id="5346c-124">È quindi possibile fare clic sul pulsante per eliminare definitivamente il record.</span><span class="sxs-lookup"><span data-stu-id="5346c-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="5346c-125">Aggiunta di un collegamento di eliminazione per l'elenco di film</span><span class="sxs-lookup"><span data-stu-id="5346c-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="5346c-126">Si inizia aggiungendo un **eliminare** collegare le `WebGrid` helper.</span><span class="sxs-lookup"><span data-stu-id="5346c-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="5346c-127">Questo collegamento è simile al **modifica** collegamento è stato aggiunto nell'esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="5346c-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="5346c-128">Aprire il *Movies.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="5346c-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="5346c-129">Modifica il `WebGrid` markup nel corpo della pagina aggiungendo una colonna.</span><span class="sxs-lookup"><span data-stu-id="5346c-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="5346c-130">Ecco il markup modificato:</span><span class="sxs-lookup"><span data-stu-id="5346c-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="5346c-131">La nuova colonna è questa:</span><span class="sxs-lookup"><span data-stu-id="5346c-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="5346c-132">Il modo in cui la griglia è configurata, il **Edit** colonna è più a sinistra nella griglia e il **eliminare** colonna è più a destra.</span><span class="sxs-lookup"><span data-stu-id="5346c-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="5346c-133">(È presente una virgola dopo il `Year` colonna a questo punto, nel caso in cui non si noti che.) Non c'è niente di speciale in cui vengono inseriti queste colonne di collegamento ed è possibile inserire facilmente uno accanto a altro.</span><span class="sxs-lookup"><span data-stu-id="5346c-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="5346c-134">In questo caso, risultano separate per renderli più difficile da ottenere confuse.</span><span class="sxs-lookup"><span data-stu-id="5346c-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Pagina di film con collegamenti di modifica e dettagli contrassegnata per mostrare che non si tratta di uno accanto a altro](deleting-data/_static/image3.png)

<span data-ttu-id="5346c-136">La nuova colonna viene visualizzato un collegamento (`<a>` elemento) con il testo "Elimina".</span><span class="sxs-lookup"><span data-stu-id="5346c-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="5346c-137">La destinazione del collegamento (relativi `href` attributi) è il codice che in definitiva viene risolto simile a questo URL, con la `id` valore diverso per ogni film:</span><span class="sxs-lookup"><span data-stu-id="5346c-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="5346c-138">Questo collegamento verrà richiamato una pagina denominata *DeleteMovie* e passare l'ID del film selezionato.</span><span class="sxs-lookup"><span data-stu-id="5346c-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="5346c-139">Questa esercitazione non descritte in dettaglio come costruire il collegamento, in quanto è quasi identico al **Edit** collegamento dall'esercitazione precedente ([aggiornamento dei dati di un Database in ASP.NET Web Pages](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="5346c-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="5346c-140">Creazione della pagina Delete</span><span class="sxs-lookup"><span data-stu-id="5346c-140">Creating the Delete Page</span></span>

<span data-ttu-id="5346c-141">Ora è possibile creare la pagina che sarà la destinazione per il **eliminare** collegamento nella griglia.</span><span class="sxs-lookup"><span data-stu-id="5346c-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5346c-142">**Importante** la tecnica di selezionando prima un record da eliminare e quindi usando una pagina separata e un pulsante per confermare il processo è estremamente importante per la sicurezza.</span><span class="sxs-lookup"><span data-stu-id="5346c-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="5346c-143">Come già detto nelle esercitazioni precedenti, rendendo *eventuali* ordinamento della modifica al sito Web deve *sempre* essere eseguita tramite un modulo &mdash; , ovvero mediante un'operazione HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="5346c-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="5346c-144">Se si è stato possibile modificare il sito appena selezionando un collegamento (vale a dire usando un'operazione GET), le persone è stato possibile effettuare richieste semplici per il sito ed eliminare i dati.</span><span class="sxs-lookup"><span data-stu-id="5346c-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="5346c-145">Anche un agente di ricerca del motore di ricerca che indicizza il sito è stato possibile eliminare inavvertitamente dati solo dai collegamenti seguenti.</span><span class="sxs-lookup"><span data-stu-id="5346c-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="5346c-146">Quando l'app consente agli utenti di modificare un record, è necessario presentare all'utente il record per la modifica comunque.</span><span class="sxs-lookup"><span data-stu-id="5346c-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="5346c-147">Ma si potrebbe essere tentati di ignorare questo passaggio per l'eliminazione di un record.</span><span class="sxs-lookup"><span data-stu-id="5346c-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="5346c-148">Non ignorare questo passaggio, tuttavia.</span><span class="sxs-lookup"><span data-stu-id="5346c-148">Don't skip that step, though.</span></span> <span data-ttu-id="5346c-149">(È anche utile per gli utenti visualizzino il record e confermare che si vuole eliminare il record che sono destinate.)</span><span class="sxs-lookup"><span data-stu-id="5346c-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="5346c-150">In una serie di esercitazioni successive, verrà illustrato come aggiungere funzionalità di accesso in modo che un utente deve accedere prima di eliminare un record.</span><span class="sxs-lookup"><span data-stu-id="5346c-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>


<span data-ttu-id="5346c-151">Creare una pagina denominata *DeleteMovie.cshtml* e sostituire quello presente nel file con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="5346c-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="5346c-152">Questo markup è simile al *EditMovie* pagine, con la differenza che invece di usare le caselle di testo (`<input type="text">`), incluso il markup `<span>` elementi.</span><span class="sxs-lookup"><span data-stu-id="5346c-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="5346c-153">Non è niente qui da modificare.</span><span class="sxs-lookup"><span data-stu-id="5346c-153">There's nothing here to edit.</span></span> <span data-ttu-id="5346c-154">Tutto è necessario eseguire è di visualizzare i dettagli del film in modo che gli utenti possono assicurarsi che si vuole eliminare il film a destra.</span><span class="sxs-lookup"><span data-stu-id="5346c-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="5346c-155">Il markup contiene già un collegamento che consente all'utente di tornare alla pagina di elenco di film.</span><span class="sxs-lookup"><span data-stu-id="5346c-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="5346c-156">Ad esempio la *EditMovie* pagina, l'ID del film selezionato viene archiviato in un campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="5346c-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="5346c-157">(Viene passato nella pagina in primo luogo come valore stringa di query.) È presente un `Html.ValidationSummary` chiamata che verrà visualizzati gli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="5346c-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="5346c-158">In questo caso, l'errore potrebbe essere che nessun ID del film è stato passato alla pagina o che l'ID del film non è valido.</span><span class="sxs-lookup"><span data-stu-id="5346c-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="5346c-159">Questa situazione può verificarsi se un utente è stata eseguita questa pagina senza selezionare prima un film nel *film* pagina.</span><span class="sxs-lookup"><span data-stu-id="5346c-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="5346c-160">Didascalia del pulsante viene **eliminare film**, e l'attributo name è impostato su `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="5346c-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="5346c-161">Il `name` attributo verrà utilizzato nel codice per identificare il pulsante di invio del modulo.</span><span class="sxs-lookup"><span data-stu-id="5346c-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="5346c-162">È possibile scrivere codice per 1) leggere i dettagli del film quando la pagina viene visualizzata prima di tutto e 2) effettivamente eliminare il film quando l'utente fa clic sul pulsante.</span><span class="sxs-lookup"><span data-stu-id="5346c-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="5346c-163">Aggiunta di codice per leggere un singolo filmato</span><span class="sxs-lookup"><span data-stu-id="5346c-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="5346c-164">In cima il *DeleteMovie.cshtml* pagina, aggiungere il blocco di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="5346c-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="5346c-165">Questo markup è quello utilizzato per il codice corrispondente nel *EditMovie* pagina.</span><span class="sxs-lookup"><span data-stu-id="5346c-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="5346c-166">Ottiene l'ID del film all'esterno di stringa di query e Usa l'ID per leggere un record dal database.</span><span class="sxs-lookup"><span data-stu-id="5346c-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="5346c-167">Il codice include il test di convalida (`IsInt()` e `row != null`) per assicurarsi che l'ID del film viene passata alla pagina sia valido.</span><span class="sxs-lookup"><span data-stu-id="5346c-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="5346c-168">Tenere presente che questo codice deve essere eseguito solo alla prima che esecuzione della pagina.</span><span class="sxs-lookup"><span data-stu-id="5346c-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="5346c-169">Non si vuole leggere nuovamente il record di film dal database quando l'utente sceglie il **film eliminare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="5346c-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="5346c-170">Pertanto, il codice per leggere il film si trova all'interno di un test con la dicitura `if(!IsPost)` &mdash; vale a dire *se la richiesta non è un'operazione post (invio del modulo)*.</span><span class="sxs-lookup"><span data-stu-id="5346c-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="5346c-171">Aggiunta di codice per eliminare il film selezionato</span><span class="sxs-lookup"><span data-stu-id="5346c-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="5346c-172">Per eliminare il film quando l'utente fa clic sul pulsante, aggiungere il codice seguente all'interno di parentesi graffa di chiusura di `@` blocco:</span><span class="sxs-lookup"><span data-stu-id="5346c-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="5346c-173">Questo codice è simile al codice per l'aggiornamento di un record esistente, ma più semplice.</span><span class="sxs-lookup"><span data-stu-id="5346c-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="5346c-174">Il codice viene eseguito essenzialmente un SQL `Delete` istruzione.</span><span class="sxs-lookup"><span data-stu-id="5346c-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="5346c-175">Come nel *EditMovie* pagina, il codice si trova in un `if(IsPost)` blocco.</span><span class="sxs-lookup"><span data-stu-id="5346c-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="5346c-176">Questa volta il `if()` condizione è un po' più complicata:</span><span class="sxs-lookup"><span data-stu-id="5346c-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="5346c-177">Esistono due condizioni qui.</span><span class="sxs-lookup"><span data-stu-id="5346c-177">There are two conditions here.</span></span> <span data-ttu-id="5346c-178">Il primo è che viene inviata la pagina, come si è visto prima &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="5346c-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="5346c-179">La seconda condizione è `!Request["buttonDelete"].IsEmpty()`, vale a dire che la richiesta include un oggetto denominato `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="5346c-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="5346c-180">È vero, è un metodo indiretto d' quale pulsante di invio del modulo di test.</span><span class="sxs-lookup"><span data-stu-id="5346c-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="5346c-181">Se un modulo contiene più pulsanti di invio, viene visualizzato solo il nome del pulsante su cui è stato fatto clic nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="5346c-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="5346c-182">Pertanto, in modo logico, se il nome di un particolare pulsante viene visualizzato nella richiesta &mdash; o come indicato nel codice, se tale pulsante non è vuoto &mdash; che corrisponde al pulsante che ha inviato il form.</span><span class="sxs-lookup"><span data-stu-id="5346c-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="5346c-183">Il `&&` operatore significa "e" (AND logico).</span><span class="sxs-lookup"><span data-stu-id="5346c-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="5346c-184">Di conseguenza l'intera `if` condizione è...</span><span class="sxs-lookup"><span data-stu-id="5346c-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="5346c-185">*Questa richiesta è una richiesta post (non una richiesta di primo)*</span><span class="sxs-lookup"><span data-stu-id="5346c-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="5346c-186">AND</span><span class="sxs-lookup"><span data-stu-id="5346c-186">AND</span></span>  
  
<span data-ttu-id="5346c-187">*Il* `buttonDelete` *pulsante si tratta del pulsante che ha inviato il form.*</span><span class="sxs-lookup"><span data-stu-id="5346c-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="5346c-188">Questo modulo (in effetti, questa pagina) contiene un solo pulsante, in modo che il test aggiuntivo per `buttonDelete` tecnicamente non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="5346c-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="5346c-189">Tuttavia, si sta per eseguire un'operazione che verrà rimossi in modo permanente i dati.</span><span class="sxs-lookup"><span data-stu-id="5346c-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="5346c-190">Pertanto, si desidera come assicurarsi che si sta eseguendo l'operazione solo quando l'utente ha richiesto in modo esplicito viene possibile.</span><span class="sxs-lookup"><span data-stu-id="5346c-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="5346c-191">Si supponga, ad esempio, è espanso in un secondo momento in questa pagina e aggiungervi altri pulsanti.</span><span class="sxs-lookup"><span data-stu-id="5346c-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="5346c-192">Anche in questo caso il codice che elimina il film verrà eseguito solo se il `buttonDelete` è stato fatto clic sul pulsante.</span><span class="sxs-lookup"><span data-stu-id="5346c-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="5346c-193">Ad esempio la *EditMovie* pagina, ottenere l'ID del campo nascosto e quindi eseguire il comando SQL.</span><span class="sxs-lookup"><span data-stu-id="5346c-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="5346c-194">La sintassi per il `Delete` istruzione è:</span><span class="sxs-lookup"><span data-stu-id="5346c-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="5346c-195">È fondamentale per includere il `WHERE` clausola e l'ID.</span><span class="sxs-lookup"><span data-stu-id="5346c-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="5346c-196">Se si omette la clausola WHERE *verranno eliminati tutti i record nella tabella*.</span><span class="sxs-lookup"><span data-stu-id="5346c-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="5346c-197">Come si è visto, il valore ID è passare al comando SQL utilizzando un segnaposto.</span><span class="sxs-lookup"><span data-stu-id="5346c-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="5346c-198">Testare il processo di eliminazione di film</span><span class="sxs-lookup"><span data-stu-id="5346c-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="5346c-199">A questo punto è possibile testare.</span><span class="sxs-lookup"><span data-stu-id="5346c-199">Now you can test.</span></span> <span data-ttu-id="5346c-200">Eseguire la *film* e fare clic su **eliminare** accanto a un filmato.</span><span class="sxs-lookup"><span data-stu-id="5346c-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="5346c-201">Quando la *DeleteMovie* verrà visualizzata la pagina, fare clic su **eliminare film**.</span><span class="sxs-lookup"><span data-stu-id="5346c-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Elimina pagina film con il pulsante Elimina film evidenziato](deleting-data/_static/image4.png)

<span data-ttu-id="5346c-203">Quando si fa clic sul pulsante, il codice elimina il film e restituisce l'elenco di film.</span><span class="sxs-lookup"><span data-stu-id="5346c-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="5346c-204">Non esiste è possibile cercare il film eliminato e verificare che è stata eliminata.</span><span class="sxs-lookup"><span data-stu-id="5346c-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="5346c-205">In arrivo</span><span class="sxs-lookup"><span data-stu-id="5346c-205">Coming Up Next</span></span>

<span data-ttu-id="5346c-206">L'esercitazione successiva illustra come concedere a tutte le pagine nel sito di un aspetto comune e il layout.</span><span class="sxs-lookup"><span data-stu-id="5346c-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="5346c-207">Elenco completo per la pagina di film (aggiornata con collegamenti di eliminazione)</span><span class="sxs-lookup"><span data-stu-id="5346c-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="5346c-208">Elenco completo per la pagina DeleteMovie</span><span class="sxs-lookup"><span data-stu-id="5346c-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="5346c-209">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5346c-209">Additional Resources</span></span>

- [<span data-ttu-id="5346c-210">Introduzione alla programmazione Web ASP.NET usando la sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="5346c-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="5346c-211">[Istruzione SQL DELETE](http://www.w3schools.com/sql/sql_delete.asp) sul sito W3Schools</span><span class="sxs-lookup"><span data-stu-id="5346c-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5346c-212">[Precedente](updating-data.md)
> [Successivo](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="5346c-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
