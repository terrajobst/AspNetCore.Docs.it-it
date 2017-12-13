---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Aggiunta di una colonna al modello | Documenti Microsoft
author: shanselman
description: "Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà un'applicazione web semplice la lettura e scrittura da un database."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 4a4fc144dcbed8ad14411332df7acccc032ddf46
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="3eb1e-104">Aggiunta di una colonna al modello</span><span class="sxs-lookup"><span data-stu-id="3eb1e-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="3eb1e-105">da [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="3eb1e-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="3eb1e-106">Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="3eb1e-107">Si creerà un'applicazione web semplice la lettura e scrittura da un database.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="3eb1e-108">Visitare il [area risorse di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="3eb1e-109">In questa sezione verrà illustrata come è possibile apportare modifiche allo schema del database e gestire le modifiche all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="3eb1e-110">Aggiungere una colonna "Rating" alla tabella dei film.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="3eb1e-111">Tornare all'IDE e fare clic su Esplora Database.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="3eb1e-112">Fare clic con il pulsante destro nella tabella di film e selezionare Apri definizione tabella.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="3eb1e-113">Aggiungere una colonna di "Livello", come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="3eb1e-114">Poiché non sono ora le classificazioni, la colonna consente valori null.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="3eb1e-115">Fare clic su Salva.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-115">Click Save.</span></span>

<span data-ttu-id="3eb1e-116">[![Modifica tabella filmati](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3eb1e-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="3eb1e-117">Successivamente, tornare a Esplora soluzioni e aprire il file Movies.edmx (che si trova nella cartella \Models).</span><span class="sxs-lookup"><span data-stu-id="3eb1e-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="3eb1e-118">Fare clic con il pulsante destro nell'area di progettazione (area bianca) e selezionare il modello di aggiornamento dal Database.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="3eb1e-119">[![Filmati - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="3eb1e-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="3eb1e-120">Verrà avviata il "aggiornamento guidato".</span><span class="sxs-lookup"><span data-stu-id="3eb1e-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="3eb1e-121">Fare clic sulla scheda di aggiornamento all'interno di esso e fare clic su Fine.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="3eb1e-122">La classe modello film verrà quindi aggiornata con la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-122">Our Movie model class will then be updated with the new column.</span></span>

![Aggiornamento guidato (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="3eb1e-124">Dopo aver fatto clic su Fine, è possibile visualizzare che la nuova colonna di classificazione è stato aggiunto all'entità nel modello in esame film.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="3eb1e-125">[![Entità di film](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="3eb1e-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="3eb1e-126">È stato aggiunto a una colonna nel modello di database, ma le visualizzazioni non sa su di esso.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="3eb1e-127">Aggiornamento delle visualizzazioni con le modifiche al modello</span><span class="sxs-lookup"><span data-stu-id="3eb1e-127">Update Views with Model Changes</span></span>

<span data-ttu-id="3eb1e-128">Esistono diversi modi, è possibile aggiornare i modelli di visualizzazione per riflettere la nuova colonna di classificazione.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="3eb1e-129">Queste viste è stato creato da generarli tramite la finestra di dialogo Aggiungi visualizzazione, è possibile eliminarli e ricrearli nuovamente.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="3eb1e-130">Tuttavia, in genere gli utenti saranno già apportate le modifiche apportate per i modelli di visualizzazione dalla generazione di scaffolding iniziale e saranno necessario aggiungere o eliminare i campi manualmente, esattamente come è stato fatto con il campo ID per la creazione.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="3eb1e-131">Aprire il modello \Views\Movies\Index.aspx e aggiungere un &lt;th&gt;classificazione&lt;/th&gt; all'intestazione della tabella di film.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="3eb1e-132">Il mio aggiunto dopo il genere.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-132">I added mine after Genre.</span></span> <span data-ttu-id="3eb1e-133">Quindi, nella stessa posizione di colonna, ma più in basso, aggiungere una riga per la nuova classificazione di output.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="3eb1e-134">Il nostro modello Index.aspx finale sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3eb1e-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="3eb1e-135">Verrà quindi aprire il modello \Views\Movies\Create.aspx e aggiungere un'etichetta e una casella di testo per la nuova proprietà di classificazione:</span><span class="sxs-lookup"><span data-stu-id="3eb1e-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="3eb1e-136">Il nostro modello Create.aspx finale sarà simile al seguente e modificare titolo e secondaria del browser &lt;h2&gt; titolo simile al seguente "Creare un filmato" mentre ci troviamo in!</span><span class="sxs-lookup"><span data-stu-id="3eb1e-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="3eb1e-137">Eseguire l'app e a questo punto si dispone di un nuovo campo nel database che è stato aggiunto alla pagina di creazione.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="3eb1e-138">Aggiungere un nuovo filmato - questa volta con una classificazione uguale a - e fare clic su Crea.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="3eb1e-139">[![Creare un filmato - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="3eb1e-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="3eb1e-140">Dopo aver fatto clic su Crea, è inviata alla pagina di indice in cui si nuovi film sia elencato con la nuova colonna nel database di classificazione</span><span class="sxs-lookup"><span data-stu-id="3eb1e-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="3eb1e-141">[![Elenco di film - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="3eb1e-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="3eb1e-142">In questa esercitazione di base ottenuto è iniziato le controller, associarli a viste e al passaggio dati hardcoded.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="3eb1e-143">Quindi è creato e progettato un Database e inserire alcuni dati in.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="3eb1e-144">Abbiamo recuperati i dati dal database e viene visualizzato in una tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="3eb1e-145">Quindi è stato aggiunto un modulo di creazione per consentire all'utente di aggiungere dati al database stessi da all'interno dell'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="3eb1e-146">È l'aggiunta di convalida, quindi effettuata la convalida di utilizzare JavaScript sul lato client.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="3eb1e-147">Infine, è modificato il database per includere una nuova colonna di dati, quindi aggiornare le due pagine per creare e visualizzare questi nuovi dati.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="3eb1e-148">Si consiglia per continuare con l'esercitazione di livello intermedio ora "[negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" nonché numerosi video e risorse in [https://asp.net/mvc](https://asp.net/mvc) per ulteriori informazioni su ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="3eb1e-149">Usufruisci!</span><span class="sxs-lookup"><span data-stu-id="3eb1e-149">Enjoy!</span></span>

- <span data-ttu-id="3eb1e-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) e [ @shanselman ](http://twitter.com/shanselman) su Twitter.</span><span class="sxs-lookup"><span data-stu-id="3eb1e-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="3eb1e-151">Precedente</span><span class="sxs-lookup"><span data-stu-id="3eb1e-151">Previous</span></span>](getting-started-with-mvc-part7.md)
