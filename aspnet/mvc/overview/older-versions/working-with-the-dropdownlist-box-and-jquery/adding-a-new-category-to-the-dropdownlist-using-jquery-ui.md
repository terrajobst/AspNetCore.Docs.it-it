---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Aggiunta di una nuova categoria a DropDownList tramite l'interfaccia utente di jQuery | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: e2528b74b714a3f691b07ed2429b3fe9eb3c2074
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802526"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="156cb-102">Aggiunta di una nuova categoria a DropDownList tramite l'interfaccia utente di jQuery</span><span class="sxs-lookup"><span data-stu-id="156cb-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="156cb-103">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="156cb-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="156cb-104">Il codice HTML `Select` tag è ideale per presentare un elenco di dati della categoria fisso, ma spesso è necessario aggiungere una nuova categoria.</span><span class="sxs-lookup"><span data-stu-id="156cb-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="156cb-105">Si supponga che si vuole aggiungere il genere "Opera" per le categorie nel nostro database?</span><span class="sxs-lookup"><span data-stu-id="156cb-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="156cb-106">In questa sezione, useremo l'indirizzo dell'interfaccia utente di jQuery per aggiungere una finestra di dialogo che è possibile usare per aggiungere una nuova categoria.</span><span class="sxs-lookup"><span data-stu-id="156cb-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="156cb-107">L'immagine seguente mostra come l'interfaccia utente verrà visualizzate nel browser.</span><span class="sxs-lookup"><span data-stu-id="156cb-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="156cb-108">Quando un utente seleziona il **Aggiungi nuovo Genre** collegamento, una finestra di dialogo popup richiede l'immissione di un nuovo nome di genere (e facoltativamente una descrizione).</span><span class="sxs-lookup"><span data-stu-id="156cb-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="156cb-109">L'immagine seguente mostra le **Genre aggiungere** finestra di dialogo popup.</span><span class="sxs-lookup"><span data-stu-id="156cb-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="156cb-110">Quando viene immesso un nuovo nome al genere e il **salvare** pulsante è premuto, si verifica quanto segue:</span><span class="sxs-lookup"><span data-stu-id="156cb-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="156cb-111">Una chiamata AJAX invia i dati per il metodo di creazione del Controller Genre, che salva il genere di nuovo il database e restituisce le nuove informazioni di genere (genere nome e ID) in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="156cb-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="156cb-112">JavaScript aggiunge i nuovi dati genre all'elenco di selezione.</span><span class="sxs-lookup"><span data-stu-id="156cb-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="156cb-113">JavaScript rende il genere di nuovo l'elemento selezionato.</span><span class="sxs-lookup"><span data-stu-id="156cb-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="156cb-114">Nell'immagine seguente, **Opera** è stato aggiunto al database e selezionato nella **Genre** nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="156cb-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="156cb-115">Aprire il *Views\StoreManager\Create.cshtml* file e sostituire il markup del genere con il codice seguente il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="156cb-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="156cb-116">Il `_ChooseGenre` contiene tutta la logica per associare il JavaScript e jQuery utilizzata per implementare la funzionalità di genere Aggiungi nuova visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="156cb-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="156cb-117">Dopo che è stata completata il codice sarà semplice eseguire la stessa operazione con l'interfaccia utente di artista.</span><span class="sxs-lookup"><span data-stu-id="156cb-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="156cb-118">In Esplora soluzioni fare clic con il pulsante destro il *Views\StoreManager* cartella e selezionare **Add**, quindi **visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="156cb-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="156cb-119">Nel **nome della vista** di input, immettere `_ChooseGenre` quindi selezionare **Add**.</span><span class="sxs-lookup"><span data-stu-id="156cb-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="156cb-120">Sostituire il markup nel *Views\StoreManager\\_ChooseGenre.cshtml* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="156cb-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="156cb-121">La prima riga dichiara che si sta passando un `Album` come il nostro modello, esattamente allo stesso modo del modello istruzione trovata nella visualizzazione di creazione.</span><span class="sxs-lookup"><span data-stu-id="156cb-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="156cb-122">Le righe successive sono le **etichetta** markup dell'helper.</span><span class="sxs-lookup"><span data-stu-id="156cb-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="156cb-123">La riga successiva è la **DropDownList** helper chiama, esattamente come in visualizzazione di creazione originale.</span><span class="sxs-lookup"><span data-stu-id="156cb-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="156cb-124">La riga successiva viene aggiunto un collegamento con il nome `Add New Genre`, e consente di disegnare come un pulsante.</span><span class="sxs-lookup"><span data-stu-id="156cb-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="156cb-125">La riga contenente `ValidationMessageFor` viene copiato direttamente dalla visualizzazione di creazione.</span><span class="sxs-lookup"><span data-stu-id="156cb-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="156cb-126">Le righe seguenti:</span><span class="sxs-lookup"><span data-stu-id="156cb-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="156cb-127">Crea un elemento div nascosto, con l'ID di `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="156cb-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="156cb-128">Si userà jQuery collegherà nostro **Genre aggiungere** finestra di dialogo con l'ID `genreDialog` in questo elemento DIV.</span><span class="sxs-lookup"><span data-stu-id="156cb-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="156cb-129">Le ultime due tag di script contengono collegamenti per i file JavaScript che verrà usato per implementare la funzionalità genre nuovo componente.</span><span class="sxs-lookup"><span data-stu-id="156cb-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="156cb-130">Il */Scripts/chooseGenre.js* file viene fornito per l'utente nel progetto, verranno esaminate, più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="156cb-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="156cb-131">Eseguire l'applicazione e fare clic sui **Aggiungi nuovo Genre** pulsante.</span><span class="sxs-lookup"><span data-stu-id="156cb-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="156cb-132">Nel **aggiungere Genre** finestra di dialogo immettere **Opera** nel **nome** casella di input.</span><span class="sxs-lookup"><span data-stu-id="156cb-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="156cb-133">Fare clic sul pulsante **Salva**.</span><span class="sxs-lookup"><span data-stu-id="156cb-133">Click the **Save** button.</span></span> <span data-ttu-id="156cb-134">Una chiamata AJAX consente di creare la categoria Opera e quindi popola l'elenco a discesa con Opera e imposta Opera come il genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="156cb-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="156cb-135">Immettere un artista, title e prezzo e quindi selezionare il **Create** pulsante.</span><span class="sxs-lookup"><span data-stu-id="156cb-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="156cb-136">Se si immette un prezzo minore di $8.99, il nuovo album verranno visualizzati nella parte superiore della visualizzazione Index.</span><span class="sxs-lookup"><span data-stu-id="156cb-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="156cb-137">Verificare che la nuova voce di album è stata salvata nel database.</span><span class="sxs-lookup"><span data-stu-id="156cb-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="156cb-138">Provare a creare un nuovo genre con solo una lettera.</span><span class="sxs-lookup"><span data-stu-id="156cb-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="156cb-139">Il codice seguente il *Models\Genre.cs* file imposta la lunghezza minima e massima del nome del genere.</span><span class="sxs-lookup"><span data-stu-id="156cb-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="156cb-140">La convalida lato client segnala che è necessario immettere una stringa compresa tra 2 e 20 caratteri.</span><span class="sxs-lookup"><span data-stu-id="156cb-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="156cb-141">Esaminando come un genere nuova viene aggiunto al Database e l'elenco Select.</span><span class="sxs-lookup"><span data-stu-id="156cb-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="156cb-142">Aprire il *Scripts\chooseGenre.js* file ed esaminare il codice.</span><span class="sxs-lookup"><span data-stu-id="156cb-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="156cb-143">La seconda riga Usa l'ID `genreDialog` per creare una finestra di dialogo nel tag div nel *Views\StoreManager\\_ChooseGenre.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="156cb-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="156cb-144">La maggior parte dei parametri denominati sono necessita di spiegazione.</span><span class="sxs-lookup"><span data-stu-id="156cb-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="156cb-145">Il `autoOpen` parametro è impostato su false, selezionando il **Genre creare** pulsante verrà aperta la finestra di dialogo in modo esplicito (illustrato quest'ultimo su).</span><span class="sxs-lookup"><span data-stu-id="156cb-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="156cb-146">La finestra di dialogo ha due pulsanti, **salvare** e **Annulla**.</span><span class="sxs-lookup"><span data-stu-id="156cb-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="156cb-147">Il **annullare** pulsante chiude la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="156cb-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="156cb-148">Il codice seguente illustra il **salvare** pulsante (funzione).</span><span class="sxs-lookup"><span data-stu-id="156cb-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="156cb-149">Il `var createGenreForm` viene selezionato il `createGenreForm` ID.</span><span class="sxs-lookup"><span data-stu-id="156cb-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="156cb-150">Il `createGenreForm` ID è stato impostato nel seguente codice trovato nel *Views\Genre\\_CreateGenre.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="156cb-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="156cb-151">Il [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) overload helper utilizzato nel *Views\Genre\\_CreateGenre.cshtml* file genera codice HTML con un attributo di azione che contiene l'URL per inviare il modulo.</span><span class="sxs-lookup"><span data-stu-id="156cb-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="156cb-152">È possibile verificarlo visualizzando la pagina di album create in un browser e selezionando origine Mostra nel Visualizzatore.</span><span class="sxs-lookup"><span data-stu-id="156cb-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="156cb-153">Il markup seguente mostra il codice HTML generato che contiene il tag form.</span><span class="sxs-lookup"><span data-stu-id="156cb-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="156cb-154">Il componente aggiuntivo jQuery `$.post` riga effettua una chiamata AJAX all'attributo dell'azione (`/StoreManager/Create`) e passa i dati dalle **Genre creare** nella finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="156cb-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="156cb-155">I dati sono costituiti da nome per il genere di nuovo e una descrizione facoltativa.</span><span class="sxs-lookup"><span data-stu-id="156cb-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="156cb-156">Se la chiamata AJAX ha esito positivo, il nuovo nome al genere e il valore vengono aggiunti al markup Select e il genere nuova è impostato sul valore selezionato.</span><span class="sxs-lookup"><span data-stu-id="156cb-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="156cb-157">Poiché si tratta di markup generata in modo dinamico, è possibile visualizzare la nuova opzione Seleziona per visualizzare il codice sorgente nel browser.</span><span class="sxs-lookup"><span data-stu-id="156cb-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="156cb-158">È possibile visualizzare il nuova pagina HTML con strumenti di sviluppo F12 di Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="156cb-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="156cb-159">Per visualizzare la nuova opzione select, in Internet Explorer 9, premere il tasto F12 per avviare gli strumenti di sviluppo F12.</span><span class="sxs-lookup"><span data-stu-id="156cb-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="156cb-160">Passare alla pagina Create e aggiungere un nuovo genre in modo che il genere nuovo è selezionato nell'elenco di selezione genre.</span><span class="sxs-lookup"><span data-stu-id="156cb-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="156cb-161">In strumenti di sviluppo F12:</span><span class="sxs-lookup"><span data-stu-id="156cb-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="156cb-162">Selezionare la scheda HTML.</span><span class="sxs-lookup"><span data-stu-id="156cb-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="156cb-163">Premere l'icona di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="156cb-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="156cb-164">Nella casella di ricerca immettere GenreID.</span><span class="sxs-lookup"><span data-stu-id="156cb-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="156cb-165">Usando l'icona Avanti,</span><span class="sxs-lookup"><span data-stu-id="156cb-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="156cb-166">individuare il tag di selezione seguente:</span><span class="sxs-lookup"><span data-stu-id="156cb-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="156cb-167">Espandere l'ultimo valore di opzione.</span><span class="sxs-lookup"><span data-stu-id="156cb-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="156cb-168">Il seguente codice nel *Scripts\chooseGenre.js* file illustra la procedura il **aggiungere nuovo Genre** pulsante ottiene connesso per l'evento click e il comportamento della **aggiungere nuovo Genre** è la finestra di dialogo creato.</span><span class="sxs-lookup"><span data-stu-id="156cb-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="156cb-169">La prima riga crea una funzione di fare clic su collegato per il **aggiungere nuovo Genre** pulsante.</span><span class="sxs-lookup"><span data-stu-id="156cb-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="156cb-170">Il markup seguente dal Views\StoreManager\\_ChooseGenre.cshtml file Mostra come il **Aggiungi nuovo Genre** pulsante viene creato:</span><span class="sxs-lookup"><span data-stu-id="156cb-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="156cb-171">Il metodo load crea e apre la finestra di dialogo Aggiungi Genre e chiama il componente aggiuntivo jQuery `parse` metodo in modo che la convalida del client si verifica nei dati immessi nella finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="156cb-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="156cb-172">In questa sezione si è appreso come creare una finestra di dialogo che consente di aggiungere nuovi dati di categoria a un elenco di selezione.</span><span class="sxs-lookup"><span data-stu-id="156cb-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="156cb-173">È possibile seguire la stessa procedura per creare l'interfaccia utente per aggiungere un nuovo artista all'elenco di selezione artista.</span><span class="sxs-lookup"><span data-stu-id="156cb-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="156cb-174">Questa esercitazione ha presentato una panoramica dell'utilizzo con l'helper HTML di ASP.NET MVC **DropDownList**.</span><span class="sxs-lookup"><span data-stu-id="156cb-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="156cb-175">Per altre informazioni sull'uso con il **DropDownList**, vedere la sezione riferimenti di addizione riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="156cb-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="156cb-176">È possibile comunicarlo se in questa esercitazione è stato utile.</span><span class="sxs-lookup"><span data-stu-id="156cb-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="156cb-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="156cb-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="156cb-178">Riferimenti aggiuntivi</span><span class="sxs-lookup"><span data-stu-id="156cb-178">Additional References</span></span>

- <span data-ttu-id="156cb-179">[ASP.NET MVC: Cascading gli elenchi a discesa esercitazione](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) da [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="156cb-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="156cb-180">[Scelta](http://harvesthq.github.com/chosen/) plug-in A JavaScript che supportano la selezione multipla e filtro.</span><span class="sxs-lookup"><span data-stu-id="156cb-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="156cb-181">Contributors</span><span class="sxs-lookup"><span data-stu-id="156cb-181">Contributors</span></span>

- [<span data-ttu-id="156cb-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="156cb-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="156cb-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="156cb-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="156cb-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="156cb-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="156cb-185">Revisori</span><span class="sxs-lookup"><span data-stu-id="156cb-185">Reviewers</span></span>

- <span data-ttu-id="156cb-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="156cb-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="156cb-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="156cb-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="156cb-188">Mike Pope</span><span class="sxs-lookup"><span data-stu-id="156cb-188">Mike Pope</span></span>
- <span data-ttu-id="156cb-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="156cb-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="156cb-190">Precedente</span><span class="sxs-lookup"><span data-stu-id="156cb-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
