---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: "Esaminare la modalità ASP.NET MVC scaffolds del DropDownList Helper | Documenti Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: abd9b5c09e942b966eb3eaaebe1b315c30b8e0c0
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="7809c-102">Esaminare la modalità ASP.NET MVC scaffolds del DropDownList Helper</span><span class="sxs-lookup"><span data-stu-id="7809c-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="7809c-103">Da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7809c-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="7809c-104">In **Esplora**, fare doppio clic su di *controller* cartella e quindi selezionare **Aggiungi Controller**.</span><span class="sxs-lookup"><span data-stu-id="7809c-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="7809c-105">Denominare il controller **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="7809c-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="7809c-106">Impostare le opzioni per il **Aggiungi Controller** finestra di dialogo come illustrato nell'immagine seguente.</span><span class="sxs-lookup"><span data-stu-id="7809c-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="7809c-107">Modificare il *StoreManager\Index.cshtml* consente di visualizzare e rimuovere `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="7809c-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="7809c-108">Rimozione di `AlbumArtUrl` semplifichino la presentazione.</span><span class="sxs-lookup"><span data-stu-id="7809c-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="7809c-109">Il codice completo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7809c-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="7809c-110">Aprire il *Controllers\StoreManagerController.cs* file e individuare il `Index` metodo.</span><span class="sxs-lookup"><span data-stu-id="7809c-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="7809c-111">Aggiungere il `OrderBy` clausola in modo gli album verranno ordinati in base al prezzo.</span><span class="sxs-lookup"><span data-stu-id="7809c-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="7809c-112">Il codice completo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7809c-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="7809c-113">Ordinamento in base al prezzo rende più semplice testare le modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="7809c-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="7809c-114">Quando si esegue il test della modifica e creare i metodi, è possibile usare un prezzo minimo, pertanto i dati salvati verranno visualizzati per primi.</span><span class="sxs-lookup"><span data-stu-id="7809c-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="7809c-115">Aprire il *StoreManager\Edit.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="7809c-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="7809c-116">Aggiungere la riga seguente immediatamente dopo il tag della legenda.</span><span class="sxs-lookup"><span data-stu-id="7809c-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="7809c-117">Il codice seguente mostra il contesto di questa modifica:</span><span class="sxs-lookup"><span data-stu-id="7809c-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="7809c-118">Il `AlbumId` è necessario apportare modifiche al record di un album.</span><span class="sxs-lookup"><span data-stu-id="7809c-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="7809c-119">Premere CTRL+F5 per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7809c-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="7809c-120">Selezionare questa opzione per il **Admin** collegare, quindi selezionare il **Crea nuovo** collegamento per creare un nuovo album.</span><span class="sxs-lookup"><span data-stu-id="7809c-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="7809c-121">Verificare che le informazioni è state salvate.</span><span class="sxs-lookup"><span data-stu-id="7809c-121">Verify the album information was saved.</span></span> <span data-ttu-id="7809c-122">Modifica di un album e verificare le modifiche apportate vengono rese persistenti.</span><span class="sxs-lookup"><span data-stu-id="7809c-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="7809c-123">Lo Schema di Album</span><span class="sxs-lookup"><span data-stu-id="7809c-123">The Album Schema</span></span>

<span data-ttu-id="7809c-124">Il `StoreManager` controller creato tramite il meccanismo di scaffolding MVC accedervi CRUD (Create, Read, Update, Delete) gli album nel database dell'archivio musica.</span><span class="sxs-lookup"><span data-stu-id="7809c-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="7809c-125">Di seguito è riportato lo schema per informazioni:</span><span class="sxs-lookup"><span data-stu-id="7809c-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="7809c-126">Il `Albums` tabella non memorizza il genere album e la descrizione, archivia una chiave esterna per il `Genres` tabella.</span><span class="sxs-lookup"><span data-stu-id="7809c-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="7809c-127">Il `Genres` tabella contiene in genere nome e descrizione.</span><span class="sxs-lookup"><span data-stu-id="7809c-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="7809c-128">Analogamente, il `Albums` tabella non contiene il nome di artisti album, ma una chiave esterna per il `Artists` tabella.</span><span class="sxs-lookup"><span data-stu-id="7809c-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="7809c-129">Il `Artists` tabella contiene il nome dell'artista.</span><span class="sxs-lookup"><span data-stu-id="7809c-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="7809c-130">Se si esaminano i dati di `Albums` tabella, è possibile visualizzare ogni riga contiene una chiave esterna per il `Genres` tabella e una chiave esterna per il `Artists` tabella.</span><span class="sxs-lookup"><span data-stu-id="7809c-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="7809c-131">Nell'immagine seguente mostra alcuni dati della tabella dal `Albums` tabella.</span><span class="sxs-lookup"><span data-stu-id="7809c-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="7809c-132">Il Tag HTML Select</span><span class="sxs-lookup"><span data-stu-id="7809c-132">The HTML Select Tag</span></span>

<span data-ttu-id="7809c-133">Il codice HTML `<select>` elemento (creato dal codice HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) viene utilizzato per visualizzare un elenco completo dei valori (ad esempio l'elenco di generi).</span><span class="sxs-lookup"><span data-stu-id="7809c-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="7809c-134">Per i moduli di modifica, quando il valore corrente è noto, l'elenco di selezione può visualizzare il valore corrente.</span><span class="sxs-lookup"><span data-stu-id="7809c-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="7809c-135">È stato illustrato questo precedentemente quando si imposta il valore selezionato **comici**.</span><span class="sxs-lookup"><span data-stu-id="7809c-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="7809c-136">Elenco di selezione è ideale per la visualizzazione di categoria o dati di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="7809c-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="7809c-137">Il `<select>` elemento per la chiave esterna Genre viene visualizzato l'elenco di nomi di genere possibili, ma quando si salva il modulo la proprietà Genre viene aggiornata con Genre valore della chiave esterna, non il nome visualizzato genere.</span><span class="sxs-lookup"><span data-stu-id="7809c-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="7809c-138">Nell'immagine seguente, il genere selezionato è **Disco** e artista **Donna Summer**.</span><span class="sxs-lookup"><span data-stu-id="7809c-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="7809c-139">Esame di MVC ASP.NET scaffolding codice</span><span class="sxs-lookup"><span data-stu-id="7809c-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="7809c-140">Aprire il *Controllers\StoreManagerController.cs* file e individuare il `HTTP GET Create` metodo.</span><span class="sxs-lookup"><span data-stu-id="7809c-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="7809c-141">Il `Create` metodo aggiunge due [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) oggetti per il `ViewBag`, uno per contenere le informazioni di genre e uno per contenere le informazioni artista.</span><span class="sxs-lookup"><span data-stu-id="7809c-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="7809c-142">Il [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) overload del costruttore utilizzato in precedenza accetta tre argomenti:</span><span class="sxs-lookup"><span data-stu-id="7809c-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="7809c-143">*elementi*: un [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contenenti gli elementi nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="7809c-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="7809c-144">Nell'esempio precedente, l'elenco di generi restituito dal `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="7809c-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="7809c-145">*dataValueField*: il nome della proprietà di **IEnumerable** elenco che contiene il valore della chiave.</span><span class="sxs-lookup"><span data-stu-id="7809c-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="7809c-146">Nell'esempio precedente, `GenreId` e `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="7809c-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="7809c-147">*dataTextField*: il nome della proprietà di **IEnumerable** elenco che contiene le informazioni da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="7809c-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="7809c-148">In entrambi artisti e la tabella genre il `name` campo viene usato.</span><span class="sxs-lookup"><span data-stu-id="7809c-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="7809c-149">Aprire il *Views\StoreManager\Create.cshtml* file ed esaminare il `Html.DropDownList` markup di supporto per il campo genre.</span><span class="sxs-lookup"><span data-stu-id="7809c-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="7809c-150">La prima riga indica che la visualizzazione di creazione accetta un `Album` modello.</span><span class="sxs-lookup"><span data-stu-id="7809c-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="7809c-151">Nel `Create` metodo illustrato sopra, è stato passato alcun modello, in modo che la vista ottenga un **null** `Album` modello.</span><span class="sxs-lookup"><span data-stu-id="7809c-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="7809c-152">A questo punto viene creato un nuovo album pertanto non `Album` dati.</span><span class="sxs-lookup"><span data-stu-id="7809c-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="7809c-153">Il [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload illustrato in precedenza prende il nome del campo da associare al modello.</span><span class="sxs-lookup"><span data-stu-id="7809c-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="7809c-154">Viene inoltre utilizzato il nome specificato per cercare un **ViewBag** oggetto che contiene un [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) oggetto.</span><span class="sxs-lookup"><span data-stu-id="7809c-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="7809c-155">Utilizza questo overload, verrà richiesto di nome di **ViewBag SelectList** oggetto `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="7809c-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="7809c-156">Il secondo parametro (`String.Empty`) è il testo da visualizzare quando è selezionato alcun elemento.</span><span class="sxs-lookup"><span data-stu-id="7809c-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="7809c-157">Questo è esattamente quello desiderato quando si crea un nuovo album.</span><span class="sxs-lookup"><span data-stu-id="7809c-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="7809c-158">Se è possibile rimuovere il secondo parametro e utilizzato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7809c-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="7809c-159">Elenco di selezione predefinito verrà impostato per il primo elemento o Rock in questo esempio.</span><span class="sxs-lookup"><span data-stu-id="7809c-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="7809c-160">Esaminare il `HTTP POST Create` metodo.</span><span class="sxs-lookup"><span data-stu-id="7809c-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="7809c-161">Questo overload del metodo di `Create` metodo accetta un `album` oggetto, creato dal sistema di associazione del modello MVC ASP.NET dai valori di modulo registrato.</span><span class="sxs-lookup"><span data-stu-id="7809c-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="7809c-162">Quando si invia un nuovo album, se lo stato del modello è valido e non sono presenti errori di database, il database viene aggiunto il nuovo album.</span><span class="sxs-lookup"><span data-stu-id="7809c-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="7809c-163">Nella figura seguente illustra la creazione di un nuovo album.</span><span class="sxs-lookup"><span data-stu-id="7809c-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="7809c-164">È possibile utilizzare il [strumento fiddler](http://www.fiddler2.com/fiddler2/) per esaminare i valori del form inseriti tale associazione di modelli di MVC ASP.NET utilizza per creare l'oggetto album.</span><span class="sxs-lookup"><span data-stu-id="7809c-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="7809c-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="7809c-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="7809c-166">La creazione di ViewBag SelectList refactoring</span><span class="sxs-lookup"><span data-stu-id="7809c-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="7809c-167">Entrambi i `Edit` metodi e `HTTP POST Create` metodo sono identico codice per impostare il **SelectList** nel **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="7809c-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="7809c-168">In spirito [secca](http://en.wikipedia.org/wiki/Don't_repeat_yourself), abbiamo questo codice di eseguire il refactoring.</span><span class="sxs-lookup"><span data-stu-id="7809c-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="7809c-169">Ci accerteremo utilizzo di questo codice sottoposto a refactoring in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="7809c-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="7809c-170">Creare un nuovo metodo per aggiungere un genere e artista **SelectList** per il **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="7809c-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="7809c-171">Sostituire le due righe impostando il `ViewBag` in ogni il `Create` e `Edit` metodi con una chiamata al `SetGenreArtistViewBag` metodo.</span><span class="sxs-lookup"><span data-stu-id="7809c-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="7809c-172">Il codice completo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7809c-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="7809c-173">Creare un nuovo album e modificare un album per verificare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="7809c-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="7809c-174">Passare in modo esplicito il SelectList DropDownList</span><span class="sxs-lookup"><span data-stu-id="7809c-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="7809c-175">Le viste di creare e modificare create per l'utilizzo di scaffolding di ASP.NET MVC seguente **DropDownList** overload:</span><span class="sxs-lookup"><span data-stu-id="7809c-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="7809c-176">Il `DropDownList` di seguito è riportato il markup per la visualizzazione di creazione.</span><span class="sxs-lookup"><span data-stu-id="7809c-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="7809c-177">Perché il `ViewBag` proprietà per il `SelectList` denominato `GenreId`, **DropDownList** helper utilizzerà il `GenreId` **SelectList** nel **ViewBag** .</span><span class="sxs-lookup"><span data-stu-id="7809c-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="7809c-178">Nell'esempio seguente **DropDownList** overload, la `SelectList` viene passato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="7809c-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="7809c-179">Aprire il *Views\StoreManager\Edit.cshtml* file e modificare il **DropDownList** chiamata per passare in modo esplicito il **SelectList**, utilizzando l'overload del precedente.</span><span class="sxs-lookup"><span data-stu-id="7809c-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="7809c-180">Questa operazione per la categoria di genere.</span><span class="sxs-lookup"><span data-stu-id="7809c-180">Do this for the Genre category.</span></span> <span data-ttu-id="7809c-181">Il codice completo è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="7809c-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="7809c-182">Eseguire l'applicazione e fare clic su di **Admin** collegare, quindi passare a un album Jazz e selezionare il **modifica** collegamento.</span><span class="sxs-lookup"><span data-stu-id="7809c-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="7809c-183">Anziché mostrare Jazz come il genere attualmente selezionato, viene visualizzato Rock.</span><span class="sxs-lookup"><span data-stu-id="7809c-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="7809c-184">Quando l'argomento di stringa (la proprietà da associare) e **SelectList** oggetto con lo stesso nome, il valore selezionato non viene utilizzato.</span><span class="sxs-lookup"><span data-stu-id="7809c-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="7809c-185">Quando è presente alcun valore selezionato fornito, browser predefinito per il primo elemento di **SelectList**(ovvero **Rock** nell'esempio precedente).</span><span class="sxs-lookup"><span data-stu-id="7809c-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="7809c-186">Si tratta di una limitazione nota del **DropDownList** helper.</span><span class="sxs-lookup"><span data-stu-id="7809c-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="7809c-187">Aprire il *Controllers\StoreManagerController.cs* file e modificare il **SelectList** oggetto nomi `Genres` e `Artists`.</span><span class="sxs-lookup"><span data-stu-id="7809c-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="7809c-188">Il codice completo è illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="7809c-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="7809c-189">I nomi di generi e artisti sono nomi migliori per le categorie, in quanto contengono solo l'ID di ogni categoria.</span><span class="sxs-lookup"><span data-stu-id="7809c-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="7809c-190">Abbiamo fatto in precedenza il refactoring dei pagamenti.</span><span class="sxs-lookup"><span data-stu-id="7809c-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="7809c-191">Anziché modificare il **ViewBag** in quattro metodi, le modifiche sono state isolate per il `SetGenreArtistViewBag` metodo.</span><span class="sxs-lookup"><span data-stu-id="7809c-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="7809c-192">Modifica il **DropDownList** chiamare nella creazione e modifica delle viste per usare la nuova **SelectList** nomi.</span><span class="sxs-lookup"><span data-stu-id="7809c-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="7809c-193">Di seguito è riportato il nuovo tag per la visualizzazione di modifica:</span><span class="sxs-lookup"><span data-stu-id="7809c-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="7809c-194">Visualizzazione di creazione richiede una stringa vuota per evitare che venga visualizzato il primo elemento di SelectList.</span><span class="sxs-lookup"><span data-stu-id="7809c-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="7809c-195">Creare un nuovo album e modificare un album per verificare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="7809c-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="7809c-196">Testare il codice di modifica selezionando un album con un genere diverso da Rock.</span><span class="sxs-lookup"><span data-stu-id="7809c-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="7809c-197">Con un modello di visualizzazione del DropDownList Helper</span><span class="sxs-lookup"><span data-stu-id="7809c-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="7809c-198">Creare una nuova classe nella cartella ViewModel denominata `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="7809c-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="7809c-199">Sostituire il codice di `AlbumSelectListViewModel` classe con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7809c-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="7809c-200">Il `AlbumSelectListViewModel` costruttore accetta un album, un elenco degli artisti e generi e crea un oggetto contenente album e un `SelectList` per generi e artisti.</span><span class="sxs-lookup"><span data-stu-id="7809c-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="7809c-201">Compilare il progetto pertanto `AlbumSelectListViewModel` è disponibile quando si crea una vista nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="7809c-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="7809c-202">Aggiungere un `EditVM` metodo il `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="7809c-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="7809c-203">Il codice completo è illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7809c-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="7809c-204">Fare clic destro `AlbumSelectListViewModel`selezionare **risolvere**, quindi **utilizzando MvcMusicStore.ViewModels;**.</span><span class="sxs-lookup"><span data-stu-id="7809c-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="7809c-205">In alternativa, è possibile aggiungere la seguente istruzione using:</span><span class="sxs-lookup"><span data-stu-id="7809c-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="7809c-206">Fare clic destro `EditVM` e selezionare **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="7809c-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="7809c-207">Utilizzare le opzioni riportate di seguito.</span><span class="sxs-lookup"><span data-stu-id="7809c-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="7809c-208">Selezionare **Aggiungi**, quindi sostituire il contenuto del *Views\StoreManager\EditVM.cshtml* file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7809c-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="7809c-209">Il `EditVM` markup è molto simile all'originale `Edit` markup con le eccezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="7809c-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="7809c-210">Le proprietà del modello di `Edit` visualizzati sono nel formato `model.property`(ad esempio, `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="7809c-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="7809c-211">Le proprietà del modello di `EditVm` visualizzati sono nel formato `model.Album.property`(ad esempio, `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="7809c-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="7809c-212">Ciò accade perché il `EditVM` visualizzazione viene passato un contenitore un `Album`, non un `Album` come nel `Edit` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="7809c-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="7809c-213">Il **DropDownList** secondo parametro viene fornito dal modello di visualizzazione, non il **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="7809c-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="7809c-214">Il **BeginForm** helper nel `EditVM` vista in modo esplicito il postback al `Edit` metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7809c-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="7809c-215">Registrazione di eseguire il backup per il `Edit` azione, non è necessario scrivere un `HTTP POST EditVM` azione e riutilizzare il `HTTP POST` `Edit` azione.</span><span class="sxs-lookup"><span data-stu-id="7809c-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="7809c-216">Eseguire l'applicazione e modificare un album.</span><span class="sxs-lookup"><span data-stu-id="7809c-216">Run the application and edit an album.</span></span> <span data-ttu-id="7809c-217">Modificare l'URL da utilizzare `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="7809c-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="7809c-218">Modificare un campo e premere il **salvare** per verificare il funzionamento del codice.</span><span class="sxs-lookup"><span data-stu-id="7809c-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="7809c-219">Scelta dell'approccio è necessario utilizzare?</span><span class="sxs-lookup"><span data-stu-id="7809c-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="7809c-220">Tutti e tre gli approcci illustrati sono acceptible.</span><span class="sxs-lookup"><span data-stu-id="7809c-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="7809c-221">Molti sviluppatori preferiscono explictily passare il `SelectList` per il `DropDownList` utilizzando il `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="7809c-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="7809c-222">Questo approccio presenta il vantaggio di offrendo la possibilità di utilizzare un nome più appropriato per la raccolta.</span><span class="sxs-lookup"><span data-stu-id="7809c-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="7809c-223">Un'avvertenza è è possibile assegnare il nome di `ViewBag SelectList` lo stesso nome della proprietà del modello dell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="7809c-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="7809c-224">Alcuni sviluppatori preferiscono l'approccio ViewModel.</span><span class="sxs-lookup"><span data-stu-id="7809c-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="7809c-225">Altri si consideri il codice più dettagliato e HTML dell'approccio ViewModel generato uno svantaggio.</span><span class="sxs-lookup"><span data-stu-id="7809c-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="7809c-226">In questa sezione aver acquisito tre approcci per l'utilizzo di **DropDownList** con i dati di categoria.</span><span class="sxs-lookup"><span data-stu-id="7809c-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="7809c-227">Nella sezione successiva, vi mostreremo come aggiungere una nuova categoria.</span><span class="sxs-lookup"><span data-stu-id="7809c-227">In the next section, we'll show how to add a new category.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7809c-228">[Precedente](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Successivo](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="7809c-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
