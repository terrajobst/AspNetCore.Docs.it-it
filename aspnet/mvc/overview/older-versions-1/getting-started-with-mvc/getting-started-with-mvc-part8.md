---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Aggiunta di una colonna al modello. | Microsoft Docs
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 22a6c4e5a07e81d5876cc442e68926094e3a243d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831463"
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="03466-104">Aggiunta di una colonna al modello</span><span class="sxs-lookup"><span data-stu-id="03466-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="03466-105">da [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="03466-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="03466-106">Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="03466-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="03466-107">Si creerà una semplice applicazione web che legge e scrive da un database.</span><span class="sxs-lookup"><span data-stu-id="03466-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="03466-108">Visitare il [centro di formazione di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.</span><span class="sxs-lookup"><span data-stu-id="03466-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="03466-109">In questa sezione verrà illustrato come è possibile apportare modifiche allo schema del database e gestire le modifiche all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="03466-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="03466-110">Aggiungere una colonna "Rating" alla tabella dei film.</span><span class="sxs-lookup"><span data-stu-id="03466-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="03466-111">Tornare all'IDE e fare clic su Esplora Database.</span><span class="sxs-lookup"><span data-stu-id="03466-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="03466-112">Fare clic con il pulsante destro tabella Movie e selezionare Apri definizione tabella.</span><span class="sxs-lookup"><span data-stu-id="03466-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="03466-113">Aggiungere una colonna di "Rating" come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="03466-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="03466-114">Poiché ora non c'è alcun classificazioni, è possibile la colonna ammette valori null.</span><span class="sxs-lookup"><span data-stu-id="03466-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="03466-115">Fare clic su Salva.</span><span class="sxs-lookup"><span data-stu-id="03466-115">Click Save.</span></span>

<span data-ttu-id="03466-116">[![Modifica tabella film](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="03466-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="03466-117">Successivamente, tornare a Esplora soluzioni e aprire il file Movies.edmx (che si trova nella cartella \Models).</span><span class="sxs-lookup"><span data-stu-id="03466-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="03466-118">Fare clic con il pulsante destro sull'area di progettazione (area bianca) e selezionare il modello di aggiornamento dal Database.</span><span class="sxs-lookup"><span data-stu-id="03466-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="03466-119">[![Film - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="03466-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="03466-120">Verrà avviata la "procedura guidata Aggiorna".</span><span class="sxs-lookup"><span data-stu-id="03466-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="03466-121">Scegliere la scheda aggiornamento all'interno di esso e fare clic su Fine.</span><span class="sxs-lookup"><span data-stu-id="03466-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="03466-122">La classe del modello Movie verrà quindi aggiornata con la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="03466-122">Our Movie model class will then be updated with the new column.</span></span>

![Aggiornamento guidato (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="03466-124">Dopo aver fatto clic su Fine, è possibile visualizzare che la nuova colonna Rating è stata aggiunta all'entità film nel modello.</span><span class="sxs-lookup"><span data-stu-id="03466-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="03466-125">[![Entità film](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="03466-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="03466-126">È stata aggiunta una colonna nel modello di database, ma le visualizzazioni non sa su di esso.</span><span class="sxs-lookup"><span data-stu-id="03466-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="03466-127">Aggiornare le viste con le modifiche al modello</span><span class="sxs-lookup"><span data-stu-id="03466-127">Update Views with Model Changes</span></span>

<span data-ttu-id="03466-128">Esistono alcuni modi è stato possibile aggiornare i modelli di visualizzazione in modo da riflettere la nuova colonna di punteggio.</span><span class="sxs-lookup"><span data-stu-id="03466-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="03466-129">Poiché abbiamo creato queste visualizzazioni da generarli tramite la finestra di dialogo Aggiungi visualizzazione, è possibile eliminarli e ricrearli nuovamente.</span><span class="sxs-lookup"><span data-stu-id="03466-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="03466-130">Tuttavia, in genere le persone saranno già state apportate delle modifiche ai propri modelli di visualizzazione dalla generazione iniziale sottoposto a scaffolding e saranno necessario aggiungere o eliminare i campi manualmente, esattamente come è stato fatto con il campo ID per la creazione.</span><span class="sxs-lookup"><span data-stu-id="03466-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="03466-131">Aprire il modello di \Views\Movies\Index.aspx e aggiungere una &lt;th&gt;Rating&lt;/th&gt; nell'intestazione della tabella Movie.</span><span class="sxs-lookup"><span data-stu-id="03466-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="03466-132">È stato aggiunto il mio dopo Genre.</span><span class="sxs-lookup"><span data-stu-id="03466-132">I added mine after Genre.</span></span> <span data-ttu-id="03466-133">Quindi, nella stessa posizione di colonna, ma basso, aggiungere una riga per restituire il nuovo controllo Rating.</span><span class="sxs-lookup"><span data-stu-id="03466-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="03466-134">Il modello di index. aspx finale avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="03466-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="03466-135">È possibile quindi aprire il modello di \Views\Movies\Create.aspx e aggiungere un'etichetta e una casella di testo per la nuova proprietà di classificazione:</span><span class="sxs-lookup"><span data-stu-id="03466-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="03466-136">Il modello di tornarci finale sarà simile al seguente e modificare titolo e replica secondaria del browser &lt;h2&gt; title su un valore come "Creazione di un film" mentre ci troviamo qui!</span><span class="sxs-lookup"><span data-stu-id="03466-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="03466-137">Eseguire l'app e a questo punto si ha un nuovo campo nel database che è stato aggiunto alla pagina Create.</span><span class="sxs-lookup"><span data-stu-id="03466-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="03466-138">Aggiungere un nuovo film - questa volta con una classificazione uguale a - e fare clic su Crea.</span><span class="sxs-lookup"><span data-stu-id="03466-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="03466-139">[![Creare un filmato - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="03466-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="03466-140">Dopo che si fa clic su Crea, si viene reindirizzati alla pagina di indice dove si nuovo film sia elencato con la nuova colonna di classificazione nel database</span><span class="sxs-lookup"><span data-stu-id="03466-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="03466-141">[![Elenco di film - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="03466-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="03466-142">Questa esercitazione di base tutto quello che ti a rendere i controller, associandole con viste e passano dati hardcoded.</span><span class="sxs-lookup"><span data-stu-id="03466-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="03466-143">Quindi viene creato e progettato un Database e inserire alcuni dati in.</span><span class="sxs-lookup"><span data-stu-id="03466-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="03466-144">Abbiamo i dati recuperati dal database e viene visualizzato in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="03466-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="03466-145">Quindi è stato aggiunto un modulo di creazione che consentono di aggiungere dati al database se stessi da all'interno dell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="03466-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="03466-146">Abbiamo aggiunto la convalida, verrà visualizzata la convalida usare JavaScript sul lato client.</span><span class="sxs-lookup"><span data-stu-id="03466-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="03466-147">Infine, abbiamo modificato il database in modo da includere una nuova colonna di dati e successivamente aggiornare alle due pagine per creare e visualizzare questi nuovi dati.</span><span class="sxs-lookup"><span data-stu-id="03466-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="03466-148">Ora ti suggeriamo per proseguire con l'esercitazione di livello intermedio "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", nonché i video e le risorse a molti [ https://asp.net/mvc ](https://asp.net/mvc) per altre informazioni su ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="03466-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="03466-149">Buona lettura.</span><span class="sxs-lookup"><span data-stu-id="03466-149">Enjoy!</span></span>

- <span data-ttu-id="03466-150">Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) e [ @shanselman ](http://twitter.com/shanselman) su Twitter.</span><span class="sxs-lookup"><span data-stu-id="03466-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="03466-151">Precedente</span><span class="sxs-lookup"><span data-stu-id="03466-151">Previous</span></span>](getting-started-with-mvc-part7.md)
