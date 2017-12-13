---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Aggiunta di una nuova categoria a DropDownList tramite jQuery UI | Documenti Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 0cc51fbe84124a62f0c1254faab796cbcdc7efd6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="7e55c-102">Aggiunta di una nuova categoria a DropDownList tramite jQuery UI</span><span class="sxs-lookup"><span data-stu-id="7e55c-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="7e55c-103">Da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7e55c-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="7e55c-104">Il codice HTML `Select` tag è ideale per presentare un elenco di dati di categoria predefinito, ma spesso è necessario aggiungere una nuova categoria.</span><span class="sxs-lookup"><span data-stu-id="7e55c-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="7e55c-105">Si supponga che si vuole aggiungere il genere "Opera" per le categorie dal database?</span><span class="sxs-lookup"><span data-stu-id="7e55c-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="7e55c-106">In questa sezione, si utilizzerà jQuery UI per aggiungere una finestra di dialogo che è possibile utilizzare per aggiungere una nuova categoria.</span><span class="sxs-lookup"><span data-stu-id="7e55c-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="7e55c-107">L'immagine seguente mostra come l'interfaccia utente sarà presente nel browser.</span><span class="sxs-lookup"><span data-stu-id="7e55c-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="7e55c-108">Quando un utente seleziona il **aggiungere nuovo Genre** collegamento, una finestra di dialogo popup richiede l'immissione di un nuovo nome di genere (e facoltativamente una descrizione).</span><span class="sxs-lookup"><span data-stu-id="7e55c-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="7e55c-109">L'immagine seguente mostra il **aggiungere Genre** finestra di dialogo popup.</span><span class="sxs-lookup"><span data-stu-id="7e55c-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="7e55c-110">Quando viene immesso un nuovo nome genre e **salvare** pulsante è premuto, si verifica quanto segue:</span><span class="sxs-lookup"><span data-stu-id="7e55c-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="7e55c-111">Una chiamata AJAX invia i dati per il metodo di creazione del Controller Genre, che salva il genere di nuovo nel database e restituisce le nuove informazioni di genere (nome genre e ID) in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="7e55c-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="7e55c-112">JavaScript aggiunge i nuovi dati genre all'elenco di selezione.</span><span class="sxs-lookup"><span data-stu-id="7e55c-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="7e55c-113">JavaScript rende il genere di nuovo l'elemento selezionato.</span><span class="sxs-lookup"><span data-stu-id="7e55c-113">JavaScript makes the new genre the selected item.</span></span>

 <span data-ttu-id="7e55c-114">Nell'immagine seguente, **Opera** è stato aggiunto al database e selezionato il **Genre** nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="7e55c-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="7e55c-115">Aprire il *Views\StoreManager\Create.cshtml* file e sostituire il markup genre con quanto segue nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7e55c-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="7e55c-116">Il `_ChooseGenre` contiene tutta la logica per associare le JavaScript e jQuery utilizzata per implementare la funzionalità di genere Aggiungi nuova visualizzazione parziale.</span><span class="sxs-lookup"><span data-stu-id="7e55c-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="7e55c-117">È stato completato il codice sarà semplice eseguire la stessa operazione con l'artista dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="7e55c-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="7e55c-118">In Esplora soluzioni, fare clic destro la *Views\StoreManager* cartella e selezionare **Aggiungi**, quindi **vista**.</span><span class="sxs-lookup"><span data-stu-id="7e55c-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="7e55c-119">Nel **nome vista** di input, immettere `_ChooseGenre` selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="7e55c-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="7e55c-120">Sostituire il markup di *Views\StoreManager\\_ChooseGenre.cshtml* file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7e55c-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="7e55c-121">La prima riga dichiara che si sta passando un `Album` come modello in esame esattamente lo stesso modello istruzione disponibili nella visualizzazione di creazione.</span><span class="sxs-lookup"><span data-stu-id="7e55c-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="7e55c-122">Le righe successive sono il **etichetta** markup di supporto.</span><span class="sxs-lookup"><span data-stu-id="7e55c-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="7e55c-123">La riga successiva è il **DropDownList** helper chiama, esattamente come la visualizzazione di creazione originale.</span><span class="sxs-lookup"><span data-stu-id="7e55c-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="7e55c-124">La riga successiva viene aggiunto un collegamento con il nome `Add New Genre`, e stili ad esempio un pulsante.</span><span class="sxs-lookup"><span data-stu-id="7e55c-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="7e55c-125">La riga contenente `ValidationMessageFor` viene copiato direttamente dalla visualizzazione di creazione.</span><span class="sxs-lookup"><span data-stu-id="7e55c-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="7e55c-126">Le righe seguenti:</span><span class="sxs-lookup"><span data-stu-id="7e55c-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="7e55c-127">Crea un elemento div nascosto, con l'ID di `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="7e55c-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="7e55c-128">Utilizzeremo jQuery per collegare il nostro **aggiungere Genre** la finestra di dialogo con l'ID `genreDialog` in questo tag div.</span><span class="sxs-lookup"><span data-stu-id="7e55c-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="7e55c-129">Le ultime due tag script contengono collegamenti ai file JavaScript che verrà utilizzato per implementare la funzionalità in genere nuovo Aggiungi.</span><span class="sxs-lookup"><span data-stu-id="7e55c-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="7e55c-130">Il */Scripts/chooseGenre.js* file viene fornito automaticamente nel progetto, verrà esaminato, più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7e55c-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="7e55c-131">Eseguire l'applicazione e fare clic su di **aggiungere nuovo Genre** pulsante.</span><span class="sxs-lookup"><span data-stu-id="7e55c-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="7e55c-132">Nel **aggiungere Genre** finestra di dialogo immettere **Opera** nel **nome** casella di input.</span><span class="sxs-lookup"><span data-stu-id="7e55c-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="7e55c-133">Fare clic su di **salvare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="7e55c-133">Click the **Save** button.</span></span> <span data-ttu-id="7e55c-134">Una chiamata AJAX crea la categoria Opera e quindi popola l'elenco a discesa con Opera e imposta Opera come il genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="7e55c-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="7e55c-135">Immettere un artista, titolo e prezzo, quindi selezionare il **crea** pulsante.</span><span class="sxs-lookup"><span data-stu-id="7e55c-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="7e55c-136">Se si immette un prezzo inferiore a $8.99, il nuovo album verrà visualizzato nella parte superiore della visualizzazione dell'indice.</span><span class="sxs-lookup"><span data-stu-id="7e55c-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="7e55c-137">Verificare che la nuova voce album è stata salvata nel database.</span><span class="sxs-lookup"><span data-stu-id="7e55c-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="7e55c-138">Provare a creare un nuovo genere con solo una lettera.</span><span class="sxs-lookup"><span data-stu-id="7e55c-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="7e55c-139">Nell'esempio di codice il *Models\Genre.cs* file imposta la lunghezza minima e massima del nome del genere.</span><span class="sxs-lookup"><span data-stu-id="7e55c-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="7e55c-140">La convalida lato client segnala che è necessario immettere una stringa di caratteri compreso tra 2 e 20.</span><span class="sxs-lookup"><span data-stu-id="7e55c-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="7e55c-141">Esaminare come un genere nuovo viene aggiunto al Database e l'elenco Select.</span><span class="sxs-lookup"><span data-stu-id="7e55c-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="7e55c-142">Aprire il *Scripts\chooseGenre.js* file ed esaminare il codice.</span><span class="sxs-lookup"><span data-stu-id="7e55c-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="7e55c-143">La seconda riga viene utilizzato l'ID `genreDialog` per creare una finestra di dialogo nel tag div nel *Views\StoreManager\\_ChooseGenre.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="7e55c-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="7e55c-144">La maggior parte dei parametri denominati sono necessita di spiegazione.</span><span class="sxs-lookup"><span data-stu-id="7e55c-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="7e55c-145">Il `autoOpen` parametro è impostato su false, selezionando il **creare Genre** pulsante verrà aperta la finestra di dialogo in modo esplicito (descritto quest'ultimo su).</span><span class="sxs-lookup"><span data-stu-id="7e55c-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="7e55c-146">La finestra di dialogo include due pulsanti, **salvare** e **Annulla**.</span><span class="sxs-lookup"><span data-stu-id="7e55c-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="7e55c-147">Il **Annulla** pulsante consente di chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="7e55c-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="7e55c-148">Il codice seguente illustra il **salvare** pulsante funzione.</span><span class="sxs-lookup"><span data-stu-id="7e55c-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="7e55c-149">Il `var createGenreForm` sia selezionato il `createGenreForm` ID.</span><span class="sxs-lookup"><span data-stu-id="7e55c-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="7e55c-150">Il `createGenreForm` ID è stato impostato nel codice seguente, vedere il *Views\Genre\\_CreateGenre.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="7e55c-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="7e55c-151">Il [Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd492714.aspx) overload helper utilizzato nel *Views\Genre\\_CreateGenre.cshtml* file genera HTML con un attributo action contenente l'URL per inviare il form.</span><span class="sxs-lookup"><span data-stu-id="7e55c-151">The [Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="7e55c-152">È possibile visualizzare questa visualizzazione della pagina di album create in un browser e selezionando Mostra origine nel browser.</span><span class="sxs-lookup"><span data-stu-id="7e55c-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="7e55c-153">Di seguito viene illustrato il codice HTML generato contenente il tag form.</span><span class="sxs-lookup"><span data-stu-id="7e55c-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="7e55c-154">Di jQuery `$.post` riga esegue una chiamata di AJAX per l'attributo action (`/StoreManager/Create`) e passa i dati di **Genre creare** la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="7e55c-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="7e55c-155">I dati sono costituiti da nome per il nuovo genre e una descrizione facoltativa.</span><span class="sxs-lookup"><span data-stu-id="7e55c-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="7e55c-156">Se la chiamata di AJAX ha esito positivo, il nuovo nome genre e il valore vengono aggiunti al markup selezionare e il nuovo genere è impostato sul valore selezionato.</span><span class="sxs-lookup"><span data-stu-id="7e55c-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="7e55c-157">Poiché si tratta di markup generato dinamicamente, è possibile visualizzare la nuova opzione di seleziona la visualizzazione di origine nel browser.</span><span class="sxs-lookup"><span data-stu-id="7e55c-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="7e55c-158">È possibile visualizzare la nuova pagina HTML con gli strumenti di sviluppo F12 di Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="7e55c-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="7e55c-159">Per visualizzare la nuova opzione di seleziona, in Internet Explorer 9, premere il tasto F12 per avviare gli strumenti di sviluppo F12.</span><span class="sxs-lookup"><span data-stu-id="7e55c-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="7e55c-160">Passare alla pagina di creare e aggiungere un nuovo genere in modo che il genere nuovo è selezionato nell'elenco di selezione genere.</span><span class="sxs-lookup"><span data-stu-id="7e55c-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="7e55c-161">In strumenti di sviluppo F12:</span><span class="sxs-lookup"><span data-stu-id="7e55c-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="7e55c-162">Selezionare la scheda HTML.</span><span class="sxs-lookup"><span data-stu-id="7e55c-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="7e55c-163">Raggiunto l'icona di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="7e55c-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="7e55c-164">Nella casella di ricerca, immettere GenreID.</span><span class="sxs-lookup"><span data-stu-id="7e55c-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="7e55c-165">Utilizzando l'icona,</span><span class="sxs-lookup"><span data-stu-id="7e55c-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
 <span data-ttu-id="7e55c-166">individuare il tag di selezione seguente:</span><span class="sxs-lookup"><span data-stu-id="7e55c-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="7e55c-167">Espandere l'ultimo valore di opzione.</span><span class="sxs-lookup"><span data-stu-id="7e55c-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="7e55c-168">Nell'esempio di codice il *Scripts\chooseGenre.js* file illustra il **aggiungere nuovo Genre** pulsante ottiene collegato all'evento, fare clic su e il modo in **aggiungere nuovo Genre** è la finestra di dialogo creato.</span><span class="sxs-lookup"><span data-stu-id="7e55c-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="7e55c-169">La prima riga crea collegata a una funzione di fare clic su di **aggiungere nuovo Genre** pulsante.</span><span class="sxs-lookup"><span data-stu-id="7e55c-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="7e55c-170">Il markup seguente dal Views\StoreManager\\_ChooseGenre.cshtml file Mostra come **aggiungere nuovo Genre** pulsante è stato creato:</span><span class="sxs-lookup"><span data-stu-id="7e55c-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="7e55c-171">Il metodo load crea e apre la finestra di dialogo Aggiungi Genre e chiama il jQuery `parse` metodo pertanto la convalida del client si verifica nei dati immessi nella finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="7e55c-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="7e55c-172">In questa sezione si è appreso come creare una finestra di dialogo che consente di aggiungere nuovi dati di categoria a un elenco di selezione.</span><span class="sxs-lookup"><span data-stu-id="7e55c-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="7e55c-173">È possibile seguire la stessa procedura per creare l'interfaccia utente per aggiungere un nuovo artista all'elenco di selezione artista.</span><span class="sxs-lookup"><span data-stu-id="7e55c-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="7e55c-174">In questa esercitazione è assegnato a una panoramica dell'utilizzo con l'helper HTML di ASP.NET MVC **DropDownList**.</span><span class="sxs-lookup"><span data-stu-id="7e55c-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="7e55c-175">Per ulteriori informazioni sull'utilizzo di **DropDownList**, vedere la sezione riferimenti aggiunta.</span><span class="sxs-lookup"><span data-stu-id="7e55c-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="7e55c-176">Saremmo lieti di sapere se questa esercitazione è stato utile.</span><span class="sxs-lookup"><span data-stu-id="7e55c-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="7e55c-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="7e55c-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="7e55c-178">Riferimenti aggiuntivi</span><span class="sxs-lookup"><span data-stu-id="7e55c-178">Additional References</span></span>

- <span data-ttu-id="7e55c-179">[ASP.NET MVC – CSS gli elenchi a discesa esercitazione](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) da [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="7e55c-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="7e55c-180">[Scelta](http://harvesthq.github.com/chosen/) plug-in A JavaScript che supportano la selezione multipla e filtro.</span><span class="sxs-lookup"><span data-stu-id="7e55c-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="7e55c-181">Contributors</span><span class="sxs-lookup"><span data-stu-id="7e55c-181">Contributors</span></span>

- [<span data-ttu-id="7e55c-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="7e55c-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="7e55c-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="7e55c-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="7e55c-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="7e55c-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="7e55c-185">Revisori</span><span class="sxs-lookup"><span data-stu-id="7e55c-185">Reviewers</span></span>

- <span data-ttu-id="7e55c-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="7e55c-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="7e55c-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="7e55c-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="7e55c-188">Mike Pope</span><span class="sxs-lookup"><span data-stu-id="7e55c-188">Mike Pope</span></span>
- <span data-ttu-id="7e55c-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="7e55c-189">Tom Dykstra</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7e55c-190">Precedente</span><span class="sxs-lookup"><span data-stu-id="7e55c-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
