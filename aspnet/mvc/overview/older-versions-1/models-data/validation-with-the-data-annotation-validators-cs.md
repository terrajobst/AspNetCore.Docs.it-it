---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: Convalida con i validator di annotazione dei dati (c#) | Documenti Microsoft
author: microsoft
description: "È possibile sfruttare il raccoglitore di modelli dati annotazione per la convalida all'interno di un'applicazione MVC ASP.NET. Informazioni su come utilizzare i diversi tipi di convalida..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 306dcb0197dfc9317ea9665dd2b1c058ba8bd712
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="validation-with-the-data-annotation-validators-c"></a><span data-ttu-id="14abc-104">Convalida con i validator di annotazione dei dati (c#)</span><span class="sxs-lookup"><span data-stu-id="14abc-104">Validation with the Data Annotation Validators (C#)</span></span>
====================
<span data-ttu-id="14abc-105">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="14abc-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="14abc-106">È possibile sfruttare il raccoglitore di modelli dati annotazione per la convalida all'interno di un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="14abc-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="14abc-107">Informazioni su come utilizzare i diversi tipi di attributi di convalida e utilizzarli in Entity Framework Microsoft.</span><span class="sxs-lookup"><span data-stu-id="14abc-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="14abc-108">In questa esercitazione imparare a usare i validator di annotazione dei dati per eseguire la convalida in un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="14abc-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="14abc-109">Il vantaggio di utilizzare i validator di annotazione dei dati è che consentono di eseguire la convalida semplicemente aggiungendo uno o più attributi, ad esempio la o attributo StringLength: per una proprietà di classe.</span><span class="sxs-lookup"><span data-stu-id="14abc-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="14abc-110">Prima di poter utilizzare i validator di annotazione dei dati, è necessario scaricare lo strumento di associazione dati modello di annotazioni.</span><span class="sxs-lookup"><span data-stu-id="14abc-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="14abc-111">È possibile scaricare l'esempio di Raccoglitore modello annotazioni dati dal sito Web CodePlex facendo [qui](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span><span class="sxs-lookup"><span data-stu-id="14abc-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="14abc-112">È importante tenere presente che il gestore di associazione di dati le annotazioni del modello non è una parte del framework di MVC ASP.NET Microsoft ufficiale.</span><span class="sxs-lookup"><span data-stu-id="14abc-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="14abc-113">Anche se il gestore di associazione di dati le annotazioni del modello è stato creato dal team di Microsoft ASP.NET MVC, Microsoft non offre supporto ufficiale del prodotto per il gestore di associazione dati modello annotazioni descritto e utilizzati in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="14abc-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="14abc-114">Utilizzando lo strumento di associazione dati modello annotazione</span><span class="sxs-lookup"><span data-stu-id="14abc-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="14abc-115">Per utilizzare lo strumento di associazione di dati le annotazioni del modello in un'applicazione ASP.NET MVC, è necessario prima aggiungere un riferimento all'assembly Microsoft.Web.Mvc.DataAnnotations.dll e l'assembly System.ComponentModel.DataAnnotations.dll.</span><span class="sxs-lookup"><span data-stu-id="14abc-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="14abc-116">Selezionare l'opzione di menu **progetto, Aggiungi riferimento**.</span><span class="sxs-lookup"><span data-stu-id="14abc-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="14abc-117">Fare clic su di **Sfoglia** scheda e passare al percorso in cui è stato scaricato (e decompressi) nell'esempio di Raccoglitore di modelli di dati le annotazioni (vedere **figura 1**).</span><span class="sxs-lookup"><span data-stu-id="14abc-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

<span data-ttu-id="14abc-118">**Figura 1**: aggiunta di un riferimento al Binder di modello di dati le annotazioni ([fare clic per visualizzare l'immagine ingrandita](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="14abc-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span></span>

<span data-ttu-id="14abc-119">Selezionare l'assembly Microsoft.Web.Mvc.DataAnnotations.dll sia assembly DataAnnotations e fare clic su di **OK** pulsante.</span><span class="sxs-lookup"><span data-stu-id="14abc-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="14abc-120">È possibile utilizzare l'assembly DataAnnotations incluso in .NET Framework Service Pack 1 con lo strumento di associazione dati modello di annotazioni.</span><span class="sxs-lookup"><span data-stu-id="14abc-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="14abc-121">È necessario utilizzare la versione dell'assembly DataAnnotations incluso con il download di esempio di Raccoglitore modello di dati le annotazioni.</span><span class="sxs-lookup"><span data-stu-id="14abc-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="14abc-122">Infine, è necessario registrare il gestore di associazione modello DataAnnotations nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="14abc-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="14abc-123">Aggiungere la riga di codice seguente all'applicazione\_gestore dell'evento Start () in modo che l'applicazione\_metodo Start () è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="14abc-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

<span data-ttu-id="14abc-124">Questa riga di codice registra il ataAnnotationsModelBinder come gestore di associazione del modello predefinito per l'intera applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="14abc-124">This line of code registers the ataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="14abc-125">Usando gli attributi di convalida di annotazione dei dati</span><span class="sxs-lookup"><span data-stu-id="14abc-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="14abc-126">Quando si utilizza lo strumento di associazione di dati le annotazioni del modello, utilizzare attributi di validator per eseguire la convalida.</span><span class="sxs-lookup"><span data-stu-id="14abc-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="14abc-127">Lo spazio dei nomi System.ComponentModel.DataAnnotations include i seguenti attributi di convalida:</span><span class="sxs-lookup"><span data-stu-id="14abc-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="14abc-128">Intervallo: consente di verificare se il valore di una proprietà è compreso tra un intervallo di valori specificato.</span><span class="sxs-lookup"><span data-stu-id="14abc-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="14abc-129">RegularExpression: consente di verificare se il valore di una proprietà corrisponde a un criterio di espressione regolare specificata.</span><span class="sxs-lookup"><span data-stu-id="14abc-129">RegularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="14abc-130">Richiesto: consente di contrassegnare una proprietà come obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="14abc-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="14abc-131">StringLength: consente di specificare una lunghezza massima per una proprietà di stringa.</span><span class="sxs-lookup"><span data-stu-id="14abc-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="14abc-132">Convalida-classe di base per tutti gli attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="14abc-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="14abc-133">Se le esigenze di convalida non vengono soddisfatti da uno qualsiasi dei validator standard sempre possibile la possibilità di creare un attributo di validator personalizzato ereditando un nuovo attributo di convalida dell'attributo di convalida di base.</span><span class="sxs-lookup"><span data-stu-id="14abc-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="14abc-134">La classe di prodotto in **listato 1** viene illustrato come utilizzare questi attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="14abc-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="14abc-135">Le proprietà di nome, descrizione e UnitPrice sono contrassegnate come richiesto.</span><span class="sxs-lookup"><span data-stu-id="14abc-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="14abc-136">La proprietà Name deve avere una lunghezza di stringa di caratteri inferiore a 10.</span><span class="sxs-lookup"><span data-stu-id="14abc-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="14abc-137">Infine, la proprietà UnitPrice deve corrispondere a un criterio di espressione regolare che rappresenta un importo in valuta.</span><span class="sxs-lookup"><span data-stu-id="14abc-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

<span data-ttu-id="14abc-138">**Elenco 1**: Models\Product.cs</span><span class="sxs-lookup"><span data-stu-id="14abc-138">**Listing 1**: Models\Product.cs</span></span>

<span data-ttu-id="14abc-139">La classe di prodotto viene illustrato come utilizzare un attributo aggiuntivo: l'attributo DisplayName.</span><span class="sxs-lookup"><span data-stu-id="14abc-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="14abc-140">L'attributo DisplayName consente di modificare il nome della proprietà quando la proprietà viene visualizzata in un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="14abc-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="14abc-141">Invece di visualizzare il messaggio di errore "è obbligatorio il campo UnitPrice" è possibile visualizzare il messaggio di errore "il campo Prezzo è obbligatorio".</span><span class="sxs-lookup"><span data-stu-id="14abc-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="14abc-142">Se si desidera personalizzare completamente il messaggio di errore visualizzato da un validator è possibile assegnare un messaggio di errore personalizzata per la proprietà del validator messaggio di errore simile al seguente:`<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="14abc-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="14abc-143">È possibile utilizzare la classe di prodotto in **listato 1** con l'azione del controller nel metodo di creazione **listato 2**.</span><span class="sxs-lookup"><span data-stu-id="14abc-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="14abc-144">Questa azione del controller ricompilata la visualizzazione Create quando lo stato del modello contiene errori.</span><span class="sxs-lookup"><span data-stu-id="14abc-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

<span data-ttu-id="14abc-145">**Il listato 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="14abc-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="14abc-146">Infine, è possibile creare la visualizzazione in **listato 3** facendo clic l'azione del metodo di creazione e selezionando l'opzione di menu **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="14abc-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="14abc-147">Creare una visualizzazione fortemente tipizzata con la classe di prodotto come classe di modello.</span><span class="sxs-lookup"><span data-stu-id="14abc-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="14abc-148">Selezionare **crea** dall'elenco a discesa del contenuto di visualizzazione (vedere **figura 2**).</span><span class="sxs-lookup"><span data-stu-id="14abc-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

<span data-ttu-id="14abc-149">**Figura 2**: aggiunta della visualizzazione di creazione</span><span class="sxs-lookup"><span data-stu-id="14abc-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

<span data-ttu-id="14abc-150">**Elenco di 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="14abc-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="14abc-151">Rimuovere il campo Id dal modulo di creazione generato dal **Aggiungi visualizzazione** opzione di menu.</span><span class="sxs-lookup"><span data-stu-id="14abc-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="14abc-152">Poiché il campo Id corrisponde a una colonna Identity, non si desidera consentire agli utenti di immettere un valore per questo campo.</span><span class="sxs-lookup"><span data-stu-id="14abc-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="14abc-153">Se si invia il form per la creazione di un prodotto e non si immette i valori per i campi obbligatori, quindi i messaggi di errore di convalida nella **figura 3** vengono visualizzati.</span><span class="sxs-lookup"><span data-stu-id="14abc-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

<span data-ttu-id="14abc-154">**Figura 3**: i campi obbligatori mancanti</span><span class="sxs-lookup"><span data-stu-id="14abc-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="14abc-155">Se si immette un importo di valuta non valido, quindi il messaggio di errore in **figura 4** viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="14abc-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

<span data-ttu-id="14abc-156">**Figura 4**: importo di valuta non valido</span><span class="sxs-lookup"><span data-stu-id="14abc-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="14abc-157">Utilizzo di validator annotazione dei dati con Entity Framework</span><span class="sxs-lookup"><span data-stu-id="14abc-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="14abc-158">Se si utilizza Microsoft Entity Framework per generare classi del modello di dati è possibile applicare gli attributi di convalida direttamente alle classi.</span><span class="sxs-lookup"><span data-stu-id="14abc-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="14abc-159">Poiché la finestra di progettazione di Entity Framework genera classi del modello, le modifiche apportate alle classi modello verranno sovrascritto alla successiva che si apportano modifiche nella finestra di progettazione.</span><span class="sxs-lookup"><span data-stu-id="14abc-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="14abc-160">Se si desidera utilizzare i validator con le classi generate da Entity Framework è necessario creare classi di dati di metadati.</span><span class="sxs-lookup"><span data-stu-id="14abc-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="14abc-161">Applicare i validator per la classe di metadati dati invece di applicare i validator alla classe effettiva.</span><span class="sxs-lookup"><span data-stu-id="14abc-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="14abc-162">Ad esempio, si supponga che sia stato creato una classe di film tramite Entity Framework (vedere **figura 5**).</span><span class="sxs-lookup"><span data-stu-id="14abc-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="14abc-163">Si supponga inoltre che si desidera rendere il titolo del film e Director proprietà obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="14abc-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="14abc-164">In tal caso, è possibile creare la classe parziale e dati in meta **listato 4**.</span><span class="sxs-lookup"><span data-stu-id="14abc-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

<span data-ttu-id="14abc-165">**Figura 5**: classe film generato da Entity Framework</span><span class="sxs-lookup"><span data-stu-id="14abc-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

<span data-ttu-id="14abc-166">**Listato 4**: Models\Movie.cs</span><span class="sxs-lookup"><span data-stu-id="14abc-166">**Listing 4**: Models\Movie.cs</span></span>

<span data-ttu-id="14abc-167">Il file in **listato 4** contiene due classi denominate film e MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="14abc-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="14abc-168">La classe film è una classe parziale.</span><span class="sxs-lookup"><span data-stu-id="14abc-168">The Movie class is a partial class.</span></span> <span data-ttu-id="14abc-169">Corrisponde alla classe parziale generata da Entity Framework che è contenuto nel file DataModel.Designer.vb.</span><span class="sxs-lookup"><span data-stu-id="14abc-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="14abc-170">.NET framework attualmente non supporta proprietà parziale.</span><span class="sxs-lookup"><span data-stu-id="14abc-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="14abc-171">Pertanto, non è possibile applicare gli attributi di convalida per le proprietà della classe film definita nel file DataModel.Designer.vb applicando gli attributi di convalida per le proprietà della classe film definita nel file **listato 4**.</span><span class="sxs-lookup"><span data-stu-id="14abc-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="14abc-172">Si noti che la classe parziale film è decorata con un attributo MetadataType che fa riferimento la classe MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="14abc-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="14abc-173">La classe MovieMetaData contiene le proprietà del proxy per le proprietà della classe film.</span><span class="sxs-lookup"><span data-stu-id="14abc-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="14abc-174">Gli attributi di convalida vengono applicati alle proprietà della classe MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="14abc-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="14abc-175">La proprietà Title, Director e DateReleased sono tutti contrassegnate come proprietà obbligatorie.</span><span class="sxs-lookup"><span data-stu-id="14abc-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="14abc-176">La proprietà di directory deve essere assegnata una stringa contenente meno di 5 caratteri.</span><span class="sxs-lookup"><span data-stu-id="14abc-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="14abc-177">Infine, in cui viene applicato l'attributo DisplayName per la proprietà DateReleased per visualizzare un messaggio di errore "il campo rilasciato data è obbligatorio."</span><span class="sxs-lookup"><span data-stu-id="14abc-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="14abc-178">anziché l'errore "il campo DateReleased è obbligatorio."</span><span class="sxs-lookup"><span data-stu-id="14abc-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="14abc-179">Si noti che le proprietà del proxy nella classe MovieMetaData non necessaria rappresentare gli stessi tipi di proprietà corrispondenti nella classe film.</span><span class="sxs-lookup"><span data-stu-id="14abc-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="14abc-180">Ad esempio, la proprietà Director è una proprietà stringa nella classe film e proprietà di un oggetto nella classe MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="14abc-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="14abc-181">La pagina in **figura 6** illustra i messaggi di errore restituiti quando si immettono valori non validi per le proprietà di film.</span><span class="sxs-lookup"><span data-stu-id="14abc-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

<span data-ttu-id="14abc-182">**Figura 6**: utilizzo di validator con Entity Framework ([fare clic per visualizzare l'immagine ingrandita](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="14abc-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="14abc-183">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="14abc-183">Summary</span></span>

<span data-ttu-id="14abc-184">In questa esercitazione è stato descritto come sfruttare il raccoglitore di modelli dati annotazione per la convalida all'interno di un'applicazione MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="14abc-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="14abc-185">È stato descritto come utilizzare i diversi tipi di attributi di convalida, ad esempio la e StringLength.</span><span class="sxs-lookup"><span data-stu-id="14abc-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="14abc-186">È stato inoltre descritto come utilizzare questi attributi quando si lavora con Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="14abc-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="14abc-187">[Precedente](validating-with-a-service-layer-cs.md)
[Successivo](creating-model-classes-with-the-entity-framework-vb.md)</span><span class="sxs-lookup"><span data-stu-id="14abc-187">[Previous](validating-with-a-service-layer-cs.md)
[Next](creating-model-classes-with-the-entity-framework-vb.md)</span></span>
