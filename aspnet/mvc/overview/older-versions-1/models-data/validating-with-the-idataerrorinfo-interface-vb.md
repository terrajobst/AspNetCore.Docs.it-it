---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Convalida con l'interfaccia IDataErrorInfo (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther illustra come visualizzare i messaggi di errore di convalida personalizzata implementando l'interfaccia IDataErrorInfo in una classe di modello.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: e9b989d0110c3947583fd70bd38b29dcb2bb5c31
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826937"
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a><span data-ttu-id="f6125-103">Convalida con l'interfaccia IDataErrorInfo (VB)</span><span class="sxs-lookup"><span data-stu-id="f6125-103">Validating with the IDataErrorInfo Interface (VB)</span></span>
====================
<span data-ttu-id="f6125-104">da [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="f6125-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="f6125-105">Stephen Walther illustra come visualizzare i messaggi di errore di convalida personalizzata implementando l'interfaccia IDataErrorInfo in una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="f6125-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="f6125-106">L'obiettivo di questa esercitazione è per indicare un approccio all'esecuzione della convalida in un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f6125-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="f6125-107">Informazioni su come impedire l'invio di un form HTML senza fornire valori per i campi modulo necessari.</span><span class="sxs-lookup"><span data-stu-id="f6125-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="f6125-108">In questa esercitazione descrive come eseguire la convalida tramite l'interfaccia IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="f6125-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="f6125-109">Presupposti</span><span class="sxs-lookup"><span data-stu-id="f6125-109">Assumptions</span></span>

<span data-ttu-id="f6125-110">In questa esercitazione si utilizzerà il database MoviesDB e la tabella di database di film.</span><span class="sxs-lookup"><span data-stu-id="f6125-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="f6125-111">Questa tabella contiene le colonne seguenti:</span><span class="sxs-lookup"><span data-stu-id="f6125-111">This table has the following columns:</span></span>

<a id="0.6_table01"></a>


| <span data-ttu-id="f6125-112">**Nome della colonna**</span><span class="sxs-lookup"><span data-stu-id="f6125-112">**Column Name**</span></span> | <span data-ttu-id="f6125-113">**Tipo di dati**</span><span class="sxs-lookup"><span data-stu-id="f6125-113">**Data Type**</span></span> | <span data-ttu-id="f6125-114">**Consenti valori null**</span><span class="sxs-lookup"><span data-stu-id="f6125-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f6125-115">Id</span><span class="sxs-lookup"><span data-stu-id="f6125-115">Id</span></span> | <span data-ttu-id="f6125-116">Int</span><span class="sxs-lookup"><span data-stu-id="f6125-116">Int</span></span> | <span data-ttu-id="f6125-117">False</span><span class="sxs-lookup"><span data-stu-id="f6125-117">False</span></span> |
| <span data-ttu-id="f6125-118">Titolo</span><span class="sxs-lookup"><span data-stu-id="f6125-118">Title</span></span> | <span data-ttu-id="f6125-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="f6125-119">Nvarchar(100)</span></span> | <span data-ttu-id="f6125-120">False</span><span class="sxs-lookup"><span data-stu-id="f6125-120">False</span></span> |
| <span data-ttu-id="f6125-121">Director</span><span class="sxs-lookup"><span data-stu-id="f6125-121">Director</span></span> | <span data-ttu-id="f6125-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="f6125-122">Nvarchar(100)</span></span> | <span data-ttu-id="f6125-123">False</span><span class="sxs-lookup"><span data-stu-id="f6125-123">False</span></span> |
| <span data-ttu-id="f6125-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="f6125-124">DateReleased</span></span> | <span data-ttu-id="f6125-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="f6125-125">DateTime</span></span> | <span data-ttu-id="f6125-126">False</span><span class="sxs-lookup"><span data-stu-id="f6125-126">False</span></span> |


<span data-ttu-id="f6125-127">In questa esercitazione, usare Microsoft Entity Framework per generare classi modello del mio database.</span><span class="sxs-lookup"><span data-stu-id="f6125-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="f6125-128">La classe di film generata da Entity Framework viene visualizzata nella figura 1.</span><span class="sxs-lookup"><span data-stu-id="f6125-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


<span data-ttu-id="f6125-129">[![L'entità film](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f6125-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span></span>

<span data-ttu-id="f6125-130">**Figura 01**: entità del filmato ([fare clic per visualizzare l'immagine con dimensioni normali](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f6125-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="f6125-131">Per altre informazioni sull'utilizzo di Entity Framework per generare le classi di modello di database, vedere che l'esercitazione intitolata Creazione di classi del modello con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f6125-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="f6125-132">La classe Controller</span><span class="sxs-lookup"><span data-stu-id="f6125-132">The Controller Class</span></span>

<span data-ttu-id="f6125-133">Si usano controller Home per filmati elenco e creare nuovi film.</span><span class="sxs-lookup"><span data-stu-id="f6125-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="f6125-134">Il codice per questa classe è contenuto nel listato 1.</span><span class="sxs-lookup"><span data-stu-id="f6125-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="f6125-135">**Listato 1 - Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="f6125-135">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

<span data-ttu-id="f6125-136">La classe controller Home nel listato 1 contiene due azioni create ().</span><span class="sxs-lookup"><span data-stu-id="f6125-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="f6125-137">La prima azione consente di visualizzare il form HTML per la creazione di un nuovo film.</span><span class="sxs-lookup"><span data-stu-id="f6125-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="f6125-138">La seconda azione create () esegue l'inserimento effettivo di questo nuovo film nel database.</span><span class="sxs-lookup"><span data-stu-id="f6125-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="f6125-139">La seconda azione create () viene richiamata quando il form visualizzato per la prima azione create () viene inviato al server.</span><span class="sxs-lookup"><span data-stu-id="f6125-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="f6125-140">Si noti che la seconda azione create () contiene le righe di codice seguenti:</span><span class="sxs-lookup"><span data-stu-id="f6125-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

<span data-ttu-id="f6125-141">La proprietà IsValid restituisce false quando si verifica un errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="f6125-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="f6125-142">In tal caso, viene visualizzata di nuovo la visualizzazione di creazione che contiene il form HTML per la creazione di un film.</span><span class="sxs-lookup"><span data-stu-id="f6125-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="f6125-143">Creazione di una classe parziale</span><span class="sxs-lookup"><span data-stu-id="f6125-143">Creating a Partial Class</span></span>

<span data-ttu-id="f6125-144">La classe di film viene generata da Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f6125-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="f6125-145">È possibile visualizzare il codice per la classe Movie se si espande il file MoviesDBModel.edmx nella finestra Esplora soluzioni e aprire il file MoviesDBModel.Designer.vb nell'Editor del codice (vedere la figura 2).</span><span class="sxs-lookup"><span data-stu-id="f6125-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.vb file in the Code Editor (see Figure 2).</span></span>


<span data-ttu-id="f6125-146">[![Il codice per l'entità film](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f6125-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span></span>

<span data-ttu-id="f6125-147">**Figura 02**: il codice per l'entità film ([fare clic per visualizzare l'immagine con dimensioni normali](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="f6125-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span></span>


<span data-ttu-id="f6125-148">La classe di film è una classe parziale.</span><span class="sxs-lookup"><span data-stu-id="f6125-148">The Movie class is a partial class.</span></span> <span data-ttu-id="f6125-149">Ciò significa che possiamo aggiungere un'altra classe parziale con lo stesso nome per estendere le funzionalità della classe di film.</span><span class="sxs-lookup"><span data-stu-id="f6125-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="f6125-150">Si aggiungerà la logica di convalida per la nuova classe parziale.</span><span class="sxs-lookup"><span data-stu-id="f6125-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="f6125-151">Aggiungere la classe nel listato 2 nella cartella Models.</span><span class="sxs-lookup"><span data-stu-id="f6125-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="f6125-152">**Listato 2 - Models\Movie.vb**</span><span class="sxs-lookup"><span data-stu-id="f6125-152">**Listing 2 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

<span data-ttu-id="f6125-153">Si noti che la classe nel listato 2 include i *parziale* modificatore.</span><span class="sxs-lookup"><span data-stu-id="f6125-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="f6125-154">Tutti i metodi o proprietà che si aggiunge a questa classe diventano parte della classe film generato da Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f6125-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="f6125-155">Aggiunta di metodi parziali OnChanged e OnChanging</span><span class="sxs-lookup"><span data-stu-id="f6125-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="f6125-156">Quando Entity Framework genera una classe di entità, Entity Framework aggiunge automaticamente alla classe i metodi parziali.</span><span class="sxs-lookup"><span data-stu-id="f6125-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="f6125-157">Entity Framework genera OnChanging e OnChanged metodi parziali che corrispondono a ogni proprietà della classe.</span><span class="sxs-lookup"><span data-stu-id="f6125-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="f6125-158">Nel caso la classe di film, Entity Framework crea i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f6125-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="f6125-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="f6125-159">OnIdChanging</span></span>
- <span data-ttu-id="f6125-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="f6125-160">OnIdChanged</span></span>
- <span data-ttu-id="f6125-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="f6125-161">OnTitleChanging</span></span>
- <span data-ttu-id="f6125-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="f6125-162">OnTitleChanged</span></span>
- <span data-ttu-id="f6125-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="f6125-163">OnDirectorChanging</span></span>
- <span data-ttu-id="f6125-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="f6125-164">OnDirectorChanged</span></span>
- <span data-ttu-id="f6125-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="f6125-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="f6125-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="f6125-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="f6125-167">Il metodo OnChanging viene chiamato a destra prima che venga modificata la proprietà corrispondente.</span><span class="sxs-lookup"><span data-stu-id="f6125-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="f6125-168">Il metodo OnChanged viene chiamato a destra dopo la modifica della proprietà.</span><span class="sxs-lookup"><span data-stu-id="f6125-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="f6125-169">È possibile sfruttare i vantaggi di questi metodi parziali per aggiungere la logica di convalida per la classe di film.</span><span class="sxs-lookup"><span data-stu-id="f6125-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="f6125-170">La classe Movie nel listato 3 di aggiornamento verifica che le proprietà Title e direttore vengono assegnate valori non vuoti.</span><span class="sxs-lookup"><span data-stu-id="f6125-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="f6125-171">Un metodo parziale è un metodo definito in una classe che non si deve implementare.</span><span class="sxs-lookup"><span data-stu-id="f6125-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="f6125-172">Se non si implementa un metodo parziale il compilatore rimuove la firma del metodo e tutte le chiamate al metodo pertanto vi sono previsti costi di runtime associati al metodo parziale.</span><span class="sxs-lookup"><span data-stu-id="f6125-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="f6125-173">Nell'Editor di codice di Visual Studio, è possibile aggiungere un metodo parziale digitando la parola chiave *parziale* seguita da uno spazio per visualizzare un elenco di righe parzialmente eseguite da implementare.</span><span class="sxs-lookup"><span data-stu-id="f6125-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


<span data-ttu-id="f6125-174">**Listato 3 - Models\Movie.vb**</span><span class="sxs-lookup"><span data-stu-id="f6125-174">**Listing 3 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

<span data-ttu-id="f6125-175">Ad esempio, se si tenta di assegnare una stringa vuota per la proprietà Title, quindi un messaggio di errore viene assegnato a un dizionario denominato \_errori.</span><span class="sxs-lookup"><span data-stu-id="f6125-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="f6125-176">A questo punto, in realtà non accade nulla quando si assegna una stringa vuota per la proprietà Title e viene aggiunto un errore a privato \_campo errori.</span><span class="sxs-lookup"><span data-stu-id="f6125-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="f6125-177">È necessario implementare l'interfaccia IDataErrorInfo per esporre questi errori di convalida per il framework ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f6125-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="f6125-178">Implementa l'interfaccia IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="f6125-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="f6125-179">L'interfaccia IDataErrorInfo è stato incluso in .NET framework dalla prima versione.</span><span class="sxs-lookup"><span data-stu-id="f6125-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="f6125-180">Questa interfaccia è un'interfaccia molto semplice:</span><span class="sxs-lookup"><span data-stu-id="f6125-180">This interface is a very simple interface:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

<span data-ttu-id="f6125-181">Se una classe implementa l'interfaccia IDataErrorInfo, il framework di MVC ASP.NET utilizzerà questa interfaccia durante la creazione di un'istanza della classe.</span><span class="sxs-lookup"><span data-stu-id="f6125-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="f6125-182">Ad esempio, il controller Home azione create () accetta un'istanza della classe film:</span><span class="sxs-lookup"><span data-stu-id="f6125-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

<span data-ttu-id="f6125-183">Il framework ASP.NET MVC crea l'istanza del film passato all'azione create () con uno strumento di associazione del modello (il DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="f6125-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="f6125-184">Lo strumento di associazione del modello è responsabile della creazione di un'istanza dell'oggetto film associando i campi di form HTML a un'istanza dell'oggetto film.</span><span class="sxs-lookup"><span data-stu-id="f6125-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="f6125-185">Il DefaultModelBinder rileva se una classe implementa l'interfaccia IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="f6125-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="f6125-186">Se una classe implementa questa interfaccia lo strumento individuerebbe richiama l'indicizzatore IDataErrorInfo.this per ogni proprietà della classe.</span><span class="sxs-lookup"><span data-stu-id="f6125-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="f6125-187">Se l'indicizzatore restituisce un messaggio di errore lo strumento individuerebbe aggiunge questo messaggio di errore per lo stato del modello automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f6125-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="f6125-188">Il DefaultModelBinder controlla anche la proprietà IDataErrorInfo.Error.</span><span class="sxs-lookup"><span data-stu-id="f6125-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="f6125-189">Questa proprietà è lo scopo di rappresentare gli errori di convalida specifico non di proprietà associati alla classe.</span><span class="sxs-lookup"><span data-stu-id="f6125-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="f6125-190">Potrebbe ad esempio, si desidera applicare una regola di convalida che dipende dai valori di più proprietà della classe di film.</span><span class="sxs-lookup"><span data-stu-id="f6125-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="f6125-191">In tal caso, si restituisce un errore di convalida dalla proprietà di errore.</span><span class="sxs-lookup"><span data-stu-id="f6125-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="f6125-192">La classe Movie aggiornata nel listato 4 implementa l'interfaccia IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="f6125-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="f6125-193">**Listato 4 - Models\Movie.vb (implementa l'interfaccia IDataErrorInfo)**</span><span class="sxs-lookup"><span data-stu-id="f6125-193">**Listing 4 - Models\Movie.vb (implements IDataErrorInfo)**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

<span data-ttu-id="f6125-194">Nel listato 4, controlla la proprietà dell'indicizzatore di \_raccolta errori per vedere se contiene una chiave che corrisponde al nome della proprietà passata all'indicizzatore.</span><span class="sxs-lookup"><span data-stu-id="f6125-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="f6125-195">Se non si verificano errori di convalida associato alla proprietà viene restituita una stringa vuota.</span><span class="sxs-lookup"><span data-stu-id="f6125-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="f6125-196">Non devi modificare il controller Home in alcun modo per usare la classe film modificata.</span><span class="sxs-lookup"><span data-stu-id="f6125-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="f6125-197">La pagina visualizzata nella figura 3 illustra cosa accade quando viene immesso alcun valore per i campi modulo titolo o Director.</span><span class="sxs-lookup"><span data-stu-id="f6125-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


<span data-ttu-id="f6125-198">[![La creazione automatica di metodi di azione](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f6125-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span></span>

<span data-ttu-id="f6125-199">**Figura 03**: un formato con valori mancanti ([fare clic per visualizzare l'immagine con dimensioni normali](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f6125-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span></span>


<span data-ttu-id="f6125-200">Si noti che il valore DateReleased viene convalidato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f6125-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="f6125-201">Poiché la proprietà DateReleased non accetta valori NULL, il DefaultModelBinder genera un errore di convalida per questa proprietà automaticamente quando non dispone di un valore.</span><span class="sxs-lookup"><span data-stu-id="f6125-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="f6125-202">Se si desidera modificare il messaggio di errore per la proprietà DateReleased quindi è necessario creare uno strumento di associazione di modelli personalizzato.</span><span class="sxs-lookup"><span data-stu-id="f6125-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="f6125-203">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f6125-203">Summary</span></span>

<span data-ttu-id="f6125-204">In questa esercitazione è stato descritto come usare l'interfaccia IDataErrorInfo per generare i messaggi di errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="f6125-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="f6125-205">In primo luogo, abbiamo creato una classe parziale di film che estende le funzionalità della classe parziale film generata da Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f6125-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="f6125-206">Successivamente, abbiamo aggiunto la logica di convalida per i film classe OnTitleChanging() e OnDirectorChanging() metodi parziali.</span><span class="sxs-lookup"><span data-stu-id="f6125-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="f6125-207">Infine, abbiamo implementato l'interfaccia IDataErrorInfo per esporre questi messaggi di convalida per il framework ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f6125-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f6125-208">[Precedente](performing-simple-validation-vb.md)
> [Successivo](validating-with-a-service-layer-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f6125-208">[Previous](performing-simple-validation-vb.md)
[Next](validating-with-a-service-layer-vb.md)</span></span>
